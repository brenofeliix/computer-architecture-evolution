# ENTREGA 02 — Organização e Comunicação do Sistema

## Principais Componentes e Unidades
- Unidade de Controle (UC): Interpreta as instruções e coordena sua execução.

- Unidade Lógica e Aritmética (ULA / ALU) Avançada: Aplicação direta. Projetada para suportar operações complexas diretamente no hardware (como manipulação de strings, operações aritméticas diretamente na memória, etc.).

- Banco de Registradores: Armazenam temporariamente dados, endereços e resultados. Aplicação direta. Possui um número menor de registradores de propósito geral em comparação ao RISC, focando em registradores especializados (acumuladores, registradores de índice, ponteiros de pilha).

- Unidade de Gerenciamento de Memória (MMU) e Cache: Equivalente técnico/moderno. Nos processadores CISC contemporâneos (como x86-64), há caches L1, L2 e L3 altamente complexas para mitigar o gargalo de acesso à memória principal.

- Memória principal (RAM): A memória RAM armazena programas e dados em uso. A memória cache mantém cópias de informações acessadas frequentemente, reduzindo o tempo médio de acesso.

- Entrada e saída (E/S): Os mecanismos de entrada e saída permitem a comunicação com dispositivos externos, como teclado, mouse, monitor, armazenamento e interfaces de rede. Tendo uma aplicação direta, que possibilita que o computador receba informações e apresente ou transmita resultados.

- Barramentos e interconexões: Transportam dados, endereços e sinais de controle entre os componentes.

---
## Organização e Conexões
Em um sistema baseado em CISC, os componentes são organizados para permitir a busca(fetch), a decodificação(decode) e a execução das instruções(execute), além da transferência de dados entre registradores, memória e unidades de processamento.

**Caminho de dados (Datapath)**
O datapath é o conjunto de componentes e conexões por onde os dados circulam durante o processamento. Ele inclui registradores, unidades de execução, caminhos de transferência e outros elementos internos.

Na arquitetura CISC, esse caminho precisa suportar diferentes tipos de instruções e operandos, inclusive operações que acessam a memória diretamente.

**Organização tradicional e microprogramação**
Em implementações CISC tradicionais, uma instrução complexa pode acionar uma sequência de microinstruções que coordenam operações internas menores. Isso permite implementar instruções mais elaboradas sem precisar construir um circuito independente para cada uma.

O fluxo simplificado é:
1 - A instrução de máquina é buscada na memória.
2 - A Unidade de Controle identifica e decodifica a instrução.
3 - Se necessário, uma sequência de microinstruções coordena as operações internas.
4 - Os dados são transferidos para os registradores ou unidades de execução.
5 - O resultado é produzido e, quando necessário, armazenado na memória.

---
## Mecanismos de Comunicação e Barramentos
Na arquitetura CISC, os componentes se comunicam por meio de barramentos e interconexões internas. Os principais são:
- *Barramento de dados*: Transporta dados e resultados.
- *Barramento de endereços*: Identifica as posições de memória acessadas.
- *Barramento de controle*: Transmite sinais que coordenam operações.
- *Interconexões internas*: Conectam registradores, unidades de execução e caches.
- *Interfaces de entrada e saída*: Permitem a comunicação com periféricos.
Nos processadores modernos, também existem conexões internas mais sofisticadas, que variam conforme o modelo.

*Acessos Múltiplos à Memória*: Como uma única instrução CISC pode ler um operando da memória, realizar uma operação aritmética e gravar o resultado de volta na memória, o barramento de dados sofre intensa atividade por instrução executada.

## Limitações desta Organização
Atrasos de comunicação: A transferência de dados entre processador e memória pode ser relativamente lenta.

Complexidade do Decodificador: O circuito necessário para decodificar instruções de tamanho variável e gerenciar o microcódigo ocupa mais espaço no silício e consome mais energia térmica.

Gargalo de Desempenho (Memory Wall): A dependência de acessos frequentes à memória para buscar operandos complexos pode desacelerar o processamento se não houver hierarquia de cache robusta.

Ineficiência de Pipeline: Devido à variabilidade no tamanho e no tempo de execução das instruções, implementar pipelines profundos e eficientes é consideravelmente mais complexo do que em arquiteturas reduzidas (RISC).

---
##  Diagrama do CISC mostrando os principais componentes e suas conexões(Não consegui colar imagem no GitHub).



                    ARQUITETURA CISC
             Principais componentes e conexões


   +------------------------------------------------+
   |              MEMÓRIA PRINCIPAL (RAM)            |
   |                                                |
   |       Armazena programas, instruções e dados    |
   +------------------------+-----------------------+
                            |
                            |
   ==================================================
       BARRAMENTOS DE DADOS, ENDEREÇOS E CONTROLE
   ==================================================
        |                   |                   |
        | Dados             | Endereços         | Controle
        |                   |                   |
   +----v-------------------------------------------v----+
   |                  PROCESSADOR CISC                   |
   |                                                     |
   |  +-----------------------------------------------+  |
   |  |             UNIDADE DE CONTROLE (UC)          |  |
   |  |                                               |  |
   |  |  • Busca e decodifica instruções              |  |
   |  |  • Coordena a execução das operações         |  |
   |  |  • Pode utilizar microprogramação             |  |
   |  +----------------------+------------------------+  |
   |                         |                           |
   |              Interconexões internas                |
   |                         |                           |
   |       +-----------------+----------------+          |
   |       |                 |                |          |
   |  +----v--------+  +-----v------+  +------v-------+  |
   |  | BANCO DE    |  | ULA / ALU  |  | CACHE E MMU |  |
   |  | REGISTRADORES|  |            |  |             |  |
   |  |             |  | Operações  |  | Cache:      |  |
   |  | Armazena    |  | aritméticas|  | acelera o   |  |
   |  | dados,      |  | e lógicas  |  | acesso à    |  |
   |  | endereços e |  |            |  | memória     |  |
   |  | resultados  |  |            |  |             |  |
   |  +------+------+  +------+-----+  | MMU:        |  |
   |         |                |        | gerencia    |  |
   |         +----------------+        | endereços e |  |
   |          Troca de dados           | proteção    |  |
   |                                   +------+------+\ |
   |                                          |        |
   |  +---------------------------------------v-----+  |
   |  |          INTERCONEXÕES INTERNAS             |  |
   |  | Conectam registradores, ULA, cache e demais  |  |
   |  | unidades do processador.                    |  |
   |  +---------------------------------------------+  |
   +-------------------------+---------------------------+
                             |
                             |
                Barramentos e interfaces de E/S
                             |
           +-----------------v-----------------+
           |  CONTROLADORES DE ENTRADA E SAÍDA  |
           |                                    |
           |  Gerenciam a comunicação com os    |
           |  dispositivos periféricos          |
           +-----------------+------------------+
                             |
             +---------------+---------------+
             |               |               |
        +----v----+     +----v----+     +----v--------+
        | TECLADO |     | MONITOR |     | ARMAZENAMENTO|
        +---------+     +---------+     +-------------+
                                              |
                                        +-----v-----+
                                        | REDE /    |
                                        | OUTROS    |
                                        | PERIFÉRICOS|
                                        +-----------+


             FLUXO GERAL DE FUNCIONAMENTO

  1. BUSCA:
     A instrução é obtida da memória.

  2. DECODIFICAÇÃO:
     A Unidade de Controle interpreta a instrução.

  3. EXECUÇÃO:
     Os registradores fornecem dados e a ULA ou outra
     unidade realiza a operação solicitada.

  4. ARMAZENAMENTO:
     O resultado é guardado em registradores ou memória.

  5. ENTRADA E SAÍDA:
     Quando necessário, os resultados são enviados
     aos periféricos por controladores e interfaces.
