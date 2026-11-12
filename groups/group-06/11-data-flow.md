# 11. Acesso aos Dados e Fluxo de Informação

## 11.1 Origem das Informações
Na Arquitetura de Harvard clássica, as informações utilizadas pelo processador vêm de dois espaços de armazenamento distintos: a memória de instruções e a memória de dados. A memória de instruções armazena o programa, ou seja, a sequência de comandos que determina quais operações o processador deve executar. Essas instruções são buscadas pelo processador durante a execução e podem indicar operações matemáticas, comparações, transferências de informações e desvios na sequência do programa. Já a memória de dados armazena as informações que serão utilizadas nessas operações, como números, variáveis e resultados intermediários. A principal diferença entre essas categorias está em suas funções: as instruções determinam o que deve ser feito, enquanto os dados representam os valores sobre os quais as operações serão realizadas. Na Arquitetura de Harvard clássica, essas memórias possuem caminhos de acesso separados, permitindo que o processador busque instruções e acesse dados simultaneamente, desde que a implementação ofereça os recursos necessários. Portanto, a origem das informações depende de sua finalidade: os comandos são obtidos na memória de instruções, enquanto os valores necessários ao processamento são obtidos na memória de dados.

## 11.2 Programa e pseudoinstruções

Considere a memória de dados inicialmente preenchida com `A = 5`, `B = 7` e `C = 0`. A memória de instruções contém:

```text
LOAD R1, [A]        ; R1 recebe 5
LOAD R2, [B]        ; R2 recebe 7
ADD   R3, R1, R2    ; R3 recebe 12
STORE [C], R3       ; C recebe 12
```

`LOAD`, `ADD` e `STORE` são **pseudoinstruções didáticas**, não instruções de uma ISA específica. Os registradores e a ULA representam uma implementação possível, não elementos exigidos pela definição Harvard. O exemplo separa a origem das instruções da origem dos operandos (Arvind et al., 2015, cap. 5).

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
