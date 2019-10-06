# Aula 11

# Continuação de Multiplicação (Algoritmo de Booth)

Os algoritmos de multiplicação vistos lidam com números sem sinal. Para aplicá-los em números com sinal, deve-se:

> Melhor que o da aula em visão de números de operações, menos tempo.

- 1 - Guardar o sinal dos operandos,
- 2 - Convertr os operandos em positivos (operação inversa do complemento a dois),
- 3 - Aplicar o algoritmo,
- 4 - Se os sinais dos operandos fossem diferentes, negar o resultado.

Para evitar essas conversões, há um terceiro método: o algoritmo de Booth. O algoritmo de Booth adiciona um bit menos significativo ao registrados produto, e consiste nos seguintes passos:

- Passo 1: Prodito[63...32] = Produto[-1] = 0
- Passo 2: Produto[31...0] = Q.
- Passo 3: Se Produto[0...-1] = 01, faça Produto[63...32]=Produto[63..32]+m. Senão, se o Produto[0...-1]=10, faça Produto[63...32]-=m.
- Passo 4: Faça o **deslocamento aritmético** do registrador Produto 1 bit à direita.
- Passo 5: Se não for a 32º repetição, volte ao Passo3.

Obs: Deslocamento aritmético significa, fazer um deslocamento lógico, porém preservando o valor do bit mais significativo.

Exemplo: 7(decimal) * 3(decimal) = 0111(bin) * 0011(bin)

|It.|Step| Product | Add 1 bit to right|
|---|-----|----------|-|
|0|Inicialização|0000 0011 0|
|1|P[7..4]-=M|1001 0011 0| 1100 1001 1|
|2| Passo 3 - Passo 4|1100 1001 1| 1110 0100 1|
|3| Passo 3 - Passo 4|0101 0100 1| 0010 1010 0|
|4| Passo 3 - Passo 4|0010 1010 0| **0001 0101 0** = 21|

---

Exemplo da aula passada: 8(decimal) * 7(decimal) = 1000(bin) * 0111(bin)

|It.|Step| Product | Add 1 bit to right|
|---|-----|----------|-|
|0|Inicialização|0000 0111|
|1|P[7..4]-=M|1000 0111| 0100 0011|
|2| Passo 3 - Passo 4|1100 0011 | 0110 0001 |
|3| Passo 3 - Passo 4|1110 0001 | 0111 0000 |
|4| Passo 3 - Passo 4|0111 0000 | **0011 1000** = 56|

---

## Instruções em assembly

Para fazer a multiplicação, o MIPS contém a instrução

    mul reg1, reg2

que inplementa algum dos algoritmos que vimos. Para implementá-las, a arquitetura oferece um par de registradores de 32 para simular o registrador Produto de 64 bits: são o **Hi** e o **Lo**

    |op1| * |op2| = |Hi|Lo|

As instruções mult e multu fazem a multiplicação de números com o sem sinal, respectivamente. Para acessar os dados nos registradores Hi e Lo, há as instruções mfhi (move from Hi) e mflo (move from Lo). Para tirar os dados dos registradores Hi e Lo, há as instruções mthi (move to Hi) e mtlo (move to Lo).

Exemplo:    mult $s0, $s1
            mflo $s3
            mfhi $s2