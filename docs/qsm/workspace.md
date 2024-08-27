# Workspace

Este documento fornece uma visão geral da interface do Workspace, destacando as
principais funcionalidades e configurações disponíveis.

## Explorando a tela do Workspace

A interface utilizada para visualizar os status dos tickets é mostrada na imagem
abaixo. A seguir, destacamos os principais elementos dessa tela:

1. **Workspace**: Aba que irá indicar qual é o Workspace que você está
   visualizando.
2. **Abas**: São todas as abas que seu Workspace pode ter, configuradas pra
   tipos de visualizações específicas com base em filtros pelos dados dos
   tickets.
3. **Botão de criação**: Permite a criação de novos tickets, seja de forma
   individual ou em lote, desde que as permissões e configurações necessárias
   estejam habilitadas.
4. **Tabela de visualização**: Exibe a lista de tickets disponíveis e suas
   respectivas informações. Esta tabela também oferece ferramentas para busca,
   filtragem, e outras ações para melhor gerenciar os tickets. Entenda melhor
   sobre as tabelas do Quoti clicando
   [aqui](https://docs.quoti.cloud/quoti-databases/).

![Visão do Workspace](https://storage.googleapis.com/quoti-docs-pictures/QSM/workspace/workspace-visao-geral.jpg)
**Figura 1**: Visão geral do Workspace

## Configurações Avançadas de Layout

No texto anterior, mencionamos as "configurações necessárias". Mas, o que
exatamente são essas configurações e como podemos ajustá-las? O Workspace
oferece uma gama de opções de personalização para que você possa adaptar a
interface às suas necessidades. Essas personalizações são feitas por meio de um
arquivo JSON, permitindo que você ajuste o layout e o comportamento da interface
conforme suas preferências.

### Passos para configuração do Layout

#### 1° passo

Para acessar as configurações avançadas de layout, é necessário adicionar o
parâmetro de configuração `&c=1` no final da URL da plataforma em seu navegador.

#### 2° passo

Após adicionar o parâmetro, um botão de configuração aparecerá no seu Workspace.
Clique nesse botão para abrir um pop-up onde você poderá ajustar o layout de
acordo com suas preferências. Agora, você está pronto para personalizar seu
Workspace.

**Observação: é necessário que seu perfil tenha as permissões necessárias para
que possa visualizar esse botão.**

### Entendendo o JSON de Configuração

O JSON de configuração é fundamental para definir as opções avançadas de layout
nesta tela. Com ele, você pode realizar diversas customizações, como:

1. Alterar ou adicionar novas abas à interface.
2. Configurar parâmetros de busca nas tabelas, incluindo filtros, opções de
   ordenação e outros critérios.
3. Aplicar estilos personalizados usando CSS em diferentes componentes da tela.

#### Relação entre Propriedades do JSON e Componentes da Tela

![Propriedades do JSON](https://storage.googleapis.com/quoti-docs-pictures/QSM/workspace/workspace-props-json.png)
**Figura 2**: Propriedades do JSON

1. **Botão de configuração**: Abre o JSON de configuração.

**Propriedades do JSON:**

2. **tabs**: Configura as abas do Workspace.
3. **defaultTab**: Configura o que será exibido nas tabelas das abas do
   Workspace.
4. **ticketAgentOverview**: Define as configurações específicas da interface
   dentro de um ticket.

## Configurando a **_defaultTab_** do Workspace

![DefaultTab do Workspace](https://storage.googleapis.com/quoti-docs-pictures/QSM/workspace/workspace-defaulttab-json.png)
**Figura 3**: DefaultTab do Workspace

A _defaultTab_ é responsável por definir o que será exibido nas tabelas das
diferentes abas dentro do Workspace. Com ela, você pode configurar desde o
comportamento do botão de criação até os campos, atributos e outros parâmetros
que serão utilizados nas tabelas.

Existem dois componentes que podem ser renderizados no Workspace. O componente
padrão é a **_ticketTable_**, e outro é uma tabela do Quoti, chamada de
**_qtDatabase_**. A ticketTable também é uma tabela padrão do Quoti, na qual
você pode conter diversas personalizações dentro dela, de acordo com o que você
precisar. Conheça mais sobre uma **_qtDatabase_** clicando
[aqui](https://docs.quoti.cloud/quoti-databases/).

**Exemplo de configuração de uma _ticketTable_:**

```javascript
{
  "order": [
    [
      "TicketGoals.expireAt",
      "ASC"
    ]
  ],
  "fields": [
    {
      "name": "id",
      "label": "id",
      "sortable": true
    },
    {
      "name": "ticketType100046AdditionalInfo.estado",
      "label": "UF",
      "sortable": true
    },
    {
      "name": "ticketAssignedToUserName",
      "label": "Negociador",
      "convert": "{{ double_keys_open }} (input) => { return (input => input?.length ? $shortName(input) : 'Não definido')(input) } {{ double_keys_close }}"
    },
    {
      "name": "status",
      "label": "Status do acordo",
      "sortable": true
    },
    {
      "name": "createdAt",
      "type": "date",
      "label": "Criado em",
      "convert": "{{ double_keys_open }} (input) => { return (input => {return $$moment(input).format('LLL')})(input) } {{ double_keys_close }}",
      "sortable": true
    },
    {
      "name": "updatedAt",
      "type": "date",
      "label": "Atualizado em",
      "sortable": true
    }
  ],
  "params": {
    "includeQueue": false,
    "additionalInfos": [
      {
        "ticketTypeId": 100046
      }
    ],
    "columnsToSearch": [
      "id",
      "description",
      "status",
      "$ticketType100046AdditionalInfo.estado$",
      "$ticketType100046AdditionalInfo.estado$"
    ],
    "includeSharedGroup": false,
    "includeTicketGoals": true,
    "includeAdditionalInfos": true
  },
  "attributes": [
    "id",
    [
      "TicketType.name",
      "type"
    ],
    [
      "ticketRecipient.name",
      "ticketRecipientName"
    ],
    [
      "ticketAssignedToUser.name",
      "ticketAssignedToUserName"
    ],
    [
      "ticketCreator.name",
      "ticketCreatedByName"
    ],
    [
      "Category.name",
      "category"
    ],
    "description",
    "status",
    "createdAt",
    "updatedAt",
    "isAppointment",
    "scheduleDate",
    "duration",
    [
      "TicketGoals.expire_at",
      "goalsExpiresAt"
    ],
    [
      "TicketGoals.finished_at",
      "goalsFinishedAt"
    ],
    [
      "TicketGoals.paused_at",
      "goalsPausedAt"
    ]
  ],
  "filterConfig": {
    "hideQueue": true,
    "hideCategory": true,
    "hideCreatedBy": true,
    "hideRecipient": true,
    "ticketTypeIds": [
      100046
    ],
    "hideCreatedByt": true,
    "hideTicketType": false,
    "hideSectorTicket": true,
    "disableTicketType": true,
    "hideSearchByTitle": true,
    "assignedToUserWhere": {
      "userProfileId": 100024
    },
    "listAvailableToOrder": [
      {
        "name": "Tempo de criação",
        "value": "created_at"
      },
      {
        "name": "Última atualização",
        "value": "updatedAt"
      }
    ],
    "statusFromTicketTypes": [
      100046
    ]
  },
  "createDialogTitle": "Adicionar acordos",
  "createTicketsInBatch": {
    "sheetColumns": [
      {
        "text": "Negociador",
        "type": "text",
        "value": "negociador",
        "sheetColumn": "Negociador",
        "showInTable": true
      },
      {
        "text": "Estado",
        "type": "text",
        "value": "estado",
        "sheetColumn": "Estado",
        "showInTable": true
      },
      {
        "text": "Forma de pagamento",
        "type": "text",
        "value": "forma_de_pagamento",
        "sheetColumn": "Forma de pagamento",
        "showInTable": true
      },
      {
        "text": "Tipo",
        "type": "text",
        "value": "tipo",
        "sheetColumn": "Tipo",
        "showInTable": true
      }
    ],
    "exampleSheetLink": "https://storage.googleapis.com/beyond-quoti.appspot.com/CSM/link-de-example",
    "webhookSaveAction": "https://workflow.quoti.cloud/webhook/webhook-de-exemplo"
  }
}
```

### Parâmetros do JSON e o que fazem

1. **order**: Define a ordem em que os itens da tabela serão exibidos.
2. **fields**: Especifica as colunas que serão renderizadas na tabela.
3. **params**: Parâmetros utilizados na requisição para buscar os tickets.
4. **attributes**: Não sei o que é.
5. **filterConfig**: Configura o campo de busca e filtro da tabela.
6. **createDialogTitle**: Define o título da janela de dialog que aparece ao
   criar um novo ticket.
7. **createTicketsInBatch**: Configura a inserção de novos tickets em lote via
   planilha.

Como dito anteriormente, essa foi uma configuração para defaultTab que tem como
componente default a _ticketTable_. Para renderizar uma _qtDatabase_ ao invés
disso, podemos passar a propriedade **_slug_** no JSON de configuração, assim:

![Configurando qtDatabase no Workspace](https://storage.googleapis.com/quoti-docs-pictures/QSM/workspace/workspace-slug-qtdatabase.png)
**Figura 4**: Configurando qtDatabase no Workspace

Quando se trata de um _qtDatabase_, passamos algumas propriedades diferentes de
quando é um _ticketTable_, como pode reparar no exemplo acima:

1. A propriedade **fields** foi trocada por **headers**.
2. A propriedade **filter** foi trocada por **where**.

### Configurando o botão de criação

Existem algumas formas de configurar o botão de criação:

- **Criação em lote**: Assim como foi explicado no exemplo acima, podemos
  configurar nossa criação em lote dentro do defaultTab na propriedade
  **_createTicktesInBatch_**.

- **Criação por formulário**: Dessa forma, precisamos passar a propriedade
  **_formId_** com o ID do formulario para dentro de defaultTab, dessa forma:

```javascript
{
  "defaultTab": {
    "slug": "qtDatabase",
    "table": "audiencia_prazos",
    "formId": 100327, // ID do formulário que será exibido.
  }
}
```

- **Criação por catálogo**: Dessa forma, precisamos passar qual vai ser o
  catálogo dentro de cada Tab, dessa forma:

![Criação por catálogo](https://storage.googleapis.com/quoti-docs-pictures/QSM/workspace/workspace-default-tab-catalog.png)

Ainda, você pode configurar algumas propriedades de acordo com os números da
imagem:

1. **Botão de criação**: Você pode passar propriedades como **_color_**,
   **_outlined_**, etc. Para saber o que é suportado, consulte essa documentação
   [aqui](https://v2.vuetifyjs.com/en/api/v-btn/).
2. **Configurações do catálogo**: Dentro de cada Tab, teremos a propriedade
   **_createTicket_** e dentro dela iremos passar o **_catalog_**, como mostrado
   na imagem acima.
3. **Título de criação do dialog**: Você pode configurar o nome que será exibido
   na criação de um dialog. O resultado ficará assim como na imagem abaixo:

![Título do dialog](https://storage.googleapis.com/quoti-docs-pictures/QSM/workspace/workspace-dialog-title.png)
**Figura 6**: Título do dialog

## Configurando as **_tabs_** do Workspace

![Tabs do Workspace](https://storage.googleapis.com/quoti-docs-pictures/QSM/workspace/workspace-tabs-json.png)
**Figura 7**: Abas do Workspace

Assim como mostrado na imagem acima, no item **1** podemos ver todas as 6 abas
que nosso Workspace possui, e no item **2** podemos ver como elas são
configuradas.

**Exemplo de configuração:**

```javascript
{
  "path": "ticketTable", // Identificador da Tab no Workspace.
  "slug": "ticketTable", // Identificador que diz qual componente será exibido naquela Tab.
  "text": "Acordos aguardando negociador", // Texto que irá aparecer ao lado da tabela.
  "filter": { // Filtros que aquela tabela da Tab vai receber.
    "isAppointment": false,
    "assignedToUser": "{{ double_keys_open }} $validatePermissions(['agreements.manager']) ? undefined : $me.id {{ double_keys_close }}",
    "$TicketType.id$": 100046,
    "$TicketType.name$": {
      "not": "Aprovação"
    },
    "$ticketType100046AdditionalInfo.status_da_comunicacao$": "Aguardando negociador"
  },
  "tabTitle": "Aguardando negociador" // Título da Tab.
}
```

## Configurando o **_ticketAgentOverview_**

O _ticketAgentOverview_ é responsável por definir as configurações específicas
da interface dentro de um ticket, injetando essas configurações diretamente na
tela de visão geral do agente. Isso permite personalizar a experiência do agente
ao interagir com os tickets.

**Exemplo de configuração:**

```javascript
"ticketAgentOverview": {
    "general": {
      "extraTabsMaxWidth": 70, // Indica a largura máxima das extraTabs.
      "extraTabsMinWidth": 20, // Indica a largura mínima das extraTabs.
      "isExtraTabsResize": true, // Indica se a extraTab será redimensionável.
      "initialExtraTabsWidth": 35 // Indica a largura inicial das extraTabs.
    },
    "listOneParams": {
      "additionalInfos": [
        {
          "ticketTypeId": 100046 // Lista as informações adicionais de um ticket.
        }
      ]
    }
  }
```
