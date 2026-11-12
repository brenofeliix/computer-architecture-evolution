# Grupo 09

## Arquitetura

GPU (Graphics Processing Unit)

---

## Integrantes

-Gabriel Eduardo Dembinski 

-Gabriel Rico Manha

-José Lucas Rodrigues Ferreira 

-Lucas Hitoshi Nagai

-Marcos Vinicios Miranda Herculano



## Arquitetura / processador escolhido

*GPU (Graphics Processing Unit)* — arquitetura de processamento paralelo, originalmente voltada à renderização gráfica e hoje também utilizada como coprocessador de propósito geral (GPGPU), especialmente no treinamento de modelos de inteligência artificial.

## Breve contexto histórico

A GPU surgiu nos anos 1990 para resolver um problema que a CPU não conseguia atender com desempenho suficiente: calcular milhões de pixels e vértices por segundo em gráficos 3D. Em 1999, a NVIDIA lançou a GeForce 256, comercializada como a primeira "GPU" do mercado. Em 2006, a NVIDIA criou o CUDA, plataforma que permitiu utilizar o poder de processamento paralelo da GPU para qualquer tipo de cálculo repetitivo, não apenas gráficos — abrindo caminho para o uso em simulações científicas, computação de alto desempenho e, mais recentemente, inteligência artificial.

## Características técnicas básicas

*ISA (Instruction Set Architecture):* as GPUs NVIDIA não expõem uma ISA nativa fixa ao programador; o código CUDA é compilado para uma ISA virtual intermediária chamada *PTX (Parallel Thread Execution), que o driver traduz para a ISA nativa de cada geração de hardware, chamada **SASS*. Isso permite compatibilidade entre diferentes gerações de GPU sem recompilar o programa do zero.

*Tamanho da palavra:* predominantemente *32 bits* — a maioria dos registradores, operações aritméticas e threads de um warp opera nesse tamanho. Operações e endereçamento de memória de maior porte utilizam *64 bits*.

*Processador / família:* núcleos CUDA organizados em blocos chamados *Streaming Multiprocessors (SMs)*. Principais famílias: NVIDIA (GeForce, Tesla, Ampere, Hopper) e AMD (GCN/RDNA, com a plataforma aberta ROCm/HIP como alternativa ao CUDA).

*Memória:* hierarquia própria, separada da RAM do host — registradores (privados por thread) → memória compartilhada e cache L1 (por SM) → cache L2 (compartilhada entre SMs) → memória global/VRAM (maior capacidade, porém mais lenta).

*Entrada e saída:* a GPU não possui portas de E/S próprias; toda comunicação com o restante do sistema é feita através do barramento *PCIe* (ligação com a CPU/host), da interconexão *NVLink* (entre múltiplas GPUs) e de transferências diretas via *DMA*.

## Importância para a evolução da computação

A GPU representa uma mudança de paradigma na forma de projetar processadores: em vez de poucos núcleos complexos otimizados para tarefas sequenciais, ela prioriza milhares de núcleos simples executando operações em paralelo (modelo SIMT). Essa escolha arquitetural, inicialmente pensada para gráficos, tornou-se a base da computação de alto desempenho moderna — sustentando desde simulações científicas até a revolução da inteligência artificial, já que o treinamento de redes neurais depende fundamentalmente da mesma capacidade que motivou a criação da GPU: realizar um volume massivo de operações matemáticas simples e repetitivas ao mesmo tempo.

## Navegação da documentação

### Fundamentos e organização

1. [História e contexto da GPU](01-history.md)
2. [Visão geral do sistema](02-system-overview.md)
3. [CPU, unidades de processamento e execução](03-cpu.md)
4. [Organização da memória](04-memory.md)
5. [Barramentos e interconexões](05-bus.md)
6. [Entrada e saída](06-input-output.md)
7. [ISA e modelo de programação](07-instruction-set.md)

### Desempenho, evolução e aplicações

8. [Comparação, integração e desempenho moderno](08-modern-comparison.md)
9. [Legado, influência e perspectivas futuras](09-legacy.md)
10. [Questões orientadas à pesquisa e respostas](10-research-question.md)
11. [Exemplo técnico: detecção de objetos em vídeo na GPU](11-technical-example.md)

### Referências

- [Referências bibliográficas e documentação técnica](references.md)
