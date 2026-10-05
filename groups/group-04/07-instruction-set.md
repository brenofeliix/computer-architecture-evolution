# 7. Instruction Set

## 7.1 ISA Overview
O conjunto de instruções da RISC-V foi estruturado sob o conceito de ISA base congelada + extensões modulares.
Ao contrário de arquiteturas cumulativas que acumulam milhares de instruções com o passar das décadas, a RISC-V define núcleos base enxutos e imutáveis:
* RV32I / RV64I: A base inteira padrão (com menos de 50 instruções), contendo apenas as operações computacionais, lógicas, saltos e transferências essenciais para compilar e executar programas completos em C;
* RV32E: Versão compacta com apenas 16 registradores, voltada para microcontroladores ultraconservadores em área de silício.

A arquitetura adota a filosofia clássica RISC pura e Load/Store: nenhuma instrução matemática ou lógica lê ou escreve diretamente na memória RAM; todas as operações ocorrem estritamente entre registradores do processador.

## 7.2 Instruction Types
No conjunto base RV32I, todas as instruções possuem comprimento fixo de 32 bits e são alinhadas em múltiplos de 4 bytes na memória. Existem 6 formatos básicos de instrução, projetados para manter os identificadores dos registradores-fonte (rs1, rs2) e registrador-destino (rd) exatamente nas mesmas posições de bits:

* Tipo R (Register): Operações aritméticas e lógicas puras entre registradores (ex.: add, sub, and, or, sll);
* Tipo I (Immediate): Operações com constantes imediatas de 12 bits, cargas de memória e saltos indiretos (ex.: addi, lw, jalr);
* Tipo S (Store): Instruções de gravação de dados na memória. Não geram valor de retorno para registrador (rd), dividindo o valor imediato em dois pedaços de bits para preservar o alinhamento de decodificação (ex.: sw, sb, sh);
* Tipo B (Branch): Desvios condicionais com deslocamento de endereço imediato relativo ao PC (ex.: beq, bne, blt, bge);
* Tipo U (Upper Immediate): Carrega valores imediatos de 20 bits na parte alta do registrador (ex.: lui, auipc);
* Tipo J (Jump): Saltos incondicionais com longo alcance relativo ao PC (ex.: jal).

## 7.3 Addressing Modes
Diferente de arquiteturas complexas (CISC) que oferecem dezenas de modos de endereçamento em hardware, a base RISC-V reduz os modos aos mecanismos estritamente essenciais, transferindo combinações complexas para o compilador:

1. Modo Registrador (Register Direct): Os operandos estão situados diretamente dentro de registradores (ex.: add rd, rs1, rs2);
2. Modo Imediato (Immediate): O operando é uma constante codificada dentro da própria palavra de instrução (ex.: addi rd, rs1, imm);
3. Modo Base + Deslocamento (Base Register + Displacement): Único modo de endereçamento para acesso à memória de dados (load e store). O endereço efetivo é formado pela soma do conteúdo de um registrador-base com um imediato com sinal de 12 bits: $\text{Endereço} = \text{Reg}[rs1] + \text{imm}$ (ex.: lw rd, offset(rs1));
4. Modo Relativo ao Program Counter (PC-Relative): Utilizado em instruções de salto condicional (Tipo B) e incondicional (Tipo J). O endereço de destino é calculado somando um valor imediato ao PC atual: $\text{Destino} = \text{PC} + \text{imm}$;
5. Modo Indireto por Registrador (Register Indirect): Utilizado na instrução jalr, permitindo saltos absolutos ou retornos de procedimentos para qualquer endereço contido em registrador somado a um offset: $\text{Destino} = \text{Reg}[rs1] + \text{imm}$.

## 7.4 Assembly Example
Abaixo, um programa Assembly RV32I completo e funcional que soma os primeiros $N$ números inteiros (ou seja, $\sum_{i=1}^{N} i$) e armazena o resultado em memória:

    # Programa: Soma dos primeiros N números inteiros (N = 5)
    # Resultado esperado: 1 + 2 + 3 + 4 + 5 = 15 (0x0F)

    .section .data
    n_val:      .word 5          # Valor de entrada N
    resultado:  .word 0          # Local reservado para a soma final
    
    .section .text
    .globl _start
    
    _start:
        # 1. Carrega ponteiros e variáveis
        la   t0, n_val           # t0 = endereço de n_val
        lw   t1, 0(t0)           # t1 = lê o valor N (5)
        
        li   t2, 0               # t2 = acumulador (soma = 0)
        li   t3, 1               # t3 = contador atual (i = 1)
    
    loop_soma:
        bgt  t3, t1, fim_loop    # Se i > N, sai do laço
        add  t2, t2, t3          # soma = soma + i
        addi t3, t3, 1           # i = i + 1
        j    loop_soma           # Volta para a próxima iteração
    
    fim_loop:
        # 2. Grava o resultado na memória
        la   t4, resultado       # t4 = endereço de 'resultado'
        sw   t2, 0(t4)           # Escreve a soma final na memória
    
    trava:
        j    trava               # Laço infinito para encerrar execução

## 7.5 Execution Example
Acompanhamento passo a passo do fluxo de execução do trecho do laço (loop_soma) da seção anterior:

1. Estado Inicial:
   * Registrador t1 contém 5 (limite $N$), acumulador t2 contém 0 e registrador t3 contém 1 ($i$ inicial);

3. Iteração 1:
   * bgt t3, t1, fim_loop: Testa se $1 > 5$ (Falso). O fluxo continua sequencialmente;
   * add t2, t2, t3: Soma acumulada passa para $0 + 1 = 1$ (t2 = 1);
   * addi t3, t3, 1: Incrementa o contador para $1 + 1 = 2$ (t3 = 2);
   * j loop_soma: O PC é recarregado com o endereço do rótulo loop_soma;

3. Iterações 2 a 5: O processo se repete sequencialmente, acumulando os valores parciais:
   * Ao fim de $i = 2$: t2 = 3, t3 = 3;
   * Ao fim de $i = 3$: t2 = 6, t3 = 4;
   * Ao fim de $i = 4$: t2 = 10, t3 = 5;
   * Ao fim de $i = 5$: t2 = 15, t3 = 6;

4. Finalização e Desvio:
   * No início da 6ª iteração, a instrução bgt t3, t1, fim_loop avalia $6 > 5$ (Verdadeiro);
   * O hardware calcula o endereço relativo do rótulo fim_loop e desvia o PC;
   * sw t2, 0(t4): O valor 15 (0x0000000F) presente em t2 é gravado no endereço físico mapeado da variável resultado.
A comparação direta do conjunto de instruções da RISC-V com arquiteturas comerciais consolidadas (x86-64 e ARM) evidencia as diferenças filosóficas de design:
## 7.6 Modern Architectures

| Característica | RISC-V (RV32I/RV64I) |	x86-64	| ARM (AArch64 / ARMv8-A) |
|---------|--------------|--------|--------|
| Filosofia de Projeto | RISC pura, limpa e modular | CISC (com micro-operações internas) | RISC moderna de alto desempenho |
| Comprimento da Instrução | Fixo (32 bits na base; 16 bits na extensão C) | Extremamente variável (1 a 15 bytes) | Fixo (32 bits; instruções Thumb legadas em 32-bit ARM) |
| Modelo de Acesso à Memória | Estritamente Load/Store | Acesso direto via operações da ALU (ex.: ADD [mem], reg) | Estritamente Load/Store |
| Complexidade de Decodificação | Muito baixa (campos de registradores em posições fixas) | Altíssima (exige múltiplos estágios de decodificadores complexos) | Baixa a moderada |
| Evolução da ISA | Base imutável; expansão modular via extensões padronizadas | Acumulativa (dezenas de extensões e legados herdados do 8086) | Por versões fechadas da arquitetura (ARMv7, ARMv8, ARMv9) |
| Controle de Condição | Branches comparam registradores diretamente (beq rs1, rs2); sem bits globais de flag | Depende de registrador central de flags (EFLAGS) | Suporte a registrador de condição (PSTATE/NZCV) |
| Modelo de Licenciamento | Especificação aberta e livre de royalties | Licença cruzada estrita e proprietária (Intel/AMD) | Licenciamento comercial de IP Core fechado (ARM Ltd.) |
