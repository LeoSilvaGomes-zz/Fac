# Aula 14

## O padrão IEEE754
O padrão IEEE754 especifica as seguintes representações:

| Precisão simples |   | Precisão Dupla |   | Representação |
|       ---        |---|       ---      |---|      ---      |
Expoente | Fração | Expoente | Fração |   |
0 | 0 | 0 | 0 | **Zero** | 
0 | !=0 | 0 | !=0 | +/- **Número desnormalizado**
1-254 |  * | 1-2046 | * | +/- **Ponto flutuante**
255 | 0 | 2047 | 0 +/- | **Infinito**
255 | !=0 | 2047 | !=0| **NaN** (Not a Number) 

***Obs***:
> Um número em notação científica está normalizado se estiver na forma
> 
> *(-1)⁵ * 1,xxx..x * 2^yyy...*
>

Na representação, o expoente aparece antes da fração para facilitar a ordenação: **números com expoentes maiores são maiores do que aqueles com expoentes menores**. 

Por isso, representar expoentes em complemento a dois compromete essa estratégia. Por exemplo:

                    ----------------------
    1,0 * 2^(-1) =  |0|11111111|000...000|   (I)
                    ----------------------
                    |S|  exp.  |  fração

Enquanto que:

                 ----------------------
    1,0 * 2^1 =  |0|00000001|000...000|   (II)
                 ----------------------
                 |S|  exp.  |  fração


Note que ***1,0 * 2^1*** **>** ***1,0 * 2^(-1)*** , porém, **(I) > (II)**.

Por isso, o que se faz é representar o expoente mais negativo por 000...(bin) e o maior por 111...111(bin). Essa representação é chamada de **representação deslocada** e é feita somando-se 127 ao expoente na precisão simples e 1023 na dupla.

Por exemplo,*-1* é representado por 
> -1 + 127 = 126

Esse deslocamento é chamado de **Bias**. Com isso, o número em ponto flutuante é representado da seguinte forma:

    (-1)⁵ * (1+Fração) + 2^(E-Bias)


Com essa representação, o menor número positivo que podemos representar é 
>| 0 | 00000001 | 000...000 |
>----------------------
>S(1bit)    
>Expoente(8bits)    
>Fração(23bits)
>
> Que corresponde a:
>
> *N*(min) = 1,000...0(bin) * 2^(1-127)   
>  = 1,000...0 * 2^(-126)    
> ~= 1,2 * 10^-38

Enquanto que o maior é 
>| 0 | 11111110 | 111...111 |
>----------------------
>S(1bit)    
>Expoente(8bits)    
>Fração(23bits)
>
> *N*(max) = 1,111...1(bin) * 2^(127) = (2-2^23) * 2^127   
> ~= 2^128  
> ~= 3,4*10^38  

Analogamente, podemos calcular os extremos para **precisão dupla**, levando-nos à seguinte capacidade de representação.

| Precisão | *E*min | *E*max | *N*min | *N*max |
|--- | --- | --- | --- | --- |
| Simples | -126 | 127 | 2^(-126) ~= 1,2*10^(-38) | 2^128 ~= 3,4*10^38 
| Dupla | -1022 | 1023 | 2^(-1022) ~= 2,2*10^(-308) | 2^1024 ~= 1,8*10^308 

___
# Operações aritméticas em ponto flutuante
## Adição
Para fazer adição, seguimos o seguinte algoritmo:
- 1. Compare os expoentes dos números. Desloque a fração do menor expoente para a direita e some 1 ao expoente que os expoentes coincidam;
- 2. Some as frações;
- 3. Normalize a soma;
    - Desloque a fração à direita e incremente o expoente;
        - ou
    - Desloque a fração à esquerda e decremente o expoente;
- 4. Se houve **undeflow** ou **overflow**
        - lance uma exceção
- 4. Senão
        - Arredonde a fração para o número de bits apropriado

### Exemplo
0,5(dec) - 0,4375(dec) = 1,0(bin)*2^(-1) - 1,110*2^(-2)

1. 1,110 * 2^(-2) -> 0,1110 * 2^(-1)
2. 1,1110
3. Nada a fazer
4. Nada a Fazer     
**Resultado** = 1,1110 * 2^(-1)

### Exemplo
1,1011 * 2^-3 + 1,1100 * 2^(-4)

1. 0,11100 * 2^(-3)
2. 10,1001 * 2^(-3)
3. 1,01001 * 2^(-2)
4. Nada a fazer

___
## Subtração 
O algoritmo da subtração segue os mesmos passos do algoritmo da **adição**, com a diferença de que será realizada a operação de subtração ao invés da soma.

___
## Multiplicação
O algoritmo para multiplicação é o seguinte:

- 1. Some os expoentes e, em seguida, some um **Bias** ao resultado;
    >   (*E*1 - Bias) + (*E*2 - Bias) + Bias    
    > = (*E*1 - *E*2) - Bias 
- 2. Multiplique as frações;
- 3. Normalize o produto;
- 4. Se houve underflow ou overflow
        - lance uma exceção;
- 4. senão
        - faça o **arredondamento**;
- 5. Ajuste o sinal do produto com base no sinal dos operandos;

___
## Arredondamento

O **IEEE754** admite 4 padrões de arredondamento:

1. **Sempre para cima**:    
    2,11 = 2,2  
    2,15 = 2,2  
    2,19 = 2,2

2. **Sempre para baixo**:   
    2,11 = 2,1
    2,15 = 2,1
    2,19 = 2,1

3. **Truncamento**:     
    Simplesmente despreza os bits menos significativos

4. **Ao mais próximo**:     
    2,11 = 2,1  
    2,19 = 2,2

    Neste caso, e *2,15*? A solução é escolher a solução mais coerente do ponto de vista estatístico: ao dígito **par** mais próximo    
        
    2,15 = 2,2
    2,25 = 2,2 