## Quoti
É o motor que serve como base para todos os nossos produtos, o Quoti possibilita a criação de ambientes white label e oferece agilidade no desenvolvimento de soluções. Graças a módulos prontos que abstraem boa parte do trabalho e custos de desenvolvimento (como autenticação, gerenciamento de usuários, construção de formulários, gerenciamento de banco de dados etc). Com as equipes assim podendo focar na entrega de valor.

## Extensões & Marketplace
Dentro do Quoti, os usuários podem desenvolver Extensões, que são páginas totalmente customizáveis criadas em Vue e integradas ao ambiente do cliente. Além disso, as extensões criadas podem ser disponibilizadas no Marketplace de Extensões, onde outros clientes podem instalá-las e utilizá-las dentro de seus próprios ambientes. Isso amplia as possibilidades de personalização e colaboração dentro do ecossistema do Quoti.

## UI Builder
O UI Builder é uma ferramenta essencial dentro do Quoti. Ele permite a construção de interfaces e fluxos completos através de uma abordagem visual, utilizando toda a flexibilidade do Vue.js, mas com menos necessidade de código. Porém com todo o suporte a interações avançadas, chamadas de API, manipulação dinâmica de dados e muito mais.

![type:video](https://www.youtube.com/embed/dyFLkJLyBA0?si=sjhzFHuyBuJYKXYs)

## Componentes Html 

·       **Html Text Input:** Campo básico para o usuário digitar informações. Pode ser usado em formulários, cadastros, pesquisas e muito mais.
 
·        **Loop:** Permite repetir componentes ou elementos com base em uma lista. Ideal para criar conteúdos dinâmicos.
 
·        **Paragraph:** Exibe um parágrafo de texto simples, ótimo para descrições ou explicações no meio da tela.
 
·        **Text:** Exibe textos rápidos e diretos, sem a estrutura de parágrafo.
 
 
 
 
 
## Vuetify Components
 
 
·       **v-btn:** Um botão que pode ser usado para disparar ações como salvar, enviar ou navegar entre páginas.
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/buttons/)


·        **v-card:** Estrutura visual que organiza conteúdos como textos, imagens e botões de forma elegante.
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/cards/)

·        **v-col:** Responsável por dividir o layout em colunas. Usado para montar grids responsivos.
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/grids/)

·        **v-container:** Um container que centraliza e organiza o conteúdo dentro da página.
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/grids/)
 
·        **v-divider:** Linha fina usada para separar visualmente seções ou blocos de conteúdo.
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/dividers/)

·        **v-icon:** Ícones prontos que ajudam a deixar a interface mais visual e intuitiva.
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/icons/)

·        **v-img:** Componente para exibir imagens, com suporte para placeholders e carregamento otimizado.
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/images/)

·        **v-list:** Listagem de itens que pode ser usada em menus, listas de opções ou navegação.[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/lists/)

·        **v-progress-circular:** Indicador de carregamento em formato de círculo, bom para feedback de processos.
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/progress-circular/)

·        **v-progress-linear:** Barra de progresso linear, usada para indicar carregamentos ou etapas
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/progress-linear/)
 	
·        **v-row w/ 2 v-col:** Linha com duas colunas, perfeita para dividir conteúdos lado a lado.
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/grids/)
 
·        **v-tab:** Representa uma aba de navegação dentro de um conjunto de abas.
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/tabs/)

·        **v-tab-item:** O conteúdo que aparece ao clicar em uma aba (v-tab).
 [(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/tabs/)

·        **v-tabs:** Gerencia um grupo de abas de navegação para organizar melhor o conteúdo.
 [(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/tabs/)

·        **v-tabs-items:** Define e gerencia o conteúdo trocado entre as abas.
 [(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/tabs/)

·        **v-text-field:** Campo de texto estilizado, usado para entradas como nome, email ou senha
[(Clique aqui e Saiba Mais)](https://v2.vuetifyjs.com/en/components/text-fields/)



## Quoti Components

Em geral, todos os componentes listados abaixo fazem parte da biblioteca do Quoti. Eles foram desenvolvidos para facilitar o seu trabalho e acelerar o processo de desenvolvimento, oferecendo soluções prontas para tarefas comuns da interface.

**qt-autocomplete:** Componente de autocompletar que exibe uma lista de sugestões conforme o usuário digita. É altamente personalizável e pode buscar dados tanto de uma base do Quoti quanto de qualquer API externa configurada por você. Ideal para campos de seleção dinâmica, como usuários, produtos ou categorias.


**qt-avatar:** Componente visual utilizado para exibir avatares (fotos ou iniciais de usuários). Simples de configurar, ele é útil para representar perfis de maneira visual em listas, cards ou cabeçalhos.


**qt-database:** Componente que permite exibir dados de qualquer base do Quoti em formato de tabela. Com poucos ajustes, é possível listar, adicionar ou remover colunas, aplicar diferentes estilos de exibição e muito mais.  
Obs: este componente é baseado no qt-table-list, portanto, quase todas as funcionalidades da tabela também estão disponíveis aqui.


**qt-extension:** Componente usado para renderizar extensões da sua organização. Basta informar o path da extensão desejada para que ela seja exibida. Você pode inclusive aninhar extensões, carregando múltiplas páginas dentro de uma única tela.


**qt-form-response:** Componente que exibe um formulário para o usuário responder. Para utilizá-lo, basta informar o formId do formulário que deseja renderizar. Ele é altamente configurável, permitindo personalizações como salvar rascunho, modo de exibição, entre outras.


**qt-header:** Componente visual que funciona como o cabeçalho da sua página. Pode incluir título, botões de ação, conforme a necessidade do layout.


**qt-table-list:** Componente que renderiza uma tabela de dados personalizável.  Suporta paginação, filtros e ordenação, sendo indicado para exibir listas como usuários, produtos, registros, etc. Ele é flexível e pode ser integrado facilmente com diferentes fontes de dados.


**qt-tooltip:** Componente que exibe um balão de dica (tooltip) ao passar o mouse sobre um elemento. É utilizado para mostrar informações adicionais sem ocupar espaço na interface.

## Referências

- [Documentação Vue 2](https://v2.vuejs.org/v2/guide/)
- [Documentação Vuetify](https://v2.vuetifyjs.com/en/getting-started/installation/)
- [Quoti StoryBook:](https://beta.docs.quoti.dev)

