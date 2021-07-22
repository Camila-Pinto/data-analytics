![Riachuelo](https://www.riachuelo.com.br/static/version1623357894/frontend/Corra/webjump/pt_BR/images/logo.svg)

> Área - Data Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados - Projeto Espelho Interativo
Última atualização: 27/11/2020 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />

### Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa&#231;&#227;o)
- [Geral](#geral)
- [Espelho Interativo - PLP](#espelho-interativo-plp)
- [Espelho Interativo - PDP](#espelho-interativo-pdp)
- [Espelho Interativo - Sacola](#espelho-interativo-sacola)
- [Espelho Interativo - Chamada do Vendedor](#espelho-interativo-chamada-do-vendedor)
- [Espelho Interativo - NPS](#espelho-interativo-nps)



## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [http://espelhov2.omni.riachuelo.net/](http://espelhov2.omni.riachuelo.net/).

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-T6RSN49');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-T6RSN49" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
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


### Geral

**Na interações entre os Cenários do Provador**<br />

- **Onde:** No Header das páginas do &quot;Espelho Interativo&quot;, seção &quot;Cenários&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:header',
    'eventAction': 'clique:cenario',
    'eventLabel': 'interacao:[[nome-cenario]]',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-cenario]] | &#039;praia&#039;, &#039;parque&#039;, &#039;viagem&#039; e etc. | Deve retornar a opção de cenário que o uuário escolheu. |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**Na interações com os botões &quot;Voltar&quot;, &quot;Play/Pausa&quot;, &quot;Dimunuir&quot; e etc.**<br />

- **Onde:** No Header das páginas do &quot;Espelho Interativo&quot;, seção &quot;Música&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:header',
    'eventAction': 'clique:opcoes:musica',
    'eventLabel': 'interacao:[[nome-botao]]',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;play&#039;, &#039;pausa&#039;, &#039;dimuniur&#039;, &#039;aumentar&#039; e etc. | Deve retornar o nome do botão que o usuário interagiu. |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**Na interação com o toggle para ativar/desativar o Espelho Interativo.**<br />

- **Onde:** No Header das páginas do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:header',
    'eventAction': 'interacao:toggle',
    'eventLabel': '[[on-ou-off]]',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[on-ou-off]] | &#039;on&#039; ou &#039;off&#039; | Deve retornar se o usuário ativou ou desativou o provador virtual. |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**No clique do botão &quot;Chamar Colaborador(a)&quot;**<br />

- **Onde:** No Footer das páginas do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:footer',
    'eventAction': 'clique-botao-chamar-colaborador',
    'eventLabel': 'chamar-colaborador',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


### Espelho Interativo - PLP

**No carregamento da pagina de PLP do Espelho Interativo**<br />

- **Onde:** Nas páginas de PLP do &quot;Espelho Interativo&quot;.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'virtual_pageview',
    'name': '[[nome-produto]]',     
    'id': '[[sku-produto-pai]]',
    'price': '[[preco-produto]]',
    'brand': '[[marca-produto]]',
    'category': '[[departamento-categoria-produto]]',
    'variant': '[[codigo-dco]]',
    'dimension4': '[[cd4-user-ip-loja]]',
    'dimension13': '[[cd13-product-idadedoproduto]]',
    'dimension27': '[[product-padronagemdoproduto]]',
    'dimension30': '[[product-subcategoria]]',
    'dimension32': '[[product-preçooriginal]]',
    'dimension38': '[[product-lifestyle]]',
    'dimension39': '[[product-gender]]',
    'dimension42': '[[product-skufilho]]'

  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] |  'calca-masculina-super-skinny-em-jeans' | Deve retornar o nome do produto|
| [[sku-produto-pai]] |   '13239635' | Deve retornar o SKU do produto - pai|
| [[preco-produto]] |   '139.99' | Deve retornar o preço do produto|
| [[marca-produto]] |   'pool-original' | Deve retornar a marca do produto|
| [[departamento-categoria-produto]] |   'masculino' | Deve retornar o departamento do produto|
| [[codigo-dco]] |  '325' | Deve retornar o código DCO - Categoria do produto|
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |
| [[cd13-product-idadedoproduto]] |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] |  &#039;12&#039; | Preço do produto (dê) |
| [[product-lifestyle]] | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |

<br />


**No clique do botão &quot;Começar de Novo&quot;**<br />

- **Onde:** Nas páginas de PLP do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:plp',
    'eventAction': 'clique:botao',
    'eventLabel': 'comecar-de-novo',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**No clique no Produto, cor ou tamanho**<br />

- **Onde:** Na página PLP do &quot;Espelho Interativo&quot;, seção &quot;Produtos no Provador&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:plp',
    'eventAction': 'clique:produtos-no-provador',
    'eventLabel': '[[opcao-clicada]]',
    'name': '[[nome-produto]]',     
    'id': '[[sku-produto-pai]]',
    'price': '[[preco-produto]]',
    'brand': '[[marca-produto]]',
    'category': '[[departamento-categoria-produto]]',
    'variant': '[[codigo-dco]]',
    'dimension4': '[[cd4-user-ip-loja]]',
    'dimension13': '[[cd13-product-idadedoproduto]]',
    'dimension25': '[[product-tamanhodoproduto]]',
    'dimension26': '[[product-cordoproduto]]',
    'dimension27': '[[product-padronagemdoproduto]]',
    'dimension30': '[[product-subcategoria]]',
    'dimension32': '[[product-preçooriginal]]',
    'dimension38': '[[product-lifestyle]]',
    'dimension39': '[[product-gender]]',
    'dimension42': '[[product-skufilho]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] |  'calca-masculina-super-skinny-em-jeans' | Deve retornar o nome do produto|
| [[sku-produto-pai]] |   '13239635' | Deve retornar o SKU do produto - pai|
| [[preco-produto]] |   '139.99' | Deve retornar o preço do produto|
| [[marca-produto]] |   'pool-original' | Deve retornar a marca do produto|
| [[departamento-categoria-produto]] |   'masculino' | Deve retornar o departamento do produto|
| [[codigo-dco]] |  '325' | Deve retornar o código DCO - Categoria do produto|
| [[opcao-clicada]] | &#039;produto&#039;, &#039;cor&#039; ou &#039;tamanho&#039; | Deve retornar a opção clicada. |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |
| [[cd13-product-idadedoproduto]] |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[product-tamanhodoproduto]] |  &#039;p&#039;, &#039;m&#039;, &#039;8-12&#039;, &#039;42&#039; | Tamanho do produto |
| [[product-cordoproduto]] | &#039;vermelho&#039; | Cor do produto  |
| [[product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] |  &#039;12&#039; | Preço do produto (dê) |
| [[product-lifestyle]] | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |

<br />


**No clique do botão &quot;Sacola&quot;**<br />

- **Onde:** Nas páginas de PLP do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:plp',
    'eventAction': 'clique:botao',
    'eventLabel': 'sacola',
    'dimension4': '[[cd4-user-ip-loja]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**No clique dos botões &quot;Adicionar à sacola&quot; e &quot;Remover da sacola&quot;**<br />

- **Onde:** Nas páginas de PLP do &quot;Espelho Interativo&quot;, seção &quot;Produtos no Provador&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:plp',
    'eventAction': 'clique:botao:sacola',
    'eventLabel': '[[adicionar-ou-remover]]',
    'name': '[[nome-produto]]',     
    'id': '[[sku-produto-pai]]',
    'brand': '[[marca-produto]]',
    'category': '[[departamento-categoria-produto]]',
    'variant': '[[codigo-dco]]',
    'dimension4': '[[cd4-user-ip-loja]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[adicionar-ou-remover]] | &#039;adicionar&#039; ou &#039;remover&#039; | Deve retornar se o usuário adicionou ou removeu um item da sacola. |
| [[nome-produto]] |  'calca-masculina-super-skinny-em-jeans' | Deve retornar o nome do produto|
| [[sku-produto-pai]] |   '13239635' | Deve retornar o SKU do produto - pai|
| [[marca-produto]] |   'pool-original' | Deve retornar a marca do produto|
| [[departamento-categoria-produto]] |   'masculino' | Deve retornar o departamento do produto|
| [[codigo-dco]] |  '325' | Deve retornar o código DCO - Categoria do produto|
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**No callback de erro, em todos os produtos carregados na PLP**<br />

- **Onde:** Nas páginas de PLP do &quot;Espelho Interativo&quot;, seção &quot;Produtos no Provador&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:plp',
    'eventAction': 'callback:produtos',
    'eventLabel': '[[sucesso-ou-erro]]',
    'name': '[[nome-produto]]',     
    'id': '[[sku-produto-pai]]',
    'brand': '[[marca-produto]]',
    'category': '[[departamento-categoria-produto]]',
    'variant': '[[codigo-dco]]',
    'dimension4': '[[cd4-user-ip-loja]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] |  'calca-masculina-super-skinny-em-jeans' | Deve retornar o nome do produto|
| [[sku-produto-pai]] |   '13239635' | Deve retornar o SKU do produto - pai|
| [[marca-produto]] |   'pool-original' | Deve retornar a marca do produto|
| [[departamento-categoria-produto]] |   'masculino' | Deve retornar o departamento do produto|
| [[codigo-dco]] |  '325' | Deve retornar o código DCO - Categoria do produto|
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**No callback do carregamento das imagens dos produtos no Provador Virtual**<br />

- **Onde:** Nas páginas de PLP do &quot;Espelho Interativo&quot;, seção &quot;Produtos no Provador&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:plp',
    'eventAction': 'callback:imagens',
    'eventLabel': '[[sucesso-ou-erro]]',
    'name': '[[nome-produto]]',     
    'id': '[[sku-produto-pai]]',
    'brand': '[[marca-produto]]',
    'category': '[[departamento-categoria-produto]]',
    'variant': '[[codigo-dco]]',
    'dimension4': '[[cd4-user-ip-loja]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] |  'calca-masculina-super-skinny-em-jeans' | Deve retornar o nome do produto|
| [[sku-produto-pai]] |   '13239635' | Deve retornar o SKU do produto - pai|
| [[marca-produto]] |   'pool-original' | Deve retornar a marca do produto|
| [[departamento-categoria-produto]] |   'masculino' | Deve retornar o departamento do produto|
| [[codigo-dco]] |  '325' | Deve retornar o código DCO - Categoria do produto|
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


### Espelho Interativo - PDP

**No carregamento da pagina de PDP do Espelho Interativo**<br />

- **Onde:** Nas páginas de PDP do &quot;Espelho Interativo&quot;.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'virtual_pageview',
    'name': '[[nome-produto]]',     
    'id': '[[sku-produto-pai]]',
    'price': '[[preco-produto]]',
    'brand': '[[marca-produto]]',
    'category': '[[departamento-categoria-produto]]',
    'variant': '[[codigo-dco]]',
    'dimension4': '[[cd4-user-ip-loja]]',
    'dimension13': '[[cd13-product-idadedoproduto]]',
    'dimension27': '[[product-padronagemdoproduto]]',
    'dimension30': '[[product-subcategoria]]',
    'dimension32': '[[product-preçooriginal]]',
    'dimension38': '[[product-lifestyle]]',
    'dimension39': '[[product-gender]]',
    'dimension42': '[[product-skufilho]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] |  'calca-masculina-super-skinny-em-jeans' | Deve retornar o nome do produto|
| [[sku-produto-pai]] |   '13239635' | Deve retornar o SKU do produto - pai|
| [[preco-produto]] |   '139.99' | Deve retornar o preço do produto|
| [[marca-produto]] |   'pool-original' | Deve retornar a marca do produto|
| [[departamento-categoria-produto]] |   'masculino' | Deve retornar o departamento do produto|
| [[codigo-dco]] |  '325' | Deve retornar o código DCO - Categoria do produto|
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |
| [[cd13-product-idadedoproduto]] |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] |  &#039;12&#039; | Preço do produto (dê) |
| [[product-lifestyle]] | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |

<br />


**No clique do botão &quot;Voltar&quot;**<br />

- **Onde:** Na página de PDP do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:pdp',
    'eventAction': 'clique:botao',
    'eventLabel': 'voltar',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**No clique dos botão &quot;Adicionar á sacola&quot;**<br />

- **Onde:** Na página de PDP do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:pdp',
    'eventAction': 'clique:botao',
    'eventLabel': 'adicionar-a-sacola',
    'name': '[[nome-produto]]',     
    'id': '[[sku-produto-pai]]',
    'brand': '[[marca-produto]]',
    'category': '[[departamento-categoria-produto]]',
    'variant': '[[codigo-dco]]',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] |  'calca-masculina-super-skinny-em-jeans' | Deve retornar o nome do produto|
| [[sku-produto-pai]] |   '13239635' | Deve retornar o SKU do produto - pai|
| [[marca-produto]] |   'pool-original' | Deve retornar a marca do produto|
| [[departamento-categoria-produto]] |   'masculino' | Deve retornar o departamento do produto|
| [[codigo-dco]] |  '325' | Deve retornar o código DCO - Categoria do produto|
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**No clique dos botões &quot;Solicitar Peça&quot; e &quot;Adicionar á sacola&quot;**<br />

- **Onde:** Na página de PDP do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:pdp',
    'eventAction': 'clique-botao-chamar-colaborador',
    'eventLabel': 'solicitar-peca',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


### Espelho Interativo - Sacola

**No carregamento da pagina da Sacola do Espelho Interativo**<br />

- **Onde:** Na página de Sacola do &quot;Espelho Interativo&quot;.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'virtual_pageview',
    'name': '[[nome-produto]]',     
    'id': '[[sku-produto-pai]]',
    'price': '[[preco-produto]]',
    'brand': '[[marca-produto]]',
    'category': '[[departamento-categoria-produto]]',
    'variant': '[[codigo-dco]]',
    'dimension4': '[[cd4-user-ip-loja]]',
    'dimension13': '[[cd13-product-idadedoproduto]]',
    'dimension27': '[[product-padronagemdoproduto]]',
    'dimension30': '[[product-subcategoria]]',
    'dimension32': '[[product-preçooriginal]]',
    'dimension38': '[[product-lifestyle]]',
    'dimension39': '[[product-gender]]',
    'dimension42': '[[product-skufilho]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] |  'calca-masculina-super-skinny-em-jeans' | Deve retornar o nome do produto|
| [[sku-produto-pai]] |   '13239635' | Deve retornar o SKU do produto - pai|
| [[preco-produto]] |   '139.99' | Deve retornar o preço do produto|
| [[marca-produto]] |   'pool-original' | Deve retornar a marca do produto|
| [[departamento-categoria-produto]] |   'masculino' | Deve retornar o departamento do produto|
| [[codigo-dco]] |  '325' | Deve retornar o código DCO - Categoria do produto|
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |
| [[cd13-product-idadedoproduto]] |  &#039;X-day&#039; | Idade do produto cadastrado no Magento (checkout) |
| [[product-padronagemdoproduto]] | &#039;florido&#039;, &#039;listado&#039; | Padrão da estampa do produto |
| [[product-subcategoria]] |  &#039;310090&#039; | Código da categoria GM  |
| [[product-preçooriginal]] |  &#039;12&#039; | Preço do produto (dê) |
| [[product-lifestyle]] | &#039;casual&#039;, &#039;esportivo&#039; | Estilo do produto  |
| [[product-gender]] | &#039;unisex&#039;, &#039;feminino&#039; | Genero do produto |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |

<br />


**No clique do botão &quot;Voltar&quot;**<br />

- **Onde:** Na página de Sacola do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:sacola',
    'eventAction': 'clique:botao',
    'eventLabel': 'voltar',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**No clique do botões &quot;Remover da Sacola&quot;**<br />

- **Onde:** Na página de Sacola do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:sacola',
    'eventAction': 'clique:botao',
    'eventLabel': 'remover-da-sacola',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**No clique do botão &quot;Pagamento&quot;**<br />

- **Onde:** Na página de Sacola do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:sacola',
    'eventAction': 'clique:botao',
    'eventLabel': 'pagamento',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


### Espelho Interativo - Chamada do Vendedor

**No carregamento do modal &quot;Nosso colaborador(a) já esta a caminho&quot;**<br />

- **Onde:** No modal de &quot;Chamada do vendedor&quot; do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'virtual_pageview',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-de-acesso]] | &#039;chamar-colaborador-footer&#039;, &#039;solicitar-peca-pdp&#039; ou &#039;pagamento-sacola&#039; | Deve retornar de qual botão veio a chamada do vendedor. |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


**No clique dos botões &quot;Cancelar&quot; ou &quot;Finalizar&quot;**<br />

- **Onde:** No modal de &quot;Chamada do vendedor&quot; do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:modal',
    'eventAction': 'clique:botao:chamada-do-vendedor',
    'eventLabel': '[[nome-botao]]',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;cancelar&#039; ou &#039;finalizar&#039; | Deve retornar nome do botão clicado. |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

<br />


### Espelho Interativo - NPS

**No clique das opções disponíveis no NPS**<br />

- **Onde:** Na página de NPS do &quot;Espelho Interativo&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'espelho-interativo:nps',
    'eventAction': 'clique:opcoes',
    'eventLabel': '[[opcao-clicada]]',
    'dimension4': '[[cd4-user-ip-loja]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-clicada]] | &#039;odiei&#039;, &#039;nao-gostei&#039;, &#039;sei-la&#039; e etc | Deve retornar nome da opção clicada. |
| [[cd4-user-ip-loja]] |  &#039;1283798723&#039;, &#039;32746234&#039; e etc | Deve retornar o IP da loja que o cliente que esta fazendo sua experiência do provador |

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
