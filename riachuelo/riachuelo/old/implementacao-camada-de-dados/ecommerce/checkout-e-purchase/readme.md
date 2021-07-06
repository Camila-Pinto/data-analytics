![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Enhanced Ecommerce

Última atualização: 11/09/2020 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:@zoly.com.br)

<br />

## Objetivo

Este documento tem como objetivo instruir a implementação da camada de dados para utilização de recursos do enhanced ecommerce referentes ao ambiente [Riachuelo](http:riachuelo.com.br//).

<br />

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
    'atributo1': 'valor1',
    'atributo2': 'valor2'
  }];
</script>
```

 OU

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'atributo1': 'valor1',
    'atributo2': 'valor2'
  });
</script>
```

## Implementação

A documentação foi descrita para algumas áreas especificas do ambiente [Riachuelo](http:riachuelo.com.br//).

---

## Especificações Globais:

**Itens Gerais:**<br />

Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores;<br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais;<br />
Caso a informação solicitada não estiver disponível retornar **undefined**.

<br />

### Checkout:
- **Onde:** Nas telas de Sua Sacola, CPF, Cadastro, Entrega e Pagamento.

```html
<script>
dataLayer.push({
  'event': 'r_checkout',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'checkout',
  'eventLabel': '',
  'ecommerce': {
    'checkout': {
      'actionField': {'step': '[[checkout-index]]'},
      'products': [{
        'name': '[[nome-produto]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]',
        'dimension13':'[[idade-do-produto]]',
        'dimension18':'[[departamento-do-produto]]',
        'dimension19':'[[id-do-look]]',
        'dimension20':'[[categoria-do-produto]]',
        'dimension21':'[[look-completo]]',
        'dimension24':'[[ordem-de-insercao]]',
        'dimension25':'[[tamanho-do-produto]]',
        'dimension26':'[[cor-do-produto]]',
        'dimension27':'[[padronagem-do-produto]]',
        'dimension30':'[[sub-catgoria]]',
        'dimension32':'[[preco-original]]',
        'dimension38':'[[lifestyle]]',
        'dimension39':'[[gender]]',
        'dimension40':'[[exclusivo-ecommerce+mktplace]]',
        'dimension42':'[[sku-filho]]'
      }]
    }
  }
});
</script>
```
 
<br />
 
| Nome      | Variável          | Exemplo           | Descrição           |
| :------------ | :------------------------ | :------------------------ | :---------------------------- |
| actionField   | [[checkout-index]]    | Sua Sacola: '1'/ Entrar CPF: '2'/  Form Cadastro: '3' / Entrega: '4' / Pagamento: '5'            | Retornar "1" ou "2" ou "3" ou "4" ou "5" os exemplos citados     |
| name      | [[nome-produto]]      | 'calca-masculina-super-skinny-em-jeans'            | Nome do produto         |
| id      | [[id-produto]]      | '13239635'            | SKU do produto - pai         |
| price     | [[preco-produto]]     | '139.99'            | Preço do produto        |
| brand    | [[marca-produto]]   | 'pool-original'            | Marca do produto      |
| category    | [[categoria-produto]]   | 'masculino'            | Categoria do produto      |
| variant     | [[variacao-produto]]    | '325'            | Código DCO - Categoria do produto       |
| quantity    | [[quantidade-produto]]  | '1'            | Quantidade do produto     |
| dimension13    | [[idade-do-produto]]  | 'X-day'            | Idade do produto cadastrado no Magento (checkout).     |
| dimension18    | [[departamento-do-produto]]  | 'masculino'            | Departamento da empresa principal do produto.      |
| dimension19    | [[id-do-look]]  | 'produto-sem-vinculo'            | ID do produto do tipo look.      |
| dimension20    | [[categoria-do-produto]]  | 'acessorio'            | Categoria secundaria  do produto.      |
| dimension21    | [[look-completo]]  | 'trueou 'false'            | Look Completo.      |
| dimension24    | [[ordem-de-insercao]]  | '1,2,3 ..'            | Ordem de inserção ao carrinho      |
| dimension25    | [[tamanho-do-produto]]  | 'p, m, 8-12, 42'            | Tamanho do produto.      |
| dimension26    | [[cor-do-produto]]  | 'vermelho'            | Cor do produto.      |
| dimension27    | [[padronagem-do-produto]]  | 'florido, listado'            | Cor do produto.      |
| dimension30    | [[sub-catgoria]]  | '310090'            | Código da categoria GM.      |
| dimension32    | [[preco-original]]  | '12.00'            | Preço do produto (dê).      |
| dimension38    | [[lifestyle]]     |  'casual, esportivo'           | Estilo do produto.  |
| dimension39    | [[gender]]     |  'unisex, feminino'           | Genero do produto.  |
| dimension40    | [[exclusivo-ecommerce+mktplace]]      | 'sim:riachuelo', 'nao:riachuelo', 'nao:pontofrio', 'nao:extra' | Deve retornar se o produto é exclusivo do ecommerce e o seu seller/marketplace |
| dimension42    | [[sku-filho]]     |  '2552'           | ID filho do produto.  |

<br />

### Checkout Option - Frete:
**Ao selecionar uma opção de frete**<br />

- **Onde:** Na página de carrinho e de checkout

```html
<script>
  dataLayer.push({
  'event': 'r_checkoutoption',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'checkoutoption',
  'eventLabel': 'tipo-de-frete',
    'ecommerce': {
      'checkout_option': {
        'actionField': {'step':'4','option':'frete:[[nome-entrega]]:[[previsao]]:[[valor]]}'}
      }
    }
  });
</script>
```

| Variável        | Exemplo         | Descrição                   |
| :-------------------- | :-------------------- | :-------------------------------------------  |
| [[nome-entrega]]      | 'normal', 'expresso ', 'entrega-agendada','retirar-em-loja:shopping-tiete-plaza', 'retirar-em-loja:shopping-bourbon-sp' etc      |  Deve retornar os nomes dos tipos de entrega.                  |
| [[previsao]]     |  '3-dias, 4-dias,5-dias'     |  Deve retornar a quantidade de dias             |
| [[valor]]     | '0.00', '2.99', '5.00'     | Deve retornar os valores dos fretes.             |

<br />
 
### Checkout Option - Pagamento:
**Ao selecionar uma opção de pagamento**<br />

- **Onde:** Na página de carrinho e de checkout

```html
<script>
  dataLayer.push({
  'event': 'r_checkoutoption',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'checkoutoption',
  'eventLabel': 'tipo-de-pagamento',
    'ecommerce': {
      'checkout_option': {
        'actionField': {'step':'5','option':'pagamento:[[tipo pagamento]]:[[opcao-adicional]]'}
      }
    }
  });
</script>
```

| Variável        | Exemplo         | Descrição                   |
| :-------------------- | :-------------------- | :-------------------------------------------  |
| [[tipo pagamento]]      | 'cartao', 'paypal', 'boleto' etc      | Deve retornar o nome da opção de pagamento clicada.                  |
| [[opcao-adicional]]     | 'vale-troca'     | Deve retornar a opção de pagamento adicional.             |

<br />
 
### Purchase:

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no carregamento da página de transação, e se o usuário acessar novamente o link ou atualizar a página, o objeto não deve ser disparado novamente.
 
```html
<script>
dataLayer.push({
  'event': 'r_purchase',
  'eventCategory': 'enhanced-ecommerce',
  'eventAction': 'purchase',
  'eventLabel': '',
  'dimension2':'[[tipo-cartao]]',
  'dimension3':'[[bandeira-cartao]]',
  'dimension22':'[[metodo-de-pagamento]]',
  'dimension23':'[[status-do-pagamento]]',
  'dimension28':'[[frete]]',
  'dimension46':'[[parcelamento]]',
  'metric1':'[[desconto-frete]]'
  'metric2':'[[desconto-funcionario]]',
  'metric3':'[[desconto-promocao]]',
  'metric4':'[[total-de-descontos]]',
  'ecommerce': {
    'purchase': {
      'actionField': {
        'id': '[[id-transacao]]',       //ID da transação é obrigatório
        'revenue': '[[valor-total-transacao]]',
        'shipping': '[[frete-transacao]]',
        'coupon': '[[cupom-transacao]]',
        'affiliation': '[[loja-transacao]]',
        'tax': '[[taxa-transacao]]',
      },
      'products': [{
        'name': '[[nome-produto]]',       //Nome ou ID do produto é obrigatório
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]',
        'dimension13':'[[idade-produto]]',
        'dimension18':'[[departamento-produto]]',
        'dimension19':'[[id-do-look]]',
        'dimension20':'[[categoria-do-produto]]',
        'dimension21':'[[look-completo]]',
        'dimension24':'[[ordem-de-insercao]]',
        'dimension25':'[[tamanho-do-produto]]',
        'dimension26':'[[cor-do-produto]]',
        'dimension27':'[[padronagem-do-produto]]',
        'dimension30':'[[sub-categoria]]',
        'dimension32':'[[preco-original]]',
        'dimension38':'[[lifestyle]]',
        'dimension39':'[[gender]]',
        'dimension40':'[[exclusivo-ecommerce+mktplace]]',
        'dimension42':'[[sku-filho]]',
      }]
    }
  }
});
</script>
```

<br />
 
*Descrição Purchase:*
 
| Nome      | Variável          | Exemplo       | Descrição                     |
| :------------ | :------------------------ | :---------------- | :------------------------------------------------ |
| id      | [[id-transacao]]      | '"000011652"'        | ID único da transação.                 |
| revenue     | [[valor-total-transacao]] | '139.99'        | Valor total da transação incluindo frete e taxas  |
| shipping    | [[frete-transacao]]     | '0.00'        | Frete da transação                |
| coupon    | [[cupom-transacao]]   | 'cupom-2018'        | Cupom de desconto usado na transação        |
| affiliation | [[loja-transacao]]    | 'Riachuelo'        | Loja que ocorreu a transação            |
| tax       | [[taxa-transacao]]    | '0.00'        | Taxas da transação                |

<br />

*Descrições Atributos Extras:*

| Variável        | Exemplo         | Descrição                   |
| :-------------------- | :-------------------- | :-------------------------------------------  |
| [[tipo-cartao]]      | 'itau, riachuelo'     | Tipo do cartão utilizado na compra.                  |
| [[bandeira-cartao]]     | 'visa, mastercard'     | Bandeira do cartão utilizada na compra             |
| [[metodo-de-pagamento]]     | 'cartao-de-credito, boleto'     | Nome do tipo do pagamento.             |
| [[status-do-pagamento]]     | 'aprovado'     | Nome do status do pagamento.             |
| [[frete]]     | 'normal, expresso, entrega-agendada'     | Nome do tipo de entrega.             |
| [[desconto-frete]]     | '10.00'     | Valor do desconto de frete.             |
| [[desconto-funcionario]]     | '22.00'     | Valor do desconto para funcionário             |
| [[desconto-promocao]]     | '15.00'     | Valor do desconto promocional - cupom, primeira compra, etc.             |
| [[total-de-descontos]]     | '47.00'     | Valor da soma de todos os descontos - promoção, frete e funcionário.             |
| [[parcelamento]]            |  '2'           | Quantidade de parcelas.  |

Caso a informação solicitada não estiver disponível retornar **undefined**.

<br />
 
*Descrição Produtos:*
 
| Nome      | Variável          | Exemplo           | Descrição               |
| :------------ | :------------------------ | :------------------------ | :------------------------------------ |
| name      | [[nome-produto]]      | 'calca-masculina-super-skinny-em-jeans'            | Nome do produto         |
| id      | [[id-produto]]      | '13239635'            | SKU do produto - pai         |
| price     | [[preco-produto]]     | '139.99'            | Preço do produto        |
| brand    | [[marca-produto]]   | 'pool-original'            | Marca do produto      |
| category    | [[categoria-produto]]   | 'masculino'            | Categoria do produto      |
| variant     | [[variacao-produto]]    | '325'            | Código DCO - Categoria do produto      |
| quantity    | [[quantidade-produto]]  | '1'            | Quantidade do produto     |
| coupon    | [[cupom-produto]]     | 'cupom-2018'            | Cupom de desconto usado no produto  |
| dimension13    | [[idade-produto]]     | 'X-day'            | Idade do produto cadastrado no Magento (checkout).  |
| dimension18    | [[departamento-produto]]     | 'masculino'            | Departamento da empresa principal do produto.  |
| dimension19    | [[id-do-look]]     |  'produto-sem-vinculo'           | ID do produto do tipo look.  |
| dimension20    | [[categoria-do-produto]]     |  'acessorio'           | Categoria secundaria  do produto.  |
| dimension21    | [[look-completo]]     |  'true ou false'           | Look Completo.  |
| dimension24    | [[ordem-de-insercao]]     |  '1,2,3 ..'           | Ordem de inserção ao carrinho.  |
| dimension25    | [[tamanho-do-produto]]     |  'p, m , 8-12, 42'           | Tamanho do produto.  |
| dimension26    | [[cor-do-produto]]     |  'vermelho'           | Cor do produto.  |
| dimension27    | [[padronagem-do-produto]]     |  'florido, listado'           | Padrão da estampa do produto.  |
| dimension30    | [[sub-categoria]]     |  '310090'           | Código da categoria GM.  |
| dimension32    | [[preco-original]]     |  '12.00'           | Preço do produto (dê).  |
| dimension38    | [[lifestyle]]     |  'casual, esportivo'           | Estilo do produto.  |
| dimension39    | [[gender]]     |  'unisex, feminino'           | Genero do produto.  |
| dimension40    | [[exclusivo-ecommerce+mktplace]]      | 'sim:riachuelo', 'nao:riachuelo', 'nao:pontofrio', 'nao:extra' | Deve retornar se o produto é exclusivo do ecommerce e o seu seller/marketplace |
| dimension42    | [[sku-filho]]     |  '2552'           | ID filho do produto.  |



<br />
 
---
  
### Considerações Finais
 
> Recomendações do Google:
> 1. [Enhanced Ecommerce - Checkout](https://developers.google.com/tag-manager/enhanced-ecommerce#checkout)
> 2. [Enhanced Ecommerce - Purchase](https://developers.google.com/tag-manager/enhanced-ecommerce#purchases)
 
> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)
 
<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    document.querySelectorAll("h1 a")[0].style.display = 'none';
  });
</script>
