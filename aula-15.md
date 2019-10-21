# Aula 15

## Pontos flutuantes

As principais instruções do coprocessador 1 são:

1. Aritméticas:

    - adição;       add.s res, reg1, reg2
    - subtração:    sub.s res, reg1, reg2
    - multipliação: mul.s res, reg1, reg2
    - dicisão:      div.s res, reg1, reg2

Obs: Todas as instruções do processador 1 seguem a seguinte convenção: aquelas terminadas por **".s"** lidam exclusivamente com números de precisão **simples** e possuem uma **irmã** terminado em **".d"** que lida com números em precisão são dupla

Ex: 
    
    add.s $f0, $f1, $f2
    - faz $f0 = $f1 + $f2. Todos estão em precisão simples.
    add.d $f0, $f2, $f4
    - faz ($f0, $f1) = ($f2, $f3) + ($f4, $f5)

Todos os operandos estão em precisão dupla

2. Instruções de acesso à memória

    - l.s reg, offset(base): carrega um ponto flutuante da memória
    - s.s reg, offset(base): salva um ponto flutuante na memória.

Ex:

     |> reg. do proc. 1
    l.s $f0, 100($t2)   -> base (reg. do coprec. 0) 
               |> offset (número inteiro)

3. Movimentação de dados

    - mov.s reg1, reg2: faz reg1=reg2

Ex:

    mov.s $f0, $f1.

---

- mfc1  reg_c0, reg_c1 : reg_c0 = reg_c1 
- mtc1  reg_c0, reg_c1 : reg_c1 = reg_c0

Essa instruções copiam os dados de um registrador do coprocessador 0 p/ o 1 e vice-vers.

- mfc1: move **from** coprocessor 1
- mtc1: move **to** coprocessor 1

Obs: Não há conversão de dados nessa movimentação!

Ex:

``` c
    mfc1 $t0, $f0  # $t0 = $f0
```

4. Conversão de dados

As instruções de conversão são feitas no coprocessador 1 e seguem a seguinte lógica:

    instrução reg-dest, reg-origem

As instruções possuem o formato

    cvt. {d,s,w}.{d.s.w},

onde 

    - w representa inteiro,
    - s representa precisão simples e
    - d representa precisão dupla

logo, temos as instruções;

    - cvt.d.s   - cvt.w.d   - cvt.s.d
    - cvt.d.w   - cvt.w.s   - cvt.s.w

Ex: 

              |> double
              |    |> inteiro
    cvt.d.w  $f0, $f1
        | |> inteiro
        |> double

Converte um inteiro em $f1 para um double, que será armazenado em $f0

5. Desvios condicionais

São feitos em duas etapas

- a. Comparação entre valores dos registrafores e
- b. Desvio baseado no resultado da comparação

As instruções de comparação tem a estrutura

    instrução reg1, reg2 #compara reg1 com reg2 (nessa ordem)

As instruções são:

    - c.eq.s    - c.lt.s    - c.gt.s
    - c.ne.s    - c.le.s    - c.ge.s

As instruções de comparação salvam o resultado num bit controlador estpecial chamdao **code**. Se o resultado d comparação for verdadeiro, o bit 1 é salvo em code, em caso contrário, ) é salvo. Os desvios são feitos com base no valor de **code**. Há duas instruções de desvio:

- bc1t label # devia para **label** se code=1 (true)
- bc1f label # devia para **label** se code=0 (false)

Ex: Traduza o código a seguir;

    float f2c(float fahr){
        return (5.0/9.0)*(fahr - 32.0)
    }

---

         .data
    c5:  .float 5.0
    c9:  .float 9.0
    c32: .float 32.0

    f2c:    l.s  $f10, c5
            l.s  $f11, c9
            l.s  $f12, c32

            div.s $f10, $f10, $f11
            sub.v $f0, $f0, $f12
            mul.s $f0, $f10, $f0

            jr $ra