## Objetivo
Imagine o seguinte cenário, você está desenvolvendo um sistema de gestão para uma empresa que precisa cadastrar fornecedores através de um Formulário no Quoti.
E para isso você recebe os seguintes requisitos:

- [x] Esses fornecedores podem ser pessoas físicas (identificadas por CPF) ou jurídicas (identificadas por CNPJ), e por questões de experiência de usuário, apenas o campo de preenchimento de CPF ou CNPJ deve ser exibido baseado na escolha do usuário por pessoa física ou jurídica, nunca os dois ao mesmo tempo.
- [x] Após o preenchimento do campo de CNPJ existe uma particularidade, deve ser feito uma busca pelos dados da empresa em uma base externa (por exemplo, na Receita Federal).
- [x] Enquanto essa busca está em andamento, um diálogo com uma mensagem de carregamento é exibido para o fornecedor.
- [x] Quando os dados da empresa são retornados os campos referentes à empresa (como Nome da empresa, Nome Fantasia, e Endereço) são exibidos automaticamente como campos readonly abaixo do campo CNPJ.

<div class="annotate">
O objetivo dessa seção é explicar o que são os Quoti Formflows e como utilizar essa ferramenta, replicando cenários exatamente como o <b>descrito acima</b>(1), e outros ainda mais desafiadores.
</div>

1.   🚀 Caso já deseje ver como implementar o exemplo acima, siga para a seção [Colocando em prática](practice)

## O que é
Os Quoti Formflows são fluxos de ações para os Formulários criados no Quoti. Que permitem que o usuário configure ações em um determinado formulário após o mesmo ser carregado na tela do usuário que o estiver preenchendo.
!!! info "Casos de uso"
     Exibir/ocultar um campo, desabilitar campos dinamicamente, fazer chamadas HTTP, preencher campo com base no valor de outro, checar permissões de usuário, todos esses são exemplos de casos de uso comuns dos Formflows.

![Tela de formflow](https://storage.googleapis.com/quoti-docs-pictures/forms/formflows/Formflow.png)

Nas seguintes seções vamos discorrer sobre os principais conceitos do Formflows para seu funcionamento. Caso deseje ver como resolver o exemplo acima, siga direto para [Colocando em prática](practice).