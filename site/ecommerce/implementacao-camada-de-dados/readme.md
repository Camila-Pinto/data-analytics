![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados - Riachuelo - Ecommerce
Última atualização: 14/06/2021 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

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
  
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd4-hit-userstatus]] | &#039;sim&#039;, &#039;nao&#039; | Deve retornar se o usuário já fez alguma compra anteriormente (sim) ou se não é sua primeira compra (nao). |
| [[cd5-hit-loginstatus]] | &#039;logged-in&#039;, &#039;logged-out&#039; | Status do login do usuário |
| [[cd7-user-usermagentoid]] | &quot;0123456&quot; | ID único do usuário definido após cadastro (MagentoID) |
| [[cd17-hit-pagename]] | &#039;home&#039;, &#039;pdp-produto-1&#039; | Nome amigável da página definido |


---


### General


**No carregamento do modal lateral  -Sua Sacola;**<br />

- **Onde:** Em todas as páginas do site.
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': 'virtual-pageview',
    'page': 'pageName:  /modal-carrinho',
});
</script>
```




<br />


**No carregamento do modal de login**<br />

- **Onde:** Em todas as páginas do site.
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': 'virtual_pageview',
    'page': 'pageName: /modal-cpf',
});
</script>
```




<br />


**No carregamento do modal lateral  - Sua Sacola com produtos;**<br />

- **Onde:** No modal lateral, em todas as páginas.
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': 'virtual_pageview',
    'page': 'pageName:  /modal-carrinho-com-produtos/',
});
</script>
```




<br />


**No carregamento do modal lateral  - Sua Sacola sem produtos;**<br />

- **Onde:** No modal lateral, em todas as páginas.
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': 'virtual_pageview',
    'page': 'pageName: /modal-sacola',
});
</script>
```




<br />


**No carregamento do modal de login para confirmar o cpf**<br />

- **Onde:** No modal lateral, em todas as páginas.
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': 'virtual_pageview',
    'page': 'pageName: /modal-confirmacao-cpf/',
});
</script>
```




<br />


**No carregamento do modal de login para confirmar a senha**<br />

- **Onde:** No modal lateral, em todas as páginas.
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': 'virtual_pageview',
    'page': 'pageName: /modal-confirmacao-senha/',
});
</script>
```




<br />


**No carregamento do modal lateral  - Sua Sacola sem produtos ou com produtos;**<br />

- **Onde:** No modal lateral, em todas as páginas.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'visualizou-modal-lateral',
'noInteraction': '1',
    'eventLabel': 'sacola:[[com ou sem]]-produtos'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[com ou sem]] | &#039;sacola:com-produtos&#039;, &#039;sacola:sem-produtos&#039;  | Deve retornar se a sacola está com ou sem produtos. |

<br />


**No carregamento do modal de login para confirmar o cpf e senha**<br />

- **Onde:** No modal lateral, em todas as páginas.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'visualizou-modal-lateral',
'noInteraction': '1',
    'eventLabel': '[[cpf ou senha]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cpf ou senha]] | &#039;cpf&#039;, &#039;senha&#039; | Deve retornar a confirmação que o usuário preencheu cpf e senha. |

<br />


**Na aparição do modal de newsletter**<br />

- **Onde:** No modal de cadastro de newsletter flutuante em todas as páginas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'visualizou-modal',
'noInteraction': '1',
    'eventLabel': 'newsletter'
  });
</script>
```




<br />


**Após o preenchimento do campo de email do newsletter**<br />

- **Onde:** No cadastro de newsletter flutuante em todas as páginas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'preencheu-campo:cadastro-newsletter',
    'eventLabel': 'email'
  });
</script>
```




<br />


**Na interação para fechar o modal de newsletter**<br />

- **Onde:** No modal de cadastro de newsletter flutuante em todas as páginas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'r_form_modal',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'enviar:cadastro-newsletter',
    'eventLabel': 'fechou'
  });
</script>
```




<br />


**No sucesso ou erro da tentativa de cadastro de newsletter**<br />

- **Onde:** No modal de cadastro de newsletter flutuante em todas as páginas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'r_form_modal',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'enviar:cadastro-newsletter',
    'eventLabel': '[[sucesso ou erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou erro]] | &quot;sucesso&quot; ou &quot;erro&quot; | Deve retornar o status do envio. |

<br />


**No clique dos links do Stick Bar (pré header)**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Carter&#039;s&quot;, &quot;Pool&quot;, &quot;Jeans&quot;, &quot;Geek&quot; etc
    
```html
<div
   data-gtm-event-category='riachuelo:geral'
   data-gtm-event-action='clique:header-stick-bar'
   data-gtm-event-label='[[nome-clicado]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clicado]] | &#039;cartes&#039;, &#039;pool&#039;, &#039;plus-size&#039; e etc | Deve retornar o nome do link do stick bar clicado. |

<br />


**No clique dos links do header**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;riachuelo&quot;(logo), &quot;login&quot;, &quot;minha-conta&quot; etc
    
```html
<div
   data-gtm-event-category='riachuelo:geral'
   data-gtm-event-action='clique:header'
   data-gtm-event-label='[[nome-clicacdo]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clicado]] | &#039;riachuelo&#039; (logo), &#039;login&#039;, &#039;minha-conta&#039; e etc | Deve retornar o nome do link do header e no logo clicado. |

<br />


**No clique dos links do menu superior (Obs: ignorar a categoria e subcategoria quando o clique for diretamente nos links principais do menu superior. Ignorar subcategoria quando o clique for apenas no link de categoria.)**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Novidades&quot;, &quot;Masculino&quot;, &quot;Feminino&quot;, &quot;Calça&quot; etc
    
```html
<div
   data-gtm-event-category='riachuelo:geral'
   data-gtm-event-action='clique:menu-superior'
   data-gtm-event-label='[[nome-clicacdo]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clicacdo]] | &#039;novidades&#039;, &#039;masculino&#039;, &#039;feminino&#039;, &#039;alfaiataria&#039;, &#039;outlet&#039;, &#039;busca&#039;, &#039;camisetas, tenis, polo_manga_curta&#039;, &#039;super_skinny&#039;, &#039;regular&#039;, &#039;feminino:vestido_feminino_colecao_feminina&#039;, &#039;feminino:busca_feminino_colecao_feminina&#039; | Deve retornar o nome do link principal do menu. |

<br />


**No clique dos ícones de &quot; + &quot;  e &quot; - &quot; do footer para abrir as categorias**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'clique:footer:categoria',
    'eventLabel': '[[nome-categoria]]:[[abriu-ou-fechou]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-categoria]] | &quot;cartao-riachuelo&quot;, &quot;sobre-a-riachuelo&quot;, &quot;moda-que-transforma&quot; e etc | Deve retornar o nome da categoria do footer. |
| [[abriu-ou-fechou]] | &quot;abriu&quot; ou &quot;fechou&quot; | Deve retornar o status da categoria. |

<br />


**No clique dos links interno do footer**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Carter&#039;s&quot;, &quot;Pool&quot;, &quot;Jeans&quot;, &quot;Geek&quot; etc
    
```html
<div
   data-gtm-event-category='riachuelo:geral'
   data-gtm-event-action='clique:footer:link'
   data-gtm-event-label='[[nome-categoria]]:[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-categoria]] | &quot;cartao-riachuelo&quot;, &quot;sobre-a-riachuelo&quot;, &quot;moda-que-transforma&quot; e etc | Deve retornar o nome da categoria do footer. |
| [[nome-link]] | &#039;saiba-como-adquirir &#039;a-empresa&#039;, &#039;entre-costuras&#039;  e etc | Deve retornar o nome do link do footer clicado. |

<br />


**Após a seleção de interesse dentro do checkbox para se cadastrar em newsletter (OBS: Caso seja selecionado mais de uma opção, deve retornar todas as opções concatenadas, separando por ponto e vírgula ( ; ). Ex: &quot;novidades;feminino;&quot; etc)**<br />

- **Onde:** No cadastro de newsletter, localizado no footer em todas as páginas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'selecionou-interesse:newsletter-footer',
    'eventLabel': '[[interesse-selecionado]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[interesse-selecionado]] | &quot;novidades&quot;, &quot;selecionar-todos&quot;, &quot;infantil&quot; e etc. | Deve retornar o interesse selecionado no checkbox. |

<br />


**Após o preenchimento do campo do newsletter**<br />

- **Onde:** No cadastro de newsletter, localizado no footer em todas as páginas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'preencheu-campo:cadastro-newsletter-footer',
    'eventLabel': 'email'
  });
</script>
```




<br />


**No sucesso ou erro da tentativa de cadastro de newsletter**<br />

- **Onde:** No cadastro de newsletter, localizado no footer em todas as páginas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'enviar:cadastro-newsletter-footer',
    'eventLabel': '[[sucesso ou erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou erro]] | &quot;sucesso&quot; ou &quot;erro&quot; | Deve retornar o status do envio. |

<br />


**No clique nos links de redes sociais**<br />

- **Onde:** Localizado no footer em todas as páginas
    - **Titulo ou nome do botão/link:** &quot;Riachuelo-facebook&quot;, &quot;Riachuelo-Youtube&quot; etc
    
```html
<div
   data-gtm-event-category='riachuelo:geral'
   data-gtm-event-action='clique:link:rede-social:footer'
   data-gtm-event-label='[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &quot;riachuelo-facebook&quot;,  &quot;riachuelo-youtube&quot;, &quot;riachuelo-linkedin&quot; e etc | Deve retornar o nome do link da rede social clicada. |

<br />


**Na aparição do banner de lightbox**<br />

- **Onde:** No banner de lightbox ao acessar o site na página home
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'visualizou-banner',
    'eventLabel': 'lightbox'
  });
</script>
```




<br />


**Ao sair do banner de lightbox**<br />

- **Onde:** No banner de lightbox ao acessar o site na página home
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'saiu-banner',
    'eventLabel': 'lightbox'
  });
</script>
```




<br />


**Ao clicar no botão de &quot;Conheça a coleção de dia das crianças&quot;**<br />

- **Onde:** No banner de lightbox ao acessar o site na página home
    
```html
<div
   data-gtm-event-category='riachuelo:geral'
   data-gtm-event-action='clique:botao-banner'
   data-gtm-event-label='conheca-colecao-dia-das-criancas'
>Botão</div>
```



<br />


**No clique dos botões ou links no modal &quot;Sacola&quot;**<br />

- **Onde:** Na página de detalhe de produto
    
```html
<div
   data-gtm-event-category='riachuelo:geral'
   data-gtm-event-action='clique:[[botao ou link]]:modal-sacola'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao ou link]] | &#039;botao&#039; ou &#039;link&#039;. | Deve retornar o tipo de elemento clicado. |
| [[nome-item]] | &#039;ver-todos-os-itens&#039;, &#039;ir-para-sacola&#039;. | Deve retornar o nome do item clicado, |

<br />


### Lightbox de Abandono - Checkout

**Exibição do lightbox de frete grátis.**<br />

- **Onde:** Todas as páginas do checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:geral',
    'eventAction': 'exibiu:lightbox',
'noInteraction': '1',
    'eventLabel': '[[id-lightbox]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[id-lightbox]] | &#039;opc-boleto-10&#039;, &#039;opc-boleto-14&#039;, &#039;opc-boleto-16&#039;, etc. | Disparar o ID do lightbox. |

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[id-lightbox]] | &#039;opc-boleto-10&#039;, &#039;opc-boleto-14&#039;, &#039;opc-boleto-16&#039;, etc. | Disparar o ID do lightbox. |
| [[botao-clicado]] | &#039;ir-as-compras&#039; ou &#039;continuar&#039;, nao-obrigado | Retornar o texto do elemento que foi clicado. |

<br />


### Login e  Cadastro

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;facebook, google&#039;. | Deve retornar o botão clicado. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;cpf&#039; , &#039;senha&#039; | Deve retornar o nome do campo preenchido. |

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[link-botao]] | &#039;link' ou 'botao&#039; e etc | Deve retornar o elemento clicado. |
| [[nome-item]] | &#039;esqueceu-senha&#039;, &#039;continuar-cpf&#039;, &#039;continuar-senha&#039; e etc | Deve retornar o nome do item clicado. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;, &#039;voce-nao-digitou-os-dados-corretamente&#039;, etc | Deve retornar o sucesso ou descrição do erro da tentativa de login |
| [[cd5-hit-loginstatus]] | &#039;logged-in&#039;, &#039;logged-out&#039; | Status do login do usuário |
| [[cd7-user-usermagentoid]] | &quot;0123456&quot; | ID único do usuário definido após cadastro (MagentoID) |
| [[cd59-hit-tipologin]] | &quot;login-cpf-senha&quot;, &quot;facebook&quot;, &quot;google&quot; e etc | Retornar o tipo de login realizado pelo usuário. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome-completo, e-mail, data-de-nascimento&#039; e etc. | Deve retornar o nome do campo preenchido. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &quot;sucesso&quot; ou &quot;dados-invalidos&quot; | Deve retornar o status do envio. |

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;politica-de-privacidade, nao-sei-meu-cep, termos-e-condicoes-de-uso&#039;. | Deve retornar o link clicado. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[plp ou pdp]] | &#039;plp&#039;, &#039;pdp&#039; e etc | Deve retornar o nome da página. |
| [[id-produto]] | &#039;0655&#039;, &#039;8566&#039; etc | Deve retornar o id do produto adicionado a lista de desejos. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[abriu ou fechou]] | &#039;abriu&#039;, &#039;fechou&#039; | Deve retornar a ação do usuário. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[abriu ou fechou]] | &#039;abriu&#039;, &#039;fechou&#039; | Deve retornar a ação do usuário. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tamanho]] | &#039;p&#039;, &#039;m&#039; &#039;g&#039; etc | Deve retornar o tamanho sugerido. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-frete]] | &#039;normal&#039;, &#039;expresso &#039;, &#039;entrega-agendada&#039;,&#039;retirar-em-loja&#039;  | Deve retornar os nomes dos tipos de frete. |
| [[previsao]] | &#039;xx-dias&#039;, &#039;2-dias&#039; e etc. | Deve retornar a quantidade de dias para a entrega. |
| [[valor]] | &#039;0.00&#039;, &#039;2.99&#039;, &#039;5.00&#039; | Deve retornar os valores dos fretes. |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:richlo&#039;, &#039;nao:richlo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-rede-social]] | &#039;facebook&#039;, &#039;twitter&#039;, &#039;pinterest&#039;, &#039;whatsapp&#039; e etc | Deve retornar o nome da rede social clicada. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:richlo&#039;, &#039;nao:richlo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[localizacao-filtro]] | &#039;busca&#039;, &#039;plp&#039; | Deve retornar a localização do filtro. |
| [[filtro]] | &#039;departamento&#039;, &#039;categorias&#039;, &#039;tamanho&#039;, &#039;ordenar&#039;, &#039;mais-filtros&#039;, &#039;modal-mais-filtros&#039; e etc | Deve retornar o nome do filtro. |
| [[filtro-selecionado]] | &#039;menor-preco&#039;, &#039;masculino&#039;, &#039;infanto-juvenil&#039;, &#039;list&#039;, &#039;grid&#039;,  &#039;definicao-valor:0-300&#039; e etc | Deve retornar o label do filtro selecionado. |

<br />


**No clique do botão &quot;Aplicar&quot; para aplicar os filtros selecionados**<br />

- **Onde:** Na página de busca, após buscar algum termo e também nas páginas de Listas de Produtos
    - **Titulo ou nome do botão/link:** &quot;Aplicar&quot;
    
```html
<div
   data-gtm-event-category='riachuelo:filtro'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[localizacao-filtro]]:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[localizacao-filtro]] | &#039;busca&#039;, &#039;plp&#039; | Deve retornar a localização do filtro. |
| [[nome-botao]] | &#039;limpar&#039; ou &#039;aplicar&#039; | Deve retornar o nome do botão. |

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[id-pedido]] | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;informacoes-do-pedido&#039;, &#039;produtos-comprados&#039;, &#039;postagens-do-pedido&#039; | Deve retornar o nome do item clicado. |

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[devolver ou trocar]] | &#039;devolver&#039; ou &#039;trocar&#039; | Deve retornar o nome do item clicado. |
| [[id-pedido]] | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| [[id-produto-filho]] | &#039;0655&#039;, &#039;8566&#039;. No caso de mais que um ID do produto na devolução enviar &#039;1895/0655/8566&#039; | Deve retornar os ids dos produtos filhos referente ao pedido de devolução. |

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;, &#039;voltar&#039; ou &#039;aceito&#039;  | Deve retornar o nome do botão ou ícone clicado. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[motivo-da-solicitacao]] | &#039;arrependimento&#039;, &#039;defeito&#039;, etc | Deve retornar a opção selecionada em &quot;Motivo da solicitação&quot;. |
| [[id-pedido]] | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| [[id-produto-filho]] | &#039;0655&#039;, &#039;8566&#039;. No caso de mais que um ID do produto na devolução enviar &#039;1895/0655/8566&#039; | Deve retornar os ids dos produtos filhos referente ao pedido de devolução. |

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039; ou &#039;confirmar&#039;  | Deve retornar o nome do botão clicado. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mensagem-alerta]] | &#039;nao-aceitamos-a-devolucao-via-autoatendimento-com-valor-inferior-a-R$80,90&#039; etc.  | Deve retornar a mensagem de alerta. |
| [[id-pedido]] | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| [[id-produto-filho]] | &#039;0655&#039;, &#039;8566&#039;. No caso de mais que um ID do produto na devolução enviar &#039;1895/0655/8566&#039; | Deve retornar os ids dos produtos filhos referente ao pedido de devolução. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;avancar&#039;,  &#039;voltar&#039; | Deve retornar o nome do botão clicado. |
| [[id-pedido]] | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| [[id-produto-filho]] | &#039;0655&#039;, &#039;8566&#039;. No caso de mais que um ID do produto na devolução enviar &#039;1895/0655/8566&#039; | Deve retornar os ids dos produtos filhos referente ao pedido de devolução. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;, &#039;ocorreu-um-erro&#039;, etc | Deve retornar o sucesso ou descrição do erro de solicitação de devolução. |
| [[id-pedido]] | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| [[id-produto-filho]] | &#039;0655&#039;, &#039;8566&#039;. No caso de mais que um ID do produto na devolução enviar &#039;1895/0655/8566&#039; | Deve retornar os ids dos produtos filhos referente ao pedido de devolução. |

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;adicionar&#039;, &#039;fechar&#039; &#039;salvar-e-finalizar&#039;, &#039;remover&#039; | Deve retornar o nome do botão ou ícone clicado. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;, &#039;ocorreu-um-erro&#039; etc | Deve retornar o sucesso ou descrição do erro de inserção de terceiros para retirada de pedidos. |
| [[id-pedido]] | &#039;236565&#039; &#039;65264561&#039; etc | Deve retornar o id do pedido. |
| [[numero-pessoas]] | &#039;1&#039; ou &#039;2&#039;. | Deve retornar o numero de pessoas que farão a retirada. |

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-pergunta]] | &#039;por-que-o-meu-pedido-foi-cancelado&#039;, &#039;por-que-o-pedido-tem-mais-de-uma-entrega&#039;, &#039;por-que-este-item-foi-cancelado&#039; e etc. | Deve retornar o nome da pergunta clicada. |

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-pergunta]] | &#039;por-que-o-meu-pedido-foi-cancelado&#039;, &#039;por-que-o-pedido-tem-mais-de-uma-entrega&#039;, &#039;por-que-este-item-foi-cancelado&#039; e etc. | Deve retornar o nome da pergunta clicada. |
| [[sim-nao]] | &#039;sim&#039; ou &#039;nao&#039;. | Deve retornar se o usuario clicou em sim ou nao. |

<br />



### Checkout 

**Na interação com o campo para calcular o frete e prazo**<br />

- **Onde:** Na etapa sacola
    
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


**No clique do link &quot;Não sei meu cep&quot;**<br />

- **Onde:** Na etapa sacola
    
```html
<div
   data-gtm-event-category='riachuelo:checkout'
   data-gtm-event-action='interacao:link:sacola'
   data-gtm-event-label='nao-sei-meu-cep'
>Botão</div>
```



<br />


**Na tentativa de callback para calcular o frete, apos clicar em aplicar**<br />

- **Onde:** Na etapa sacola
    
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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;erro:nao-e-possivel-entregar-neste-cep&#039; e etc | Deve retornar a mensagem de sucesso ou tipo de erro. |

<br />


**Ao selecionar a quantidade do produto**<br />

- **Onde:** Na etapa sacola e fluxo de checkout.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'interacao:quantidade:[[etapa]]',
    'eventLabel': '[[mais ou menos]]:[[quantidade]]:[[categoria-produto]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mais ou menos]] | &#039;mais&#039; ou &#039;menos&#039; | Deve retornar o nome do item clicado. |
| [[quantidade]] | &#039;1&#039;, &#039;3&#039;, &#039;4&#039; e etc | Deve retornar a quantidade. |
| [[etapa]] | &#039;sacola&#039;, &#039;pagamento&#039; e etc | Deve retornar a etapa em que ocorreu o clique. |
| [[categoria-produto]] | 'masculino', 'feminino' e etc | Deve retornar a categoria do produto |

<br />


**No callback apos selecionar a quantidade do produto**<br />

- **Onde:** Na etapa sacola e Checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'callback:quantidade:[[etapa]]',
    'eventLabel': '[[status]]:[[quantidade]]:[[categoria-produto]]',
    'dimension40': '[[product-exclusivoecommerce+marketplace]]',
    'dimension30': '[[product-subcategoria]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039;, &#039;erro:nao-existe-a-quantidade-em-estoque&#039;, &#039;restam-apenas-2-unidades-deste-item-no-estoque&#039; e etc | Deve retornar a mensagem de sucesso ou tipo de erro; |
| [[quantidade]] | &#039;1&#039;, &#039;3&#039;, &#039;4&#039; e etc | Deve retornar a quantidade. |
| [[etapa]] | &#039;sacola&#039;, &#039;pagamento&#039; e etc | Deve retornar a etapa em que ocorreu o clique. |
| [[categoria-produto]] | &#039;masculino&#039;, &#039;feminino&#039; e etc | Deve retornar a categoria do produto. |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:richlo&#039;, &#039;nao:richlo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |

<br />


**No clique dos icones para editar ou remover os produtos**<br />

- **Onde:** No fluxo de checkout e sacola
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'clique:icone:[[etapa]]',
    'eventLabel': '[[nome-icone]]:[[categoria-produto]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-icone]] | &#039;editar&#039;, &#039;remover&#039; | Deve retornar o nome do icone clicado. |
| [[etapa]] | &#039;sacola&#039;, &#039;pagamento&#039; e etc | Deve retornar a etapa em que ocorreu o clique. |
| [[categoria-produto]] | 'masculino', 'feminino' e etc | Deve retornar a categoria do produto |

<br />


**No clique dos links  e botões**<br />

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


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[etapa]] | &#039;sacola&#039;, &#039;pagamento&#039; e etc | Deve retornar a etapa em que ocorreu o clique. |
| [[tipo-item]]  | &#039;botao&#039;, &#039;link&#039;| Deve retornar se o item é um botão ou um link|
| [[nome-item]]  | &#039;editar-sacola&#039; e etc| Deve retornar se o nome do item clicado|

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
    'dimension40': '[[product-exclusivoecommerce+marketplace]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso-ou-tipo-erro]] | &#039;sucesso&#039;, &#039;cupom-invalido&#039; e etc. | Deve retornar sucesso ou o tipo de erro. |
| [[etapa]] | &#039;sacola&#039;, &#039;pagamento&#039; e etc | Deve retornar a etapa em que ocorreu o clique. |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:richlo&#039;, &#039;nao:richlo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |

<br />




**No carregamento dos tipos de entrega disponíveis**<br />

- **Onde:** Na página de checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'tipos-de-entrega:[[origem-lista]]',
'noInteraction': '1',
    'eventLabel': '[{&#039;t&#039;:&#039;[[tipo-frete]]&#039;,&#039;d&#039;:&#039;[[previsao]]&#039;,&#039;p&#039;:&#039;[[valor]]&#039;},{&#039;t&#039;:&#039;[[tipo-frete]]&#039;,&#039;d&#039;:&#039;[[previsao]]&#039;,&#039;p&#039;:&#039;[[valor]]&#039;}]',
    'dimension40': '[[product-exclusivoecommerce+marketplace]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[origem-lista]] | &#039;pdp&#039;, &#039;pagamento&#039; ou &#039;sacola&#039; | Deve retornar o local de origem que a lista de tipos de entrega aparece. |
| [[tipo-frete]] | &#039;normal&#039;, &#039;expresso &#039;, &#039;entrega-agendada&#039;,&#039;retirar-em-loja&#039;  | Deve retornar os nomes dos tipos de frete. |
| [[previsao]] | &#039;DD/MM/AAAA&#039;| Deve retornar a quantidade de dias para a entrega. |
| [[valor]] | &#039;0.00&#039;, &#039;2.99&#039;, &#039;5.00&#039; | Deve retornar os valores dos fretes. |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:richlo&#039;, &#039;nao:richlo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |

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
    'dimension40': '[[product-exclusivoecommerce+marketplace]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[origem-lista]] | &#039;pdp&#039;, &#039;checkout&#039; ou &#039;sacola&#039;  | Deve retornar o local de origem que a lista de tipos de entrega aparece. |
| [[tipo-frete]] | &#039;normal&#039;, &#039;expresso &#039;, &#039;entrega-agendada&#039;,&#039;retirar-em-loja&#039;  | Deve retornar os nomes dos tipos de frete. |
| [[previsao]] | &#039;DD/MM/AAAA&#039;, | Deve retornar a quantidade de dias para a entrega. |
| [[valor]] | &#039;0.00&#039;, &#039;2.99&#039;, &#039;5.00&#039; | Deve retornar os valores dos fretes. |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |

<br />


**No clique do icone para editar o endereço**<br />

- **Onde:** Na página de checkout
    
```html
<div
   data-gtm-event-category='riachuelo:checkout'
   data-gtm-event-action='clique:icone:[[etapa]]'
   data-gtm-event-label='editar-endereco'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[etapa]] | &#039;sacola&#039;, &#039;pagamento&#039; e etc | Deve retornar a etapa em que ocorreu o clique. |

<br />


**No clique do icones e botões no modal &quot;meus endereços salvos&quot;**<br />

- **Onde:** Na página de checkout
    
```html
<div
   data-gtm-event-category='riachuelo:checkout'
   data-gtm-event-action='clique:[[icone ou botao]]'
   data-gtm-event-label='meus-enderecos:[[nome-item]]:[[posicao-endereco]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[icone ou botao]] | &#039;icone&#039;, &#039;botao&#039; | Deve retornar o tipo de elemento clicado. |
| [[nome-item]] | &#039;voltar&#039;, &#039;salvar, &#039;fechar&#039;, &#039;excluir&#039; | Deve retornar o nome do item clicado. |
| [[posicao-endereco]] | &#039;posicao-1&#039;, &#039;posicao-2&#039; e etc | Deve retornar a posição do endereço clicado. |

<br />


**No clique dos botão ou link no resumo do pedido**<br />

- **Onde:** Na página de sacola
    
```html
<div
   data-gtm-event-category='riachuelo:checkout'
   data-gtm-event-action='clique:[[botao ou link]]:sacola'
   data-gtm-event-label='resumo-pedido:[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao ou link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o tipo de elemento clicado. |
| [[nome-item]] | &#039;continuar-comprando, &#039;ir-para-pagamento&#039; | Deve retornar o nome do item clicado. |

<br />


**Na interação com os tipos de cartão na seção de &#039;pagamentos&#039;,para saber se o cliente selecionou um cartão salvo ou clicou em usar outro cartão**<br />

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



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-cartao]] | &#039;usar-outro-cartao&#039;, &#039;cartao-salvo&#039; e etc | Deve retornar  se o cliente estava usando um cartão salvo para pagar ou se ele clicou em usar outro cartao. |

<br />


**No callback ao adicionar um cupom**<br />

- **Onde:** Na página de checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'callback:adicionar-cupom',
    'eventLabel': '[[sucesso-ou-tipo-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso-ou-tipo-erro]] | &#039;sucesso&#039;, &#039;cupom-invalido&#039; e etc. | Deve retornar sucesso ou o tipo de erro. |

<br />


**Ao clicar no botão &#039;consultar&#039;  ou &#039;aplicar&#039;, após selecionar &#039;Cartão Presente&#039;, &#039;Vale troca&#039; ou outros cartões como forma de pagamento**<br />

- **Onde:** Na página de checkout
    
```html
<div
   data-gtm-event-category='riachuelo:checkout'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]:[[tipo-cartao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-cartao]] | &#039;cartao-presente&#039;, &#039;vale-troca&#039;, &#039;cartao-de-credito&#039; e etc | Deve retornar o tipo de cartão removido. |
| [[nome-botao]] | &#039;consultar&#039; , &#039;aplicar&#039; e etc | deve retornar o nome do botão clicado. |

<br />



**Ao clicar no icone de remover&#039; , após selecionar &#039;Cartão Presente&#039; , &#039;Vale Troca&#039; ou outros cartões como forma de pagamento**<br />

- **Onde:** Na página de checkout
    
```html
<div
   data-gtm-event-category='riachuelo:checkout'
   data-gtm-event-action='clique:icone'
   data-gtm-event-label='remover:[[tipo-cartao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-cartao]] | &#039;cartao-presente&#039;, &#039;vale-troca&#039;, &#039;cartao-de-credito&#039; e etc | Deve retornar o tipo de cartão removido. |

<br />



**No clique nos links  &#039;Adicionar novo cartão presente&#039;, &#039;Aplicar novo vale troca&#039;, &#039;Usar outro cartão&#039; após selecionar algum cartão como forma de pagamento**<br />

- **Onde:** Na página de checkout
    
```html
<div
   data-gtm-event-category='riachuelo:checkout'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;usar-outro-cartao&#039;, &#039;aplicar-novo-vale-troca&#039;,&#039;aplicar-novo-cartao-presente&#039;, &#039;continuar-comprando&#039; e etc. |  Deve retornar o nome do link clicado. |

<br />

**Na interação com os campos do formulário de cadastro de cliente.**<br />

- **Onde:** Na etapa de dados pessoais do checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'r_GlobalEvent',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'interacao:dados-pessoais',
    'eventLabel': 'erro:[[nome campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome campo]] | &#039;nome&#039;, &#039;email&#039; etc | Deve retornar o nome do campo preenchido. |

<br />


**No erro de preenchimento de campo**<br />

- **Onde:** Na página de checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'interacao:campo:[[nome-campo]]',
    'eventLabel': 'erro:[[tipo-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;numero-do-cartao&#039;, &#039;data-de-validade&#039; etc | Deve retornar o nome do campo preenchido. |
| [[tipo-erro]] | &#039;dados-invalidos&#039; etc | Deve retornar a descrição do erro de preechimento de campo. |

<br />


**Ao clicar nos botão Finalizar compra&#039; .**<br />

- **Onde:** Na página de checkout
    
```html
<div
   data-gtm-event-category='riachuelo:checkout'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='finalizar-compra'
>Botão</div>
```



<br />



**No callback  de finalização de compra**<br />

- **Onde:** Ao tentar finalizar a compra
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'callback:finalizar-pedido',
    'eventLabel': '[[status]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; , &#039;erro:dados-invalidos&#039;, &#039;erro:cartao-invalido&#039; &#039;erro:endereco-invalido&#039; e etc| Deve retornar se a compra foi realizada com sucesso ou erro|

<br />


### Enhanced E-commerce 

**Na visualização de cada banner exibido durante a navegação**<br />

- **Onde:** Em todas as páginas que exibirem banners (home, clp, plp)
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'promotionImpression',
    'eventCategory': 'enhanced-ecommerce',
    'eventAction': 'promoView',
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
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[id-promocao]] | &quot;https://midia.fotos-riachuelo.com.br/fotos/megamenu/home/janeiro2020/ADS-5506/Carrossel-home-mcasa-kit-colcha_.jpg&quot; e etc | Deve retornar o link da imagem do banner |
| [[nome-promocao]] | &quot;polos-diferenciadas&quot; | Deve retornar o nome amigável do banner |
| [[arte-banner]] | &quot;hero;clp;banner:clicavel&quot;, &quot;mosaico;blog;banner:nao-clicavel&quot; e etc | Deve retornar o formato, o tipo de página que o banner foi clicado e o tipo do banner  |
| [[posicao-promocao]] | &quot;1&quot; | Deve retornar a posição que o banner é exibido  |


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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[id-promocao]] | &quot;https://midia.fotos-riachuelo.com.br/fotos/megamenu/home/janeiro2020/ADS-5506/Carrossel-home-mcasa-kit-colcha_.jpg&quot; e etc | Deve retornar o link da imagem do banner |
| [[nome-promocao]] | &quot;polos-diferenciadas&quot; | Deve retornar o nome amigável do banner |
| [[arte-banner]] | &quot;hero;clp;banner:clicavel&quot;, &quot;mosaico;blog;banner:nao-clicavel&quot; e etc | Deve retornar o formato, o tipo de página que o banner foi clicado e o tipo do banner  |
| [[posicao-promocao]] | &quot;1&quot; | Deve retornar a posição que o banner é exibido  |

<br />


**Na visualização de uma vitrine de produtos**<br />

- **Onde:** Em todas as páginas que exibirem uma lista de produtos (home, PLP, PDP)
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productImpressions',
  'eventCategory':'enhanced-ecommerce',
  'eventAction': 'impressions',
  'noInteraction': '1',
  'ecommerce': {
    'impressions': [{
'dimension40': '[[product-exclusivoecommerce+marketplace]]',
  'dimension58': '[[product-selo/tag]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'list': '[[lista-produto]]',
        'variant': '[[variacao-produto]]',
        'position': '[[posicao-produto]]'
    }]
  }
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do  seller/marketplace |
| [[product-selo/tag]] |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. OBS: Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[lista-produto]] | &quot;feminino-blusa&quot;, &quot;masculino-polo&quot;, &quot;acessorios-e-relogios-aneis&quot;, &quot;feminino_em-destaque_plus-size&quot;, &quot;plp_campanhas_modacasa_fevereiro-2020_pascoa-casa-riachuelo&quot;, &quot;pdp_quem-viu-tambem-viu&quot;, &quot;home-destaques&quot;, &quot;novidades&quot; e etc | Nome da lista que o produto aparece, no caso de PLP, o nome da PLP tem que ser a URL ao invés de usar a “/” colocar “_“ |
| [[posicao-produto]] | &quot;2&quot; | Posição que o produto aparece em uma lista de produtos |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |

<br />


**No clique dos produtos da vitrine interagidos na página**<br />

- **Onde:** Em todas as páginas que exibirem uma lista de produtos (home, PLP, PDP)
    

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
'dimension13': '[[cd13-product-idadedoproduto]]',
  'dimension24': '[[product-ordemdeinserção]]',
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
| [[cd13-product-idadedoproduto]] |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[product-ordemdeinserção]] | 1,2,3 .. | Ordem de inserção ao carrinho |
| [[product-padronagemdoproduto]] | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] |  &#039;12&#039;, &#039;produto-temporariamente-indisponivel&#039;,&#039;indisponivel&#039; etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| [[product-lifestyle]] | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[product-selo/tag]] |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. OBS: Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |
| [[lista-produto]] | &quot;feminino-blusa&quot;, &quot;masculino-polo&quot;, &quot;acessorios-e-relogios-aneis&quot;, &quot;feminino_em-destaque_plus-size&quot;, &quot;plp_campanhas_modacasa_fevereiro-2020_pascoa-casa-riachuelo&quot;, &quot;pdp_quem-viu-tambem-viu&quot;, &quot;home-destaques&quot;, &quot;novidades&quot; e etc | Nome da lista que o produto aparece, no caso de PLP, o nome da PLP tem que ser a URL ao invés de usar a “/” colocar “_“ |
| [[posicao-produto]] | &quot;2&quot; | Posição que o produto aparece em uma lista de produtos |

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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd13-product-idadedoproduto]] |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[product-padronagemdoproduto]] | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] |  &#039;12&#039;, &#039;produto-temporariamente-indisponivel&#039;,&#039;indisponivel&#039; etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| [[product-lifestyle]] | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e seu id do seller/marketplace |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[product-selo/tag]] |  &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. OBS: Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |

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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd13-product-idadedoproduto]] |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[product-ordemdeinserção]] | 1,2,3 .. | Ordem de inserção ao carrinho |
| [[product-tamanhodoproduto]] |  &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| [[product-cordoproduto]] | &#039;vermelho&#039; | Cor do produto  |
| [[product-padronagemdoproduto]] | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] | &#039;12&#039;, &#039;produto-temporariamente-indisponivel&#039;,&#039;indisponivel&#039; etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| [[product-lifestyle]] | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do  seller/marketplace |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |

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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd13-product-idadedoproduto]] |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[product-ordemdeinserção]] | 1,2,3 .. | Ordem de inserção ao carrinho |
| [[product-tamanhodoproduto]] |  &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| [[product-cordoproduto]] | &#039;vermelho&#039; | Cor do produto  |
| [[product-padronagemdoproduto]] | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] | &#039;12&#039;, &#039;produto-temporariamente-indisponivel&#039;,&#039;indisponivel&#039; etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| [[product-lifestyle]] | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do  seller/marketplace |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |

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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd13-product-idadedoproduto]] |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[product-ordemdeinserção]] | 1,2,3 .. | Ordem de inserção ao carrinho |
| [[product-tamanhodoproduto]] |  &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| [[product-cordoproduto]] | &#039;vermelho&#039; | Cor do produto  |
| [[product-padronagemdoproduto]] | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] | &#039;12&#039;, &#039;produto-temporariamente-indisponivel&#039;,&#039;indisponivel&#039; etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| [[product-lifestyle]] | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |
| [[passo-checkout]] | &quot;1&quot; | Na etapa de sacola de compras |
| [[passo-checkout]] | &quot;2&quot; | No carregamento da página de formulário de cadastro para usuarios que estão no fluxo de compra e não possui conta. |
| [[passo-checkout]] | &quot;3&quot; | No carregamento da página de confirmação do endereço de entrega e frete |
| [[passo-checkout]] | &quot;4&quot; | No carregamento da página de seleção do método de pagamento|

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
        'actionField':  {'step':'3,'option':'shipping:[[nome-entrega]]:[[valor]]:[[previsao]]'},
    }
  }
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-entrega]] | &#039;normal&#039;, &#039;expresso &#039;, &#039;entrega-agendada&#039;,&#039;retirar-em-loja:shopping-tiete-plaza&#039;, &#039;retirar-em-loja:shopping-bourbon-sp&#039; etc | Deve retornar os nomes dos tipos de entrega. |
| [[valor]] | &#039;&#039;0.00&#039;, &#039;2.99&#039;, &#039;5.00&#039;. | Deve retornar os valores dos fretes. |
| [[previsao]] | &#039;3-dias, 4-dias,5-dias&#039; | Deve retornar a quantidade de dias. |

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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-pagamento]] | &#039;cartao&#039;, &#039;paypal&#039;, &#039;boleto&#039; etc | Deve retornar o nome da opção de pagamento clicada. |
| [[opcao-adicional-1]] | &#039;vale-troca&#039; | Deve retornar a opção de pagamento adicional.(Se não houver opções adicionais de pagamento retornar as variaveis vazias.) |
| [[opcao-adicional-2]] | &#039;cartao-presente&#039;, &#039;cartao-presente/vale-troca&#039; e etc | Deve retornar a segunda opção(Se não houver opções adicionais de pagamento retornar as variaveis vazias) |
| [[cd22-hit-metodopagamento]] |  &#039;cartao-credito&#039;, &#039;cartao-credito/vale-troca&#039;, &#039;boleto&#039;  | Nome de todos os tipos de pagamento selecionados |
| [[cartãopresente]] | 100.00&#039;, &#039;50.00&#039; e etc | Deve retornar o valor pago por cartão presente utilizado. |
| [[valetroca]] | 40.00&#039;, &#039;23.00&#039;, &#039;200.00&#039; | Deve retornar o valor pago pelo vale troca utilizado. |
| [[pagamentocomum]] | 140.00&#039;, &#039;67.00&#039;, &#039;100.00&#039; | Deve retornar o valor pago em formas de  pagamentos comuns como, por exemplo, boleto, cartão de credito e etc |

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
        'shipping': '[[frete-transacao]]'
        'tax': '[[taxa-transacao]]'
        'coupon': '[[coupon-transacao]]'
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

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd2-tipodecartão]] |  &#039;credito&#039;, &#039;riachuelo&#039; | Tipo do cartão utilizado na compra |
| [[cd3-bandeiracartão]] | &quot;visa&quot;, &quot;mastercard&quot; | Bandeira do cartão utilizada na compra |
| [[cd13-product-idadedoproduto]] |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[cd22-hit-metodopagamento]] |  &#039;cartao-de-credito&#039;, &#039;boleto&#039;  | Nome do tipo do pagamento. |
| [[cd23-hit-statuspagamento]] |  &#039;aprovado&#039;   | Nome do status do pagamento.  |
| [[product-ordemdeinserção]] | 1,2,3 .. | Ordem de inserção ao carrinho |
| [[product-tamanhodoproduto]] |  &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| [[product-cordoproduto]] | &#039;vermelho&#039; | Cor do produto  |
| [[product-padronagemdoproduto]] | &#039;florido&#039;,&#039;listado&#039; | Padrão da estampa do produto |
| [[cd28-hit-frete]] |  &#039;normal, expresso, entrega-agendada&#039;  | Nome do tipo de entrega.  |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] | &#039;12&#039;, &#039;produto-temporariamente-indisponivel&#039;,&#039;indisponivel&#039; etc | Preço do produto (dê). OBS: Caso o preço do mesmo apresente um erro e não traga a informação de preço, substituir a informação do “Preço DE” pelo “callback” do erro de preço.  |
| [[product-lifestyle]] | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[product-exclusivoecommerce+marketplace]] | &#039;sim:riachuelo&#039;, &#039;nao:riachuelo&#039;, &#039;nao:pontofrio&#039;, &#039;nao:extra&#039;, &#039;nao:sem-seller&#039; | Deve retornar se o produto é exclusivo do ecommerce e o seu id do seller/marketplace |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[cd46-hit-parcelamento]] |  &#039;2:sem-juros&#039;, &#039;6:com-juros&#039; e etc | Quantidade de parcelas e se foi com ou sem juros |
| [[descontofrete]] |  &#039;10&#039; | Valor do desconto de frete |
| [[descontofuncionário]] |  &#039;22&#039; | Valor do desconto para funcionário |
| [[descontopromoção]] |  &#039;15&#039; | Valor do desconto promocional - cupom, primeira compra, etc |
| [[totaldedesconto]] |  &#039;47&#039; | Soma de todos os descontos |
| [[cartãopresente]] | 100.00&#039;, &#039;50.00&#039; e etc | Deve retornar o valor pago por cartão presente utilizado. |
| [[valetroca]] | 40.00&#039;, &#039;23.00&#039;, &#039;200.00&#039; | Deve retornar o valor pago pelo vale troca utilizado. |
| [[pagamentocomum]] | 140.00&#039;, &#039;67.00&#039;, &#039;100.00&#039; | Deve retornar o valor pago em formas de  pagamentos comuns como, por exemplo, boleto, cartão de credito e etc |
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
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |

<br />

---

## Considerações Finais
> Recomendações do Google:
> 1. [Enhanced Ecommerce - Product Impressions](https://developers.google.com/tag-manager/enhanced-ecommerce#product-impressions)
> 2. [Enhanced Ecommerce - Product Clicks](https://developers.google.com/tag-manager/enhanced-ecommerce#product-clicks)
> 3. [Enhanced Ecommerce - Promotion View](https://developers.google.com/tag-manager/enhanced-ecommerce#promo-impressions)
> 4. [Enhanced Ecommerce - Promotion Clicks](https://developers.google.com/tag-manager/enhanced-ecommerce#promo-clicks)
> 5. [Enhanced Ecommerce - AddToCart / Remove From Cart](https://developers.google.com/tag-manager/enhanced-ecommerce#cart)
> 6. [Enhanced Ecommerce -Product Detail](https://developers.google.com/tag-manager/enhanced-ecommerce#details)
> 7. [Enhanced Ecommerce - Checkout](https://developers.google.com/tag-manager/enhanced-ecommerce#checkout)
> 8. [Enhanced Ecommerce - Checkout Option](https://developers.google.com/tag-manager/enhanced-ecommerce#checkout_option)
> 9. [Enhanced Ecommerce - Purchase](https://developers.google.com/tag-manager/enhanced-ecommerce#purchases)

<br/>

> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<script>
  document.addEventListener('DOMContentLoaded', function(event) {
    document.querySelectorAll('h1 a')[0].style.display = 'none';
  });
</script>
