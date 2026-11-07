## 5. Bus Architecture (Arquitetura de Barramento)

## 5.1 Bus Overview (Visão Geral do Barramento)
A arquitetura MIMD (Multiple Instruction, Multiple Data) na Taxonomia de Flynn carateza sistemas paralelos nos quais múltiplos núcleos/ processadores executam intruções diferentes de forma assíncrona sobre conjuntos de dados distintos. A organização do sistema de comunicação e barramentos na MIMD varia conforme o paradigma da memória adotado:

Memória Compartilhada MIMD (Sistemas Fortemente Acoplados/ SMP/ NUMA/ UMA): Aplicação é direta, ou seja, múltiplos processadores compartilham um barramento de sistemas para acessar a memória principal compartilhada e os módulos de E/S.

Memória Distribuída MIMD (Sistemas Fracamente Acoplados/ Clusters/ MPPs): Cada nó possui seu próprio processador, memória local e barramento interno de sistema. A comunicação global entre nós não utiliza um barramento centralizado tradicional, eles utilizam uma Rede de Interconexão (Interconnection Network) ou barramentos/ chaves de alta velocidade baseados em troca de menssagens (Message Passing).

## 5.2 Data Bus (Barramento de Dados)
O barramento de dados é classificado como aplicação direta em sistemas MIMD de memória compartilhada, e também no barramento local de cada nó em sistemas de memória distribuída. Trate-se do conjunto de linhas físicas responsáveis por transportar as intruções lidas da memória e os operandos lidos/ escritos entre processadores, memórias e dispositivos.

## 5.3 Address Bus (Barramento de Endereço)
O barramentio de endereços é classificado como aplicação direta em sistemas SMP/UMA e como equivalente técnico em sistemas NUMA e de memória distribuída. Sua função principal é de transportar os endereços físicos de memória ou endereços de E/S emitidos por um processador que deseja realizar uma operação de leitura ou escrita. A estrutura é divida em duas partes:

Memória Compartilhada (UMA): Todos os processadores enxergam um único espaço de endereçamento contínuo no barramento de endereços compartilhado.

Memória Distribuída: Cada processador acessa apenas seu espaço de endereçamento local através de seu próprio barramento de endereços locais. Para acessar a memória de outro nó utiliza-se envio de pacotes de dados via rede de interconexão contendo o Id do nó de destino e o deslocamento de memória (Remote Direct Memory Acess - RDMA ou Message Passing).

## 5.4 Control Bus (Barramento de Controle)
O barramento de controle é classificado como aplicação direta em barramentos compartilhados e como equivalente técnico em redes de interconexão. Sua função principal é de transportar sinais de controle, temporização, requisição, autorização e status. Entre os mecanismos e sinais específicos na MIMD, destacam-se:

Sinais de Arbitragem de Barramento (Bus Request/ Bus Grant): Coordenam qual processador recebe permissão so árbitro central ou distribuído para utilizar o barramento.

Sinais de Leitura/Escrita e Confirmação (Read/Write e ACK/ NACK): Sinalizam operações de leitura, escrita e término da tranferência.

Sinais de Coerência de Cache (Invalidate / Read Invalidate / Bus Lock): Essenciais para garantir que, quando um processador altera uma variável na memória compartilhada, as cópias armazenadas nas caches dos demais processadores sejam invalidadas ou atualizadas.

Sinais de Interrupção Inter-processador (IPI - Inter-Processor Interrupts): Permitem que um núcleo envie interrupções diretas a outro núcleo para sincronização ou agendamento de tarefas.

## 5.5 Communication Process (Processo de Comunicação)
O processo de comunicação entre a CPU e a memória em uma arquitetura MIMD varia de acordo com o paradigma da arquitetura:

Memória Compartilhada MIMD (Ex: Barramento Único Compartilhado com Snooping): Requisição de Acesso: A CPU P1 precisa de um dado não presente em sua cache local (Cache Miss). A CPU emite um sinal de Bus Request no barramento de controle. Arbitragem: O árbitro de barramento avalia as requisições pendentes de todas as CPUs e concede permissão (Bus Grant) para P1. Endereçamento e Snooping: P1 coloca o endereço no Address Bus e o comando de leitura no Control Bus. As outras CPUs (P2... PN) monitoram o barramento (snoop) para verificar se possuem a linha de cache modificada correspondente àquele endereço. Transferência de Dados: Se nenhuma outra cache possuir o dado modificado, o módulo de Memória Compartilhada responde colocando o bloco de dados no Data Bus. Se outra CPU (P2) possuir o dado atualizado em sua cache local, P2 intercepta a resposta e envia o dado diretamente pelo Data Bus para P1 e atualiza a memória. Finalização: P1 captura os dados do Data Bus e o barramento é liberado para o próximo ciclo de arbitragem.

Memória distribuída MIMD (Ex: Message Passing) Solicitação Local: P1 determina que precisa de dados localizados na memória do nó N2. Empacotamento de Mensagem: P1 constrói uma mensagem via software (ex: biblioteca MPI) ou hardware contendo o nó de destino (N2), tamanho da mensagem e dados/comando. Envio via Interface de Rede: A mensagem é enviada pelo barramento local de P1 para a sua interface de rede local. Roteamento: A mensagem trafega pela rede de interconexão (ex: Mesh, Hypercube, Crossbar) até o nó N2. Processamento e Resposta: O controlador do nó N2 lê a memória local de N2 via seu barramento interno e envia uma mensagem de resposta com os dados de volta para P1.

## 5.6 Bus Width (Largura do Barramento)
​A largura do barramento classifica-se como Aplicação Direta nos barramentos locais e de memória, e como Equivalente Técnico na largura dos canais e links das redes de interconexão.​Sua análise dimensional abrange:​Barramento de Dados (Data Bus Width): Apresenta dimensões típicas de 64, 128, 256 ou 512 bits — ou a largura de uma linha de cache completa em transferências por bloco via modo burst — visando maximizar o rendimento da memória compartilhada.

​Barramento de Endereços (Address Bus Width): Varia geralmente de 32 a 64 bits, definindo o limite físico e o espaço de endereçamento da memória compartilhada.

​Canais de Interconexão em Redes de Mensagem: Definidos pela largura do flit ou phit (normalmente 16, 32 ou 64 bits em paralelo por link físico ou via interfaces seriais de alta velocidade multipista, como PCIe e InfiniBand).

## 5.7 Performance Impact (Impacto no Desempenho)
​O projeto do barramento e dos mecanismos de comunicação representa o principal gargalo de desempenho nas arquiteturas MIMD:

​Contenção de Barramento (Bus Contention): Em sistemas de barramento único compartilhado, a quantidade de processadores escaláveis é severamente limitada (em geral entre 8 e 32 processadores), pois a tentativa simultânea de acesso à memória por múltiplos núcleos gera contenção e altas latências de espera.

Overhead de Coerência de Cache (Cache Coherence Overhead): O volume extra de mensagens de invalidação e atualização transmitido no barramento de controle tende a saturar a largura de banda disponível à medida que o número de núcleos aumenta.

​Latência de Comunicação Inter-nós: Em sistemas distribuídos, a latência de transferência de dados via Message Passing é significativamente maior do que o acesso local à memória, demandando algoritmos otimizados para minimizar a troca de dados.

Escalabilidade (Scalability): Barramentos compartilhados possuem baixa escalabilidade (O(N) de contenção). Em contrapartida, redes de interconexão dinâmicas (Crossbar) ou estáticas (Mesh, Hypercube) oferecem suporte para centenas ou milhares de processadores, embora aumentem a complexidade do hardware.

## 5.8 Comparison with Current Architectures (Comparação com Arquiteturas Atuais)
Topologia de Barramento/Conexão: Memória Compartilhada MIMD (Histórica/Tradicional ex: Sequent Symmetry): Barramento físico único e compartilhado (Shared Multi-drop Bus). Memória Distribuída MIMD (ex: Clusters HPC / Supercomputadores): Redes de interconexão (Mesh, Torus, Hypercube, InfiniBand).​Arquiteturas Modernas de Múltiplos Núcleos (Multicore / NUMA / SOC): Malhas no-chip (Network-on-Chip - NoC), Crossbar interno e links ponto a ponto (PCIe, CXL, UPI/Infinity Fabric).

​Acesso à Memória: Memória Compartilhada MIMD: UMA (Uniform Memory Access), com latência uniforme para qualquer endereço lido por qualquer CPU. Memória Distribuída MIMD: NORMA (No Remote Memory Access), onde apenas a memória local é acessada diretamente por instruções do processador.​Arquiteturas Modernas: NUMA (Non-Uniform Memory Access), onde o acesso à memória local do socket é consideravelmente mais rápido do que à memória remota.

​Escalabilidade: Memória Compartilhada MIMD: Baixa, devido à rápida saturação do barramento compartilhado. Memória Distribuída MIMD: Altíssima, permitindo dimensionamento para milhares de nós e processadores.​Arquiteturas Modernas: Média a Alta em nível de chip/nó (de dezenas a centenas de núcleos por sistema).

​Mecanismo de Comunicação Principal: Memória Compartilhada MIMD: Leitura/escrita direta na memória compartilhada com coerência de cache mantida por Snooping. Memória Distribuída MIMD: Troca explícita de mensagens via software/hardware (Message Passing / MPI).​Arquiteturas Modernas: Protocolos de coerência baseados em diretórios (Directory-based) sobre redes NoC e uso de CXL para expansão de memória.

​Contenção e Gargalo: Memória Compartilhada MIMD: Disputa direta e simultânea pelos barramentos de controle, dados e endereços. Memória Distribuída MIMD: Latência de rede e custo de empacotamento das mensagens.​Arquiteturas Modernas: Congestionamento na malha interna (NoC) e gargalos de largura de banda no controlador de memória.