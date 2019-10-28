# Aula 17

## Instruções de acesso à memória

São do tipo I: lw $t0, 0($s0)

|op |rs |rb endereço

| op |   | rs |   | rb | | endereço | |
|       ---        |---|       ---      |---|      ---      | ---|---|---
|6 bits | | 5 bits | | 5 bits | | 16 bits | |

Operam da seguinte forma:

a. calculam um endereço de memória como sendo

    reg_base + offset

b. lê um dado do registrador e armazena na memória **ou** lê um dado da memória e armazena num registrador.

A etapa **a** faz uso de um **somador** e a **b**, de um **banco de registradores**, ambos os mesmo utilizados pelas instruções lõgicas e aritméticas.

Além desses elementos, é necessário uma **unidade de extensão de sinal**, que estende o sinal do endereço de 16 bits calculado na etapa **a** para um de 32 bits a ser usado peo somador.


    16   | Extensão | 32
    ---> |    de    | --->
    bits |   sinal  | bits

Por último, é necessário o uso de uma **unidade de memória** de dados que escreve ou leia dados em um dado endereço.

Por ter operações de leitura e escrita, a memória de dados possui um sinal de controle de leitura e escrita, separados, embora apenas um seja ativado por ciclo de clock.


                          | memwrite
                __________|_________
    Endereço    |              Dado |
    ----------> |              lido |----->
                |                   |
                |                   |
                |                   |
    Dado        |      Memória de   |
    ----------> |          dados    |
    de Escrita  |                   |
                -----------|----------
                           | memread


## Instruções de desvio

Também seguem o formato do tipo I. Estamos interessados na instrução

    beq reg1, reg2, label

Essa instrução possui dois operandos registradores e um endereço que indica qual instrução será alvo no desvio. Esse endereço na verdade é um **offset**, e o **endereço de destino do desvio** é calculado usando-se o PC e offset neste sentido:

- o endereço base para cálculo do endereço de destino do desvio é pc +4
- offset representa o número de palavras, e não de bytes.

Deste modo,
> Endereço de = (Pc+4) + offset * 4. destino do desvio

Para decidir qual seá a próxima instrução, a comparação reg1=reg2 é feita. Se for verdadeira, a próxima instrução será a do endereço de destino do desvio; nesse caso, dizemos que o desvio é  **tomado** segue a sequência e dizemos que o desvio é **não tomado**.

Logo, o caminho de dados deve ser capaz de

1. Calcular o endereço de destino do desvio e
2. comparar o conteúdo dos registradores.

Para **2**, utiliza-se a ULA para fazer reg1-reg2

Se a flag **zero** da ULA for 1, então reg1=reg2. Caso contrário, reg1 != reg2. Os elementos do caminho de dados para desvios é ilustrado no Slide 1.

## Um caminho de dados para as 3 classes

O objetivo é executar qualquer instrução em um único ciclo de clock, portanto qualquer elemento do caminho de dados que é utilizado por mais de uma classe de instruções não precisa ser duplicado.

As operações das instruções que vimos diferentes.

- As intruções lógicas e aritméticas utilizam a ULA com entradas vindas de registradores, enquanto de acesso à memória e desvio condicional recebem um dos operandos do extensor de sinal.

- O valor armazenado num registrador pode vir da ULA (tipo R), ou da memória (lw).

Para resolver essas diferenças, um multiplexador é colocado na entrada da ULA e outro na porta do banco de registradores.

A primeira versão de um caminho de dados pode ser visto no Slide 2.

Para incluir a etapa comum(busca de instruções), faz-se necessário o uso de um multiplexador para selecionar se o endereço da próxima instruções é PC+4 ou o endereço de destino do desvio. Um caminho de dados assim pode ser visto no Slide 3.