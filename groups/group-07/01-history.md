# 1. Historical Background

## 1.1 Architecture Name

MIMD (Multiple Instruction, Multiple Data).

## 1.2 Year of Creation

A arquitetura MIMD foi formalmente introduzida em 1966.

## 1.3 Country

Estados Unidos da América.

## 1.4 Inventor

Proposto pelo cientista da computação Michael J. Flynn.

## 1.5 Organization / Company

Universidade Northwestern e posteriormente na Universidade Stanford

## 1.6 Historical Context

What was happening in computing when this architecture appeared?

emergência da arquitetura MIMD (Multiple Instruction, Multiple Data) no final da década de 1960 está profundamente atrelada ao esgotamento das limitações físicas e operacionais do modelo sequencial tradicional. Durante as primeiras duas décadas da computação comercial, a indústria operou predominantemente sob o paradigma do modelo Von Neumann clássico — ou SISD (Single Instruction, Single Data) —, no qual uma única unidade central de processamento executava uma instrução por vez sobre um único elemento de dado.
 emergência da arquitetura MIMD (Multiple Instruction, Multiple Data) no final da década de 1960 está profundamente atrelada ao esgotamento das limitações físicas e operacionais do modelo sequencial tradicional. Durante as primeiras duas décadas da computação comercial, a indústria operou predominantemente sob o paradigma do modelo Von Neumann clássico — ou SISD (Single Instruction, Single Data) —, no qual uma única unidade central de processamento executava uma instrução por vez sobre um único elemento de dado.
Simultaneamente, a indústria de software vivenciava a revolução da multiprogramação e dos sistemas operacionais de tempo compartilhado (time-sharing). Pela primeira vez, computadores precisavam atender a múltiplos usuários de forma simultânea e executar tarefas heterogêneas sem que um processo bloqueasse o outro. Esse ecossistema evidenciou que a execução estritamente sequencial era ineficiente para a rotina dos grandes centros de processamento de dados, que necessitavam de dinamismo no escalonamento de tarefas.
Diante dessas exigências físicas e operacionais, a formulação teórica da arquitetura MIMD por Michael J. Flynn, em 1966, forneceu o arcabouço conceitual necessário para a computação paralela de propósito geral. Diferente da abordagem SIMD (Single Instruction, Multiple Data), que aplicava uma única instrução a um vetor de dados simultaneamente (como nos processadores vetoriais do período), a arquitetura MIMD propôs um sistema composto por múltiplos processadores verdadeiramente independentes. Cada unidade de processamento passava a ter a capacidade de executar seu próprio fluxo de instruções sobre seu próprio conjunto de dados, viabilizando o paralelismo no nível de tarefas e rotinas assíncronas.
Em suma, a criação da arquitetura MIMD não foi um evento isolado, mas sim a resposta teórica e prática a um momento de inflexão na história da tecnologia. Ela preencheu a lacuna deixada pela arquitetura sequencial tradicional ao unificar os avanços no suporte de hardware em circuitos integrados com as demandas emergentes de multiprogramação, assentando as bases para os sistemas multiprocessados modernos, supercomputadores e processadores multicore contemporâneos.

Evolução para a Terceira Geração
Início da década de 1960
A transição das válvulas e transistores discretos para os circuitos integrados (ICs) permitiu densidades de componentes drasticamente maiores, reduzindo custos e abrindo espaço para arquiteturas mais complexas nas CPUs.

Surgimento da Multiprogramação
Meados da década de 1960
Sistemas operacionais pioneiros introduziram o compartilhamento de tempo (time-sharing) e o gerenciamento de múltiplos programas na memória, exigindo suporte de hardware para interrupções e proteção de memória.

Proposição da Taxonomia de Flynn
1966
Michael J. Flynn publica o estudo que formaliza as quatro categorias de processamento de dados (SISD, SIMD, MISD, MIMD), estabelecendo a base teórica para o processamento paralelo autônomo.

Pioneirismo Acadêmico e Industrial
Final dos anos 1960 - Anos 1970
Projetos como o Burroughs D825 e, posteriormente, supercomputadores acadêmicos exploram o acoplamento de múltiplos processadores independentes que compartilham memória principal ou barramentos.

## 1.7 Problem to Solve

Which technological limitations motivated its creation?

A criação da arquitetura MIMD (Multiple Instruction, Multiple Data) foi motivada diretamente pelas limitações físicas e estruturais do modelo monoprocessado tradicional, conhecido como arquitetura Von Neumann clássica ou SISD (Single Instruction, Single Data). A limitação fundamental desse modelo reside na execução estritamente sequencial: um único fluxo de instruções processa um único fluxo de dados por vez através de um barramento compartilhado entre a unidade de processamento e a memória. Esse gargalo de transferência, somado aos limites térmicos e elétricos que impediam o aumento indefinido do clock dos transistores — como a velocidade da luz no silício e o consumo de energia —, impôs um teto físico ao rendimento dos computadores monoprocessados.
O problema central que a arquitetura MIMD visava solucionar era a incapacidade dos sistemas sequenciais (e mesmo das abordagens de paralelismo rígido, como o SIMD) em processar workloads complexas, assíncronas e heterogêneas de maneira eficiente. Enquanto a arquitetura SIMD resolvia com sucesso o processamento vetorial repetitivo (executando a mesma instrução sobre um grande volume de dados), ela falhava ao tentar lidar com tarefas genéricas do mundo real. Aplicações complexas exigem múltiplos programas ou threads executando lógicas e fluxos de controle completamente distintos simultaneamente, sem que a estagnação de um processo paralise os demais.

Diante disso, a arquitetura MIMD solucionou o gargalo do desempenho uniprocessado ao introduzir o paralelismo no nível de tarefas e de rotinas independentes. Ao dotar o sistema de múltiplos processadores autônomos — cada um com seu próprio contador de programa, unidade de controle e capacidade de buscar suas próprias instruções e dados —, a arquitetura eliminou a dependência de um fluxo de controle centralizado. Esse avanço permitiu que a capacidade total do computador passasse a escalar com o número de unidades de processamento adicionadas, e não apenas com o incremento da frequência de clock de um único chip.

Em suma, a motivação por trás do surgimento da arquitetura MIMD foi a superação do gargalo de Von Neumann e das barreiras físicas de frequência, enquanto seu objetivo prático foi viabilizar a computação paralela de propósito geral. Essa mudança paradigmática garantiu que o crescimento da capacidade computacional continuasse de forma sustentável, servindo de fundação para os supercomputadores modernos, clusters de processamento e os processadores multicore utilitários da atualidade.

## 1.8 Main Contributions

1. Autonomia Total dos Processadores
Diferente das arquiteturas anteriores — onde uma única unidade de controle orientava múltiplos elementos de cálculo (como no SIMD) —, cada processador em um sistema MIMD possui sua própria Unidade de Controle (UC), sua Unidade Lógica e Aritmética (ULA) e seu próprio Contador de Programa (PC). Isso significa que cada núcleo busca, decodifica e executa suas próprias instruções sobre seus próprios dados de maneira totalmente independente.

2. Suporte ao Paralelismo no Nível de Tarefas (Task-Level Parallelism)
Sistemas MIMD viabilizaram a execução de programas e algoritmos heterogêneos ao mesmo tempo. Enquanto um processador pode estar executando uma simulação numérica complexa, outro pode gerenciar entradas e saídas de dados e um terceiro pode rodar uma interface de usuário ou banco de dados. Essa capacidade superou a limitação das arquiteturas vetoriais/SIMD, que exigiam que todos os dados fossem submetidos à mesma operação matemática idêntica.

3. Processamento Assíncrono e Desacoplado
Os processadores em uma rede MIMD não precisam operar no mesmo ciclo de clock de maneira sincronizada. Cada unidade avança em seu próprio ritmo de execução, utilizando mecanismos de interrupção e semáforos de software para coordenar e trocar dados apenas quando necessário. Esse assincronismo evita que a estagnação (stall) de uma tarefa em determinado núcleo trave a execução dos demais.

4. Flexibilidade nas Arquiteturas de Memória e Escala
A proposta do MIMD permitiu o desenvolvimento de dois modelos fundamentais de organização de memória, viabilizando o escalonamento dos sistemas:

Memória Compartilhada (SMP / NUMA): Múltiplos processadores acessam um único espaço de endereçamento global de memória, facilitando a comunicação direta via variáveis compartilhadas (base dos processadores multicore atuais).

Memória Distribuída (Clusters / Massively Parallel Processing): Cada nó de processamento possui sua própria memória local e se comunica com os demais nós por meio de uma rede de barramentos ou rede de alta velocidade via troca de mensagens (base dos supercomputadores e computação em nuvem).

## References

FLYNN, Michael J. Very high-speed computing systems. Proceedings of the IEEE, v. 54, n. 12, p. 1901-1909, dez. 1966.

FLYNN, Michael J. Some computer organizations and their effectiveness. IEEE Transactions on Computers, v. C-21, n. 9, p. 948-960, set. 1972.

HENNESSY, John L.; PATTERSON, David A. Arquitetura de computadores: uma abordagem quantitativa. 5. ed. Rio de Janeiro: Elsevier, 2014.

STALLINGS, William. Arquitetura e organização de computadores. 10. ed. São Paulo: Pearson Education do Brasil, 2017.

TANENBAUM, Andrew S.; BOS, Herbert. Sistemas operacionais modernos. 4. ed. São Paulo: Pearson Education do Brasil, 2016.