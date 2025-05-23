# Permissões QSM

Caso algumas dessas permissões ainda não exista em sua organização, você poderá criá-la pela tela /permissions

```json
{
  "ticketUserAction.messageIsPrivate.list": {
    "description": "Permite visualizar mensagens privadas"
  },
  "ticketUserAction.list": {
    "description": "Permite listar todas as actions"
  },
  "ticketUserAction.typeIsMessage.list": {
    "description": "Permite listar actions do tipo mensagem"
  },
 "ticket.updateStatusToCancel": {
    "description": "Permite cancelar um chamado"
	},
  "ticket.ui.dependentTickets":{
    "description": "Usuários com essa permissão verão os chamados associados"
  },
  "ticket.isManager": {
    "description": "Permissão para identificar um gestor"
  },
  "ticket.create": {
    "description": "Permite criar um ticket"
  },
  "sla.pause": {
    "description": "Permite pausar um SLA relativo a um ticket"
  },
  "sla.play": {
    "description": "Permite retomar um SLA que estava pausado relativo a um ticket"
  },
  "ticket.list": {
    "description": "Permite listar todos os tickets, independente de status e atribuição"
  },
  "ticket.onMyQueues.list": {
    "description": "Permite listar tickets da minha fila"
  },
  "ticket.createdByIsMe.list": {
    "description": "Permite listar todos os tickets criados pelo usuário conectado na plataforma"
  },
  "ticket.assignedUserIsMe.list": {
    "description": "Permite listar todos os tickets atribuídos ao usuário conectado na plataforma"
  },
  "ticket.assignedUserIsNull.list": {
    "description": "Permite listar todos os tickets não atribuídos à ninguém"
  },
  "ticket.recipientUserIsMe.list": {
    "description": "Permite listar os tickets cujo o recipient é o usuário conectado na plataforma"
  },
  "ticket.isApproval.updateStatus": {
    "description": "Permite aprovar um chamado"
  },
  "ticket.update": {
    "description": "Permite editar as informações base de qualquer ticket (não pode alterar o status)"
  },
  "ticket.assignedUserIsMe.update": {
    "description": "Permite editar as informações base de tickets atribuídos para o usuário conectado na plataforma"
  },
  "ticket.updateStatus": {
    "description": "Permite alterar o status de qualquer ticket"
  },
  "ticket.assignedUserIsMe.updateStatus": {
    "description": "Permite alterar o status de um ticket atribuído ao usuário conectado na plataforma"
  },
  "ticket.updateAssignedUserToAnyUser": {
    "description": "Permite atribuir um ticket para qualquer outro usuário"
  },
  "ticket.updateAssignedUserToMe": {
    "description": "Permite atribuir um ticket para o próprio usuário conectado na plataforma"
  },
  "ticket.assignedUserIsMe.updateAssignedUserToNull": {
    "description": "Permite remover a sua atribuição a um ticket específico"
  },
  "ticket.delete": {
    "description": "Permite apagar qualquer ticket"
  },
  "ticket.createdByIsMe.delete": {
    "description": "Permite apagar qualquer ticket criado para o usuário conectado na plataforma"
  },
  "ticket.ui.create.onlyAtCatalog": {
    "description": "Usuários com essa permissão não poderão criar chamados pela tela de Tickets e Meus tickets"
  },
  "ticketType.upsertConfigs": {
    "description": "Permite alterar a configuração avançada salva no tipo do chamado"
  },
  "category.upsertConfigs": {
    "description": "Permite alterar a configuração avançada salva na categoria do chamado"
  }
}
```