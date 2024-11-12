## Objetivo

Nesta seção, vamos colocar em prática o uso dos QSM, aplicando seus conceitos e ferramentas para implementar o exemplo apresentado no **Gerenciamento de Tickets**.

### Criar Tipo de Caso

Primeiramente, é necessário criar um **ticketType**, que servirá como base para o nosso caso de uso.

1. Acesse a página que lista todos os tipos de tickets disponíveis em `/ticketTypes`.

   ![Criar um ticketType](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/criarTicket.png)
   ![Configurando ticketType](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/preenchedoTicketType.png)

2. Para criar um formulário vinculado ao ticket, defina campos para que o usuário descreva o problema. Também é possível configurar os status do ticket, como, por exemplo: Pendente, Em andamento, Cancelado e Concluído.

   ![Criar formulário adicional e status](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/criarFormularioTicket.png)
   ![Salvando formulário](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/salvandoTicketType.png)

### Criar Fila

Em segundo lugar, é necessário criar uma **fila** para definir o setor ou a prioridade em que o ticket será alocado.

1. Acesse a página de filas disponíveis em `/groups/fila?groupTypeName=fila`.

   ![Criar uma fila](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/criarFila.png)
   ![Salvando fila](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/salvandoFila.png)

### Criar Categoria

Em seguida, é necessário criar uma **categoria** que especificará a qual grupo o ticket criado pertencerá.

1. Acesse a página de categorias disponíveis em `/categories`.

   ![Criar uma categoria](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/criarCategorie.png)
   ![Configurando uma categoria](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/configurandoCategoria.png)

2. Todo ticket criado por meio dessa categoria será direcionado automaticamente para a fila definida.

   ![Adicionando a categoria a uma fila](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/selecionandoFila.png)

3. É possível criar um formulário específico tanto para o ticket quanto para a categoria.

   ![Adicionando um formulário à categoria](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/criarFormularioCategoria.png)
   ![Configurando formulário da categoria](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/configurandoFormCategoria.png)
   ![Salvando categoria](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/salvandoCategoria.png)

### Criar Catálogo

Após configurar o `ticketType`, fila e categorias, o próximo passo é criar um **catálogo** (catalog), que pode incluir um conjunto de serviços ou um único serviço.

1. Acesse a página de [Catalog](/orgs/byndcloud/qsm/catalog/) e siga as etapas até a seção 1.6.
2. Após criar o catálogo, ele poderá ser acessado pela URL `/app/catalog/meu_catalogo`.

### Criar Conjunto de Serviços

Agora, você utilizará todas as configurações feitas anteriormente, como `ticketType` e categorias.

1. Acesse seu catálogo de serviços e clique em **Adicionar item**.

   ![Adicionar item ao catálogo](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/adicionandoItemCatalog.png)

2. Escolha a opção **Conjunto de Serviços** e preencha os dados necessários, como nome, resumo dos serviços e imagem.

   ![Tipo de Serviço no catálogo](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/tipoServicoCatalog.png)
   ![Configurando conjunto criado](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/configurandoConjuntoServico.png)

### Criar Serviço

Dentro do conjunto de serviços criado, é possível adicionar serviços específicos.

1. Acesse o conjunto criado e clique para gerenciar os serviços.

   ![Gerenciar conjunto criado](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/conjuntoCriado.png)

2. Escolha um serviço, associando-o ao `ticketType` e à categoria desejada. Cada serviço deve ter uma categoria única, com um formulário correspondente.

   ![Escolhendo um ticketType](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/configurandoServicoTicketType.png)
   ![Escolhendo uma categoria](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/configurandoServicoCategoria.png)

3. Configure os campos a serem exibidos para o usuário. Se o formulário já inclui descrição e anexo, essas opções não precisam ser selecionadas.

   ![Configurando serviço criado](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/configurandoServico1.png)

Após essas configurações, você terá um formulário acessível ao clicar no serviço cadastrado.

   ![Acessando Serviço](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/criandoUmticket.png)

Ao criar um ticket, você será redirecionado para a tela de informações detalhadas do ticket. Nessa tela, é possível visualizar e gerenciar todos os dados relevantes.

Para personalizar a tela conforme as necessidades da sua organização, siga o exemplo abaixo:

### Visão geral do atendente

```javascript
{
  "hooks": {
     "ticketLoaded": "{{ double_keys_open }}  {{ double_keys_close }}"    
  },
  "mainTab": {
    "icon": "mdi-ticket",
    "showIcon": true,
    "hideClose": false,
    "iconColor": "#757575"
  },
  "extraTabs": [
    {
      "path": "detailsOnly",
      "slug": "detailsOnly",
      "text": "Detalhes",
      "TicketDetails": {
        "Header": {
          "hideName": true,
          "hideText": true
        },
        "saveBtn": "Salvar informações",
        "hideBody": true,
     "saveFormsCallback": "{{ double_keys_open }} () => { $fetchTicket() } {{ double_keys_close }}",
        "TicketPrimaryInfos": {
          "hideQueue": true,
          "hideStatus": true,
          "hidePriority": true,
          "readOnlyType": true,
          "readOnlyStatus": true,
          "readOnlyCategory": true
        },
        "hideTicketPrimaryInfos": true,
        "hideAdditionalInfoTitle": true
      }
    },
    {
      "path": "history",
      "slug": "history",
      "text": "Eventos",
      "TicketActions": {}
    },
    {
      "path": "relatedResources",
      "slug": "relatedResources",
      "text": "Recursos Relacionados"
    },
    {
      "path": "approvals",
      "slug": "approvals",
      "text": "Aprovações"
    }
  ],
  "sideBarTabs": [
    {
      "icon": "mdi-information",
      "path": "recordInfo",
      "slug": "recordInfo",
      "text": "Informações",
      "useGroupAssoc": false,
      "whereUserSearch": {
        "userProfileId": {
          "not": 100007
        }
      },
      "enableChangeAssignedToUserToAnyUser": "(ticketAgentOverview){{ double_keys_open }} !$isCancelled && !$isClosed && !$isSolved {{ double_keys_close }}"
    },
    {
      "icon": "mdi-attachment",
      "path": "attachments",
      "slug": "attachments",
      "text": "Anexos"
    }
  ],
  "AgentOverviewHeader": {
    "Slot1": {
      "children": [
        {
          "slug": "ticketPrimaryInfos",
          "props": {
            "ticket": "{{ double_keys_open }} $ticket {{ double_keys_close }}",
            "configs": {
              "infos": [
                {
                  "slug": "text",
                  "text": "Status",
                  "label": "status",
                  "modifier": {
                      "value": "{{ double_keys_open }} $ticket?.status || 'Não identificado' {{ double_keys_close }}"
                  },
                  "modifierSlug": "text"
                },
                {
                  "slug": "text",
                  "text": "Categoria",
                  "label": "categoria",
                  "modifier": {
                    "value": "{{ double_keys_open }} $ticket?.Category?.name || 'Não identificado' {{ double_keys_close }}"
                  },
                  "modifierSlug": "text"
                },
                {
                  "slug": "text",
                  "text": "Criado em",
                  "label": "criado em",
                  "modifier": {
                   "value": "{{ double_keys_open }} $$moment($ticket?.createdAt).format('DD/MM/YYYY [às] HH:mm') || 'Não identificado' {{ double_keys_close }}"
                  },
                  "modifierSlug": "text"
                },
                {
                  "slug": "text",
                  "text": "Responsável",
                  "label": "responsavel",
                  "modifier": {
                    "value": "{{ double_keys_open }} $ticket?.ticketAssignedToUser?.name || 'Não identificado' {{ double_keys_close }}"
                  },
                  "modifierSlug": "text"
                },
                {
                  "slug": "text",
                  "text": "Última atualização",
                  "label": "ultima atualizacao",
                  "modifier": {
                    "value": "{{ double_keys_open }} $$moment($ticket?.updatedAt).format('DD/MM/YYYY [às] HH:mm') || 'Não identificado' {{ double_keys_close }}"
                  },
                  "modifierSlug": "text"
                }
              ]
            }
          }
        }
      ]
    },
    "title": "{{ double_keys_open }} 'Chamado ' +  $ticket?.id {{ double_keys_close }}",
    "hideStatus": true,
    "titleCaption": "Solicitação associada ao",
    "hideCreatedAt": true,
    "oldActionButtons": [
      {
        "icon": "mdi-delete",
        "text": "Remover chamado",
        "type": "general",
        "color": "red",
        "dense": true,
        "action": "{{ double_keys_open }} (input) => { return (async () => { await $preDeleteTicket($ticket); $closeTab($ticket) })() } {{ double_keys_close }}",
        "outlined": true,
        "condition": "{{ double_keys_open }} (input) => { return $userCanDeleteTicket } {{ double_keys_close }}",
        "depressed": true,
        "aksConfirmation": false,
        "hideTextOnDesktop": true
      },
      {
        "icon": "mdi-download",
        "text": "Download",
        "type": "general",
        "dense": true,
        "action": "{{ double_keys_open }} (input) => { return (async () => { await $downloadTicket() })() } {{ double_keys_close }}",
        "outlined": true,
        "condition": "{{ double_keys_open }} (input) => { return true } {{ double_keys_close }}",
        "depressed": true,
        "aksConfirmation": false,
        "hideTextOnDesktop": true,
        "disableAutomaticRefresh": true
      },
      {
        "icon": "mdi-close",
        "text": "Cancelar chamado",
        "type": "general",
        "dense": true,
        "action": "{{ double_keys_open }} (input) => { return (async () => { await $cancelTicket() })() } {{ double_keys_close }}",
        "outlined": false,
        "textProp": true,
        "condition": "{{ double_keys_open }} (input) => { return $userCanCancelTicket } {{ double_keys_close }}",
        "depressed": true,
        "aksConfirmation": false,
        "disableAutomaticRefresh": true
      },
      {
        "icon": "mdi-account",
        "text": "Pegar para mim",
        "type": "general",
        "color": "primary",
        "dense": true,
        "action": "{{ double_keys_open }} (input) => { return (async () => { await $updateTicket({id: $ticket.id, assignedToUser: $me?.id, status: 'Em andamento' }) })() } {{ double_keys_close }}",
        "outlined": false,
        "condition": "{{ double_keys_open }} (input) => { return !$assignedToUserIsMe && !$isCancelled && !$isClosed && !$isSolved } {{ double_keys_close }}",
        "depressed": true,
        "dialogText": "Deseja ser o atendente responsável por esse chamado?",
        "dialogTitle": "Pegar para mim",
        "aksConfirmation": true
      },
      {
        "icon": "mdi-human-queue",
        "text": "Devolver para a fila",
        "type": "general",
        "color": "primary",
        "dense": true,
        "action": "{{ double_keys_open }} (input) => { return (async () => { await $updateTicket({id: $ticket.id, assignedToUser: null, status: 'pendente' }) })() } {{ double_keys_close }}",
        "outlined": true,
        "condition": "{{ double_keys_open }} (input) => { return $enableReturnToQueue } {{ double_keys_close }}",
        "depressed": true,
        "dialogText": "Deseja devolver esse chamado para a fila?",
        "dialogTitle": "Devolver para a fila",
        "aksConfirmation": true
      },
      {
        "icon": "check",
        "text": "Resolver",
        "type": "general",
        "color": "primary",
        "dense": true,
        "action": "{{ double_keys_open }} (input) => { return (async () => { await $updateTicket({id: $ticket.id, status: 'Resolvido'}) })() } {{ double_keys_close }}",
        "outlined": false,
        "condition": "{{ double_keys_open }} (input) => { return ($enableChangeStatus && $ticket.status === 'Em andamento') } {{ double_keys_close }}",
        "depressed": true,
        "dialogText": "Após essa ação, o chamado ficará com status \"Resolvido\". Deseja realizar esse procedimento?",
        "dialogTitle": "Resolver chamado",
        "aksConfirmation": true
      }
    ],
    "allowResizeHeader": false,
    "groupedButtonName": "Realizar ação",
    "maxVisibleButtons": 3
  }
}
```
Para mais informações sobre cada propriedade e para visualizar detalhes sobre elas, consulte a [Visão geral do atendente](/orgs/byndcloud/qsm/agentOverview/).

### Criar workspace

Se desejar, você pode criar um workspace para listar todos os tickets de um tipo ou categoria específicos. Siga o tutorial abaixo para configurar o seu:

1. Acesse /databases e procure por **workspaces**, clique em **Acessar database**

   ![Pesquisar por workspace](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/databaseWorkspaces.png)

2. Você será redirecionado para a tela de listagem de workspaces, onde também é possível criar novos. Clique em **Novo** e, em seguida, selecione **individualmente** para iniciar a criação

   ![Criar workspace](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/criarWorkspace.png)

3. Em seguida, será exibido um diálogo onde você poderá definir o path e o nome do seu workspace, além de configurar algumas opções básicas. O valor padrão para o path é tickets, enquanto o nome será o identificador de referência dentro do sistema.
    ![Criando workspace](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/criandoWorkspace.png)


4. Configurações básica para *workspace*:

```javascript
{
  "tabs": [
    {
      "path": "ticketTable",
      "slug": "ticketTable",
      "text": "Chamados em aberto",
      "filter": {
        "status": [
          "pendente"
        ]
      },
      "tabTitle": "Pendente"
    },
    {
      "path": "ticketTable",
      "slug": "ticketTable",
      "text": "Chamados em andamento",
      "filter": {
        "status": [
          "Em andamento"
        ]
      },
      "tabTitle": "Em andamento"
    },
    {
      "path": "ticketTable",
      "slug": "ticketTable",
      "text": "Chamados resolvidos",
      "filter": {
        "status": [
          "Resolvido"
        ]
      },
      "tabTitle": "Resolvidos"
    },
    {
      "path": "ticketTable",
      "slug": "ticketTable",
      "text": "Todos os chamados",
      "filter": {},
      "tabTitle": "Todos"
    }
  ],
  "defaultTab": {
    "order": [
      [
        "createdAt",
        "DESC"
      ]
    ],
    "fields": [
      {
        "name": "id",
        "label": "id",
        "sortable": true
      },
      {
        "name": "solicitante",
        "label": "Solicitado por"
      },
      {
        "name": "category",
        "label": "Categoria"
      },
      {
        "name": "status",
        "label": "Status",
        "sortable": true
      },
      {
        "name": "createdAt",
        "type": "date",
        "label": "Criado em",
        "convert": "{{ double_keys_open }} (input) => { return $$moment(input).format('LLL') } {{ double_keys_close }}",
        "sortable": true
      }
    ],
    "params": {
      "includeQueue": false,
      "additionalInfos": [
        {
          "ticketTypeId":  id do ticketType
        }
      ],
      "columnsToSearch": [
        "id",
        "status"
      ],
      "includeSharedGroup": false,
      "includeTicketGoals": false,
      "includeAdditionalInfos": true
    },
    "attributes": [
      "id",
      "body",
      [
        "TicketType.name",
        "type"
      ],
      [
        "ticketRecipient.name",
        "solicitante"
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
      [
        "TicketType.id",
        "ticketTypeId"
      ],
      "priority",
      "description",
      "status",
      "createdAt",
      "updatedAt",
      "isAppointment",
      "scheduleDate",
      "duration"
    ],
    "filterConfig": {
      "hideQueue": true,
      "hideCategory": true,
      "hideCreatedBy": true,
      "hideRecipient": true,
      "hideCreatedByt": true,
      "hideTicketType": true,
      "hideSectorTicket": true,
      "hideSearchByTitle": true,
      "assignedToUserWhere": {},
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
      "statusFromTicketTypes": [  id do ticketType ]
    }
  },
  "ticketAgentOverview": {
    "general": {
      "extraTabsMaxWidth": 70,
      "extraTabsMinWidth": 20,
      "isExtraTabsResize": false,
      "initialExtraTabsWidth": 65
    },
    "listOneParams": {
      "additionalInfos": [
        {
          "ticketTypeId": id do ticketType
        }
      ]
    }
  }
}
```
Para mais informações sobre cada propriedade e para visualizar detalhes sobre elas, consulte o [Workspace](/orgs/byndcloud/qsm/workspace/).


### Criar menu
Por fim, para exibir a tela do workspace no menu lateral ou superior:

  ![Menu](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/menu.png)

1. Acesse /menu.
2. Escolha o tipo de menu e o perfil de usuário que terá acesso. Se você é administrador, selecione o perfil correspondente ou escolha outro conforme necessário.
3. Depois aperte o botão de **Novo**
  ![Criando um menu](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/configurandoMenu.png)

4. Selecione o tipo de menu. Para um menu suspenso, escolha uma categoria; caso contrário, opte por uma página. Neste caso, vamos selecionar uma página.

  ![Escolhendo um tipo de menu](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/tipoMenu.png)

5. Em seguida, crie um menu e configure-o para renderizar o workspace que foi criado.
  ![Editando o menu](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/EditandoMenu.png)

6. Uma aba lateral de edição será aberta, onde você poderá definir o nome do menu, a página (o workspace) e a path do workspace criado, no fim da configuração aperte o botão salva
    ![Salvando menu](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/salvandoMenu.png)

6. Ao final, o menu lateral estará configurado corretamente.
    ![Menu configurado](https://storage.googleapis.com/quoti-docs-pictures/QSM/practice/menuLateral.png)