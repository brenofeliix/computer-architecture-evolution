# 4. Memory Organization

## 4.1 Memory Hierarchy

A RISC-V não define uma hierarquia de memória física específica. A organização pode variar de acordo com a implementação do processador e da plataforma.

Em um sistema RISC-V típico, pode existir uma hierarquia semelhante à encontrada em outros processadores modernos:

**Registradores → Cache L1 → Cache L2/L3 → RAM → Armazenamento**

Os registradores estão diretamente associados ao núcleo do processador e apresentam acesso muito rápido. As caches armazenam cópias de dados e instruções que podem ser reutilizados pelo processador, reduzindo a latência média dos acessos à memória. A RAM fornece o espaço principal para dados e programas em execução, enquanto dispositivos de armazenamento, como SSDs e eMMC, são utilizados para manter dados de forma persistente.

Entretanto, nenhum desses níveis, sua quantidade ou sua capacidade é obrigatória na arquitetura RISC-V. A especificação permite diferentes organizações de memória, desde sistemas simples com pouca memória até processadores multicore com múltiplos níveis de cache.

## 4.2 RAM 

A **RAM (Random Access Memory)** é utilizada para armazenar temporariamente programas e dados que estão sendo utilizados pelo sistema.

Na RISC-V, a memória é organizada em um espaço de endereços **endereçável por byte**. O processador realiza operações de leitura e escrita utilizando instruções de acesso à memória, como `LB`, `LW` e `LD` para leituras e `SB`, `SW` e `SD` para escritas, dependendo da largura da arquitetura e das extensões disponíveis.

A especificação RISC-V não determina uma quantidade específica de RAM. A capacidade disponível depende da plataforma em que o processador é implementado. O espaço de endereçamento também pode conter regiões destinadas a dispositivos de entrada e saída, além da memória principal.

## 4.3 ROM

A **ROM (Read-Only Memory)** é uma memória não volátil utilizada para armazenar informações que precisam permanecer disponíveis mesmo quando o sistema está desligado.

Em sistemas RISC-V, uma ROM pode ser utilizada, por exemplo, para armazenar código de inicialização, firmware ou outras informações permanentes do sistema. Entretanto, a presença, capacidade e tecnologia utilizada para implementar essa ROM não são determinadas pela ISA RISC-V.

A especificação define **Physical Memory Attributes (PMAs)** para descrever características das regiões físicas de memória. Uma dessas regiões pode corresponder a uma ROM integrada ao chip, cujos atributos são determinados pelo projeto do hardware.

## 4.4 Cache

A **cache** é uma memória de alta velocidade utilizada para armazenar cópias de dados e instruções que podem ser acessados novamente pelo processador.

Em uma implementação RISC-V, podem existir caches L1, L2 e até L3, dependendo do projeto. Entretanto, a arquitetura **não exige uma determinada quantidade, capacidade ou organização de caches**.

A especificação define conceitos relacionados à cacheabilidade e à coerência de memória, mas deixa a organização física para a implementação. O tamanho dos caches, número de níveis, tamanho dos blocos e outras características podem variar entre diferentes processadores RISC-V.

Uma cache pode satisfazer uma operação de leitura sem que seja necessário acessar diretamente a memória principal. Dessa forma, ela reduz a latência média dos acessos e pode melhorar o desempenho do processador.

## 4.5 Storage

O **armazenamento** corresponde aos dispositivos utilizados para manter dados de maneira persistente, mesmo quando o sistema está desligado.

Em uma plataforma RISC-V, podem ser utilizados diferentes tipos de armazenamento, como:

* Flash;
* eMMC;
* SSD;
* cartões SD;
* outros dispositivos de armazenamento conectados à plataforma.

A escolha do dispositivo de armazenamento não faz parte da ISA RISC-V. Ela depende da plataforma e dos componentes utilizados pelo fabricante.

Além disso, dispositivos e outras regiões de hardware podem ser mapeados no espaço de endereços do sistema, permitindo que o processador interaja com eles por meio de acessos à memória.

## 4.6 Memory Capacity

A RISC-V **não possui uma capacidade de memória única ou fixa**.

A capacidade máxima efetivamente utilizável depende principalmente da largura do endereço físico suportada pela implementação, do sistema operacional e da plataforma.

Por exemplo, uma implementação RV32 utiliza endereços de 32 bits em seu modelo básico, enquanto uma implementação RV64 utiliza registradores e endereços efetivos de 64 bits. Em sistemas RV64 que utilizam memória virtual, diferentes esquemas podem ser empregados, como **Sv39, Sv48 e Sv57**, proporcionando diferentes espaços de endereçamento virtual.

Isso permite que a arquitetura seja utilizada em sistemas com capacidades de memória muito diferentes, desde microcontroladores com pequenas quantidades de memória até sistemas computacionais mais complexos.

Portanto, não seria correto afirmar que "um processador RISC-V possui X GB de RAM" sem especificar um modelo ou implementação concreta.

## 4.7 Comparison with Modern Systems

A organização de memória dos sistemas RISC-V pode ser comparada à de arquiteturas modernas como x86-64 e ARM, mas é importante diferenciar **ISA** de **implementação**.

| Característica          | RISC-V                                                  | x86-64                                         | ARM                                    |
| ----------------------- | ------------------------------------------------------- | ---------------------------------------------- | -------------------------------------- |
| RAM                     | Definida pela plataforma                                | Definida pela plataforma                       | Definida pela plataforma               |
| Cache                   | Opcional e definida pela implementação                  | Definida pela implementação                    | Definida pela implementação            |
| ROM/Firmware            | Definida pela plataforma                                | Definida pela plataforma                       | Definida pela plataforma               |
| Armazenamento           | Definido pela plataforma                                | Definido pela plataforma                       | Definido pela plataforma               |
| Espaço de endereçamento | Depende da implementação (RV32/RV64 e demais recursos)  | Depende da implementação e do modo de operação | Depende da implementação e arquitetura |
| Memória virtual         | Definida pela arquitetura privilegiada e suas extensões | Suportada pela arquitetura                     | Suportada pela arquitetura             |
| Modelo de memória       | RVWMO                                                   | Modelos definidos pela arquitetura             | Modelos definidos pela arquitetura     |
| Hierarquia de cache     | Não especificada de forma fixa                          | Não especificada de forma fixa                 | Não especificada de forma fixa         |
| Customização            | Alta, devido à natureza modular e às extensões          | Mais limitada pela ISA proprietária            | Definida pelas especificações ARM      |

A principal diferença conceitual é que a RISC-V foi projetada como uma **ISA aberta e modular**, permitindo que diferentes fabricantes desenvolvam implementações com organizações de memória distintas. A especificação define o comportamento que deve ser observado pelo software, enquanto características como quantidade de RAM, tamanho das caches e organização física da memória ficam principalmente a cargo da implementação.

O modelo de memória RISC-V é denominado **RVWMO (RISC-V Weak Memory Ordering)**. Ele define regras para a ordenação observável das operações de memória, mas não exige uma hierarquia de cache específica ou mesmo a utilização de um protocolo de coerência específico.
