![Riachuelo](https://www.riachuelo.com.br/static/version1623357894/frontend/Corra/webjump/pt_BR/images/logo.svg)

> Área - Data Analytics<br />
> Documento de Especificação Técnica

<br />

## Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Dimensões Globais](#dimens&#245;es-globais)
- [Geral](#geral)
- [Home](#home)
- [Modal Importante](#modal-importante)
- [Customização](#customiza&#231;&#227;o)
- [Customização - Enviar minha foto](#customiza&#231;&#227;o-enviar-minha-foto)
- [Acesso - Login](#acesso-login)
- [Acesso - Redefinir minha senha](#acesso-redefinir-minha-senha)
- [Cadastro](#cadastro)
- [Checkout](#checkout)
- [Pedido finalizado com Sucesso](#pedido-finalizado-com-sucesso)
- [NPS](#nps)
- [Enhanced Ecommerce](#enhanced-ecommerce)

<br />

## Implementação da Camada de dados - Projeto Rchlo+
Última atualização: 23/07/2021 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [riachuelomais.com.br](riachuelomais.com.br).

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-N5SF4CX');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-N5SF4CX" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
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
    'dimension2': '[[gaclientid]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[userid]] |  &#039;01234&#039; | ID único de usuário definido após o cadastro |
| [[gaclientid]] |  &#039;XXXXXXXXXX:XXXX:XX&#039; | ID aleatório do Google Analytics |
| [[etapacheckout]] |  &#039;identificacao&#039;, &#039;entrega&#039;, &#039;cupons&#039; ou &#039;pagamento&#039; | Retorna a etapa do checkout que foi carregada |

---


### Geral


**No clique dos links do Footer.**<br />

- **Onde:** No footer da pagina

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:footer',
    'eventAction': 'clique:link',
    'eventLabel': '[[nome-link]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;condicoes-gerais-e-politica-de-troca&#039;, &#039;portal-de-privacidade&#039;, &#039;atendimento&#039; e etc. | Deve retornar o nome do link clicado. |

<br />


**No clique no botão &quot;Inicio&quot;**<br />

- **Onde:** Nas telas de erros

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:tela-de-erro',
    'eventAction': 'clique:botao:inicio',
    'eventLabel': '[[nome-erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-erro]] | &#039;eita-alguma-coisa-deu-errado&#039;, &#039;pagina-nao-encontrada&#039; e etc | Deve retornar o nome do erro que aconteceu. |

<br />


### Home

**No clique do botões &quot;Acompanhar meu pedido&quot; e &quot;Criar minha estampa&quot;**<br />

- **Onde:** Na Home do site.

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:home',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;acompanhar-meu-pedido&#039; ou &#039;criar-minha-estampa&#039; | Deve retornar o nome do botão clicado. |

<br />


### Modal Importante

**No clique do botões &quot;Cancelar&quot; ou &quot;Ok, Entendi&quot;**<br />

- **Onde:** No modal &quot;Importante&quot;, após ter clicado no botão &quot;Criar minha estampa&quot;


```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:home:modal:importante',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;cancelar&#039; ou &#039;ok-entendi&#039; | Deve retornar o nome do botão clicado. |

<br />


### Customização

**No clique na peça selecionada.**<br />

- **Onde:** Na página de customização do site.


```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao',
    'eventAction': 'clique:peca',
    'eventLabel': '[[tipo-de-peca]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-de-peca]] | &#039;camiseta&#039;, &#039;body&#039;, &#039;jaqueta&#039; e etc. | Deve retornar o tipo da peça clicada |

<br />


**No clique no modelo selecionado.**<br />

- **Onde:** Na página de customização do site.


```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao',
    'eventAction': 'clique:modelo-peca',
    'eventLabel': '[[modelo-clicado]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[modelo-clicado]] | &#039;camiseta-masculina&#039;, &#039;camiseta-feminina&#039;, &#039;camiseta-juvenil&#039; e etc. | Deve retornar o modelo clicado. |

<br />


**No clique do botão &quot;Continuar&quot; do modal &quot;Bem Vindo&quot;**<br />

- **Onde:** Na página de customização do site, modal Bem Vindo.


```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao:modal:bem-vindo',
    'eventAction': 'clique:botao',
    'eventLabel': 'continuar'
  });
</script>
```

<br />


**Na interação com os campos Texto, Enviar minha foto, Customizar e Nossas estampas**<br />

- **Onde:** Na página de customização do site, seção &quot;Criar minha estampa&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao',
    'eventAction': 'interacao:campo',
    'eventLabel': '[[nome-campo]]',
    'dimension5': '[[modelodapeça]]',
    'dimension25': '[[product-tamanhodoproduto]]',
    'dimension27': '[[product-padronagemdoproduto]]',
    'dimension30': '[[product-subcategoria]]',
    'dimension32': '[[product-preçooriginal]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;texto&#039;, &#039;enviar-minha-foto&#039;, &#039;customizar&#039; ou &#039;nossas-estampas&#039; | Deve retornar o nome do campo. |
| [[modelodapeça]] | camiseta-masculina&#039;, &#039;camiseta-feminina&#039;, &#039;camiseta-juvenil&#039; e etc. | Retorna o modelo da peça selecionada |
| [[product-tamanhodoproduto]] |  &#039;p&#039;, &#039;m&#039;, &#039;8-12&#039;, &#039;42&#039; | Tamanho do produto |
| [[product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] |  &#039;12&#039; | Preço do produto (dê) |


<br />


**No clique do botão &quot;Apagar tudo&quot;**<br />

- **Onde:** Na página de customização do site, seção &quot;Criar minha estampa&quot;

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao',
    'eventAction': 'clique:botao',
    'eventLabel': '[[item-clicado]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[item-clicado]] | 'centralizar', 'apagar-tudo', 'girar' e etc | Deve retornar o nome do item clicado. |

<br />


**Nos cliques dos botões &quot;Alterar&quot; ou &quot;Continuar&quot;**<br />

- **Onde:** Na página de Confirmação da estampa.

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao:confirmacao-estampa',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;alterar&#039; ou &#039;continuar&#039; | Deve retornar o nome do botão clicado. |

<br />


**No callback de erro dos termos**<br />

- **Onde:** Na página de &quot;Aceite os termos e condições&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao',
    'eventAction': 'callback:termos',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[erro]] | &#039;aceite-os-termos-para-prosseguir&#039; e etc. | Deve retornar o callback de erro dos termos. |

<br />


**Nos cliques dos botões &quot;Voltar&quot; ou &quot;Aceitar e Prosseguir&quot;**<br />

- **Onde:** Na página de &quot;Aceite os termos e condições&quot;

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao:aceite-os-termos',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039; ou &#039;aceitar-e-prosseguir&#039; | Deve retornar o nome do botão clicado. |

<br />


### Customização - Enviar minha foto

**No clique do botão &quot;Fazer upload do Computador&quot;**<br />

- **Onde:** Na página de customização do site, seção &quot;Enviar Minha Foto&quot;

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao:enviar-minha-foto',
    'eventAction': 'clique:botao',
    'eventLabel': 'fazer-upload-pelo-computador'
  });
</script>
```

<br />


**No callback de Smartphone conectado**<br />

- **Onde:** Na página de customização do site, seção &quot;Enviar Minha Foto&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao:enviar-minha-foto',
    'eventAction': 'callback:qr-code',
    'eventLabel': '[[status-conexao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status-conexao]] | &#039;seu-smartphone-esta-conectado&#039; ou &#039;smaptphone-nao-encontrado&#039; | Deve retornar o status da conexão com o telefone. |

<br />


**No callback de envio de foto**<br />

- **Onde:** Na página de customização do site, seção &quot;Enviar Minha Foto&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao:enviar-minha-foto',
    'eventAction': 'callback:envio-de-foto',
    'eventLabel': '[[sucesso-ou-erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso-ou-erro]] | &#039;foto-recebida-com-sucesso&#039;, &#039;atencao-o-arquivo-e-muito-grande&#039;, &#039;formato-de-arquivo-invalido&#039; e etc. | Deve retornar o callback da tentativa de fazer o envio de foto. |

<br />


**No clique nos botões &quot;Enviar uma nova foto&quot; ou &quot;Tentar Novamente&quot;**<br />

- **Onde:** Na página de customização do site, seção &quot;Enviar Minha Foto&quot;

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:customizacao:enviar-minha-foto',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;enviar-uma-nova-foto&#039; ou &#039;tentar-novamente&#039; | Deve retornar o nome do botão clicado. |

<br />


### Acesso - Login

**Na interação com as opções de linguagem da página**<br />

- **Onde:** Na página de &quot;Login&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:acesso',
    'eventAction': 'interacao:opcoes-de-linguagem',
    'eventLabel': '[[linguagem-escolhida]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[linguagem-escolhida]] | 'portugues-brasil' ou 'english' | Deve retornar o nome da linguagem escolhida. |

<br />

**Na interação com os campos &quot;CPF&quot; e &quot;Senha&quot;**<br />

- **Onde:** Na página de &quot;Login&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:acesso',
    'eventAction': 'preencheu-campo',
    'eventLabel': '[[nome-campo]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;cpf&#039; ou &#039;senha&#039; | Deve retornar o nome do campo preenchido. |

<br />


**No clique em todos os elementos da página**<br />

- **Onde:** Na página de "Login"

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:acesso',
    'eventAction': 'clique:[[elemento]]',
    'eventLabel': '[[nome-elemento]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[elemento]] | 'botao', 'link', 'checkbox' ou 'password-visibility' | Deve retornar o nome do elemento.|
| [[nome-elemento]] | 'acessar', 'quero-criar-minha-conta', 'esquece-minha-senha', 'mostrar-senha', 'esconder-senha', 'mantenha-me-conectado' e etc. | Deve retornar o nome do elemento clicado. |

<br />


**No callback dos campos &quot;CPF&quot; e &quot;Senha&quot;**<br />

- **Onde:** Na página de &quot;Login&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:acesso',
    'eventAction': 'callback:[[nome-campo]]',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;cpf&#039; ou &#039;senha&#039; | Deve retornar o nome do campo que iremos receber o callback.  |
| [[erro]] | &#039;insira-sua-senha-para-acessar&#039;, &#039;senha-invalida&#039; e etc. | Deve retornar o callback de erro dos campos CPF e Senha. |

<br />


### Acesso - Redefinir minha senha

**Na interação com o campo &quot;CPF&quot;.**<br />

- **Onde:** Na página de &quot;Redefinir minha senha&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:acesso:redefir-senha',
    'eventAction': 'preencheu-campo',
    'eventLabel': '[[email-ou-cpf]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[email-ou-cpf]] | 'email' ou 'cpf' | Deve retornar o nome do elemento clicado.  |


<br />


**No clique do botão &quot;Redefir Senha&quot;**<br />

- **Onde:** Na página de &quot;Redefinir minha senha&quot;

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:acesso:redefir-senha',
    'eventAction': 'clique:[[botao-ou-link]]',
    'eventLabel': '[[item-clicado]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-ou-link]] | 'botao' ou 'link' | Deve retornar o elemento clicado.  |
| [[item-clicado]] | 'continuar' ou 'entrar' | Deve retornar do elemento clicado.  |


<br />


**No callback do campo &quot;CPF&quot;**<br />

- **Onde:** Na página de &quot;Redefinir minha senha&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:acesso:redefir-senha',
    'eventAction': 'callback:campo-cpf-ou-email',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[erro]] | &#039;cpf-nao-encontrado&#039;, &#039;cpf-invalido&#039; e etc | Deve retornar o callback de erro do campo CPF. |

<br />


**No callback de e-mail enviado.**<br />

- **Onde:** Na página de &quot;Redefinir minha senha&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:acesso:redefir-senha',
    'eventAction': 'callback:email-enviado',
    'eventLabel': '[[sucesso-ou-erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso-ou-erro]] | &#039;sucesso&#039;, &#039;erro:email-incompleto&#039; e etc | Deve retornar o callback de sucesso ou erro do e-mail enviado. |

<br />


### Cadastro

**Na interação com as opções de linguagem da página**<br />

- **Onde:** Nas páginas dos formulários de Cadastro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:[[etapa-cadastro]]',
    'eventAction': 'interacao:opcoes-de-lingaguem',
    'eventLabel': '[[linguagem-escolhida]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[etapa-cadastro]] | 'cadastro', 'criar-senha', 'verifique-seu-email' ou 'acao-realizada-com-sucesso' | Deve retornar a etapa do cadastro que usuário está. |
| [[linguagem-escolhida]] | 'portugues-brasil' ou 'english' | Deve retornar o nome da linguagem escolhida. |

<br />

**No preenchimento dos campos**<br />

- **Onde:** Na página de &quot;Cadastro&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:cadastro',
    'eventAction': 'preencheu-campo',
    'eventLabel': '[[nome-campo]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome-completo&#039;, &#039;email&#039;, &#039;cpf&#039; e etc. | Deve retornar o nome do campo preenchido. |

<br />


**No clique no botões, links e elementos da página**<br />

- **Onde:** Na página de &quot;Cadastro&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:cadastro',
    'eventAction': 'clique:[[botao-ou-link]]',
    'eventLabel': '[[nome-item-clicado]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-ou-link]] | 'botao', 'link', 'password-visibility' e etc. | Deve retornar o elemento clicado. |
| [[nome-item-clicado]] | 'continuar', 'entrar', 'condicoes' ou 'privacidade' | Deve retornar do elemento clicado. |

<br />


**No callback dos campos obrigatórios**<br />

- **Onde:** Na página de &quot;Cadastro&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:cadastro',
    'eventAction': 'callback:[[nome-campo]]',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome-completo&#039;, &#039;email&#039;, &#039;cpf&#039;, &#039;endereco&#039;, &#039;bairro&#039; e etc. | Deve retornar o nome do campo que está retornando o callback. |
| [[erro]] | &#039;dado-incorreto&#039;, &#039;cpf-invalido&#039;, &#039;bairro-nao-existente&#039; e etc. | Deve retornar o callback de erro do campo. |

<br />

**No preenchimento dos campos**<br />

- **Onde:** Na página de "Criar senha"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:cadastro:criar-senha',
    'eventAction': 'callback:[[nome-campo]]',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | 'senha', 'confirme-sua-nova-senha' | Deve retornar o nome do campo que está retornando o callback. |
| [[erro]] | 'senha-invalida', 'confirme-sua-senha' e etc | Deve retornar o callback de erro do campo. |

<br />

**No clique no botões, links e elementos da página**<br />

- **Onde:** Na página de "Criar senha"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:cadastro:criar-senha',
    'eventAction': 'clique:[[elemento]]',
    'eventLabel': '[[nome-elemento]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[elemento]] | 'botao', 'link', 'checkbox' ou 'password-visibility' | Deve retornar o nome do elemento.  |
| [[nome-elemento]] | 'enviar', 'aceito-os-termos', 'termos-e-condicoes', 'declaracao-de-privacidade', 'mostrar-senha', 'esconder-senha' e etc. | Deve retornar o nome do elemento clicado. |

<br />

**No clique no link "Reenviar", para reenviar o e-mail**<br />

- **Onde:** Na página de "Verifique seu e-mail"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:cadastro:verifique-seu-email',
    'eventAction': 'clique:link',
    'eventLabel': 'reenviar-email'
  });
</script>
```

<br />

**No clique do botão "Acessar"**<br />

- **Onde:** Após verificar o e-mail de cadastro, na página "Ação realizada com sucesso"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:cadastro:acao-realizada-com-sucesso',
    'eventAction': 'clique:botao',
    'eventLabel': 'acessar'
  });
</script>
```

<br />

**No carregamento da página**<br />

- **Onde:** Após clicar no botão "Acessar" confirmando que o e-mail foi verificado.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:cadastro:acao-realizada-com-sucesso',
    'eventAction': 'callback',
    'eventLabel': '[[sucesso-ou-erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso-ou-erro]] | 'sucesso:seu-endereco-de-email-foi-confirmado', 'erro:email-incompleto' e etc | Deve retornar o callback de sucesso ou erro do e-mail enviado/conclusão do cadastro. |

<br />

### Checkout

**No clique para escolher o &quot;Endereço de Retirada&quot;**<br />

- **Onde:** Na página de &quot;Sua Compra&quot;, seção &quot;Endereço para Retirada&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:checkout',
    'eventAction': 'clique:endereco-de-retirada',
    'eventLabel': '[[nome-loja]]',
    'dimension4': '[[etapacheckout]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-loja]] | &#039;morumbi-shopping&#039; e etc. | Deve retornar o nome da loja escolhida para retirada. |
| [[etapacheckout]] |  &#039;identificacao&#039;, &#039;entrega&#039;, &#039;cupons&#039; ou &#039;pagamento&#039; | Retorna a etapa do checkout que foi carregada |

<br />

**No clique nos botões de todas as etapas do checkout**<br />

- **Onde:** Na página de &quot;Sua Compra&quot;, seção &quot;Endereço para Retirada&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:checkout',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]',
    'dimension4': '[[etapacheckout]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] |'voltar', 'continuar', 'consultar', 'aplicar', 'finalizar-compra', 'salvar' e etc | Deve retornar o nome do botão clicado. |
| [[etapacheckout]] |  &#039;identificacao&#039;, &#039;entrega&#039;, &#039;cupons&#039; ou &#039;pagamento&#039; | Retorna a etapa do checkout que foi carregada |

<br />

**No callback de sucesso ou erro ao inserir um Cupom ou Vale Presente**<br />

- **Onde:** Na página de &quot;Sua Compra&quot;, seção &quot;Cupons, Vales ou Cartão Presentes&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:checkout',
    'eventAction': 'callback:[[cupom-ou-vale-presente]]',
    'eventLabel': '[[sucesso-ou-erro]]',
    'dimension4': '[[etapacheckout]]',
    'dimension6': '[[nome-cupom-ou-vale-presente]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cupom-ou-vale-presente]] | &#039;cupom&#039; ou &#039;vale-presente&#039;. | Deve retornar qual item esta retornando o callback. |
| [[sucesso-ou-erro]] | &#039;cupom-invalido&#039;, &#039;cupom-aplicado-com-sucesso&#039;, &#039;esse-cartao-nao-possui-saldo&#039; e etc. | Deve retornar o callback de sucesso ou erro do campo. |
| [[etapacheckout]] |  &#039;identificacao&#039;, &#039;entrega&#039;, &#039;cupons&#039; ou &#039;pagamento&#039; | Retorna a etapa do checkout que foi carregada |
| [[nome-cupom-ou-vale-presente]] | 'bem-vindo', 'cupom-promocional' e etc | Deve retornar o nome do cupom inserido |

<br />


**No clique no ícone &quot;X&quot; para remover um cupom ou Vale Presente.**<br />

- **Onde:** Na página de &quot;Sua Compra&quot;, seção &quot;Cupons, Vales ou Cartão Presentes&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:checkout',
    'eventAction': 'clique:icone:remover',
    'eventLabel': '[[cupom-ou-vale-presente]]',
    'dimension4': '[[etapacheckout]]',
    'dimension6': '[[nome-cupom-ou-vale-presente]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cupom-ou-vale-presente]] | &#039;cupom&#039; ou &#039;vale-presente&#039;. | Deve retornar qual item foi removido. |
| [[etapacheckout]] |  &#039;identificacao&#039;, &#039;entrega&#039;, &#039;cupons&#039; ou &#039;pagamento&#039; | Retorna a etapa do checkout que foi carregada |
| [[nome-cupom-ou-vale-presente]] | 'bem-vindo', 'cupom-promocional' e etc | Deve retornar o nome do cupom inserido |

<br />

**No callback de erro, caso o usuário tente passa sem efetuar o pagamento.**<br />

- **Onde:** Na página de &quot;Sua Compra&quot;, seção &quot;Pagamento&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:checkout',
    'eventAction': 'callback:pagamento',
    'eventLabel': '[[erro]]',
    'dimension4': '[[etapacheckout]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[erro]] | &#039;por-favor-selecione-uma-opcao-de-pagamento&#039; | Deve retornar o callback de erro. |
| [[etapacheckout]] |  &#039;identificacao&#039;, &#039;entrega&#039;, &#039;cupons&#039; ou &#039;pagamento&#039; | Retorna a etapa do checkout que foi carregada |

<br />


**No clique na opção de &quot;Alterar&quot;**<br />

- **Onde:** Na página de &quot;Sua Compra&quot;, seção &quot;Resumo do Pedido&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:checkout',
    'eventAction': 'clique:opcao-alterar',
    'eventLabel': '[[opcao-escolhida]]',
    'dimension4': '[[etapacheckout]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-escolhida]] | 'entrega' ou 'pagamento' | Deve retornar o nome da opção clicada. |
| [[etapacheckout]] |  &#039;identificacao&#039;, &#039;entrega&#039;, &#039;cupons&#039; ou &#039;pagamento&#039; | Retorna a etapa do checkout que foi carregada |

<br />


**No callback de erro, quando não conseguir finalizar o pedido.**<br />

- **Onde:** Na página de &quot;Sua Compra&quot;, seção &quot;Resumo do Pedido&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:checkout',
    'eventAction': 'callback:resumo-do-pedido',
    'eventLabel': '[[erro]]',
    'dimension4': '[[etapacheckout]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[erro]] | &#039;nao-foi-possivel-concluir-seu-pedido&#039; e etc. | Deve retornar o callback de erro. |
| [[etapacheckout]] |  &#039;identificacao&#039;, &#039;entrega&#039;, &#039;cupons&#039; ou &#039;pagamento&#039; | Retorna a etapa do checkout que foi carregada |

<br />


### Pedido finalizado com Sucesso

**No clique nos botões &quot;Ver Boleto&quot; e &quot;Ver Pedido&quot;**<br />

- **Onde:** Na página de &quot;Pedido finalizado com Sucesso&quot;

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:pedido-finalizado-com-sucesso',
    'eventAction': 'clique:botao',
    'eventLabel': '[[erro]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;ver-boleto&#039; ou &#039;ver-pedido&#039; | Deve retornar o nome do botão clicado. |

<br />


### NPS

**No clique nas opções de avaliação**<br />

- **Onde:** Na página &quot;Avalie&quot;, seção &quot;O que achou da nossa experiência?&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:nps',
    'eventAction': 'clique:opcao-de-avaliacao',
    'eventLabel': '[[opcao-escolhida]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-escolhida]] | &#039;nao-gostei&#039;, &#039;gostei&#039; e etc | Deve retornar o nome da opção clicada. |

<br />


**Na tentativa de callback para enviar a avaliação de Nps**<br />

- **Onde:** Na página &quot;Avalie&quot;, seção &quot;O que achou da nossa experiência?&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlo-mais:nps',
    'eventAction': 'callback:avaliacao-experiencia',
    'eventLabel': '[[opcao]]:[[sugestao]]:[[status]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao]] | &#039;nao-gostei&#039;, &#039;gostei&#039; e etc | Deve retornar o nome da opção clicada. |
| [[sugestao]] | &#039;site-lento&#039;, &#039;default&#039; e etc | Deve retornar a sugestão feita pelo usuário. |
| [[status]] | &#039;sugestao-enviada&#039;, &#039;erro:nao-foi-possivel-enviar-sugestao&#039; e etc | Deve retornar a mensagem de sucesso ou tipo de erro. |

<br />

### Enhanced Ecommerce

**No carregamento da página de Detalhe do Produto**<br />

- **Onde:** Na página de customização do site.
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productDetail',
  'eventCategory': 'rchlo-mais:enhanced-ecommerce',
  'eventAction': 'productDetail',
  'noInteraction': '1',
    'ecommerce': {
      'detail': {
        'products': [{
          'name': '[[nome-produto]]',
          'id': '[[id-produto]]',
          'price': '[[preco-produto]]',
          'brand': '[[marca-produto]]',
          'category': '[[categoria-produto]]',
          'variant': '[[variacao-produto]]',
          'dimension5': '[[modelodapeça]]',
          'dimension25': '[[product-tamanhodoproduto]]',
          'dimension27': '[[product-padronagemdoproduto]]',
          'dimension30': '[[product-subcategoria]]',
          'dimension32': '[[product-preçooriginal]]',
        }]
      }
    }
 });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |
| [[modelodapeça]] | camiseta-masculina&#039;, &#039;camiseta-feminina&#039;, &#039;camiseta-juvenil&#039; e etc. | Retorna o modelo da peça selecionada |
| [[product-tamanhodoproduto]] |  &#039;p&#039;, &#039;m&#039;, &#039;8-12&#039;, &#039;42&#039; | Tamanho do produto |
| [[product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] |  &#039;12&#039; | Preço do produto (dê) |

<br />


**No clique do botão &quot;Comprar&quot;**<br />

- **Onde:** Na página de customização do site, seção &quot;Criar minha estampa&quot;
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'addToCart',
  'eventCategory':'rchlo-mais:enhanced-ecommerce',
  'eventAction': 'addToCart',
  'ecommerce': {
    'add': {
      'products': [{
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'dimension5': '[[modelodapeça]]',
        'dimension25': '[[product-tamanhodoproduto]]',
        'dimension27': '[[product-padronagemdoproduto]]',
        'dimension30': '[[product-subcategoria]]',
        'dimension32': '[[product-preçooriginal]]',
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |
| [[modelodapeça]] | camiseta-masculina&#039;, &#039;camiseta-feminina&#039;, &#039;camiseta-juvenil&#039; e etc. | Retorna o modelo da peça selecionada |
| [[product-tamanhodoproduto]] |  &#039;p&#039;, &#039;m&#039;, &#039;8-12&#039;, &#039;42&#039; | Tamanho do produto |
| [[product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] |  &#039;12&#039; | Preço do produto (dê) |

<br />


**No carregamento da página de &quot;Pedido Finalizado com Sucesso&quot;**<br />

- **Onde:** Na página de &quot;Pedido finalizado com Sucesso&quot;
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'purchase',
  'eventCategory': 'rchlo-mais:enhanced-ecommerce',
  'eventAction': 'purchase',
  'noInteraction': '1',
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
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]',
        'dimension5': '[[modelodapeça]]',
        'dimension25': '[[product-tamanhodoproduto]]',
        'dimension27': '[[product-padronagemdoproduto]]',
        'dimension30': '[[product-subcategoria]]',
        'dimension32': '[[product-preçooriginal]]',
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[id-transacao]] | &quot;000011652&quot; | ID único da transação |
| [[valor-total-transacao]] | &quot;139.99&quot; | Valor total da transação incluindo frete e taxas |
| [[frete-transacao]] | &quot;0.00&quot; | Valor do frete da transação |
| [[coupon-transacao]] | &quot;cupom-2018&quot; | Cupom de desconto utilizado na transação - promoção nivel pedido |
| [[taxa-transacao]] | &quot;0.00&quot; | Valor de taxas da transação |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |
| [[erro]] | &#039;nao-foi-possivel-concluir-seu-pedido&#039; e etc. | Deve retornar o callback de erro. |
| [[modelodapeça]] | camiseta-masculina&#039;, &#039;camiseta-feminina&#039;, &#039;camiseta-juvenil&#039; e etc. | Retorna o modelo da peça selecionada |
| [[product-tamanhodoproduto]] |  &#039;p&#039;, &#039;m&#039;, &#039;8-12&#039;, &#039;42&#039; | Tamanho do produto |
| [[product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] |  &#039;12&#039; | Preço do produto (dê) |

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
