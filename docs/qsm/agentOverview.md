# Visão geral do Atendente
Este documento fornece uma visão geral da interface do atendente, destacando as principais funcionalidades e configurações disponíveis. 

## Conhecendo a tela do atendente
A interface utilizada para o gerenciamento de tickets é mostrada na imagem abaixo. A seguir, destacamos os principais elementos dessa tela:

1. **Informação Básica de Identificação do Ticket**: Número de identificação do ticket.
2. **Cabeçalho do Ticket**: Área que exibe informações resumidas e ações rápidas.
3. **Tabs extras**: Funcionalidades adicionais e complementares ao gerenciamento do ticket.
4. **Sidebar**: Navegação lateral com acesso a outras funcionalidades e seções da plataforma.

![Visão Geral do Atendente](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/agentOverview.jpeg)
**Figura 1**: Visão geral do atendente

## Configurações Avançadas de Layout

Para acessar as configurações avançadas de layout, é necessário adicionar o parâmetro de configuração `&c=1` no final da URL da plataforma em seu navegador. 

![Visão Geral do Atendente - Com JSON](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/agentOverview2.jpeg)
**Figura 2**: Configurações avançadas de layout

### Hierarquia de Configurações de Layout

Assim como a configuração do workspace, a configuração da visão do ticket também pode ser atualizada de algumas maneiras, seguindo a seguinte ordem de prioridade: 

1. **Categoria**: Configuração mais prioritária, que sobrescreve todas as outras.
2. **Tipo do Chamado**: Tem prioridade sobre as configurações de workspace e extensão, mas pode ser sobrescrita pela categoria.
3. **Workspace**: Sobrescreve as configurações de extensão, mas pode ser sobrescrito pela categoria ou tipo de chamado.
4. **Extensão**: Configuração com a menor prioridade, pode ser sobrescrita por qualquer uma das opções acima.

### Entendendo o JSON de Configuração

Esse JSON é responsável por toda a configuração avançada de layout presente nessa tela. Com ele, podemos realizar diversas customizações, como:

1. Exibir o formulário de um ticket conforme o perfil de um usuário.
2. Exibir um botão para mudar o status de um ticket.
3. Exibir um seletor de usuário para especificar quem será o atendente do ticket.
4. Aplicar uma estilização usando CSS em diversos componentes da tela.

#### Relação entre Propriedades do JSON e Componentes da Tela

Na **Figura 2**, podemos identificar as seguintes propriedades:

- **hooks**: configura os gatilhos que serão executados quando a página ou o ticket for carregado.
- **mainTab**: configura o item 1 presente na Figura 1.
- **AgentOverviewHeader**: configura o item 2 presente na Figura 1.
- **extraTabs**: configura o item 3 presente na Figura 1.
- **sideBarTabs**: configura o item 4 presente na Figura 1.
---

Para não tornar essa documentação exaustiva, apresentaremos alguns casos de uso para as configurações de layout, mas todo o JSON Schema pode ser acessado clicando [aqui](https://beyondco.notion.site/JSON-Schema-Configura-es-avan-adas-de-layout-227872a70cdc478ab9067c585b5b1931?pvs=25)
### Configurando gatilhos da página
Os gatilhos da página podem ser configurados pela propriedade **hooks**, conforme mostrado abaixo: 
````javascript
{
  "hooks":{
    "ticketLoaded": "{{ double_keys_open }} async ()=> { console.log('Esta mensagem será exibida sempre que o ticket for carregado'); } {{ double_keys_close }}"
    "created": "{{ double_keys_open }} async ()=> { console.log('Esta mensagem será exibida apenas uma vez quando a página for criada'); } {{ double_keys_close }}"    
  }
}
````

## Configurando a tab selecionada

![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/mainTab.jpeg)

**Figura 3**: Configuração da main tab

O layout acima foi gerada com a seguinte configuração presente na propriedade **mainTab**

````javascript
{
  "mainTab": {
    "icon": "mdi-folder", // indica o ícone que será exibido.
    "showIcon": true, // indica se devemos ou não exibir um ícone
    "hideCloseBtn": false, // Indica se devemos permitir que o usuário feche a tab do ticket que está sendo mostrado
    "iconColor": "#757575", // Indica a cor do ícone
  }
}
````
> A lista completa de ícones disponíveis podem ser acessada [aqui](https://pictogrammers.com/library/mdi/).

Veja outra opção de configuração para a tab principal
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/mainTab2.jpeg)
**Figura 4**: Outra configuração da main tab
````javascript
{
  "mainTab": {
    "icon": "mdi-folder",
    "showIcon": true,
    "hideCloseBtn": true, // proíbe o usuário de fechar a tab
    "text": "{{ double_keys_open }} 'Processo ' + $ticket.id {{ double_keys_close }}", // define o texto que será exibido na tab selecionada
    "iconColor": "red"
  }
}
````

## Configurando o cabeçalho da página
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/header.jpeg?v=2)
**Figura 5**: Configuração do cabeçalho do ticket

````javascript
{
  "title": "{{ double_keys_open }} $ticket?.description {{ double_keys_close }}",
  "hideStatus": false,
  "titleCaption": "Processo de n°",
  "hideCreatedAt": false,
  "oldActionButtons": [
    {
      "icon": "mdi-account",
      "text": "Pegar para mim",
      "type": "general",
      "color": "primary",
      "dense": true,
      "action": "{{ double_keys_open }} (input) => { return (async () => { await $updateTicket({id: $ticket.id, assignedToUser: $me?.id })})() } {{ double_keys_close }}",
      "outlined": false,
      "condition": "{{ double_keys_open }} (input) => { return $enableGetForMe && !$ticket.assignedToUser && $ticket.status === 'Pré-cadastrado' } {{ double_keys_close }}",
      "depressed": true,
      "dialogText": "Deseja ser o cadastrador responsável por este proceso?",
      "dialogTitle": "Pegar para mim",
      "aksConfirmation": true
    },
    {
      "icon": "mdi-human-queue",
      "text": "Devolver para a fila",
      "type": "general",
      "color": "primary",
      "dense": true,
      "action": "{{ double_keys_open }} (input) => { return (async () => { await $updateTicket({id: $ticket.id, assignedToUser: null})})() } {{ double_keys_close }}",
      "outlined": false,
      "textProp": true,
      "condition": "{{ double_keys_open }} (input) => { return $enableReturnToQueue && $ticket.status === 'Pré-cadastrado' } {{ double_keys_close }}",
      "depressed": true,
      "dialogText": "Deseja devolver este processo para a fila?",
      "dialogTitle": "Devolver para a fila",
      "aksConfirmation": true
    },
    {
      "text": "Finalizar cadastro",
      "type": "general",
      "color": "primary",
      "dense": true,
      "action": "{{ double_keys_open }} (input) => { return (async () => { $callWebhook('https://workflow.quoti.cloud/webhook/meu-webhook-teste', {body: {formResponseId: $ticket.ticketTypeFormResponseId} }) })() } {{ double_keys_close }}",
      "outlined": false,
      "condition": "{{ double_keys_open }} (input) => { return $ticket?.status === 'Pré-cadastrado' && $ticket.assignedToUser === $me.id } {{ double_keys_close }}",
      "depressed": true,
      "dialogText": "Essa ação irá mudar o status do processo para Ativo, permitindo que outros perfis de usuários consigam tratá-lo. Certeza que deseja seguir com essa ação?",
      "dialogTitle": "Finalizar cadastro",
      "aksConfirmation": true
    }
  ],
  "groupedButtonName": "Realizar ação", // Esse será o nome do botão menu que será exibido, caso a quantidade de botões na tela ultrapasse a propiedade maxVisibleButtons
  "maxVisibleButtons": 3 // indica a quantidade máxima de botões que poderão ser exibidos na tela. Caso ultrapasse essa quantidade, os botões serão agrupados em um único botão do tipo menu
}
````
## Configurando extraTabs e sidebar
As propriedades extraTabs e sidebar utilizam o mesmo JSON Schema. Isso significa que uma configuração definida para uma tab na sidebar pode ser facilmente transferida para uma tab em extraTabs sem necessidade de modificar sua estrutura. Portanto, os exemplos apresentados para extraTabs são igualmente aplicáveis à sidebar e vice-versa.
Neste tópico, exploraremos algumas das opções e personalizações possíveis tanto para extraTabs quanto para sidebar.

As extraTabs e sidebar são configuradas por um json com as seguintes propriedades comuns a todas as customizações:
**slug**: Identifica qual componente será utilizado na redenrização da tab.
**path**: Representa o identificador daquela tab. É essencial cada tab possuir um path único.
**text**: Indica o texto que será exibido na tab da extraTab ou no título da sidebar
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/tabs%20e%20sidebar.jpeg)
**Figura 6**: Configuração das tabs

### Detalhes de um ticket
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/deitals.jpeg)
**Figura 7**: Renderização das informações de um ticket
````javascript
{
  "path": "detailsOnly",
  "slug": "detailsOnly",
  "text": "Detalhes",
  "TicketDetails": {
      "Header": {
        "hideName": false,
        "hideText": false
      },
      "saveBtn": "Salvar informações",
      "hideBody": false,
      "userCanEditForm": "{{ double_keys_open }} $ticket.status === 'Pré-cadastrado' ? $ticket.assignedToUser === $me.id : $ticket.status !== 'Arquivado' && $me.userProfileId !== 100027 {{ double_keys_close }}",
      "saveFormsCallback": "{{ double_keys_open }} () => { $fetchTicket() } {{ double_keys_close }}",
      "hideTicketPrimaryInfos": false
  }
}
````
---
### Eventos de um ticket
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/history.jpeg)
**Figura 8**: Eventos de um ticket
````javascript
{
  "icon": "mdi-history",
  "path": "historyAllEventsl",
  "slug": "history",
  "TicketActions": {
    "title": "Todos os eventos",
    "filter": {
      "or": [
        {
          "ticketId": "{{ double_keys_open }} $ticket.id {{ double_keys_close }}"
        },
        {
          "$ticketActions.parent_ticket_id$": "{{ double_keys_open }} $ticket.id {{ double_keys_close }}"
        }
      ]
    },
    "params": {
      "ticketAttributes": [
        "id",
        "parentTicketId"
      ],
      "ticketAdditionalInfos": [
        {
          "ticketTypeId": 100047
        },
        {
          "ticketTypeId": 100049
        }
      ]
    },
    "contextId": "ticketActionProcesso",
    "allowWrite": "{{ double_keys_open }} $userCanCreateAction {{ double_keys_close }}",
    "ticketKind": "processos",
    "cardConfigs": {
      "type": {},
      "default": {
        "body": "(ticketActionProcesso){{ double_keys_open }} action => '<h3>' +  $getTitle(action) + ' | ticketId: ' +  action.ticketId +  '</h3>' + $getBody(action) {{ double_keys_close }}",
        "footer": "{{ double_keys_open }} action => action.ticketActionCreator.name + ' • ' + $$moment(action.createdAt).format('lll') {{ double_keys_close }}",
        "hideTitle": true,
        "hideSubtitle": true
      }
    },
    "quillOptions": {
      "modules": {
        "toolbar": [
          [
            "bold",
            "italic",
            "underline",
            "strike",
            {
              "header": [
                1,
                2,
                3,
                4,
                5,
                6,
                false
              ]
            },
            {
              "color": []
            },
            {
              "background": []
            }
          ]
        ]
      }
    },
    "actionButtons": [
      {
        "icon": "mdi-message-lock",
        "text": "Criar nota privada",
        "dense": true,
        "action": "(ticketActionProcesso){{ double_keys_open }} () => $sendMessage('privateMessage') {{ double_keys_close }}",
        "loading": "(ticketActionProcesso){{ double_keys_open }} () => $sendingMessage {{ double_keys_close }}",
        "outlined": true,
        "condition": true,
        "depressed": true
      }
    ],
    "createBtnIcon": "mdi-whatsapp",
    "ticketCreatedHeader": "Criação do processo",
    "ticketUpdatedHeader": "Atualização do processo"
  }
}
````
---
### Databases
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/databases.jpeg)
**Figura 9**: Exibindo databases
````javascript
{
  "path": "databaseParts",
  "slug": "database",
  "text": "Partes",
  "databaseProps": {
    "table": "processos_envolvidos",
    "where": {
      "processo_id": "{{ double_keys_open }} $ticket.id {{ double_keys_close }}"
    },
    "formId": 100314,
    "params": {
      "include": [
        {
          "multiple": false,
          "tableName": "processos_envolvidos_advogados",
          "attributes": [
            "nome",
            "adv_num_oab"
          ],
          "foreignKey": "advogado_principal"
        }
      ]
    },
    "headers": [
      {
        "name": "nome",
        "label": "Nome",
        "sortable": true
      },
      {
        "name": "cpf",
        "label": "CPF/CNPJ",
        "sortable": true
      },
      {
        "name": "polo",
        "label": "Polo",
        "sortable": true
      },
      {
        "name": "tipo_pessoa",
        "label": "Tipo",
        "sortable": true
      },
      {
        "name": "ProcessosEnvolvidosAdvogados.nome",
        "label": "Nome do advogado",
        "sortable": true
      },
      {
        "name": "ProcessosEnvolvidosAdvogados.adv_num_oab",
        "label": "Nº da OAB",
        "sortable": true
      },
      {
        "name": "outros_advogados",
        "label": "Possui outros advogados",
        "convert": "{{ double_keys_open }} (input) => { return input? 'Sim' : 'Não'} {{ double_keys_close }}",
        "sortable": true
      },
      {
        "name": "edit",
        "label": "Editar",
        "sortable": false
      },
      {
        "name": "delete",
        "label": "Excluir ",
        "sortable": false
      }
    ],
    "workflowVars": "{{ double_keys_open }} [{'name': 'ticket','value': $ticket}] {{ double_keys_close }}",
    "tableOriginIsView": true
  },
  "databaseEvents": {}
}
````
---
### Tickets filhos
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/ticketsChildren2.jpeg)
**Figura 10**: Exibindo ticket associados
````javascript
{
  "path": "ticketChildren",
  "slug": "ticketChildren",
  "text": "Aditivos",
  "order": [
    [
      "created_at",
      "desc"
    ]
  ],
  "fields": [
    {
      "name": "createdAt",
      "label": "Criado em",
      "convert": "{{ double_keys_open }} (input) => { return $$moment(input).format('DD/MM/YYYY') } {{ double_keys_close }}",
      "sortable": true
    },
    {
      "name": "ticketType19AdditionalInfo.tipo_de_aditivo",
      "label": "Tipo",
      "sortable": true
    },
    {
      "name": "ticketType19AdditionalInfo.data_de_inicio_aditivo",
      "label": "Inicio de Vigência",
      "convert": "{{ double_keys_open }} (input) => { return input ? $$moment(input).format('DD/MM/YYYY') : '-' } {{ double_keys_close }}"
    },
    {
      "name": "ticketType19AdditionalInfo.nova_data_de_termino",
      "label": "Fim de Vigência",
      "convert": "{{ double_keys_open }} (input) => { return input ? $$moment(input).format('DD/MM/YYYY') : '-' } {{ double_keys_close }}"
    },
    {
      "name": "ticketType19AdditionalInfo.formalizacao_aditivo_prazo",
      "label": "Formalização",
      "convert": "{{ double_keys_open }} (input) => { return input ? input : '-' } {{ double_keys_close }}",
      "sortable": true
    },
    {
      "name": "ticketType19AdditionalInfo.valor_total_do_contrato",
      "label": "Valor original",
      "convert": "{{ double_keys_open }} (input) => { return new Intl.NumberFormat('pt-BR', {  style: 'currency',currency: 'BRL' }).format(input/100) } {{ double_keys_close }}"
    },
    {
      "name": "ticketType19AdditionalInfo.valor_total_com_reajuste",
      "label": "Valor Reajustado",
      "convert": "{{ double_keys_open }} (input) => { return new Intl.NumberFormat('pt-BR', {  style: 'currency',currency: 'BRL' }).format(input/100) } {{ double_keys_close }}"
    }
  ],
  "filter": {
    "$TicketType.id$": 19
  },
  "params": {
    "includeQueue": false,
    "additionalInfos": [
      {
        "ticketTypeId": 19
      }
    ],
    "includeSharedGroup": false,
    "includeTicketGoals": false,
    "includeAdditionalInfos": false,
    "includeTicketTypeAdditionalInfos": false
  },
  "createTicket": true,
  "filterConfig": {
    "hideQueue": true,
    "hideStatus": true,
    "hideRecipient": true,
    "hideSectorTicket": true,
    "disableTicketType": true,
    "hideAssignedToUser": true
  },
  "createTicketCatalog": "assetsChanges"
}
````
---
### SLAs de um ticket
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/slas.jpeg)
**Figura 11**: SLAs presente no ticket
````javascript
{
  "path": "slas",
  "slug": "slas",
  "text": "SLAs"
}
````
---
### Aprovações
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/aprovacao.jpeg)
**Figura 12**: Aprovações presente no ticket
````javascript
{
  "path": "approvals",
  "slug": "approvals",
  "text": "Aprovações"
}

````
---
### Anexos
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/anexos.jpeg)
**Figura 13**: Anexos presente no ticket
````javascript
{
  "icon": "mdi-attachment",
  "path": "attachments",
  "slug": "attachments",
  "text": "Anexos"
}
````
---
### Atendente, interessado e criador do ticket
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/recordInfo.jpeg)
**Figura 14**: Exibindo atendente, interessado e criado do ticket
````javascript
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
}
````
---
### Exibir componente personalizado
Também é possível renderizar diversos outros componentes do QSM e do próprio Quoti, inclusive renderizar extensões inteiras nas extraTabs e sidebar.
A figura 15, mostra um exemplo da renderização do componente ticketPrimaryInfos, utilizando a propriedade `slug = slot`.
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/slot.jpeg)
**Figura 15**: Exibindo componentes personalizados
````javascript
{
  "path": "meuSlotTeste",
  "slug": "slot",
  "slot": {
    "children": [
      {
        "slug": "ticketPrimaryInfos",
        "props": {
          "class": "mt-2 ml-4",
          "ticket": "{{ double_keys_open }} $ticket {{ double_keys_close }}",
          "configs": {
            "infos": [
              {
                "slug": "text",
                "text": "Status",
                "label": "Status",
                "modifier": {
                  "value": "{{ double_keys_open }} $ticket?.status || 'Não identificado' {{ double_keys_close }}"
                },
                "modifierSlug": "text"
              },
              {
                "slug": "text",
                "text": "Cliente",
                "label": "Cliente",
                "modifier": {
                  "value": "Empresa Teste"
                },
                "modifierSlug": "text"
              },
              {
                "slug": "text",
                "text": "Data de cadastro",
                "label": "Data de cadastro",
                "modifier": {
                  "value": "{{ double_keys_open }} $$moment($ticket?.createdAt).format('DD/MM/YYYY [às] HH:mm') || 'Não identificado' {{ double_keys_close }}"
                },
                "modifierSlug": "text"
              },
              {
                "slug": "text",
                "text": "Pré cadastro",
                "label": "Pré cadastro",
                "modifier": {
                  "value": "{{ double_keys_open }} $ticket?.ticketType100047AdditionalInfo?.Status_pre_cadastro || 'Não identificado' {{ double_keys_close }}"
                },
                "modifierSlug": "text"
              }
            ]
          }
        }
      }
    ]
  }
}

````
Observe que através da propriedade `slot.children`, conseguimos especificar uma lista de componentes que serão renderizado nas extraTabs/sidebar. A seguir, veremos a lista de possíveis valores possível para a propriedade slug presente no objeto children.

- **Informações do ticket**: representado pelo slug **ticketPrimaryInfos** e indicado para representar qualquer informação do ticket, podendo, inclusive, alterá-la. Exemplo: exibir o status e o responsável pelo chamado.
- **Botões de ações**: representado pelo slug **actionButtons** e indicado para realizar ações dentro ou fora do ticket. Exemplo: renderizar um botão para mudar o status do ticket ou enviar um e-mail para o cliente.
- **Seletor de categorias**: representado pelo slug **categorySelector** e indicado para selecionar uma categoria de um chamado. Exemplo: permitir que o usuário mude a categoria do chamado.
- **Extensões**: representado pelo slug **qtExtension** e indicado para renderizar uma extensão instalada na organização. Exemplo: renderizar a extensão de gerenciamento global dos SLAs.
- **Tabela simples**: representado pelo slug **csmTable** e indicado para tabelas simples que não possuem relação com nenhum database ou view do quoti. Exemplo: mostrar tabela de IMC com colunas Valor e Resultado. Na maioria dos casos, é mais aconselhado criar um database.
- **Tabela de tickets**: representado pelo slug **ticketTable** e indicado para renderizar chamados sobre qualquer filtro. Exemplo: mostrar chamados da categoria X que estão com atendente Y.
- **Database**: representado pelo slug **qtDatabase** e indicado para renderizar qualquer database. Exemplo: mostrar databases que possuem uma relação semântica com o chamado.

Essa lista está em contínua atualização, contate-nos para saber todos os valores possíveis ou caso tenha uma sugestão sobre qual componente desejaria vê-lo nessa lista.



 

