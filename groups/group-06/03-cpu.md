# 3. CPU

## 3.1 Unidades de processamento e Relação Entre Elas
A unidade de processamento é a parte do computador responsável por executar as instruções de um programa e realizar operações sobre os dados recebidos. Em um modelo geral de CPU, ela envolve três componentes fundamentais: a Unidade Lógica e Aritmética (ULA), os registradores e a unidade de controle. A ULA realiza operações matemáticas, como adição e subtração, e operações lógicas, como comparações entre valores. Os registradores são pequenas unidades de armazenamento de alta velocidade localizadas dentro do processador, utilizadas para guardar temporariamente dados, instruções, endereços e resultados intermediários durante a execução. Já a unidade de controle coordena o funcionamento dos componentes, interpretando as instruções e enviando sinais que determinam quais operações devem ser realizadas e em que sequência. Esses elementos trabalham em conjunto: a unidade de controle identifica a operação necessária, os registradores fornecem os valores utilizados e a ULA executa o processamento. Após a operação, o resultado pode ser armazenado em um registrador ou enviado para a memória, conforme a instrução executada. Embora a organização interna possa variar entre processadores, a ULA, os registradores e a unidade de controle são elementos comuns nos modelos tradicionais de CPU.
Na Arquitetura de Harvard, esses componentes de processamento desempenham funções semelhantes às encontradas em outras organizações de computadores. A principal diferença não está na definição da ULA, dos registradores ou da unidade de controle, mas na maneira como as instruções e os dados são armazenados e acessados. Na organização Harvard clássica, existe uma memória específica para as instruções do programa e outra para os dados utilizados durante a execução, com caminhos de acesso independentes. Dessa forma, a unidade de controle pode buscar instruções na memória de programa enquanto o processador acessa dados na memória correspondente, desde que a implementação permita essas operações simultâneas. A ULA continua responsável pelos cálculos e pelas operações lógicas, enquanto os registradores armazenam temporariamente as informações necessárias ao processamento. Portanto, é importante distinguir os elementos comuns de uma CPU das características específicas da Arquitetura de Harvard: os componentes internos executam e coordenam as operações, enquanto a separação entre as memórias de instruções e de dados define a organização Harvard.

## 3.2 Registradores e Armazenamento Interno Temporário
Os registradores e acumuladores desempenham a função de locais de armazenamento internos ao processador, caracterizados por altíssima velocidade de acesso, destinados a reter temporariamente dados, operandos, endereços ou estados operacionais necessários durante o fluxo de execução das instruções; diferentemente da memória de dados principal — que oferece uma capacidade de armazenamento ampla e externa ao núcleo para persistir informações do sistema —, esses componentes internos são utilizados de forma transitória para manter valores intermediários e resultados parciais logo após serem manipulados pela unidade lógica e aritmética, agilizando as operações computacionais sem a latência de acessos constantes à memória principal, sendo fundamental destacar que características como a quantidade exata, o tamanho em bits e a organização estrutural desses registradores e acumuladores não possuem um padrão universal fixo, variando inteiramente conforme a arquitetura e a implementação específica adotada por cada fabricante ou modelo de processador.

## 3.3 ULA e Operações Aritméticas e Lógicas
A Unidade Lógica e Aritmética (ULA) executa transformações físicas nos dados por meio de circuitos combinatórios, realizando operações aritméticas (como adição e subtração) e lógicas bit a bit (como AND, OR e deslocamentos). Na Arquitetura Harvard, esse processamento é otimizado devido à separação física entre as memórias e barramentos de instruções e de dados, o que permite que o sistema busque a próxima instrução enquanto a ULA manipula os dados atuais de forma simultânea e sem conflitos.

A circulação dos dados ocorre em um ciclo fechado e sincronizado: os operandos são lidos do Banco de Registradores e enviados diretamente para as entradas da ULA. Após o processamento, a ULA atualiza o Registrador de Flags com o status do resultado (como indicações de zero ou sinal negativo) e envia o valor final de volta pelo barramento interno para ser gravado no registrador de destino ou na memória de dados.

## 3.4 Unidade de Controle e Coordenação dos Componentes
A Unidade de Controle (UC) atua como o elemento central de gestão e coordenação dentro do processador. A sua função primordial consiste em direcionar o fluxo de informações e gerir o funcionamento global das restantes unidades (como a ULA e os registadores), garantindo que as operações ocorrem de forma sincronizada e na ordem correta.

* **Emissão de Sinais de Controlo:** A Unidade de Controle interpreta os códigos das instruções provenientes da memória de programa e gera impulsos ou sinais elétricos específicos para os demais componentes. Estes sinais determinam, por exemplo, quando a ULA deve efetuar uma operação, quando os registadores devem reter ou libertar dados, e como os barramentos devem gerir as transferências.
* **Papel Coordenador:** A UC assume um papel estritamente diretivo sobre os circuitos internos e canais de comunicação, assegurando a articulação entre as ordens lógicas e as ações físicas executadas pelo hardware, sem a necessidade de esgotar o ciclo completo e sequencial detalhado de cada instrução de forma isolada.
## 3.5 Clock e Sincronização

## 3.6 Ciclo de Instrução


## 3.7 Diagrama das Unidades Principais
Figura 1 — **Unidades de processamento em um modelo geral com organização Harvard.**

O diagrama apresenta um modelo funcional didático de processador conectado a espaços separados de memória de instruções e de dados. No caminho de instruções, o contador de programa (PC) fornece o endereço da próxima instrução à memória de instruções, e a instrução lida é encaminhada ao processador para interpretação pela Unidade de Controle. No caminho de dados, valores mantidos nos registradores podem ser enviados à ULA como operandos; o resultado pode voltar aos registradores ou, conforme a operação, ser usado para acessar a memória de dados. Em uma operação de escrita, um valor pode sair dos registradores em direção à memória de dados.

As setas laranjas representam sinais de controle: elas coordenam operações, como selecionar registradores, orientar a ULA e indicar leitura ou escrita na memória. Esses sinais têm função diferente dos caminhos de dados: eles controlam o que os componentes fazem, enquanto os caminhos verdes transportam valores. Os caminhos azuis representam a comunicação das instruções.

A separação entre os caminhos de instruções e dados representa a organização Harvard. Ela permite que os acessos sejam independentes; a realização simultânea depende da implementação. O diagrama é conceitual: não define quantidade ou largura de registradores, frequência de clock, conjunto de instruções nem um projeto único de CPU.

[Visualizar o diagrama das unidades de processamento](images/harvard-unidades-processamento.svg)

## 3.8 Paralelismo e Potencial de Desempenho

## 3.9 Limitações e Dependência da Implementação
