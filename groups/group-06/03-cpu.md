# 3. CPU

## 3.1 Unidades de processamento e Relação Entre Elas

## 3.2 Registradores e Armazenamento Interno Temporário

## 3.3 ULA e Operações Aritméticas e Lógicas

## 3.4 Unidade de Controle e Coordenação dos Componentes

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
