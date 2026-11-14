# 3. CPU
Sem chip central: Formado por uma rede distribuída de até 2.400 válvulas termiônicas (tubos de vácuo).
Cálculo instantâneo: Executava operações lógicas (XOR) na velocidade da luz para cruzar os dados.
Lógica cabeada: Não lia linhas de código; o processamento era guiado pela fiação física dos painéis (plugboards).
Paralelismo real: Processava 5 bits simultaneamente (um caractere completo por ciclo).

## 3.1 Processor Overview
Estrutura: era formado por uma rede distribuída de até 2.400 válvulas termiônicas.
Função: Executar cálculos lógicos (XOR) na velocidade da luz para decifrar padrões.
Velocidade: Operava a 5 kHz (5.000 operações simultâneas por segundo).
Controle: a lógica do processamento era ditada pela fiação manual de cabos.
Paralelismo: Processava 5 bits ao mesmo tempo (um caractere completo por ciclo de clock).

## 3.2 Registers
Capacidade: Extremamente limitada; armazenavam apenas bits individuais (estados 0 ou 1).
Função Principal: Simular e memorizar eletronicamente as posições das rodas da máquina de criptografia alemã (Lorenz) a cada ciclo.
Volatilidade: Totalmente voláteis; os dados desapareciam instantaneamente assim que a máquina era desligada.

## 3.3 ALU
Não possuía um Registrador de Instrução (IR) - A "instrução" do computador era codificada fisicamente na posição dos cabos nos painéis (plugboards) e chaves manuais.

## 3.4 Control Unit
Válvulas termiônicas e relés que coordenavam o andamento dos testes.
Sem Decodificador: Não lia ou decodificava instruções de software; o comportamento do circuito era fixado pela fiação dos painéis de cabos (plugboards).
Função Principal: Sincronizar o fluxo de dados controlando o ritmo do leitor óptico (através do sinal de clock de 5 kHz).
Tomada de Decisão: Monitorava os contadores eletrônicos e decidia de forma autônoma quando interromper o processamento para enviar o resultado à impressora.

## 3.5 Clock (gerador de impulsos)
Era o coração do sincronismo da máquina:
Frequência: Operava a 5 kHz (5.000 ciclos por segundo).
Origem: Gerado pelos furos centrais da própria fita de papel perfurado no leitor óptico.
Função: Sincronizar a leitura dos dados da fita com o cálculo das válvulas (CPU).
Limite: A velocidade era limitada pelo estresse físico da fita de papel, que girava a 50 km/h.

## 3.6 Instruction Cycle
Não tinha programas armazenados na memória, seu ciclo de operação era contínuo e baseado puramente em hardware:
1. Leitura (Clock): Um furo central na fita de papel aciona o pulso de sincronismo (5 kHz).
2. Entrada de Bits: O leitor óptico captura os furos da mensagem e os envia via cabos.
3. Processamento Combinatório: As válvulas realizam instantaneamente a operação lógica (XOR) entre a mensagem e o padrão interno.
4. Acúmulo: Os contadores baseados em válvulas somam o resultado do teste.
5. Avaliação/Saída: A unidade de controle verifica se a pontuação atingiu a meta; se sim, interrompe o ciclo e imprime a resposta.
Explain

Fetch

Decode

Execute

## 3.7 CPU Diagram

(Add image)

## 3.8 Advantages
Velocidade Incomparável: Reduziu o tempo de quebra de códigos complexos de semanas para poucas horas.
Eletrônica Pura: O uso de válvulas termiônicas eliminou os atrasos causados por peças mecânicas móveis.
Automação Estatística: Analisava e somava milhares de combinações de caracteres de forma totalmente automática.
Leitura Óptica Eficiente: Processava dados na velocidade física limite de fitas de papel (5.000 caracteres por segundo).
Paralelismo Real: Conseguia processar múltiplos bits de dados de uma só vez.
## 3.9 Limitations
Propósito Único: Projetado exclusivamente para quebrar a cifra alemã Lorenz; não podia rodar outras tarefas (como contabilidade ou física).
Programação Manual: Sem software; para mudar qualquer lógica, era necessário desligar a máquina e refazer toda a fiação física de cabos e chaves.
Falta de Memória RAM: Não armazenava dados de programas; dependia da velocidade física da fita de papel girando nas polias para ler a mensagem.
Gargalo Mecânico: Embora a eletrônica fosse na velocidade da luz, a velocidade total era limitada pela resistência do papel da fita, que se rompia se passasse de 50 km/h.
Instabilidade do Hardware: As milhares de válvulas termiônicas geravam calor extremo, queimavam com frequência e exigiam manutenção diária rigorosa.
