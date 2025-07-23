# Distribuição Automática de Chamados

## O que é

A **distribuição automática de chamados** é uma funcionalidade desenvolvida para atribuir chamados automaticamente aos atendentes de uma fila, **sem a necessidade de ação manual** (como clicar em "Pegar para mim").

O objetivo é garantir que o fluxo de atendimento seja contínuo e eficiente, **seguindo regras automatizadas** para distribuir os chamados.

Atualmente, o algoritmo utilizado para essa distribuição é o **Round Robin**, descrito abaixo.

---

## Como funciona atualmente

Atualmente, o QSM utiliza o algoritmo **Round Robin** como estratégia de distribuição automática de chamados. 

**O que é o Round Robin?**

O algoritmo Round Robin distribui os chamados de maneira cíclica entre os atendentes disponíveis na fila. Ele **não considera a carga de trabalho atual** ou a quantidade de chamados já atribuídos a cada atendente.

**Funcionamento:**

1. Existe uma lista ordenada de atendentes (ex: A, B, C).
2. Cada novo chamado é atribuído ao próximo da lista.
3. Quando o último da lista recebe um chamado, o algoritmo retorna ao primeiro e repete o ciclo.

**Exemplo com 3 atendentes (A, B, C)**

| Chamado | Atendente |
| --- | --- |
| 1 | A |
| 2 | B |
| 3 | C |
| 4 | A |
| 5 | B |
| 6 | C |

**Resumo**: O Round Robin garante distribuição justa por ordem, mas **não se preocupa com o número de chamados ativos** de cada atendente.

> ⚠️ Importante: Atualmente, a lógica de distribuição automática não considera SLAs distintos (como TPR menor ou maior) para priorização de chamados.
> 
> 
> A ordem de distribuição é baseada exclusivamente no algoritmo **Round Robin**, independentemente da urgência ou prioridade do SLA.
> 

---

## Situações que disparam a distribuição

A distribuição automática de chamados ocorre em três situações principais:

1. **Criação de chamado**
    
    O chamado pode ser criado já atribuído a um atendente ou sem responsável.
    
    Se houver atendente disponível com slot livre, o sistema faz a atribuição automática.
    
    Caso contrário, o chamado permanece na fila aguardando.
    
2. **Encerramento de chamado (status = Fechado)**
    
    Quando um atendimento é finalizado, o sistema verifica se o atendente ainda possui slots disponíveis.
    
    Se houver capacidade, o próximo chamado da fila é atribuído automaticamente.
    
3. **Mudança de status da comunicação para “Aguardando Cliente”**
    
    Quando o atendente altera o status da conversa para “Aguardando Cliente”, ele se torna elegível para novos atendimentos.
    
    O sistema então tenta alocar automaticamente o próximo chamado da fila.
    

> Observação: Quando o atendente está com um chamado ativo, ele sai temporariamente da fila de distribuição até liberar um slot.
> 

---

## Funcionamento da lógica

A lógica da distribuição automática é executada por meio de **eventos internos (hooks)**, que são acionados sempre que um chamado é criado ou atualizado (por exemplo, ao mudar de status).

O algoritmo segue os seguintes passos:

- Verifica se há chamados na fila sem responsável.
- Checa se o atendente possui slots disponíveis para novos atendimentos.
- Caso as condições sejam atendidas, o sistema executa uma ação interna que atribui automaticamente o próximo chamado ao atendente.

> A operação de alocação ocorre através de uma chamada interna protegida, que respeita regras de negócio e autenticação do sistema.
> 

Se não houver atendente disponível no momento, os chamados permanecem na fila até que alguém fique livre.

---

## Banco de dados e regras

A lógica de distribuição considera algumas configurações específicas:

- **Regras de distribuição por fila**: definem como os chamados devem ser atribuídos, de acordo com o tipo de atendimento ou prioridade.
- **Capacidade simultânea de atendimento**: cada grupo de atendentes tem um limite de atendimentos ativos simultâneos.
    
![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2F2025%2F07%2F59c64a9c80a4c539f9ccc0a1c4b8c24c.jpg?alt=media&token=85a7ba4e-55e8-44ee-ad0d-f1010a02ae56)
    
> O campo simultaneous_calls define a quantidade máxima de chamados ativos que um atendente pode ter ao mesmo tempo dentro de uma determinada fila (grupo).
> Isso evita sobrecarga e mantém o equilíbrio do atendimento.
    
- **Consulta de atendimentos ativos**: o sistema monitora em tempo real quantos chamados estão atribuídos a cada atendente.

Além disso, a lógica considera os seguintes dados para tomar decisões:

- **Status do ticket**: como Aberto, Fechado ou Aguardando Cliente.
- **Status da comunicação**: identifica quem respondeu por último, para entender se a conversa está ativa ou em espera.

---

# Exemplos

## 1. Criar Hook de `afterUpdate` na tabela `tickets`

Acesse **Gestão de Hooks** > Criar um **Novo Hook**

- **Tipo**: `model`
- **Recurso**: `tickets`
- **Evento**: `afterUpdate`
- **Handler**:

```json
{
  "url": "https://<sua-api>/api/v1/${orgSlug}/webhooks/tickets/after-update/allocate-ticket",
  "type": "http"
}
```

- **Condições**:
```json
{
  "all": [
    {
      "fact": "dataAfterEvent",
      "path": "$.ticketTypeId",
      "value": 3,
      "operator": "equal"
    },
    {
      "any": [
        {
          "fact": "requestData",
          "path": "$.body.status",
          "value": "Fechado",
          "operator": "equal"
        },
        {
          "fact": "requestData",
          "path": "$.body.communicationStatus",
          "value": "Aguardando cliente",
          "operator": "equal"
        }
      ]
    }
  ]
}
```
## 2. Criar Hook de `beforeUpdate` na tabela `users`

- **Tipo**: `model`
- **Recurso**: `users`
- **Evento**: `beforeUpdate`
- **Handler**:

```json
{
  "url": "https://<sua-api>/api/v1/${orgSlug}/webhooks/users/status/before-update/allocate-ticket",
  "type": "http"
}
```
- **Condições**:
```json
{
  "all": [
    {
      "fact": "requestData",
      "path": "$.body.status.length",
      "value": 1,
      "operator": "greaterThan"
    }
  ]
}
```
## 3. Configurar `ticketAllocationRules`
Acesse a base de dados `ticketAllocationRules` e crie uma nova regra.

- **Condições**:
```json
{
  "any": [
    {
      "fact": "requestData",
      "path": "$.event.existsTicketOnQueue",
      "value": false,
      "operator": "equal"
    },
    {
      "fact": "requestData",
      "path": "$.event.allocateTicketsToUser",
      "value": true,
      "operator": "equal"
    },
    {
      "fact": "requestData",
      "path": "$.event",
      "value": "ticketAllocation",
      "operator": "equal"
    }
  ]
}
```
- **includeToUserGroups**:

```json
[
  {
    "multiple": false,
    "tableName": "activeChatsByAttendant",
    "targetKey": "userId",
    "foreignKey": "userId"
  }
]

```

- **whereToUserGroups**:

```json
{
  "or": [
    {
      "$ActiveChatsByAttendant.count$": {
        "lt": "$QsmGroup.simultaneous_calls$"
      }
    },
    {
      "$ActiveChatsByAttendant.count$": {
        "is": null
      }
    }
  ],
  "groupId": "{{ '$ticket.assignedTo' }}",
  "$User.status$": ["Disponível"],
  "$User.user_profile_id$": 100028
}
```

## Considerações finais
- A fila de distribuição é composta por chamados sem responsável e com status ativo (não finalizados).

- A lógica de distribuição é acionada automaticamente e não depende de ações manuais dos usuários.

