## Resultados da Investigação

1. **De onde vêm os dados:**
Os dados originam-se no ambiente físico por meio de variações térmicas. Um sensor digital (DS18B20/DHT22) faz a conversão Analógico/Digital interna e transmite o binário para o ESP32 via protocolo One-Wire/I2C.
2. **Como chegam às unidades de processamento:** 
O ESP32 aloca o valor na sua SRAM interna, converte o número para formato texto (string) e despacha um datagrama UDP via rádio Wi-Fi (2.4 GHz). A placa de rede do computador receptor captura os pacotes por interrupção de hardware (IRQ), repassa para o buffer de rede do Sistema Operacional e este entrega ao socket Python (porta `5005`).
3. **Onde resultados intermediários são mantidos:**
A leitura mais recente fica alocada na RAM principal (Heap do Python) na variável `temperatura`. O histórico é mantido em uma fila de tamanho fixo na RAM, e o resultado de cada análise fica isolado no escopo local de memória da sua respectiva função/processo.
4. **Movimentação da informação:**
No PC, assim que o dado único chega, o script principal realiza chamadas de sistema para clonar o contexto e disparar processos paralelos. O dado é copiado para a pilha de execução (Stack) de cada subprocesso por meio do barramento de sistema da CPU.
5. **Ordem ou prioridade dos acessos:**
O dado é inserido no histórico antes do disparo dos processos analíticos. A ordem real de execução das instruções analíticas na CPU não é sequencial, sendo decidida dinamicamente pelo escalonador do Sistema Operacional.
6. **Níveis de armazenamento e aceleração:**
No hardware do PC, a CPU puxa o dado da RAM para o nível mais rápido de armazenamento voluntário: a **Cache L1**. Como o modelo é MISD (Múltiplas Instruções, Dado Único), os diferentes núcleos da CPU acessam concorrentemente o dado direto da Cache L1 compartilhada, minimizando o tempo de busca na RAM física. No software, o acesso ao histórico por índice `[-1]` opera em tempo constante O(1).
7. **Limitações ou gargalos:** 
   * *Rede:* Flutuações e perdas no protocolo UDP sobre Wi-Fi.
   * *GIL (Global Interpreter Lock):* Trava do Python que impede paralelismo real em threads de CPU.
   * *Memória:* Risco de estouro de memória (Memory Leak) com histórico infinito.

---

###  Representação do Caminho Percorrido pelos Dados

```text
==========================================================================================
                     CAMINHO DO DADO NO PROTÓTIPO MISD (TEMPERATURA)
==========================================================================================

 [ AMBIENTE FÍSICO ]  ➔ Grandeza Térmica (Calor)
         │
         ▼
 [ SENSOR DIGITAL ]   ➔ Conversão A/D interna ➔ Transmissão via barramento One-Wire/I2C
         │
         ▼
 [ CPU DO ESP32 ]     ➔ Processador extrai o dado e aloca na SRAM do ESP32
         │
         ▼
 [ RÁDIO WI-FI ESP ]  ➔ Encapsula em pacote UDP ➔ Modula em Radiofrequência (2.4 GHz)
         │
   ((( ONDAS NO AR ))) 🔀 [GARGALO 1: Latência, interferência e perda de pacotes]
         │
         ▼
 [ PLACA REDE PC ]    ➔ Recebe frames ➔ Dispara Interrupção de Hardware (IRQ) ao S.O.
         │
         ▼
 [ BUFFER DO S.O. ]   ➔ Kernel aloca pacote na pilha de rede e entrega ao Socket do Python
         │
         ▼
 [ MEMÓRIA RAM PC ]   ➔ Instanciação na Heap do Python (Objeto Float: `temperatura`)
         │
         ▼ 
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ CPU DO COMPUTADOR (Mecanismo de Execução MISD)                                         │
│                                                                                        │
│                   ┌── Puxa dado da RAM ──> [ CACHE L1 ] <── Compartilhada (Leitura) ──┐ │
│                   │                           │                               │       │
│                   ▼                           ▼                               ▼       │
│           [ PROCESSO 1 ]              [ PROCESSO 2 ]                  [ PROCESSO 3 ]  │
│                 │                           │                               │         │
│         Instrução Tipo A:            Instrução Tipo B:               Instrução Tipo C: │
│         Verificar Limite            Comparar Histórico              Detectar Anomalias │
│                 │                           │                               │         │
│         [NÚCLEO 1 DA CPU]           [NÚCLEO 2 DA CPU]               [NÚCLEO 3 DA CPU]  │
│                 │                           │                               │         │
│                 └───────────────────────────┴───────┬───────────────────────┘         │
│                                                     │ (Paralelismo real via hardware) │
└─────────────────────────────────────────────────────┼─────────────────────────────────┘
                                                      ▼
                                             [ TERMINAL / SAÍDA ]
```

---

###  Atualização de Arquivos e Código do Protótipo

Para mitigar os gargalos descritos, atualizamos os códigos do projeto utilizando **Multiprocessamento Real** (contornando o gargalo do GIL do Python para obter execução paralela em múltiplos núcleos de hardware) e **Buffers Circulares** (limitando o consumo de RAM).

*   `server_misd_final.py`: Implementação do servidor receptor em Python com `multiprocessing` e `collections.deque(maxlen=100)`.
*   `boot_esp32.py`: Script cliente para leitura de sensores e despacho UDP no microcontrolador.
*   `relatorio_misd_final.md`: Documentação técnica detalhada adicionada à Wiki/Docs do repositório.





###  Referências

 **Taxonomia de Flynn & Arquiteturas Paralelas:**
   * CORNELL Virtual Workshop. *Introduction to Parallel Computing: Flynn's Taxonomy*. Cornell University Center for Advanced Computing. Disponível em: <https://cornell.edu>.
   * SILVA, S. L. *Arquitetura de Computadores e Sistemas Paralelos*. 3. ed. Rio de Janeiro: LTC, 2021. *(Discussão sobre a raridade e aplicações de tolerância a falhas do modelo MISD)*.

 **Comunicação de Rede e Protocolos no ESP32:**
   * ESPRESSIF SYSTEMS. *lwIP (Lightweight IP) Stack*. In: *ESP-IDF Programming Guide*. Disponível em: <https://espressif.com>. *(Documentação oficial sobre o comportamento do buffer UDP e pilha de rede no microcontrolador)*.
   * POSTEL, J. *User Datagram Protocol (UDP)*. RFC 768, Internet Engineering Task Force (IETF), 1980. Disponível em: <https://ietf.org>.

 **Arquitetura Interna do Sensor de Temperatura:**
   * MAXIM INTEGRATED. *DS18B20 Programmable Resolution 1-Wire Digital Thermometer*. Datasheet rev. 6. Disponível em: <https://maximintegrated.com>. *(Detalhamento do conversor A/D interno e registradores de memória do sensor)*.

 **Gerenciamento de Memória e Concorrência no Python:**
   * PYTHON SOFTWARE FOUNDATION. *multiprocessing — Process-based parallelism*. Python 3.12 Documentation. Disponível em: <https://python.org>. *(Referência sobre a mitigação do GIL através da clonagem de processos)*.
   * REAL PYTHON. *What Is the Python Global Interpreter Lock (GIL)?*. Disponível em: <https://realpython.com>. *(Análise técnica sobre o impacto do GIL na limitação de threads de CPU)*.
