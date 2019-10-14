# Aula 13

### Continuação da aula passada

Obs: Note que ambos o quociente e o resto recebem deslocamentos à esquerda. Além isso, os bits menos significativos do resto permanecem nulos, Isso sugere um segundo refinamento no algoritmo de divisão.


    Passo 1: Salve o dividendo na porção menos significativa do Resto e     defina cnt=1.
    Passo 2: Faça um deslocamento à esquerda no Resto.
    Passo 3: Resto [63..32]-= Dividor.
    Passo 4: Se Resto >= 0, faça um deslocamento de 1 bit à esquerda no Resto   e defina Resto[0]=1. Se Resto<0 restaure o valor no resto e desloque-o 1 bit à esquerda.
    Passo 5: Se cnt < 32, cnt++ e volte ao Passo 3.
    Passo 6: Desloque Resto[63..32] 1 bit à direita.

Exemplo:

    4dec / 5dec = 0100 bin / 0101 bin

|It.|Step| Resto | 
|---|-----|----------|
|0|Inicialização|0000 0100|
|||0000 1000|
|1|3|1010 1000|
||4|0001 0000|
|2|3|1010 0000| 
||4|0010 0000|
|3|3|1010 0000| 
||4|0100 0000|
|4|3|1111 0000 | 
||4|1000 0000|
|- |srl Resto[63..32]|0100 0000|

---

### Agora com número negativo

    -7dec / 2dec = -3dec, -1dec
                   -4dec, +1dec

Para a divisão de inteiros com sinal:

 - 1 - Salve o sinal do dividendo e do divisor, e torne-os positivos.
 - 2 - Faça a divisão sando o algoritmo acima.
 - 3 - Faça com que o resto tenha o mesmo sinal que o dividendo tinha ao início.
 - 4 - Tome o quociente negativo se o sinal do dividendo era diferente que o do divisor.

 Exemplo:
    
### Sinais na divisão de números (resultado e resto)

      7 / 2 = 3, resto 1.
     -7 / 2 = -3, resto -1.
     7 / -2 = -3, resto 1.
    -2 / -2 = 3, resto -1.


## Instruções MIPS

Assembly MIPS, temos a instrução

    div    dividendo, divisor
                    |
                    v
            registradores

O resultado é salvo no par de registradores Hi elo:

    |Resto|Quociente|
      Hi      Lo

Deste modo:

    div $s0, $s1     # faz $s0 / $s1
    mflo $s2         # $s2 = quociente
    mfhi $s3         # $s3 = resto

## Ponto flutuante

A representação de números reais no computador baseia-se na **notação científica**. Nessa notação, o número é escrito com

    +- F * 10^E,

onde F é um real tal que 1 <= F < 10, chamado de **fração** (ou significando, ou mantissa) e E é um inteiro chamado **expoente**. Tal notação serve para representar qualquer número, por exemplo:

    3.155.760.000 = 3,155760 * 10⁹

Em binário, o formato geral da notação científica é:

    1,xxxxx...x * 2^yyyyyy... (**)

Essa notação recebe o nome de ponto flutuante porque para preservar o formato padrão (**), é necessário deslocar o ponto binário constantemente.

***Representação em ponto flutuante***

Um número real em notação científica é representado em uma palavra da seguinte forma:

|31|30|29|...|23|22|21|...|3|2|1|0| 
|--|--|--|--|--|--|--|--|--|--|--|--|
|Bit de sinal|\|||Expoente (8 bits)||\||| Fração (23 bits) ||||

Logo, o número em ponto flutuante possui o seguinte formato:

    (1)⁵ * F * 2^E

Essa é a representação e precisão simples (o float, em C).

Nessa representação:

- 1 - Se o número for muito grande de forma que o expoente não caiba nos 8 bits, temos  um **overflow**
- 2 - Se o número for muito pequeno de forma e expoente (muito próximo de zero), ocorre o que achamamos de underflow.

Casos assim ocorrem com frequência na representação de precisão simples. Pro isso, usamos com frequência a representação com **precisão dupla:**

|31|30|29|...|20|19|18|...|3|2|1|0| 
|--|--|--|--|--|--|--|--|--|--|--|--|
|S|\|||Expoente (10 bits)||\||| Fração (20 bits) ||||

|31|30|29|...|20|19|18|...|3|2|1|0| 
|--|--|--|--|--|--|--|--|--|--|--|--|
||||||Fração (32 bits)|||||||

Esses formatos seguem o padrão de ponto flutuante IEEE 754, usado amplamente nas arquiteturas desde a década de 1980.

Nesse padrão, para ampliar a capacidade de representação da fração, o 1 fica implícito e apenas a porção à direita da vírgula é representada.

A única exceção a essa representação é o zero. Ademais, temos:

    (-1)⁵ * (1+Fração) * 2^E

O padrão IEE 754 especifica a representação da seguinte forma:

    1,xxxx * 2^yyyy (2048)

Divisão por 0: **NaN**
Infinito: **Inf**