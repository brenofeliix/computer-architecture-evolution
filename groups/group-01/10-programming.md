# 10. Physical Programming

## 10.1 Hardware-Level Programming
O ENIAC não possuía ISA pois toda sua programação era manual, sem uma memória de programa pra determinar a sequência de operações. O mais próximo do conceito de ISA, no ENIAC, foi após a modificação feita em 1948, que permitiu que ele executasse programas representados por instruções codificadas.

### 10.1.1 O que é ISA?
ISA é sigla para Instruction Set Architecture, que significa Arquitetura de Conjunto de Instruções

É a especificação que define as instruções que um processador pode executar e como elas são representadas e utilizadas pelos softwares.

A ISA é o que fica entre o software e o hardware, como um dicionário de comandos que a CPU pode entender e executar

Como principais elementos definidos pela ISA, podemos citar:
Conjunto de instruções, operações como adição, subtração, carregamento e armazenamento;
Registradores, recursos de armazenamento internos visíveis ao programador, o espaço de trabalho mais rápido e direto do computador;
Formatos de instrução, como os códigos da operação e seus argumentos são representados;
Modos de endereçamento, como a instrução identifica os dados que vão ser utilizados;
Fluxo de controle, instruções de comparação, desvio e outras que alteram a sequência de execução.

>Obs.: Nem todas ISAs vão ter exatamente os mesmos recursos, mas todas definem uma interface de programação para determinada arquitetura.

### 10.1.2 Por que o ENIAC não possuía ISA
O ENIAC original, de 1946, não tinha ISA porque não utilizava instruções codificadas armazenadas em uma memória de programa para determinar a sequência de operações. Ao invés disso, a programação era feita por configuração física de cabos, interruptores e unidades eletrônicas.

O ENIAC tinha as operações definidas no próprio hardware (adição, subtração, multiplicação e divisão). Apesar disso, as operações não eram organizadas em um conjunto de instruções de máquina comparáveis aos das próximas arquiteturas que viriam com o decorrer do século.

Assim, é melhor afirmar que o ENIAC de 1946 não possuía uma ISA de programa armazenado, ao invés de dizer que não tinha qualquer mecanismo de instrução/controle.

#### Principais razões:

Falta de memória de programa convencional. O ENIAC possuía registradores e acumuladores capazes de armazenar valores numéricos mas não armazenava uma sequência de instruções como os computadores de programa armazenado;

Programação por conexões físicas. O fluxo de execução era determinado por cabos e os interruptores determinavam as operações que cada unidade faria;

Ausência de instruções com endereços de memória. Os dados eram transferidos entre unidades por conexões estabelecidas antes da operação;

Controle distribuído. O funcionamento do ENIAC dependia da cooperatividade de diversas unidades eletrônicas, ao invés de um processador central que decodificasse instruções armazenadas.

### 10.1.3 Como era feita a programação do ENIAC
A programação original do computador exigia que as programadoras transformassem o problema matemático que deveria ser resolvido em uma sequência de operações. Após isso, elas precisavam configurar fisicamente o computador pra executar as operações demandadas.

Esse processo era dividido em diferentes etapas até chegar ao resultado final:
1. Análise do problema matemático, onde as programadoras estudavam as equações e definiam quais operações seriam necessárias, os dados utilizados e as unidades eletrônicas que participariam do problema.
2. Distribuição das operações, onde elas eram atribuídas aos componentes disponíveis, como os 20 acumuladores, a unidade multiplicadora, a unidade divisora e tabelasde funções.
3. Conexão dos cabos, onde elas configuravam os cabos de transmissão de dados e sinais de programação entre as unidades.
4. Configuração de interruptores, que determinavam o comportamento das unidades (operações, parâmetros e configurações de controle).
5. Organização da sequência de execução, que era coordenada por sinais elétricos de programação. Onde Master Programmer ajudava a controlar repetições e mudanças na sequência e a Cycling Unit enviava pulsos de sincronização para as unidades.
6. Testes e execução, onde as programadoras testavam as conexões e verificavam os resultados. Caso houvesse algum erro, isso podia exigir mais alterações físicas.

> Obs.: Um processo desse podia levar de horas à dias de planejamento e configuração, enquanto a execução era extremamente rápida.

Em um exemplo prático de uma operação de adição:
150 + 350 = 500
No ENIAC, o procedimento simplificado seria:
1. Configurar os argumentos 150 e 350 nos acumuladores.
2. Configurar os interruptores para a adição.
3. Estabelecer as conexões para transmitir o valor de um acumulador para o outro.
4. Enviar o sinal que inicia a operação.
5. Depois da execução, o acumulador final conteria o resultado 500.

### 10.1.4 Como o ENIAC controlava as operações sem uma ISA
Apesar do ENIAC não possuir uma ISA tradicional, ele continha componentes especializados no seu hardware que controlavam seu funcionamento. Tais quais:
Cycling Unit, que gerava pulsos;
Master Programmer, que controlava sequências, repetições e outros;
Acumuladores, que armazenavam números e realizavam adições/subtrações;
Program Trays, que transportavam sinais de controle entre as unidades;
Digit Trays, que transportavam informações numéricas;
Function Tables, que armazenavam valores e tabelas de constantes.

### 10.1.5 A evolução do ENIAC
Em 1948, o ENIAC foi modificado para executar programas representados por instruções codificadas. A modificação foi desenvolvida para reduzir a necessidade de reorganizar fisicamente os cabos em cada programa. As Function Tables começaram a ser utilizadas pra armazenar sequências de instruções pela configuração de interruptores. 
Dessa maneira, o ENIAC de 1948 passava a se assemelhar mais com o conceito de uma arquitetura com conjunto de instruções (ISA).

![Marlyn e Ruth com a punch cards](./images/photos/Marlyn%20Wescoff%20and%20Ruth%20Lichterman%20demonstrating%20a%20punch%20card%20machine..jpg)

## 10.2 Cable Routing
- Como a conexão dos cabos definia a sequência de operações

## 10.3 Switch Configuration
- Configuração manual das chaves rotativas nos acumuladores e painéis

## 10.4 Flow Control
- Como loops e condições eram cabeadas através do Programador Mestre

## 10.5 The First Programmers
- A papel histórico das 6 programadoras do ENIAC

## 10.6 Debugging Process
- Como a equipe de operação encontrava erros (ou bugs literais kkkk)

## 10.7 Operation Example
- Passo a passo de como, por exemplo, uma adição era feita

## 10.8 1948 Modification
- Evolução para um mecanismo de programa armazenado central