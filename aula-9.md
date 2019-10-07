# Aula 9

# Aritmética Computacional
Vamos trabalhar com 3 bases numéricas: 2 (binário), 10 (decimal) &, menos frequentemente, 16 (hexadecimal).

Os binários, em MIPS 32, possui 32 bits de largura. Chamaremos de **bit mais significativo** o bit mais à esquerda e **bit menos significativo** o bit mais à direita.
    
     31 32 31 30 29 28 27  ...   6  5  4  3  2  1  0
    ------------------------------------------------- 
    |  |  |  |  |  |  |  | ... |  |  |  |  |  |  |  |
    -------------------------------------------------

> Dentro da arquitetura MIPS 32, o **bit mais significativo** é o bit 31 & o **bit menos significativo** é o bit 0.

Com esse modelo, podemos **representar** 2^32 números distintos. Essa limitação é a capacidade de representação de um computador, e neste limite ele é capaz de efetuar operações aritméticas.

Quando, numa dessas operações, acontece de o resultado não caber nesse espaço de 32 bits, diz-se que houve um **overflow**. Cabe ao programa e, em última instância, ao sistema operacional, decidir o que fazer quando isso ocorre.

___
## Observações
- 1 O nome "complemento a dois" deve-se ao fato de que um número somado ao seu negativonuma representação de *n* bits dá 2^n  
        - 
- 2 A instrução **lb** (load byte), carrega um byte (8 bits) na representação de 32 bits e replica o bit mais significativo, pois considera que o byte possui sinal.
        -
- 3 Por outro lado, o **lbu** (load byte unsigned) sempre preenche os bits mais significativos com zeros, já que considera que o byte não possui sinal
        -
- 4 Cuidado também com as instruções **slt** e **sltu**.
        -
     > Exemplo: Suponha qye $s0 contenha 0 em todos os bits e $s1 contenha 1 em todos os bits
     >      
     >  31   32  31  ...   2   1   0 
     >   
     > | 1 | 1 | 1 | ... | 1 | 1 | 1 |
     >   | 
     >   |- 
     >   | Representações do número   
     >   | Sem sinal: 2^32 - 1  
     >   | Com sinal: -1    
     >
     > Logo e fizermos:
     >
     >  slt $t0, $s1, $s0 então $t0=1 
     >
     >  sltu $t0, $s1, $s0 então $t0=0

- 6 slt reg, reg1, reg2
        
         reg = 0, se reg1 >= reg2
         reg = 1, se reg1 <  reg2

- 5 Como negar um número emassembly MIPS? Queremos negar o $s0:
        
        ### Maneira correta:
                nor  $s1, $s0, $zero
                addi $s1, s1, 1
        ### Maneira errada
                multi $s1, $s1, -1

___
## Tabela de detecção overflow na adição

Overflow na soma acontece nas seguintes operações:

Operação | A | B | Resultado
- | - |  - | -
**A + B** | >=0 | >=0 | <0
**A + B** | <0  | <0  |  >=0
**A - B** | >=0 | <0  |  <0
**A - B** | <0  | >=0 |  >=0

>Soma de números com sinais opostos e subtração de números com mesmo sinal **nunca** acarretam em overflow 


Na arquitetura MIPS
- **add**, **addi** e **sub** causam exceções no overflow.
- **addu**, **addiu** e **subu** não causam exceções no overflow.

___
## Exceções
Também chamadas de interrupções, as exceções são eventos não planejados que interrompem a execução do programa. 

Deste modo, exceções lançadas por instruções são indesejadas. Por isso, em caso desuspeita de exceções, deve-se explorar as instruções que não causam interrupções para detecção de overflow.

___
## Detectando overflow na adição
1. Considere $t1 e $t2 números **com sinal**.
        
        addu $t0, $t1, $t2          // t0 = t1 + t2
        xor $t3, $t1, $t2           // MSB
                                    // 1 caso $t1 e $t2 tiverem sinais opostos
                                    // 0 caso contrário
        
        slt $t4, $t3, $zero         // t3 é negativo?
        bne $t4 $zero, sem_overflow // se t4!=0 não há overflow
        xor $t3 $t0,$t1         
        slt $t4, $t3, $zero
        bne $t4, $zero, overflow



