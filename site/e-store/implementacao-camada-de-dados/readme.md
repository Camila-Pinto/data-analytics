![Riachuelo](https://www.riachuelo.com.br/static/version1623357894/frontend/Corra/webjump/pt_BR/images/logo.svg)

> Área - Data Analytics<br />
> Documento de Especificação Técnica

<br />

## Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Dimensões Globais](#dimens&#245;es-globais)
- [Desktop - Eventos Gerais](#desktop---eventos-gerais)
- [Desktop - Identificação ](#desktop---identifica%c3%a7%c3%a3o)
- [Desktop - Home](#desktop---home)
- [Desktop - PLP](#desktop---plp)
- [Desktop - Modal Sacola](#desktop---modal-sacola)
- [Desktop - Sacola-QR Code](#desktop---sacola-qr-code)
- [MSite - Login - Esqueci Senha e Cadastro](#msite---login---esqueci-senha-e-cadastro)
- [MSite - Checkout](#msite---checkout)
- [Logoff](#logoff)
- [Enhanced Ecommerce - Desktop-Msite](#enhanced-ecommerce---desktop-msite)

<br />

## Implementação da Camada de dados - Projeto E-Store
Última atualização: 16/09/2021 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [http://e-store.omni.riachuelo.net/](http://e-store.omni.riachuelo.net/).

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-MJZLTJF');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-MJZLTJF" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
  <!-- End Google Tag Manager (noscript) -->
  //...
  </body>
</html>
```

Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/quickstart)


## Observações
> Os valores especificados entre colchetes `[[ ]]` são variáveis dinâmicas e devem ser substituídas por seus respectivos valores.<br />

> Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais. <br />

> Caso o site já possua o Google Analytics instalado, será necessário a remoção do código de **todas as páginas do site**: <br />

```html

<script>
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o), m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)})(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

ga('create', 'UA-XXXXXXXX-X', 'auto');
ga('send', 'pageview');
</script>

```

---

```html

<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-XXXXXXX-X"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){window.dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-XXXXXXX-X');
</script>
```

---

## Overview e Descrições Técnicas

### Camada de dados (DataLayer)

> É um array de objetos javascript utilizado pelo Google Tag Manager para receber em seus atributos, dados importantes do site.
Para implementar o dataLayer no site, o desenvolvedor pode utilizar formas diferentes para preencher os dados. Essas formas são dependentes da ação estabelecida na documentação e também do nível da interação.

**Instalação**<br />
Inserir a camada de dados antes do snippet de instalação do Google Tag Manager. Exemplo:

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer = [{
    'atributo1': '[[valor1]]',
    'atributo2': '[[valor2]]'
  }];
</script>
```

OU

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'atributo1': '[[valor1]]',
    'atributo2': '[[valor2]]'
  });
</script>
```

### Atributos HTML (Data Attributes)

> São atributos customizados inseridos nos elementos HTML da página, permitindo a inclusão de dados adicionais.

**Instalação**
1. Elementos: ```<div>Elemento</div>``` <br />
Todos os elementos do html que serão clicados, deverão ser mapeados recebendo os atributos com sua estrutura no item.

```html
<div
  data-gtm-event-category='[[exemplo:valor-categoria]]'
  data-gtm-event-action='[[exemplo:valor-acao]]'
  data-gtm-event-label='[[exemplo:valor-rotulo]]'
 >
  Texto do elemento
</div>
```

#### Importante:
> Também devem ter os data-attributes `data-gtm-event-category`, `data-gtm-event-action` e `data-gtm-event-label`. Preenchidos conforme instruções específicas.

<br />

## Implementação

A documentação foi descrita para algumas áreas especificas do ambiente do site ou aplicativo.


### Especificações Globais:

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores; <br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais; <br />
Caso a informação solicitada não estiver disponivel retornar o valor com tipagem undefined.

---

### Dimensões Globais:

**Dimensões Customizadas para todas as páginas:**<br />
Deve ser disparado um push de dataLayer no momento de carregamento de todas as páginas do site (Considerar também todas as trocas de Path, quando o conteúdo da página é alterado mas a página não recarrega).<br />

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'dimension1': '[[cd1-user-userid]]',
    'dimension4': '[[cd4-hit-loginstatus(cliente)]]',
    'dimension5': '[[cd5-user-cartID]]',
    'dimension6': '[[cd6-hit-loja]]',
    'dimension7': '[[cd7-hit-matricula-funcionario]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[cd1-user-userid]]` |  &#039;01234&#039; | ID único do usuário definido após cadastro (MagentoID) |
| `[[cd4-hit-loginstatus(cliente)]]` |  &#039;Logged in&#039;, &#039;Logged out&#039; | Status do login do usuário (CLIENTE) |
| `[[cd5-user-cartID]]` |  '102030dfasdf' | ID únido do carrinho do usuário |
| `[[cd6-hit-loja]]` |  'abc', 'santos', 'morumbi' e etc | Retorna o nome da Loja |
| `[[cd7-hit-matricula-funcionario]]` | '3129832', '2313214' e etc | Retorna a matrícula do funcionario |


---


### Desktop - Eventos Gerais

**No clique nos links do header**<br />

- **Onde:** Em todas as páginas que estiverem disponíveis
Título: "sacola" (ícone), "Riachuelo" (logo)
    

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:geral',
    'eventAction': 'clique:header',
    'eventLabel': '[[link-clicado]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[link-clicado]]` | &#039;riachuelo&#039;(logo), &#039;login&#039;, &#039;sacola&#039;, &#039;sair&#039;, &#039;inicio&#039; e etc | Deve retornar o nome do link clicado. |

<br />

**No callback, após fazer uma busca**<br />

- **Onde:** Em todas as páginas que estiverem disponíveis
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:geral',
    'eventAction': 'callback:busca',
    'eventLabel': '[[termo-buscado]]:[[erro]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[termo-buscado]]` | &#039;vestidos&#039;, &#039;calca-masculina&#039; e etc | Deve retornar o termo buscado pelo usuário. |
| `[[erro]]` | &#039;nao-encontramos-nenhum-resultado&#039;, &#039;ops-ocorreu-um-erro&#039;, &#039;pagina-nao-encontrada&#039; e etc | Deve retornar o erro apresentado após fazer a busca e não encontrar nenhum resultado. |

<br />


**Ao carregar uma página de não encontrada ou ocorreu um erro por conta do serviço ou servidor.**<br />

- **Onde:** Em todas as páginas que estiverem disponíveis
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:geral',
    'eventAction': 'erro:carregamento',
    'eventLabel': '[[nome-erro]]',
    'noInteraction': '1'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-erro]]` | &#039;ocorreu-erro-serviço,&#039; &#039;404-not-found&#039; | Deve retornar o nome do erro ocorrido na página genérica. |

<br />

### Desktop - Identificação 

**Após interagir com os campos de "Matrícula", "CPF" e "Selecione a Loja" para fazer o login**<br />

- **Onde:** No modal de identificação
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:lightbox-identificacao',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-campo]]` | 'matricula', 'cpf', 'selecione-a-loja' e etc | Deve retornar o nome do campo preenchido pelo usuário.|

<br />

**No clique do botão "Acessar" para validar os campos preenchidos de Matricula, CPF e Loja**<br />

- **Onde:** No modal de identificação
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:lightbox-identificacao',
    'eventAction': 'clique:botao',
    'eventLabel': 'acessar'
  });
</script>
```

<br />

**No callback, após clicar no botão "Acessar" para validar os campos preenchidos CPF e Senha com sucesso**<br />

- **Onde:** No modal de identificação
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:lightbox-identificacao',
    'eventAction': 'callback:login',
    'eventLabel': '[[sucesso ou tipo-erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-campo]]` | 'sucesso', 'erro:matricula-invalida' e etc | Deve retornar o label do botão 'sucesso' ou o status de erro retorno.|

<br />

### Desktop - Home

**No clique dos cards de Departamento**<br />

- **Onde:** Na home do Desktop
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:home',
    'eventAction': 'clique:card-departamento',
    'eventLabel': '[[nome-card]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-card]]` | 'feminino', 'infantil', 'casa-riachuelo' e etc | Deve retornar o nome do card clicado.|

<br />

### Desktop - PLP 

**No clique do filtro de "Escolha a Categoria"**<br />

- **Onde:** Na PLP
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:plp',
    'eventAction': 'clique:filtro-categoria',
    'eventLabel': '[[nome-categoria]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-categoria]]` | 'colecao-feminina', 'lifestyles', 'praia' e etc |Deve retornar o nome da categoria clicada.|

<br />

**No clique do filtro de "Escolha a Subcategoria"**<br />

- **Onde:** Na PLP
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:plp',
    'eventAction': 'clique:filtro-subcategoria',
    'eventLabel': '[[nome-subcategoria]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-subcategoria]]` | 'blusa', 'calca', 'vestido' e etc | Deve retornar o nome da subcategoria clicada.|

<br />

**Na interação com os filtros**<br />

- **Onde:** Na PLP
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:plp',
    'eventAction': 'clique:filtro-produto',
    'eventLabel': '[[nome-filtro]]:[[opcao-escolhida]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-filtro]]` | 'cor', 'departamento', 'modelagem' e etc | Deve retornar o nome do filtro usado pelo usuário.|
| `[[opcao-escolhida]]` | 'vermelha', 'jeans-feminino', 'skinny' e etc | Deve retornar a opção escolhida.|

<br />

**Na ordenação da lista de produtos, "Relevância"**<br />

- **Onde:** Na PLP
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:plp',
    'eventAction': 'clique:ordenacao',
    'eventLabel': '[[nome-ordenacao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-ordenacao]]` |  'preco:menores-primeiro', 'preco:maiores-primeiro' e etc | Deve retornar o nome da ordenação escolhida.|

<br />

**No clique do botão "Carregar mais"**<br />

- **Onde:** Na PLP
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:plp',
    'eventAction': 'clique:botao',
    'eventLabel': 'carregar-mais'
  });
</script>
```

<br />

### Desktop - Modal Sacola 

**Nos cliques dos elementos**<br />

- **Onde:** No modal da sacola
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:modal-sacola',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-botao]]` |  'fechar', 'continuar-comprando' e 'finalizar-compra' | Deve retornar o nome do botão clicado.|

<br />

**No callback da sacola**<br />

- **Onde:** No modal da sacola
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:modal-sacola',
    'eventAction': 'callback:modal-sacola',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[erro]]` | 'a-quantidade-desejada-esta-indisponivel-no-momento' e etc | Deve retornar o erro apresentado na sacola.|

<br />

### Desktop - Sacola-QR Code

**No callback de erro na página**<br />

- **Onde:** Na página da Sacola
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:sacola',
    'eventAction': 'callback:sacola',
    'eventLabel': '[[erro]]',
    'noInteraction': '1'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[erro]]` | 'a-quantidade-desejada-esta-indisponivel-no-momento', 'sacola-vazia' e etc | Deve retornar o erro aprensentado na sacola.|

<br />

**No clique no botões de "Continuar" e "Continuar Comprando"**<br />

- **Onde:** Na página da Sacola
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:sacola',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-botao]]` | 'continuar' e 'continuar-comprando' | Deve retornar o nome do botão clicado.|

<br />

**No clique nos botões de "Remover" ou "Cancelar"**<br />

- **Onde:** No lightbox "Tem certeza que quer remover esse item da sua sacola?"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:sacola',
    'eventAction': 'clique:botao:lightbox:remover-item-sacola',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-botao]]` | 'remover', 'cancelar' ou 'fechar' | Deve retornar o nome do botão clicado.|

<br />

**No clique no botão de "Ir para página inicial"**<br />

- **Onde:** Na página de "Sua sacola está vazia"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:sacola',
    'eventAction': 'clique:botao',
    'eventLabel': 'ir-para-pagina-inicial'
  });
</script>
```

<br />

**No clique dos botões "Continuar Conectado" ou "Finalizar"**<br />

- **Onde:** Na tela de "Para manter a segurança vamos finalizr a sua sacola"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:sacola:modal-seguranca',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-botao]]` |  'continuar-conectado' e 'fechar' | Deve retornar o nome do botão clicado.|

<br />

**No clique nos botões**<br />

- **Onde:** Na tela de instruções do QR Code
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:instrucoes-qr-code',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-botao]]` |  'voltar' e 'continuar' | Deve retornar o nome do botão clicado.|

<br />

**No clique no botão "Fechar"**<br />

- **Onde:** Na tela de acesse o QR Code
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:qr-code',
    'eventAction': 'clique:botao',
    'eventLabel': 'fechar'
  });
</script>
```

<br />

---

### MSite - Login - Esqueci Senha e Cadastro

**Na interação com os campos para fazer login**<br />

- **Onde:** Na página de Login
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:login',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-campo]]` | 'cpf', 'senha' e etc | Deve retornar o nome do campo preenchido.|

<br />

**No clique do botão "Continuar" para validar os campos preenchidos e fazer o login**<br />

- **Onde:** Na página de Login
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:login',
    'eventAction': 'clique:botao',
    'eventLabel': 'continuar',
  });
</script>
```

<br />

**No callback, após clicar no botão "Continuar" para validar os campos preenchidos e fazer o login**<br />

- **Onde:** Na página de Login
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:login',
    'eventAction': 'callback:login',
    'eventLabel': '[[sucesso ou tipo-erro]]',
    'dimension1': '[[cd1-user-userid]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[sucesso ou tipo-erro]]` | 'a-quantidade-desejada-esta-indisponivel-no-momento', 'sacola-vazia' e etc | Deve retornar o erro aprensentado na sacola.|
| `[[cd1-user-userID]]` | '01234312hbjhdsfs32' e etc | CPF racheado do usuário|

<br />

**No clique no botão de "Esqueci minha senha"**<br />

- **Onde:** Na página para Redefinir Senha
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:redefinir-senha',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:cpf',
  });
</script>
```

<br />

**No clique do botão "Redefinir minha senha" para validar os campos preenchidos e redefinir a senha**<br />

- **Onde:** Na página para Redefinir Senha
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:redefinir-senha',
    'eventAction': 'clique:botao',
    'eventLabel': 'redefinir-minha-senha'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[erro]]` | 'a-quantidade-desejada-esta-indisponivel-no-momento', 'sacola-vazia' e etc | Deve retornar o erro aprensentado na sacola.|

<br />

**No callback, após clicar no botão "Redefinir minha senha" para validar os campos preenchidos e redefinir a senha**<br />

- **Onde:** Na página para Redefinir Senha
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:redefinir-senha',
    'eventAction': 'callback:redefinir-senha',
    'eventLabel': '[[sucesso ou tipo-erro]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[sucesso ou tipo-erro]]` | 'sucesso', 'erro:insira-um-cpf-para-prosseguir', 'erro:cpf-invalido' e etc | Deve retornar o callback da aplicação. |

<br />

**Na interação com os campos para criar conta**<br />

- **Onde:** No formulário de cadastro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:criar-conta',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-campo]]` | 'cpf', 'email', 'data-nascimento', telefone', 'senha', 'confirmar-senha', cep' e etc | Deve retornar o nome do campo preenchido.|

<br />

**No clique nos botões de "Continuar", "termos e condições de uso" e "Política de Privacidade"**<br />

- **Onde:** No formulário de cadastro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:criar-conta',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[erro]]` | 'continuar', 'termos-condicoes-uso', 'politica-privacidade' e etc | Deve retornar o nome do botão clicado.|

<br />

**No callback, após clicar no botão "Continuar" para validar os campos preenchidos**<br />

- **Onde:** No formulário de cadastro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:criar-conta',
    'eventAction': 'callback:cadastro',
    'eventLabel': '[[sucesso ou tipo-erro]]',
    'dimension1': '[[cd1-user-userid]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[sucesso ou tipo-erro]]` | 'a-quantidade-desejada-esta-indisponivel-no-momento', 'sacola-vazia' e etc | Deve retornar o erro aprensentado na sacola.|
| `[[cd1-user-userid]]` | '01234312hbjhdsfs32' e etc | CPF racheado do usuário|

<br />

### MSite - Checkout

**No callback de "Ocorreu um problema com a sua sacola"**<br />

- **Onde:** Na página da Sacola do MSite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:sacola',
    'eventAction': 'callback:sacola',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[erro]]` | 'solicite-ao-colaborador-um-novo-link-ou-qr-code' | Deve retornar o tipo de erro apresentado.|

<br />

**No clique dos botões "Remover" ou "Cancelar"**<br />

- **Onde:** No modal "Tem certeza de que quer remover esse item da sua sacola?" Na página da Sacola do MSite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:sacola:modal:remover-esse-item',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-botao]]` | 'editar-endereco-1', 'editar-endereco-2', 'adicionar-novo-endereco' e etc | Deve retornar o nome do botão clicado.|

<br />


**No clique no botão "Continuar"**<br />

- **Onde:** Na página da Sacola do MSite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:sacola',
    'eventAction': 'clique:botao',
    'eventLabel': 'continuar'
  });
</script>
```

<br />

**No clique das opções na página de Compra**<br />

- **Onde:** Na página de Compra do MSite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:compra',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-botao]]` | 'entrega', 'cupom-de-desconto' e 'pagamento' | Deve retornar o nome do botão clicado.|

<br />

**No callback da página de Entrega**<br />

- **Onde:** Na página de Entrega
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:endereco',
    'eventAction': 'callback:endereco',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[erro]]` | 'erro:estes-produtos-nao-podem-ser-entregues-na-regia-selecionada' | Deve retornar o tipo de erro apresentado.|

<br />

**No clique no botão de "Adicionar novo endereço" ou "Editar Endereço", na seção de Endereço**<br />

- **Onde:** Na página de Entrega, na seção de Endereço
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:endereco',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-botao]]` | 'editar-endereco-1', 'editar-endereco-2', 'adicionar-novo-endereco' e etc | Deve retornar o nome do botão clicado.|

<br />

**Na interação com os campos para editar um endereço já cadastrado ou um novo endereço**<br />

- **Onde:** Nas páginas de "Editar endereço" ou "Novo Endereço" do MSite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:[[nome-formulario]]',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-formulario]]` | 'editar-endereco' ou 'novo-endereco' | Deve retornar o nome do formulário que o botão está localizado.|
| `[[nome-campo]]` | 'cep', 'nome', telefone'  e etc | Deve retornar o nome do campo preenchido.|

<br />

**No clique no botão de "Salvar", para "Editar um endereço já cadastrado" ou "Adicionar um novo endereço"**<br />

- **Onde:** Nas páginas de "Editar endereço" ou "Novo Endereço" do MSite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:[[nome-formulario]]',
    'eventAction': 'clique:botao',
    'eventLabel': 'salvar'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-formulario]]` | 'editar-endereco' ou 'novo-endereco' | Deve retornar o nome do formulário que o botão está localizado.|

<br />

**No callback, após clicar no botão "Salvar" para validar os campos preenchidos**<br />

- **Onde:** Nas páginas de "Editar endereço" ou "Novo Endereço"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:[[nome-formulario]]',
    'eventAction': 'callback:endereco',
    'eventLabel': '[[sucesso ou tipo-erro]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-formulario]]` | 'editar-endereco' ou 'novo-endereco' | Deve retornar o nome do formulário que o botão está localizado.|
| `[[sucesso ou tipo-erro]]` | 'sucesso', 'erro:preencha-campo-bairro' e etc | Deve retornar o label do botão 'sucesso' ou o status de erro retorno.|

<br />

**No clique nos botões "Adicionar", "Continuar" e no ìcone para excluir o cupom**<br />

- **Onde:** Na página de Cupom
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:cupom',
    'eventAction': 'clique:botao',
    'eventLabel': '[[cupom]]:[[nome-botao]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[cupom]]` | 'zap10', 'seja-bem-vindo' e etc | Deve retornar o cupom utlizado.|
| `[[nome-botao]]` | 'adicionar', 'continuar', 'excluir-cupom' e etc | Deve retornar o nome do botão clicado.|

<br />

**No callback para inserir o Cupom de desconto**<br />

- **Onde:** Na página de Cupom
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:cupom',
    'eventAction': 'callback:cupom:[[cupom]]',
    'eventLabel': '[[sucesso ou tipo-erro]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[cupom]]` | 'zap10', 'seja-bem-vindo' e etc | Deve retornar o cupom utlizado.|
| `[[sucesso ou tipo-erro]]` | 'sucesso', 'erro:cupom-invalido', 'erro:cupom-vencido', 'erro:informe-um-cupom-valido' e etc | Deve retornar o status sucesso ou de erro retorno.|

<br />

**No clique nas opções "Forma de Pagamento"**<br />

- **Onde:** Na página de "Pagamento"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:pagamento',
    'eventAction': 'clique:opcao',
    'eventLabel': '[[opcao-selecionada]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[opcao-selecionada]]` | 'cartao-riachuelo-ou-cartao-credito', 'boleto' e etc | Deve retornar a opção selecionada.|

<br />

**No callback para validar dados do Cartão.**<br />

- **Onde:** Na página de "Dados do Cartão"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:pagamento',
    'eventAction': 'callback:dados-cartao',
    'eventLabel': '[[sucesso ou tipo-erro]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[sucesso ou tipo-erro]]` | 'sucesso', 'erro:numero-cartao-invalido', 'erro:nome-obrigatorio' e etc | Deve retornar o status sucesso ou de erro retorno.|

<br />

**Nas opções de parcelamento**<br />

- **Onde:** Na página de "Pagamento"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:pagamento',
    'eventAction': 'interacao:parcelamento',
    'eventLabel': '[[opcao-selecionada]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[opcao-selecionada]]` | '1x35,91', '2x50,00' e etc | Deve retornar a opção selecionada para parcelamento.|

<br />

**No clique dos botões "Continuar"**<br />

- **Onde:** Nas páginas de "Pagamento" e "Dados do Cartão"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:pagamento',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-pagina]]:continuar'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-pagina]]` | 'pagamento' ou 'dados-do-cartao' | Deve retornar o nome da página.|

<br />

**No clique do botão "Finalizar"**<br />

- **Onde:** Na página de Compra
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:compra',
    'eventAction': 'clique:botao',
    'eventLabel': 'finalizar'
  });
</script>
```

<br />

**No callback após clicar no botão "Finalizar"**<br />

- **Onde:** Na página de Compra
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:compra',
    'eventAction': 'callback:finalizar-compra',
    'eventLabel': '[[sucesso ou tipo-erro]]'
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[sucesso ou tipo-erro]]` | 'sucesso', 'erro:cupom-invalido', 'erro:pagamento-recusado', 'erro:entrega' e etc | Deve retornar o status sucesso ou de erro retorno.|

<br />

**No clique do botão "Continuar"**<br />

- **Onde:** Na página de Compra, modal "Desculpe, o pagamento do seu pedido foi recusado"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:compra:modal:pagamento-recusado',
    'eventAction': 'clique:botao',
    'eventLabel': 'continuar'
  });
</script>
```

<br />

**No clique do botão "Copiar Código" para copiar o código de barras**<br />

- **Onde:** Na página de Pedido Realizado com Sucesso
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:purchase',
    'eventAction': 'clique:botao',
    'eventLabel': 'copiar-codigo-barras'
  });
</script>
```

<br />

### Logoff

**No clique no botão de "Desconectar Agora"**<br />

- **Onde:** Na caixa de alerta de segurança
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:logoff:alerta-seguranca',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]',
  });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-botao]]` | 'continuar-conectado' e 'desconectar-agora' | Deve retornar o nome do botão clicado.|

<br />

**No carregamento da página de Logoff**<br />

- **Onde:** Na página de "Você se desconectou com sucesso"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'noInteraction': '1',
    'eventCategory': 'estore:logoff',
    'eventAction': 'desconectado:ate-logo',
    'eventLabel': 'deslogado-com-sucesso',
  });
</script>
```
<br />

### Enhanced Ecommerce - Desktop-Msite

**Na visualização de uma vitrine de produtos**<br />

- **Onde:** Exclusivo para o desktop. Em todas as páginas que exibirem uma lista de produtos
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productImpression',
  'eventCategory':'estore:enhanced-ecommerce',
  'eventAction': 'impressions',
  'noInteraction': '1',
    'ecommerce': {
    'impressions': [{
      'dimension16': '[[cd16-product-selo/tag]]',
      'dimension17': '[[cd17-product-exclusivo-ecommerce+marketplace]]',
      'dimension19': '[[cd19-product-gm]]',
      'dimension21': '[[cd21-product-estilo]]',
      'dimension22': '[[cd22-product-genero]]',
      'name': '[[nome-produto]]',
      'id': '[[id-produto]]',
      'list': '[[lista-produto]]',
      'price': '[[preco-produto]]',
      'brand': '[[marca-produto]]',
      'category': '[[categoria-produto]]',
      'position': '[[posicao-produto]]'
    }]
  }
});
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[cd16-product-selo/tag]]` |  'selo:black-friday', 'tag:frete-gratis' e etc | Deve retornar o tipo do selo ou tag e a promoção **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por "" ; "" (Ponto e vírgula) |
| `[[cd17-product-exclusivo-ecommerce+marketplace]]` | 'sim:rchlo', 'nao:rchlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller' | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| `[[cd19-product-gm]]` |  '101508', '310011', '204002', etc | Subcategorias composto por 6 numeros |
| `[[cd21-product-estilo]]` |   'life-style', 'esporte' e etc | Estilo do produto |
| `[[cd22-product-genero]]` |  'feminino', 'unissex', 'infantil' e etc | Genero do produto |
| `[[nome-produto]]` |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| `[[id-produto]]` |  &#039;13239635&#039; | SKU do produto - pai |
| `[[preco-produto]]` |  &#039;139,99&#039; | Preço do produto |
| `[[marca-produto]]` |  'rchlo', 'carters', 'levis' e etc | Marca do produto |
| `[[categoria-produto]]` |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| `[[lista-produto]]` |   'plp:feminino', 'plp:dia-das-maes', 'plp:infantil', 'busca:blusa', 'busca:blusa-florida' e etc | Nome da lista que o produto aparece |
| `[[posicao-produto]]` |  '1', '2', '3' e etc | Posição que o produto aparece em uma lista de produtos |

<br />


**No clique dos produtos da vitrine interagidos na página**<br />

- **Onde:** Exclusivo para o desktop. Em todas as páginas que exibirem uma lista de produtos
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': 'productClick',
    'eventCategory': 'estore:enhanced-ecommerce',
    'eventAction': 'click',
      'ecommerce': {
      'click': {
        'actionField': {'list': '[[lista-produto]]'},
        'products': [{
          'dimension16': '[[cd16-product-selo/tag]]',
          'dimension17': '[[cd17-product-exclusivo-ecommerce+marketplace]]',
          'dimension19': '[[cd19-product-gm]]',
          'dimension21': '[[cd21-product-estilo]]',
          'dimension22': '[[cd22-product-genero]]',
          'name': '[[nome-produto]]',
          'id': '[[id-produto]]',
          'price': '[[preco-produto]]',
          'brand': '[[marca-produto]]',
          'category': '[[categoria-produto]]',
          'position': '[[posicao-produto]]'
        }]
      }
    }
});
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[cd16-product-selo/tag]]` |  'selo:black-friday', 'tag:frete-gratis' e etc | Deve retornar o tipo do selo ou tag e a promoção **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por "" ; "" (Ponto e vírgula) |
| `[[cd17-product-exclusivo-ecommerce+marketplace]]` | 'sim:rchlo', 'nao:rchlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller' | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| `[[cd19-product-gm]]` |  '101508', '310011', '204002', etc | Subcategorias composto por 6 numeros |
| `[[cd21-product-estilo]]` |   'life-style', 'esporte' e etc | Estilo do produto |
| `[[cd22-product-genero]]` |  'feminino', 'unissex', 'infantil' e etc | Genero do produto |
| `[[nome-produto]]` |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| `[[id-produto]]` |  &#039;13239635&#039; | SKU do produto - pai |
| `[[preco-produto]]` |  &#039;139,99&#039; | Preço do produto |
| `[[marca-produto]]` |  'rchlo', 'carters', 'levis' e etc | Marca do produto |
| `[[categoria-produto]]` |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| `[[lista-produto]]` |   'plp:feminino', 'plp:dia-das-maes', 'plp:infantil', 'busca:blusa', 'busca:blusa-florida' e etc | Nome da lista que o produto aparece |
| `[[posicao-produto]]` |  '1', '2', '3' e etc | Posição que o produto aparece em uma lista de produtos |

<br />

**No carregamento da página de detalhe**<br />

- **Onde:** Na página de detalhes de produto
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productDetail',
  'eventCategory': 'estore:enhanced-ecommerce',
  'eventAction': 'detail',
  'noInteraction': '1',
    'ecommerce': {
    'detail': {
      'products': [{
        'dimension16': '[[cd16-product-selo/tag]]',
        'dimension17': '[[cd17-product-exclusivo-ecommerce+marketplace]]',
        'dimension19': '[[cd19-product-gm]]',
        'dimension20': '[[cd20-product-preco-de]]',
        'dimension21': '[[cd21-product-estilo]]',
        'dimension22': '[[cd22-product-genero]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
      }]
    }
  }
 });
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[cd16-product-selo/tag]]` |  'selo:black-friday', 'tag:frete-gratis' e etc | Deve retornar o tipo do selo ou tag e a promoção **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por "" ; "" (Ponto e vírgula) |
| `[[cd17-product-exclusivo-ecommerce+marketplace]]` | 'sim:rchlo', 'nao:rchlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller' | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| `[[cd19-product-gm]]` |  '101508', '310011', '204002', etc | Subcategorias composto por 6 numeros |
| `[[cd20-product-preco-de]]` | '139,99' | Preço de do produto |
| `[[cd21-product-estilo]]` |   'life-style', 'esporte' e etc | Estilo do produto |
| `[[cd22-product-genero]]` |  'feminino', 'unissex', 'infantil' e etc | Genero do produto |
| `[[nome-produto]]` |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| `[[id-produto]]` |  &#039;13239635&#039; | SKU do produto - pai |
| `[[preco-produto]]` |  &#039;139,99&#039; | Preço do produto |
| `[[marca-produto]]` |  'rchlo', 'carters', 'levis' e etc | Marca do produto |
| `[[categoria-produto]]` |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| `[[variacao-produto]]` |  '101, 310, 204, etc' | DCO Categorias composto por 3 números |


<br />


**Em todas as opções para adicionar produto ao carrinho e na opção de acrescentar a quantidade do produto na sacola**<br />

- **Onde:** Na página de detalhes de produto no Desktop, na página da Sacola do MSite, na tela da Sacola do APP E-store e no clique do botão "Adicionar ao Carrinho" no APP E-store, tela de produto
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'addToCart',
  'eventCategory':'estore:enhanced-ecommerce',
  'eventAction': 'add',
    'ecommerce': {
    'add': {
      'products': [{
        'dimension15': '[[cd15-product-cor]]',
        'dimension16': '[[cd16-product-selo/tag]]',
        'dimension17': '[[cd17-product-exclusivo-ecommerce+marketplace]]',
        'dimension18': '[[cd18-product-tamanho-produto]]',
        'dimension19': '[[cd19-product-gm]]',
        'dimension20': '[[cd20-product-preco-de]]',
        'dimension21': '[[cd21-product-estilo]]',
        'dimension22': '[[cd22-product-genero]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[cd15-product-cor]]` | 'branco', 'preto' e etc | Retornar a cor do produto |
| `[[cd16-product-selo/tag]]` |  'selo:black-friday', 'tag:frete-gratis' e etc | Deve retornar o tipo do selo ou tag e a promoção **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por "" ; "" (Ponto e vírgula) |
| `[[cd17-product-exclusivo-ecommerce+marketplace]]` | 'sim:rchlo', 'nao:rchlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller' | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| `[[cd18-product-tamanho-produto]]` | 'p', 'm', '8-12', '42' | Tamanho do produto |
| `[[cd19-product-gm]]` |  '101508', '310011', '204002', etc | Subcategorias composto por 6 numeros |
| `[[cd20-product-preco-de]]` | '139,99' | Preço de do produto |
| `[[cd21-product-estilo]]` |   'life-style', 'esporte' e etc | Estilo do produto |
| `[[cd22-product-genero]]` |  'feminino', 'unissex', 'infantil' e etc | Genero do produto |
| `[[nome-produto]]` |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| `[[id-produto]]` |  &#039;13239635&#039; | SKU do produto - pai |
| `[[preco-produto]]` |  &#039;139,99&#039; | Preço do produto |
| `[[marca-produto]]` |  'rchlo', 'carters', 'levis' e etc | Marca do produto |
| `[[categoria-produto]]` |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| `[[variacao-produto]]` |  '101, 310, 204, etc' | DCO Categorias composto por 3 números |
| `[[quantidade-produto]]` |  '1', '2' e etc | Quantidade do produto|

<br />


**Em todas as opções para remover produtos do carrinho.**<br />

- **Onde:** Na página de detalhes de produto no Desktop, na página da Sacola do MSite, na tela da Sacola do APP E-store e no clique do botão "Adicionar ao Carrinho" no APP E-store, tela de produto
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'removeFromCart',
  'eventCategory': 'estore:enhanced-ecommerce',
  'eventAction': 'remove',
    'ecommerce': {
    'remove': {
      'products': [{
        'dimension15': '[[cd15-product-cor]]',
        'dimension16': '[[cd16-product-selo/tag]]',
        'dimension17': '[[cd17-product-exclusivo-ecommerce+marketplace]]',
        'dimension18': '[[cd18-product-tamanho-produto]]',
        'dimension19': '[[cd19-product-gm]]',
        'dimension20': '[[cd20-product-preco-de]]',
        'dimension21': '[[cd21-product-estilo]]',
        'dimension22': '[[cd22-product-genero]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[cd15-product-cor]]` | 'branco', 'preto' e etc | Retornar a cor do produto |
| `[[cd16-product-selo/tag]]` |  'selo:black-friday', 'tag:frete-gratis' e etc | Deve retornar o tipo do selo ou tag e a promoção **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por "" ; "" (Ponto e vírgula) |
| `[[cd17-product-exclusivo-ecommerce+marketplace]]` | 'sim:rchlo', 'nao:rchlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller' | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| `[[cd18-product-tamanho-produto]]` | 'p', 'm', '8-12', '42' | Tamanho do produto |
| `[[cd19-product-gm]]` |  '101508', '310011', '204002', etc | Subcategorias composto por 6 numeros |
| `[[cd20-product-preco-de]]` | '139,99' | Preço de do produto |
| `[[cd21-product-estilo]]` |   'life-style', 'esporte' e etc | Estilo do produto |
| `[[cd22-product-genero]]` |  'feminino', 'unissex', 'infantil' e etc | Genero do produto |
| `[[nome-produto]]` |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| `[[id-produto]]` |  &#039;13239635&#039; | SKU do produto - pai |
| `[[preco-produto]]` |  &#039;139,99&#039; | Preço do produto |
| `[[marca-produto]]` |  'rchlo', 'carters', 'levis' e etc | Marca do produto |
| `[[categoria-produto]]` |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| `[[variacao-produto]]` |  '101, 310, 204, etc' | DCO Categorias composto por 3 números |
| `[[quantidade-produto]]` |  '1', '2' e etc | Quantidade do produto|

<br />


**No carregamento das páginas do fluxo de compra do  checkout.**<br />

- **Onde:** Nas páginas de Carrinho(sacola), "Endereço", "Opções de Entrega", "Cupons,vales e trocas" e "Pagamento"

**Obs: Vamos considerar o fluxo do MSite para esse fluxo de checkout**

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'checkout',
  'eventCategory': 'estore:enhanced-ecommerce',
  'eventAction': 'checkout',
  'noInteraction': '1',
  'dimension10': '[[cd10-hit-metodo-pagamento]]',
  'dimension11': '[[cd11-hit-frete]]',
  'dimension12': '[[cd12-hit-parcelamento]]',
  'dimension14': '[[cd14-hit-prazo-frete]]',
  'ecommerce': {
    'checkout': {
      'actionField': {'step': '[[passo-checkout]]'},
      'products': [{
        'dimension13': '[[cd13-product-sku-filho]]',
        'dimension15': '[[cd15-product-cor]]',
        'dimension16': '[[cd16-product-selo/tag]]',
        'dimension17': '[[cd17-product-exclusivo-ecommerce+marketplace]]',
        'dimension18': '[[cd18-product-tamanho-produto]]',
        'dimension19': '[[cd19-product-gm]]',
        'dimension20': '[[cd20-product-preco-de]]',
        'dimension21': '[[cd21-product-estilo]]',
        'dimension22': '[[cd22-product-genero]]',
        'dimension24': '[[cd24-product-frete-do-produto]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[cd10-hit-metodo-pagamento]]` | 'cartao', 'boleto' | Nome do tipo do pagamento. |
| `[[cd11-hit-frete]]` |  'normal, expresso, entrega-agendada'  | Nome do tipo de entrega. |
| `[[cd12-hit-parcelamento]]` | '2', '3' e etc | Quantidade de parcelas |
| `[[cd13-product-sku-filho]]` | '2552' | ID filho do produto |
| `[[cd14-hit-prazo-frete]]` |  'dd/mm/yyyy', '3-dias', '2-dias' e etc | Prazo do frete escolhido |
| `[[cd15-product-cor]]` | 'branco', 'preto' e etc | Retornar a cor do produto |
| `[[cd16-product-selo/tag]]` |  'selo:black-friday', 'tag:frete-gratis' e etc | Deve retornar o tipo do selo ou tag e a promoção **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por "" ; "" (Ponto e vírgula) |
| `[[cd17-product-exclusivo-ecommerce+marketplace]]` | 'sim:rchlo', 'nao:rchlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller' | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| `[[cd18-product-tamanho-produto]]` | 'p', 'm', '8-12', '42' | Tamanho do produto |
| `[[cd19-product-gm]]` |  '101508', '310011', '204002', etc | Subcategorias composto por 6 numeros |
| `[[cd20-product-preco-de]]` | '139,99' | Preço de do produto |
| `[[cd21-product-estilo]]` |   'life-style', 'esporte' e etc | Estilo do produto |
| `[[cd22-product-genero]]` |  'feminino', 'unissex', 'infantil' e etc | Genero do produto |
| `[[cd24-product-frete-do-produto]]` |   'tipo-de-frete:prazo:valor-do-frete' | Retorna todas as informações de frete concatenadas do produto |
| `[[nome-produto]]` |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| `[[id-produto]]` |  &#039;13239635&#039; | SKU do produto - pai |
| `[[preco-produto]]` |  &#039;139,99&#039; | Preço do produto |
| `[[marca-produto]]` |  'rchlo', 'carters', 'levis' e etc | Marca do produto |
| `[[categoria-produto]]` |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| `[[variacao-produto]]` |  '101, 310, 204, etc' | DCO Categorias composto por 3 números |
| `[[quantidade-produto]]` |  '1', '2' e etc | Quantidade do produto|

<br />


**No Carregamento da página de compra quando o usuário já selecionou um modo de entrega**<br />

- **Onde:**  No carregamento da página de compra, com a etapa de "Entrega" já concluída.

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'checkoutOption',
  'eventCategory': 'estore:enhanced-ecommerce',
  'eventAction': 'checkoutOption',
  'eventLabel': [{
    't':'[[nome-entrega]]',
    'd':'[[previsao-entrega]]',
    'p':'[[valor]]'
  }],
    'ecommerce': {
    'checkout_option': {
      'actionField': {'step':'[[passo-checkout]]','option':'[[shipping ou payment]]:[[opcao escolhida]]:[[previsao-entrega]]'},
    }
  }
});
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[nome-entrega]]` | 'frete-gratis', 'normal', 'expressa' | Retorna a opção de entrega |
| `[[previsao-entrega]]` |  'dd/mm/yyyy', '3-dias', '2-dias' e etc | Retorna a previsão de entrega |
| `[[valor]]` | '7.50', '0.00' e etc | Retorna o valor do frete |
| `[[passo-checkout]]` | '1', '2', '3', '4' ou '5' | Retorna o step do checkout da página carregada |
| `[[shipping ou payment]]` | 'shipping' ou 'payment' | Deve retornar a opção escolhida pelo usuário |
| `[[opcao escolhida]]` | 'boleto', 'cartao-de-credito', 'frete-gratis', 'expressa' e etc | Deve retornar a opção de frete ou pagamento escolhida pelo usuário |

<br />

**No carregamento da página  de resumo de compra  após confirmação.**<br />

- **Onde:** Na página de confirmação de compra
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'purchase',
  'eventCategory': 'estore:enhanced-ecommerce',
  'eventAction': 'purchase',
  'noInteraction': '1',
  'metric1': '[[cm1-hit-cupom-valor]]',
  'dimension8': '[[cd8-hit-tipo-de-cartao]]',
  'dimension9': '[[cd9-hit-bandeira-cartao]]',
  'dimension10': '[[cd10-hit-metodo-pagamento]]',
  'dimension11': '[[cd11-hit-frete]]',
  'dimension12': '[[cd12-hit-parcelamento]]',
  'dimension14': '[[cd14-hit-prazo-frete]]',
  'dimension23': '[[cd23-hit-origem-de-venda]]',
  'ecommerce': {
    'purchase': {
      'actionField': {
        'id': '[[id-transacao]]',
        'affiliation': '[[afiliacao-transacao]]',
        'revenue': '[[valor-total-transacao]]',
        'shipping': '[[frete-transacao]]'
        'tax': '[[taxa-transacao]]'
        'coupon': '[[coupon-transacao]]'
      },
      'products': [{
        'dimension13': '[[cd13-product-sku-filho]]',
        'dimension15': '[[cd15-product-cor]]',
        'dimension16': '[[cd16-product-selo/tag]]',
        'dimension17': '[[cd17-product-exclusivo-ecommerce+marketplace]]',
        'dimension18': '[[cd18-product-tamanho-produto]]',
        'dimension19': '[[cd19-product-gm]]',
        'dimension20': '[[cd20-product-preco-de]]',
        'dimension21': '[[cd21-product-estilo]]',
        'dimension22': '[[cd22-product-genero]]',
        'dimension24': '[[cd24-product-frete-do-produto]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável    | Exemplo     | Descrição       |
| :---------- | :---------- | :-------------- |
| `[[cm1-hit-cupom-valor]]` | '0', '3,99', '12,90' e etc | Retorna o valor do Cupom |
| `[[cd8-hit-tipo-de-cartao]]` |  'credito', 'riachuelo' | Tipo do cartão utilizado na compra |
| `[[cd9-hit-bandeira-cartao]]` |  'mastercard', 'visa' e etc | Bandeira do cartão utilizada na compra |
| `[[cd10-hit-metodo-pagamento]]` | 'cartao', 'boleto' | Nome do tipo do pagamento. |
| `[[cd11-hit-frete]]` |  'normal, expresso, entrega-agendada'  | Nome do tipo de entrega. |
| `[[cd12-hit-parcelamento]]` | '2', '3' e etc | Quantidade de parcelas |
| `[[cd13-product-sku-filho]]` | '2552' | ID filho do produto |
| `[[cd14-hit-prazo-frete]]` |  'dd/mm/yyyy', '3-dias', '2-dias' e etc | Prazo do frete escolhido |
| `[[cd15-product-cor]]` | 'branco', 'preto' e etc | Retornar a cor do produto |
| `[[cd16-product-selo/tag]]` |  'selo:black-friday', 'tag:frete-gratis' e etc | Deve retornar o tipo do selo ou tag e a promoção **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por "" ; "" (Ponto e vírgula) |
| `[[cd17-product-exclusivo-ecommerce+marketplace]]` | 'sim:rchlo', 'nao:rchlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller' | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| `[[cd18-product-tamanho-produto]]` | 'p', 'm', '8-12', '42' | Tamanho do produto |
| `[[cd19-product-gm]]` |  '101508', '310011', '204002', etc | Subcategorias composto por 6 numeros |
| `[[cd20-product-preco-de]]` | '139,99' | Preço de do produto |
| `[[cd21-product-estilo]]` |   'life-style', 'esporte' e etc | Estilo do produto |
| `[[cd22-product-genero]]` |  'feminino', 'unissex', 'infantil' e etc | Genero do produto |
| `[[cd23-hit-origem-de-venda]]` | 'venda-em-loja', 'venda-pelo-whatsapp' e etc | Retornar a origem da venda |
| `[[cd24-product-frete-do-produto]]` |   'tipo-de-frete:prazo:valor-do-frete' | Retorna todas as informações de frete concatenadas do produto|
| `[[id-transacao]]` |  &#039;000011652&#039; | ID único da transação |
| `[[valor-total-transacao]]` |  &#039;139,99&#039; | Valor total da transação incluindo frete e taxas |
| `[[frete-transacao]]` |  &#039;129,99&#039; | Valor do frete da transação |
| `[[coupon-transacao]]` |  &#039;cupom-2018&#039; | Cupom de desconto utilizado na transação - promoção nivel pedido |
| `[[afiliacao-transacao]]` | 'riachuelo', 'levis', 'rihappy' e etc | Nome fixo da loja - usada para Marketplace |
| `[[taxa-transacao]]` |  &#039;3,99&#039; | Valor de taxas da transação |
| `[[nome-produto]]` |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| `[[id-produto]]` |  &#039;13239635&#039; | SKU do produto - pai |
| `[[preco-produto]]` |  &#039;139,99&#039; | Preço do produto |
| `[[marca-produto]]` |  'rchlo', 'carters', 'levis' e etc | Marca do produto |
| `[[categoria-produto]]` |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| `[[variacao-produto]]` |  '101, 310, 204, etc' | DCO Categorias composto por 3 números |
| `[[quantidade-produto]]` |  '1', '2' e etc | Quantidade do produto|

<br />

---

## Considerações Finais

> Documentações Oficiais do Google: <br />
> [Google Analytics - Avaliação de comércio eletrônico ](https://developers.google.com/analytics/devguides/collection/analyticsjs/ecommerce?hl=pt-br)
> [Google Tag Manager - Guia do desenvolvedor ](https://developers.google.com/tag-manager/enhanced-ecommerce)

<script>
  document.addEventListener('DOMContentLoaded', function(event) {
    document.querySelectorAll('h1 a')[0].style.display = 'none';
  });
</script>
