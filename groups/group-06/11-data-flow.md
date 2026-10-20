# 11. Acesso aos Dados e Fluxo de Informação

## 11.1 Origem das Informações

## 11.2 Caminho das Instruções

## 11.3 Acesso e Circulação de Dados
Na organização Harvard clássica, as instruções do programa e os dados ocupam espaços de memória separados e utilizam caminhos independentes até o processador. Para uma operação aritmética, os valores podem ser lidos de registradores e encaminhados à ULA, que produz um resultado. Quando a instrução precisa buscar um valor armazenado, o processador calcula o endereço e solicita a leitura à memória de dados; o valor retornado pode ser encaminhado a um registrador. Em uma operação de armazenamento, um valor mantido em registrador é enviado ao endereço indicado na memória de dados. Assim, o fluxo depende da instrução executada: uma operação entre registradores pode não acessar a memória de dados, enquanto uma carga ou um armazenamento envolve esse caminho.  

A separação entre os caminhos de instruções e dados permite que seus acessos sejam independentes. Ela pode possibilitar que uma instrução seja buscada enquanto ocorre um acesso a dados, mas a simultaneidade depende da implementação. O diagrama desta seção representa os caminhos de forma geral; ele não especifica tecnologias, larguras ou barramentos que sejam obrigatórios em toda arquitetura Harvard.

**[Figura 1 — Fluxo de dados na organização Harvard clássica](images/harvard-classica-fluxo-informacao.svg)**

*Legenda: As setas indicam o sentido do fluxo. A separação permite acessos independentes; a simultaneidade depende da implementação. Fonte: elaboração própria, com base em Arvind et al. (2015).*

## 11.4 Resultados Intermediários

## 11.5 Ordem e Independência dos Acessos

## 11.6 Armazenamento, Aceleração e Limitações