# Grupo 03 — Estudo da Arquitetura MISD

## 1. Members

Integrantes da equipe:

- Tiago Henrique Souza Lima
- Emanuel Borges Vale
- Elaine Cardoso de Souza Barros
- Augusto Spolavori Siqueira
- Joao Guilherme Alves de Souza Oliveira
- João Victor Ferreira de Lima Moura

## 2. Selected Architecture

**Arquitetura selecionada:** MISD (Multiple Instruction, Single Data).

**Plataforma de hardware do projeto:** ESP32.

**Linguagem de programação utilizada no processamento:** Python.

O grupo selecionou o modelo MISD, pertencente à taxonomia de Flynn, como objeto de estudo para compreender a relação entre fluxos de instruções e fluxos de dados nas arquiteturas de computadores.

A proposta prática consiste no desenvolvimento de um protótipo acadêmico de monitoramento de temperatura. Um sensor realizará a medição, o ESP32 será responsável pela coleta e transmissão dos dados por Wi-Fi, e um computador executará um programa em Python para processar as informações recebidas.

O mesmo valor de temperatura será utilizado em diferentes análises, como verificação de limites, comparação com registros anteriores e identificação de possíveis anomalias.

É importante distinguir o conceito arquitetural MISD da implementação proposta: executar diferentes análises sobre o mesmo valor em um programa convencional não comprova, por si só, uma arquitetura MISD física. O protótipo servirá como demonstração conceitual da reutilização de um mesmo dado em diferentes processos de análise, permitindo estudar os princípios relacionados à classificação de Flynn.

## 3. Initial Overview

O MISD é uma das quatro categorias tradicionalmente associadas à taxonomia de Flynn, proposta para classificar organizações de computadores considerando os fluxos de instruções e de dados.

A sigla MISD significa *Multiple Instruction, Single Data*, traduzida como Múltiplas Instruções, Um Único Dado. Em termos conceituais, esse modelo representa uma organização na qual múltiplos fluxos de instruções operam sobre um único fluxo de dados.

O estudo dessa classificação permite compreender diferentes formas de organizar o processamento computacional e compará-las com outros modelos, como SISD (*Single Instruction, Single Data*), SIMD (*Single Instruction, Multiple Data*) e MIMD (*Multiple Instruction, Multiple Data*).

No projeto do Grupo 03, o objeto de estudo será relacionado a um protótipo de monitoramento de temperatura. A temperatura coletada será encaminhada a um computador, onde o programa em Python utilizará o valor recebido para realizar análises com finalidades distintas.

As análises planejadas são:

- **Verificação de limite:** determinar se a temperatura ultrapassou um valor previamente estabelecido.
- **Comparação histórica:** comparar a medição atual com temperaturas registradas anteriormente.
- **Detecção de possíveis anomalias:** identificar variações que atendam aos critérios definidos pelo grupo.

O objetivo é relacionar o funcionamento do protótipo aos conceitos de organização e processamento de dados, reconhecendo as diferenças entre uma demonstração em software e uma implementação física de arquitetura paralela.

## 4. Historical Context

### Período e ano

A classificação associada à taxonomia de Flynn foi desenvolvida no contexto das pesquisas sobre organização de computadores e processamento paralelo. Um dos trabalhos acadêmicos fundamentais de Michael J. Flynn foi publicado em setembro de 1972, com o título *Some Computer Organizations and Their Effectiveness*.

O artigo contribuiu para a análise e classificação de diferentes organizações computacionais de acordo com seus fluxos de instruções e dados.

### Fabricante ou organização

A taxonomia de Flynn está associada ao pesquisador Michael J. Flynn e à pesquisa acadêmica em arquitetura de computadores. Portanto, o MISD não é um processador ou produto comercial fabricado por uma empresa específica.

Para o desenvolvimento do protótipo, o grupo utilizará a plataforma ESP32, desenvolvida pela Espressif Systems, uma empresa especializada em soluções de conectividade e microcontroladores.

### Contexto de surgimento

O desenvolvimento de computadores com diferentes formas de organização do processamento tornou necessário estabelecer critérios para comparar arquiteturas e compreender suas características.

Nesse contexto, a classificação de Flynn passou a ser utilizada para organizar os modelos de computadores de acordo com a quantidade de fluxos de instruções e de dados envolvidos no processamento.

Essa classificação contribuiu para o estudo de sistemas sequenciais, processamento de dados em paralelo e sistemas com múltiplos fluxos de instruções.

O MISD representa uma das categorias dessa classificação. Embora seja importante para o estudo teórico das arquiteturas, sua implementação prática é menos comum do que a de outras categorias, e sistemas reais nem sempre se enquadram perfeitamente em apenas um modelo.

### Importância histórica

A importância histórica da taxonomia de Flynn está na criação de um modelo conceitual que facilita o estudo das organizações computacionais.

Sua classificação permite comparar diferentes estratégias de processamento e compreender como a organização dos fluxos de instruções e dados influencia o funcionamento dos sistemas.

O estudo do MISD também permite discutir as dificuldades de relacionar uma classificação teórica a sistemas reais, especialmente quando se consideram o paralelismo, a distribuição de tarefas e o compartilhamento de informações.

Para o Grupo 03, esse contexto histórico fundamenta o estudo conceitual que será relacionado ao protótipo de monitoramento de temperatura.

## 5. Technical Characteristics

### 5.1. Arquitetura e ISA

O MISD é uma categoria da taxonomia de Flynn, e não uma arquitetura de conjunto de instruções específica. Por isso, não possui uma ISA própria.

A ISA (*Instruction Set Architecture*) define o conjunto de instruções e os aspectos visíveis ao software de um processador específico.

No protótipo, a plataforma escolhida é o ESP32. Para a família ESP32 original, o processador utiliza a arquitetura de instruções Xtensa LX6 de 32 bits. Entretanto, as características exatas dependem do modelo de chip ou módulo utilizado.

O ESP32 é a plataforma de hardware do projeto; ele não deve ser classificado automaticamente como uma implementação física MISD apenas por participar de um sistema que utiliza o mesmo dado em diferentes análises.

### 5.2. Tamanho de palavra

O tamanho de palavra corresponde à quantidade de bits utilizada como referência em determinadas operações do processador.

No ESP32 original, os núcleos Xtensa LX6 são de 32 bits. Essa característica influencia o processamento de valores inteiros, operações aritméticas e outros aspectos da execução de instruções.

A representação numérica da temperatura no programa Python, por sua vez, depende dos tipos de dados utilizados pelo software e não deve ser confundida diretamente com o tamanho de palavra do microcontrolador.

### 5.3. Processador

O ESP32 original utiliza um microprocessador Xtensa LX6 de 32 bits, disponível em configurações de um ou dois núcleos, dependendo da variante.

O processador executa as instruções responsáveis pelo funcionamento do microcontrolador, incluindo tarefas de controle, leitura de periféricos e comunicação.

No projeto, o ESP32 será utilizado para receber os dados do sensor e transmiti-los ao computador por Wi-Fi.

O processamento das análises de temperatura será realizado no computador por meio de um programa desenvolvido em Python.

### 5.4. Memória

A memória é responsável por armazenar instruções, variáveis e dados necessários à execução dos programas.

No ESP32 original, os recursos de memória incluem ROM e SRAM internas. A quantidade de memória disponível e a presença de memória flash integrada ou externa dependem da variante ou do módulo utilizado.

No protótipo, a memória do ESP32 será utilizada durante a execução do firmware e a manipulação das leituras do sensor. No computador, o programa Python utilizará os recursos de memória do sistema operacional para receber e processar as temperaturas.

O histórico das medições poderá ser mantido em um arquivo ou outro mecanismo de armazenamento simples, cuja definição ocorrerá durante o desenvolvimento.

### 5.5. Entrada e saída

O ESP32 possui interfaces de entrada e saída que permitem a comunicação com sensores, dispositivos externos e outros sistemas.

Entre os recursos disponíveis, conforme o modelo, estão:

- GPIO, para entrada e saída digital;
- I²C e SPI, para comunicação com periféricos;
- UART, para comunicação serial;
- ADC, para conversão de sinais analógicos;
- interfaces de conectividade sem fio, incluindo Wi-Fi.

No protótipo, o sensor de temperatura fornecerá as medições ao ESP32. O microcontrolador enviará os valores por Wi-Fi para o computador, que receberá as informações no programa Python.

A interface elétrica e o protocolo necessários para conectar o sensor dependerão do componente efetivamente escolhido pelo grupo.

### 5.6. Comunicação e conectividade

A conectividade é uma característica relevante da plataforma ESP32. A versão original oferece comunicação Wi-Fi na faixa de 2,4 GHz e Bluetooth, conforme as especificações do fabricante.

O Wi-Fi será utilizado como meio de comunicação entre o ESP32 e o computador. O protocolo de aplicação responsável pelo envio dos dados será definido durante a implementação.

O sistema deverá garantir que os valores transmitidos sejam recebidos e interpretados corretamente pelo programa de processamento.

### 5.7. Software e processamento dos dados

O projeto utilizará um firmware no ESP32 para realizar as funções relacionadas à leitura do sensor e à transmissão das informações.

No computador, Python será utilizado para receber a temperatura e executar as análises previstas.

O programa deverá utilizar a mesma medição como entrada para três finalidades principais: verificar limites, consultar o histórico e identificar possíveis anomalias.

Essas operações serão implementadas conforme os critérios estabelecidos pelo grupo. Inicialmente, o protótipo terá finalidade acadêmica e não será destinado ao controle de equipamentos industriais.

### 5.8. Características relevantes para o projeto

As principais características técnicas relacionadas à proposta são:

- processamento embarcado no ESP32;
- leitura de dados provenientes de um sensor;
- comunicação entre dispositivos por Wi-Fi;
- processamento de dados no computador;
- utilização de uma mesma medição em diferentes análises;
- possibilidade de armazenamento de registros históricos;
- separação entre aquisição, transmissão, processamento e apresentação dos resultados.

A quantidade exata de memória, o número de núcleos e as interfaces disponíveis deverão ser confirmados a partir do modelo de ESP32 efetivamente utilizado.

## 6. Relevance to Computer Architecture

O estudo do MISD é importante para a Arquitetura de Computadores porque permite compreender como os sistemas podem ser classificados de acordo com a organização de seus fluxos de instruções e dados.

A taxonomia de Flynn oferece uma base para comparar diferentes modelos de processamento e estudar a relação entre hardware, execução de instruções e tratamento das informações.

O MISD também é relevante por permitir discutir as particularidades e limitações de uma categoria menos comum em implementações convencionais, além de incentivar uma análise crítica sobre a diferença entre conceitos arquiteturais e programas que apenas reproduzem parte de sua ideia.

No protótipo do Grupo 03, o estudo será relacionado ao percurso de uma informação desde sua aquisição até a apresentação dos resultados. O sensor realizará a medição, o ESP32 transmitirá o dado e o computador executará diferentes análises sobre a temperatura recebida.

Essa aplicação permitirá estudar conceitos complementares, como processamento, memória, comunicação entre dispositivos, interfaces de entrada e saída e organização do software.

É importante ressaltar que a implementação proposta não pretende demonstrar que o ESP32 possui uma arquitetura MISD nativa. O objetivo é utilizar o projeto como instrumento didático para compreender a classificação de Flynn, explorar o uso de um mesmo dado em diferentes operações e discutir o que seria necessário para caracterizar uma implementação arquitetural MISD propriamente dita.

Dessa forma, o trabalho conecta a pesquisa histórica e teórica à construção de um protótipo funcional, contribuindo para a compreensão dos princípios fundamentais da Arquitetura de Computadores.

## Referências

As fontes utilizadas na pesquisa estão organizadas no arquivo [`references.md`](references.md), que reúne referências acadêmicas e documentação técnica oficial.

