![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Dimensões Globais](#dimens&#245;es-globais)
- [Eventos Gerais](#eventos-gerais)
- [Home](#home)
- [Login e Esqueci Senha](#login-e-esqueci-senha)
- [Logoff](#logoff)
- [Categorias](#categorias)
- [Checkout](#checkout)
- [PDP](#pdp)
- [Enhanced Ecommerce](#enhanced-ecommerce)

<br />

## Implementação da Camada de dados - Riachuelo - e-Store
Última atualização: 15/12/2020 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

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
    'dimension5': '[[cd5-user-funcionarioid]]',
    'dimension6': '[[cd6-hit-loja]]',
    'dimension7': '[[cd7-hit-dispositivo]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd1-user-userid]] |  &#039;01234&#039; | ID único do usuário definido após cadastro (MagentoID) |
| [[cd4-hit-loginstatus(cliente)]] |  &#039;Logged in&#039;, &#039;Logged out&#039; | Status do login do usuário (CLIENTE) |
| [[cd5-user-funcionarioid]] |  &#039;10203040&#039; | ID único do funcionário |
| [[cd6-hit-loja]] |  &#039;ABC&#039; | Nome da Loja preenchido |
| [[cd7-hit-dispositivo]] |  &#039;Dispositivo-1&#039; | Nome do dispositivo usado |

---


### Eventos Gerais


**No clique nos links do header**<br />

- **Onde:** Em todas as páginas que estiverem disponíveis
    - **Titulo ou nome do botão/link:** Título: &quot;sacola&quot; (ícone), &quot;Riachuelo&quot; (logo), &quot;login&quot;, &quot;sair&quot;, &quot;início&quot; e etc
    
```html
<div
   data-gtm-event-category='estore:geral'
   data-gtm-event-action='clique:header'
   data-gtm-event-label='[[link-clicado]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[link-clicado]] | &#039;riachuelo&#039;(logo), &#039;login&#039;, &#039;sacola&#039;, &#039;sair&#039;, &#039;inicio&#039; e etc | Deve retornar o nome do link clicado. |

<br />


**Após interagir com o campo de busca e clicar na lupa (ícone para realizar a busca)**<br />

- **Onde:** Em todas as páginas que estiverem disponíveis
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:geral',
    'eventAction': 'interacao:busca',
    'eventLabel': '[[termo-buscado]]:[[erro]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[termo-buscado]] | &#039;vestidos&#039;, &#039;calca-masculina&#039; e etc | Deve retornar o termo buscado pelo usuário. |
| [[erro]] | &#039;nao-encontramos-nenhum-resultado&#039;, &#039;ops-ocorreu-um-erro&#039;, &#039;pagina-nao-encontrada&#039; e etc | Deve retornar o erro apresentado após fazer a busca e não encontrar nenhum resultado. |

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
| [[nome-erro]] | &#039;ocorreu-erro-serviço,&#039; &#039;404-not-found&#039; | Deve retornar o nome do erro ocorrido na página genérica. |

<br />


### Home

**Após interagir com os campos de matrícula e de cpf para fazer o login**<br />

- **Onde:** Na página home do site
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:lightbox-identificacao',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;matricula&#039;, &#039;cpf&#039; e etc | Deve retornar o nome do campo preenchido pelo usuário. |

<br />


**No clique do botão &quot;Continuar&quot; para validar os campos preenchidos de CPF e Senha**<br />

- **Onde:** Na página home do site
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:lightbox-identificacao',
    'eventAction': 'tentativa:login',
    'eventLabel': 'continuar'
  });
</script>
```




<br />


**No callback, após clicar no botão &quot;Continuar&quot; para validar os campos preenchidos CPF e Senha com sucesso**<br />

- **Onde:** Na página home do site
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:lightbox-identificacao',
    'eventAction': 'callback:login',
    'eventLabel': '[[sucesso ou tipo-erro]]',
    'dimension5': '[[cd5-user-funcionarioid]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;, &#039;erro:matricula-invalida&#039; e etc | eve retornar o label do botão &#039;sucesso&#039; ou o status de erro retorno. |
| [[cd5-user-funcionarioid]] |  &#039;10203040&#039; | ID único do funcionário |

<br />


**Após interagir com os campos de &quot;Selecione a loja&quot; e de &quot;Selecione o Dispositivo&quot;**<br />

- **Onde:** Na página home do site
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:lightbox-identificacao',
    'eventAction': 'interacao:campo',
    'eventLabel': '[[campo]]:[[opcao-selecionada]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[campo]] | &#039;selecione-a-loja&#039; ou &#039;selecione-o-dispositivo&#039; | Deve retornar o nome do campo . |
| [[opcao-selecionada]] | &#039;loja-1&#039;, &#039;loja-abc&#039;, &#039;computador&#039;, &#039;totem&#039;, &#039;espelho&#039; e etc | Deve retornar a opção selecionada pelo usuário. |

<br />


**No clique do botão &quot;Continuar&quot;  para validar os itens selecionados de Loja e Dispositivo**<br />

- **Onde:** Na página home do site
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:lightbox-identificacao',
    'eventAction': 'tentativa:loja-dispositivo',
    'eventLabel': 'continuar'
  });
</script>
```




<br />


**No callback, após clicar no botão &quot;Continuar&quot; depois de ter selecionado a Loja e Dispositivo**<br />

- **Onde:** Na página home do site
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:lightbox-identificacao',
    'eventAction': 'callback:loja-dispositivo',
    'eventLabel': '[[sucesso ou tipo-erro]]',
    'dimension5': '[[cd5-user-funcionarioid]]',
    'dimension6': '[[cd6-hit-loja]]',
    'dimension7': '[[cd7-hit-dispositivo]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;, &#039;erro:selecione-a-loja-para-prosseguir&#039; e etc | eve retornar o label do botão &#039;sucesso&#039; ou o status de erro retorno. |
| [[cd5-user-funcionarioid]] |  &#039;10203040&#039; | ID único do funcionário |
| [[cd6-hit-loja]] |  &#039;ABC&#039; | Nome da Loja preenchido |
| [[cd7-hit-dispositivo]] |  &#039;Dispositivo-1&#039; | Nome do dispositivo usado |

<br />


### Login e Esqueci Senha

**Na interação com os campos para fazer login**<br />

- **Onde:** Na página de Login
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:login',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]]  | &#039;cpf&#039;, &#039;senha&#039; e etc | Deve retornar o nome do campo preenchido. |

<br />


**No clique do botão &quot;Continuar&quot; para validar os campos preenchidos e fazer o login**<br />

- **Onde:** Na página de Login
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:login',
    'eventAction': 'tentativa:login',
    'eventLabel': 'continuar'
  });
</script>
```




<br />


**No callback, após clicar no botão &quot;Continuar&quot; para validar os campos preenchidos e fazer o login**<br />

- **Onde:** Na página de login
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:login',
    'eventAction': 'callback:login',
    'eventLabel': '[[sucesso ou tipo-erro]]',
    'dimension1': '[[cd1-user-userid]]',
    'dimension5': '[[cd5-user-funcionarioid]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;,  &#039;erro:senha-invalida&#039; e etc | Deve retornar o label do botão &#039;sucesso&#039; ou o status de erro retorno. |
| [[cd1-user-userid]] |  &#039;01234&#039; | ID único do usuário definido após cadastro (MagentoID) |
| [[cd5-user-funcionarioid]] |  &#039;10203040&#039; | ID único do funcionário |

<br />


**No clique no botão de &quot;Esqueci minha senha&quot;**<br />

- **Onde:** Na página de login
    
```html
<div
   data-gtm-event-category='estore:login'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='esqueci-senha'
>Botão</div>
```



<br />


**Na interação com o campo de cpf para redefinir senha**<br />

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


**No clique do botão &quot;Redefinir minha senha&quot; para validar os campos preenchidos e redefinir a senha**<br />

- **Onde:** Na página para Redefinir Senha
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:redefinir-senha',
    'eventAction': 'tentativa:senha',
    'eventLabel': 'redefinir'
  });
</script>
```




<br />


**No callback, após clicar no botão &quot;Redefinir minha senha&quot; para validar os campos preenchidos e redefinir a senha**<br />

- **Onde:** Na página para Redefinir Senha
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:redefinir-senha',
    'eventAction': 'callback:senha',
    'eventLabel': '[[sucesso ou tipo-erro]]',
    'dimension1': '[[cd1-user-userid]]',
    'dimension5': '[[cd5-user-funcionarioid]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;, &#039;erro:insira-um-cpf-para-prosseguir&#039;, &#039;erro:cpf-invalido&#039; e etc | Deve retornar o label do botão &#039;sucesso&#039; ou o status de erro retorno. |
| [[cd1-user-userid]] |  &#039;01234&#039; | ID único do usuário definido após cadastro (MagentoID) |
| [[cd5-user-funcionarioid]] |  &#039;10203040&#039; | ID único do funcionário |

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
    'eventLabel': 'preencheu:[[nome-campo]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]]  | &#039;cpf&#039;, &#039;email&#039;, &#039;data-nascimento&#039;, telefone&#039;, &#039;senha&#039;, &#039;confirmar-senha&#039;, cep&#039; e etc | Deve retornar o nome do campo preenchido. |

<br />


**Na interação com os checkboxes para receber ofertas por e-mail e por sms**<br />

- **Onde:** No formulário de cadastro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:criar-conta',
    'eventAction': 'interacao:checkbox',
    'eventLabel': 'preencheu:[[nome-campo]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]]  | &#039;receber-ofertas-conteudos-por-email&#039;, ou &#039;receber-ofertas-conteudos-por-sms&#039; | Deve retornar o nome do campo preenchido. |

<br />


**No clique no botão de &quot;Continuar&quot;, &quot;termos e condições de uso&quot; e &quot;Política de Privacidade&quot;**<br />

- **Onde:** No formulário de cadastro
    
```html
<div
   data-gtm-event-category='estore:criar-conta'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;termos-condicoes-uso&#039;, &#039;politica-privacidade&#039; e etc | Deve retornar o nome do botão clicado. |

<br />


**No clique do botão &quot;Continuar&quot; para validar os campos preenchidos**<br />

- **Onde:** No formulário de cadastro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:criar-conta',
    'eventAction': 'tentativa:cadastro',
    'eventLabel': 'continuar'
  });
</script>
```




<br />


**No callback, após clicar no botão &quot;Continuar&quot; para validar os campos preenchidos**<br />

- **Onde:** No formulário de cadastro
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:criar-conta',
    'eventAction': 'callback:cadastro',
    'eventLabel': '[[sucesso ou tipo-erro]]',
    'dimension1': '[[cd1-user-userid]]',
    'dimension5': '[[cd5-user-funcionarioid]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;,&#039;erro:campo:bairro;nome;cpf&#039; e etc | Deve retornar o label do botão &#039;continuar&#039; ou o status de erro retorno. |
| [[cd1-user-userid]] |  &#039;01234&#039; | ID único do usuário definido após cadastro (MagentoID) |
| [[cd5-user-funcionarioid]] |  &#039;10203040&#039; | ID único do funcionário |

<br />


### Logoff

**No clique no botão de &quot;Desconectar Agora&quot;**<br />

- **Onde:** Na caixa de alerta de segurança
    
```html
<div
   data-gtm-event-category='estore:alerta-seguranca'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar-conectado&#039; e &#039;Desconectar Agora&#039; | Deve retornar o nome do botão clicado. |

<br />


**No carregamento da página de Logoff**<br />

- **Onde:** Na página de &quot;Você se desconectou com sucesso&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:logoff',
    'eventAction': 'desconectado:ate-logo',
    'eventLabel': 'deslogado-com-sucesso',
  });
</script>
```

<br />


### Categorias

**No clique para selecionar as categorias, na seção de &quot;Escolha por Categoria&quot;**<br />

- **Onde:** Na página de lista de produtos
    
```html
<div
   data-gtm-event-category='estore:clp'
   data-gtm-event-action='clique:[[categoria ou subcategoria]]'
   data-gtm-event-label='[[item-selecionada]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[item-selecionada]] | &#039;colecao-feminina&#039;,  &#039;plus-size&#039;, &#039;esporte-fitness&#039; e etc | Deve retornar a  categoria selecionada. |
| [[categoria ou subcategoria]] | &#039;categoria&#039;,  &#039;subcategoria&#039; | Deve retornar a se foi selecionado um elemento de categoria ou subcategoria . |

<br />


**No clique nos filtros**<br />

- **Onde:** Na página de lista de produtos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:clp',
    'eventAction': 'clique:filtro',
    'eventLabel': '[[filtro]]:[[filtro-selecionado]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[filtro]] | &#039;tamanho&#039;, &#039;marcas&#039;, &#039;padronagem&#039;, &#039;cor&#039;, &#039;relevancia&#039; e etc | Deve retornar o nome do filtro. |
| [[filtro-selecionado]] | &#039;marca-1&#039;, &#039;bordado&#039;, &#039;vermelho&#039;,   &#039;preco:maiores-primeiro&#039;, &#039;lancamentos&#039; e etc | Deve retornar o label do filtro selecionado. |

<br />


**No clique no &quot;ordenar por&quot;**<br />

- **Onde:** Na página de lista de produtos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:[[clp ou busca]]',
    'eventAction': 'clique:ordenar-por',
    'eventLabel': '[[ordem-selecionado]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[ordem-selecionado]] | &#039;por-preco&#039;, &#039;por-relevancia&#039; e etc | Deve retornar o label do ordem selecionado. |

<br />


### Checkout

**No clique nos botões de &quot;Continuar&quot;, &quot;+&quot;(Aumentar), &quot;-&quot; (Diminuir), Lixeira**<br />

- **Onde:** Na página de Sacola
    
```html
<div
   data-gtm-event-category='estore:sacola'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;aumentar-quantidade&#039;, &#039;diminuir-quantidade&#039;, &#039;excluir&#039; e etc | Deve retornar o nome do botão clicado. |

<br />


**No callback de erro de produtos indisponiveis**<br />

- **Onde:** No ligthbox de &quot;A quantidade desejada está indisponível no momento&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:sacola',
    'eventAction': 'callback:produtos',
    'eventLabel': 'item-indisponivel'
  });
</script>
```




<br />


**No clique no botão de &quot;Continuar&quot;**<br />

- **Onde:** No lightbox de &quot;A quantidade desejada está indisponível no momento&quot;
    
```html
<div
   data-gtm-event-category='estore:sacola'
   data-gtm-event-action='clique:botao:lightbox:quantidade-desejada-indisponivel'
   data-gtm-event-label='continuar'
>Botão</div>
```



<br />


**No clique nos botões de &quot;Remover&quot; ou &quot;Cancelar**<br />

- **Onde:** No lightbox de &quot;Tem certeza que quer remover esse item da sua sacola?&quot;
    
```html
<div
   data-gtm-event-category='estore:sacola'
   data-gtm-event-action='clique:botao:lightbox:remover-item-sacola'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;remover&#039;, &#039;cancelar&#039; e etc | Deve retornar o nome do botão clicado. |

<br />


**No callback após remover todos os produtos do carrinho**<br />

- **Onde:** No ligthbox de &quot;Sua sacola está vazia&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:sacola',
    'eventAction': 'callback:produtos',
    'eventLabel': 'sacola-vazia'
  });
</script>
```




<br />


**No clique no botão de &quot;Desconectar&quot;**<br />

- **Onde:** No lightbox de &quot;Sua sacola está vazia&quot;
    
```html
<div
   data-gtm-event-category='estore:sacola'
   data-gtm-event-action='clique:botao:lightbox:sacola-vazia'
   data-gtm-event-label='desconectar'
>Botão</div>
```



<br />


**No clique nas opções**<br />

- **Onde:** Na tela de &quot;Compra&quot;
    
```html
<div
   data-gtm-event-category='estore:compra'
   data-gtm-event-action='clique:opcao'
   data-gtm-event-label='[[opcao-selecionada]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] | &#039;entrega&#039;, &#039;cupons-vale-cartao-presente&#039;, &#039;pagamento&#039; e etc | Deve retornar a opção selecionada. |

<br />


**No clique no botão de &quot;Adicionar novo endereço&quot; ou &quot;Editar Endereço&quot;, na seção de Endereço**<br />

- **Onde:** Na página de Entrega, na seção de Endereço
    
```html
<div
   data-gtm-event-category='estore:checkout:endereco'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;editar-endereco-1&#039;, &#039;editar-endereco-2&#039;, &#039;adicionar-novo-endereco&#039; e etc | Deve retornar o nome do botão clicado. |

<br />


**Na interação com os campos para editar um endereço já cadastrado ou um novo endereço**<br />

- **Onde:** Nas páginas de &quot;Editar endereço&quot; ou &quot;Novo Endereço&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:[[nome-formulario]]',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]]  |  &#039;cep&#039;, &#039;nome&#039;, telefone&#039;  e etc | Deve retornar o nome do campo preenchido. |
| [[nome-formulario]] | &#039;editar-endereco&#039; ou &#039;novo-endereco&#039; | Deve retornar o nome do formulário que o botão está localizado. |

<br />


**No clique no botão de &quot;Salvar&quot;, para &quot;Editar um endereço já cadastrado&quot; ou &quot;Adicionar um novo endereço&quot;**<br />

- **Onde:** Nas páginas de &quot;Editar endereço&quot; ou &quot;Novo Endereço&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:[[nome-formulario]]',
    'eventAction': 'tentativa:adicionar',
    'eventLabel': 'salvar',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-formulario]] | &#039;editar-endereco&#039; ou &#039;novo-endereco&#039; | Deve retornar o nome do formulário que o botão está localizado. |

<br />


**No callback, após clicar no botão &quot;Salvar&quot; para validar os campos preenchidos**<br />

- **Onde:** Nas páginas de &quot;Editar endereço&quot; ou &quot;Novo Endereço&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:[[nome-formulario]]',
    'eventAction': 'callback:adicionar',
    'eventLabel': '[[sucesso ou tipo-erro]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-formulario]] | &#039;editar-endereco&#039; ou &#039;novo-endereco&#039; | Deve retornar o nome do formulário que o botão está localizado. |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;, &#039;erro:preencha-campo-bairro&#039; e etc | Deve retornar o label do botão &#039;sucesso&#039; ou o status de erro retorno. |

<br />


**No clique nas opções de &quot;Cupons ou Vales&quot; ou &quot;Cartão Presente&quot;**<br />

- **Onde:** Na página de &quot;Cupons, Vales ou Cartão Presente&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:cupons-vales-cartao-presente',
    'eventAction': 'clique:opcao',
    'eventLabel': '[[opcao-selecionada]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] | &#039;cupons-vales&#039;, &#039;cartao-presente&#039; e etc | Deve retornar a opção selecionada. |

<br />


**No clique nas opções de &quot;Cartaõ Riachuelo ou Cartão de Crédito&quot; ou &quot;Boleto&quot;**<br />

- **Onde:** Na página de &quot;Pagamento&quot;
    
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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] | &#039;cartao-riachuelo-cartao-credito&#039;, &#039;boleto&#039; e etc | Deve retornar a opção selecionada. |

<br />


**No callback, após clicar no botão &quot;Continuar&quot; para validar os campos preenchidos**<br />

- **Onde:** Nas páginas de &quot;Cartão de Crédito&quot;, &quot;Cartão Riachuelo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout:pagamento',
    'eventAction': 'callback:cartao-credito',
    'eventLabel': '[[sucesso ou tipo-erro]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;, &#039;erro:numero-invalido&#039;, &#039;erro:nome-obrigatorio&#039;, &#039;erro:cpf-incompleto&#039;, &#039;erro:selecione-parcelamento&#039; e etc | Deve retornar o status sucesso ou de erro retorno. |

<br />


**No clique no botão de &quot;Editar&quot; das etapas anteriores**<br />

- **Onde:** Na página de checkout,
    
```html
<div
   data-gtm-event-category='estore:checkout'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;editar-entrega&#039;, &#039;editar-endereco&#039;, &#039;editar-pagamento&#039; | Deve retornar o nome do botão clicado. |

<br />


**No carregamento das opções de entrega**<br />

- **Onde:** Na página de checkout,
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'noInteraction': '1',
    'eventCategory': 'estore:checkout',
    'eventAction': 'tipos-de-entrega',
    'eventLabel': [{
      't':'[[nome-entrega]]',
      'd':'[[previsao]]',
      'p':'[[valor]]'
        },
      {
        't':'[[nome-entrega]]',
        'd':'[[previsao]]',
        'p':'[[valor]]'
        }],
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-entrega]] | &#039;normal&#039;, &#039;expresso &#039;, &#039;entrega-agendada&#039;,&#039;retirar-em-loja&#039;  | Deve retornar os nomes dos tipos de entrega. |
| [[previsao]] | &#039;DD/MM/AAAA&#039;, | Deve retornar a quantidade de dias para a entrega. |
| [[valor]] | &#039;&#039;0.00&#039;, &#039;2.99&#039;, &#039;5.00&#039; | Deve retornar os valores dos fretes. |

<br />


**No  clique do botão &quot;Aplicar&quot; para validar os campos preenchidos de &quot;Cupom&quot;**<br />

- **Onde:** Na página de Carrinho, no Step de &quot;Cupons, vales e trocas&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout',
    'eventAction': 'tentativa:inserir-cupom',
    'eventLabel': 'aplicar',
  });
</script>
```

<br />


**No callback, após clicar no botão &quot;Aplicar&quot; para validar os campos preenchidos de &quot;Cupom&quot;**<br />

- **Onde:** Na página de Carrinho, no Step de &quot;Cupons, vales e trocas&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout',
    'eventAction': 'callback:inserir-cupom',
    'eventLabel': '[[sucesso ou tipo-erro]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;, &#039;erro:cupom-invalido&#039; e etc | Deve retornar o status do retorno. |

<br />


**No clique do botão &quot;Finalizar&quot; para validar os campos preenchidos de &quot;Pagamento&quot; e finalizar a compra**<br />

- **Onde:** Na página de Compra
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:checkout',
    'eventAction': 'callback:tentativa:finalizar-compra',
    'eventLabel': '[[finalizar ou tipo-erro]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[finalizar ou tipo-erro]] | &#039;finalizar&#039;, &#039;erro:verifique-os-dados-inseridos-ou-utilize-outro-meio-pagamento&#039;, &#039;erro:nao-foi-possivel-concluir-tente-novamente&#039; e etc | Deve retornar o label do botão &#039;finalizar&#039; ou o status de erro retorno. |

<br />


**No clique do botão &quot;Copiar Código&quot; para copiar o código de barras**<br />

- **Onde:** Na página de Pedido Realizado com Sucesso
    
```html
<div
   data-gtm-event-category='estore:purchase'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='copiar-codigo-barras'
>Botão</div>
```



<br />


### PDP

**No callback de erro de adicionar ao carrinho quando cor ou tamanho estiver sem seleção**<br />

- **Onde:** Nas páginas de produtos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'estore:produto',
    'eventAction': 'callback:sem-selecao',
    'eventLabel': 'selecionar:[[cor ou tamanho]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cor ou tamanho]] | &#039;m&#039;, &#039;p&#039;, &#039;azul&#039;, &#039;vermelho&#039; e etc | Deve retornar a cor ou o tamanho selecionado. |

<br />


### Enhanced Ecommerce

**Na visualização do grupo de banners exibidos**<br />

- **Onde:** Na página home do site
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'promoView',
    'eventCategory': 'estore:enhanced-ecommerce',
    'eventAction': 'promoView',
    'noInteraction': '1',
    'ecommerce': {
    'promoView': {
      'promotions': [{
        'id': '[[id-promocao]]',
        'name': '[[nome-promocao]]',
        'position': '[[posicao-promocao]]',
      }]
    }
  }
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[id-promocao]] |  &#039;banner123&#039;, &#039;banner234&#039; e etc | ID único do Banner |
| [[nome-promocao]] |  &#039;masculino&#039;, &#039;infantil&#039;, &#039;teen&#039; e etc | Nome amigável do banner |
| [[posicao-promocao]] |  &#039;1&#039; | Posição que o banner é exibido  |

<br />


**No clique dos banners**<br />

- **Onde:** Na página home do site
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'promotionClick',
  'eventCategory': 'estore:enhanced-ecommerce',
  'eventAction': 'promoClick',
  'ecommerce': {
  'promoClick': {
    'promotions': [{
      'id': '[[id-promocao]]',
      'name': '[[nome-promocao]]',
      'position': '[[posicao-promocao]]',
    }]
  }
}
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[id-promocao]] |  &#039;banner123&#039;, &#039;banner234&#039; e etc | ID único do Banner |
| [[nome-promocao]] |  &#039;masculino&#039;, &#039;infantil&#039;, &#039;teen&#039; e etc | Nome amigável do banner |
| [[posicao-promocao]] |  &#039;1&#039; | Posição que o banner é exibido  |

<br />


**Na visualização de uma vitrine de produtos**<br />

- **Onde:** Em todas as páginas que exibirem uma lista de produtos
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productImpressions',
  'eventCategory':'estore:enhanced-ecommerce',
  'eventAction': 'impressions',
    'ecommerce': {
    'impressions': [{
        'dimension15': '[[cd15-product-lookcompleto]]',
        'dimension16': '[[cd16-product-iddolook]]',
        'dimension17': '[[cd17-product-selo/tag]]',
        'dimension18': '[[cd18-product-exclusivoecommerce]]',
        'dimension19': '[[cd19-product-variante]]',
        'dimension20': '[[cd20-product-gm]]',
        'dimension21': '[[cd21-product-preçode]]',
        'dimension22': '[[cd22-product-estilo]]',
        'dimension23': '[[cd23-product-genero]]',
        'dimension24': '[[cd24-product-lojamktplace]]',
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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd15-product-lookcompleto]] |  &#039;true&#039; ou &#039;false&#039; | Look Completo |
| [[cd16-product-iddolook]] |  &#039;produto-sem-vinculo&#039; | ID do produto do tipo look |
| [[cd17-product-selo/tag]] |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| [[cd18-product-exclusivoecommerce]] |  &#039;true&#039; ou &#039;false&#039; | Se é um produto exclusivo do ecommerce |
| [[cd19-product-variante]] |  &#039;p&#039;, &#039;m&#039;, &#039;8-12&#039;, &#039;42&#039; | Tamanho do produto |
| [[cd20-product-gm]] |  &#039;101508&#039;, &#039;310011&#039;, &#039;204002&#039;, etc | Subcategorias composto por 6 numeros |
| [[cd21-product-preçode]] |  &#039;139,99&#039; | Preço de do produto |
| [[cd22-product-estilo]] |  &#039;life-style&#039; | Estilo do produto |
| [[cd23-product-genero]] |  &#039;feminino&#039; | Genero do produto |
| [[cd24-product-lojamktplace]] |  &#039;riachuelo&#039;, &#039;loja-a&#039;, &#039;loja-b&#039; | Nome da loja mktplace |
| [[nome-produto]] |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| [[id-produto]] |  &#039;13239635&#039; | SKU do produto - pai |
| [[preco-produto]] |  &#039;139,99&#039; | Preço do produto |
| [[marca-produto]] |  &#039;pool-original&#039; | Marca do produto |
| [[categoria-produto]] |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| [[lista-produto]] |  &#039;feminino-blusa&#039;, &#039;masculino-polo&#039;, &#039;acessorios-e-relogios-aneis&#039;, &#039;feminino_em-destaque_plus-size&#039;, &#039;plp_quem-viu-tambem-viu&#039;, &#039;pdp_quem-viu-tambem-viu&#039;, &#039;home-destaques&#039;, &#039;novidades&#039; e etc | Nome da lista que o produto aparece |
| [[posicao-produto]] |  &#039;2&#039; | Posição que o produto aparece em uma lista de produtos |

<br />


**No clique dos produtos da vitrine interagidos na página**<br />

- **Onde:** Em todas as páginas que exibirem uma lista de produtos
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': 'productClick',
    'eventCategory': 'estore:enhanced-ecommerce',
    'eventAction': 'click',
    'eventLabel': '-',
        'ecommerce': {
        'click': {
            'actionField': {'list': '[[lista-produto]]'},
            'products': [{
                'dimension15': '[[cd15-product-lookcompleto]]',
                'dimension16': '[[cd16-product-iddolook]]',
                'dimension17': '[[cd17-product-selo/tag]]',
                'dimension18': '[[cd18-product-exclusivoecommerce]]',
                'dimension19': '[[cd19-product-variante]]',
                'dimension20': '[[cd20-product-gm]]',
                'dimension21': '[[cd21-product-preçode]]',
                'dimension22': '[[cd22-product-estilo]]',
                'dimension23': '[[cd23-product-genero]]',
                'dimension24': '[[cd24-product-lojamktplace]]',
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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd15-product-lookcompleto]] |  &#039;true&#039; ou &#039;false&#039; | Look Completo |
| [[cd16-product-iddolook]] |  &#039;produto-sem-vinculo&#039; | ID do produto do tipo look |
| [[cd17-product-selo/tag]] |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| [[cd18-product-exclusivoecommerce]] |  &#039;true&#039; ou &#039;false&#039; | Se é um produto exclusivo do ecommerce |
| [[cd19-product-variante]] |  &#039;p&#039;, &#039;m&#039;, &#039;8-12&#039;, &#039;42&#039; | Tamanho do produto |
| [[cd20-product-gm]] |  &#039;101508&#039;, &#039;310011&#039;, &#039;204002&#039;, etc | Subcategorias composto por 6 numeros |
| [[cd21-product-preçode]] |  &#039;139,99&#039; | Preço de do produto |
| [[cd22-product-estilo]] |  &#039;life-style&#039; | Estilo do produto |
| [[cd23-product-genero]] |  &#039;feminino&#039; | Genero do produto |
| [[cd24-product-lojamktplace]] |  &#039;riachuelo&#039;, &#039;loja-a&#039;, &#039;loja-b&#039; | Nome da loja mktplace |
| [[nome-produto]] |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| [[id-produto]] |  &#039;13239635&#039; | SKU do produto - pai |
| [[preco-produto]] |  &#039;139,99&#039; | Preço do produto |
| [[marca-produto]] |  &#039;pool-original&#039; | Marca do produto |
| [[categoria-produto]] |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| [[lista-produto]] |  &#039;feminino-blusa&#039;, &#039;masculino-polo&#039;, &#039;acessorios-e-relogios-aneis&#039;, &#039;feminino_em-destaque_plus-size&#039;, &#039;plp_quem-viu-tambem-viu&#039;, &#039;pdp_quem-viu-tambem-viu&#039;, &#039;home-destaques&#039;, &#039;novidades&#039; e etc | Nome da lista que o produto aparece |
| [[posicao-produto]] |  &#039;2&#039; | Posição que o produto aparece em uma lista de produtos |

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
  'eventLabel': '-',
    'ecommerce': {
    'detail': {
      'products': [{
        'dimension15': '[[cd15-product-lookcompleto]]',
        'dimension16': '[[cd16-product-iddolook]]',
        'dimension17': '[[cd17-product-selo/tag]]',
        'dimension18': '[[cd18-product-exclusivoecommerce]]',
        'dimension19': '[[cd19-product-variante]]',
        'dimension20': '[[cd20-product-gm]]',
        'dimension21': '[[cd21-product-preçode]]',
        'dimension22': '[[cd22-product-estilo]]',
        'dimension23': '[[cd23-product-genero]]',
        'dimension24': '[[cd24-product-lojamktplace]]',
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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd15-product-lookcompleto]] |  &#039;true&#039; ou &#039;false&#039; | Look Completo |
| [[cd16-product-iddolook]] |  &#039;produto-sem-vinculo&#039; | ID do produto do tipo look |
| [[cd17-product-selo/tag]] |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| [[cd18-product-exclusivoecommerce]] |  &#039;true&#039; ou &#039;false&#039; | Se é um produto exclusivo do ecommerce |
| [[cd19-product-variante]] |  &#039;p&#039;, &#039;m&#039;, &#039;8-12&#039;, &#039;42&#039; | Tamanho do produto |
| [[cd20-product-gm]] |  &#039;101508&#039;, &#039;310011&#039;, &#039;204002&#039;, etc | Subcategorias composto por 6 numeros |
| [[cd21-product-preçode]] |  &#039;139,99&#039; | Preço de do produto |
| [[cd22-product-estilo]] |  &#039;life-style&#039; | Estilo do produto |
| [[cd23-product-genero]] |  &#039;feminino&#039; | Genero do produto |
| [[cd24-product-lojamktplace]] |  &#039;riachuelo&#039;, &#039;loja-a&#039;, &#039;loja-b&#039; | Nome da loja mktplace |
| [[nome-produto]] |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| [[id-produto]] |  &#039;13239635&#039; | SKU do produto - pai |
| [[preco-produto]] |  &#039;139,99&#039; | Preço do produto |
| [[marca-produto]] |  &#039;pool-original&#039; | Marca do produto |
| [[categoria-produto]] |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| [[variacao-produto]] |  &#039;101, 310, 204, etc&#039; | DCO Categorias composto por 3 números |

<br />


**No clique para adicionar produto ao carrinho**<br />

- **Onde:** Na página de detalhes de produto
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'addToCart',
  'eventCategory':'estore:enhanced-ecommerce',
  'eventAction': 'add',
  'eventLabel': '-',
    'ecommerce': {
    'add': {
      'products': [{
        'dimension14': '[[cd14-product-skufilho]]',
        'dimension15': '[[cd15-product-lookcompleto]]',
        'dimension16': '[[cd16-product-iddolook]]',
        'dimension17': '[[cd17-product-selo/tag]]',
        'dimension18': '[[cd18-product-exclusivoecommerce]]',
        'dimension19': '[[cd19-product-variante]]',
        'dimension20': '[[cd20-product-gm]]',
        'dimension21': '[[cd21-product-preçode]]',
        'dimension22': '[[cd22-product-estilo]]',
        'dimension23': '[[cd23-product-genero]]',
        'dimension24': '[[cd24-product-lojamktplace]]',
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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd14-product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[cd15-product-lookcompleto]] |  &#039;true&#039; ou &#039;false&#039; | Look Completo |
| [[cd16-product-iddolook]] |  &#039;produto-sem-vinculo&#039; | ID do produto do tipo look |
| [[cd17-product-selo/tag]] |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| [[cd18-product-exclusivoecommerce]] |  &#039;true&#039; ou &#039;false&#039; | Se é um produto exclusivo do ecommerce |
| [[cd19-product-variante]] |  &#039;p&#039;, &#039;m&#039;, &#039;8-12&#039;, &#039;42&#039; | Tamanho do produto |
| [[cd20-product-gm]] |  &#039;101508&#039;, &#039;310011&#039;, &#039;204002&#039;, etc | Subcategorias composto por 6 numeros |
| [[cd21-product-preçode]] |  &#039;139,99&#039; | Preço de do produto |
| [[cd22-product-estilo]] |  &#039;life-style&#039; | Estilo do produto |
| [[cd23-product-genero]] |  &#039;feminino&#039; | Genero do produto |
| [[cd24-product-lojamktplace]] |  &#039;riachuelo&#039;, &#039;loja-a&#039;, &#039;loja-b&#039; | Nome da loja mktplace |
| [[nome-produto]] |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| [[id-produto]] |  &#039;13239635&#039; | SKU do produto - pai |
| [[preco-produto]] |  &#039;139,99&#039; | Preço do produto |
| [[marca-produto]] |  &#039;pool-original&#039; | Marca do produto |
| [[categoria-produto]] |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| [[variacao-produto]] |  &#039;101, 310, 204, etc&#039; | DCO Categorias composto por 3 números |
| [[quantidade-produto]] |  &#039;1&#039;, &#039;2&#039; e etc | Quantidade do produto |

<br />


**No clique para remover produto do carrinho, dentro do lightbox de &quot;Tem certeza que quer remover esse item da sua sacola?&quot;**<br />

- **Onde:** No lightbox de &quot;Tem certeza que quer remover esse item da sua sacola?&quot;
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'removeFromCart',
  'eventCategory': 'estore:enhanced-ecommerce',
  'eventAction': 'remove',
  'eventLabel': '-',
    'ecommerce': {
    'remove': {
      'products': [{
        'dimension14': '[[cd14-product-skufilho]]',
        'dimension15': '[[cd15-product-lookcompleto]]',
        'dimension16': '[[cd16-product-iddolook]]',
        'dimension17': '[[cd17-product-selo/tag]]',
        'dimension18': '[[cd18-product-exclusivoecommerce]]',
        'dimension19': '[[cd19-product-variante]]',
        'dimension20': '[[cd20-product-gm]]',
        'dimension21': '[[cd21-product-preçode]]',
        'dimension22': '[[cd22-product-estilo]]',
        'dimension23': '[[cd23-product-genero]]',
        'dimension24': '[[cd24-product-lojamktplace]]',
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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd14-product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[cd15-product-lookcompleto]] |  &#039;true&#039; ou &#039;false&#039; | Look Completo |
| [[cd16-product-iddolook]] |  &#039;produto-sem-vinculo&#039; | ID do produto do tipo look |
| [[cd17-product-selo/tag]] |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| [[cd18-product-exclusivoecommerce]] |  &#039;true&#039; ou &#039;false&#039; | Se é um produto exclusivo do ecommerce |
| [[cd19-product-variante]] |  &#039;p&#039;, &#039;m&#039;, &#039;8-12&#039;, &#039;42&#039; | Tamanho do produto |
| [[cd20-product-gm]] |  &#039;101508&#039;, &#039;310011&#039;, &#039;204002&#039;, etc | Subcategorias composto por 6 numeros |
| [[cd21-product-preçode]] |  &#039;139,99&#039; | Preço de do produto |
| [[cd22-product-estilo]] |  &#039;life-style&#039; | Estilo do produto |
| [[cd23-product-genero]] |  &#039;feminino&#039; | Genero do produto |
| [[cd24-product-lojamktplace]] |  &#039;riachuelo&#039;, &#039;loja-a&#039;, &#039;loja-b&#039; | Nome da loja mktplace |
| [[nome-produto]] |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| [[id-produto]] |  &#039;13239635&#039; | SKU do produto - pai |
| [[preco-produto]] |  &#039;139,99&#039; | Preço do produto |
| [[marca-produto]] |  &#039;pool-original&#039; | Marca do produto |
| [[categoria-produto]] |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| [[variacao-produto]] |  &#039;101, 310, 204, etc&#039; | DCO Categorias composto por 3 números |
| [[quantidade-produto]] |  &#039;1&#039;, &#039;2&#039; e etc | Quantidade do produto |

<br />


**No carregamento das páginas do fluxo de compra do  checkout.**<br />

- **Onde:** Nas páginas de Carrinho(sacola), &quot;Endereço&quot;, &quot;Opções de Entrega&quot;, &quot;Cupons,vales e trocas&quot; e &quot;Pagamento&quot;
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'checkout',
  'eventCategory': 'estore:enhanced-ecommerce',
  'eventAction': 'checkout',
  'eventLabel': '-',
  'dimension12': '[[cd112-hit-frete]]',
  'dimension13': '[[cd13-hit-parcelamento]]',
  'ecommerce': {
    'checkout': {
      'actionField': {'step': '[[passo-checkout]]'},
      'products': [{
        'dimension14': '[[cd14-product-skufilho]]',
        'dimension15': '[[cd15-product-lookcompleto]]',
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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[checkout-index]] |  | Retornar &quot;1&quot; ou &quot;2&quot; ou &quot;3&quot; ou &quot;4&quot; ou &quot;5&quot; os exemplos citados |
| [[cd112-hit-frete]] |  &#039;normal, expresso, entrega-agendada&#039;  | Nome do tipo de entrega.  |
| [[cd13-hit-parcelamento]] |  &#039;2&#039; | Quantidade de parcelas |
| [[cd14-product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[cd15-product-lookcompleto]] |  &#039;true&#039; ou &#039;false&#039; | Look Completo |
| [[nome-produto]] |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| [[id-produto]] |  &#039;13239635&#039; | SKU do produto - pai |
| [[preco-produto]] |  &#039;139,99&#039; | Preço do produto |
| [[marca-produto]] |  &#039;pool-original&#039; | Marca do produto |
| [[categoria-produto]] |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| [[variacao-produto]] |  &#039;101, 310, 204, etc&#039; | DCO Categorias composto por 3 números |
| [[quantidade-produto]] |  &#039;1&#039;, &#039;2&#039; e etc | Quantidade do produto |
| [[passo-checkout]] |  &#039;1&#039; | Página de carrinho de compras |
| [[passo-checkout]] |  &#039;2&#039; | Página de confirmação do endereço de entrega |
| [[passo-checkout]] |  &#039;3&#039; | Página de seleção de opções de entrega |
| [[passo-checkout]] |  &#039;4&#039; | Página de seleção de cupons e vales ou trocas |
| [[passo-checkout]] |  &#039;5&#039; | Página de seleção do método de pagamento |

<br />


**Ao selecionar uma opção de entrega ou pagamento**<br />

- **Onde:** Na página de Entrega, no Step de "Modo de Envio" e na página de Pagamento
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'checkout',
  'eventCategory': 'estore:enhanced-ecommerce',
  'eventAction': 'checkoutoption',
  'eventLabel': '[[opcoes-de-entrega ou tipo-de-pagamento]]',
    'ecommerce': {
    'checkout': {
      'actionField': {'step':'[[passo-checkout]]','option':'[[shipping ou payment]]:[[opcao escolhida]]:[[previsao-entrega]]'},
    }
  }
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcoes-de-entrega ou tipo-de-pagamento]] | 'opcoes-de-entrega' ou 'tipo-de-pagamento' | Deve retornar a opção escolhida pelo usuário |
| [[shipping ou payment]] | 'shipping' ou 'payment' | Deve retornar a opção escolhida pelo usuário |
| [[opcao escolhida]] | 'boleto', 'cartao-de-credito', 'frete-gratis' e etc | Deve retornar a opção escolhida pelo usuário |
| [[previsao]] | &#039;DD/MM/AAAA&#039;, | Deve retornar a quantidade de dias para a entrega. |
| [[passo-checkout]] |  &#039;1&#039; | Página de carrinho de compras |
| [[passo-checkout]] |  &#039;2&#039; | Página de confirmação do endereço de entrega |
| [[passo-checkout]] |  &#039;3&#039; | Página de seleção de opções de entrega |
| [[passo-checkout]] |  &#039;4&#039; | Página de seleção de cupons e vales ou trocas |
| [[passo-checkout]] |  &#039;5&#039; | Página de seleção do método de pagamento |

<br />


**No carregamento da página  de resumo de compra  após confirmação.**<br />

- **Onde:** Na página de confirmação de compra
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'purchase',
  'eventCategory': 'eestore:nhanced-ecommerce',
  'eventAction': 'purchase',
  'dimension5': '[[cd5-user-funcionarioid]]',
  'dimension6': '[[cd6-hit-loja]]',
  'dimension8': '[[cd8-hit-tipodecartão]]',
  'dimension9': '[[cd9-hit-bandeiracartão]]',
  'dimension10': '[[cd10-hit-metodopagamento]]',
  'dimension11': '[[cd11-hit-statuspagamento]]',
  'dimension12': '[[cd112-hit-frete]]',
  'dimension13': '[[cd13-hit-parcelamento]]',
  'dimension26': '[[cd26-hit-valorfrete]]',
  'dimension27': '[[cd27-hit-prazofrete]]',
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
        'dimension14': '[[cd14-product-skufilho]]',
        'dimension15': '[[cd15-product-lookcompleto]]',
        'dimension16': '[[cd16-product-iddolook]]',
        'dimension17': '[[cd17-product-selo/tag]]',
        'dimension18': '[[cd18-product-exclusivoecommerce]]',
        'dimension19': '[[cd19-product-variante]]',
        'dimension20': '[[cd20-product-gm]]',
        'dimension21': '[[cd21-product-preçode]]',
        'dimension22': '[[cd22-product-estilo]]',
        'dimension23': '[[cd23-product-genero]]',
        'dimension24': '[[cd24-product-lojamktplace]]',
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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd5-user-funcionarioid]] |  &#039;10203040&#039; | ID único do funcionário |
| [[cd6-hit-loja]] |  &#039;ABC&#039; | Nome da Loja preenchido |
| [[cd8-hit-tipodecartão]] |  &#039;credito&#039;, &#039;riachuelo&#039; | Tipo do cartão utilizado na compra |
| [[cd9-hit-bandeiracartão]] |  &#039;mastercard&#039; | Bandeira do cartão utilizada na compra |
| [[cd10-hit-metodopagamento]] |  &#039;cartao&#039;, &#039;boleto&#039; | Nome do tipo do pagamento. |
| [[cd11-hit-statuspagamento]] |  &#039;aprovado&#039;   | Nome do status do pagamento.  |
| [[cd112-hit-frete]] |  &#039;normal, expresso, entrega-agendada&#039;  | Nome do tipo de entrega.  |
| [[cd13-hit-parcelamento]] |  &#039;2&#039; | Quantidade de parcelas |
| [[cd14-product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[cd15-product-lookcompleto]] |  &#039;true&#039; ou &#039;false&#039; | Look Completo |
| [[cd16-product-iddolook]] |  &#039;produto-sem-vinculo&#039; | ID do produto do tipo look |
| [[cd17-product-selo/tag]] |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. **OBS:** Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| [[cd18-product-exclusivoecommerce]] |  &#039;true&#039; ou &#039;false&#039; | Se é um produto exclusivo do ecommerce |
| [[cd19-product-variante]] |  &#039;p&#039;, &#039;m&#039;, &#039;8-12&#039;, &#039;42&#039; | Tamanho do produto |
| [[cd20-product-gm]] |  &#039;101508&#039;, &#039;310011&#039;, &#039;204002&#039;, etc | Subcategorias composto por 6 numeros |
| [[cd21-product-preçode]] |  &#039;139,99&#039; | Preço de do produto |
| [[cd22-product-estilo]] |  &#039;life-style&#039; | Estilo do produto |
| [[cd23-product-genero]] |  &#039;feminino&#039; | Genero do produto |
| [[cd24-product-lojamktplace]] |  &#039;riachuelo&#039;, &#039;loja-a&#039;, &#039;loja-b&#039; | Nome da loja mktplace |
| [[cd26-hit-valorfrete]] |  &#039;gratuito&#039;, &#039;15.90&#039; e etc | Valor do frete escolhido |
| [[cd27-hit-prazofrete]] |  &#039;DD/MM/AAAA&#039; | Prazo do frete escolhido |
| [[id-transacao]] |  &#039;000011652&#039; | ID único da transação |
| [[valor-total-transacao]] |  &#039;139,99&#039; | Valor total da transação incluindo frete e taxas |
| [[frete-transacao]] |  &#039;129,99&#039; | Valor do frete da transação |
| [[coupon-transacao]] |  &#039;cupom-2018&#039; | Cupom de desconto utilizado na transação - promoção nivel pedido |
| [[afiliacao-transacao]] |  &#039;Riachuelo&#039; | Nome fixo da loja - usada para Marketplace |
| [[taxa-transacao]] |  &#039;3,99&#039; | Valor de taxas da transação |
| [[nome-produto]] |  &#039;calca-masculina-super-skinny-em-jeans&#039; | Nome do produto |
| [[id-produto]] |  &#039;13239635&#039; | SKU do produto - pai |
| [[preco-produto]] |  &#039;139,99&#039; | Preço do produto |
| [[marca-produto]] |  &#039;pool-original&#039; | Marca do produto |
| [[categoria-produto]] |  &#039;masculino/calças&#039; | Departamento e Categoria do produto |
| [[variacao-produto]] |  &#039;101, 310, 204, etc&#039; | DCO Categorias composto por 3 números |
| [[quantidade-produto]] |  &#039;1&#039;, &#039;2&#039; e etc | Quantidade do produto |

<br />


<br />

---

## Considerações Finais
> Recomendações do Google:
> 0. [GA - Index ID ]($reference->url)
> 1. []($reference->url)

> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<script>
  document.addEventListener('DOMContentLoaded', function(event) {
    document.querySelectorAll('h1 a')[0].style.display = 'none';
  });
</script>
