# JSON Schema Configurações avançadas de layout
=== "JSON"

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "definitions": {
      "executeMock": {
        "type": "boolean",
        "description": "Indica se devemos utilizar a configuração mockada. Este parâmetro só é útil pelos desenvolvedores do Quoti-CSM e quando a extensão encontra-se no /develop",
        "example": true
      },

      "defaultTab": {
        "type": "object",
        "description": "Propriedade responsável pela customização da tabela principal mostrada na tela do workspace. Exemplo de configuração que podemos realizar com essa prop: customizar colunas, títulos, filtros, itens que serão exibidos, número itens por página e literlamente qualquer caracteristica presente na tabela. Caso sua customização não encontre disponível, comunique sua ideia à equipe do Quoti",
        "properties": {
          "fields": {
        "type": "array",
        "description": "Todas as colunas que estarão presentes na tabela. Podemos configurar nome, ordem e valor que será exibido em cada célula da coluna.",
        "items": {
          "type": "object",
          "properties": {
            "name": {
              "type": "string",
              "description": "Nome da propriedade retornada pelo CSM-API",
              "example": "id"
            },
            "label": {
              "type": "string",
              "description": "Nome que será exibido na coluna da tabela",
              "example": "Número"
            },
            "sortable": {
              "type": "boolean",
              "description": "Indica se a coluna é ordenável",
              "example": true
            },
            "convert": {
              "type": "string",
              "description": "Função usada para converter o valor da célula. Utilizada para criar colunas computadas",
              "example": "qtFunction return (input => input?.length ? this.shortName(input) : 'Não definido')(input)"
            }
          }
        }
      },
          "params": {
            "type": "object",
            "description": "Essa prop será utilizada na requisição https de listagem dos tickets. Ou seja, nela podemos especificar qualquer campo listado na documentação https://api.csm.quoti.cloud/api-explorer/#/Tickets/get__orgSlug__tickets",
            "properties": {
              "includeQueue": {
                "description": "Incluir informações da fila associada",
                "type": "boolean",
                "example": false
              },
              "includeSharedGroup": {
                "type": "boolean",
                "description": "Incluir chamados que possuem grupo compartilhado",
                "example": false
              },
              "includeTicketGoals": {
                "type": "boolean",
                "description": "Incluir metas do chamado",
                "example": true
              },
              "limit": {
                "type": "number",
                "description": "Lista a quantidade de itens que será retornada por página",
                "example": 15
              }
            }
          },
          "createTicket": {
            "type": "boolean",
            "description": "Indica se é permitido criar chamado pela tela do workspace",
            "example": true
          },
          "createDialogTitle": {
            "type": "string",
            "description": "Título da criação do chamado",
            "example": "Adicionar chamado"
          },
          "attributes": {
            "type": "array",
            "description": "Indica a prop attributes presente na listagem dos chamados. ",
            "items": {
              "anyOf": [
                {
                  "type": "string",
                  "description": "Nome da propriedade retornada pelo CSM API",
                  "example": "id"
                },
                {
                  "type": "array",
                  "description": "Nome da propriedade retornada pelo CSM API seguida de um alias",
                  "items": [
                    {
                      "type": "string",
                      "description": "Nome da propriedade",
                      "example": "TicketType.name"
                    },
                    {
                      "type": "string",
                      "description": "Alias",
                      "example": "type"
                    }
                  ]
                }
              ]
            }
          },
          "filter": {
            "type": "object",
            "description": "Essa propriedade será passada para o parâmetro where da requisição de listagem dos chamados. Com esse parâmetro conseguimos especificar quais chamados deverão ser exibidos de acordo com suas características. Observe pelos exemplos que é possível filtrar por propriedades de modelos que estão associado ao chamado. Ou seja, é possível filtrar pelo ticketType, categoria, fila, informações adicionais entre outros",
            "properties": {
              "status": {
                "type": "array",
                "items": {
                  "type": "string",
                  "description": "Filtra por status"
                },
                "example": ["Em Andamento", "Pausado", "Aberto"]
              },
              "assignedToUser": {
                "type": "array",
                "items": {
                  "type": "number",
                  "description": "Atribuído ao usuário"
                },
                "example": ["run qtFunction return this.me?.id"]
              },
              "isAppointment": {
                "type": "boolean",
                "description": "Filtra por chamados agendados",
                "example": false
              },
              "$TicketType.name$": {
                "type": "object",
                "properties": {
                  "not": {
                    "type": "array",
                    "items": {
                      "type": "string",
                      "description": "Filtra por nome do tipo do chamado"
                    },
                    "example": ["Aprovação"]
                  }
                }
              },
              "$TicketType.id$": {
                "type": "object",
                "properties": {
                  "not": {
                    "type": "array",
                    "items": {
                      "type": "string",
                      "description": "Excluir"
                    },
                    "example": [
                      "run qtFunction return this.constants.TICKET_TYPE_CHAT_ID"
                    ]
                  }
                }
              }
            }
          },
          "filterConfig": {
            "type": "object",
            "description": "Configura propriedade que serão exibidas no componente filtro",
            "properties": {
              "hideQueue": {
                "type": "boolean",
                "description": "Indica se será possível filtrar pela fila",
                "example": true
              },
              "hideRecipient": {
                "type": "boolean",
                "description": "Indica se será possível filtrar pelo destinatário",
                "example": true
              },
              "disableCategory": {
                "type": "boolean",
                "description": "Indica se será possível filtrar pela categoria",
                "example": true
              },
              "hideSectorTicket": {
                "type": "boolean",
                "description": "Indica se será possível filtrar por chamados compartilhados",
                "example": true
              },
              "disableTicketType": {
                "type": "boolean",
                "description": "Indica se será possível filtrar pelo tipo do chamado",
                "example": true
              },
              "hideAssignedToUser": {
                "type": "boolean",
                "description": "Indica se será possível filtrar pelo responsável do chamado",
                "example": true
              },
              "notIncludeStatusFromTicketTypes": {
                "type": "array",
                "items": {
                  "type": "string",
                  "description": "Importante quando queremos listar apenas status de um determinado tipo no seletor de status do filtro",
                  "example": [
                    "run qtFunction return this.constants.TICKET_TYPE_CHAT_ID"
                  ]
                }
              }
            }
          },
          "order": {
            "type": "array",
            "description": "Indica a ordem padrão dos chamados, assim que a página é carregada",
            "items": {
              "type": "array",
              "items": [
                {
                  "type": "string",
                  "description": "Campo de ordenação",
                  "example": "TicketGoals.expireAt"
                },
                {
                  "type": "string",
                  "description": "Direção da ordenação",
                  "example": "ASC"
                }
              ]
            }
          }
        }
      }
    },
    "workspace": {
      "executeMock": { "$ref": "#/definitions/executeMock" },
      "defaultTab": { "$ref": "#/definitions/defaultTab" },
      "tabs": {
        "description": "Permite realizar customização a nivel de tabs.",
        "type": "array",
        "items": {
          "allOf": [
            { "$ref": "#/definitions/defaultTab" },
            {
              "type": "object",
              "properties": {
                "slug": {
                  "type": "string",
                  "description": "Identificador da tabela. É boa prática não usar espaço e nem caracteres especiais",
                  "example": "ticketTable"
                },
                "path": {
                  "type": "string",
                  "description": "É boa prática deixar idêntico ao slug",
                  "example": "ticketTable"
                },
                "text": {
                  "type": "string",
                  "description": "Título da tabela",
                  "example": "Chamados em aberto que sou o responsável"
                },
                "tabTitle": {
                  "type": "string",
                  "description": "Título da aba",
                  "example": "Meus chamados"
                }
              }
            }
          ]
        }
      },
      "ticketAgentOverview": { "$ref": "#/ticketTypeOrCategory" },
      "useTicketOverview": {
        "type": "boolean",
        "description": "Usar a antiga visão geral do chamado",
        "example": false
      }
    },
    "ticketTypeOrCategory": {
      "type": "object",
      "properties": {
        "params": { "$ref": "#/definitions/executeMock" },
        "texts": {
          "type": "object",
          "description": "Indica a configuração de textos que está presente na tela ticketAgentOverview",
          "properties": {
            "title": {
              "type": "string",
              "description": "Título da tela",
              "examples": ["Chamado", "Ativo"]
            },
            "sidebar": {
              "type": "object",
              "properties": {
                "openBy": {
                  "type": "string",
                  "description": "Texto na sideBar indicando quem criou o chamado",
                  "examples": ["Criado por"]
                }
              }
            },
            "generalInfo": {
              "type": "object",
              "properties": {
                "title": {
                  "type": "string",
                  "description": "Título da seção de informações gerais",
                  "examples": ["Informações Gerais"]
                },
                "ticketName": {
                  "type": "string",
                  "description": "Nome do chamado",
                  "examples": ["Chamado", "Ativo"]
                },
                "deleteTicketBtn": {
                  "type": "string",
                  "description": "Texto do botão para apagar chamado",
                  "examples": ["Apagar chamado", "Apagar ativo"]
                }
              }
            },
            "ticketActions": {
              "type": "object",
              "description": "Descreve textos relacionados a seção de ações do chamado",
              "properties": {
                "title": {
                  "type": "string",
                  "description": "Título da seção de ações do chamado",
                  "examples": ["Eventos do chamado", "Eventos do ativo"]
                },
                "createBtn": {
                  "type": "string",
                  "description": "Texto do botão para criar uma mensagem",
                  "examples": ["Enviar"]
                },
                "ticketKind": {
                  "type": "string",
                  "description": "Natureza do chamado",
                  "examples": ["chamado", "ativo"]
                },
                "createPrivatelyBtn": {
                  "type": "string",
                  "description": "Texto do botão para criar nota privada",
                  "examples": ["Criar nota privada"]
                },
                "ticketCreatedHeader": {
                  "type": "string",
                  "description": "Título que será exibido na ação 'criação de um chamado'",
                  "examples": ["Criação do chamado", "Criação do ativo"]
                },
                "ticketUpdatedHeader": {
                  "type": "string",
                  "description": "Título que será exibido na ação 'criação de um chamado'",
                  "examples": ["Atualização do chamado", "Atualização do ativo"]
                }
              }
            },
            "additionalInfo": {
              "type": "object",
              "properties": {
                "title": {
                  "type": "string",
                  "description": "Título da seção de informações adicionais",
                  "examples": ["Dados da Demanda", "Informações Adicionais"]
                },
                "saveBtn": {
                  "type": "string",
                  "description": "Texto do botão para salvar informações",
                  "examples": ["Salvar informações"]
                }
              }
            },
            "cancelTicketBtn": {
              "type": "string",
              "description": "Texto do botão para cancelar chamado",
              "examples": ["Cancelar chamado", "Cancelar ativo"]
            },
            "downloadTicketBtn": {
              "type": "string",
              "description": "Texto do botão para baixar dados do chamado",
              "examples": ["Baixar dados"]
            }
          }
        },
        "extraTabs": {
          "type": "array",
          "items": {
            "allOf": [
              { "$ref": "#/definitions/defaultTab" },
              {
                "type": "object",
                "properties": {
                  "condition": {
                    "type": "boolean",
                    "description": "Indica se a tab irá ou não aparecer",
                    "examples": [true, false]
                  },
                  "slug": {
                    "type": "string",
                    "description": "Slug da guia extra",
                    "examples": ["details", "ticketChildren"]
                  },
                  "path": {
                    "type": "string",
                    "description": "Caminho da guia extra. é boa prática o path ser igual ao slug",
                    "examples": ["details", "ticketChildren"]
                  },
                  "text": {
                    "type": "string",
                    "description": "Título da guia extra",
                    "examples": ["Detalhes", "Atendimentos Associados"]
                  }
                }
              }
            ],
            "createTicketCatalog": {
              "type": "string",
              "description": "Catálogo de criação de chamado"
            }
          },
          "description": "Configuração de guias extras"
        },
        "hideSLAs": {
          "type": "boolean",
          "description": "Indica se a tab ticketSla será mostrada. [Deprecated] Use ticket extraTabs[0].condition",
          "examples": [true]
        },
        "hideQueue": {
          "type": "boolean",
          "description": "Indica se a tab ticketSla será mostrada. [Deprecated] Use ticket extraTabs[0].condition",
          "examples": [true, false]
        },
        "hideMedias": {
          "type": "boolean",
          "description": "Indica se a tab ticketSla será mostrada. [Deprecated] Use ticket extraTabs[0].condition",
          "examples": [true, false]
        },
        "hidePriority": {
          "type": "boolean",
          "description": "Indica se a prioridade está oculta",
          "examples": [true]
        },
        "readOnlyType": {
          "type": "boolean",
          "description": "Indica se é possivel alterar o tipo do chamado",
          "examples": [true]
        },
        "executeMock": {
          "type": "boolean",
          "description": "Indica se devemos utilizar a configuração mockada. Este parâmetro só é útil pelos desenvolvedores do Quoti-CSM e quando a extensão encontra-se no /develop",
          "example": true
        },
        "ticketTypeId": {
          "type": "integer",
          "description": "ID do tipo de chamado. Usado em conjunto com a prop executeMock. Aplicará mock apenas nos chamados que possuirem esse tipo de chamado",
          "examples": [100049, 4]
        },
        "sideBarTabs": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "icon": {
                "type": "string",
                "description": "Ícone da barra lateral",
                "examples": ["mdi-information"]
              },
              "path": {
                "type": "string",
                "description": "Caminho da guia da barra lateral",
                "examples": ["attachments", "recordInfo", "agentAssist"]
              },
              "slug": {
                "type": "string",
                "description": "Slug da guia da barra lateral",
                "examples": ["attachments", "recordInfo", "agentAssist"]
              },
              "text": {
                "type": "string",
                "description": "Texto da guia da barra lateral",
                "examples": ["Informações"]
              }
            }
          },
          "description": "Configuração de guias da barra lateral"
        },
        "actionButtons": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "text": {
                "type": "string",
                "description": "Texto do botão de ação",
                "examples": ["Criar Novo Atendimento", "Nova configuração"]
              },
              "type": {
                "type": "string",
                "description": "Tipo do botão de ação",
                "examples": ["createTicket", "updateTicket", "general"]
              },
              "color": {
                "type": "string",
                "description": "Cor do botão de ação",
                "examples": ["primary"]
              },
              "dense": {
                "type": "boolean",
                "description": "Indica se o botão de ação é denso",
                "examples": [true]
              },
              "outlined": {
                "type": "boolean",
                "description": "Indica se o botão de ação é contornado",
                "examples": [false]
              },
              "condition": {
                "type": "boolean",
                "description": "Condição para exibição do botão de ação",
                "examples": [true]
              },
              "depressed": {
                "type": "boolean",
                "description": "Indica se o botão de ação é deprimido",
                "examples": [true]
              },
              "action": {
                "type": "string",
                "description": "Ação associada ao botão de ação",
                "examples": [
                  "qtFunction return this.createChildrenTicket({'headerTitle': 'Criar novo Atendimento','categoryId': 100045, 'ticketTypeId': 100050, 'ticketKind': 'atendimento'})",
                  "qtFunction return this.createChildrenTicket({'headerTitle': 'Manutenção - Ajuste Configuração','categoryId': 100771, 'ticketKind': 'movimentação'})"
                ]
              }
            }
          },
          "description": "Configuração de botões de ação"
        },
        "hideApprovals": {
          "type": "boolean",
          "description": "Indica se as aprovações estão ocultas",
          "examples": [true]
        },
        "readOnlyStatus": {
          "type": "boolean",
          "description": "Indica se o status é somente leitura",
          "examples": [true, false]
        },
        "serviceCatalog": {
          "type": "object",
          "properties": {
            "skipConfirmationBeforeCreation": {
              "description": "Indica se a confirmação deve ser ignorada antes da criação.",
              "type": "boolean",
              "examples": [true, false]
            }
          }
        },
        "readOnlyCategory": {
          "description": "Indica se a categoria é somente leitura.",
          "type": "boolean",
          "examples": [true, false]
        },
        "createTicketCatalog": {
          "description": "Catálogo utilizado para criar tickets.",
          "type": "integer",
          "examples": [153727]
        },
        "skipConfirmationBeforeCreation": {
          "description": "Indica se a confirmação deve ser ignorada antes da criação.",
          "type": "boolean",
          "examples": [true, false]
        },
        "hideDependentTickets": {
          "description": "Indica se os tickets associados devem ser ocultados.",
          "type": "boolean",
          "examples": [true, false]
        }
      }
    }
  }
}
```