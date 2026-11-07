# 7. Instruction Set

## 7.1 ISA Overview
Em arquiteturas MIMD (Multiple Instruction, Multiple Data), o sistema não possui uma única ISA (Instruction Set Arquitecture) centralizada e sequencial. Em vez disso, o sistema opera descentralizadoatravés de múltiplos fluxos de controle independentes. Diferente do modelo tradicional de SISD (Von Neumann) que existe apenas um Contador de Programa (PC - Program Counter), na arquitetura MIMD cada núcleo de processamento possui sua própria Unidade de Controle (UC) e seu próprio PC. Na memória compartilhada (SMP - Symmetric Multiprocessing) o sistema recebe a definição de execução via múltiplos threads ou processos compartilhando o mesmo espaço de endereçamento. O acesso aos dados é feito diretamento na RAM comum, dispensando tranferênncia de dados. Na memória distribuída (Clusters) cada nó possui sua memória local isolada. O sistema recebe tarefas divididas em subproblemas sem acesso direto à memória de outros nós, exigindo coordenação via comunicação em rede.

## 7.2 Instruction Types
As arquiteturas MIMD suportam tanto as instruções convencionais de cada núcleo isolado quanto operações exclusivas para gerenciar a paralização. Nas instruções aritméticas e lógicas na memória compartilhada são executadas de forma autônoma por cada núcleo sobre partes distintas de dados e para evitar conflitos de condição de corrida (race conditions) quando múltiplas threads alteram a mesma região de memória é feito as primitivas de sincronização como operações atômicas(ex: Test-and-Set, Compare-and-Swap), mutexes e semáforos. Já na computação local na memória distribuída ocorre quando as instruções nativas do processador opera sobre dados da memória local de cada nó. Sendo feita as primitivas de comunicação em rede, que são instruções de passagem de mensagem explícitas para envio e recebimento de dados entre diferentes máquinas do cluster.

## 7.3 Addressing Modes
Nos modelos de endereçamentos em memória compartilhada é feito acesso direto ao Heap, ou seja, os operandos e parâmetros podem residir na memória RAM globalou nos registradores locais de cada núcleo. Também, é feito o mapameamento físico dos operandos e threads a núcleos de CPU específicos (Thread Affinity) e, por fim, exige gerenciamento via hardware para evitar inconsistências em cópias locais mantidas nos caches L1/L2/L3 de cada processador, ou seja, é realizado a coerência de cache. Para o endereçamento nos modelos de memória distribuída cada nó  acessa estritamente seus operandos locais, além disso os dados remotos exigem rotinas de mapeamento lógico, endereçamento IP/Porta e topologia de rede para rotear mensagens de atualização de parâmetros.

## 7.4 Assembly Example

import threading

#Função executada por cada thread (representa uma tarefa autônoma)
def tarefa(id_nucleo, dado):
    resultado = dado * 2
    print(f"[Núcleo {id_nucleo}] Instrução executada sobre o dado {dado} -> Resultado: {resultado}")

#Dados diferentes para cada thread (Multiple Data)
dados = [10, 20, 30]
threads = []

#Criando múltiplos fluxos de execução independentes (Multiple Instruction)
for i, d in enumerate(dados, start=1):
    t = threading.Thread(target=tarefa, args=(i, d))
    threads.append(t)
    t.start() # Dispara o fluxo paralelo em cada núcleo

#Aguarda a finalização de todas as threads
for t in threads:
    t.join()

## 7.5 Execution Example

1. ​Definição de Tarefas e Dados (Multiple Data): Criamos uma lista dados = [10, 20, 30]. Cada elemento será entregue a uma thread separada para ser processado de forma independente.
2. ​Criação das Threads (Multiple Instruction): No loop for, instanciamos três threads. Cada thread atua como uma Unidade de Controle (UC) autônoma com seu próprio Contador de Programa (PC).
3. ​Execução Autônoma (t.start()): O comando t.start() instrui o sistema operacional e o hardware a alocar a execução de cada thread em núcleos de processamento distintos (quando disponíveis). O Núcleo 1 calcula 10 vezes 2, o Núcleo 2 calcula 20 vezes 2 e o Núcleo 3 calcula 30 vezes 2 simultaneamente.
4. ​Assincronismo: Como cada núcleo opera de forma autônoma, a ordem de impressão dos resultados no terminal pode variar (não-determinismo), demonstrando a ausência de um relógio sequencial único.
5. ​Sincronização Final (t.join()): O loop final aguarda a conclusão de todos os núcleos antes de encerrar o programa principal.

## 7.6 Modern Architectures

Nas arquiteturas modernas de processadores (como as famílias x86 e ARM), o modelo MIMD manifesta-se principalmente através do suporte a multiprocessamento simétrico (SMP) em nível de chip. Os processadores modernos integram múltiplos núcleos físicos e lógicos, onde cada um possui seus próprios registradores e caches locais (L1/L2), enquanto compartilham níveis superiores de memória cache (L3) e a memória RAM principal. Em contraste com os sistemas de memória distribuída (clusters), onde o compartilhamento de dados exige explicitamente o envio de mensagens através de protocolos de rede como o MPI, os sistemas SMP modernos permitem uma programação mais direta, onde as threads leem e escrevem em um espaço de endereçamento comum. No entanto, enquanto a escalabilidade dos sistemas SMP é limitada pela capacidade física e pelos gargalos de barramento de uma única máquina, os clusters conseguem escalabilidade quase ilimitada adicionando novos nós à rede, tornando-se o padrão dominante para supercomputadores e processamento de Big Data.
​Apesar da alta capacidade de processamento oferecida pelo modelo MIMD em sistemas modernos, a programação paralela introduz limitações teóricas e práticas significativas. A primeira delas é descrita pela Lei de Amdahl, que estabelece que o ganho máximo de velocidade de um programa em uma arquitetura paralela é estritamente limitado pela fração do código que permanece inerentemente sequencial. Além disso, a complexidade do software aumenta consideravelmente devido ao assincronismo dos núcleos: como a ordem de execução deixa de ser determinística, o sistema torna-se vulnerável a falhas graves de concorrência, como deadlocks (bloqueios mútuos entre processos), starvation (privação de recursos) e condições de corrida (race conditions). Por fim, existe um overhead substancial de infraestrutura e comunicação; em sistemas SMP, o tráfego excessivo para manter a coerência das caches pode degradar o desempenho, enquanto em clusters a latência e a largura de banda da rede de interconexão frequentemente se tornam o principal gargalo da aplicação.
