## Como desenvolver um CRUD utilizando Quoti Extensions

### Exemplo utilizando o qt-database

```javascript
<template lang="pug">
div
  qt-header(title="Atalhos da plataforma")
  v-container
    qt-database(:headers="headers" storeModule="shortcuts")
</template>

<script>
export default {
  data() {
    return {
      headers: {
        name: {
          label: 'Nome',
          required: true,
          type: {
            name: 'text'
          }
        },
        icon: {
          label: 'Icone',
          required: true,
          type: {
            name: 'icon'
          }
        },
        link: {
          label: 'Link',
          required: true,
          type: {
            name: 'text'
          }
        },
        edit: {
          label: 'Editar',
          width: '100',
          align: 'center',
          sortable: false
        },
        delete: {
          label: 'Deletar',
          width: '100',
          align: 'center',
          sortable: false
        }
      }
    }
  }
}
</script>
```

### Resultado:
<img src="https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/legis%2F2024%2F06%2F0a75d3c48816d1374790b475048d18cf.png?alt=media&token=395dd06d-04ce-4d92-82de-a8ab8dc66842
" width="850" height="400">


### Conceitos principais
Headers:
Nós passamos uma prop chamada "headers" no qual será responsável pelo controle do componente em geral, através dela nós podemos definir as colunas da tabela, e os campos que serão preenchidos em uma edição/criação de um elemento.

storeModule:
Essa prop é responsável por definir exatamente a "store" que estará sendo chamada para realização do CRUD, ou seja, no exemplo acima quando colocamos o valor "shortcuts", nós estamos dizendo que essa tabela será responsável por listar, criar, editar, e excluir uma "shortcut".

### Desbravando o Headers
Headers é um objeto com vários objetos dentro, no qual cada um é responsável por uma coluna da tabela e seu campo respectivamente.
Estrutura padrão de um objeto do headers:
```javascript
        name: {
          label: 'Name',
          required: true,
          type: {
            name: 'text'
          },
        sortable: true
        },
```
1 - Label: Nome que irá ser exibido na coluna e no campo do Dialog.
2 - Required: Se este campo será obrigatório na criação de um novo elemento.
3 - Type: Tipo do campo na criação/edição de um elemento.
4 - Sortable: Se aquela coluna da tabela poderá ser utilizada para ordenar a tabela.

Ao acessar a criação/edição de um elemento isso será retornado no Dialog:
<img src="https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/legis%2F2024%2F06%2F630477b958492a1236428bf8b62d4af8.png?alt=media&token=cd4302d6-a4b9-4b87-b2f5-a4e876e4d8d4" width="500" height="200">

Types suportados: text, newFormId, number, textarea, buttonaction, richtext, newFormId, pageSelector, json, boolean, icon, color, multiplechoice.

Obs: 
1 - Para utilizar o tipo newFormId (um campo de formulário no qual é possível configurar informações adicionais) é necessário também passar outras propriedades no objeto, exemplo:
```javascript
formId: {
    label: 'Dados adicionais',
    type: {
        name: 'newFormId'
    },
    additionalConfigs: {
        prefixDatabase: 'product_type',
        ready: () => {
            return !!this.form.id
        }
    },
  required: true
}

```

Resultado:
<img src="https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/legis%2F2024%2F06%2Fed3a71f69926fa95a3ee0cda474ee6ad.png?alt=media&token=ab741c91-0e79-460b-99a8-8fa8d31935e4" width="450" height="400">

2 - O tipo de number suporta contador de caracteres:
```javascript
name: {
          label: 'Exemplo com counter',
          type: {
            name: 'text',
            counter: 10
          },
        }
      },
```

3 - O tipo do multiplechoice pode ser manipulado de diferentes maneiras:
```javascript
config_login_method: {
    label: 'Método de Login',
    required: true,
    type: {
      name: 'multiplechoice',
      useSelect: true,
      itemText: 'name',
      itemValue: 'name',
      returnObject: true,
    },
    selectableValues: [
        {
            name: 'Usuário'
        },
        {
            name: 'teste editado 2'
        },
    ]
}
```
Isso irá resultado em um campo de múltipla escolha no qual recebe um array de objetos e retorna um objeto:

<img src="https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/legis%2F2024%2F06%2F456a8bbce83e84999c61d86a25eeb26b.png?alt=media&token=679ef7fd-0760-4dad-b8c4-0ee0bc4aa7de" width="450" height="400">



### Ocultando colunas na tabela:
Nem sempre você irá querer que todos os inputs presentes na criação/edição de elementos sejam colunas na tabela, é possível controlar exatamente quais headers irão ser exibidos através da prop tableHeadersOrder.
Exemplo:
```javascript
<template lang="pug">
div
  v-container
    qt-database(:headers="headers" storeModule="shortcuts":tableHeadersOrder="tableHeadersOrder")
</template>

<script>
export default {
  data() {
    return {
      tableHeadersOrder: [
        'name',
        'office'
      ]
      headers: {
        name: {
          label: 'Nome',
          required: true,
          type: {
            name: 'text'
          }
        },
        office: {
          label: 'Empresa',
          required: true,
          type: {
            name: 'text'
          }
        }
      }
    }
}
</script>
```

Dessa forma apesar de "office" estar presente no headers, ele não irá ser uma coluna da tabela, já que não se encontra no array de tableHeadersOrder.

### Ocultando inputs no Dialog:
Também é possível controlar quais inputs iráo ser exibidos no dialog de criação/edição com a proriedade hidden, dessa forma um header irá ser exibido exlusivamente na coluna:
Exemplo:
```javascript
headers: {
    createdAt: {
      label: 'Criado em',
      visibility: 'hidden',
      convert: (value) => {
        return !value ? '' : this.$moment(value).format('LLL')
      }
    }
}
```


### Adicionando textos/html antes ou depois de campos:
```javascript
<template lang="pug">
div
  v-container
    qt-database(:headers="headers" storeModule="shortcuts")
          template(#after.name)
              span Estou após name
</template>

<script>
export default {
  data() {
    return {
        headers: {
            name: {
                label: 'name',
                type: {
                    name: 'text'
                }
            }
        }
    }
}
</script>
```

### Utilizando o v-form:
Átravés da prop v-form é possível ter uma maior liberdade sobre os valores dos inputs configurados no headers. Através dela nós conseguimos estar mudando o valor de determinados elementos com a lógica que quisermos.
Exemplo:
```javascript
<template lang="pug">
div
  v-container
    qt-database(
      ref="hooksDatabase"
      v-model="form"
      :headers="headers"
      storeModule="hooks"
      :tableHeadersOrder="tableHeadersOrder"
    )
</template>

<script>
export default {
  data() {
    return {
        form: {
            form.slug: null,
        },
        headers: {
            slug: {
                label: 'Slug',
                required: true,
                type: {
                    name: 'text'
                }
            }
        }
    },
    watch: {
        'form.slug'(nv) {
          this.form.slug = slugify(nv)
        }
    },
    Methods: {
        slugify() {
              if (!str) {
                return str
              }
              str = str.replace(/^\s+|\s+$/g, '') 
              var from = 'àáäâèéëêìíïîòóöôùúüûñç·/_,:;'
              var to = 'aaaaeeeeiiiioooouuuunc------'
              for (var i = 0, l = from.length; i < l; i++) {
                str = str.replace(new RegExp(from.charAt(i), 'g'), to.charAt(i))
              }a
              str = str
                .replace(/[^a-zA-Z0-9 -]/g, '') 
                .replace(/\s+/g, separator) 
                .replace(/-+/g, separator) 
              return str
        }
    },
</script>
```
Observe como dessa forma conseguimos manipular o valor de um input da maneira que quisermos.
É importante salientar que através do form declado nós também conseguimos estar enviando variáveis que não estão presentes no headers para as requisições de criação/edição. 


