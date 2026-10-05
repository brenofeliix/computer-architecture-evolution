# Entrega 01 — Mapeamento Inicial da Arquitetura

## Members
* 1 - Iago Henrico Paiva Fouletto
* 2 - Iago Gabriel Bortuluzzi
* 3 - Guilherme Simões de Souza
* 4 - Gustavo Balsan Jurach

## Selected Architecture
* Arquitetura **RISC-V**

## Initial Overview
A arquitetura RISC-V é um conjunto de instruções (ISA) totalmente aberto e gratuito, projetado para servir como um padrão universal e flexível que atende desde microcontroladores simples até chips robustos de servidores e inteligência artificial.

## Historical Context
* **Período/Ano:** Criada em 2010.
* **Fabricante ou Organização:** Desenvolvida na Universidade da Califórnia em Berkeley e mantida atualmente pela associação global sem fins lucrativos RISC-V International.
* **Contexto de surgimento:** Surgiu para solucionar o custo excessivo e as restrições de arquiteturas fechadas (como ARM e x86), além de combater o acúmulo de instruções antigas e complexas em chips legados ao longo de décadas, apostando em modularidade.
* **Importância histórica:** Democratizou o design de processadores ao eliminar a necessidade de pagamento de licenças ou patentes fechadas, permitindo que qualquer pessoa, universidade ou empresa crie sua própria tecnologia.

## Technical Characteristics
* **Arquitetura/ISA:** Em vez de criar um chip enorme que tenta fazer de tudo logo de fábrica, ele é dividido em uma base simples e módulos opcionais. RISC (Reduced Instruction Set Computer) com conjunto de instruções modular e base fixa enxuta (menos de 50 instruções no RV32I/RV64I), complementada por extensões opcionais como M (multiplicação/divisão), F/D (ponto flutuante), C (comprimidas) e V (vetorial).
* **Tamanho de palavra:** Versões de 32 bits (RV32I) e 64 bits (RV64I).
* **Processador:** Núcleo estruturado com Unidade Lógica e Aritmética (ALU), Unidade de Controle, pipeline de 5 etapas (Busca, Decodificação, Execução, Acesso à Memória e Escrita).
* **Memória:** Hierarquia envolvendo registradores internos (32 posições, com o x0 fixo em zero), caches L1/L2 e Memória Principal (RAM), utilizando paginação (como Sv39 e Sv48) para proteção e memória virtual.
* **Entrada e saída:** Utiliza o modelo de memória mapeada (MMIO), tratando sensores, redes e periféricos como endereços normais de leitura e escrita.
* **Outras características relevantes:** Suporte nativo a múltiplos núcleos, extensões de segurança (níveis de privilégio Usuário, Supervisor e Máquina por meio de registradores CSRs) e paralelismo de dados (vetores).

## Relevance to Computer Architecture
O RISC-V é de suma importância para compreender a evolução das arquiteturas de computadores por introduzir o conceito de **open-source hardware** (hardware de código aberto). Ao quebrar o modelo tradicional de monopólios e licenças restritivas de conjuntos de instruções, ele permitiu flexibilidade total de projeto, modularidade em blocos e forte avanço na pesquisa acadêmica, em sistemas embarcados e em dispositivos de Internet das Coisas (IoT).
