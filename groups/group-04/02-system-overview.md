# 2. System Overview

## 2.1 General Description
A arquitetura RISC-V é organizada de forma modular: um núcleo (core) processador segue uma base de instruções fixa e enxuta (RV32I/RV64I), responsável por buscar, decodificar e executar instruções, enquanto se comunica com a memória e periféricos por meio de um barramento de interconexão. Extensões opcionais (M, F/D, C, V) podem ser adicionadas ao núcleo base para ampliar suas capacidades sem alterar a estrutura fundamental.

## 2.2 Main Components
- CPU: É chamado de **HART** (hardware thread) como a unidade que executa o ISA, os 32 registradores de propósito geral (x0 fixo em zero), os níveis de privilégio (Usuário/Supervisor/Máquina) e os registradores de controle CSR.
- Memory: O RISC-V define oficialmente, em sua especificação privilegiada, o modelo de ordenação de memória RVWMO (RISC-V Weak Memory Ordering), responsável por reger como operações de leitura e escrita se comportam entre múltiplos núcleos, além dos esquemas de memória virtual e paginação (Sv32, Sv39, Sv48), utilizados para isolamento e proteção de processos. Já a hierarquia física de cache (L1/L2) não é parte da especificação, fica a critério de cada implementação.
- Interconexão: O RISC-V não define um protocolo de barramento específico, isso fica a critério de cada fabricante, que pode adotar padrões como AXI, TileLink ou Wishbone. Uma plataforma RISC-V pode reunir múltiplos núcleos, aceleradores e memórias interligados por essa estrutura, mas a forma como eles se comunicam não faz parte do padrão da ISA.
I/O: O RISC-V não possui instruções dedicadas de entrada e saída, como o INPUT/OUTPUT de arquiteturas mais antigas (ex: x86). Em vez disso, adota o modelo de MMIO (Memory-Mapped I/O), tratando periféricos como endereços comuns de memória, acessados pelas próprias instruções de leitura e escrita (load/store). Essa é a prática padrão definida nas especificações de plataforma do RISC-V — ou seja, o I/O existe conceitualmente, mas reaproveita a lógica de memória em vez de constituir um bloco isolado.

## 2.3 Block Diagram

<img width="1536" height="1024" alt="ChatGPT Image 18 de set  de 2026, 22_30_31" src="https://github.com/user-attachments/assets/7d5630ea-e9bc-4435-9a27-666ab09a5287" />

## 2.4 Data Flow
O fluxo de dados no RISC-V segue o ciclo lógico definido pela própria especificação do ISA, a instrução é buscada na memória, decodificada para identificar a operação, executada na unidade lógica-aritmética, acessa a memória quando necessário (leitura ou escrita) e por fim tem seu resultado escrito de volta em um registrador. Esse ciclo busca, decodificação, execução, acesso à memória e escrita refletindo o comportamento que a arquitetura define como cada instrução deve se comportar. A divisão física desse fluxo em um pipeline de exatamente 5 estágios, no entanto, é uma forma clássica de implementação  (usada mais para fins didáticos), e não uma exigência da especificação em si.

## 2.5 Main Characteristics
| Feature | Description | Origem |
|---------|--------------|--------|
| Tipo de ISA | RISC (Reduced Instruction Set Computer), aberto e modular | Especificação |
| Tamanho de palavra | 32 bits (RV32I) e 64 bits (RV64I) | Especificação |
| Base de instruções | Menos de 50 instruções na base fixa (RV32I/RV64I) | Especificação |
| Extensões | M (mult/div), F/D (ponto flutuante), C (comprimidas), V (vetorial) | Especificação |
| Registradores | 32 registradores de propósito geral (x0 fixo em zero) | Especificação |
| Modelo de memória | RVWMO e esquemas de paginação (Sv32, Sv39, Sv48) | Especificação |
| Segurança | Níveis de privilégio (Usuário, Supervisor, Máquina) via CSR | Especificação |
| Entrada/Saída | Memory-Mapped I/O (MMIO) | Convenção de plataforma |
| Pipeline | 5 estágios (Busca, Decodificação, Execução, Memória, Escrita) | Implementação |
| Cache | Hierarquia L1/L2 | Implementação |
| Interconexão | Barramento (AXI, TileLink, Wishbone, etc.) | Implementação |
| Licenciamento | Aberto, livre e sem royalties | Especificação |
