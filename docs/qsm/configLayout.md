# Configuração avançada de layout

## Arquivo exemplo de configuração

=== "JSON"

    ???+ info "Configuração para o tipo de chamado"

        ```json
        {
          "executeMock": true,
          "ticketTypeId": 4,
          "actionButtons": [
            {
              "type": "general",
              "action": "qtFunction this.downloadTicket()",
              "aksConfirmation": false,
              "text": "",
              "condition": true,
              "outlined": true,
              "dense": true,
              "depressed": true,
              "icon": "mdi-download",
              "disableAutomaticRefresh": true
            },
            {
              "type": "general",
              "action": "qtFunction this.cancelTicket()",
              "aksConfirmation": false,
              "text": "Cancelar chamado",
              "condition": "qtFunction return this.userCanCancelTicket",
              "outlined": true,
              "dense": true,
              "depressed": true,
              "icon": "mdi-close",
              "disableAutomaticRefresh": true
            },
            {
              "type": "general",
              "action": "qtFunction return (async () => { await this.updateTicket({id: this.ticket.id, assignedToUser: this.me?.id })})()",
              "aksConfirmation": true,
              "text": "Pegar para mim",
              "dialogTitle": "Pegar para mim",
              "dialogText": "Deseja ser o atendente responsável por esse chamado?",
              "condition": "qtFunction return this.enableGetForMe",
              "outlined": false,
              "color": "primary",
              "dense": true,
              "depressed": true,
              "icon": "mdi-account"
            },
            {
              "type": "general",
              "action": "qtFunction return (async () => { await this.updateTicket({id: this.ticket.id, assignedToUser: null})})()",
              "aksConfirmation": true,
              "text": "Devolver para a fila",
              "dialogTitle": "Devolver para a fila",
              "dialogText": "Deseja devolver esse chamado para a fila?",
              "condition": "qtFunction return this.enableReturnToQueue",
              "outlined": false,
              "color": "primary",
              "dense": true,
              "depressed": true,
              "icon": "mdi-human-queue"
            },
            {
              "type": "general",
              "action": "qtFunction return (async () => { await this.updateTicket({id: this.ticket.id, status: 'Resolvido'})})()",
              "aksConfirmation": true,
              "text": "Resolver",
              "dialogTitle": "Resolver chamado",
              "dialogText": "Após essa ação, o chamado ficará com status 'Resolvido'. Deseja realizar esse procedimento?",
              "condition": "qtFunction return this.enableChangeStatus && this.ticket.status === 'Em Andamento'",
              "outlined": false,
              "color": "primary",
              "dense": true,
              "depressed": true,
              "icon": "check"
            }
          ],
          "extraTabs": [
            {
              "slug": "details",
              "path": "details",
              "text": "Detalhes"
            },
            {
              "slug": "slas",
              "path": "slas",
              "text": "SLAs"
            },
            {
              "slug": "relatedResources",
              "path": "relatedResources",
              "text": "Recursos Relacionados"
            },
            {
              "slug": "approvals",
              "path": "approvals",
              "text": "Aprovações"
            },
            {
              "slug": "ticketChildren",
              "path": "ticketChildren",
              "text": "Movimentações associadas",
              "filterConfig": {
                "hideQueue": true,
                "hideRecipient": true,
                "hideSectorTicket": true,
                "disableTicketType": true,
                "hideAssignedToUser": true,
                "hideStatus": true
              },
              "filter": {
                "$TicketType.name$": {
                  "not": ["Aprovação"]
                }
              },
              "createTicketCatalog": "assetsChanges",
              "fields": {
                "id": {"label": "Id", "sortable": true},
                "description": {"label": "Assunto", "sortable": true},
                "category": {"label": "Categoria"},
                "ticketCreatedByName": {
                  "label": "Criador",
                  "convert": "qtFunction return (input => input?.length ? this.shortName(input) : 'Não definido')(input)"
                },
                "createdAt": {
                  "label": "Criado em",
                  "sortable": true,
                  "convert": "qtFunction return (input => {return this.$moment(input).format('LLL')})(input)"
                },
                "updatedAt": {"label": "Atualizado em", "sortable": true}
              },
              "order": [["created_at", "desc"]]
            },
            {
              "slug": "ticketChildren",
              "path": "ticketChildren2",
              "text": "Orçamento",
              "filterConfig": {
                "hideQueue": true,
                "hideRecipient": true,
                "disableCategory": true,
                "hideSectorTicket": true,
                "disableTicketType": true,
                "hideAssignedToUser": true,
                "statusFromTicketTypes": [100033]
              },
              "fields": {
                "id": {"label": "Id", "sortable": true},
                "description": {"label": "Assunto", "sortable": true},
                "status": {"label": "Status", "sortable": true},
                "ticketTypeAdditionalInfo.custos_adicionais": {"label": "Custo adicional"},
                "ticketTypeAdditionalInfo.valor_de_fechamento": {"label": "Valor de fechamento"},
                "ticketCreatedByName": {
                  "label": "Criador",
                  "convert": "qtFunction return (input => input?.length ? this.shortName(input) : 'Não definido')(input)"
                },
                "createdAt": {
                  "label": "Criado em",
                  "sortable": true,
                  "convert": "qtFunction return (input => {return this.$moment(input).format('LLL')})(input)"
                },
                "updatedAt": {"label": "Atualizado em", "sortable": true}
              },
              "attributes": [
                "id",
                "status",
                ["ticketCreator.name", "ticketCreatedByName"],
                ["Category.name", "category"],
                "description",
                "updatedAt",
                "createdAt",
                "ticketTypeAdditionalInfo.custos_adicionais",
                "ticketTypeAdditionalInfo.valor_de_fechamento"
              ],
              "params": {
                "includeAdditionalInfos": true,
                "includeTicketGoals": false,
                "includeQueue": false,
                "includeSharedGroup": false,
                "includeTicketTypeAdditionalInfos": true
              },
              "filter": {},
              "createTicketCatalog": 153727,
              "order": [["created_at", "desc"]]
            }
          ],
          "sideBarTabs": [
            {
              "slug": "recordInfo",
              "path": "recordInfo",
              "text": "Informações",
              "icon": "mdi-information"
            }
          ],
          "recordInfo": {
            "title": "Informações do validador",
            "hideAssignedToUser": true,
            "recipientText": "Permissionário",
            "recipientLabel": "run qtFunction return 'Permissão: ' + (this.ticket?.ticketRecipient?.matricula || '')"
          },
          "texts": {
            "title": "Ativo",
            "generalInfo": {
              "title": "Informações Gerais",
              "ticketName": "Ativo",
              "deleteTicketBtn": "Apagar ativo"
            },
            "ticketActions": {
              "title": "Eventos do ativo",
              "createBtn": "Enviar",
              "createPrivatelyBtn": "Criar nota privada",
              "ticketCreatedHeader": "Criação do ativo",
              "ticketUpdatedHeader": "Atualização do ativo",
              "ticketKind": "ativo"
            },
            "additionalInfo": {
              "title": "Informações Adicionais",
              "saveBtn": "Salvar informações"
            },
            "cancelTicketBtn": "Cancelar ativo",
            "associatedTickets": {
              "title": "Ativos associados",
              "assocTicketHeader": "Ativo",
              "newAssocTicketBtn": "Associar novo ativo"
            },
            "sidebar": {
              "openBy": "Criado por"
            },
            "downloadTicketBtn": "Baixar dados"
          },
          "hideSLAs": true,
          "hideQueue": true,
          "hideMedias": true,
          "hidePriority": true,
          "readOnlyType": true,
          "hideApprovals": true,
          "readOnlyStatus": true,
          "readOnlyCategory": true,
          "createTicketCatalog": 153727,
          "hideDependentTickets": false
        }
        ```


## Introdução

Esta página tem como objetivo documentar a customização da tela `/workspace` através da base de dados workspace e/ou tipo de chamado e/ou categoria

## Onde realizar as configurações avançadas

### Tipos de chamados

![tipos de chamados](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2025%2F05%2F7ac319c3ca6420505a2ecfaa29f87464.png?alt=media&token=2bfad1f5-5e4b-40cf-a07d-62f64198a295)

### Categoria

![categoria](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2025%2F05%2Fbd84d593c7854e30660032f534d58cc3.png?alt=media&token=abd352bb-320a-49c1-983c-aef166860b4e)

### Base de dados workspace

![base de dados](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2025%2F05%2F740796ff203aa0ebef131bd686567d63.png?alt=media&token=9e660e9e-054d-44aa-b6b1-48e29b848f0f)

## Ordem da customização

A ordem da customização segue o padrão a seguir:

1. Categoria
2. Tipo de chamado
3. Workspace (base de dados)

**Equivalente em JS**

```jsx
const categoryConfig = {...}
const ticketTypeConfig = {...}
const workspaceConfig = {...}
const finalConfig = { ...workspaceConfig,...ticketTypeConfig,...categoryConfig}
```

Exemplo:

**Configuração da categoria**

```sql
{
  "a": 2,
  "b": 3,
  "c": 4
}
```

**Configuração do tipo de chamado**

```sql
{
  "x": 20
}
```

**Configuração do workspace**

```sql
{
  "x": 25,
  "y": 45,
  "b": 83
}
```

**Configuração resultante**

```sql
{
  "x": 20,
  "y": 45,
	"a": 2,
  "b": 3,
  "c": 4
}
```

# O que é possível configurar

É possível configurar toda a tela de workspace. Para simplificar podemos dividir essa tela em duas partes: Listagem de todos os chamados e Visão geral de um chamado

**Listagem de todos os chamados**

![listagem dos chamados](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2025%2F05%2F740796ff203aa0ebef131bd686567d63.png?alt=media&token=9e660e9e-054d-44aa-b6b1-48e29b848f0f)

Essa tela é configurada apenas pela base de dados `workspace`. 

**Exemplo de customização que podemos realizar nessa tela**

1. Escolher quais abas serão exibidas
2. Escolher quais colunas serão exibidas em cada aba
3. Limitar filtro


**Visão geral de um chamado**

A visão de um chamado pode ser customizada pela categoria, tipo de chamado e base de dados workspace, seguindo a ordem de customização já citada.

## Acessando dados interno da plataforma usando o JSON de configuração

É comum atrelar uma customização ao estado atual do chamado. Por exemplo: exibir um botão “Pegar para mim” apenas quando o status do chamado não for “Resolvido” ou “Fechado” e o usuário for atendente e que não seja o responsável pelo chamado

Para atender essa demanda, toda propriedade do JSON que possuir um texto começando com `qtFunction` será automaticamente convertido em código JS e encapsulado em uma função dentro da nossa plataforma. 

Caso o desenvolvedor queira apenas retornar uma função

Exemplo

```json
{
  "a": 2,
  "b": "qtFunction this.ticket.status", // uma função que retornará o status do ticket quando executada
  "c": "run qtFunction this.ticket.status"// variável que representa o status do ticket
}
```

**JSON Schema**

Para melhor visualização do jSON Schema utilize um desses sites

1. [https://jsonformatter.org/json-viewer](https://jsonformatter.org/json-viewer)
2. [https://jsonformatter.org/7980d5](https://jsonformatter.org/7980d5)

[JSON Schema Configurações avançadas de layout](./configJSON.md)

**actionButtons**

```json
"actionButtons": [
    {
      "type": "general",
      "action": "qtFunction this.downloadTicket()",
      "aksConfirmation": false,
      "text": "",
      "condition": true,
      "outlined": true,
      "dense": true,
      "depressed": true,
      "icon": "mdi-download",
      "disableAutomaticRefresh": true
    },
    {
      "type": "general",
      "action": "qtFunction this.cancelTicket()",
      "aksConfirmation": false,
      "text": "Cancelar chamado",
      "condition": "qtFunction return this.userCanCancelTicket",
      "outlined": true,
      "dense": true,
      "depressed": true,
      "icon": "mdi-close",
      "disableAutomaticRefresh": true
    },
    {
      "type": "general",
      "action":
        "qtFunction return (async () => { await this.updateTicket({id: this.ticket.id, assignedToUser: this.me?.id })})()",
      "aksConfirmation": true,
      "text": "Pegar para mim",
      "dialogTitle": "Pegar para mim",
      "dialogText": "Deseja ser o atendente responsável por esse chamado?",
      "condition": "qtFunction return this.enableGetForMe",
      "outlined": false,
      "color": "primary",
      "dense": true,
      "depressed": true,
      "icon": "mdi-account"
    },
    {
      "type": "general",
      "action":
        "qtFunction return (async () => { await this.updateTicket({id: this.ticket.id, assignedToUser: null})})()",
      "aksConfirmation": true,
      "text": "Devolver para a fila",
      "dialogTitle": "Devolver para a fila",
      "dialogText": "Deseja devolver esse chamado para a fila?",
      "condition": "qtFunction return this.enableReturnToQueue",
      "outlined": false,
      "color": "primary",
      "dense": true,
      "depressed": true,
      "icon": "mdi-human-queue"
    },
    {
      "type": "general",
      "action":
        "qtFunction return (async () => { await this.updateTicket({id: this.ticket.id, status: 'Resolvido'})})()",
      "aksConfirmation": true,
      "text": "Resolver",
      "dialogTitle": "Resolver chamado",
      "dialogText":
        "Após essa ação, o chamado ficará com status 'Resolvido'. Deseja realizar esse procedimento?",
      "condition":
        "qtFunction return this.enableChangeStatus && this.ticket.status === 'Em Andamento'",
      "outlined": false,
      "color": "primary",
      "dense": true,
      "depressed": true,
      "icon": "check"
    },
		{
      "text": "Nova configuração",
      "type": "createTicket",
      "color": "primary",
      "dense": true,
      "action": "qtFunction return this.createChildrenTicket({'headerTitle': 'Manutenção - Ajuste Configuração','categoryId': 100771, 'ticketKind': 'movimentação'})",
      "outlined": false,
      "condition": "qtFunction return this.ticket.status === 'Em Utilização' && this.$validatePermissions(['asset.isBarraSystem'])",
      "depressed": true
    }
  ],
```

**extraTabs**

```json
// podemos retirar qualquer uma dessa tabs
"extraTabs": [
    { "slug": "details", "path": "details", "text": "Detalhes" }, // mostra as informações básicas de um chamado
    { "slug": "slas", "path": "slas", "text": "SLAs" }, // Exibe SLAs do chamado
    {
      "slug": "relatedResources", // exibe chamados filhos e possibilita a criação de novos chamados filhos
      "path": "relatedResources",
      "text": "Recursos Relacionados"
    },
    { "slug": "approvals", "path": "approvals", "text": "Aprovações" }, // exibe as aprovações do chamado
    {
      "slug": "ticketChildren", // exibe uma tabela com os chamados filhos do chamado principal
      "path": "ticketChildren",
      "text": "Movimentações associadas",
      "filterConfig": { // configuração do filtro
        "hideQueue": true,
        "hideRecipient": true,
        "hideSectorTicket": true,
        "disableTicketType": true,
        "hideAssignedToUser": true,
        "hideStatus": true
      },
      "filter": { // where que será passado para a query de listagem dos chamados
        "$TicketType.name$": {
          "not": ["Aprovação"]
        }
      },
      "createTicketCatalog": "assetsChanges", // catálogo do chamado que será criado
      "fields": { // colunas que serão exibidas na tabela
        "id": { "label": "Id", "sortable": true },
        "description": { "label": "Assunto", "sortable": true },
        "category": { "label": "Categoria" },
        "ticketCreatedByName": {
          "label": "Criador",
          "convert": "qtFunction return (input => input?.length ? this.shortName(input) : 'Não definido')(input)"
        },
        "createdAt": {
          "label": "Criado em",
          "sortable": true,
          "convert": "qtFunction return (input => {return this.$moment(input).format('LLL')})(input)"
        },
        "updatedAt": { "label": "Atualizado em", "sortable": true }
      },
      "order": [["created_at", "desc"]] // ordem da listagem dos chamados
    },
    {
      "slug": "ticketChildren", // mesmo slug do item anterior, mas lembre-se de definir um path diferente
      "path": "ticketChildren2",
      "text": "Orçamento",
      "filterConfig": {
        "hideQueue": true,
        "hideRecipient": true,
        "disableCategory": true,
        "hideSectorTicket": true,
        "disableTicketType": true,
        "hideAssignedToUser": true,
        "statusFromTicketTypes": [100033]
      },
      "fields": {
        "id": { "label": "Id", "sortable": true },
        "description": { "label": "Assunto", "sortable": true },
        "status": { "label": "Status", "sortable": true },
        "ticketTypeAdditionalInfo.custos_adicionais": {
          "label": "Custo adicional"
        },
        "ticketTypeAdditionalInfo.valor_de_fechamento": {
          "label": "Valor de fechamento"
        },
        "ticketCreatedByName": {
          "label": "Criador",
          "convert": "qtFunction return (input => input?.length ? this.shortName(input) : 'Não definido')(input)"
        },
        "createdAt": {
          "label": "Criado em",
          "sortable": true,
          "convert": "qtFunction return (input => {return this.$moment(input).format('LLL')})(input)"
        },
        "updatedAt": { "label": "Atualizado em", "sortable": true }
      },
      "attributes": [
        "id",
        "status",
        ["ticketCreator.name", "ticketCreatedByName"],
        ["Category.name", "category"],
        "description",
        "updatedAt",
        "createdAt",
        "ticketTypeAdditionalInfo.custos_adicionais",
        "ticketTypeAdditionalInfo.valor_de_fechamento"
      ],
      "params": {
        "includeAdditionalInfos": true,
        "includeTicketGoals": false,
        "includeQueue": false,
        "includeSharedGroup": false,
        "includeTicketTypeAdditionalInfos": true
      },
      "filter": { },
      "createTicketCatalog": 153727,
      "order": [["created_at", "desc"]]
    }
  ],
```