# Aula 22

##  Continuação da ultima aula

Da memória principal para o cache: dado um endereço de memória x e uma cache com m = 2^n blocos, cada bloco com c = 2^b bytes, o endereço (i,j) de x na cache será:


    i = (x/c)%m
    j = (x%c)

Ex: endereço 11 da memória principal:

    i = (11/4)%8 = 2%8 = 2
    j = 11%4 = 3

    11 = 001011

    001011 / 2² = srl 2 = 0010
    001011 % 2² = 11

Logo, no mapeamento direto por blocos:

1. Parte-se do endereço do dado na memória principal
    - O tamanho da memória principal é sempre potência de dois (Digamos 2^t)
2. Quanto à cache, devemos saber quantos blocos ela possui (quantidade de linha, 2^n) e qual o tamanho em bytes, de cada bloco (quantidade de colunda 2^b).

Deste modo:

Endereço da memória principal (t bits)

                    |               |
    tag             |   linha       |   coluna
    (t-(n+b) bits)  |   (n bits)    |   (b bits)
                    |               |

## Tamanho real da cache

Para calcular o tamanho real da memória cache

1. Deve-se identificar quantos blocos ela possui e qual o tamanho (em bytes) de cada uma.
2. Deve-se calcular o tamanho da tag com isso, o tamanho real da cache será:

```
    qtde.       x   |   1 + tamanho + tamanho   |
    de blocos       |       da tag    do bloco  |
```

Cuidado! O tamanho da tag geralmente é dado em bits, portanto, para usar a fórmula acima, o tamanho do bloco também deve estar em bits.

No nosso exemplo,

    Tamanho
    da cache    =   8   x   (1  +  1  + 32) = 8 x 34 bits
    (real)                                  = 34 bytes

Exemplo:

Qual o tamanho real de uma memória cache diretamente mapeada com 16kb e blocos de 4 palavras, considerando-se que a memória principal possui 4gb?

1. Memória principal = 4gb = 2² * 2³⁰B
                           = 2³²B

Memória cache:
-> Tamanho de um bloco = 4 palavras = 2⁴B
-> Quantidade de blocos = 16KB/16Kb = 1K = 2¹⁰

2. Tamanho da tag:

    Endereço da memória principal (32bits)

```
                |               |
    tag         |   linha       |   coluna
    18 bits     |   10 bits     |   4 bits
                |               |
```

    Tamanho
    real da cache = 2¹⁰ * (1 + 18 + 16 * 8)
                  = 2¹⁰ * 147 bits = 147 Kb
                  = 18,375 * 2¹⁰ bytes = 18,375 Kb

### Exemplo

Considere uma cache com 64 blocos e 16 bytes por bloco. Para qual endereço da cache o endereço 1200 da mamória principal é mapeado?

    K = 1200    |   i = 1200 / 16 = 75 % 64 = 11
                |   j = 1200 % 16 = 0

### Falahas de acesso

Dado end_mem na forma (bloco = 2^b)
                      (qtd.blocos = 2^n)

---

Quando houver uma falha (seja eka de dados ou de instrução), o processador fica parado aguardando o dado ficar disponível (gerando bolhas no caso de pipeline).

Quanto maior o tamanho do bloco, menor tende a ser a quantidade de falhas em decorrência da localidade de espacial. Todavia, há de se ter um trade-off entre o tamanho total da cache e o tamanho de cada bloco, pois à medida que o tamanho do bloco se aproxima do tamanho da cache, a localidade temporal seria desfavorecida. Além disso, quanto maior o tamanho do bloco, maior a penalidade de falha.