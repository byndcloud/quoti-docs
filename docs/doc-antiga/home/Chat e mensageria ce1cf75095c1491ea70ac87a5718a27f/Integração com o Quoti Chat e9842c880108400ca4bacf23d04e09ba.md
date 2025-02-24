# Integração com o Quoti Chat

!!! warning "Documentação descontinuada"
    Esta página faz parte de uma versão descontinuada da documentação. Está presente neste acervo e neste formato enquanto seu conteúdo não é inteiramente migrado para as demais sessões desta documentação.





## Começo rápido

Caso você tenha a necessidade de integrar um canal de comunicação (chat) externo à Quoti Platform, você tem a opção de realizar essa integração através das APIs públicas do Quoti.

### Integração com Live Chat externo

![Quoti Chat Integration Flow.drawio.png](Quoti_Chat_Integration_Flow.drawio.png)

Considerando um cenário em que você possui um *Live Chat* desenvolvido fora da Plataforma Quoti e precisa que as mensagens enviadas através do *Live Chat* por um cliente sejam enviadas para um atendente humano usuário do Quoti Chat, você terá que realizar as seguintes etapas:

1. Obter uma Conta de Serviço para realizar as chamadas à API do Quoti:
    1. Acesse `https://quoti.cloud/{organization}/serviceaccounts`
    2. Clique no botão para criar uma nova Service Account
    3. Em todas as chamadas às APIs utilize no `headers` da requisição `"BearerStatic {serviceAccountToken}"`

---

1. Cadastrar um novo usuário com as informações do Cliente, através da chamada à API:
    
    
    ```bash
    curl -X POST "https://api.quoti.cloud/api/v1/**{organization}**/users/save" -H "accept: */*" -H "BearerStatic: **{serviceAccountToken}**" -H "Content-Type: application/json" -d "{\"userProfileId\":**{userProfileId}**,\"name\":\"**{name}**\",\"user\":\"**{userIdentifier}**\",\"registered\":true}"
    ```
    
    1. {organization} - o Slug da sua organização no Quoti
    2. {serviceAccountToken} - o token da conta de serviço, criado na Etapa 1
    3. {userProfileId} - Id do perfil do usuário que você está criando. Para descobrir os perfis existentes na sua organização e verificar o ID numérico deles, acesse: `https://quoti.cloud/{organization}/userProfiles`
    4. {name} - Nome do usuário que será criado
    5. {userIdentifier} - identificador único do usuário alfanumérico e que pode ser escolhido arbitrariamente pelo desenvolvedor

---

1. Cadastrar uma nova sala (Chat Room), através da chamada à API:
    
    ```bash
    curl -X POST "https://api.quoti.cloud/api/v1/**{organization}**/chat/rooms" -H "accept: application/json" -H "BearerStatic: **{serviceAccountToken}**" -H "Content-Type: application/json" -d "{\"members\":{\"users\":[{\"id\":**{idDoCliente}**,\"name\":\"**{nomeDoCliente}**\"}],\"groups\":[{\"id\":**{filaId}** }]},\"name\":\"**{roomName}**\",\"roomNameRule\":\"asDefined\",\"webhookUrl\":\"{webhookUrl}\"}"
    ```
    
    1. {organization} - o Slug da sua organização no Quoti
    2. {serviceAccountToken} - o token da conta de serviço, criado na Etapa 1
    3. {idDoCliente} - ID do usuário, criado na [Etapa 2](Integrac%CC%A7a%CC%83o%20com%20o%20Quoti%20Chat%20e9842c880108400ca4bacf23d04e09ba.md)
    4. {nomeDoCliente} - nome do cliente que será exibido para o Agente no Quoti Chat, opcional
    5. {filaId} - Id numérico do grupo do tipo fila de atendimento para o qual será direcionado a nova sala. [Como localizar](Integrac%CC%A7a%CC%83o%20com%20o%20Quoti%20Chat%20e9842c880108400ca4bacf23d04e09ba.md)
    6. {roomName} - Nome da sala que será criada e exibida para o Agente
    7. {webhookUrl} - Link do webhook do seu *Live chat*. Esse Webhook será chamado toda vez que o Atendente realizar uma operação no Quoti Chat, como: enviar uma nova mensagem, aceitar uma conversa ou finalizar uma conversa. Deve retornar HTTP status CODE 200.

---

1. Para cada nova mensagem enviada pelo Cliente através do *Live Chat* você deverá chamar a seguinte API:
    
    ```bash
    curl -X POST "https://api.quoti.cloud/api/v1/{organization}/chat/messages" -H "accept: application/json" -H "BearerStatic: {serviceAccountToken}" -H "Content-Type: application/json" -d "{\"text\":\"{texto}\",\"chatId\":\"{chatId}\",\"authorId\":{authorId},\"recipientId\":0}"
    ```
    
    1. {serviceAccountToken} - o token da conta de serviço, criado na Etapa 1
    2. {organization} - o Slug da sua organização no Quoti
    3. {authorId} - ID do usuário, criado na [Etapa 2](Integrac%CC%A7a%CC%83o%20com%20o%20Quoti%20Chat%20e9842c880108400ca4bacf23d04e09ba.md)
    4. {chatId} - Chat Room ID, criado na [Etapa 3](Integrac%CC%A7a%CC%83o%20com%20o%20Quoti%20Chat%20e9842c880108400ca4bacf23d04e09ba.md)
    5. {texto} - Mensagem enviada pelo Cliente que deverá ser repassada para o Quoti Chat

### Integração com Live Chat externo + Bot para transbordo humano

![Quoti Chat Integration Flow.drawio (1).png](Integrac%CC%A7a%CC%83o%20com%20o%20Quoti%20Chat%20e9842c880108400ca4bacf23d04e09ba/Quoti_Chat_Integration_Flow.drawio_(1).png)

Considerando um cenário em que você possui um *Live Chat* desenvolvido fora da Plataforma Quoti que possui um *bot* de atendimento e precisa que a partir de um determinado momento as interações sejam transferidas para um atendente humano usuário do Quoti Chat (transbordo humano), você terá que realizar as seguintes etapas:

---

1. Criar um novo usuário através da interface com o Perfil "Bot”:
    1. Acesse o site `https://quoti.cloud/{organization}/user`
    2. Clique no botão "+" no canto inferior direito:
        
        ![Captura de Tela 2022-08-19 às 08.59.50.png](Captura_de_Tela_2022-08-19_as_08.59.50.png)
        
    3. Clique na opção "CRIRAR USUÁRIO":
        
        ![Captura de Tela 2022-08-19 às 09.00.26.png](Captura_de_Tela_2022-08-19_as_09.00.26.png)
        
    4. Preencha as informações:
        1. Nome: Informe o nome do seu Bot
        2. Login: Utilize um identificador único de sua preferência, para grande maioria dos cenários essa informação não será utilizada novamente.
        3. Perfil: Selecione a opção "Bot". Caso ela não exista entre em contato com o [suporte](mailto:suporte@beyondcompany.com.br).
        4. Email: Cadastre um email válido.

---

1. Cadastrar um novo usuário com as informações do Cliente, através da chamada à API:

---

1. Cadastrar uma nova sala (Chat Room), através da chamada à API:
    
    ```bash
    curl -X POST "https://api.quoti.cloud/api/v1/**{organization}**/chat/rooms" -H "accept: application/json" -H "BearerStatic: **{serviceAccountToken}**" -H "Content-Type: application/json" -d "{\"members\":{\"users\":[{\"id\":**{idDoCliente}**,\"name\":\"**{nomeDoCliente}**\"},{\"id\":**{idDoBot}**,\"name\":\"**{nomeDoBot}**\"}]},\"name\":\"**{roomName}**\",\"roomNameRule\":\"asDefined\",\"webhook\":\"**{webhookUrl}**\"}"
    ```
    
    1. {organization} - o Slug da sua organização no Quoti
    2. {serviceAccountToken} - o token da conta de serviço, criado na Etapa 1
    3. {idDoCliente} - ID do usuário, criado na Etapa 3
    4. {nomeDoCliente} - nome do cliente que será exibido para o Agente no Quoti Chat, opcional
    5. {idDoBot} - ID do bot, criado na Etapa 2. [Como encontrar o ID do bot](Integrac%CC%A7a%CC%83o%20com%20o%20Quoti%20Chat%20e9842c880108400ca4bacf23d04e09ba.md)
    6. {nomeDoBot} - nome do Bot que será exibido para o Agente no Quoti Chat, opcional
    7. {roomName} - Nome da sala que será criada e exibida para o Agente posteriormente
    8. {webhookUrl} - Link do webhook do seu *Live chat*. Esse Webhook será chamado toda vez que o Atendente realizar uma operação no Quoti Chat, como: enviar uma nova mensagem, aceitar uma conversa ou finalizar uma conversa. Será realizada uma requisição do tipo POST e ele deve retornar HTTP status CODE 200.

---

1. Para garantir que o histórico de mensagens trocadas pelo Cliente e o Bot estejam disponíveis para o Atendente após o transbordo, você deverá chamar a seguinte API para cada nova mensagem enviada pelo Cliente ou Bot através do *Live Chat*:
    
    ```bash
    curl -X POST "https://api.quoti.cloud/api/v1/**{organization}**/chat/messages" -H "accept: application/json" -H "BearerStatic:  **{serviceAccountToken}**" -H "Content-Type: application/json" -d "{\"text\":\"**{messageText}**\",\"chatId\":\"**{roomId}**\",\"authorId\":**{authorId}**,\"recipientId\":0}"
    ```
    
    1. {organization} - o Slug da sua organização no Quoti
    2. {serviceAccountToken} - o token da conta de serviço, criado na Etapa 1
    3. {messageText} - O texto da mensagem enviada pelo Cliente ou Bot
    4. {roomId} - ID da sala de conversa criada na Etapa 4
    5. {authorId} - o ID do usuário autor da mensagem (Bot ou Cliente) criados na Etapa 2 e Etapa 3

---

1. Quando você quiser realizar o transbordo da conversa iniciada pelo Bot para uma fila de atendentes humanos (usuários do Quoti Chat), você deverá realizar a seguinte chamada à API:
    
    ```bash
    curl -X POST "https://api.quoti.cloud/api/v1/**{organization}**/chat/rooms/assoc" -H "accept: application/json" -H "BearerStatic: **{serviceAccountToken}**" -H "Content-Type: application/json" -d "{\"roomId\":\"**{roomId}**\",\"groupsIds\":[**{filaId}**],\"usersIds\":[]}"
    ```
    
    1. {organization} - o Slug da sua organização no Quoti
    2. {serviceAccountToken} - o token da conta de serviço, criado na Etapa 1
    3. {roomId} - ID da sala de conversa criada na Etapa 4
    4. {filaId} - Id numérico do grupo do tipo fila de atendimento para o qual será direcionado a nova sala. [Como localizar](Integrac%CC%A7a%CC%83o%20com%20o%20Quoti%20Chat%20e9842c880108400ca4bacf23d04e09ba.md)

---

1. Caso você queira fechar uma conversa diretamente pelo seu bot (marcando como resolvido), você deverá realizar a seguinte chamada à API:
    
    ```bash
    curl -X POST "https://api.quoti.cloud/api/v1/**{organization}**/chat/rooms/**{roomId}**" -H "accept: application/json" -H "BearerStatic: **{serviceAccountToken}**" -H "Content-Type: application/json" -d "{\"chatStatusId\": 100002}"
    ```
    
    1. {organization} - o Slug da sua organização no Quoti
    2. {roomId} - ID da sala de conversa criada na Etapa 4

## Troubleshooting

- Como gerenciar os grupos de atendimentos/filas?
    
    Você pode gerenciar os grupos de atendimentos de duas formas:
    
    1) Através da API do Quoti: https://api.quoti.cloud/api-explorer/#/Groups
    
    2) Através da Interface de Gerenciamento de Grupos: https://quoti.cloud/{organization}/groups/fila
    
- As salas não aparecem para o meu usuário
    
    Verifique se o seu usuário ou o usuário Atendente que necessita visualizar o chat está dentro do grupo/fila como Membro. Caso não esteja, ao ser incluído você terá acesso à todas as salas do grupo/fila. Para saber como gerenciar grupos/filas [clique aqui](Integrac%CC%A7a%CC%83o%20com%20o%20Quoti%20Chat%20e9842c880108400ca4bacf23d04e09ba.md)
    
- Como descobrir o ID de um usuário no Quoti?
    
    A maneira mais simples para se descobrir o ID de um usuário é através da chamada à API que lista usuários: https://api.quoti.cloud/api-explorer/#/Users/get__orgSlug__users_list
    
- Como testar as APIs do Quoti?
    
    Você pode testar as APIs do Quoti na documentação interativa disponível em: [https://api.quoti.cloud/api-explorer/#/](https://api.quoti.cloud/api-explorer/#/)
    
- Quais outros parâmetros ou atributos eu posso enviar nas requisições às APIs do Quoti?
    
    Você pode visualizar todos os parâmetros disponíveis para as APIs do Quoti na documentação interativa disponível em: [https://api.quoti.cloud/api-explorer/#/](https://api.quoti.cloud/api-explorer/#/)
    
- Como faço para não receber chamadas no meu webhook em mensagens já enviadas pelo meu bot?
    
    Neste caso, conforme a documentação da [API de envio de uma nova mensagem](https://api.quoti.cloud/api-explorer/#/Chat/post__orgSlug__chat_messages), você pode passar um *query parameter* chamado “ignoreWebhook=true” na URL da API. Exemplo:
    
    ```bash
    curl -X POST "https://api.quoti.cloud/api/v1/**{organization}**/chat/messages?**ignoreWebhook=true**"
    ```
    
- Como posso passar dados adicionais na criação de uma sala (*chat room*)?
    
    Para muitas integrações se faz necessário que seja salvo junto à sala de conversa (chat room) informações extras que permitam uma melhor parametrização e identificação de atributos específicos de cada sala nos pontos de integração. No Quoti Chat isso pode ser feito adicionando o parâmetro `jsonData` no corpo (*body*) da [requisição POST de criação de uma nova sala (chat room)](Integrac%CC%A7a%CC%83o%20com%20o%20Quoti%20Chat%20e9842c880108400ca4bacf23d04e09ba.md):
    
    ```json
    {
    "jsonData": {
    		"string": "abc",
    		"boolean": false,
    		"number": 12345,
    		"object": {},
    		"array": []
    	}
    }
    ```
    
    Após a definição desses dados, eles passarão a ser retornados nas APIs de consulta (GET /chat/rooms) e no Webhook executado após cada alteração de uma sala específica. Os dados adicionais sempre serão retornados na propriedade `jsonData`.
    
    
    ⚠️ **ATENÇÃO:** Caso você faça atualização nessa propriedade através do método PUT de /chat/rooms todo o objeto jsonData será sobrescrito e o conteúdo anterior poderá ser perdido. Não há ***merge*** automático dos dados dessa propriedade.
    
    
