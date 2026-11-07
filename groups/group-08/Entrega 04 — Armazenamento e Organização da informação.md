# Entrega 04 — Armazenamento e Organização da Informação

| Campo | Descrição |
|---|---|
| **Objeto estudado** | Arquitetura CISC (exemplos: IBM System/360, DEC VAX, Intel x86/x86-64) |
| **Entrega** | 04 — Armazenamento e Organização da Informação |
---
## Resumo

Em máquinas CISC, os dados ficam em **registradores (poucos)** e na **memória principal**, e as instruções podem operar diretamente sobre operandos na memória. Programas ficam na RAM (modelo de programa armazenado), enquanto o **comportamento das instruções** fica em uma **memória de controle (microcódigo)**. A configuração do sistema é mantida em registradores especiais e em tabelas na RAM. O armazenamento permanente é externo à CPU e só chega às instruções via RAM e sistema operacional.

## 1. Onde os dados são mantidos

| Local | IBM System/360 | DEC VAX | Intel x86-64 |
|---|---|---|---|
| Registradores gerais | 16 × 32 bits | 16 × 32 bits (R12=AP, R13=FP, R14=SP, R15=PC) | 16 × 64 bits |
| Ponto flutuante / vetorial | 4 × 64 bits | Usa os registradores gerais | x87 e registradores SIMD (XMM/YMM/ZMM) |
| Estado / flags | PSW | PSL | RFLAGS |
| Memória principal | Núcleo magnético (maioria dos modelos) | Memória semicondutora | DRAM (DDR) |
| Pilha | Sem pilha em hardware (áreas de salvamento + registrador de ligação) | Pilha em RAM (SP), usada por CALLS/CALLG/RET | Pilha em RAM (RSP), usada por PUSH/POP/CALL/RET |

**Característica CISC:** instruções aceitam operandos diretamente na memória (S/360 e x86: registrador-memória; VAX: até memória-memória), então a RAM faz parte do caminho normal de execução de uma instrução, e não só de carga e armazenamento.
---
## 2. Onde programas, instruções e configurações são mantidos

- **Programas do usuário:** na RAM (instruções e dados no mesmo espaço), carregados do armazenamento secundário.
- **Microcódigo:** guardado em uma **memória de controle** dentro da CPU. Cada instrução complexa vira uma sequência de microinstruções. Nas CPUs x86 atuais, o microcódigo pode ser corrigido por atualização carregada na inicialização (volátil: reaplicada a cada boot) — ver [Intel SDM, Vol. 3](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html).
- **Firmware:** BIOS/UEFI em ROM/flash, responsável pela inicialização.
- **Configuração de estado e proteção:**

| Arquitetura | Onde a configuração fica |
|---|---|
| System/360 | PSW (estado atual) e áreas fixas no início da memória, onde ficam os PSWs de interrupção |
| VAX | PSL e registradores internos do processador (acessados por `MTPR`/`MFPR`); tabelas de páginas na RAM |
| x86-64 | Registradores de controle (`CR0`, `CR3`, `CR4`), MSRs (`RDMSR`/`WRMSR`), `GDT`/`IDT` e tabelas de páginas na RAM; opções de plataforma em CMOS/NVRAM ou variáveis UEFI |
---
## 3. Armazenamento temporário

| Nível | Função | Tecnologia |
|---|---|---|
| Registradores e flags | Operandos e estado imediato | Circuitos internos da CPU |
| Buffer/fila de instruções | Instruções de tamanho variável aguardando decodificação | SRAM interna |
| TLB | Cache de traduções endereço virtual → físico (VAX e x86) | SRAM/CAM |
| Caches L1/L2/L3 | Reduzir o tempo de acesso à RAM | SRAM |
| Cache de micro-ops (x86 moderno) | Guarda instruções CISC já traduzidas em micro-operações | SRAM |
| RAM | Programas e dados em execução, pilha, tabelas | Núcleo magnético → DRAM |
---
## 4. Armazenamento permanente

- **ROM/flash:** firmware, bootloader e microcódigo-base.
- **NVRAM/CMOS:** configurações de plataforma mantidas por bateria ou flash.
- **Armazenamento secundário:** fitas, cartões e discos nas máquinas clássicas; HDD/SSD hoje.

O armazenamento permanente **não é endereçável** pelas instruções comuns da CPU: os dados são trazidos para a RAM por E/S e pelo sistema operacional.
---
## 5. Tecnologias utilizadas

| Tecnologia | Uso | Característica |
|---|---|---|
| Núcleo magnético | Memória principal do System/360 | Não volátil, leitura destrutiva |
| ROM / memória de controle | Microcódigo | Leitura rápida, conteúdo fixo ou atualizável |
| SRAM | Registradores, TLB e caches | Muito rápida, cara |
| DRAM | Memória principal atual | Densa, mais lenta, exige refresh |
| Flash (NOR/NAND) | Firmware e SSD | Não volátil |
| Magnética (disco/fita) | Armazenamento secundário | Grande capacidade, acesso lento |
---
## 6. Capacidade

| Máquina | Espaço de endereçamento | Observação |
|---|---|---|
| System/360 | 24 bits → 16 MiB | Modelos reais variavam de poucos KiB a cerca de 1 MiB |
| VAX | 32 bits virtuais → 4 GiB | Páginas de 512 bytes |
| x86-64 | 48 bits virtuais (256 TiB) ou 57 bits com paginação de 5 níveis | Endereço físico limitado pelo modelo de CPU |

Caches e registradores têm tamanho que **varia por modelo**; consultar a documentação do processador específico antes de citar números.
---
## 7. Velocidade e limitações de acesso

Ordem de grandeza medida por Drepper (2007) em um Pentium M: **~3 ciclos (L1)**, **~14 ciclos (L2)** e **~240 ciclos (RAM)**. Valores atuais diferem, mas a proporção entre os níveis se manteve. Ver [Drepper](http://www.akkadia.org/drepper/cpumemory.pdf).

**Limitações típicas de CISC ligadas ao armazenamento:**

- Poucos registradores geram mais acessos à memória.
- Modos de endereçamento complexos podem exigir **vários acessos à memória em uma única instrução**, o que complica o pipeline.
- Instruções de tamanho variável dificultam a busca e a decodificação.
- Dados e instruções disputam o mesmo caminho até a memória (gargalo de von Neumann).
- O descompasso entre CPU e DRAM torna a hierarquia de caches indispensável.
---
## 8. Relação entre armazenamento e funcionamento

1. **Densidade de código:** instruções complexas fazem mais por instrução e ocupam menos da memória, que era cara.
2. **Microprogramação:** a memória de controle faz o papel da unidade de controle, permitindo o mesmo conjunto de instruções em hardwares diferentes (ideia central do System/360).
3. **Ciclo de execução:** busca na RAM → decodificação (microcódigo ou micro-ops) → execução com registradores e cache como intermediários.
4. **Estado do sistema:** registradores de estado/controle e tabelas na RAM definem modo de operação, proteção e memória virtual.
5. **Evolução:** o x86 moderno mantém a interface CISC, mas executa micro-ops internamente com arquivo de registradores físicos e caches.
---
## Fontes

| # | Referência | Usada em |
|---|---|---|
| 1 | Intel. *Intel 64 and IA-32 Architectures Software Developer’s Manual*. [Link](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html) | Seções 1, 2, 3, 6 |
| 2 | IBM. *System/360 Principles of Operation* (A22-6821). [Link](https://ftp.mirrorservice.org/sites/www.bitsavers.org/pdf/ibm/360/princOps/A22-6821-6_360PrincOpsJan67.pdf) | Seções 1, 2, 6 |
| 3 | DEC. *VAX Architecture Handbook* (1981). [Link](https://50anni.informatica.unisa.it/wp-content/uploads/2023/02/02-VAX-architecture-handbook-1981.pdf) | Seções 1, 2, 6 |
| 4 | DREPPER, U. *What Every Programmer Should Know About Memory* (2007). [Link](http://www.akkadia.org/drepper/cpumemory.pdf) | Seções 3, 7 |
