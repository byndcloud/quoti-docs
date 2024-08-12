Atualmente, a BeyondCo utiliza bancos de dados SQL. Substituir o uso das Quoti Databases por um banco NoSQL acarretaria em alguns desafios significativos:

-   **Aumento de Custo**: Manter dois bancos de dados separados (um SQL e um NoSQL) elevaria os custos operacionais.
-   **Impossibilidade de Referenciamento**: Não seria possível referenciar as tabelas existentes do nosso banco de dados SQL.

As Quoti Databases foram desenvolvidas para mitigar esses problemas.

No entanto, as Quoti Databases também têm suas limitações. Devido à forma como são implementadas, enfrentamos desafios de performance, tanto em termos de uso de memória quanto de tempo de execução.

Apesar disso, essas limitações são atenuadas por meio da materialização das tabelas, um tópico que será abordado na próxima seção.