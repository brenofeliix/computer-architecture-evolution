# Grupo 03

# Protótipo de Monitoramento de Temperatura baseado no conceito MISD

## Integrantes

* Tiago Henrique Souza Lima
* Emanuel Borges Vale
* Elaine Cardoso de Souza Barros
* Augusto Spolavori Siqueira
* Joao Guilherme Alves de Souza Oliveira
* João Victor Ferreira de Lima Moura

---

# 1. Introdução

Este projeto tem como objetivo desenvolver um **protótipo acadêmico de monitoramento de temperatura baseado no conceito MISD (Multiple Instruction, Single Data)**.

A proposta consiste em utilizar um **sensor de temperatura conectado a um ESP32** para realizar a coleta de informações. Após a leitura, o ESP32 será responsável por transmitir o valor da temperatura por meio de uma conexão **Wi-Fi** para um computador.

No computador, um programa desenvolvido em **Python** receberá os dados enviados pelo ESP32. A partir desse valor, o programa realizará diferentes processos de análise.

O mesmo dado de temperatura poderá ser utilizado para:

* verificar se a temperatura ultrapassou um determinado limite;
* comparar a temperatura atual com temperaturas registradas anteriormente;
* identificar possíveis comportamentos considerados anormais.

Dessa maneira, o projeto procura demonstrar de forma prática a ideia central do modelo MISD: **um mesmo dado de entrada sendo submetido a diferentes instruções ou processos de análise**.

O projeto possui finalidade acadêmica e será desenvolvido de forma gradual. Portanto, não tem como objetivo criar inicialmente um sistema industrial completo, mas sim construir um protótipo funcional que permita compreender a relação entre **coleta de dados, comunicação, processamento e análise**.

---

# 2. Contextualização

O monitoramento de temperatura é uma aplicação encontrada em diferentes áreas da tecnologia. A temperatura pode ser utilizada para acompanhar equipamentos, ambientes ou processos e identificar alterações que possam exigir alguma ação.

Em um sistema de monitoramento tradicional, um sensor realiza uma leitura e essa informação é encaminhada para um sistema responsável pelo processamento.

Neste projeto, além de realizar a coleta e o processamento da temperatura, será demonstrado como **um único dado pode ser utilizado em diferentes análises**.

Por exemplo, supondo que o sensor registre uma temperatura de **38 °C**, o programa em Python poderá utilizar esse mesmo valor para executar diferentes operações:

* verificar se 38 °C está acima do limite definido;
* comparar 38 °C com medições anteriores;
* verificar se o valor apresenta uma alteração significativa em relação ao histórico.

As análises utilizam a mesma informação de entrada, mas possuem objetivos diferentes.

Essa característica será utilizada para relacionar o funcionamento do protótipo ao conceito de **MISD (Multiple Instruction, Single Data)**.

---

# 3. Problema

O problema abordado pelo projeto consiste em desenvolver uma maneira simples de **coletar uma informação de temperatura, transmiti-la para um computador e utilizar esse mesmo dado em diferentes processos de análise**.

Além da coleta da temperatura, é necessário considerar todo o caminho percorrido pela informação:

1. o sensor precisa realizar a medição;
2. o ESP32 precisa receber essa informação;
3. o dado precisa ser transmitido ao computador;
4. o programa precisa receber e interpretar a informação;
5. o mesmo dado precisa ser utilizado pelas diferentes análises;
6. os resultados precisam ser armazenados e apresentados;
7. o funcionamento de todas essas etapas precisa ser testado.

O projeto procura organizar essas etapas em um único protótipo, permitindo estudar tanto o funcionamento do sistema quanto a aplicação do conceito MISD.

---

# 4. Objetivos

## 4.1 Objetivo geral

Desenvolver um protótipo acadêmico de monitoramento de temperatura capaz de coletar dados por meio de um sensor conectado a um ESP32, transmitir essas informações por Wi-Fi para um computador e utilizar o mesmo dado de entrada em diferentes processos de análise desenvolvidos em Python, demonstrando o conceito MISD.

## 4.2 Objetivos específicos

* Definir o problema e os objetivos do sistema de monitoramento.
* Identificar quais dados serão necessários para o funcionamento do protótipo.
* Conectar um sensor de temperatura ao ESP32.
* Realizar a leitura da temperatura por meio do ESP32.
* Estabelecer a comunicação entre o ESP32 e o computador utilizando Wi-Fi.
* Desenvolver um programa em Python para receber os dados.
* Utilizar o mesmo dado de temperatura em diferentes processos de análise.
* Verificar se a temperatura ultrapassou um limite estabelecido.
* Comparar a temperatura atual com informações anteriores.
* Identificar possíveis anomalias nos dados.
* Armazenar as informações necessárias para consultas futuras.
* Apresentar os resultados de maneira simples e compreensível.
* Realizar testes para verificar o funcionamento do protótipo.

---

# 5. Conceito MISD

MISD é a sigla para **Multiple Instruction, Single Data**, que pode ser traduzida como **Múltiplas Instruções, Um Único Dado**.

A ideia principal desse modelo é que um mesmo fluxo de dados possa ser utilizado por diferentes instruções ou processos.

No projeto, o conceito será demonstrado utilizando a temperatura coletada pelo sensor.

Depois que o computador receber uma determinada temperatura, o programa em Python poderá utilizar esse mesmo valor em diferentes análises.

Por exemplo:

**Temperatura recebida: 38 °C**

Essa informação poderá ser utilizada simultaneamente ou em sequência para:

* verificar o limite de temperatura;
* comparar com valores anteriores;
* analisar possíveis alterações ou anomalias.

Portanto, embora exista apenas **um dado de entrada**, existem diferentes operações realizadas sobre ele.

É essa relação entre **um dado e diferentes processos de análise** que será utilizada para demonstrar o conceito MISD no projeto.

---

# 6. Arquitetura do protótipo

A arquitetura do protótipo será composta por diferentes partes, cada uma com uma função específica.

## 6.1 Sensor de temperatura

O sensor será responsável por realizar a medição da temperatura.

Ele representa a etapa de entrada do sistema, pois é a partir dele que a informação utilizada pelo restante do projeto será obtida.

A cada leitura, o sensor produzirá um valor correspondente à temperatura identificada.

---

## 6.2 ESP32

O ESP32 será utilizado como dispositivo responsável por receber os dados do sensor e realizar a comunicação com o computador.

Além de trabalhar com a leitura do sensor, o ESP32 terá a função de enviar os dados utilizando uma conexão Wi-Fi.

Dessa maneira, ele funcionará como uma ponte entre a etapa de coleta e a etapa de processamento.

---

## 6.3 Comunicação Wi-Fi

A comunicação Wi-Fi será utilizada para transportar os dados coletados pelo ESP32 até o computador.

O objetivo dessa etapa é permitir que a temperatura medida pelo sensor deixe o ESP32 e chegue ao programa responsável pelo processamento.

A comunicação será uma parte importante dos testes, pois será necessário verificar se o computador está recebendo corretamente os valores enviados pelo ESP32.

---

## 6.4 Computador

O computador será responsável por executar o programa desenvolvido em Python.

Depois que o ESP32 enviar a temperatura, o computador receberá o valor e disponibilizará essa informação para os diferentes processos de análise.

---

## 6.5 Python

Python será a principal linguagem utilizada na etapa de processamento.

O programa deverá receber a temperatura enviada pelo ESP32 e utilizar esse mesmo valor nas diferentes análises propostas pelo projeto.

Entre as principais funções do programa estarão:

* receber os dados;
* interpretar o valor da temperatura;
* verificar limites;
* comparar dados históricos;
* identificar possíveis anomalias;
* gerar resultados;
* contribuir para o armazenamento das informações.

---

# 7. Funcionamento do projeto

O funcionamento do sistema será dividido em etapas para facilitar seu desenvolvimento e entendimento.

Primeiramente, o sensor realizará uma medição de temperatura.

O valor obtido será enviado para o ESP32, que ficará responsável por receber essa informação.
# Grupo 03 — Estudo da Arquitetura MISD

## 1. Members

**Integrantes da equipe:**

- Tiago Henrique Souza Lima
- Emanuel Borges Vale
- Elaine Cardoso de Souza Barros
- Augusto Spolavori Siqueira
- Joao Guilherme Alves de Souza Oliveira
- João Victor Ferreira de Lima Moura

Depois disso, o ESP32 utilizará a conexão Wi-Fi para transmitir o valor ao computador.

No computador, o programa em Python receberá a informação.

A partir desse momento, o mesmo valor será utilizado em diferentes processos.

Uma das análises será responsável por verificar se a temperatura ultrapassou um limite previamente definido.

Outra análise utilizará dados anteriores para realizar uma comparação com a temperatura atual.

Uma terceira análise poderá verificar se a temperatura apresenta um comportamento diferente do esperado, indicando uma possível anomalia.

Os resultados dessas análises serão então apresentados ao usuário e as informações necessárias poderão ser armazenadas para utilização posterior.

---

# 8. Análises realizadas pelo sistema

## 8.1 Verificação de limite

A primeira análise terá como objetivo verificar se a temperatura atual ultrapassou um determinado limite.

Por exemplo, se o limite estabelecido for 40 °C e o sensor registrar 42 °C, o programa deverá identificar que o valor ultrapassou o limite.

Essa análise permitirá demonstrar uma decisão simples baseada no valor recebido.

---

## 8.2 Comparação com o histórico

A segunda análise utilizará informações de temperaturas registradas anteriormente.

O objetivo será comparar a temperatura atual com os valores anteriores para verificar como ela está se comportando ao longo do tempo.

Por exemplo, se as últimas medições estiverem próximas de 25 °C e uma nova leitura apresentar 35 °C, o programa poderá identificar uma alteração em relação ao histórico.

Essa etapa também será importante para demonstrar a necessidade de armazenamento dos dados.

---

## 8.3 Identificação de possíveis anomalias

A terceira análise terá como objetivo identificar possíveis alterações fora do comportamento esperado.

A identificação de uma anomalia será baseada nos critérios definidos durante o desenvolvimento do projeto.

Por exemplo, uma temperatura muito diferente dos valores registrados anteriormente poderá ser considerada uma possível anomalia.

Essa análise não terá como objetivo realizar um diagnóstico complexo, mas demonstrar como o mesmo dado pode ser utilizado para uma finalidade diferente das outras análises.

---

# 9. Dados de entrada

O principal dado de entrada do projeto será a **temperatura medida pelo sensor**.

Além do valor da temperatura, poderão ser registradas informações complementares necessárias para organizar o histórico, como o momento em que a medição foi realizada.

Essas informações permitirão relacionar uma temperatura específica ao momento em que ela foi coletada.

O projeto deverá manter o foco nos dados realmente necessários para o funcionamento do protótipo, evitando a inclusão de sensores ou informações que não sejam necessárias para demonstrar o conceito proposto.

---

# 10. Armazenamento dos dados

O armazenamento será utilizado principalmente para manter as informações necessárias para a comparação entre a temperatura atual e as temperaturas anteriores.

A partir dos dados armazenados, o programa poderá consultar valores anteriores e utilizá-los nas análises.

O armazenamento também permitirá manter um histórico das medições realizadas durante os testes.

A forma definitiva de armazenamento será definida durante o desenvolvimento do projeto, de acordo com a necessidade e a complexidade do protótipo.

O objetivo é utilizar uma solução simples e adequada ao caráter acadêmico do trabalho.

---

# 11. Apresentação dos resultados

Depois que os dados forem processados, os resultados deverão ser apresentados de maneira simples e compreensível.

O usuário deverá conseguir identificar informações como:

* temperatura recebida;
* resultado da verificação de limite;
* comparação com informações anteriores;
* indicação de possível anomalia.
## 2. Selected Architecture

**Arquitetura selecionada:** MISD (Multiple Instruction, Single Data).

**Plataforma de hardware do projeto:** ESP32.

**Linguagem de programação utilizada no processamento:** Python.

O grupo selecionou o modelo MISD, pertencente à taxonomia de Flynn, como objeto de estudo para compreender a relação entre fluxos de instruções e fluxos de dados nas arquiteturas de computadores.

A proposta prática consiste no desenvolvimento de um protótipo acadêmico de monitoramento de temperatura. Um sensor realizará a medição, o ESP32 será responsável pela coleta e transmissão dos dados por Wi-Fi, e um computador executará um programa em Python para processar as informações recebidas.

O mesmo valor de temperatura será utilizado em diferentes análises, como verificação de limites, comparação com registros anteriores e identificação de possíveis anomalias.

**Observação:** é importante distinguir o conceito arquitetural MISD da implementação proposta. Executar diferentes análises sobre o mesmo valor em um programa convencional não comprova, por si só, uma arquitetura MISD física. O protótipo servirá como demonstração conceitual da reutilização de um mesmo dado em diferentes processos de análise, permitindo estudar os princípios relacionados à classificação de Flynn.

---

## 3. Initial Overview

O MISD é uma das quatro categorias tradicionalmente associadas à taxonomia de Flynn, proposta para classificar organizações de computadores considerando os fluxos de instruções e de dados.

A sigla MISD significa *Multiple Instruction, Single Data*, traduzida como **Múltiplas Instruções, Um Único Dado**. Em termos conceituais, esse modelo representa uma organização na qual múltiplos fluxos de instruções operam sobre um único fluxo de dados.

O estudo dessa classificação permite compreender diferentes formas de organizar o processamento computacional e compará-las com outros modelos, como:

- **SISD:** Single Instruction, Single Data;
- **SIMD:** Single Instruction, Multiple Data;
- **MISD:** Multiple Instruction, Single Data;
- **MIMD:** Multiple Instruction, Multiple Data.

No projeto do Grupo 03, o objeto de estudo será relacionado a um protótipo de monitoramento de temperatura. A temperatura coletada será encaminhada a um computador, onde o programa em Python utilizará o valor recebido para realizar análises com finalidades distintas.

As análises planejadas são:

- **Verificação de limite:** determinar se a temperatura ultrapassou um valor previamente estabelecido.
- **Comparação histórica:** comparar a medição atual com temperaturas registradas anteriormente.
- **Detecção de possíveis anomalias:** identificar variações que atendam aos critérios definidos pelo grupo.

O objetivo é relacionar o funcionamento do protótipo aos conceitos de organização e processamento de dados, reconhecendo as diferenças entre uma demonstração em software e uma implementação física de arquitetura paralela.

---

## 4. Historical Context

### 4.1. Período e ano

A classificação associada à taxonomia de Flynn foi desenvolvida no contexto das pesquisas sobre organização de computadores e processamento paralelo.

Um dos trabalhos acadêmicos fundamentais de Michael J. Flynn foi publicado em setembro de 1972, com o título *Some Computer Organizations and Their Effectiveness*.

O artigo contribuiu para a análise e classificação de diferentes organizações computacionais de acordo com seus fluxos de instruções e dados.

### 4.2. Fabricante ou organização

A taxonomia de Flynn está associada ao pesquisador Michael J. Flynn e à pesquisa acadêmica em arquitetura de computadores. Portanto, o MISD não é um processador ou produto comercial fabricado por uma empresa específica.

Para o desenvolvimento do protótipo, o grupo utilizará a plataforma ESP32, desenvolvida pela Espressif Systems, uma empresa especializada em soluções de conectividade e microcontroladores.

### 4.3. Contexto de surgimento

O desenvolvimento de computadores com diferentes formas de organização do processamento tornou necessário estabelecer critérios para comparar arquiteturas e compreender suas características.

Nesse contexto, a classificação de Flynn passou a ser utilizada para organizar os modelos de computadores de acordo com a quantidade de fluxos de instruções e de dados envolvidos no processamento.

Essa classificação contribuiu para o estudo de sistemas sequenciais, processamento de dados em paralelo e sistemas com múltiplos fluxos de instruções.

O MISD representa uma das categorias dessa classificação. Embora seja importante para o estudo teórico das arquiteturas, sua implementação prática é menos comum do que a de outras categorias, e sistemas reais nem sempre se enquadram perfeitamente em apenas um modelo.

### 4.4. Importância histórica

A importância histórica da taxonomia de Flynn está na criação de um modelo conceitual que facilita o estudo das organizações computacionais.

Sua classificação permite comparar diferentes estratégias de processamento e compreender como a organização dos fluxos de instruções e dados influencia o funcionamento dos sistemas.

O estudo do MISD também permite discutir as dificuldades de relacionar uma classificação teórica a sistemas reais, especialmente quando se consideram o paralelismo, a distribuição de tarefas e o compartilhamento de informações.

Para o Grupo 03, esse contexto histórico fundamenta o estudo conceitual que será relacionado ao protótipo de monitoramento de temperatura.

Inicialmente, a apresentação poderá ser realizada diretamente pelo programa em Python, utilizando informações exibidas na tela.

Caso seja necessário durante o desenvolvimento, outras formas simples de apresentação poderão ser utilizadas.

O objetivo principal não é desenvolver uma interface complexa, mas garantir que os resultados das análises possam ser compreendidos pelo usuário.
## 5. Technical Characteristics

### 5.1. Arquitetura e ISA

O MISD é uma categoria da taxonomia de Flynn, e não uma arquitetura de conjunto de instruções específica. Por isso, não possui uma ISA própria.

A ISA (*Instruction Set Architecture*) define o conjunto de instruções e os aspectos visíveis ao software de um processador específico.

No protótipo, a plataforma escolhida é o ESP32. Para a família ESP32 original, o processador utiliza a arquitetura de instruções Xtensa LX6 de 32 bits. Entretanto, as características exatas dependem do modelo de chip ou módulo utilizado.

O ESP32 é a plataforma de hardware do projeto; ele não deve ser classificado automaticamente como uma implementação física MISD apenas por participar de um sistema que utiliza o mesmo dado em diferentes análises.

# 12. Testes e validação

Os testes serão realizados para verificar se todas as principais partes do protótipo estão funcionando corretamente.

A primeira etapa será verificar se o sensor consegue realizar as leituras de temperatura corretamente.

Depois, será necessário verificar se o ESP32 consegue receber os valores e se comunicar com o computador por Wi-Fi.

Também será necessário verificar se o programa em Python consegue receber corretamente os dados enviados.

Após a comunicação ser validada, serão realizados testes com as diferentes análises.

Serão utilizados valores diferentes de temperatura para verificar situações como:

* temperatura abaixo do limite;
* temperatura próxima do limite;
* temperatura acima do limite;
* temperatura semelhante às medições anteriores;
* temperatura diferente do histórico;
* possíveis situações de anomalia.

Por fim, será verificado se os resultados são armazenados e apresentados corretamente.

Os testes terão como objetivo identificar erros e confirmar se o protótipo atende ao funcionamento definido nas etapas anteriores.

---

# 13. Tecnologias e componentes

| Componente/Tecnologia   | Função no projeto                       |
| ----------------------- | --------------------------------------- |
| Sensor de temperatura   | Realizar a medição                      |
| ESP32                   | Receber a leitura e transmitir os dados |
| Wi-Fi                   | Realizar a comunicação com o computador |
| Computador              | Executar o sistema de processamento     |
| Python                  | Receber e analisar os dados             |
| Armazenamento           | Guardar dados e histórico               |
| Sistema de apresentação | Exibir os resultados das análises       |

A escolha dessas tecnologias busca manter o projeto simples, permitindo que o grupo consiga compreender e implementar cada etapa.

---

# 14. Desenvolvimento em 9 semanas

O projeto será desenvolvido progressivamente durante nove semanas.

## Semana 1 — Problema e objetivo

Nesta etapa será definido o problema que o sistema pretende resolver e será estudada a relação entre o problema de monitoramento e o conceito MISD.

Também serão definidos os objetivos gerais e específicos do protótipo.

## Semana 2 — Dados de entrada

Será definido quais dados serão necessários para o funcionamento do sistema.

O principal dado será a temperatura coletada pelo sensor.

Também será analisada a forma como essas informações serão utilizadas nas etapas seguintes.

## Semana 3 — Coleta dos dados

Será realizada a integração entre o sensor de temperatura e o ESP32.

O objetivo será conseguir realizar a leitura da temperatura e verificar se os valores estão sendo recebidos corretamente.

## Semana 4 — Comunicação

Será desenvolvida a comunicação entre o ESP32 e o computador utilizando Wi-Fi.

Nessa etapa será verificado se o valor coletado pelo sensor consegue chegar corretamente ao computador.

## Semana 5 — Aplicação do MISD

Será estudado e implementado o conceito de utilizar o mesmo dado em diferentes processos.

O valor recebido será utilizado como entrada para as diferentes análises planejadas.

## Semana 6 — Processamento

Serão implementadas em Python as análises relacionadas ao limite, histórico e identificação de possíveis anomalias.

Essa etapa representa a principal aplicação prática do conceito MISD no protótipo.

## Semana 7 — Armazenamento

Será implementada a forma de armazenamento das temperaturas e dos resultados necessários para manter o histórico.
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

- **GPIO:** entrada e saída digital;
- **I²C e SPI:** comunicação com periféricos;
- **UART:** comunicação serial;
- **ADC:** conversão de sinais analógicos;
- **Wi-Fi:** conectividade sem fio.

No protótipo, o sensor de temperatura fornecerá as medições ao ESP32. O microcontrolador enviará os valores por Wi-Fi para o computador, que receberá as informações no programa Python.

A interface elétrica e o protocolo necessários para conectar o sensor dependerão do componente efetivamente escolhido pelo grupo.

### 5.6. Comunicação e conectividade

A conectividade é uma característica relevante da plataforma ESP32. A versão original oferece comunicação Wi-Fi na faixa de 2,4 GHz e Bluetooth, conforme as especificações do fabricante.

O Wi-Fi será utilizado como meio de comunicação entre o ESP32 e o computador. O protocolo de aplicação responsável pelo envio dos dados será definido durante a implementação.

O sistema deverá garantir que os valores transmitidos sejam recebidos e interpretados corretamente pelo programa de processamento.

### 5.7. Software e processamento dos dados

O projeto utilizará um firmware no ESP32 para realizar as funções relacionadas à leitura do sensor e à transmissão das informações.

No computador, Python será utilizado para receber a temperatura e executar as análises previstas.

O programa deverá utilizar a mesma medição como entrada para três finalidades principais:

1. Verificar se a temperatura ultrapassou os limites estabelecidos.
2. Consultar o histórico e comparar a medição atual com registros anteriores.
3. Identificar possíveis anomalias com base nos critérios definidos pelo grupo.

Essas operações serão implementadas conforme os critérios estabelecidos pelo grupo. Inicialmente, o protótipo terá finalidade acadêmica e não será destinado ao controle de equipamentos industriais.

### 5.8. Características relevantes para o projeto

As principais características técnicas relacionadas à proposta são:

- Processamento embarcado no ESP32.
- Leitura de dados provenientes de um sensor.
- Comunicação entre dispositivos por Wi-Fi.
- Processamento de dados no computador.
- Utilização de uma mesma medição em diferentes análises.
- Possibilidade de armazenamento de registros históricos.
- Separação entre aquisição, transmissão, processamento e apresentação dos resultados.

A quantidade exata de memória, o número de núcleos e as interfaces disponíveis deverão ser confirmados a partir do modelo de ESP32 efetivamente utilizado.

O objetivo será permitir que o sistema utilize informações anteriores durante as análises.

## Semana 8 — Apresentação

Será definida a forma de apresentação dos resultados ao usuário.

O sistema deverá apresentar as informações de maneira simples, permitindo compreender o resultado das análises.

## Semana 9 — Testes e validação

Será realizado um conjunto de testes para verificar todas as etapas do protótipo.

Serão avaliadas a coleta, transmissão, processamento, armazenamento e apresentação dos dados.

Após os testes, serão identificados possíveis problemas e realizadas as correções necessárias.

---

# 15. Escopo do projeto
## 6. Relevance to Computer Architecture

O estudo do MISD é importante para a Arquitetura de Computadores porque permite compreender como os sistemas podem ser classificados de acordo com a organização de seus fluxos de instruções e dados.

A taxonomia de Flynn oferece uma base para comparar diferentes modelos de processamento e estudar a relação entre hardware, execução de instruções e tratamento das informações.

O MISD também é relevante por permitir discutir as particularidades e limitações de uma categoria menos comum em implementações convencionais, além de incentivar uma análise crítica sobre a diferença entre conceitos arquiteturais e programas que apenas reproduzem parte de sua ideia.

No protótipo do Grupo 03, o estudo será relacionado ao percurso de uma informação desde sua aquisição até a apresentação dos resultados. O sensor realizará a medição, o ESP32 transmitirá o dado e o computador executará diferentes análises sobre a temperatura recebida.

Essa aplicação permitirá estudar conceitos complementares, como processamento, memória, comunicação entre dispositivos, interfaces de entrada e saída e organização do software.

É importante ressaltar que a implementação proposta não pretende demonstrar que o ESP32 possui uma arquitetura MISD nativa. O objetivo é utilizar o projeto como instrumento didático para compreender a classificação de Flynn, explorar o uso de um mesmo dado em diferentes operações e discutir o que seria necessário para caracterizar uma implementação arquitetural MISD propriamente dita.

Dessa forma, o trabalho conecta a pesquisa histórica e teórica à construção de um protótipo funcional, contribuindo para a compreensão dos princípios fundamentais da Arquitetura de Computadores.

O projeto possui um escopo acadêmico e simplificado.

Não será desenvolvido inicialmente um sistema industrial completo de monitoramento.

O foco será demonstrar o funcionamento de um protótipo capaz de:

* medir temperatura;
* receber a medição no ESP32;
* transmitir a informação por Wi-Fi;
* receber o dado no computador;
* processar a informação utilizando Python;
* utilizar o mesmo dado em diferentes análises;
* armazenar informações necessárias;
* apresentar os resultados;
* realizar testes de funcionamento.

A simplificação do projeto permite que o grupo concentre seus esforços na compreensão do conceito MISD e na construção gradual do protótipo.

---

# 16. Resultado esperado

Ao final do desenvolvimento, espera-se que o protótipo consiga realizar o processo completo de monitoramento da temperatura.

O sensor deverá realizar a medição, o ESP32 deverá receber e transmitir a informação e o computador deverá receber o valor por meio da comunicação Wi-Fi.

O programa em Python deverá então utilizar o mesmo valor de temperatura para realizar diferentes análises.

Espera-se que seja possível verificar o limite da temperatura, comparar a medição atual com informações anteriores e identificar possíveis anomalias de acordo com os critérios definidos pelo grupo.

Também será esperado que os resultados possam ser armazenados e apresentados ao usuário de maneira simples.

O principal resultado acadêmico será demonstrar, por meio de uma aplicação prática, como **um único dado de entrada pode ser utilizado por diferentes processos**, relacionando o funcionamento do protótipo ao conceito de **MISD (Multiple Instruction, Single Data)**.

---

# 18. Considerações finais

O projeto propõe a construção de um protótipo simples de monitoramento de temperatura para demonstrar, de maneira prática, a aplicação do conceito MISD.

A utilização de um sensor, ESP32, comunicação Wi-Fi e Python permitirá ao grupo acompanhar todo o caminho percorrido por um dado, desde sua coleta até sua análise.

O ponto principal do projeto será utilizar **a mesma temperatura recebida como entrada para diferentes processos**, permitindo verificar o limite, comparar com o histórico e identificar possíveis anomalias.

O desenvolvimento em etapas também permitirá que cada parte do sistema seja construída e testada separadamente antes da integração final.

Dessa forma, o projeto busca unir os conhecimentos de **arquitetura de computadores, programação, comunicação de dados e processamento de informações** em um único protótipo acadêmico.

O resultado final deverá demonstrar de forma clara e prática a relação entre o monitoramento de temperatura e o conceito **Multiple Instruction, Single Data (MISD)**.
## Referências

As fontes utilizadas na pesquisa estão organizadas no arquivo [references.md](references.md), que reúne referências acadêmicas e documentação técnica oficial fornecida.
