# 8. Modern Comparison

Comparação da arquitetura **RISC-V** com outras arquiteturas e processadores fundamentais da computação moderna e histórica (**ARMv8/v9**, **x86-64** e arquiteturas clássicas como **MIPS**).

---

## 8.1 Finalidade e Modelo de Licenciamento

* **RISC-V:** Padrão aberto, livre e livre de royalties (*open standard*), gerido internacionalmente pela fundação neutra *RISC-V International*. Sua finalidade é prover uma ISA universal, extensível e atemporal, cobrindo desde microcontroladores embarcados de ultrabaixo consumo até aceleradores de Inteligência Artificial e supercomputadores de datacenters, sem risco de embargos comerciais ou dependência de fornecedor único (*vendor lock-in*).
* **ARM (ARMv8/v9):** Arquitetura proprietária desenvolvida pela ARM Ltd. É licenciada comercialmente sob rigorosos acordos de Propriedade Intelectual (IP core licensing ou architectural licensing). Destina-se predominantemente a smartphones, computação móvel, automotiva e, mais recentemente, servidores e desktops (ex.: Apple Silicon).
* **x86-64 (Intel / AMD):** Arquitetura proprietária protegida por patentes cruzadas entre Intel e AMD. Voltada tradicionalmente para computação pessoal de alta performance, estações de trabalho e servidores corporativos. É um modelo de ecossistema totalmente fechado à criação de novos fabricantes.
* **MIPS (MIPS I a VI):** Arquitetura RISC pioneira do meio acadêmico e comercial dos anos 1980 e 1990. Foi historicamente voltada para estações de trabalho, consoles de videogame e roteadores embarcados. Operou como padrão proprietário durante o auge e, apesar de tardias tentativas de abertura, perdeu relevância industrial para o ecossistema aberto do RISC-V.

---

## 8.2 Organização e Filosofia de Design

* **RISC-V:** Filosofia RISC pura com paradigma **Modular**. É composta por uma base enxuta e estritamente congelada (menos de 50 instruções no `RV32I` ou `RV64I`), sobre a qual são acopladas extensões padronizadas opcionais (ex.: `M` para multiplicação/divisão inteira, `A` para atômicas, `F`/`D` para ponto flutuante, `C` para instruções comprimidas e `V` para vetores). Evita o acúmulo de débito técnico no silício: processadores pequenos só implementam o necessário.
* **ARM:** Filosofia RISC comercial que evoluiu para um modelo de grande complexidade. A transição para ARMv8-A (AArch64) introduziu uma ISA limpa de 64 bits, mas que ainda mantém especificações massivas com centenas de instruções obrigatórias por perfil, encarecendo a implementação em hardwares minimalistas.
* **x86-64:** Filosofia **CISC** (Complex Instruction Set Computer) com mais de 45 anos de legado acumulado. Para manter compatibilidade binária regressiva com o Intel 8086 de 1978, os chips modernos gastam parcela considerável da área de silício e consumo elétrico em decodificadores complexos que traduzem em tempo de execução instruções CISC variáveis (1 a 15 bytes) em micro-operações ($\mu\text{ops}$) internas semelhantes a RISC.
* **MIPS:** RISC clássico baseado em arquitetura ortogonal simples de 32 registradores e instruções de tamanho fixo em 32 bits.

---

## 8.3 Processamento e Organização da CPU

* **Registradores de Uso Geral:**
  * **RISC-V:** 32 registradores ortogonais universais (`x0`–`x31`), onde `x0` é fixado por hardware em zero absoluto. Não impõe registradores dedicados para condições ou operações aritméticas implícitas.
  * **ARM (AArch64):** 31 registradores de propósito geral (`X0`–`X30`) mais ponteiro de pilha dedicado (`SP`) e registrador de zero (`XZR`). Utiliza flags de condição explícitas (NZCV) em um registrador de status.
  * **x86-64:** 16 registradores de uso geral (`RAX`, `RBX`, `RCX`, `RDX`, `RSI`, `RDI`, `RBP`, `RSP`, `R8`–`R15`). O uso de vários registradores ainda carrega restrições herdadas do design original de 16 bits.
* **Mecanismos de Salto e Desvios:**
  * **RISC-V:** Realiza testes condicionais e desvio em uma única instrução (ex.: `beq rs1, rs2, offset`), dispensando registradores dedicados de flags/condição e facilitando o paralelismo. **Eliminou deliberadamente os *branch delay slots***, evitando gargalos em pipelines superescalares e fora de ordem modernos.
  * **MIPS:** Introduziu pioneiramente o *branch delay slot* no silício, uma decisão arquitetural que obrigava a execução da instrução seguinte ao salto. Embora vantajoso em pipelines simples de 5 estágios nos anos 1980, tornou-se um grande obstáculo para processadores superescalares e fora de ordem modernos.
  * **x86-64:** Depende de operações aritméticas prévias que atualizam o registrador `EFLAGS`, seguidas por instruções separadas de salto condicional (`Jcc`).

---

## 8.4 Armazenamento e Hierarquia de Memória

* **Modelo de Memória:**
  * **RISC-V:** Arquitetura estrita **Load/Store** — operações aritméticas e lógicas operam unicamente entre registradores; a memória é acessada exclusivamente via `load` e `store`. Define formalmente o modelo de consistência de memória **RVWMO** (*RISC-V Weak Memory Ordering*), oferecendo flexibilidade máxima para alto desempenho em hardware com sincronização explícita via instruções `fence`.
  * **ARM:** Também Load/Store, adota modelo de consistência de memória fraca (Weakly Ordered).
  * **x86-64:** Suporta instruções onde um dos operandos pode ser um endereço direto de memória (ex.: `add [rbx], rax`), demandando circuitos de controle e acesso à memória complexos durante a execução. Opera sob o modelo estrito **TSO** (*Total Store Order*).
* **Gerenciamento de Memória Virtual:**
  * **RISC-V:** Especifica esquemas de paginação elegantes e escaláveis por hardware na MMU: **Sv32** (para RV32), **Sv39**, **Sv48** e **Sv57** (para RV64, permitindo endereçar terabytes a petabytes virtuais em 3, 4 ou 5 níveis de tabelas de páginas). Inclui proteção física nativa por hardware com **PMP** (*Physical Memory Protection*).
  * **x86-64 / ARM:** Empregam paginação de 4 a 5 níveis em 64 bits (ex.: x86-64 4-level/5-level paging), com suporte amplo a virtualização assistida por hardware.

---

## 8.5 Comunicação e Barramentos

* **RISC-V:** Não impõe um barramento fechado. Adota barramentos e redes no chip (*Network-on-Chip* - NoC) abertos, modulares e livres de royalties, como o **TileLink** (desenvolvido em Berkeley), **Wishbone** e interfaces de indústria padronizadas como **AMBA AXI4/AXI5** e interconexões externas de altíssima velocidade como **PCIe 5.0/6.0** e **CXL** (*Compute Express Link*).
* **ARM:** Depende historicamente do ecossistema proprietário de barramentos **AMBA** (AXI, AHB, APB, CHI), amplamente consolidado na indústria.
* **x86-64:** Utiliza barramentos e interconexões ponto a ponto proprietárias de altíssima largura de banda e coerência de cache entre soquetes, como **Intel UPI/QPI** e **AMD Infinity Fabric**.

---

## 8.6 Paralelismo e Vetorização

* **RISC-V (Extensão Vetorial - `RVV`):** Projetada sob a filosofia *Vector-Length Agnostic* (VLA). O código vetorial compilado para RISC-V não é amarrado à largura física dos registradores do silício. Uma mesma sequência binária pode executar em um núcleo embarcado com registradores vetoriais de 128 bits ou em um supercomputador com registradores de 1024 bits sem necessidade de recompilação.
* **ARM (NEON / SVE):** Tradicionalmente utilizou NEON (vetores fixos de 128 bits). O padrão mais recente, SVE/SVE2, também adotou abordagem agnóstica de comprimento, inspirando-se em conceitos similares aos do RISC-V.
* **x86-64 (SSE / AVX / AVX-512):** Vetores de largura estritamente fixa e incremental (128 bits no SSE, 256 bits no AVX/AVX2, 512 bits no AVX-512). Cada expansão de largura física exigiu novas instruções na ISA, gerando enorme proliferação de opcodes e complexidade no compilador.

---

## 8.7 Tabela Comparativa Geral

| Critério | RISC-V | ARMv8/v9 (AArch64) | x86-64 (AMD64 / Intel 64) | MIPS (MIPS I–VI) |
|---|---|---|---|---|
| **Modelo da ISA** | Aberta, Modular, RISC | Proprietária, Licenciada, RISC | Fechada, Proprietária, CISC | Proprietária (histórica), RISC |
| **Filosofia de Projeto** | Base congelada + Módulos opcionais | Padrão monolítico por perfil | Acúmulo histórico com decodificação $\mu\text{op}$ | RISC ortogonal clássico |
| **Tamanho da Instrução** | 32 bits (16 bits com extensão `C`) | 32 bits fixos | Variável (1 a 15 bytes) | 32 bits fixos |
| **Registradores (GPR)** | 32 ortogonais (`x0` aterrado) | 31 (`X0`–`X30`) + SP + XZR | 16 (`RAX`, `RBX`, `RCX`...) | 32 (`$0` aterrado a zero) |
| **Acesso à Memória** | Load/Store estrito (RVWMO) | Load/Store estrito (Weak) | Operações memória-registrador (TSO) | Load/Store estrito |
| **Desvios e Saltos** | Comparação e salto integrados (sem flags) | Baseado em flags de condição (NZCV) | Instruções separadas de flag e salto | Branch delay slot obrigatório |
| **Vetorização / SIMD** | RVV (Vector-Length Agnostic) | NEON (fixo) / SVE (agnóstico) | SSE/AVX/AVX-512 (tamanhos fixos) | MDMX / MSA (tamanhos fixos) |
| **Customização** | Opcodes livres reservados (`custom-0..3`) | Altamente restrita sob licença cara | Inexistente para terceiros | Restrita e licenciada |
| **Ecossistema Comercial** | Em rápido crescimento industrial | Altamente maduro e dominante em mobile | Dominante em PCs e servidores | Mercado legado e descontinuado |

---

## 8.8 Vantagens e Limitações do RISC-V Frente aos Concorrentes

### Vantagens
1. **Ausência de Royalties e Independência Geopolítica:** Nenhuma entidade ou nação pode revogar o direito de implementar e fabricar chips RISC-V.
2. **Eficiência Térmica e de Área:** Por não carregar transistores dedicados a decodificar legados de décadas, os núcleos RISC-V apresentam excelente relação de instruções por ciclo por watt (IPC/Watt).
3. **Aceleração Customizada de Domínio (DSA):** Facilidade de anexar aceleradores de hardware e instruções proprietárias para IA, criptografia ou processamento de sinais sem fragmentar os softwares padrão.

### Limitações
1. **Risco de Fragmentação:** Como as extensões são modulares, hardwares com combinações arbitrárias de módulos exigem a adesão estrita aos perfis padronizados de plataforma (como *RVA22* e *RVA23*) para garantir compatibilidade binária no ecossistema Linux/Android.
2. **Maturidade de Software Comercial Proprietário:** Embora o suporte a compiladores abertos (GCC, LLVM), Linux, FreeBSD e Android esteja plenamente consolidado, bibliotecas proprietárias de jogos e ferramentas de produtividade para o usuário final ainda se concentram maciçamente em x86 e ARM.
