# 5. Arquitetura de barramentos e interconexões

## 5.1 Visão geral

Em uma GPU moderna, o termo barramento abrange tanto a conexão externa com o
computador quanto as interconexões internas entre unidades de execução e
memória. A CPU envia comandos e dados pelo PCI Express (PCIe), enquanto o
controlador de memória da GPU conecta os SMs ou CUs à VRAM. Em servidores,
NVLink e tecnologias equivalentes podem conectar GPUs entre si com maior
largura de banda.

Esse fluxo resolve o gargalo que motivou a criação da GPU: a CPU foi projetada
para executar instruções complexas com baixa latência, mas os jogos 3D
passaram a exigir milhões de cálculos geométricos por segundo. A GPU assume o
trabalho repetitivo e a CPU continua coordenando a aplicação e a lógica geral.

## 5.2 Barramento de dados

O caminho de dados transporta comandos, instruções, entradas e resultados. Um
kernel pode receber dados da RAM, ser transferido para a memória da GPU e
produzir um resultado na VRAM. O DMA permite que uma transferência entre
memória e dispositivo ocorra sem que a CPU copie cada palavra individualmente.

## 5.3 Endereçamento

A aplicação usa endereços virtuais administrados pelo sistema operacional,
driver e runtime. O driver mapeia esses endereços para regiões acessíveis pela
GPU. Dentro do kernel, cada thread calcula endereços de elementos de matrizes,
imagens ou tensores; a unidade de memória converte esses acessos para a
hierarquia de memória do dispositivo.

## 5.4 Controle

O caminho de controle transporta comandos de alocação, cópia, sincronização e
inicialização de kernels. Filas de comandos permitem que a CPU prepare
operações e que a GPU as execute de forma assíncrona. Eventos e barreiras
indicam quando uma operação terminou ou quando seus dados podem ser
consumidos por outra etapa.

## 5.5 Processo de comunicação

1. A aplicação solicita ao runtime a alocação ou a transferência dos dados.
2. O driver prepara os buffers e os comandos para a GPU.
3. O PCIe ou uma interconexão equivalente transporta os dados entre RAM e
   memória do dispositivo.
4. A GPU agenda o kernel nos SMs ou CUs, que acessam registradores, caches,
   memória compartilhada e VRAM.
5. O resultado permanece na GPU para outro kernel ou é copiado para a RAM.

Em uma placa dedicada, o PCIe funciona como uma rodovia de alta velocidade
para os comandos e dados entre CPU e GPU. A VRAM dedicada funciona como um
buffer de alta largura de banda para texturas, quadros, matrizes e tensores.
Depois do processamento, o pipeline de saída converte os resultados gráficos
em sinais de vídeo para o monitor, por interfaces como HDMI ou DisplayPort.

## 5.6 Largura e capacidade

A largura do enlace indica quantos bits podem ser transportados por ciclo,
enquanto a taxa de transferência também depende da frequência, da codificação e
da geração do protocolo. A largura do barramento de memória e a tecnologia da
VRAM determinam a largura de banda interna disponível para os SMs ou CUs.
Esses valores não devem ser confundidos com a capacidade da memória: largura
de banda mede fluxo por segundo; capacidade mede quanto pode ser armazenado.

## 5.7 Impacto no desempenho

Transferências pequenas e frequentes entre CPU e GPU sofrem com latência e
podem anular o ganho do processamento paralelo. Por isso, aplicações de alto
desempenho normalmente transferem lotes maiores, mantêm os dados na VRAM e
encadeiam kernels quando possível. Acesso coalescido, reutilização em cache ou
memória compartilhada e uma interconexão adequada reduzem os gargalos.

## 5.8 Comparação com arquiteturas atuais

| Interconexão | Uso principal | Característica |
|---|---|---|
| PCIe | CPU, RAM e GPU dedicada | Interoperável, mas mais lento que a memória interna da GPU |
| NVLink ou equivalente | GPUs e aceleradores em servidores | Maior largura de banda para comunicação entre dispositivos |
| Barramento de memória | SMs/CUs e VRAM | Sustenta o fluxo de dados durante os kernels |
| Memória compartilhada e caches | Threads e unidades próximas | Reduzem acessos à VRAM, com capacidade limitada |

Assim, a GPU não depende de um único barramento: seu desempenho resulta da
combinação entre a conexão externa, a interconexão interna e a forma como o
software organiza comandos e dados.

## 5.9 Relação entre CPU, GPU e vazão

| Aspecto | CPU | GPU |
|---|---|---|
| Foco | Finalizar uma tarefa rapidamente | Processar milhares de dados por segundo |
| Núcleos | Poucos núcleos complexos | Muitas unidades matemáticas simples |
| Estrutura | Cache amplo e controle preditivo | ALUs organizadas para repetição |
| Melhor uso | Controle, decisões e tarefas sequenciais | Pixels, matrizes, tensores e simulações |

CPU e GPU são complementares. O throughput elevado da GPU só aparece quando há
trabalho independente suficiente e os dados chegam pela interconexão no ritmo
necessário.
