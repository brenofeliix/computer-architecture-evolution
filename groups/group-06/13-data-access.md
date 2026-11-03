# 13. Seleção e Acesso aos Dados

## 13.1 Escopo e Exemplo Adotado
Um operando é um valor, registrador ou referência a uma posição de memória utilizado por uma instrução durante sua execução. No ATmega328P, os operandos podem ser fornecidos diretamente na instrução, obtidos dos registradores de uso geral ou localizados na memória de dados por meio de endereços. Por exemplo, na instrução ADD R16, R17, os registradores R16 e R17 identificam os valores que serão somados, e o resultado é armazenado em R16. Quando os dados estão na memória, instruções específicas permitem acessá-los por endereçamento direto ou indireto, utilizando registradores como ponteiros para localizar suas posições. Também existem mecanismos para acessar posições específicas da memória de programa, como a Flash, por meio de instruções próprias. Esses recursos mostram como o processador identifica, seleciona e recebe os dados necessários para realizar uma operação. Entretanto, os mecanismos apresentados são características da implementação AVR do ATmega328P, e não regras obrigatórias de toda arquitetura Harvard. O princípio geral de Harvard está na separação entre memória de instruções e memória de dados, enquanto a quantidade de registradores, os modos de endereçamento, as instruções e as formas de acesso dependem de cada processador.

## 13.2 Localização dos Dados e dos Operandos

## 13.3 Identificação e Seleção dos Operandos
No ATmega328P, a instrução indica seus operandos conforme formas definidas pela ISA AVR. Em ADD R16, R17, os valores são identificados pelos registradores especificados na instrução. Em LDI R16, K, K representa uma constante imediata fornecida como operando da instrução. Já em LDS R16, k, k identifica uma posição do espaço de dados cujo conteúdo é carregado em R16. Uma alternativa é o acesso indireto, como em LD R16, X, no qual o endereço do dado é indicado pelo conteúdo do registrador ponteiro X [1], [2].

Esses exemplos mostram maneiras distintas de selecionar operandos nesta ISA: por registrador, por constante e por posição de memória. O endereço identifica onde buscar um valor; ele não deve ser confundido com o próprio valor. Também é necessário distinguir essas formas de endereçamento da organização Harvard: a separação entre os espaços de instruções e dados é uma característica organizacional, enquanto os mnemônicos e operandos apresentados pertencem ao ATmega328P/AVR [1], [2].

## 13.4 Modos de Endereçamento e Mecanismos de Acesso
O microcontrolador ATmega328P utiliza uma arquitetura do tipo Load/Store, separando rigidamente as operações internas do banco de registradores das transferências com a memória de dados SRAM. Enquanto cópias puras entre registradores (MOV) ou carregamentos imediatos (LDI) ocorrem instantaneamente em apenas um ciclo de clock via barramento interno, operações com a memória de dados (LDS, STS, LD, ST) exigem o endereçamento direto ou indireto por ponteiros ($X, Y, Z$) e demandam dois ciclos de clock.Além disso, a arquitetura oferece mecanismos especializados e eficientes para o hardware periférico, como o endereçamento de E/S direto (IN, OUT) para os primeiros 64 registradores, e o endereçamento indireto com deslocamento (LDD, STD) usando ponteiros somados a um offset. Essa diversidade de modos de endereçamento no manual AVR permite otimizar tanto o tempo de execução quanto o consumo de espaço no código, determinando com precisão como cada operando é localizado e transferido.

## 13.5 Exemplo de Seleção e Uso de um Dado
Para ilustrar de forma concreta como um dado é selecionado e utilizado na arquitetura do microcontrolador ATmega328P, considera-se a operação de carregamento e manipulação de um valor utilizando uma instrução real documentada na ISA AVR:

* **Instrução de Seleção:** `LDS Rd, k` (Load Direct from Data Space), que seleciona e carrega um valor diretamente de um endereço fixo da memória de dados (SRAM) para um registador de trabalho do processador[cite: 35].

| Etapa | Instrução / Operação | Origem do Dado | Destino do Dado / Uso Posterior |
| :--- | :--- | :--- | :--- |
| **1. Seleção e Leitura** | `LDS R16, 0x0100`[cite: 35] | Endereço `0x0100` da memória de dados (SRAM) do ATmega328P. | O valor armazenado nesse endereço é lido e copiado para o registador de destino `R16`[cite: 35]. |
| **2. Utilização Prática** | `ADD R17, R16` | O operando está agora guardado no registador interno `R16`[cite: 35]. | O valor é somado ao conteúdo do registador `R17`, atualizando o resultado e as flags de estado. |

Este fluxo demonstra que, embora a organização Harvard garanta a separação física entre os caminhos de programa e de dados, a seleção e o endereçamento de um operando dentro da memória de dados seguem as regras específicas da arquitetura interna do microcontrolador escolhido[cite: 32, 35].
## 13.6 Limitações e Relação com a Organização Harvard

O princípio geral da arquitetura de Harvard reflete-se no ATmega328P através da separação física entre as memórias de instruções e de dados. Contudo, os mecanismos de acesso aos operandos, a arquitetura Load/Store e os modos de endereçamento são exclusivos desta implementação AVR e não representam regras universais teóricas. Como limitação técnica documentada, as operações na memória externa de dados exigem dois ciclos de relógio, criando um estrangulamento de desempenho face às operações executadas num único ciclo nos registadores internos.   
