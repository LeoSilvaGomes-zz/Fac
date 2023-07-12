# Aula 3

## As instruções elementares

Vimos as instruções

- add
- sub
- mul

Exemplo:

O que um compilador de C para arquitetura MIPS poderia produzir para a expressão:

    f = (g + h) - (i + j);

    add t0, g, h
    add t1, i, j
    sub f, t0, t1

Como o MIPS trabalha com registradores, as "variáveis" usadas nos comandos devem ser os próprios registradores. A arquitetura MIPS possui, no processador principal, 32 registradores.

Registradores   |   Numéricos   |   Descrição
--------------  |   ---------   |   -----
$zero           |   0           |   Constante zero
$at             |   1           |   Resevado ao assembler 
$v0-$v1         |   2-3         |   Retorno de funções
$a0-$a3         |   4-7         |   Argumentos de funções
$t0-$t7         |   8-15        |   Temporários
$s0-$s7         |   16-23       |   Salvos
$t8-$t9         |   24-25       |   Temporários
$k0-$k1         | $gp             |   28          |   Ponteiro Global
$sp             |   29          |   Ponteiro da pilha
$fp     26-27       |   Reservado ao S.O.
$gp             |   28          |   Ponteiro Global
$sp             |   29          |   Ponteiro da pilha
$fp             |   30          |   Ponteiro de frame
$ra             |   31          |   Endereço de Retorno

Exemplo:
    
    $s0 - f         add $t0, $s1, $s2
    $s1 - g         add $t1, $s3, $s4
    $s2 - h         sub $s0, $t0, $t1
    $s3 - i
    $s4 - j

Numa arquitetura, a quantidade de memória (de registradores) é limitado, e isso não é suficiente para lidar com programas complexos que contêm milhares de variáveis. Por isso, um computador possui unidades de memória de maior capacidade de armazenamento (como a memória RAM, o HD, etc). À memória de maior capacidade chamaremos de memória principal. A memória princiapl é vista como um grande vetor unidimensional.

Assim sendo, o MIPS possui instruções de transferência de dados que recuperam ou armazenam dados da/para a memória principal. São elas:

 - \b (bad byte) e lw (load word) carregam um byte de dado e uma palavra inteira da memória principal para o resgitrador, respectivamente.
 - sb (store byte) e sw (store word) salvam um byte e uma palavra do registrador para a memória, respectivamente.
 
Exemplo:

    lw $t0, 20($a0) carrega uma palavra inteira do endereço de memória [$a0+20] no r$gp             |   28          |   Ponteiro Global
$sp             |   29          |   Ponteiro da pilha
$fp   egistrador $t0

Essas instruções utilizam um endereçamento indexado, que é feito da seguinte forma: offset(reg-base), onde

- reg-base é o registrador que contém o endereço base
- offset é o deslocamento necessário a partir do endereço base

Importante: como cada palavra possui 4 bytes (= 32bits), o endereçamento da memória é feito de 4 em 4 unidades

### Memória princial:
Endereço | Dados | Posição
---      | ----  | --
16       |       | 4
12       |       | 3
 8       |       | 2         
 4       |       | 1            
 0       |       | 0

    Endereço = 4 * Posição

A sintaxe das instruções de acesso à memória é:

    op reg_operando, offset(reg_base)
                               |
                               V
                            endereço

Exemplo:

Suponhamos que A seja um vetor de 100 palavras e as variáveis g e h estejam associadas aos registradores $s1 e $s2. Suponha ainda que o endereço base de A esteja em $s3. Como compilar a instrução:

    g = h + A[8]
    
    lw $t0, 32($s3)
    add $s1, $s2, $t0

e quanto à instruções:
    
    A[12] = g+ A[8];

    lw $t0, 32($s3)     !=  $t0=A[8] 
    add $t0, $s2, $t0   !=  $t0=g + A[8]
    sw $t0, 48($s3)     !=  A[12]=$t0

    register int i;

Obs: No MIPS, endereço bases devem ser multiplos de 4. Isso é chamado restrição de alinhamento

Exercício:

Consideremos o código a seguir. Suponha que o endereço base de A está em $s3 e result, em $s4

    int A[4] = {1, 2, 3, 4};
    int result=A[0]+A[1]+A[2]+A[3]

    lw $t0, 0($s3)
    add $s4, $t0, $zero
    lw $t0, 4($s3)
    add $s4, $s4, $t0
    lw $t0, 8($s3)
    add $s4, $s4, $t0
    lw $t0, 12($s3)
    add $s4, $s4, $t0