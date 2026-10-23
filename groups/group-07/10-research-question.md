# 1. Como a arquitetura MIMD quebra o modelo tradicional de execução sequencial de código?

## 1.1 O Modelo Tradicional: O Fluxo de Controle Centralizado: 
    No modelo de computação tradicional — baseado na Arquitetura de Von Neumann e classificado como SISD (Single Instruction, Single Data) na Taxonomia de Flynn —, a execução é estritamente sequencial.
    O coração desse modelo é o Contador de Programa (PC - Program Counter), um registrador na CPU que armazena o endereço da próxima instrução a ser executada.
    A linearidade: O processador busca a instrução apontada pelo PC, executa-a, incrementa o PC para a próxima linha e repete o ciclo (Fetch-Decode-Execute). 
    O determinismo: Existe apenas uma única "linha do tempo". O estado das variáveis do programa muda em uma ordem previsível e exata, linha por linha.

## 1.2 A Quebra do Modelo pelo MIMD: Descentralização e Autonomia


    A arquitetura MIMD destrói essa linearidade ao introduzir Múltiplos Contadores de Programa independentes no sistema. Em vez de uma única CPU controlando tudo, temos múltiplos núcleos de processamento (ou múltiplos processadores) operando simultaneamente. 

    A quebra do modelo tradicional ocorre através de três pilares: 

    A. Autonomia de Instrução (Múltiplas Instruções) 
    
    Cada núcleo possui sua própria Unidade de Controle (UC) e seu próprio Contador de Programa (PC). Isso significa que, no exato mesmo ciclo de clock: 
    
     O Núcleo 1 pode estar executando uma instrução de desvio (if/else) em um sistema de login.

     O Núcleo 2 pode estar executando uma operação aritmética (sum) em um relatório de vendas. 

     O Núcleo 3 pode estar aguardando uma resposta de rede (I/O wait).

     Impacto: Não existe mais uma "receita passo a passo" centralizada. O fluxo de controle tornou-se distribuído e fragmentado.
    
    B. Autonomia de Dados (Múltiplos Dados)
    
    Cada fluxo de instrução opera sobre fluxos de dados completamente diferentes. Os núcleos podem acessar diferentes regiões da memória RAM ou memórias locais distintas (no caso de sistemas distribuídos).
    
     Impacto: O conceito de "estado do programa" muda. No modelo sequencial, o estado global é alterado de forma previsível. No MIMD, múltiplos processadores alteram partes diferentes do estado do sistema de forma simultânea e concorrente. 
    
    C. Assincronismo e Não-Determinismo 
    
    Os núcleos em uma arquitetura MIMD funcionam de forma assíncrona. Eles não esperam uns pelos outros para avançar. Fatores externos de hardware — como latência de acesso à memória cache (cache misses), interrupções do Sistema Operacional ou variações de temperatura no chip — fazem com que um núcleo execute suas tarefas ligeiramente mais rápido ou mais devagar que o outro a cada microssegundo. 
    
     Impacto: O tempo linear desaparece. Se você disparar a Tarefa A no Núcleo 1 e a Tarefa B no Núcleo 2, é impossível prever matematicamente qual linha de código terminará primeiro. O software torna-se não-determinista.

## 1.3 O Paradoxo da Engenharia de Software no MIMD
    

   Para um Engenheiro de Software, o MIMD transfere a complexidade do hardware para o código. 
    
    Como o hardware MIMD quebrou a linha do tempo sequencial, o desenvolvedor é obrigado a recriar a ordem e a previsibilidade artificialmente no software quando os dados dependem uns dos outros. Se o Código A precisa de um dado que está sendo gerado pelo Código B em outro núcleo, o engenheiro não pode apenas "torcer" para que dê tempo. 
    
    É necessário programar utilizando mecanismos de sincronização (como Threads, barreiras, semáforos, locks ou passagem de mensagens). Se esses mecanismos falharem, o sistema sofre com problemas exclusivos do mundo paralelo, como as condições de corrida ou os deadlocks. 

# 2. Qual é a diferença prática entre MIMD de Memória Compartilhada (SMP) e Memória Distribuída (Clusters) na hora de programar?


 ## 2.1 Memória compartilhada

Na memória compartilhada, os processadores ou núcleos possuem acesso a um espaço de memória comum. Isso significa que diferentes processos ou threads podem acessar as mesmas estruturas de dados, tornando a comunicação entre eles mais direta. Na prática, essa característica facilita a programação, pois o programador não precisa necessariamente transferir manualmente os dados entre os processadores. Entretanto, quando várias threads acessam ou modificam os mesmos dados simultaneamente, é necessário utilizar mecanismos de sincronização, como mutexes, semáforos e operações atômicas, para evitar conflitos e condições de corrida. Além disso, mesmo existindo uma memória compartilhada, os processadores podem possuir caches próprias, sendo necessária a manutenção da coerência dos dados armazenados nesses caches.

 ## 2.2 Memória distribuída 

Já na memória distribuída, cada processador ou nó possui sua própria memória local e independente. Dessa forma, um processo executado em um computador não consegue acessar diretamente uma informação armazenada na memória de outro computador. Quando um processo necessita de dados que estão em outro nó, é necessário realizar uma comunicação, normalmente por meio de uma rede e utilizando mecanismos de troca de mensagens, como os disponibilizados pelo MPI (Message Passing Interface). Por esse motivo, a programação em sistemas distribuídos tende a ser mais complexa, pois o programador precisa considerar a divisão dos dados, a comunicação entre os nós e o custo associado à transferência das informações.

## 2.3 Diferenças entre esses tipos de memórias

Uma diferença prática importante entre os dois modelos está, portanto, na maneira como os dados são compartilhados. Em um sistema SMP, diferentes threads podem trabalhar sobre uma mesma estrutura de dados armazenada na memória compartilhada. Por exemplo, em uma aplicação de processamento de imagens, diferentes núcleos podem processar partes de uma mesma imagem, acessando dados que estão disponíveis no espaço de memória comum. Em um cluster, por outro lado, a imagem ou o conjunto de dados pode ser dividido entre diferentes computadores, ficando cada nó responsável por uma parte do processamento. Caso um nó precise de informações que estejam armazenadas em outro, será necessário realizar uma comunicação pela rede.

Outra diferença relevante está relacionada ao desempenho. Na memória compartilhada, o acesso aos dados tende a ser mais simples e a comunicação entre os processadores pode ocorrer por meio do espaço de memória comum. Entretanto, quando muitas threads tentam acessar os mesmos dados, podem surgir problemas de contenção, sincronização e acesso concorrente. Na memória distribuída, cada nó possui acesso rápido à sua própria memória, mas a comunicação com outros nós depende da rede, que apresenta latência e limitações de largura de banda. Assim, em aplicações distribuídas, reduzir a quantidade de comunicação entre os nós pode ser fundamental para obter um bom desempenho.

 ## 2.4 Para qual finalidade cada uma é mais indicada ? 

A escolha entre memória compartilhada e memória distribuída também depende do contexto de utilização. A memória compartilhada é particularmente adequada para aplicações executadas em uma única máquina com vários núcleos, especialmente quando existe uma necessidade frequente de compartilhar dados entre os processos ou threads. Ela pode ser utilizada, por exemplo, em servidores, processamento de imagens e vídeos, aplicações científicas, simulações e outros programas paralelos que possam aproveitar os diversos núcleos disponíveis em um mesmo computador.

A memória distribuída, por sua vez, é indicada principalmente quando o problema pode ser dividido em partes e processado por diferentes computadores ou quando a capacidade de uma única máquina não é suficiente. Esse modelo é utilizado em clusters e pode ser aplicado em supercomputadores, grandes simulações científicas, processamento de grandes volumes de dados, inteligência artificial e outras aplicações de grande escala. Uma de suas principais vantagens é a possibilidade de aumentar a capacidade de processamento adicionando novos nós ao sistema, embora isso também aumente a necessidade de gerenciamento da comunicação entre as máquinas.

Dessa forma, duas evidências podem ser utilizadas para compreender a aplicação desses modelos em diferentes contextos. A primeira é a forma de compartilhamento e comunicação dos dados: enquanto o SMP permite que os processos acessem diretamente uma memória comum, os clusters exigem a comunicação pela rede para que processos localizados em diferentes nós compartilhem informações. A segunda é a capacidade de escalabilidade: um sistema de memória compartilhada está limitado principalmente aos recursos de uma única máquina, enquanto um sistema de memória distribuída pode aumentar sua capacidade adicionando novos computadores ao cluster.


# 3.  O que é o problema da "Coerência de Cache" em hardware MIMD e como ele afeta o software?

# 4. Como a Lei de Amdahl define o limite de performance de um software rodando em MIMD?

# 5. Por que as arquiteturas MIMD sofrem com o problema de "Deadlock" e como evitá-lo? 

# 6. O que acontece se dois processadores tentarem alterar o mesmo dado na memória compartilhada?

# 7. Qual é o maior desafio ao programar para sistemas MIMD?

# 8. Onde a arquitetura MIMD é usada no dia a dia?

# 9. Qual é a principal vantagem da memória distribuída em relação à compartilhada?

# 10. O que pode ser a continuação da arquitetura MIMD ?