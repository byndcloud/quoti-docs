
# Visão Geral

  

Assim como em qualquer banco de dados não-relacional, é possível realizar as seguintes operações nas Quoti Databases:

  

1. criar uma Quoti Database.

2. criar uma instância nessa Quoti Database.

3. editar uma instância.

4. deletar uma instância.

  

Adicionalmente, na nossa interface, permitimos:

  

1. filtrar os dados de uma Quoti Database.

2. fazer download da Quoti Database como csv.

  

## Tutoriais

### Criar Uma Quoti Database

1. Acesse a seção de `/databases` da interface de alguma organização específica. Nesse exemplo, utilizaremos a organização beyond e acessaremos o link [https://quoti.cloud/beyond/databases](https://quoti.cloud/beyond/databases).

2. Clique no botão de `Novo`.
![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F1a882577e8645de7f9bc190424708869.png?alt=media&token=81ed2bf3-857b-4b7b-9a15-3b5b26a4bfaf)

3. Na seção de `Propriedades`, escreva o nome da sua Quoti Database.
![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F73f2aae41e44b6df01a33f66d9507b58.png?alt=media&token=aa1cc077-bc21-46df-aa80-85ce35246677)

4. Na seção de `Hooks` , você pode, opcionalmennte, criar Quoti Hooks para a sua Quoti Database. Quoti Hooks serão explicados em uma seção posterior a essa.![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F3203de5a0e01fa310752b5adfddfcf4a.png?alt=media&token=7cfe7490-fb5b-4ab5-a8ed-36269fa4006d)

5. Na seção de `Propriedades`, clique em `Continuar`, para criar o esquema da sua quoti-database. Em seguida, clique em `Adicionar Campo` . A sua quoti-database não será criada com sucesso se não tiver algum campo.![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F25b7ae290e085ae572da8abedc3b6855.png?alt=media&token=a18da52a-4eb8-4255-a22c-23ebadcb13fb)
  ![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F25474031eac9486d7ff4693efe70d6cd.png?alt=media&token=8af85a17-15bf-45e1-9158-38fed62ae9bc)
  Para cada campo, é obrigatório haver:
    - o `título`, que aparecerá na interface.
    - o `nome da variável` , que será utilizado pelos desenvolvedores. Ele deve ser escrito em [snake case](https://www.alura.com.br/artigos/convencoes-nomenclatura-camel-pascal-kebab-snake-case).
    - o `tipo`.

    Opicionalmente, é possível:

      - preencher a `Dica da Questão`, que serve para dar mais informações sobre aquele campo para o usuário que terá que preenchê-lo.

      - definir o campo como obrigatório.
      
      ![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2Fdb9494d300289dcf3097547b98d5e484.png?alt=media&token=eb3309b5-0fbe-41fc-b6aa-9ef7a893ca7e)
    > **Alguns tipos possuem configurações especiais.** Seguem alguns exemplos:
    > 
    >   - **Os tipos de `Múltipla Escolha` e de `Caixas de seleção` devem ter opções:**
    > 
    >      ![Múltipla Escolha e Caixas de seleção](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F7d0e142cdc42297acb856b480e9746b3.png?alt=media&token=de6ca438-e5b8-4bae-bc5d-29c517be8b1e)
    > 
    >   - **O campo de Anexo deve ter o formato de arquivo a ser aceito e o tamanho máximo do arquivo.** Adicionalmente, é possível definir se
    > deve-se aceitar apenas um, ou múltiplos arquivos:
    > ![Formato de arquivo e tamanho máximo](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2Fb2217a7b92cbfda0f39e8adfd93e5742.png?alt=media&token=7708667f-31ec-4637-8117-85bec620c1d6)


  
  
6. Por fim, clique em `Salvar base de dados`.
![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F1ce2cf1dde2d2aa291e6bbd8e7446759.png?alt=media&token=e9d43637-80c7-4677-ab6a-e7f039ee9246)

### Criar Uma Instância

1. Encontre a sua Quoti Database e clique no ícone de `Acessar database`.
![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2Fc3c85ad676ec34a4a4f1528930250b9b.png?alt=media&token=02de2485-528e-402f-ae33-3470c2456f97)

2. Clique em `Novo` e você terá a opção de criar uma instância individualmente, ou de criar instâncias em lote, a partir de um arquivo de planilha.
![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F8780424c38e03c24a501a0252aaec066.png?alt=media&token=a493f1bb-20a4-4754-9716-56b1d19acf68)

3. Fim! A criação de instâncias é super simples.

> No caso de arquivos anexos, você terá acesso ao link desse arquivo no firebase.
> ![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F36fe10dd9ccbfbaa1a586291dd6e2650.png?alt=media&token=2f78e758-c8c2-4073-bd58-c828e858973e)

### Editar Uma Instância

  

1. Identifique a instância que deseja editar e clique no ícone de lápis.![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F8f1b50201bf79bd11c9c8082dc51bd98.png?alt=media&token=8da1702f-09f1-4271-9f03-4444cea759f2)

2. Edite os campos e clique em `Salvar`.![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2Fc780cd841a0cbc0dc6a41284828a46ab.png?alt=media&token=256452ff-17d5-41d2-a9f2-763d28994211)

### Deletar Uma Instância

1. Identifique a instância que deseja editar e clique no ícone de lixeira.![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2Ffa2ab593ba4be5982608aa0253f14842.png?alt=media&token=b97782fc-c145-4509-b26c-219dbb211158)

2. Clique em `Sim` no popup gerado.![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F475108141adecddb86d63bab8529e5d5.png?alt=media&token=5a34c4c8-2643-4006-b9fc-8f2e7daf6686)

### Filtrar Dados

1. Clique no ícone de listagem e selecione os campos que devem ser considerados na busca.![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2Ff7e32cf04bb72bcb4e1036f40c42ec7f.png?alt=media&token=f4d64f9d-bac3-4b14-8e93-772759082786)![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F3b4ac3c0932c03fcab558418181728a9.png?alt=media&token=19e64f98-1dfb-4eb0-899f-37a36b01ffa9)

2. Utilize o campo de texto para escrever a sua busca e aperte a tecla `enter` ou clique no ícone da lupa.
![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2F5da7581ce4963b2133515241bf158318.png?alt=media&token=900f5ad7-031a-4bc9-9c1d-ada6798dc7bb)

  

### Fazer Download Da Quoti Database Como CSV

1. Clique no ícone de download.![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2Fb8d8d127c15fa1d8cb60f761a293971c.png?alt=media&token=8caf98b9-dbf8-45aa-b462-77c7c2a90184)

2. Clique em `Exportar` no popup gerado.![enter image description here](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2024%2F08%2Fb5bcf8fc5adff672d3c94d5abe4bd767.png?alt=media&token=a20fb74d-ace4-4cd5-b13a-e15961142ec3)