# 4. Memory Organization
Na arquitetura MIMD, vários processadores executam diferentes instruções sobre diferentes conjuntos de dados simultaneamente, é necessário organizar a memória de maneira eficiente para permitir o armazenamento, o acesso e o compartilhamento das informações, a organização influencia diretamente desempenho, velocidade de processamento e comunicação entre os processadores.

## 4.1 Memory Hierarchy
Na arquitetura MIMD, a hierarquia é importante porque os processadores precisam acessar dados constantemente durante a execução das tarefas e geralmente é organizada da seguinte forma:
Registradores: ficam dentro do processador e armazenam informações que estão sendo utilizadas imediatamente.
Cache: armazena temporariamente dados e instruções utilizados com frequência pelo processador.
RAM: guarda os programas e dados que estão sendo executados.
Armazenamento: guarda permanentemente arquivos, programas e informações.
Quanto mais próxima a memória estiver do processador, geralmente mais rápida será sua velocidade de acesso, porém menor será sua capacidade.
Na arquitetura MIMD cada processador pode possuir seus próprios registradores e caches, já a memória RAM pode ser compartilhada ou distribuída, dependendo da organização do sistema.

## 4.2 RAM
Na arquitetura MIMD, a RAM, memória principal do computador que armazena temporariamente os dados dos programas que estão sendo utilizados, pode funcionar de duas maneiras:
Memória compartilhada: todos os processadores acessam uma mesma memória principal.
Memória distribuída: cada processador possui sua própria memória local.

## 4.3 ROM
A ROM é uma memória não volátil que é utilizada para armazenar instruções e informações essenciais para o funcionamento do computador.
Suas principais funções incluem:
Armazenar o firmware do sistema.
Auxiliar no processo de inicialização do computador.
Guardar instruções básicas de funcionamento do hardware.
Na arquitetura MIMD a ROM não é responsável pelo processamento paralelo, mas fornece instruções fundamentais para a inicialização e o funcionamento do sistema. Nos dias de hoje, muitos sistemas utilizam memórias Flash para armazenar firmware, como BIOS e UEFI.

## 4.4 Cache
A memória cache é uma memória de alta velocidade que armazena temporariamente dados e instruções utilizados frequentemente pelo processador.
Sua principal função é reduzir o tempo necessário para acessar informações, evitando que o processador precise buscar constantemente os dados na memória RAM.
Existem três níveis principais:
Cache L1: muito rápida, pequena e geralmente exclusiva de cada núcleo.
Cache L2: maior capacidade que a L1, mas costuma ser um pouco mais lenta.
Cache L3: geralmente maior e frequentemente compartilhada entre vários núcleos do processador.
Na arquitetura MIMD cache é importante pois vários processadores podem trabalhar ao mesmo tempo, já em sistemas com memória compartilhada, é necessário manter a consistência dos dados entre as diferentes caches, conhecido como Cache Coherence.

## 4.5 Storage
Storage é o armazenamento permanente utilizado para guardar arquivos, programas, sistemas operacionais e documentos.
O armazenamento possui como principais dispositivos:
HDD: usa discos magnéticos para armazenar informações.
SDD: usa memória Flash, maaior velocidade de leitura e gravção.
NVMe: permite que SSDs se comuniquem com o sistema por meio de interfaces de alta velocidade, como PCIe.
Sistemas MIMD podem utilizar dispositivos de armazenamento compartilhados ou independentes para fornecer dados aos diferentes processadores.
SDD: usa memória Flash, maior velocidade de leitura e gravação.
NVMe: permite que SSDs se comuniquem com o sistema por meio de interfaces de alta velocidade, como PCIe.
Sistemas MIMD podem utilizar dispositivos de armazenamento compartilhados ou independentes para fornecer dados aos diferentes processadores.

## 4.6 Memory Capacity
A capacidade de memória representa a quantidade de informações que um sistema consegue armazenar.
Na arquitetura MIMD, essa capacidade depende da organização utilizada.
Sendo as duas principais:
1. Memória compartilhada
Todos os processadores utilizam uma mesma memória principal.
2. Memória distribuída
Cada processador ou computador possui sua própria memória.
A comunicação entre as memórias acontece por troca de mensagens.
A capacidade de memória influencia na quantidade de processos executados simultaneamente, no volume de dados que pode ser processado, o desempenho das aplicações e a escalabilidade do sistema.

## 4.7 Comparison with Modern Systems
A arquitetura MIMD é amplamente utilizada nos computadores modernos, principalmente em processadores multicore, servidores e supercomputadores.
Atualmente, os sistemas combinam diferentes tecnologias de memória para melhorar o desempenho e permitir o processamento paralelo.
MIMD tradicional- múltiplos processadores, memória compartilhada ou distribuída, cache por processador, HDD e outros dispositivos, barramentos e redes, capacidade dependente da organização.
Sistemas  Modernos- CPUs multicore e múltiplos processadores, DDR4, DDR5 e HBM, cache L1, L2 e L3 com diferentes organizações, SSD, NVMe e armazenamento distribuído, interconexões de alta velocidade, pode alcançar grandes capacidades em servidores e clusters.
Sistemas Modernos- CPUs multicore e múltiplos processadores, DDR4, DDR5 e HBM, cache L1, L2 e L3 com diferentes organizações, SSD, NVMe e armazenamento distribuído, interconexões de alta velocidade, pode alcançar grandes capacidades em servidores e clusters.
Exemplos de uso do MIMD são:
Computadores multicore
Servidores
Supercomputadores
Clusters
Um exemplo importante é um servidor moderno com dois processadores, cada um contendo vários núcleos, memória RAM de grande capacidade e armazenamento SSD NVMe. Os núcleos podem executar diferentes programas simultaneamente, seguindo o princípio MIMD.

## 4.8 Data Origin, Flow and Intermediate Results (Origem, Fluxo e Resultados Intermediários)
Para atender aos requisitos de acesso a dados na arquitetura MIMD:
- **Origem dos Dados:** Os dados primários originam-se em dispositivos de armazenamento de massa (SSDs NVMe, HDDs ou armazenamento distribuído em rede) e são carregados para a memória principal (RAM) ou distribuídos diretamente para as memórias locais dos nós.
- **Resultados Intermediários:** Durante o processamento paralelo, os resultados intermediários de cálculos lógicos e aritméticos são mantidos primariamente nos **registradores locais** de cada núcleo para acesso imediato, ou descarregados nas **caches de nível L1/L2** dedicadas. Em operações colaborativas de grande porte, são armazenados temporariamente na RAM compartilhada ou enviados a outros nós via rede de interconexão.
- **Representação do Caminho dos Dados (Data Path):**
  `Armazenamento (SSD/NVMe/Rede) ➔ Memória Principal (RAM Compartilhada ou Distribuída) ➔ Barramentos / Rede de Interconexão ➔ Cache L3/L2 ➔ Cache L1 ➔ Registradores do Núcleo ➔ Unidade Lógica e Aritmética (ALU).`
