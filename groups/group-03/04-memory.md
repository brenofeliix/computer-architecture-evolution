# 4. ARMAZENAMENTO E GESTÃO DE ESTADOS NA ARQUITETURA DO PROTÓTIPO

## 4.1 Formas de Armazenamento Temporário e Volátil

O armazenamento temporário constitui a camada responsável por manter os dados de forma volátil durante as etapas de aquisição, transmissão de rede, preparação estrutural e processamento analítico. Na arquitetura definida para este protótipo, essa função é distribuída entre a memória interna do microcontrolador ESP32 e a memória principal (RAM) do computador local que vai atuar como servidor. Ambas as tecnologias operam com semicondutores de acesso aleatório voláteis, o que significa que seus estados dependem da manutenção da alimentação elétrica. Suas funções arquiteturais são complementares: a primeira atende às demandas de buffers do firmware na ponta da aquisição, enquanto a segunda sustenta a organização do fluxo linear de dados recebidos e sua disponibilização para as instruções analíticas.

### 4.1.1 Memória SRAM do ESP32

A memória SRAM (*Static Random-Access Memory*) integrada ao chip SoC (*System on a Chip*) da série ESP32 (variante ESP32-D0WD-V3) é utilizada para armazenar temporariamente as variáveis de execução, as pilhas de tarefas (*stacks*), as estruturas de controle do firmware e os buffers de comunicação necessários à execução do programa. Por sua natureza física baseada em células de seis transistores (6T SRAM) que não necessitam de ciclos de atualização (*refresh*), ela oferece tempos de acesso coerentes com o ciclo de clock do processador, operando com baixa latência na manipulação imediata de dados.

No protótipo estabelecido, a SRAM retém as leituras consecutivas obtidas do sensor de temperatura. O firmware aloca uma estrutura estática de dados (*struct*) que atua como buffer de aquisição para armazenar a amostra numérica de temperatura antes de seu encapsulamento em formato de rede (JSON). Esse arranjo organiza as informações de forma contígua na memória volátil antes do acionamento da pilha de protocolos de comunicação.

A dimensão desse buffer foi projetada para ser estável, operando de forma síncrona com a frequência de amostragem definida de 1 Hz. Esse dimensionamento de projeto visa mitigar os riscos de fragmentação do heap e falhas de alocação dinâmica, sendo sua estabilidade contínua e adequação prática sob estresse operacional critérios avaliados durante a futura fase de testes do protótipo.

A alocação interna da SRAM divide o espaço de endereçamento entre o sistema de execução básico, o núcleo do sistema operacional de tempo real (FreeRTOS) e as variáveis de controle das pilhas de rádio (Wi-Fi e TCP/IP). O firmware organiza as variáveis de estado operacional — que indicam se a aquisição de temperatura está ativa ou se há conexões Wi-Fi pendentes —, de modo a separar a volatilidade inerente aos dados de amostragem contínua da lógica de controle do microcontrolador. A adequação dessa estrutura lógica contra travamentos e perdas de pacotes será verificada sistematicamente nos testes.

### 4.1.2 Memória RAM do Computador e Buffers de Processamento

No computador selecionado para atuar como servidor local do sistema, a memória principal RAM (*Random-Access Memory*), do tipo DRAM (*Dynamic RAM*), mantém os dados brutos recebidos da interface de rede, as estruturas de dados dinâmicas instanciadas pela aplicação Python e os estados intermediários gerados durante o processamento das regras analíticas. O acesso direto à RAM foi definido para permitir que as rotinas de software executem as validações de consistência e as varreduras lógicas de algoritmos sem a latência imposta por acessos repetitivos ao subsistema de armazenamento em massa.

Assim que um pacote contendo a amostra de temperatura chega pela interface de rede, o programa Python faz a validação de integridade do cabeçalho e insere os dados em uma estrutura de fila na memória RAM. Para garantir a integridade do processamento posterior, cada leitura é consolidada em um registro lógico único que contém: o identificador sequencial da amostra (gerado pelo firmware na origem), a marca temporal (*timestamp*) de recepção fornecida pelo sistema operacional e a temperatura medida.

Nesse arranjo de software, a fila hospedada em RAM funciona como uma área de desacoplamento entre o subsistema de rede e o motor analítico. Caso a taxa de chegada de mensagens sofra oscilações temporárias causadas por variações da rede Wi-Fi, os registros aguardam ordenados na memória volátil até que as rotinas de análise finalizem suas tarefas pendentes.

A aplicação Python gerencia esse espaço de trabalho de modo a reter o registro de temperatura na memória RAM até que as etapas de processamento analítico e a preparação dos comandos SQL sejam concluídas. Essa estratégia de gerenciamento visa mitigar riscos de descarte prematuro de dados durante o ciclo de processamento ativo, tratando a RAM como o ambiente de manipulação imediata do ecossistema, cuja eficiência e consumo real de recursos serão quantificados experimentalmente na homologação do software.

## 4.2 Formas de Armazenamento Permanente e Persistência

O armazenamento permanente compreende os mecanismos físicos e lógicos integrados ao protótipo para conservar de forma estável o código executável do sistema, as configurações essenciais de conectividade e o histórico completo de medições, garantindo a integridade dos estados operacionais mesmo após desligamentos mecânicos ou falhas de energia do hardware. No ecossistema do projeto, essa persistência é distribuída entre a memória Flash não volátil do ESP32 e as tabelas estruturadas no Sistema Gerenciador de Banco de Dados Relacional (SGBDR) MySQL, instalado no computador local.

O projeto separa a persistência do código de controle (firmware) da persistência das séries temporais geradas pelo sensor. O firmware reside fixo no microcontrolador, enquanto o banco de dados centraliza as informações históricas estruturadas para consultas futuras e exibição na interface do usuário.

### 4.2.1 Memória Flash do ESP32

A memória Flash SPI integrada ao módulo ESP32-WROOM-32E-N4 funciona como o armazenamento não volátil responsável por resguardar o firmware desenvolvido em C/C++ e os parâmetros permanentes de inicialização. Ao contrário da SRAM, seu conteúdo baseado em células flutuantes de silício (tecnologia NOR) é preservado após a interrupção da alimentação elétrica do hardware, fazendo com que o microcontrolador execute suas rotinas imediatamente após ser energizado, sem a necessidade de recargas externas do programa.

O espaço lógico da Flash é organizado por meio de uma tabela de partições padrão definida no ambiente de desenvolvimento, dividindo o chip nas seguintes regiões dedicadas:

*   **Partição da Aplicação (*Factory App*):** Região onde reside o binário compilado do firmware, contendo as instruções estáticas de controle do sensor de temperatura, empacotamento de protocolos e rotinas de tratamento de erros.
*   **Região NVS (*Non-Volatile Storage*):** Área configurada no código para salvar dados em formato de chave-valor. Ela foi selecionada para armazenar parâmetros persistentes essenciais, como os dados de autenticação (SSID e senha) da rede Wi-Fi local.

A Flash é tratada no firmware estritamente como uma memória de leitura durante a rotina padrão de monitoramento. O projeto proíbe a gravação contínua de amostras de temperatura na Flash do microcontrolador para evitar o desgaste prematuro das células devido aos ciclos limitados de apagamento e escrita dessa tecnologia. A gravação das credenciais de rede na NVS ocorre exclusivamente durante a configuração inicial do dispositivo; esse mecanismo representa uma decisão de armazenamento local para fins de reinicialização automática, não oferecendo isolamento criptográfico avançado contra extração física de hardware se recursos de criptografia da Flash não forem habilitados independentemente no chip.

### 4.2.2 Banco de Dados MySQL no Servidor Local

No computador local, a persistência definitiva e a manutenção histórica do fluxo de dados foram definidas por meio do SGBDR MySQL. Essa tecnologia foi selecionada por sua arquitetura cliente-servidor robusta, suporte completo a transações ACID (*Atomicity, Consistency, Isolation, Durability*) e ampla eficiência no gerenciamento de concorrência através do motor de armazenamento InnoDB, garantindo a integridade dos dados sob o fluxo constante de requisições enviadas pelo script Python.

O modelo de dados foi planejado para refletir a separação entre o dado de entrada coletado e as saídas analíticas geradas. As informações são organizadas em três tabelas principais interligadas por restrições de integridade referencial:

#### Tabela 1 — Estrutura relacional do banco de dados projetada no MySQL

| Tabela | Campos Principais | Finalidade e Descrição |
| :--- | :--- | :--- |
| **dispositivo** | id_dispositivo (PK), identificador, descricao | Cadastra a identidade e a descrição física do módulo ESP32 emissor. |
| **amostra** | id_amostra (PK), id_dispositivo (FK), timestamp_recepcao, temperatura | Registra a medição bruta de temperatura recebida pelo computador e a marca temporal de recepção. |
| **resultado_analise** | id_resultado (PK), id_amostra (FK), tipo_analise, valor_calculado, status_alerta | Armazena de forma individual cada diagnóstico gerado pelas três rotinas analíticas para a amostra correspondente. |

A chave primária (PK) define a identificação única do registro e a chave estrangeira (FK) estabelece o vínculo relacional com as outras tabelas. Os tipos de dados foram mapeados de forma estrita: números inteiros para chaves, números de ponto flutuante (*floats*) para a grandeza de temperatura do sensor e campos do tipo `DATETIME` para as marcas temporais.

A tabela amostra preserva o dado bruto original de temperatura recebido pelo computador, enquanto a tabela resultado_analise registra os resultados individuais emitidos pelas três rotinas analíticas (Limites, Histórico e Anomalias). O esquema lógico adota um modelo em que cada amostra processada gera três registros independentes na tabela resultado_analise (um para cada tipo de análise). Essa separação visa garantir a rastreabilidade completa do sistema, permitindo correlacionar de forma direta cada alerta isolado com os dados físicos de temperatura que o motivaram.

Para assegurar a consistência dos estados permanentes, o script Python foi projetado para executar as operações de gravação utilizando transações SQL explícitas. A operação COMMIT conclui a transação segundo o mecanismo de persistência do MySQL. A aplicação utilizará essa confirmação como referência para o encerramento do processamento da amostra, desde que o fluxo de controle impeça sua liberação prematura da memória de trabalho.

## 4.3 Tecnologias de Armazenamento, Capacidades e Velocidades de Acesso

O protótipo utiliza uma hierarquia de armazenamento para equilibrar velocidade de processamento e segurança de persistência. A SRAM e a RAM DDR operam na camada de acesso direto ao processador, enquanto a Flash e o dispositivo de armazenamento em massa sólido (SSD) garantem a retenção permanente a longo prazo.

A Tabela 2 apresenta as especificações técnicas de referência dos componentes de hardware selecionados para compor o ecossistema do projeto.

#### Tabela 2 — Especificações previstas e nominais do hardware selecionado

| Tecnologia de Armazenamento | Componente Selecionado | Capacidade Nominal | Características de Acesso (Referência) | Papel no Protótipo |
| :--- | :--- | :--- | :--- | :--- |
| **SRAM Interna** | Chip SoC ESP32-D0WD-V3 | 520 KiB | Semicondutora volátil de acesso aleatório. | Buffers do sensor e variáveis locais do firmware em C/C++. |
| **Flash Externa** | Módulo ESP32-WROOM-32E-N4 | 4 MB | Semicondutora NOR não volátil (SPI). | Guarda o binário do firmware compilado e chaves NVS de rede. |
| **RAM DDR4 (Prevista)** | Computador Servidor Alvo | 16 GiB | Memória dinâmica volátil (DRAM) de alta densidade. | Hospeda as filas de entrada do script Python e objetos de análise. |
| **SSD NVMe (Previsto)** | Computador Servidor Alvo | 512 GB | Memória Flash NAND não volátil de alta velocidade. | Suporta a base de dados do MySQL e os logs do sistema. |

Fonte: Fundamentada nas especificações técnicas de referência da Espressif Systems e de Patterson e Hennessy (2021).

### 4.3.1 Capacidade e Latência na Memória Embarcada
Embora a documentação oficial da família ESP32 confirme a presença de 520 KiB de SRAM física total no SoC, o ambiente de desenvolvimento compila o firmware de modo a reservar fatias fixas para o controlador de rádio Wi-Fi e funções internas do núcleo do FreeRTOS. A aplicação do firmware foi dimensionada para consumir uma fração reduzida deste espaço para as estruturas de dados do sensor de temperatura, operando com uma margem de segurança voltada a evitar travamentos por estouro de memória, cuja estabilidade será verificada nos testes.

Na Flash integrada ao módulo, as operações de escrita ocorrem em blocos lógicos estruturados (setores), exigindo tempos de apagamento e gravação superiores aos ciclos de leitura com base nos dados de referência do fabricante. Por esse motivo, o firmware restringe o uso de funções de escrita na partição NVS apenas ao momento da configuração inicial do dispositivo, operando em modo de leitura estrita durante a rotina padrão de monitoramento para evitar o desgaste físico prematuro das células flutuantes.

### 4.3.2 Capacidade e Desempenho no Servidor
No computador local, a alocação de 16 GiB de memória RAM DDR4 fornece a configuração prevista para garantir que as filas de processamento em Python operem sem contenção de recursos com as demais tarefas do sistema operacional hospedeiro. O uso de estruturas nativas (como coleções do tipo deque) foi definido com o objetivo de manter a manipulação de dados eficiente durante as operações de inserção e remoção de registros na memória volátil.

A camada de persistência definitiva do banco de dados repousa sobre um SSD previsto com interface física NVMe M.2, cujas taxas nominais de leitura e escrita sequenciais minimizam gargalos físicos de busca comuns em mídias magnéticas. Ressalta-se que a velocidade da interface física e os algoritmos internos do SSD não garantem, por si só, a integridade das transações do banco de dados; essa propriedade depende diretamente da correta coordenação entre os comandos do MySQL, as configurações do motor InnoDB (como a liberação síncrona do log de transações) e as primitivas de escrita do sistema operacional.

## 4.4 Relação entre o Armazenamento e o Funcionamento do Sistema MISD

O arranjo do sistema de armazenamento constitui a base estrutural para implementar a arquitetura conceitual MISD (Multiple Instruction, Single Data) proposta no projeto. De acordo com a taxonomia de Flynn (1972), um sistema MISD exige que múltiplos fluxos de instruções operem concorrentemente sobre um único e idêntico fluxo de dados.

O protótipo demonstra esse princípio conceitual no ambiente de software do servidor local, utilizando a memória RAM como a região de compartilhamento central de dados. O dado de temperatura é lido da rede e organizado em uma estrutura lógica na RAM, que atua como o Fluxo Único de Dados (Single Data). O uso de um computador convencional para essa tarefa serve para fins de transmissão didática do fluxo lógico de processamento, não significando que o hardware do computador constitua, fisicamente, um processador estruturado sob a arquitetura MISD.

### 4.4.1 Manutenção do Fluxo Lógico Único
Para implementar a especificação de fluxo único de dados, o script Python foi projetado para instanciar as amostras recebidas de forma isolada na RAM. Uma vez criado o registro correspondente a uma leitura do sensor de temperatura, as rotinas analíticas realizam exclusivamente operações de leitura sobre esses valores lógicos, impedindo modificações colaterais durante o ciclo de processamento.

As três rotinas de processamento de software representam as Múltiplas Instruções (Multiple Instruction). Elas foram projetadas como funções independentes que acessam o mesmo endereço de memória via referências de dados ou ponteiros compartilhados. Cada algoritmo executa sua lógica matemática específica sobre o mesmo dado de temperatura:

Verificação do Limite: Comparação direta do valor atual com limites fixos parametrizados.
Comparação com o Histórico: Integração do dado atual com médias e valores históricos consultados na base de dados.
Identificação de Anomalias: Detecção de desvios estatísticos e variações fora do padrão comportamental.
Cada rotina publica seus resultados em canais ou estruturas isoladas no MySQL, preservando o fluxo de entrada idêntico para as demais instruções, conforme preconizado pela taxonomia de Flynn (1972).

#### Tabela 3 — Mapeamento de Múltiplas Instruções (MISD) e Ações de Persistência

| Rotina Analítica (Instrução) | Descrição Operacional da Ação | Destino de Persistência no MySQL |
| :--- | :--- | :--- |
| **Verificação do Limite** | Comparação direta do valor atual de temperatura com limites fixos parametrizados no sistema. | Tabela `resultado_analise` (`tipo_analise = 'Limite'`) |
| **Comparação com o Histórico** | Integração do dado atual com médias e valores históricos consultados previamente na base de dados. | Tabela `resultado_analise` (`tipo_analise = 'Historico'`) |
| **Identificação de Anomalias** | Detecção de desvios estatísticos e variações abruptas fora do padrão comportamental esperado. | Tabela `resultado_analise` (`tipo_analise = 'Anomalia'`) |

### 4.4.2 Compartilhamento de Memória e Concorrência

Na implementação do interpretador padrão do Python (CPython), o Global Interpreter Lock (GIL) restringe a execução simultânea de bytecode Python por múltiplas threads a um único núcleo de processamento. Desse modo, o uso de threads na aplicação Python atua como um mecanismo previsto para gerenciar a concorrência de tarefas de entrada e saída (como aguardar dados da rede ou interagir com o MySQL através de conexões de rede locais), mas não realiza o paralelismo computacional simultâneo em múltiplos núcleos de CPU.

Por esse motivo, as três rotinas analíticas foram projetadas como funções consecutivas ou concorrentes gerenciadas pelo interpretador, as quais utilizam a estrutura da amostra na RAM como constante compartilhada durante o ciclo de processamento. O modelo não assume a execução paralela simétrica em nível de hardware. O banco de dados relacional MySQL atua como o ponto de convergência final, unificando os vereditos produzidos pelas rotinas e associando-os à chave estrangeira da amostra geradora no armazenamento permanente.

## 4.5 Análise de Limitações, Gargalos e Restrições de Hardware

A eficiência planejada para a arquitetura depende do equilíbrio dinâmico entre a velocidade com que o ESP32 envia os dados e a capacidade do servidor de processá-los e persisti-los em disco. O comportamento de vazão e as restrições físicas teóricas do hardware foram investigados com base em características documentadas dos componentes.

### 4.5.1 Sobrecarga dos Buffers e do Banco de Dados

A principal limitação teórica considerada ocorre se a taxa média de chegada de registros de temperatura pela rede superar de forma contínua a taxa média de persistência e análise do sistema, forçando o acúmulo de mensagens na fila de entrada hospedada em RAM. Caso a fila atinja o limite máximo de sua capacidade física e operacional sob uma sobrecarga constante, o tempo disponível até que ocorra uma saturação por estouro de buffer reduz-se linearmente.
Para mitigar essa restrição e evitar o acúmulo excessivo de mensagens, a arquitetura do sistema prevê a otimização das operações de escrita no MySQL por meio do agrupamento de inserções em transações (Bulk Insert). O ciclo de vida previsto para o gerenciamento de cada amostra de temperatura na RAM segue um fluxo estruturado:

* **O pacote de dados é recebido pela interface socket e inserido na cauda da fila de entrada em RAM.**
* **As três instruções analíticas realizam a leitura concorrente do registro na fila.**
* **Os resultados gerados são agrupados e estruturados em memória na fila de preparação de comandos SQL.**
* **A aplicação envia o lote de operações e aguarda a conclusão da transação síncrona via primitiva COMMIT do MySQL.**
* **O registro correspondente é liberado da fila de entrada da RAM, desocupando o espaço de trabalho.**

### 4.5.2 Limitações de Escrita e Desgaste da Memória Flash

Como as memórias Flash baseadas em tecnologia NOR sofrem degradação física progressiva devido aos ciclos acumulados de apagamento e gravação em suas células, a arquitetura do firmware estabelece que não haverá escrita de amostras do sensor na Flash do ESP32.

Os dados medidos de temperatura passam exclusivamente pelas estruturas de buffer voláteis da SRAM. A Flash foi reservada para o armazenamento estático do firmware e para a leitura dos parâmetros de rede Wi-Fi durante a inicialização do hardware. Essa decisão de projeto elimina o desgaste prematuro do chip de memória do microcontrolador durante a operação padrão do sistema.

### 4.5.3 Latência de Rede e Consistência entre a Ponta e o Servidor
A transmissão de dados através da rede sem fio Wi-Fi introduz uma latência variável decorrente das condições do ambiente de rádio e retransmissões do protocolo TCP. Existe um intervalo de tempo entre o momento da coleta física no sensor e a confirmação de persistência no banco de dados.

Para solucionar os impactos da latência de rede e assegurar a consistência cronológica das informações, a arquitetura estabelece uma estratégia baseada no identificador sequencial de 16 bits gerado pelo firmware no instante da leitura. O firmware incrementa esse contador estático a cada amostragem, reiniciando-o automaticamente em zero ao atingir o limite de overflow (65.535). O servidor Python lê esse identificador e o associa ao carimbo de tempo (timestamp) de recepção fornecido pelo computador no momento da gravação no banco. Desse modo, o identificador sequencial atua como referência estável para ordenar as séries temporais de forma correta no MySQL e detectar eventuais lacunas provocadas por quedas de pacotes, independentemente de atrasos causados pela rede sem fio.

### 4.5.4 Comportamento Previsto em Falhas de Conectividade

A execução prevê uma política de contingência limitada para lidar com falhas temporárias de conexão Wi-Fi. Caso o canal de comunicação caia, o firmware do ESP32 suspende as tentativas imediatas de envio socket e armazena os registros lógicos em uma estrutura de fila circular estática pré-alocada na SRAM.

Por se tratar de um espaço volátil e restrito a 32 KiB dedicados, a fila circular adotará uma política de descarte por cauda (Drop-Tail): assim que o espaço em SRAM atingir sua capacidade máxima, as amostras novas serão descartadas para preservar o histórico cronológico inicial do início da falha. Quando a conectividade for restaurada, o firmware realiza o reenvio sequencial dos registros acumulados.

### 4.5.5 Síntese das Restrições e Medidas de Mitigação

#### Tabela 4 — Matriz de restrições de armazenamento e mitigações planejadas

| Natureza da Restrição | Consequência Potencial no Sistema | Solução Técnica de Mitigação Projetada |
| :--- | :--- | :--- |
| **Volatilidade da SRAM do ESP32** | Perda das medições em caso de queda de energia na ponta. | Envio imediato do registro formatado para o servidor central após a coleta. |
| **Limite físico de espaço na SRAM** | Falhas de alocação e travamento do microcontrolador. | Uso de buffers estáticos pré-alocados de tamanho fixo no firmware. |
| **Gargalo de I/O em escritas SQL** | Acúmulo de mensagens na fila e estouro da RAM do servidor. | Planejamento de transações estruturadas em lote (*Bulk Insert*) no MySQL. |
| **Desgaste por ciclos da Flash** | Falha permanente de hardware no chip do módulo ESP32. | Restrição de escritas de monitoramento na Flash, mantendo os dados em SRAM. |
| **Latência da interface Wi-Fi** | Inversão ou atraso na ordem das amostras recebidas. | Injeção de identificadores sequenciais na origem pelo firmware para ordenação. |
| **Queda de conexão com o servidor** | Perda de pacotes contínuos durante o período de isolamento. | Fila temporária na SRAM com política de descarte do tipo *Drop-Tail*. |
| **Restrições do GIL no Python** | Limitação na execução paralela de múltiplas threads de CPU. | Uso de dados imutáveis em RAM e foco em concorrência voltada a tarefas de I/O. |

A análise demonstra que a gestão combinada das memórias voláteis e permanentes organiza o armazenamento de modo a atender aos requisitos previstos para o protótipo. A validação prática dessas premissas, a confirmação do funcionamento estável do software sob cenários de falha e a medição do desempenho real do hardware serão realizadas de forma sistemática durante os futuros testes de homologação do protótipo.

## Referências
ESPRESSIF SYSTEMS. ESP32 Series Datasheet. v. 4.4. Shanghai: Espressif Systems, 2026. Disponível em: espressif.com. Acesso em: 18 out. 2026.

ESPRESSIF SYSTEMS. ESP32-WROOM-32E & ESP32-WROOM-32UE Datasheet. v. 1.5. Shanghai: Espressif Systems, 2026. Disponível em: espressif.com. Acesso em: 18 out. 2026.

ESPRESSIF SYSTEMS. ESP-IDF Programming Guide: Non-Volatile Storage Library. Shanghai: Espressif Systems, 2026. Disponível em: espressif.com. Acesso em: 18 out. 2026.

ESPRESSIF SYSTEMS. ESP-IDF Programming Guide: SPI Flash and Partition Tables. Shanghai: Espressif Systems, 2026. Disponível em: espressif.com. Acesso em: 18 out. 2026.

FLYNN, Michael J. Some computer organizations and their effectiveness. IEEE Transactions on Computers, New York, v. C-21, n. 9, p. 948-960, set. 1972. DOI: 10.1109/TC.1972.5009071.

ORACLE CORPORATION. MySQL 8.4 Reference Manual: InnoDB Storage Engine. Redwood Shores: Oracle Corporation, 2026. Disponível em: mysql.com. Acesso em: 18 out. 2026.
PATTERSON, David A.; HENNESSY, John L. Computer organization and design RISC-V edition: the hardware/software interface. 2. ed. Cambridge, MA: Morgan Kaufmann, 2021.

PYTHON SOFTWARE FOUNDATION. Python 3.15.0 Documentation: Threading in Python. Wilmington: Python Software Foundation, 2026. Disponível em: python.org. Acesso em: 18 out. 2026.

STALLINGS, William. Computer organization and architecture: designing for performance. 10. ed. Boston: Pearson, 2016.

TANENBAUM, Andrew S.; AUSTIN, Todd. Structured computer organization. 6. ed. Boston: Pearson, 2013.
