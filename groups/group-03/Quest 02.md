# Questão de Pesquisa 2 — Dados de entrada

**Quais dados devem ser coletados pelo sistema e como eles serão utilizados para representar o funcionamento do projeto?**

## 1. Introdução

O projeto tem como principal dado de entrada a **temperatura medida pelo sensor DS18B20**, que será conectado ao ESP32. Essa informação será utilizada como base para o funcionamento do protótipo, permitindo acompanhar o caminho percorrido pelo dado desde a coleta até o processamento e a apresentação dos resultados.

A coleta dos dados é uma etapa fundamental, pois todas as operações realizadas pelo sistema dependerão das informações obtidas pelo sensor. Após a medição, o valor da temperatura será recebido pelo ESP32 e transmitido por meio de uma conexão Wi-Fi para um computador, onde será processado por um programa desenvolvido em Python.

Além do valor da temperatura, o sistema poderá registrar informações complementares, como a data e o horário de cada medição. Esses registros permitirão organizar os dados coletados e construir um histórico que poderá ser utilizado nas análises posteriores.

A escolha desses dados está relacionada ao escopo acadêmico e simplificado do projeto, que busca demonstrar a utilização de um mesmo dado de entrada em diferentes processos de análise, estabelecendo uma relação didática com o conceito MISD (*Multiple Instruction, Single Data*).

## 2. Identificação dos dados de entrada

O sistema utilizará principalmente a temperatura medida pelo sensor. Para organizar as informações e permitir as análises propostas, poderão ser considerados os seguintes dados:

| Dado | Descrição | Finalidade |
|---|---|---|
| Temperatura | Valor numérico medido pelo sensor DS18B20, em graus Celsius (°C). | Principal dado de entrada utilizado nas análises. |
| Data e horário | Momento em que cada medição foi realizada ou registrada pelo sistema. | Organizar o histórico e identificar quando cada temperatura foi obtida. |
| Limite de temperatura | Valor de referência definido pelo grupo para a análise. | Verificar se a temperatura recebida ultrapassou o limite estabelecido. |
| Temperaturas anteriores | Valores registrados em medições anteriores. | Comparar a temperatura atual com o histórico. |
| Critérios de análise | Regras definidas pelo grupo para avaliar alterações e possíveis anomalias. | Orientar a interpretação dos dados recebidos. |

É importante diferenciar os dados efetivamente coletados pelo sensor das informações utilizadas pelo programa. A temperatura será medida pelo DS18B20, enquanto a data e o horário poderão ser associados à medição pelo sistema. O limite de temperatura e os critérios de análise serão definidos pelo grupo, e as temperaturas anteriores serão obtidas a partir dos registros armazenados.

Dessa forma, o projeto não dependerá exclusivamente de uma leitura isolada. O valor atual poderá ser interpretado considerando os parâmetros definidos e as informações registradas anteriormente.

## 3. A temperatura como principal dado de entrada

A temperatura será o principal dado de entrada porque representa a informação obtida diretamente do ambiente monitorado. O sensor DS18B20 será responsável por realizar a medição, produzindo um valor que será utilizado pelas etapas seguintes do protótipo.

Por exemplo, supondo que o sensor registre uma temperatura de 38 °C, esse valor será encaminhado ao ESP32 e, posteriormente, transmitido ao computador por Wi-Fi.

O fluxo de coleta e transmissão será representado da seguinte maneira:

```text
Ambiente monitorado
        |
        v
Sensor DS18B20
        |
        v
Temperatura medida
        |
        v
ESP32
        |
        v
Comunicação Wi-Fi
        |
        v
Computador executando Python
        |
        v
Processamento do dado recebido
```

Nesse processo, o sensor será responsável pela medição, o ESP32 realizará a comunicação com o computador e o programa em Python utilizará o valor recebido para executar as análises previstas.

O valor da temperatura será, portanto, a informação central do sistema. As demais informações terão a função de auxiliar na organização, na interpretação e na comparação dos dados.

## 4. Informações complementares e organização dos registros

Embora a temperatura seja o principal dado de entrada, registrar o momento de cada medição poderá contribuir para o funcionamento do protótipo.

Por exemplo, o sistema poderá organizar as leituras da seguinte forma:

| Medição | Temperatura | Horário registrado |
|---|---:|---|
| 1 | 25 °C | 10:00 |
| 2 | 27 °C | 10:05 |
| 3 | 29 °C | 10:10 |
| 4 | 35 °C | 10:15 |

Os valores apresentados são apenas um exemplo ilustrativo de como os registros poderão ser organizados durante os testes. Eles não representam resultados reais obtidos pelo grupo.

Com essas informações, será possível identificar a sequência das medições e observar as alterações de temperatura ao longo do tempo.

O horário também ajudará a diferenciar as leituras, permitindo relacionar cada valor ao momento em que foi registrado. Caso o grupo implemente o registro da data, será possível organizar o histórico entre diferentes dias de teste.

Essas informações serão especialmente úteis para a comparação com o histórico, pois essa análise depende da existência de registros anteriores.

## 5. Utilização dos dados nas análises do sistema

Depois que o computador receber a temperatura enviada pelo ESP32, o programa em Python utilizará esse mesmo valor nos três processos de análise definidos no projeto: verificação de limite, comparação com o histórico e identificação de possíveis anomalias.

Cada processo terá uma finalidade específica, mas todos utilizarão a temperatura recebida como dado principal de entrada.

### 5.1. Verificação de limite

A primeira análise verificará se a temperatura atual ultrapassou um limite previamente estabelecido pelo grupo.

Por exemplo, supondo que o limite definido para um teste seja de 40 °C e que o sensor registre 38 °C, o programa realizará a comparação:

```text
Temperatura recebida: 38 °C
Limite definido: 40 °C

38 < 40

Resultado: temperatura abaixo do limite.
```

Se uma nova leitura apresentar 42 °C, o resultado será diferente:

```text
Temperatura recebida: 42 °C
Limite definido: 40 °C

42 > 40

Resultado: temperatura acima do limite.
```

Os valores utilizados nesse exemplo são ilustrativos. O limite definitivo deverá ser estabelecido pelo grupo de acordo com os objetivos e as condições dos testes.

Essa análise demonstrará como o sistema pode utilizar o dado recebido para realizar uma verificação baseada em um parâmetro previamente definido.

### 5.2. Comparação com o histórico

A segunda análise utilizará a temperatura atual e os valores registrados anteriormente para observar possíveis alterações ao longo do tempo.

Por exemplo, supondo que o histórico contenha as seguintes temperaturas:

```text
25 °C
26 °C
25 °C
27 °C
```

Se a temperatura atual for de 35 °C, o programa poderá comparar esse valor com as medições anteriores e identificar uma diferença significativa.

Essa comparação não significa, necessariamente, que exista um problema. Ela indica apenas que o valor atual apresenta uma alteração em relação aos registros utilizados como referência.

Os critérios para determinar o que será considerado uma alteração significativa deverão ser definidos pelo grupo durante a implementação.

Essa análise também demonstra a importância de armazenar as temperaturas, pois os valores anteriores serão necessários para realizar as comparações.

### 5.3. Identificação de possíveis anomalias

A terceira análise verificará se a temperatura recebida apresenta um comportamento diferente do esperado, considerando os critérios estabelecidos pelo grupo.

Por exemplo, supondo que as medições anteriores estejam próximas de 25 °C e que uma nova leitura apresente 45 °C, o programa poderá identificar esse valor como uma possível anomalia, caso ele ultrapasse os critérios definidos para essa análise.

A identificação de uma possível anomalia não significa que o sensor esteja necessariamente com defeito ou que exista uma falha no ambiente monitorado. O resultado representa apenas uma indicação de que o dado merece atenção de acordo com as regras implementadas.

Essa análise terá finalidade didática, permitindo demonstrar como a mesma temperatura pode ser utilizada para investigar uma condição diferente daquela verificada pela análise de limite ou pela comparação histórica.

## 6. Relação entre os dados de entrada e o conceito MISD

A definição dos dados de entrada está diretamente relacionada ao objetivo principal do projeto: demonstrar, de maneira didática, a utilização de um mesmo dado em diferentes processos de análise.

MISD significa *Multiple Instruction, Single Data*, expressão que pode ser traduzida como múltiplas instruções e um único dado.

No protótipo, essa ideia será representada pela utilização da mesma temperatura recebida pelo computador em três análises distintas.

Considerando uma leitura de 38 °C, o funcionamento poderá ser representado da seguinte forma:

```text
                 Temperatura: 38 °C
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
   Verificação de   Comparação com   Identificação
       limite        o histórico    de anomalias
          |              |              |
          v              v              v
      Resultado      Resultado      Resultado
       da análise     da análise     da análise
```

Nesse exemplo, existe um único valor principal de entrada, mas são realizadas operações diferentes sobre ele.

A verificação de limite examina a relação entre a temperatura e um valor de referência. A comparação histórica considera as medições anteriores. A identificação de anomalias utiliza os critérios definidos para procurar comportamentos considerados fora do esperado.

Assim, cada análise produz uma informação específica, embora todas utilizem a mesma temperatura recebida.

É importante destacar que essa implementação representa uma demonstração conceitual do MISD em software. O uso de diferentes processos em Python não significa, por si só, que o computador esteja utilizando uma arquitetura MISD física, conforme a classificação tradicional da Taxonomia de Flynn.

## 7. Armazenamento e utilização posterior dos dados

Os dados coletados também poderão ser armazenados para permitir consultas e comparações futuras.

O armazenamento deverá manter, principalmente, os valores de temperatura e, quando implementado, o momento de cada medição. Os resultados das análises também poderão ser registrados para facilitar a consulta das informações produzidas pelo sistema.

Por exemplo, um registro poderá apresentar:

```text
Data e horário: 15/10/2026 10:15
Temperatura: 35 °C
Verificação de limite: abaixo do limite
Comparação histórica: alteração identificada
Análise de anomalia: resultado conforme os critérios definidos
```

Esse registro é apenas um exemplo ilustrativo. A estrutura definitiva dependerá da implementação realizada pelo grupo.

A manutenção do histórico permitirá que o programa consulte valores anteriores para comparar com novas medições. Além disso, os registros poderão ajudar nos testes, pois permitirão verificar quais temperaturas foram utilizadas e quais resultados foram produzidos.

A forma definitiva de armazenamento será definida durante o desenvolvimento, priorizando uma solução simples e adequada ao caráter acadêmico do protótipo.

## 8. Importância dos dados para o funcionamento do protótipo

Os dados de entrada são essenciais porque todas as etapas seguintes dependem das informações obtidas pelo sensor.

Sem a coleta da temperatura, o sistema não terá o valor necessário para executar as análises propostas. Sem os registros anteriores, a comparação histórica ficará limitada. Da mesma forma, sem parâmetros definidos pelo grupo, não será possível determinar de maneira consistente quais valores ultrapassam os limites ou atendem aos critérios de possível anomalia.

Por isso, será necessário garantir que o valor medido seja recebido corretamente pelo ESP32, transmitido ao computador e interpretado pelo programa em Python.

Também será importante verificar se as informações estão sendo organizadas de maneira adequada para permitir o armazenamento, a consulta e a apresentação dos resultados.

A definição dos dados de entrada, portanto, contribuirá para estabelecer o que será coletado, quais informações serão utilizadas como referência e como o sistema deverá interpretar cada medição.

## 9. Escopo dos dados utilizados

Para manter o projeto dentro da proposta acadêmica definida pelo grupo, o sistema trabalhará principalmente com a temperatura medida pelo sensor DS18B20.

A data e o horário poderão ser utilizados como informações complementares, enquanto os limites, os registros históricos e os critérios de análise servirão de apoio ao processamento.

Não será necessário adicionar outros sensores para demonstrar o funcionamento proposto. A intenção é manter o foco em um único tipo principal de dado e acompanhar seu percurso desde a coleta até os resultados.

Essa simplificação permitirá concentrar o desenvolvimento nas etapas de coleta, transmissão, processamento, análise, armazenamento e apresentação, sem ampliar desnecessariamente a complexidade do protótipo.

## Conclusão da Questão 2

Portanto, o principal dado de entrada do projeto será a temperatura medida pelo sensor DS18B20, podendo ser acompanhada da data e do horário de cada medição para organizar o histórico. Também serão utilizados parâmetros definidos pelo grupo e registros anteriores para orientar as análises realizadas pelo programa.

O sensor realizará a medição, o ESP32 receberá o valor e o transmitirá por Wi-Fi para o computador, onde o Python utilizará a mesma temperatura nos processos de verificação de limite, comparação com o histórico e identificação de possíveis anomalias.

Os dados poderão ser armazenados para consultas futuras e para auxiliar na verificação dos resultados durante os testes.

Dessa forma, a definição dos dados de entrada permite estabelecer as informações necessárias para o funcionamento do protótipo e demonstra a relação entre coleta, comunicação, processamento e análise. Ao utilizar a mesma temperatura como entrada para diferentes processos, o projeto representa, de maneira didática, a ideia central do MISD: **um único dado de entrada sendo utilizado em diferentes processos de análise**.