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
$k0-$k1         |   26-27       |   Reservado ao S.O.
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

    lw $t0, 20($a0) carrega uma palavra inteira do endereço de memória [$a0+20] no registrador $t0