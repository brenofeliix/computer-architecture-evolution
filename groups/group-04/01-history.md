# 1. Historical Background and Evolution

Contexto histórico, evolução tecnológica e linhagem da arquitetura **RISC-V**.

---

## 1.1 Architecture Name
**RISC-V** (pronuncia-se *"risk-five"*): Quinta geração dos projetos acadêmicos de arquiteturas de computação com conjunto reduzido de instruções (*Reduced Instruction Set Computer*) concebidos na Universidade da Califórnia em Berkeley.

---

## 1.2 Year of Creation
Desenvolvimento iniciado formalmente em **maio de 2010**, com a primeira versão estável e congelamento da base do conjunto de instruções ratificados em **2014**.

---

## 1.3 Country
Estados Unidos (UC Berkeley, Califórnia) na sua criação; atualmente com governança global neutra sediada na **Suíça**.

---

## 1.4 Inventor(s)
* **Krste Asanović** (Professor da UC Berkeley);
* **Andrew Waterman** (Pesquisador e estudante de pós-graduação na UC Berkeley);
* **Yunsup Lee** (Pesquisador e estudante de pós-graduação na UC Berkeley);
* **David A. Patterson** (Professor emérito da UC Berkeley, pioneiro do conceito RISC e autor das principais obras de referência em arquitetura de computadores).

---

## 1.5 Organization and Governance Evolution
* **2010–2015:** Conduzido pelo laboratório *Parallel Computing Laboratory* (Par Lab) da UC Berkeley como projeto de pesquisa e ensino.
* **2015–2019:** Criação da *RISC-V Foundation* nos Estados Unidos, reunindo dezenas de empresas de semicondutores para padronizar e ratificar extensões da arquitetura.
* **2019–Presente:** Incorporação da **RISC-V International** como uma associação internacional sem fins lucrativos sob a legislação suíça. A mudança da sede para Genebra foi uma decisão estratégica para garantir neutralidade jurídica e política global, impedindo que restrições comerciais ou embargos estatais unilaterais prejudicassem o acesso ao padrão.

---

## 1.6 Antecedentes e Linhagem Tecnológica

O RISC-V é a culminação de quatro décadas de pesquisa e lições aprendidas em microarquitetura:

1. **RISC I e RISC II (UC Berkeley, 1981–1984):** Liderados por David Patterson, demonstraram que compiladores e processadores simples, baseados em instruções regulares de um único ciclo e janelas de registradores (*register windows*), superavam em velocidade processadores CISC complexos da época (como o VAX e o Intel 8086).
2. **SOAR e SPUR (RISC III e RISC IV, 1984–1989):** Explorações pioneiras de RISC voltadas para linguagens dinâmicas (Smalltalk e Lisp) e multiprocessamento em memória compartilhada.
3. **MIPS e SPARC (1985–2000):** A popularização comercial do paradigma RISC na indústria e meio acadêmico.
4. **O Esgotamento das Alternativas Proprietárias (2010):** Ao iniciar novos projetos de chips no laboratório de Berkeley, os pesquisadores procuraram ISAs existentes. Arquiteturas x86 e ARM estavam protegidas por patentes e exigiam acordos caros e complexos; o MIPS e o SPARC acumulavam instruções obsoletas e débitos técnicos. Decidiu-se, portanto, criar uma ISA limpa, moderna e definitiva.

---

## 1.7 Principais Mudanças e Decisões de Design (O que Permaneceu vs. O que Foi Abandonado)

A evolução do RISC-V destaca-se por aprender com os erros e acertos de gerações anteriores de computadores:

### Características que Permaneceram
* **Filosofia Load/Store estrita:** Apenas instruções explícitas de leitura (`load`) e escrita (`store`) tocam a memória RAM; todas as operações lógicas e aritméticas ocorrem estritamente entre registradores.
* **Registrador Zero fixado em Hardware (`x0`):** Presente desde o RISC I e MIPS, simplifica significativamente a síntese de circuitos, permitindo derivar instruções pseudo-úteis (como `nop`, `mv`, `neg`) sem gastar opcodes adicionais.
* **Ortogonalidade dos Registradores:** Qualquer registrador de uso geral (`x1` a `x31`) pode receber o resultado de qualquer operação de números inteiros.

### Características Abandonadas (Eliminação de Débito Técnico)
* **Eliminação dos *Branch Delay Slots*:**
  * *Histórico:* No MIPS e SPARC dos anos 1980, a instrução imediatamente após um desvio condicional sempre executava devido ao atraso de cálculo do pipeline de 5 estágios.
  * *Mudança no RISC-V:* Eliminado completamente. Em processadores modernos com previsão dinâmica de desvios (*Branch Predictors*) e execução fora de ordem (*Out-of-Order*), os delay slots tornaram-se um gargalo inútil que desperdiçava área e complicava a recuperação de exceções.
* **Eliminação de Instruções com Microcódigo Interno:** Nenhuma instrução executa sequências implícitas multietapas em hardware (ao contrário de `pusha`/`popa` do x86 ou instruções de carregar múltiplos registradores do ARM antigo).
* **Eliminação de Janelas de Registradores (*Register Windows*):** Embora usadas no RISC I e SPARC, consumiam muita área de silício e causavam *overheads* imprevisíveis de chaveamento de contexto no sistema operacional.
* **Eliminação de Modos Complexos de Endereçamento:** O RISC-V aboliu o auto-incremento de ponteiros e endereçamentos indiretos com múltiplas somas e escalas em uma só instrução, simplificando radicalmente os estágios de decodificação e cálculo de endereço.

---

## 1.8 Evolução Tecnológica e Futuros Sucessores

* **Base Imutável e Extensibilidade Modular:**
  * Ao contrário de arquiteturas do passado que precisavam quebrar compatibilidade a cada década, a base inteira (`RV32I` e `RV64I`) foi matematicamente simplificada e formalmente congelada. Um binário compilado hoje para RV32I rodará exatamente da mesma forma em um chip RISC-V daqui a 50 anos.
* **Perfis de Plataforma (RVA20, RVA22, RVA23):**
  * Para solucionar o receio de fragmentação no mundo dos sistemas operacionais (Linux/Android), a *RISC-V International* estabeleceu perfis de aplicação (RVA) que agrupam conjuntos obrigatórios de extensões para cada geração de computadores pessoais e servidores corporativos.
* **Relação com Sucessores e Tecnologias Futuras:**
  * O RISC-V não busca ser substituído por uma nova ISA, mas servir de fundamento para novas microarquiteturas de silício:
    * Integração com **Chiplets** abertos (padrão UCIe);
    * Aceleradores dedicados à Inteligência Artificial e Aprendizado de Máquina (acoplados via barramentos abertos e instruções customizadas);
    * Plataforma padrão para **Computação Quântica e Fotônica**, onde os controladores de pulsos e estados físicos dependem de núcleos RISC-V embarcados de altíssima precisão temporal e zero overhead.

---

## Referências
* ASANOVIĆ, K.; PATTERSON, D. A. *Instruction Sets Should Be Free: The Case For RISC-V*. UC Berkeley Technical Report No. UCB/EECS-2014-146, 2014.
* PATTERSON, D. A.; WATERMAN, A. *Guia Prático RISC-V: O Atlas de uma Arquitetura Aberta*. Strawberry Canyon, 2017.
* RISC-V INTERNATIONAL. *The RISC-V Instruction Set Manual, Volume I: Unprivileged ISA*. Document Version 20191213. Disponível em: <https://riscv.org>.
