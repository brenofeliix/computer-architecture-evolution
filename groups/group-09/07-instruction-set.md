# 7. Instruction Set

## 7.1 ISA Overview
Uma GPU não é programada normalmente como uma CPU tradicional. O programador escreve um kernel em CUDA C++, HIP, OpenCL ou outra linguagem, e o compilador transforma esse código em instruções que a GPU consegue executar.

Para GPUs NVIDIA, o código costuma ser compilado para **PTX (Parallel Thread Execution)**, uma ISA virtual intermediária. Em tempo de execução, o driver pode traduzir o PTX para a instrução nativa da geração da GPU, chamada SASS. Em GPUs AMD, o HIP e o ROCm utilizam outra cadeia de compilação e instruções próprias, mas a ideia geral também é transformar kernels em operações executáveis pelas unidades paralelas.

O modelo de execução é SIMT (Single Instruction, Multiple Threads). A mesma instrução é aplicada por várias threads, normalmente agrupadas em warps ou wavefronts, sobre dados diferentes.

## 7.2 Instruction Types
As instruções disponíveis variam conforme o fabricante e a geração, mas podem ser organizadas em grupos:

- **Aritméticas:** soma, subtração, multiplicação, divisão e operações de ponto flutuante.
- **Lógicas e de comparação:** AND, OR, XOR, deslocamentos, igualdade e relações de ordem.
- **Memória:** leitura e escrita em memória global, compartilhada, local, constante e de textura.
- **Controle de fluxo:** saltos, chamadas, retornos, laços e predicados condicionais.
- **Sincronização:** barreiras e operações atômicas para coordenar threads.
- **Conversão:** transformação entre tipos como inteiros, FP32, FP16, BF16 e outros formatos.
- **Especializadas:** operações matriciais, de vídeo, ray tracing e outras funções específicas do hardware.

## 7.3 Addressing Modes
Os endereços podem apontar para diferentes espaços de memória. Uma instrução pode acessar registradores privados da thread, memória compartilhada do bloco, memória global na VRAM ou regiões constantes e de textura.

O endereço geralmente é calculado a partir de um registrador, um índice de thread e um deslocamento. Por exemplo, em um vetor, a thread pode calcular `i = blockIdx.x * blockDim.x + threadIdx.x` e usar `i` para acessar `A[i]`. A forma como as threads acessam posições consecutivas influencia o aproveitamento da largura de banda.

## 7.4 Assembly Example
O exemplo abaixo usa uma instrução PTX representativa para somar dois valores em registradores:

```ptx
add.s32 %r3, %r1, %r2;
```

Essa instrução soma os valores inteiros de 32 bits em `%r1` e `%r2` e grava o resultado em `%r3`. Os registradores são privados da thread. Em um warp, várias threads podem executar a mesma instrução simultaneamente com seus próprios valores.

## 7.5 Execution Example
Considere a operação de soma de vetores `C[i] = A[i] + B[i]`:

1. A aplicação configura os vetores, a grade e o número de threads por bloco.
2. O compilador transforma o kernel em código intermediário e o driver pode gerar instruções nativas para a GPU.
3. Cada thread calcula seu índice `i` usando identificadores do bloco e da própria thread.
4. Instruções de carregamento buscam `A[i]` e `B[i]` e colocam os valores em registradores.
5. Uma instrução aritmética soma os registradores.
6. Uma instrução de armazenamento grava o resultado em `C[i]`.
7. Uma condição impede que threads fora do tamanho do vetor escrevam em posições inválidas.
8. Após todas as threads terminarem, o programa pode sincronizar e usar o vetor resultante.

## 7.6 Modern Architectures
Não é adequado comparar uma GPU diretamente com ARM ou x86 como se fossem o mesmo tipo de processador. ARM e x86 são ISAs de CPUs de propósito geral, enquanto a GPU usa um modelo paralelo com grupos de threads e muitas unidades funcionais.

CUDA, HIP e OpenCL escondem parte das instruções nativas e oferecem uma camada de programação mais portátil. Essa abstração facilita o desenvolvimento, mas pode esconder diferenças de desempenho entre fabricantes e gerações.

## 7.7 Configuração e Modelo de Programação
Antes de executar um kernel, o programa precisa configurar o dispositivo, buffers de memória, quantidade de blocos e threads, parâmetros, ordem das operações e sincronizações.

A participação humana acontece na escrita do programa e na escolha desses parâmetros. Depois que o kernel é enviado, a distribuição de blocos, o escalonamento de warps e a execução das instruções ocorrem automaticamente.

## 7.8 Limitações do Modelo
- O desempenho depende de haver trabalho suficiente para muitas threads.
- Desvios diferentes dentro do mesmo warp ou wavefront podem causar divergência.
- A ISA nativa varia entre fabricantes e gerações.
- Erros de acesso à memória ou de sincronização podem produzir resultados incorretos.
- Transferências entre CPU e GPU podem custar mais que a própria execução.
- O código precisa respeitar os limites de registradores, memória compartilhada, VRAM e número de threads.
