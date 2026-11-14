# 7. Instruction Set

## 7.1 ISA Overview
O Colossus não tinha um ISA (Instruction Set Architecture) no sentido moderno. Ele não possuía um conjunto de instruções de máquina gravadas na memória para o processador executar passo a passo.
Ele era uma máquina de propósito específico (projetada exclusivamente para criptoanálise da cifra Lorenz), e não um computador de propósito geral.

## 7.2 Instruction Types
Não existiam instruções tradicionais como LOAD, STORE, ADD ou JUMP.
As "instruções" eram operações lógicas booleanas implementadas diretamente através do comportamento físico de milhares de válvulas termiônicas (tubos de vácuo).

## 7.3 Addressing Modes
Os computadores modernos usam modos de endereçamento (imediato, direto, registrador) para buscar dados na RAM. O Colossus lia os dados de entrada diretamente de uma fita de papel perfurada que rodava a alta velocidade através de um leitor ótico, comparando os fluxos de dados de forma sequencial.

## 7.4 Assembly Example
Como era "programado": Não existia linguagem Assembly. Para "programar" o Colossus para uma tarefa diferente, os operadores (frequentemente mulheres do WRNS - Women's Royal Naval Service) precisavam alterar fisicamente cabos de conexão (patch cables), chaves e interruptores na máquina, além de configurar os rotores de simulação.
A execução não lia instruções da memória, mas sim processava os caracteres da fita de papel perfurada combinando-os com as engrenagens lógicas das válvulas para encontrar padrões estatísticos que quebravam a criptografia.

## 7.5 Execution Example
LW  R1, num1      # Carrega o valor de num1 no registrador R1
    LW  R2, num2      # Carrega o valor de num2 no registrador R2
    ADD R3, R1, R2    # Soma R1 e R2, e guarda o resultado em R3
    SW  R3, resultado # Salva o valor de R3 de volta na memória
    
Explain step by step.
Passo a Passo Detalhado na CPUPasso 1: LW R1, num1 (Carregar Palavra)O que a CPU faz:Busca (Fetch): O processador lê a instrução na memória de programa.Decodificação: A unidade de controle entende que é um comando de leitura (Load Word).Acesso à Memória: A CPU busca no endereço de memória num1 o valor lá guardado (10).Resultado: O registrador R1 passa a conter o valor 10.Passo 2: LW R2, num2 (Carregar Palavra)O que a CPU faz:A CPU lê a próxima instrução.Acessa o endereço de memória num2 e busca o valor lá guardado (20).Resultado: O registrador R2 passa a conter o valor 20.Passo 3: ADD R3, R1, R2 (Somar Registradores)O que a CPU faz:A unidade de controle envia os dados dos registradores R1 (10) e R2 (20) para a ULA (Unidade Lógica e Aritmética).A ULA executa a operação matemática: $10 + 20 = 30$.O valor resultante é gravado no registrador de destino R3.Resultado: O registrador R3 passa a conter o valor 30.Passo 4: SW R3, resultado (Salvar Palavra)O que a CPU faz:O processador pega o valor que está dentro de R3 (30).Escreve esse valor de volta na memória RAM, na posição reservada com o nome resultado.Resultado: A variável resultado na memória principal passa a armazenar definitivamente o valor 30.

## 7.6 Modern Architectures
Enquanto o Colossus representava a pré-história da computação digital com hardware dedicado, as arquiteturas modernas (x86 e ARM) definem o padrão atual de computadores de propósito geral baseados no conceito de programa armazenado (Arquitetura de Von Neumann).

Compare with ARM or x86.
x86 (CISC - Intel/AMD):
Conceito de ISA: Conjunto de instruções complexo (CISC).
Programação: Software gravado em memória RAM e executado pela CPU.
Flexibilidade: Propósito geral, com foco em alto desempenho bruto (desktops, servidores, notebooks).
Tecnologia: Microprocessadores de silício de alta performance.
ARM (RISC - Apple/Qualcomm):
Conceito de ISA: Conjunto de instruções reduzido (RISC).
Programação: Software gravado em memória RAM com instruções de tamanho fixo.
Flexibilidade: Propósito geral, com foco em alta eficiência energética (smartphones, tablets, chips Apple M-series).
Tecnologia: Microprocessadores de silício otimizados para baixo consumo de bateria.
