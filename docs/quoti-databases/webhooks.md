# Webhooks em Quoti Databases

Ao adicionar, remover, editar ou fazer qualquer operação em uma tabela do Quoti Databases, uma série de processos internos ocorrem na plataforma. Um deles é a chamada de Webhooks.

Webhooks no Quoti Databases tem as seguintes características:

- Os webhooks são chamados a partir de eventos de operações no banco de dados.
- Podem ter caráter bloqueante. Isso é: pode bloquear uma operação como de escrita, por exemplo.
- Podem modificar o dado que estaria sendo criado ou editado, trazendo a possibilidade de tratar campos, por exemplo.
- Podem ser configurados para serem disparados em condições específicas, como apenas quando uma coluna específica recebe algum valor específico.

## Eventos

Aqui está uma lista de possíveis eventos que o Quoti pode detectar para chamar um webhook

- Eventos relacionado a criação de registros:
    - `beforeCreate` - Chamado antes de um registro ser criado
    - `beforeBulkCreate` - Chamado antes de ser criado vários registros (em lote)
    - `afterCreate` - Chamado após a criação de qualquer registro
    - `afterBulkCreate` - Chamado após a criação de vários registros (em lote)
- Eventos relacionados a atualização de registros:
    - `beforeUpdate` - Chamado antes de um registro ser atualizado
    - `beforeBulkUpdate` - Chamado antes de vários registros serem atualizados (em lote)
    - `afterUpdate` - Chamado depois de um registro ser atualizado
    - `afterBulkUpdate` - Chamado depois de vários registros serem atualizados (em lote)
- Eventos relacionados a deleção de registros:
    - `beforeDestroy` - Chamado antes de um registro ser apagado
    - `beforeBulkDestroy` - Chamado antes de vários registros serem apagados (em lote)
    - `afterDestroy` - Chamado depois de um registro ser apagado
    - `afterBulkDestroy` - Chamado depois de vários registros serem apagados (em lote)
- Eventos relacionados a listagem de registros:
    - `beforeFind` - Chamado antes de uma solicitação de listagem de registros

## Cadastrando Webhooks

A página de gestão de hooks do seu ambiente está disponível em https://quoti.cloud/`seuambiente`**/hooks**

![CleanShot 2024-08-12 at 9 .42.37@2x.png](https://storage.googleapis.com/beyond-quoti.appspot.com/docs/qtdatabases/webhooks/CleanShot_2024-08-12_at_9_.42.372x.png)

Você pode cadastrar um novo webhook através do botão “+ Novo”

![CleanShot 2024-08-12 at 9 .44.11@2x.png](https://storage.googleapis.com/beyond-quoti.appspot.com/docs/qtdatabases/webhooks/CleanShot_2024-08-12_at_9_.44.112x.png)

1. **Tipo de hook**
    - Para webhooks de Quoti Databases, você deve selecionar sempre o tipo “table”.
2. **Nome do recurso**
    - Aqui você deve escolher o nome da sua tabela que acionará os hooks.
3. **Handler**
    - Este campo recebe um JSON, que deve conter em seu corpo as propriedades `url` e `type`. Respectivamente, guardam o link que será chamado e o tipo de requisição
4. **Eventos**
    - Este campo pode receber múltiplos eventos, os quais servirão como gatilhos para acionar o webhook.
5. **Condições**
    - Campo opcional que define condições específicas no qual a plataforma deve considerar para disparar o webhook. Este campo deve receber um JSON no formato documentado [neste link](https://github.com/cachecontrol/json-rules-engine/blob/HEAD/docs/rules.md#conditions).
6. **Configurações**
    - Campo opcional que define configurações avançadas de um webhook. O formato esperado é JSON e suas possível propriedades são as seguintes:
        - `returnBeforeData` - Campo booleano (com valor true ou false) que determina se a plataforma deve incluir a informação de ‘beforeData’ no corpo da chamada do webhook para eventos de atualização. ‘beforeData’ representa os dados na tabela antes da finalização da edição do registro em andamento.
        - `returnAfterData` - Campo booleano (com valor true ou false) que determina se a plataforma deve incluir a informação de ‘afterData’ no corpo da chamada do webhook para eventos de atualização. ‘afterData’ representa os dados na tabela após a finalização da edição do registro que acabou de ser feita - neste caso, apenas nos eventos de ‘afterUpdate’ este campo estará com os dados definidos.

### Operadores disponíveis

| Tipo              | Operador                                                               | Descrição                                                     |
|-------------------|------------------------------------------------------------------------|---------------------------------------------------------------|
| **Booleano**      | `all`, `any`, `not`                                                    | Controlam a lógica de múltiplas                               |
| **String/Número** | `equal`, `notEqual`                                                    | Igualdade ou diferença exata (`===` / `!==`)                  |
| **Numérico**      | `lessThan`, `lessThanInclusive`, `greaterThan`, `greaterThanInclusive` | Comparações numéricas diretas                                 |
| **Array**         | `in`, `notIn`, `contains`, `doesNotContain`                            | Verifica a presença ou ausência de um valor dentro de um array|

> Operadores que suportam `value` como lista: `in`, `notIn`, `contains`, `doesNotContain`

> ℹ️ Para detalhes completos sobre sintaxe, operadores e exemplos de uso, consulte a [documentação da biblioteca](https://www.npmjs.com/package/json-rules-engine).

## Exemplos de Webhooks

### Exemplo 1 – Webhook somente para categorias específicas

Dispara apenas se o `categoryId` enviado estiver em uma lista predefinida (ex: manutenção de ATM e validadores):

**Configurações:**

```json
{
  "asyncHook": true,
  "returnAfterData": false,
  "returnBeforeData": false
}
```

**Condições:**

```json
{
  "all": [
    {
      "fact": "requestData",
      "path": "$.body.categoryId",
      "value": [
        100004, 100015, 100016, 100019, 100031,
        100032, 100033, 100034, 100035, 100036,
        100037, 100038, 100039, 100040, 100041
      ],
      "operator": "in",
      "description": "Apenas ticket manutenção de ATM e validadores"
    }
  ]
}
```

---

### Exemplo 2 – Webhook ignorando chats e CCO

Executa o webhook apenas quando o (`ticketTypeId`) **não** corresponder a determinados valores específicos:

> 💡 Como mostrado na imagem abaixo, o conteúdo retornado na chave after deve ser referenciado como dataAfterEvent na condição. Logo, o fact correto a ser utilizado será "dataAfterEvent"

![Exemplo dataAfterEvent](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2025%2F06%2F64b5a783ab971960a240d5155c94a645.png?alt=media&token=c2f158f1-3b95-4947-b6f4-005b242ade9b)

**Configurações:**

```json
{
  "attributes": ["ticketTypeId"],
  "returnAfterData": true,
  "returnBeforeData": false
}
```

**Condições:**

```json
{
  "all": [
    {
      "fact": "dataAfterEvent",
      "path": "$.ticketTypeId",
      "value": [3, 11],
      "operator": "notIn",
      "description": "Chamados diferentes de chats e CCO"
    }
  ]
}
```

---

### Exemplo 3 – Webhook só dispara se `status` não for nulo

Verifica se a propriedade `status` existe e contém pelo menos 1 item:

**Condições:**

```json
{
  "all": [
    {
      "fact": "requestData",
      "path": "$.body.status.length",
      "value": 1,
      "operator": "greaterThan",
      "description": "Status diferente de nulo"
    }
  ]
}
```

## Caso de teste

Vamos criar um webhook de afterCreate em uma tabela e ver como ele é chamado!

![CleanShot 2024-08-12 at 9 .59.12@2x.png](https://storage.googleapis.com/beyond-quoti.appspot.com/docs/qtdatabases/webhooks/CleanShot_2024-08-12_at_9_.59.122x.png)

![CleanShot 2024-08-12 at 9 .59.27@2x.png](https://storage.googleapis.com/beyond-quoti.appspot.com/docs/qtdatabases/webhooks/CleanShot_2024-08-12_at_9_.59.272x.png)

Após a criação, é necessário definir o “Status do webhook” como “Ativo” para que a plataforma o considere.

![CleanShot 2024-08-12 at 10 .00.11@2x.png](https://storage.googleapis.com/beyond-quoti.appspot.com/docs/qtdatabases/webhooks/CleanShot_2024-08-12_at_10_.00.112x.png)

Agora que temos nosso webhook definido, vamos criar um dado aleatório na tabela relacionada e verificar a chamada de webhook que a plataforma fez.

Aqui está o corpo da chamada HTTP feita pela plataforma:

![CleanShot 2024-08-12 at 10 .16.49@2x.png](https://storage.googleapis.com/beyond-quoti.appspot.com/docs/qtdatabases/webhooks/CleanShot_2024-08-12_at_10_.16.492x.png)

O conteúdo de toda chamada contém:

- Parâmetro `user` - Onde vem uma cópia da instancia do usuário que está fazendo a operação, incluindo grupos e permissões
- Parâmetro `resourceType` - Contém o valor do campo “Tipo de hook” definido na hora da criação do webhook. No caso de webhooks de Quoti Databases, esse valor sempre será “table”.
- Parâmetro `resourceName` - Contém o nome da tabela onde o evento se originou.
- Parâmetro `event` - Contém o nome do evento que está sendo disparado
- Parâmetro `data` - Objeto que contém os dados que estão sendo criados ou atualizados ou apagados, de acordo com o evento. Caso o hook tenha as configuração. "returnBeforeData” e “returnAfterData” habilitadas, os dados relacionados também serão retornados dentro deste objeto.