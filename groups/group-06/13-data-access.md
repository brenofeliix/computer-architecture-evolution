# 13. Seleção e Acesso aos Dados

## 13.1 Escopo e exemplo adotado

Esta seção usa o ATmega328P como exemplo concreto de uma implementação AVR. No dispositivo, a memória de programa Flash e o espaço de dados são distintos. O princípio geral de Harvard ajuda a explicar essa separação; já os registradores, as instruções, os modos de endereçamento e os tempos descritos abaixo são características específicas do AVR e do ATmega328P (Atmel, 2015, seções 6 e 7; Microchip, s.d.).

## 13.2 Onde os operandos podem estar

O ATmega328P possui 32 registradores de uso geral de 8 bits, 2 KB de SRAM interna e 32 KB de Flash para programa. Os registradores de uso geral fazem parte do núcleo da CPU; a SRAM é acessada pelo espaço de dados. O mapa de dados também inclui os registradores de E/S: os endereços de E/S 0x00–0x3F correspondem aos endereços 0x20–0x5F do espaço de dados, e os registradores de E/S estendidos ocupam 0x60–0xFF. A SRAM interna começa em 0x0100 e vai até 0x08FF (Atmel, 2015, seções 6.2 e 6.3).

Um endereço identifica uma posição no espaço de dados; o valor é o conteúdo armazenado nessa posição. Essa distinção permite compreender instruções como `LDS R16, 0x0100`: `0x0100` é o endereço consultado e o byte lido é o valor carregado em `R16*.

## 13.3 Como a instrução identifica o operando

As formas de identificar operandos são definidas pela ISA AVR. Em `ADD R16, R17`, os operandos são os conteúdos dos registradores indicados. Em `LDI R16, K`, `K` é uma constante imediata. Em `LDS R16, k`, `k` é um endereço direto do espaço de dados, cujo conteúdo é carregado em `R16`. A instrução `LD R16, X` usa o endereço indicado pelo registrador ponteiro `X` (Microchip, s.d.).

Esses exemplos distinguem seleção por registrador, constante e endereço de memória. São formas da ISA AVR; não são requisitos universais de toda arquitetura Harvard.

## 13.4 Modos de acesso e custo em ciclos

O AVR disponibiliza instruções diferentes para movimentar dados. `LDS` e `STS` usam endereço direto no espaço de dados; `LD` e `ST` permitem acesso indireto por registradores ponteiro; `LDD` e `STD` acrescentam um deslocamento ao endereço indicado por `Y` ou `Z`. O conjunto também inclui instruções de E/S, como `IN` e `OUT` (Microchip, s.d.).

O número de ciclos depende da instrução e da variante do núcleo AVR. Para o ATmega328P, a tabela do datasheet especifica dois ciclos para `LDS` e `STS`, e um ciclo para operações como `ADD`, `MOV` e `LDI`. Esses valores descrevem esse dispositivo; não devem ser generalizados para todas as implementações Harvard (Atmel, 2015, seção 28).

## 13.5 Exemplo de seleção e uso de um dado

Considere que o endereço de SRAM `0x0100` contenha o valor 5 e que `R17` contenha 7:

| Etapa | Instrução AVR | Efeito |
| :--- | :--- | :--- |
| 1. Leitura | `LDS R16, 0x0100` | Carrega em `R16` o byte armazenado no endereço `0x0100` da SRAM. |
| 2. Operação | `ADD R17, R16` | Soma `R16` a `R17`; o resultado fica em `R17` e as flags pertinentes são atualizadas. |

Depois da sequência, `R17 = 12`. As instruções e os operandos são reais da ISA AVR; o exemplo ilustra o uso de um dado do espaço de dados, sem afirmar que todo processador Harvard utiliza os mesmos registradores ou instruções (Atmel, 2015, seção 28; Microchip, s.d.).

## 13.6 Limites do exemplo

O ATmega328P exemplifica uma implementação AVR com espaços distintos de programa e dados. A organização Harvard, por si só, não determina os modos de endereçamento, a quantidade de registradores, a largura das palavras, a latência ou a quantidade de memória. Esses detalhes precisam ser verificados na documentação do processador escolhido. O exemplo descreve a ISA e alguns recursos do ATmega328P, não uma microarquitetura universal (Atmel, 2015; Pawson, 2022).

