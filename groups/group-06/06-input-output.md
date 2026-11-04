# 6. Operações de E/S (Entradas e Saídas)

## 6.1 Escopo da Entrada e Relação com Harvard
A Arquitetura de Harvard é um modelo de organização de computadores caracterizado pela separação entre as instruções de um programa e os dados utilizados durante sua execução. Essa separação permite que instruções e dados sejam armazenados em estruturas distintas e, nas implementações modernas, tenham caminhos de acesso independentes. Para compreender sua origem histórica, é importante conhecer o computador Harvard Mark I, desenvolvido por Howard Aiken em colaboração com a IBM e instalado na Universidade Harvard em 1944. O Mark I utilizava mecanismos físicos diferentes para fornecer instruções e dados à máquina. As instruções eram lidas por meio de uma fita de papel perfurada, enquanto os dados podiam ser introduzidos por cartões perfurados, fitas específicas ou interruptores para a configuração de valores numéricos. Dessa forma, a separação entre instruções e dados não dependia simplesmente dos periféricos de entrada, mas da maneira como a máquina organizava e processava cada tipo de informação. É importante destacar que a denominação Arquitetura de Harvard foi consolidada posteriormente e que a classificação histórica do Mark I como exemplo dessa arquitetura é discutida por pesquisadores.

## 6.2 Entrada do Programa
No contexto da Arquitetura de Harvard, a entrada do programa corresponde ao processo de fornecer à máquina as instruções que determinam quais operações devem ser executadas e em que sequência. No Harvard Mark I, esse processo começava com a preparação do programa pelos programadores, que precisavam transformar um problema matemático em uma sequência de instruções compatíveis com as operações da máquina. Depois, essas instruções eram codificadas em uma fita de papel perfurada. O mecanismo de controle de sequência realizava a leitura da fita, uma linha de cada vez, e acionava a operação correspondente. Após executar uma instrução, o sistema avançava para a seguinte, respeitando a sequência estabelecida. Diferentemente dos computadores modernos que armazenam programas em memórias eletrônicas, o Mark I recebia suas instruções por meio desse suporte físico externo. A fita podia ser preparada, corrigida e reutilizada, e sequências de instruções podiam ser armazenadas para resolver outros problemas. Esse método demonstra como as instruções eram representadas e fornecidas separadamente dos dados utilizados nos cálculos.


## 6.3 — Entrada dos Dados do Problema
Os dados numéricos eram fornecidos ao Harvard Mark I de forma totalmente segregada das instruções de controle, utilizando três mecanismos principais: cartões perfurados padrão IBM de 80 colunas para a entrada de variáveis e dados dinâmicos, leitores de fitas numéricas secundárias para tabelas de funções, e um painel com 60 conjuntos de chaves rotativas manuais para o armazenamento de constantes de até 24 dígitos. A preparação dessas informações ocorria antes da execução, quando a equipe de programadores e operadores — que incluía nomes como Grace Hopper — perfurava previamente os dados em cartões usando máquinas keypunches e ajustava manualmente cada chave de constante no painel. Durante a operação, os técnicos carregavam os lotes de cartões nos leitores mecânicos para que os dados fossem alimentados automaticamente nos registradores sob demanda, mantendo uma separação rígida em relação à fita principal de controle de 24 canais, a qual continha exclusivamente a lógica de programação.

## 6.4 — Produção e obtenção dos resultados
Nos primórdios da arquitetura Harvard, personificada pelo computador Harvard Mark I na década de 1940, o processamento dos dados ocorria por meio de relés eletromecânicos e engrenagens. Como as instruções de controle e os dados numéricos viajavam por caminhos totalmente separados, a unidade central realizava os cálculos aritméticos de forma isolada e retinha os resultados temporariamente em registradores mecânicos de acumulação, aguardando o comando para a sua transmissão externa.
Uma vez concluído o processamento, o resultado era produzido e materializado por meio de impulsos elétricos enviados a dispositivos periféricos específicos. A máquina controlava diretamente máquinas de escrever automáticas da IBM, que digitavam os números de forma autônoma em longas folhas de papel, ou acionava perfuradoras de cartões, que traduziam os dados numéricos em furos físicos gravados em cartões de cartolina.
Finalmente, os cientistas e operadores humanos obtinham esses resultados de forma estritamente física, destacando as folhas impressas para leitura direta e publicação de tabelas matemáticas, ou recolhendo as pilhas de cartões perfurados das gavetas de saída para armazenamento ou reinserção em futuros cálculos. Vale destacar que esse fluxo mecânico era uma característica da tecnologia da época e não deve ser generalizado para a arquitetura Harvard como um todo, já que sistemas modernos baseados nesse mesmo conceito utilizam saídas totalmente digitais e eletrônicas.


## 6.5 Mecanismos de Saída e Obtenção dos Resultados
A organização Harvard não determina quais dispositivos de saída um computador deve usar; isso depende da implementação. No Harvard Mark I, os resultados podiam ser registrados em cartões perfurados ou apresentados como números impressos por máquinas de escrever. A documentação da coleção de instrumentos científicos de Harvard descreve essas duas formas de saída, e o Smithsonian identifica uma máquina de escrever elétrica como equipamento de saída do Mark I.
Na saída impressa, os números ficavam diretamente legíveis para as pessoas. Os operadores recolhiam os resultados impressos ao final do processo. Assim, o Mark I podia transformar o resultado do cálculo em um registro físico, destinado à leitura humana ou à manipulação em cartões.

## 6.6 — Participação Humana e Fluxo Integrado
Nas primeiras implementações operacionais baseadas na Arquitetura Harvard (como o computador Harvard Mark I), a interação com o sistema dependia fundamentalmente do trabalho manual direto exercido por operadores e programadores. A comunicação entre o ambiente externo e a máquina exigia um fluxo integrado composto por várias etapas físicas e sequenciais:

1. **Preparação de Mídias e Programação:** Programadores e operadores codificavam as instruções e tabelas numéricas perfurando fita de papel ou cartões perfurados.
2. **Configuração Inicial do Sistema:** Antes da execução, o operador devia configurar manualmente chaves e comutadores no painel da máquina para definir parâmetros iniciais de cálculo e registradores.
3. **Alimentação de Dados e Instruções:** O operador inseria as fitas perfuradas de programa nos leitores de instrução e as fitas de dados nos leitores de dados.
4. **Execução e Monitoramento:** Durante a computação, o operador acompanhava os sinais visuais do painel e mantinha a máquina alimentada continuamente.
5. **Obtenção dos Resultados:** Os resultados processados eram direcionados para máquinas de escrever elétricas ou perfuradoras de cartão, de onde o operador retirava fisicamente as impressões e relatórios finais.

'''mermaid
graph LR
    subgraph Entrada ["1. Entrada / Preparação Manual"]
        A[Operador / Programador] -->|Perfuração de Mídias| B[Fitas e Cartões Perfurados]
        A -->|Configuração Manual| C[Chaves e Painel de Controle]
    end

    subgraph Processamento ["2. Processamento e Interação"]
        B -->|Leitura de Instruções e Dados| D[CPU / Unidades da Arquitetura Harvard]
        C -->|Sinais de Parâmetros| D
    end

    subgraph Saida ["3. Saída e Extração"]
        D -->|Sinais de Resultado| E[Impressoras / Máquinas de Escrever / Perfuradoras]
        E -->|Coleta de Resultados Impressos| F[Operador / Usuário Final]
    end

    style Entrada fill:#f9f9f9,stroke:#333,stroke-width:2px
    style Processamento fill:#e1f5fe,stroke:#0288d1,stroke-width:2px
    style Saida fill:#e8f5e9,stroke:#388e3c,stroke-width:2px

    Legenda do Fluxo:

    Entrada Manual: Ação humana direta no preparo de mídias físicas (fitas/cartões) e ajuste de chaves.

    Processamento: Leitura independente e execução das instruções e dados pela CPU.

    Saída: Impressão ou gravação física do resultado retido e analisado pelo operador.


## 6.7 Comparação Moderna
A diferença central em relação a E/S é que a Arquitetura de Harvard clássica mantém barramentos totalmente independentes para instruções e dados, garantindo que o acesso a periféricos (via barramento de dados) nunca dispute caminho com a busca de instruções. Já a Arquitetura Harvard moderna (modificada) preserva a separação apenas no nível de cache e trata a E/S quase sempre como memória mapeada dentro do espaço de dados, compartilhando o barramento de memória principal. Assim, ambos os modelos oferecem bom desempenho para E/S, mas a Harvard clássica é mais “pura” na separação física, enquanto a moderna privilegia a flexibilidade e a compatibilidade com sistemas operacionais e carregamento dinâmico de programas.


