# 2. System Overview

## 2.1 General Description

A arquitetura CISC (Complex Instruction Set Computer) é um modelo de projeto de processador cujo objetivo central é executar tarefas complexas usando o menor número possível de instruções de assembly. Para isso, cada instrução é capaz de realizar múltiplas operações de baixo nível em um único comando — por exemplo, uma instrução pode buscar um dado na memória, executar uma operação aritmética sobre ele e armazenar o resultado de volta, tudo isso especificado por uma única linha de código de máquina.

Essa filosofia se opõe diretamente à arquitetura RISC (Reduced Instruction Set Computer), que prioriza instruções simples e de execução rápida, geralmente uma por ciclo de clock. No CISC, o processador é responsável por interpretar instruções de tamanho variável e complexidade elevada, o que exige uma Unidade de Controle (Control Unit) mais sofisticada, frequentemente implementada por meio de microcódigo (microprogramação) em vez de lógica cabeada (hardwired logic).

## 2.2 Main Components

- **CPU (Central Processing Unit):** núcleo de processamento, dividido essencialmente em Unidade de Controle (Control Unit) e Unidade Lógica e Aritmética (ALU).
  - **Control Unit (Unidade de Controle):** decodifica as instruções complexas e gera os sinais de controle necessários para coordenar os demais componentes. No CISC, geralmente é microprogramada — ou seja, cada instrução complexa é traduzida internamente em uma sequência de microinstruções mais simples.
  - **ALU (Arithmetic Logic Unit):** executa as operações aritméticas (soma, subtração, multiplicação) e lógicas (AND, OR, NOT, comparações) demandadas pelas instruções.
  - **Registradores (Registers):** pequenas unidades de armazenamento de alta velocidade dentro da CPU, usadas para guardar dados temporários, endereços e resultados intermediários. No CISC, o número de registradores de uso geral costuma ser menor do que no RISC, já que parte das operações trabalha diretamente com a memória.

- **Memory (Memória):** armazena tanto as instruções do programa quanto os dados manipulados por ele. Uma característica marcante do CISC é permitir instruções do tipo memória-para-memória (memory-to-memory), nas quais os operandos podem ser lidos e gravados diretamente na memória, sem passar obrigatoriamente por registradores.

- **Bus (Barramento):** conjunto de linhas físicas que interligam CPU, memória e dispositivos de I/O, permitindo o tráfego de dados, endereços e sinais de controle. Geralmente dividido em:
  - **Barramento de Dados:** transporta os dados propriamente ditos.
  - **Barramento de Endereços:** indica a posição de memória a ser acessada.
  - **Barramento de Controle:** carrega os sinais que coordenam leitura, escrita e temporização das operações.

- **I/O (Input/Output):** conjunto de interfaces e controladores responsáveis pela comunicação entre o processador e dispositivos externos (teclado, disco, rede, etc.), geralmente gerenciados por meio de instruções específicas de I/O ou de I/O mapeado em memória.

## 2.4 Data Flow

O fluxo de dados em uma arquitetura CISC segue, de forma geral, o seguinte ciclo:

1. **Busca (Fetch):** a Unidade de Controle busca a próxima instrução na memória, utilizando o endereço indicado pelo Program Counter (PC), e a carrega no Instruction Register (IR).
2. **Decodificação (Decode):** como as instruções CISC têm tamanho e formato variáveis (e podem envolver múltiplos operandos e modos de endereçamento complexos), a decodificação costuma ser mais elaborada do que no RISC. Nessa etapa, a Unidade de Controle traduz a instrução complexa em uma sequência de microinstruções (microcódigo) que descrevem passo a passo o que o hardware deve fazer.
3. **Busca de Operandos (Operand Fetch):** os dados necessários são buscados — podendo vir de registradores ou diretamente da memória, já que o CISC permite operações memória-para-memória.
4. **Execução (Execute):** a ALU realiza a operação aritmética ou lógica especificada, seguindo a sequência de microinstruções gerada na decodificação.
5. **Armazenamento do Resultado (Write-back/Store):** o resultado da operação é gravado de volta em um registrador ou diretamente em uma posição de memória, dependendo da instrução original.
6. **Atualização do PC:** o Program Counter é atualizado para apontar para a próxima instrução, e o ciclo recomeça.

Por permitir que uma única instrução complexa substitua várias instruções simples, o CISC reduz o número total de instruções buscadas na memória, mas cada instrução pode levar vários ciclos de clock para ser completamente executada, já que internamente ela é decomposta em múltiplos microcomandos.

## 2.5 Main Characteristics

| Feature | Description |
|---------|-------------|
| Tamanho das instruções | Variável (podem ocupar de 1 a vários bytes) |
| Número de instruções | Elevado (centenas de instruções distintas) |
| Ciclos por instrução | Múltiplos ciclos de clock por instrução |
| Modos de endereçamento | Grande variedade (direto, indireto, indexado, memória-memória, etc.) |
| Implementação da Unidade de Controle | Predominantemente microprogramada (microcódigo) |
| Acesso à memória | Instruções podem operar diretamente sobre a memória (memory-to-memory) |
| Número de registradores | Relativamente menor em comparação ao RISC |
| Complexidade do hardware | Alta (unidade de controle e decodificador mais complexos) |
| Complexidade do compilador | Menor, pois o hardware assume parte da complexidade |
| Uso de memória de programa | Otimizado/compacto (código ocupa menos espaço) |
| Exemplos de arquiteturas | x86 (Intel/AMD), VAX, System/360, Motorola 68k |