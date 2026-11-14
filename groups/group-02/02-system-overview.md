# 2. System Overview
Propósito: Automatizar a busca estatística das chaves da máquina alemã Lorenz.
Arquitetura: Processamento digital em tempo real através de fiação e hardware fixo.
Sincronismo: Fita de papel em loop fornece os dados e dita o ritmo (clock) do sistema.
Cálculo: Válvulas testam combinações lógicas em alta velocidade simulando as engrenagens alemãs.
Saída: Impressão automática do código descoberto assim que a meta estatística é atingida.

## 2.1 General Description
O Colossus era uma máquina de grande porte, pesando cerca de 5 toneladas e ocupando o tamanho de uma sala inteira (aproximadamente 5 metros de largura por 2 metros de altura). Ele não tinha uma tela ou sistema operacional como os computadores atuais; sua interface era composta por painéis cheios de interruptores, cabos e luzes.
## 2.2 Main Components
Válvulas Termiônicas: Realizavam os cálculos lógicos em altíssima velocidade.
Leitor Óptico: Convertia os furos da fita de papel em pulsos elétricos.
A "Cama de Solteiro": Estrutura mecânica de polias que girava a fita de papel.
Painéis de Conexão: Cabos e interruptores usados para programar a máquina manualmente.
Geradores de Impulsos: Sincronizavam a leitura dos dados com o processamento.
Circuitos Lógicos: Testavam as combinações matemáticas da cifra alemã.
Máquina de Escrever Elétrica: Imprimia automaticamente os resultados finais obtidos.

- CPU - circuitos de válvulas termiônicas,
- Memory - não tinha RAM; a fita de papel perfurado funcionava como uma memória de leitura instantânea (armazenando a mensagem alemã), enquanto as válvulas mantinham pequenos dados e estados lógicos temporários.
- Bus - Feito de forma física e estática através dos painéis de cabos (plugboards) e interruptores, que direcionavam o fluxo de pulsos elétricos entre os leitores e os circuitos lógicos
- I/O - A entrada de dados (Input) era o leitor óptico fotocélula das fitas perfuradas; a saída (Output) era a máquina de escrever elétrica que imprimia as configurações decifradas.

## 2.3 Block Diagram

(Add image)

## 2.4 Data Flow
Leitura Física: A fita de papel perfurado com a mensagem alemã gira continuamente nas polias.
Geração de Bits: Fotocélulas convertem os furos da fita iluminada em pulsos elétricos binários.
Sincronização: Furos centrais geram um sinal de clock de 5 kHz para ditar o ritmo.
Roteamento: Painéis de cabos direcionam os pulsos elétricos para as válvulas de cálculo.
Combinação Lógica: Válvulas fazem operações (como XOR) entre a fita e simulações das rodas alemãs.
Acúmulo de Resultados: Contadores eletrônicos somam os acertos e coincidências estatísticas geradas no teste.
Disparo de Saída: Ao atingir a meta estatística, o circuito envia a chave para a impressora.

Explain how data moves through the architecture.
• Entrada ➔ Barramento: O leitor óptico converte os furos da fita em bits elétricos.
• Barramento ➔ CPU: Os cabos dos painéis direcionam esses bits para as válvulas.
• CPU ➔ Memória: As válvulas processam a lógica (XOR) e somam os acertos nos contadores.
• CPU ➔ Saída: Ao atingir a meta estatística, o resultado é enviado e impresso.

## 2.5 Main Characteristics
Pioneiro: Primeiro computador digital, eletrônico e programável do mundo.
Veloz: Lia e processava 5.000 caracteres por segundo.
Eletrônico: Equipado com até 2.400 válvulas termiônicas.
Programável: Configurado manualmente via cabos e interruptores.
Especializado: Criado exclusivamente para quebrar a cifra Lorenz.
Gigante: Pesava 5 toneladas e ocupava uma sala.
Secreto: Mantido sob sigilo total.
Table

| Feature | Description |
|---------|-------------|
