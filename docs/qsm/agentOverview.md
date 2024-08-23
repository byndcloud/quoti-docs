# Visão geral do Atendente

Este documento fornece uma visão geral da interface do atendente, destacando as principais funcionalidades e configurações disponíveis. 

## Conhecendo a tela do atendente

A interface utilizada para o gerenciamento de tickets é mostrada na imagem abaixo. A seguir, destacamos os principais elementos dessa tela:

1. **Informação Básica de Identificação do Ticket**: Número de identificação do ticket.
2. **Cabeçalho do Ticket**: Área que exibe informações resumidas e ações rápidas.
3. **Aba extras**: Funcionalidades adicionais e complementares ao gerenciamento do ticket.
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
2. **Tipo do Chamado**: Tem prioridade sobre as configurações de workspace e extensão, mas pode ser sobreescrita pela categoria.
3. **Workspace**: Sobrescreve as configurações de extensão, mas pode ser sobrescrito pela categoria ou tipo de chamado.
4. **Extensão**: Configuração com a menor prioridade, pode ser sobrescrita por qualquer uma das opções acima.

### Entendendo o JSON de Configuração

Esse JSON é responsável por toda a configuração avançada de layout presente nessa tela. Com ele, podemos realizar diversas customizações, como:

1. Exibir o formulário de um ticket de acordo com o perfil de um usuário.
2. Exibir um botão para mudar o status de um ticket.
3. Exibir um seletor de usuário para especificar quem será o atendente do ticket.
4. Aplicar uma estilização usando CSS em diversos componentes da tela.

#### Relação entre Propriedades do JSON e Componentes da Tela

Na **Figura 2**, podemos identificar as seguintes propriedades:

- **hooks**: Configura os gatilhos que serão executados quando a página ou o ticket for carregado.
- **mainTab**: Configura o item 1 presente na Figura 1.
- **AgentOverviewHeader**: Configura o item 2 presente na Figura 1.
- **extraTabs**: Configura o item 3 presente na Figura 1.
- **sideBarTabs**: Configura o item 4 presente na Figura 1.
---

Para não tornar essa documentação exaustiva, apresentaremos alguns casos de uso para as configurações de layout, mas todo o JSON Schema pode ser acessado clicando [aqui](https://beyondco.notion.site/JSON-Schema-Configura-es-avan-adas-de-layout-227872a70cdc478ab9067c585b5b1931?pvs=25)
### Configurando gatilhos da página
Os gatilhos da página podem ser configurados pela propriedade **hooks**, conforme mostrado abaixo: 
````javascript
{
    "hooks":{
        "ticketLoaded": "`\{\{` async ()=> { console.log('Esta mensagem será exibida sempre que o ticket for carregado'); } \}\}"
        "created": "\{\{ async ()=> { console.log('Esta mensagem será exibida apenas uma vez quando a página for criada'); } \}\}"    
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
````javascript
{
    "mainTab": {
        "icon": "mdi-folder",
        "showIcon": true,
        "hideCloseBtn": true, // proíbe o usuário de fechar a tab
        "text": "\{\{ 'Processo ' + $ticket.id /}/}", // define o texto que será exibido na tab selecionada
        "iconColor": "red"
  }
}
````

## Configurando o cabeçalho da página
![](https://storage.googleapis.com/quoti-docs-pictures/QSM/agentOverview/header.jpeg?v=2)
**Figura 3**: Configuração da cabeçalho do ticket

````javascript
{
    "title": "\{\{ $ticket?.description /}/}",
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
        "action": "\{\{ (input) => { return (async () => { await $updateTicket({id: $ticket.id, assignedToUser: $me?.id })})() } /}/}",
        "outlined": false,
        "condition": "\{\{ (input) => { return $enableGetForMe && !$ticket.assignedToUser && $ticket.status === 'Pré-cadastrado' } /}/}",
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
        "action": "\{\{ (input) => { return (async () => { await $updateTicket({id: $ticket.id, assignedToUser: null})})() } /}/}",
        "outlined": false,
        "textProp": true,
        "condition": "\{\{ (input) => { return $enableReturnToQueue && $ticket.status === 'Pré-cadastrado' } /}/}",
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
        "action": "\{\{ (input) => { return (async () => { $callWebhook('https://workflow.quoti.cloud/webhook/meu-webhook-teste', {body: {formResponseId: $ticket.ticketTypeFormResponseId} }) })() } /}/}",
        "outlined": false,
        "condition": "\{\{ (input) => { return $ticket?.status === 'Pré-cadastrado' && $ticket.assignedToUser === $me.id } /}/}",
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
As propriedades extraTabs e sidebar utilizam o mesmo JSON Schema. Isso significa que uma configuração definida para uma aba na sidebar pode ser facilmente transferida para uma aba em extraTabs sem necessidade de modificar sua estrutura. Portanto, os exemplos apresentados para extraTabs são igualmente aplicáveis à sidebar, e vice-versa.
Neste tópico, exploraremos algumas as opções e personalizações possíveis tanto para extraTabs quanto para sidebar.
### Detalhes de um ticket
### Eventos de um ticket
### Databases
### Tickets filhos
### SLAs de um ticket
### Aprovações
### Anexos
### Atendente, destinatário e criador do ticket
### Exibir componente personalizado

![]()
**Figura 6**: Configuração da cabeçalho do ticket

 

