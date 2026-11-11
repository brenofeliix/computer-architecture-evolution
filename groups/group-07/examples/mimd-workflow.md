# 11. Exemplo Técnico de Funcionamento: Processamento Paralelo de Vetores em MIMD

## 11.1 Cenário Escolhido
O cenário escolhido para demonstrar o funcionamento prático da arquitetura MIMD é a **Soma e Transformação Paralela de um Vetor de Dados** distribuído entre múltiplos núcleos de processamento independentes.

---

## 11.2 Passo a Passo da Execução

### 1. Qual é a Entrada
- **Dados de Entrada:** Uma matriz ou vetor numérico de grande porte (ex: `dados = [10, 20, 30, 40]`).
- **Instruções de Entrada:** Um programa de software estruturado em múltiplas tarefas independentes (threads ou processos), onde cada tarefa define uma operação aritmética autónoma a ser aplicada sobre uma fração do vetor.

### 2. Como o Sistema é Configurado ou Programado
- O sistema operacional e o runtime da aplicação alocam fluxos de execução distintos.
- Através de primitivas de programação concorrente (como bibliotecas de threads ou MPI em sistemas distribuídos), define-se a **afinidade de threads**, mapeando explicitamente cada sub-tarefa para um núcleo de CPU físico independente, garantindo fluxos de controle assíncronos e desacoplados (*Multiple Instruction*).

### 3. Quais Componentes Participam
- **Múltiplos Núcleos de CPU:** Cada um dotado da sua própria Unidade de Controle (UC), Contador de Programa (PC) e Registradores.
- **Hierarquia de Cache (L1/L2/L3):** Armazenam localmente os dados e as linhas de cache de cada núcleo.
- **Memória RAM Compartilhada / Barramento de Sistema:** Onde residem os dados globais e por onde trafegam os pedidos de leitura/escrita e os sinais de coerência de cache (ex: protocolo MESI).
- **Unidade Lógica e Aritmética (ALU):** Responsável por executar efetivamente os cálculos matemáticos.

### 4. Como as Informações Circulam
1. **Carregamento:** Os blocos de dados iniciais são lidos do armazenamento de massa para a memória RAM.
2. **Distribuição via Barramento:** Cada núcleo emite um sinal de requisição (*Bus Request*). O árbitro de barramento concede o acesso, e os dados específicos de cada fatia do vetor fluem através do barramento de dados para as caches L1/L2 dos núcleos correspondentes.
3. **Isolamento de Estado:** Cada núcleo processa exclusivamente o seu fluxo de dados sem interferir diretamente no espaço de trabalho dos demais (*Multiple Data*).

### 5. Quais Operações são Realizadas
- Instruções de carregamento (*Load*) para trazer os elementos do vetor para os registradores locais.
- Instruções aritméticas independentes executadas em paralelo pelas ALUs de cada núcleo (ex: multiplicação de cada elemento por um fator escalar, `resultado = dado * 2`).
- Instruções de armazenamento (*Store*) para devolver os resultados processados às respetivas posições de memória ou buffers de saída.

### 6. Como o Resultado é Produzido
- Os resultados parciais calculados de forma assíncrona por cada núcleo são depositados nas respetivas regiões de cache ou memória compartilhada.
- Uma rotina de sincronização final (como uma barreira ou junção de threads) aguarda a conclusão de todos os fluxos independentes, consolidando os blocos transformados num vetor de saída final coerente e integrado.