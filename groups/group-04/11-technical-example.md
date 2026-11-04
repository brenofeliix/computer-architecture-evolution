# Issue 11 — Exemplo Técnico de Funcionamento

## 1. Introdução e Tarefa Representativa
Para demonstrar o funcionamento prático da arquitetura **RISC-V (RV32I)**, selecionamos uma operação aritmética iterativa representativa: a **soma acumulada dos N primeiros números inteiros** (somatório sequencial com controle de fluxo, uso de registradores e acesso à memória RAM).

## 2. Passo a Passo do Funcionamento Técnico
O ciclo de execução e o comportamento da arquitetura nesta tarefa ocorrem através das seguintes etapas:

- **Qual é a entrada:** O limite numérico `N` armazenado inicialmente na memória RAM (apontado pelo registrador base `t0`) e o conjunto de instruções binárias em formato RV32I carregadas na memória de programa.
- **Como o sistema é configurado ou programado:** O programa utiliza registradores específicos de uso geral para gerenciar o estado da execução:
  - `t0`: Armazena o endereço base de referência na RAM.
  - `t1`: Recebe o valor limite `N` carregado da memória.
  - `t2`: Atua como o registrador acumulador (iniciado com `0`).
  - `t3`: Atua como o contador iterativo de loop (iniciado com `1`).
- **Quais componentes participam:** O *Program Counter (PC)* direciona a busca; a *Unidade de Controle (UC)* decodifica as instruções de 32 bits; o *Banco de Registradores* mantém o estado dinâmico; a *Unidade Lógica e Aritmética (ULA)* executa os cálculos e comparações; e os barramentos com a *Memória Principal* realizam as operações de carga e armazenamento.
- **Como as informações circulam:** O *PC* busca a instrução na memória; a *UC* decodifica o *opcode*; a *ULA* processa os dados diretamente entre os registradores internos; e, quando necessário, as instruções de `load`/`store` interagem com o barramento externo para ler ou gravar na RAM.
- **Quais operações são realizadas:** O fluxo executa leituras de dados (`lw`), operações aritméticas de incremento e soma (`addi`, `add`), e saltos condicionais baseados em comparação (`bgt` e `j`).
- **Como o resultado é produzido:** Após acumular ciclicamente os valores na ULA, o desvio condicional encerra o loop ao atingir o limite `N`, e a instrução final de escrita (`sw`) grava o resultado consolidado do acumulador de volta na posição de saída da RAM.

## 3. Código Assembly Representativo (RV32I)
```assembly
# Exemplo: Soma acumulada dos N primeiros números no RISC-V

_start:
    lw   t1, 0(t0)      # Carrega o limite N da memória RAM para o registrador t1
    li   t2, 0          # Inicializa o acumulador t2 = 0
    li   t3, 1          # Inicializa o contador iterativo t3 = 1

loop:
    bgt  t3, t1, fim    # Desvio condicional: se t3 > t1, salta para o rótulo fim
    add  t2, t2, t3     # Executa na ULA: t2 = t2 + t3 (soma acumulada)
    addi t3, t3, 1      # Incrementa o contador: t3 = t3 + 1
    j    loop           # Salto incondicional de volta ao início do loop

fim:
    sw   t2, 4(t0)      # Grava o resultado final acumulado de volta na memória RAM em (t0 + 4)
    
