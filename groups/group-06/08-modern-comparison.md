# 8. Comparação, desempenho e limitações

## 8.1 Escopo da comparação

A comparação usa a organização clássica de Harvard e o modelo clássico de von Neumann como modelos de organização da memória. Harvard separa os espaços de instruções e de dados; von Neumann os reúne em um espaço compartilhado. Esses rótulos, por si sós, não especificam uma ISA, uma microarquitetura, um processador, um clock ou um resultado de desempenho. A implementação determina os componentes físicos e o comportamento temporal (Arvind et al., 2015, cap. 5).

## 8.2 Comparação por critérios

| Critério | Harvard clássica | Von Neumann clássica |
|---|---|---|
| Organização da memória | Espaços separados para instruções e dados. | Espaço de memória compartilhado por instruções e dados. |
| Acesso | Caminhos independentes para os dois espaços no modelo clássico. | No arranjo de caminho compartilhado, instruções e dados disputam o recurso quando solicitados no mesmo intervalo. |
| Consequência possível | A independência pode permitir busca de instrução junto a um acesso a dados, se as memórias e o processador conseguirem atendê-los. | Um recurso compartilhado pode exigir arbitragem ou espera quando há solicitações concorrentes. |
| O que o modelo não fixa | Capacidades, larguras, tecnologias, latências, ISA ou execução em pipeline. | O número de portas, a largura, a latência ou o desempenho de uma máquina concreta. |

A comparação trata de organizações clássicas. Uma implementação pode incluir caches, múltiplos caminhos, memórias com mais de uma porta ou outras decisões que alterem a ocorrência de contenção; por isso, “von Neumann” não significa que toda máquina tenha necessariamente um único barramento físico (Arvind et al., 2015; MIT 6.175, Lab 5).

## 8.3 Comunicação e paralelismo, quando aplicável

Na organização Harvard, a memória de instruções fornece a instrução solicitada pelo processador pelo caminho de instruções. A memória de dados recebe pedidos de leitura ou escrita pelo caminho de dados. Como os caminhos são separados, os pedidos não precisam disputar o mesmo canal; acessos simultâneos, porém, dependem dos recursos e do controle da implementação.

No modelo clássico de von Neumann, instruções e dados compartilham o espaço de memória. Se a implementação também usar um recurso de acesso compartilhado que só possa atender uma solicitação por vez, uma busca de instrução e um acesso a dados concorrentes podem gerar um hazard estrutural: um pedido espera pelo recurso. Essa é uma condição do recurso compartilhado, não uma propriedade necessária de toda implementação rotulada como von Neumann (MIT 6.175, Lab 5).

A possibilidade de buscar instruções e acessar dados em paralelo não equivale à execução simultânea de duas instruções completas e não implica pipeline. Um pipeline é uma escolha de microarquitetura, com etapas e mecanismos próprios para controlar dependências e conflitos. Harvard pode favorecer a sobreposição de acessos, mas não determina a existência de pipeline nem garante um ganho fixo de desempenho (Arvind et al., 2015, cap. 5; MIT 6.175, Lab 5).

## 8.4 Desempenho e capacidade de processamento

Não há base para afirmar que Harvard é sempre mais rápida que von Neumann. Harvard descreve uma organização; sem escolher implementações concretas, não há frequências, tempos de acesso ou taxas de execução para comparar. Uma comparação quantitativa precisa indicar a máquina, a mesma tarefa, a precisão dos resultados, as condições de entrada e saída, o ambiente de execução e uma métrica comum. A independência dos caminhos pode reduzir uma disputa específica de acesso, mas o desempenho total também depende de latência, largura de banda, unidade de processamento, controle, dependências e características da carga de trabalho (Arvind et al., 2015).

## 8.5 Vantagens condicionais e limitações

A separação Harvard permite dimensionar os espaços de instruções e de dados de forma independente, o que pode ser útil quando os requisitos dos dois tipos de informação são diferentes. Se os espaços forem implementados em memórias distintas, a capacidade livre de um deles não é automaticamente transferida ao outro; essa consequência depende da implementação. A organização também requer recursos de memória e acesso próprios para cada espaço, com custos e complexidade que variam conforme o projeto.

Em contrapartida, o espaço compartilhado do modelo von Neumann pode permitir distribuir a memória disponível entre código e dados com maior flexibilidade, enquanto uma interface de acesso compartilhada pode criar contenção quando há pedidos concorrentes. Essas são tendências condicionais dos modelos; não determinam sozinhas custo, capacidade ou velocidade de um equipamento específico (Arvind et al., 2015, cap. 5).

## 8.6 Relação com máquinas históricas e implementações posteriores

O Harvard Mark I é um antecedente histórico associado ao nome: era uma calculadora eletromecânica cuja sequência de instruções era lida em fita de papel e cujos valores eram mantidos em contadores. Esses detalhes descrevem o Mark I e não definem as memórias, os componentes ou a tecnologia de toda organização Harvard eletrônica (Harvard University, Collection of Historical Scientific Instruments, “Function”; Pawson, 2022).

Microcontroladores AVR documentam uma implementação concreta com memória de programa e memória de dados separadas. Ela pode ilustrar decisões de uma implementação Harvard, mas suas instruções, registradores, capacidades e mecanismos de acesso pertencem à família AVR, não ao modelo geral (Microchip Technology Inc., AVR documentation). A semelhança entre essa implementação, caches divididas e o modelo abstrato não basta para demonstrar uma linha causal direta desde o Mark I; a relação histórica deve ser apoiada por documentação específica (Pawson, 2022).