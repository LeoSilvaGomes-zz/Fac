# Aula 7

### Recursividade

    n! = n*(n-1)*(n-2)...1

    int fat(int n){
        if(n<1) return 1;
        else return n*fat(n-1);
    }

---

    fat:addi $sp, $sp, -8
        sw $ra, 0($sp)
        sw $a0, 4($sp)
        slti $t0, $a0, 1
        beq $t0, $zero, L1
        addi $v0, $zero, 1
        addi $sp, $sp, 8
        jr $ra

    L1: addi $a0, $a0, -1
        jal fat

        lw $ra, 0($sp)
        lw $a0, 4($sp)
        addi $sp, $sp, 4
        mul $v0, $a0, $v0
        jr $ra

O segmento da pilha que contém valores salvos e as variáveis de um escopo do procedimento é chamado **frame de procedimento** ou **registro de ativação**. Para indicar o frame de procedimento, existe o registrador $fp (frame pointer):

        |$ra (n=3)|
        |$a0 (n=3)|
    $fp-|$ra (n=2)| - registro de ativação da função fat(2)
    $sp-|$a0 (n=2)|

A arquitetura MIPS possui a seguinte convenção de alocação de memória:

        | Pilha         |
    $sp-| ^|            | -> Heap
        | |v            | 
        | Dado dinâmico | 
    $gp-| Dado estático | -> variáveis globais e estáticos
    PC- | Texto         | -> instruções
        | Reservado     |

        Memória Principal

- **Texto** contém o **código de máquina** (compilado e montado). Program counter (PC) é um elemento de hardware que aponta para a instrução que está sendo executada.

- **Dados estáticos** é a porção reservada à variaveis globais e estáticas.

- **Dados dinâmicos** é a porção reservada para valores locais. Espaço usado pelo malloc do C, por exemplo, também conhecido por **heap**.

## Caracteres e Strings

Os computadores usam 8 bits (1 byte) para representar caracteres. O padrão de representação está na tabela ASCII (American Standard Code for Information Interchange). Dos 128 caracteres na tabela ASCII:

- 33 são sinais de controle (\n, \t, \0) e
- 95 são sinais gráficos (letras, sinais de pontuação, símbolos matemáticos).

As instruções de acesso à memória para caracteres são:

- lb $t0, 0($sp) lê um byte
- lbu $t0, 0($sp) lê um byte sem sinal
- sb $t0, 0($sp) salva um byte

**Strings** são cadeias de caracteres e podem ser representados de 3 formas:

- 1- A primeira posição da string indica o tamanho
- 2- Uma variavel aacompanhante possui o tamanho da string;
- 3- A última posição da string é ocupada por um caracter que marca seu final.

Exemplo: Vamos traduzir a função strcpy de C para assembly MIPS.

    void strcpy(char x[], char y[]){
        for(int i=0; (x[i] = y[i]) != '\0'; i++);
    }

    $a0 - endereço base x
    $a1 - endereço base y

---

    strcpy: add $t0, $a0, $zero
            add $t1, $a1, $zero
    loop:   lb $t2, 0($t1)
            sb $t2, 0($t0)
            addi $t0, $t0, 1
            addi $t1, $t1, 1
            bne $t2, $zero, loop

## Pseudo instruções de manipulação de dados

- **li reg, cons** carrega no registrador **reg** a contante **cons**.

- **la reg, label** carrega no registrador **reg** o endereço do **label**.

- **move reg1, reg2** copia o conteúdo do registrador reg1 para o reg2.

Há ainda as pseudo instruções condicionais:

- bge reg1, reg2, label
(branch if greater or equal)

- bgt reg1, reg2, label
(branch if greater than)

- ble reg1, reg2, label
(branch if less or equal)

- blt reg1, reg2, label
(branch if less or than)

