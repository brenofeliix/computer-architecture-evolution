# 9. Legacy

## 9.1 Historical Importance
O desenvolvimento de máquinas computacionais programáveis foi motivado pela necessidade de realizar cálculos matemáticos complexos, repetitivos e demorados com maior rapidez e precisão. Antes dessas máquinas, muitos cálculos científicos, militares e de engenharia eram realizados manualmente, exigindo muito tempo e aumentando a possibilidade de erros. Nesse contexto, Howard H. Aiken iniciou, em 1937, o projeto de uma máquina capaz de executar automaticamente sequências de operações matemáticas, contando com o apoio da IBM para sua construção.
O Harvard Mark I foi desenvolvido entre 1939 e 1944 e entrou em operação na Universidade Harvard em 1944, durante a Segunda Guerra Mundial. Também conhecido como Automatic Sequence Controlled Calculator (ASCC), era uma máquina eletromecânica de grandes dimensões, composta por relés, contadores, eixos e outros mecanismos físicos utilizados para realizar cálculos automaticamente.
As instruções eram fornecidas por meio de uma fita de papel perfurada, que determinava a sequência de operações a serem executadas. Os dados, por sua vez, podiam ser introduzidos por cartões perfurados e outros mecanismos de entrada, sendo mantidos em contadores e dispositivos de armazenamento durante os cálculos. Dessa forma, a máquina utilizava meios diferentes para controlar as operações e fornecer os valores necessários ao processamento.
É importante destacar que o funcionamento eletromecânico e o uso de fita perfurada eram características específicas do Harvard Mark I, relacionadas à tecnologia disponível na época. Essas características não devem ser transportadas diretamente para diagramas modernos da arquitetura de Harvard, pois representam escolhas de implementação daquela máquina histórica. O conceito principal está na separação funcional entre instruções e dados, que pode ser aplicada utilizando tecnologias e componentes diferentes, sem depender dos mecanismos físicos utilizados pelo Mark I.

## 9.2 Scientific Contributions
A evolução das abordagens inspiradas na organização Harvard demonstra mudanças significativas na forma de implementar a separação de instruções e dados ao longo do tempo:

* **Diferença entre modelos históricos e eletrônicos:** Enquanto máquinas pioneiras utilizavam suportes físicos distintos e desconectados (como fitas de papel perfurado para instruções e cartões ou relés para dados), as implementações eletrônicas posteriores consolidaram circuitos integrados e memórias semicondutoras de alta velocidade.
* **Aplicação em sistemas modernos:** A separação funcional evoluiu para o conceito de "Harvard Modificada" em microcontroladores modernos (como a arquitetura AVR), onde as memórias físicas de programa e dados continuam separadas, mas com permissões controladas de leitura cruzada.
* **Flexibilização da separação estrita:** Sistemas computacionais contemporâneos de alto desempenho adotam caches L1 separadas (instruções e dados) para ganho de velocidade imediata, unificando os níveis superiores de memória para otimizar o uso do espaço.
* **Características preservadas:** O princípio fundamental de garantir canais de acesso simultâneo e isolados para o fluxo de instruções e de operandos permaneceu como base estrutural em processadores embarcados e de arquitetura RISC.
* **Sistemas relacionados e distinção histórica:** Arquiteturas modernas com caches divididas são funcionalmente tributárias do princípio de acesso independente, mas não devem ser tratadas como sucessoras diretas e lineares da máquina eletromecânica original sem a devida evidência documental da evolução tecnológica.

## 9.3 Contribuições

A arquitetura de Harvard clássica é uma organização de computador em que **instruções e dados ficam em memórias distintas, com caminhos de acesso (barramentos) próprios**. O nome é retrospectivo: refere-se ao Harvard Mark I (projeto iniciado por Howard H. Aiken em 1937, em operação em 1944), que já separava a fita de instruções dos contadores de dados.

| Contribuição | Descrição | Período / contexto |
|---|---|---|
| **Separação física entre instruções e dados** | Duas memórias independentes, uma para o programa e outra para os dados, em vez de um único espaço de armazenamento. | Origem histórica no Mark I (1939–1944) |
| **Acesso simultâneo** | Com barramentos separados, é possível buscar a próxima instrução enquanto se lê ou grava um operando, sem disputa pelo mesmo caminho. | Formulada como vantagem em estudos posteriores de arquitetura de computadores |
| **Liberdade de dimensionamento** | Como as memórias são independentes, podem ter larguras de palavra e tamanhos diferentes (por exemplo, instruções de 14 bits e dados de 8 bits em alguns microcontroladores). | Presente em implementações eletrônicas posteriores |
| **Contraponto à memória única** | Serve como referência de comparação com a organização de programa armazenado em memória única descrita no *First Draft of a Report on the EDVAC* (1945). | Segunda metade dos anos 1940 em diante |

> **Observação:** fita perfurada, relés e contadores eletromecânicos são escolhas de implementação do Mark I. A contribuição conceitual atribuída à arquitetura é a **separação funcional** entre instruções e dados.

---

## 9.4 Características preservadas ou abandonadas

### Comparativo entre o modelo clássico e implementações posteriores

| Característica | Harvard clássica (modelo histórico/conceitual) | Implementações posteriores | Situação |
|---|---|---|---|
| **Memórias separadas para instruções e dados** | Dois espaços de armazenamento distintos | Microcontroladores com memória de programa e de dados separadas (ex.: AVR, PIC) | **Preservada** |
| **Barramentos independentes** | Caminhos próprios para cada fluxo | Mantidos em microcontroladores e em DSPs | **Preservada** |
| **Busca simultânea de instrução e dado** | Vantagem central do modelo | Aproveitada em pipelines, microcontroladores e caches L1 divididas | **Preservada** |
| **Programa não acessível como dado** | O programa não era lido nem alterado como dado | Em Harvard modificada, há instruções ou caminhos que permitem ler a memória de programa como dados (ex.: leitura de constantes) | **Modificada** |
| **Separação em todos os níveis** | Separação total | Em CPUs de alto desempenho, a separação costuma ficar no nível L1; níveis superiores de memória são unificados | **Flexibilizada** |
| **Meio físico (fita, relés, contadores)** | Eletromecânico | Memórias semicondutoras (flash, SRAM, cache) | **Abandonada** |
| **Leitura estritamente sequencial do programa** | Instruções lidas em ordem | Acesso por endereço, com desvios, saltos e sub-rotinas | **Abandonada** |

### Síntese

- **Permaneceu:** o princípio de caminhos independentes para instruções e dados e o ganho de desempenho decorrente.
- **Mudou:** a separação deixou de ser sempre total; surgiram variantes ("Harvard modificada") com acesso cruzado controlado.
- **Deixou de ser usado como antes:** a tecnologia física original e a execução puramente sequencial.

> **Nota de cautela:** "Harvard modificada" não tem definição única; o termo é usado de formas diferentes na literatura, e a classificação de cada processador depende do critério adotado.

---

## 9.5 Influência

Este tópico separa o que tem **evidência documental** do que é apenas **semelhança** entre sistemas.

### Com evidência documental

- **Uso do nome em textos de arquitetura:** a literatura de arquitetura de computadores adotou o termo "arquitetura Harvard" em referência ao Mark I, geralmente em contraste com a organização de memória única. Essa relação de **nomenclatura** é documentada.
- **Descrição de implementações como Harvard:** manuais e livros didáticos classificam microcontroladores com memórias de programa e de dados separadas, e caches L1 divididas, como Harvard ou Harvard modificada. Isso documenta a **classificação**, não que esses projetos tenham sido derivados do Mark I.

### Semelhança sem evidência de causalidade

- **Microcontroladores, DSPs e processadores embarcados** com memórias separadas reproduzem o princípio, mas não há, neste material, fonte que comprove que seus projetistas partiram da máquina de Harvard.
- **Caches L1 de instruções e de dados separadas** têm motivação de desempenho. A semelhança funcional com o modelo clássico **não** demonstra herança direta.

> **Regra adotada:** só se afirma influência quando há fonte que a documente (citação, relato de projetistas, registro de projeto). Semelhança funcional isolada não é tratada como causalidade.

---

## 9.6 Relevância técnica e histórica

### Relevância técnica

- Permite compreender **por que separar instruções e dados** melhora o desempenho: acesso simultâneo, menos disputa por barramento e tempos de acesso mais previsíveis.
- Esclarece o **compromisso** em relação à memória única: a Harvard ganha vazão e previsibilidade, mas perde a flexibilidade de tratar o programa como dado.
- Explica escolhas de projeto atuais, como as **variantes modificadas** e as **caches L1 divididas**, nas quais o princípio é aplicado com tecnologia diferente.
- Ajuda a distinguir **conceito de implementação**: a ideia vale independentemente do meio físico.

### Importância histórica

- A arquitetura está ligada ao **Harvard Mark I**, marco da computação programável eletromecânica nos anos 1940.
- É um dos **dois grandes modelos de referência** usados para descrever a organização de memória de computadores, ao lado do modelo de memória única.
- Seu nome vem de uma **atribuição posterior**; por isso, o lugar histórico é o de um conceito nomeado retrospectivamente a partir de uma máquina pioneira, e não o de origem comprovada de todos os sistemas que hoje separam instruções e dados.

### Separação resumida

| Dimensão | O que o estudo mostra |
|---|---|
| **Técnica** | O princípio de separação entre instruções e dados segue útil em projetos atuais, independentemente da tecnologia. |
| **Histórica** | O modelo nasce da experiência eletromecânica do Mark I e consolidou-se como referência conceitual; sua ligação com sistemas modernos é de classificação, não de sucessão direta comprovada. |

---
