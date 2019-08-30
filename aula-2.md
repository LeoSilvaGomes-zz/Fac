# Aula 2

## Arquiteturas de RISC e CISC

### Intruções
    RISC                        |   CISC
    - Consomem um único de      |   - podem consumir mais de um ciclo
    processamento               |   de processamento
    - Baixo número              |   - Alto número
    - Simples padronizadas     |   - mais complexos

### Projetos 
    RISC                        |   CISC
    Centrado no software: o     |   Centrado em hardware: o
    compilador é responsaável   |   conjunto de intruções e
    por compor instruções pa-   |   capaz de executar operações
    ra comandos deling de alto  |   de alto nível
    nivel                       |

### Memória RAM
    RISC                        |   CISC
    uso menos eficiente         |   uso mais eficiente

### Execução
    RISC                        |   CISC
    Uma camada de instruções    |   Suporta microprograma
    (direto no hardware)        |

## Tipos de computadores

- Computadores pessoais
- Computadores para jogos (videogames)
    - GPU
- Servidos
- Mainframes
- Microcontroladores
- Computadores descartáveis
- Computador portátil

---

          Linguagem
    ----------------------
    Conjunto de Instruções
    ----------------------
          Instrução

## A arquitetura MIPS

MIPS, um acrônimo para microprocessador without pipelines stags, é um conjunto de instruções criado na década de 1980. O MIPS é uma arquitetura CISC baseada em registradores.

    Registrador é uma memoria que fica dentro da CPU e que é capaz de armazenar n bits.

Nós usaremos a versão de 32 bits do MIPS. Isso significa que cada registrador possui capacidade de armazenar 32 bits. Por isso, um dado de 32 bits ocorre com frequência nessa arquitetura e recebe o nome de palavra.

A CPU utiliza apenas registradores para realizar as operações. Não obstante, há instruções próprias de acesso à mémoria.

O padrão básico de uma instrução do assembly MIPS trabalha com um mnemônico e três operandos: um de destino e dois de origem.

Exemplo:
    
       add           a,       b,c
        |            |         |
        v            v         v
    mnemônico     destino   origem 
    caracteriza           | 
    a operação            v
                      operandos

## Instruções elementares

Temos 3 conjuntos de instruções na ISA do MIPS:

- Lógicas e aritméticos
- Desvio
- Acesso a memória

As 3 operações aritméticas são as mais elementos em MIPS:

- add a,b,c #a=b+c
- sub a,b,c #a=b-c
- mut a,b,c #a=b*c
