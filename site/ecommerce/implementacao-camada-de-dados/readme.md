![Riachuelo](https://www.riachuelo.com.br/static/version1623357894/frontend/Corra/webjump/pt_BR/images/logo.svg)

> Área - Data Analytics<br />
> Documento de Especificação Técnica


## Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Dimensões Globais](#dimens&#245;es-globais)
- [General](#general)
- [Lightbox de Abandono Checkout](#lightbox-de-abandono-checkout)
- [Login e Cadastro](#login-e-cadastro)
- [Novo Fluxo Login](#novo-fluxo-login)
- [Novo Fluxo - Esqueceu a Senha](#novo-fluxo-esqueceu-a-senha)
- [Novo Fluxo - Cadastro](#novo-fluxo-cadastro)
- [PLP e PDP](#plp-e-pdp)
- [Filtro PLP](#filtro-plp)
- [Minha Conta - Informações da Conta](#minha-conta---informa&#231;&#245;es-da-conta)
- [Minha Conta - Catálogo de Endereços](#minha-conta---cat&#225;logo-de-endere&#231;os)
- [Minha Conta - Devolução Online e Retirar em Loja](#minha-conta---devolu%c3%a7%c3%a3o-online-e-retirar-em-loja)
- [Checkout ](#checkout)
- [Baixe o APP](#baixe-o-app)
- [LP Cadastro Live](#lp-cadastro-live)
- [Novo Enhanced E-commerce](#novo-enhanced-e-commerce)


<br />

## Implementação da Camada de dados - Projeto Ecommerce
Última atualização: 19/11/2021 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de https://www.riachuelo.com.br.

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.


```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-WP5D5HK');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-WP5D5HK" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
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
    'dimension4': '[[cd4-hit-userstatus]]',
    'dimension5': '[[cd5-hit-loginstatus]]',
    'dimension7': '[[cd7-user-usermagentoid]]',
    'dimension17': '[[cd17-hit-pagename]]',
    'dimension61': '[[cd61-user-cartID]]',
  });
</script>
```


| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[cd4-hit-userstatus]]` | &#039;sim&#039;, &#039;nao&#039; | Deve retornar se o usuário já fez alguma compra anteriormente (sim) ou se não é sua primeira compra (nao). |
| `[[cd5-hit-loginstatus]]` | &#039;logged-in&#039;, &#039;logged-out&#039; | Status do login do usuário |
| `[[cd7-user-usermagentoid]]` | '0123456' | ID único do usuário definido após cadastro (MagentoID) |
| `[[cd17-hit-pagename]]` | &#039;home&#039;, &#039;pdp-produto-1&#039; | Nome amigável da página definido |
| `[[cd61-user-cartID]]` |  '04245fsdf4fsdaf32fsd' | Deve retornar o ID do carrinho do usuário |


---


### General

**No clique dos links do Stick Bar (pré header)**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** 'carters', 'jeans', 'nossas-lojas' , 'compre-pelo-whatsapp' e etc
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'clique:header-stick-bar',
    'eventLabel': '[[nome-clicado]]'
  });
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[nome-clicado]]` | 'carters', 'jeans', 'nossas-lojas' , 'compre-pelo-whatsapp' e etc | Deve retornar o nome do link do stick bar clicado. |

<br />

**No clique dos links do header**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;riachuelo&quot;(logo), &quot;login&quot;, &quot;minha-conta&quot; etc

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'clique:header',
    'eventLabel': '[[nome-clicado]]'
  });
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[nome-clicado]]` |  'riachuelo' (logo), 'login', 'minha-conta', 'lista-de-desejos', 'sacola' e etc | Deve retornar o nome do link do header e no logo clicado. |

<br />


**No clique dos links do menu superior (Obs: ignorar a categoria e subcategoria quando o clique for diretamente nos links principais do menu superior. Ignorar subcategoria quando o clique for apenas no link de categoria.)**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Novidades&quot;, &quot;Masculino&quot;, &quot;Feminino&quot;, &quot;Calça&quot; etc
    

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'clique:menu-superior',
    'eventLabel': '[[nome-clicado]]'
  });
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[nome-clicacdo]]` | 'novidades', 'masculino', 'feminino', 'alfaiataria', 'outlet', 'camisetas, 'feminino:colecao-feminina:vestido', 'masculino:plus-size:camisa-e-polo', 'masculino:esporte-e-fitness:ver-todos' e etc | Deve retornar o nome do link principal do menu. |

<br />

**No clique dos links interno do footer**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'clique:footer:[[link-ou-icone]]',
    'eventLabel': '[[nome-categoria]]:[[nome-link]]'
  });
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[link-ou-icone]]` | 'link' ou 'icone' |Retorna o tipo de icone que o usuario clicou.  |
| `[[nome-categoria]]` | 'cartao-riachuelo', 'sobre-a-riachuelo', 'sustentabilidade', 'baixe-nosso-app' e etc | Deve retornar o nome da categoria do footer. |
| `[[nome-link]]` | 'saiba-como-adquirir', 'a-empresa', 'na-pratica', 'google-play', 'app-store' e etc | Deve retornar o nome do link do footer clicado. |

<br />

**No clique do botão "Cadastrar" (OBS: Caso seja selecionado mais de uma opção, deve retornar todas as opções concatenadas, separando por ponto e vírgula ( ; ). Ex: "novidades;feminino;" etc)**<br />

- **Onde:** No cadastro de newsletter, localizado no footer em todas as páginas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'cadastrar:newsletter-footer',
    'eventLabel': 'selecionou-interesse:[[interesse-selecionado]]'
  });
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[interesse-selecionado]]` | 'novidades', 'selecionar-todos', 'infantil', 'novidades-feminino' e etc. | Deve retornar o interesse selecionado no checkbox. |

<br />

**No clique nos links de redes sociais/acessibilidade**<br />

- **Onde:** Localizado no footer em todas as páginas
    - **Titulo ou nome do botão/link:** &quot;Riachuelo-facebook&quot;, &quot;Riachuelo-Youtube&quot; etc
    

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'clique:link:rede-social:footer',
    'eventLabel': '[[nome-link]]'
  });
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[nome-link]]` |  'riachuelo-facebook',  'riachuelo-youtube', 'riachuelo-linkedin', 'riachuelo-casa', 'acessibilidade' e etc | Deve retornar o nome do link da rede social clicada. |

<br />

**No clique dos botões ou links no modal "Sacola"**<br />

- **Onde:** Na página de detalhe de produto

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'clique:[[botao ou link]]:modal-sacola',
    'eventLabel': '[[nome-item]]'
  });
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[botao ou link]]` | 'botao' ou 'link'. | Deve retornar o tipo de elemento clicado. |
| `[[nome-item]]` | 'ver-todos-os-itens', 'ir-para-sacola'. | Deve retornar o nome do item clicado |

<br />

**No clique nas opções de "Termos mais buscados" recomendados**<br />

- **Onde:** No campo de busca, em todas as páginas que estiver disponível

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'clique:termos-mais-buscados',
    'eventLabel': '[[item-clicado]]'
  });
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[item-clicado]]` | 'vestido-feminino', 'cropped', 'biquini' e etc | Retorna o item clicado pelo usuário. |

<br />

### Lightbox de Abandono Checkout

**Exibição do lightbox de frete grátis.**<br />

- **Onde:** Todas as páginas do checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'noInteraction': '1',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'exibiu:lightbox',
    'eventLabel': '[[id-lightbox]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[id-lightbox]]` | &#039;opc-boleto-10&#039;, &#039;opc-boleto-14&#039;, &#039;opc-boleto-16&#039;, etc. | Disparar o ID do lightbox. |

<br />


**Clique em &#039;ir as compras&#039;, &#039;continuar&#039;, &#039;não, obrigado&#039;, &#039;X&#039; para fechar.**<br />

- **Onde:** Lightbox de frete grátis nas páginas do checkout.
    
```html
<div
   data-gtm-event-category='riachuelo:geral'
   data-gtm-event-action='clique:lightbox'
   data-gtm-event-label='[[id lightbox]]:[[botao-clicado]]'
>Botão</div>
```


| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[id-lightbox]]`| &#039;opc-boleto-10&#039;, &#039;opc-boleto-14&#039;, &#039;opc-boleto-16&#039;, etc. | Disparar o ID do lightbox. |
| `[[botao-clicado]]` | &#039;ir-as-compras&#039; ou &#039;continuar&#039;, nao-obrigado | Retornar o texto do elemento que foi clicado. |

<br />


### Login e Cadastro

**Ao clicar no botão de “Cadastre-se usando suas Redes Sociais”**<br />

- **Onde:** Modal de Cadastro/Login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:login',
    'eventAction': 'clique:cadastre-com-redes-sociais',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-botao]]` | &#039;facebook, google&#039;. | Deve retornar o botão clicado. |

<br />

**No preenchimento do campo &quot;Enter Cpf&quot; e &quot;Senha&quot;**<br />

- **Onde:** No modal Entrar/Criar Conta
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:login',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[nome-campo]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-campo]]` | &#039;cpf&#039; , &#039;senha&#039; | Deve retornar o nome do campo preenchido. |

<br />

**No clique dos links ou botões "Esqueceu a sua senha? Clique aqui", "Continuar Cpf", "Continuar Senha"**<br />

- **Onde:** No modal Entrar/Criar Conta
    
```html
<div
   data-gtm-event-category='riachuelo:login'
   data-gtm-event-action='clique:[[link-botao]]'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[link-botao]]` | &#039;link' ou 'botao&#039; e etc | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;esqueceu-senha&#039;, &#039;continuar-cpf&#039;, &#039;continuar-senha&#039; e etc | Deve retornar o nome do item clicado. |

<br />

**No callback da tentativa de login**<br />

- **Onde:** No modal Entrar/Criar Conta
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'login',
    'eventCategory': 'riachuelo:login',
    'eventAction': 'tentativa:callback',
    'eventLabel': '[[sucesso ou tipo-erro]]',
    'dimension5': '[[cd5-hit-loginstatus]]',
    'dimension7': '[[cd7-user-usermagentoid]]',
    'dimension59': '[[cd59-hit-tipologin]]',
  });
</script>
```



| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[sucesso ou tipo-erro]]` | &#039;sucesso&#039;, &#039;voce-nao-digitou-os-dados-corretamente&#039;, etc | Deve retornar o sucesso ou descrição do erro da tentativa de login |
| `[[cd5-hit-loginstatus]]` | &#039;logged-in&#039;, &#039;logged-out&#039; | Status do login do usuário |
| `[[cd7-user-usermagentoid]]` | &quot;0123456&quot; | ID único do usuário definido após cadastro (MagentoID) |
| `[[cd59-hit-tipologin]]` | &quot;login-cpf-senha&quot;, &quot;facebook&quot;, &quot;google&quot; e etc | Retornar o tipo de login realizado pelo usuário. |

<br />

**No preenchimento dos campos do formulário de cadastro.**<br />

- **Onde:** Na página de cadastro.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:cadastro',
    'eventAction': 'preencheu-campo:cadastro',
    'eventLabel': '[[nome-campo]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-campo]]` | &#039;nome-completo, e-mail, data-de-nascimento&#039; e etc. | Deve retornar o nome do campo preenchido. |

<br />

**Ao clicar no botão continuar.**<br />

- **Onde:** Na página de cadastro.
    
```html
<div
   data-gtm-event-category='riachuelo:cadastro'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='continuar'
>Botão</div>
```

<br />

**No sucesso ou erro da tentativa de cadastro.**<br />

- **Onde:** Na página de cadastro.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:cadastro',
    'eventAction': 'enviar:cadastro',
    'eventLabel': '[[sucesso ou tipo-erro]]'
  });
</script>
```



| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[sucesso ou tipo-erro]]` | &quot;sucesso&quot; ou &quot;dados-invalidos&quot; | Deve retornar o status do envio. |

<br />

**No clique dos links da página de formulário**<br />

- **Onde:** Na página de cadastro.
    
```html
<div
   data-gtm-event-category='riachuelo:cadastro'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='[[nome-link]]'
>Botão</div>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-link]]` | &#039;politica-de-privacidade, nao-sei-meu-cep, termos-e-condicoes-de-uso&#039;. | Deve retornar o link clicado. |

<br />

**Ao efetivar as alterações cadastrais em &quot;Informações da conta&quot;**<br />

- **Onde:** Na página de &quot;Minha conta&quot; (painel do cliente)
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'r_alteracao_dados_cadastrais',
    'eventCategory': 'riachuelo:minha-conta',
    'eventAction': 'clique:editar:informacoes-da-conta',
    'eventLabel': 'sucesso'
  });
</script>
```

<br />

### Novo Fluxo Login

**No clique dos botões**<br />

- **Onde:** No modal "Que bom ver você por aqui"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:modal:login',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-botao]]` | 'entrar' ou 'quero-criar-a-minha-conta' | Deve retornar o nome do botão clicado. |

<br />

**Na interação com os campos "CPF" e "Senha"**<br />

- **Onde:** Na página de "Login"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:login',
    'eventAction': 'preencheu-campo',
    'eventLabel': '[[nome-campo]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-campo]]` | 'cpf-ou-email' ou 'senha' |Deve retornar o nome do campo preenchido.  |

<br />

**No clique em todos os elementos da página**<br />

- **Onde:** Na página de "Login"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:login',
    'eventAction': 'clique:[[elemento]]',
    'eventLabel': '[[nome-elemento]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[elemento]]` | 'botao', 'link', 'checkbox' ou 'password-visibility' |Deve retornar o nome do elemento.|
| `[[nome-elemento]]` | 'entrar', 'quero-criar-minha-conta', 'esqueceu-a-senha', 'mostrar-senha', 'esconder-senha', 'privacidade-condicoes' e etc. |Deve retornar o nome do elemento clicado. |

<br />

**No clique do botões para fazer Login com alguma Rede Social**<br />

- **Onde:** Na página de "Login"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:social-login',
    'eventAction': 'clique:botao',
    'eventLabel': '[[rede-social]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[rede-social]]` | 'facebook', 'google' e etc |Deve retornar o nome da Rede Social escolhida para efetuar o Login.|

<br />

**No callback dos campos "CPF" e "Senha"**<br />

- **Onde:** Na página de "Login"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:login',
    'eventAction': 'callback:[[nome-campo]]',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-campo]]` | 'cpf' ou 'senha' |Deve retornar o nome do campo que iremos receber o callback.|
| `[[erro]]` | 'insira-sua-senha-para-acessar', 'senha-invalida' e etc. |Deve retornar o callback de erro dos campos CPF e Senha|

<br />

### Novo Fluxo Esqueceu a Senha

**No callback dos campos "CPF" e "Senha"**<br />

- **Onde:** Na página de "Esqueceu a senha?"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:login:esqueceu-senha',
    'eventAction': 'preencheu-campo',
    'eventLabel': 'seu-email-ou-cpf'
  });
</script>
```

<br />

**No clique nos botões ou links**<br />

- **Onde:** Em todo o fluxo "Esqueceu a senha?"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:login:esqueceu-senha',
    'eventAction': 'clique:[[botao-link]]',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[botao-link]]` | 'botao' ou 'link' |Deve retornar o elemento clicado.|
| `[[nome-botao]]` | 'continuar', 'lembrei-minha-senha', 'voltar-para-a-home', 'atendimento-ao-cliente' e etc |Deve retornar o nome do botão clicado.|

<br />

**No callback do campo "Seu e-mail ou CPF"**<br />

- **Onde:** Na página de "Esqueceu a senha?"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:login:esqueceu-senha',
    'eventAction': 'callback:campo:seu-email-ou-cpf',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[erro]]` | 'insira-sua-senha-para-acessar', 'senha-invalida' e etc. | Deve retornar o callback de erro dos campos CPF e Senha.|

<br />

### Novo Fluxo Cadastro

**No preenchimento dos campos**<br />

- **Onde:** Na página de Cadastro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:cadastro',
    'eventAction': 'preencheu-campo',
    'eventLabel': '[[nome-campo]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-campo]]` | 'nome-completo', 'email', 'data-de-nascimento', 'endereco', 'bairro' e etc | Deve retornar o nome do campo preenchido. |

<br />

**No clique no botões, links e elementos da página**<br />

- **Onde:** Na página de Cadastro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:cadastro',
    'eventAction': 'clique:[[elemento]]',
    'eventLabel': '[[nome-item-clicado]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :------------------- |
| `[[elemento]]` | 'botao', 'link', 'checkbox' e etc. | Deve retornar o elemento clicado.|
| `[[nome-item-clicado]]`| 'nao-sei-meu-cep', 'termos-e-condicoes', 'declaracao-de-privacidade', 'finalizar-meu-cadastro' e etc | Deve retornar o nome do elemento clicado. |

<br />

**No callback dos campos obrigatórios**<br />

- **Onde:** Na página de Cadastro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:cadastro',
    'eventAction': 'callback:[[nome-campo]]',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :------------------- |
| `[[nome-campo]]` | 'nome-completo', 'email', 'cpf', 'endereco', 'bairro' e etc. | Deve retornar o nome do campo que está retornando o callback.|
| `[[erro]]` | 'dado-incorreto', 'cpf-invalido', 'bairro-nao-existente' e etc. | Deve retornar o callback de erro do campo.|


<br />

**No clique dos botões**<br />

- **Onde:** Nos modais "Já existe uma conta com CPF ou e-mail"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:modal:conta-existente:[[cpf-ou-email]]',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :------------------- |
| `[[cpf-ou-email]]` | 'cpf' ou 'email' | Deve retornar a informação informada no modal.|
| `[[nome-botao]]` | 'entrar-na-minha-conta', 'usar-outro-email' ou 'usar-outro-cpf'. | Deve retornar o nome do botão clicado.|

<br />

### PLP e PDP

**No clique para adicionar um produto à lista de desejos**<br />

- **Onde:** Na página de detalhes de produto e em todas as páginas que exibirem uma lista de produtos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'r_like_products',
    'eventCategory': 'riachuelo:[[plp ou pdp]]',
    'eventAction': 'clique:adicionar-a-lista-de-desejos',
    'eventLabel': '[[id-produto]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[plp ou pdp]]` | &#039;plp&#039;, &#039;pdp&#039; e etc | Deve retornar o nome da página. |
| `[[id-produto]]` | &#039;0655&#039;, &#039;8566&#039; etc | Deve retornar o id do produto adicionado a lista de desejos. |

<br />


**Na interação com o modal de tabela de medidas**<br />

- **Onde:** Na página de detalhe de produto
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:pdp',
    'eventAction': 'interacao:tabela-de-medidas',
    'eventLabel': '[[abriu ou fechou]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[abriu ou fechou]]` | &#039;abriu&#039;, &#039;fechou&#039; | Deve retornar a ação do usuário. |

<br />

**Na interação com o modal de provador virtual**<br />

- **Onde:** Na página de detalhe de produto
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:pdp',
    'eventAction': 'interacao:provador-virtual',
    'eventLabel': '[[abriu ou fechou]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[abriu ou fechou]]` | &#039;abriu&#039;, &#039;fechou&#039; | Deve retornar a ação do usuário. |

<br />

**No sucesso de recomendação de tamanho**<br />

- **Onde:** No modal de provador virtual
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:pdp',
    'eventAction': 'sugestao:provador-virtual',
    'eventLabel': '[[tamanho]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[tamanho]]` | &#039;p&#039;, &#039;m&#039; &#039;g&#039; etc | Deve retornar o tamanho sugerido. |

<br />

**No carregamento dos tipos de entrega disponíveis, apos consultar frete**<br />

- **Onde:** Na página de detalhes do produto
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:pdp',
    'eventAction': 'tipos-de-entrega',
    'noInteraction': '1',
    'eventLabel': "[{'t':'[[tipo-frete]]','d':'[[previsao]]','p':'[[valor]]'},{'t':'[[tipo-frete]]','d':'[[previsao]]','p':'[[valor]]'}]",
    'dimension40': '[[product-exclusivoecommerce+marketplace]]',
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[tipo-frete]]` | &#039;normal&#039;, &#039;expresso &#039;, &#039;entrega-agendada&#039;,&#039;retirar-em-loja&#039;  | Deve retornar os nomes dos tipos de frete. |
| `[[previsao]]` | &#039;xx-dias&#039;, &#039;2-dias&#039; e etc. | Deve retornar a quantidade de dias para a entrega. |
| `[[valor]]` | &#039;0.00&#039;, &#039;2.99&#039;, &#039;5.00&#039; | Deve retornar os valores dos fretes. |
| `[[product-exclusivoecommerce+marketplace]]` | &#039;sim:richlo&#039;, &#039;nao:richlo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |

<br />


**No clique em compartilhar o produto nas redes sociais**<br />

- **Onde:** Na página de detalhes do produto
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:pdp',
    'eventAction': 'clique:compartilhar',
    'eventLabel': '[[nome-rede-social]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-rede-social]]` | &#039;facebook&#039;, &#039;twitter&#039;, &#039;pinterest&#039;, &#039;whatsapp&#039; e etc | Deve retornar o nome da rede social clicada. |

<br />

**No clique do link &quot;Vendido e entregue por&quot;**<br />

- **Onde:** Na página de detalhes do produto
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:pdp',
    'eventAction': 'clique:link',
    'eventLabel': 'vendido-e-entregue-por',
    'dimension40': '[[product-exclusivoecommerce+marketplace]]',
  });
</script>
```



| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[product-exclusivoecommerce+marketplace]]` | &#039;sim:richlo&#039;, &#039;nao:richlo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |

<br />

### Filtro PLP

**No clique nos filtros nas páginas de Busca e de PLPs (OBS: Caso seja selecionado mais de um filtro, retornar de forma concatenada, separando por ponto e vírgula ( ; ). Ex: &#039;plp:tamanho:46;48&#039; )**<br />

- **Onde:** Na página de busca, após buscar algum termo e também nas páginas de Listas de Produtos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:filtro',
    'eventAction': 'clique:filtro',
    'eventLabel': '[[localizacao-filtro]]:[[filtro]]:[[filtro-selecionado]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[localizacao-filtro]]` | &#039;busca&#039;, &#039;plp&#039; | Deve retornar a localização do filtro. |
| `[[filtro]]` | &#039;departamento&#039;, &#039;categorias&#039;, &#039;tamanho&#039;, &#039;ordenar&#039;, &#039;mais-filtros&#039;, &#039;modal-mais-filtros&#039; e etc | Deve retornar o nome do filtro. |
| `[[filtro-selecionado]]` | &#039;menor-preco&#039;, &#039;masculino&#039;, &#039;infanto-juvenil&#039;, &#039;list&#039;, &#039;grid&#039;,  &#039;definicao-valor:0-300&#039; e etc | Deve retornar o label do filtro selecionado. |

<br />

**No clique nos botões "Aplicar" ou "Limpar" para aplicar os filtros selecionados**<br />

- **Onde:** Na página de busca, após buscar algum termo e também nas páginas de Listas de Produtos    

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:filtro',
    'eventAction': 'clique:botao',
    'eventLabel': '[[localizacao-filtro]]:[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[localizacao-filtro]]` | &#039;busca&#039;, &#039;plp&#039; | Deve retornar a localização do filtro. |
| `[[nome-botao]]` | &#039;limpar&#039; ou &#039;aplicar&#039; | Deve retornar o nome do botão. |

<br />

### Minha Conta - Informações da Conta

**Na interação com o checkbox para receber Ofertas e Novidades**<br />

- **Onde:** Na página de "Informações da Conta"  

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:informacoes-da-conta',
    'eventAction': 'interacao:checkbox:ofertas-e-novidades',
    'eventLabel': '[[acao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[acao]]` | 'selecionou' ou 'desselecionou' | Retorna a ação do usuário. |

<br />

**No clique nos botões da página**<br />

- **Onde:** Na página de "Informações da Conta"  

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:informacoes-da-conta',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-botao]]` | 'salvar', 'alterar-senha', 'alterar-email', 'salvar-novo-e-mail' e etc | Retorna o nome do botão clicado. |

<br />

**No callback de erro dos campos da página**<br />

- **Onde:** Na página de "Informações da Conta"

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:informacoes-da-conta',
    'eventAction': 'callback:campo:[[nome-campo]]',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-campo]]` | 'data-ou-nascimento', 'novo-e-mail', 'senha' e etc | Retorna o nome do campo que houve algum callback informativo ou de erro. |
| `[[erro]]` | 'erro:cpf-invalido', 'erro:senhas-nao-conferem' e etc | Retorna o callback de erro apresentado ao usuário. |

<br />

### Minha Conta - Catálogo de Endereços

**No clique nos elementos da página**<br />

- **Onde:** Na página de "Catálogo de Endereços"

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:catalogo-de-enderecos',
    'eventAction': 'clique:[[elemento]]',
    'eventLabel': '[[elemento-clicado]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[elemento]]` | 'botao' ou 'link' | Retorna o tipo de elemento clicado. |
| `[[elemento-clicado]]` | 'editar', 'excluir', 'adicionar-novo-endereco' e etc |Retorna o nome do elemento clicado.|

<br />

**No clique dos botões do Pop up para excluir um Endereço de Entrega**<br />

- **Onde:** Na página de "Catálogo de Endereços"

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:catalogo-de-enderecos:modal:excluir-endereco',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-botao]]` | 'cancelar', 'excluir' ou 'fechar' |Retorna o nome do botão clicado.|

<br />

**Na interação com os checkboxs**<br />

- **Onde:** Na página de "Catálogo de Endereços", etapa de "Adicionar novo endereço de entrega"

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:catalogo-de-enderecos:cadastrar-novo-endereco',
    'eventAction': 'interacao:checkbox:[[nome-opcao]]',
    'eventLabel': '[[acao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-opcao]]` | 'salvar-como-endereco-entrega-principal' ou 'salvar-como-endereco-de-cobranca-principal' |Retorna o nome da opção selecionada.|
| `[[acao]]` | 'selecionou' ou 'desselecionou' |Retorna a ação do usuário.|

<br />

**No clique do botão "Salvar", para salvar um novo endereço**<br />

- **Onde:** Na página de "Catálogo de Endereços", etapa de "Adicionar novo endereço de entrega"

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:catalogo-de-enderecos:cadastrar-novo-endereco',
    'eventAction': 'clique:botao',
    'eventLabel': 'salvar'
  });
</script>
```

<br />

**No callback de erro dos campos da página**<br />

- **Onde:** Na página de "Catálogo de Endereços", etapa de "Adicionar novo endereço de entrega"

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:catalogo-de-enderecos:cadastrar-novo-endereco',
    'eventAction': 'callback:campo:[[nome-campo]]',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-campo]]` | 'nome-completo', 'numero-de-telefone', 'cep' e etc |Retorna o nome do campo que houve algum callback informativo ou de erro.|
| `[[erro]]` | 'erro:esse-campo-nao-pode-ficar-vazio', 'erro:cep-invalido' e etc |Retorna o callback de erro apresentado ao usuário.|

<br />

### Minha Conta - Devolução Online e Retirar em Loja

**No clique em &quot;Visualizar Pedido&quot;**<br />

- **Onde:** Na página de &quot;Meus Pedidos&quot;
    - **Titulo ou nome do botão/link:** &quot;Visualizar Pedido&quot;
    
```html
<div
   data-gtm-event-category='riachuelo:minha-conta:meus-pedidos'
   data-gtm-event-action='clique:meus-pedidos'
   data-gtm-event-label='visualizar-pedido:[[id-pedido]]'
>Botão</div>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[id-pedido]]` | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |

<br />


**No clique das abas em &quot;Detalhes do pedido&quot;**<br />

- **Onde:** Na página de detalhes do pedido
    - **Titulo ou nome do botão/link:** &quot;Informações do Pedido&quot;, &quot;Produtos Comprados&quot;, &quot;Postagens do Pedido&quot;
    
```html
<div
   data-gtm-event-category='riachuelo:minha-conta:meus-pedidos'
   data-gtm-event-action='clique:detalhes-do-pedido'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-item]]` | &#039;informacoes-do-pedido&#039;, &#039;produtos-comprados&#039;, &#039;postagens-do-pedido&#039; | Deve retornar o nome do item clicado. |

<br />


**No clique em &quot;Trocar&quot; ou &quot;Devolver&quot;**<br />

- **Onde:** Na página de detalhes do pedido, em &quot;Produtos Comprados&quot; e dentro de produto
    - **Titulo ou nome do botão/link:** &quot;Trocar&quot; ou &quot;Devolver&quot;
    
```html
<div
   data-gtm-event-category='riachuelo:minha-conta:meus-pedidos'
   data-gtm-event-action='clique:produto:detalhes-do-pedido'
   data-gtm-event-label='[[devolver ou trocar]]:[[id-pedido]]:[[id-produto-filho]]'
>Botão</div>
```


| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[devolver ou trocar]]` | &#039;devolver&#039; ou &#039;trocar&#039; | Deve retornar o nome do item clicado. |
| `[[id-pedido]]` | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| `[[id-produto-filho]]` | &#039;0655&#039;, &#039;8566&#039;. No caso de mais que um ID do produto na devolução enviar &#039;1895/0655/8566&#039; | Deve retornar os ids dos produtos filhos referente ao pedido de devolução. |

<br />


**No clique dos botões &quot;Voltar&quot;, &quot;Avançar&quot; ou &quot;Fechar&quot; - Disparar clique de &quot;Avançar&quot; apenas quando &quot;Li e Concordo com os termos” estiver selecionado**<br />

- **Onde:** No modal de &quot;Termos e Regras de Devolução&quot;
    - **Titulo ou nome do botão/link:** &quot;Voltar&quot;, &quot;Aceito&quot; e &quot;Fechar&quot;
    
```html
<div
   data-gtm-event-category='riachuelo:minha-conta:meus-pedidos'
   data-gtm-event-action='clique:termos-e-regras-de-devolucao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-botao]]` | &#039;fechar&#039;, &#039;voltar&#039; ou &#039;aceito&#039;  | Deve retornar o nome do botão ou ícone clicado. |

<br />

**Na interação com os itens relacionados à devolução**<br />

- **Onde:** Na página de &quot;Solicitação de Devolução&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
    'eventAction': 'interacao:solicitacao-de-devolucao',
    'eventLabel': '[[motivo-da-solicitacao]]:[[id-pedido]]:[[id-produto-filho]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[motivo-da-solicitacao]]` | &#039;arrependimento&#039;, &#039;defeito&#039;, etc | Deve retornar a opção selecionada em &quot;Motivo da solicitação&quot;. |
| `[[id-pedido]]` | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| `[[id-produto-filho]]` | &#039;0655&#039;, &#039;8566&#039;. No caso de mais que um ID do produto na devolução enviar &#039;1895/0655/8566&#039; | Deve retornar os ids dos produtos filhos referente ao pedido de devolução. |

<br />

**No clique dos botões &quot;Voltar&quot; e&quot;Confirmar&quot;**<br />

- **Onde:** Na página de &quot;Solicitação de Devolução&quot;
    - **Titulo ou nome do botão/link:** &quot;Voltar&quot; e &quot;Confirmar&quot;
    
```html
<div
   data-gtm-event-category='riachuelo:minha-conta:meus-pedidos'
   data-gtm-event-action='clique:solicitacao-de-devolucao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-botao]]` | &#039;voltar&#039; ou &#039;confirmar&#039;  | Deve retornar o nome do botão clicado. |

<br />

**Na aparição do alerta após o carregamento da página de solicitação de devolução (Obs: quando a solicitação não for válida por conta de compra com valor inferior ao valor mínimo retornar o valor mínimo na mensagem de erro.)**<br />

- **Onde:** Na página de &quot;Solicitação de Devolução&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
    'eventAction': 'alerta:solicitacao-de-devolucao',
    'eventLabel': '[[mensagem-alerta]]:[[id-pedido]]:[[id-produto-filho]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[mensagem-alerta]]` | &#039;nao-aceitamos-a-devolucao-via-autoatendimento-com-valor-inferior-a-R$80,90&#039; etc.  | Deve retornar a mensagem de alerta. |
| `[[id-pedido]]` | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| `[[id-produto-filho]]` | &#039;0655&#039;, &#039;8566&#039;. No caso de mais que um ID do produto na devolução enviar &#039;1895/0655/8566&#039; | Deve retornar os ids dos produtos filhos referente ao pedido de devolução. |

<br />

**No clique em &quot;Voltar&quot; ou &quot;Avançar&quot;**<br />

- **Onde:** No modal de &quot;Resumo de Pedido&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
    'eventAction': 'clique:resumo:solicitacao-de-devolucao',
    'eventLabel': '[[nome-botao]]:[[id-pedido]]:[[id-produto-filho]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-botao]]` | &#039;avancar&#039;,  &#039;voltar&#039; | Deve retornar o nome do botão clicado. |
| `[[id-pedido]]` | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| `[[id-produto-filho]]` | &#039;0655&#039;, &#039;8566&#039;. No caso de mais que um ID do produto na devolução enviar &#039;1895/0655/8566&#039; | Deve retornar os ids dos produtos filhos referente ao pedido de devolução. |

<br />

**No callback da tentativa de solicitação de devolução ou troca**<br />

- **Onde:** Na página de &quot;Solicitação de Devolução ou Troca&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
    'eventAction': 'callback:solicitacao-de-devolucao',
    'eventLabel': '[[sucesso ou tipo-erro]]:[[id-pedido]]:[[id-produto-filho]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[sucesso ou tipo-erro]]` | &#039;sucesso&#039;, &#039;ocorreu-um-erro&#039;, etc | Deve retornar o sucesso ou descrição do erro de solicitação de devolução. |
| `[[id-pedido]]` | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| `[[id-produto-filho]]` | &#039;0655&#039;, &#039;8566&#039;. No caso de mais que um ID do produto na devolução enviar &#039;1895/0655/8566&#039; | Deve retornar os ids dos produtos filhos referente ao pedido de devolução. |

<br />

**Ao selecionar opção de autorizar retirada de pedido por terceiros**<br />

- **Onde:** Na página de detalhes de pedido
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
    'eventAction': 'interacao:meus-pedidos',
    'eventLabel': 'autorizo-a-retirada-do-meu-pedido-por-outras-pessoas'
  });
</script>
```

<br />

**No clique dos botões e ícones do modal de retirada de pedido por terceiros**<br />

- **Onde:** Na página de detalhes de pedido
    
```html
<div
   data-gtm-event-category='riachuelo:minha-conta:meus-pedidos'
   data-gtm-event-action='clique:retirada-por-terceiros'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-botao]]` | &#039;adicionar&#039;, &#039;fechar&#039; &#039;salvar-e-finalizar&#039;, &#039;remover&#039; | Deve retornar o nome do botão ou ícone clicado. |

<br />

**No callback da tentativa de inserção de terceiros para retirar pedido**<br />

- **Onde:** Na página de detalhes de pedido
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
    'eventAction': 'tentativa:retirada-por-terceiros',
    'eventLabel': '[[sucesso ou tipo-erro]]:[[id-pedido]]:[[numero-pessoas]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[sucesso ou tipo-erro]]` | &#039;sucesso&#039;, &#039;ocorreu-um-erro&#039; etc | Deve retornar o sucesso ou descrição do erro de inserção de terceiros para retirada de pedidos. |
| `[[id-pedido]]` | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| `[[numero-pessoas]]` | &#039;1&#039; ou &#039;2&#039;. | Deve retornar o numero de pessoas que farão a retirada. |

<br />

**No clique das perguntas &#039;Por que meu pedido foi cancelado?&#039;, &#039;Por que o pedido tem mais de uma entrega?&#039;, &#039;Por que este item foi cancelado&#039; e perguntas similares.**<br />

- **Onde:** Na página de detalhes de pedido
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
    'eventAction': 'clique:pergunta',
    'eventLabel': '[[nome-pergunta]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-pergunta]]` | &#039;por-que-o-meu-pedido-foi-cancelado&#039;, &#039;por-que-o-pedido-tem-mais-de-uma-entrega&#039;, &#039;por-que-este-item-foi-cancelado&#039; e etc. | Deve retornar o nome da pergunta clicada. |

<br />

**No clique de &#039;sim&#039; ou &#039;não&#039;**<br />

- **Onde:** No modal apresentado na pagina, após clicar na pergunta mostrada em &#039;detalhes do pedido&#039;.
    
```html
<div
   data-gtm-event-category='riachuelo:minha-conta:meus-pedidos'
   data-gtm-event-action='informacao-foi-util-pra-voce'
   data-gtm-event-label='[[nome-pergunta]]:[[sim-nao]]'
>Botão</div>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-pergunta]]` | &#039;por-que-o-meu-pedido-foi-cancelado&#039;, &#039;por-que-o-pedido-tem-mais-de-uma-entrega&#039;, &#039;por-que-este-item-foi-cancelado&#039; e etc. | Deve retornar o nome da pergunta clicada. |
| `[[sim-nao]]` | &#039;sim&#039; ou &#039;nao&#039;. | Deve retornar se o usuario clicou em sim ou nao. |

<br />

### Checkout 

**Na interação com o campo para calcular o frete e prazo**<br />

- **Onde:** No fluxo de checkout, etapa de Sacola
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'interacao:campo:sacola',
    'eventLabel': 'digite-seu-cep'
  });
</script>
```

<br />

**No callback para calcular o frete, apos clicar em aplicar**<br />

- **Onde:** No fluxo de checkout, etapa de Sacola
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'callback:sacola',
    'eventLabel': 'calcular-frete:[[sucesso ou tipo-de-erro]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[sucesso ou tipo-de-erro]]` | 'sucesso', 'erro:nao-e-possivel-entregar-neste-cep' e etc| Deve retornar a mensagem de sucesso ou tipo de erro. |

<br />

**No callback de erro para adição e remoção de produtos no carrinho**<br />

- **Onde:** Na etapa sacola e Checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'callback:quantidade:[[etapa]]',
    'eventLabel': '[[status]]:[[quantidade]]:[[categoria-produto]]',
    'dimension30': '[[product-sub-categoria]]',
    'dimension40': '[[product-exclusivo-ecommerce+marketplace]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[etapa]]` |'sacola', 'entrega-e-pagamento' e etc| Deve retornar a etapa em que ocorreu o clique. |
| `[[status]]` | 'erro:nao-existe-a-quantidade-em-estoque', 'restam-apenas-2-unidades-deste-item-no-estoque' e etc| Deve retornar a mensagem de erro. |
| `[[quantidade]]` | '1', '3', '4' e etc | Deve retornar a quantidade. |
| `[[categoria-produto]]` |  'masculino', 'feminino' e etc| Deve retornar a categoria do produto. |
| `[[product-sub-categoria]]` | '310090'| Código da categoria GM  |
| `[[product-exclusivo-ecommerce+marketplace]]` |'sim:richlo', 'nao:richlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller'| Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |

<br />

**No clique dos botões do modal "Tem certeza que deseja excluir este prouto?"**<br />

- **Onde:** No modal "Deseja excluir este produto", após clicar no ícone de "Remover tudo"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'clique:modal:deseja-excluir-este-produto',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-botao]]` | 'voltar', 'excluir', 'fechar' e etc | Retorna o nome do botão clicado. |

<br />

**No clique do ícone de informação, onde informa que você pode comprar até o máximo 10 unidades do item**<br />

- **Onde:** No fluxo de checkout, etapa de sacola, quando excede o máximo de 10 produtos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'clique:icone:informacao',
    'eventLabel': 'maximo-de-10-unidades-desse-item'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-botao]]` | 'voltar', 'excluir', 'fechar' e etc | Retorna o nome do botão clicado. |

<br />

**No clique dos links e botões**<br />

- **Onde:** No fluxo de checkout e sacola
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'clique:[[tipo-item]]:[[etapa]]',
    'eventLabel': '[[nome-item]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[tipo-item]]` | 'botao', 'link' | Deve retornar se o item é um botão ou um link.  |
| `[[etapa]]` | 'sacola', 'entrega-e-pagamento' e etc | Deve retornar a etapa em que ocorreu o clique. |
| `[[nome-item]]` | 'editar-sacola', 'nao-sei-meu-cep', 'editar-endereco', 'mais-opcoes-de-envio', 'aplicar-cupom' e etc | Deve retornar o nome do item clicado. |

<br />

**No callback ao carregar a página de checkout. Obs.: Um exemplo é quando o produto não poderá ser entregue na região selecionada, este produto será removido automaticamente do carrinho e será disparado um callback de erro explicando o caso**<br />

- **Onde:** Na página de checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'callback:[[etapa]]',
    'eventLabel': '[[sucesso ou tipo-erro]]',
    'dimension40': '[[product-exclusivo-ecommerce+marketplace]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[etapa]]` | 'sacola', 'entrega-e-pagamento' e etc | Deve retornar a etapa em que ocorreu o clique. |
| `[[sucesso ou tipo-erro]]` | 'sucesso', 'erro:cupom-invalido', 'erro:produto-indisponivel' e etc.| Deve retornar sucesso ou o tipo de erro. |
| `[[product-exclusivo-ecommerce+marketplace]]` |'sim:richlo', 'nao:richlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller'| Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |

<br />

**No tipo de entrega selecionada**<br />

- **Onde:** Na página de pdp e checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'selecionou:entrega:[[origem-lista]]',
    'eventLabel': '[[tipo-frete]]:[[previsao]]:[[valor]]',
    'dimension40': '[[product-exclusivo-ecommerce+marketplace]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[origem-lista]]` |'pdp', 'checkout' ou 'sacola' | Deve retornar o local de origem que a lista de tipos de entrega aparece. |
| `[[tipo-frete]]` | 'expresso ', 'entrega-agendada', 'retirar-em-loja', 'retirar-em-loja-expresso' | Deve retornar os nomes dos tipos de frete. |
| `[[previsao]]` | 'DD/MM/AAAA' | Deve retornar a quantidade de dias para a entrega. |
| `[[valor]]` | '0.00', '2.99', '5.00'| Deve retornar os valores dos fretes. |
| `[[product-exclusivo-ecommerce+marketplace]]` |'sim:richlo', 'nao:richlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller'| Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |

<br />

**No clique do icones e botões no modal "Meus Endereços Salvos"**<br />

- **Onde:** Na página de checkout, após clicar no ícone de "Editar Endereço" na página de "Entrega e Pagamento"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout:modal:enderecos-salvos',
    'eventAction': 'clique:[[icone ou botao]]',
    'eventLabel': '[[nome-item]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[icone ou botao]]` |'icone', 'botao' | Deve retornar o tipo de elemento clicado.  |
| `[[nome-item]]` | 'fechar', 'editar-endereco', 'novo-endereco', 'salvar' e etc |Deve retornar o nome do item clicado. |

<br />

**No clique dos botão ou link no resumo do pedido**<br />

- **Onde:** Na página de checkout, etapas Sacola e Entrega e Pagamento
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'clique:[[botao ou link]]:[[etapa]]',
    'eventLabel': 'resumo-pedido:[[nome-item]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[botao ou link]]` |'botao' ou 'link' | Deve retornar o tipo de elemento clicado. |
| `[[etapa]]` | 'sacola', 'entrega-e-pagamento', 'identificacao' e etc | Deve retornar a etapa em que ocorreu o clique. |
| `[[nome-item]]` | 'continuar-comprando, 'ir-para-pagamento', 'finalizar-pedido' e etc |Deve retornar o nome do item clicado. |

<br />

**Na interação com os tipos de cartão na seção de 'pagamentos', para saber se o cliente selecionou um cartão salvo ou clicou em usar outro cartão**<br />

- **Onde:** Na página de checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'selecionou:cartao',
    'eventLabel': '[[tipo-cartao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[tipo-cartao]]` |'usar-outro-cartao' ou 'cartao-salvo'e etc | Deve retornar  se o cliente estava usando um cartão salvo para pagar ou se ele clicou em usar outro cartao.  |

<br />

**No callback ao adicionar um cupom**<br />

- **Onde:** Na página de checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'callback:adicionar-cupom',
    'eventLabel': '[[sucesso-ou-tipo-erro]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[sucesso-ou-tipo-erro]]` |'sucesso', 'erro:cupom-invalido' e etc.|Deve retornar sucesso ou o tipo de erro. |

<br />

**Ao clicar no botão 'consultar' ou 'aplicar', após selecionar 'Cartão Presente', 'Vale troca' ou outros cartões como forma de pagamento**<br />

- **Onde:** Na página de checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]:[[tipo-cartao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-botao]]` |'consultar' , 'aplicar' e etc| Deve retornar o nome do botão clicado. |
| `[[tipo-cartao]]` |'cartao-presente', 'vale-troca', 'cartao-de-credito' e etc.|Deve retornar o tipo de cartão removido. |

<br />

**Ao clicar no icone de 'remover', após selecionar 'Cartão Presente' , 'Vale Troca' ou outros cartões como forma de pagamento**<br />

- **Onde:** Na página de checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'clique:icone',
    'eventLabel': 'remover:[[tipo-cartao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[tipo-cartao]]` |'cartao-presente', 'vale-troca', 'cartao-de-credito' e etc.|Deve retornar o tipo de cartão removido. |

<br />

**Ao clicar no icone de 'remover', após selecionar 'Cartão Presente' , 'Vale Troca' ou outros cartões como forma de pagamento**<br />

- **Onde:** Na página de checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'clique:icone',
    'eventLabel': 'remover:[[tipo-cartao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[tipo-cartao]]` |'cartao-presente', 'vale-troca', 'cartao-de-credito' e etc.|Deve retornar o tipo de cartão removido. |

<br />

**No callback ao incluir alguma "Forma de Pagamento"**<br />

- **Onde:** Na página de checkout, etapa "Formas de Pagamento"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'callback:[[forma-pagamento]]',
    'eventLabel': '[[sucesso-ou-tipo-erro]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[forma-pagamento]]` |'cartao-de-credito', 'vale-troca', 'cartao-presente' e etc|Retorna a forma de pagamento.|
| `[[sucesso-ou-tipo-erro]]` |'sucesso', 'erro:por-favor-insira-um-numero-de-vale-troca-valido', 'erro:cartao-presente-invalido', 'erro:cartao-de-credito-invalido' e etc.|Deve retornar sucesso ou o tipo de erro |

<br />

**No clique dos botões da etapa de cadastro**<br />

- **Onde:** Nas etapas de identificação do checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout:identificacao:[[login-cadastro-esqueci-senha]]',
    'eventAction': 'clique:[[botao ou link]]',
    'eventLabel': '[[nome-item]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[login-cadastro-esqueci-senha]]` |'login', 'formulario-de-cadastro', 'esqueci-senha' e etc |Retorna a etapa da indentificação que aconteceu o clique. |
| `[[botao ou link]]` |'botao' ou 'link'|Deve retornar o tipo de elemento clicado.  |
| `[[nome-item]]` |'continuar', 'esqueceu-a-senha', 'privacidade-e-condicoes', 'nao-sei-meu-cep' e etc |Deve retornar o nome do item clicado. |

<br />

**No callback de erro dos campos em todas as etapas de identificação**<br />

- **Onde:** Nas etapas de identificação do checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout:identificacao:[[login-cadastro-esqueci-senha]]',
    'eventAction': 'callback:campo:[[nome-campo]]',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[login-cadastro-esqueci-senha]]` |'login', 'formulario-de-cadastro', 'esqueci-senha' e etc |Retorna a etapa da indentificação que aconteceu o clique. |
| `[[nome-campo]]` |'nome', 'telefone', 'email', 'cpf', 'senha', 'confirma-senha' e etc|Retorna o nome do campo preenchido. |
| `[[erro]]` |'erro:cpf-invalido', 'erro:senha-invalida', 'erro:cep-invalido', 'erro:senha-diferente-da-anterior' e etc |Deve retornar o erro que o campo apresentar. |

<br />

**No clique nos botões e links**<br />

- **Onde:** Na página de sucesso de compra
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout-sucesso',
    'eventAction': 'clique:[[tipo-item]]',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[tipo-item]]` |'botao', 'link' |Deve retornar o tipo do item clicado. |
| `[[nome-botao]]` |'cadastrar', 'copiar-codigo', 'imprimir-boleto', 'politica-privacidade', 'sua-conta' e etc|Deve retornar o nome do botão clicado. |

<br />

**No callback de erro ao tentar finalizar uma compra**<br />

- **Onde:** Ao tentar finalizar a compra
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'callback:tentativa-de-compra',
    'eventLabel': '[[status]]'
  });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[status]]` |'sucesso' ,'erro:dados-invalidos', 'erro:cartao-invalido', 'erro:endereco-invalido' e etc|Deve retornar se a compra foi realizada com sucesso ou o tipo de  erro relacionado.|

<br />

### Baixe o APP

**No cliques que redirecionam para baixar o App nas Lojas Apple Store e Google Play**<br />

- **Onde:** Na página Baixe o App.
    
```html
<div
   data-gtm-event-category='riachuelo:baixe-o-app'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[botao-clicado]]'
>Botão</div>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[botao-clicado]]` | 'apple-store', 'google-play'| Deve retornar qual botão o usuário clicou. |

<br />

### LP Cadastro Live

**No clique do botão 'CADASTRAR'**<br />

- **Onde:** Na LP cadastro live
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:lp:live-commerce',
    'eventAction': 'clique:botao',
    'eventLabel': 'cadastrar'
  });
</script>
```

<br />

### Novo Enhanced E-commerce 

**Na visualização dos banners promocionais**<br />

- **Onde:**  Em todas as páginas que exibirem banners promocionais (home, PLP, CLP)
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'promotionImpression',
    'noInteraction': '1',
    'eventCategory': 'enhanced-ecommerce',
    'eventAction': 'promoView',
    'ecommerce': {
      'promoView': {
        'promotions': [{
          'id': '[[id-promocao]]',
          'name': '[[nome-promocao]]',
          'position': '[[posicao-promocao]]',
          'creative': '[[arte-banner]]'
        }]
      }
    }
  });
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[id-promocao]]` | &quot;https://midia.fotos-riachuelo.com.br/fotos/megamenu/home/janeiro2020/ADS-5506/Carrossel-home-mcasa-kit-colcha_.jpg&quot; e etc | Deve retornar o link da imagem do banner |
| `[[nome-promocao]]` | "polos-diferenciadas", "moschino-for-riachuelo", "carters-leve-segunda-peca-50-off" | Deve retornar o nome amigável do banner |
| `[[arte-banner]]` | "hero:home", "hero:infantil", "faixa-superior:home", 'faixa-1:home", 'barra-de-servicos:home' "mosaico:home", "promocional:home", "blog:home" e etc | Deve retornar o formato, o tipo de página que o banner foi clicado e o tipo do banner  |
| `[[posicao-promocao]]` | "1", "2", "3" e etc | Deve retornar a posição que o banner é exibido  |

<br />

**No clique dos banners**<br />

- **Onde:** Em todas as páginas que exibirem banners (home, clp, plp)
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'promotionClick',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'promoClick',
  'ecommerce': {
    'promoClick': {
      'promotions': [{
        'id': '[[id-promocao]]',
        'name': '[[nome-promocao]]',
        'position': '[[posicao-promocao]]',
        'creative': '[[arte-banner]]'
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[id-promocao]]` | &quot;https://midia.fotos-riachuelo.com.br/fotos/megamenu/home/janeiro2020/ADS-5506/Carrossel-home-mcasa-kit-colcha_.jpg&quot; e etc | Deve retornar o link da imagem do banner |
| `[[nome-promocao]]` | "polos-diferenciadas", "moschino-for-riachuelo", "carters-leve-segunda-peca-50-off" | Deve retornar o nome amigável do banner |
| `[[arte-banner]]` | "hero:home", "hero:infantil", "faixa-superior:home", 'faixa-1:home", 'barra-de-servicos:home' "mosaico:home", "promocional:home", "blog:home" e etc | Deve retornar o formato, o tipo de página que o banner foi clicado e o tipo do banner  |
| `[[posicao-promocao]]` | "1", "2", "3" e etc | Deve retornar a posição que o banner é exibido  |

<br />

**Na visualização de uma vitrine de produtos**<br />

- **Onde:** Em todas as páginas que exibirem uma lista de produtos (home, PLP, PDP)
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productImpression',
  'noInteraction': '1',
  'eventCategory':'enhanced-ecommerce',
  'eventAction': 'impressions',
  'ecommerce': {
    'impressions': [{
      'dimension30': '[[product-sub-categoria]]',
      'dimension32': '[[product-preco-original]]',
      'dimension40': '[[product-exclusivoecommerce+marketplace]]',
      'dimension42': '[[product-sku-filho]]',
      'dimension58': '[[product-selo/tag]]',
      'name': '[[nome-produto]]',
      'id': '[[id-produto]]',
      'list': '[[lista-produto]]',
      'variant': '[[variacao-produto]]',
      'position': '[[posicao-produto]]',
      'brand': '[[marca-produto]]',
      'category': '[[categoria-produto]]',
      'price': '[[preco-produto]]'
    }]
  }
});
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[product-sub-categoria]]` | '310090' | Código da categoria GM  |
| `[[product-preco-original]]` |  '12', 'produto-temporariamente-indisponivel', 'indisponivel' etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| `[[product-exclusivoecommerce+marketplace]]` | 'sim:richlo', 'nao:richlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller' | Deve retornar se o produto é exclusivo do ecommerce e o seu id do  seller/marketplace |
| `[[product-sku-filho]]` | '2552' | ID filho do produto |
| `[[product-selo/tag]]` |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. OBS: Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| `[[nome-produto]]` | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| `[[id-produto]]` | &quot;13239635&quot; | SKU do produto - pai |
| `[[lista-produto]]` | "home:destaques", "plp:dia-das-maes:moderna", "clp:masculino:plus-size:camiseta", "clp:moda-intima:linha-plus-size:cintas-e-modeladores", "home:novidades", "home:outlet", "plp:moschino-for-riachuelo", "pdp:quem-viu-tambem-gostou" e etc | Nome da lista que o produto aparece, no caso de PLP, o nome da PLP tem que ser a URL ao invés de usar a “/” colocar “-“ |
| `[[posicao-produto]]` | &quot;2&quot; | Posição que o produto aparece em uma lista de produtos |
| `[[variacao-produto]]` | &quot;325&quot; | Código DCO - Categoria do produto |
| `[[marca-produto]]` |"pool-original" | Marca do produto |
| `[[categoria-produto]]` |"masculino" | Departamento do produto |
| `[[preco-produto]]` | "139.99" | Preço do produto |

<br />

**No clique dos produtos da vitrine interagidos na página**<br />

- **Onde:** Em todas as páginas que exibirem uma lista de produtos (home, CLP, PLP, PDP)
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productClick',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'click',
  'ecommerce': {
    'click': {
      'actionField': {'list': '[[lista-produto]]'},
      'products': [{
        'dimension30': '[[product-sub-categoria]]',
        'dimension32': '[[product-preco-original]]',
        'dimension40': '[[product-exclusivoecommerce+marketplace]]',
        'dimension42': '[[product-sku-filho]]',
        'dimension58': '[[product-selo/tag]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'list': '[[lista-produto]]',
        'variant': '[[variacao-produto]]',
        'position': '[[posicao-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'price': '[[preco-produto]]'
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[product-sub-categoria]]` | '310090' | Código da categoria GM  |
| `[[product-preco-original]]` |  '12', 'produto-temporariamente-indisponivel', 'indisponivel' etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| `[[product-exclusivoecommerce+marketplace]]` | 'sim:richlo', 'nao:richlo', 'nao:pontofrio', 'nao:extra', 'nao:sem-seller' | Deve retornar se o produto é exclusivo do ecommerce e o seu id do  seller/marketplace |
| `[[product-sku-filho]]` | '2552' | ID filho do produto |
| `[[product-selo/tag]]` |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. OBS: Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| `[[nome-produto]]` | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| `[[id-produto]]` | &quot;13239635&quot; | SKU do produto - pai |
| `[[lista-produto]]` | "home:destaques", "plp:dia-das-maes:moderna", "clp:masculino:plus-size:camiseta", "clp:moda-intima:linha-plus-size:cintas-e-modeladores", "home:novidades", "home:outlet", "plp:moschino-for-riachuelo", "pdp:quem-viu-tambem-gostou" e etc | Nome da lista que o produto aparece, no caso de PLP, o nome da PLP tem que ser a URL ao invés de usar a “/” colocar “-“ |
| `[[posicao-produto]]` | &quot;2&quot; | Posição que o produto aparece em uma lista de produtos |
| `[[variacao-produto]]` | &quot;325&quot; | Código DCO - Categoria do produto |
| `[[marca-produto]]` |"pool-original" | Marca do produto |
| `[[categoria-produto]]` |"masculino" | Departamento do produto |
| `[[preco-produto]]` | "139.99" | Preço do produto |

<br />

**No carregamento da página de detalhe ou no quickview com informações detalhadas de produto.**<br />

- **Onde:** Na página de detalhes de produto e na página de lista de produtos
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productDetail',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'detail',
  'noInteraction': '1',
  'ecommerce': {
    'detail': {
      'products': [{
        'dimension13': '[[cd13-product-idadedoproduto]]',
        'dimension27': '[[product-padronagemdoproduto]]',
        'dimension30': '[[product-subcategoria]]',
        'dimension32': '[[product-preçooriginal]]',
        'dimension38': '[[product-lifestyle]]',
        'dimension39': '[[product-gender]]',
        'dimension40': '[[product-exclusivoecommerce+marketplace]]',
        'dimension42': '[[product-skufilho]]',
        'dimension58': '[[product-selo/tag]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'category': '[[categoria-produto]]'
      }]
    }
  }
 });
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[cd13-product-idadedoproduto]]` |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| `[[product-padronagemdoproduto]]` | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| `[[product-subcategoria]]` |  &#039;310090&#039; | Código da categoria GM  |
| `[[product-subcategoria]]` |  &#039;310090&#039; | Código da categoria GM  |
| `[[product-preçooriginal]]` |  &#039;12&#039;, &#039;produto-temporariamente-indisponivel&#039;,&#039;indisponivel&#039; etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| `[[product-lifestyle]]` | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| `[[product-gender]]` | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| `[[product-exclusivoecommerce+marketplace]]` | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e seu id do seller/marketplace |
| `[[product-skufilho]]` |  &#039;2552&#039; | ID filho do produto |
| `[[product-selo/tag]]` |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. OBS: Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| `[[nome-produto]]` | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| `[[id-produto]]` | &quot;13239635&quot; | SKU do produto - pai |
| `[[preco-produto]]` | &quot;139.99&quot; | Preço do produto |
| `[[categoria-produto]]` | &quot;masculino&quot; | Departamento do produto |

<br />

**No clique para adicionar produto ao carrinho**<br />

- **Onde:** Na página de detalhes de um produto
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'addToCart',
  'eventCategory':'enhanced-ecommerce',
  'eventAction': 'add',
    'ecommerce': {
    'add': {
      'products': [{
        'dimension13': '[[cd13-product-idadedoproduto]]',
        'dimension24': '[[product-ordemdeinserção]]',
        'dimension25': '[[product-tamanhodoproduto]]',
        'dimension26': '[[product-cordoproduto]]',
        'dimension27': '[[product-padronagemdoproduto]]',
        'dimension30': '[[product-subcategoria]]',
        'dimension32': '[[product-preçooriginal]]',
        'dimension38': '[[product-lifestyle]]',
        'dimension39': '[[product-gender]]',
        'dimension40': '[[product-exclusivoecommerce+marketplace]]',
        'dimension42': '[[product-skufilho]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[cd13-product-idadedoproduto]]` |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| `[[product-ordemdeinserção]]` | 1,2,3 .. | Ordem de inserção ao carrinho |
| `[[product-tamanhodoproduto]]` |  &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| `[[product-cordoproduto]]` | &#039;vermelho&#039; | Cor do produto  |
| `[[product-padronagemdoproduto]]` | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| `[[product-subcategoria]]` |  &#039;310090&#039; | Código da categoria GM  |
| `[[product-subcategoria]]` |  &#039;310090&#039; | Código da categoria GM  |
| `[[product-preçooriginal]]` | &#039;12&#039;, &#039;produto-temporariamente-indisponivel&#039;,&#039;indisponivel&#039; etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| `[[product-lifestyle]]` | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| `[[product-gender]]` | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| `[[product-exclusivoecommerce+marketplace]]` | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do  seller/marketplace |
| `[[product-skufilho]]` |  &#039;2552&#039; | ID filho do produto |
| `[[nome-produto]]` | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| `[[id-produto]]` | &quot;13239635&quot; | SKU do produto - pai |
| `[[preco-produto]]` | &quot;139.99&quot; | Preço do produto |
| `[[marca-produto]]` | &quot;pool-original&quot; | Marca do produto |
| `[[categoria-produto]]` | &quot;masculino&quot; | Departamento do produto |
| `[[quantidade-produto]]` | &quot;1&quot; | Quantidade do produto |

<br />

**No clique da lixeira para remover produto do carrinho,**<br />

- **Onde:** No carrinho modal lateral - Sua Sacola;
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'removeFromCart',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'remove',
  'ecommerce': {
    'remove': {
      'products': [{
        'dimension13': '[[cd13-product-idadedoproduto]]',
        'dimension24': '[[product-ordemdeinserção]]',
        'dimension25': '[[product-tamanhodoproduto]]',
        'dimension26': '[[product-cordoproduto]]',
        'dimension27': '[[product-padronagemdoproduto]]',
        'dimension30': '[[product-subcategoria]]',
        'dimension32': '[[product-preçooriginal]]',
        'dimension38': '[[product-lifestyle]]',
        'dimension39': '[[product-gender]]',
        'dimension40': '[[product-exclusivoecommerce+marketplace]]',
        'dimension42': '[[product-skufilho]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[cd13-product-idadedoproduto]]` |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| `[[product-ordemdeinserção]]` | 1,2,3 .. | Ordem de inserção ao carrinho |
| `[[product-tamanhodoproduto]]` |  &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| `[[product-cordoproduto]]` | &#039;vermelho&#039; | Cor do produto  |
| `[[product-padronagemdoproduto]]` | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| `[[product-subcategoria]]` |  &#039;310090&#039; | Código da categoria GM  |
| `[[product-subcategoria]]` |  &#039;310090&#039; | Código da categoria GM  |
| `[[product-preçooriginal]]` | &#039;12&#039;, &#039;produto-temporariamente-indisponivel&#039;,&#039;indisponivel&#039; etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| `[[product-lifestyle]]` | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| `[[product-gender]]` | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| `[[product-exclusivoecommerce+marketplace]]` | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do  seller/marketplace |
| `[[product-skufilho]]` |  &#039;2552&#039; | ID filho do produto |
| `[[nome-produto]]` | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| `[[id-produto]]` | &quot;13239635&quot; | SKU do produto - pai |
| `[[preco-produto]]` | &quot;139.99&quot; | Preço do produto |
| `[[marca-produto]]` | &quot;pool-original&quot; | Marca do produto |
| `[[categoria-produto]]` | &quot;masculino&quot; | Departamento do produto |
| `[[quantidade-produto]]` | &quot;1&quot; | Quantidade do produto |

<br />

**No carregamento das páginas do fluxo de compra do  checkout.**<br />

- **Onde:** Nas páginas de Carrinho, CPF, Cadastro, Entrega e Pagamento
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'checkout',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'checkout',
  'noInteraction': '1',
  'ecommerce': {
    'checkout': {
      'actionField': {'step': '[[passo-checkout]]'},
      'products': [{
        'dimension13': '[[cd13-product-idadedoproduto]]',
        'dimension24': '[[product-ordemdeinserção]]',
        'dimension25': '[[product-tamanhodoproduto]]',
        'dimension26': '[[product-cordoproduto]]',
        'dimension27': '[[product-padronagemdoproduto]]',
        'dimension30': '[[product-subcategoria]]',
        'dimension32': '[[product-preçooriginal]]',
        'dimension38': '[[product-lifestyle]]',
        'dimension39': '[[product-gender]]',
        'dimension40': '[[product-exclusivoecommerce+marketplace]]',
        'dimension42': '[[product-skufilho]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[cd13-product-idadedoproduto]]` |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| `[[product-ordemdeinserção]]` | 1,2,3 .. | Ordem de inserção ao carrinho |
| `[[product-tamanhodoproduto]]` |  &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| `[[product-cordoproduto]]` | &#039;vermelho&#039; | Cor do produto  |
| `[[product-padronagemdoproduto]]` | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| `[[product-subcategoria]]` |  &#039;310090&#039; | Código da categoria GM  |
| `[[product-subcategoria]]` |  &#039;310090&#039; | Código da categoria GM  |
| `[[product-preçooriginal]]` | &#039;12&#039;, &#039;produto-temporariamente-indisponivel&#039;,&#039;indisponivel&#039; etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| `[[product-lifestyle]]` | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| `[[product-gender]]` | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| `[[product-exclusivoecommerce+marketplace]]` | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| `[[product-skufilho]]` |  &#039;2552&#039; | ID filho do produto |
| `[[nome-produto]]` | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| `[[id-produto]]` | &quot;13239635&quot; | SKU do produto - pai |
| `[[preco-produto]]` | &quot;139.99&quot; | Preço do produto |
| `[[marca-produto]]` | &quot;pool-original&quot; | Marca do produto |
| `[[categoria-produto]]` | &quot;masculino&quot; | Departamento do produto |
| `[[quantidade-produto]]` | &quot;1&quot; | Quantidade do produto |
| `[[passo-checkout]]` | &quot;1&quot; | Na etapa de sacola de compras |
| `[[passo-checkout]]` | &quot;2&quot; | No carregamento da página de formulário de cadastro para usuarios que estão no fluxo de compra e não possui conta. |
| `[[passo-checkout]]` | &quot;3&quot; | No carregamento da página de confirmação do endereço de entrega e frete |
| `[[passo-checkout]]` | &quot;4&quot; | No carregamento da página de seleção do método de pagamento|

<br />

**Ao selecionar uma opção de frete**<br />

- **Onde:** Na página de carrinho e de checkout
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'checkoutOption',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'checkoutoption',
  'eventLabel': 'tipo-de-frete',
    'ecommerce': {
    'checkout_option': {
        'actionField':  {'step':'3','option':'shipping:[[nome-entrega]]:[[valor]]:[[previsao]]'},
    }
  }
});
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[nome-entrega]]` | &#039;normal&#039;, &#039;expresso &#039;, &#039;entrega-agendada&#039;,&#039;retirar-em-loja:shopping-tiete-plaza&#039;, &#039;retirar-em-loja:shopping-bourbon-sp&#039; etc | Deve retornar os nomes dos tipos de entrega. |
| `[[valor]]` | &#039;&#039;0.00&#039;, &#039;2.99&#039;, &#039;5.00&#039;. | Deve retornar os valores dos fretes. |
| `[[previsao]]` | &#039;3-dias, 4-dias,5-dias&#039; | Deve retornar a quantidade de dias. |

<br />

**Apos selecionar todas as formas de pagamento, OBSERVAÇÃO: Se não houver opções adicionais de pagamento retornar as variaveis vazias.**<br />

- **Onde:** Na página de carrinho e de checkout

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'checkoutOption',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'checkoutoption',
  'eventLabel': 'tipo-de-pagamento',
  'dimension22': '[[cd22-hit-metodopagamento]]',
  'metric5': '[[cartãopresente]]',
  'metric6': '[[valetroca]]',
  'metric7': '[[pagamentocomum]]',
   'ecommerce': {
    'checkout_option': {
        'actionField':  {'step':'4','option':'payment:[[tipo-pagamento]]:[[opcao-adicional-1]]:[[opcao-adicional-2]]'},
    }
  }
});
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[tipo-pagamento]]` | &#039;cartao&#039;, &#039;paypal&#039;, &#039;boleto&#039; etc | Deve retornar o nome da opção de pagamento clicada. |
| `[[opcao-adicional-1]]` | &#039;vale-troca&#039; | Deve retornar a opção de pagamento adicional.(Se não houver opções adicionais de pagamento retornar as variaveis vazias.) |
| `[[opcao-adicional-2]]` | &#039;cartao-presente&#039;, &#039;cartao-presente/vale-troca&#039; e etc | Deve retornar a segunda opção(Se não houver opções adicionais de pagamento retornar as variaveis vazias) |
| `[[cd22-hit-metodopagamento]]` |  &#039;cartao-credito&#039;, &#039;cartao-credito/vale-troca&#039;, &#039;boleto&#039;  | Nome de todos os tipos de pagamento selecionados |
| `[[cartãopresente]]` | 100.00&#039;, &#039;50.00&#039; e etc | Deve retornar o valor pago por cartão presente utilizado. |
| `[[valetroca]]` | 40.00&#039;, &#039;23.00&#039;, &#039;200.00&#039; | Deve retornar o valor pago pelo vale troca utilizado. |
| `[[pagamentocomum]]` | 140.00&#039;, &#039;67.00&#039;, &#039;100.00&#039; | Deve retornar o valor pago em formas de  pagamentos comuns como, por exemplo, boleto, cartão de credito e etc |

<br />

**No carregamento da página  de resumo de compra  após confirmação.**<br />

- **Onde:** Na página de confirmação de compra
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'purchase',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'purchase',
  'noInteraction': '1',
  'dimension2': '[[cd2-tipodecartão]]',
  'dimension3': '[[cd3-bandeiracartão]]',
  'dimension22': '[[cd22-hit-metodopagamento]]',
  'dimension23': '[[cd23-hit-statuspagamento]]',
  'dimension28': '[[cd28-hit-frete]]',
  'dimension46': '[[cd46-hit-parcelamento]]',
  'metric1': '[[descontofrete]]',
  'metric2': '[[descontofuncionário]]',
  'metric3': '[[descontopromoção]]',
  'metric4': '[[totaldedesconto]]',
  'metric5': '[[cartãopresente]]',
  'metric6': '[[valetroca]]',
  'metric7': '[[pagamentocomum]]',
  'ecommerce': {
    'purchase': {
      'actionField': {
        'id': '[[id-transacao]]',
        'revenue': '[[valor-total-transacao]]',
        'shipping': '[[frete-transacao]]',
        'tax': '[[taxa-transacao]]',
        'coupon': '[[coupon-transacao]]',
      },
      'products': [{
        'dimension13': '[[cd13-product-idadedoproduto]]',
        'dimension24': '[[product-ordemdeinserção]]',
        'dimension25': '[[product-tamanhodoproduto]]',
        'dimension26': '[[product-cordoproduto]]',
        'dimension27': '[[product-padronagemdoproduto]]',
        'dimension30': '[[product-subcategoria]]',
        'dimension32': '[[product-preçooriginal]]',
        'dimension38': '[[product-lifestyle]]',
        'dimension39': '[[product-gender]]',
        'dimension40': '[[product-exclusivoecommerce+marketplace]]',
        'dimension42': '[[product-skufilho]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo         | Descrição            |
| :-------------- | :-------------- | :------------------- |
| `[[cd2-tipodecartão]]` |  &#039;credito&#039;, &#039;riachuelo&#039; | Tipo do cartão utilizado na compra |
| `[[cd3-bandeiracartão]]` | &quot;visa&quot;, &quot;mastercard&quot; | Bandeira do cartão utilizada na compra |
| `[[cd13-product-idadedoproduto]]` |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| `[[cd22-hit-metodopagamento]]` |  &#039;cartao-de-credito&#039;, &#039;boleto&#039;  | Nome do tipo do pagamento. |
| `[[cd23-hit-statuspagamento]]` |  &#039;aprovado&#039;   | Nome do status do pagamento.  |
| `[[product-ordemdeinserção]]` | 1,2,3 .. | Ordem de inserção ao carrinho |
| `[[product-tamanhodoproduto]]` |  &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| `[[product-cordoproduto]]` | &#039;vermelho&#039; | Cor do produto  |
| `[[product-padronagemdoproduto]]` | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| `[[cd28-hit-frete]]` |  &#039;normal, expresso, entrega-agendada&#039;  | Nome do tipo de entrega.  |
| `[[product-subcategoria]]` |  &#039;310090&#039; | Código da categoria GM  |
| `[[product-subcategoria]]` |  &#039;310090&#039; | Código da categoria GM  |
| `[[product-preçooriginal]]` | &#039;12&#039;, &#039;produto-temporariamente-indisponivel&#039;,&#039;indisponivel&#039; etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| `[[product-lifestyle]]` | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| `[[product-gender]]` | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| `[[product-exclusivoecommerce+marketplace]]` | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| `[[product-skufilho]]` |  &#039;2552&#039; | ID filho do produto |
| `[[cd46-hit-parcelamento]]` |  &#039;2:sem-juros&#039;, &#039;6:com-juros&#039; e etc | Quantidade de parcelas e se foi com ou sem juros |
| `[[descontofrete]]` |  &#039;10&#039; | Valor do desconto de frete |
| `[[descontofuncionário]]` |  &#039;22&#039; | Valor do desconto para funcionário |
| `[[descontopromoção]]` |  &#039;15&#039; | Valor do desconto promocional - cupom, primeira compra, etc |
| `[[totaldedesconto]]` |  &#039;47&#039; | Soma de todos os descontos |
| `[[cartãopresente]]` | 100.00&#039;, &#039;50.00&#039; e etc | Deve retornar o valor pago por cartão presente utilizado. |
| `[[valetroca]]` | 40.00&#039;, &#039;23.00&#039;, &#039;200.00&#039; | Deve retornar o valor pago pelo vale troca utilizado. |
| `[[pagamentocomum]]` | 140.00&#039;, &#039;67.00&#039;, &#039;100.00&#039; | Deve retornar o valor pago em formas de  pagamentos comuns como, por exemplo, boleto, cartão de credito e etc |
| `[[id-transacao]]` | &quot;000011652&quot; | ID único da transação |
| `[[valor-total-transacao]]` | &quot;139.99&quot; | Valor total da transação incluindo frete e taxas |
| `[[frete-transacao]]` | &quot;0.00&quot; | Valor do frete da transação |
| `[[coupon-transacao]]` | &quot;cupom-2018&quot; | Cupom de desconto utilizado na transação - promoção nivel pedido |
| `[[taxa-transacao]]` | &quot;0.00&quot; | Valor de taxas da transação |
| `[[nome-produto]]` | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| `[[id-produto]]` | &quot;13239635&quot; | SKU do produto - pai |
| `[[preco-produto]]` | &quot;139.99&quot; | Preço do produto |
| `[[marca-produto]]` | &quot;pool-original&quot; | Marca do produto |
| `[[categoria-produto]]` | &quot;masculino&quot; | Departamento do produto |
| `[[quantidade-produto]]` | &quot;1&quot; | Quantidade do produto |

<br />

---

## Considerações Finais


> Documentações Oficiais do Google: <br />
> [Google Analytics - Avaliação de comércio eletrônico ](https://developers.google.com/analytics/devguides/collection/analyticsjs/ecommerce?hl=pt-br) <br />
> [Google Tag Manager - Guia do desenvolvedor ](https://developers.google.com/tag-manager/enhanced-ecommerce)

<script>
  document.addEventListener('DOMContentLoaded', function(event) {
    document.querySelectorAll('h1 a')[0].style.display = 'none';
  });
</script>
