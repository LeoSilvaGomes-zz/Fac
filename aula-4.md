# Aula 4

**Observação**: É comum que programas possuam mais variáveis que a quantidade de registradores do processador. Por isso, é necessário escolher quais variáveis ficam na memória e quais nos registradores. Antigamente, o programador tomava essa escolha (veja o modificador register da linguagem C, por exemplo). Atualmente, o próprio compilador procura gerar um assembly que otimize essa escolha. Esse processo de escolha é chamado spilling registers.

## Assembly para Linguagem de Máquina
    
A representação do assembly no nível lógico consiste em traduzir cada instrução para uma representação numérica binária. Essa representação binária é o que chamamos de linguagem de máquina.

Para representar uma instrução em linguagem de máquina, é feita uma conversão da seguinte forma:

    operação op1, op2, op3   |   
                    |        |   -> Formato geral das instruções
                    v        |
              registradores  |


- Os registradores são mapeados de acordo com o número de 0 a 31 a que são associados (veja a tabela dos registradores).
- A cada operação é associado um ou dois identificadores numéricos

Exemplo: 

    A instrução
    add $t0, $s1, $s2

    é mapeado, em decimal, da seguinte forma.

    | 0 | 17 | 18 | 8 | 0 | 32 |

Cada um dos segmentos dessa representação é chamado campo.

- O primeiro e o último caracterizam a operação add.
- O segundo e o terceiro campus representam os operandos.
- O quarto campo é o resgistrador que recebe o resultado.
- O quinto campo é o deslocamento (Como é irrelevante no caso, recebeu zero).

A forma geral da representação desse tipo de instruções é:

    |   op   |   rs   |   rt   |   rd   | shamt  | funct  |
    | 6 bits | 5 bits | 5 bits | 5 bits | 5 bits | 6 bits |

- **op** é a operação (opcode)
- **rs** e **rt** são os operadores
- **rd** é o destino
- **shamt** é a quantidade de deslocamento (shift a mount)
- **funct** é o código da função (complementar opcode)

Este formato chama-se formato R, ou tipo-R. No exemplo acima, teríamos, para a máquina:


    | 000000 | 10001  | 10010  | 01000  | 00000  | 100000 |
    | 6 bits | 5 bits | 5 bits | 5 bits | 5 bits | 6 bits |

Considere agora a instrução:

    lw reg, offset(reg_base)

Ela precisa de dois registradores e uma constante numérica. Se utilizarmos o formato R, a constante deveria ser armazenada no campo **rd**, que possui 5 bits. Com isso, a constante limitaria-se a 2⁵ possibilidade, ou seja, variaria entre 0 e 31. Por essa razão, há ainda o formmato I ou do tipo-I.

    |   op   |   rs   |   rt   | const/endereço  |
    | 6 bits | 5 bits | 5 bits |    16 bits      |

Observação: O R vem de resgistrador e I vem de imediato. Há uma classe de instruções chamadas imediatas que lidam com constantes 9numéricas nas instruções ao invés de registradores

Exemplo: 

    add $t0, $s0, $s1
    addi $t0, $s0, 5

    A imediata usa o segundo operando como número
    Apenas numeros inteiros

Exemplo:

Seja **A** um vetor com endereço base em $s0 e uma variável **h** em $s1. O código:

### Assembly

    A[300] = h + A[300];
    lw $t0, 1200($s0)
    add $t0, %s1, $t0
    sw $t0, 1200($s0)

---

### Linguagem de máquina

|   op   |   rs   |   rt   |  (rd   | end/shamt  | funct) |
|--------|--------|--------|--------|------------|--------|
| 35     | 16     | 8      |            1200              |
| 0      | 17     | 8      | 8      | 0          | 32     |
| 43     | 16     | 8      |            1200              |

### Importante

- 1 Uma instrução também é uma palavra.
- 2 Por serem palavras os dados (arquitetura de Van Nemann). Isso é chamado de programa armazenado.

Observação: Uma palavra pode representar inteiros de -2³¹ a 2³¹ - 1, ou -2.147.483.648 a 2.147.483.647.
O campo contante, de -2¹⁵ a 2¹⁵ - 1, ou -32.768 a 32.767

Para representarmos o 0 tiramos 1 na parte positiva, por isso do "-1".

## Operações

Há 5 operações lógicas
- 1 **Shift lógica à esquerda** desloca todos os bits de uma palavra para a esquerda. O bit mais significativo é descartada.

Exemplo:

    0010 1000 <²- 1010 0000

Em Assembly MIPS, a instrução é 

    sll $t2, $t0, 2.
    (shift left logical)

- 2 **Shift lógico à direita** é análo ao sll. Em assembly:

```
srl $t2, $t0, 2.
(shift right logical)
```

- 3 **AND** é o **e** lógico bit a bit

Exemplo:

    0010 1010 |-> e
    1011 0110 |
    __________
    0010 0010

Em assembly MIPS:

    and $t0, $t1, $t2.

- 4 **OR** é o **ou** lógico bit a bit.

Exemplo:

    0010 1010 |-> ou
    1011 0110 |
    __________
    1011 1110

Em assembly MIPS:

    or $t0, $t1, $t2.

- 5 **NOT** é o **não** lógico bit a bit. Para manter o padrão de 3 operandos, o assembly MIPS possui a instrução:

    nor $t0, $t1, $t2

que faz o **not** do **or** entre $t1 e $t2:

Observações:

- 1 Como fazer o NOT apenas?
    - nor $t2, $t1, $t1
    - nor $t2, $t1, $zero

- 2 sll e srl são do formato R. Deste modo, usa-se o campo shamt. É desnecessário suas representações em tipo-I, já que deslocamentos maiores que 31 bits não fazem sentido.

- 3 Também há andi e ori.

Exemplo:

    andi $t1, 4t0, 5.

- 4 Fazer shift a esquerda de deslocamento i equivale a multiplicar por 2^i.