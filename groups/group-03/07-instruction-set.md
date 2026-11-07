# Arquitetura MISD : especificação de operações na arquitetura MISD

## Introdução

A MISD (Multiple Instruction, Single Data) não tem ISA própria. É uma classe da taxonomia de Flynn, definida pela multiplicidade dos fluxos de instruções e de dados (Flynn, 1972). Por isso cada máquina dessa classe é programada de um jeito diferente, e este documento responde a cada requisito por caso representativo:

- **Computadores de voo do ônibus espacial:** sistema redundante de aviônica.
- **Arranjo sistólico**, com o TPU do Google como exemplo moderno.
- **Protótipo do Grupo 03:** sensor DS18B20, ESP32 e scripts Python.

## 1. Como o sistema recebe a definição do que deverá executar

Em uma MISD, a definição chega como vários conjuntos de instruções ou funções, um por unidade, todos aplicados ao mesmo dado.

- **Ônibus espacial:** o software de voo (PASS) é carregado de memória em massa e a tripulação escolhe o programa por teclado. Quatro computadores executam código idêntico e um quinto roda software independente (BFS). Diferenças entre missões entram como constantes de dados (Spector e Gifford, 1984).
- **Arranjo sistólico:** a função vem do projeto das células e do fluxo de dados. Os pesos são pré-carregados nas células e os dados entram pela borda (Kung, 1982).
- **TPU:** o servidor hospedeiro envia as instruções por PCIe a um buffer de instruções. O programa nasce de um modelo TensorFlow que o driver compila (Jouppi et al., 2017).
- **Protótipo:** scripts Python definem as três análises, o firmware do ESP32 define leitura e envio, e o ESP32 manda comandos de 8 bits ao DS18B20 pelo barramento 1-Wire (Analog Devices, DS18B20).

## 2. Existência de instruções e de ISA

Cada componente tem uma situação diferente em relação a instruções:

- **Computador de voo do ônibus espacial:** tem instruções. Usa o processador AP-101, programas em HAL/S e sistema operacional em assembler.
- **Célula de arranjo sistólico clássico:** não tem instruções. Executa uma operação simples fixa por célula, como `y ← y + w·x`.
- **TPU:** tem instruções, com ISA do tipo CISC e cerca de uma dúzia de instruções.
- **ESP32:** tem instruções, com ISA Xtensa (núcleo LX6 de 32 bits) e instruções de 16 e 24 bits.
- **DS18B20:** não tem instruções, só comandos. São 11 comandos de 8 bits (5 de ROM e 6 de função).
- **Scripts Python:** não se aplica. São funções em texto-fonte, executadas por processador convencional.

**ISA Xtensa (ESP32).** O núcleo usa instruções de 24 bits e a opção de densidade de código acrescenta instruções de 16 bits (Cadence, Xtensa ISA Summary). O datasheet do ESP32 informa pipeline de 7 estágios, FPU e instruções DSP (Espressif, ESP32 Datasheet).

**ISA do TPU.** As instruções seguem a tradição CISC e incluem um campo de repetição, porque chegam por um barramento PCIe relativamente lento (Jouppi et al., 2017).

**Conceitos não aplicáveis:**

- **ISA da MISD:** não existe, porque a classe descreve fluxos e não um conjunto de instruções.
- **Instruções por célula no arranjo sistólico clássico:** as células não buscam instruções, e a função fica definida pelo projeto.
- **Instruções no DS18B20:** o sensor recebe comandos de 8 bits e não executa um programa.
- **ISA no servidor do protótipo:** o processador do servidor (referência: Intel Xeon) é convencional e não é MISD. O comportamento MISD vem das três análises sobre o mesmo valor.

## 3. Tipos de operações suportadas

**DS18B20: conjunto completo de comandos** (Analog Devices, DS18B20):

- **Search ROM (ROM, `F0h`):** identifica os códigos ROM de todos os dispositivos no barramento.
- **Read ROM (ROM, `33h`):** lê o código de 64 bits; só com um dispositivo no barramento.
- **Match ROM (ROM, `55h`):** endereça um dispositivo pelo código de 64 bits.
- **Skip ROM (ROM, `CCh`):** endereça todos os dispositivos sem enviar código.
- **Alarm Search (ROM, `ECh`):** só responde quem tem o alarme ativo.
- **Convert T (função, `44h`):** inicia uma conversão de temperatura.
- **Write Scratchpad (função, `4Eh`):** grava TH, TL e configuração (3 bytes).
- **Read Scratchpad (função, `BEh`):** lê até 9 bytes, incluindo o CRC.
- **Copy Scratchpad (função, `48h`):** copia TH, TL e configuração para a EEPROM.
- **Recall E2 (função, `B8h`):** recupera TH, TL e configuração da EEPROM.
- **Read Power Supply (função, `B4h`):** informa se o dispositivo usa alimentação parasita.

**Demais casos:**

- **Scripts Python do protótipo:** três análises sobre a mesma leitura: verificação de limite, comparação com o histórico e detecção de anomalias.
- **ESP32 (Xtensa):** carga, armazenamento, salto e chamada, desvio condicional, movimentação, aritmética, lógica, deslocamento e controle do processador. As opções incluem laços sem sobrecarga, MAC16, multiplicação e divisão de 32 bits e ponto flutuante (Cadence, Xtensa ISA Summary).
- **TPU:** cinco instruções principais (`Read_Host_Memory`, `Read_Weights`, `MatrixMultiply/Convolve`, `Activate` e `Write_Host_Memory`), mais configuração, sincronização, `nop` e `halt` (Jouppi et al., 2017).
- **Arranjo sistólico:** multiplicar e acumular em cada célula, aplicado a filtros, convolução, DFT e multiplicação de matrizes (Kung, 1982).
- **Ônibus espacial:** guiagem, navegação e controle de voo, mais operações de redundância, como seleção do valor médio entre sensores e votação nos atuadores (Spector e Gifford, 1984).

## 4. Forma de codificação

A codificação muda a cada componente:

- **DS18B20 (comandos):** cada comando tem 8 bits e vai pelo fio do bit menos significativo ao mais significativo. Toda transação começa com um pulso de reset, e o sensor responde com um pulso de presença.
- **DS18B20 (dados):** a temperatura ocupa 16 bits em complemento de dois, com 0,0625 °C por unidade em 12 bits (`0191h` = +25,0625 °C). O código ROM tem 64 bits: família `28h`, número de série de 48 bits e CRC de 8 bits.
- **ESP32 (Xtensa):** instruções de 24 bits no núcleo e de 16 bits na opção de densidade de código, em formatos como RRR, RRI8, CALL, BRI8 e RRRN.
- **TPU:** a instrução `MatrixMultiply` tem 12 bytes: endereço do buffer unificado, endereço do acumulador, comprimento e o restante de opcode e flags.
- **Arranjo sistólico clássico:** a operação é lógica fixa da célula, e um bit de marcação sinaliza ao acumulador quando entregar o resultado e zerar.
- **Scripts Python:** não há codificação própria; a especificação é texto-fonte.

## 5. Operandos ou parâmetros

Os operandos variam conforme o componente:

- **ESP32 (Xtensa):** registradores gerais (AR) e valores imediatos, com os campos `r`, `s` e `t` escolhendo os registradores, como em `AR[r] ← f(AR[s], AR[t])`. Há registradores especiais para laços (`LBEG`, `LEND`, `LCOUNT`) e deslocamento (`SAR`).
- **TPU:** `MatrixMultiply` recebe endereço no buffer unificado, endereço no acumulador e comprimento. Os pesos formam blocos de 256×256 valores de 8 bits.
- **Ônibus espacial:** milhares de parâmetros de reconfiguração, como coeficientes dos polinômios de controle da subida.
- **Scripts Python do protótipo:** valor do limite (°C), tamanho da janela de histórico e critério de anomalia.
- **DS18B20:** `Match ROM` recebe o código ROM de 64 bits, e `Write Scratchpad` recebe 3 bytes.

**Scratchpad do DS18B20 (9 bytes):**

- **Byte 0:** temperatura, byte menos significativo (não gravável).
- **Byte 1:** temperatura, byte mais significativo (não gravável).
- **Byte 2:** TH (alarme alto) ou byte de usuário 1 (gravável).
- **Byte 3:** TL (alarme baixo) ou byte de usuário 2 (gravável).
- **Byte 4:** registrador de configuração, com os bits R1 e R0 (gravável).
- **Bytes 5 a 7:** reservados (não graváveis).
- **Byte 8:** CRC dos bytes 0 a 7 (não gravável).

**Resolução (bits R1 e R0):**

- **0 0:** 9 bits, tempo máximo de conversão de 93,75 ms.
- **0 1:** 10 bits, tempo máximo de conversão de 187,5 ms.
- **1 0:** 11 bits, tempo máximo de conversão de 375 ms.
- **1 1 (padrão):** 12 bits, tempo máximo de conversão de 750 ms.

## 6. Configuração necessária

Cada caso exige uma configuração antes de rodar:

- **DS18B20:** alimentação de 3,0 a 5,5 V, ou modo parasita. O fio de dados precisa de resistor de pull-up externo (cerca de 4,7 kΩ). A resolução padrão é de 12 bits, e TH, TL e configuração ficam em EEPROM. O acesso exige sempre a sequência inicialização, comando de ROM e comando de função.
- **ESP32:** opera de 2,3 a 3,6 V (3,3 V recomendado), compatível com o DS18B20. O firmware é gravado pelo modo de download por UART (Espressif, ESP32 Datasheet).
- **Firmware 1-Wire no ESP32:** o exemplo da Espressif é configurado com `idf.py menuconfig`, onde se escolhe o backend do 1-Wire (RMT ou UART), o GPIO do barramento e o uso do pull-up interno (Espressif, ds18b20_read).
- **Arranjo sistólico e TPU:** os pesos são pré-carregados nas células. No TPU, o driver prepara a execução e grava a imagem de pesos na memória de pesos (Jouppi et al., 2017).
- **Ônibus espacial:** os dados de reconfiguração são preparados a cada voo, e a tabela que define qual computador comanda cada barramento pode ser alterada por teclado.
- **Python:** o módulo `queue` troca informação com segurança entre threads, e `multiprocessing.Queue` serve para processos (Python Software Foundation, queue). Rede Wi-Fi, endereço do servidor e protocolo de transporte ainda precisam ser definidos pelo grupo.

## 7. Relação entre programação e hardware

Na MISD, programar significa dar instruções diferentes a unidades distintas que recebem o mesmo dado. O quanto disso fica em hardware ou em software varia por caso.

- **Ônibus espacial:** a redundância é obtida por software sobre computadores comerciais, e a votação em hardware fica nas saídas (atuadores) (Spector e Gifford, 1984).
- **Arranjo sistólico:** o hardware carrega o algoritmo, porque o projeto mapeia a computação em células e fluxos de dados (Kung, 1982). No TPU, o software não precisa conhecer a natureza sistólica da unidade para estar correto, mas precisa considerar a latência dela para ter bom desempenho.
- **Protótipo:** a especificação desce em quatro camadas, e só a de cima aplica várias funções ao mesmo dado.

```
 PYTHON (servidor): define as 3 análises
        │
        ▼
 Wi-Fi: transporta a leitura
        │
        ▼
 FIRMWARE DO ESP32: define leitura e envio
        │
        ▼
 BARRAMENTO 1-WIRE: comandos de 8 bits
        │
        ▼
 DS18B20: mede a temperatura
```

## 8. Limitações do modelo de programação

A limitação comum é depender de um único fluxo de dados bem distribuído e sincronizado entre as unidades.

- **Ônibus espacial:** como os quatro computadores rodam código idêntico, um erro de software atinge todos. O quinto computador (BFS), com software independente, existe para esse caso (Spector e Gifford, 1984).
- **Arranjo sistólico:** serve a problemas regulares e limitados por computação, e a banda de E/S limita o desempenho (Kung, 1982).
- **TPU:** o modelo é determinístico e de fluxo único, sem caches, previsão de desvios ou multithreading, e serve a matrizes densas (Jouppi et al., 2017).
- **DS18B20:** o sensor não responde se a sequência de inicialização, comando de ROM e comando de função for quebrada. Skip ROM seguido de leitura só funciona com um único dispositivo no barramento, e o CRC é verificado pelo mestre. A conversão leva até 750 ms em 12 bits, o que limita a cerca de 1,3 leituras por segundo (cálculo a partir do tempo de conversão).
- **ESP32 (Xtensa):** o manual completo da ISA só está disponível a licenciados da Cadence; publicamente há apenas o resumo.
- **Protótipo em Python:** as três análises rodam em um processador convencional, então a MISD é lógica. O exemplo da seção 9 usa filas e espera as três análises terminarem antes de atualizar o histórico (decisão de projeto do grupo).

## 9. Exemplo concreto

**Sequência 1-Wire para uma leitura** (um DS18B20 no barramento, adaptada do datasheet da Analog Devices):

1. O ESP32 envia o pulso de reset, e o DS18B20 responde com o pulso de presença.
2. Envia `CCh` (Skip ROM) e depois `44h` (Convert T). A conversão leva até 750 ms em 12 bits.
3. Envia outro reset, recebe a presença, e envia `CCh` e `BEh` (Read Scratchpad).
4. Lê 9 bytes e recalcula o CRC dos 8 primeiros; se divergir do nono, repete a leitura.
5. Monta a temperatura com os bytes 0 e 1. Por exemplo, `91h` e `01h` formam `0191h`, que vale 401 ÷ 16 = +25,0625 °C.

**Funções Python que recebem o mesmo valor.** O código é um exemplo meramente ilustrativo e não deve ser pensado como projeção do código final, e o limite de 30 °C e a janela de 60 leituras são valores de exemplo. Ele foi executado e as três análises recebem o mesmo valor.

```python
import queue
import threading
from collections import deque
from statistics import mean, pstdev

LIMITE_C = 30.0
historico = deque(maxlen=60)
# Lock para proteger o acesso ao histórico partilhado
historico_lock = threading.Lock()

def analisa_limite(t):
    return "ALERTA" if t > LIMITE_C else "OK"

def analisa_historico(t):
    # Garante acesso exclusivo para calcular a média de forma segura
    with historico_lock:
        if not historico:
            return 0.0
        media = mean(historico)
    return t - media

def analisa_anomalia(t):
    # Captura uma "fotografia" estável dos dados para os dois cálculos estatísticos
    with historico_lock:
        if len(historico) < 10:
            return False
        media = mean(historico)
        desvio = pstdev(historico)
        
    return abs(t - media) > 3 * (desvio or 0.1)

ANALISES = {"limite": analisa_limite, 
            "historico": analisa_historico, 
            "anomalia": analisa_anomalia}
filas = {nome: queue.Queue() for nome in ANALISES}

def trabalhador(nome, funcao):
    while True:
        t = filas[nome].get()
        resultado = funcao(t)
        print(nome, resultado)
        filas[nome].task_done()

for nome, funcao in ANALISES.items():
    threading.Thread(target=trabalhador, args=(nome, funcao), daemon=True).start()

def nova_leitura(t):           # chamada a cada leitura que chega do ESP32
    for fila in filas.values():
        fila.put(t)            # o MESMO valor vai para as três análises
    for fila in filas.values():
        fila.join()            # espera as três terminarem
        
    # Atualiza a estrutura global com proteção de escrita
    with historico_lock:
        historico.append(t)
```

**Exemplos característicos dos outros casos:**

- **Célula de arranjo sistólico:** a cada ciclo, `y_saída ← y_entrada + w · x_entrada` e `x_saída ← x_entrada`.
- **TPU:** uma instrução `MatrixMultiply` de 12 bytes multiplica um bloco B×256 da entrada pela matriz de pesos 256×256 e acumula o resultado.
- **ESP32:** `idf.py menuconfig` abre as opções do exemplo, onde se define o GPIO do barramento 1-Wire.

## 10. Fontes

- ANALOG DEVICES. DS18B20 programmable resolution 1-Wire digital thermometer: datasheet. [S. l.]: Analog Devices. Disponível em: https://wcm-cce.cldnet.analog.com/media/cn/technical-documentation/data-sheets/2812.pdf.

- CADENCE. Xtensa instruction set architecture (ISA) summary. [S. l.]: Cadence. Disponível em: https://cdn.hackaday.io/files/1960678388530528/isa-summary.pdf.

- ESPRESSIF. ESP32 series datasheet. [S. l.]: Espressif Systems. Disponível em: https://documentation.espressif.com/esp32_datasheet_en.pdf.

- ESPRESSIF. ds18b20_read: exemplo do componente ds18b20. [S. l.]: Espressif Systems. Disponível em: https://components.espressif.com/components/espressif/ds18b20/versions/0.4.0/examples/ds18b20_read.

- FLYNN, M. J. Some computer organizations and their effectiveness. IEEE Transactions on Computers, v. C-21, n. 9, p. 948–960, set. 1972. Disponível em: https://www.cs.utah.edu/~hari/teaching/paralg/Flynn72.pdf.

- JOUPPI, N. P. et al. In-datacenter performance analysis of a tensor processing unit. 2017. Disponível em: https://classes.engineering.wustl.edu/cse560m/papers/P1-Norman-1.pdf.

- KUNG, H. T. Why systolic architectures? Computer, IEEE, v. 15, n. 1, p. 37–46, jan. 1982. Disponível em: https://www.cs.virginia.edu/~smk9u/CS6501F19/kung_-_1982_-_why_systolic_architectures.pdf.

- PYTHON SOFTWARE FOUNDATION. queue: a synchronized queue class. In: The Python standard library. Disponível em: https://docs.python.org/3/library/queue.html.

- SPECTOR, A.; GIFFORD, D. The space shuttle primary computer system. Communications of the ACM, v. 27, n. 9, p. 874–900, set. 1984. DOI: 10.1145/358234.358246. Disponível em: https://eklausmeier.goip.de/pdf/shuttle_primary_computer_system.pdf.
