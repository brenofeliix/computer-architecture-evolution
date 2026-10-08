# 1. Historical Background

## 1.1 Architecture Name
GPU (Graphics Processing Unit, ou Unidade de Processamento Gráfico), com foco em processamento paralelo e computação acelerada por GPU.

## 1.2 Year of Creation
As primeiras GPUs comerciais surgiram no final da década de 1990. Em 1999, a NVIDIA popularizou o termo GPU com a GeForce 256, um chip capaz de executar no próprio hardware várias etapas do processamento gráfico. A transformação da GPU em um acelerador de propósito geral aconteceu principalmente a partir dos anos 2000, com tecnologias como CUDA.

## 1.3 Country
As primeiras arquiteturas de GPU de grande impacto comercial foram desenvolvidas principalmente nos Estados Unidos, por empresas como NVIDIA, ATI (posteriormente adquirida pela AMD) e 3dfx.

## 1.4 Inventor(s)
Não existe um único inventor. A GPU é resultado do trabalho de várias equipes de engenharia. A NVIDIA, fundada por Jensen Huang, Chris Malachowsky e Curtis Priem, teve papel importante na popularização do conceito. A ATI/AMD e outros fabricantes também contribuíram para a evolução das unidades programáveis, dos shaders e da computação paralela.

## 1.5 Organization / Company
NVIDIA, AMD e Intel são os principais fabricantes atuais de GPUs. A NVIDIA teve papel central na computação de propósito geral com CUDA; a AMD desenvolveu a plataforma ROCm/HIP; e a Intel produz GPUs integradas e dedicadas. Fabricantes de celulares, como ARM e Qualcomm, também desenvolveram GPUs para sistemas embarcados.

## 1.6 Historical Context
No início, o processamento gráfico era feito pela CPU. Com o aumento da resolução das telas, da complexidade dos jogos e do uso de imagens tridimensionais, essa abordagem passou a consumir muito tempo de processamento. As GPUs surgiram como processadores especializados em executar muitas operações semelhantes ao mesmo tempo, principalmente cálculos de vértices, pixels e cores.

O princípio central é o paralelismo: em vez de executar uma operação por vez, a GPU aplica a mesma instrução a muitos dados. Esse modelo é conhecido como SIMT (Single Instruction, Multiple Threads). Embora tenha sido criado para gráficos, ele também se mostrou útil em simulações, processamento de imagens, análise de dados e aprendizado de máquina.

Com CUDA, lançado pela NVIDIA em 2006, e com plataformas abertas como OpenCL e HIP, tornou-se possível programar a GPU para tarefas que não são exclusivamente gráficas. A partir da década de 2010, redes neurais profundas passaram a utilizar GPUs porque seu treinamento depende de grande quantidade de multiplicações de matrizes e outras operações paralelas.

## 1.7 Problem to Solve
O problema inicial era o alto custo de processar gráficos em tempo real usando somente a CPU. A GPU foi criada para:

- executar milhares de operações aritméticas em paralelo;
- liberar a CPU para controlar o sistema, os programas e a lógica do jogo;
- aumentar a taxa de atualização e a resolução das imagens;
- processar vértices, texturas, pixels e efeitos visuais com menor custo;
- aproveitar o alto paralelismo de tarefas científicas e de inteligência artificial.

Esse modelo é mais eficiente quando a tarefa pode ser dividida em muitos trabalhos independentes. Ele é menos adequado para decisões muito sequenciais, com muitos desvios ou dependências entre etapas, que continuam sendo mais apropriadas para a CPU.

## 1.8 Main Contributions
1. **Processamento paralelo em larga escala:** milhares de núcleos simples executam grupos de threads simultaneamente.
2. **Pipeline gráfico programável:** shaders permitiram programar etapas do processamento de vértices, geometria e pixels.
3. **Modelo SIMT:** threads são organizadas em grupos, chamados warps na NVIDIA e wavefronts na AMD, para executar instruções de maneira coordenada.
4. **Memória especializada:** registradores, memórias compartilhadas, caches e memória global permitem equilibrar velocidade, capacidade e largura de banda.
5. **Computação de propósito geral:** CUDA, OpenCL e HIP permitiram usar a GPU em tarefas científicas, análise de dados e simulações.
6. **Aceleração de inteligência artificial:** GPUs modernas incluem Tensor Cores ou unidades equivalentes para acelerar multiplicações de matrizes, convoluções e operações com baixa precisão.
7. **Execução heterogênea:** CPU e GPU trabalham em conjunto; a CPU coordena o programa e a GPU executa as partes com maior paralelismo.

## 1.9 GPU Moderna e Inteligência Artificial
As GPUs atuais não são apenas placas de vídeo. Elas incluem unidades especializadas para ray tracing, codificação e decodificação de vídeo, segurança e IA. Em redes neurais, os Tensor Cores aceleram operações com formatos como FP16, BF16, TF32 e INT8. Formatos menores ocupam menos memória e podem aumentar o desempenho, mas exigem técnicas para evitar perda excessiva de precisão.

No treinamento, a GPU processa lotes de dados, calcula as previsões da rede e ajusta os pesos por meio da retropropagação. Na inferência, ela executa o modelo já treinado para gerar respostas, classificações ou previsões. O desempenho depende não apenas da quantidade de núcleos, mas também da largura de banda da memória, da capacidade de armazenar o modelo e da eficiência do software.

