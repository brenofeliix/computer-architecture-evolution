# 7. Modelo de Operações e Instruções

## 7.1 Harvard e ISA
A Arquitetura de Harvard é uma forma de organização de computadores que se caracteriza pela separação entre a memória de instruções e a memória de dados, permitindo que cada uma possua caminhos de acesso independentes. A memória de instruções armazena os comandos que determinam as operações que o processador deve executar, enquanto a memória de dados guarda as informações utilizadas durante o processamento, como variáveis e resultados intermediários. Essa separação possibilita acessos independentes e pode permitir que uma instrução seja buscada enquanto dados são lidos ou escritos, dependendo da implementação. Entretanto, a Arquitetura de Harvard não define quais instruções o processador reconhece, seus códigos binários ou o conjunto de registradores disponíveis. Essas características são estabelecidas pela Arquitetura do Conjunto de Instruções, conhecida como ISA (Instruction Set Architecture), que especifica as operações disponíveis, o formato das instruções, os registradores visíveis ao programador e as regras para execução de cada comando. Portanto, Harvard e ISA são conceitos diferentes e complementares: Harvard determina como as instruções e os dados são organizados e acessados, enquanto a ISA define a interface entre o software e o processador. Essa distinção pode ser observada no microcontrolador ATmega328P, que utiliza uma organização Harvard, com memórias e caminhos separados para programa e dados, e implementa o conjunto de instruções AVR de 8 bits.

## 7.2 Como o Programa Chega ao Microcontrolador
No ecossistema do Arduino com o microcontrolador ATmega328P, o caminho percorrido por um programa desde a sua elaboração até a execução física envolve processos fundamentais de tradução e carregamento.

O fluxo inicia-se no ambiente de desenvolvimento (Arduino IDE), onde o programa é escrito pelo usuário em uma linguagem de alto nível baseada em C/C++. Essa escrita é composta por estruturas lógicas compreensíveis por humanos, como as funções setup() e loop(). No entanto, como o processador do microcontrolador não é capaz de interpretar diretamente essa sintaxe, faz-se necessário o processo de compilação. Ao acionar a verificação na IDE, uma cadeia de ferramentas (toolchain) baseada no compilador avr-gcc entra em ação para traduzir todo o código de alto nível e suas bibliotecas associadas em um arquivo binário em formato hexadecimal (.hex).

É fundamental distinguir o código escrito pelo usuário das instruções de máquina efetivamente executadas pelo ATmega328P. O código-fonte representa a abstração de alto nível e a lógica desejada; já o arquivo gerado após a compilação contém os códigos binários de operação (opcodes) específicos da arquitetura do processador. Essas instruções de máquina são decodificadas e processadas diretamente pela Unidade Lógica e Aritmética (ULA) e pelos registradores do microcontrolador. Cabe ressaltar que o código Arduino em C/C++ não deve ser confundido nem chamado de linguagem de máquina ou de Arquitetura de Conjunto de Instruções (ISA), visto que a ISA representa o conjunto abstrato de instruções suportadas pelo hardware, enquanto o código do usuário é a aplicação em linguagem de alto nível traduzida para esse conjunto.

Após a geração do binário, ocorre a etapa de carregamento (upload), responsável por transferir os dados do computador para a memória Flash não volátil do ATmega328P. Essa transferência é intermediada por uma conexão serial USB (convertida pela interface de hardware da placa) e viabilizada por um pequeno programa gravado previamente no microcontrolador, denominado bootloader (como o optiboot).

Durante o procedimento de upload, a IDE emite um sinal de reinicialização para o ATmega328P. Ao reiniciar, o bootloader entra em execução por um breve intervalo de tempo, detecta a comunicação vinda da porta serial e recebe os blocos de dados do arquivo .hex enviados pelo computador através de protocolos de comunicação dedicados, como o STK500. O bootloader grava essas instruções diretamente na memória Flash e, assim que o envio é concluído com sucesso, repassa o controle de execução para o novo programa carregado, permitindo que o microcontrolador passe a executá-lo de forma autônoma.
## 7.3 Operações e Tipos de Instrução
Para examinar operações reais, esta seção usa como exemplo o microcontrolador ATmega328P, presente na placa Arduino Uno Rev3. As instruções descritas a seguir pertencem à ISA AVR implementada por esse microcontrolador. Elas não são definidas pela Arquitetura Harvard, que descreve uma organização com espaços separados para instruções e dados [1]–[3].

Entre as operações aritméticas do AVR está ADD Rd, Rr. Essa instrução soma os valores dos registradores indicados e grava o resultado no registrador destino Rd. Assim, ela ilustra uma operação realizada pelo processador sobre operandos mantidos em registradores. O manual AVR também descreve os efeitos da operação sobre as flags de estado, que podem ser usadas por instruções posteriores [1].

A transferência de dados aparece em instruções com funções diferentes. MOV Rd, Rr copia um valor de um registrador para outro. Já LD e ST permitem, respectivamente, carregar um valor do espaço de dados para um registrador e armazenar no espaço de dados o valor de um registrador. Essas instruções não devem ser tratadas como equivalentes: MOV transfere entre registradores, enquanto LD e ST envolvem acesso ao espaço de dados.

O AVR também possui instruções de controle de fluxo, como BRNE. Essa instrução realiza um desvio condicional quando a flag Z está zerada. Como a condição depende de uma flag, normalmente uma instrução anterior — por exemplo, uma comparação — precisa ter produzido o estado que será testado. Portanto, BRNE não deve ser descrita como uma instrução que compara diretamente dois operandos.

A instrução LPM permite ler informação armazenada na memória de programa, conforme a forma de uso definida pela ISA AVR. Esse caso ajuda a relacionar o exemplo concreto à separação entre espaços de memória: a existência e a semântica de LPM, porém, são características do AVR estudado e não uma regra universal da Arquitetura Harvard.

As categorias apresentadas são uma seleção de exemplos da ISA AVR, não uma lista completa. A separação Harvard ajuda a descrever onde instruções e dados estão organizados; ela não determina, por si só, quais instruções aritméticas, de transferência ou de desvio um processador deve possuir.

## 7.4 Codificação, Operandos e Endereçamento
No ATmega328P, o modo de endereçamento é a forma como a instrução localiza seu operando (registrador, constante ou endereço de memória). Segundo o manual oficial, os principais modos são:

Registrador Direto: Usa diretamente os registradores de propósito geral (R0 a R31). Ex: ADD Rd, Rr.

Imediato (Constantes): Usa uma constante numérica de 8 bits embutida na instrução (apenas nos registradores R16 a R31). Ex: LDI Rd, K.

Dados Direto (SRAM): Acessa um endereço absoluto de 16 bits na memória RAM. Ex: LDS Rd, k.

Dados Indireto: Usa os pares de registradores X, Y ou Z como ponteiros para a SRAM (com suporte a pré-decremento ou pós-incremento). Ex: LD Rd, X+.

Indireto com Deslocamento: Usa os ponteiros Y ou Z somados a um deslocamento fixo (q). Ex: LDD Rd, Y+q.

E/S Direta (I/O): Acessa rapidamente os primeiros registradores de periféricos. Ex: IN Rd, A.

Memória de Programa (Flash): Lê constantes da memória Flash usando o ponteiro Z. Ex: LPM.

Tamanhos (Conforme o Manual)
1 palavra (16 bits): A maioria das 131 instruções do microcontrolador.

2 palavras (32 bits): Apenas instruções que exigem saltos ou endereços maiores (como JMP e CALL

## 7.5 Exemplo de Instrução ou Operação
Para ilustrar o modelo de operação e o conjunto de instruções na arquitetura AVR do microcontrolador ATmega328P, toma-se como exemplo prático uma operação aritmética simples de adição entre um registador de trabalho e uma constante imediata, documentada no manual oficial da arquitetura:

1. **Instrução Utilizada:** `LDI` (Load Immediate) seguida de `ADD` ou `SUBI`. Especificamente, a instrução **`LDI Rd, K`** (Load Immediate) carrega uma constante imediata $K$ num registador de uso geral ($Rd$, compreendido entre R16 e R31).
2. **Sequência de Execução da Operação de Soma:**
   * **Passo 1 (Carregamento de Valores):** A instrução `LDI R16, 0x05` busca o valor imediato `0x05` da memória de programa e armazena-o diretamente no registador interno `R16`.
   * **Passo 2 (Operação Aritmética):** Utiliza-se a instrução real **`ADD Rd, Rr`** (Add Registers) ou **`ADIW Rdl, K`** (Add Immediate to Word). Por exemplo, a instrução `ADD R16, R17` soma o conteúdo do registador `R17` ao registador `R16`.
3. **Utilização de Valores e Destino do Resultado:** 
   * Os operandos são obtidos diretamente a partir dos registadores internos de trabalho do processador.
   * O resultado da operação aritmética é gravado de forma imediata no registador de destino (`Rd`, neste caso, o `R16`), atualizando simultaneamente as flags de estado no registador de estatísticas (SREG) conforme o resultado obtido (como flags de zero, negativo ou *carry*).
## 7.6 Relação com o Hardware e Limitações

O modelo geral da Arquitetura de Harvard caracteriza-se pela separação física entre a memória de instruções e a memória de dados, dotando o sistema de caminhos de acesso independentes para cada uma. No ATmega328P, o que corresponde a essa ideia de Harvard é puramente a existência de espaços e barramentos separados: um voltado para o programa e outro voltado para os dados. 

Por outro lado, o comportamento específico e as regras de execução pertencem somente ao microcontrolador escolhido e à sua Arquitetura do Conjunto de Instruções (ISA). Detalhes de implementação como o uso da ISA AVR de 8 bits, os modos de endereçamento, a presença de registradores de propósito geral (R0 a R31) e o tamanho das instruções (maioria de 16 bits, com algumas de 32 bits para saltos) são exclusivos deste processador. A própria semântica das operações também é uma particularidade da ISA: por exemplo, a instrução `LPM` é usada para ler informações da memória de programa, o que ilustra a separação dos espaços de memória, mas sua existência não é uma regra universal da Arquitetura de Harvard, e sim uma característica de hardware do AVR estudado.

Ao avaliar os limites deste modelo de operações, é fundamental não afirmar que a arquitetura de Harvard garante, por si só, uma execução simultânea ou um desempenho superior. Embora a separação dos barramentos possibilite acessos independentes e possa permitir que a busca por uma instrução ocorra ao mesmo tempo em que os dados são lidos ou escritos, essa simultaneidade não é garantida pelo modelo teórico, dependendo estritamente da implementação específica do hardware. 

**Comparação com ARM e x86:**
Quando comparamos a organização do ATmega328P com processadores das famílias ARM ou x86, as distinções de hardware tornam-se evidentes:
* **x86:** Tradicionalmente baseia-se no modelo de Von Neumann (ou de memória unificada), onde tanto os dados quanto as instruções residem no mesmo espaço de memória principal e compartilham os mesmos barramentos lógicos gerais, embora internamente o processador possa utilizar caches separadas.
* **ARM:** Muitos microprocessadores e microcontroladores ARM modernos adotam uma "Arquitetura Harvard Modificada". Eles mantêm caminhos separados e simultâneos no nível mais próximo ao processador (caches L1 separadas para dados e instruções) visando contornar gargalos de desempenho, mas no nível da memória principal, o espaço de endereçamento frequentemente é unificado. 
* **ATmega328P (AVR):** Mantém a separação rígida do modelo Harvard em sua totalidade (memória Flash exclusiva para programa e SRAM exclusiva para dados), motivo pelo qual requer instruções específicas da sua ISA, como a `LPM`, para conseguir transitar informações lendo dados armazenados no espaço de instruções.
