# Aula 1

### Informações gerais

<p>
Atendimento: <br />
segunda - 16h às 17h <br />
Site: <br />
johnlenongardenghi.com.br/courses/fac-19-2 <br />
Email:<br />
john.gardenghi@unb.br
</p>

### Ementa

    1 - Introdução à arquitetura de computadores
    2 - Introdução à programação em linguagem de montagem
    3 - Aritmética computacional 
    4 - Arquitetura interna de um processador 
    5 - Hierarquia de memória 
    6 - Barramentos de dados

## Conteúdo

Arquitetura de computadores é o projeto estrutural de um computador, envolvendo os componentes físicos e lógicos essenciais ao seu funcionamento.

Os componentes essenciais de um computador são divididos em 3 camadas:

software de aplicação | software de sistema | hardware

- Software de aplicação: são aplicações típicas que os usuários utilizam - Ex: Banco de dados, sistemas de gestão, IDEs para desenvolvimento.
- Software de sistema: são programas que fornecem intermediários entre aplicação e hardware - Ex: Sistemas operacionais, compiladores e montadores.
- Hardware: é a parte física, executa apenas operações binárias. O hardware entende apenas sinais elétricos, que se restringe a ligado e desligado. deste modo, o alfabeto de um computador é composto de apenas duas letras: os dígitos binários ou bits (0 e 1)

Arquitetura de Von Neumann:


    memória <-> unidade lógica de aritmética
        ^           ^           |         ^
        |           |           v         |
        v           v           out       in
    unidade de controle  

Como a interação com o hardware usando código binário é ineficiente e improdutiva, foi-se criando linguagens que se assmelham mais à linguagem humana.

Todavia, as primeiras linguagens eram muito arcaicas, e se pareciam muito com as operações que o hardware era capaz de executar. As linguagens de alto nivel foram aparecendo com anos de desenvolvimento e aparecem até hoje.

Elas tem por objetivo:

<p>
1 - Fazer com que o programador escreva algo mais próximo de sua linguagem.<br />
2 - Aumentar a produtividade. <br />
3 - Fazer com que programas sejam independentes da plataforma.
</p> 

Isso gerou níveis de abstração:

<p>
Aplicação <br />
↓ código fonte <br />
Linguagem de alto nivel <br />
↓ compilador <br />
Linguagem de montagem (assembly)<br />
↓ montador (assembler)<br />
Sistema operacional <br />
↓ linker <br />
Linguagem de máquina <br />
↓ <br />
Microprograma <br />
↓ <br />
Lógica Digital <br />
</p>

Toda plataforma de hardware possui um conjunto de operações que são capazes de executar. Essas operações são obstraídas em instruções. Assim sendo, toda plataforma possui sua ARQUITETURA DO CONJUNTO DE INSTRUÇÕES (ISA, Instruction set Architective), que também é chamado de assembly da arquitetura.

A ISA de uma plataforma pode ser classificada em dois tipos, com relação à sua complexidade:

1 - RISC (Reducted Instruction set Computing) é um conjunto de instruções reduzido capaz de executar apenas algumas operações simples

2 - CISC (Complex Instruction set Architecture) é um conjunto composto por diversas instruções complexas.