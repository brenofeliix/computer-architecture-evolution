# 11. Exemplo Técnico de Funcionamento: Multiplicação de Matrizes na GPU

## 11.1 Tarefa escolhida

A tarefa escolhida é a multiplicação de duas matrizes:

```text
C = A x B
```

Essa operação é representativa de GPUs porque cada elemento de `C` pode ser
calculado de forma relativamente independente. O mesmo padrão aparece em
renderização, simulações numéricas e redes neurais.

Considere:

- `A` com dimensões `M x K`;
- `B` com dimensões `K x N`;
- `C` com dimensões `M x N`.

Cada elemento do resultado é calculado por:

```text
C[row][col] = soma(A[row][k] * B[k][col]), para k de 0 até K - 1
```

## 11.2 Entrada

A entrada é formada por:

1. os valores da matriz `A`;
2. os valores da matriz `B`;
3. as dimensões `M`, `K` e `N`.

As matrizes são armazenadas como sequências de valores na memória. Em uma
representação linear por linhas, o elemento `A[row][k]` ocupa a posição
`row * K + k`, enquanto `B[k][col]` ocupa a posição `k * N + col`.

## 11.3 Configuração e programação

O programa executado pela CPU reserva memória para as matrizes, copia os dados
para a memória global da GPU e configura a grade de execução. O kernel abaixo
associa uma thread a cada posição da matriz de saída:

```cuda
__global__ void multiplicar(const float* A, const float* B, float* C,
                            int M, int K, int N) {
    int col = blockIdx.x * blockDim.x + threadIdx.x;
    int row = blockIdx.y * blockDim.y + threadIdx.y;

    if (row < M && col < N) {
        float soma = 0.0f;
        for (int k = 0; k < K; ++k) {
            soma += A[row * K + k] * B[k * N + col];
        }
        C[row * N + col] = soma;
    }
}
```

Uma configuração possível é:

```cuda
dim3 threads(16, 16);
dim3 blocks((N + threads.x - 1) / threads.x,
            (M + threads.y - 1) / threads.y);

multiplicar<<<blocks, threads>>>(A, B, C, M, K, N);
```

Cada bloco possui `16 x 16` threads. A divisão com arredondamento para cima
garante blocos suficientes mesmo quando `M` ou `N` não são múltiplos de 16.
Por isso, o teste dos limites dentro do kernel é necessário.

## 11.4 Componentes envolvidos

- **CPU:** prepara os dados, reserva os buffers, configura a execução e
  solicita o resultado.
- **Runtime e driver:** transferem os dados, carregam o kernel e informam a
  GPU sobre a grade de execução.
- **Memória global da GPU:** armazena `A`, `B` e `C`.
- **Blocos e threads:** distribuem as posições de `C` entre as unidades de
  execução.
- **Registradores:** guardam `row`, `col`, `k` e o acumulador `soma` de cada
  thread.
- **ALU:** realiza as multiplicações e somas de ponto flutuante.
- **Escalonador e SMs/CUs:** distribuem os blocos entre as unidades disponíveis.

## 11.5 Circulação das informações

1. A CPU cria ou recebe as matrizes `A` e `B` na memória do sistema.
2. O runtime reserva espaço para `A`, `B` e `C` na memória global da GPU.
3. Os valores de `A` e `B` são transferidos da RAM para a VRAM.
4. A CPU inicia o kernel com uma grade bidimensional de blocos.
5. Cada thread calcula suas coordenadas `row` e `col` usando os identificadores
   do bloco e sua posição dentro dele.
6. A thread acessa a linha correspondente de `A` e a coluna correspondente de
   `B`.
7. Os valores lidos são multiplicados, acumulados e usados para produzir um
   elemento de `C`.
8. A thread grava o acumulador na posição `C[row * N + col]` da memória global.
9. A aplicação aguarda a conclusão do kernel e copia `C` de volta para a RAM,
   quando o resultado precisa ser usado pela CPU.

## 11.6 Operações realizadas

Para cada posição válida de `C`, uma thread:

1. inicializa `soma` com zero;
2. percorre os `K` elementos necessários;
3. calcula o endereço de `A[row * K + k]`;
4. calcula o endereço de `B[k * N + col]`;
5. multiplica os dois valores;
6. adiciona o produto ao acumulador;
7. armazena o resultado final em `C[row * N + col]`.

Os blocos podem ser executados em diferentes SMs ou CUs, e as threads de um
mesmo bloco podem executar em grupos SIMT. O hardware decide a ordem de
execução dos blocos, desde que os recursos estejam disponíveis. Como cada
thread escreve uma posição diferente de `C`, não há uma atualização concorrente
do mesmo elemento nesse exemplo.

## 11.7 Produção e validação do resultado

Quando todas as threads terminam, a matriz `C` contém o produto de `A` por
`B`. Para verificar o resultado, a aplicação pode calcular algumas posições
na CPU ou comparar toda a matriz com uma implementação de referência,
considerando uma tolerância para diferenças de ponto flutuante.

O resultado também pode permanecer na memória da GPU para ser consumido por
outro kernel. Essa alternativa evita uma transferência desnecessária entre
VRAM e RAM. Se a aplicação precisar exibir, salvar ou processar o resultado na
CPU, o runtime realiza a cópia após a sincronização.

Assim, a tarefa completa envolve mais do que a execução do código CUDA: os
dados precisam ser preparados, transferidos e associados às threads corretas;
as unidades da GPU realizam os cálculos em paralelo; e o resultado precisa ser
disponibilizado ao próximo estágio do sistema.
