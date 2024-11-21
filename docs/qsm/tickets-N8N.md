## Objetivo

Nesta seção, vamos aprender como usar o QSM + N8n para criar tickets automaticamente e atualizar eventos relacionados aos tickets criados.


### Criando hooks para eventos do chamado

É necessário criar um **hooks** que servirá como base para chamar o n8n quando um chamado for criado.

1. Acesse a página de hooks disponíveis em `/hooks` e clique no botão **Novo**.

   ![Acessando um hooks](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/createHooks.png)

2. Preencha os campos conforme descrito abaixo:

  2.1. **Tipo de Hook**: Indique o tipo de recurso que o webhook irá monitorar. No nosso caso, será um *modelo* (model).

  2.2. **Nome do Recurso (Base de Dados)**: Informe o nome da base de dados que o webhook irá escutar. No exemplo, será o *TicketUserActions*, pois se refere a eventos de tickets que serão atualizados.

  2.3. **Handler**: Este campo corresponde ao endpoint configurado no nó *Webhook* do n8n (que veremos em seguida). Ele define a URL para onde as solicitações serão enviadas.

  2.4. **Eventos**: Aqui, você define quando o nó será acionado. As opções incluem: após a criação, após a edição, antes da criação, antes da edição, entre outras.

  2.5. **Condição**: Neste campo, você especifica a condição para a execução do nó. Por exemplo, pode ser configurado para ser acionado apenas para um ticket específico ou apenas para uma determinada categoria. Caso não seja necessário envie como "null"

  ```javascript
  {
  "all": [
    {
      "fact": "requestData",
      "path": "$.body.data.ticket.ticketType",
      "value": [
        "100000",
      ],
      "operator": "in",
      "description": "Only tickets of type different than 3, 4 and 5 (assets and chats)"
    }
  ]
  }
  ```


  ![Preenchendo campos](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/createdHooks.png)

Conforme ilustrado na imagem acima, este hook será acionado sempre que um chamado for criado. (Nota: Na imagem, o nome ticket é utilizado, mas é importante lembrar que, nesse contexto, todo chamado é considerado um ticket.) Os dados do chamado que sofreu a alteração serão enviados automaticamente.

Após criado, deixe seu nó ativo:
  ![Nó ativo](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/hooksActive.png)

### Criar Workflow para Criação de Tickets

1. **Acesse a página de workflows**
   - Acesse a interface de workflows no sistema.

   ![Acesse o workflow](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/openWorkflow.png)

2. **Crie um novo workflow**
   - Clique em "Criar novo workflow".

   ![Criar workflow](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/createWorkflow.png)

3. **Adicione um nó trigger**
   - Crie um nó *trigger* chamado "Em X horas" (caso deseje que o ticket seja criado automaticamente a cada intervalo de tempo).

   ![Nó Trigger](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/noTrigger.png)


4. **Adicione um nó Javascript para montar os dados da requisição**
   - Crie um nó *Javascript* que irá montar o objeto de dados para o ticket a ser criado. O código abaixo é um exemplo de como esse nó pode ser configurado:

  ```javascript
   const ticketToCreate = {
      "description": "chamado teste", // Descrição do chamado
       "ticketStatus": "pendente", // Status do chamado
       "body": "",
       "attachments": [],
       "assignedTo": 90, // Fila do chamado:
       "recipient": 601, // Responsável pelo chamado
       "createdBy": 601, // Criado por
       "categoryId": 100001, // Categoria do chamado
       "ticketTypeId": 100000, // Tipo do chamado
       "ticketTypeAdditionalInfo": {
          "report": "Empresa 123",
          "anexo": []
        },
        "categoryAdditionalInfo": {
          "tipo_do_software": "Setor xyz",
          "problema_principal": "Projeto abc"
        },
       
   }
   return {json: {ticketToCreate}}

  ```
  ![Nó de javascript](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/noJavascript.png)

  **4.1. Como obter os IDs correspondentes:**

    - **assignedTo:** Para encontrar o ID da fila, acesse [https://quoti.cloud/{{organização}}/groups/fila?groupTypeName=fila&hideChangeStatus=true](https://quoti.cloud/{{organização}}/groups/fila?groupTypeName=fila&hideChangeStatus=true&). 

    - **recipient:** Para encontrar o ID do criador, acesse [https://quoti.cloud/{{organização}}/users](https://quoti.cloud/{{organização}}/users). Abra o DevTools (pressione F12 no navegador), clique sobre o usuário desejado e, em seguida, será gerada uma requisição com o respectivo ID.
      
    ![Exemplo de ID do usuário](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/idUser.png)

    - **createdBy:** Para encontrar o ID do criador, acesse [https://quoti.cloud/{{organização}}/users](https://quoti.cloud/{{organização}}/users). Abra o DevTools (pressione F12 no navegador), clique sobre o usuário desejado e, em seguida, será gerada uma requisição com o respectivo ID.
      
    ![Exemplo de ID do usuário](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/idUser.png)

    - **categoryId:** Acesse [https://quoti.cloud/{{organização}}/app/categories](https://quoti.cloud/{{organização}}/app/categories) para obter o ID da categoria.

    - **ticketTypeAdditionalInfo:** Acesse a página */ticketTypes* para visualizar os campos do formulário do tipo de caso. Clique no botão **Editar** (ícone lápis) ao lado do tipo de caso desejado. Em seguida, role até o final da janela de diálogo para encontrar os campos do formulário associados a esse tipo de caso.

    ![ticketTypes](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/CamposTicketTypes.png)

    ![Formtickets](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/camposTickets.png)

    - **categoryAdditionalInfo:** Acesse a página */categories* para visualizar os campos do formulário da categoria. Clique no botão **Editar** (ícone lápis) ao lado do tipo de caso desejado. Em seguida, role até o final da janela de diálogo para encontrar os campos do formulário associados a essa categoria.

    ![ticketTypes](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/camposCategories.png)

    ![formTickets](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/campoCategories.png)

    - **ticketTypeId:** Acesse [https://quoti.cloud/{{organização}}/app/ticketTypes](https://quoti.cloud/{{organização}}/app/ticketTypes) para obter o ID do tipo de caso.


5. **Adicione um nó HTTP para enviar a requisição**
  - Crie um nó **HTTP** com o method POST, endpoint: *https://api.csm.quoti.cloud/api/v1/{{organização}}/tickets*. Passe o objeto montado no nó anterior para que o ticket seja criado no sistema.
  - Para obter o BearerStatic, acesse a página de **Service Account** da sua organização no seguinte [https://quoti.cloud/{{organização}}/serviceaccounts](https://quoti.cloud/{{organização}}/serviceaccounts) e copie o token do seu ambiente.

  exemplo do nó (copie e cole no seu workflow):

  ```javascript
  {
    "meta": {
      "instanceId": "994f6ec3e5b18860fe331bff83e7ae8bf524205c4d1007723e1059e43a78ea7e"
    },
    "nodes": [
      {
        "parameters": {
          "method": "POST",
          "url": "=https://api.csm.quoti.cloud/api/v1/{{org}}/tickets",
          "sendHeaders": true,
          "headerParameters": {
            "parameters": [
              {
                "name": "=BearerStatic",
                "value": "=bearerStatic"
              }
            ]
          },
          "sendBody": true,
          "specifyBody": "json",
        "jsonBody": "json criado no nó anterior",
          "options": {}
        },
        "id": "ba0ec7a5-1056-4b73-b16b-dcff01c584ae",
        "name": "Cria chamados",
        "type": "n8n-nodes-base.httpRequest",
        "typeVersion": 4.1,
        "position": [
          800,
          280
        ]
      }
    ],
    "connections": {}
  }
  ```


***Agora vamos criar um exemplo para enviar uma mensagem no eventos quando o chamado for criado***


6. Visualização do workflow criado
  ![nó http](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/exempleCreateTicket.png)


### Crie um workflow para eventos do chamado

Após configurar o `hooks`, o próximo passo é criar um **workflow**, siga o  é reproduzido quando um **chamado** sofrer alteração.

1. Acesse novamente a página de workflows.

2. Crie um novo **workflow**, como descrito nos passos anteriores.


3. Adicione um nó **Webhook** que irá receber os dados do hook configurado. O endpoint configurado no hook (campo **handler**) será utilizado aqui.
     ![nó webhook](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/webhooksEvents.png)

4. Adicione um nó **javascript** para montar a estrutura da **mensagem** que será enviada para o evento do ticket. Abaixo está um exemplo de como configurar o nó:

  ```javascript

  const ticket = {}; // Replace with actual ticket object
  const createEvents = {
      "details": "<p>criar mensaagem</p>", //mensagem para o ticket criado
      "ticketId": ticket.ticketId, //id do ticket criado (vindo do webhook)
      "data": "{}",
      "type": "message"
  }
  return {json: {createEvents}}
  ```
   ![nó javascript](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/noJavascriptEvents.png)

5. Adicione um nó **HTTP** para enviar mensagem  configurada no nó javascript para o eventos do ticket.
exemplo para nó http:
  - Para obter o BearerStatic, acesse a página de **Service Account** da sua organização no seguinte [https://quoti.cloud/{{organização}}/serviceaccounts](https://quoti.cloud/{{organização}}/serviceaccounts) e copie o token do seu ambiente.
  ```javascript

      {
      "meta": {
        "instanceId": "994f6ec3e5b18860fe331bff83e7ae8bf524205c4d1007723e1059e43a78ea7e"
      },
      "nodes": [
        {
          "parameters": {
            "method": "POST",
            "url": " https://api.csm.quoti.cloud/api/v1/beyondschool/ticket-user-actions",
            "sendHeaders": true,
            "headerParameters": {
              "parameters": [
                 {
                  "name": "=BearerStatic",
                  "value": "=bearerStatic"
                }
              ]
            },
            "sendBody": true,
            "specifyBody": "json",
            "jsonBody": "json criado no nó anterior",
            "options": {}
          },
          "id": "da02a560-ad89-408d-b52d-0fc85bcaa3fc",
          "name": "HTTP Request",
          "type": "n8n-nodes-base.httpRequest",
          "typeVersion": 4.1,
          "position": [
            1100,
            460
          ]
        }
      ],
      "connections": {}
    }
  ```

6. Visualização do workflow criado
  ![nó http](https://storage.googleapis.com/quoti-docs-pictures/QSM/hooksN8N/exempleTicketUserActions.png)