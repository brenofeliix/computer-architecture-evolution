# 6. Input and Output

## 6.1 Input Devices
A especificação da ISA RISC-V não define dispositivos físicos de entrada específicos nem cria instruções exclusivas para leitura de periféricos (como comandos legados de leitura de porta).
Em vez disso, a interação com qualquer dispositivo de entrada — como teclados, botões, sensores ambientais ou receptores de rede — ocorre por meio de MMIO (Memory-Mapped I/O). O hardware mapeia os registradores de status e dados desses dispositivos em faixas específicas do espaço de endereçamento da CPU.
Para ler uma entrada do mundo exterior, o processador utiliza suas instruções padrão de leitura de memória (LB, LH, LW, LD). O controlador do periférico captura esse endereço no barramento e devolve o dado recebido, tornando o processo de leitura de entrada idêntico, para o pipeline da CPU, a uma leitura de RAM.

## 6.2 Output Devices
De maneira análoga aos dispositivos de entrada, os dispositivos de saída — como displays gráficos, controladores de vídeo, LEDs, atuadores ou transmissores seriais — não possuem instruções especiais de escrita na arquitetura RISC-V.
O envio de comandos ou dados para componentes de saída é realizado através das instruções padrão de armazenamento na memória (SB, SH, SW, SD). O processador grava o dado no registrador de controle ou de buffer do periférico mapeado em memória.
A especificação privilegia regras de consistência para essas regiões: através dos atributos físicos de memória (PMAs), regiões de saída podem ser marcadas como não-cacheáveis e suscetíveis a efeitos colaterais (side effects), garantindo que escritas consecutivas não sejam descartadas ou mescladas indevidamente pela hierarquia de cache.

## 6.3 Communication Interfaces
As interfaces de comunicação em um sistema RISC-V são integradas como periféricos externos ao núcleo da CPU e controladas via barramento de interconexão do SoC. Exemplos amplamente utilizados em plataformas RISC-V incluem:
UART: Comunicação serial assíncrona comumente usada para terminais de console e depuração básica;
SPI e I2C: Barramentos seriais síncronos voltados à comunicação de baixa velocidade com circuitos integrados, memórias Flash EEPROM e sensores;
PCIe (PCI Express): Interconexão de alta velocidade utilizada em SoCs de alto desempenho para placas gráficas, aceleradores e adaptadores de rede;
Ethernet / CAN: Comunicação para redes locais e aplicações industriais/automotivas.
A ISA não regulamenta os registradores desses controladores. As plataformas tipicamente adotam padrões consolidados da indústria (como UARTs compatíveis com o padrão 16550) ou especificações abertas da própria comunidade.

## 6.4 Peripheral Devices
Os periféricos em um sistema RISC-V englobam tanto componentes utilitários de suporte ao núcleo quanto controladores de I/O em larga escala:
Controladores de Interrupção: Elementos fundamentais para I/O orientado a interrupção. O ecossistema define componentes padronizados como o CLINT (Core-Local Interruptor) / CLIC (Core-Local Interrupt Controller) para sistemas de tempo real, e o PLIC (Platform-Level Interrupt Controller) ou AIA (Advanced Interrupt Architecture) para distribuir interrupções de periféricos entre múltiplos núcleos (HARTS);
Timers (Temporizadores): Mapeados via registradores de controle e status (CSRs de tempo) ou como registradores de hardware de plataforma (ex.: mtime e mtimecmp), fundamentais para alternância de tarefas e escalonamento em sistemas operacionais;
Controladores de DMA (Direct Memory Access): Dispositivos que transferem blocos volumosos de dados diretamente entre periféricos e a memória RAM sem ocupar ciclos da CPU;
GPIO (General Purpose Input/Output): Linhas de pinos programáveis de entrada e saída direta em microcontroladores.

## 6.5 Examples
Para ilustrar a comunicação de I/O em RISC-V, o exemplo mais comum é a transmissão de um caractere via porta serial UART 16550 mapeada em memória:

    # Exemplo conceitual: Envio de um caractere via UART MMIO
    # Endereço base da UART mapeado em 0x10000000
    
    li   t0, 0x10000000       # Carrega o endereço base da UART
    li   t1, 0x41             # Caractere ASCII 'A'
    
    wait_tx_ready:
        lb   t2, 5(t0)            # Lê o Line Status Register (LSR) no offset +5
        andi t2, t2, 0x20         # Testa se o bit 5 (Transmitter Holding Empty) está ativo
        beqz t2, wait_tx_ready    # Enquanto não estiver pronto, continua aguardando
        sb   t1, 0(t0)            # Escreve o caractere 'A' no offset 0 (THR) -> transmite
    
Neste modelo, polling de status e transmissão são feitos exclusivamente com instruções de carga e descarga (lb e sb), sem nenhum comando proprietário de I/O.

## 6.6 Historical Evolution
A abordagem de tratamento de entrada e saída evoluiu de forma expressiva ao longo da história da computação:
Port-Mapped I/O (PMIO) / Instruções Dedicadas: Arquiteturas clássicas das décadas de 1970 e 1980 (como Intel 8086/x86) utilizavam um espaço de endereçamento de I/O segregado da memória RAM, dependendo de instruções de máquina específicas (como IN e OUT) e linhas de controle físico de barramento exclusivas;
Consolidação do Memory-Mapped I/O (MMIO): Com a ascensão das arquiteturas RISC na década de 1980 (como MIPS e ARM), eliminou-se o espaço isolado de portas. Periféricos passaram a residir no mesmo mapa de memória linear, simplificando decodificadores e o conjunto de instruções;
Desacoplamento por Barramentos Padronizados e DMA: A evolução de sistemas multicore delegou transferências massivas para barramentos estruturados (como AXI e PCIe) gerenciados por DMA, reduzindo o tráfego direto de polling no núcleo principal;
Abordagem RISC-V: O RISC-V eliminou qualquer resquício de instruções de porta legadas. O controle de ordenação e previsibilidade de periféricos é resolvido diretamente através de regras de consistência de memória fracas (RVWMO) combinadas com a instrução FENCE (ou a extensão de I/O FENCE.I).

## 6.7 Modern Comparison
| Característica | RISC-V | x86-64 | ARM |
|---------|--------------|--------|--------|
| Instruções Dedicadas de I/O | Não possui (apenas instruções de memória) | Possui (IN, OUT, INS, OUTS) para compatibilidade legada | Não possui (modelo estritamente MMIO) |
| Modelo Predominante de I/O | Memory-Mapped I/O (MMIO) | Predominantemente MMIO (embora ainda suporte PMIO) | Memory-Mapped I/O (MMIO) |
| Controlador de Interrupções Padrão | PLIC / CLIC / AIA (padronizados abertamente) | APIC / x2APIC | GIC (Generic Interrupt Controller) |
| Sincronização de I/O | Instrução FENCE com flags de I/O (I, O, R, W) | Instruções serializadoras (ex.: MFENCE, SFENCE) | Instruções de barreira de memória (DMB, DSB) |
| Acesso Direto à Memória (DMA) | Suportado via controladores integrados no SoC | Suportado nativamente via chipset/controladores | Suportado via componentes de barramento AMBA |
