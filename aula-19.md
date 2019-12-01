# Aula 19

## Implementação com pipeline

**Pipeline** é uma técninca de implementação em que instuções são divididas em etapas e várias etapas são executadas ao mesmo tempo por ciclo de clock.     
As instruções do assembly MIPS podem ser divididas em 5 etapas.     
- 1. Buscar a instrução na memória de instruções
- 2. Ler dados de registradores
- 3. Executar uma operação aritmética
- 4. Acessar a memória de dados
- 5. Escrever i resyktadi byn registrador

Usando tais etapas, é possível aplicar a técnica de pipeline.

### Exemplo
Consideremos as instruções do subconjunto que estamos tratando, e os os seguintes tempos de operação para as principais unidades funcionais:
- ***200ps*** para acesso à memória
- ***100ps*** para operações com a **ULA**
- ***50ps*** para leitura e escrita de registradores

O tempo de execução de cada instrução, com base nas etapas descritas acima, é dado abaixo:

Instrução | Etapa 1 | Etapa 2 | Etapa 3 | Etapa 4 | Etapa 5 | **Total** | 
--------- |-------- | ------- | ------- | ------- | ------- | ----- |
lw        | 200     | 50      | 100     | 200     | 50      | 600   |
sw        | 200     | 50      | 100     | 200     | ------- | 550   |
tipo R    | 200     | 50      | 100     | ------- | 50      | 400   |
beq       | 200     | 50      | 100     | ------- | ------- | 350   |

Numa implementação de monociclo, o tempo de clock deveria ser ***600ps***, que é o tempo da instrução mais lenta. No pipeline, seria 200ps, que é o tempo da etapa mais lenta.

> 3 execuções de ***lw*** em monociclo: **1800ps**.     
> 3 execuções de ***lw*** em pipeline: 1000 + 200 + 200 = **1400ps**.       

Portanto, para 3 execuções de lw, o pipeline é 1,3 vezes mais rápido que o monociclo.       

1000.003 execuções       
    |-> **Monociclo**: ***600.001.800ps***      
    |-> **Pipeline**: 1000 + 200*1.000.002 = ***200.001.400ps***    


Nesse cenário, pipeline é 2,99 vezes mais rápido.       

Desse modo, podemos concluir que:
- O pipeline melhora o desempenho aumentando a **vazão** ao invés de otimizar o tempo individiaul de uma instrução. Isso é interessante, pois um processaddor executa milhões de milhões de instruções por segundo.

___
## Hazards de pipeline
Hazards, em pipeline, acontecem quando uma etapa de uma instrução não pode ser executada por algum impedimento.     
Há 3 tipos de hazards de pipeline:
- 1. Estruturais
- 2. Hazards de dados
- 3. Hazards de controle

### Hazards estruturais
Hazards estruturais acontecem quando, por algum motivo estrutural da arquitetura, alguma etapa da instrução não pudesse ser executada.      

A nossa implementação do caminho de dados já resolve qualquer hazard estrutural. Mas teríamos um hazard estrutural na seguinte circunstância:       
Suponha que tivéssemos apenas uma unidade de memória do nosso caminho de dados (para acessar tanto instruções quanto dados).    
Suponha que no exemplo do **Slide14** tivéssemos uma quarta chamada a ***lw***. 
Essa última não poderia ser executada no quarto ciclo de clock, pois seria necessário buscá-la na memória, que já estaria sendo ocupada pela quarta etapa da primeira ***lw***.


### Hazards de dados
Acontecem quando os dados para executar uma instrução numa etapa não estão disponíveis. Por exemplo:        

    add $s0, $t0, $t1
    add $s1, $s0, $t2

A segunda adição utiliza o resultado da primeira, que estará disponível apenas depois da quinta etapa da *primeira add*.      
para um caso desse, uma possível solução seria passar o resultado da adição diretamente para a *segunda add*, antes mesmo de terminar a primeira.   
Para tanto, cria-se um dispositivo que encaminhe o resultado da ULA diretamente como entrada da próxima instrução.  
Essa técnica é chamada de **forwading** ou **bypassing**.

Neste caso, teríamos

<img src="../images/desenhofac.png">

O forwading só funciona se a etapa de destino estiver a frente no tempo que o estágio de origem.    
Por isso, nem sempre é possível resolver todos os hazards de dados com forwading.    
Por exemplo:    

    add $s0, 20($t1)
    add $t2, $s0, $s1

O dado na lw estará disponível apenas na quarta etapa:

<img src="../images/desenhofac2.png">