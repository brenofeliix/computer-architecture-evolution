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
A conexão dos cabos era um dos principais mecanismos de programação do ENIAC, pois determinava como os dados e os sinais de controle circulavam entre suas unidades funcionais. Diferentemente dos computadores modernos, nos quais a sequência de operações é definida principalmente por instruções armazenadas na memória, o ENIAC dependia de conexões físicas realizadas manualmente pelos operadores.
Os cabos conectavam unidades como acumuladores, multiplicadores, divisores e unidades de controle, estabelecendo os caminhos percorridos pelos resultados intermediários durante os cálculos. Por exemplo, o resultado de uma multiplicação poderia ser encaminhado para um acumulador, que realizaria operações adicionais e enviaria o resultado para outra unidade, conforme as conexões estabelecidas.
Além disso, as conexões entre os painéis determinavam quais unidades seriam acionadas e como os sinais elétricos coordenariam a execução das operações. Dessa forma, a sequência de processamento era definida pela combinação entre o cabeamento físico e os mecanismos de controle do sistema.
A programação por cabos oferecia flexibilidade para resolver diferentes problemas matemáticos, mas exigia tempo e atenção. Para executar uma nova tarefa, frequentemente era necessário modificar as conexões manualmente, tornando a reprogramação um processo trabalhoso e sujeito a erros de configuração.

## 10.3 Switch Configuration
A configuração das chaves e dos interruptores do ENIAC era uma etapa fundamental da programação, pois permitia definir parâmetros numéricos, selecionar operações matemáticas e estabelecer determinadas condições de funcionamento das unidades. Os operadores realizavam esses ajustes manualmente nos painéis de controle, de acordo com as necessidades de cada problema.
Nos acumuladores, as chaves rotativas e outros controles eram utilizados para configurar aspectos das operações aritméticas, como a quantidade de repetições de determinadas operações e os parâmetros necessários à execução dos cálculos. Dependendo da unidade e de sua função, os controles também permitiam selecionar operações, definir condições de funcionamento e ajustar a sequência de execução.
A configuração dos painéis complementava o cabeamento físico: enquanto os cabos estabeleciam as conexões entre as unidades, as chaves determinavam como determinados componentes deveriam operar dentro dessa estrutura. Assim, a execução de um programa dependia da combinação entre as conexões físicas e os ajustes realizados pelos operadores.
Embora esse método permitisse adaptar o ENIAC a diferentes tarefas, a configuração manual exigia conhecimento técnico, planejamento e verificação cuidadosa. Um ajuste incorreto poderia comprometer os cálculos ou impedir a execução adequada do programa. Portanto, as chaves e os controles representavam elementos essenciais para o funcionamento do ENIAC, mas também evidenciavam as limitações de sua programação em comparação com os sistemas digitais modernos.

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

Descrição do passo a passo de como ações e equações eram preparadas e feitas pelo sistema.

### 10.7.1 Elementos

Descrição de quais eram, onde se encontravam e qual a serventia dos elementos para o processo.

#### 10.7.1.1 Bandejas de dígitos	

Se encontravam empilhadas acima dos painéis, contendo 11 fios e um terra, cada bandeja passava por 4 painéis. Sua principal serventia era levar números entre unidades, sendo 10 fios para os dígitos e 1 para o sinal, todos ao mesmo tempo.

#### 10.7.1.2 Cabos de dígito	

Vinham das tomadas de entrada e saída de dígito de cada unidade, sendo levados até os terminais de 12 pontos das bandejas. Eram responsaveis por conectar a unidade ao “tronco” de dígitos.

#### 10.7.1.3 Bandejas e cabos de programa	

Montadas na parte de baixo do painel, continham 11 linhas de programa por tronco, essas bandejas levavam os pulsos de programa (CPP) que eram a forma do sistema dar um sinal de "início" e "final" de processo.

#### 10.7.1.4 Chaves de programa	

Ficavam na frente de cada unidade, nos controles de programa, as chaves definiam o que a unidade faz quando o pulso chega.

#### 10.7.1.5 Caixa de carga resistiva	

Era plugada em um terminal livre na ponta do tronco, permitindo ligar vários dispositivos em paralelo no mesmo tronco.

#### 10.7.1.6 Neons	

separados um por flip-flop, nos painéis, usados para mostrar o estado interno. Com os modos de “um tempo de adição” e “um pulso”, permitiam parar a máquina e conferir.

### 10.7.2 Explicação da razão de montagem

Tendo em vista que apenas um número de 10 dígitos circula por tronco de cada vez, a caixa de carga resistiva em complemento com o restante permitia várias unidades poderem “ouvir” o mesmo número.

Com vários troncos era possivel realizar transferências simultâneas, e é o operador quem planeja esse paralelismo.

Quando os controles são ligados à mesma linha de programa começam juntos. Já a saída de programa de um controle de repetição, ligada a outra linha, dispara o que vem depois.


### 10.7.3 Exemplos diretos

Exemplos de operação própriamnete ditos.

#### 10.7.3.1 Adição

A adição, representada por (x + y) tinha seu resultado no acumulador de y. Nessa operação, o papel dos cabos de digíto era levar a saída de transmissão do acumulador que guarda x para uma bandeja de dígitos, e a entrada α do acumulador de y é ligada à mesma bandeja. Já nas chaves, a primeira era ligada em A (transmitir, sem limpar, repetição 1) e a segunda em α (receber). Os cabos de programa se organizavam para que uma linha de programa fosse conectada às entradas de programa dos dois controles, para que comecem juntos. O pulso chega e, num único tempo de adição (200 µs), o primeiro emite os pulsos de dígito e o segundo avança seus anéis, propagando o vai-um após isso a saída de programa do controle vai a outra linha, iniciando a operação seguinte.

Um exemplo mais simples seria: os acumuladores 6, 7 e 8 guardam A, B e C. O acumulador 6 transmite A pela bandeja 1 e retém, o acumulador 7 só limpa, e o 8 recebe A e mantém. Resultando em A, 0 e A + C.

#### 10.7.3.2 Subtração

A montagem fisíca é a mesma, mas a chave fica em S e o cabo sai da tomada de saída “subtract”. Por exemplo, no acumulador 7 essa saída levaria a bandeja 3. O número saía como complemento, com sinal invertido.

##### 10.7.3.2.1 Transferência dos dados

Um acumulador transmite e outro recebe. Se o emissor tem a chave de limpeza, o valor é movido, não copiado.

##### 10.7.3.2.2 Consulta a tabela de função

Um acumulador envia o argumento (0 a 99) à tabela, por cabo de dígito, lá chaves da tabela escolhem entre F(x−2) e F(x+2) e o número de repetições. A tabela transmite pela bandeja, e um acumulador receptor guarda o valor.

#### 10.7.3.3 Multiplicação

Os acumuladores 11 (multiplicador) e 12 (multiplicando) recebem os operandos pelos canais α e β. O multiplicador tem conexões fixas com os acumuladores associados, e seus receptores ligam direto aos circuitos dos acumuladores de produto, sem usar os controles deles, já a saída de resposta liga por cabo direto, sem carga resistiva. Assim a o valor do multiplicador + 4 tempos de adição (11 para um multiplicador de 7 dígitos) era o tempo de ciclos usado, a tabuada de resistores faz o trabalho de encontrar e selecionar o resultado, nisso o acumulador 15 transmite o produto, positivo e negativo, e limpa. Dos 20 dígitos do produto, ficam os 10 mais significativos. No fim um CPP sai do 11º tempo, para que algum acumulador receba o resultado e os operandos sejam preservados.

#### 10.7.3.4 Divisão e raiz quadrada

os acumuladores associados para a operação eram: 4 (quociente), 5 (numerador), 7 (denominador) e 9 (deslocador), o método consistia em subtrações e somas repetidas. Testa-se o sinal do resto parcial a cada passo. Se ficou negativo, desloca-se o resto e soma-se o divisor até voltar a positivo. O acumulador de quociente conta as operações em cada casa decimal para manter a conta das divisões.

Na raiz quadrada o processo é semelhante, onde apenas se difere que no fim, o acumulador 7 guarda o dobro da raiz e o 5 guarda o resto.

para ambos os processos, a disposição dos cabos é a mesma do multiplicador, com saída ligada direto à entrada de dígito, sem carga.

### 10.7.4 Leitura e impressão

O leitor lê 8 números de 10 dígitos de um cartão para os relés do transmissor de constantes, em cerca de 2.500 tempos de adição (0,5 s). Cada número pode sair inteiro ou só a metade esquerda ou direita, em um tempo de adição. A impressora grava os 8 acumuladores (1, 2, 15 a 20) em cerca de 1.500 tempos de adição (0,3 s). A relação entre dígitos do acumulador e colunas do cartão é feita em plugboard.
Após todo o processo o leitor só emite seu CPP de término depois de receber um pulso de programa de outra unidade. Isso deixa o resto da máquina trabalhar em paralelo enquanto o cartão é lido.

## 10.6 Debugging Process
- Como a equipe de operação encontrava erros (ou bugs literais kkkk)

## 10.7 Operation Example
- Passo a passo de como, por exemplo, uma adição era feita

## 10.8 1948 Modification

A evolução para um mecanismo de programa armazenado central

### 10.8.1 Razão

Sabendo que montar um problema novo levava semanas de preparo e verificação , e acertar chaves e cabos podia levar dias, e que seu uso principal era balística, com problemas parecidos entre si. Von Neumann sugeriu escolher o código por chaves, mantendo os cabos fixos em problemas-padrão. A execução paralela dificultava a programação, e um código “conversor” foi criado para operar em série. No fim, a velocidade perdida era aceitável, quase todo problema real já era limitado pela E/S eletromecânica, mesmo sem usar o paralelismo.

### 10.8.2 data

O projeto foi idealizado em 1947. Em julho daquele ano, Adele Goldstine escreveu um plano de controle central e Clippinger sintetizou o trabalho do grupo de programação em um código de 60 ordens. O conversor foi instalado em março de 1948 com o primeiro programa complexo rodou em 12 de abril de 1948: uma simulação de Monte Carlo de nêutrons, de John e Klara von Neumann.

### 10.8.3 Instalação

Havia propostas de 51, 60 e 94 ordens. A de 51 usava só o hardware original, e as outras exigiam o conversor. Cada tabela de função guardava 600 instruções de dois dígitos: uma linha de 12 dígitos guarda seis pares.

#### 10.8.3.1 O mecanismo

Um seletor de ordens, formado por 12 portas controladas em pares por um anel de 6 estágios, libera um par de dígitos por vez de cada linha, depois dos seis pares, o argumento da tabela aumenta em 1 e o anel volta ao início. Isso funciona como o contador de programa, já o conversor é um stepper de 10 estágios combinado com os 10 steppers de 6 estágios do programador mestre. Um dígito α (0 a 9) entra no primeiro, e o dígito β (0 a 5) entra nos outros. O resultado são 60 pulsos distintos, cada um estimulando uma ordem. Dessa forma um endereço de três dígitos no acumulador 6 escolhe entre as 3 tabelas.
A sequência básica antes de cada ordem leva 7 tempos de adição, mas costuma começar antes de a ordem anterior terminar. Já os acumuladores ganharam papéis fixos, sendo o 6 como contador de programa, o 15 como acumulador principal e o 8 como ponteiro das tabelas. Dessa forma as conexões originais passaram a ser tratadas como fiação permanente.

### 10.8.4 Impacto

O principal ganho foi que reprogramar agora passou de dias para horas. contudo, a velocidade caiu por um fator de 6 e o paralelismo acabou. Houve também uma limitação nova, o programa ficava em chaves de tabelas de função, ou seja, memória só de leitura. Por isso alguns historiadores não o consideram um verdadeiro computador de programa armazenado. Tanto que Haigh, Priestley e Rope preferem chamá-lo de “paradigma de código moderno”
Na sequência de evolução do Eniac, foi percebido que o programa original não ficava na memória de dados porque os 20 acumuladores eram pequenos demais para guardar dados e programa. Sendo assim um motivo para a expansão posterior para núcleo magnético em 1953.