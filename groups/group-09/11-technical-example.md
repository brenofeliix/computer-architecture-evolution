# 11. Exemplo técnico: inferência de objetos em vídeo na GPU

## 11.1 Problema real

Considere uma câmera de segurança transmitindo vídeo em 1920 x 1080 a 30
quadros por segundo. O sistema precisa identificar pessoas e veículos quase em
tempo real usando um detector de objetos treinado previamente e convertido para
TensorRT.

O objetivo não é apenas "executar a rede na GPU". O objetivo é entregar um
resultado por quadro com baixa latência e sem perder quadros:

```text
quadro da câmera -> pré-processamento -> inferência -> pós-processamento
                  -> caixas, classes e confiança
```

Esse caso é um exemplo de multiplicação de matrizes e convoluções aplicado a
uma situação observável. Ele envolve entrada, memória, comunicação CPU-GPU,
unidades especializadas, sincronização e uma saída verificável.

## 11.2 Componentes e responsabilidades

| Componente | Responsabilidade | Evidência no projeto |
|---|---|---|
| Câmera e decodificador | Produzir e decodificar os quadros | [06-input-output.md](06-input-output.md) |
| CPU | Controlar a aplicação, filas e decisões | [03-cpu.md](03-cpu.md) |
| Driver/runtime | Alocar memória e lançar kernels | [07-instruction-set.md](07-instruction-set.md) |
| VRAM | Manter imagem, pesos e tensores | [04-memory.md](04-memory.md) |
| SMs/CUs | Executar kernels de pré/pós-processamento | [02-system-overview.md](02-system-overview.md) |
| Tensor Cores | Acelerar convoluções e GEMMs em FP16/INT8 | [09-legacy.md](09-legacy.md) |
| Aplicação/host | Exibir, armazenar ou encaminhar as detecções | [08-modern-comparison.md](08-modern-comparison.md) |

## 11.3 Fluxo detalhado

1. A câmera entrega um quadro ao decodificador.
2. O quadro é convertido para RGB e redimensionado para a entrada do modelo,
   por exemplo 640 x 640.
3. O quadro é colocado em memória fixada (*pinned memory*) ou em um buffer
   acessível pela GPU.
4. Uma transferência assíncrona copia o quadro para a VRAM enquanto a GPU
   conclui o quadro anterior.
5. O runtime lança os kernels de normalização e inferência.
6. Os Tensor Cores executam convoluções e multiplicações de matrizes em
   precisão mista.
7. O pós-processamento aplica limiar de confiança e supressão de caixas
   sobrepostas (NMS).
8. O resultado retorna somente como uma lista pequena de detecções, e não como
   uma imagem inteira.
9. A CPU exibe, grava ou envia as detecções para outro serviço.

O pipeline deve usar pelo menos dois buffers. Enquanto o buffer A é processado,
o buffer B recebe o próximo quadro. Eventos CUDA sincronizam somente a
dependência necessária, evitando uma espera global a cada etapa.

## 11.4 Modelo simplificado de execução

```cuda
// Uma thread normaliza um pixel; o kernel é lançado em uma grade 2D.
__global__ void normalizar(const uchar3* entrada, half* tensor,
                           int largura, int altura) {
    int x = blockIdx.x * blockDim.x + threadIdx.x;
    int y = blockIdx.y * blockDim.y + threadIdx.y;
    if (x < largura && y < altura) {
        int i = y * largura + x;
        tensor[3 * i + 0] = entrada[i].x / 255.0f;
        tensor[3 * i + 1] = entrada[i].y / 255.0f;
        tensor[3 * i + 2] = entrada[i].z / 255.0f;
    }
}
```

O kernel acima não representa toda a rede neural. Ele demonstra a associação
entre dados e threads e a conversão do quadro para o tensor de entrada. A
inferência é delegada a uma biblioteca otimizada, como TensorRT, que escolhe
kernels, formatos e Tensor Cores adequados.

Uma chamada conceitual do pipeline é:

```cpp
cudaMemcpyAsync(d_frame, h_frame, bytes,
                cudaMemcpyHostToDevice, stream);
normalizar<<<blocos, threads, 0, stream>>>(d_frame, d_tensor, 640, 640);
context->enqueueV3(stream);       // chamada conceitual do detector
posprocessar<<<blocos, threads, 0, stream>>>(d_predicoes, d_deteccoes);
cudaMemcpyAsync(h_deteccoes, d_deteccoes, bytes_saida,
                cudaMemcpyDeviceToHost, stream);
```

O trecho é pseudocódigo de integração: uma aplicação real precisa configurar
os endereços dos tensores no contexto TensorRT antes de chamar `enqueueV3`.
`h_frame` deve ser reutilizado e alocado como memória fixada; `bytes_saida` é
pequeno porque somente as detecções são copiadas de volta.

## 11.5 Onde estão os gargalos?

O tempo de um quadro pode ser aproximado por:

```text
Tquadro = Tcaptura + Tdecodificação + Ttransferência
        + Tpré-processamento + Tinferência + Tpós-processamento
```

O gargalo é o maior estágio do caminho crítico, não necessariamente o número
de núcleos da GPU.

| Gargalo | Sintoma observável | Causa |
|---|---|---|
| Transferência PCIe | GPU ociosa entre quadros | Cópias síncronas ou quadro inteiro indo e voltando |
| Pré-processamento | Inferência rápida, FPS baixo | Conversão e redimensionamento na CPU |
| Inferência | Fila cresce e latência aumenta | Modelo grande, precisão FP32 ou VRAM insuficiente |
| Pós-processamento | GPU termina, CPU demora | NMS e filtragem serial na CPU |
| Memória | Baixa ocupação e muitos stalls | Acessos não coalescidos ou pouca reutilização |
| Sincronização | Lacunas no timeline | `cudaDeviceSynchronize()` usado em cada etapa |
| Divergência | Threads do warp seguem caminhos diferentes | Muitos `if` e tamanhos de entrada irregulares |

Não se deve afirmar que a GPU é mais rápida sem medir. A evidência mínima é
comparar latência média e percentil 95, FPS, uso da GPU, volume de transferência
e ocupação antes e depois da otimização.

## 11.6 Solução completa para cada gargalo

### A. Reduzir a comunicação

- manter pesos, tensores intermediários e resultados na VRAM;
- copiar de volta apenas as caixas detectadas;
- usar memória fixada e `cudaMemcpyAsync`;
- usar DMA e, quando disponível, captura/decodificação diretamente em memória
  acessível pela GPU;
- processar vários quadros por lote quando a latência permitir.

### B. Aumentar a eficiência da inferência

- converter o modelo para TensorRT;
- usar FP16 ou INT8 com calibração e validação da acurácia;
- usar Tensor Cores;
- reduzir a resolução ou escolher uma variante menor do detector;
- manter um contexto de inferência aquecido, sem recriá-lo por quadro.

FP16/INT8 não são uma otimização gratuita: a acurácia deve ser comparada com
um conjunto de validação. Se a precisão cair além do aceitável, usar FP16 ou
quantização seletiva somente em camadas tolerantes.

### C. Sobrepor as etapas

- usar streams e eventos para encadear captura, cópia, inferência e saída;
- usar *double buffering* ou *ring buffer*;
- evitar sincronização global;
- executar pré-processamento e pós-processamento na GPU quando o custo da
  transferência para a CPU for maior que o custo do kernel.

### D. Melhorar o acesso à memória

- armazenar pixels e tensores em formato contíguo;
- fazer acessos coalescidos;
- reutilizar blocos de dados em cache ou memória compartilhada;
- escolher dimensões de bloco compatíveis com o warp;
- observar registradores e ocupação com um profiler.

### E. Controlar a fila e a qualidade do serviço

- medir a idade do quadro, não somente o FPS médio;
- descartar quadros antigos quando a latência for mais importante que analisar
  todos eles;
- limitar a fila para evitar crescimento indefinido;
- separar a thread de captura da thread de visualização;
- aplicar *backpressure* quando a GPU estiver saturada.

## 11.7 Demonstração simples e verificável

Para demonstrar o funcionamento, usar uma imagem com uma pessoa e um veículo e comparar
três versões do mesmo pipeline:

1. CPU: pré-processamento, inferência e pós-processamento na CPU;
2. GPU ingênua: cópia síncrona do quadro e inferência na GPU;
3. GPU otimizada: FP16/TensorRT, buffers fixos, cópia assíncrona, dois buffers
   e pós-processamento na GPU.

Registrar em uma tabela:

| Versão | Latência média | P95 | FPS | Transferências | Acurácia |
|---|---:|---:|---:|---:|---:|
| CPU | medir | medir | medir | não aplicável | referência |
| GPU ingênua | medir | medir | medir | quadro completo | comparar |
| GPU otimizada | medir | medir | medir | detecções | comparar |

O resultado esperado não é um número universal. A hipótese é que a versão
otimizada reduza o tempo de transferência e sobreponha etapas, mantendo a
acurácia dentro da tolerância definida. Isso pode ser demonstrado com
Nsight Systems, eventos CUDA e um cronômetro na aplicação. Se essas ferramentas
não estiverem disponíveis, os mesmos estágios podem ser medidos com eventos
CUDA e temporizadores monotônicos no host.

## 11.8 Respostas diretas para a banca

- **O conceito existe?** Sim: threads SIMT, VRAM, PCIe, streams, DMA e Tensor
  Cores aparecem no fluxo real.
- **Arquitetura ou implementação?** A arquitetura define unidades, memória e
  modelo de execução; TensorRT, tamanho de bloco, buffers e streams são
  decisões de implementação.
- **E se uma etapa falhar?** O pipeline deve registrar o erro, liberar o
  buffer e aplicar uma política explícita: repetir, reduzir resolução ou
  descartar o quadro. Não se deve apresentar uma detecção falsa.
- **Onde está o gargalo?** No estágio com maior tempo medido no caminho crítico;
  pode ser PCIe, inferência, memória, CPU ou sincronização.
- **Qual é a limitação?** GPU depende de paralelismo, VRAM e alimentação
  eficiente de dados; FP16/INT8 podem reduzir precisão e aumentam a necessidade
  de validação.
- **Como demonstrar?** Fixar a mesma entrada, comparar três versões, medir
  latência/FPS/P95 e validar as detecções com uma referência.

## 11.9 Relação com a arquitetura estudada

O exemplo conecta as partes anteriores do projeto:

- a CPU coordena o fluxo e toma decisões de controle;
- o PCIe/DMA movimenta o quadro e as detecções entre host e dispositivo;
- a hierarquia de memória mantém pesos e tensores próximos das unidades de
  execução;
- threads SIMT processam pixels e elementos de tensores em paralelo;
- Tensor Cores aceleram as operações matriciais quando o formato numérico é
  compatível;
- streams, eventos e buffers duplos permitem sobrepor comunicação e cálculo.

Assim, a conclusão não é que uma GPU sempre vence uma CPU. A conclusão é que
uma carga com muitos dados semelhantes pode se beneficiar da GPU quando o
algoritmo, o acesso à memória e a comunicação forem organizados para manter as
unidades ocupadas.
