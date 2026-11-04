# 6. Input and Output

## 6.1 Input Devices
Uma GPU normalmente não recebe dados diretamente de teclado ou mouse. Ela recebe informações do sistema hospedeiro, geralmente por meio da CPU e do driver da GPU. As principais entradas são:

- **Dados gráficos:** vértices, texturas, modelos 3D, imagens e comandos de renderização.
- **Dados de computação:** matrizes, tensores, vetores, vídeos e conjuntos de dados.
- **Programas e instruções:** kernels CUDA, HIP, OpenCL ou shaders, compilados para uma representação que a GPU consegue executar.
- **Configurações:** tamanho dos blocos de threads, formato dos dados, parâmetros de renderização e configurações de memória.
- **Comandos da aplicação:** pedidos para copiar dados, iniciar um kernel, renderizar uma cena ou retornar um resultado.

Em uma placa dedicada, os dados podem sair da memória RAM e chegar à VRAM por PCI Express. Em GPUs integradas, CPU e GPU podem compartilhar parte da memória principal.

## 6.2 Output Devices
Os resultados produzidos pela GPU podem ter diferentes destinos:

- **Tela:** a GPU gera imagens e envia o quadro final ao monitor por interfaces como HDMI ou DisplayPort, normalmente através do controlador de vídeo.
- **Memória da GPU:** resultados intermediários, imagens, tensores e pesos de modelos permanecem na VRAM para serem usados por outros kernels.
- **Memória da CPU:** resultados numéricos, classificações ou imagens podem ser copiados da GPU para a RAM.
- **Codificador de vídeo ou armazenamento:** a GPU pode produzir vídeo comprimido ou dados que serão gravados por outro componente do sistema.
- **Aplicação:** em IA, a saída pode ser uma classificação, uma previsão, uma detecção de objeto ou uma resposta gerada por um modelo.

## 6.3 Communication Interfaces
As interfaces de comunicação fazem a ligação entre a GPU e o ambiente externo:

- **PCI Express (PCIe):** transporta comandos e dados entre CPU, RAM e GPU em placas dedicadas.
- **NVLink e interconexões equivalentes:** permitem a comunicação de alta velocidade entre GPUs e outros componentes em sistemas específicos.
- **DMA (Direct Memory Access):** permite transferências entre memória e dispositivo com pouca participação direta da CPU.
- **Driver e runtime:** APIs como CUDA, HIP e OpenCL permitem que programas reservem memória, enviem kernels, configurem operações e leiam resultados.
- **HDMI e DisplayPort:** transportam imagem e áudio para dispositivos de exibição.
- **Interfaces de memória:** conectam o processador gráfico à VRAM e determinam a largura de banda disponível para os dados.

## 6.4 Peripheral Devices
Os periféricos não precisam estar ligados diretamente à GPU. Eles normalmente se comunicam com a CPU ou com controladores do sistema, que encaminham os dados para a GPU quando necessário. Exemplos são monitor, câmera, armazenamento, rede, teclado e mouse.

A GPU também pode interagir com dispositivos de captura de vídeo, placas de rede e aceleradores por meio de APIs, barramentos e mecanismos de cópia direta. Em servidores, várias GPUs podem trocar dados por uma interconexão própria, sem depender sempre da CPU.

## 6.5 Examples
### Exemplo: renderização de uma imagem

**Entrada:** a aplicação fornece vértices, texturas, configurações e comandos de renderização.

**Processamento/interação:** a CPU envia os comandos pelo driver e pelo PCIe. A GPU distribui as tarefas entre suas unidades de execução, calcula a geometria, aplica texturas e transforma os pixels. O resultado é armazenado em um framebuffer.

**Saída:** o controlador de vídeo lê o framebuffer e envia a imagem ao monitor por HDMI ou DisplayPort.

### Exemplo: inferência de inteligência artificial

**Entrada:** a aplicação fornece uma imagem ou outro conjunto de dados e os pesos de um modelo.

**Processamento/interação:** o driver transfere os tensores para a memória da GPU, inicia os kernels e as unidades de IA executam operações matriciais em paralelo.

**Saída:** o resultado é mantido na VRAM ou copiado para a CPU, onde a aplicação apresenta uma classe, previsão ou detecção.

## 6.6 Historical Evolution
No início, GPUs eram usadas principalmente para receber comandos gráficos da CPU e enviar imagens ao monitor. Com shaders programáveis, passaram a receber programas menores para controlar etapas da renderização. A partir de CUDA, OpenCL e HIP, também passaram a receber kernels de computação geral, dados científicos e tensores de IA.

Essa evolução mudou a entrada e a saída: a GPU deixou de ser apenas um componente de exibição e passou a funcionar como acelerador de dados. Seus resultados podem ser imagens, vídeos, cálculos científicos ou respostas de modelos de inteligência artificial.

## 6.7 Modern Comparison
Em uma GPU moderna, a entrada e a saída são predominantemente digitais e controladas por software. Teclado, mouse e impressora não são partes específicas da arquitetura da GPU, portanto não são necessários para explicar seu funcionamento. A interação humana acontece indiretamente, quando uma pessoa executa um programa, escolhe configurações, envia uma imagem ou solicita uma inferência.

O processo pode ser automático, quando um programa envia kernels e recebe resultados sem intervenção durante cada operação, ou manual, quando o usuário inicia uma renderização, seleciona um arquivo ou altera parâmetros. As principais limitações são:

- a largura de banda e a latência do PCIe podem tornar transferências CPU-GPU lentas;
- a capacidade da VRAM limita o tamanho de imagens, conjuntos de dados e modelos;
- cópias frequentes entre RAM e VRAM reduzem o desempenho;
- formatos de baixa precisão podem acelerar a IA, mas causar perda de qualidade;
- falhas de driver, incompatibilidade de API ou configuração inadequada podem impedir a execução.

Assim, o desempenho da GPU depende do equilíbrio entre processamento, comunicação e armazenamento. Uma GPU muito rápida pode ficar ociosa se os dados e comandos não chegarem no momento adequado.
