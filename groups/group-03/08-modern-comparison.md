# 8. INTEGRAÇÃO, LIMITAÇÕES E DESEMPENHO

**Entrega 08 — Issue #26**

## 8.1 Introdução

Nesta etapa, são analisados os componentes que integram o protótipo de monitoramento de temperatura, considerando a comunicação entre eles, a organização do processamento, o armazenamento das informações e o desempenho esperado. Também são discutidas as limitações da arquitetura e as medidas adotadas para reduzir seus possíveis impactos. O objeto de estudo é um protótipo acadêmico de monitoramento de temperatura, composto por um sensor conectado a um microcontrolador ESP32, que transmite as leituras por Wi-Fi a um computador local operando como servidor. No computador, uma aplicação em Python disponibiliza a mesma leitura a três rotinas independentes de análise (limites, histórico e anomalias). Os resultados são consolidados, gravados em um banco de dados MySQL e apresentados em um dashboard.

O projeto toma como referência conceitual a categoria MISD (*Multiple Instruction, Single Data*) da Taxonomia de Flynn (FLYNN, 1966; 1972), na qual diferentes fluxos de instruções atuam sobre um único fluxo de dados. É importante destacar que o sistema foi desenvolvido como um protótipo acadêmico. Portanto, sua arquitetura utiliza o conceito MISD como referência para organizar as análises em software, não representando um processador MISD físico nem um equipamento industrial certificado. As três análises são unidades lógicas implementadas em software, e o MISD funciona como modelo de organização para explicar e estruturar o sistema.

Os componentes foram selecionados e a arquitetura foi definida, porém a validação experimental ainda será realizada na etapa de testes. Por essa razão, este documento adota análise qualitativa fundamentada nas características técnicas dos componentes e **não apresenta métricas medidas** de latência, vazão, perda de pacotes ou consumo de memória. As capacidades citadas (520 KiB, 4 MB, 16 GiB e 512 GB) são nominais ou previstas, e não indicam a parcela efetivamente disponível à aplicação.

As decisões técnicas adotadas são: sensor DS18B20, ligado ao ESP32 pela interface One-Wire; transmissão por Wi-Fi em requisições HTTP POST com corpo JSON, sobre TCP; recepção por um servidor HTTP em Python; execução das três análises em sequência; e identificação de cada amostra por um contador de 32 bits e por um tempo de aquisição. As descrições de funcionamento deste documento correspondem a essa arquitetura definida e projetada. Os mecanismos descritos (buffers, filas, transações e contingência) são apresentados como funcionamento projetado, e seu desempenho e sua confiabilidade ainda dependem dos testes de homologação; por isso, nenhum resultado medido é apresentado nesta entrega.

O documento investiga: como os componentes cooperam; que características favorecem o funcionamento; quais são as limitações e os gargalos; que fatores determinam o desempenho; como se dão a comunicação, o armazenamento e o paralelismo; e, por fim, qual característica do sistema mais influencia sua capacidade de cumprir a finalidade para a qual foi projetado (seção 8.11).

---

## 8.2 Visão integrada da arquitetura

### 8.2.1 Organização dos componentes

A solução é composta por nove elementos: sensor de temperatura, ESP32, comunicação Wi-Fi, computador-servidor, aplicação Python, três rotinas de análise, unidade lógica de consolidação, banco de dados relacional e dashboard. A arquitetura definitiva utiliza **exclusivamente um sensor de temperatura**.

A divisão de responsabilidades segue um princípio: o microcontrolador, de recursos limitados, executa apenas aquisição e transmissão; o computador, com mais memória e capacidade de processamento, executa validação, análise, consolidação e persistência.

**Tabela 8.1 — Responsabilidades dos componentes**

| Componente | Responsabilidade | Observação |
|---|---|---|
| Sensor | Medir a temperatura (DS18B20, One-Wire) | Único ponto de medição |
| ESP32 | Ler o sensor, numerar a amostra e transmitir (1 Hz) | Não executa as análises |
| Wi-Fi | Transportar os pacotes (HTTP POST com JSON sobre TCP) | Comunicação assíncrona |
| Aplicação Python | Validar, enfileirar e distribuir a leitura | Distribuição lógica |
| Análise de limite | Comparar com o limite definido | Instrução 1 |
| Análise histórica | Comparar com medições anteriores | Instrução 2 |
| Análise de anomalia | Identificar desvios do padrão | Instrução 3 |
| Unidade de decisão | Definir NORMAL, ALERTA ou ANOMALIA | Após as três saídas |
| MySQL (InnoDB) | Guardar amostras e resultados | Transações ACID |
| Dashboard | Mostrar leituras e estados | HTML, CSS e JavaScript |

### 8.2.2 Fluxo de informações

A Figura 8.1 apresenta o caminho completo de uma leitura, da aquisição à visualização.

```text
+--------------------------------------+
|        SENSOR DE TEMPERATURA         |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|                ESP32                 |
|      aquisição e identificação       |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|                WI-FI                 |
|             transmissão              |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|     SERVIDOR / APLICAÇÃO PYTHON      |
|         validação do pacote          |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|        FILA DE ENTRADA (RAM)         |
+--------------------------------------+
                   |
     +-------------+-------------+
     |             |             |
     v             v             v
+----------+  +----------+  +----------+
|  LIMITE  |  |HISTÓRICO |  | ANOMALIA |
+----------+  +----------+  +----------+
     |             |             |
     +-------------+-------------+
                   |
                   v
+--------------------------------------+
|             CONSOLIDAÇÃO             |
|      NORMAL / ALERTA / ANOMALIA      |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|        FILA DE PREPARAÇÃO SQL        |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|             MYSQL / SSD              |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|              DASHBOARD               |
+--------------------------------------+
```

*Figura 8.1 — Arquitetura integrada e fluxo de dados. Fonte: elaborado pelo grupo.*

O fluxo é predominantemente linear até a fila de entrada e, a partir dela, divergente (uma leitura para três análises) e convergente (três resultados para uma decisão). Essa forma de "losango" é a assinatura estrutural do projeto e explica tanto seus benefícios quanto seus gargalos: a divergência exige distribuição consistente do dado, e a convergência exige sincronização.

---

## 8.3 Integração do processamento

### 8.3.1 Distribuição do dado único

Na Taxonomia de Flynn, as organizações são classificadas segundo o número de fluxos de instruções e de dados (FLYNN, 1972; TANENBAUM; AUSTIN, 2013). No MISD, vários fluxos de instruções operam sobre um único fluxo de dados. No protótipo, o fluxo de dados único é a leitura de temperatura, e os três fluxos de instruções são as rotinas de limite, histórico e anomalia. Trata-se de uma organização lógica inspirada no MISD, implementada por três rotinas de software que operam sobre a mesma amostra; o ESP32 e o computador não formam um processador MISD.

```text
+--------------------------------------+
|             SINGLE DATA              |
|        leitura de temperatura        |
+--------------------------------------+
                   |
     +-------------+-------------+
     |             |             |
     v             v             v
+----------+  +----------+  +----------+
| INSTR. 1 |  | INSTR. 2 |  | INSTR. 3 |
|  LIMITE  |  |HISTÓRICO |  | ANOMALIA |
+----------+  +----------+  +----------+
     |             |             |
     v             v             v
+----------+  +----------+  +----------+
|RESULT. 1 |  |RESULT. 2 |  |RESULT. 3 |
+----------+  +----------+  +----------+
     |             |             |
     +-------------+-------------+
                   |
                   v
+--------------------------------------+
|             CONSOLIDAÇÃO             |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|          ESTADO DO SISTEMA           |
+--------------------------------------+
```

*Figura 8.2 — Processamento MISD conceitual aplicado ao protótipo. Fonte: elaborado pelo grupo.*

Quando uma amostra chega, o Python foi projetado para validar o cabeçalho e inserir o registro lógico (identificador sequencial, tempo de aquisição, *timestamp* de recepção e temperatura) em uma fila na RAM. O registro é então disponibilizado às três análises como dado de entrada compartilhado, sem alteração durante o ciclo de processamento. Essa imutabilidade é uma decisão arquitetural relevante: como nenhuma análise modifica o registro, não há necessidade de exclusão mútua para leitura, e as três rotinas operam sobre exatamente o mesmo valor, o que elimina divergências de entrada dentro de um mesmo ciclo. As três análises são executadas em sequência para cada amostra, na ordem limite, histórico e anomalia, em um único fluxo de processamento.

As três análises possuem finalidades distintas:

- **Limites:** compara a temperatura atual a um limite predefinido. Com limite de 40 °C, uma leitura de 38 °C resulta em "dentro do limite" e uma de 42 °C em "acima do limite". É uma comparação pontual, sem memória.
- **Histórico:** compara a leitura com medições anteriores e permite observar se a temperatura está subindo, caindo ou relativamente estável. Depende da quantidade e da qualidade do histórico disponível.
- **Anomalias:** verifica se a leitura foge do padrão recente, podendo utilizar critérios estatísticos simples, como média, desvio e variação das últimas leituras. O resultado indica uma **possível** anomalia, não um diagnóstico definitivo.

A complementaridade entre elas é o ponto central: uma leitura de 31 °C está dentro do limite, mas um salto de 8 °C em relação ao histórico pode caracterizar comportamento anômalo. Nenhuma análise isolada capturaria os dois fenômenos.

### 8.3.2 Consolidação dos resultados

Uma unidade lógica de decisão combina as três saídas e define o estado final: NORMAL, ALERTA ou ANOMALIA. A Tabela 8.2 reproduz exemplos ilustrativos de funcionamento; seus valores não são resultados de testes.

**Tabela 8.2 — Exemplos ilustrativos de consolidação**

| Temperatura | Limite | Histórico | Anomalia | Resultado |
|---|---|---|---|---|
| 28 °C | Dentro | Estável | Normal | NORMAL |
| 36 °C | Acima | Estável | Normal | ALERTA |
| 31 °C | Dentro | Subiu 8 °C | Fora do padrão | ANOMALIA |

A consolidação ocorre somente depois que as três análises da **mesma amostra** terminam. Como a execução é sequencial, essa condição é garantida pela própria ordem das chamadas, sem necessidade de sincronização entre *threads*. O tempo de ciclo de uma amostra é a soma dos tempos das três análises e da consolidação. Como as rotinas são computacionalmente simples, espera-se que esse tempo seja dominado pelas operações de entrada e saída (consulta ao histórico, gravação), e não pela lógica em si; essa expectativa será verificada nos testes.

Cada amostra processada gera três registros independentes na tabela `resultado_analise`, um por rotina, vinculados à amostra de origem por chave estrangeira. Isso assegura rastreabilidade: é possível reconstituir por que determinado estado foi atribuído a uma leitura.

---

## 8.4 Comunicação e sincronização

### 8.4.1 Interfaces e mecanismos de transporte

O sensor adotado é o DS18B20, ligado ao ESP32 pela interface One-Wire. O ESP32 envia cada leitura ao servidor por Wi-Fi, em requisição HTTP POST com corpo em JSON, transportada sobre TCP. No computador, um servidor HTTP em Python recebe a requisição em um endpoint de recepção, valida o pacote e o insere na fila de entrada; a aplicação grava os resultados no MySQL, e o dashboard é uma interface web em HTML, CSS e JavaScript. O pacote JSON contém o identificador sequencial, o tempo de aquisição e a temperatura. O protocolo e as interfaces estão definidos; seu comportamento sob carga e sob falhas de rede ainda será verificado nos testes.

```text
+--------------------------------------------+
|                   SENSOR                   |
|                  DS18B20                   |
+--------------------------------------------+
                      |  interface One-Wire
                      v
+--------------------------------------------+
|                   ESP32                    |
|               firmware C/C++               |
+--------------------------------------------+
                      |  Wi-Fi: HTTP POST (JSON) sobre TCP
                      v
+--------------------------------------------+
|               SERVIDOR HTTP                |
|       endpoint de recepção (Python)        |
+--------------------------------------------+
                      |
                      v
+--------------------------------------------+
|              APLICAÇÃO PYTHON              |
+--------------------------------------------+
                      |  conexão cliente-servidor, transações SQL
                      v
+--------------------------------------------+
|               MYSQL / INNODB               |
+--------------------------------------------+
                      |
                      v
+--------------------------------------------+
|                 DASHBOARD                  |
|           HTML, CSS e JavaScript           |
+--------------------------------------------+
```

*Figura 8.3 — Comunicação entre os componentes. Fonte: elaborado pelo grupo.*

O hardware do servidor é apenas a plataforma de execução e não é uma máquina MISD.

### 8.4.2 Mecanismos de comunicação internos

Três mecanismos sustentam a organização: (i) a **distribuição do dado**, em que a mesma informação é disponibilizada às análises; (ii) a **sincronização**, que garante que as três saídas correspondam à mesma amostra; e (iii) a **coleta dos resultados**, que reúne as saídas para produzir o estado final. No protótipo, esses mecanismos foram projetados com uma fila de entrada, uma referência a um registro imutável compartilhado, chamadas de função em sequência e consultas SQL.

### 8.4.3 Temporização e ordenação

O ESP32 e o computador possuem relógios independentes. O ESP32 controla o intervalo entre leituras (1 Hz) com seu clock interno; o computador processa as amostras conforme elas chegam. Como a comunicação Wi-Fi é assíncrona e sujeita a latência variável e retransmissões TCP, a ordem de chegada pode divergir da ordem de medição. Para tratar isso, o firmware foi definido para atribuir a cada amostra um **identificador sequencial de 32 bits**, gerado na leitura e incrementado a cada amostragem, e um **tempo de aquisição** (milissegundos desde a inicialização do ESP32, lido no instante da medição), ambos enviados no pacote JSON. O servidor associa esses campos ao *timestamp* de recepção, o que permite ordenar a série temporal e detectar lacunas por perda de pacotes.

O identificador de 32 bits foi adotado no lugar de um contador de 16 bits, que se repetiria a cada 65.536 amostras, isto é, em cerca de 18,2 horas a 1 Hz. Com 32 bits, a repetição levaria cerca de 136 anos na mesma taxa (cálculo aritmético, não medição). O identificador volta a zero quando o ESP32 é reiniciado; nesse caso, o servidor distingue os ciclos de funcionamento pelo *timestamp* de recepção. O tempo de aquisição resolve o segundo problema: amostras reenviadas após uma queda de conexão (seção 8.9) chegam com *timestamp* de recepção posterior à medição, mas mantêm o instante original de aquisição, que permite reconstituir os intervalos entre as leituras. Como também é zerado a cada reinício do ESP32, esse tempo só é comparável dentro de um mesmo ciclo de funcionamento. A correção da ordenação será verificada na homologação.

---

## 8.5 Armazenamento e gerenciamento de dados

A arquitetura separa dados temporários (voláteis) de dados permanentes, e separa a persistência do firmware da persistência das medições (TANENBAUM; AUSTIN, 2013; STALLINGS, 2016).

```text
ESP32
+--------------------------------------------+
|     FLASH NOR SPI - 4 MB (não volátil)     |
|       firmware e NVS (SSID e senha)        |
+--------------------------------------------+
                      |  carga do firmware e credenciais
                      v
+--------------------------------------------+
|          SRAM - 520 KiB (volátil)          |
|    buffers, pilhas, FreeRTOS, Wi-Fi/TCP    |
+--------------------------------------------+
                      |  Wi-Fi: leituras
                      v
COMPUTADOR-SERVIDOR
+--------------------------------------------+
|        RAM DDR4 - 16 GiB (volátil)         |
|      filas, estados das análises, SQL      |
+--------------------------------------------+
                      |  COMMIT em lote
                      v
+--------------------------------------------+
|      SSD NVMe - 512 GB (não volátil)       |
|          MySQL, histórico e logs           |
+--------------------------------------------+
```

*Figura 8.4 — Hierarquia de armazenamento. Fonte: elaborado pelo grupo.*

**Tabela 8.3 — Hierarquia de armazenamento (capacidades nominais ou previstas)**

| Tecnologia | Componente | Capacidade | Volatilidade | Função |
|---|---|---|---|---|
| SRAM | ESP32-D0WD-V3 | 520 KiB | Volátil | Buffers e variáveis do firmware |
| Flash NOR SPI | ESP32-WROOM-32E-N4 | 4 MB | Não volátil | Firmware e NVS |
| DRAM DDR4 | Computador-servidor | 16 GiB | Volátil | Filas e processamento |
| SSD NVMe | Computador-servidor | 512 GB | Não volátil | MySQL e logs |

### 8.5.1 Memórias voláteis

**SRAM do ESP32.** É uma memória estática (células de seis transistores), de baixa latência e sem necessidade de *refresh*. Mantém variáveis de execução, pilhas e estruturas de controle, além de ser compartilhada com o FreeRTOS e com as pilhas Wi-Fi e TCP/IP (ESPRESSIF SYSTEMS, 2026a). Assim, os 520 KiB nominais **não estão integralmente disponíveis** ao buffer de amostras. O firmware foi projetado para usar uma `struct` estática como buffer de aquisição, que guarda a leitura antes de seu encapsulamento em JSON. A alocação estática pré-definida torna previsível o uso de memória e reduz o risco de fragmentação do *heap* e de falhas de alocação dinâmica. A estabilidade dessa organização frente a travamentos e perdas de pacotes ainda deve ser verificada nos testes.

**RAM do computador.** Os 16 GiB de DRAM DDR4 mantêm os dados recebidos, as estruturas dinâmicas do Python, a fila de entrada, os estados intermediários das análises, os resultados temporários e a preparação dos comandos SQL. A fila de entrada desacopla a recepção de rede do processamento analítico: oscilações temporárias na rede ou picos de carga são absorvidos enquanto houver espaço. O registro permanece na RAM até a conclusão das análises e da persistência. O consumo real de memória e a eficiência dessa estratégia serão avaliados na homologação.

### 8.5.2 Memórias não voláteis

**Flash do ESP32.** Armazena o binário do firmware (partição *Factory App*) e parâmetros persistentes em formato chave-valor na partição NVS, no caso, o SSID e a senha da rede Wi-Fi, gravados na configuração inicial (ESPRESSIF SYSTEMS, 2026c; 2026d). Como a Flash tem ciclos limitados de apagamento e escrita, **não se gravam amostras de temperatura nela**. Ressalta-se ainda que guardar credenciais na NVS não constitui, por si só, isolamento criptográfico avançado contra extração física do hardware.

**MySQL e SSD.** O MySQL, com o motor InnoDB, foi escolhido pela arquitetura cliente-servidor, suporte a transações ACID, gerenciamento de concorrência e integridade (ORACLE CORPORATION, 2026). O modelo de dados possui três tabelas: `dispositivo` (id_dispositivo, identificador, descricao), `amostra` (id_amostra, id_dispositivo, timestamp_recepcao, temperatura) e `resultado_analise` (id_resultado, id_amostra, tipo_analise, valor_calculado, status_alerta).

```text
+--------------------------------------------+
|                DISPOSITIVO                 |
|--------------------------------------------|
|            id_dispositivo (PK)             |
|               identificador                |
|                 descricao                  |
+--------------------------------------------+
                      |  1 : N  (um dispositivo, várias amostras)
                      v
+--------------------------------------------+
|                  AMOSTRA                   |
|--------------------------------------------|
|              id_amostra (PK)               |
|            id_dispositivo (FK)             |
|             timestamp_recepcao             |
|                temperatura                 |
+--------------------------------------------+
                      |  1 : 3  (cada amostra, três resultados)
                      v
+--------------------------------------------+
|             RESULTADO_ANALISE              |
|--------------------------------------------|
|             id_resultado (PK)              |
|              id_amostra (FK)               |
|                tipo_analise                |
|              valor_calculado               |
|               status_alerta                |
+--------------------------------------------+
```

*Figura 8.5 — Modelo de dados planejado. Fonte: elaborado pelo grupo.*

O programa Python foi projetado para usar transações explícitas, e o **COMMIT** é o critério para considerar concluída a persistência de uma amostra, só então liberada da fila de entrada. O agrupamento de inserções (*Bulk Insert*) reduz a sobrecarga por operação de escrita. O SSD NVMe oferece altas taxas nominais de leitura e escrita, mas a velocidade física não garante, isoladamente, a integridade: esta depende da coordenação entre os comandos do MySQL, o InnoDB, a liberação síncrona do log de transações e as primitivas de escrita do sistema operacional (ORACLE CORPORATION, 2026).

---

## 8.6 Capacidade de processamento e desempenho

O desempenho de um sistema deve ser avaliado em relação à sua finalidade e ao seu contexto tecnológico (PATTERSON; HENNESSY, 2017). Para este protótipo, a pergunta pertinente não é "quão rápido é", mas "o sistema acompanha a taxa de chegada das amostras com latência compatível com o monitoramento de temperatura?".

**Carga de trabalho.** A frequência de amostragem definida é de 1 Hz. Cada amostra gera um registro em `amostra` e três em `resultado_analise`, isto é, quatro linhas por segundo, ou 86.400 amostras e 345.600 linhas por dia em operação contínua (valores derivados aritmeticamente da definição, não medidos). Estima-se que seja uma carga baixa para um servidor com 16 GiB de RAM e SSD NVMe, e as operações de cada análise (comparação, média, desvio das últimas leituras) têm custo computacional reduzido.

**Fatores que influenciam o desempenho.**

- *Rede:* a latência do Wi-Fi é variável e depende do ambiente de rádio e das retransmissões TCP. Espera-se que seja o componente de maior variância do tempo total entre medição e visualização.
- *Persistência:* a latência de gravação com COMMIT, que envolve a liberação síncrona do log de transações, deve ser a operação mais custosa do lado do servidor. O *Bulk Insert* troca latência individual por vazão.
- *Execução das análises:* as três análises são executadas em sequência, em um único fluxo de processamento. A decisão decorre do GIL do CPython, que impede que *threads* executem bytecode Python simultaneamente em múltiplos núcleos (PYTHON SOFTWARE FOUNDATION, 2026): como as análises são cálculos curtos limitados por CPU, usar *threads* não reduziria o tempo de ciclo e apenas acrescentaria sincronização. A concorrência fica restrita à recepção dos pacotes, que o servidor HTTP enfileira enquanto o fluxo de processamento consome a fila. Distinguem-se, portanto: **independência lógica** (as análises não dependem umas das outras), **execução sequencial** (adotada para as três análises), **concorrência** (tarefas intercaladas, presente apenas entre recepção e processamento) e **paralelismo em hardware** (execução simultânea real, não utilizado). O modelo lógico MISD não exige paralelismo físico.
- *Janela do histórico:* o tamanho da janela e os critérios estatísticos afetam tanto o custo das análises quanto a sensibilidade dos resultados.
- *Recursos do ESP32:* o microcontrolador, com memória e processamento limitados, restringe-se a aquisição e transmissão, o que preserva sua operação estável.

**Latência de ponta a ponta.** O intervalo entre a medição física e a atualização do dashboard é a soma de aquisição, transmissão, recepção, enfileiramento, análises, consolidação, persistência e atualização da interface. Não há valor medido para nenhuma dessas parcelas; a latência total será caracterizada na homologação.

**Indicadores a avaliar.** Latência média e máxima de ponta a ponta; taxa de processamento de amostras; uso de memória RAM; tempo de gravação no MySQL; e quantidade de amostras perdidas.

---

## 8.7 Vantagens arquiteturais

**Decisão mais confiável.** Verificar a mesma temperatura por três critérios distintos amplia a capacidade de interpretação do dado. O ganho é de interpretação, e não de medição: não elimina erros da leitura original.

**Independência das análises.** Cada rotina tem finalidade própria e pode ser modificada, testada e corrigida sem necessariamente alterar as demais, o que facilita manutenção e validação incremental.

**Expansão lógica.** Uma nova análise pode ser acrescentada sobre o mesmo dado sem alterar a aquisição. O custo é o aumento da carga de distribuição e consolidação, e da latência do ciclo (que cresce com a soma dos tempos das rotinas).

**Menor carga no microcontrolador.** Ao deslocar as análises para o computador, o ESP32 mantém apenas tarefas leves e previsíveis.

**Consistência da entrada.** O uso de um registro imutável compartilhado evita divergências entre as análises dentro do mesmo ciclo.

**Hierarquia de armazenamento.** A divisão entre SRAM, RAM, Flash e SSD separa dados temporários, processamento imediato, código e configurações permanentes e histórico duradouro, atribuindo responsabilidades claras a cada componente.

**Adequação didática.** A arquitetura demonstra com clareza o princípio de múltiplas instruções sobre um único fluxo de dados, além de exercitar conceitos de comunicação, memória, concorrência e persistência.

---

## 8.8 Limitações e gargalos

### 8.8.1 Restrições de hardware

**Ponto único de falha na aquisição.** Há um sensor e um ESP32. Se o sensor falhar ou fornecer leitura incorreta, as três análises operam sobre o mesmo dado incorreto: a redundância está nas análises, não na medição física. Essa é a limitação mais característica de uma organização de fluxo de dados único (TANENBAUM; AUSTIN, 2013).

**Recursos do ESP32.** A memória e o processamento limitados impedem análises elaboradas no próprio dispositivo; além disso, a SRAM é compartilhada com o FreeRTOS e com as pilhas Wi-Fi e TCP/IP, o que reduz o espaço disponível aos buffers.

**Ruído do sensor.** Variações espúrias podem produzir falsos alertas ou falsas anomalias, especialmente porque a detecção de anomalias é sensível a desvios.

### 8.8.2 Restrições de comunicação

**Dependência da rede.** Sem conectividade, o servidor deixa de receber leituras em tempo real, o processamento é interrompido temporariamente e o dashboard se atrasa.

**Latência e ordem.** A latência variável do Wi-Fi e as retransmissões TCP podem atrasar ou inverter a ordem de chegada das amostras. O identificador sequencial e o tempo de aquisição enviados pelo ESP32 mitigam a inversão (seção 8.4.3); ambos, porém, voltam a zero quando o ESP32 é reiniciado, o que exige o *timestamp* de recepção para distinguir os ciclos de funcionamento.

### 8.8.3 Restrições de armazenamento

**Volatilidade da SRAM.** Uma queda de energia do ESP32 faz perder as amostras ainda não enviadas, inclusive as da fila de contingência.

**Gargalo de escrita SQL.** Se a taxa de chegada superar continuamente a capacidade de processamento e persistência, as mensagens se acumulam na fila de entrada em RAM e, atingido o limite, pode ocorrer estouro de buffer. A condição é estável apenas se a taxa média de saída superar a de entrada.

**Desgaste da Flash.** Os ciclos limitados de apagamento e escrita tornariam inadequada a gravação contínua de amostras.

**Capacidades nominais.** Os 520 KiB, 4 MB, 16 GiB e 512 GB são valores nominais ou previstos; o espaço efetivamente utilizável é menor.

### 8.8.4 Restrições de processamento

**GIL.** O CPython restringe a execução simultânea de bytecode a um único núcleo, limitando o paralelismo por *threads* (PYTHON SOFTWARE FOUNDATION, 2026). Por isso as análises são executadas em sequência.

**Natureza do MISD.** O modelo depende de um único fluxo de entrada e de um ponto de coleta. A distribuição e a consolidação podem se tornar gargalo conforme novas análises são incorporadas, e a aplicabilidade restringe-se a problemas que se beneficiam de diferentes operações sobre o mesmo dado (FLYNN, 1972; DUNCAN, 1990).

**Qualidade do histórico.** A análise histórica e a detecção de anomalias dependem da quantidade e da qualidade das medições anteriores e da escolha da janela e dos critérios estatísticos.

---

## 8.9 Estratégias de mitigação

**Tabela 8.4 — Matriz de limitações e mitigações**

| Restrição | Consequência | Mitigação |
|---|---|---|
| Volatilidade da SRAM | Perda de medições sem energia | Envio imediato ao servidor |
| Espaço limitado na SRAM | Falhas de alocação e travamentos | Buffers estáticos pré-alocados |
| Gargalo de escrita SQL | Acúmulo de mensagens e estouro da RAM | Transações em lote (Bulk Insert) |
| Desgaste da Flash | Possível falha permanente | Não gravar amostras na Flash |
| Latência Wi-Fi | Atrasos e inversão de ordem | Identificador e tempo de aquisição na origem |
| Queda de conectividade | Interrupção do envio | Contingência temporária: fila circular na SRAM com Drop-Tail |
| Restrição do GIL | Pouco paralelismo por threads | Análises em sequência sobre dados imutáveis |

**Fluxo de persistência.** O ciclo planejado para conter o acúmulo na RAM é: (1) o pacote chega ao endpoint HTTP; (2) o registro entra na fila de entrada; (3) as três análises consultam o registro; (4) os resultados são agrupados em uma fila de preparação SQL; (5) o lote é enviado ao MySQL; (6) aguarda-se o COMMIT; (7) o registro é liberado da fila de entrada. A liberação só após o COMMIT evita perda de dados por liberação prematura, ao custo de manter mais registros em RAM durante a espera.

**Contingência para falha de Wi-Fi.** A fila circular é um mecanismo de contingência temporária, e não uma garantia de armazenamento ou de recuperação dos dados. O comportamento esperado é descrito na Figura 8.6.

```text
+--------------------------------------+
|          WI-FI INDISPONÍVEL          |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|          SUSPENSÃO DO ENVIO          |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|        FILA CIRCULAR NA SRAM         |
|                32 KiB                |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|          Há espaço na fila?          |
+--------------------------------------+
                   |
         +---------+----------+
        SIM                   NÃO
         |                    |
         v                    v
+-----------------+  +-----------------+
|    Armazenar    |  |    Drop-Tail:   |
|     amostra     |  | descarta a nova |
+-----------------+  +-----------------+
         |                    |
         +---------+----------+
                   |
                   v
+--------------------------------------+
|           WI-FI RESTAURADO           |
+--------------------------------------+
                   |
                   v
+--------------------------------------+
|          REENVIO SEQUENCIAL          |
|       dos registros acumulados       |
+--------------------------------------+
```

*Figura 8.6 — Contingência durante falha de conectividade. Fonte: elaborado pelo grupo.*

Com a política *Drop-Tail*, quando a fila está cheia as amostras novas são descartadas e as antigas preservadas, mantendo a ordem cronológica a partir do início da interrupção. O tempo de autonomia depende do tamanho do registro armazenado na fila e da taxa de amostragem, segundo a relação: *autonomia ≈ 32 KiB ÷ (tamanho do registro × 1 amostra/s)*. Como o tamanho do registro depende da implementação do firmware, não se estabelece aqui um valor. **A política não garante o armazenamento integral em qualquer período de desconexão**: quando a fila enche, as amostras novas são perdidas; as lacunas resultantes podem ser detectadas pelo identificador sequencial, e a fila, por estar na SRAM, é perdida em caso de reinício ou falta de energia. A estratégia reduz a perda em quedas curtas, mas não assegura a continuidade do histórico.

**Execução sequencial e GIL.** As três análises são executadas em sequência sobre o mesmo registro imutável. Não foram adotadas *threads* para elas porque, no CPython, o GIL impediria ganho de desempenho em cálculos limitados por CPU; o registro imutável dispensa mecanismos de exclusão mútua.

---

## 8.10 Análise da adequação do sistema

A finalidade do protótipo é acadêmica e o problema, monitorar temperatura de equipamentos com atualização contínua e histórico consultável. Em relação a esse propósito, a arquitetura é considerada adequada, com base em análise qualitativa das características dos componentes e sem medições de desempenho, pelos seguintes motivos:

1. **Compatibilidade entre carga e recursos.** Uma taxa de 1 Hz e operações analíticas simples são compatíveis, em princípio, com a capacidade nominal do servidor; o ESP32 executa somente funções leves.
2. **Separação de responsabilidades.** O microcontrolador adquire e transmite, o servidor analisa e persiste, o banco preserva e o dashboard apresenta.
3. **Proteção contra falhas previsíveis.** Há resposta definida para os riscos principais: queda de conectividade, acúmulo na fila, desgaste da Flash, fragmentação do *heap* e inversão de ordem.
4. **Demonstração do conceito MISD.** O sistema ilustra, em software, múltiplas instruções sobre um único dado.

Em contrapartida, a adequação é limitada a um contexto de baixa criticidade. O sistema não é tolerante a falhas de aquisição, não possui redundância de sensor, não oferece garantia de entrega integral durante desconexões longas e não foi validado experimentalmente. Esses fatores impedem sua utilização em ambientes que exijam certificação ou disponibilidade garantida. A conclusão, portanto, é de adequação ao escopo didático, com a ressalva de que a confirmação quantitativa depende dos testes.

---

## 8.11 Resposta à questão central

**Qual característica do objeto estudado mais influencia sua capacidade de realizar o trabalho para o qual foi projetado?**

A característica que mais influencia o desempenho do sistema é a **organização em torno de um fluxo único de temperatura, distribuído a três análises independentes e sustentado por uma cadeia de aquisição e comunicação entre o ESP32 e o computador**. O argumento se desenvolve em quatro passos.

*Primeiro, ela define o ganho do sistema.* O trabalho para o qual o protótipo foi projetado é classificar o estado térmico de forma mais confiável que uma verificação isolada. Esse ganho decorre diretamente da aplicação de três critérios complementares (valor absoluto, evolução temporal e desvio estatístico) à mesma leitura. Os exemplos da Tabela 8.2 mostram que o estado ANOMALIA surge justamente do cruzamento de análises: uma leitura dentro do limite pode ser anômala em relação ao histórico.

*Segundo, ela define o teto desse ganho.* Como o dado é único, a confiabilidade da classificação é limitada pela confiabilidade da leitura. A diversidade de instruções mitiga erros de **interpretação**, mas não de **aquisição**: um valor errado produz três análises coerentes entre si e igualmente erradas. O mesmo raciocínio vale para o ruído do sensor, que alimenta as três rotinas, e para a qualidade do histórico, que depende de leituras recebidas e ordenadas corretamente.

*Terceiro, ela determina onde o desempenho é decidido.* Como as análises são leves e operam sobre um registro imutável em RAM, o custo computacional das três análises em sequência é baixo, e a limitação do GIL não afeta a arquitetura. O desempenho efetivo é decidido pelas etapas de E/S que cercam o processamento: a latência e a continuidade do enlace Wi-Fi (que alimentam o fluxo único) e a persistência com COMMIT (que o escoa). Essas são, nesta ordem de raciocínio, as fontes prováveis de atraso, de acúmulo na fila e de perda de amostras, e é contra elas que se dirigem as mitigações da Tabela 8.4.

*Quarto, ela explica o equilíbrio das decisões arquiteturais.* A transferência das análises para o computador, a fila de desacoplamento, a identificação sequencial, a fila circular com *Drop-Tail*, o *Bulk Insert* e a não gravação na Flash são respostas ao fato de que todo o sistema depende da chegada ordenada e íntegra de um único fluxo ao servidor.

Em síntese, a distribuição de uma mesma leitura a três análises independentes é o que confere ao sistema seu valor, e a dependência da qualidade da aquisição e da comunicação é o que o limita. Essas duas faces da mesma característica são as que mais condicionam sua capacidade de realizar o monitoramento de temperatura. A confirmação quantitativa dessa conclusão, em particular a identificação do componente que domina a latência total, será feita na homologação.

---

## 8.12 Considerações finais

A análise integrada mostrou que o protótipo articula aquisição, transmissão, processamento, consolidação, persistência e visualização em uma arquitetura coerente com a finalidade acadêmica. A organização inspirada no MISD estrutura o processamento em três instruções sobre um mesmo dado e favorece independência, manutenção e consistência de entrada, ao custo de depender de um único ponto de aquisição e de um canal de comunicação sem redundância. A hierarquia SRAM, Flash, RAM e SSD distribui adequadamente as responsabilidades, e as estratégias de mitigação (buffers estáticos, identificação sequencial, fila de contingência, *Bulk Insert*, transações com COMMIT e preservação da Flash) respondem às limitações identificadas, sem eliminá-las.

---

## 8.13 Referências

DUNCAN, R. A survey of parallel computer architectures. **Computer**, IEEE, fev. 1990.

ESPRESSIF SYSTEMS. **ESP32 Series Datasheet**. v. 4.4. Shanghai: Espressif Systems, 2026a. Disponível em: espressif.com. Acesso em: 18 out. 2026.

ESPRESSIF SYSTEMS. **ESP32-WROOM-32E & ESP32-WROOM-32UE Datasheet**. v. 1.5. Shanghai: Espressif Systems, 2026b. Disponível em: espressif.com. Acesso em: 18 out. 2026.

ESPRESSIF SYSTEMS. **ESP-IDF Programming Guide: Non-Volatile Storage Library**. Shanghai: Espressif Systems, 2026c. Disponível em: espressif.com. Acesso em: 18 out. 2026.

ESPRESSIF SYSTEMS. **ESP-IDF Programming Guide: SPI Flash and Partition Tables**. Shanghai: Espressif Systems, 2026d. Disponível em: espressif.com. Acesso em: 18 out. 2026.

FLYNN, M. J. Very high-speed computing systems. **Proceedings of the IEEE**, v. 54, n. 12, 1966.

FLYNN, M. J. Some computer organizations and their effectiveness. **IEEE Transactions on Computers**, New York, v. C-21, n. 9, p. 948–960, set. 1972. DOI: 10.1109/TC.1972.5009071.

ORACLE CORPORATION. **MySQL 8.4 Reference Manual: InnoDB Storage Engine**. Redwood Shores: Oracle Corporation, 2026. Disponível em: mysql.com. Acesso em: 18 out. 2026.

PATTERSON, D. A.; HENNESSY, J. L. **Computer organization and design**: the hardware/software interface. 5. ed. Elsevier, 2017.

PYTHON SOFTWARE FOUNDATION. **Python 3.15.0 Documentation**: Threading in Python. Wilmington: Python Software Foundation, 2026. Disponível em: python.org. Acesso em: 18 out. 2026.

STALLINGS, W. **Computer organization and architecture**: designing for performance. 10. ed. Boston: Pearson, 2016.

TANENBAUM, A. S.; AUSTIN, T. **Structured computer organization**. 6. ed. Boston: Pearson, 2013.
