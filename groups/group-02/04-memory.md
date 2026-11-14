# 4. Memory Organization
Externa (Leitura): A fita de papel perfurado funcionava como armazenamento da mensagem, lida continuamente em loop.
Interna (Registradores): Circuitos de válvulas termiônicas que guardavam apenas bits individuais para simular os estados das rodas alemãs.
Acúmulo (Contadores): Bancos de válvulas que somavam temporariamente os acertos estatísticos de cada teste.
Sem Armazenamento de Software: O programa não ocupava memória; a lógica ficava gravada na fiação física dos painéis.
## 4.1 Memory Hierarchy
era baseada na velocidade do hardware físico e de seus componentes eletrônicos. Como não existia memória RAM ou discos magnéticos, a pirâmide de velocidade e capacidade funcionava assim:
        / \      1. Registradores (Válvulas) -> Mais rápidos (Tempo real)
       /   \      2. Contadores (Válvulas)    -> Médio/Rápido (Acúmulo)
      /     \     3. Fita de Papel Perfurado  -> Mais lenta (Armazenamento físico)
     /_______\
## 4.2 RAM
Não existia RAM
## 4.3 ROM
Não era um chip eletrônico, mas sim um sistema mecânico e físico:
A Fita de Papel como ROM: A fita de papel perfurado funcionava exatamente como uma memória ROM. Os dados da mensagem alemã eram gravados nela de forma permanente (através dos furos) e a máquina apenas lia esses dados repetidamente, sem poder alterá-los.
Rodas Mecânicas Auxiliares: O Colossus possuía uma série de interruptores que imitavam os padrões fixos das engrenagens da máquina alemã Lorenz. Esses padrões eram "estáticos", servindo como tabelas de consulta de apenas leitura para o processador.
Sem Código Gravado: Diferente dos computadores modernos que usam a ROM para guardar o sistema operacional (BIOS), o Colossus não precisava disso, pois sua inicialização e comportamento eram definidos mudando a fiação nos painéis.

## 4.4 Cache
Não possuia.

## 4.5 Storage
Dados (Mensagem): Armazenados de forma permanente e apenas para leitura na fita de papel perfurado (equivalente a uma ROM/Disco Rígido).
Estados das Rodas: Armazenados temporariamente em bits individuais nos registradores de válvulas termiônicas (equivalente aos registradores da CPU).
Resultados dos Testes: Armazenados e somados de forma volátil nos contadores eletrônicos baseados em válvulas.
Instruções (Programa): Não ocupavam memória; ficavam gravadas fisicamente na fiação dos painéis de cabos (plugboards).

## 4.6 Memory Capacity
Era incrivelmente limitada para os padrões atuais e distribuída assim:
Memória Eletrônica Interna (Registradores e Contadores): Cerca de poucos bytes ou bits no total, servindo apenas para guardar estados de engrenagens (0 ou 1) e somas estatísticas acumuladas.
Memória de Massa Externa (Fita de Papel): Podia armazenar milhares de caracteres (dependendo apenas do comprimento físico da fita de papel perfurado) que continham a mensagem criptografada.
Memória de Programa: Zero, pois as instruções não ocupavam espaço digital e eram gravadas na fiação física dos cabos.

## 4.7 Comparison with Modern Systems
Uma comparação direta e simples entre o Colossus e um computador atual (como o seu celular ou PC):
CPU: O Colossus usava 2.400 válvulas de vidro gigantes que esquentavam como lâmpadas; um celular moderno usa um único chip com bilhões de transistores microscópicos.
Velocidade (Clock): O Colossus calculava a 5 kHz (5 mil vezes por segundo); um computador moderno opera na casa dos 3 GHz (3 bilhões de vezes por segundo), sendo milhões de vezes mais rápido.
Memória RAM: O Colossus tinha zero de RAM (guardava apenas alguns bits em tempo real); um celular moderno tem de 8 a 16 Gigabytes de RAM para rodar vários aplicativos ao mesmo tempo.
Armazenamento: O Colossus lia uma fita de papel física que quebrava se girasse rápido demais; hoje usamos SSDs silenciosos que guardam milhões de arquivos digitalmente.
Programas (Software): No Colossus, para mudar de programa era preciso desligar a máquina e trocar cabos de lugar manualmente; hoje você muda de aplicativo com um clique na tela.
Table
