![Riachuelo](https://www.riachuelo.com.br/static/version1623357894/frontend/Corra/webjump/pt_BR/images/logo.svg)

> Área - Data Analytics<br />
> Documento de Especificação Técnica

<br />

## Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Dimensões Globais](#dimens&#245;es-globais)

<br />

## Implementação da Camada de dados - Riachuelo - Carters
Última atualização: 07/12/2020 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [www.carters.com.br](www.carters.com.br).

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-5BWM275');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-5BWM275" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
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
    'dimension1': '[[cd1-user-usermagentoid]]',
    'dimension5': '[[cd5-hit-loginstatus]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd1-user-usermagentoid]] | &quot;0123456&quot; | ID único do usuário definido após cadastro (MagentoID) |
| [[cd5-hit-loginstatus]] | &#039;logged-in&#039;, &#039;logged-out&#039; | Status do login do usuário |

---


### General


**No carregamento do modal lateral  - Sua Sacola sem produtos ou com produtos;**<br />

- **Onde:** No modal lateral, em todas as páginas.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'carters:geral',
    'eventAction': 'visualizou-modal-lateral',
'noInteraction': '1',
    'eventLabel': 'sacola:[[com ou sem]]-produtos'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[com ou sem]] | &#039;sacola:com-produtos&#039;, &#039;sacola:sem-produtos&#039; | Deve retornar se a sacola está com ou sem produtos. |

<br />


**No carregamento do modal de login para confirmar o cpf e senha**<br />

- **Onde:** No modal lateral, em todas as páginas.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'carters:geral',
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


**No clique dos links do header**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;riachuelo&quot;(logo), &quot;login&quot;, &quot;minha-conta&quot; etc
    
```html
<div
   data-gtm-event-category='carters:geral'
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
   data-gtm-event-category='carters:geral'
   data-gtm-event-action='clique:menu-superior'
   data-gtm-event-label='[[nome-clicacdo]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-clicacdo]] | &#039;novidades&#039;, &#039;masculino&#039;, &#039;feminino&#039;, &#039;alfaiataria&#039;, &#039;outlet&#039;, &#039;busca&#039;, &#039;camisetas, tenis, polo_manga_curta&#039;, &#039;super_skinny&#039;, &#039;regular&#039;, &#039;feminino:vestido_feminino_colecao_feminina&#039;, &#039;feminino:busca_feminino_colecao_feminina&#039; | Deve retornar o nome do link principal do menu. |

<br />


**No clique dos links interno do footer**<br />

- **Onde:** Em todas as páginas em que estiverem disponíveis
    - **Titulo ou nome do botão/link:** &quot;Carter&#039;s&quot;, &quot;Pool&quot;, &quot;Jeans&quot;, &quot;Geek&quot; etc
    
```html
<div
   data-gtm-event-category='carters:geral'
   data-gtm-event-action='clique:footer:link'
   data-gtm-event-label='[[nome-categoria]]:[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-categoria]] | &quot;cartao-riachuelo&quot;, &quot;sobre-a-riachuelo&quot;, &quot;moda-que-transforma&quot; e etc | Deve retornar o nome da categoria do footer. |
| [[nome-link]] | &#039;saiba-como-adquirir &#039;a-empresa&#039;, &#039;entre-costuras&#039;  e etc | Deve retornar o nome do link do footer clicado. |

<br />


**Após o preenchimento do campo do newsletter**<br />

- **Onde:** No cadastro de newsletter, localizado no footer em todas as páginas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'carters:geral',
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
    'eventCategory': 'carters:geral',
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
   data-gtm-event-category='carters:geral'
   data-gtm-event-action='clique:link:rede-social:footer'
   data-gtm-event-label='[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &quot;riachuelo-facebook&quot;,  &quot;riachuelo-youtube&quot;, &quot;riachuelo-linkedin&quot; e etc | Deve retornar o nome do link da rede social clicada. |

<br />


### Login e  Cadastro

**Ao clicar no botão de “Cadastre-se usando suas Redes Sociais”**<br />

- **Onde:** Modal de Cadastro/Login.
    
```html
<div
   data-gtm-event-category='carters:login'
   data-gtm-event-action='clique:cadastre-com-redes-sociais'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
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
    'eventCategory': 'carters:login',
    'eventAction': 'preencheu:campo',
    'eventLabel': '[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;cpf&#039; , &#039;senha&#039; | Deve retornar o nome do campo preenchido. |

<br />


**No clique do link &quot;Esqueceu a sua senha? Clique aqui&quot;**<br />

- **Onde:** No modal Entrar/Criar Conta
    
```html
<div
   data-gtm-event-category='carters:login'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='esqueceu-senha'
>Botão</div>
```



<br />


**No callback da tentativa de login**<br />

- **Onde:** No modal Entrar/Criar Conta
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'carters:login',
    'eventAction': 'tentativa:callback',
    'eventLabel': '[[sucesso ou tipo-erro]]',
    'dimension1': '[[cd1-user-usermagentoid]]',
    'dimension5': '[[cd5-hit-loginstatus]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-erro]] | &#039;sucesso&#039;, &#039;voce-nao-digitou-os-dados-corretamente&#039;, etc | Deve retornar o sucesso ou descrição do erro da tentativa de login |
| [[cd1-user-usermagentoid]] | &quot;0123456&quot; | ID único do usuário definido após cadastro (MagentoID) |
| [[cd5-hit-loginstatus]] | &#039;logged-in&#039;, &#039;logged-out&#039; | Status do login do usuário |

<br />


**No preenchimento dos campos do formulário de cadastro.**<br />

- **Onde:** Na página de cadastro.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'carters:cadastro',
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
   data-gtm-event-category='carters:cadastro'
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
    'eventCategory': 'carters:cadastro',
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
   data-gtm-event-category='carters:cadastro'
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
    'event': 'event',
    'eventCategory': 'carters:minha-conta',
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
    'event': 'event',
    'eventCategory': 'carters:[[plp ou pdp]]',
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
    'eventCategory': 'carters:pdp',
    'eventAction': 'interacao:tabela-de-medidas',
    'eventLabel': '[[abriu ou fechou]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[abriu ou fechou]] | &#039;abriu&#039;, &#039;fechou&#039; | Deve retornar a ação do usuário. |

<br />


### Filtro PLP

**No clique nos filtros nas páginas de Busca e de PLPs (OBS: Caso seja selecionado mais de um filtro, retornar de forma concatenada, separando por ponto e vírgula ( ; ). Ex: &#039;plp:tamanho:46;48&#039; )**<br />

- **Onde:** Na página de busca, após buscar algum termo e também nas páginas de Listas de Produtos
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'carters:filtro',
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
   data-gtm-event-category='carters:filtro'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[localizacao-filtro]]:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[localizacao-filtro]] | &#039;busca&#039;, &#039;plp&#039; | Deve retornar a localização do filtro. |
| [[nome-botao]] | &#039;limpar&#039; ou &#039;aplicar&#039; | Deve retornar o nome do botão. |

<br />


### Checkout

**Na interação com os campos do formulário de cadastro de cliente.**<br />

- **Onde:** Na etapa de dados pessoais do checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'carters:checkout',
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
    'eventCategory': 'carters:checkout',
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


**No carregamento dos tipos de entrega disponíveis**<br />

- **Onde:** Na página de carrinho e checkout
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'carters:checkout',
    'eventAction': 'tipos-de-entrega:[[cart ou checkout]]',
'noInteraction': '1',
    'eventLabel': '[{&quot;t&quot;:&quot;[[nome-entrega]]&quot;,&quot;d&quot;:&quot;[[previsao]]&quot;,&quot;p&quot;:&quot;[[valor]]&quot;},{&quot;t&quot;:&quot;[[nome-entrega]]&quot;,&quot;d&quot;:&quot;[[previsao]]&quot;,&quot;p&quot;:&quot;[[valor]]&quot;}]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cart ou checkout]] | &#039;checkout&#039; ou &#039;cart&#039; | Deve retornar o local que a lista de tipos de entrega aparece. |
| [[nome-entrega]] | &#039;normal&#039;, &#039;expresso &#039;, &#039;entrega-agendada&#039;,&#039;retirar-em-loja&#039; | Deve retornar os nomes dos tipos de entrega. |
| [[previsao]] | &#039;DD/MM/AAAA&#039;, | Deve retornar a quantidade de dias para a entrega. |
| [[valor]] | &#039;&#039;0.00&#039;, &#039;2.99&#039;, &#039;5.00&#039; | Deve retornar os valores dos fretes. |

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
  'eventAction': 'promoImpressions',
  'noInteraction': '1',
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
| [[id-promocao]] | &quot;https://midia.fotos-riachuelo.com.br/fotos/megamenu/home/janeiro2020/ADS-5506/Carrossel-home-mcasa-kit-colcha_.jpg&quot; e etc | Deve retornar o link da imagem do banner |
| [[nome-promocao]] | &quot;polos-diferenciadas&quot; | Deve retornar o nome amigável do banner |
| [[arte-banner]] | &quot;hero;clp;banner:clicavel&quot;, &quot;mosaico;blog;banner:nao-clicavel&quot; e etc | Deve retornar o formato, o tipo de página que o banner foi clicado e o tipo do banner |
| [[posicao-promocao]] | &quot;1&quot; | Deve retornar a posição que o banner é exibido |

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
| [[arte-banner]] | &quot;hero;clp;banner:clicavel&quot;, &quot;mosaico;blog;banner:nao-clicavel&quot; e etc | Deve retornar o formato, o tipo de página que o banner foi clicado e o tipo do banner |
| [[posicao-promocao]] | &quot;1&quot; | Deve retornar a posição que o banner é exibido |

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
'dimension20': '[[cd20-product-selo/tag]]',
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
| [[cd20-product-selo/tag]] | &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. OBS: Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
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
'dimension7': '[[cd7-product-idadedoproduto]]',
  'dimension10': '[[cd10-product-ordemdeinserção]]',
  'dimension13': '[[cd13-product-padronagemdoproduto]]',
  'dimension15': '[[cd15-product-subcategoria]]',
  'dimension16': '[[cd16-product-preçooriginal]]',
  'dimension17': '[[cd17-product-gender]]',
  'dimension18': '[[cd18-product-skufilho]]',
  'dimension20': '[[cd20-product-selo/tag]]',
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
| [[cd7-product-idadedoproduto]] | &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[cd10-product-ordemdeinserção]] | 1,2,3 .. | Ordem de inserção ao carrinho |
| [[cd13-product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[cd15-product-subcategoria]] | &#039;310090&#039; | Código da categoria GM |
| [[cd16-product-preçooriginal]] | &#039;12&#039; | Preço do produto (dê) |
| [[cd17-product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[cd18-product-skufilho]] | &#039;2552&#039; | ID filho do produto |
| [[cd20-product-selo/tag]] | &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. OBS: Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
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
'dimension7': '[[cd7-product-idadedoproduto]]',
  'dimension10': '[[cd10-product-ordemdeinserção]]',
  'dimension13': '[[cd13-product-padronagemdoproduto]]',
  'dimension15': '[[cd15-product-subcategoria]]',
  'dimension16': '[[cd16-product-preçooriginal]]',
  'dimension17': '[[cd17-product-gender]]',
  'dimension18': '[[cd18-product-skufilho]]',
  'dimension20': '[[cd20-product-selo/tag]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
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
| [[cd7-product-idadedoproduto]] | &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[cd10-product-ordemdeinserção]] | 1,2,3 .. | Ordem de inserção ao carrinho |
| [[cd13-product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[cd15-product-subcategoria]] | &#039;310090&#039; | Código da categoria GM |
| [[cd16-product-preçooriginal]] | &#039;12&#039; | Preço do produto (dê) |
| [[cd17-product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[cd18-product-skufilho]] | &#039;2552&#039; | ID filho do produto |
| [[cd20-product-selo/tag]] | &#039;selo:black-friday&#039;, &#039;tag:frete-gratis&#039; e etc | Deve retornar o tipo do selo ou tag e a promoção. OBS: Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por &quot; ; &quot; (Ponto e vírgula) |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |

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
'dimension7': '[[cd7-product-idadedoproduto]]',
  'dimension10': '[[cd10-product-ordemdeinserção]]',
  'dimension11': '[[cd11-product-tamanhodoproduto]]',
  'dimension12': '[[cd12-product-cordoproduto]]',
  'dimension13': '[[cd13-product-padronagemdoproduto]]',
  'dimension15': '[[cd15-product-subcategoria]]',
  'dimension16': '[[cd16-product-preçooriginal]]',
  'dimension17': '[[cd17-product-gender]]',
  'dimension18': '[[cd18-product-skufilho]]',
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
| [[cd7-product-idadedoproduto]] | &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[cd10-product-ordemdeinserção]] | 1,2,3 .. | Ordem de inserção ao carrinho |
| [[cd11-product-tamanhodoproduto]] | &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| [[cd12-product-cordoproduto]] | &#039;vermelho&#039; | Cor do produto |
| [[cd13-product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[cd15-product-subcategoria]] | &#039;310090&#039; | Código da categoria GM |
| [[cd16-product-preçooriginal]] | &#039;12&#039; | Preço do produto (dê) |
| [[cd17-product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[cd18-product-skufilho]] | &#039;2552&#039; | ID filho do produto |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |

<br />


**No clique para remover produto do carrinho**<br />

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
'dimension7': '[[cd7-product-idadedoproduto]]',
  'dimension10': '[[cd10-product-ordemdeinserção]]',
  'dimension11': '[[cd11-product-tamanhodoproduto]]',
  'dimension12': '[[cd12-product-cordoproduto]]',
  'dimension13': '[[cd13-product-padronagemdoproduto]]',
  'dimension15': '[[cd15-product-subcategoria]]',
  'dimension16': '[[cd16-product-preçooriginal]]',
  'dimension17': '[[cd17-product-gender]]',
  'dimension18': '[[cd18-product-skufilho]]',
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
| [[cd7-product-idadedoproduto]] | &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[cd10-product-ordemdeinserção]] | 1,2,3 .. | Ordem de inserção ao carrinho |
| [[cd11-product-tamanhodoproduto]] | &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| [[cd12-product-cordoproduto]] | &#039;vermelho&#039; | Cor do produto |
| [[cd13-product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[cd15-product-subcategoria]] | &#039;310090&#039; | Código da categoria GM |
| [[cd16-product-preçooriginal]] | &#039;12&#039; | Preço do produto (dê) |
| [[cd17-product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[cd18-product-skufilho]] | &#039;2552&#039; | ID filho do produto |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |
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
'dimension7': '[[cd7-product-idadedoproduto]]',
  'dimension10': '[[cd10-product-ordemdeinserção]]',
  'dimension11': '[[cd11-product-tamanhodoproduto]]',
  'dimension12': '[[cd12-product-cordoproduto]]',
  'dimension13': '[[cd13-product-padronagemdoproduto]]',
  'dimension15': '[[cd15-product-subcategoria]]',
  'dimension16': '[[cd16-product-preçooriginal]]',
  'dimension17': '[[cd17-product-gender]]',
  'dimension18': '[[cd18-product-skufilho]]',
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
| [[cd7-product-idadedoproduto]] | &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[cd10-product-ordemdeinserção]] | 1,2,3 .. | Ordem de inserção ao carrinho |
| [[cd11-product-tamanhodoproduto]] | &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| [[cd12-product-cordoproduto]] | &#039;vermelho&#039; | Cor do produto |
| [[cd13-product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[cd15-product-subcategoria]] | &#039;310090&#039; | Código da categoria GM |
| [[cd16-product-preçooriginal]] | &#039;12&#039; | Preço do produto (dê) |
| [[cd17-product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[cd18-product-skufilho]] | &#039;2552&#039; | ID filho do produto |
| [[nome-produto]] | &quot;calca-masculina-super-skinny-em-jeans&quot; | Nome do produto |
| [[id-produto]] | &quot;13239635&quot; | SKU do produto - pai |
| [[preco-produto]] | &quot;139.99&quot; | Preço do produto |
| [[marca-produto]] | &quot;pool-original&quot; | Marca do produto |
| [[categoria-produto]] | &quot;masculino&quot; | Departamento do produto |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |
| [[passo-checkout]] | &quot;1&quot; | No clique de Fechar Pedido no rodapé do modal de Sua Sacola |
| [[passo-checkout]] | &quot;2&quot; | No carregamento do modal de indentificação com CPF |
| [[passo-checkout]] | &quot;3&quot; | No carregamento da página de formulário de cadastro |
| [[passo-checkout]] | &quot;4&quot; | No carregamento da página de confirmação do endereço de entrega |
| [[passo-checkout]] | &quot;5&quot; | No carregamento da página de seleção do método de pagamento |

<br />


**Ao selecionar uma opção de pagamento ou frete**<br />

- **Onde:** Na página de carrinho e de checkout
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'checkoutOption',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'checkoutOption',
  'eventLabel': 'tipode-de-[[pagamento ou frete]]-method',
    'ecommerce': {
    'checkout_option': {
        'actionField':  {'step':'4','option':'frete:[[opcao escolhida]]:[[previsao-entrega]]'},
    }
  }
});
</script>
```

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'checkoutOption',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'checkoutOption',
  'eventLabel': 'tipode-de-[[pagamento ou frete]]-method',
    'ecommerce': {
    'checkout_option': {
        'actionField':  {'step':'5','option':'pagamento:[[opcao escolhida]]'},
    }
  }
});
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao escolhida]] | &#039;cartao-de-credito&#039;, &#039;boleto&#039;, &#039;normal&#039;, &#039;expresso &#039;, &#039;entrega-agendada&#039;,&#039;retirar-em-loja:shopping-tiete-plaza&#039;, &#039;retirar-em-loja:shopping-bourbon-sp&#039; | Deve retornar o nome da opção de pagamento ou entrega escolhida. |
| [[valor]] | &#039;&#039;0.00&#039;, &#039;2.99&#039;, &#039;5.00&#039;. | Deve retornar os valores dos fretes. |
| [[opcao-adicional]] | &#039;vale-troca&#039; | Deve retornar a opção de pagamento adicional. |
| [[previsao]] | &#039;3-dias, 4-dias,5-dias&#039; | Deve retornar a quantidade de dias. |
| [[pagamento ou frete]] | &#039;pagamento&#039; ou &#039;frete&#039; | Deve retornar o nome da etapa do checkout. |

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
'dimension4': '[[cd4-tipodecartão]]',
  'dimension6': '[[cd6-bandeiracartão]]',
  'dimension8': '[[cd8-hit-metodopagamento]]',
  'dimension9': '[[cd9-hit-statuspagamento]]',
  'dimension14': '[[cd14-hit-frete]]',
  'dimension19': '[[cd19-hit-parcelamento]]',
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
'dimension10': '[[cd10-product-ordemdeinserção]]',
  'dimension11': '[[cd11-product-tamanhodoproduto]]',
  'dimension12': '[[cd12-product-cordoproduto]]',
  'dimension13': '[[cd13-product-padronagemdoproduto]]',
  'dimension15': '[[cd15-product-subcategoria]]',
  'dimension16': '[[cd16-product-preçooriginal]]',
  'dimension17': '[[cd17-product-gender]]',
  'dimension18': '[[cd18-product-skufilho]]',
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
| [[cd4-tipodecartão]] | &#039;credito&#039;, &#039;riachuelo&#039; | Tipo do cartão utilizado na compra |
| [[cd6-bandeiracartão]] | &quot;visa&quot;, &quot;mastercard&quot; | Bandeira do cartão utilizada na compra |
| [[cd8-hit-metodopagamento]] | &#039;cartao-de-credito&#039;, &#039;boleto&#039; | Nome do tipo do pagamento. |
| [[cd9-hit-statuspagamento]] | &#039;aprovado&#039; | Nome do status do pagamento. |
| [[cd10-product-ordemdeinserção]] | 1,2,3 .. | Ordem de inserção ao carrinho |
| [[cd11-product-tamanhodoproduto]] | &#039;p&#039;,&#039;&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; | Tamanho do produto |
| [[cd12-product-cordoproduto]] | &#039;vermelho&#039; | Cor do produto |
| [[cd13-product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[cd14-hit-frete]] | &#039;normal, expresso, entrega-agendada&#039; | Nome do tipo de entrega. |
| [[cd15-product-subcategoria]] | &#039;310090&#039; | Código da categoria GM |
| [[cd16-product-preçooriginal]] | &#039;12&#039; | Preço do produto (dê) |
| [[cd17-product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[cd18-product-skufilho]] | &#039;2552&#039; | ID filho do produto |
| [[cd19-hit-parcelamento]] | &#039;2&#039; | Quantidade de parcelas |
| [[descontofrete]] | &#039;10&#039; | Valor do desconto de frete |
| [[descontofuncionário]] | &#039;22&#039; | Valor do desconto para funcionário |
| [[descontopromoção]] | &#039;15&#039; | Valor do desconto promocional - cupom, primeira compra, etc |
| [[totaldedesconto]] | &#039;47&#039; | Soma de todos os descontos |
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
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |

<br />


### 

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
