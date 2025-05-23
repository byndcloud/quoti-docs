# Configurações em databases usando JVue

## Customizações em uma linha expandida da tabela

![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F7f50adacc36c58ba160601d2276cc52a.png?alt=media&token=c6e226ba-e716-4c3e-ac87-8bce76faed94)

![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F43614e6866723f972cd9e5f84f2eb9a4.png?alt=media&token=29b4cfe9-58a1-41a1-9f64-f35d729e0712)

![image.png](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F9a30f82a8946e26c5c9ae2515461ac74.png?alt=media&token=694128c7-d181-4e2a-b87c-8d41c00a9191)

### JSON Exemplo (defaultTab > ticketTable)
{% raw %}
```json
{
  "qtTableProps": {
  "showExpand": true,
  "hideDownloadBtn": "{{ $me.userProfileId === 100028 }}",
  "expandedComponent": {
    "tds": [
      {
        "components": [
          {
            "is": "SlotCSM",
            "style": {
              "width": "800px",
              "margin": "auto"
            },
            "children": [
              {
                "slug": "chat",
                "props": {
                  "height": "600px",
                  "ticket": "(expanded-panel){{ { ...$item, ChatRoom: { id: $item.ticketChatId } } }}",
                  "MessageList": {
                    "showMessageInput": "{{ true }}"
                  }
                }
              }
            ]
          }
        ]
      }
    ]
  }
}
}
```
{% endraw %}

## Customizações no header da tabela

![Untitled](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F2ae252c9435e5b3b9296df9adff447dd.png?alt=media&token=2ff4e412-8448-4844-9ad2-49c230214c86)

## Customizações em uma célula

![Untitled](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F9cd994ad9685d8f6835d4e981f01e6d1.png?alt=media&token=174677cb-e9de-4e58-9881-0212d702e2c8)

![Untitled](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F8453c447127c46b3d1981fe7ab2b69a7.png?alt=media&token=991639d9-1f62-4bdf-82b2-4c1dc54655eb)

![Untitled](https://firebasestorage.googleapis.com/v0/b/beyond-quoti.appspot.com/o/beyond%2Fquoti-docs%2F1981f4c57cdf23fc29b2b4e0a526dd83.png?alt=media&token=7d2d80f2-a6e0-4558-a8f9-f6239a09fdc3)

### JSON Exemplo
{% raw %}
```json
{
  "tabs": [
    {
      "path": "ticketTable",
      "slug": "ticketTable",
      "text": "",
      "filter": {
        "status": "Pré-cadastrado",
        "$TicketType.id$": 100047
      },
      "tabTitle": "Pré-cadastrados",
      "createTicketsInBatch": {
        "extraInfo": {
          "userId": "{{ $me.id }}",
          "version": "(ticketTable){{ $importBatch.middleFieldValues.version }}"
        },
        "middleField": [
          {
            "is": "v-text-field",
            "hint": "Este nome pode auxiliar na identificação de importações específicas no Histórico de Importações.",
            "dense": true,
            "label": "Nome da importação (opcional)",
            "v-model": "version",
            "outlined": true,
            "persistent-hint": true
          }
        ],
        "resumeField": {
          "slot": {
            "children": [
              {
                "slug": "csmTable",
                "props": {
                  "data": "(ticketTable){{ $importBatch?.result?.data?.data || [] }}",
                  "fields": "(ticketTable){{ $importBatch?.result?.data?.fields || [] }}"
                }
              }
            ]
          },
          "resumeInsertSheet": {
            "alert": true,
            "title": "(ticketTable){{ $importBatch?.result?.data?.processCounter + ' processos importados com sucesso' }}",
            "subtitle": "Confira abaixo a quantidade de processos importados por cliente.",
            "alertIcon": "mdi-clock",
            "alertText": "As informações de capa serão capturadas em até 60 dias úteis. Você pode acompanhar os resultados da importação no <strong>Histórico de importações.</strong>"
          }
        },
        "sheetColumns": [
          {
            "text": "Número do processo",
            "type": "text",
            "value": "Numero_do_processo",
            "sheetColumn": "Número do processo",
            "showInTable": true
          },
          {
            "text": "Data de recebimento",
            "type": "text",
            "value": "Data_de_recebimento",
            "sheetColumn": "Data de recebimento",
            "showInTable": true
          },
          {
            "text": "Responsável pelo cadastro",
            "type": "text",
            "value": "Responsavel_pelo_cadastro",
            "sheetColumn": "Responsável pelo cadastro",
            "showInTable": true
          },
          {
            "text": "Cliente",
            "type": "text",
            "value": "Cliente",
            "sheetColumn": "Cliente",
            "showInTable": true
          },
          {
            "text": "Processo eletrônico",
            "type": "text",
            "value": "Processo_eletronico",
            "sheetColumn": "Processo eletrônico",
            "showInTable": true
          }
        ],
        "exampleSheetLink": "https://storage.googleapis.com/beyond-quoti.appspot.com/CSM/martorelli-ccc/Acordos_Modelo.xlsx",
        "webhookSaveAction": "https://workflow.quoti.cloud/webhook/bulk-create-process"
      }
    },
    {
      "path": "ticketTable",
      "slug": "ticketTable",
      "text": "",
      "filter": {
        "status": "Ativo",
        "$TicketType.id$": 100047
      },
      "tabTitle": "Ativos"
    },
    {
      "path": "ticketTable",
      "slug": "ticketTable",
      "text": "",
      "filter": {
        "status": "Arquivados",
        "$TicketType.id$": 100047
      },
      "tabTitle": "Arquivados"
    }
  ],
  "defaultTab": {
    "order": [
      [
        "TicketGoals.expireAt",
        "ASC"
      ]
    ],
    "fields": [
      {
        "name": "id",
        "label": "id",
        "sortable": true
      },
      {
        "name": "ticketType100047AdditionalInfo.Numero_do_processo",
        "label": "Nº do processo",
        "sortable": false,
        "headerComponents": [
          {
            "is": "v-rating",
            "style": {
              "color": "red"
            },
            "value": 3
          }
        ]
      },
      {
        "name": "ticketType100047AdditionalInfo.Cliente_name",
        "label": "Cliente",
        "sortable": true,
        "components": [
          {
            "is": "v-chip",
            "close": true,
            "style": {
              "color": "primary"
            },
            "v-html": "(qt-table-cell){{ $value }}"
          }
        ],
        "headerComponents": [
          {
            "is": "span",
            "close": true,
            "style": {
              "color": "red"
            },
            "v-html": "(qt-table-header){{ $text }}"
          }
        ]
      },
      {
        "name": "createdAt",
        "label": "Data de recebimento",
        "convert": "{{ (input) => { return (input => {return $$moment(input).format('LLL')})(input) } }}",
        "sortable": false,
        "components": [
          {
            "is": "div",
            "qtChildren": [
              {
                "is": "h3",
                "style": {
                  "color": "red"
                },
                "v-text": "teste 1"
              },
              {
                "is": "div",
                "qtChildren": [
                  {
                    "is": "h3",
                    "style": {
                      "color": "red"
                    },
                    "v-text": "teste 2"
                  }
                ]
              }
            ]
          }
        ],
        "headerComponents": [
          {
            "is": "qt-tooltip",
            "top": true,
            "text": "Informações do Levi",
            "v-model": false,
            "qtChildren": [
              {
                "is": "span",
                "v-text": "btn do Levi 3",
                "#activator": " { on, attrs } "
              }
            ]
          }
        ]
      },
      {
        "name": "ticketType100047AdditionalInfo.Prazo_para_cadastro",
        "label": "Prazo para cadastro",
        "convert": "{{ ()=>{return 3} }}",
        "sortable": true,
        "components": [
          {
            "is": "span",
            "qtChildren": [
              "aaaa"
            ]
          }
        ]
      },
      {
        "name": "ticketType100047AdditionalInfo.Responsavel_pelo_cadastro_name",
        "label": "Responsável",
        "convert": "{{ () => {return 1} }}",
        "sortable": true,
        "components": [
          {
            "is": "v-rating",
            "style": {
              "color": "red"
            },
            "value": 1
          }
        ]
      },
      {
        "name": "ticketType100047AdditionalInfo.Status_pre_cadastro",
        "label": "Pré-cadastro",
        "convert": "{{ () => {return 100185} }}",
        "sortable": true,
        "components": [
          {
            "is": "qt-autocomplete",
            "v-if": true,
            "style": {
              "color": "red",
              "width": "250px"
            }
          }
        ]
      },
      {
        "name": "updatedAt",
        "type": "date",
        "label": "Última atualização",
        "sortable": true
      },
      {
        "name": "status",
        "label": "Salvar",
        "sortable": true,
        "components": [
          {
            "is": "v-btn",
            "v-if": "{{ true }}",
            "color": "primary",
            "style": {},
            "@click": "{{ (event) => { console.log('aa', event)} }}",
            "v-text": "Botão do Vini"
          }
        ]
      }
    ],
    "params": {
      "includeQueue": false,
      "additionalInfos": [
        {
          "ticketTypeId": 100047
        }
      ],
      "columnsToSearch": [
        "id",
        "createdAt",
        "updatedAt",
        "$ticketType100047AdditionalInfo.Numero_do_processo$",
        "$ticketType100047AdditionalInfo.Cliente$",
        "$ticketType100047AdditionalInfo.Prazo_para_cadastro$",
        "$ticketType100047AdditionalInfo.Responsavel_pelo_cadastro$",
        "$ticketType100047AdditionalInfo.status_pre_cadastro$"
      ],
      "includeSharedGroup": false,
      "includeTicketGoals": true,
      "includeAdditionalInfos": true
    },
    "createBtn": {
      "name": "Cadastrar"
    },
    "attributes": [
      "id",
      [
        "TicketType.name",
        "type"
      ],
      [
        "ticketRecipient.name",
        "ticketRecipientName"
      ],
      [
        "ticketAssignedToUser.name",
        "ticketAssignedToUserName"
      ],
      [
        "ticketCreator.name",
        "ticketCreatedByName"
      ],
      [
        "Category.name",
        "category"
      ],
      "description",
      "status",
      "createdAt",
      "updatedAt",
      "isAppointment",
      "scheduleDate",
      "duration",
      [
        "TicketGoals.expire_at",
        "goalsExpiresAt"
      ],
      [
        "TicketGoals.finished_at",
        "goalsFinishedAt"
      ],
      [
        "TicketGoals.paused_at",
        "goalsPausedAt"
      ]
    ],
    "ticketTable": {
      "qtTableProps": {
        "showExpand": false,
        "expandedComponent": {
          "tds": [
            {
              "components": [
                {
                  "props": {
                    "is": "qt-form-response",
                    "mode": "only-fields",
                    "style": {
                      "width": "100%"
                    },
                    "formId": 100265,
                    "sendBtn": "Salvar 1",
                    "showSaveBtn": true,
                    "formResponseId": 113021,
                    "enableUpdateResponses": true,
                    "useQtDatabasesEndpoints": true
                  }
                }
              ]
            }
          ]
        }
      }
    },
    "filterConfig": {
      "hideQueue": true,
      "hideCategory": true,
      "hideCreatedBy": true,
      "hideRecipient": true,
      "hideCreatedByt": true,
      "hideTicketType": true,
      "hideSectorTicket": true,
      "hideSearchByTitle": true,
      "assignedToUserWhere": {
        "userProfileId": 100024
      },
      "listAvailableToOrder": [
        {
          "name": "Tempo de criação",
          "value": "created_at"
        },
        {
          "name": "Última atualização",
          "value": "updatedAt"
        }
      ],
      "statusFromTicketTypes": [
        100046
      ]
    },
    "createDialogTitle": "Adicionar processos"
  },
  "ticketAgentOverview": {
    "general": {
      "extraTabsMaxWidth": 70,
      "extraTabsMinWidth": 20,
      "isExtraTabsResize": true,
      "initialExtraTabsWidth": 35
    },
    "listOneParams": {
      "additionalInfos": [
        {
          "ticketTypeId": 100047
        }
      ]
    }
  }
}
```
{% endraw %}