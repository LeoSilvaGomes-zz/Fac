# Aula 23

## Tratamento de escritas

Quando o processador executa uma instrução de escrita na mémoria, o processador faz a escrita na memória cache, sem alterar diretamente a memória principal, respeitando a hierarquia. Quando isso acontece, a memória cache fica com um valor diferente da memória principal, e dizemos que os dados estão inconsistentes, Há duas estratégias para resolver isso.

 1. Write-through: os dados escritos na cache são imediatamente progamados para a memória princiapl. Enquanto a propagação é feita, o processador fica ocioso. Como acesso à memória principal é lento, isso afeta o desempenho do processador para instruções de escrita. Para solucionar esse problema de tempo, usa-se um buffer de escrita: um buffer que armazena os dados que devem ser propagados à memória principal. Assim que os dados são escritos no buffer, o processador entende que o processo de escrita terminou e prossegue com o processamento.

 2. Write-back: os dados escritos na cache permanecem apenas na cache até que ocorra uma falha. Com isso, o sistema de memória aproveita a penalidade de falha para resolver a inconsistência.

 ## Medindo o desempenho da cache

 Quando se fala em desempenho, geralmente estamos preocupados com o **tempo de execução.**

 O tempo absoluto de CPU que um programa consome é:

    Tempo de    =    ciclos de      *       Período
    CPU              clock da cpu           de clock

Nesse tempo, há de se levar em conta o acesso à memória. Nesse contexto, quando há um **acerto**, o tempo de acesso à memória pode ser incluído no próprio tempo de clock da CPU. Todavia, falahas interrompem a execução do CPU a afetam o tempo de um programa:

    Tempo de    =    ciclos de   +   ciclos de      *       Período
    CPU              clock cpu       clock de               de clock
                                     stall de memória

Os stalls (bolhas) de memória podem ser de leitura e escrita. Os de leitura são:

    Ciclos de        Leituras            Taxa de                Penalidade
    stall de    =    Perguntas     *     falhas de      *       de falha
    leitura                              leitura

O tempo de stalls de escrita dependem da estratégia adotada. Para write-through, uma falha de dados faz com que o bloco que contém a palavra a ser sobrescrita seja antes buscada na memória principal. Além disso, uma escrita pode gerar stalls no procesador caso o buffer de escrita esteja cheio. Deste modo, 

    Ciclos de       leitura         Taxa             Penalidade
    stall de    =   ________    *   de falhas   *    de falhas
    leitura         Programa

        Stalls do
    +   buffer de escrita

Os stalls do buffer de escrita podem ser egnorados, pois os buffers são projetados para atender ao menos o dobro da escrita média em programas. Logo, os ciclos de stall de escrita coincidem com de leitura. O mesmo aontece com o esquea write-back, por definição.

Logo, 

    Ciclos de       Acesso à memória        Taxa de          Penalidade
    clock de    =   ________________    *   falha       *    de falha
    stall de            programa
    memória

                    Intruções       Falhas          Penalidade
                =   _________   *   ______      *   de falha
                    Programa        Instrução

Obs: Calcular os ciclos de clock consumido por um programa é algo muito dependente de programas e exigiria testes particulares para cada um. Por isso, processadores possuem uma medida médio chamada CPI-ciclos de clock por instrução. Com essa medida,

    ciclos de                           Quantidade
    clock de       =    CPI     *       de instruções
    um programa

**Exemplo**

Seja um cache com falhas de
- 2% para instruções
- 4% para dados

Se um processador possui um CPI de 2, sem considerar stalls de memória, e a penalidade de falha é 100 ciclos, determine o quão mais rápida seria o processador se a cache nunca falhasse. COnsidere que em médio, 36% das instruções são de acesso à memória.

## Cache multinível

O objetivo é reduzir a penalidade de falha e o tempo de acerto. Num sistema de cahce multinível:

- a priméria possui blocos de tamanho menor, objetivando-se diminuir o **tempo de acerto**.
- a(s) secundário(s) possuem blocos de tamanho maior, objetivando-se minimizar a taxa de falhas em detrimento do tempo de acerto.