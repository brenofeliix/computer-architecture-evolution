# 9. Legado da GPU

## 9.1 Importância histórica

A GPU surgiu para retirar da CPU o trabalho repetitivo de transformar vértices,
aplicar texturas e produzir pixels. A GeForce 256, lançada pela NVIDIA em
1999, popularizou o nome GPU e consolidou a ideia de um processador dedicado
ao pipeline gráfico. A partir de shaders programáveis e de plataformas como
CUDA, OpenCL e HIP, o mesmo hardware passou a executar computação geral.

Nos anos 1990, a transição dos gráficos 2D para jogos 3D aumentou em cerca de
100 vezes a carga de cálculo vetorial. O hardware de função fixa resolvia apenas
o desenho de pixels; os shaders programáveis, que se consolidaram a partir de
2001, deram aos desenvolvedores liberdade para programar efeitos do pipeline.

## 9.2 Contribuições científicas

O modelo SIMT, a execução de milhares de threads e a alta largura de banda da
VRAM tornaram viáveis simulações, álgebra linear, processamento de imagens e
aprendizado de máquina em larga escala. A multiplicação de matrizes e as
convoluções, fundamentais para redes neurais, aproveitam diretamente esse
paralelismo.

No modelo SIMD, uma única instrução é enviada a múltiplos núcleos para operar
sobre dados diferentes. Isso economiza circuitos de decodificação e é eficiente
para aplicar a mesma operação a milhões de pixels. Na prática, as GPUs
modernas ampliam essa ideia com SIMT: threads são agrupadas em warps ou
wavefronts e executam instruções coordenadas.

## 9.3 Impacto industrial

GPUs são usadas em jogos, criação de conteúdo, visualização, automóveis,
servidores, supercomputadores e serviços de nuvem. A inclusão de Tensor Cores,
unidades de ray tracing e codificadores de vídeo mostra uma evolução de
processador gráfico para acelerador heterogêneo especializado.

A revolução GPGPU/CUDA, iniciada em 2006, permitiu executar C/C++ genérico para
ciência e matemática, sem limitar a GPU ao desenho de imagens. A mesma
capacidade de repetir operações matemáticas em paralelo também tornou as GPUs
adequadas à mineração de criptomoedas, cujos algoritmos de hashing exigem
bilhões de tentativas de combinações por segundo. Em alguns cenários, o
throughput de hashing chegou a superar o de CPUs em até 50 vezes, embora o
consumo de energia, a volatilidade econômica e a mudança dos algoritmos tenham
limitado esse uso.

## 9.4 Impacto social

O processamento gráfico em tempo real permitiu interfaces visuais mais
complexas, jogos imersivos, ferramentas de projeto e visualização científica.
Mais recentemente, GPUs reduziram o tempo de treinamento e inferência de
modelos de IA. Esse avanço também trouxe custos: alto consumo de energia,
dependência de cadeias de fabricação especializadas e concentração de
plataformas de software em poucos fornecedores.

## 9.5 Influência na computação moderna

A GPU ajudou a consolidar a arquitetura heterogênea: a CPU coordena o fluxo
geral e o acelerador executa regiões altamente paralelas. O conceito de
aceleradores especializados influenciou TPUs, NPUs, FPGAs e outros
processadores de domínio específico. Permaneceram o paralelismo de dados, a
hierarquia de memória e a divisão entre controle e cálculo; foram modificados
o grau de programabilidade, a precisão numérica e a especialização das
unidades.

Redes neurais são, matematicamente, grandes conjuntos de multiplicações de
matrizes. O marco AlexNet, em 2012, mostrou o impacto prático de treinar uma
rede em duas GPUs GTX 580, reduzindo um processo de semanas para dias. Essa
mudança ajudou a tornar viável o treinamento de modelos modernos e, mais
recentemente, de LLMs em clusters com milhares de GPUs interconectadas.

## 9.6 Características abandonadas e lições

GPUs antigas eram essencialmente pipelines gráficos fixos. Essa organização
foi parcialmente abandonada em favor de shaders e kernels programáveis.
Também deixou de ser suficiente medir apenas a taxa de pixels: desempenho
atual depende de largura de banda, ocupação, latência, consumo e eficiência do
software. A principal lição é que uma arquitetura só entrega seu potencial
quando o algoritmo, a memória e o modelo de programação estão alinhados.

## 9.7 Perspectivas futuras

Os sucessores não serão necessariamente substitutos únicos da GPU. A tendência
é combinar CPU, GPU, NPU, FPGA e memória de alta largura de banda em sistemas
heterogêneos. GPUs continuarão relevantes para gráficos e cargas paralelas,
enquanto aceleradores especializados poderão assumir inferência, segurança ou
outras tarefas com melhor eficiência energética.

Uma tendência é reduzir o gargalo do PCIe integrando CPU e GPU no mesmo
substrato. Em sistemas com memória unificada, ambos acessam o mesmo bloco de
RAM, evitando cópias desnecessárias. Exemplos incluem Apple Silicon, consoles
modernos e a plataforma NVIDIA Grace Hopper.

Outra tendência é a especialização para IA. CUDA Cores clássicos atendem
computação geral e gráficos com precisões FP32/FP64, enquanto Tensor Cores usam
precisão mista, como FP16, FP8 e INT4, para acelerar operações matriciais de
redes neurais. O futuro combina CPU, GPU, NPU, FPGA e memória de alta largura
de banda em vez de depender de um único processador.
