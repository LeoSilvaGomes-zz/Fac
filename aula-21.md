# Aula 21

## Aula à memória cache

Dados da memória principal são carregados para a memória cache sempre que processador fez uma requisição e o dado não está presente na cache.

A primeira questão é como saber se um dado, requisitado por seu endereço na memória principal, está ou não presente na cache. Para tanto, é necessário fazer um mapeamento dos endereços. O mapeamento mais comum é o mapeamento direto. Nesse esquema, cada endereço da memória princiapl é mapeado para um único local da cache. Uma cache que usa essa estratégia de mapeamento é dita diretamente mapeada. O mapeamento de um enderço. E numa cache que possui B blocos é dado por: E % B (% é o operador módulo).

Mapeamento:

      ____           ____
    0|____|         |____| 0 - 0     
    1|____|         |____| 1 - 1
    2|____|         |____| 2 - 2
    3|____|         |____| 3 - 3
                    |____| 4 - 0
                    |____| 5 - 1
                    |____| 6 - 2
                    |____| 7 - 3
                    |____| 8 - 0
                    |____| 9 - 1
                    |____|10 - 2
                    |____|11 - 3



Para o mapeamento direto, é necessário que as unidades de memória possuam tamaho potência de dois, digamos que:
- a memória principal possui tamanho 2^t
- a memória cache possui tamanho 2^n

O mapeamento direto conta com 3 elementos.

- O **índice** são os n bits menos significativos da memória principal e representa o endereço mapeado na cache.
- A **tag** são os bits mais significativos do endereço da memória principal e
- O **bit de validade** marca a validade do dado presente na cache.

     ____________________   
    | |         |        |
    | |         |        |
    | |         |        |
    | |         |        |
    |_|_________|________|
     |     |        |
     v     v        v
    bit   tag      dados
    de
    validade

No exemplo acima, a amemória principal possui 16=2⁴ bytes e a cache, 2²=4 bytes. Logo, a tag possui 2 bits e o tamanho real da cache é:

        4         *   (1   +   2   +   8)
    quantidade        bit     tag     tamanho
    de blocos     *   de   +       +  dos dados
    (linhas) da       val.
    cache

    =  44 bits = 5,5 bytes

Em função do princípio da localidade espacial, dados costumam ser carregados para a memória cache por **blocos**. Por exemplo, na arquitetura MIP, uma palavra possui 4 bits, portanto faria mais sentido carregar blocos de 4 bytes da memória principal para a cache.

Por isso, existe o mapeamento direto por blocos.

    __________________________________________
    |______|______|______|______|______|______| 0
    |______|______|______|______|______|______| 1
    |______|______|______|______|______|______| 2
    |______|______|______|______|______|______| 3
        |     |   \_____________ ____________/
        v     v                  v 
       bit   tag     bloco de dados de 4 bytes 
       de 
       validade

---
            __________
    000000 |__________| 0
    000001 |__________| 1
    000010 |__________| 2
    000011 |__________| 3       Memória
    000100 |__________| 4       principal
    000101 |__________| 5       de 64 bytes
    000110 |__________| 6
    000111 |__________| 7
    001000 |__________| 8
    001001 |__________| 9


|     |   |                 |     |     |     |    |          | 
| --- | - | -               | --- | --- | --- | -  |      --- | 
| 0,0 | 0 |                 | 0,0 | 0,1 | 0,2 |    | Matriz   | 
| 0,1 | 1 |  mapeamento ->  | 1,0 | 1,1 | 1,2 | -> | m linhas |
| 0,2 | 2 |                 | 2,1 | 2,1 | 2,2 |    | n coluna |
| 1,0 | 3 | 
| 1,1 | 4 |
| 1,2 | 5 |
| 2,0 | 6 |
| 2,1 | 7 |
| 2,2 | 8 |
