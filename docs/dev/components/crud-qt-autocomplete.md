## Como utilizar CRUD no qt-autocomplete

### O que é:

Nada mais é que uma forma de você ter um CRUD para criação e edição de itens em
um autocomplete. Dessa forma facilitando o processo e não sendo necessário levar
o usuário para outra tela.

### Exemplo:

![image](https://github.com/user-attachments/assets/854af172-6b20-417d-acb1-cecc62e5df6b)
![image](https://github.com/user-attachments/assets/4f1a94e7-4e44-489a-9c2d-43a3a7bf81c8)

### Como funciona:

O CRUD ocorre com base em um "database" e um formulário. No momento, outros
casos não são suportados. Os campos exibidos durante a criação ou edição de um
item correspondem aos campos do formulário fornecido. Quando o formulário é
submetido, as alterações são refletidas no banco de dados associado à rota
"/databases".

### Como utilizar:

Para funcionar é necessário passar 3 props no componente de autocomplete do
Quoti:

```javascript
:allowCrud="true"
:getFormIdToCrud="() => 101255"
:getFormResponseIdToCrud="(item) => item.id"
```

1 - "allowCrud" para indicar a utilização CRUD. 2 - "getFormIdToCrud" para
indicar o formulário que será utilizado para criação e edição de itens. 3 -
"getFormResponseIdToCrud" para indicar o caminho até o id do item selecionado.
