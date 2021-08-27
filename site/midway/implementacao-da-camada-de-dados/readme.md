![Riachuelo](https://www.riachuelo.com.br/static/version1623357894/frontend/Corra/webjump/pt_BR/images/logo.svg)

> Área - Data Analytics<br />
> Documento de Especificação Técnica

<br />

## Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Dimensões Globais](#dimens&#245;es-globais)
- [Geral - Cliente não logado](#geral-cliente-n&#227;o-logado)
- [Home](#home)
- [Cartões](#cart&#245;es)
- [Cartões - Cartão RCHLO](#cart&#245;es-cart&#227;o-rchlo)
- [Cartões - Visa e Mastercard](#cart&#245;es-visa-e-mastercard)
- [Parceiros](#parceiros)
- [Produtos Financeiros](#produtos-financeiros)
- [Atendimento](#atendimento)
- [Faça Parte](#fa&#231;a-parte)
- [Enhanced E-commerce](#enhanced-e-commerce)
- [Login](#login)
- [Area logada](#area-logada)
- [Quitação de Dívida](#quita&#231;&#227;o-de-d&#237;vida)
- [Simulação de Empréstimo](#simula&#231;&#227;o-de-empr&#233;stimo)

## Implementação da Camada de dados - Projeto Midway Site
Última atualização: 27/08/2021 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [http://www.midwayfinanceira.com.br/](http://www.midwayfinanceira.com.br/).

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-52SM6DD');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-52SM6DD" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
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
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[userid]] | &quot;01234&quot; | ID único de usuário defeinido após o cadastro |

---


### Geral - Cliente não logado


**No clique dos itens do menu e sub-menu**<br />

- **Onde:** Em todas as páginas do site.
    
```html
<div
   data-gtm-event-category='midway:geral'
   data-gtm-event-action='clique:menu-e-sub-menu'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;midway&#039;, &#039;cartoes&#039;, &#039;produtos-financeiros&#039;, &#039;nossa-historia&#039;, &#039;emprestimo&#039; e etc. | Deve retornar o nome clicado. |

<br />


**No clique dos itens do header**<br />

- **Onde:** Em todas as páginas do site.
    
```html
<div
   data-gtm-event-category='midway:geral'
   data-gtm-event-action='clique:header'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;logo&#039;, &#039;acesso-rchlo&#039;. | Deve retornar o nome clicado. |

<br />


**No clique dos itens do footer**<br />

- **Onde:** Em todas as páginas do site.
    
```html
<div
   data-gtm-event-category='midway:geral'
   data-gtm-event-action='clique:footer'
   data-gtm-event-label='[[secao]]:[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[secao]] | &#039;central-de-relacionamento&#039;, &#039;redes-sociais&#039;, &#039;baixe-o-app&#039;, &#039;transparencia-pra-voce&#039;. | Deve retornar o nome da seção. |
| [[nome-item]] | &#039;duvidas-frequentes&#039;, &#039;instagram&#039;, &#039;app-store&#039;, &#039;demonstracoes-financeiras&#039; e etc. | Deve retornar o nome clicado. |

<br />


**No clique do ícone &quot;Precisa de ajuda?&quot;.**<br />

- **Onde:** Em todas as páginas do site.
    
```html
<div
   data-gtm-event-category='midway:geral'
   data-gtm-event-action='clique:icone'
   data-gtm-event-label='precisa-de-ajuda'
>Botão</div>
```



<br />


**Na interação com o chat &quot;Precisa de ajuda?&quot;.**<br />

- **Onde:** Em todas as páginas do site.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:geral',
    'eventAction': 'clique:chat',
    'eventLabel': 'botao:[[nome-botao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039; ou &#039;iniciar&#039;. | Deve retornar o nome do botão clicado. |

<br />


### Home



**No clique dos cards do carrosel na seção &quot;Para você!&quot;**<br />

- **Onde:** Na página home.
    
```html
<div
   data-gtm-event-category='midway:home'
   data-gtm-event-action='clique:[[botao]]'
   data-gtm-event-label='card:[[nome-card]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao]] | &#039;conheca&#039;, &#039;clique-aqui&#039; e etc. | Deve retornar o nome do botão clicado |
| [[nome-card]] | &#039;seguros&#039;, &#039;assistencias&#039; e etc. | Deve retornar o nome do card clicado. |

<br />

**No clique para baixar o app na seção &quot;Nosso Aplicativo&quot;**<br />

- **Onde:** Na página home.
    
```html
<div
   data-gtm-event-category='midway:home'
   data-gtm-event-action='clique:nosso-aplicativo'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;app-store&#039; ou &#039;google-play&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique do botão da ultima seção**<br />

- **Onde:** Na página home.
    
```html
<div
   data-gtm-event-category='midway:home'
   data-gtm-event-action='clique:exemplo-secao'
   data-gtm-event-label='botao:exemplo'
>Botão</div>
```



<br />


**No clique dos cards da ultima seção.**<br />

- **Onde:** Na página home.
    
```html
<div
   data-gtm-event-category='midway:home'
   data-gtm-event-action='clique:exemplo-secao'
   data-gtm-event-label='card:[[nome-card]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;nome-card&#039; e etc. | Deve retornar o nome do card clicado. |

<br />


**No clique em Anterior ou Próximo, na ultima seção.**<br />

- **Onde:** Na página home.
    
```html
<div
   data-gtm-event-category='midway:home'
   data-gtm-event-action='clique:exemplo-secao'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;anterior&#039; ou &#039;proximo&#039;. | Deve retornar o nome do botão clicado. |

<br />


### Cartões

**No clique dos botões &quot;Saiba Mais&quot; , no menu &quot;Cartões&quot;.**<br />

- **Onde:** No página de Cartões.
    
```html
<div
   data-gtm-event-category='midway:cartoes'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='saiba-mais:[[secao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[secao]] | &#039;cartao-riachuelo&#039;, &#039;cartao-riachuelo-visa-e-mastercard&#039;. | Deve retornar o nome da seção. |

<br />


**No clique dos botões da área &quot;Baixe o app&quot;**<br />

- **Onde:** Na página de Cartões
    
```html
<div
   data-gtm-event-category='midway:cartoes'
   data-gtm-event-action='clique:baixe-o-app'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;app-store&#039; ou &#039;google-play&#039;, &#039;lojas-rchlo&#039;. | Deve retornar o nome do botão clicado. |

<br />

### Cartões - Cartão RCHLO


**No clique do botão &quot;Consulte taxas e tarifas&quot; , da seção &quot;Cartão Riachuelo Visa e MasterCard&quot;.**<br />

- **Onde:** Na página de Cartão Riachuelo.
    
```html
<div
   data-gtm-event-category='midway:cartao-riachuelo'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='consulte-taxas-e-tarifas:cartao-riachuelo'
>Botão</div>
```



<br />

**No clique dos botões da área &quot;Baixe o app&quot;**<br />

- **Onde:** Na página de Cartão Riachuelo.
    
```html
<div
   data-gtm-event-category='midway:cartao-riachuelo'
   data-gtm-event-action='clique:baixe-o-app'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;app-store&#039; ou &#039;google-play&#039;, &#039;lojas-rchlo&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique para fechar o modal &quot;Tarifas&quot;**<br />

- **Onde:** Na página de Cartão Riachuelo.
    
```html
<div
   data-gtm-event-category='midway:cartao-riachuelo'
   data-gtm-event-action='clique:modal'
   data-gtm-event-label='botao:fechar'
>Botão</div>
```



<br />


### Cartões - Visa e Mastercard


**No clique do botão &quot;Consulte taxas e tarifas&quot; , da seção &quot;Cartão Riachuelo Visa e MasterCard&quot;.**<br />

- **Onde:** Na página de Cartão Riachuelo Visa e Mastercard.
    
```html
<div
   data-gtm-event-category='midway:cartao-riachuelo-visa-mastercard'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='consulte-taxas-e-tarifas:cartao-riachuelo-visa-e-mastercard'
>Botão</div>
```



**No clique dos botões da área &quot;Baixe o app&quot;**<br />

- **Onde:** Na página de Cartão Riachuelo Visa e Mastercard.
    
```html
<div
   data-gtm-event-category='midway:cartao-riachuelo-visa-mastercard'
   data-gtm-event-action='clique:baixe-o-app'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;app-store&#039; ou &#039;google-play&#039;, &#039;lojas-rchlo&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique para fechar o modal &quot;Tarifas&quot;**<br />

- **Onde:** Na página de Cartão Riachuelo Visa e Mastercard.
    
```html
<div
   data-gtm-event-category='midway:cartao-riachuelo-visa-mastercard'
   data-gtm-event-action='clique:modal'
   data-gtm-event-label='botao:fechar'
>Botão</div>
```



<br />


### Parceiros

**No clique dos cards dos parceiros**<br />

- **Onde:** Na página Parceiros.

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:parceiros',
    'eventAction': 'clique:[[botao]]',
    'eventLabel': 'card:[[nome-do-parceiro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao]] | &#039;conheca&#039;, &#039;clique-aqui&#039;, e etc. | Deve retornar o nome do botão clicado.|
| [[nome-do-parceiro]] | &#039;estrela&#039;, &#039;shopclub&#039;,  &#039;marabraz &#039; e etc. | Deve retornar o nome do botão clicado.|


<br />

### Produtos Financeiros


**No clique dos botões &quot;Saiba Mais&quot;  no carrossel &quot;Seguros e Serviços&quot;**<br />

- **Onde:** No página Produtos Financeiros.
    
```html
<div
   data-gtm-event-category='midway:produtos-financeiros'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='saiba-mais:card:[[nome-card]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;seguros&#039;, &#039;assistencias&#039;, e etc. | Deve retornar o nome do card. |

<br />


**No clique dos cards no ultimo carrossel**<br />

- **Onde:** No página Produtos Financeiros.
    
```html
<div
   data-gtm-event-category='midway:produtos-financeiros'
   data-gtm-event-action='clique:carrossel:card'
   data-gtm-event-label='[[nome-card]]:[[indice]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;nome-card&#039; e etc. | Deve retornar o nome do card. |
| [[indice]] | &#039;1&#039;, 2&#039;, &#039;3&#039; etc. | Deve retornar a posição clicada do carrossel. |

<br />


**No clique das abas da seção &quot;Seguros e serviços&quot;**<br />

- **Onde:** No página Produtos Financeiros.
    
```html
<div
   data-gtm-event-category='midway:produtos-financeiros'
   data-gtm-event-action='clique:aba'
   data-gtm-event-label='[[nome-aba]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-aba]] | &#039;assistencia-24-horas-moto-premiavel-super&#039;, &#039;&#039;assistencia-24-horas-moto-premiavel-mega&#039; | Deve retornar o nome da aba clicada. |

<br />


**No clique dos botões das abas**<br />

- **Onde:** No página Produtos Financeiros.
    
```html
<div
   data-gtm-event-category='midway:produtos-financeiros'
   data-gtm-event-action='clique:aba'
   data-gtm-event-label='[[nome-aba]]:botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-aba]] | &#039;assistencia-24-horas-moto-premiavel-super&#039;, &#039;&#039;assistencia-24-horas-moto-premiavel-mega&#039;. | Deve retornar o nome da aba clicada. |
| [[nome-botao]] | &#039;lojas-rchlo&#039;, &#039;central-de-relacionamento&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique do link nas abas.**<br />

- **Onde:** No página Produtos Financeiros.
    
```html
<div
   data-gtm-event-category='midway:produtos-financeiros'
   data-gtm-event-action='clique:aba'
   data-gtm-event-label='[[nome-aba]]:link:[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-aba]] | &#039;assistencia-24-horas-moto-premiavel-super&#039;, &#039;&#039;assistencia-24-horas-moto-premiavel-mega&#039;. | Deve retornar o nome da aba clicada. |
| [[nome-link]] | &#039;condicoes-gerais-da-assistencia-24-horas-moto-premiavel-super&#039;, &#039;condicoes-gerais-da-assistencia-24-horas-moto-premiavel-mega&#039; | Deve retornar o nome do link clicado. |

<br />


**No clique para fechar o modal &quot;Condições Gerais&quot;**<br />

- **Onde:** No página Produtos Financeiros.
    
```html
<div
   data-gtm-event-category='midway:produtos-financeiros'
   data-gtm-event-action='clique:modal'
   data-gtm-event-label='botao:fechar'
>Botão</div>
```

**No clique nos links e botões, que não são cards**<br />

- **Onde:** No página Produtos Financeiros.
    
```html
<div
   data-gtm-event-category='midway:produtos-financeiros'
   data-gtm-event-action='clique:[[elemento]]'
   data-gtm-event-label='[[nome-do-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[elemento]] | &#039;botao&#039;, &#039;&#039;link&#039;. | Deve retornar o tipo de elemento clicado. |
| [[nome-do-botao]] | &#039;acompanhe-os-sorteados-do-mes&#039;, &#039;saiba-mais&#039; | Deve retornar o nome do link clicado. |

<br />


### Atendimento


**No clique dos links da página.**<br />

- **Onde:** Na página de Atendimento.
    
```html
<div
   data-gtm-event-category='midway:atendimento'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;duvidas-frequentes&#039;, &#039;clicando-aqui&#039;. | Deve retornar o nome do link clicado. |

<br />


**No clique dos botões na seção &quot;Siga nas nossas redes&quot;.**<br />

- **Onde:** No página de Atendimento.
    
```html
<div
   data-gtm-event-category='midway:atendimento'
   data-gtm-event-action='clique:nossas-redes'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;instagram&#039;, &#039;facebook&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />


### Faça Parte

**No clique dos botões da seção &quot;Trabalhe com a gente&quot;**<br />

- **Onde:** No página Faça Parte.
    
```html
<div
   data-gtm-event-category='midway:faca-parte'
   data-gtm-event-action='clique:trabalhe-com-a-gente'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;cadastre-se&#039;, &#039;conheca-a-midway&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique dos botões da seção &quot;Seja uma startup Parceira&quot;**<br />

- **Onde:** No página Faça Parte.
    
```html
<div
   data-gtm-event-category='midway:faca-parte'
   data-gtm-event-action='clique:startup-parceira'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;saiba-mais-sobre-o-programa&#039;, &#039;cadastre-sua-startup&#039;. | Deve retornar o nome do botão clicado. |

<br />


**Na interação com o formulário &quot;Trabalhe com a gente&quot;**<br />

- **Onde:** No página Faça Parte.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:faca-parte',
    'eventAction': 'interacao:formulario-trabalhe-com-a-gente',
    'eventLabel': 'preencheu:campo:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome&#039;, &#039;telefone&#039; e etc. | Deve retornar o nome do campo preenchido. |

<br />


**No clique do botão da seção &quot;Tem uma startup?&quot;**<br />

- **Onde:** No página Faça Parte.
    
```html
<div
   data-gtm-event-category='midway:faca-parte'
   data-gtm-event-action='clique:tem-uma-startup'
   data-gtm-event-label='botao:cadastre-sua-startup'
>Botão</div>
```



<br />


**Na interação com o formulário &quot;Tem uma startup?&quot;**<br />

- **Onde:** No página Faça Parte.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:faca-parte',
    'eventAction': 'interacao:formulario-tem-uma-startup',
    'eventLabel': 'preencheu:campo:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome&#039;, &#039;telefone&#039; e etc. | Deve retornar o nome do campo preenchido. |

<br />


**No callback da tentiva de envio dos formulários**<br />

- **Onde:** No página Faça Parte.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'midway:faca-parte',
    'eventAction': 'envio:formulario',
    'eventLabel': '[[sucesso ou tipo-do-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-do-erro]] | &#039;sucesso&#039;, &#039;erro:campo-preenchido-errado&#039;, &#039;erro:falta-preencher-campo&#039;. | Deve retornar a mensagem de sucesso ou o tipo do erro. |

<br />


### Enhanced E-commerce

**Na visualização dos banners**<br />

- **Onde:** Em todas as páginas que estiver disponivel
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'promotionImpression',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'promotionImpression',
    'ecommerce': {
    'promoView': {
      'promotions': [{
        'id': '[[id-promocao]]',
        'name': '[[nome-promocao]]',
        'position': '[[posicao-promocao]]'
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
| [[nome-promocao]] | &quot;nome-banner&quot; | Nome amigável do banner |
| [[posicao-promocao]] | &quot;1&quot; | Posição que o banner é exibido |
| [[arte-banner]] | https://xd.adobe.com/view/5319cd27-2f6a-4f0e-54c9-ff003aab8346-2528/screen/f3374df0-bd57-43f2-8d02-4783b0360b7c/Home-menu-extendido?fullscreen | URL da imagem do banner |

<br />


**No clique em um banner**<br />

- **Onde:** Em todas as páginas que estiver disponivel
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'promotionClick',
  'eventCategory': 'enhanced-ecommerce',
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
| [[nome-promocao]] | &quot;nome-banner&quot; | Nome amigável do banner |
| [[posicao-promocao]] | &quot;1&quot; | Posição que o banner é exibido |
| [[arte-banner]] | https://xd.adobe.com/view/5319cd27-2f6a-4f0e-54c9-ff003aab8346-2528/screen/f3374df0-bd57-43f2-8d02-4783b0360b7c/Home-menu-extendido?fullscreen | URL da imagem do banner |

<br />


### Login

**Na interação com os campos de &quot;Acesso cartões RCHLO&quot;**<br />

- **Onde:** No modal de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:login',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;cpf&#039; ou &#039;senha&#039;. | Deve retornar o nome do campo preenchido. |

<br />


**No clique dos botões e link de &quot;Acesso cartões RCHLO&quot;**<br />

- **Onde:** No modal de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:login',
    'eventAction': 'clique:[[elemento]]',
    'eventLabel': '[[nome-elemento]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[elemento]] | &#039;botao&#039; ou &#039;link&#039;. | Deve retornar o elemento. |
| [[nome-elemento]] | &#039;esqueceu-a-senha&#039;, &#039;continuar&#039;, &#039;primeiro-acesso&#039;. | Deve retornar o nome do elemento clicado. |

<br />


**Na interação com os campos &quot;Esqueci Minha Senha&quot;**<br />

- **Onde:** No modal de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:login',
    'eventAction': 'interacao:esqueci-minha-senha',
    'eventLabel': 'preencheu:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;cpf&#039;, &#039;email&#039;, &#039;celular&#039; e etc. | Deve retornar o nome do campo preenchido. |

<br />


**Ao selecionar os campos &quot;Esqueci Minha Senha&quot;**<br />

- **Onde:** No modal de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:login',
    'eventAction': 'interacao:esqueci-minha-senha',
    'eventLabel': 'selecionou:[[nome-campo]]:[[valor-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;selecione-o-tipo-do-seu-cartao&#039;. | Deve retornar o nome do campo preenchido. |
| [[valor-campo]] | &#039;riachuelo&#039;, &#039;riachuelo-visa-ou-mastercard&#039;. | Deve retornar o valor do campo selecionado. |

<br />


**No clique dos botões &quot;Esqueci Minha Senha&quot;**<br />

- **Onde:** No modal de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:login',
    'eventAction': 'clique:esqueci-minha-senha',
    'eventLabel': 'botao:[[nome-botao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039; ou &#039;fechar&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No callback de erro ou sucesso de esqueci minha senha**<br />

- **Onde:** No modal de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'midway:login',
    'eventAction': 'envio:callback-esqueci-minhas-senha',
    'eventLabel': '[[sucesso ou tipo-de-erro]]',
    'dimension1': '[[User ID]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;nao-foi-possivel&#039; e etc. | Deve retornar sucesso ou o tipo de erro. |
| [[userid]] | &quot;01234&quot; | ID único de usuário defeinido após o cadastro |

<br />


**No callback de erro ou sucesso de login**<br />

- **Onde:** No modal de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'login',
    'eventCategory': 'midway:login',
    'eventAction': 'envio:callback-login',
    'eventLabel': '[[sucesso ou tipo-de-erro]]',
    'dimension1': '[[User ID]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;nao-foi-possivel&#039; e etc. | Deve retornar sucesso ou o tipo de erro. |
| [[userid]] | &quot;01234&quot; | ID único de usuário defeinido após o cadastro |

<br />


**Na interação com os campos &quot;Primeiro Acesso ao conta online cartões Rchlo&quot;**<br />

- **Onde:** No modal de primeiro acesso..
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:login',
    'eventAction': 'interacao:primeiro-acesso',
    'eventLabel': 'preencheu:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;cpf&#039;, &#039;email&#039;, &#039;celular&#039; e etc. | Deve retornar o nome do campo preenchido. |

<br />


**Ao selecionar os campos &quot;Primeiro Acesso ao conta online cartões Rchlo&quot;**<br />

- **Onde:** No modal de primeiro acesso..
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:login',
    'eventAction': 'interacao:primeiro-acesso',
    'eventLabel': 'selecionou:[[nome-campo]]:[[valor-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;selecione-o-tipo-do-seu-cartao&#039;. | Deve retornar o nome do campo preenchido. |
| [[valor-campo]] | &#039;riachuelo&#039;, &#039;riachuelo-visa-ou-mastercard&#039;. | Deve retornar o valor do campo selecionado. |

<br />


**No clique dos botões &quot;Primeiro Acesso ao conta online cartões Rchlo&quot;**<br />

- **Onde:** No modal de primeiro acesso..
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:login',
    'eventAction': 'interacao:primeiro-acesso',
    'eventLabel': 'botao:[[nome-botao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039; ou &#039;fechar&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No callback do formulário de Primeiro Acesso**<br />

- **Onde:** No modal de primeiro acesso.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:login',
    'eventAction': 'envio:callback-primeiro-acesso',
    'eventLabel': '[[sucesso ou tipo-de-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;nao-foi-possivel&#039; e etc. | Deve retornar sucesso ou o tipo de erro. |

<br />


### Area logada

**No clique dos itens do header.**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:header'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;compre-online&#039;, &#039;perfil&#039;, &#039;configuracoes&#039;. | Deve retornar o nome do item clicado. |

<br />


**No clique dos itens do footer.**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:footer'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;a-midway&#039;, &#039;cartoes-riachuelo&#039;, &#039;seguros-e-servicos&#039; e etc. | Deve retornar o nome do item clicado. |

<br />


**Na interação para abrir ou fechar o modal do perfil.**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:logada',
    'eventAction': 'clique:modal-perfil',
    'eventLabel': '[[acao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[acao]] | &#039;abrir&#039; ou &#039;fechar&#039; | Deve retornar a ação do usuário. |

<br />


**No clique dos itens do modal do perfil.**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-perfil'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;ultimas-transacoes&#039;, &#039;alterar-senha-eletronica&#039;, &#039;fatura-por-email&#039;, &#039;sair&#039;. | Deve retornar o nome do item clicado. |

<br />


**No clique do botão &quot; Atendimento&quot;, abaixo do header.**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='atendimento'
>Botão</div>
```



<br />


**No clique do botão &quot; fechar&quot;, no modal de atendimento dos cartões**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-atendimento'
   data-gtm-event-label='fechar'
>Botão</div>
```



<br />


**Na interação com o modal &quot;Alterar senha Eletrônica&quot;**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:logada',
    'eventAction': 'interacao:modal-alterar-senha-eletronica',
    'eventLabel': 'preencheu:campo:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;insira-sua-senha-atual&#039;, &#039;crie-sua-nova-senha&#039; e etc. | Deve retornar o nome do campo preenchido. |

<br />


**No clique do botão &quot;Alterar senha&quot; no modal &quot;Alterar senha Eletrônica&quot;**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-alterar-senha-eletronica'
   data-gtm-event-label='botao:alterar-senha'
>Botão</div>
```



<br />


**No callback da tentiva &#039;Alterar senha Eletrônica&quot;**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'midway:logada',
    'eventAction': 'envio:callback-alterar-senha-eletronica',
    'eventLabel': '[[sucesso ou tipo-do-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-do-erro]] | &#039;sucesso&#039;, &#039;nao-foi-possivel&#039; e etc. | Deve retornar a mensagem da tentiva de envio do callback. |

<br />


**No clique no menu**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:menu'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;cartoes&#039;, &#039;faturas&#039;, &#039;seguros&#039; e etc. | Deve retornar o nome do clicado do menu. |

<br />


**No clique do botão &quot;Compre Online&quot;**<br />

- **Onde:** No menu cartões
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:menu'
   data-gtm-event-label='cartoes:botao:compre-online'
>Botão</div>
```



<br />


**No clique do botões da seção &quot;Ultimas transações&quot;**<br />

- **Onde:** No menu cartões
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:ultimas-transacoes'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fatura-por-email&#039;, &#039;declaracoes-anuais&#039;, &#039;ver-fatura-detalhada&#039;. | Deve retornar o nome do botão clicado. |

<br />


**Na interação para selecionar um filtro &quot;Escolha o período desejado&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:logada',
    'eventAction': 'interacao:menu:faturas',
    'eventLabel': 'filtro:escolha-o-periodo-desejado:[[valor-filtro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor-filtro]] | &#039;ultimos-12-meses&#039; e etc. | Deve retornar o valor do filtro. |

<br />


**No clique para selecionar faturas na seção &quot;Historico de faturas&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:historico-de-faturas'
   data-gtm-event-label='[[nome-fatura]]:[[acao-fatura]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fatura]] | &#039;fatura-atual-janeiro-2020&#039;, &#039;fatura-anterior-dezembro-2019&#039;, &#039;fatura-futura-fevereiro-2020&#039; e etc. | Deve retornar o nome da faturada clicada. |
| [[acao-fatura]] | &#039;aberta&#039; ou &#039;fechada&#039;. | Deve retornar a ação da fatura. |

<br />


**Ao selecionar os checkbox na seção &quot;Historico de faturas&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:logada',
    'eventAction': 'interacao:historico-de-faturas:checkbox',
    'eventLabel': '[[nome-checkbox]]:[[nome-fatura]]:[[acao-fatura]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-checkbox]] | &#039;titular&#039; ou &#039;dependentes&#039;. | Deve retornar o nome do checkbox selecionado. |
| [[nome-fatura]] | &#039;fatura-atual-janeiro-2020&#039;, &#039;fatura-anterior-dezembro-2019&#039;, &#039;fatura-futura-fevereiro-2020&#039; e etc. | Deve retornar o nome da faturada clicada. |
| [[acao-fatura]] | &#039;aberta&#039; ou &#039;fechada&#039;. | Deve retornar a ação da fatura. |

<br />


**No clique dos botões seção &quot;Historico de faturas&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:historico-de-faturas'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fatura-em-pdf&#039;, &#039;gerar-boleto&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique dos botões no modal ao clicar em &quot;fatura em pdf&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-fatura-em-pdf'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;, &#039;imprimir&#039; , &#039;download&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique dos botões no modal ao clicar em &quot;gerar boleto&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-opcoes-de-pagamento'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;, &#039;gerar-boleto&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique dos ícones de informações no modal ao clicar em &quot;gerar boleto&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-opcoes-de-pagamento'
   data-gtm-event-label='icone:informacoes:[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;pagamento-total&#039;, &#039;pagamento-minimo&#039; e etc. | Deve retornar o nome do item. |

<br />


**No clique do link &quot;consulte e condições e taxas aqui&quot; no modal ao clicar em &quot;gerar boleto&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-opcoes-de-pagamento'
   data-gtm-event-label='link:consulte-condicoes-e-taxas-aqui'
>Botão</div>
```



<br />


**Ao selecionar a opção de pagamento no modal &quot;gerar boleto&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:logada',
    'eventAction': 'clique:modal-opcoes-de-pagamento',
    'eventLabel': '[[opcao-pagamento]]:[[nome-opcao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-pagamento]] | &#039;pagamento-total&#039;, &#039;pagamento-minino&#039;, &#039;parcelamento-de-fatura&#039; e etc. | Deve retornar a ação selecionado. |
| [[nome-opcao]] | &#039;12x&#039;, &#039;5x&#039;, &#039;4x&#039; e etc. | Deve retornar a opção selecionada. |

<br />


**No clique dos botões do modal &quot;Boleto&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-boleto'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;, &#039;copiar-codigo&#039;, &#039;imprimir&#039;, &#039;download&#039;. | Deve retornar o nome do botão clicado |

<br />


**No clique dos botões do modal &quot;fatura por email&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-fatura-por-email'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;, &#039;confirmar-adesao&#039;. | Deve retornar o nome do botão clicado |

<br />


**Na interação com os campos do modal &quot;fatura por email&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:logada',
    'eventAction': 'interacao:modal-fatura-por-email',
    'eventLabel': 'preencheu:campo:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;insira-seu-email&#039;, &#039;insira-seu-celular&#039;. | Deve retornar o nome do campo preenchido. |

<br />


**Na interação com os checkbox do modal &quot;fatura por email&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:logada',
    'eventAction': 'interacao:modal-fatura-por-email',
    'eventLabel': 'checkbox:aceito-receber-a-fatura'
  });
</script>
```




<br />


**No clique dos botões do modal &quot;fatura por email&quot; foi ativada**<br />

- **Onde:** No menu Faturas
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-fatura-por-email'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;central-de-relacionamento&#039;, ou &#039;ok&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No callback do modal &quot;fatura por email&quot;**<br />

- **Onde:** No menu Faturas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'midway:logada',
    'eventAction': 'envio:callback-modal-fatura-por-email',
    'eventLabel': '[[sucesso ou tipo-do-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-do-erro]] | &#039;sucesso&#039;, &#039;erro:faltou-preencher-campos&#039;, &#039;nao-foi-possivel&#039; e etc. | Deve retornar o sucesso ou o tipo do erro. |

<br />


**No clique dos links da seção &quot;Meus seguros contratados&quot;, caso você não possua seguros**<br />

- **Onde:** No menu Seguros.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:menu:seguros'
   data-gtm-event-label='link:[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;central-de-relacionamento&#039;, &#039;regras-aqui&#039;. | Deve retornar o nome do link clicado. |

<br />


**No clique dos botões da seção &quot;Meus seguros contratados&quot;.**<br />

- **Onde:** No menu Seguros.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:menu:seguros'
   data-gtm-event-label='botao:[[nome-botao]]:[[produto-contratado]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;certificado-de-adesao&#039; ou &#039;termo-de-cancelamento&#039;. | Deve retornar o nome do botão clicado. |
| [[produto-contratado]] | &#039;odontologico-riachuelo&#039;, &#039;seguro-desemprego-premiavel&#039; e etc. | Deve retornar o nome do produto contratado. |

<br />


**No clique do link &quot;Regras aqui&quot; da seção &quot;Meus seguros contratados&quot;.**<br />

- **Onde:** No menu Seguros.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:menu:seguros'
   data-gtm-event-label='link:regras-aqui'
>Botão</div>
```



<br />


**No clique dos botões do modal &quot;certificado de adesão&quot; do produto contratado**<br />

- **Onde:** No menu Seguros.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-certificado-de-adesao'
   data-gtm-event-label='[[produto-contratado]]:botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[produto-contratado]] | &#039;odontologico-riachuelo&#039;, &#039;seguro-desemprego-premiavel&#039; e etc. | Deve retornar o nome do produto contratado. |
| [[nome-botao]] | &#039;fechar&#039;, &#039;gerar-termo-de-adesao&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique dos botões do modal &quot;termo de cancelamento&quot; do produto contratado**<br />

- **Onde:** No menu Seguros.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:modal-termo-de-cancelamento'
   data-gtm-event-label='[[produto-contratado]]:botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[produto-contratado]] | &#039;odontologico-riachuelo&#039;, &#039;seguro-desemprego-premiavel&#039; e etc. | Deve retornar o nome do produto contratado. |
| [[nome-botao]] | &#039;fechar&#039;, &#039;gerar-termo-de-cancelamento&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique do botão &quot;Simular empréstimos&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:menu:emprestimos'
   data-gtm-event-label='botao:simular-emprestimos'
>Botão</div>
```



<br />


**No callback da tentiva de simular emprestimos**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'midway:logada',
    'eventAction': 'envio:callback-simular-emprestimos',
    'eventLabel': '[[sucesso ou tipo-do-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-do-erro]] | &#039;sucesso&#039;, &#039;ops-o-emprestimo-pessoal-nao-esta-disponivel&#039; e etc. | Deve retornar a mensagem da tentiva de envio do callback. |

<br />


**No clique do botões na seção &quot;Simulação de empréstimo&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:menu:emprestimos'
   data-gtm-event-label='botao:quero-simular:[[tipo-emprestimo]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-emprestimo]] | &#039;saque-facil&#039; ou &#039;credito-pessoal&#039;. | Deve retornar o tipo de emprestimo. |

<br />


**No clique do link na seção &quot;Simulação de empréstimo&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:menu:emprestimos'
   data-gtm-event-label='link:central-de-relacionamento'
>Botão</div>
```



<br />


**Na interação com os filtros na seção &quot;Simulação saque facil&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:logada',
    'eventAction': 'interacao:simulacao-saque-facil',
    'eventLabel': 'filtro:[[nome-filtro]]:[[valor-do-filtro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-filtro]] | &#039;valor-final&#039;, &#039;vencimento-primeira-parcela&#039;, &#039;quantidade-de-parcelas&#039;. | Deve retornar o nome do filtro. |
| [[valor-filtro]] | &#039;2500&#039;, &#039;30-dias&#039;, &#039;3x&#039; e etc. | Deve retornar o valor do filtro selecionado, |

<br />


**No callback da tentiva de &quot;Simulação saque facil&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'midway:logada',
    'eventAction': 'envio:callback-simulacao-saque-facil',
    'eventLabel': '[[sucesso ou tipo-do-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-do-erro]] | &#039;sucesso&#039;, &#039;nao-foi-possivel&#039; e etc, | Deve retornar a mensagem da tentiva de envio do callback. |

<br />


**No clique do botão na seção &quot;Simulação saque facil&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:simulacao-saque-facil'
   data-gtm-event-label='botao:simular'
>Botão</div>
```



<br />


**No clique do link na seção &quot;Simulação saque facil&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:simulacao-saque-facil'
   data-gtm-event-label='link:central-de-relacionamento'
>Botão</div>
```



<br />


**No clique do botão &quot;nova simulação&quot; na seção &quot;Simulação saque facil&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='clique:simulacao-saque-facil'
   data-gtm-event-label='botao:nova-simulacao'
>Botão</div>
```



<br />


**Na interação com os filtros na seção &quot;Simulação credito pessoal&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:logada',
    'eventAction': 'interacao:simulacao-credito-pessoal',
    'eventLabel': 'filtro:[[nome-filtro]]:[[valor-do-filtro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-filtro]] | &#039;valor-final&#039;, &#039;vencimento-primeira-parcela&#039;, &#039;quantidade-de-parcelas&#039;. | Deve retornar o nome do filtro. |
| [[valor-filtro]] | &#039;2500&#039;, &#039;30-dias&#039;, &#039;3x&#039; e etc. | Deve retornar o valor do filtro selecionado, |

<br />


**No callback da tentiva de &quot;Simulação credito pessoal&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'midway:logada',
    'eventAction': 'envio:callback-simulacao-credito-pessoal',
    'eventLabel': '[[sucesso ou tipo-do-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-do-erro]] | &#039;sucesso&#039;, &#039;nao-foi-possivel&#039; e etc, | Deve retornar a mensagem da tentiva de envio do callback. |

<br />


**No clique do botão na seção &quot;Simulação credito pessoal&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='interacao:simulacao-credito-pessoal'
   data-gtm-event-label='botao:simular'
>Botão</div>
```



<br />


**No clique do link na seção &quot;Simulação credito pessoal&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='interacao:simulacao-credito-pessoal'
   data-gtm-event-label='link:central-de-relacionamento'
>Botão</div>
```



<br />


**No clique do botão &quot;nova simulação&quot; na seção &quot;Simulação credito pessoal&quot;**<br />

- **Onde:** No menu Simular Empréstimos.
    
```html
<div
   data-gtm-event-category='midway:logada'
   data-gtm-event-action='interacao:simulacao-credito-pessoal'
   data-gtm-event-label='botao:nova-simulacao'
>Botão</div>
```

<br />

### Quitação de Dívida

**No clique dos botões**<br />

- **Onde:** Na tela de Negocie sua dívida
    
```html
<div
   data-gtm-event-category='midway:negocie-sua-divida'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]]  | 'botao:primeira-etapa', 'botao:segunda-etapa' e etc | Deve retornar o nome do botão clicado. |

<br />

**No callback após preencher os campos**<br />

- **Onde:** Na tela de Negocie sua dívida
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:negocie-sua-divida',
    'eventAction': 'interacao:campo:callback',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[erro]] | &#039;erro:cpf-invalido&#039;, &#039;erro:data-nascimento-invalido&#039;, &#039;erro:dados-invalidos&#039; e etc | Deve retornar o erro do callback, após preencher os campos. |

<br />

**No callback após validar os campos**<br />

- **Onde:** Na tela de Negocie sua dívida
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:negocie-sua-divida',
    'eventAction': 'validacao:callback',
    'eventLabel': '[[status]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;possui-pendencia&#039;, &#039;nao-foi-encontrada-nenhuma-pendencia&#039; e etc | Deve retornar o status do callback. |

<br />

**No clique dos botões para negociar a dívida**<br />

- **Onde:** Na tela de Negocie sua dívida, na etapa de "Confira os detalhes do seu cartão Riachuelo e Empréstimo Pessoal"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:negocie-sua-divida:[[produto]]',
    'eventAction': 'clique:botao',
    'eventLabel': 'valor:[[valor]]:[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[produto]] | 'cartao-riachuelo' ou 'emprestimo-pessoal' | Deve retornar o produto que usuário deseja negociar a dívida. |
| [[valor]] | &#039;1893.22&#039;, &#039;1495.38-2x&#039; e etc | Deve retornar o valor com desconto. |
| [[nome-botao]]  | &#039;pagar-com-desconto&#039;, &#039;simular-parcelamento&#039;, &#039;parcelado&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

**No clique dos links**<br />

- **Onde:** Na tela de Negocie sua dívida, na etapa de "Confira os detalhes do seu cartão Riachuelo e Empréstimo Pessoal"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:negocie-sua-divida:[[produto]]',
    'eventAction': 'clique:link',
    'eventLabel': 'mais-opcoes-parcelamento'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[produto]] | 'cartao-riachuelo' ou 'emprestimo-pessoal' | Deve retornar o produto que usuário deseja negociar a dívida. |

<br />

**No callback após tentar realizar um pagamento**<br />

- **Onde:** Na tela de Negocie sua dívida, na etapa de "Confira os detalhes do seu cartão Riachuelo e Empréstimo Pessoal"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:negocie-sua-divida:[[produto]]',
    'eventAction': 'validacao-pagamento:callback',
    'eventLabel': 'tipo:[[tipo-pagamento]]:[[status]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[produto]] | 'cartao-riachuelo' ou 'emprestimo-pessoal' | Deve retornar o produto que usuário deseja negociar a dívida. |
| [[tipo-pagamento]] | &#039;pagar-com-desconto&#039;, &#039;parcelado&#039;, &#039;simular-parcelamento&#039; e etc | Deve retornar o tipo do pagamento. |
| [[status]] | &#039;token-expirado-recarregar-pagina&#039; e etc | Deve retornar o status do callback. |

<br />


**No clique do botão de Copiar código**<br />

- **Onde:** Na tela de Negocie sua dívida, na etapa de &quot;Parabéns por não perder esta oportunidade&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:negocie-sua-divida:[[produto]]',
    'eventAction': 'clique:botao',
    'eventLabel': '[[tipo-pagamento]]:copiar-codigo'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[produto]] | 'cartao-riachuelo' ou 'emprestimo-pessoal' | Deve retornar o produto que usuário deseja negociar a dívida. |
| [[tipo-pagamento]] | &#039;pagar-com-desconto&#039;, &#039;parcelado&#039;, &#039;simular-parcelamento&#039; e etc | Deve retornar o tipo do pagamento. |

<br />

### Simulação de Empréstimo

**Após a interação com o campo de CPF e clicar em "Simular Já"**<br />

- **Onde:** Na tela de Comece nos falando seu CPF
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:simulacao-emprestimo',
    'eventAction': 'interacao:campo',
    'eventLabel': 'cpf'
  });
</script>
```

<br />


**No callback após tentar prosseguir**<br />

- **Onde:** Na tela de dados pessoais
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:simulacao-emprestimo',
    'eventAction': 'dados-pessoais:callback',
    'eventLabel': '[[erro]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[erro]] | &#039;erro:data-nascimento-invalido&#039;, &#039;erro:email-inexistente&#039; e etc | Deve retornar o erro apresentado. |

<br />

**No clique dos botões**<br />

- **Onde:** Na tela de dados pessoais
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:simulacao-emprestimo',
    'eventAction': 'clique:botao:dados-pessoais',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | 'voltar' ou 'prosseguir'. | Deve retornar o nome do botão.  |

<br />

**No clique dos botões**<br />

- **Onde:** Na tela de &quot;Vi que você ainda não tem um cartão RCHLO&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:simulacao-emprestimo',
    'eventAction': 'clique:botao:vi-que-nao-tem-cartao-rchlo',
    'eventLabel': 'baixe-app:[[nome-botao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;google-play&#039;, &#039;app-store&#039; e etc | Deve retornar o nome do botão. |

<br />


**No clique dos botões**<br />

- **Onde:** Na tela de &quot;Infelizmente nesse momento você não tem limite pré aprovado...&quot;, caso seja um correntista sem limite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:simulacao-emprestimo',
    'eventAction': 'clique:botao:nao-tem-limite-pre-aprovado',
    'eventLabel': 'baixe-app:[[nome-botao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;google-play&#039;, &#039;app-store&#039; e etc | Deve retornar o nome do botão. |

<br />


**No clique dos botões**<br />

- **Onde:** Na tela de &quot;Seja bem vindo ao simulador do Empréstimo Midway&quot;, caso seja um correntista com limite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:simulacao-emprestimo',
    'eventAction': 'clique:botao:bem-vindo-simulador-emprestimo',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;prosseguir&#039; e etc | Deve retornar o nome do botão. |

<br />


**No clique dos botões**<br />

- **Onde:** Na tela de &quot;Quanto você precisa?&quot;, caso seja um correntista com limite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:simulacao-emprestimo',
    'eventAction': 'clique:botao:quanto-voce-precisa',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;prosseguir&#039;, &#039;editar&#039; e etc | Deve retornar o nome do botão. |

<br />


**No clique dos botões**<br />

- **Onde:** Na tela de &quot;Primeiro pagamento em até&quot;, caso seja um correntista com limite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:simulacao-emprestimo',
    'eventAction': 'clique:botao:primeiro-pagamento-ate',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;prosseguir&#039; e etc | Deve retornar o nome do botão. |

<br />



**No clique dos botões**<br />

- **Onde:** Na tela de &quot;Resumo da simulação&quot;, caso seja um correntista com limite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:simulacao-emprestimo',
    'eventAction': 'clique:botao:resumo-simulacao',
    'eventLabel': '[[nome-botao]]',
    'dimension4': '[[valor]]',
    'dimension5':'[[parcelas]]',
    'dimension6': '[[1ºpagamento]]',
    'dimension7': '[[taxas]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;nova-simulacao&#039;, &#039;contratacao&#039; e etc | Deve retornar o nome do botão. |
| [[valor]] |  &#039;emprestimo-1500.00&#039; e etc | Deve retornar o valor da simulação |
| [[parcelas]] |  &#039;emprestimo-15x-120.00&#039; e etc | Deve retornar a quantidde de parcelas |
| [[1ºpagamento]] |  &#039;ate-80-dias&#039; e etc | Deve retornar quando será o primeiro pagamento |
| [[taxas]] |  &#039;9.99%&#039; e etc | Deve retornar a taxa |

<br />



**No clique dos botões**<br />

- **Onde:** Na tela de &quot;Gostou e quer contratar?&quot;, caso seja um correntista com limite
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'midway:simulacao-emprestimo',
    'eventAction': 'clique:botao:gostou-quer-contratar',
    'eventLabel': 'baixe-app:[[nome-botao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;google-play&#039;, &#039;app-store&#039; e etc | Deve retornar o nome do botão. |

<br />


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
