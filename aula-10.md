# Aula 10

Para verificar overflow na adição de números sem sinal:

    addu $t0, $t1, $t2
    nor $t3, $t1, $zero #primeiro passo p/ comp. a dois

Até aqui, $t3

=2³² - $t1 - 1. Note que, se $t3 < $t2, então:

    $t3 = 2³²-$t1-1 < $t2
    => 2³²-1<$t1+$t2

Logo, se $t3 < $t2, então, há

    overflow:
        sltu $t3, $t3, $t2
        bne $t3, $xero, overflow

## Multiplicação

numa multiplicação MxQ, temos M o multiplicando e Q o multiplicador.
 
      M = 1000 bin
    x
      Q = 1001 bin
      -----------
          1000  
         0000
        0000
       1000
      -----------
       1001000 bin

> Consideremos Q=qn*(qn+1)\*(qn+2)\*(qn+3)...\*q2\*q1*q0

A cada etapa i da multiplicação, há i deslocamentos a esquerda de (q x M. Ao final, todos os deslocamentos são somados, derando o produto. Ou seja:

    P = M(q0*2⁰+q1*2¹+q2*2²+...+qn*2^n)

Como  o produto costuma ser muito maior que os operandos, a arquitetura implementa um registrador especial de 64 bits para armazenar o produto. Note qu esse espaço costuma ser suficiente pois a multiplicação de um número de n bits por um de m bits resulta num número de n+m bits.


### Algoritmos

Consideramos que os operandos representam números ***sem sinal***. Para realizar a multiplicação, o algoritmo mais trivial (igual fazemos no papel) é o seguinte.

    Dados: M o multiplicando e Q o multiplicador.
    Saída: P o produto
    Passo1: Inicialize P=0 e contador=1.
    Passo2: Faça P= P+q0*M.
    Passo3: Faça o deslocamento lógico de um bit à esquerda, em M.
    Passo4: Faça um deslocamento lógico de um bit à direita em Q.
    Passo5: Se contador = 32, pare. Se não, contador++ e volte ao Passo2

Para implementar esse algoritmo, a arquitetura precisa dos seguintes registradores:

    M |         64 bits            |
                Q |    32 bits     |
    P |         64 bits            |

Exemplo:

    M |          1000| 8 bits
    Q       |    1000| 4 bits
    P |          1000| 8 bits

    M |         10000| 8 bits
    Q       |     100| 4 bits
    P |          1000| 8 bits

    M |        100000| 8 bits
    Q       |      10| 4 bits
    P |          1000| 8 bits

    M |       1000000| 8 bits
    Q       |       1| 4 bits
    P |          1000| 8 bits

    M |       1000000| 8 bits
    Q       |       1| 4 bits
    P |       1001000| 8 bits

Não é o necessário um registrador de 64 bits para o multiplicando usando a estrutura:

        Multiplicando
              | 32 bits
              v
     --> ULA de 32 bits
     |         |
     |         v     Produto
     |   | desloca à direita  | 64 bits
     |        ^
     |________|

Chegamos ao seguinte algoritmo:

    Passo1: P[62...32] = 0.
    Passo2: P[31...0] = Q.
    Passo3: Se P[0]=1, faça
            P[62...32]=P[62...32]+M.
    Passo4: Desloque P um bit à direita.
    Passo5: Se contador = 32, pare. Se não, contador++ e volte ao Passo3


Exemplo: 
    
    3dec*2dec = 0011bin\*0010bin

    It  Passo               Produto
    0   Valores iniciais    0000 0010
    1   3                   0000 0010
        4                   0000 0010
    2   3                   0011 0001
        4                   0001 1000
    3   3                   0001 1000
        4                   0000 1100
    4   3                   0000 1100
        4                   0000    0110