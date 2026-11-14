QP6 - Como foi a evolução dos 16 para os 64 bits?

*Visão geral*: A família x86 nasceu com o Intel 8086 (1978) e passou por três grandes gerações: 16 bits (8086 e 80286), 32 bits (80386 em diante) e 64 bits (x86-64, a partir de 2003). A cada geração o tamanho dos registradores e do espaço de endereçamento aumentou, sempre mantendo a compatibilidade com o software antigo.

*Era de 16 bits (1978-1985)*: O 8086 tinha registradores de 16 bits (AX, BX, CX, DX, SI, DI, BP, SP) e barramento de endereços de 20 bits, o que permitia acessar até 1 MB de memória. Como 16 bits só endereçam 64 KB, a memória era organizada em segmentos, com o endereço formado por segmento:offset. O 8088, usado nos primeiros IBM PC, era igual por dentro, mas com barramento externo de dados de 8 bits (mais barato). O 80286 (1982) continuou de 16 bits, passou a endereçar 24 bits (16 MB) e introduziu o modo protegido.

*Era de 32 bits (1985-2003)*: O 80386 (1985) ampliou os registradores para 32 bits (EAX, EBX, ..., ESP, EIP) e permitiu endereçar até 4 GB de memória. Também popularizou o endereçamento plano (*flat*), no qual o programa enxerga a memória como um único bloco contínuo, sem depender de segmentos de 64 KB. Esse modelo viabilizou sistemas operacionais multitarefa mais robustos, como o Unix.

*Era de 64 bits (2003 em diante)*: O x86-64 (também chamado de AMD64, x64 ou Intel 64) foi lançado pela AMD em 2003 (Opteron) e depois adotado pela Intel. Os registradores passaram a ter 64 bits (RAX, RBX, ..., RSP, RIP) e o número de registradores de propósito geral subiu de 8 para 16, com R8 a R15. O espaço de endereçamento teórico chegou a 16 EB, contra os 4 GB do x86 de 32 bits. Na prática, os processadores atuais implementam bem menos que isso.

*Compatibilidade*: O x86-64 manteve modos de operação para o software antigo. O *long mode* executa código de 64 bits e oferece um modo de compatibilidade para aplicações de 32 bits. Os processadores modernos ainda conseguem funcionar como um 8086 no modo real, e por isso o conjunto de instruções carrega a herança de cada geração.

Resumo comparativo:

| Geração | Exemplo | Registradores | Memória endereçável |
|---|---|---|---|
| 16 bits | 8086 (1978) | 16 bits | 1 MB |
| 32 bits | 80386 (1985) | 32 bits | 4 GB |
| 64 bits | x86-64 (2003) | 64 bits (16 de propósito geral) | 16 EB (teórico) |

Fontes:
- Intel x86 Assembly, material de aula (KAIST) - https://softsec.kaist.ac.kr/depot/sangkilc/is561/03-assembly.pdf
- Evolução dos processadores x86, 8086 e 8088 (Multiasm, RoboticLab) - https://home.roboticlab.eu/en/multiasm/papc/chapter_6_1?do=diff
- x64 vs. x86: Key Differences Explained (phoenixNAP) - https://phoenixnap.com/kb/x64-vs-x86
