## Objetivo

Nessa seção vamos por em prática o uso dos Formflows aplicando os seus conceitos e ferramentas para implementar o exemplo da introdução visto na [Visão Geral](/orgs/byndcloud/forms/formflows/).

!!! example "Mais exemplos"
     Caso você deseje ver mais exemplos e ter mais ideias do que pode ser feito com os Formflows acesse a [antiga documentação](https://beyondco.notion.site/Exemplos-207c19723bcf479fb9ececd77094a5b6).

Sendo assim desejamos alcançar os seguintes fluxos:

- Iniciar com campos ocultos
``` mermaid
graph LR
  A[Start] --> B{Formulário aberto};
  B --> C[Oculta CPF];
  C --> D[Oculta CNPJ];
  D --> E[Oculta Razão social];
  E --> F[Oculta Nome Fantasia];
  F --> G[Oculta Endereço];
```

- Exibir CPF ou CNPJ
``` mermaid
graph LR
  A[Start] --> B{Pessoa física?};
  B -->|Verdadeiro| C[Exibe CPF];
  C --> D[Oculta CNPJ];
  D --> B;
  B -->|Falso| E[Oculta CPF];
  E --> F[Exibe CNPJ];
  F --> B;
```
- Busca e preenchimento de campos com dados da empresa


## Implementação

### Configuração inicial
Primeiramente temos que criar um formulário, para isso utilizaremos os [Quoti Databases](/orgs/byndcloud/quoti-databases/).

Acesse a página que lista todos os bancos de dados em /databases.
![Databases](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Databases.png)

Então crie um novo database para armazenar as respostas do seu formulário de fornecedores.
![Clicar para criar novo Database](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Click%20new%20db.png)
![Escolher nome](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Choose%20name.png)

Clique em continuar e configure seu formulário com todos os campos necessários. Nesse caso vamos precisar dos seguintes campos:

- Nome: nome do fornecedor, campo de resposta curta;
- Tipo de pessoa: múltipla escolha com opção "Pessoa física" ou "Pessoa jurídica";
- CPF\: campo do tipo CPF;
- CNPJ\: campo do tipo CNPJ;
- Razão social: campo de resposta curta;
- Nome Fantasia: campo de resposta curta;
- Endereço: campo de resposta curta;

Todos os campos devem ser obrigatórios e para simplificar posteriormente, copie e cole o campo "Título" para "Nome da variável" conforme nas imagens abaixo.
![Mudar nome da variável 1](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Change%20name%201.png)
![Mudar nome da variável 2](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Change%20name%202.png)

Clique para salvar a base de dados. Atualize a página, procure pela base de dados que você acabou de criar e clique para gerenciá-la.
![Gerenciar database](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Manage%20db.png)

Então clique na opção para editar Formflow daquele formulário. Você será redirecionado para o Formflow Builder!
![Gerenciar formflow](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Manage%20formflow.png)
![Tela de formflow](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Formflow.png)

### Fluxo 1 - Iniciar com campos ocultos
Nosso primeiro fluxo consiste em ocultar os campos que não queremos que sejam exibidos assim que nosso fornecedor abrir o formulário. São eles:

- CPF
- CNPJ
- Razão social
- Nome fantasia
- Endereço

Para isso vamos nomear esse nosso fluxo de "Inicia campos ocultos". Abaixo nomearemos o primeiro nó de "Formulário aberto" com o mesmo tipo. E então adicionamos um nó com nome "Oculta campos".

![Nome do primeiro nó](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Name%20node.png)
![Tipo do primeiro nó](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Type%20node.png)
![Nome do segundo nó](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Name%20node%202.png)

Esse campo deve ser do tipo que realiza modificações em propriedades de um campo. Em seguida selecione quais campos que desejamos alterar as propriedades. Em seguida adicione uma ação.
![Tipo do segundo nó](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Type%20node%202.png)
![Selecione os campos](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Select%20fields.png)
![Selecione os campos que deseja editar as propriedades](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Select%20fields%202.png)
![Adicione ação](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Add%20action.png)

Configure para a ação ser do tipo que modifica a visibilidade, selecione seu valor para oculto.
Salve o Formflow para checarmos se nossas alterações funcionaram!
Acesse novamente a página que lista todos os bancos de dados em /databases e procure pelo database criado por você. Então clique para adicionar um novo individualmente.
![Mudar visibilidade](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Change%20visibility.png)
![Oculto](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Hidden.png)
![Acessar banco de dados](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Access%20db.png)
![Adicionar novo individualmente](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Add%20new.png)

O formulário possui apenas os campos de Nome e Tipo de Pessoa 🎉
![Apenas 2 campos](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Only%202%20fields.png)

Com isso nosso fluxo 1 ficou com essa cara:
``` mermaid
graph LR
  A[Start] --> B{Formulário aberto};
  B --> C[Oculta CPF];
  C --> D[Oculta CNPJ];
  D --> E[Oculta Razão social];
  E --> F[Nome Fantasia];
  F --> G[Endereço];
```

### Fluxo 2 - Exibir CPF ou CNPJ
Para esse fluxo queremos exibir/ocultar os campos CPF/CNPJ com base na resposta do campo anterior de Tipo de pessoa. Para isso vamos criar um novo fluxo e batizá-lo de "Oculta/Exibe CPF/CNPJ" e dessa vez o nosso tipo de nó será o que observa por modificações em um campo do formulário. O campo será o "Tipo_de_pessoa".
![Cria novo fluxo](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Create%20new%20flow.png)
![Modificações em um campo](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Node%20fieldWatch.png)

Em seguida adicione um novo nó do tipo que verifica condições e adicione uma ação para verificar se o valor é igual a algo. No valor, escreva qual valor você deseja que o campo seja para ir para o fluxo de verdadeiro, caso contrário ele irá pra falso. No exemplo queremos saber se o campo Tipo de Pessoa tem o valor "Pessoa física" ou não.
![Verifica condicoes](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/cond.png)
![Igual a](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/eq.png)
![Pessoa física](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/eq%20value.png)
![2 fluxos possíveis](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/2%20flows.png)

Para ambos os casos verdadeiro ou falso escolhemos o tipo do nó que realiza modificações em uma propriedade de um campo e selecionamos o campo de CPF.
Em seguida adicionamos uma nova ação para o caso verdadeiro, sua ação deve ser do tipo que modifica a visibilidade e seu valor deve ser para exibir. Para o caso falso fazemos o mesmo, apenas mudando que ao inves de exibir, vamos ocultar.
![Exibir/Ocultar CPF](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/2%20flows%201.png)

Por fim adicionamos mais um nó para ambos os casos verdadeiro e falso do tipo que realiza modificações em propriedades de um campo, selecionando agora o campo CNPJ. Para o caso verdadeiro, criamos uma nova ação que modifica a visibilidade, dessa vez com valor para Ocultar. No caso falso a mesma abordagem, modificando apenas o valor para exibir.
![Exibir/Ocultar CNPJ](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/2%20flows%202.png)

Salve e teste!
![Visualizar formulario](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/View.png)
![Exibe CPF](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/CPF%20show.png)
![Exibe CNPJ](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/CNPJ%20show.png)

Com isso nosso fluxo 2 ficou com essa cara:
``` mermaid
graph LR
  A[Start] --> B{Pessoa física?};
  B -->|Verdadeiro| C[Exibe CPF];
  C --> D[Oculta CNPJ];
  D --> B;
  B -->|Falso| E[Oculta CPF];
  E --> F[Exibe CNPJ];
  F --> B;
```


### Fluxo 3 - Busca e preenchimento de campos com dados da empresa
Vamos criar um novo fluxo dessa vez com o nome "Carrega dados da empresa". Para esse vamos querer que o primeiro nó seja o que observa por modificações em um campo. O campo observado será o campo de CNPJ. Então adicionamos um nó que verifica condições igual o fluxo anterior, porém a ação adicionada é do tipo que verifica condição atendida. Esse tipo de ação permite que você escreva condicionais mais complexas que apenas uma comparação de igualdade.

Para buscarmos pelas informações da empresa precisamos que dois critérios sejam atendidos:
- Que o campo CNPJ esteja preenchido completamente
- 
salvei algumas imagens na pasta screenshots com prints de onde parei
$field.nome.isValid()
https://publica.cnpj.ws/cnpj/34975590000190