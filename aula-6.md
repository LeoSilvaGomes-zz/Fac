# Aula 6

### Continuação da aula passada


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

---

            add $t0, $s0, $zero     # $t0 = end base de A
            lw $s1, 0($t0)          # $s1 = A[0]
            sll $t1, $s2, 2         # $t1 = n*4
    loop:   addi $t0, $t0, 4
            slt $t2, $t0, $t1       # $t0 < $t1 (i<n)?
            beq $t2, $zero, exit
            lw $t3, 0($t0)          # $t3=A[i]?
            slt $t4, $s1, $t3       # meior < A[i]?
            beq $t4, $zero, loop
            add $s1, $t3, $zero     # maior = A[i]
            j loop
    exit:

## Funções

Quando uma função é chamada, toda a execução é **desviada** para o procedimento e um novo **escopo** é criado. Quando a função termina, o programa segue o fluxo de execução a partir da chamada da função. É essa dinâmica de desvio e de criação de escopo que devem nos atentar no assmbly.

Para nos ajudar com isso, contamos com os seguintes registradores;

- $a0 - $a3: são usados para parâmetros de funções;
- $v0 - $v3: são usados para retorno da função;
- $ra: guarda o endereço de origem da chamada à função.

A instrução jump-and-link:

    jal endereço
        (label)

desvia o fluxo de execução ao endereço dado e salva o endereço da próxima instruçãono registrador $ra. Há ainda a instrução **jump to register:**

    jr $ra

que desvia o fluxo de execução para o endereço armazenado em $va (é como se fosse o **return** que usamos em linguagem C);

O procedimento que faz a chamada à função é denominado **caller** e a função chamada, **callee**. Para fazer a chamada a uma função em assembly:

- 1 - O caller coloca os valores dos parâmetros nos registradores $a0 - $a3. Se necessário, faz uso da memória principal.
- 2 - O caller utiliza a instrução **jal** para realizar  a chamada ao callee
- 3 - O callee realiza o procedimento, salva os eventuais resultados nos registradores $v0 - $v1, e retorna o caller usando a instrução **jr**

Nessas chamadas à função, o armazenamento de dados na memória é bem comum. Por isso, existe uma região da memória principal  dedicada a esse uso chamada **pilha**. Pilha é uma estrutura sequencial do tipo "o último a entrar é o primeiro a sair". O registrados $sp (stack pointer) contém o último endereço alocado da pilha. Essa pilha "cresce" para baixo, isto é, de endereços maiores para menores:

          <- $sp
    |   |           |$s0|  
    |   |   salva   |$s1| <- $sp  
    |   | --------> |   |  
    |   | $s0 e $s1 |   |  
    |   |           |   |  

    int exemplo (int g, int h, int i, int j){
        int f;
        f = (g + h) - (i + j);
        return f;
    }

    f - $v0
    g - $a0
    h - $a1
    i - $a2
    j - $a3

---

Exemplo:

    addi $sp, $sp, -8   |
    sw $s1, 0($sp)      | -> armazena na pilha
    sw $s0, 4($sp)      |

    add $s0, $a0, $a1
    add $s1, $a2, $a3
    sub $v0, $s0, $s1

    lw $s0, 4($sp)      |
    lw $s1, 0($sp)      | -> restaura valores da pilha
    addi $sp, $sp, 8    |

    jr $ra #return
