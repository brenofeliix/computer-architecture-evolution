QP2 - Como a arquitetura CISC organiza e acessa a memória?

A arquitetura CISC (Complex Instruction Set Computer) organiza e acessa a memória principal de maneira a maximizar a flexibilidade e otimizar o espaço do código.

**Modelo de Organização Linear e Unificada**: A memória principal é tratada como uma sequência linear e contínua de bytes endereçáveis, onde dados e instruções compartilham o mesmo espaço físico de endereçamento.

**Modos de Endereçamento Complexos**: Oferece grande variedade de formas para localizar dados (direto, indireto, indexado e base-índice), facilitando o acesso eficiente a estruturas complexas e vetores (arrays) na memória. Por exemplo, o modo indexado permite acessar diferentes posições de um vetor usando um índice, tornando a localização dos dados mais prática.

**Operações Register-to-Memory**: Diferente de arquiteturas mais simples, o CISC permite que uma única instrução busque dados diretamente da memória, execute o processamento aritmético/lógico e grave o resultado de volta na memória, sem exigir comandos intermediários de leitura e escrita.

**Suporte Nativo a Pilha e Organização de Bytes**: Conta com instruções dedicadas no hardware (como PUSH e POP) para gerenciar sub-rotinas e alocação automática na pilha, utilizando habitualmente o formato de armazenamento Little Endian para os bytes.

Fontes: Livro: Arquitetura e Organização de Computadores: Projetando para o Desempenho — William Stallings e  Instituto de Matemática e Estatística da USP (IME-USP) Arquitetura de Computadores - https://eaulas.usp.br/portal/course.action?course=33377
