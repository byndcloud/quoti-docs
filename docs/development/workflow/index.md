## Objetivo

Explicar como podemos utilizar Quoti Workflows para realizar customizações dentro de uma organização do Quoti, além de explicar como as ferramentas nativas da plataforma funcionam.

  

## O que é

Uma ferramenta no code / low code para criação de fluxos e aplicações. Existe uma documentação extensa sobre todas as funcionalidades disponíveis na ferramenta de workflows chamada n8n, que foi utilizada como base para implementação dos Quoti Workflows: [https://docs.n8n.io/](https://docs.n8n.io/)

Recomendamos se familiarizar com as seguintes seções antes de continuar a utilizar essa documentação.

![](https://storage.googleapis.com/quoti-docs-pictures/workflow/docs_n8n.png)

### Caveat

A documentação do n8n está considerando a versão > 1.0 dele, a versão do n8n usada no Quoti é a 0.237.x, então algumas funcionalidades podem não estar disponíveis.

  

## Como acessar workflows

Na sua plataforma do Quoti navegue para quoti.cloud/_suaOrgSlug_/app/workflow. Segue um print da tela na organização interna da Beyond no Quoti:
![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Como_acessar_workflows.png)
É possível que o seu domínio seja diferente de quoti.cloud, mas para utilizar
essa ferramenta será necessário acessar por lá. Não se preocupe, apesar do link
ser diferente, a plataforma é a mesma.

## Nós do Quoti

O Quoti fornece nós customizados para facilitar a integração a plataforma. Especificamente para validar tokens de autenticação (Bearer) e gerenciar Quoti Databases. ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812220511.png)

### Credencial Quoti API

Existe um tipo de credencial específico para utilizar os nós do Quoti, com dois tipos de tokens.

**Service Account Token**: (Também chamado de BearerStatic) É um token estático para um usuário específico. Todas as operações realizadas com ele ficam associadas ao usuário em questão, geralmente uma conta de serviço. Para criar uma basta acessar /serviceaccounts, clicar no botão de (+) com um token seguro e aleatório: ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812215648.png)

**API Key**: Token com acesso mais abrangente, permitindo atribuições de permissões para autenticação via API do Quoti. Para obter uma é necessário solicitá-la diretamente ao time do Quoti.

* Depois de conseguir os tokens, basta [criar uma nova credencial](https://docs.n8n.io/credentials/add-edit-credentials/) do tipo Quoti API ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812215957.png)Você não precisa dos dois tokens para criar uma credencial, porém, cada um deles serve um propósito: a API Key serve para o nó de Quoti Auth e o Service Account Token serve para o nó de Quoti Databases.

### Nó Quoti Auth

Geralmente utilizado em conjunto com um nó de Webhook para verificar se um token não está expirado e pertence a um usuário da sua organização. Quando o token é valido, o nó retorna as informações do usuário dono do token.

![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812221251.png)

Por padrão o token é buscado nos dados de input do nó, dentro de um objeto chamado "headers", para facilitar a criação de endpoint autenticados com o nó de Webhook.

Caso necessário, também é possível especificar o token utilizando a opção chamada User's Token (geralmente utilizando uma expressão) ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812221553.png)

Também é possível verificar se o usuário identificado pelo token tem um conjunto de permissões utilizando o campo de Permissions. A sintaxe dele é de um array de objetos, permitindo fazer a verificação de múltiplas permissões como explicado aqui: https://www.npmjs.com/package/quoti-auth na secção de **Checking user permissions**.

O nó possui duas saídas, uma para quando a autenticação e autorização dão certo (pass) e outra para quando falham (fail). ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812221913.png)

### Nó Quoti Databases

Permite fazer CRUD em qualquer database da plataforma. Basta selecionar o Database, uma Operação e preencher os campos necessários para a operação. ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812223747.png)

#### Operação de Create

Permite adicionar linhas a um Quoti Database. É possível adicionar as colunas e seus valores. O nó já traz as colunas que existem no database automaticamente. ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812224516.png)

#### Operação de Update

Bem parecida com a operação de Create mas é necessário especificar o ID da linha que será atualizada.

![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812224805.png)

#### Operação de Delete

Apaga uma linha pelo ID dela.

![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812224850.png)

  

#### Operação List

Permite listar uma ou mais linhas do Quoti Database. Permite realizar filtros nos valores das colunas, limitar a quantidade de itens que é retornada e agregar os resultados em um só item do n8n ou em vários.

![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812224941.png)

##### Pro-tip

Neste nó existe uma propriedade de "Filter" que permite a utilização de um JSON para aplicar filtros complexos. Por exemplo: fazer comparação de maior que e menor que com datas e números, fazer lógicas de AND e OR e filtrar por linhas nas quais uma coluna possui uma substring específica (operador LIKE no SQL).

Para saber como usar o nó de filter, verifique a documentação do parâmetro where API de Quoti Databases [aqui](https://api.quoti.cloud/api-explorer/#/Resources/get__orgSlug__resources__table_) ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812225406.png)

#### Operação Get

Permite buscar uma linha do database pelo ID dela.

![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812225733.png)

  

## Um exemplo prático do uso de workflows

Suponha que estamos montando uma aplicação para registrar apostas relacionada às olimpíadas. Especificamente permitiremos que usuários possam criar apostas para disputas que vão acontecer. Também deve ser possível editar as apostas, mas **apenas até o momento que as disputas começarem**, depois não fica mais permitido editar as apostas.

Vamos utilizar dois Quoti Databases para guardar os dados, o primeiro irá guardar o calendário de disputas, com 3 colunas: *Descrição*, *Esporte*, *A data e horário de início da disputa*
![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812203434.png)

O segundo irá guardar as apostas, com 2 colunas: *A disputa* (ID de uma linha do banco de dados anterior) e o *País que irá ganhar o ouro*:
![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812203630.png)

Agora ainda é possível criar apostas até mesmo para disputas que já passaram,
como é possível ver no print anterior, então vamos resolver esse problema com um
workflow seguindo o passo a passo:
1. Primeiro, vamos criar o workflow e adicionar um nó de webhook nele, acessando a página de workflow do Quoti, vamos adicionar um novo: ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812204056.png)
2. Agora vamos dar um nome a dele e adicionar um nó de webhook: ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812204350.png) ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812204456.png)

3. Seremos levados imediatamente para a interface de configuração do webhook: ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812204552.png)

4. Vamos dar uma boa URL, utilizar o método POST para o webhook e escolher responder ele com um nó de "Respond To Webhook" (isso vai ser bem útil em alguns passos) ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812205031.png)

5. Agora vamos copiar a URL de produção do webhook (https://workflow.quoti.cloud/webhook/validar-apostas) e ativar o workflow: ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812205152.png) ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812205210.png)

6. Com o workflow criado vamos criar um hook no quoti database para o evento beforeUpdate, para que possamos abortar a operação caso necessário: ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812205453.png) ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812205547.png)

7. Com o hook criado, vamos tentar editar uma aposta apenas para verificar quais dados vamos receber no workflow ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812205912.png)

8. No workflow, se formos na aba de execução poderemos ver a execução em questão e quais dados teremos ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812210331.png)Podemos ver que temos vários dados do usuário (escondidos no print), e que realizamos uma requisição mudando país para Colômbia. Além disso é possível verificar que antes o país era Austrália.

9. Vamos copiar os dados do webhook na execução e [pinar](https://docs.n8n.io/data/data-pinning/) no nosso editor para criar as nossas regras no workflow! ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812210716.png) ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812210753.png)

10. Agora que temos dados podemos criar um workflow para validar se ainda é permitido editar uma aposta. A primeira coisa que precisamos fazer é conectar o nosso nó de Webhook a um novo nó de Quoti Databases e usar a coluna "dispute_id" para buscar os dados : ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812211734.png)Os nós devem ficar conectados, assim:

11. Agora podemos adicionar um nó de IF e verificar se a data da disputa é maior que a data atual: ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812212150.png) ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812212703.png)Usamos uma expressão para gerar uma data do momento atual com `new Date()`. No caso do exemplo, a data da disputa não ocorre depois da data atual, então precisaríamos bloquear essa operação.

12. Para tal vamos conectar um nó de *Respond To Webhook* para retornar um erro e abortar a operação: ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812212913.png) ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812214056.png)Retornamos um JSON com `{ "aborted": true, "message": "Você não pode mais editar a aposta!" }` e um código HTTP 403, indicando que o usuário não tem permissão para realizar a operação.

13. Agora já temos um workflow que bloqueia operações de edição de apostas quando a disputa em questão já começou! Vamos salvar o workflow e tentar editar a aposta para a disputa com id 1610 novamente! ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812213515.png) ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812214208.png)

14. Dessa vez vemos a mensagem de erro na interface! E se recarregarmos a página podemos verificar que nada foi alterado! ![](https://storage.googleapis.com/quoti-docs-pictures/workflow/Pasted_image_20240812214247.png)

15. Assim, nossas apostas agora estão seguras! É importante ressaltar que essa validação é feita pela API do Quoti, então mesmo que utilizássemos a API diretamente e não a interface (frontend) a validação ainda seria feita e o mesmo erro iria acontecer!

## Outros Exemplos de Casos de Uso

Mais alguns dos casos de uso de workflows:

- Sincronização de dados
  - No passado, já precisamos garantir que todas as pessoas que tem um certo
    perfil também participem de um  grupo específico.
- Enriquecimento de Dados
  - Seria possível utilizar hooks para preencher campos de um banco de dados com
    base em outros campos. Por  exemplo: dado um campo de "CEP", utilizar uma
    API para buscar informações do CEP e preencher outros campos como "Nome da
    Rua", "Cidade" e "Bairro".
- Bots do Telegram
  - É possível usar o nó de Trigger do Telegram para criar bots usando workflows.
- Cron jobs
  - É possível usar o nó de Schedule para executar workflows periodicamente. Por
    exemplo, para verificar  integridade de dados e que regras de negócio estão
    sendo cumpridas.
- Integrações com outros sistemas
  - Facilitar a comunicação entre APIs que não foram feitas para "conversarem".
    Pode-se utilizar um  workflow para mapear dados vindos de uma API para outro
    formato que uma segunda API entenda.