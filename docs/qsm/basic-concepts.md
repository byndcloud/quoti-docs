# Conceitos Básicos

## Tickets

Ticket são o recurso mais básico do QSM. Todos os outros conceitos servem para
dar significado a este. Ele representa uma solicitação, problema ou atividade
que precisa de atenção e resolução. É como se fossem um “container” com todas as
informações relacionadas a um problema ou atividade específica.

Um exemplo do uso de tickets:

Suponha que Bob foi contratado pela Beyond e precisa de um email corporativo, a
chefe dele, Alice, precisa criar uma solicitação para o departamento de RH para
que ela tenha seu novo email gerado. O departamento de RH possui várias pessoas
e uma delas deve ser a responsável por atuar nesta solicitação, neste caso Eva
será a responsável. Assim que a solicitação é criada o seu status é "Pendente",
quando Eva se torna responsável e começa a atuar nela, o status vai para "Em
andamento", quando Eva termina, a solicitação vai para o status de "Finalizada"
e Bob já tem seu endereço de email.

O exemplo anterior demonstra algumas características de tickets:

1. Eles possuem um tipo: Solicitação
2. Eles possuem uma categoria: Emissão de Email Corporativo
3. Eles podem ter um solicitante: Alice, a chefe de Bob
4. Eles podem ter um destinatário (a pessoa que se beneficia com a resolução do
   ticket): Bob, a pessoa que foi contratada
5. Eles possuem um status que muda com o tempo: “Pendente” → "Em andamento" →
   "Finalizada"
6. Eles podem ter um responsável: Eva, do RH
7. Eles podem ter uma fila de pessoas habilitadas a lidar com o problema: o
   departamento de RH

É possível ver o que aconteceu no ticket observando o histórico do mesmo.

Pode ter sub tickets podem existir outros tickets que são "filhos" do ticket
atual. Geralmente eles são utilizados para resolver sub-problemas associados a
um problema maior. Expandindo o exemplo acima, poderia haver mais um passo para
Alice (a chefe) solicitar equipamentos para Bob (que foi contratado)

## Tipos de tickets

Os tipos representam a natureza ou propósito de um ticket. São utilizadas para
classificar e entender o tipo de requisição ou problema que o ticket representa.
Alguns exemplos são Solicitações, Incidentes, Ativos, Documentos ou Contrato.

Os possíveis status que um ticket pode ter ficam especificados no tipo do
ticket. Por exemplo:

1. Solicitação: Pendente, Em Andamento, Gerado
2. Documento: Pendente, Gerado, Aprovado

Por último, para cada tipo de ticket também é possível criar um formulário que
conterá informações adicionais estruturadas sobre o ticket. Expandindo o exemplo
acima, poderíamos ter um campo no formulário de solicitação que contém o
departamento da pessoa que fez a solicitação, para que o RH já saiba disso
quando precisar atuar na solicitação

A interface de workspaces também pode ser configurada com base no tipo do ticket
e isso é explicado em detalhes no [guia de workspaces](./workspace.md).

## Categoria do ticket

Geralmente utilizado para dar uma classificação mais granular para o tipo do
ticket. Exemplos de categorias poderiam ser: Solicitação de Suporte, Computador
(Ativo), Comprovante de Matrícula (Documento), Contrato de Prestação de Serviço
(Contrato).

Assim como o tipo do ticket, a categoria também pode possuir informações
adicionais específicas para tickets daquela categoria. Seguindo o exemplo acima,
uma informação que poderia existir no formulário da categoria seria o email que
foi criado pelo sistema para Bob. Neste caso, quem preencheria esse campo seria
o RH.

## Filas

São grupos de pessoas que podem atuar como responsáveis por tickets. Geralmente
é quem consegue resolver o problema que o ticket representa. No tipo do ticket é
possível configurar qual será a fila padrão que o ticket irá receber.
