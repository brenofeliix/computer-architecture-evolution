# 4. Memory Organization

## 4.1 Organização do Armazenamento
A Arquitetura de Harvard organiza o armazenamento do computador em dois espaços distintos: a memória de instruções e a memória de dados. A memória de instruções é responsável por armazenar os comandos que orientam o processador sobre quais operações devem ser executadas, enquanto a memória de dados guarda as informações utilizadas durante o processamento, como números, variáveis e resultados intermediários. Essa separação permite que o processador busque instruções e acesse dados simultaneamente, desde que a implementação possua caminhos de comunicação independentes e recursos adequados. Dessa forma, é possível reduzir a disputa pelo acesso à memória e melhorar o desempenho do sistema. Apesar de desempenharem funções diferentes, os dois espaços trabalham em conjunto durante a execução dos programas: as instruções determinam as operações, e os dados fornecem as informações necessárias para realizá-las. É importante destacar que a Arquitetura de Harvard não exige uma tecnologia específica para essas memórias, pois seu princípio fundamental está na separação entre o armazenamento e o acesso às instruções e aos dados.

## 4.2 Armazenamento de Programa e Instruções
Para manter e acessar instruções, o modelo geral dita que o sistema utiliza um ponteiro de endereço (Program Counter) para localizar, por meio de barramentos específicos, o código armazenado em um espaço de memória dedicado e enviá-lo ao registrador da unidade de controle para decodificação e execução sequencial. No entanto, a separação estrutural de barramentos característica da arquitetura Harvard define estritamente a independência física ou lógica entre os caminhos de dados e de código, não se limitando ao uso de memórias não voláteis como ROM ou Flash; um exemplo concreto e prático disso ocorre nos processadores modernos com arquitetura Harvard modificada, nos quais o código mantido na RAM principal é transferido para uma Cache L1 de Instruções (L1i) composta por SRAM de alta velocidade, permitindo que a CPU busque a próxima instrução no barramento de código ao mesmo tempo em que lê ou escreve dados na Cache L1d.

## 4.3 Memória de Dados e Armazenamento Temporário
Na organização Harvard, os dados usados durante a execução ficam em um espaço separado daquele usado para armazenar as instruções. Os valores podem ser mantidos na memória de dados ou, conforme a implementação, em registradores e acumuladores. Registradores são pequenas unidades de armazenamento dentro do processador, usadas para guardar valores que ele está utilizando; eles não devem ser confundidos com a memória RAM. A arquitetura Harvard, por si só, não determina qual tecnologia será usada nem a quantidade de memória disponível. 
Um exemplo histórico associado à Harvard é o Mark I. Segundo a coleção de instrumentos científicos da Universidade Harvard, ele tinha 72 contadores de armazenamento, construídos a partir de máquinas mecânicas de soma modificadas. Eles guardavam valores usados nos cálculos e eram lidos e operados conforme as instruções da fita de sequência. Esse exemplo mostra que o armazenamento de dados não precisa ser feito com RAM: a tecnologia depende do sistema específico. As características do Mark I descrevem essa máquina, não toda implementação Harvard.

## 4.4 Configurações e Dados Persistentes
No contexto da arquitetura de software, informações e configurações duradouras que precisam persistir além do ciclo de vida de uma execução do programa, são mantidas na camada de persistência. Essa camada geralmente é composta por sistemas de gerenciamento de bancos de dados (relacionais ou NoSQL), sistemas de arquivos estruturados (como arquivos JSON, XML ou YAML para configurações de ambiente) ou serviços de armazenamento em nuvem. A responsabilidade dessa estrutura é garantir propriedades fundamentais como a durabilidade e a consistência dos dados, isolando o estado da aplicação de falhas de hardware ou reinicializações do sistema operacional.
Por outro lado, o processo de entrada e saída (I/O), foca estritamente na camada de apresentação ou de interface, tratando do fluxo de dados em tempo de execução. Enquanto a persistência lida com o armazenamento estático e de longo prazo, os mecanismos de I/O são dinâmicos e efêmeros, responsáveis por capturar as requisições do usuário (entradas) e exibir as respostas processadas (saídas) por meio de consoles, APIs REST ou interfaces gráficas. Portanto, a distinção arquitetural reside no propósito: o I/O gerencia a comunicação imediata e a transformação dos dados trafegados entre o usuário e o sistema, enquanto o armazenamento lida com a retenção histórica e a integridade desses dados após o término dessa interação.

[ Usuário / Cliente ]
                 │
                 ▼  (Dados Efêmeros / Parâmetros)
┌─────────────────────────────────────────────────────────┐
│              CAMADA DE INTERFACE (I/O)                  │
│  - Captura as entradas do usuário                       │
│  - Exibe os resultados e saídas processadas             │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼ (Dados Validados)
┌─────────────────────────────────────────────────────────┐
│               CAMADA DE LÓGICA / NEGÓCIO                │
│  - Processa as regras da Entrega 03                     │
│  - Toma decisões de roteamento e cálculo                │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼ (Acesso ao Estado/Configuração)
┌─────────────────────────────────────────────────────────┐
│                 CAMADA DE PERSISTÊNCIA                  │
│  - Armazenamento duradouro (Arquivos JSON/YAML/BD)      │
│  - Mantém informações que sobrevivem ao fim do processo │
└─────────────────────────────────────────────────────────┘

## 4.5 Tecnologia, capacidade e acesso
Na Arquitetura Harvard clássica, os limites de armazenamento e velocidade dependem estritamente das tecnologias físicas adotadas na implementação analisada, não sendo uma limitação do modelo teórico em si. 

Tomando como caso concreto o **Harvard Mark I (IBM ASCC)**, o sistema utilizava suportes eletromecânicos e físicos distintos para guardar instruções e dados:

### 1. Tecnologias Utilizadas
* **Memória de Programa (Instruções):** Utilizava fita de papel perfurada de 24 canais. As instruções eram lidas mecanicamente sequência a sequência através de contactos elétricos que detetavam os furos na fita.
* **Memória de Dados (Variáveis e Registos):** Utilizava relés eletromecânicos e contadores de rodas dentadas (acumuladores mecânicos) para armazenar valores numéricos temporários durante os cálculos.

### 2. Capacidade de Armazenamento
* **Capacidade de Dados:** O sistema possuía 72 acumuladores mecânicos principais para armazenamento de dados temporários, onde cada acumulador conseguia guardar um número de até 23 dígitos decimais (mais um sinal positivo/negativo).
* **Capacidade de Instruções:** Praticamente ilimitada na prática, pois dependia apenas do comprimento físico da fita de papel perfurada fornecida ao leitor.

### 3. Velocidade e Limitações de Acesso
* **Velocidade de Leitura de Instruções:** A leitura da fita de papel era limitada pelo avanço mecânico do leitor, operando a uma taxa aproximada de **200 passos por minuto** (cerca de 3,3 instruções por segundo).
* **Tempo de Acesso aos Dados:** O tempo de acesso e modificação dos acumuladores mecânicos era determinado pela rotação dos eixos e acionamento dos relés, levando cerca de **0,3 segundos para adições/subtrações** e até **6 segundos para multiplicações**.
* **Impacto no Funcionamento do Sistema:** A extrema lentidão no acesso mecânico e a impossibilidade de realizar desvios condicionais simples na fita (saltos/loops dinâmicos sem intervenção manual ou fitas em loop fechado) faziam com que o tempo global de execução dependesse do ritmo dos componentes eletromecânicos e da preparação correta das mídias físicas.

## 4.6 Aplicabilidade dos Conceitos do Template

A arquitetura Harvard define-se estritamente pela separação lógica e de barramentos entre o armazenamento, não exigindo tecnologias de hardware específicas. Os tipos de memória associados a ela dividem-se entre os espaços conceituais exigidos pelo modelo e as tecnologias físicas adotadas no mundo real.

Memória de Instruções (Program Memory)

Função: Armazenar os comandos, o código compilado ou as sequências de operações que o processador deve buscar e executar.

Classificação: Característica geral e obrigatória. A existência de um espaço exclusivo para as instruções, com caminhos de comunicação próprios, é o pilar da arquitetura Harvard.

Memória de Dados (Data Memory)

Função: Guardar variáveis, resultados intermediários, pilhas de execução (stack) e valores lidos ou escritos durante o processamento.

Classificação: Característica geral e obrigatória. O isolamento deste espaço em relação à memória de instruções permite que o processador leia uma instrução e acesse um dado simultaneamente.

Memória Flash / EEPROM / Mask ROM

Função: Prover armazenamento de leitura rápida e retenção não-volátil para o código do programa.

Classificação: Implementação específica. É a tecnologia padrão adotada para materializar a Memória de Instruções em microcontroladores modernos (como a família AVR, base do Arduino ou PICs), garantindo que o programa persista sem energia.

SRAM (Static RAM)

Função: Fornecer armazenamento volátil de altíssima velocidade para leitura e escrita em tempo de execução.

Classificação: Implementação específica. É frequentemente a tecnologia escolhida para materializar a Memória de Dados no chip de microcontroladores, pois dispensa os ciclos de atualização (refresh) exigidos por memórias RAM comuns.

Memória Cache L1 (Dividida em I-Cache e D-Cache)

Função: Acelerar o processamento mantendo cópias locais de instruções e dados que estão em uso frequente, minimizando a lentidão no acesso a níveis de memória inferiores.

Classificação: Implementação específica. É a base da chamada "Arquitetura Harvard Modificada". Em processadores modernos (como ARM Cortex-A, Intel, AMD), a memória principal costuma ser unificada, mas a memória Cache L1 dentro do processador é fisicamente separada entre Cache de Instruções e Cache de Dados para garantir o desempenho simultâneo característico do modelo Harvard.

Mídias Físicas e Eletromecânicas (Fitas de Papel e Contadores)

Função: Fitas de papel perfuradas ditavam as instruções em sequência, enquanto contadores mecânicos de rodas dentadas e relés armazenavam e operavam os valores matemáticos.

Classificação: Implementação específica e histórica. Foi o mecanismo utilizado no computador Harvard Mark I original, demonstrando que a arquitetura independe completamente da existência de RAM, ROM ou chips de silício.

