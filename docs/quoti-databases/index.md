# Visão Geral de Quoti Databases

Esta seção tem como objetivo explicar o que são as Quoti Databases e como utilizar essas ferramentas amplamente usadas na BeyondCo e essenciais para um desenvolvimento ágil.



## O Que São Quoti Databases?


Quoti Databases são, essencialmente, uma **virtualização de coleções**.

> Em computação, **virtualização** refere-se à criação de uma versão virtual (em vez de real) de algo.

Em vez de criarmos tabelas relacionais, que são mais rígidas e difíceis de modificar, utilizamos uma estrutura que simula coleções (ou "tabelas não-relacionais") dentro dos bancos de dados relacionais do Quoti.
### Bancos de Dados Relacionais vs. Não Relacionais

Compreender os conceitos de bancos de dados **relacionais** e **não relacionais** é crucial para desenvolvedores. Em um banco de dados relacional, os dados são organizados em tabelas com um esquema fixo, o que torna a modificação desse esquema mais complexa e menos flexível. Já em um banco de dados não relacional, os dados são armazenados em coleções, permitindo uma maior flexibilidade na estrutura dos dados, onde o esquema pode ser facilmente modificado para se adaptar a novas necessidades.

Para uma explicação mais detalhada, consulte a documentação da AWS: [Diferença entre bancos de dados relacionais e não relacionais](https://aws.amazon.com/pt/compare/the-difference-between-relational-and-non-relational-databases/).

### Multi-Tenancy
Seguindo a arquitetura **multi-tenant**, essas Quoti Databases existem para cada uma das organizações inquilinas do Quoti.
>Software **multi-tenant**:
> Basicamente, um software que implementa o conceito de *multitenancy* foi desenvolvido para suportar múltiplos inquilinos ("tenants”).
![multi-tenant](https://imgs.search.brave.com/wHkdrpnBFHLCu2pCi9nzq4dQIXLt9NKt3SwedbJWHPY/rs:fit:860:0:0:0/g:ce/aHR0cHM6Ly9tZWRp/YS5saWNkbi5jb20v/ZG1zL2ltYWdlL0Q0/RDEyQVFHdTZBTEJ3/RWdOUUEvYXJ0aWNs/ZS1jb3Zlcl9pbWFn/ZS1zaHJpbmtfNjAw/XzIwMDAvMC8xNzA2/MTAwODI5NjQ2P2U9/MjE0NzQ4MzY0NyZ2/PWJldGEmdD05MUhn/M0xwQktRTENMcjFw/WV9xWmFEcE9paHMy/aHR2RFRJMXlfNGFu/WUk4)

### Objetivo
As Quoti Databases são projetadas para fornecer a flexibilidade de um banco de dados não relacional, enquanto aproveitam a robustez e a estrutura dos bancos de dados relacionais, possibilitando tanto um desenvolvimento rápido, quanto uma estrutura segura.