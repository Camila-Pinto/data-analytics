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
- [Login](#login)
- [Lista de Presentes - Visão Convidados](#lista-de-presentes---vis&#227;o-convidados)
- [Página de Busca](#p&#225;gina-de-busca)
- [Enhanced E-commerce](#enhanced-e-commerce)

<br />

## Implementação da Camada de dados - Projeto Do Meu Jeito
Última atualização: 26/10/2021 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [http://domeujeito.com.br/](http://domeujeito.com.br/).

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-T86MKXC');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-T86MKXC" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
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
    'dimension13': '[[quantidadepresentes]]',
    'dimension14': '[[tipolista]]',
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[userid]]` | &quot;01234&quot; | ID único de usuário definido após o cadastro |
| `[[quantidadepresentes]]` | &quot;45-itens&quot;, etc | Deve retornar a quantidade de presentes que os noivos adicionaram na lista |
| `[[tipolista]]` | &quot;cha-de-bebe&quot;, &#039;lista-de-casamento&#039;, &#039;open-house&#039; e etc. | Deve retornar o tipo de lista selecionada |

---

### Geral 

**No clique dos itens do footer.**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:geral',
    'eventAction': 'clique:footer',
    'eventLabel': '[[nome-item]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[nome-item]]` | 'politica-de-privacidade', 'termos-e-condicoes-de-uso', 'acessibilidade' e etc | Deve retornar o nome do item clicado. |

<br />

**No clique dos itens do Header.**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:geral',
    'eventAction': 'clique:header',
    'eventLabel': '[[nome-item]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[nome-item]]` | 'logo-riachuelo', 'logo-do-meu-jeito', 'perguntas-frequentes', 'como-funciona', 'beneficios', 'acesse-sua-conta' e etc | Deve retornar o nome do item clicado. |

<br />


**No clique do ícone &quot;Acessar sacola&quot;**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:geral',
    'eventAction': 'clique:icone',
    'eventLabel': 'acessar-sacola'
  });
</script>
```

<br />

**Na tentativa de callback para enviar o modal Nps.**<br />

- **Onde:** Em todas as páginas que estiver disponível;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:nps',
    'eventAction': 'callback:enviar:modal',
    'eventLabel': '[[opcao]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| [[opcao]] | &#039;nao-gostei&#039;, &#039;gostei&#039;, &#039;sei-la&#039; e etc | Deve retornar o nome da opção clicada. |

<br />

### Home 

**Na interação com os campos "Buscar uma Lista".**<br />

- **Onde:** Na página Home.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:home',
    'eventAction': 'interacao:campo',
    'eventLabel': '[[nome-campo]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[nome-campo]]` | 'nome-dos-anfitrioes', 'data-do-evento' e etc | Retorna o nome do campo interagido.|

<br />

**No clique dos botões.**<br />

- **Onde:** Na página Home.

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:home',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[nome-botao]]` | 'buscar', 'criar-lista', 'criar-lista-gratis' e etc | Retorna o nome do botão clicado. |

<br />

**Na interação para abrir ou fechar uma pergunta**<br />

- **Onde:** Na página Home, no menu Perguntas Frequentes
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:home',
    'eventAction': 'interacao:perguntas-frequentes',
    'eventLabel': '[[nome-pergunta]]:[[acao]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[nome-pergunta]]` | 'o-que-e-do-meu-jeito-com-br', 'como-compartilhar-minha-lista' e etc.
 | Retorna o nome da pergunta.  |
| `[[acao]]` | 'abrir' ou 'fechar'. | Retorna a ação do usuário. |

<br />

**Na interação com o vídeo de "Como Funciona?"**<br />

- **Onde:** Na página Home, seção de "Como Funciona?"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:home',
    'eventAction': 'interacao:video:como-funciona',
    'eventLabel': '[[acao]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[acao]]` | 'play' ou 'pause' | Retorna a ação do usuário com o video.  |

<br />

### Login

**No clique dos links no fluxo de Login**<br />

- **Onde:** Nas página de login, todas as etapas
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:login:[[etapa]]',
    'eventAction': 'clique:link',
    'eventLabel': '[[nome-link]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[etapa]]` | 'dados', 'senha', 'lista' ou 'evento' | Retorna a etapa que o usuário está. |
| `[[nome-link]]` | 'ja-tem-uma-conta-entre', 'termos-de-adesao' e etc | Retorna o nome do link clicado. |

<br />

**No clique no botão "Continuar" para cada etapa**<br />

- **Onde:** Nas páginas de login, todas as etapas
- **Obs:** Na etapa de "Lista", retornar o tipo de lista escolhido. Para as outras etapas, podemos desconsiderar a dimension14
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:login:[[etapa]]',
    'eventAction': 'clique:botao',
    'eventLabel': 'continuar',
    'dimension14': '[[tipo-de-lista]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[etapa]]` | 'dados', 'senha', 'lista' ou 'evento' | Retorna a etapa que o usuário está. |
| `[[tipo-de-lista]]` | 'cha-de-bebe', 'lista-de-casamento', 'open-house' e etc | Retorna o tipo de lista selecionada |

<br />

**Na interação com o checkbox de "Aceite os termos"**<br />

- **Onde:** Na página de login, etapa de "Senha"
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:login:[[etapa]]',
    'eventAction': 'interacao:checkbox',
    'eventLabel': 'aceito-os-termos'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[etapa]]` | 'dados', 'senha', 'lista' ou 'evento' | Retorna a etapa que o usuário está. |

<br />

### Lista de Presentes - Visão Convidados

**Na interação com o campo de Busca**<br />

- **Onde:** Na página de Lista de Presentes, visão Convidados
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:lista-de-presentes:convidados',
    'eventAction': 'interacao:busca',
    'eventLabel': '[[termo-buscado]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[termo-buscado]]` | 'cafeteira', 'televisao', 'conjunto-de-mesa' e etc | Retorna o termo buscado pelo usuário. |

<br />

**Na interação com o filtro "Ordenar Por"**<br />

- **Onde:** Na página de Lista de Presentes, visão Convidados
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:lista-de-presentes:convidados',
    'eventAction': 'interacao:filtro:ordenar-por',
    'eventLabel': '[[filtro-selecionado]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[filtro-selecionado]]` | 'maior-valor', 'menor-valor', 'personalizado' e etc | Retorna o filtro selecionado.  |

<br />

### Página de Busca

**Na interação com os tipos de Busca**<br />

- **Onde:** Na página de Busca
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:buscar-lista',
    'eventAction': 'interacao:campo:[[nome-campo]]',
    'eventLabel': '[[termo-buscado]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[nome-campo]]` | 'insira-aqui-os-nomes', 'data', 'tipo-de-lista' e etc | Retorna o nome do campo preenchido. |
| `[[termo-buscado]]` | 'joao-marcos', '25/12', 'aniversario', 'cha-bebe' e etc | Retorna o termo buscado pelo usuário.  |

<br />

**No clique do botão "Buscar"**<br />

- **Onde:** Na página de Busca
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:buscar-lista',
    'eventAction': 'clique:botao',
    'eventLabel': 'buscar'
  });
</script>
```

<br />

**No clique do botão "Ver Lista"**<br />

- **Onde:** Na página de Busca
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:buscar-lista',
    'eventAction': 'clique:botao',
    'eventLabel': 'ver-lista',
    'dimension14': '[[tipo-de-lista]]'
  });
</script>
```

| Variável        | Exemplo        | Descrição            |
| :-------------- | :------------- | :--------------------|
| `[[tipo-de-lista]]` | 'cha-de-bebe', 'lista-de-casamento', 'open-house' e etc | Retorna o tipo de lista selecionada |

<br />

### Enhanced E-commerce

**Na visualização de uma vitrine de produtos**<br />

- **Onde:** Em todas as páginas que exibirem uma lista de produtos (CLP, PLP, PDP e etc)

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productImpression',
  'noInteraction': '1',
  'eventCategory':'enhanced-ecommerce',
  'eventAction': 'impressions',
  'dimension12': '[[dm12-vendedor]]',
  'ecommerce': {
    'impressions': [{
      'name': '[[nome-produto]]',
      'id': '[[id-produto]]',
      'list': '[[lista-produto]]',
      'variant': '[[variacao-produto]]',
      'position': '[[posicao-produto]]',
      'brand': '[[marca-produto]]',
      'category': '[[categoria-produto]]',
      'price': '[[preco-produto]]',
      'dimension11': '[[dm11-product-subcategoria]]'
    }]
  }
});
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[nome-produto]]` | 'cafeteira' | Nome do produto |
| `[[id-produto]]` | '13239635' | SKU do produto - pai |
| `[[lista-produto]]` | 'decoracao' | Nome da lista que o produto aparece |
| `[[posicao-produto]]` | '2' | Posição que o produto aparece em uma lista de produtos |
| `[[variacao-produto]]` | '325' | Código DCO - Categoria do produto |
| `[[marca-produto]]` |'pool-original' | Marca do produto |
| `[[categoria-produto]]` |'masculino' | Departamento do produto |
| `[[preco-produto]]` | '310090' | Preço do produto |
| `[[dm11-product-subcategoria]]` | '310090' | Código da categoria GM  |
| `[[dm12-vendedor]]` | 'riachuelo', etc | Deve retornar o nome da loja que esta vendendo o produto |

<br />

**No clique dos produtos da vitrine interagidos na página**<br />

- **Onde:** Em todas as páginas que exibirem uma lista de produtos (CLP, PLP, PDP e etc)

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'productClick',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'click',
  'dimension12': '[[dm12-vendedor]]',
  'ecommerce': {
    'click': {
      'actionField': {'list': '[[lista-produto]]'},
        'products': [{
          'name': '[[nome-produto]]',
          'id': '[[id-produto]]',
          'list': '[[lista-produto]]',
          'variant': '[[variacao-produto]]',
          'position': '[[posicao-produto]]',
          'brand': '[[marca-produto]]',
          'category': '[[categoria-produto]]',
          'price': '[[preco-produto]]',
          'dimension11': '[[dm11-product-subcategoria]]'
        }]
    }
  }
});
</script>
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[nome-produto]]` | 'cafeteira' | Nome do produto |
| `[[id-produto]]` | '13239635' | SKU do produto - pai |
| `[[lista-produto]]` | 'decoracao' | Nome da lista que o produto aparece |
| `[[posicao-produto]]` | '2' | Posição que o produto aparece em uma lista de produtos |
| `[[variacao-produto]]` | '325' | Código DCO - Categoria do produto |
| `[[marca-produto]]` |'pool-original' | Marca do produto |
| `[[categoria-produto]]` |'masculino' | Departamento do produto |
| `[[preco-produto]]` | '310090' | Preço do produto |
| `[[dm11-product-subcategoria]]` | '310090' | Código da categoria GM  |
| `[[dm12-vendedor]]` | 'riachuelo', etc | Deve retornar o nome da loja que esta vendendo o produto |

<br />

**Ao adicionar um produto da lista de presentes ou no filtro de quantidade do carrinho**<br />

- **Onde:** Na página de lista de presentes para comprar um dos itens ou na página sacola
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'addToCart',
  'eventCategory':'dmj:enhanced-ecommerce',
  'eventAction': 'addToCart',
  'dimension12': '[[dm12-vendedor]]',
  'dimension13': '[[dm13-quantidadepresentes]]',
  'ecommerce': {
    'add': {
      'products': [{
        'dimension6': '[[dm6-product-cor]]',
        'dimension7': '[[dm7-product-estampa]]',
        'dimension8': '[[dm8-product-estilo]]',
        'dimension9': '[[dm9-product-tamanho]]',
        'dimension10': '[[dm10-product-skufilho]]',
        'dimension11': '[[dm11-product-subcategoria]]',
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

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[nome-produto]]` | &quot;cafeteira&quot; | Nome do produto |
| `[[id-produto]]` | &quot;i17mcjf106-771-2&quot; | SKU do produto |
| `[[preco-produto]]` | &quot;139,99&quot; | Preço do produto |
| `[[categoria-produto]]` | &quot;casa&quot;, &quot;moda&quot;, &quot;eletronicos&quot; e etc | Categoria do produto (departamento onde o produto aparece) |
| `[[variacao-produto]]` | &quot;325&quot; | Código DCO - Categoria do produto |
| `[[marca-produto]]` | &quot;nespresso&quot; | Marca do produto |
| `[[quantidade-produto]]` | &quot;1&quot; | Quantidade do produto |
| `[[dm6-product-cor]]` | &quot;rosa&quot;, &quot;azul&quot;, etc | Cor do produto |
| `[[dm7-product-estampa]]` | &quot;face-selva&quot;, &quot;floral&quot;, etc | Estampa do produto |
| `[[dm8-product-estilo]]` | &quot;feminino-delicado&quot;, &quot;casual-e-atemporal, etc | Estilo do produto |
| `[[dm9-product-tamanho]]` | p&#039;,&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; e etc | Tamanho do produto |
| `[[dm10-product-skufilho]]` |  &#039;2552&#039; | ID filho do produto |
| `[[dm11-product-subcategoria]]` |  '310090' | Código da categoria GM  |
| `[[dm12-vendedor]]` | &quot;riachuelo&quot;, etc | Deve retornar o nome da loja ques esta vendendo o produto |
| `[[dm13-quantidadepresentes]]` | &quot;45-itens&quot;, etc | Deve retornar a quantidade de presentes que os noivos adicionaram na lista |

<br />

**Ao remover um produto da lista ou diminuir a quantidade deste no filtro de quantidade**<br />

- **Onde:** Na página de sacola

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'removeFromCart',
  'eventCategory': 'dmj:enhanced-ecommerce',
  'eventAction': 'removeFromCart',
  'dimension12': '[[dm12-vendedor]]',
  'dimension13': '[[dm13-quantidadepresentes]]',
  'ecommerce': {
    'remove': {
      'products': [{
        'dimension6': '[[dm6-product-cor]]',
        'dimension7': '[[dm-7product-estampa]]',
        'dimension8': '[[dm8-product-estilo]]',
        'dimension9': '[[dm9-product-tamanho]]',
        'dimension10': '[[dm10-product-skufilho]]',
        'dimension11': '[[dm11-product-subcategoria]]',
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

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[nome-produto]]` | 'cafeteira' | Nome do produto |
| `[[id-produto]]` | 'i17mcjf106-771-2' | SKU do produto |
| `[[preco-produto]]` | '139,99' | Preço do produto |
| `[[categoria-produto]]` | 'casa', 'moda', 'eletronicos' e etc | Categoria do produto (departamento onde o produto aparece) |
| `[[variacao-produto]]` | '325' | Código DCO - Categoria do produto |
| `[[marca-produto]]` | 'nespresso' | Marca do produto |
| `[[quantidade-produto]]` | '1' | Quantidade do produto |
| `[[dm6-product-cor]]` | 'rosa', 'azul', etc | Cor do produto |
| `[[dm-7product-estampa]]` | 'face-selva', 'floral', etc | Estampa do produto |
| `[[dm8-product-estilo]]` | 'feminino-delicado', 'casual-e-atemporal, etc | Estilo do produto |
| `[[dm9-product-tamanho]]` | 'p','m','8-12','42' e etc | Tamanho do produto |
| `[[dm10-product-skufilho]]` |  '2552' | ID filho do produto |
| `[[dm11-product-subcategoria]]` | '310090' | Código da categoria GM  |
| `[[dm12-vendedor]]` | 'riachuelo', etc | Deve retornar o nome da loja ques esta vendendo o produto |
| `[[dm13-quantidadepresentes]]` | '45-itens', etc | Deve retornar a quantidade de presentes que os noivos adicionaram na lista |

<br />

**Nas etapas de checkout**<br />

- **Onde:** Nas páginas de checkout
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'checkout',
  'eventCategory': 'dmj:enhanced-ecommerce',
  'eventAction': 'checkout',
  'noInteraction': '1',
  'dimension12': '[[dm12-vendedor]]',
  'dimension13': '[[dm13-quantidadepresentes]]',
  'ecommerce': {
    'checkout': {
      'actionField': {'step': '[[passo-checkout]]'},
      'products': [{
        'dimension6': '[[dm6-product-cor]]',
        'dimension7': '[[dm7-product-estampa]]',
        'dimension8': '[[dm8-product-estilo]]',
        'dimension9': '[[dm9-product-tamanho]]',
        'dimension10': '[[dm10-product-skufilho]]',
        'dimension11': '[[dm11-product-subcategoria]]',
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

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[checkout-index]]` | '1' ou '2' ou '3' | Retornar o index de acordo com a página que o usuário está.  |
| `[[nome-produto]]` | 'cafeteira' | Nome do produto |
| `[[id-produto]]` | 'i17mcjf106-771-2' | SKU do produto |
| `[[preco-produto]]` | '139,99' | Preço do produto |
| `[[categoria-produto]]` | 'casa', 'moda', 'eletronicos' e etc | Categoria do produto (departamento onde o produto aparece) |
| `[[variacao-produto]]` | '325' | Código DCO - Categoria do produto |
| `[[marca-produto]]` | 'nespresso' | Marca do produto |
| `[[quantidade-produto]]` | '1' | Quantidade do produto |
| `[[dm6-product-cor]]` | 'rosa', 'azul', etc | Cor do produto |
| `[[dm7-product-estampa]]` | 'face-selva', 'floral', etc | Estampa do produto |
| `[[dm8-product-estilo]]` | 'feminino-delicado', 'casual-e-atemporal, etc | Estilo do produto |
| `[[dm9-product-tamanho]]` | 'p','m','8-12','42' e etc | Tamanho do produto |
| `[[dm10-product-skufilho]]` |  '2552' | ID filho do produto |
| `[[dm11-product-subcategoria]]` | '310090' | Código da categoria GM  |
| `[[dm12-vendedor]]` | 'riachuelo', etc | Deve retornar o nome da loja ques esta vendendo o produto |
| `[[dm13-quantidadepresentes]]` | '45-itens', etc | Deve retornar a quantidade de presentes que os noivos adicionaram na lista |
| `[[passo-checkout]]` | '1' | Página de carrinho de compras |
| `[[passo-checkout]]` | '2' | Página de confirmação do endereço de entrega |
| `[[passo-checkout]]` | '3' | Página de seleção do método de pagamento |

<br />


**No carregamento da página de confirmação de pedido**<br />

- **Onde:** Na página de pedido
    
```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'purchase',
  'eventCategory': 'dmj:enhanced-ecommerce',
  'eventAction': 'purchase',
  'noInteraction': '1',
  'dimension4': '[[dm4-transaction-pagamento]]',
  'dimension5': '[[dm5-transaction-parcelas]]',
  'dimension12': '[[dm12-vendedor]]',
  'dimension13': '[[dm13-quantidadepresentes]]',
  'ecommerce': {
    'purchase': {
      'actionField': {
        'id': '[[id-transacao]]',
        'revenue': '[[valor-total-transacao]]',
        'tax': '[[taxa-transacao]]'
      },
      'products': [{
        'dimension6': '[[dm6-product-cor]]',
        'dimension7': '[[dm7-product-estampa]]',
        'dimension8': '[[dm8-product-estilo]]',
        'dimension9': '[[dm9-product-tamanho]]',
        'dimension10': '[[dm10-product-skufilho]]',
        'dimension11': '[[dm11-product-subcategoria]]',
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

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| `[[userid]]` | '01234' | ID único de usuário definido após o cadastro |
| `[[dm4-transaction-pagamento]]` | 'a-vista', 'cartao-de-credito', 'debito' | Meio de pagamento |
| `[[dm5-transaction-parcelas]]` | '1x', '4x', '12x' | Número de parcelas |
| `[[dm6-product-cor]]` | 'rosa', 'azul', etc | Cor do produto |
| `[[dm7-product-estampa]]` | 'face-selva', 'floral', etc | Estampa do produto |
| `[[dm8-product-estilo]]` | 'feminino-delicado', 'casual-e-atemporal, etc | Estilo do produto |
| `[[product-tamanho]]` | 'p','m','8-12','42' e etc | Tamanho do produto |
| `[[dm10-product-skufilho]]` |  '2552' | ID filho do produto |
| `[[dm11-product-subcategoria]]` | '310090' | Código da categoria GM  |
| `[[dm12-vendedor]]` | 'riachuelo', etc | Deve retornar o nome da loja ques esta vendendo o produto |
| `[[quantidadepresentes]]` | '45-itens', etc | Deve retornar a quantidade de presentes que os noivos adicionaram na lista |
| `[[id-transacao]]` | '000011652' | Número do pedido |
| `[[valor-total-transacao]]` | '139,99' | Valor total da transação incluindo frete e taxas |
| `[[taxa-transacao]]` | '0.00' | Valor de taxas da transação |
| `[[nome-produto]]` | 'cafeteira' | Nome do produto |
| `[[id-produto]]` | 'i17mcjf106-771-2' | SKU do produto |
| `[[preco-produto]]` | '139,99' | Preço do produto |
| `[[categoria-produto]]` | 'casa', 'moda', 'eletronicos' e etc | Categoria do produto (departamento onde o produto aparece) |
| `[[variacao-produto]]` | '325' | Código DCO - Categoria do produto |
| `[[marca-produto]]` | 'nespresso' | Marca do produto |
| `[[quantidade-produto]]` | '1' | Quantidade do produto |

<br />

---

## Considerações Finais


> Documentações Oficiais do Google: <br />
> [Google Analytics - Avaliação de comércio eletrônico ](https://developers.google.com/analytics/devguides/collection/analyticsjs/ecommerce?hl=pt-br) <br />
> [Google Tag Manager - Guia do desenvolvedor ](https://developers.google.com/tag-manager/enhanced-ecommerce))

<script>
  document.addEventListener('DOMContentLoaded', function(event) {
    document.querySelectorAll('h1 a')[0].style.display = 'none';
  });
</script>
