# Documentação - Funcionalidade de Filtro por Coluna

## Visão Geral

A funcionalidade de **filtro por coluna** foi implementada para melhorar o desempenho nas buscas de tickets nos componentes que utilizam o `ticketTable`. O principal objetivo é reduzir a latência e aumentar a precisão das pesquisas, permitindo ao usuário escolher em qual coluna deseja realizar a busca.

!!! info "Melhoria de Desempenho"
    O tempo de busca em uma organização foi reduzido de **11 segundos para 2.5 segundos** com o uso dessa funcionalidade.

## Contexto

A aplicação apresentava alta latência nas buscas, especialmente ao procurar informações específicas dos tickets. A causa estava relacionada à estrutura de busca baseada em uma lista simples de colunas, o que tornava o processo menos eficiente.

---

## O que mudou

### Estrutura de busca anterior

A busca era feita com base em uma lista de strings:

```json
{
  "columnsToSearch": ["id", "description"]
}
```
### Nova estrutura

Agora, a busca é configurada com um array de objetos contendo as propriedades `text` e `value`, possibilitando a exibição de um componente de seleção (select) para que o usuário escolha a coluna:


```json
{
  "columnsToSearch": [
    { "text": "Id", "value": "id" },
    { "text": "Descrição", "value": "description" }
  ]
}
```

O componente select será exibido somente quando `columnsToSearch` for um array de objetos no formato esperado.

## Disponibilidade para Usuário

Ao buscar por tickets, um campo de seleção será exibido permitindo ao usuário escolher a coluna na qual deseja realizar a busca, tornando o processo mais eficiente e relevante.

![image](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F3cb2100db1c85e1d343a2ddf30405f37.jpg?alt=media&token=590dccb9-14d5-42ca-92c1-da101d6be2e9)

## Considerações Finais

A funcionalidade é reutilizável em qualquer componente que utilize o `ticketTable`.

O componente select será exibido apenas se `columnsToSearch` estiver no formato de array de objetos contendo as propriedades `text` e `value`.