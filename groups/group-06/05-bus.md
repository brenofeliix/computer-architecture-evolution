# 5. Bus Architecture

## 5.1 Bus Overview
A organização Harvard clássica separa a memória de programa da memória de dados e fornece caminhos independentes de comunicação entre a CPU e cada espaço de memória. Assim, a busca de instruções não precisa usar obrigatoriamente o mesmo caminho das transferências de dados. Em um diagrama de blocos, esses caminhos podem ser representados como uma interface para a memória de programa e outra para a memória de dados.

Os termos barramento de endereço, barramento de dados e barramento de controle ajudam a explicar as funções dos sinais em cada interface. Porém, a organização física desses sinais depende da implementação. A arquitetura Harvard, por si só, não define uma quantidade fixa de fios, uma largura de barramento ou um protocolo elétrico específico.

## 5.2 Data Bus
O caminho da memória de programa transporta instruções da memória de programa para a CPU. O caminho da memória de dados transporta valores entre a memória de dados e a CPU. Dependendo da operação, os dados podem sair da memória em uma leitura ou sair da CPU em uma gravação.

Manter esses caminhos independentes significa que a transferência de uma instrução não precisa disputar o mesmo caminho de dados usado por uma operação na memória de dados. Isso descreve uma possibilidade da organização; a implementação determina se as transferências ocorrem ao mesmo tempo.

## 5.3 Address Bus
A CPU identifica uma posição na memória de programa quando busca uma instrução. Ela identifica uma posição na memória de dados quando uma operação precisa ler ou gravar um dado. Na organização Harvard clássica, esses acessos pertencem a espaços de memória separados e usam caminhos de comunicação independentes.

Por isso, um endereço da memória de programa e um endereço da memória de dados pertencem a espaços diferentes. A arquitetura não exige que os dois espaços usem a mesma faixa de endereços ou a mesma largura de endereço. Esses detalhes dependem da implementação.

## 5.4 Control Bus
A Unidade de Controle (UC) é responsável por interpretar as instruções recebidas pelo processador e gerar sinais elétricos que coordenam o funcionamento dos componentes internos. Esses sinais controlam o fluxo de informações, ativando os elementos necessários para a execução de cada instrução. Na arquitetura Harvard, a UC organiza as operações de leitura e escrita na memória por meio de sinais específicos, garantindo que cada componente execute sua função corretamente.

Os principais sinais de controle da memória de dados são o MemRead, responsável por permitir a leitura de informações armazenadas na memória, e o MemWrite, que permite gravar dados na memória. Dessa forma, a arquitetura Harvard utiliza memórias e barramentos separados para instruções e dados, possibilitando acessos independentes e, em determinadas situações, simultâneos. Isso contribui para aumentar a velocidade e a eficiência do processamento, sendo especialmente útil em microcontroladores, processadores digitais de sinais (DSPs) e sistemas de tempo real.

## 5.5 Communication Process
Um fluxo típico de instruções e dados pode ser descrito assim:

1. O contador de programa identifica o endereço da próxima instrução na memória de programa.
2. A interface da memória de programa envia essa instrução à CPU.
3. A CPU decodifica a instrução e determina qual operação deve realizar.
4. Se a operação precisar de um valor da memória de dados, a CPU seleciona um endereço e solicita uma leitura.
5. A memória de dados devolve o valor à CPU, que pode processá-lo em seus registradores ou na ULA.
6. Se a operação precisar gravar um resultado na memória de dados, a CPU envia o resultado e o endereço de destino pela interface dessa memória.

Como os caminhos de programa e de dados são independentes, uma implementação pode sobrepor a busca de uma instrução a uma transferência de dados. A sequência e o momento dessa sobreposição dependem do projeto do processador. O diagrama representa a organização, e não uma sequência de ciclos garantida.

## 5.6 Bus Width
A organização Harvard não define uma largura universal para os barramentos. A largura do caminho da memória de programa e a do caminho da memória de dados dependem da implementação do processador. Elas podem ser iguais ou diferentes.

Se o grupo apresentar valores de largura, deve associá-los a um processador ou sistema específico e usar a documentação desse sistema como fonte. Sem escolher uma implementação, a conclusão correta é que essa característica depende do projeto.

## 5.7 Performance Impact

O principal impacto da Arquitetura de Harvard no desempenho de um processador é a eliminação do chamado "Gargalo de von Neumann". Como existem barramentos físicos separados para a memória de programa (instruções) e para a memória de dados, a CPU pode realizar a busca de uma instrução e acessar um dado (leitura ou gravação) simultaneamente, no mesmo ciclo de clock.

## 5.8 Comparison with Current Architectures

Hoje em dia, a arquitetura Harvard normal, é usada principalmente em chips mais simples e específicos, como microcontroladores (encontrados em sistemas embarcados e placas como o Arduino).

Já os processadores modernos de uso geral (computadores, servidores e celulares) utilizam a Arquitetura Harvard Modificada, que mistura duas ideias para obter o melhor custo-benefício:

Por dentro do processador: Funciona como Harvard. O processador possui memórias super rápidas (Caches L1) separadas para as instruções e para os dados. Isso permite que ele leia os dois ao mesmo tempo, mantendo a alta velocidade.

Por fora  : Funciona como von Neumann, as instruções e os dados se misturam na mesma memória principal (RAM) e viajam pelo mesmo barramento. Isso simplifica a construção da placa-mãe e barateia o sistema.
