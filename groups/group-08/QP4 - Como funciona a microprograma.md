QP4 - Como funciona a microprogramação do CISC?

*Microprogramação*: É uma técnica de projeto da unidade de controle em que os sinais de controle não são gerados por circuitos lógicos fixos (como na abordagem *hardwired*), mas lidos de uma memória interna ao processador. Cada instrução de máquina é interpretada como uma pequena sequência de passos internos, armazenada nessa memória. A ideia foi proposta por Maurice Wilkes no início da década de 1950. Por facilitar a implementação de instruções complexas, é a técnica dominante em arquiteturas CISC.

*Microinstruções e Microprograma*: Uma microinstrução descreve o conjunto de micro-operações executadas ao mesmo tempo (por exemplo, transferir um registrador para a ULA ou ler a memória). Cada bit da microinstrução costuma corresponder a uma linha de controle, ligada ou desligada. Uma sequência de microinstruções forma o microprograma, também chamado de *firmware*. Cada instrução de máquina (como ADD ou MOV) possui o seu próprio microprograma.

*Memória de Controle*: É uma memória rápida (normalmente ROM) dentro da CPU que guarda todos os microprogramas. Os sinais de controle são obtidos lendo essas microinstruções.

*Funcionamento (passo a passo)*: (1) a instrução é buscada na memória e colocada no registrador de instrução (IR); (2) o opcode é traduzido no endereço inicial do seu microprograma na memória de controle; (3) o sequenciador (microprogram counter) lê as microinstruções em ordem; (4) cada microinstrução ativa os sinais de controle que movem dados pelo caminho de dados (registradores, ULA, barramentos); (5) ao fim do microprograma, o controle volta a buscar a próxima instrução. Microinstruções podem conter campos de condição e de endereço para desvios dentro do próprio microprograma.

*Tipos de Microinstrução*: A *horizontal* tem um bit por linha de controle, o que permite grande paralelismo e execução rápida, mas gera palavras largas. A *vertical* usa campos codificados, que são mais curtos, mas precisam de decodificação e são um pouco mais lentos.

*Vantagens e Desvantagens*: A microprogramação é mais barata e menos sujeita a erros de implementação, além de permitir corrigir ou alterar instruções modificando o microcódigo. Em compensação, é mais lenta que uma unidade *hardwired* de tecnologia comparável. Por isso, processadores RISC costumam usar controle *hardwired*. Já os CISC modernos, como o x86, decodificam as instruções simples diretamente e usam microcódigo apenas para as mais complexas.

Fontes:
- Notas de aula COA, Unidade 5: Microprogrammed Control Unit (NIT Srinagar) - https://nitsri.ac.in/Department/Electronics%20&%20Communication%20Engineering/COA-_Unit_5.pdf
- Microprogrammed Control Unit in Computer Organization - https://ccbp.in/blog/articles/microprogrammed-control-unit-in-computer-organization
- Slides de Arquitetura de Computadores (DCC-UFMG), sobre microprograma e CISC x RISC - https://homepages.dcc.ufmg.br/~jussara/swb/slides/cap2.pdf
- Livro: Arquitetura e Organização de Computadores: Projetando para o Desempenho (Stallings), capítulo sobre controle microprogramado
