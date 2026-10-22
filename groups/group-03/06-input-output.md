# ENTREGA 03 — Entrada, Saída e Interação com o Ambiente

**Grupo:** 03 — Estudo da Arquitetura MISD

## 1. Introdução

Esta etapa tem como objetivo investigar os mecanismos de entrada e saída de informações e a interação entre os componentes físicos e lógicos do protótipo de monitoramento de temperatura. O estudo relaciona o funcionamento do sistema aos conceitos de Arquitetura de Computadores, considerando a maneira como os dados são coletados, transmitidos, processados, armazenados e apresentados ao usuário.

O projeto propõe a utilização de um sensor de temperatura DS18B20, um microcontrolador ESP32, uma conexão Wi-Fi e um programa desenvolvido em Python. Cada componente possui uma função específica dentro do sistema. O sensor é responsável por medir a temperatura do ambiente, o ESP32 realiza a aquisição e a preparação dos dados, a conexão Wi-Fi permite a comunicação com o computador e o Python executa as análises programadas.

A investigação também considera como os programas e as configurações são inseridos nos dispositivos, quais interfaces permitem a comunicação entre os componentes, como ocorre a participação humana e quais limitações podem interferir no funcionamento do protótipo.

Além disso, o estudo permite compreender que um sistema computacional não depende apenas do processamento de informações. Ele também precisa de mecanismos adequados para receber dados externos, comunicar-se com outros dispositivos e apresentar resultados compreensíveis.

É importante destacar que o projeto possui finalidade acadêmica. A utilização de diferentes análises sobre os mesmos dados permite discutir o conceito MISD (*Multiple Instruction, Single Data*), mas não comprova, isoladamente, a implementação física dessa arquitetura. Para afirmar que existe uma arquitetura MISD real, seria necessário demonstrar tecnicamente como múltiplos fluxos de instruções operam sobre um único fluxo de dados.

## 2. Entrada de informações no sistema

A entrada de informações corresponde ao processo pelo qual um sistema computacional recebe dados provenientes do ambiente externo ou de outros dispositivos. Esses dados podem representar valores físicos, comandos, configurações ou informações armazenadas anteriormente.

No protótipo proposto, a principal entrada é a temperatura medida pelo sensor DS18B20. Essa informação representa uma grandeza física do ambiente que precisa ser convertida em um formato que os componentes eletrônicos e os programas consigam interpretar.

A entrada de dados é uma etapa fundamental, pois a qualidade das análises realizadas posteriormente depende da qualidade e da validade das informações recebidas. Se uma medição estiver incorreta, incompleta ou não chegar ao programa, os resultados poderão ser comprometidos.

### 2.1. Coleta de temperatura pelo sensor DS18B20

O DS18B20 é um sensor digital de temperatura que realiza medições e disponibiliza os valores por meio de comunicação digital. Diferentemente de um sensor que fornece apenas um sinal analógico para ser interpretado por um conversor externo, o DS18B20 possui recursos internos para converter a medição em um valor digital.

O sensor utiliza o protocolo 1-Wire para comunicar-se com o dispositivo controlador. Essa comunicação permite que o ESP32 solicite e receba as informações de temperatura por meio de uma interface elétrica apropriada.

O processo começa quando o sensor é colocado em contato térmico com o ambiente ou com o objeto que se deseja monitorar. Após realizar a medição, o componente disponibiliza o valor digital para que o ESP32 possa obtê-lo de acordo com as instruções do programa embarcado.

Para que a coleta funcione corretamente, é necessário que o sensor esteja conectado de maneira adequada, receba a alimentação necessária e seja reconhecido pelo programa executado no microcontrolador.

A leitura pode ocorrer repetidamente, conforme o intervalo definido na programação. Por exemplo, o sistema pode ser configurado para solicitar novas medições em intervalos regulares. O intervalo exato dependerá da implementação adotada pelo grupo.

O sensor funciona, portanto, como uma interface entre o ambiente físico e o sistema computacional. Ele transforma uma grandeza física, a temperatura, em uma informação digital que poderá ser transmitida e analisada.

### 2.2. Tipos de dados recebidos

A informação principal recebida pelo sistema é o valor numérico da temperatura, normalmente expresso em graus Celsius. Entretanto, para organizar o monitoramento, o programa pode trabalhar com outras informações associadas a cada medição.

Os dados utilizados podem incluir:

- **Valor da temperatura:** representa a medição obtida pelo sensor.
- **Data e horário da medição:** indicam quando o valor foi coletado, caso o sistema implemente esse recurso.
- **Identificação da medição:** permite diferenciar os registros por meio de números sequenciais ou identificadores.
- **Estado da leitura:** indica se o valor recebido passou pelas verificações de validade implementadas.
- **Informações de comunicação:** podem indicar a origem da mensagem ou auxiliar na identificação de problemas durante a transmissão.

Nem todas essas informações são fornecidas diretamente pelo sensor. O DS18B20 fornece a leitura da temperatura, enquanto outros dados podem ser acrescentados pelo ESP32 ou pelo programa em Python.

Por exemplo, o computador pode associar um horário à medição quando recebe a mensagem. Da mesma maneira, o programa pode gerar um número sequencial para organizar os registros.

Essa organização é importante porque permite diferenciar uma medição isolada de um conjunto de registros utilizados para acompanhar a evolução da temperatura ao longo do tempo.

### 2.3. Entrada automática e entrada manual

O protótipo pode combinar mecanismos de entrada automática e manual.

A **entrada automática** ocorre quando o sistema realiza a coleta e a transmissão das medições sem exigir que uma pessoa digite cada valor. Depois que os componentes e os programas são configurados, o ESP32 pode obter as leituras do sensor e encaminhá-las ao computador de acordo com a lógica implementada.

A **entrada manual** ocorre quando o usuário fornece informações ou realiza ações necessárias ao funcionamento do sistema. Entre essas ações estão a definição de limites de temperatura, a alteração de parâmetros, a inicialização dos programas e a consulta aos resultados.

Também pode haver entrada manual durante os testes, quando o grupo utiliza valores simulados para verificar se as regras de análise funcionam corretamente. Esses valores de teste devem ser diferenciados das medições reais fornecidas pelo sensor.

A combinação desses mecanismos demonstra que a automação não elimina a participação humana. O sistema pode executar as tarefas repetitivas, enquanto o usuário define os parâmetros, acompanha os resultados e intervém quando necessário.

## 3. Inserção de programas, instruções e configurações

Para que o protótipo funcione, é necessário fornecer instruções aos dispositivos responsáveis pela coleta e pelo processamento das informações. Essas instruções determinam quais operações devem ser realizadas, em que condições devem ocorrer e como os dados precisam ser tratados.

A programação permite transformar os componentes físicos em um sistema capaz de executar tarefas específicas. Sem as instruções adequadas, o sensor pode estar conectado ao microcontrolador, mas o sistema não necessariamente realizará a coleta, a transmissão ou a análise desejada.

### 3.1. Programação do ESP32

O ESP32 é um microcontrolador que executa um programa embarcado. Esse programa pode ser desenvolvido em um computador utilizando um ambiente de desenvolvimento compatível e, posteriormente, transferido para o dispositivo.

Depois que o programa é gravado e iniciado, o microcontrolador executa as instruções previstas. Essas instruções podem determinar como o sensor será inicializado, quando as medições serão solicitadas e de que maneira os dados serão enviados ao computador.

Entre as operações que podem ser programadas estão:

- Inicializar os componentes e as interfaces utilizados.
- Configurar a comunicação com o sensor DS18B20.
- Solicitar novas medições de temperatura.
- Verificar se a leitura foi obtida corretamente.
- Preparar os dados em um formato definido pelo projeto.
- Estabelecer ou utilizar uma conexão Wi-Fi.
- Transmitir as informações ao computador.
- Repetir o processo de coleta conforme o intervalo configurado.
- Tratar situações em que o sensor ou a comunicação não funcionem como esperado.

O programa embarcado define o comportamento do ESP32 durante a execução. Por isso, alterações no código podem modificar o intervalo de coleta, a forma de organização das mensagens e os procedimentos adotados quando ocorrem falhas.

O funcionamento também depende das condições físicas do sistema. Mesmo que o programa esteja correto, uma conexão inadequada, a falta de alimentação ou um problema no sensor poderá impedir a coleta das informações.

### 3.2. Programação em Python

No computador, o programa em Python é responsável por receber e analisar os dados transmitidos pelo ESP32. Ele interpreta as mensagens recebidas e executa as regras definidas pelo grupo para produzir informações úteis ao monitoramento.

O programa pode utilizar variáveis para armazenar temporariamente os valores, instruções condicionais para verificar limites, operações matemáticas para comparar medições e mecanismos de armazenamento para manter um histórico.

As instruções podem ser organizadas em diferentes etapas. Inicialmente, o programa precisa receber a mensagem enviada pelo ESP32. Em seguida, deve interpretar seu conteúdo, verificar se o valor possui um formato esperado e determinar quais análises serão realizadas.

Após essas verificações, o Python pode comparar a temperatura com os limites configurados, consultar registros anteriores, aplicar critérios para identificar possíveis anomalias e apresentar os resultados.

A linguagem Python permite implementar essas operações utilizando instruções executadas pelo processador do computador. O comportamento do programa depende da lógica desenvolvida, dos parâmetros utilizados e da maneira como as situações inesperadas são tratadas.

Também é necessário distinguir a execução de um programa de sua programação. A programação corresponde à criação e à organização das instruções; a execução corresponde ao momento em que o computador interpreta ou executa essas instruções para realizar as tarefas previstas.

### 3.3. Configurações do sistema

Além dos programas, o funcionamento depende de configurações que estabelecem os parâmetros utilizados durante a execução.

Entre as configurações possíveis estão:

- Intervalo entre as medições.
- Parâmetros necessários para a conexão Wi-Fi.
- Endereço e mecanismo utilizados para enviar os dados ao computador.
- Formato das mensagens transmitidas.
- Limites mínimo e máximo de temperatura, quando aplicáveis.
- Critérios para comparar a medição atual com o histórico.
- Regras utilizadas para identificar possíveis anomalias.
- Local e formato de armazenamento dos registros.
- Mensagens apresentadas quando ocorre uma falha.

Esses parâmetros podem ser definidos diretamente no código ou em arquivos específicos, dependendo da organização escolhida pelo grupo.

É importante diferenciar instruções e parâmetros. As instruções determinam as operações realizadas pelo sistema, enquanto os parâmetros fornecem valores e condições que essas operações utilizam.

Por exemplo, uma instrução pode determinar que o programa compare a temperatura com um limite superior. O valor de 35 °C pode ser utilizado como parâmetro dessa comparação.

Essa separação facilita a manutenção do sistema, pois permite modificar determinados valores sem necessariamente alterar toda a lógica do programa.

## 4. Processamento das informações

O processamento corresponde à etapa em que os dados recebidos são interpretados e submetidos às instruções dos programas. No protótipo, existem operações realizadas no ESP32 e outras executadas no computador.

O processamento no microcontrolador está relacionado principalmente à obtenção, à organização e à transmissão das medições. Já o processamento no computador está relacionado à interpretação dos valores recebidos e à aplicação das análises definidas pelo grupo.

### 4.1. Processamento realizado pelo ESP32

O ESP32 executa as instruções responsáveis pela comunicação com o sensor e pela preparação dos dados. Ele atua como intermediário entre a medição física e a aplicação de software executada no computador.

Após obter uma leitura do DS18B20, o microcontrolador pode organizar o valor em uma mensagem com formato definido pelo projeto. Essa mensagem precisa ser estruturada de maneira que o programa em Python consiga identificar e interpretar seu conteúdo.

Por exemplo, o ESP32 pode enviar uma mensagem contendo um campo que identifica a temperatura e outro que contém o valor medido. O formato efetivo dependerá do mecanismo de comunicação implementado.

A organização correta é importante porque o receptor precisa conhecer a estrutura da mensagem. Se o ESP32 enviar informações em um formato diferente daquele esperado pelo Python, o programa poderá interpretar o conteúdo incorretamente ou rejeitar a mensagem.

O microcontrolador também precisa lidar com as operações relacionadas à conexão. Caso a comunicação não esteja disponível, o programa deve adotar um comportamento definido, como tentar restabelecer a conexão ou indicar que a transmissão não foi concluída.

### 4.2. Processamento realizado pelo computador

O computador recebe os dados transmitidos pelo ESP32 e executa as instruções do programa em Python. Nessa etapa, o valor da temperatura pode ser submetido a diferentes operações de análise.

Uma primeira operação consiste em verificar se a mensagem recebida está no formato esperado. Essa verificação ajuda a evitar que dados incompletos ou inválidos sejam tratados como medições confiáveis.

Depois de validar a mensagem, o programa pode realizar as análises previstas no projeto.

**Verificação de limites de temperatura**

A verificação de limites compara o valor recebido com parâmetros previamente estabelecidos. O objetivo é identificar se a temperatura está dentro da faixa configurada ou se ultrapassou algum limite.

Por exemplo, se o limite superior estiver definido em 35 °C e o programa receber uma medição válida de 38 °C, a comparação poderá identificar que o valor está acima do limite.

O resultado depende da regra programada. O grupo precisa definir quais condições serão utilizadas e qual mensagem será apresentada quando uma delas for atendida.

**Comparação com o histórico**

A comparação com o histórico utiliza registros anteriores para analisar as mudanças da temperatura ao longo do tempo. Essa operação pode ajudar a identificar aumentos, reduções e variações entre medições.

Por exemplo, uma leitura atual pode ser comparada com uma medição anterior para calcular a diferença entre os dois valores. Também é possível observar uma sequência de registros para compreender se a temperatura apresenta uma tendência de aumento ou redução.

Essa análise depende da existência de um histórico armazenado e de uma organização adequada dos registros. Sem dados anteriores disponíveis, o programa não consegue realizar uma comparação histórica completa.

**Identificação de possíveis anomalias**

A identificação de anomalias procura reconhecer valores ou comportamentos que se afastem dos critérios considerados esperados pelo sistema.

O programa pode utilizar limites, diferenças entre medições ou regras baseadas no histórico para indicar possíveis situações incomuns.

Entretanto, uma variação não representa necessariamente uma falha. Uma mudança de temperatura pode ser normal dependendo das condições do ambiente monitorado. Por esse motivo, os critérios devem ser definidos de maneira coerente com o objetivo do projeto.

Essas três análises possuem finalidades diferentes. A verificação de limites observa condições previamente estabelecidas; a comparação histórica considera medições anteriores; e a identificação de anomalias utiliza critérios para reconhecer comportamentos incomuns.

### 4.3. Relação entre o processamento e o conceito MISD

MISD significa *Multiple Instruction, Single Data*, traduzido como múltiplas instruções e um único fluxo de dados. Na taxonomia de Flynn, essa classificação considera a organização dos fluxos de instruções e de dados em sistemas computacionais.

No contexto do projeto, o mesmo valor de temperatura pode ser utilizado como entrada para diferentes análises. Uma medição, por exemplo, pode ser verificada em relação a um limite, comparada com registros anteriores e examinada por regras de identificação de anomalias.

Essa abordagem oferece ao grupo uma oportunidade de estudar como diferentes operações podem produzir resultados distintos a partir de uma mesma informação de entrada.

Entretanto, é necessário manter a precisão técnica. Executar várias funções em sequência dentro de um único programa Python não caracteriza, por si só, uma arquitetura física MISD. Da mesma forma, a existência de um sensor, um ESP32 e um computador não comprova que o sistema possui múltiplos fluxos de instruções operando sobre um único fluxo de dados na organização prevista pela taxonomia.

Portanto, o protótipo deve ser apresentado como uma aplicação acadêmica de monitoramento de temperatura que permite discutir o conceito MISD e a utilização de diferentes análises sobre os mesmos dados, sem afirmar uma implementação física que ainda não tenha sido demonstrada.

## 5. Produção e apresentação dos resultados

A saída corresponde às informações produzidas pelo sistema após o processamento dos dados. No protótipo, os resultados são gerados pelo programa em Python e podem ser apresentados por meio do terminal ou de outra interface que venha a ser implementada.

A saída é importante porque transforma os dados processados em informações que podem ser compreendidas pelo usuário. Uma temperatura isolada representa um valor numérico, enquanto uma mensagem que informa que esse valor ultrapassou determinado limite fornece uma interpretação baseada nas regras do programa.

### 5.1. Tipos de resultados produzidos

O programa pode apresentar diferentes resultados, de acordo com as funcionalidades implementadas:

- Temperatura atual recebida do sensor.
- Indicação de que a medição está dentro dos limites configurados.
- Aviso de que a temperatura ultrapassou um limite.
- Diferença entre a temperatura atual e uma medição anterior.
- Resumo das variações observadas no histórico.
- Identificação de valores que atendam aos critérios de possível anomalia.
- Mensagens que informem falhas na recepção ou na interpretação dos dados.
- Indicação de que não foi possível obter uma nova medição.

Esses resultados não são produzidos automaticamente apenas pela presença do sensor e do computador. Cada funcionalidade depende das instruções correspondentes no programa.

Também é necessário diferenciar a ausência de uma nova medição de uma temperatura sem alterações. Se o sistema deixar de receber dados, não se pode concluir que a temperatura permaneceu estável. O programa deve considerar a possibilidade de falha de comunicação ou interrupção da coleta.

### 5.2. Exemplo de resultado

Considere que o programa esteja configurado com um limite superior de 35 °C e receba uma medição válida de 38 °C.

O programa compara o valor recebido com o limite estabelecido. Como 38 °C é superior a 35 °C, a condição programada é atendida e o sistema pode apresentar uma mensagem informando que a temperatura ultrapassou o limite.

A saída ilustrativa poderia ser:

```text
Temperatura atual: 38 °C
Limite superior configurado: 35 °C
Situação: temperatura acima do limite
```

Esse exemplo demonstra como um valor físico coletado pelo sensor pode ser transformado em uma informação interpretável pelo usuário.

A mensagem não significa necessariamente que o ambiente esteja em situação de perigo. Ela indica apenas que a medição ultrapassou o parâmetro configurado. A interpretação da gravidade depende do contexto de uso e dos limites apropriados ao ambiente monitorado.

### 5.3. Saída informacional e saída física

É necessário distinguir a apresentação de informações da atuação direta sobre o ambiente.

No protótipo descrito, a saída principal é informacional. O programa apresenta temperaturas, comparações e possíveis alertas por meio de uma interface de software.

Não está prevista, na proposta atual, a necessidade de acionar motores, relés, ventiladores ou outros dispositivos físicos. Portanto, não se deve afirmar que o sistema realiza ações automáticas sobre o ambiente enquanto essa funcionalidade não tiver sido implementada e testada.

Em uma versão futura, poderiam ser acrescentados dispositivos capazes de executar ações físicas a partir dos resultados do processamento. Essa ampliação exigiria novos componentes, instruções específicas e testes para verificar se as ações são realizadas de maneira correta.

## 6. Dispositivos, mecanismos e interfaces externas

A interação entre o ambiente físico e o sistema computacional depende de dispositivos e interfaces responsáveis pela entrada, pela transmissão e pela saída de informações.

Cada componente possui características próprias e precisa funcionar de maneira compatível com os demais. Uma falha em qualquer etapa pode comprometer o resultado final, mesmo quando as outras partes estão funcionando corretamente.

### 6.1. Sensor DS18B20

O DS18B20 é responsável pela medição da temperatura. Sua interface digital utiliza o protocolo 1-Wire, que permite a comunicação com o controlador por uma linha de dados, além das conexões elétricas necessárias para alimentação e referência elétrica.

No projeto, o sensor representa a principal interface entre o ambiente físico e o sistema computacional. Ele fornece os valores que serão utilizados nas análises posteriores.

A qualidade das informações depende das características do componente, das condições de instalação e da adequação da medição ao ambiente monitorado. Problemas de conexão ou condições inadequadas de operação podem comprometer a confiabilidade dos dados.

Também é importante verificar se o programa consegue reconhecer o sensor e obter as leituras corretamente antes de iniciar a transmissão das informações.

### 6.2. Microcontrolador ESP32

O ESP32 é responsável por executar o programa embarcado, obter as leituras do sensor e preparar os dados para transmissão. O dispositivo possui recursos de processamento, memória e comunicação que permitem integrar diferentes componentes do protótipo.

No projeto, o ESP32 funciona como um ponto de integração entre a medição física e o programa executado no computador.

Sua função não se limita a encaminhar os dados. O microcontrolador também precisa executar as instruções necessárias para estabelecer a comunicação com o sensor, organizar as mensagens e utilizar a conexão Wi-Fi conforme a implementação escolhida.

O funcionamento depende da alimentação elétrica, da programação e da configuração correta das interfaces utilizadas.

### 6.3. Comunicação Wi-Fi

A conexão Wi-Fi permite transmitir as informações do ESP32 ao computador sem a necessidade de uma conexão física direta entre os dois dispositivos para o envio dos dados.

Para que a comunicação funcione, os componentes precisam utilizar configurações compatíveis e um mecanismo de troca de mensagens definido pelo projeto.

A conexão Wi-Fi, por si só, não determina como os dados serão enviados ou recebidos. É necessário implementar um mecanismo de comunicação, como uma conexão TCP ou uma interface HTTP, conforme a solução escolhida pelo grupo.

A escolha do mecanismo influencia a forma de estabelecer a conexão, organizar as mensagens e tratar eventuais falhas.

Também é necessário considerar que a comunicação sem fio pode ser afetada pela disponibilidade da rede, pela intensidade do sinal e por interferências. Por isso, o programa deve considerar situações em que os dados não cheguem ao destino esperado.

### 6.4. Computador e terminal

O computador executa o programa em Python, recebe as mensagens transmitidas pelo ESP32 e realiza as análises definidas pelo grupo.

O terminal pode ser utilizado como uma interface de saída para apresentar as temperaturas, os resultados das comparações e as mensagens de aviso. Essa solução é adequada para demonstrar o funcionamento básico do protótipo, pois permite acompanhar as informações produzidas pelo programa.

O teclado e o mouse são utilizados principalmente para desenvolver, configurar, executar e controlar os programas. Eles não são responsáveis pela coleta automática da temperatura durante a operação normal do sistema.

Caso o grupo desenvolva uma interface gráfica, os resultados poderão ser apresentados por meio de janelas, tabelas ou gráficos. Entretanto, essas funcionalidades adicionais dependem de implementação específica.

### 6.5. Armazenamento de dados

O armazenamento permite conservar as medições para consultas e análises posteriores. O histórico de temperaturas pode ser mantido em arquivos ou em um banco de dados, conforme a solução adotada.

Quando os registros são armazenados de maneira organizada, o programa pode recuperar medições anteriores e utilizá-las em comparações. A inclusão de informações como data, horário e identificador também pode facilitar a organização dos dados.

Entretanto, o histórico não é criado automaticamente pela existência do sensor ou do Python. É necessário implementar a gravação das medições, definir o formato dos registros e estabelecer como as informações serão recuperadas.

Se o projeto ainda não possuir armazenamento persistente, a comparação histórica deverá ser descrita como uma funcionalidade planejada ou condicionada à implementação, e não como uma capacidade já comprovada.

## 7. Participação humana no processo

A participação humana ocorre em diferentes momentos do funcionamento do protótipo. Mesmo quando a coleta e as análises são automatizadas, o sistema depende de pessoas para sua preparação, configuração, manutenção e interpretação dos resultados.

### 7.1. Participação antes da execução

Antes de iniciar o monitoramento, o usuário precisa preparar os componentes físicos e os programas.

Essa preparação pode incluir a montagem do circuito, a conexão do sensor ao ESP32, a verificação da alimentação elétrica, o desenvolvimento do programa embarcado e a configuração da comunicação com o computador.

Também é necessário preparar o programa em Python e definir os parâmetros utilizados nas análises, como limites de temperatura e critérios de comparação.

Essas ações são importantes porque uma configuração incorreta pode impedir o funcionamento do sistema ou provocar resultados incompatíveis com o comportamento esperado.

### 7.2. Participação durante a execução

Durante a operação normal, o sistema pode realizar automaticamente a coleta, a transmissão e o processamento das medições.

A participação humana pode ocorrer quando o usuário consulta os resultados, verifica mensagens de aviso, altera parâmetros ou investiga problemas de comunicação.

Se houver interrupção da transmissão ou falha na leitura do sensor, pode ser necessária uma intervenção para identificar a causa e restabelecer o funcionamento.

O grau de autonomia depende das funcionalidades implementadas. Um sistema que apenas apresenta mensagens no terminal exige que o usuário acompanhe essa interface para perceber os avisos, enquanto uma versão com notificações específicas poderia informar determinadas ocorrências de maneira diferente.

### 7.3. Participação depois da execução

Após observar os resultados, o usuário pode analisar o comportamento da temperatura, verificar se ocorreram valores fora dos limites e avaliar se os parâmetros utilizados são adequados ao objetivo do monitoramento.

Também pode ser necessário consultar o histórico para compreender a evolução das medições e distinguir uma alteração isolada de uma mudança persistente.

A interpretação final depende do contexto. O programa apresenta resultados de acordo com as regras implementadas, mas não substitui automaticamente a avaliação humana sobre o significado das medições.

## 8. Mecanismos automáticos e manuais

O protótipo combina operações automáticas e intervenções manuais. Essa divisão permite identificar quais atividades são realizadas pelos dispositivos e programas e quais dependem da participação do usuário.

| Etapa | Tipo de operação | Descrição |
|---|---|---|
| Montagem do circuito | Manual | Conexão do sensor ao ESP32 e verificação da alimentação. |
| Programação do microcontrolador | Manual | Desenvolvimento e transferência das instruções para o ESP32. |
| Configuração do programa em Python | Manual | Preparação do software responsável por receber e analisar os dados. |
| Definição dos parâmetros | Manual ou previamente configurada | Escolha dos limites e critérios utilizados nas análises. |
| Medição da temperatura | Automática | O sensor realiza a medição conforme a operação configurada. |
| Aquisição da leitura pelo ESP32 | Automática | O microcontrolador obtém o valor por meio da interface do sensor. |
| Preparação da mensagem | Automática | O programa embarcado organiza os dados conforme o formato definido. |
| Transmissão dos dados | Automática, após configuração | O ESP32 envia as informações pelo mecanismo de comunicação implementado. |
| Recepção no computador | Automática, após inicialização | O programa recebe as mensagens enviadas pelo ESP32. |
| Análise das medições | Automática | O Python executa as regras de processamento programadas. |
| Armazenamento do histórico | Automática, se implementado | O programa registra as medições para consultas posteriores. |
| Consulta dos resultados | Manual | O usuário observa e interpreta as informações apresentadas. |
| Correção de falhas | Geralmente manual | O usuário investiga problemas de hardware, comunicação ou software. |

A tabela demonstra que a automação depende de uma configuração prévia. O sistema executa as tarefas previstas pelas instruções, mas não elimina a necessidade de desenvolvimento, manutenção e acompanhamento.

Também é importante observar que algumas operações descritas como automáticas dependem da implementação correspondente. Por exemplo, o armazenamento do histórico somente poderá ocorrer automaticamente se o programa possuir essa funcionalidade.

## 9. Limitações do processo de entrada, processamento e saída

O funcionamento do protótipo depende da qualidade das medições, da comunicação entre os dispositivos, da execução correta dos programas e da adequação dos critérios utilizados nas análises.

Compreender essas limitações é importante para evitar conclusões incorretas e identificar melhorias que podem ser realizadas durante o desenvolvimento do projeto.

### 9.1. Limitações do sensor

O sensor possui características específicas de precisão, resolução e faixa de operação. A instalação inadequada, as condições ambientais e os problemas de conexão podem afetar a qualidade das medições.

Além disso, o fato de uma leitura ser digital não garante que ela represente corretamente a temperatura do ambiente. O programa precisa considerar a possibilidade de erros de leitura e evitar tratar qualquer valor recebido como necessariamente válido.

Para reduzir esses problemas, é importante verificar as conexões, observar o comportamento das medições e implementar verificações compatíveis com o sensor utilizado.

### 9.2. Limitações da comunicação

A transmissão por Wi-Fi depende da disponibilidade da rede, da intensidade do sinal e da configuração dos dispositivos. Interferências, desconexões e problemas no mecanismo de comunicação podem impedir que os dados cheguem ao computador.

Também podem ocorrer situações em que uma mensagem seja recebida incompleta ou não corresponda ao formato esperado pelo programa.

Uma implementação mais robusta deve considerar o tratamento de erros, a validação das mensagens e o comportamento esperado quando a comunicação for interrompida.

É importante que o sistema diferencie a ausência de dados de uma medição normal. Se não houver novas informações, o programa não deve apresentar automaticamente o último valor como se fosse uma nova leitura.

### 9.3. Limitações do processamento

O programa em Python executa somente as análises previstas em suas instruções. Erros no código, condições não consideradas e parâmetros inadequados podem produzir resultados incorretos.

Por exemplo, um limite configurado de maneira inadequada pode gerar avisos frequentes ou deixar de sinalizar situações relevantes.

A identificação de anomalias também depende dos critérios escolhidos. Um sistema baseado apenas em limites fixos pode não reconhecer todas as situações incomuns e pode classificar como anormal uma variação que seja esperada nas condições reais do ambiente.

Por isso, as regras precisam ser avaliadas e testadas com diferentes valores de entrada. Os testes devem verificar tanto os casos esperados quanto as situações de erro.

### 9.4. Limitações do histórico

A comparação com registros anteriores depende da existência de um histórico válido. Se as medições não forem armazenadas, forem perdidas ou apresentarem registros incompletos, a análise temporal poderá ser prejudicada.

Também é necessário manter uma organização consistente dos valores e, quando aplicável, dos horários associados às medições.

Caso existam períodos sem coleta, o programa deve considerar essa ausência ao interpretar a sequência dos registros. Uma diferença entre duas medições pode ser influenciada pelo intervalo de tempo que as separa.

### 9.5. Limitações de disponibilidade

O monitoramento depende do funcionamento dos componentes físicos e dos programas. Se o ESP32 for desligado, o sensor apresentar problemas ou o programa em Python deixar de executar, a coleta ou a análise poderá ser interrompida.

A disponibilidade da rede também influencia a transmissão dos dados.

O sistema precisa considerar situações em que não existam medições recentes. Quando isso ocorrer, uma mensagem de ausência de dados pode ser mais adequada do que apresentar um valor antigo sem indicar que ele não foi atualizado.

### 9.6. Limitações da saída

A apresentação de mensagens no terminal é suficiente para demonstrar o funcionamento básico, mas possui limitações para acompanhar longos períodos de monitoramento.

Gráficos, tabelas, interfaces visuais e mecanismos de notificação poderiam facilitar a interpretação dos resultados. Entretanto, essas funcionalidades exigem desenvolvimento adicional e não devem ser consideradas existentes antes de sua implementação.

Também é necessário garantir que as mensagens sejam claras e correspondam às condições verificadas pelo programa. Um aviso deve informar o que foi identificado, sem apresentar conclusões que os dados não permitem sustentar.

### 9.7. Limitações relacionadas à segurança e à confiabilidade

Como o projeto trabalha com informações de temperatura, a interpretação dos resultados precisa considerar o objetivo do monitoramento. Os limites utilizados devem ser compatíveis com o contexto escolhido.

O protótipo possui finalidade acadêmica e não deve ser considerado um sistema industrial de segurança sem validações específicas, testes adequados e mecanismos de confiabilidade compatíveis com esse uso.

As conclusões da pesquisa devem permanecer limitadas às funcionalidades implementadas e efetivamente testadas pelo grupo.

## 10. Exemplo prático de entrada, processamento, interação e saída

Esta seção apresenta um exemplo de funcionamento do protótipo sem utilizar fluxogramas. O objetivo é demonstrar, por meio de uma situação concreta, como uma medição realizada no ambiente pode ser transformada em uma informação apresentada ao usuário.

### 10.1. Entrada: obtenção da temperatura

Considere que o sensor DS18B20 esteja instalado no ambiente que o grupo deseja monitorar. Durante a operação, o sensor realiza uma medição e disponibiliza o valor digital correspondente.

Para este exemplo, será considerada uma medição válida de 38 °C.

O ESP32 obtém esse valor por meio da comunicação 1-Wire. Em seguida, o programa embarcado prepara a informação para transmissão ao computador.

Nesse momento, a entrada principal do sistema é a temperatura medida. O valor representa uma informação originada no ambiente físico e convertida em dados digitais para uso computacional.

### 10.2. Interação e transmissão das informações

Depois de obter a medição, o ESP32 organiza o valor de acordo com o formato definido pelo grupo e transmite a mensagem ao computador por meio da conexão Wi-Fi.

O programa em Python precisa receber a mensagem e interpretar seu conteúdo. Para isso, o formato enviado pelo microcontrolador deve ser compatível com o formato esperado pelo programa receptor.

Uma mensagem ilustrativa poderia conter o campo `temperatura` associado ao valor `38`. A estrutura real dependerá do mecanismo de comunicação escolhido durante a implementação.

Essa etapa demonstra a interação entre os componentes físicos e lógicos. O sensor fornece a medição, o microcontrolador prepara a informação e a comunicação permite que o programa executado no computador tenha acesso ao dado.

### 10.3. Processamento: análise do valor recebido

Após receber a mensagem, o programa em Python verifica se o conteúdo pode ser interpretado como uma medição válida.

Em seguida, considera o parâmetro configurado para o limite superior de temperatura. Neste exemplo, o limite está definido em 35 °C.

O programa compara os dois valores. Como a medição de 38 °C é superior ao limite de 35 °C, a condição estabelecida é atendida.

Se a funcionalidade estiver implementada, o programa também poderá consultar o histórico e aplicar critérios de identificação de possíveis anomalias. Essas análises adicionais dependem da existência dos registros e das regras programadas.

### 10.4. Saída: apresentação do resultado

Depois de concluir as operações previstas, o programa apresenta uma mensagem ao usuário indicando que a temperatura ultrapassou o limite configurado.

Um resultado ilustrativo seria:

```text
Temperatura recebida: 38 °C
Limite superior configurado: 35 °C
Resultado: limite ultrapassado
```

Essa saída permite compreender o resultado do processamento sem exigir que o usuário realize manualmente a comparação numérica.

O sistema transforma a medição recebida em uma informação interpretável, conforme as regras estabelecidas pelo programa.

A mensagem não significa, por si só, que exista uma situação de perigo. Ela apenas informa que o valor ultrapassou o limite configurado. A interpretação dessa condição depende do ambiente monitorado e da finalidade do sistema.

### 10.5. Significado do exemplo para o projeto

O exemplo demonstra a relação entre a entrada de dados, a interação entre dispositivos, o processamento computacional e a produção de uma saída informacional.

A temperatura é obtida pelo sensor e disponibilizada ao ESP32. O microcontrolador transmite o valor ao computador, onde o Python executa as verificações programadas. Por fim, o resultado é apresentado ao usuário.

Essa descrição permite analisar o funcionamento do sistema sem recorrer a fluxogramas, mantendo o foco nos componentes envolvidos e nas responsabilidades de cada etapa.

## 11. Relação com Arquitetura de Computadores

O estudo da entrada, saída e interação com o ambiente permite relacionar o protótipo a diferentes conceitos de Arquitetura de Computadores.

### 11.1. Entrada de dados

O sensor DS18B20 representa uma fonte externa de informações. Ele realiza uma medição física e disponibiliza o resultado em formato digital para o microcontrolador.

Esse mecanismo demonstra como um sistema computacional pode receber dados de dispositivos externos, em vez de depender exclusivamente de informações digitadas por uma pessoa.

### 11.2. Processamento de informações

O ESP32 executa as instruções embarcadas necessárias para obter e preparar as medições. O computador, por sua vez, executa o programa em Python para interpretar os dados e aplicar as regras de análise.

Essa divisão permite observar que diferentes dispositivos podem participar de um mesmo sistema, cada um executando tarefas específicas.

### 11.3. Memória e armazenamento

Durante a execução, os dispositivos utilizam memória para manter instruções, variáveis e informações temporárias necessárias às operações.

O armazenamento permanente das temperaturas é uma funcionalidade distinta. Ele depende da implementação de arquivos ou de um banco de dados para conservar as medições e permitir consultas posteriores.

Essa distinção ajuda a compreender que manter temporariamente um valor durante a execução não é o mesmo que registrar esse valor para recuperá-lo em outro momento.

### 11.4. Comunicação entre dispositivos

A comunicação 1-Wire permite a troca de informações entre o sensor e o ESP32. A conexão Wi-Fi possibilita a transmissão de dados entre o microcontrolador e o computador.

Essas interfaces demonstram que o funcionamento do sistema depende de mecanismos de comunicação compatíveis, capazes de transportar as informações entre os componentes.

A definição dos protocolos e formatos de mensagem é importante para garantir que o receptor consiga interpretar corretamente os dados enviados.

### 11.5. Saída de informações

O terminal ou outra interface de software pode apresentar as temperaturas e os resultados produzidos pelo programa.

Essa saída permite que o usuário acompanhe o comportamento do sistema e compreenda as condições identificadas pelas análises.

A apresentação dos resultados faz parte da interação entre o sistema computacional e a pessoa que utiliza o protótipo.

### 11.6. Controle por software

Os programas determinam a sequência de operações, os critérios de processamento e as condições para produzir os resultados.

O software também estabelece como o sistema reage a determinadas situações, como o recebimento de uma temperatura acima do limite ou a ocorrência de uma falha de comunicação.

Dessa maneira, o funcionamento depende da integração entre hardware, software, memória e interfaces de comunicação.

### 11.7. Relação com a taxonomia de Flynn

A taxonomia de Flynn classifica organizações de computadores de acordo com os fluxos de instruções e de dados. Dentro dessa classificação, MISD corresponde a *Multiple Instruction, Single Data*.

No projeto, diferentes análises podem utilizar a mesma medição de temperatura como entrada lógica. Isso permite estudar a aplicação de operações distintas sobre uma mesma informação.

Entretanto, essa característica, isoladamente, não comprova uma arquitetura física MISD. Para estabelecer essa classificação, seria necessário demonstrar como os fluxos de instruções e de dados são organizados na implementação computacional.

Assim, a relação com MISD deve ser apresentada como um aspecto conceitual investigado pelo grupo, mantendo a distinção entre o estudo acadêmico do modelo e a comprovação de sua implementação física.

## 12. Conclusão

A investigação sobre entrada, saída e interação com o ambiente permitiu detalhar como as informações são coletadas por um sensor, transmitidas por um microcontrolador, processadas por um programa computacional e apresentadas ao usuário.

O estudo identificou que o DS18B20 é responsável pela medição da temperatura, enquanto o ESP32 realiza a aquisição dos dados e executa as instruções necessárias para preparar as informações. A comunicação Wi-Fi permite encaminhar as mensagens ao computador, onde o Python pode realizar verificações de limites, comparações históricas e análises de possíveis anomalias.

Também foi possível compreender a diferença entre as operações automáticas e manuais. A coleta e o processamento podem ocorrer automaticamente após a configuração, enquanto o desenvolvimento dos programas, a definição dos parâmetros, a manutenção dos componentes e a interpretação dos resultados continuam dependendo da participação humana.

A análise das limitações demonstrou que a confiabilidade do sistema depende de fatores como a qualidade das medições, a estabilidade da comunicação, a validação das mensagens, a organização do histórico e a adequação das regras de processamento.

Além disso, a pesquisa permitiu relacionar o protótipo aos conceitos de entrada de dados, processamento, memória, armazenamento, comunicação e saída de informações estudados em Arquitetura de Computadores.

Por fim, o projeto oferece uma oportunidade acadêmica para compreender a integração entre hardware e software e discutir a aplicação conceitual de MISD por meio de diferentes análises sobre uma mesma informação. A classificação do protótipo como uma implementação física MISD, entretanto, exigiria evidências técnicas adicionais sobre a organização dos fluxos de instruções e de dados.

## 13. Referências

1. ANALOG DEVICES. *DS18B20: Programmable Resolution 1-Wire Digital Thermometer.* Disponível em: https://www.analog.com/en/products/ds18b20.html. Acesso em: 20 out. 2026.

2. ESPRESSIF SYSTEMS. *ESP32 Series Datasheet.* Disponível em: https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf. Acesso em: 20 out. 2026.

3. PYTHON SOFTWARE FOUNDATION. *Python Documentation.* Disponível em: https://docs.python.org/3/. Acesso em: 20 out. 2026.

4. FLYNN, Michael J. *Some Computer Organizations and Their Effectiveness.* IEEE Transactions on Computers, v. C-21, n. 9, p. 948–960, 1972. DOI: https://doi.org/10.1109/TC.1972.5009071.
