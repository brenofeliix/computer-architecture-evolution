# Issue 10 — Representação de Operações e Acesso aos Dados

## 1. Introdução e Modelo de Acesso (Load/Store)
Na arquitetura RISC-V (RV32I), a manipulação de dados segue rigorosamente o modelo **Load/Store** (ou registrador-registrador). Isso significa que:
- Nenhuma instrução aritmética ou lógica acessa a memória RAM diretamente.
- Os dados devem obrigatoriamente ser trazidos da memória para os registradores internos da CPU através de instruções de carga (`load`).
- As modificações ocorrem exclusivamente entre registradores e na Unidade Lógica e Aritmética (ULA).
- Os resultados calculados são enviados de volta à memória por meio de instruções de armazenamento (`store`).

## 2. Como os Operandos são Fornecidos
Os operandos que participam das operações de cálculo são fornecidos de três maneiras principais:
1. **Registradores de Uso Geral:** A maioria das instruções especifica diretamente os registradores de origem (`rs1`, `rs2`) e de destino (`rd`) dentro do banco de 32 registradores (de `x0` a `x31`).
2. **Valores Imediatos (Immediates):** Constantes numéricas embutidas diretamente no formato da própria instrução de 32 bits (utilizado em instruções do tipo I, S e B), evitando a necessidade de buscar valores adicionais em outro lugar da memória.
3. **Registrador Fixo (Hardwired):** O registrador `x0` sempre retorna o valor `0` e qualquer escrita nele é descartada, servindo como operando auxiliar para operações como atribuições e limpezas de registradores.

## 3. Modos de Endereçamento e Localização de Dados Armazenados
O RISC-V adota um conjunto reduzido e eficiente de modos de endereçamento para localizar dados na memória:
- **Endereçamento Base + Deslocamento (Offset):** É o único modo utilizado pelas instruções de acesso à memória (`lw`, `sw`, etc.). O endereço efetivo do dado na RAM é calculado somando o valor contido em um registrador base (`rs1`) a um valor de deslocamento imediato de 12 bits fornecido na instrução.
- **Endereçamento por Registrador:** Utilizado em operações puramente internas da ULA (ex: `add rd, rs1, rs2`), onde os dados já estão prontos nos registradores.
- **Endereçamento Relativo ao PC (Program Counter):** Utilizado em saltos condicionais e desvios, onde o endereço de destino é calculado somando o valor atual do `PC` a um deslocamento imediato.

## 4. Exemplo Concreto de Seleção e Utilização de Dados
Para ilustrar o acesso aos dados em uma operação típica, considere o trecho de código em assembly logo abaixo:

```assembly
lw t1, 0(t0)   # Carrega o dado da memória apontada por (t0 + 0) para o registrador t1
addi t1, t1, 5 # Soma o valor imediato 5 ao operando armazenado no registrador t1
sw t1, 4(t0)   # Salva o resultado final de volta na memória, no endereço (t0 + 4)
