# 4. Memory Organization

## 4.1 Organização do armazenamento

Na organização Harvard clássica, instruções e dados ocupam espaços de armazenamento separados. A separação é a característica central do modelo; capacidade, tecnologia, largura, latência e permissões de leitura e escrita são decisões da implementação. Caminhos independentes podem permitir acessos concorrentes, mas não garantem que eles ocorram simultaneamente (Arvind et al., 2015, cap. 5).

## 4.2 Armazenamento de programa e instruções

O espaço de instruções contém o programa que o processador busca para executar. Uma implementação concreta precisa de algum mecanismo para selecionar a próxima instrução; esse mecanismo pode incluir um contador de programa, mas Harvard não especifica um PC, um registrador de instrução ou uma sequência de controle particular. Tais elementos pertencem ao projeto do processador e à sua microarquitetura.

## 4.3 Memória de dados e armazenamento temporário

O espaço de dados contém operandos, valores de entrada e resultados. A implementação pode também usar registradores ou outros armazenamentos internos temporários; esses elementos não são sinônimos de memória RAM e sua quantidade e organização não são determinadas por Harvard. O caminho de dados é usado quando uma instrução concreta solicita uma leitura ou escrita.

## 4.4 Exemplo histórico: Harvard Mark I

O Mark I era uma máquina eletromecânica específica, não uma implementação eletrônica do modelo abstrato atual. A coleção de instrumentos científicos de Harvard descreve 72 contadores mecânicos de armazenamento e o controle de sequência que lia instruções em fita de papel, uma linha por vez. Esses fatos ilustram escolhas daquela máquina e não devem ser atribuídos a toda implementação Harvard (Harvard University, Collection of Historical Scientific Instruments, “Function”).

## 4.5 Tecnologia, capacidade e acesso

| Referência | O que se pode afirmar | O que depende da implementação |
|---|---|---|
| Modelo Harvard | Há espaços distintos para instruções e dados. | Capacidade, tecnologia, largura, latência e simultaneidade efetiva. |
| Harvard Mark I | Usava fita de papel para a sequência de instruções e contadores mecânicos para valores. | Esses meios não são requisitos de Harvard eletrônica. |
| AVR documentado | É um exemplo de projeto concreto com memória de programa e memória de dados separadas. | Instruções, registradores, capacidade e tempos do AVR não são regras gerais do modelo (Microchip Technology Inc., AVR documentation). |

Não atribuímos taxas de execução ou capacidade ao modelo abstrato. Números de desempenho só são informativos quando associados a uma máquina, uma tarefa e uma métrica identificáveis.

## 4.6 Aplicabilidade

Ao analisar armazenamento, separe o espaço conceitual do mecanismo físico que o implementa. Uma fita, contador mecânico, memória Flash, SRAM, registrador ou cache descreve uma escolha histórica ou de projeto. Não se deve presumir que o espaço livre de uma memória possa ser transferido para o outro, nem afirmar que instruções nunca possam ser lidas como dados: ambas as conclusões dependem da implementação e da interface oferecida (Arvind et al., 2015; Pawson, 2022).