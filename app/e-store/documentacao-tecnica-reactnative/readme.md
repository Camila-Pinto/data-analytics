![Riachuelo](https://www.riachuelo.com.br/static/version1623357894/frontend/Corra/webjump/pt_BR/images/logo.svg)

> Área - Data Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação de Tags Firebase - Projeto E-store APP

Última atualização: 21/09/2021 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />


## Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Definições Globais](#defini%c3%a7%c3%b5es-globais)
- [APP E-store - Identificação](#app-e-store---identifica%c3%a7%c3%a3o)
- [APP E-store - Menu](#app-e-store---menu)
- [APP E-store - Home](#app-e-store---home)
- [APP E-store - Produto](#app-e-store---produto)
- [APP E-store - Sacola](#app-e-store---sacola)


## Objetivo

Este documento tem como objetivo instruir a implementação de Tags de Firebase Analytics para o aplicativo de Riachuelo Midway App.


## Implementação

> Os links abaixo descrevem como realizar a implementação do Firebase em seu aplicativo.

[Link - Adicionar o Firebase ao projeto do Android](https://rnfirebase.io/docs/master/installation/android).
[Link - Adicionar o Firebase ao projeto do iOS](https://rnfirebase.io/docs/master/installation/ios).

<br />

## Especificações Globais

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[ ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores;<br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais;<br />
Caso a informação solicitada não estiver disponível retornar tipagem `undefined`.

---

### Definições Globais

**Definições Customizadas para todas as telas:**<br />
Quando ocorrer as trocas de telas o código abaixo deve ser definido para que os hits sejam enviados ao Firebase Analytics.<br />

- **Onde:** No carregamento de todas as telas do aplicativo

```javascript
    Analytics.setUserProperty("dimension1", "[[cd1-user-userID]]"),
    Analytics.setUserProperty("dimension4", "[[cd4-hit-loginstatus(cliente)]]"),
    Analytics.setUserProperty("dimension5", "[[cd5-user-cartID]]"),
    Analytics.setUserProperty("dimension6", "[[cd6-hit-loja]]"),
    Analytics.setUserProperty("dimension7", "[[cd7-hit-matricula-funcionario]]");
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[cd1-user-userid]]` |  &#039;01234&#039; | ID único do usuário definido após cadastro  |
| `[[cd4-hit-loginstatus(cliente)]]` |  &#039;Logged in&#039;, &#039;Logged out&#039; | Status do login do usuário (CLIENTE) |
| `[[cd5-user-cartID]]` |  '102030dfasdf' | ID únido do carrinho do usuário |
| `[[cd6-hit-loja]]` |  'abc', 'santos', 'morumbi' e etc | Retorna o nome da Loja |
| `[[cd7-hit-matricula-funcionario]]` | '3129832', '2313214' e etc | Retorna a matrícula do funcionario |

<br />

### APP E-store - Identificação 

- **Onde:** Na visualização da tela de login

```javascript
    Analytics.logScreenView("/login/");
```

<br />

- **Quando:** Após interagir com os campos de "Matrícula", "CPF" e "Selecione a Loja" para fazer o login
- **Onde:** No modal de identificação

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-estore:lightbox-identificacao",
        	eventAction: "interacao:campo",
        	eventLabel: "preencheu:[[nome-campo]]"
        });
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-campo]]` |'matricula', 'cpf', 'selecione-a-loja' e etc | Deve retornar o nome do campo preenchido pelo usuário.|

<br />

- **Quando:** No clique do botão "Acessar" para validar os campos preenchidos de Matricula, CPF e Loja
- **Onde:** No modal de identificação

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-estore:lightbox-identificacao",
        	eventAction: "clique:botao",
        	eventLabel: "acessar"
        });
```

<br />

- **Quando:** No callback, após clicar no botão "Acessar" para validar os campos preenchidos CPF e Senha
- **Onde:** No modal de identificação

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-estore:lightbox-identificacao",
        	eventAction: "callback:login",
        	eventLabel: "[[sucesso ou tipo-erro]]"
        });
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[sucesso ou tipo-erro]]` | 'sucesso', 'erro:matricula-invalida' e etc | Deve retornar o label do botão 'sucesso' ou o status de erro retorno. |

<br />

### APP E-store - Menu

- **Onde:** Na visualização do modal Menu

```javascript
    Analytics.logScreenView("/modal-menu/");
```

<br />

- **Quando:** No clique das opções
- **Onde:** No modal do Menu

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-estore:modal-menu",
        	eventAction: "clique:opcao",
        	eventLabel: "[[nome-opcao]]"
        });
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-opcao]]` |'fechar:modal-menu', 'exibir-sacola', 'sair', 'sobre' e etc | Deve retornar o nome da opção clicada.|

<br />

### APP E-store - Home

- **Onde:** Na visualização da Home

```javascript
    Analytics.logScreenView("/home/");
```

<br />

- **Quando:** No clique no botão "Pesquisar"
- **Onde:** Na Home do APP

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-estore:home",
        	eventAction: "clique:botao",
        	eventLabel: "pesquisar:[[modo-pesquisa]]"
        });
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[modo-pesquisa]]` | 'bipe-produtos', 'codigo-produto', 'sku-produto', 'descricao-produto' e etc | Deve retornar o modo de pesquisa para pesquisar um produto.|

<br />

- **Quando:** No callback de erro na pesquisa de algum produto
- **Onde:** Na Home do APP

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-estore:home",
        	eventAction: "callback:erro",
        	eventLabel: "[[tipo-erro]]"
        });
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[tipo-erro]]` | 'produto-nao-encontrado', 'sku-invalido', 'produto-sem-estoque' e etc | Deve retornar o tipo de erro ocorrido.|

<br />

### APP E-store - Produto

- **Onde:** Na visualização da tela de Produto

```javascript
    Analytics.logScreenView("/produto/");
```

<br />

- **Quando:** Na seleção das opções "Cor" e "Tamanho"
- **Onde:** Na tela de Produto

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-estore:produto",
        	eventAction: "clique:opcao:[[tipo-opcao]]",
        	eventLabel: "[[opcao-selecionada]]"
        });
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[tipo-opcao]]` |'cor', 'tamanho' e etc| Deve retornar o tipo de opção que o usuário está interagindo.|
| `[[opcao-selecionada]]` |'cor:preta', 'cor:branca', 'tamanho:pp', 'tamanho:p' e etc | Deve retornar a opção selecionada.|

<br />

### APP E-store - Sacola

- **Onde:** Na visualização da tela de Sacola

```javascript
    Analytics.logScreenView("/sacola/");
```

<br />

- **Quando:** No clique dos botões "Adicionar mais Produtos" e "Finalizar Compra"
- **Onde:** Na tela da sacola do APP

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-estore:sacola",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-botao]]` |'adicionar-mais-produtos', 'finalizar-compra' e etc | Deve retornar o nome do botão clicado.|

<br />

- **Onde:** Na visualização do modal "Selecione o tipo de venda para finalizar"

```javascript
    Analytics.logScreenView("/sacola/modal-tipo-de-venda-para-finalizar/");
```

<br />

- **Quando:** No clique dos botões "Venda em Loja" ou "Venda pelo Whatsapp"
- **Onde:** No modal "Selecione o tipo de venda para finalizar"

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-estore:modal-tipo-de-venda",
        	eventAction: "clique:botao",
        	eventLabel: "finalizar",
		dimension23: "[[cd23-hit-origem-de-venda]]"
        });
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[cd23-hit-origem-de-venda]]` | 'venda-em-loja', 'venda-pelo-whatsapp' e etc | Retornar a origem da venda|

<br />

- **Onde:** Na visualização das telas de "Sacola criada com Sucesso"

```javascript
    Analytics.logScreenView("/sacola/[[origem-venda]]-sacola-criada-com-sucesso/");
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[origem-venda]]` | 'venda-em-loja', 'venda-pelo-whatsapp' e etc | Retornar a origem da venda|

<br />

- **Quando:** No clique das opções da tela
- **Onde:** Na tela de "Sacola criada com Sucesso"

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-estore:sacola",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]",
		dimension23: "[[cd23-hit-origem-de-venda]]"
        });
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-botao]]` | 'copiar-link', 'continuar-nesta-tela', 'finalizar' e etc | Deve retornar o nome do botião clicado.|
| `[[cd23-hit-origem-de-venda]]` | 'venda-em-loja', 'venda-pelo-whatsapp' e etc | Retornar a origem da venda|

<br />

---

<script>
  document.addEventListener('DOMContentLoaded', function(event) {
    document.querySelectorAll('h1 a')[0].style.display = 'none';
  });
</script>

