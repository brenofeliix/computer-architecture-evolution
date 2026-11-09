# 10. Questões orientadas à pesquisa

Este item reúne as nove perguntas que seriam apresentadas e as respectivas respostas aplicadas à
arquitetura estudada.

## 10.1 Por que a CPU “pediu socorro” na década de 1990?

A CPU foi projetada para executar instruções complexas em uma fila com baixa
latência. Com a chegada dos jogos 3D, softwares como *Quake* passaram a exigir
milhões de cálculos geométricos por segundo. A CPU travava ao tentar calcular
polígonos e cuidar simultaneamente da lógica do jogo. A transição do 2D para o
3D aumentou aproximadamente 100 vezes a carga de cálculo vetorial, criando o
gargalo que levou ao desenvolvimento das GPUs.

## 10.2 Como a GPU se comunica com o computador?

A GPU recebe comandos e dados da CPU principalmente pelo barramento PCIe, que
funciona como uma conexão de alta velocidade entre os dois componentes. A
VRAM dedicada armazena texturas, quadros, matrizes e tensores com alta largura
de banda. Após o processamento, o pipeline de saída converte os resultados
gráficos em sinais de vídeo para o monitor. Em sistemas de alto desempenho,
NVLink e interconexões equivalentes ligam várias GPUs, enquanto DMA reduz a
participação direta da CPU nas transferências.

## 10.3 Qual é a diferença entre CPU e GPU?

A CPU é o “maestro” da baixa latência: possui poucos núcleos
ultracomplexos, cache amplo e controle preditivo, sendo adequada para terminar
rapidamente uma tarefa, controlar o sistema e lidar com decisões sequenciais.
A GPU é o “exército” do throughput: possui milhares de núcleos matemáticos
simples e ALUs otimizadas para repetição, processando muitos dados por
segundo.

CPU e GPU não são concorrentes absolutos. A CPU coordena o programa, lê dados
e toma decisões; a GPU executa pixels, matrizes, tensores e outras operações
com alto paralelismo. A GPU perde vantagem em tarefas sequenciais, com
dependências, divergência ou transferências frequentes pela PCIe.

## 10.4 Como funciona o modelo SIMD?

SIMD significa *Single Instruction, Multiple Data*: uma única instrução é
enviada a múltiplos núcleos para que executem a mesma operação sobre dados
diferentes. Por exemplo, a GPU pode adicionar 10 ao brilho de dois milhões de
pixels ao mesmo tempo. O modelo economiza circuitos ao evitar decodificadores
de instrução individuais.

GPUs modernas ampliam esse princípio com SIMT (*Single Instruction, Multiple
Threads*). Threads são agrupadas em warps ou wavefronts; quando seguem o mesmo
caminho, o hardware aproveita bem o paralelismo. Desvios diferentes dentro do
grupo causam divergência e reduzem a eficiência.

## 10.5 O que foi a revolução GPGPU e a criação do CUDA?

Nos anos 1990, as GPUs tinham função fixa e eram usadas apenas para desenhar
pixels. Em 2001, shaders programáveis deram aos desenvolvedores liberdade para
programar efeitos do pipeline gráfico. Em 2006, CUDA inaugurou a era GPGPU ao
permitir executar C/C++ genérico na GPU para ciência, matemática e outras
tarefas não gráficas.

Essa mudança transformou a GPU em um acelerador programável. OpenCL e HIP
ampliaram a possibilidade de usar modelos semelhantes em diferentes
fabricantes, enquanto CUDA ofereceu uma plataforma integrada de linguagem,
bibliotecas, runtime e ferramentas.

## 10.6 Por que as GPUs dominaram a mineração?

Provas de trabalho usam algoritmos de hashing com operações matemáticas
repetitivas. Como muitas tentativas de combinações podem ser feitas
independentemente, a GPU executa força bruta massiva em paralelo. Em certos
cenários, seu throughput de hashing chegou a ser até 50 vezes superior ao de
processadores convencionais.

Essa vantagem não é universal: depende do algoritmo, da implementação e do
acesso à memória. O consumo de energia, a disponibilidade de hardware, a
volatilidade das criptomoedas e a mudança para algoritmos menos favoráveis às
GPUs também limitaram essa aplicação.

## 10.7 Por que a inteligência artificial depende tanto das GPUs?

Redes neurais são, matematicamente, grandes cálculos matriciais. Multiplicações
de matrizes, convoluções e operações de vetores contêm muitas operações
independentes, exatamente o padrão em que as GPUs são eficientes.

O marco AlexNet, em 2012, mostrou esse efeito: o treinamento em duas GPUs GTX
580 reduziu um processo de semanas para dias. Modelos atuais, incluindo LLMs,
exigem clusters de milhares de GPUs interconectadas, além de memória com alta
largura de banda e software capaz de distribuir os dados.

## 10.8 O que são Tensor Cores e hardware para IA?

CUDA Cores clássicos atendem computação geral e gráficos 3D, normalmente com
precisões FP32 ou FP64 e uma operação por ciclo. Tensor Cores são unidades
especializadas para redes neurais: trabalham com precisão mista, como FP16,
FP8 e INT4, e aceleram multiplicações de matrizes inteiras em uma operação
especializada.

Essa especialização aumenta o desempenho e a eficiência energética em IA, mas
não substitui os núcleos clássicos. A escolha depende do algoritmo, do formato
numérico e do equilíbrio desejado entre velocidade, consumo e precisão.

## 10.9 Qual é o futuro das arquiteturas de GPU?

Uma tendência é reduzir o gargalo do PCIe integrando CPU e GPU no mesmo
substrato. A memória unificada permite que ambos acessem o mesmo bloco de RAM,
evitando cópias desnecessárias. Apple Silicon, consoles modernos e a plataforma
NVIDIA Grace Hopper são exemplos dessa direção.

O futuro também será heterogêneo: CPU, GPU, NPU, FPGA e aceleradores
especializados trabalharão juntos. GPUs continuarão importantes para gráficos,
simulações e cargas paralelas, enquanto NPUs e Tensor Cores poderão executar
IA com maior eficiência. Portanto, os possíveis sucessores complementam a GPU
em vez de simplesmente eliminá-la.
