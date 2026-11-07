# 3. CPU

**Grupo 3 | Objeto de estudo:** Monitoramento de temperatura com processamento MISD
**Disciplina:** Arquitetura e Organização de Computadores
**Professor:** Breno Felix Sousa

**Equipe:** Tiago Henrique Souza Lima, Emanuel Borges Vale, Elaine Cardoso de Souza Barros, Augusto Spolavori Siqueira, João Guilherme Alves de Souza Oliveira, João Victor Ferreira de Lima Moura

---

## 3.1 Processor Overview

Na taxonomia de Flynn, **MISD (Multiple Instruction, Single Data)** é a classe em que **várias unidades executam instruções diferentes sobre o mesmo dado**. No nosso projeto, o dado único é a **leitura de temperatura** do sensor. Três análises independentes (limite, histórico e anomalia) processam essa mesma leitura, e o resultado final é um estado do sistema: **normal, alerta ou anomalia**.

O objeto **não possui uma CPU única que concentre todo o processamento**. O trabalho é dividido entre dois níveis:

1. **ESP32**: coleta e envia a leitura.
2. **Computador com Python**: executa as três análises (as "unidades de instrução múltipla") e consolida o resultado.

### Unidades que realizam processamento

| Unidade | Onde roda | Função principal |
|---|---|---|
| **Sensor de temperatura** | Hardware externo ao ESP32 | Converte a grandeza física (temperatura) em sinal elétrico/valor medido |
| **ESP32** (unidade de aquisição) | Microcontrolador | Lê o sensor, associa o horário e envia o valor por Wi-Fi |
| **Análise de Limite** (instrução 1) | Python, no computador | Compara a temperatura atual com o valor máximo permitido |
| **Análise de Histórico** (instrução 2) | Python, no computador | Compara a temperatura atual com as leituras anteriores |
| **Análise de Anomalia** (instrução 3) | Python, no computador | Compara a temperatura atual com o padrão recente e identifica valores fora do padrão |
| **Unidade de decisão (consolidação)** | Python, no computador | Combina os três resultados e define o estado: normal, alerta ou anomalia |

### Tipos de operações realizadas

**ESP32 (aquisição e transmissão)**
- Leitura do sensor (conversão analógico-digital ou protocolo digital, conforme o sensor usado).
- Marcação do horário da leitura.
- Montagem e envio do pacote de dados por Wi-Fi.

**Análises em Python**

| Análise | Entrada (usa) | Operação | Saída (gera) |
|---|---|---|---|
| **Limite** | Temperatura atual | Comparação lógica (`temperatura > limite`) | Dentro ou acima do limite |
| **Histórico** | Temperatura atual + leituras anteriores | Subtração/comparação com leituras passadas | Variação: subiu, caiu ou estável |
| **Anomalia** | Temperatura atual + padrão recente | Comparação com o padrão recente (por exemplo, média e desvio das últimas leituras) | Normal ou fora do padrão |

**Consolidação:** regras lógicas combinam as três saídas. Exemplos com valores ilustrativos:

| Temperatura | Limite | Histórico | Anomalia | Resultado |
|---|---|---|---|---|
| 28 °C | Dentro | Estável | Normal | **Normal** |
| 36 °C | Acima | Estável | Normal | **Alerta** |
| 31 °C | Dentro | Subiu 8 °C | Fora do padrão | **Anomalia** |

> Os valores acima são apenas de exemplo, para ilustrar o funcionamento.

### Divisão de responsabilidades

- **O ESP32 não decide nada.** Ele só mede e envia, garantindo que todas as análises recebam **exatamente o mesmo dado**. Isso é o que caracteriza o *Single Data* do MISD.
- **Cada análise é independente.** Uma não depende do resultado da outra; cada uma aplica sua própria "instrução" sobre a mesma leitura.
- **A consolidação é a única etapa que enxerga todos os resultados** e produz a decisão final.

### Particularidades do objeto

- Não há uma CPU convencional única: o "processamento" é distribuído entre **hardware embarcado (ESP32)** e **software em computador (Python)**.
- As unidades MISD são **lógicas (software)**, não circuitos físicos separados.
- O objetivo do MISD aqui é a **confiabilidade da decisão**: um dado, várias análises, uma decisão confiável.
- Máquinas MISD "puras" são raras. O exemplo clássico da literatura é a **redundância nos computadores do Space Shuttle** (Sklaroff, 1976), em que unidades distintas processam o mesmo dado para aumentar a confiabilidade. Nosso projeto segue a mesma ideia.

---

## 3.2 Registers

O projeto não usa um conjunto de registradores de CPU tradicional. Os mecanismos equivalentes são:

| Mecanismo | Papel |
|---|---|
| **Registradores do ESP32** | Guardam temporariamente a leitura do sensor e os dados de comunicação durante a aquisição e o envio |
| **Variável da leitura atual** | Armazena o dado único compartilhado pelas três análises |
| **Estrutura de histórico** (lista ou buffer das últimas leituras) | Equivale a uma memória de curto prazo, usada pelo histórico e pela anomalia |
| **Variáveis de resultado** | Cada análise guarda a própria saída (dentro/acima, subiu/caiu/estável, normal/fora do padrão) |
| **Estado do sistema e registro de alertas** | Armazenam o resultado consolidado e o log de eventos |

---

## 3.3 ALU

Não há uma ALU única. As operações aritméticas e lógicas estão distribuídas entre as análises:

- **Aritméticas:** diferenças entre leituras (análise de histórico) e cálculos estatísticos simples, como média e desvio (análise de anomalia).
- **Lógicas:** comparação com o limite e regras de consolidação do estado.
- **Especializadas:** cada análise é uma unidade especializada em um tipo de verificação. O **ESP32** também é especializado, pois integra conversor analógico-digital e rádio Wi-Fi.

---

## 3.4 Control Unit

- **Controle no ESP32:** programa (firmware) que define o intervalo entre leituras e o envio dos dados.
- **Controle no computador:** programa em Python que recebe o dado, aciona as três análises e consolida o resultado.
- **Parâmetros de controle:** limite de temperatura permitido, tamanho da janela do histórico e critério de anomalia.

---

## 3.5 Clock

**Temporização:** o ESP32 funciona com seu clock interno e define o intervalo entre as leituras. O computador tem o seu próprio clock e processa cada leitura quando ela chega. Os dois clocks são independentes.

**Sincronização**
- A leitura é associada a um **horário**, que serve de referência para o histórico e para ordenar os eventos.
- As três análises **precisam receber a mesma leitura** antes de a consolidação ser feita: o resultado final só é calculado quando as três saídas daquela leitura estão disponíveis.
- A comunicação entre ESP32 e computador é **assíncrona** (por Wi-Fi), então o computador processa as leituras na ordem em que chegam.

**Paralelismo**
- **Conceitual:** o modelo é MISD, ou seja, três fluxos de instruções diferentes atuando sobre um único fluxo de dados.
- **Na implementação:** as três análises podem rodar em paralelo (threads ou processos) ou em sequência sobre a mesma leitura. Nos dois casos o modelo lógico continua sendo MISD, pois as análises são independentes e usam o mesmo dado.
- **No hardware:** o ESP32 possui dois núcleos de processamento, mas, neste projeto, a divisão principal de tarefas está nas análises em Python.

---

## 3.6 Instruction Cycle

**Explain**

Aqui o ciclo de instrução corresponde ao caminho de **uma leitura de temperatura**, do sensor até o estado final do sistema. A cada nova leitura o ciclo se repete.

**Fetch**

Busca do dado: o ESP32 lê o sensor, associa o horário e envia o valor por Wi-Fi. O programa em Python recebe o pacote no computador.

**Decode**

Interpretação do dado: o Python extrai do pacote a temperatura, o horário e a origem, e entrega **o mesmo dado** às três análises (limite, histórico e anomalia).

**Execute**

Execução: as três análises processam a leitura, cada uma com sua instrução. A unidade de decisão combina os três resultados e define o estado (normal, alerta ou anomalia), que é atualizado no gráfico das leituras e no registro de alertas.

---

## 3.7 CPU Diagram

```mermaid
flowchart TD
    S["Sensor de temperatura<br/>mede a temperatura"]
    E["ESP32<br/>lê, marca o horário e envia"]
    W["Wi-Fi<br/>transmite ao computador"]
    P["Python<br/>recebe o dado único"]

    S --> E --> W --> P

    P -->|"mesmo dado"| L["Análise de LIMITE<br/>dentro ou acima do limite"]
    P -->|"mesmo dado"| H["Análise de HISTÓRICO<br/>subiu, caiu ou estável"]
    P -->|"mesmo dado"| A["Análise de ANOMALIA<br/>normal ou fora do padrão"]

    L --> D["Unidade de decisão<br/>combina os resultados"]
    H --> D
    A --> D

    D --> R["Estado do sistema:<br/>NORMAL | ALERTA | ANOMALIA"]
    R --> G["Gráfico das leituras"]
    R --> LOG["Registro de alertas"]
```

**Mapeamento com a taxonomia de Flynn:**

```mermaid
flowchart LR
    SD["Single Data<br/>(temperatura)"] --> I1["Instrução 1<br/>Limite"]
    SD --> I2["Instrução 2<br/>Histórico"]
    SD --> I3["Instrução 3<br/>Anomalia"]
    I1 --> O["Resultado<br/>consolidado"]
    I2 --> O
    I3 --> O
```

---

## 3.8 Advantages

- **Decisão mais confiável:** o mesmo dado é verificado de três formas diferentes antes de o sistema indicar um estado.
- **Independência das análises:** cada análise pode ser alterada, testada ou corrigida sem afetar as outras.
- **Fácil de expandir:** é possível acrescentar uma nova análise sobre o mesmo dado sem mudar a aquisição.
- **ESP32 leve:** o microcontrolador só mede e envia, e o processamento mais pesado fica no computador.
- **Dado consistente:** como todas as análises recebem a mesma leitura, não há divergência de entrada entre elas.
- **Boa para ensino:** o projeto mostra de forma clara o conceito de MISD na taxonomia de Flynn.

---

## 3.9 Limitations

- **Dependência de rede:** se o Wi-Fi falhar, o computador deixa de receber as leituras e as análises param.
- **Ponto único de falha na aquisição:** há um só sensor e um só ESP32; se o dado de entrada estiver errado, **as três análises usam o mesmo dado errado**. A redundância existe nas análises, não na medição.
- **Latência:** o tempo entre medir, transmitir e analisar limita a resposta "em tempo real".
- **Recursos do ESP32:** memória e capacidade de processamento limitadas, por isso a análise é feita no computador.
- **Qualidade do histórico e da detecção de anomalia:** dependem da quantidade de leituras anteriores e do critério escolhido (por exemplo, o tamanho da janela).
- **Ruído do sensor:** pode gerar falsos alertas ou falsas anomalias.

---

## Referências

- FLYNN, M. J. Very high-speed computing systems. *Proceedings of the IEEE*, v. 54, n. 12, 1966.
- SKLAROFF, J. R. Redundancy management technique for Space Shuttle computers. *IBM Journal of Research and Development*, v. 20, n. 1, 1976.
- TANENBAUM, A. S.; AUSTIN, T. *Organização Estruturada de Computadores*. 6. ed. Pearson, 2013.
- STALLINGS, W. *Arquitetura e Organização de Computadores*. 10. ed. Pearson, 2017.
- PATTERSON, D. A.; HENNESSY, J. L. *Organização e Projeto de Computadores*. 5. ed. Elsevier, 2017.
- ESPRESSIF SYSTEMS. *ESP32 Series Datasheet*. Disponível em: espressif.com.

**Autores e editoras brasileiras**

- WEBER, R. F. *Fundamentos de Arquitetura de Computadores*. 4. ed. Porto Alegre: Bookman, 2012.
- MONTEIRO, M. A. *Introdução à Organização de Computadores*. 5. ed. Rio de Janeiro: LTC, 2007.
- BORGES, G. P. *Arquitetura e Organização de Computadores: Uma Introdução*. Rio de Janeiro: LTC, 2024.
