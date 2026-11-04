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
A história da programação do ENIAC é indissociável da história de seis mulheres: Kathleen McNulty, Frances Bilas, Betty Jean Jennings, Ruth Lichterman, Elizabeth Snyder e Marlyn Wescoff. Antes mesmo de o ENIAC ser concluído, essas mulheres já trabalhavam para o Exército dos Estados Unidos com o cargo oficial de "Computers" (Computadores). O trabalho delas consistia em calcular manualmente as exaustivas tabelas de trajetórias balísticas usando calculadoras de mesa mecânicas e o Analisador Diferencial (uma máquina analógica gigantesca).

Quando o hardware do ENIAC ficou pronto, os engenheiros que o construíram (John Mauchly e J. Presper Eckert) focaram na viabilidade elétrica da máquina. Não existiam manuais de instrução, linguagens de programação ou guias de melhores práticas. A tarefa de descobrir como fazer aquele leviatã de 30 toneladas de fato calcular as trajetórias foi entregue a essas seis mulheres.

O trabalho delas foi monumental. Como o ENIAC não tinha uma ISA (conforme visto no tópico 10.1), elas precisaram aprender a lógica da máquina estudando os diagramas de blocos e os esquemas elétricos do hardware. Elas tinham que quebrar equações diferenciais complexas em passos minúsculos (adições, multiplicações, transferências de dados) e, em seguida, mapear fisicamente qual Acumulador faria qual conta, em qual momento, e para onde o resultado seria enviado.

Elas essencialmente inventaram a disciplina da programação de computadores eletrônicos. O processo de roteamento de cabos nas Digit Trays e Program Trays e a configuração de milhares de chaves rotativas (detalhado nos tópicos anteriores) era executado por elas. Apesar de terem sido as mentes lógicas que deram vida à máquina, durante a demonstração pública do ENIAC em 1946, elas não foram devidamente creditadas. Nas fotografias da época, a mídia frequentemente assumia que elas eram apenas modelos contratadas para posar ao lado dos painéis, ou "refrigerator ladies" (moças da geladeira, em referência às propagandas de eletrodomésticos da época). O reconhecimento formal como as primeiras programadoras do mundo só veio décadas mais tarde.

## 10.6 Debugging Process
Se configurar os cabos do ENIAC já era uma tarefa monumental, descobrir onde um erro havia ocorrido era um verdadeiro trabalho de detetive físico. Em uma máquina com mais de 17.000 válvulas, quilômetros de fiação interna e milhares de cabos externos, os erros podiam ter duas naturezas: lógicos (um cabo plugado no soquete errado) ou de hardware (um tubo queimado ou um relé travado).

Para encontrar e corrigir esses erros, a equipe desenvolveu técnicas engenhosas que, de forma analógica, deram origem a muitos dos conceitos de depuração (debugging) que conhecemos hoje.

### 10.6.1 Visual Inspection
A principal interface de feedback do ENIAC não era uma tela, mas sim milhares de pequenas lâmpadas de neon âmbar espalhadas pelos painéis. Cada Acumulador possuía lâmpadas conectadas aos seus contadores de anel, permitindo que as programadoras "lessem" o número exato que estava armazenado naquela unidade apenas olhando para as luzes acesas.

Se um cálculo estivesse dando o resultado final errado, a primeira etapa do debug era rodar um problema de teste (uma equação cuja resposta exata já havia sido calculada à mão). As programadoras observavam as luzes piscarem. Se o Acumulador 7 devesse mostrar o número 5.000 no meio da equação, mas as luzes mostrassem 0, elas sabiam exatamente em qual painel físico o erro estava ocorrendo.

### 10.6.2 Physical Breakpoints
Como a máquina rodava a 100 kHz, as luzes de neon durante a execução normal piscavam tão rápido que formavam apenas um borrão luminoso. Era impossível ver onde o erro acontecia em tempo real. Para resolver isso, a equipe inventou o conceito de Breakpoint (Ponto de Parada) de forma literal.

Se elas desconfiassem que o erro estava acontecendo na 15ª etapa do programa, elas caminhavam até o painel correspondente e simplesmente desconectavam o cabo coaxial de Program Pulse que ligaria a 16ª etapa. Quando a máquina rodava, ela executava tudo em velocidade máxima até chegar no cabo desconectado. Sem o pulso para continuar, o ENIAC "congelava" com todos os dados estáticos salvos nos Acumuladores. As programadoras podiam então caminhar pela sala com uma prancheta, anotando os valores das lâmpadas de neon de cada painel para verificar se a matemática estava correta até aquele ponto.

### 10.6.3 Stepping
Se o erro fosse muito sutil, congelar a máquina não era suficiente; era preciso ver a operação acontecendo em câmera lenta. Como vimos no tópico 3.3, a Unidade Cíclica possuía modos de operação manual.

As programadoras podiam alterar a chave da Unidade Cíclica do modo contínuo para o modo One Add Time (Um Tempo de Adição). Nesse modo, a máquina não rodava sozinha. A operadora segurava um controle remoto com um botão (ligado à máquina por um longo fio). Cada vez que ela apertava o botão, a Unidade Cíclica disparava os pulsos correspondentes a apenas um único ciclo de 200 microssegundos e parava. Isso permitia que a equipe "stepasse" (avançasse passo a passo) a execução do programa, observando fisicamente as válvulas acenderem e apagarem, rastreando o caminho exato do pulso elétrico pelos cabos frontais até encontrar o ponto exato onde a lógica falhava.

> Nota: Sobre bugs literais, embora a famosa história da mariposa encontrada presa em um relé (que popularizou o termo "bug") tenha acontecido com a cientista Grace Hopper no computador Harvard Mark II em 1947, o ENIAC tinha seus próprios problemas biológicos. O intenso calor e a luz gerados pelas milhares de válvulas atraíam insetos voadores que frequentemente causavam curtos-circuitos nos painéis abertos. Além disso, ratos ocasionalmente roíam o isolamento dos cabos inferiores, criando falhas de aterramento que exigiam que os engenheiros caçassem os fios mastigados pelas calhas da sala. Literalmente, caçando bugs e roedores.

## 10.7 Operation Example
- Passo a passo de como, por exemplo, uma adição era feita

## 10.8 1948 Modification
- Evolução para um mecanismo de programa armazenado central