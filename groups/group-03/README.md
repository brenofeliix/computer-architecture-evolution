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

Inicialmente, a apresentação poderá ser realizada diretamente pelo programa em Python, utilizando informações exibidas na tela.

Caso seja necessário durante o desenvolvimento, outras formas simples de apresentação poderão ser utilizadas.

O objetivo principal não é desenvolver uma interface complexa, mas garantir que os resultados das análises possam ser compreendidos pelo usuário.

---

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
