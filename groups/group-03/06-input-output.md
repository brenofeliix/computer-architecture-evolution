# ENTREGA 03 — Entrada, Saída e Interação com o Ambiente

**Data:** 24/09/2026  
**Grupo:** 03 — Estudo da Arquitetura MISD

## 1. Introdução

Esta etapa tem como objetivo investigar os mecanismos de entrada e saída de informações e a interação entre os componentes físicos e lógicos do protótipo de monitoramento de temperatura, relacionando seu funcionamento aos conceitos estudados em Arquitetura de Computadores.

O projeto propõe utilizar um sensor de temperatura DS18B20, um microcontrolador ESP32, uma conexão Wi-Fi e um programa desenvolvido em Python. Esses componentes trabalham em conjunto para coletar informações do ambiente, transmitir os valores medidos, processar os dados recebidos e apresentar resultados ao usuário.

A investigação busca compreender como as informações são capturadas, como os programas e as configurações são inseridos, quais mecanismos permitem a comunicação entre os dispositivos e como os resultados são produzidos. Também são consideradas as limitações que podem afetar o funcionamento do sistema.

É importante destacar que o projeto possui finalidade acadêmica. A utilização de diferentes análises sobre os mesmos dados permite estudar o processamento de informações, mas não comprova, isoladamente, a implementação física de uma arquitetura MISD (*Multiple Instruction, Single Data*).

## 2. Entrada de informações no sistema

A entrada de informações corresponde ao processo pelo qual o sistema recebe dados provenientes do ambiente externo. No protótipo proposto, a principal entrada é a temperatura medida pelo sensor DS18B20.

### 2.1. Coleta de temperatura pelo sensor

O DS18B20 é um sensor digital de temperatura que realiza medições e disponibiliza os valores por meio de comunicação digital. O sensor realiza internamente a conversão da medição em um valor digital.

O componente utiliza o protocolo 1-Wire para se comunicar com o dispositivo controlador. Essa comunicação permite que o ESP32 solicite e receba os valores de temperatura por meio de uma interface elétrica apropriada.

O processo de entrada ocorre nas seguintes etapas:

1. O sensor entra em contato térmico com o ambiente monitorado.
2. O componente realiza a medição da temperatura.
3. A medição é convertida em uma representação digital.
4. O ESP32 obtém a leitura conforme a lógica implementada no programa.
5. O valor fica disponível para as etapas seguintes do sistema.

Dessa forma, o sensor funciona como uma interface entre uma grandeza física, a temperatura, e o sistema computacional, que trabalha com dados digitais.

### 2.2. Tipos de dados recebidos

A informação principal é o valor numérico da temperatura, normalmente expresso em graus Celsius. Para facilitar o processamento e o armazenamento, o sistema também pode associar outras informações à medição, como:

- **Valor da temperatura:** resultado numérico obtido pelo sensor.
- **Data e horário:** momento em que a medição foi registrada, caso essa funcionalidade seja implementada.
- **Identificação da medição:** número sequencial ou identificador utilizado para organizar os registros.
- **Estado da leitura:** informação sobre a validade da medição, quando houver verificações implementadas.

Essas informações adicionais não são necessariamente fornecidas diretamente pelo sensor. O horário, por exemplo, pode ser atribuído pelo computador, enquanto o identificador pode ser criado pelo programa em Python.

A organização dos dados facilita a comparação entre medições e a identificação de possíveis alterações no comportamento da temperatura.

### 2.3. Entrada automática e entrada manual

O protótipo combina dois tipos de entrada:

- **Entrada automática:** corresponde à coleta das temperaturas pelo sensor e à transmissão dos valores para o computador. Após a configuração, o processo pode ocorrer sem que uma pessoa precise digitar cada medição.
- **Entrada manual:** corresponde às ações realizadas pelo usuário durante a configuração e o acompanhamento do sistema, como definir limites de temperatura, alterar parâmetros, iniciar programas e consultar resultados.

Essa separação demonstra que a automação da coleta não elimina a participação humana. O usuário continua responsável por definir os parâmetros de funcionamento e interpretar as informações apresentadas.

## 3. Inserção de programas, instruções e configurações

Para que o protótipo funcione, é necessário inserir instruções nos dispositivos responsáveis pela coleta e pelo processamento dos dados. Essas instruções determinam como os componentes devem operar e como as informações devem ser tratadas.

### 3.1. Programação do ESP32

O ESP32 é um microcontrolador que executa um programa embarcado. Esse programa pode ser desenvolvido em um computador utilizando um ambiente de desenvolvimento compatível e, posteriormente, transferido para o dispositivo.

Entre as instruções que podem ser implementadas estão:

- Inicializar as interfaces e os componentes utilizados.
- Configurar a comunicação com o sensor DS18B20.
- Solicitar a medição da temperatura.
- Verificar se a leitura foi obtida corretamente.
- Organizar o valor para transmissão.
- Estabelecer ou utilizar a conexão Wi-Fi.
- Enviar os dados ao computador.
- Repetir a coleta conforme o intervalo configurado.

O programa define a sequência de operações que o microcontrolador deve executar. Assim, o comportamento do dispositivo depende tanto das instruções gravadas quanto das condições físicas e de comunicação existentes durante sua execução.

### 3.2. Programação em Python

No computador, o programa em Python é responsável por receber e analisar os dados transmitidos pelo ESP32.

O código pode conter instruções condicionais, cálculos, comparações e mecanismos de registro das medições. A partir dessas instruções, o sistema transforma os valores recebidos em informações úteis para o acompanhamento da temperatura.

O programa pode ser organizado nas seguintes etapas:

1. Receber os dados enviados pelo ESP32.
2. Interpretar a mensagem recebida.
3. Verificar se o valor possui um formato válido.
4. Comparar a temperatura com os limites estabelecidos.
5. Consultar os registros anteriores, caso exista um histórico.
6. Aplicar os critérios de identificação de possíveis anomalias.
7. Apresentar ou armazenar os resultados.

A linguagem Python permite implementar essas operações por meio de instruções executadas pelo processador do computador. A ordem e as condições dessas instruções são determinadas pelo código desenvolvido pelo grupo.

### 3.3. Configurações do sistema

Além dos programas, o funcionamento depende de configurações que determinam como os dados serão coletados, transmitidos e analisados.

Exemplos de configurações incluem:

- Intervalo entre as medições.
- Parâmetros de conexão à rede Wi-Fi.
- Endereço ou mecanismo utilizado para encaminhar os dados ao computador.
- Limites mínimo e máximo de temperatura, quando aplicáveis.
- Critérios de comparação com o histórico.
- Condições utilizadas para classificar uma leitura como potencialmente anômala.
- Local e formato de armazenamento dos registros.

Essas configurações podem ser definidas no código ou em arquivos específicos, dependendo da implementação adotada.

É importante diferenciar as instruções dos parâmetros: as instruções determinam as operações realizadas pelo sistema, enquanto os parâmetros definem valores e condições utilizados durante a execução.

## 4. Processamento das informações

O processamento é a etapa em que os dados recebidos são interpretados e submetidos às instruções dos programas. No projeto, essa etapa ocorre principalmente no computador que executa o Python, embora o ESP32 também realize operações locais para obter e preparar as medições.

### 4.1. Processamento no ESP32

O ESP32 executa as instruções responsáveis pela comunicação com o sensor e pela preparação dos dados. Ele atua como intermediário entre a coleta física e o programa executado no computador.

Após obter a medição, o microcontrolador pode organizar o valor em uma mensagem com formato definido pelo projeto. Essa mensagem é transmitida ao computador por meio da conexão Wi-Fi.

A organização correta dos dados é importante porque o receptor precisa compreender o conteúdo recebido. Se o ESP32 enviar uma mensagem em um formato diferente daquele esperado pelo Python, o programa poderá não conseguir interpretar o valor.

### 4.2. Processamento no computador

O computador recebe os dados e executa as instruções do programa em Python. Nessa etapa, o valor da temperatura pode ser submetido a diferentes operações.

**Verificação de limites:** o programa compara a medição com os limites definidos pelo usuário. Por exemplo, se o limite superior estiver configurado em 35 °C e uma leitura válida indicar 38 °C, o programa poderá sinalizar que o valor ultrapassou o limite estabelecido.

**Comparação com o histórico:** a medição atual pode ser comparada com registros anteriores para observar aumentos, reduções e variações da temperatura ao longo do tempo. Essa funcionalidade depende da existência de registros armazenados.

**Identificação de possíveis anomalias:** o programa pode aplicar regras para detectar valores que se afastem de um comportamento esperado. Essas regras podem utilizar limites, diferenças entre medições ou outros critérios definidos no código.

As três análises possuem finalidades diferentes. A verificação de limites observa condições previamente estabelecidas; a comparação com o histórico considera valores anteriores; e a identificação de anomalias depende dos critérios utilizados para reconhecer comportamentos incomuns.

Uma diferença em relação ao histórico não significa necessariamente que exista uma falha. Por isso, os resultados devem ser interpretados considerando as condições do ambiente e as regras implementadas.

### 4.3. Relação com o conceito MISD

MISD significa *Multiple Instruction, Single Data*, ou múltiplas instruções sobre um único fluxo de dados. Na taxonomia de Flynn, esse conceito é utilizado para classificar organizações de computadores segundo os fluxos de instruções e de dados.

No contexto deste projeto, diferentes análises podem utilizar o mesmo valor de temperatura como entrada lógica. Por exemplo, uma medição pode ser utilizada para verificar limites e, separadamente, para realizar comparações com valores anteriores.

Essa abordagem ajuda o grupo a estudar a ideia de aplicar operações distintas aos mesmos dados. Entretanto, executar várias funções em sequência em um único programa Python não caracteriza, por si só, uma implementação física MISD.

Portanto, o protótipo deve ser compreendido como uma aplicação acadêmica de monitoramento que permite discutir o conceito MISD, sem confundir o uso de múltiplas análises com a comprovação de uma arquitetura MISD física.

## 5. Produção e apresentação dos resultados

A saída corresponde às informações produzidas pelo sistema depois que os dados são processados. No protótipo, os resultados são gerados pelo programa em Python e podem ser apresentados ao usuário por meio do terminal ou de outra interface implementada pelo grupo.

### 5.1. Resultados possíveis

O programa pode apresentar diferentes tipos de resultados:

- Temperatura atual recebida do sensor.
- Indicação de que a leitura está dentro dos limites definidos.
- Aviso de que a temperatura ultrapassou um limite configurado.
- Comparação entre a medição atual e valores anteriores.
- Identificação de valores que atendam aos critérios de possível anomalia.
- Mensagens indicando falhas na recepção ou na interpretação dos dados.

Essas saídas dependem das funcionalidades efetivamente implementadas. Um resultado só poderá ser produzido se o programa possuir instruções para realizar a análise correspondente.

### 5.2. Exemplo de resultado

Considere que o sistema esteja configurado com um limite superior de 35 °C e receba uma medição válida de 38 °C.

O processamento poderá produzir a seguinte saída:

```text
Temperatura atual: 38 °C
Limite superior configurado: 35 °C
Situação: temperatura acima do limite
```

Esse exemplo demonstra como um valor físico coletado pelo sensor pode ser transformado em uma informação interpretável pelo usuário.

O resultado não significa necessariamente que o ambiente esteja em situação de perigo. Ele apenas indica que a medição ultrapassou o limite configurado no programa.

### 5.3. Saída informacional e saída física

É necessário distinguir a saída de informações da atuação sobre o ambiente.

No protótipo descrito, a saída principal é informacional: o programa apresenta resultados e possíveis alertas. Não há, na proposta atual, necessidade de acionar motores, relés, ventiladores ou outros dispositivos físicos.

Caso sejam adicionados componentes desse tipo futuramente, o sistema poderá passar a realizar ações físicas com base nos resultados processados. Essa funcionalidade, entretanto, não deve ser descrita como existente enquanto não for implementada e testada.

## 6. Dispositivos, mecanismos e interfaces externas

A comunicação entre os componentes depende de interfaces que permitem a entrada, a transmissão e a saída de informações.

### 6.1. Sensor DS18B20

O DS18B20 é responsável pela medição da temperatura. Sua interface digital utiliza o protocolo 1-Wire, que possibilita a comunicação com o controlador por uma linha de dados, além das conexões elétricas necessárias à alimentação e à referência elétrica.

O sensor representa a principal interface entre o ambiente físico e o sistema computacional. Sua precisão, instalação e condições de operação influenciam a qualidade das informações coletadas.

### 6.2. ESP32

O ESP32 recebe as informações do sensor e executa o programa embarcado. O dispositivo possui recursos de processamento, memória e comunicação, incluindo Wi-Fi, que podem ser utilizados para transmitir os dados ao computador.

No projeto, o ESP32 funciona como um ponto de integração entre a medição física e a aplicação de software.

### 6.3. Comunicação Wi-Fi

A interface Wi-Fi permite a transmissão sem fio das informações entre o ESP32 e o computador. Para que a comunicação funcione, os dispositivos precisam utilizar uma configuração compatível e um mecanismo de troca de mensagens definido pelo projeto.

A conexão Wi-Fi, por si só, não define como os dados serão enviados. É necessário implementar um mecanismo de comunicação, como uma conexão TCP ou uma interface HTTP, conforme a solução escolhida pelo grupo.

A escolha do mecanismo influencia a forma de estabelecer a conexão, enviar as mensagens e tratar eventuais falhas.

### 6.4. Computador e terminal

O computador executa o programa em Python, recebe os dados e produz os resultados. O terminal pode ser utilizado como interface de saída para exibir as temperaturas e as mensagens geradas pelo programa.

O teclado e o mouse são utilizados principalmente para desenvolver, executar e configurar os programas. Eles não são responsáveis pela coleta automática da temperatura durante a operação normal do sistema.

### 6.5. Armazenamento de dados

O histórico de temperaturas pode ser armazenado em arquivos ou em um banco de dados, dependendo da implementação. O armazenamento permite recuperar valores anteriores para comparações e análises.

Entretanto, o histórico não é criado automaticamente apenas pela existência do sensor ou do Python. É necessário implementar a gravação das medições e definir como os registros serão organizados e recuperados.

## 7. Participação humana no processo

A participação humana ocorre em diferentes momentos do funcionamento do protótipo.

### 7.1. Antes da execução

Antes de iniciar o monitoramento, o usuário precisa montar e conectar os componentes, desenvolver ou carregar o programa embarcado, configurar a comunicação e preparar o programa em Python.

Também pode ser necessário definir os limites de temperatura e os critérios de análise.

### 7.2. Durante a execução

Durante a operação normal, o sistema pode realizar automaticamente a coleta, a transmissão e o processamento das medições.

A intervenção humana pode ocorrer quando o usuário consulta os resultados, altera configurações, reinicia componentes ou resolve falhas de comunicação.

### 7.3. Depois da execução

Os resultados podem ser utilizados para observar o comportamento da temperatura, verificar a ocorrência de valores fora dos limites e avaliar a necessidade de ajustes nos parâmetros.

A interpretação final continua dependendo do contexto. O programa apresenta resultados de acordo com as regras implementadas, mas não substitui automaticamente a avaliação humana sobre o significado das medições.

## 8. Mecanismos automáticos e manuais

O protótipo combina operações automáticas e intervenções manuais.

| Etapa | Tipo de operação | Descrição |
|---|---|---|
| Montagem do circuito | Manual | Conexão do sensor ao ESP32 e verificação da alimentação. |
| Programação do microcontrolador | Manual | Desenvolvimento e transferência das instruções para o ESP32. |
| Medição da temperatura | Automática | O sensor realiza a medição conforme a operação configurada. |
| Recepção da medição pelo ESP32 | Automática | O microcontrolador obtém o valor por meio da interface do sensor. |
| Transmissão dos dados | Automática, após configuração | O ESP32 envia as informações conforme o mecanismo implementado. |
| Recepção no computador | Automática, após inicialização | O programa recebe as mensagens enviadas pelo ESP32. |
| Análise dos dados | Automática | O Python executa as regras de processamento programadas. |
| Definição de parâmetros | Manual ou configurada previamente | O usuário define limites e critérios utilizados nas análises. |
| Consulta dos resultados | Manual | O usuário observa e interpreta as informações apresentadas. |
| Correção de falhas | Geralmente manual | O usuário investiga problemas de hardware, comunicação ou software. |

A tabela mostra que a automação depende de uma configuração prévia. O sistema executa as tarefas previstas pelo programa, mas não elimina a necessidade de desenvolvimento, manutenção e acompanhamento.

## 9. Limitações do processo de entrada, processamento e saída

O funcionamento do protótipo depende da qualidade das medições, da comunicação entre os dispositivos e da execução correta dos programas.

### 9.1. Limitações do sensor

O sensor possui características específicas de precisão, resolução e faixa de operação. Instalação inadequada, condições ambientais e problemas de conexão podem afetar a qualidade das medições.

Além disso, uma leitura digital não garante que a medição esteja correta. O programa deve considerar a possibilidade de falhas e evitar tratar qualquer valor recebido como necessariamente válido.

### 9.2. Limitações da comunicação

A transmissão por Wi-Fi depende da disponibilidade da rede, da intensidade do sinal e da configuração dos dispositivos. Interferências, desconexões e problemas no mecanismo de comunicação podem impedir a chegada dos dados ao computador.

Por isso, uma implementação mais robusta deve considerar o tratamento de erros, a detecção de mensagens inválidas e o comportamento esperado quando a comunicação for interrompida.

### 9.3. Limitações do processamento

O programa em Python só executa as análises previstas em suas instruções. Erros no código, condições não previstas e parâmetros inadequados podem produzir resultados incorretos.

A identificação de anomalias também depende dos critérios escolhidos. Um sistema baseado apenas em limites fixos pode não reconhecer todas as situações incomuns, assim como pode sinalizar valores que não representam um problema real.

### 9.4. Limitações do histórico

A comparação com registros anteriores depende da existência de um histórico válido. Caso as medições não sejam armazenadas, sejam perdidas ou apresentem registros incompletos, a análise temporal poderá ser prejudicada.

Também é necessário manter uma organização consistente dos valores e, quando aplicável, dos horários associados a cada medição.

### 9.5. Limitações de disponibilidade

O monitoramento depende do funcionamento dos componentes físicos e dos programas. Se o ESP32 for desligado, o sensor apresentar problemas ou o programa em Python deixar de executar, a coleta ou a análise poderá ser interrompida.

O sistema também precisa de mecanismos adequados para informar ao usuário quando não houver dados disponíveis, evitando que a ausência de novas medições seja interpretada como uma temperatura estável.

### 9.6. Limitações da saída

A apresentação de uma mensagem no terminal é suficiente para demonstrar o funcionamento básico, mas oferece recursos limitados para acompanhar longos períodos de medição.

Gráficos, interfaces visuais e mecanismos de notificação podem melhorar a apresentação dos resultados, mas exigem desenvolvimento adicional.

Como o projeto tem finalidade acadêmica, suas conclusões devem ser limitadas às funcionalidades implementadas e testadas. O protótipo não deve ser tratado como um sistema industrial de segurança sem validações específicas.

## 10. Exemplo completo do fluxo de entrada, processamento e saída

O fluxo representa como uma medição realizada no ambiente se transforma em uma informação apresentada ao usuário.

### 10.1. Etapa 1 — Entrada

O sensor DS18B20 mede a temperatura e disponibiliza o valor digital ao ESP32 por meio da comunicação 1-Wire.

Exemplo de entrada:

```text
Temperatura medida: 38 °C
```

### 10.2. Etapa 2 — Transmissão

O ESP32 recebe o valor, organiza a informação conforme o formato definido no programa e transmite a mensagem ao computador por Wi-Fi.

Exemplo ilustrativo da mensagem:

```text
temperatura=38
```

O formato é apenas um exemplo. A mensagem real dependerá do protocolo e da estrutura definidos na implementação.

### 10.3. Etapa 3 — Recepção e processamento

O programa em Python recebe a mensagem, interpreta o valor e verifica se ele atende às condições esperadas.

Considerando o limite superior de 35 °C, o programa compara a temperatura de 38 °C com o parâmetro configurado e identifica que o limite foi ultrapassado.

Se existir um histórico armazenado, o sistema também poderá comparar a medição atual com registros anteriores e aplicar outros critérios de análise.

### 10.4. Etapa 4 — Saída

Após o processamento, o programa apresenta o resultado ao usuário.

Exemplo ilustrativo:

```text
Temperatura recebida: 38 °C
Limite superior: 35 °C
Resultado: limite ultrapassado
```

Esse resultado é produzido pelas instruções do programa. Ele não exige, necessariamente, uma ação física sobre o ambiente.

### 10.5. Representação resumida

**Entrada → Processamento e interação → Saída**

```text
Ambiente
   ↓
Sensor DS18B20
   ↓
ESP32
   ↓
Wi-Fi
   ↓
Computador com Python
   ↓
Análise dos dados
   ↓
Resultado apresentado ao usuário
```

O fluxo evidencia a integração entre o ambiente físico, os dispositivos eletrônicos, a comunicação e o software. Cada componente possui uma função específica e contribui para transformar uma grandeza física em informação processada.

## 11. Relação com Arquitetura de Computadores

A análise do fluxo de entrada e saída permite relacionar o protótipo a diferentes conceitos de Arquitetura de Computadores.

**Entrada de dados:** o sensor representa uma fonte externa de informações que são convertidas em dados digitais.

**Processamento:** o ESP32 executa instruções embarcadas, enquanto o computador executa o programa em Python para interpretar e analisar as medições.

**Memória e armazenamento:** os dispositivos utilizam memória durante a execução. O histórico permanente de temperaturas, por sua vez, depende de uma solução de armazenamento implementada pelo projeto.

**Comunicação:** a interface 1-Wire permite a troca de informações entre o sensor e o ESP32, enquanto o Wi-Fi possibilita a comunicação entre o microcontrolador e o computador.

**Saída de informações:** os resultados são apresentados por meio do programa, permitindo que o usuário acompanhe os valores e as condições identificadas.

**Controle por software:** os programas determinam a sequência de operações, os critérios de processamento e as condições para produzir os resultados.

Esses elementos demonstram que o funcionamento do protótipo depende da interação entre hardware, software, memória e interfaces de comunicação.

A relação com MISD deve ser tratada com precisão: o projeto utiliza a análise de um mesmo dado como oportunidade para discutir o conceito de múltiplas instruções sobre um único fluxo de dados. Entretanto, a arquitetura física do ESP32 e do computador, bem como a execução das análises em Python, não devem ser classificadas como MISD sem evidências técnicas que comprovem essa organização.

## 12. Conclusão

A pesquisa sobre entrada, saída e interação com o ambiente permitiu detalhar como as informações são coletadas por um sensor, transmitidas por um microcontrolador e processadas por um programa computacional. Também evidenciou a importância das interfaces de comunicação, da programação dos dispositivos, da configuração dos parâmetros e da apresentação dos resultados.

O estudo demonstrou que o funcionamento do protótipo depende da integração entre componentes físicos e lógicos. O sensor fornece os dados iniciais, o ESP32 realiza a aquisição e a transmissão, e o Python executa as análises previstas pelo programa.

Além disso, foram identificadas limitações relacionadas à precisão das medições, à estabilidade da comunicação, ao armazenamento do histórico e aos critérios utilizados para interpretar os resultados.

Por fim, o protótipo oferece uma aplicação acadêmica para investigar o fluxo de informações em um sistema computacional e relacionar os conceitos de entrada, processamento, saída e interação entre hardware e software à disciplina de Arquitetura de Computadores.

## 13. Referências

1. ANALOG DEVICES. *DS18B20: Programmable Resolution 1-Wire Digital Thermometer.* Disponível em: https://www.analog.com/en/products/ds18b20.html. Acesso em: 20 out. 2026.

2. ESPRESSIF SYSTEMS. *ESP32 Series Datasheet.* Disponível em: https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf. Acesso em: 20 out. 2026.

3. PYTHON SOFTWARE FOUNDATION. *Python Documentation.* Disponível em: https://docs.python.org/3/. Acesso em: 20 out. 2026.

4. FLYNN, Michael J. *Some Computer Organizations and Their Effectiveness.* IEEE Transactions on Computers, v. C-21, n. 9, p. 948–960, 1972. DOI: https://doi.org/10.1109/TC.1972.5009071.


