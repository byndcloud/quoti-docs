# Catálogos

## Como funciona o Módulo de Catálogo de Serviços

O catálogo de serviços é um módulo da plataforma Quoti que permite organizar a oferta de serviços que podem ser consumidos pelos usuários, principalmente através de chamados, ou *tickets*. Ele centraliza os conceitos de *tickets*, tipos, categorias, formulários e *workflows* para que os usuários finais consigam abrir chamados de maneira intuitiva e padronizada. 

O catálogo de serviços funciona de forma hierárquica:

*   **Catálogos:** Reúnem conjuntos de serviços e podem ser personalizados com diferentes URLs para atender a necessidades específicas, como um catálogo para ITSM ou outro para a área de infraestrutura. 
*   **Conjuntos de Serviços:** Funcionam como agrupadores de serviços relacionados, ajudando na organização e na apresentação da informação. Por exemplo, um conjunto chamado "Solicitações de E-mail" poderia conter serviços como "Criar E-mail", "Desativar E-mail", etc..
*   **Serviços:** Representam o serviço final a ser consumido pelo usuário. Ao clicar em um serviço, o usuário é direcionado a um formulário para abrir um chamado, especificando tipo, categoria e outras informações relevantes.

É importante destacar que a plataforma oferece flexibilidade na construção do catálogo, adaptando-o à identidade visual da organização e às necessidades específicas de cada cliente.

Os formulários de serviços são compostos por campos que se originam de três entidades: *ticket*, tipo e categoria. Essa estrutura permite uma organização otimizada da informação, exibindo primeiro os campos essenciais do *ticket* (como assunto e descrição), seguidos por campos específicos da categoria e, por fim, campos adicionais para detalhamento da solicitação.

A plataforma oferece diversas ferramentas para personalizar a experiência do usuário com o catálogo de serviços, incluindo:

*   **Campos Pré-definidos:** Permitem preencher previamente informações nos formulários, ocultando campos desnecessários e tornando o processo mais rápido e intuitivo.
*   **Avisos:** Possibilitam exibir mensagens informativas para os usuários, como alertas sobre problemas conhecidos ou indisponibilidades, evitando a abertura de chamados desnecessários.
*   ***Ticket Flow***: Recurso para automatizar ações no momento da abertura de um chamado, como a criação de chamados filhos para diferentes equipes, a partir de um chamado principal.
*   ***Workflows***: Ferramenta poderosa para automatizar fluxos de trabalho complexos, integrando diferentes sistemas e serviços. Permite criar APIs personalizadas dentro da plataforma para automatizar tarefas, como a criação de um usuário no Office 365 após a aprovação de um chamado.
*   ***Form Flow***: Recurso para criar fluxos de preenchimento dinâmicos nos formulários, exibindo ou ocultando campos, validando informações e tornando a experiência do usuário mais intuitiva.

Em resumo, o catálogo de serviços da plataforma Quoti oferece uma solução completa e personalizável para organizar e automatizar o atendimento aos usuários. Com suas diversas funcionalidades, é possível criar fluxos de trabalho eficientes, otimizar o tempo das equipes e proporcionar uma melhor experiência aos usuários.

## Criação de Catálogo de Serviços na Plataforma Quoti

Todo ambiente Quoti já vem com um catálogo padrão chamado **Home**, cujo path é **/catalog/home**, disponível para ser utilizado.

Para criar um catálogo de serviços na plataforma Quoti, você precisa entender a estrutura hierárquica do catálogo e configurar cada um de seus elementos. O processo envolve a criação de catálogos, conjuntos de serviços e serviços propriamente ditos.

**1. Criando um novo catálogo:**

1.1. Acesse a tela de bases de dados **/databases**.

1.2. Utilizando a barra de pesquisas, procure pela tabela **"csm_catalog"**.

1.3. Clique no botão "Acessar database":
![Acessar Base de dados do Catálogo](https://storage.googleapis.com/quoti-docs-pictures/QSM/catalog/catalog1.png)
1.4. Clique no botão "Novo":
![Adicionar novo Catálogo](https://storage.googleapis.com/quoti-docs-pictures/QSM/catalog/catalog2.png)
1.5. Clique na opção "Individualmente":
![Novo catálogo individualmente](https://storage.googleapis.com/quoti-docs-pictures/QSM/catalog/catalog3.png)
1.6. Digite o nome ("Título") e o path do catálogo. Por fim, clique no botão "Cadastrar":
![Definições do novo catálogo](https://storage.googleapis.com/quoti-docs-pictures/QSM/catalog/catalog4.png)
*   **Exemplo:** Um catálogo chamado "Meu catálogo" terá a URL **"/app/catalog/meu_catalogo"** e poderá ser acessado pelos usuários através dela.

**2. Criando Conjuntos de Serviços:**

2.1. Dentro do catálogo desejado, clique em "Adicionar Item".
![Adicionar novo item](https://storage.googleapis.com/quoti-docs-pictures/QSM/catalog/catalog7.png)
2.2. Escolha o tipo "Conjunto de Serviços".

2.3.   Preencha as informações do conjunto:
*   **Nome:** Título que aparecerá no catálogo.
*   **Subtítulo:** Descrição breve do conjunto.
*   **Corpo da Página:** Mensagem com mais detalhes sobre o conjunto, como um *disclaimer* ou aviso.
*   **Foto:** Imagem para ilustrar o conjunto.
![Adicionando um novo Conjunto de serviços](https://storage.googleapis.com/quoti-docs-pictures/QSM/catalog/catalog5.png)
*   Clique no botão "Criar" no final do formulário de criação
*   **Exemplo:** Um conjunto chamado "Solicitações de E-mail" poderia conter a descrição "Serviços relacionados a contas de e-mail", um corpo da página explicando as políticas de uso de e-mail e uma foto de um envelope.
![Resultado após a criação](https://storage.googleapis.com/quoti-docs-pictures/QSM/catalog/catalog6.png)

**3. Criando Serviços:**

3.1. Clique no título do conjunto de serviços que você deseja adicionar um serviço filho

3.2. Dentro do conjunto de serviços desejado, clique em "Adicionar Item".

3.3. Escolha o tipo "Serviço".
*   Preencha as informações do serviço:
    *   **Nome:** Título que aparecerá no catálogo.
    *   **Subtítulo:** Descrição breve do serviço.
    *   **Corpo da Página:** Mensagem com mais detalhes sobre o serviço.
    *   **Foto:** Imagem para ilustrar o serviço.
    *   **Tipo de Ticket:** Selecione o tipo de *ticket* que será aberto ao solicitar o serviço (ex: Incidente, Solicitação).
    *   **Categoria:** Selecione a categoria do *ticket* dentro do tipo escolhido (ex: "Criação de E-mail" dentro da categoria "E-mail").
*   **Exemplo:** Um serviço chamado "Criar E-mail", dentro do conjunto "Solicitações de E-mail", teria o tipo "Solicitação" e a categoria "Criação de E-mail".

**4. Configurando o Formulário do Serviço:**

*   Após criar o serviço, você pode configurar o formulário que será exibido para os usuários solicitarem o serviço.
*   O formulário é composto por campos do *ticket*, campos do tipo de *ticket* e campos da categoria.
*   **Campos do Ticket:**
    *   **Assunto:** Define o assunto do *ticket*. É possível preencher previamente esse campo e ocultá-lo para o usuário, ou deixá-lo editável.
    *   **Descrição:** Permite que o usuário forneça mais detalhes sobre a solicitação. É um campo aberto e editável.
    *   **Anexos:** Permite anexar arquivos à solicitação.
*   **Campos do Tipo de Ticket e da Categoria:**
    *   Esses campos são definidos nos tipos e categorias de *tickets*.
*   **Exemplo:** O formulário do serviço "Criar E-mail" pode ter o campo "Assunto" pré-definido como "Solicitação de Criação de E-mail" e os campos "Nome Completo", "CPF" e "Cargo" provenientes da categoria "Criação de E-mail".

**5. Personalizando a Experiência do Usuário:**

*   **Campos Pré-definidos:** Preencha previamente informações nos formulários para agilizar o processo para o usuário. 

*   **Ocultando campos padrão:** Desative campos que os usuários não precisarão preencher, como: "Assunto", "Beneficiário", "Informações Adicionais", "Descrição", "Anexo".
![Campos que podem ser ocutados](https://storage.googleapis.com/quoti-docs-pictures/QSM/catalog/catalog8.png)

*   **Ticket flows:** Utilize este recurso para automatizar ações no momento da abertura de um chamado, como a criação de chamados filhos para diferentes equipes, a partir de um chamado principal.
![Ticket Flows](https://storage.googleapis.com/quoti-docs-pictures/QSM/catalog/catalog9.png)