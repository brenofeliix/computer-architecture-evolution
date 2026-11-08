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
## 9.3 Industrial Impact

## 9.4 Social Impact

## 9.5 Influence on Modern Computing

## 9.6 Lessons Learned

## 9.7 Future Perspectives
