# Aula 5

## Condicionais

Para o uso dos condicionais, um conceito é importante

    Em assembly MIPS, rótulo é uma string simples usada para nomear um endereço da memória

Exemplo:

        lw $t0, 1200($t1)
    L1: add $t0, $s2, $t0
    L2: sub $t0, $s2, $t0
        sw $t0, 1200($t1)

Em linguagem C, a instrução mais comum para desvio condicional é o **if**. Em assembly MIPS, há duas instruções equivalentes.

- 1 - **beq reg1, reg2, label** (branch if equal) significa desvio à instrução rotulada por **label** se o calor de reg1 for igual ao de reg2.

- 2 - **bne reg1, reg2, label** (branch if not equal) significa desvio à instrução rotulada por **label** se o valor de reg1 for diferente do valor de reg2.

<p>
Essas duas instruções são denominadas de <b>desvio condicional.</b><br>
Há também uma instrução de <b>desvio incondicional;</b><br>
</p>

- **j label** (jump) desvia o fluxo de execução para a instrução rotulada por label.

Exemplo: Suponha que f a j correspondam aos cinco registrdores $s0 a $s4. Qual será o código MIPS compilado destas instruções?

    if (i == j){
        f = g + h;
    } else {
        f = g - h;
    }

    f - $s0
    g - $s1
    h - $s2
    i - $s3
    j - $s4

Em MIPS:

Primeira alternativa:
 
        bne $s3, $s4, else
        add $s0, $1, $s2
        j exit

    else: sub $s0, $s1, $s2
    exit:

Segunda alternativa;

        beq $s3, $s4, if
        sub $s0, $s1, $s2
        j exit

    if: add $s0, $s1, $s2
    exit:

Observações: A execução em assembly é sequencial, e apenas instruções de desvio alteram esse fluxo de execução.

Para ajudar com os desvios, há ainda uma instrução para testes lógicos de comparação

- **slt res, reg1, reg2** (set on less than) atribui 1 ao registrador **res** se o valor **reg1** for menor que o valor em **reg2**, e 0 caso contrário.

- **slti res, reg, cons** é a versão imediata de slt.

Observações:

    - Chama-se de **bloco básico** um conjunto de instruções que não possui desvios.
    - Todas as operações de comparação podem ser implementadas em MIPS.

### Menor ou Igual

registradores:
$t0 <= $t1?

        slt $t2, $t1, $t0
        beq $t2, $zero, mi
        j exit

    mi: # $t0 <= $t1
    exit: # $t0 > $t1

### Maior

registradores:
$t0 > $t1?

    slt $t2, $t1, $t0

### Maior ou Igual

registradores:
$t0 >= $t1?

Duas possibilidades:

    $t0 >= $t1 ou $t0 < $t1
    slt $t2, $t0, $t1

    - Se $t2 = 0, então $t0 >= $t1.

---

**Laços:** com os desvios, é possível construir laços

Exemplo:

    while(A[i] == k){
        i = i + 1;
    }

    $s0 - base de A
    $s1 - i
    $s2 - k

Em MIPS:

com beq:

            add $s1, $s0, $zero
    loop:   lw $t0, 0($s1)
            beq $t0, $s2, if
            j exit

    if:     add $1, $s1, 4
            j loop
    exit:

com bne:

        add $s1, $s0, $zero
    loop:   lw $t0, 0($s1)
            bne $t0, $s2, exit
            add $1, $s1, 4
            j loop
    
    exit:

Exemplo;

    maior = A[0];
    for (i = 1; i < n; i++){
        if(A[i] > maior){
            maior = A[i];
        }
    }

    $s0 - end base do A
    $t0 - i
    $s1 - maior
    $s2 - n

Em MIPS;

    add $s1, $s0, $zero


## Simuladores

- MARS (+pseudoinstruções)
- SPIM