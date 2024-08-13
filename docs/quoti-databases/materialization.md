# Materialização de tabelas

Como você pode perceber, uma tabela de Quoti Databases contém diversas definições relacionadas, inclusive compartilhadas com a plataforma Quoti. Dois exemplos são: Tipos do Campo e Formulários - São definições/modelos universais da plataforma.

Uma vez que entendemos que uma tabela de Quoti Databases tem muitos relacionamentos com outros dados do Quoti, podemos dizer que na prática, listar dados de uma tabela de Quoti Databases deve exigir um custo de performance alto no banco de dados físico, principalmente quando se trata de tabelas do Quoti Databases com centenas de milhares de linhas.

Para melhorar a performance de tabelas com muitas linhas, o Quoti traz uma solução chamada “materialização de tabelas”.

!!! note "AVISO"
    🚨 A materialização de tabelas é um procedimento **irreversível** no banco de dados.

## O que é a materialização de tabelas?

Uma tabela materializada é uma tabela de Quoti Databases que virou uma tabela de verdade no banco de dados físico da plataforma. Este procedimento geralmente é feito a fim de otimizar CRUDs em tabelas lentas por conta de ter muitos registros.

## O que você precisa saber antes de materializar uma tabela?

- Uma vez que a tabela foi materializada, não é possível alterar o schema - Isto é: não dá para renomear, adicionar, remover ou alterar o tipo de dado de colunas da sua tabela.

## Materializando uma tabela

1. Acesse o botão de gerenciar sua base de dados
    
    ![botão de gerenciar base de dados](https://storage.googleapis.com/beyond-quoti.appspot.com/docs/qtdatabases/materialize/materialize-03.png)
    
2. Clique no botão “⚡️ Materializar”, disponível na aba de “Propriedades” da base de dados.
    
    ![botão de materializar tabela](https://storage.googleapis.com/beyond-quoti.appspot.com/docs/qtdatabases/materialize/materialize-01.png)
    
3. O sistema processará seu pedido e avisará quando a materialização for concluída

## Como saber se minha tabela está materializada?

Todas as tabelas materializadas são apresentadas na tela de gestão de bases de dados com um ícone de Raio “⚡️” próximo ao botão de gerenciar.

![indicador de tabela materializada](https://storage.googleapis.com/beyond-quoti.appspot.com/docs/qtdatabases/materialize/materialize-02.png)