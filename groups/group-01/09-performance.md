# 9. Performance, Integration and Limitations

## 9.1 Hardware Integration
A integração do hardware do ENIAC ocorria por meio da conexão entre suas unidades de processamento, seus mecanismos de controle e seus dispositivos de armazenamento de dados e instruções. Diferentemente dos computadores modernos, que utilizam uma arquitetura integrada com memória eletrônica para armazenar programas e dados, o ENIAC dependia de módulos físicos especializados e de configurações manuais para executar suas operações.

**Processamento:** O processamento matemático era realizado por unidades especializadas, como os 20 acumuladores, responsáveis por operações de adição e subtração, e a unidade multiplicadora, destinada a cálculos de multiplicação. Essas unidades trabalhavam em conjunto, transmitindo valores numéricos por meio de conexões elétricas e pulsos eletrônicos.

**Controle:** O controle das operações era realizado principalmente pelas unidades de Cycling Unit e Master Programmer, responsáveis por coordenar a sequência de execução, sincronizar as operações e determinar quando as diferentes unidades deveriam atuar. A programação era configurada manualmente por meio de cabos, interruptores e painéis, estabelecendo os caminhos pelos quais os dados e os sinais de controle circulavam.

**Armazenamento:** Os resultados intermediários e os valores numéricos utilizados durante os cálculos eram mantidos principalmente nos acumuladores, que funcionavam como registradores de armazenamento temporário. Já os dados de entrada podiam ser fornecidos por meio de cartões perfurados, enquanto os resultados eram apresentados em dispositivos de saída, como perfuradoras de cartões e impressoras. A configuração do programa, por sua vez, dependia das conexões físicas e dos ajustes realizados pelos operadores.

**Integração entre os componentes:** O funcionamento conjunto dessas unidades dependia da coordenação entre os sinais de controle e a transferência de dados. O sistema de controle determinava a ordem das operações, os módulos de processamento realizavam os cálculos e os acumuladores mantinham os valores necessários para as etapas seguintes. Dessa forma, o ENIAC conseguia executar sequências complexas de operações matemáticas, embora sua integração dependesse de uma infraestrutura física extensa e de uma configuração manual relativamente trabalhosa.
Portanto, a integração do hardware do ENIAC representou um avanço importante na computação eletrônica, pois permitiu que diferentes unidades especializadas trabalhassem de maneira coordenada. Entretanto, a dependência de conexões físicas e a ausência de uma memória central para armazenar o programa limitavam a flexibilidade do sistema e tornavam a reconfiguração para novos problemas um processo demorado.

## 9.2 Computational Speed
O ENIAC foi projetado para ser uma máquina muito mais rápida que outras máquinas eletromecânicas de sua época. Porém, existia uma diferença no tempo de execução entre suas operações matemáticas: geralmente adições e subtrações eram realizadas muito mais rápidas que multiplicações, divisões e raíz quadrada.

Um artigo técnico publicado em 1946 por J. G. Brainerd e T. K. Sharpless [17], engenheiros envolvidos no desenvolvimento do ENIAC, apresenta os seguintes valores de desempenho:

| Operation | Execution Time | Operations per Second |
|---|---:|---:|
| Adição ou Subtração | 200 μs | 5.000 |
| Multiplicação de dois números de 10 dígitos | 2.800 μs (2,8 ms) | 360 |
| Consulta a uma função na Function Table | 1.000 μs (1 ms) | 1.000 |
> Fonte: artigo técnico The ENIAC, publicado em 1946.

Para compreender esses números:
- Adição e subtração: eram as operações mais rápidas. Uma adição podia ser concluída em 200 microssegundos, ou seja, 0,0002 segundo.
- Multiplicação: exigia aproximadamente 2,8 milissegundos, o equivalente a 14 ciclos básicos de adição.
- Consulta às Function Tables: levava aproximadamente 1 milissegundo para recuperar um valor armazenado nas tabelas de funções.

Uma publicação técnica de 1949 [18] registra que o ENIAC conseguia realizar aproximadamente 50 divisões ou extrações de raiz quadrada por segundo. Já um relatório do Office of Naval Research, referente ao período de 1951–1952 [16], registra cerca de 40 operações por segundo. Essa diferença é pontual: os valores podem variar conforme a configuração e as condições de operação da máquina. Portanto, é possível inferir que existia uma faixa aproximada de 40 a 50 operações por segundo para essas operações.

Além disso, outros documentos históricos registram que o desempenho podia mudar com modificações na configuração. Em 1952, por exemplo, um relatório [16] registrou taxas de 1.666 adições, 1.000 subtrações, 294 multiplicações e aproximadamente 40 divisões ou extrações de raiz quadrada por segundo.

## 9.3 Setup x Execution
- Levar dias para conectar os cabos de um programa que era executado em segundos

## 9.4 Energy Consumption
- Quanta energia o ENIAC usava para operar normalmente

## 9.5 Vacuum Tube Failure Rate
- Com que frequência as válvulas queimavam e quanto tempo era necessário para encontrar a defeituosa

## 9.6 I/O Bottleneck

A arquitetura do ENIAC criava um paradoxo de desempenho fascinante: a máquina era um colosso eletrônico capaz de operar na escala dos microssegundos, mas estava fisicamente acorrentada à lentidão de engrenagens, roletes e papel cartonado. Essa disparidade fundamental entre a velocidade de processamento interno e a velocidade de entrada e saída de dados gerava o maior gargalo de performance de todo o sistema.

### 9.6.1 Speed Disparity
Para compreender a gravidade desse gargalo, é necessário observar os números absolutos. Como detalhado no tópico 9.8.1, o ciclo básico da máquina levava 200 microssegundos, permitindo que um Acumulador realizasse 5.000 adições por segundo. Em contrapartida, os periféricos eletromecânicos da IBM operavam em um ritmo ditado pela física dos materiais:

- Input (Leitor de Cartões): Operava a uma taxa máxima de 125 cartões por minuto (aproximadamente 2 cartões por segundo).
- Output (Perfurador de Cartões): Era ainda mais lento, operando a uma taxa máxima de 100 cartões por minuto (cerca de 1,6 cartões por segundo), pois a ação mecânica de cravar lâminas de aço no papel exigia mais força e tempo do que simplesmente deslizar escovas elétricas sobre ele.

Isso significava que, no tempo necessário para que o Leitor de Cartões puxasse, lesse e ejetasse uma única folha de papel (cerca de 0,48 segundos), o ENIAC era capaz de executar mais de 2.400 ciclos de adição. Se um algoritmo fosse mal planejado e obrigasse a máquina a parar seus cálculos para esperar a leitura do próximo cartão, as válvulas eletrônicas ficariam ociosas por 99,9% do tempo, anulando completamente o propósito de se construir um computador eletrônico.

### 9.6.2 Algorithmic Mitigation
Para contornar essa limitação física, as equipes de programação precisavam estruturar o fluxo de dados de forma a isolar a máquina do mundo exterior pelo maior tempo possível. A regra de ouro da performance no ENIAC era: leia o mínimo possível, calcule o máximo possível internamente, e imprima apenas o estritamente necessário.

Foi exatamente para mitigar esse gargalo que unidades como o Constant Transmitter (com suas chaves rotativas manuais) e as Function Tables (Tabelas de Função) foram projetadas. Ao configurar as constantes matemáticas e as funções complexas diretamente no hardware através de interruptores e resistores, os programadores ancoravam os dados pesados dentro da própria máquina. Assim, o Leitor de Cartões era acionado apenas no primeiro segundo de execução para fornecer as variáveis iniciais de um problema. A partir desse ponto, o ENIAC entrava em um loop de processamento puramente eletrônico, rodando milhares de iterações em paralelo sem precisar interagir com o papel, até que o resultado final estivesse pronto para acionar o lento Perfurador de Cartões.

## 9.7 Suitability for Ballistics
O ENIAC não foi financiado e construído pelo Exército dos Estados Unidos apenas para ser uma máquina de calcular genérica. Seu desenvolvimento foi uma resposta direta a uma crise matemática militar: a necessidade de produzir Tabelas de Tiro (Tabelas Balísticas) para a artilharia durante a Segunda Guerra Mundial. A arquitetura descentralizada do ENIAC, incluindo a forma como lidava com o gargalo de I/O, era a tradução perfeita em hardware das equações diferenciais necessárias para resolver esse problema específico.

### 9.7.1 Atillery Trajectories
Calcular a trajetória de um projétil de artilharia não é uma simples equação de parábola de física de ensino médio. No mundo real, um projétil é afetado pela gravidade, pela densidade do ar (que muda conforme a altitude), pela velocidade do vento, pela rotação da Terra e, principalmente, pelo arrasto aerodinâmico (que varia drasticamente dependendo se o projétil está em velocidade subsônica ou supersônica).

Para descobrir onde uma bomba cairia, os matemáticos precisavam usar métodos de integração numérica (como o Método de Euler ou o Método de Heun). Isso envolvia quebrar o tempo de voo do projétil em frações minúsculas (ex: passos de 0,1 segundo). A equipe calculava a posição e a velocidade no tempo 0; usava esses resultados para calcular o arrasto e a nova posição no tempo 0,1; usava esses novos resultados para o tempo 0,2, e assim sucessivamente até que a altitude chegasse a zero (o impacto). Uma única trajetória exigia milhares de cálculos repetitivos em cascata.

### 9.7.2 Architectural Alignment
A arquitetura do ENIAC era um espelho exato desse método de integração numérica. O problema balístico era o cenário perfeito para o hardware da máquina pelos seguintes motivos:

- Paralelismo Físico: Como a posição de um projétil possui eixos X (distância) e Y (altitude), os cálculos de ambos os eixos precisavam ser atualizados a cada passo de tempo. O ENIAC podia usar um grupo de Acumuladores para processar a física do eixo X simultaneamente a outro grupo de Acumuladores processando a física do eixo Y. Isso cortava o tempo de execução da integração numérica pela metade.

- Hardcoding do Arrasto Aerodinâmico: A função de arrasto do ar era a parte mais complexa da equação, pois não era linear. O ENIAC possuía três Function Tables portáteis gigantescas. Os engenheiros mapeavam a curva de arrasto do ar diretamente nas centenas de interruptores dessas tabelas. Durante o voo simulado, a máquina consultava a tabela em velocidades eletrônicas para descobrir a resistência do ar na exata velocidade atual do projétil, sem precisar ler um único cartão perfurado para obter essa informação.

- A Simbiose com o Gargalo de I/O: O cálculo balístico era imune ao gargalo do Leitor de Cartões. Para calcular uma trajetória que durava 60 segundos de tempo de voo real, o ENIAC precisava ler apenas um ou dois cartões iniciais contendo o ângulo do canhão, a velocidade da pólvora e a densidade do ar local. A máquina então passava os próximos 20 segundos rodando loops internos furiosamente, calculando centenas de passos de tempo. O Perfurador de Cartões só era acionado de forma intermitente para registrar a posição do projétil a cada poucos segundos de voo simulado.

Em resumo, a arquitetura do ENIAC era perfeitamente adequada para a balística porque o problema exigia um volume massivo de iterações matemáticas internas e processamento paralelo, mas exigia uma quantidade minúscula de tráfego de entrada e saída, permitindo que a máquina operasse em sua velocidade eletrônica máxima sem ser estrangulada por seus periféricos mecânicos.

## 9.8 Architectural Decisions
O ENIAC foi desenvolvido para executar cálculos complexos em alta velocidade comparado às opções da época. Uma das suas funções era o processamento paralelo, que auxiliava justamente na contagem balística dos EUA na época em que foi lançado publicamente.
O computador possuía 20 acumuladores eletrônicos e cada um era capaz de armazenar números decimais de 10 dígitos com sinal (+ ou -), além de resolver operações de adição e subtração.
O paralelismo era especialmente útil no processamento de equações matemáticas, pois vários acumuladores podiam trabalhar simultaneamente em diferentes etapas de um mesmo problema.

## 9.8.1 Especificações técnicas da velocidade
Frequência dos pulsos de sincronização, 100 kHz
Intervalo entre os pulsos, 10 microssegundos
Ciclo básico de adição, 200 microssegundos
Velocidade de adição, até 5.000 adições por segundo, por unidade em funcionamento
Multiplicação de 2 números de 10 dígitos, aproximadamente 2,8 milissegundos

Os pulsos de sincronização eram comandados pela Cycling Unit, que comandava e coordenava todas as unidades independentes e ditava quando deveriam ser feitas as operações matemática, como fosse o "coração" do computador.


## 9.8.2 Como o paralelismo aumentava a velocidade?
O processamento paralelo permitia que vários acumuladores realizassem operações matemáticas ao mesmo tempo, desde que um não dependesse do resultado do outro.

Por exemplo, 4 acumuladores precisam realizar 4 operações diferentes:
Acum. 1: 100 + 200 = 300
Acum. 2: 500 + 200 = 700
Acum. 3: 300 - 120 = 180
Acum. 4: 600 + 150 = 750

Considerando os ciclos citados, sem o paralelismo seriam necessários 4 ciclos para realizar as 4 operações. Porém, com o paralelismo integrado, todas as 4 operações são realizadas em um mesmo ciclo de 200 microssegundos. Sendo assim, o paralelismo nesse caso evolui de 800 μs (4 ciclos de 200 μs) para 200 μs (1 ciclo). 

## 9.8.3 Dificuldade de configuração
Apesar da velocidade de processamento, a configuração do computador ainda era um processo complexo, demorado e sujeito a erros humanos.
Ao contrário dos computadores atuais, a configuração do ENIAC era física (com cabos, bandejas, cartões perfurados) e precisava ser configurado novamente para cada novo conjunto de cálculos.

## 9.8.4 Principais dificuldades técnicas
Conexões físicas: os cabos de programação e transmissão de dados precisavam ser alterados e reconfigurados a cada problema matemático, grandes alterações no algoritmo podiam exigir alterar diversas conexões.
Sincronização dos acumuladores: as operações paralelas não necessariamente finalizavam ao mesmo tempo, por isso era necessário coordenar os pulsos para que uma unidade não tentasse utilizar um resultado que ainda não estivesse pronto.
Identificação de erros: uma conexão incorreta poderia comprometer os resultados, assim era necessário verificar fisicamente as conexões e testar operações para conseguir identificar a raiz do erro.
Tempo de preparação: problemas mais complexos podiam exigir dias de configuração, mesmo sequer antes de iniciar a execução dos cálculos, pois tudo precisava estar de acordo antes de colocar o computador para funcionar. Segundo relatos históricos do governo dos EUA, a preparação de um problema de trajetória balística podia levar até 3 dias, enquanto a execução do problema podia ser finalizada em segundos.

## 9.8.5 Por que o paralelismo dificultava a programação (ainda mais)
A principal dificuldade do paralelismo, em relação à programação, era ter de organizar várias sequências de operações simultâneas.
No ENIAC, o programador precisava considerar quais unidades podiam trabalhar simultaneamente, quais resultados seriam compartilhados entre unidades e o momento certo em que a operação deveria começar. Assim, o processamento paralelo exigia ainda mais planejamento do que um processo sequencial, necessitando atenção detalhada nas conexões e fluxo de pulsos elétricos.

## 9.8.6 Relação entre a velocidade e o tempo de configuração
A relação entre ambos se assemelhava ao conceito do gargalo de Von Neumann. Enquanto tínhamos à disposição uma velocidade de processamento considerável, que realizava diversos cálculos complexos em segundos, a configuração e planejamento exigiam horas ou até dias de trabalho dos programadores.
Assim, a utilização do ENIAC era especialmente importante em problemas que exigiam cálculos repetitivos, como as tabelas balísticas dos EUA. 

![Cabos na Bandeja](./images/photos/lil-cables-in-tray.png)