# Arquitetura MISD: organização e comunicação entre componentes

## Introdução

Este documento explica como os elementos de uma arquitetura MISD (Multiple Instruction, Single Data) se organizam e se comunicam, primeiro na teoria e depois no protótipo de monitoramento de temperatura com ESP32 e Python.

Na Taxonomia de Flynn, a MISD é a categoria em que várias unidades executam instruções diferentes ao mesmo tempo sobre um único fluxo de dados. Cada seção a seguir responde a um requisito da pesquisa e separa o que vale para a arquitetura em geral do que vale para o protótipo.

## 1. Principais componentes ou unidades

Uma MISD reúne várias unidades de controle e de processamento em torno de uma única fonte de dados.

- **Unidade de controle (UC):** na teoria, há várias UCs, cada uma emitindo um fluxo de instruções diferente.
- **Unidade de processamento (UP):** cada UP executa o fluxo de instruções da sua UC.
- **Memória ou fonte de dados:** fornece o fluxo único de dados que todas as UPs recebem.
- **Unidade de comparação ou decisão:** em sistemas tolerantes a falhas, reúne os resultados e faz votação ou verificação.

**No protótipo:** o sensor de temperatura e o ESP32 formam a fonte do dado único. As unidades de processamento são as três análises (verificação de limite, comparação com histórico e detecção de anomalias). O banco SQL e o dashboard cuidam do armazenamento e da apresentação.

## 2. Como os elementos estão organizados

Na MISD, o dado único passa por várias unidades, e cada uma aplica uma instrução diferente a ele.

A organização pode ser **em paralelo**, com todas as unidades recebendo o mesmo dado ao mesmo tempo, ou **em pipeline (macropipeline)**, com o dado atravessando as unidades em sequência. A organização em paralelo é a mais usada nas explicações do conceito.

**No protótipo:** a organização é em paralelo. Cada leitura de temperatura é entregue às três análises, que rodam de forma independente.

## 3. Como eles se conectam

Em hardware, a conexão costuma ser um barramento ou canal de dados compartilhado que alimenta todas as UPs, mais as ligações que levam as saídas até a unidade de comparação. Em arranjos sistólicos, as conexões são diretas entre unidades vizinhas.

**No protótipo:** a conexão é lógica, não física. O sensor se liga ao ESP32 por um pino, o ESP32 se liga ao servidor por Wi-Fi e, dentro do Python, os processos recebem o mesmo dado por memória compartilhada, filas ou chamadas de função.

## 4. Como dados, sinais, comandos e resultados circulam

O dado vai de um para muitos, as instruções ficam separadas por fluxo e os resultados voltam de muitos para um.

- **Dados:** vão da fonte para todas as unidades.
- **Instruções:** cada UC envia as suas para a UP correspondente, sem comunicação entre os fluxos de instrução.
- **Resultados:** voltam das UPs para a unidade de comparação ou de saída.

**No protótipo:** o caminho é Sensor → ESP32 → Wi-Fi → Python → Limite / Histórico / Anomalia → Resultado → SQL → Dashboard.
```
 SENSOR ──► ESP32 ──Wi-Fi──► PYTHON (servidor)
                                     │  
                                     |  mesmo dado (1 para muitos) 
              ┌──────────────────────┼──────────────────────┐
              ▼                      ▼                      ▼
           Limite               Histórico               Anomalia 
              │                      │                      │
              └──────────────────────┼──────────────────────┘
                                     |  resultados (muitos para 1)
                                     ▼                                 
                               [ Resultado ]
                                     │
                                     ▼
                                 Banco SQL  ──►   Dashboard web 
```
A leitura sai do Python em um único fluxo e chega às três análises ao mesmo tempo; só depois os resultados se reúnem.

## 5. Mecanismos de comunicação existentes

Três mecanismos sustentam a comunicação em uma MISD: distribuição do dado, sincronização e coleta dos resultados.

- **Broadcast do dado:** a mesma informação é distribuída a todas as unidades.
- **Sincronização:** garante que todas processem o mesmo dado no mesmo ciclo ou leitura.
- **Coleta e comparação de resultados:** votação, redundância ou agregação das saídas.

**No protótipo:** a comunicação usa o protocolo de rede escolhido (por exemplo HTTP ou MQTT sobre Wi-Fi), filas ou threads no Python e consultas SQL para gravar e buscar o histórico.

## 6. Barramentos, interconexões, canais e interfaces

Em hardware, o elemento central é o barramento de dados compartilhado entre a fonte e as unidades de processamento; no protótipo, cada ligação equivalente é uma interface específica.

**Na arquitetura:** barramento de dados compartilhado, interconexão ponto a ponto entre unidades e interface com a unidade de comparação.

**No protótipo:**

- interface do sensor com o ESP32 (analógica ou digital, ou protocolos como I2C e one-wire, conforme o sensor);
- canal Wi-Fi entre o ESP32 e o servidor;
- interface de rede ou API no servidor;
- conexão com o banco SQL;
- interface web (HTML/JavaScript) para o dashboard.

O Intel Xeon é apenas a plataforma de execução do servidor, não uma máquina MISD.

## 7. Possíveis limitações da organização

A principal limitação da MISD é a dependência de um único fluxo de dados que alimenta todas as unidades.

- **Gargalo na distribuição do dado:** todas as unidades dependem do mesmo fluxo de entrada.
- **Aplicabilidade restrita:** poucos problemas se beneficiam de várias operações distintas sobre o mesmo dado, e por isso a MISD quase não existe comercialmente.
- **Escalabilidade limitada:** adicionar unidades aumenta a carga de distribuição e de comparação.
- **Dependência do ponto de coleta:** se o sensor ou o ESP32 falha, todas as análises ficam sem dado.

## 8. Fontes

- DUNCAN, R. A survey of parallel computer architectures. Computer, IEEE, fev. 1990.

- FLYNN, M. J. Very high-speed computing systems. Proceedings of the IEEE, v. 54, p. 1901–1909, dez. 1966.

- FLYNN, M. J. Some computer organizations and their effectiveness. IEEE Transactions on Computers, v. C-21, n. 9, p. 948–960, set. 1972. DOI: 10.1109/TC.1972.5009071.

- HENNESSY, J. L.; PATTERSON, D. A. Computer architecture: a quantitative approach. [S. l.]: Elsevier.

- KUNG, H. T. Why systolic architectures? Computer, IEEE, v. 15, n. 1, p. 37–46, jan. 1982.

- KUNG, H. T.; LEISERSON, C. E. Systolic arrays (for VLSI). In: SPARSE MATRIX PROCEEDINGS 1978. [S. l.]: SIAM, 1979. p. 256–282.

- NULL, L.; LOBUR, J. Essentials of computer organization and architecture. 5. ed. Burlington: Jones & Bartlett Learning, 2018.

- SPECTOR, A.; GIFFORD, D. The space shuttle primary computer system. Communications of the ACM, v. 27, n. 9, p. 874–900, set. 1984. DOI: 10.1145/358234.358246.

- TANENBAUM, A. S. Organização estruturada de computadores. 5. ed. [S. l.]: Pearson Prentice Hall, 2009.
