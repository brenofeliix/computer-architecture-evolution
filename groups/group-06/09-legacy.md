# 9. Evolução e legado

## 9.1 Ponto de partida histórico

O Harvard Mark I (Automatic Sequence Controlled Calculator) foi concebido por Howard Aiken em colaboração com a IBM e entrou em operação em Harvard em 1944. Era uma máquina eletromecânica: sua unidade de controle lia uma linha de instrução por vez de uma fita de papel, enquanto 72 contadores mecânicos armazenavam valores usados nos cálculos. Essa descrição é da máquina histórica específica (Harvard University, Collection of Historical Scientific Instruments, “About” e “Function”).

O Mark I ajuda a contextualizar a separação entre instruções e dados, mas não deve ser confundido com uma implementação eletrônica moderna de Harvard. A expressão “arquitetura Harvard” foi aplicada retrospectivamente a diferentes sistemas com memórias separadas; a relação entre o rótulo, o Mark I e as máquinas eletrônicas precisa ser descrita com cautela (Pawson, 2022).

## 9.2 Evolução do princípio

A organização Harvard atual é um modelo abstrato: caracteriza a separação entre espaços de instruções e dados e seus caminhos de acesso, enquanto deixa em aberto tecnologias, capacidades, larguras, instruções e componentes concretos. A partir desse princípio, diferentes processadores podem implementar memórias e interfaces separadas sem reproduzir a mecânica do Mark I (Arvind et al., 2015, cap. 5).

Em implementações posteriores, a separação pode ser estrita ou flexibilizada. Um microcontrolador pode manter memórias de programa e de dados distintas; outras máquinas podem expor um espaço mais unificado e usar estruturas internas separadas para instruções e dados. Esses exemplos demonstram decisões de projeto relacionadas, mas não comprovam, por si só, uma sucessão histórica direta a partir do Mark I (Pawson, 2022; Microchip Technology Inc., AVR documentation).

## 9.3 Contribuições e características preservadas

| Aspecto | O que permanece ou mudou | Limite da afirmação |
|---|---|---|
| Espaços de armazenamento | O modelo Harvard separa instruções e dados; implementações concretas escolhem como materializar essa separação. | A tecnologia e a capacidade não são determinadas pelo modelo. |
| Caminhos de acesso | Caminhos independentes podem permitir acessos concorrentes. | Não garantem simultaneidade em toda implementação nem pipeline. |
| Acesso ao programa como dado | Algumas implementações oferecem mecanismos específicos para ler dados da memória de programa. | Não se deve afirmar que todo sistema Harvard proíbe ou permite esse acesso. |
| Tecnologia do Mark I | Fita de papel, relés, eixos e contadores pertencem à máquina eletromecânica histórica. | Esses elementos não são requisitos da organização Harvard moderna. |

A contribuição técnica do modelo está em permitir analisar o compromisso entre acessos independentes e a flexibilidade de uma memória compartilhada. A utilidade do conceito independe da tecnologia física do Mark I; as vantagens e limitações concretas dependem da implementação (Arvind et al., 2015).

## 9.4 Influência e relação com sistemas posteriores

O termo “Harvard” é usado para classificar diferentes organizações com armazenamento separado de instruções e dados. A documentação do AVR, por exemplo, descreve uma implementação com memória de programa e de dados distintas. Isso sustenta a classificação do AVR como exemplo de organização separada; não prova que o projeto do AVR tenha sido derivado diretamente do Mark I (Microchip Technology Inc., AVR documentation; Pawson, 2022).

Caches separadas de instruções e dados também podem ser comparadas funcionalmente ao princípio de acessos independentes. Essa semelhança não demonstra influência causal: para afirmar uma linhagem histórica direta, seriam necessárias fontes sobre decisões dos projetistas e a transmissão documentada das ideias (Pawson, 2022).

## 9.5 Relevância técnica e histórica

Tecnicamente, a comparação entre Harvard e von Neumann ajuda a explicar como escolhas de armazenamento e comunicação podem afetar contenção, flexibilidade e desempenho, sem transformar um modelo em vencedor universal. Historicamente, o Mark I é relevante como máquina programável eletromecânica e como referência associada ao nome; a arquitetura abstrata não é idêntica a essa máquina nem a todos os sistemas posteriores classificados como Harvard (Harvard University, Collection of Historical Scientific Instruments; Arvind et al., 2015; Pawson, 2022).

## 9.6 Síntese

O legado estudado é um princípio de organização — separar instruções e dados — e não a preservação dos componentes físicos do Mark I. O relatório distingue fato histórico, característica do modelo e propriedade de implementação; por isso, descreve relações funcionais sem inferir automaticamente uma descendência técnica direta.