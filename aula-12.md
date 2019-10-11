# Algoritmos de Divisão
Data: 11/10
___
### Exemplo:
> 1001010(bin) / 1000(bin)
>
> Dividendo       divisor

    ----------------------------
    |                          |  Divisor
    ----------------------------
                64 bits

    ----------------------------
    |                          |  Resto
    ----------------------------
                64 bits


## Algoritmo:
Para efetuar o algoritmo de divisão, basta seguir os seguintes passos:

- 1. Salvar o Dividendo no e defina contador = 1.
- 2. Subtraia o Divisor do Resto e Salve o resultado no Resto
- 3. Faça um deslocamento à esquerda de 1 bit no quociente.
    - 1. Se resto >= 0:
        - defina o bit menos significativo do Quociente como 1.
    - 2. Se resto < 0:
        - restaure o valor original do Resto
- 4. Faça um deslocamento à direita de 1 bit no Divisor
- 5. Se contador <= 32:
    - Contador = Contador + 1
    - Voltar ao passo 2

### Exemplo:
7(dec) / 2(dec) = 0111(bin) / 0010(bin)

IT | Passo | Quociente | Divisor | Resto 
---| ---   | ---       | ---     | --- 
0  | Valor Inicial | 0000 | 0010 0000 | 0000 0111
---|-----------------|----------------|----------------|----------------
1 | 2 | 0000 | 0010 0000 | 1110 0111
1 | 3 | 0000 | 0010 0000 | 0000 0111
1 | 4 | 0000 | 0001 0000 | 0000 0111
---|-----------------|----------------|----------------|----------------
2 | 2 | 0000 | 0001 0000 | 1111 0111
2 | 3 | 0000 | 0001 0000 | 0000 0111
2 | 4 | 0000 | 0000 1000 | 0000 0111
---|-----------------|----------------|----------------|----------------
3 | 2 | 0000 | 0000 1000 | 1111 1111
3 | 3 | 0000 | 0000 1000 | 0000 0111 
3 | 4 | 0000 | 0000 0100 | 0000 0111
---|-----------------|----------------|----------------|----------------
4 | 2 | 0000 | 0000 0100 | 0000 0011 
4 | 3 | 0001 | 0000 0100 | 0000 0011
4 | 4 | 0001 | 0000 0010 | 0000 0011
---|-----------------|----------------|----------------|----------------
5 | 2 | 0001 | 0000 0010 | 0000 0001
5 | 3 | 0011 | 0000 0010 | 0000 0001
5 | 4 | 0011 | 0000 0001 | 0000 0001

Note que metde dos bits do divisor valem zero durante toda a execução do algoritmo. Note ainda que o primeiro passo nunca produzirá 1 no quociente, portanto, é desnecessário, o que pode nos poupar 1 iteração no algoritmo.

Com isto, podemos **refinar o hardware** para utilizar

    --------------
    |            | Divisor
    --------------
        32 bits

    --------------
    |            | Quociente
    --------------
        32 bits

    ----------------------------
    |                          |  Resto
    ----------------------------
                64 bits


## Algoritmo Refinado
Para realizar o algoritmo de maneira refinada, deve-se seguir os passos:
- 1. Salve o Dividendo no Resto.   Zere o Quociente.   Defina Contador = 1.
- 2. Faça um deslocamento à esquerda de 1 bit no Resto.
- 3. Subtraia o divisor dos 32 bits mais significativos do Resto e salve o resultado nos bits mais significativos do Resto.
- 4. Desloque o Quociente 1 bit à esquerda
    - 1. Se Resto >= 0 
        - Quociente[0] =1
    - 2. Se Resto < 0
        - Restaure o Resto
- 5. Se Contador < 32
    - Contador++
    - Volte ao passo 2
### Exemplo:
7(dec) / 2(dec) = 0111(bin) / 0010(bin)

IT | Passo | Quociente | Divisor | Resto 
---| ---   | ---       | ---     | --- 
0  | Valor Inicial | 0000 | 0010 | 0000 0111
---|-----------------|----------------|----------------|----------------
1 | 2 | 0000 | 0010 | 0000 1110
1 | 3 | 0000 | 0010 | 1110 1110 
1 | 4 | 0000 | 0010 | 0000 0000 
---|-----------------|----------------|----------------|----------------
2 | 2 | 0000 | 0010 | 0001 1100 
2 | 3 | 0000 | 0010 | 1111 1100 
2 | 4 | 0000 | 0010 | 0001 1100 
---|-----------------|----------------|----------------|----------------
3 | 2 | 0000 | 0010 | 0011 1000 
3 | 3 | 0000 | 0010 | 0001 1000  
3 | 4 | 0001 | 0010 | 0001 1000
---|-----------------|----------------|----------------|----------------
4 | 2 | 0001 | 0010 | 0011 0000  
4 | 3 | 0001 | 0000 | 0001 0000 
4 | 4 | 0011 | 0010 | 0001 0000 
