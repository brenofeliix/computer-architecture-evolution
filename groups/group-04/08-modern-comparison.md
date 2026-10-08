# 8. Modern Comparison

Compare your architecture with a modern processor.

## CPU
* **Histórico:** Pipelines inexistentes ou primitivos de 1 a 3 estágios com execução estritamente sequencial (*in-order*), largura de dados restrita a 8 ou 16 bits, decodificação complexa em múltiplos ciclos (microcódigo) e ausência de paralelismo no nível de instrução.
* **Moderno (RISC-V):** Núcleos escaláveis de 32 ou 64 bits (ex.: RV32I / RV64GC), variando de microcontroladores simples a pipelines superescalares agressivos com execução fora de ordem (*Out-of-Order*), múltiplos decodificadores paralelos, preditores de desvio avançados e conjunto ortogonal de 32 registradores de uso geral (`x0`–`x31`).

## Memory
* **Histórico:** Acesso direto e linear à RAM sem hierarquia de cache integrada ao chip, espaço de endereçamento restrito entre 64 KB e 1 MB (barramentos de 16 a 20 bits) e sem mecanismos de proteção ou memória virtual.
* **Moderno (RISC-V):** Hierarquia completa com múltiplos níveis de cache coerente (L1, L2, L3), gerenciamento de memória virtual por hardware via MMU (perfis padrão Sv39, Sv48 e Sv57, permitindo endereçamento de terabytes/petabytes) e isolamento físico por meio do PMP (*Physical Memory Protection*).

## Bus
* **Histórico:** Barramentos síncronos paralelos compartilhados (larguras de 8 a 16 bits), com alta contenção de sinais, baixas frequências operacionais e sujeitos a ruídos eletromagnéticos.
* **Moderno (RISC-V):** Redes intra-chip (*Network-on-Chip* - NoC) com topologias em anel ou malha (*mesh*), barramentos modernos e modulares de alta largura de banda (TileLink, Wishbone, AXI5) e links seriais ponto a ponto ultrarrápidos (PCIe 5.0/6.0, CXL).

## Performance
* **Histórico:** Taxas de processamento na faixa de dezenas a centenas de milhares de instruções por segundo (KIPS/MIPS), frequências de clock de 1 MHz a 10 MHz e IPC (*Instruções Por Ciclo*) tipicamente abaixo de 0,5.
* **Moderno (RISC-V):** Throughput medido em bilhões de instruções por segundo (GIPS) e operações vetoriais de alto paralelismo (TFLOPS/TOPS), frequências operacionais escalando de 1,5 GHz a mais de 3,5 GHz e núcleos multicore capazes de sustentar IPCs superiores a 3 ou 4 instruções por ciclo.

## Energy Consumption
* **Histórico:** Fabricados em nós litográficos de 3 a 5 micrômetros ($\mu\text{m}$), consumindo tipicamente de 0,5 W a 2,5 W para um rendimento computacional muito baixo, resultando em péssima eficiência por watt.
* **Moderno (RISC-V):** Implementado em nós avançados (FinFET/GAA de 3 nm a 12 nm). A ausência de decodificadores legados complexos garante excepcional eficiência energética, operando desde microwatts ($\mu\text{W}$) em nós de borda e IoT até centenas de watts com alto throughput por watt em datacenters.

## Applications
* **Histórico:** Automação industrial básica, calculadoras científicas, instrumentos de medição, primeiros terminais de computação pessoal e consoles de videogame de 8 bits.
* **Moderno (RISC-V):** Todo o espectro moderno da computação, incluindo sensores vestíveis (IoT), microcontroladores automotivos com segurança crítica (ASIL-D), smartphones, servidores em nuvem, switches de rede de alta densidade e aceleradores neurais para inteligência artificial.

## Advantages
* **Arquitetura Livre e Aberta:** Acesso irrestrito à especificação da ISA sem pagamento de royalties, taxas de licenciamento prévio ou risco de bloqueios comerciais por disputas geopolíticas.
* **Design Modular e Limpo:** Núcleo base compacto (`RV32I`/`RV64I`) que evita *bloatware* de silício, permitindo anexar somente as extensões oficiais necessárias (ex.: M, A, F, D, C, V).
* **Extensibilidade Reservada:** Presença de opcodes dedicados (`custom-0` a `custom-3`) para criação de instruções proprietárias e aceleradores específicos de domínio sem quebrar a compatibilidade com o software padrão.

## Disadvantages
* **Risco de Fragmentação:** A grande flexibilidade na combinação de extensões e instruções personalizadas pode gerar ecossistemas com binários incompatíveis caso os perfis de plataforma (como os perfis RVA) não sejam seguidos rigorosamente.
* **Maturidade de Ecossistema Comercial:** Embora compiladores básicos e o kernel Linux estejam maduros, softwares comerciais legados, drivers proprietários e ferramentas de desenvolvimento de terceiros ainda são menos prevalentes que no ecossistema x86 ou ARM.
* **Densidade de Código sem Compressão:** A ISA básica estritamente alinhada em 32 bits demanda maior consumo de largura de banda de instrução caso a extensão de código comprimido (`RVC`) não seja implementada.

## Comparison Table

| Feature | Historical (ex.: Intel 8086 / MOS 6502) | Modern (RISC-V - ex.: RV64GCV / SiFive P870) |
|---|---|---|
| **Modelo de ISA** | Proprietária, CISC primitiva (ou RISC de 1ª gen) | Aberta, Modular, RISC pura baseada em Load/Store |
| **Tamanho da Palavra** | 8 a 16 bits | 32 ou 64 bits nativos (especificação pronta para 128-bit) |
| **Registradores de Uso Geral** | 4 a 8 registradores com funções dedicadas | 32 registradores ortogonais universais (`x0`–`x31`) |
| **Hierarquia de Memória** | Acesso direto à DRAM/SRAM (sem cache on-chip) | Caches multinível (L1, L2, L3) + Suporte a MMU (Sv39/Sv48/Sv57) |
| **Interconexão Interna** | Barramento paralelo compartilhado (8–16 bits) | Redes no chip (NoC), interfaces TileLink, AXI5 e PCIe Gen5 |
| **Eficiência e Flexibilidade** | Arquitetura fixa em hardware, sem modularidade | Extremamente modular, extensível por aplicação, alta densidade por mm² |
