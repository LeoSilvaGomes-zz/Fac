# Hierarquia da memória do computador

                 ---------------
                 | Processador |
                 ---------------
        ----------                    |
        |Nível 1 |                    |   Distância
        |        |                    |
        |Nível 2 |                    |
        |        |                    |   Tempo de acesso
        |Nível 3 |                    |
        |        |                    |
        | ...    |                    |   Tamanho da memória
        |        |                    |
        |Nível n |                    |
        ----------                    |
                                     \/




Os dados são mantidos no nível inferior, e cópias são feitas apenas entre níveis adjacentes.    

O processador acessa os dados diretamente do nível mais superior. Se o dado requisitado pelo processador estiver presente, dizemos que houve um **acerto**.     
Caso contrário, houve uma **falha**.    

A **taxa de acerto** é a fração dos acessos do processador à memória que resultaram em acertos.     
A **taxa de falha** é a fração de acessos cujos dados não foram encontrador no nível superior.

O **tempo de acerto** é o tempo necessário para a hierarquia de memória determinar se um dado requisitado está no nível superior.   
A **penalidade de falha** é o tempo de carregamento do dado procurado de um dos níveis inferiores para o nível superior.

# Tipos de memória
Podemos destacar 4 tecnologias de memória: ***SRAM***, ***DRAM***, ***memória Flash*** e ***discos magnéticos***

## Memória *SRAM*
**SRAM** (Static Random Access Memory) é a memória de mais rápido acesso e é a tecnologia usualmente utilizada nos níveis superiores da hierarquia.     
0Memórias deste tipo podem armazenar dados enquanto houver energia passando por ela (mesmo em baixa quantidade).

## Memória *DRAM*
**DRAM** (Dinamic Random Access Memory) é uma memória de acesso mais lento que a *SRAM*. 

As memórias DRAM usam um único transistor por bit de armazenamento, por isso, são muito mais densas que as *SRAM*'s e, consequentemente, bem mais baratas.     

Os dados numa ***DRAM*** são armazenados como uma carga elétrica em um capacitor, por isso, não permanecem indefinidamente e necessitam de atualizações periódicas. Por isso é chamada "dinâmica".   

O acesso aos dados na memória ***DRAM*** é feito por linhas, e há um buffer que armazena linhas e funciona como os *SRAM*.      

Com isso, o acesso a bits aleatórios pode ser feito eficientemente.     

Além disso, as ***DRAM***'s contam com um clock interno. A vantagem disso é que o tempo de sincronização com o processador é desnecessário.     

Essa tecnologia chama-se Synchronous DRAM, ou ***SDRAM***.

A transferência de linhas entre a memória DRAM e seu buffer é feita dentro de um ciclo de clock da memória.

Versões mais atuais da ***SDRAM*** transferem dados tanto no início quanto no final do clock. Essa tecnologia chama-se Double Data Rate (**DDRSDRAM**).  
Por exemplo, uma memória DDR$-3200 SDRAM usa a versão 4 da tecnologia DDR e é capaz de fazer 3200 milhões de transferências por segundo, ou seja, tem um clock de 1600MHz.


## Memória FLash
É um tipo de memória EEPROM (Electrically Erasable Programmable Read Only Memory)).     

Semelhantes à tecnologia DRAM, a Memória Flash permite que múltiplos bits sejam lidos e escritos numa púnica operação.   

A diferença é que escritas desgastam os bits em uma memória flash. Por isso, há um controlador que "esplaha" as operações de escrita, remapando endereços 
para blocos menos utilizados.     

É a tecnologia utilizada em cartões microSD e HD's SSD.


## Discos magnéticos
São unidades de armazenamento que consistem num conkjunto de pratos magnéticos que efetuam de 5400 a 15000 rotações em torno de um eixo. Para fazer leitura e escrita, sobre cada prato há um braço que contém uma bobina eletromagnética chamada cabeça de leitura e escrita

Cada disco é dividido em circulos circuncêntricos chamados **trilhas**. Costuma-se ter algo na casa de 10000 trilhas por disco. Cada trilha é dividida em milhares de **setores**, que possuem tamanho entre 512 e 4096 bytes.

Os braços movem-se de forma síncrona entre os discos. O conjunto de trilhas que encontram-se sob a cabeça de leitura num instante de tempo é chamado **cilindro**.

A busca de dados num disco magnético consome 3 etapas:
- 1. **Tempo de busca**: mover a cabeça de leitura sobre a trilha apropriada. 
- 2. **Latência de rotação**: dada a trilha, busca o setor.
- 3. **Tempo de transferência**: tempo para transferir um bloco de bits.

O tempo médio de busca costuma ser algo entre 3ms e 13ms, a latência rotaciona média num disco de 5400 RPM algo em torno de 6ms e a taxa de transferência 100 e 200MB's.

Essa tecnologia pode sofrer **fragmentação de dados**.

A memória flash costuma ser 1000 vezes mais rápida que discos magnéticos e a DRAM; 10000 vezes naus rápida. Todavia, os discos magnéticos são entre 10 e 100 vezes mais baratos que outras tecnologias.


# Memória CACHE
É o nível de mempirua mais superior, que fornece dados direto ao processador.       

Quando o processador requisita um dado, o sistema de memória primeira verifica se o dado está ou não presente na CACHE.     
O processador requisita um dado usando seu denreço, logo a cache deve, de alguma forma, armazenar dados e mapeá-los para o endereço do nível inferior.          
Portanto, é necessário fazer um mapeamento de endereços do npivel inferior para a cache.