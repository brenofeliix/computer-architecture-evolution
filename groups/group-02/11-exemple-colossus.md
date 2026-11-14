# Issue 11 — Exemplo Técnico de Funcionamento

## Passo a Passo da Operação

### 01. ENTRADA
#### 1. Entrada (Fita de Papel Perfurada e Leitor Óptico)
Uma fita de papel perfurada fechada em loop contínuo contendo a mensagem alemã interceptada e codificada em 5 canais (código Baudot/ITA2), além de uma trilha de perfurações menores para sincronismo (sprocket holes).

#### Componentes participantes:
Sistema de polias (Bedstead), lâmpadas de projeção e matriz de células fotoelétricas.

#### Como as informações circulam:
A fita é tracionada em alta velocidade (cerca de 5.000 caracteres por segundo). Conforme a fita passa pelo leitor óptico, a presença ou ausência de furos permite ou bloqueia a passagem de luz até as células fotoelétricas, convertendo os dados mecânicos da fita em pulsos elétricos binários (0 e 1) em tempo real.

### 2. PROCESSAMENTO 

#### 2. Processamento (Configuração, Portas Lógicas e Comparação)

Como o sistema é configurado ou programado?

O Colossus não utilizava programas armazenados em memória. Antes de iniciar a leitura da fita, os operadores configuravam manualmente o painel de controle (plugboard e chaves rotativas) definindo:
O padrão de pinos conhecidos das rodas internas (como as rodas x1 e x2 da máquina Lorenz).
A função lógica booleana a ser aplicada entre os sinais da fita e os sinais gerados internamente.   O valor mínimo de coincidências (Set Total) necessário para considerar um resultado relevante.

Componentes participantes:
Anéis de válvulas tiratron (Thyratron Rings que simulam as rodas da máquina Lorenz), painel de interconexão (patch panel) e unidades lógicas compostas por válvulas termiônicas.

#### Quais operações são realizadas?

A cada pulso de sincronismo (clock) gerado pelo furo de guia da fita, os sinais da mensagem cifrada (Z) encontram os sinais gerados pelos anéis internos (x) nas portas lógicas. O circuito executa operações booleanas em paralelo (XOR, AND, OR, NOT) — por exemplo, verificando a igualdade estatística:
   
   Coincidência = ((z1 + z2) + (x1 + x2))
   
Sempre que a comparação lógica resulta em verdadeiro (1), um pulso elétrico é enviado para a etapa seguinte.

### 3. RESULTADO 

#### 3. Resultado (Contagem Eletrônica e Avaliação de Padrões)

#### Como o resultado é produzido?

Os pulsos positivos vindos das portas lógicas são acumulados em contadores eletrônicos de década (feitos com válvulas biestáveis) durante uma volta completa da fita.

#### Como as informações circulam no fechamento do ciclo:
Quando o sensor óptico detecta a marca de fim da fita, o sistema compara o total acumulado nos contadores com o limiar configurado nas chaves:  

Abaixo do limiar (Ruído): O valor é descartado, os contadores são zerados, o anel eletrônico interno avança uma posição (stepping) e uma nova combinação começa a ser testada imediatamente.

Acima do limiar (Padrão útil encontrado): O circuito aciona um relé que envia os dados para a teleimpressora elétrica, registrando no papel a contagem obtida e a posição exata das rodas naquela tentativa

#### Interpretação final:

Ao final da execução, os criptoanalistas avaliam a listagem impressa com as pontuações mais altas para identificar o alinhamento correto das rodas da máquina Lorenz e prosseguir com a decifração completa da mensagem.
