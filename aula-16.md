# Arquitetura do Processador
## Elementos do caminho de dados
O **processador**, também chamado de CPU(*Central Processing Unit*), é o dispositivo de um computador que executa as instruções definidas em uma arquitetura, de forma a realizar rigorosamente as operações por elas definidas.    

O processador possui dois componentes principais:   
* caminho de dados
* controle

O **caminho de dados** é o responsável por realizar as operações sovre os dados e conduzi-los da Entrada à Saída.   
Enquanto o **controle** decide quais elementos do caminho de dados, da memória e de periféricos são necessários para cada instrução.

Nosso objetivo agora é entender como projetar um caminho de dados para as três classes de instruções da arquitetura MIPS:
- 1. Instruções lógicas e aritméticas (em particular *add*, *sub*, *and*, *or* & *slt*)
- 2. Intruções de acesso à memória (em particular *lw* & *sw*)
- 3. Instruções de desvio condicional (em particular *beq*)

Esse subconjunto de instruções ilustra bem os princípios de projeto de um caminho de dados. Cada classe de instruções precisa de elementos básicos para ser executada.

___
## Etapas comum
Todas as instruções passam primeiro por uma etapa comum, que consiste em **buscar na memória** uma instrução, dados ou endereço.

Para executar essa primeira etapa, são necessários três elementos básicos:
- 1. Uma **memória de instruções** que, dado o endereço, retorna uma instrução
- 2. O **contador de programa** (*PC*), que contém o endereço da instrução que deve ser executada pelo processador e
- 3. O **somador**, que é utilizado para incrementar o *PC* e fazê-lo apontar para a próxima instrução a sex executada.

Na execução sequencial (sem desvios), esta etapa consiste em:
- 1. Buscar, na memória, a instrução cujo endereço encontra-se em *PC* e
- 2. incrementar o *PC* em 4 bytes para apontar para a próxima instrução.   

### Abstração do processo sequencial de instruções:

    |-------------------------------------------------------------------|
    |     ____                                           ______         |
    |    |    |     |-------------------------------->  |      \        |
    |--> |    |     |                                   |_      \       |
         | PC |     |        ___________________          \ SOMA \ -----|
    ---> |____|-----*------>| Endereço          |   4    _/      /
                            |             Inst. |---->  |       /
                            |                   |       |______/
                            | Memória de        |
                            | Instruções        |
                            |___________________|
                            
___
## Instruções lógicas e aritméticas
Relembrando, essas instruçes seguem o formato do *tipo **R***

     _______ _______ _______ _______ _______ _______
    |  op   |  rs   |  rt   |  rd   | shamt | funct |   
    |_______|_______|_______|_______|_______|_______|
     6 bits  5 bits  5 bits  5 bits  5 bits  6 bits

Para executar uma instrução deste tipo, é necessário:
- 1. Ler o conteúdo de dois registradores
- 2. Realizar uma operação aritmética com o conteúdo lido em **'1.'**
- 3. Escrever o resultado num registrador.

As etapas **'1.'** & **'3.'** usam-se de uma estrutura chamada **banco de registradores**.  
Um banco de registradores é uma estrutura que armazena os registradores da arquitetura e realiza operações de leitura e escrita dado o identificador de um registrador.

Para instruções do *tipo **R***, duas palavras devem ser lidas, e uma, escrita. Deste modo, o banco de registradores contém:
- Duas entradas que especificam os identificadores dos registradores a serem lidos 
- Uma entrada que especifica o identificador do registrador de escrita
- Uma entrada contendo a palavra a ser escrita.

O banco possui duas saídas, que são as palavras contidas nos registradores de leitura.

O banco de registradores conta ainda com sinal de controle de escrita, que será **1** se houver dados para serem escritos no registrador de escrita e **0** caso contrário.

### Abstração de um banco de registradores
         _______________________________
      5 |                               |
    --->| Reg. leitura                  |
      5 |                   Dado lido 1 |----->
    --->| Reg. leitura                  |
      5 |                               |
    --->| Reg. escrita                  |
     32 |                   Dado lido 2 |----->
    --->| Dado                          |
        |_______________________________|
                            |
                         RegWrite

**obs**: A escrita no banco de registradores é feita na transição do clock. Isso significa que a estrutura lê e escreve ao mesmo tempo: a leitura obtém o valor recém escrito na transição do clock anterior.   
Isso que viabiliza operações do tipo: *add $t0, $t0, $t1*

Outro elemento utilizado é a unidade lógica e aritmética, a *ULA*. A *ULA* é capaz de executar as operações lógicas e aritméticas próprias de cada instrução. Cada operação é identificada na *ULA* por um controle de 4 bits. A saída é gerada numa palavra de 32 bits, e um sinal que será 1 se a saída for zero.

             _ opULA (4 bits)
            |
         ___|___
     32 |       \        
    --->|_       \       
    bits  \  ULA  \ 
         _/       /
     32 |        /
    --->|_______/
    bits


___
## Instruções de acesso à memória
Seguem o formato do **tipo *I***:

     _______ _______ _______ _________________
    |  op   |  rs   |  rt   |     endereço    |   
    |_______|_______|_______|_________________|
     6 bits  5 bits  5 bits       16 bits

E operam da seguinte forma:
- 1. Calculam o endereço como sendo
    > reg-base + offset
- 2. Lê um dado do registrador e armazena na memória ou vice-versa

A etapa 1 faz uso de uma *ULA* e a etapa 2 faz uso do banco de regustradires, ambos já utilizados pelas instruções lógicas e aritméticas