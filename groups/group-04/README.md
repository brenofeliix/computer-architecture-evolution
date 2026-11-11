# Arquitetura RISC-V — Grupo 04

Trabalho de investigação e documentação técnica desenvolvido para o projeto semestral **Computer Architecture Evolution**, integrando análise histórica, conceitos de organização de computadores, evolução tecnológica, comparação com arquiteturas contemporâneas e legado da arquitetura aberta **RISC-V**.

---

## Integrantes do Grupo

* **1 - Iago Henrico Paiva Fouletto**
* **2 - Iago Gabriel Bortuluzzi**
* **3 - Guilherme Simões de Souza**
* **4 - Gustavo Balsan Jurach**

---

## Visão Geral da Arquitetura Selecionada

A arquitetura **RISC-V** (pronuncia-se *"risk-five"*) é um conjunto de instruções (ISA - *Instruction Set Architecture*) totalmente aberto, livre e livre de royalties (*royalty-free*), desenvolvido inicialmente em 2010 na Universidade da Califórnia em Berkeley e mantido atualmente pela associação internacional sem fins lucrativos **RISC-V International**, sediada na Suíça.

Diferente da maioria das ISAs comerciais (como x86 e ARM), o RISC-V não é um chip proprietário vendido por uma única empresa, mas sim uma especificação padrão pública que pode ser livremente implementada por qualquer desenvolvedor, universidade ou indústria de semicondutores. Seu design é fundamentado na filosofia **RISC pura** e no conceito de **modularidade**: possui uma base enxuta e congelada (`RV32I` ou `RV64I` com menos de 50 instruções) que pode ser complementada por extensões opcionais padronizadas (`M`, `A`, `F`, `D`, `C`, `V`, `B`, `K`), atendendo desde microcontroladores embarcados até servidores de datacenters e aceleradores de Inteligência Artificial.

---

## Sumário e Estrutura da Documentação

A documentação do grupo está organizada nos seguintes módulos temáticos:

| Capítulo / Arquivo | Foco da Investigação |
|---|---|
| [01-history.md](01-history.md) | Antecedentes históricos, criadores, governança neutra e evolução tecnológica |
| [02-system-overview.md](02-system-overview.md) | Visão geral da organização do sistema, componentes principais e diagrama em blocos |
| [03-cpu.md](03-cpu.md) | Organização interna da CPU (HART), pipeline, registradores e lógica de execução |
| [04-memory.md](04-memory.md) | Hierarquia de memória, modelo RVWMO, paginação virtual (Sv32/Sv39/Sv48) e PMP |
| [05-bus.md](05-bus.md) | Interconexões, barramentos abertos (TileLink, Wishbone) e protocolos de mercado (AXI) |
| [06-input-output.md](06-input-output.md) | Modelo de E/S mapeada em memória (MMIO) e controladores de interrupção (PLIC/CLINT/AIA) |
| [07-instruction-set.md](07-instruction-set.md) | Formatos de instrução da ISA, decodificação ortogonal e taxonomia de opcodes |
| [08-modern-comparison.md](08-modern-comparison.md) | Comparação aprofundada com ARMv8/v9, x86-64 e MIPS (desempenho, IPC, energia, vetorização) |
| [09-legacy.md](09-legacy.md) | Legado histórico, impacto científico, relevância na indústria e soberania geopolítica |
| [10-research-question.md](10-research-question.md) | Questão de pesquisa norteadora do grupo |
| [11-technical-example.md](11-technical-example.md) | Exemplo prático detalhado de código e execução na arquitetura |
| [13-operations-and-data.md](13-operations-and-data.md) | Tipos de dados suportados, representação numérica e operações fundamentais |
| [references.md](references.md) | Referências bibliográficas completas conforme normas acadêmicas |

---

## Principais Destaques Técnicos

* **Arquitetura Aberta e Sem Royalties:** Democratização do silício sem cobrança de taxas de propriedade intelectual.
* **Base Congelada e Estável:** Garantia de compatibilidade contínua de software sem quebrar o ecossistema.
* **Modelo Load/Store Puro:** Instruções estritamente ortogonais, 32 registradores universais (`x0` fixado em zero) e sem dependência de microcódigo legado.
* **Vetorização VLA (*Vector-Length Agnostic*):** Código vetorial flexível que se adapta em tempo de execução à capacidade do hardware (extensão `RVV`).
* **Aceleração por Domínio (DSA):** Facilidade de acoplamento de co-processadores neurais e criptográficos através de opcodes reservados (`custom-0` a `custom-3`).
