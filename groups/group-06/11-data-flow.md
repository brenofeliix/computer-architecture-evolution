# 11. Acesso aos Dados e Fluxo de Informação

## 11.1 Origem das Informações
Na Arquitetura de Harvard clássica, as informações utilizadas pelo processador vêm de dois espaços de armazenamento distintos: a memória de instruções e a memória de dados. A memória de instruções armazena o programa, ou seja, a sequência de comandos que determina quais operações devem ser executadas. Já a memória de dados armazena as informações utilizadas durante o processamento, como números, variáveis e resultados intermediários. As instruções indicam o que deve ser feito, enquanto os dados representam os valores sobre os quais as operações serão realizadas. Essas memórias possuem caminhos de acesso separados, permitindo a busca de instruções e o acesso aos dados simultaneamente, desde que a implementação ofereça os recursos necessários.
Para demonstrar essa organização, será utilizada como exemplo uma operação de soma entre dois valores já disponíveis. Nesse modelo, A e B representam os valores de entrada que serão utilizados no cálculo, enquanto C representa o local destinado ao armazenamento do resultado. A operação consiste em receber os dois valores, realizar a adição e guardar o resultado obtido. Esse exemplo foi escolhido porque permite demonstrar de maneira simples três etapas fundamentais do processamento: a leitura dos valores necessários, a execução de uma operação matemática e o armazenamento do resultado. Assim, é possível compreender como as informações são utilizadas durante a execução de uma instrução.
Entretanto, esse exemplo representa apenas um modelo didático para explicar a organização e o funcionamento de uma operação na Arquitetura de Harvard. Ele não descreve obrigatoriamente o funcionamento interno de uma máquina real, pois diferentes implementações podem utilizar instruções, registradores e mecanismos de acesso à memória distintos. Também é importante diferenciar o modelo didático de uma implementação concreta: o primeiro serve para facilitar a compreensão dos conceitos, enquanto o segundo corresponde à arquitetura efetivamente construída em um equipamento. Portanto, nenhum processador ou chip específico foi escolhido para esta demonstração, que tem como objetivo apresentar os conceitos gerais da organização de Harvard sem atribuí-los a uma implementação particular.

## 11.2 Programa e pseudoinstruções

Considere a memória de dados inicialmente preenchida com `A = 5`, `B = 7` e `C = 0`. A memória de instruções contém:

```text
LOAD R1, [A]        ; R1 recebe 5
LOAD R2, [B]        ; R2 recebe 7
ADD   R3, R1, R2    ; R3 recebe 12
STORE [C], R3       ; C recebe 12
```

`LOAD`, `ADD` e `STORE` são **pseudoinstruções didáticas**, não instruções de uma ISA específica. Os registradores e a ULA representam uma implementação possível, não elementos exigidos pela definição Harvard. O exemplo separa a origem das instruções da origem dos operandos (Arvind et al., 2015, cap. 5).
Para demonstrar a tarefa, considere a memória de dados inicialmente preenchida com `A = 5`, `B = 7` e `C = 0`. A memória de instruções contém a sequência abaixo:

```text
LOAD R1, [A]        ; R1 recebe o valor armazenado em A (5)
LOAD R2, [B]        ; R2 recebe o valor armazenado em B (7)
ADD   R3, R1, R2    ; R3 recebe R1 + R2 (12)
STORE [C], R3       ; C recebe o valor de R3 (12)
```

`LOAD`, `ADD` e `STORE` são **pseudoinstruções didáticas**. Elas representam operações genéricas e não pertencem a uma ISA específica. `R1`, `R2`, `R3`, a ULA e uma unidade de controle são componentes de uma implementação ilustrativa; a definição Harvard não exige esses nomes ou uma sequência única de componentes. A separação relevante para o exemplo é entre a memória que fornece as instruções e a memória que fornece os valores `A`, `B` e `C` (Arvind et al., 2015).

## 11.3 Acesso e Circulação de Dados
Na organização Harvard clássica, as instruções do programa e os dados ocupam espaços de memória separados e utilizam caminhos independentes até o processador. Para uma operação aritmética, os valores podem ser lidos de registradores e encaminhados à ULA, que produz um resultado. Quando a instrução precisa buscar um valor armazenado, o processador calcula o endereço e solicita a leitura à memória de dados; o valor retornado pode ser encaminhado a um registrador. Em uma operação de armazenamento, um valor mantido em registrador é enviado ao endereço indicado na memória de dados. Assim, o fluxo depende da instrução executada: uma operação entre registradores pode não acessar a memória de dados, enquanto uma carga ou um armazenamento envolve esse caminho.  

A separação entre os caminhos de instruções e dados permite que seus acessos sejam independentes. Ela pode possibilitar que uma instrução seja buscada enquanto ocorre um acesso a dados, mas a simultaneidade depende da implementação. O diagrama desta seção representa os caminhos de forma geral; ele não especifica tecnologias, larguras ou barramentos que sejam obrigatórios em toda arquitetura Harvard.

**[Figura 1 — Fluxo de dados na organização Harvard clássica](images/harvard-classica-fluxo-informacao.svg)**

*Legenda: As setas indicam o sentido do fluxo. A separação permite acessos independentes; a simultaneidade depende da implementação. Fonte: elaboração própria, com base em Arvind et al. (2015).*


## 11.4 Produção do resultado

A operação soma os operandos: `5 + 7 = 12`. O resultado fica temporariamente em `R3` e a pseudoinstrução `STORE` grava esse valor no endereço simbólico `C` da memória de dados. Ao final, `C = 12`.

## 11.5 Ordem lógica e acessos independentes

A sequência expressa dependências lógicas, não ciclos de clock. Os caminhos separados podem permitir a busca de uma instrução ao mesmo tempo que ocorre um acesso a dados, se a implementação conseguir atender aos dois pedidos. A separação não garante simultaneidade em toda execução nem implica pipeline ou execução simultânea de instruções completas (Arvind et al., 2015, cap. 5).

## 11.6 Escopo e limites do exemplo

Harvard caracteriza a separação entre espaços de instruções e dados e seus caminhos de acesso. O modelo não fixa capacidade, tecnologia, latência, largura de palavra, formato das instruções ou quantidade de registradores; esses pontos dependem da implementação. Quando os espaços usam memórias distintas, a capacidade livre de uma não é automaticamente transferida para a outra. Também não se pode concluir sem especificar a máquina que instruções nunca possam ser lidas como dados ou que programas não possam ser carregados. Este exemplo explica um fluxo possível, não uma microarquitetura universal (Arvind et al., 2015, cap. 5).
Componentes e suas responsabilidades

1. Memória de Instruções
Armazena as pseudoinstruções do programa (ex: carregar, somar, salvar), servindo como o roteiro passo a passo que o circuito deve seguir.

**[Figura 2 - Representação de um fluxo de soma na organização de Harvard Clássica](images/harvard-representacao-fluxo-de-soma.png)**

2. Memória de Dados
Guarda as variáveis persistentes A, B e C. Ela fornece os valores iniciais para o cálculo e armazena o resultado final da soma no endereço C.

3. Processador
Executa todo o ciclo da operação. Ele integra os demais componentes para buscar as instruções, mover os dados, realizar o cálculo e salvar o resultado.

4. PC (Contador de Programa)
Indica o endereço da próxima instrução a ser buscada. Ele dita o ritmo do programa, avançando posição por posição a cada etapa da soma.

5. Unidade de Controle
Decodifica as instruções e coordena o circuito. Ela interpreta o comando de "soma" e envia sinais elétricos que ativam os registradores e a ULA no momento certo.

6. R1, R2 e R3 (Registradores)
Armazenam os dados temporariamente dentro do processador:
• R1 e R2: Seguram os valores de A e B para processamento imediato.
• R3: Guarda o resultado da soma antes de enviá-lo para a memória.

7. ULA (Unidade Lógica e Aritmética)
Realiza o cálculo matemático da adição. Ela recebe fisicamente os valores de R1 e R2, soma-os e entrega o resultado para o registrador R3


## 11.4 Produção do resultado

Na terceira pseudoinstrução, a unidade de processamento soma os operandos: `5 + 7 = 12`. O resultado fica temporariamente em `R3`; a quarta pseudoinstrução grava esse valor no endereço simbólico `C` da memória de dados. Ao final, `C = 12`. O registrador e a ULA ilustram uma forma possível de realizar a operação; sua organização exata depende do processador escolhido.

## 11.5 Ordem lógica e acessos independentes

A sequência acima descreve dependências lógicas: é preciso ler `A` e `B` antes de somá-los e produzir o resultado antes de gravá-lo em `C`. Ela não fixa ciclos de clock. A separação dos caminhos permite que a busca de instruções e os acessos à memória de dados sejam independentes; a ocorrência de acessos simultâneos depende da organização concreta das memórias, das interfaces e do controle do processador. Essa separação, isoladamente, não garante pipeline nem execução simultânea de instruções (Arvind et al., 2015).

## 11.6 Escopo e limites do exemplo

O modelo Harvard caracteriza a separação entre os espaços de instruções e dados e seus caminhos de acesso. Ele não determina a capacidade, a tecnologia, a latência, a largura de palavra, o formato das instruções ou a quantidade de registradores; esses pontos dependem da implementação. A separação pode tornar menos flexível a redistribuição de capacidade entre programa e dados quando os espaços são implementados em memórias distintas, mas não permite concluir, sem especificar a máquina, que instruções nunca possam ser lidas como dados ou que programas não possam ser carregados. O exemplo explica um fluxo possível, não uma ISA ou microarquitetura universal (Arvind et al., 2015).
