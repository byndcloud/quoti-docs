# Data mapping QSM

O objetivo dessa página é elencar as principais tabelas usadas em nosso sistema QSM, que podem gerar dúvidas para um desenvolvedor em primeiro contato com nosso banco.

<aside>
💡

Essa página será atualizada conforme forem surgindo dúvidas sobre o entendimento das demais tabelas do sistema. 

</aside>

<aside>
⚠️

Não é o objetivo dessa página comentar sobre todas as tabelas do sistema, pois seria uma atividade bastante cansativa para o leitor, além de desnecessária para diversas tabelas que por si só apresentam significado óbvio.

Por se tratar de um sistema low code, no qual os próprios desenvolvedores de cada organização podem criar seus próprios databases, fica inviável a documentação de todas a tabelas.

</aside>

# Considerações importantes

1. Usamos o banco MySQL, mas costumamos liberar o acesso aos dados (apenas leitura) através do [BigQuery](https://cloud.google.com/bigquery/?hl=pt_br).
2. Os databases criados na plataforma (pela tela /databases, por exemplo) são visualizações do MySQL, segue um exemplo:
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/8ed191e4-8f47-4799-9ebd-b3495757dbcd/image.png)
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/ffcf5423-dc07-404e-b21a-b532ed2cfb13/image.png)
    
3. Tabelas que possuem o prefixo `tables_` são databases materializados.
4. Databases não materializados não são escaláveis, evite usar em aplicações que possuem muito registros.
    
    <aside>
    ⚠️
    
    No futuro, todos os database serão materializados
    
    </aside>
    
5. As colunas slug presente em diversas tabelas não devem ser inserido string com caracteres especiais.
    
    A coluna slug é uma identificação única e legível usada naquela tabela.
    
    - Código para transformar qualquer texto em um slug.
        
        ```jsx
        // Usamos a função abaixo quando queremos criar um slug
        function slugify(
          str,
          separator = '-',
          { transformToLowerCase = true } = {}
        ) {
          if (!str) {
            return str
          }
          str = str.replace(/^\s+|\s+$/g, '') // trim
        
          if (transformToLowerCase) {
            str = str.toLowerCase()
          }
        
          // remove accents, swap ñ for n, etc
          const from = 'ãàáäâèéëêìíïîòóöôùúüûñç·/_,:;'
          const to = 'aaaaaeeeeiiiioooouuuunc------'
          for (let i = 0, l = from.length; i < l; i++) {
            str = str.replace(new RegExp(from.charAt(i), 'g'), to.charAt(i))
          }
        
          str = str
            .replace(/[^a-zA-Z0-9 -]/g, '') // remove invalid chars
            .replace(/\s+/g, separator) // collapse whitespace and replace by -
            .replace(/-+/g, separator) // collapse dashes
        
          return str
        }
        ```
        
6. Coluna name presente em diversas tabelas é em sua maioria no formato slug.
7. Todas as nossas tabelas possuem created_at, updated_at e deleted_at e estão no formato datatime com fuso zero, portanto lembre de converter sua data para o fuso zero. 
    
    Por padrão, essas datas são preenchidas automaticamente quando as operações no banco é feita através das nossas APIs.
    
8. Usamos o padrão snake_case para nossas tabelas e camelCase para os códigos presentes em nossas API ou rotinas internas (N8N e customizações especiais no front-end).
9. Coluna sync presente em algumas tabelas indica que aquela linha foi criada/atualizada de maneira automática por alguma rotina de automação.
10. Informação adicional do tipo, da categoria, do perfil ou de qualquer outra entidade é sempre um database, portanto sempre terá o prefixo `tables_data`, em caso de databases não materializados, ou `tables_`, em caso de databases materializados.

# ER
- Arquivo com principais tabelas do ER usado no qsm   
    ```bash
    Table "users" {
      "id" int [pk, not null, increment]
      "uuid" varchar(255) [default: NULL]
      "name" varchar(255) [not null]
      "gender" varchar(255) [default: NULL]
      "user" varchar(255) [not null]
      "pass" varchar(255) [default: NULL]
      "password_strength" varchar(255) [default: NULL]
      "cpf" varchar(255) [default: NULL]
      "email" varchar(255) [default: NULL]
      "telefones" varchar(255) [default: NULL]
      "matricula" varchar(255) [default: NULL]
      "ppessoa_codigo" int [default: NULL]
      "token" varchar(964) [default: NULL]
      "token_password_reset" varchar(255) [default: NULL]
      "token_password_reset_expiration_time" datetime [default: NULL]
      "token_password_reset_times" int [default: NULL]
      "registered" tinyint(1) [not null, default: '0']
      "last_login_time" datetime [default: NULL]
      "user_created" users_user_created_enum [not null, default: 'false']
      "created_time" timestamp [default: `CURRENT_TIMESTAMP`]
      "updated_time" timestamp [default: NULL]
      "user_profile_id" int [default: NULL]
      "sync" tinyint(1) [default: '0']
      "form_response_id" int [default: NULL]
      "bio" varchar(500) [default: NULL]
      "birthday" varchar(10) [default: NULL]
      "instagram_url" varchar(255) [default: NULL]
      "facebook_url" varchar(255) [default: NULL]
      "twitter_url" varchar(255) [default: NULL]
      "linkedin_url" varchar(255) [default: NULL]
      "expires_at" datetime [default: NULL]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
      "media_id" int [default: NULL]
    
      Indexes {
        user_profile_id [name: "user_profile_id"]
        media_id [name: "media_id"]
        cpf [name: "users_cpf"]
      }
    }
    
    Table "users_profiles" {
      "id" int [pk, not null, increment]
      "name" varchar(255) [default: NULL]
      "name_plural" varchar(255) [default: NULL]
      "slug" varchar(255) [not null]
      "color" varchar(255) [default: NULL]
      "home_page_default" varchar(255) [default: NULL]
      "can_self_create_account" tinyint(1) [default: NULL]
      "form_id" int [default: NULL]
      "require_password_strength" int [not null, default: '2']
      "webhook_url" varchar(255) [default: NULL]
      "webhook_type" users_profiles_webhook_type_enum [default: NULL]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
      "config_login_method_id" int [default: NULL]
      "mfa_type" users_profiles_mfa_type_enum [default: NULL]
      "query_params" varchar(255) [default: NULL]
    
      Indexes {
        slug [unique, name: "slug"]
        config_login_method_id [name: "config_login_method_id"]
      }
    }
    
    Table "groups" {
      "id" int [pk, not null, increment]
      "parent_id" int [default: NULL]
      "product_id" int [default: NULL]
      "cod" varchar(255) [not null]
      "parent_cod" varchar(255) [default: NULL]
      "name" varchar(255) [not null]
      "display_name" varchar(255) [default: NULL]
      "status" varchar(255) [default: 'active']
      "type" varchar(255) [not null]
      "telefone" varchar(255) [default: NULL]
      "year" varchar(255) [default: NULL]
      "grades_template_id" int [default: NULL]
      "sync" tinyint(1) [default: '0']
      "form_response_id" int [default: NULL]
      "acquisition_flow" json [default: NULL]
      "ingress_start_date" datetime [default: NULL]
      "ingress_end_date" datetime [default: NULL]
      "limit_members" int [default: NULL]
      "can_be_seen_by" json [default: NULL]
      "can_be_accessed_by" json [default: NULL]
      "preregistration" tinyint(1) [default: '0']
      "waiting_list" tinyint(1) [default: '0']
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
      "created_by" int [default: NULL]
    
      Indexes {
        (cod, type) [unique, name: "groups_cod_type"]
        parent_id [name: "parent_id"]
        product_id [name: "product_id"]
        type [name: "type"]
        grades_template_id [name: "grades_template_id"]
        form_response_id [name: "form_response_id"]
        created_by [name: "groups_created_by_foreign_idx"]
      }
    }
    
    Table "permissions" {
      "id" int [pk, not null, increment]
      "name" varchar(255) [default: NULL]
      "description" varchar(255) [default: NULL]
      "type" varchar(255) [default: NULL]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
    }
    
    Table "tickets" {
      "id" int [pk, not null, increment]
      "name" varchar(255) [default: NULL]
      "email" varchar(255) [default: NULL]
      "recipient" int [default: NULL]
      "description" varchar(255) [default: NULL]
      "ticket_type_id" int [default: NULL]
      "assigned_to" int [default: NULL]
      "status" varchar(255) [default: NULL]
      "form_response_id" int [default: NULL]
      "created_by" int [default: NULL]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
      "priority" tickets_priority_enum [default: NULL]
      "assigned_to_user" int [default: NULL]
      "parent_ticket_id" int [default: NULL]
      "category_id" int [default: NULL]
      "body" longtext [not null]
      "ticket_type_form_response_id" int [default: NULL]
      "shared_group_id" int [default: NULL]
      "is_appointment" tinyint(1) [not null, default: '0']
      "schedule_date" datetime [default: NULL]
      "duration" time [default: NULL]
      "scheduled_at" datetime [default: NULL]
      "service_catalog_item_id" int [default: NULL]
      "calendar_id" int [default: NULL]
      "approval_group_id" int [default: NULL]
    
      Indexes {
        recipient [name: "recipient"]
        ticket_type_id [name: "ticket_type_id"]
        assigned_to [name: "assigned_to"]
        form_response_id [name: "form_response_id"]
        created_by [name: "created_by"]
        assigned_to_user [name: "tickets_assigned_to_user_foreign_idx"]
        parent_ticket_id [name: "tickets_parent_ticket_id_foreign_idx"]
        category_id [name: "tickets_category_id_foreign_idx"]
        ticket_type_form_response_id [name: "tickets_ticket_type_form_response_id_foreign_idx"]
        shared_group_id [name: "tickets_shared_group_id_foreign_idx"]
        approval_group_id [name: "tickets_approval_group_id_foreign_idx"]
      }
    }
    
    Table "chat_rooms" {
      "id" varchar(255) [pk, not null]
      "chat_status_id" int [default: NULL]
      "type" chat_rooms_type_enum [default: NULL]
      "requester_id" int [default: NULL]
      "created_by" int [default: NULL]
      "empty" tinyint(1) [default: '1']
      "product_id" int [default: NULL]
      "members_count" int [default: NULL]
      "icon" varchar(255) [default: NULL]
      "name" varchar(255) [default: NULL]
      "webhook_url" varchar(255) [default: NULL]
      "json_data" json [default: NULL]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
      "assigned_to" int [default: NULL]
      "ticket_id" int [default: NULL]
      "first_reply_at" datetime [default: NULL]
      "last_reply_at" datetime [default: NULL]
      "last_reply_author" int [default: NULL]
      "last_reply_message" longtext
    
      Indexes {
        chat_status_id [name: "chat_status_id"]
        requester_id [name: "requester_id"]
        created_by [name: "created_by"]
        assigned_to [name: "chat_rooms_assigned_to_foreign_idx"]
        ticket_id [name: "chat_rooms_ticket_id_foreign_idx"]
        last_reply_author [name: "chat_rooms_last_reply_author_foreign_idx"]
      }
    }
    
    Table "ticket_user_actions" {
      "id" int [pk, not null, increment]
      "details" longtext
      "data" json [default: NULL]
      "ticket_id" int [default: NULL]
      "created_by" int [default: NULL]
      "is_automatic_message" tinyint(1) [default: NULL]
      "type" ticket_user_actions_type_enum [default: NULL]
      "description" varchar(255) [default: NULL]
      "ticket_type_id" int [default: NULL]
      "assigned_to_user" int [default: NULL]
      "status" varchar(255) [default: NULL]
      "priority" ticket_user_actions_priority_enum [default: NULL]
      "media_id" int [default: NULL]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
      "category_id" int [default: NULL]
      "assigned_to" int [default: NULL]
      "recipient" int [default: NULL]
    
      Indexes {
        ticket_id [name: "ticket_id"]
        created_by [name: "created_by"]
        ticket_type_id [name: "ticket_type_id"]
        assigned_to_user [name: "assigned_to_user"]
        media_id [name: "media_id"]
        category_id [name: "ticket_user_actions_category_id_foreign_idx"]
        assigned_to [name: "ticket_user_actions_assigned_to_foreign_idx"]
        recipient [name: "ticket_user_actions_recipient_foreign_idx"]
      }
    }
    
    Table "ticket_types" {
      "id" int [pk, not null, increment]
      "name" varchar(255) [not null]
      "type" varchar(255) [not null]
      "icon" varchar(255) [not null]
      "color" varchar(255) [not null]
      "description" varchar(255) [not null]
      "default_sla_id" int [default: NULL]
      "form_id" int [default: NULL]
      "created_by" int [default: NULL]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
      "configs" json [default: NULL]
    
      Indexes {
        form_id [name: "form_id"]
        default_sla_id [name: "ticket_types_ibfk_1"]
      }
    }
    
    Table "categories" {
      "id" int [pk, not null, increment]
      "name" varchar(255) [not null]
      "type" varchar(255) [not null]
      "icon" varchar(255) [not null]
      "color" varchar(255) [not null]
      "description" varchar(255) [not null]
      "form_id" int [default: NULL]
      "created_by" int [default: NULL]
      "default_sla_id" int [default: NULL]
      "default_group_id" int [default: NULL]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
      "active" tinyint(1) [default: '1']
      "duration" time [default: NULL]
      "bookable" tinyint(1) [not null, default: '0']
      "concurrency" int [default: NULL]
      "min_schedule_time" time [default: NULL]
      "max_schedule_time" time [default: NULL]
      "open_for_schedule" tinyint(1) [default: NULL]
      "configs" json [default: NULL]
    
      Indexes {
        form_id [name: "form_id"]
        created_by [name: "created_by"]
        default_sla_id [name: "default_sla_id"]
        default_group_id [name: "default_group_id"]
      }
    }
    
    Table "calendars" {
      "id" int [pk, not null, increment]
      "title" varchar(255) [not null]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
    }
    
    Table "calendars_default_hours" {
      "id" int [pk, not null, increment]
      "calendar_id" int [default: NULL]
      "day" calendars_default_hours_day_enum [not null]
      "start" time [not null]
      "end" time [not null]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
    
      Indexes {
        calendar_id [name: "calendar_id"]
      }
    }
    
    Table "calendars_special_hours" {
      "id" int [pk, not null, increment]
      "calendar_id" int [default: NULL]
      "date" date [not null]
      "start" time [not null]
      "end" time [not null]
      "increase_hours" tinyint(1) [not null]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
    
      Indexes {
        calendar_id [name: "calendar_id"]
      }
    }
    
    Table "sla_goals" {
      "id" int [pk, not null, increment]
      "sla_id" int [default: NULL]
      "goal_id" int [default: NULL]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
    
      Indexes {
        (sla_id, goal_id) [unique, name: "sla_goals_slaId_goalId_unique"]
        goal_id [name: "goal_id"]
      }
    }
    
    Table "slas" {
      "id" int [pk, not null, increment]
      "name" varchar(255) [default: NULL]
      "description" varchar(255) [default: NULL]
      "default_assigned_to" int [default: NULL]
      "active" tinyint(1) [default: NULL]
      "calendar_id" int [default: NULL]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
    
      Indexes {
        default_assigned_to [name: "default_assigned_to"]
        calendar_id [name: "calendar_id"]
      }
    }
    
    Table "ticket_SLA" {
      "id" int [pk, not null, increment]
      "assigned_to" int [default: NULL]
      "ticket_id" int [default: NULL]
      "sla_id" int [default: NULL]
      "started_at" datetime [default: NULL]
      "finished_at" datetime [default: NULL]
      "expires_at" datetime [default: NULL]
      "created_at" datetime [not null]
      "updated_at" datetime [not null]
      "deleted_at" datetime [default: NULL]
    
      Indexes {
        (ticket_id, sla_id) [unique, name: "ticket_SLA_sla_id_ticket_id_unique"]
        assigned_to [name: "assigned_to"]
        sla_id [name: "sla_id"]
      }
    }
    
    Ref "chat_rooms_last_reply_author_foreign_idx":"users"."id" < "chat_rooms"."last_reply_author"
    
    Ref "chat_rooms_ticket_id_foreign_idx":"tickets"."id" < "chat_rooms"."ticket_id"
    
    Ref "groups_created_by_foreign_idx":"users"."id" < "groups"."created_by" [update: cascade]
    
    Ref "sla_ibfk_1":"groups"."id" < "slas"."default_assigned_to" [update: cascade, delete: set null]
    
    Ref "ticket_SLA_ibfk_1":"groups"."id" < "ticket_SLA"."assigned_to" [update: cascade, delete: set null]
    
    Ref "ticket_SLA_ibfk_2":"tickets"."id" < "ticket_SLA"."ticket_id" [update: cascade, delete: cascade]
    
    Ref "ticket_SLA_ibfk_3":"slas"."id" < "ticket_SLA"."sla_id" [update: cascade, delete: cascade]
    
    Ref "ticket_user_actions_recipient_foreign_idx":"users"."id" < "ticket_user_actions"."recipient"
    
    Ref "tickets_approval_group_id_foreign_idx":"groups"."id" < "tickets"."approval_group_id"
    
    Ref "tickets_shared_group_id_foreign_idx":"groups"."id" < "tickets"."shared_group_id"
    
    Ref: "calendars_special_hours"."calendar_id" < "calendars"."id"
    
    Ref: "calendars_default_hours"."calendar_id" < "calendars"."id"
    
    Ref: "sla_goals"."sla_id" < "slas"."id"
    
    Ref: "users"."user_profile_id" < "users_profiles"."id"
    
    Ref: "tickets"."category_id" < "categories"."id"
    
    Ref: "tickets"."ticket_type_id" < "ticket_types"."id"
    
    Ref: "tickets"."recipient" < "users"."id"
    
    Ref: "tickets"."created_by" < "users"."id"
    
    Ref: "tickets"."assigned_to_user" < "users"."id"
    ```
    

https://dbdiagram.io/d/JAE-apenas-tabela-usadas-no-CSM-6734b830e9daa85aca53ca4f

https://dbdiagram.io/d/JAE-apenas-tabela-usadas-no-CSM-6734b830e9daa85aca53ca4f

# Principais tabelas

A seguir será explicado as principais tabelas para o contexto QSM, se sentiu falta da documentação de alguma tabela, sinta-se convidado a nos enviar uma mensagem para atualizarmos essa documentação.

### users

**Principal função:** Salvar informações básicas dos usuários da plataforma. 

**Usuários são todos os clientes do live-chat e todos os funcionários da JAé que utilizam o fintalk-desk**. Vale ressaltar que um usuário cliente pode ter “n” atendimentos live-chat, logo se existem 50 mil atendimentos live-chat em um mês, não significa que foram criados 50 mil usuários com perfil cliente, pois um único cliente pode ter entrado em contato com o time de atendimento mais de uma vez naquele mês.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/936bb225-edfa-4441-bd67-470432e95953/image.png)

- Principais colunas:
    1. **user**: informação usada para fazer login na plataforma.
    2. **name, email, telefone**: colunas auto explicativas.
    3. **cpf**: dê preferência a salvar apenas números.
    4. **formResponseId**: indica a resposta do formulário presente no perfil do usuário.
    
    As demais colunas são autoexplicativas.
    

> Observação: Evite realizar mudanças diretamente nessas tabelas, pois muitas das informações dos usuários ficam em cache. Exemplo: caso mude o perfil desse usuário, é possível que suas novas permissões atreladas a esse perfil não sejam atualizadas.
A maneira mais fácil de atualizar a cache de um usuário, se você realmente precisar, é ir na tela `/userprofiles` e adicionar uma permissão para o perfil na qual seu usuário faz parte
> 

### users_profiles

**Principal função:** salvar informações dos perfis de usuários.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/479e0a5d-26a2-43d2-b6a3-34bb9bc544ef/image.png)

- Principais colunas:
    
     1. **slug**: nome identificador do perfil.
    
    1. **form_id:** aponta para um formulário adicional presente em cada usuário. Essa coluna permite salvarmos informações adicionais para cada usuário baseado em seu perfil de usuário. Exemplo: Todos os clientes deverão ter a opção de salvar número do cartão.
    2. **config_login_method_id:** nosso sistema permite escolher tipos de login conforme o perfil que exercem na organização. Exemplo: todos os atendentes irão acessar a plataforma com cpf e senha, enquanto que os clientes acessarão com a conta da Google.

### groups

**Principal função:** Contém todos os grupos que existem na plataforma. 

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/2fac9567-1fa4-49a0-a8f8-1ce98807cfa6/image.png)

- Principais colunas:
    1. **type:** é uma chave estrangeira para `groups_types`. Dentro da tabela groups_types temos a possibilidade de configurar formulários específicos.
    2. **form_response_id:** indica a resposta do formulário presente em group_types. Exempo: no QSM cada fila é um grupo do tipo `fila`. Nesse tipo temos a presença de um formulário contendo a pergunta calendarId. Dessa forma, conseguimos especificar um calendário específico para cada fila.

> Para indicar quais usuários estão em quais grupos, consulte a tabela `users_groups`
> 

<aside>
⚠️

Ao adicionar um novo usuário para um grupo, é necessário atualizar o perfil do usuário adicionando qualquer permissão. Isso fará atualização na cache dos usuários do perfil modificado.

</aside>

### permissions

**Principal função:** contém todas as permissões utilizadas na plataforma.

Sempre que queremos proteger uma determinada funcionalidade, utilizamos o conceito de permissões. Por exemplo: apenas usuários com a permissão `ticket.list` é capaz de listar qualquer ticket.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/f225fa94-6dd1-4858-b2a1-022d037f709c/image.png)

- Principais colunas:
    1. name: indica o nome da permissão. Use no formato similar das colunas slug.

### databases

**Principal função: permitir salvar informações customizadas em nosso sistema**

Database é um dos núcleos da nossa plataforma low-code, permitindo que organizações crie databases específicos para suas necessidades. 

Todos os databases não materializados possuem 2 visualizações do MySQL e começam com o prefixo `tables_data_`. Uma dessas visualizações terminam com o sufixo `no_types` e serve apenas para uso interno. Sempre de preferência as views que terminam sem esse sufixo.

- Principais colunas:
    
    Não possuem principais colunas, pois cada database terá colunas específicas para atender um determinado problema, porém podemos ressaltar colunas comuns a todos os databases: `created_by, created_at, updated_at, deleted_at, form_id`.
    

Databases materializados deixam de ser views e se tornam tabelas do MySQL com o prefixo `tables_` mais o nome do database.

<aside>
⚠️

CUIDADO: quando um database é materializado, não geramos mais itens na tabela formResponses. O formulário ainda existe, porém os dados são salvos na nova tabela.

</aside>

### tickets

**Principal função:** contém informações de todos os chamados do QSM. ****

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/84bcc97d-d885-432b-97bd-8fba61ba2f32/image.png)

- Principais colunas:
    1. **ticket_type_form_response_id:** id da resposta do formulário do tipo do chamado.
    2. **form_response_id:** id da resposta do formulário da categoria do chamado.
    3. **assigned_to_user:** id o atendente do chamado**.**
    4. assigned_to: id da fila do chamado.
    5. recipient: id do destinatário, em alguns contextos chamados de “cliente” ou “beneficiário”, do chamado.
    6. status: indica o status do chamado.
    7. ticket_type_id: indica o id do tipo do chamado.
    8. category_id: indica o id da categoria do chamado.
    9. description: resumo do chamado, geralmente um texto com menos de 250 caracteres.
    10. body: texto explicativo do chamado, usado para detalhar mais informações à respeito daquele chamado. Aceitando inclusive passar um HTML.
    
    <aside>
    ⚠️
    
    Não usamos mais as colunas e-mails e name presente em tickets. 
    
    </aside>
    

### chat_rooms

**Principal função:** contém todos os atendimentos live chats da plataforma.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/03af74db-2a38-4377-9015-b054e1282736/image.png)

Não temos salvos em nosso banco relacional as mensagens dos usuários, caso deseje consultar essas informações é necessário solicitar acesso ao nosso outro banco.

- Principais colunas
    
    id: é o mesmo id usado em nosso outro banco.
    
    **webhook_url**: é o webhook que iremos acionar qnd uma nova mensagem for enviada/recebida.
    
    **json_data**: coluna json para salvar informações gerais sobre a conversa. Exemplo: salvar o id do cliente presente em outro sistema.
    
    **ticket_id**: é a FK da tabela de tickets. Todos nossos atendimentos válidos possuem essa FK diferente de vazio.
    
    **firs_reply_at**, **last_reply_at** e **last_reply_author** são colunas importantes e autoexplicativas.
    

> Observação: colunas que não usamos no QSM. requester_id, empty, product_id, members_count, icon, type, chat_status_id, name, assigned_to.
> 

## Informações adicionais do atendimento

Campos no formulário presente no tipo de chamado “Chats” são utilizados nas imagens abaixo:

![image.png](attachment:23a4da4a-0611-4b76-acaa-e46d05219692:image.png)

![image.png](attachment:00ded07a-646d-4bcc-a2b5-9a39ce0d1596:image.png)

Esses dados estão salvos na tabela `tables_ticket_type_table_100272` . A justificativa desse nome pode ser entendida na sessão [TicketTypeAdditionalInfos e categoryAdditionalInfos](https://www.notion.so/Data-mapping-CSM-JAE-13d609ba3e9580acad43e2b0982fcf30?pvs=21)

![image.png](attachment:071b7bc8-340d-493c-9804-6e9a21c64d3c:image.png)

O gerenciamento dessa tabela pode ser realizada em https://quoti.cloud/jae/databases

- Principais colunas
    
    **n1**: Coluna n1
    
    **teste_tabulacao**: Coluna n2. Está com esse nome, pois a própria organização criou dessa forma. Não ajustamos ainda, pois é necessário verificar possíveis fluxos onde se utiliza essa coluna para minimizar impactos com a alteração.
    
    **userId_para_chamados_email:** usada apenas no contexto de e-mail. Serve para coletar informações dos usuários que entram em contato por e-mail com o suporte.
    
    **nome_para_chamados_email:** usada apenas no contexto de e-mail. Salva nome do usuário que enviou e-mail para o suporte.
    
    **cpf_para_chamados_email: usada apenas no contexto de e-mail.** Salva CPF do usuário que enviou e-mail para o suporte.
    
- Observações:
    
    Criamos as colunas **userId_para_chamados_email, nome_para_chamados_email e cpf_para_chamados_email nesse mesmo formulário de chat, por dois motivos:**
    
    1. A Alexia nos pediu que todo atendimento live-chat e e-mail tivessem os mesmo campos de tabulação do atendimento live-chat. Logo utilizamos o mesmo formulário.
    2. Eficiência em queries mysql, desa forma conseguimos obter informações de e-mail e atendimento live-char realizando left joint em apenas uma única tabela.

## SLAS

Em atendimentos live-chat, utilizamos 3 SLAs conforme mostrados na imagem abaixo.

![image.png](attachment:c666f3ba-daf3-459c-a87a-68099ca99a7a:image.png)

No banco, essas informações são salvas nas tabelas ticket_slas, ticket_goals, slas, slas_goals e goals. O id do SLA para atendimentos é 1. Portanto, caso queira ver, no banco, detalhes de um SLA de ticketId 407438, faça:

```sql
SELECT x.* FROM org_slug.ticket_slas x 
WHERE ticket_id = 407438
```

![image.png](attachment:82ae7831-6c96-4ebd-a8ac-0997f7abe491:image.png)

E caso queira ver as metas de SLA desse ticket, faça:

```sql
SELECT x.* FROM org_slug.ticket_goals x
WHERE ticket_id = 407438
```

![image.png](attachment:5c31c56d-a2db-47a8-936d-09df3abd15bc:image.png)

### ticket_user_actions

**Principal função:** Contém ****os eventos dos chamados.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/820fc1a3-ff71-4dc1-9107-94e928cf28da/image.png)

- Principais colunas
    
    **details:** armazena o texto de uma mensagem. Também aceita string no formato HTML.
    
    **data**: uma coluna json para armazenar qualquer informação considerada útil para aquela ticketUserAction. Por padrão, costumamos salvar a alteração do ticket antes da mudança, além de registar o tipo e a categoria do chamado.
    
    ```json
    {
      "old": { "assignedToUserName": "Luiz Antonio de Albuquerque Junior" }, // salvamos apenas as propriedade que mudamos
      "ticket": {
        "categoryName": "Produto precisando de revisão",
        "ticketTypeName": "Incidente/Problema"
      }
    }
    
    ```
    
    **is_automatic_message:** indica se a mensagem é automática.
    
    **ticket_id**: indica qual ticket pertence aquela alteração.
    
    **type, description, ticket_type_id, assigned_to_user, status, priority, media_id, category_id, assigned_to, recipient**: são informações que mudaram no ticket. Se o usuário mudou apenas o status para resolvido, então apenas a coluna status estará preenchida, enquanto as demais colunas estarão vazias.
    

### ticket_types e categories

**Principal função:** contém dados do tipo/categoria do chamado.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/dec9f135-c022-4423-95a4-3622f988ae96/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/f0478516-da1a-46b1-8942-83becdb1a92e/image.png)

- Principais colunas
    
    **name**: indica o nome da categoria/tipo.
    
    **type**: é uma coluna slug do name. Serve como identificador memorável do tipo. Em automações de preferência pelo type, pois o name é algo mais provável de mudar no decorrer do projeto.
    
    **icon, color**: são ícones que serão mostrados na mainTab do workspace.
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/517ed6a8-d329-4fbe-a19f-600b97dbe8a9/image.png)
    
    **form_id**: FK para forms. Útil para customizar formulários adicionais para tipo/categoria do chamado.
    
    **default_sla_id**: indica SLA padrão (presente apenas na categoria).
    
    **default_group_id**: indica a fila daquela categoria. (presente apenas na categoria).
    
    **active**: indica se é possível criar chamados para aquela categoria.
    
    **configs**: apresenta customizações para a tela do workspace, agentOverview e qsm-api. Saiba mais em ‣.
    

> Observação: a tabela de tipo e categoria são similares em conceito e em colunas. Em caso de sobreposição de configuração, damos sempre preferência à categoria.
> 

### ticketTypeAdditionalInfos e categoryAdditionalInfos

**Principal função:** Informações adicionais da categoria ou tipo do chamado.

Como já dito anteriormente, toda informação adicional é uma view do MySQL com prefixo `table_data` ou uma tabela materializada com prefixo `tables_`. 

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/103baafe-119c-468c-a6d4-6580243427e3/image.png)

No exemplo acima, a view `tables_data_category_table_100320` pertence à categoria cujo form_Id é `100320`.

Conseguimos identificar essa categoria com a seguinte query:

```jsx
SELECT * from categories c where c.form_id = 100320
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/9ef6e799-8213-408b-b551-786021d535b0/image.png)

- Principais colunas
    
    Como um database é a criação de tabelas baseadas em contextos específicos, as colunas principais variam conforme a necessidade de cada problema.
    

> O mesmo raciocínio se aplicar para qualquer informação adicional, porém as vezes colocamos o id do item em vez do form_id.
> 

### calendars

**Principal função: Contém informações sobre horário útil e feriados**

A tabela `calendars` contém apenas o nome do calendário.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/c93cf8f1-3326-49c5-bb76-99c40442408b/image.png)

A tabela `calendars_default_hours` possui informações sobre horários úteis.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/eb861afa-fcb4-46de-b0a6-b5c32ecd44cd/image.png)

A tabela `calendars_special_hours` possui informações sobre feriados.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/abb4defb-7287-4683-b7d2-a6a3efda9535/image.png)

> Observação 1: Nunca grave o end sendo menor que o start
Observação 2: Para calendários 24 horas, utilize o formato abaixo
> 
> 
> ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/1848cdf1-a04b-4ba1-8d38-02059f0d56e0/image.png)
> 

### slas e goals

**Principal função:** Contém informações sobre SLAs.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/2a6a743e-a23b-4628-9447-549f72f9b2d5/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/145c2b29-3c41-401a-8c63-b2f326124781/c8bfaf06-6fa0-4cf5-980c-f9da1f79f8ea/image.png)

Em nossa estrutura, um SLA pode possuir mais de uma meta. Meta é um objetivo, geralmente medida em tempo, para garantir uma qualidade de serviço.

O cálculo das metas do SLA só faz sentido quando usamos um calendário, geralmente obtemos essa informação através da fila presente na categoria do chamado.

Por padrão, temos 3 metas para os atendimentos live-chats:

1. **chatTimeToResolve**: tempo para resolver aquele atendimento.
2. **chatTimeBetweenResponses**: tempo entre respostas.
3. **chatTimeToFirstReplyAt**: tempo para primeira resposta.

Já para os chamados não atendimentos, costumamos ter apenas um SLA chamado `resolutionTime` para mensurar o tempo total para um chamado ser resolvido.

- Principais colunas da tabela SLAs
    
    **name**: indica o nome do SLA.
    
    **default_group_id**: indica a fila que aquele SLA se aplica, na prática, essa coluna não é usada, pois obtemos a fila pela própria categoria do chamado, mas fica a critério do desenvolvedor utilizar essa coluna em alguma automação customizada. 
    
    A fila é importante para obtermos o calendário que o SLA utilizará.
    
- Principais colunas da tabela goals
    
    **slug**: identificador da meta. Usada em automações do cálculo do SLA
    
    **value**: tempo em minutos
    

> A tabela usada em nosso sistema é slas (no plural) e não sla.
>