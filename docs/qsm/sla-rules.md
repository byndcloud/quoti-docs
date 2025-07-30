### O que é

As regras de SLA permitem configurar condições para criar, pausar, cancelar ou finalizar uma SLA utilizando [JSON Rules Engine](https://www.npmjs.com/package/json-rules-engine).

### Como funciona

Uma regra de SLA é composta por **condições** e uma **SLA**.

- Condição de criação
- Condição de pausa
- Condição de cancelamento
- Condição de finalização
- ID da SLA

Dessa forma, na criação e atualização de um ticket, sempre que o ticket atender as condições de uma regra de SLA, a SLA terá seu status alterado para o status da regra. Caso uma condição seja válida, ela será aplicada, mas é importante ter em mente que, em casos onde mais de uma condição seja válida, o status da última prevalecerá.

Ordem das condições:

1. Condição de criação
2. Condição de pausa
3. Condição de cancelamento
4. Condição de finalização

**Exemplo:** Ambas as condições de pausa e finalização foram atendidas, portanto o status final do ticket será o de finalização. A SLA terá seu status alterado para "Pausado" e, em seguida, para "Finalizado".

### Como utilizar

A extensão `slaRules` foi criada para gerenciar a funcionalidade.

![image](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2Fffba3f7aebe87cd62209d84d8146d97b.png?alt=media&token=c0af6af2-5bec-4f35-b1d1-fc9c2a97f0ae)

![image](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F1bf57d6a2f0ce848d214734e93ce32d6.png?alt=media&token=66bb543c-084c-4a99-a187-af670c2d7556)

**Exemplo de regra de SLA utilizando JSON Rules Engine:**

Regra adicionada no campo "Condições de finalização"
```json
{
  "any": [
    {
      "fact": "requestData",
      "path": "$.event.ticketId",
      "value": 123,
      "operator": "equal",
      "description": ""
    },
    {
      "fact": "requestData",
      "path": "$.event.assignedTo",
      "value": 321,
      "operator": "equal",
      "description": ""
    }
  ]
}
```
O que essa regra faz?

Ela é composta por duas condições:

- Quando um ticket com ID 123 for criado ou atualizado, a regra de finalização será aplicada, tornando o status da SLA e todas as suas metas para "Finalizado". 
- Quando um ticket pertencente a fila com ID 321 for criado ou atualizado, a regra de finalização será aplicada, tornando o status da SLA e todas as suas metas para "Finalizado". 

"any" significa que pelo menos uma das condições deve ser atendida para que a regra seja aplicada.

Para entender mais sobre a estrutura das condições, veja a documentação do [JSON Rules Engine](https://www.npmjs.com/package/json-rules-engine).