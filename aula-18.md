# Aula 18

## Implementação Monociclo

O objetivo dessa implementação é fazer com que todas as instruções do assembly sejam executadas em um único ciclo de clock do processador. Essa implementação consiste na construção de um controlador que determine o caminho que cada pedaço de uma instrução deve tomar no caminho de dados e que determine os calores dos bits de controle de alguns elementos do caminho de dados. Para este controlador, duas unidades de controle são necessárias:

1. O controlador da ULA e
2. O controlador princiapl

## O controlador da ULA

A ULA é utilizado pelas instruções das 3 classes.

1. Para instruções lógicas e aritméticas, ela realiza soma (add), subtração (sub), *e* e *ou* lógicos (and e or) e comparação (slt).

2. Para instruções de acesso à memória, calcula o endereço de memória do dado pela soma do endereço base com o offset.

3. Para o beq, a ULA realiza uma subtração.

O controlador da ULA determina o valor do sinal da ULA baseado no campo funct das instruções do tipo R e um sinal de 2 bits, que chamaremos de OpULA. O campo OpULA vale.

- 00 para realizar add em acesso à memória
- 01 para **sub** em **beq**
- 10 para operações das instruções lógicas e aritméticas

                        _____________
        OpULA---------> | Controle  | Identificador da Operação
              2 bits    |    da     | ----------------->
                        |   ULA     | (4 bits)
        funct---------> |           |
              6 bits    |___________|

Os bits da OpULA são gerados pelo controlador principal com base na instrução a ser executada.

## O controlador principal

A primeira função do controlador principal é segmentar a instrução e distribuir suas partes aos elementos do caminho de dados adequados, da seguinte forma:

1. Lógicos e aritméticos (Tipo R) 

                 _______ _______ _______ _______ _______ _______
        Campo   |  op   |  rs   |  rt   |  rd   | shamt | funct |   
                |_______|_______|_______|_______|_______|_______|
        Posição  6 bits  5 bits  5 bits  5 bits  5 bits  6 bits
                31:26   25:21   20:16   15:11   10:6    5:0

2. Acesso à memória e desvio condicional (Tipo I) 

                 _______ _______ _______ ______________________
        Campo   |  op   |  rs   |  rt   |   endereço/offset   |   
                |_______|_______|_______|_____________________|
        Posição  6 bits  5 bits  5 bits         16 bits  
                 31:26   25:21   20:16          15:0 

Deste modo, as instruções são dividas entre os canais no barramento do caminho de dados, e seguem os padrões:

- O campo op está sempre nas posições 31:26.
- Para instruções de acesso à memória:
    - **rs** é o registrador contendo o endereço base e ocupa sempre as posições 25:21.
    - **rt** é o registrador de leitura para sw e de escrita para lw e sempre ocupa as posições 20:16.
- Os registradores de leitura estão sempre nas posições 25:21 e 20:16.
- O offset (para beq, lw e sw) sempre ocupa as posições 15:0.

Não obstante, o registrador de escrita pode:

- Ocupar as posições 20:16 para lw
- Ocupar as posições 15:11 para instruções do Tipo R.

Portanto, é necessário incluir um multiplexador para resolver esse dois casos. Colocando tudo isso junto, chegamos ao caminho de dados do slide 7. Um caminho de dados completo, com a unidade de controle, é ilustrado no Slide 8. Os sinais de controle, no Slide 9, e a operação p/ as instruções, nos 10-12.