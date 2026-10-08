# Questão de Pesquisa 1 — Problema e objetivo do sistema

**Qual problema de monitoramento o projeto pretende resolver e como o processamento MISD pode contribuir para essa solução?**

## Resposta

O projeto pretende resolver o problema de **monitoramento e identificação precoce de situações anormais de temperatura**. Em um sistema de monitoramento, apenas medir a temperatura não é suficiente para determinar se uma situação é segura ou se existe algum problema. O mesmo valor de temperatura pode precisar ser analisado de diferentes maneiras para que seja possível obter uma interpretação mais confiável.

Por exemplo, uma determinada temperatura pode estar acima de um limite considerado seguro, pode representar uma alteração em relação às temperaturas registradas anteriormente ou ainda pode ser um valor considerado fora de uma faixa plausível para o funcionamento normal do sistema. Dessa forma, o projeto busca demonstrar que um único valor de temperatura pode ser utilizado para realizar diferentes análises e produzir informações mais completas para o usuário.

Esse tipo de necessidade pode ser encontrado em diferentes situações reais, como **cadeias de frio para armazenamento de vacinas, ambientes industriais, data centers e laboratórios**, nos quais alterações de temperatura podem afetar produtos, equipamentos ou o funcionamento do ambiente. O trabalho utiliza esses cenários como referência para demonstrar a importância de analisar a temperatura por diferentes critérios.

### Funcionamento proposto

O protótipo será composto por um **sensor de temperatura DS18B20**, um **ESP32** e um computador executando um programa em **Python**.

O sensor será responsável por medir a temperatura. Esse valor será enviado ao ESP32, que fará a transmissão dos dados por Wi-Fi para o computador. No computador, o programa em Python receberá o valor de temperatura e utilizará **o mesmo dado de entrada em diferentes processos de análise**.

As três análises principais propostas são:

1. **Verificação de limite:** verifica se a temperatura ultrapassou um valor considerado seguro;
2. **Comparação com o histórico:** compara a temperatura atual com temperaturas registradas anteriormente, permitindo identificar alterações significativas;
3. **Detecção de anomalias:** verifica se o valor recebido apresenta características que podem indicar uma situação anormal ou até mesmo um possível problema na medição.

Assim, em vez de utilizar a temperatura apenas para uma única decisão, o sistema utiliza o mesmo valor para obter diferentes informações.

## Relação com o MISD

A contribuição do conceito **MISD (Multiple Instruction, Single Data)** está justamente na utilização de **múltiplas instruções ou processos sobre um único fluxo de dados**.

Na Taxonomia de Flynn, o MISD representa uma organização na qual diferentes instruções trabalham sobre um mesmo dado. No projeto, essa ideia é representada da seguinte forma: o sensor fornece **um único valor de temperatura**, e esse mesmo valor é encaminhado para diferentes processos de análise.

Podemos representar a ideia de maneira simplificada:

**Sensor → ESP32 → Wi-Fi → Python → mesmo valor de temperatura**

Depois que o Python recebe esse valor, ele é utilizado por três análises:

**Temperatura → Verificação de limite**
**Temperatura → Comparação com histórico**
**Temperatura → Detecção de anomalia**

Portanto, existe **um único dado de entrada**, que é a temperatura medida pelo sensor, mas existem **diferentes operações realizadas sobre esse dado**.

Por exemplo, supondo que o sensor registre uma temperatura de **35 °C**, o programa poderia utilizar esse mesmo valor para realizar as três análises:

* A análise de **limite** poderia verificar se 35 °C ultrapassa o limite estabelecido;
* A análise de **histórico** poderia comparar 35 °C com as temperaturas registradas anteriormente;
* A análise de **anomalia** poderia verificar se 35 °C está dentro de uma faixa considerada plausível.

Cada análise produz uma informação diferente, mesmo tendo recebido exatamente o mesmo valor de temperatura como entrada.

## Por que utilizar o MISD no projeto?

A utilização do MISD é importante principalmente para demonstrar, de maneira prática, como um mesmo dado pode ser submetido a diferentes formas de processamento. Isso permite relacionar um conceito estudado em **Arquitetura e Organização de Computadores** com uma aplicação de monitoramento de temperatura.

O projeto também permite compreender que o objetivo não é simplesmente coletar uma temperatura, mas **interpretar essa temperatura sob diferentes perspectivas**. Dessa maneira, o sistema pode fornecer informações mais completas do que uma análise baseada em apenas um critério.

Essa ideia possui relação com exemplos históricos associados ao conceito de MISD, como sistemas tolerantes a falhas e arrays sistólicos. O trabalho utiliza como exemplo o sistema de computadores do ônibus espacial da NASA, no qual computadores redundantes trabalhavam sobre os mesmos dados para aumentar a confiabilidade das decisões.

No protótipo desenvolvido pelo grupo, a ideia é aplicada de forma mais simples e didática: em vez de utilizar vários processadores físicos, são utilizados **diferentes processos de análise em software**, todos trabalhando sobre o mesmo valor de temperatura.

## Limitação conceitual

É importante destacar que o projeto **não representa uma implementação de MISD em hardware no sentido estrito da Taxonomia de Flynn**.

Segundo o próprio trabalho, o MISD tradicionalmente está relacionado a múltiplos processadores físicos trabalhando sobre o mesmo dado. Como o protótipo será desenvolvido utilizando um ESP32 e um programa em Python, a equipe está utilizando o MISD como uma **analogia conceitual e didática implementada em software**.

Isso significa que o projeto não afirma que o computador utilizado possui uma arquitetura MISD física. O objetivo é demonstrar a lógica fundamental do conceito:

> **múltiplas instruções/análises → um único dado de entrada.**

Essa diferença é importante porque torna o trabalho tecnicamente mais correto e deixa claro que o protótipo foi desenvolvido para demonstrar o conceito de maneira acessível, sem depender de um hardware paralelo especializado.

## Objetivo geral do protótipo

Dessa forma, o objetivo geral do projeto é **desenvolver um protótipo acadêmico capaz de coletar uma temperatura, transmitir esse dado para um computador e submetê-lo a diferentes processos de análise, demonstrando de forma prática a ideia de múltiplas instruções atuando sobre um único dado, característica central do conceito MISD**.

O projeto busca, portanto, unir três elementos:

* **Monitoramento:** obter a temperatura por meio de um sensor;
* **Processamento:** analisar o mesmo valor por diferentes critérios;
* **Aplicação do conceito MISD:** demonstrar a relação entre um único dado de entrada e múltiplas instruções de processamento.

Com isso, o grupo consegue relacionar o conteúdo teórico da **Taxonomia de Flynn** com uma aplicação prática de monitoramento, utilizando tecnologias acessíveis como **DS18B20, ESP32 e Python**. O trabalho também deixa explícito que a proposta é uma simulação conceitual do comportamento MISD em software, e não uma implementação de hardware MISD.

### Conclusão da Questão 1

Portanto, o problema que o projeto pretende abordar é a **necessidade de analisar a temperatura por diferentes critérios para identificar situações que possam indicar condições anormais**. O MISD contribui para essa solução como modelo conceitual porque permite representar a utilização de **um mesmo dado de temperatura em diferentes processos de análise**.

Assim, o protótipo demonstra de maneira prática a relação:

**um dado de entrada → múltiplas análises → diferentes resultados → decisão mais informada.**

Essa abordagem permite que o grupo demonstre o conceito MISD em uma aplicação simples de monitoramento, ao mesmo tempo em que reconhece a diferença entre a definição tradicional do MISD em hardware e a sua utilização como modelo conceitual em software.
