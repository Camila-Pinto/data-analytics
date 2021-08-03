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
- [Inicio](#inicio)
- [Login](#login)
- [Redefinir Minha Senha](#redefinir-minha-senha)
- [Home](#home)
- [Minha loja](#minha-loja)
- [Painel Mensagens](#painel-mensagens)
- [Loja criada](#loja-criada)
- [Dashboard](#dashboard)
- [Compartilhe](#compartilhe)
- [Como Funciona](#como-funciona)
- [Junte-se a nós](#junte-se-a-n&oacute;s)
- [Tutoriais](#tutoriais)
- [Meus dados e Comissão](#meus-dados-e-comiss&#227;o)
- [Exclusão de Conta](#exclus&#227;o-de-conta)
- [Faq](#faq)
- [Enhanced E-commerce](#enhanced-e-commerce)
- [Considerações Finais](#considera&#231;&#245;es-finais)

<br />

## Implementação da Camada de dados - Projeto Rchlover
Última atualização: 22/04/2021 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [https://www.sourchlover.com.br/](https://www.sourchlover.com.br/).

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-PHQH8GH');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-PHQH8GH" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
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
    'dimension1': '[[userid]]',
    'dimension4': '[[afiliado]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[userid]] | &quot;01234&quot; | ID único de usuário defeinido após o cadastro |
| [[afiliado]] | &quot;riachuelo&quot;, &quot;cacau-show&quot;, &quot;autonomo&quot; e etc | Retornar o nome do colaborador, parceiro ou a qualquer pessoa |

---


### Geral


**No clique do botão sair.**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='rchlover:geral'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='sair'
>Botão</div>
```



<br />


**No clique do botões no modal Nps**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:nps',
    'eventAction': 'clique:botao:modal',
    'eventLabel': '[[nome-botao]]:[[parte]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;enviar&#039;, &#039;fechar&#039; | Deve retornar o nome clicado no modal. |
| [[parte]] | &#039;primeira-parte&#039; ou &#039;segunda-parte&#039; | Deve retornar em qual parte do modal está. |

<br />


**Na tentativa de callback para enviar o modal Nps**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:nps',
    'eventAction': 'callback:enviar:modal',
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


**Ao clicar nos botões que aparecem no modal de aviso.**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='rchlover:mensagem'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-elemento]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-elemento]] | &#039;fechar&#039;. &#039;ir-para-meus-dados&#039; e etc. | Deve retornar o nome do elemento. |

<br />


### Inicio

**No clique dos botões para iniciar.**<br />

- **Onde:** Na página inicial.
    
```html
<div
   data-gtm-event-category='rchlover:inicio'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;acessar&#039; ou &#039;junte-se-a-nos&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique dos botões do header**<br />

- **Onde:** Na página inicial.
    
```html
<div
   data-gtm-event-category='rchlover:inicio'
   data-gtm-event-action='clique:header'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;venda-online&#039;, &#039;como-funciona&#039;, &#039;acessar&#039; e etc | Deve retornar o nome do item clicado. |

<br />


**No clique dos botões do footer**<br />

- **Onde:** Na página inicial.
    
```html
<div
   data-gtm-event-category='rchlover:inicio'
   data-gtm-event-action='clique:footer'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;politica-de-privacidade&#039;, &#039;facebook&#039; e etc | Deve retornar o nome do item clicado. |

<br />


**No clique do botão &quot;criar minha loja&quot; de cada seção**<br />

- **Onde:** Na página inicial.
    
```html
<div
   data-gtm-event-category='rchlover:inicio'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[secao]]:criar-minha-loja'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[secao]] | &#039;mais-uma-oportunidade-para-voce&#039;, &#039;crie-agora-sua-loja&#039; e etc | Deve retornar a seção do botão criar minha loja. |

<br />


**Na interação com o vídeo**<br />

- **Onde:** Na página inicial
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:inicio',
    'eventAction': 'interacao:video',
    'eventLabel': '[[nome-video]]:[[acao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-video]] | &#039;veja-como-e-facil-aprender&#039; e etc | Deve retornar o nome do vídeo. |
| [[acao]] | &#039;play&#039; ou &#039;pause&#039; | Deve retornar a ação do usuário. |

<br />


**No clique dos botões**<br />

- **Onde:** Na página inicial
    
```html
<div
   data-gtm-event-category='rchlover:inicio'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]-perguntas-frequentes'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;cadastro&#039;, &#039;comissao&#039;, &#039;vendas&#039; ou &#039;divulgacao&#039; | Deve retornar o nome do botão clicado. |

<br />


**Na interação com as perguntas**<br />

- **Onde:** Na página inicial
    
```html
<div
   data-gtm-event-category='rchlover:inicio'
   data-gtm-event-action='interacao:pergunta'
   data-gtm-event-label='[[acao]]:[[nome-pergunta]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-pergunta]] | &#039;onde-posso-personalizar-minha-loja&#039;, &#039;o-que-e-o-sourchlover&#039; e etc. | Deve retornar o nome da pergunta. |
| [[acao]] | &#039;expandiu&#039; ou &#039;recolheu&#039; | Deve retornar a ação do usuário. |

<br />


### Login

**Na interação com os campos.**<br />

- **Onde:** Na página de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:login',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;email-ou-credenciais&#039; ou &#039;senha&#039;. | Deve retornar o nome do campo preenchido. |

<br />


**Na interação com o checkbox.**<br />

- **Onde:** Na página de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:login',
    'eventAction': 'interacao:checkbox',
    'eventLabel': 'mantenha-me-conectado'
  });
</script>
```




<br />


**No clique dos botões.**<br />

- **Onde:** Na página de login.
    
```html
<div
   data-gtm-event-category='rchlover:login'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;acessar&#039;, &#039;esqueci-minha-senha&#039; e etc | Deve retornar o nome do botão clicado. |

<br />


**No callback da tentiva de login.**<br />

- **Onde:** Na página de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:login',
    'eventAction': 'envio:callback',
    'eventLabel': '[[sucesso ou tipo-de-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039; ou &#039;erro:voce-precisa-inserir-todos-os-dados-para-continuar&#039; e etc | Deve retornar a mensagem de sucesso ou o tipo de erro. |

<br />


**No clique dos botões em Bem vindo.**<br />

- **Onde:** Na página de login.
    
```html
<div
   data-gtm-event-category='rchlover:login'
   data-gtm-event-action='clique:termo-adesao:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;recusar&#039; ou &#039;aceitar-e-prosseguir&#039;. | Deve retornar o nome do botão clicado. |

<br />


### Redefinir Minha Senha

**Na interação com o campo &quot;Email ou credenciais de acesso&quot;.**<br />

- **Onde:** Na página Redefinir minha senha.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:redefinir-minha-senha',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:email-ou-credenciais-de-acesso'
  });
</script>
```




<br />


**No clique do botão ou link.**<br />

- **Onde:** Na página Redefinir minha senha.
    
```html
<div
   data-gtm-event-category='rchlover:redefinir-minha-senha'
   data-gtm-event-action='clique:[[botao ou link]]'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao ou link]] | &#039;botao&#039; ou &#039;link&#039;. | Deve retornar o tipo do elemento clicado. |
| [[nome-item]] | &#039;continuar&#039; ou &#039;entrar&#039;  | Deve retornar o nome do item clicado. |

<br />


**Na tentiva de callback do campo &quot;Email ou credenciais de acesso&quot;.**<br />

- **Onde:** Na página Redefinir minha senha.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:redefinir-minha-senha',
    'eventAction': 'envio:callback-email-ou-credenciais',
    'eventLabel': '[[sucesso ou tipo-de-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;erro:matricula-ou-senha-invalida&#039; e etc | Deve retornar a mensagem de sucesso ou tipo de erro. |

<br />


**No preenchimento dos campos em &quot;Insira seus dados&quot;**<br />

- **Onde:** Na página Redefinir minha senha.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:redefinir-minha-senha',
    'eventAction': 'interacao:insira-seus-dados:campo',
    'eventLabel': 'preencheu:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;cpf&#039;, &#039;data-de-nascimento&#039; e etc | Deve retornar o nome do campo preenchido. |

<br />


**Na tentiva de callback para redefinir senha.**<br />

- **Onde:** Na página Redefinir minha senha.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'rchlover:redefinir-minha-senha',
    'eventAction': 'envio:callback-insira-seus-dados',
    'eventLabel': '[[sucesso ou tipo-de-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;erro:nao-foi-possivel-validar-as-informacoes-fornecidas&#039; e etc | Deve retornar a mensagem de sucesso ou tipo de erro. |

<br />


**No clique dos botões em &quot;Seu acesso expirou&quot;**<br />

- **Onde:** Na página Redefinir minha senha.
    
```html
<div
   data-gtm-event-category='rchlover:redefinir-minha-senha'
   data-gtm-event-action='clique:seu-acesso-expirou:link'
   data-gtm-event-label='clique-aqui:[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;quer-reiniciar-seu-login&#039; e etc | Deve retornar o nome da categoria do link. |

<br />


### Home

**No clique dos menus e sub-menus laterais.**<br />

- **Onde:** Na página home.
    
```html
<div
   data-gtm-event-category='rchlover:home'
   data-gtm-event-action='clique:menu'
   data-gtm-event-label='[[nome-menu]]:[[nome-sub-menu]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-menu]] | &#039;dashboard&#039;, &#039;compartilhe&#039; e etc | Deve retornar o nome do menu clicado. |
| [[nome-sub-menu]] | &#039;acompanhe-sua-evolucao&#039;, &#039;historico-de-campanhas&#039; e etc | Deve retornar o nome do sub menu clicado. |

<br />


**No clique do botão no lightbox &quot;loja.sourchlover&quot;**<br />

- **Onde:** Na página home.
    
```html
<div
   data-gtm-event-category='rchlover:home'
   data-gtm-event-action='clique:botao:lightbox-loja'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
|  [[nome-botao]] | &#039;personalizar-loja&#039;, &#039;fechar&#039;. | Deve retornar o nome do botão. |

<br />


**Nos botoes do video da home**<br />

- **Onde:** Na página home.
    
```html
<div
   data-gtm-event-category='rchlover:home'
   data-gtm-event-action='interacao:video'
   data-gtm-event-label='onboarding:[[botao-video]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-video]] | &#039;play&#039;,  &#039;pause&#039;, &#039;continuar&#039;, etc  | Retorna qual botao foi clicado pelo usuário ao ver o vídeo no onboarding. |

<br />


### Minha loja

**No clique do botão para adicionar foto de perfil, no menu minha loja, sub menu personalizar**<br />

- **Onde:** Na página minha loja
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='adicionar-foto-perfil:personalizar'
>Botão</div>
```



<br />


**No preenchimento dos campos, no menu minha loja, sub menu personalizar**<br />

- **Onde:** Na página minha loja
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-loja',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]:personalizar'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome-da-loja&#039;, &#039;descricao&#039; e etc | Deve retornar o nome do campo preenchido. |

<br />


**No clique dos botões no menu minha loja, sub menu personalizar**<br />

- **Onde:** Na página minha loja
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]:personalizar'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;copiar-link&#039;, &#039;cancelar&#039;, &#039;salvar&#039;. | Deve retornar o nome do botão. |

<br />


**No clique dos botões no lightbox &quot;selecionar foto perfil&quot; no menu minha loja, sub menu personalizar**<br />

- **Onde:** Na página minha loja
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja'
   data-gtm-event-action='clique:botao:lightbox-selecionar-foto-perfil'
   data-gtm-event-label='[[nome-botao]]:personalizar'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;salvar&#039;, &#039;cancelar&#039;, &#039;fechar&#039; | Deve retornar o nome do botão. |

<br />


**No clique de ativar/desativar &quot;minha loja&quot;**<br />

- **Onde:** Na página minha loja
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[ativar-desativar]]-lojinha:personalizar'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[ativar-desativar]] | &#039;ativar&#039; , &#039;desativar&#039;. | Deve retornar se a pessoa ativou ou desativou a lojinha. |

<br />


**Na tentativa de callback para adicionar foto do perfil, no menu minha loja, sub menu personalizar**<br />

- **Onde:** Na página minha loja
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-loja',
    'eventAction': 'envio:callback:adicionar-foto-perfil',
    'eventLabel': '[[sucesso ou tipo-de-erro]]:personalizar'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;erro:formato-imagem-invalido&#039; e etc | Deve retornar a mensagem de sucesso ou tipo de erro. |

<br />


**Na tentativa de callback para personalização da minha loja no menu minha loja, sub menu personalizar**<br />

- **Onde:** Na página minha loja
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-loja',
    'eventAction': 'envio:callback:personalizacao-minha-loja',
    'eventLabel': '[[sucesso ou tipo-de-erro]]:personalizar'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;erro:este-link-ja-esta-sendo-usado&#039; e etc | Deve retornar a mensagem de sucesso ou tipo de erro. |

<br />


**No clique do botão &quot;Adicionar produtos a loja&quot; apos o callback de sucesso, no menu minha loja, sub menu personalizar**<br />

- **Onde:** Na página minha loja
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja'
   data-gtm-event-action='clique:botao:sucesso'
   data-gtm-event-label='adicionar-produtos-a-loja:personalizar'
>Botão</div>
```



<br />


**Na interação com o campo de busca, no menu minha loja, sub menu adicionar produtos**<br />

- **Onde:** Na página minha loja
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-loja',
    'eventAction': 'interacao:campo',
    'eventLabel': 'busca:[[termo-buscado]]:adicionar-produtos'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[termo-buscado]] | &#039;sueter&#039;, &#039;camiseta&#039; e etc | Deve retornar o termo buscado. |

<br />


**No clique do botão &quot;buscar&quot;, no menu minha loja, sub menu adicionar produtos**<br />

- **Onde:** Na página minha loja
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='buscar:adicionar-produtos'
>Botão</div>
```



<br />


**Na tentativa de callback para efetuar a busca, no menu minha loja, sub menu adicionar produtos**<br />

- **Onde:** Na página minha loja
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-loja',
    'eventAction': 'envio:callback:busca',
    'eventLabel': '[[termo-buscado]]:adicionar-produtos'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[termo-buscado]] | &#039;sueter&#039;, &#039;camiseta&#039; e etc | Deve retornar o termo buscado. |

<br />


**No clique do botão &quot;Adicionar&quot; ou &quot;Remover&quot; os produtos, no menu minha loja, sub menu adicionar produtos**<br />

- **Onde:** Na página minha loja
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]:[[produto]]:adicionar-produtos'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;adicionar&#039; ou &#039;remover&#039; | Deve retornar o nome do botão clicado. |
| [[produto]] | &#039;suter-trico-listras-bege&#039; e etc | Deve retornar o nome do produto. |

<br />


**No clique do botão &quot;remover-tudo&quot; os produtos, no menu minha loja, sub menu adicionar produtos**<br />

- **Onde:** Na página minha loja
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='remover-tudo'
>Botão</div>
```



<br />


**Na interação com os filtros, no menu minha loja, sub menu adicionar produtos**<br />

- **Onde:** Na página minha loja
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-loja',
    'eventAction': 'interacao:filtro',
    'eventLabel': '[[nome-filtro]]:[[valor-selecionado]]:adicionar-produtos'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-filtro]] | &#039;novidades&#039;, &#039;feminino&#039;, &#039;masculino&#039; e etc | Deve retornar o nome do filtro. |
| [[valor-selecionado]] | &#039;colecao-feminina&#039;, &#039;plus-size&#039; e etc | Deve retornar o valor selecionado no filtro. |

<br />


**No clique dos botões ou links, no menu minha loja, sub menu adicionar produtos**<br />

- **Onde:** Na página minha loja
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja'
   data-gtm-event-action='clique:[[botao ou link]]'
   data-gtm-event-label='[[nome-item]]:adicionar-produtos'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao ou link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o tipo de elemento clicado. |
| [[nome-item]] | &#039;carregar-mais-produtos&#039;, &#039;ir-para-minha-loja&#039;, &#039;clique-aqui-para-ver-seus-produtos&#039;. | Deve retornar o nome do item clicado. |

<br />

### Painel Mensagens

**No clique dos elementos**<br />

- **Onde:** No painel do menu Mensagens
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:painel-mensagem',
    'eventAction': 'clique:[[botao ou link]]',
    'eventLabel': '[[item-clicado]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao ou link]] | 'botao' ou 'link' | Deve retornar o tipo de elemento clicado |
| [[item-clicado]] | 'meus-dados', 'carregar-mais' e etc | Deve retornar o nome do item clicado. |

<br />


### Loja criada

**No clique dos itens do header**<br />

- **Onde:** Na página da loja criada.
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja:loja-criada'
   data-gtm-event-action='clique:header'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;logo&#039;, &#039;busca&#039; e etc | Deve retornar o nome do item clicado. |

<br />


**No clique dos itens do footer**<br />

- **Onde:** Na página da loja criada.
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja:loja-criada'
   data-gtm-event-action='clique:footer'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;politica-de-privacidade&#039;, &#039;termos-e-condicoes-de-uso&#039; e etc | Deve retornar o nome do item clicado. |

<br />


**Na interação com a alternância de páginas**<br />

- **Onde:** Na página da loja criada.
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja:loja-criada'
   data-gtm-event-action='clique:alternancia'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o nome do item clicado. |

<br />


**Na interação com a busca**<br />

- **Onde:** Na página da loja criada.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-loja:loja-criada',
    'eventAction': 'interacao:campo',
    'eventLabel': 'busca:[[termo-buscado]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[termo-buscado]] | &#039;sueter&#039;, &#039;camiseta&#039; e etc | Deve retornar o termo buscado. |

<br />


**Na tentativa de callback para efetuar a busca.**<br />

- **Onde:** Na página da loja criada.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-loja:loja-criada',
    'eventAction': 'envio:callback:busca',
    'eventLabel': '[[termo-buscado]]:adicionar-produtos'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[termo-buscado]] | &#039;sueter&#039;, &#039;camiseta&#039; e etc | Deve retornar o termo buscado. |

<br />


**No clique para copiar o cupom**<br />

- **Onde:** Na página da loja criada.
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja:loja-criada'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='copiar-cupom'
>Botão</div>
```



<br />


**Na interação com o filtro**<br />

- **Onde:** Na página da loja criada.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-loja:loja-criada',
    'eventAction': 'interacao:filtro',
    'eventLabel': '[[nome-filtro]]:[[valor-selecionado]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-filtro]] | &#039;ordernar-por&#039; e etc | Deve retornar o nome do filtro. |
| [[valor-selecionado]] | &#039;menor-preco&#039;, &#039;maior-preco&#039;, 'personalizado' e etc | Deve retornar o valor selecionado no filtro. |

<br />


**No clique do link &quot;Crie a sua loja&quot;**<br />

- **Onde:** Na página da loja criada.
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja:loja-criada'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='crie-sua-loja'
>Botão</div>
```



<br />


**No clique do botão &quot;Comprar&quot;**<br />

- **Onde:** Na página da loja criada.
    
```html
<div
   data-gtm-event-category='rchlover:minha-loja:loja-criada'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='comprar:[[nome-produto]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;luminaria-de-mesa-madeira&#039; e etc. | Deve retornar o nome do produto. |

<br />

**Napágina de callback de &quot;loja não encontrada&quot; ou &quot;não existem produtos nessa loja&quot;.**<br />

- **Onde:** Na página da loja criada.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-loja:loja-criada',
    'eventAction': 'erro:calback',
    'eventLabel': '[[status]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;loja-não-encontrada&#039;, &#039;nao-existe-produtos-nessa-loja&#039; e etc. | Deve retornar qual o status de erro da tentativa de acesso a loja. |

<br />


### Dashboard

**No callback de erro &quot;Cupom não cadastrado&quot;  no sub menu acompanhe sua evolução, no menu Dashboard**<br />

- **Onde:** Em Dashboard
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:dashboard',
    'eventAction': 'envio:callback',
    'eventLabel': 'erro:cupom-nao-cadastrado:acompanhe-sua-evolucao'
  });
</script>
```




<br />


**No clique para copiar código de cupom no sub menu acompanhe sua evolução, no sub menu acompanhe sua evolução no menu Dashboard**<br />

- **Onde:** Em Dashboard
    
```html
<div
   data-gtm-event-category='rchlover:dashboard'
   data-gtm-event-action='clique:copiar-codigo'
   data-gtm-event-label='acompanhe-sua-evolucao'
>Botão</div>
```



<br />


**No clique do botão Visualizar no sub menu histórico de campanhas, no menu Dashboard**<br />

- **Onde:** Em Dashboard
    
```html
<div
   data-gtm-event-category='rchlover:dashboard:historico-de-campanhas'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='visualizar:[[nome-da-campanha]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-da-campanha]] | &#039;nome-campanha&#039; e etc | Deve retornar o nome da campanha. |

<br />


### Compartilhe

**Na interação com o campo &quot;Busca&quot; no sub menu produtos, no menu Compartilhe**<br />

- **Onde:** Em compartilhe
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:compartilhe',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:busca:produtos'
  });
</script>
```




<br />


**No clique do botão buscar, no sub menu produtos, no menu Compartilhe**<br />

- **Onde:** Em compartilhe
    
```html
<div
   data-gtm-event-category='rchlover:compartilhe'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='preencheu:busca:produtos:[[palavra-pesquisada]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[palavra-pesquisada]] |  | Deve retornar o que foi preenchido no campo da busca |

<br />


**No callback da tentiva de busca, no sub menu produtos, no menu Compartilhe**<br />

- **Onde:** Em compartilhe
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:compartilhe',
    'eventAction': 'envio:callback',
    'eventLabel': '[[sucesso ou tipo-de-erro]]:produtos'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;erro:nenhum-produto-foi-encontrado&#039; e etc | Deve retornar a mensagem de sucesso ou o tipo de erro. |

<br />


**Na interação com os filtros, no sub menu produtos, no menu Compartilhe**<br />

- **Onde:** Em compartilhe
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:compartilhe',
    'eventAction': 'interacao:filtro',
    'eventLabel': '[[nome-filtro]]:[[valor-selecionado]]:produtos'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-filtro]] | &#039;novidades&#039;, &#039;feminino&#039;, &#039;masculino&#039; e etc | Deve retornar o nome do filtro. |
| [[valor-selecionado]] | &#039;colecao-feminina&#039;, &#039;plus-size&#039; e etc | Deve retornar o valor selecionado no filtro. |

<br />


**No clique do botão &quot;gerar link&quot; de cada produto, no sub menu produtos, no menu Compartilhe**<br />

- **Onde:** Em compartilhe
    
```html
<div
   data-gtm-event-category='rchlover:compartilhe'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='gerar-link:[[nome-produto]]:produtos'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;sueter-listrado&#039; e etc | Deve retornar o nome do produto. |

<br />


**No clique dos botões do lightbox &quot;Gerar Link&quot;, no sub menu produtos, no menu Compartilhe**<br />

- **Onde:** Em compartilhe
    
```html
<div
   data-gtm-event-category='rchlover:compartilhe'
   data-gtm-event-action='clique:lightbox-gerar-link:botao'
   data-gtm-event-label='[[nome-botao]]:[[formato]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;whatsapp&#039;, &#039;copiar-link&#039;, &#039;fechar&#039; e etc | Deve retornar o nome do botão clicado. |
| [[formato]] | &#039;campanha, arte,  guide ou produtos&#039; | Deve retornar o formato utilizado. |

<br />


**No clique em &quot;Gerar Link&quot;, no sub menu campanhas, no menu Compartilhe**<br />

- **Onde:** Em compartilhe
    
```html
<div
   data-gtm-event-category='rchlover:compartilhe'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='gerar-link:[[nome-campanha]]:[[formato]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campanha]] | &#039;ta-sabendo-linha-fitness&#039; e etc. | Deve retornar o nome da campanha. |
| [[formato]] | &#039;campanha, arte,  guide ou produtos&#039; | Deve retornar o formato utilizado. |

<br />


**No clique para compartilhar em imagens , no sub menu campanhas, no menu Compartilhe**<br />

- **Onde:** Em compartilhe
    
```html
<div
   data-gtm-event-category='rchlover:compartilhe'
   data-gtm-event-action='clique:imagens:icone'
   data-gtm-event-label='[[nome-icone]]:[[nome-campanha]]:[[formato]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-icone]] | &#039;baixar&#039;, &#039;whatsapp&#039; e etc | Deve retornar o nome do botão clicado. |
| [[nome-campanha]] | &#039;ta-sabendo-linha-fitness&#039; e etc. | Deve retornar o nome da campanha. |
| [[formato]] | &#039;campanha, arte,  guide ou produtos&#039; | Deve retornar o formato utilizado. |

<br />


**No clique para baixar os guides , no sub menu campanhas, no menu Compartilhe**<br />

- **Onde:** Em compartilhe
    
```html
<div
   data-gtm-event-category='rchlover:compartilhe'
   data-gtm-event-action='clique:guide-shop:icone'
   data-gtm-event-label='baixar:[[nome-guide]]:campanhas'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-guide]] | &#039;casa-raichuelo&#039; e etc | Deve retornar o nome do guide. |

<br />


### Como Funciona

**No clique do botão &quot;Download , no sub menu politicas da campanha, no menu Como Funciona?**<br />

- **Onde:** Em Como funciona
    
```html
<div
   data-gtm-event-category='rchlover:como-funciona'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='download:politicas-da-campanha'
>Botão</div>
```



<br />


**No clique do botão &quot;Download , no sub menu termos de adesão, no menu Como Funciona?**<br />

- **Onde:** Em Como funciona
    
```html
<div
   data-gtm-event-category='rchlover:como-funciona'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='download:termos-de-adesao'
>Botão</div>
```

<br />


### Junte-se a nós

**No clique do botões em atenção**<br />

- **Onde:** Em Junte-se a nós
    
```html
<div
   data-gtm-event-category='rchlover:junte-se-a-nos'
   data-gtm-event-action='clique:atencao:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039; ou &#039;cancelar&#039;. | Deve retornar o nome do botão clicado. |

<br />


**Na interação com os campos**<br />

- **Onde:** Em Junte-se a nós
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:junte-se-a-nos',
    'eventAction': 'intercao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome-completo&#039;, &#039;cpf&#039; e etc | Deve retornar o nome do campo preenchido. |

<br />


**No callback da tentiva de criar conta**<br />

- **Onde:** Em Junte-se a nós
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'rchlover:junte-se-a-nos',
    'eventAction': 'envio:callback',
    'eventLabel': '[[sucesso ou tipo-de-erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;erro:nao-foi-possivel&#039; e etc | Deve retornar a mensagem de sucesso ou o tipo de erro. |

<br />


**No click do link, em verique seu email**<br />

- **Onde:** Em Junte-se a nós
    
```html
<div
   data-gtm-event-category='rchlover:junte-se-a-nos'
   data-gtm-event-action='clique:verifique-seu-email:link'
   data-gtm-event-label='reenviar'
>Botão</div>
```

<br />

**No click do botão acessar, em conta verificada com sucesso**<br />

- **Onde:** Em Junte-se a nós
    
```html
<div
   data-gtm-event-category='rchlover:junte-se-a-nos'
   data-gtm-event-action='clique:conta-verificada:botao'
   data-gtm-event-label='acessar'
>Botão</div>
```

<br />


### Tutoriais

**no click nos vídeos**<br />

- **Onde:** na página de Tutoriais.
    
```html
<div
   data-gtm-event-category='rchlover:tutoriais'
   data-gtm-event-action='interacao:video'
   data-gtm-event-label='[[nome-do-video]]:[[acao-do-video]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-do-video]] | &#039;video-de-onboarding&#039;, &#039;como-resgatar-comissao&#039;, etc | Retorna o nome do vídeo clicado para assistir. |
| [[acao-do-video]] | &#039;play&#039;,  &#039;pause&#039;, &#039;replay&#039;, etc | Deve retornar a açao feita no vídeo. |

<br />


### Meus dados e Comissão

**Na interação com os campos, no sub menu meus dados no menu meus dados**<br />

- **Onde:** Em Comissão e em &quot;Meus dados&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:[[nome-tela]]',
    'eventAction': 'intercao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]:meus-dados'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome-completo&#039;, &#039;cpf&#039; e etc | Deve retornar o nome do campo preenchido. |
| [[nome-tela]] | &#039;comissao&#039; ou &#039;meus-dados&#039; | Deve retornar o nome da tela em que o usuario está, &#039;meus dados&#039; para colaboradores e &#039;comissao&#039; para afiliados. |

<br />


**No clique dos botões, &quot;Salvar&quot; ou &quot;Cancelar&quot;, no sub menu meus dados no menu meus dados**<br />

- **Onde:** Em Comissão e em &quot;Meus dados&quot;
    
```html
<div
   data-gtm-event-category='rchlover:[[nome-tela]]'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]:meus-dados'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;salvar&#039; ou &#039;cancelar&#039;. | Deve retornar o nome do botão clicado. |
| [[nome-tela]] | &#039;comissao&#039; ou &#039;meus-dados&#039; | Deve retornar o nome da tela em que o usuario está, &#039;meus dados&#039; para colaboradores e &#039;comissao&#039; para afiliados. |

<br />


**No clique dos botões, no modal de atenção, no sub menu meus dados no menu meus dados**<br />

- **Onde:** Em Comissão e em &quot;Meus dados&quot;
    
```html
<div
   data-gtm-event-category='rchlover:[[nome-tela]]'
   data-gtm-event-action='clique:botao:modal-atencao'
   data-gtm-event-label='[[nome-botao]]:meus-dados'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;salvar&#039; ou &#039;editar&#039;. | Deve retornar o nome do botão clicado. |
| [[nome-tela]] | &#039;comissao&#039; ou &#039;meus-dados&#039; | Deve retornar o nome da tela em que o usuario está, &#039;meus dados&#039; para colaboradores e &#039;comissao&#039; para afiliados. |

<br />


**Na tentiva de callback para enviar os dados, no sub menu meus dados no menu meus dados**<br />

- **Onde:** Em Comissão
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'rchlover:comissao',
    'eventAction': 'envio:callback',
    'eventLabel': '[[sucesso ou tipo-de-erro]]:meus-dados'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;erro:dados-invalidos&#039; e etc | Deve retornar a mensagem de sucesso ou o tipo de erro. |

<br />


**No clique do link &quot;ajuda&quot;, no sub menu meus dados no menu meus dados**<br />

- **Onde:** Em Comissão
    
```html
<div
   data-gtm-event-category='rchlover:comissao'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='ajuda:meus-dados'
>Botão</div>
```

<br />


**No clique em &quot;download&quot;, no sub menu financeiro no menu meus dados**<br />

- **Onde:** Em Comissão
    
```html
<div
   data-gtm-event-category='rchlover:comissao'
   data-gtm-event-action='clique:botao:[[nome-demonstrativo]]'
   data-gtm-event-label='downloas:financeiro'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-demonstrativo]] | &#039;demonstrativo-comissao-agosto&#039; e etc | Deve retornar o nome do demonstrativo. |

<br />


**Na interação com o checkbox no modal de atenção, no sub menu financeiro no menu meus dados**<br />

- **Onde:** Em Comissão
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:comissao',
    'eventAction': 'interacao:checkbox:modal-atencao',
    'eventLabel': '[[nome-demonstrativo]]:sim-concordo:financeiro'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-demonstrativo]] | &#039;demonstrativo-comissao-agosto&#039; e etc | Deve retornar o nome do demonstrativo. |

<br />


**No clique do link ou botão no modal de atenção, no sub menu financeiro no menu meus dados**<br />

- **Onde:** Em Comissão
    
```html
<div
   data-gtm-event-category='rchlover:comissao'
   data-gtm-event-action='clique:[[botao ou link]]:modal-atencao'
   data-gtm-event-label='[[nome-demonstrativo]]:[[nome-item]]:financeiro'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao ou link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o tipo de elemento clicado. |
| [[nome-demonstrativo]] | &#039;demonstrativo-comissao-agosto&#039; e etc | Deve retornar o nome do demonstrativo. |
| [[nome-item]] | &#039;ajuda&#039;, &#039;confirmar&#039; | Deve retornar o nome do item clicado. |

<br />


**Na interação com o filtro de Status**<br />

- **Onde:** Em Meus pedidos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:pedidos',
    'eventAction': 'interacao:filtro',
    'eventLabel': '[[status]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | status-cancelado, status-aguardando-faturamento, etc | Deve retornar o status selecionado |

<br />


**Na interação com o filtro de Data**<br />

- **Onde:** Em Meus pedidos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:pedidos',
    'eventAction': 'interacao:filtro',
    'eventLabel': 'data'
  });
</script>
```

<br />


**Na interação com os checkboxs em &quot;Dados de contato&quot;**<br />

- **Onde:** Em &quot;Meus dados&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:meus-dados',
    'eventAction': 'interacao:checkbox:dados-de-contato',
    'eventLabel': '[[item-selecionado]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[item-selecionado]] | &#039;concordo-com-uso-dos-dados-para-contato&#039; ou  &#039;deixar-de-fornecer-meus-dados-para-contato&#039; | Deve retornar a ideia generica do checkbox selecionado. |

<br />

### Exclusão de Conta

**Na interação com o botão de exclusão de conta**<br />

- **Onde:** Em &quot;Dados de Contato&quot;, dentro do menu &quot;Minha Conta&quot;, no componente de &quot;Excluir Conta&#039;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-conta',
    'eventAction': 'clique:botao',
    'eventLabel': 'excluir-conta:[[passo]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[passo] | &#039;step-1&#039;, &#039;step-2&#039;. | Deve retortar se é o primeiro ou segundo botão &quot;Excluir minha conta&quot; do processo de exclusão. |

<br />


**Na interação com o checkbox de confimação de exclusão de conta**<br />

- **Onde:** Em &quot;Dados de Contato&quot;, dentro do menu &quot;Minha Conta&quot;, no componente de &quot;Excluir Conta&#039;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-conta',
    'eventAction': 'interacao:checkbox:[[selecionou-desselecionou]]',
    'eventLabel': 'excluir-conta'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[selecionou-desselecionou]] | 'selecionou' ou 'desselecionou' | Deve retornar se o checkbox foi selecionado ou desselecionado. |

<br />


**Na interação com os botões do modal de exclusão**<br />

- **Onde:** No modal de Exclusão
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-conta',
    'eventAction': 'clique:botao:modal-motivo-exclusao',
    'eventLabel': '[[botao]]:[[motivo]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao]] | &#039;fechar&#039;, &#039;cancelar&#039;, &#039;continuar&#039;, &#039;confirmar&#039;, etc. | Deve retornar o nome do botão ou link clicado no modal de exclusão. |
| [[motivo]] | &#039;nao-gostei-da-comeissao&#039;, &#039;nao-consigo-vender&#039;, &#039;nao-gostei-da-plataforma&#039;, &#039;outro&#039;. | Quando clicar em continuar deve trazer o motivo selecionado da exclusão. |

<br />


**Na interação com os botões do modal de confirmação de exclusão**<br />

- **Onde:** No modal de Exclusão
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-conta',
    'eventAction': 'clique:botao:modal-confirma-exclusao',
    'eventLabel': '[[botao-link]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-link]] | &#039;fechar&#039;, &#039;cancelar&#039;, &#039;clique-aqui&#039;, &#039;confirmar&#039;, &#039;finalizar&#039;, etc. | Deve retornar o nome do botão ou link clicado no modal de exclusão. |

<br />


**Quando o usuário tentar acessar a conta depois de ter solicitado a exclusão da mesma**<br />

- **Onde:** No modal de  Alerta da Exclusão
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:minha-conta',
    'eventAction': 'clique:botao:[[modal-tipo]]',
    'eventLabel': '[[botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao]] | &#039;finalizar&#039;. | Deve retornar o nome do botão clicado no modal de alerta de exclusão. |
| [[modal-tipo]] | &#039;modal-conta-excluida-em-ate-24-horas&#039;, &#039;modal-aguarde-processamento-exclusao&#039;. | Retornar o tipo de alerta de exclussão. |

<br />

### FAQ

**Ao abrir ou fechar alguma pergunta, no sub menu Dúvidas Frequentes no menu Ajuda**<br />

- **Onde:** Em Dúvidas Frequentes
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'rchlover:faq',
    'eventAction': 'intercao:pergunta',
    'eventLabel': '[[nome-pergunta]]:duvidas-frequentes:[[acao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-pergunta]] | &#039;1-quem-pode-participar-da-campanha&#039; e etc | Deve retornar o nome da pergunta que teve interação. |
| [[acao]] | &#039;abrir&#039; ou &#039;fechar&#039; | Deve retornar a ação do usuário. |

<br />


**No clique do link &quot;Clicando aqui&quot;, em &quot;Não encontrou a solução para o seu problema ou dúvida?&quot;, no sub menu Dúvidas Frequentes no menu Ajuda**<br />

- **Onde:** Em Dúvidas Frequentes
    
```html
<div
   data-gtm-event-category='rchlover:faq'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='clicando-aqui:nao-encontrou-pergunta'
>Botão</div>
```

<br />


### Enhanced E-commerce

**Na visualização de algum banner**<br />

- **Onde:** Na página home
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'promotionImpression',
    'eventCategory': 'rchlover:enhanced-ecommerce',
    'eventAction': 'promotionImpression',
    'noInteraction': '1',
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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[id-promocao]] | &quot;banner123&quot; | ID único do Banner |
| [[nome-promocao]] | &quot;o-primeiro-colocado-ganha-uma-honda-biz-0km&quot; | Nome amigável do banner |
| [[posicao-promocao]] | &quot;1&quot; | Posição que o banner é exibido  |
| [[arte-banner]] | &quot;https://www.sourchlover.com.br/imagem.jpg&quot; | URL da imagem do banner |

<br />


**No clique do banner**<br />

- **Onde:** Na página home
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'promotionClick',
  'eventCategory': 'rchlover:enhanced-ecommerce',
  'eventAction': 'promotionClick',
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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[id-promocao]] | &quot;banner123&quot; | ID único do Banner |
| [[nome-promocao]] | &quot;o-primeiro-colocado-ganha-uma-honda-biz-0km&quot; | Nome amigável do banner |
| [[posicao-promocao]] | &quot;1&quot; | Posição que o banner é exibido  |
| [[arte-banner]] | &quot;https://www.sourchlover.com.br/imagem.jpg&quot; | URL da imagem do banner |

<br />


**Na visualização de uma vitrine de produtos**<br />

- **Onde:** Em todas as páginas que exibirem uma lista de produtos
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productImpressions',
  'eventCategory':'rchlover:enhanced-ecommerce',
  'eventAction': 'productImpression',
  'noInteraction': '1',
  'ecommerce': {
    'impressions': [{
      'dimension7': '[[dco-product-codsubcategoria]]',
      'dimension8': '[[product-padronagemdoproduto]]',
      'dimension9': '[[product-subcategoria]]',
      'dimension10': '[[product-lifestyle]]',
      'dimension11': '[[product-gender]]',
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
| [[dco-product-codsubcategoria]] | 135&#039; | Código da subcategoria |
| [[product-padronagemdoproduto]] | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] | 109093&#039; | Código da categoria GM  |
| [[product-lifestyle]] | &#039;casual&#039;&#039;, &#039;&#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;&#039;, &#039;&#039;feminino&#039; | Genero do produto |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento e Categoria do produto |
| [[lista-produto]] | &quot;calcas-jeans&quot; | Nome da lista que o produto aparece |
| [[posicao-produto]] | &quot;2&quot; | Posição que o produto aparece em uma lista de produtos |

<br />


**Ao clicar no botão &quot;gerar link&quot;, &quot;Adicionar produto&quot;, &quot;comprar&quot; do produto.**<br />

- **Onde:** Em todas as páginas que exibirem uma lista de produtos
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': 'productClick',
    'eventCategory': 'rchlover:enhanced-ecommerce',
    'eventAction': 'productClick',
    'ecommerce': {
      'click': {
        'actionField': {'list': '[[lista-produto]]'},
        'products': [{
          'dimension7': '[[dco-product-codsubcategoria]]',
          'dimension8': '[[product-padronagemdoproduto]]',
          'dimension9': '[[product-subcategoria]]',
          'dimension10': '[[product-lifestyle]]',
          'dimension11': '[[product-gender]]',
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
| [[dco-product-codsubcategoria]] | 135&#039; | Código da subcategoria |
| [[product-padronagemdoproduto]] | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] | 109093&#039; | Código da categoria GM  |
| [[product-lifestyle]] | &#039;casual&#039;&#039;, &#039;&#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;&#039;, &#039;&#039;feminino&#039; | Genero do produto |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento e Categoria do produto |
| [[lista-produto]] | &quot;calcas-jeans&quot; | Nome da lista que o produto aparece |
| [[posicao-produto]] | &quot;2&quot; | Posição que o produto aparece em uma lista de produtos |

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
