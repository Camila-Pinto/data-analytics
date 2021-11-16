![Riachuelo](https://www.riachuelo.com.br/static/version1623357894/frontend/Corra/webjump/pt_BR/images/logo.svg)

> Área - Data Analytics<br />
> Documento de Especificação Técnica para Apps

<br />

## Implementação de Tags Firebase 

Última atualização: 16/11/2021 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />


## Objetivo

Este documento tem como objetivo instruir a implementação de Tags de Firebase Analytics para aplicativos.


## Implementação

> Os links abaixo descrevem como realizar a implementação do Firebase em seu aplicativo.

[Link - Adicionar o Firebase ao projeto do Android](https://rnfirebase.io/docs/master/installation/android).
[Link - Adicionar o Firebase ao projeto do iOS](https://rnfirebase.io/docs/master/installation/ios).

<br />

## Especificações Globais

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[ ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores;<br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais;<br />
Caso a informação solicitada não estiver disponível retornar tipagem `undefined`.

---

### Definições Globais

**Definições Customizadas para todas as telas:**<br />
Quando ocorrer as trocas de telas o código abaixo deve ser definido para que os hits sejam enviados ao Firebase Analytics conforme recomendado no Tagbook. <br />

```javascript
Analytics.setUserId('[[UserID]]');
Analytics.setUserProperty("dimension1", "[[Bandeira do Cartão]]");
```

**OBS:** Esta definição global deve ser disparada em todo momento que o usuário estiver logado.

**Os exemplos abaixos são fictícios, induzem apenas a forma correta para a implementação dos eventos globais.**

| Nome        | Variável    | Exemplo     | Descrição          |
| :--------   | :--------   | :-------    | :--------------    |
| [[userID]]       | [[UserID]]               | '01234'                                 | ID único de usuário definido após o cadastro (ID próprio da Midway).|
| dimension1   | [[Bandeira do Cartão]]   | "pl", "visa", "mastercard", etc | Deve retornar a bandeira do cartão utilizada pelo usuário |


### ScreenViews

- **Quando:** Em todos os momentos que uma nova tela do APP for carregada.

**OBS:** No Tagbook, temos exatamente o path pensado para a feature mapeada no aplicativo. Abaixo segue alguns exemplos de como implementar um evento de screenView.

**Os exemplos abaixos são fictícios, induzem apenas a forma correta para a implementação de um evento do tipo ScreenView**

- **Onde:** Visualização da tela "Home"

```javascript
    Analytics.logScreenView("/home/");
```

- **Onde:** Visualização da tela "Parcelamento da Fatura"

```javascript
    Analytics.logScreenView("/home/parcelamento-da-fatura/");
```

### Eventos Genéricos

- **Quando:** Em todos os eventos que no tabgook solicitar os atributos eventCategory, eventAction e eventLabel


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "eventCategory-proposto-no-tagbook",
        	eventAction: "eventAction-proposto-no-tagbook",
        	eventLabel: "eventLabel-proposto-no-tagbook"
        });
```

<br />

### Eventos Com Extras Parameters

- **Quando:** Em todos os eventos que no tabgook solicitar os atributos eventCategory, eventAction, eventLabel mais extras parameters adicionais.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "eventCategory-proposto-no-tagbook",
        	eventAction: "eventAction-proposto-no-tagbook",
        	eventLabel: "eventLabel-proposto-no-tagbook",
		nameAttributeOne: "nameAttributeOne-proposto-no-tagbook",
		nameAttributeTwo: "nameAttributeTwo-proposto-no-tagbook",
        });
```

| Variável        | Exemplo           |   Descrição          |
| :-------------- | :---------------- | :------------------- |
| `[[nameAttributeOne]]`    |  'nameAttributeOne'  | Retorna o extra parameters na coluna I, da aba Tagging Plan do Tagbook |
| `[[nameAttributeTwo]]`    |  'nameAttributeTwo'   | Retorna o extra parameters na coluna I, da aba Tagging Plan do Tagbook   |

<br />

---

## Eventos - Funil de Vendas

### Visualização da Lista de Produtos

- **Quando:** Na visualização/carregamento da tela de lista de produtos.
- **Onde:** Na tela PLP de vitrine de produtos.

```javascript
Analytics.logViewItemList({
  item_list_name: "[[nome-lista]]",
  item_id: "[[id-produto]]",
  item_category: "[[categoria-produto]]",
  item_name: "[[nome-produto]]",
  price: "[[preco-produto]"
});
```

| Variável        | Exemplo           |   Descrição          |
| :-------------- | :---------------- | :------------------- |
| `[[nome-lista]]`        |  'produtos', 'busca', 'camisas', 'tenis' e etc.                                     | Deve retornar a lista de itens.        |
 `[[id-produto]]`        | '12344', '312333'                                     | Retornar o id do produto selecionado.        |
| `[[categoria-produto]]` | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado. |
| `[[nome-produto]]`      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.         |
| `[[preco-produto]]`     | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.     |

### Adição à lista de desejos

- **Quando:** Clique na opção de adicionar a lista de desejos dos produtos

```javascript
Analytics.logAddToWishlist({
    item_id: '[[id-produto]]',
    item_name: '[[nome-produto]]',
    item_category: '[[categoria-produto]]',
    price: '[[preco-produto]'
}
```

| Variável        | Exemplo           |   Descrição          |
| :-------------- | :---------------- | :------------------- |
| `[[id-produto]]`        | '12344', '312333'                                     | Retornar o id do produto selecionado.        |
| `[[nome-produto]]`      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.         |
| `[[categoria-produto]]` | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado. |
| `[[preco-produto]]`     | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.     |

### Visualização do Produto Selecionado

- **Quando:** Na visualização/carregamento do produto carregado.
- **Onde:** Nas telas de exibição do produto escolhido.

```javascript
Analytics.logViewItem({
  item_id: "[[id-produto]]",
  item_name: "[[nome-produto]]",
  item_category: "[[categoria-produto]]",
  price: "[[preco-produto]"
});
```

| Variável        | Exemplo           |   Descrição          |
| :-------------- | :---------------- | :------------------- |
| `[[id-produto]]`        | '12344', '312333'                                     | Retornar o id do produto selecionado.        |
| `[[nome-produto]]`      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.         |
| `[[categoria-produto]]` | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado. |
| `[[preco-produto]]`     | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.     |

### Add To Cart

- **Quando:** Nas opçãoes de adicionar um novo produto ao carrinho, tais como: Clique em Comprar,  Adicionar a Sacola e no sinal de adição (+) e etc.


```javascript
Analytics.logAddToCart({
  item_id: "[[id-produto]]",
  item_name: "[[nome-produto]]",
  item_category: "[[categoria-produto]]",
  price: "[[preco-produto]",
  coupon: "[[coupon]]",
  value: "[[valor-total]]",
  quantity: "[[quantidade-produto]]"
});
```

| Variável        | Exemplo           |   Descrição          |
| :-------------- | :---------------- | :------------------- |
| `[[id-produto]]`         | '12344', '312333'                                     | Retornar o id do produto selecionado.         |
| `[[nome-produto]]`       | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.          |
| `[[categoria-produto]]`  | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado.  |
| `[[preco-produto]]`      | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.      |
| `[[coupon]]`      | 'rchlo123' etc.                      | Cupom utilizado na compra.      |
| `[[valor-total]]`      | '355.40', etc.                      | Valor total da compra.      |
| `[[quantidade-produto]]` | '1', '2', '3', etc.                                   | Retornar a quantidade do produto selecionado. |

### Remove From Cart

- **Quando:** Nas opções de remoção de algum ou todos os produtos que estão no carrinho. (esvaziar lixeira).


```javascript
Analytics.logRemoveFromCart({
  item_id: "[[id-produto]]",
  item_name: "[[nome-produto]]",
  item_category: "[[categoria-produto]]",
  price: "[[preco-produto]",
  coupon: "[[coupon]]",
  value: "[[valor-total]]",
  quantity: "[[quantidade-produto]]"
});
```

| Variável        | Exemplo           |   Descrição          |
| :-------------- | :---------------- | :------------------- |
| `[[id-produto]]`         | '12344', '312333'                                     | Retornar o id do produto selecionado.         |
| `[[nome-produto]]`       | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.          |
| `[[categoria-produto]]`  | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado.  |
| `[[preco-produto]]`      | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.      |
| `[[coupon]]`      | 'rchlo123' etc.                      | Cupom utilizado na compra.      |
| `[[valor-total]]`      | '355.40', etc.                      | Valor total da compra.      |
| `[[quantidade-produto]]` | '1', '2', '3', etc.                                   | Retornar a quantidade do produto selecionado. |

### Begin Checkout

- **Quando:** No primeiro step do checkout, quando temos os produtos do carrinho do usuário.

```javascript
Analytics.logBeginCheckout({
  value: "[[valor-da-transacao]]",
  item_id: "[[id-produto]]",
  item_name: "[[nome-produto]]",
  item_category: "[[categoria-produto]]",
  price: "[[preco-produto]",
  coupon: "[[coupon]]",
  quantity: "[[quantidade-produto]]"
});
```

| Variável        | Exemplo           |   Descrição          |
| :-------------- | :---------------- | :------------------- |
| `[[valor-da-transacao]]` | 144.80                                                | Retornar o valor da transação.               |
| `[[id-produto]]`         | '12344', '312333'                                     | Retornar o id do produto selecionado.        |
| `[[nome-produto]]`       | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.         |
| `[[categoria-produto]]`  | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado. |
| `[[preco-produto]]`      | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.     |
| `[[coupon]]`      | 'rchlo123' etc.                      | Cupom utilizado na compra.      |
| `[[quantidade-produto]]` | '1', '2', '3', etc.                                   | Retornar a quantidade do produto selecionado. |


### Add Shipping Info:

- **Quando:** Na visualização/carregamento da tela de Entrega
- **Onde:** Nas telas relacionadas a "checkout".

```javascript
Analytics.logAddShippingInfo({
  item_id: "[[id-produto]]",
  item_name: "[[nome-produto]]",
  item_category: "[[categoria-produto]]",
  price: "[[preco-produto]",
  coupon: "[[coupon]]",
  value: "[[valor-total]]",
  quantity: "[[quantidade-produto]]",
  currency: "[[currency]]",
  shipping_tier: "[[shipping_tier]]"
});
```

| Variável        | Exemplo           |   Descrição          |
| :-------------- | :---------------- | :------------------- |
| `[[id-produto]]`         | '12344', '312333'                                     | Retornar o id do produto selecionado.        |
| `[[nome-produto]]`       | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.         |
| `[[categoria-produto]]`  | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado. |
| `[[preco-produto]]`      | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.     |
| `[[coupon]]`      | 'rchlo123' etc.                      | Cupom utilizado na compra.      |
| `[[currency]]`      | 'USD' etc.                      | Moeda no formato ISO_4217..      |
| `[[shipping_tier]]`      | 'normal', 'agendada', 'retira-rapido' e etc.                      | Tipo de frete.      |
| `[[valor-total]]`      | '355.40', etc.                      | Valor total da compra.      |
| `[[quantidade-produto]]` | '1', '2', '3', etc.                                   | Retornar a quantidade do produto selecionado. |

### Add Payment Info:

- **Quando:** Na visualização/carregamento da tela de Pagamento
- **Onde:** Nas telas relacionadas a "checkout".

```javascript
Analytics.logAddPaymentInfo({
  item_id: "[[id-produto]]",
  item_name: "[[nome-produto]]",
  item_category: "[[categoria-produto]]",
  price: "[[preco-produto]",
  coupon: "[[coupon]]",
  value: "[[valor-total]]",
  quantity: "[[quantidade-produto]]",
  currency: "[[currency]]",
  payment_type: "[[payment_type]]"
});
```

| Variável        | Exemplo           |   Descrição          |
| :-------------- | :---------------- | :------------------- |
| `[[id-produto]]`         | '12344', '312333'                                     | Retornar o id do produto selecionado.        |
| `[[nome-produto]]`       | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.         |
| `[[categoria-produto]]`  | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado. |
| `[[preco-produto]]`      | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.     |
| `[[coupon]]`      | 'rchlo123' etc.                      | Cupom utilizado na compra.      |
| `[[currency]]`      | 'USD' etc.                      | Moeda no formato ISO_4217..      |
| `[[payment_type]]`      |  'boleto', 'cartao-credito' e etc                    | Tipo de pagamento.      |
| `[[valor-total]]`      | '355.40', etc.                      | Valor total da compra.      |
| `[[quantidade-produto]]` | '1', '2', '3', etc.                                   | Retornar a quantidade do produto selecionado. |


### Purchase

- **Quando:** Na visualização/carregamento da tela de confirmação de compra
- **Onde:** Na tela de confirmação de compra

```javascript
Analytics.logPurchase({
  transaction_id: '[[id-da-transacao]]',
  shipping: '[[valor-do-frete]]',
  coupon: '[[cupom]]',
  item_id: "[[id-produto]]",
  item_name: "[[nome-produto]]",
  item_category: "[[categoria-produto]]",
  price: "[[preco-produto]",
  coupon: "[[coupon]]",
  value: "[[valor-total]]",
  quantity: "[[quantidade-produto]]",
  currency: "[[currency]]",
});
```

| Variável        | Exemplo           |   Descrição          |
| :-------------- | :---------------- | :------------------- |
| `[[id-da-transacao]]`            | '12344', '312333' | Retornar o id da transação.    |
| `[[valor-do-frete]]`             | 14.00             | Retornar o valor do frete.     |
| `[[id-produto]]`         | '12344', '312333'                                     | Retornar o id do produto selecionado.        |
| `[[nome-produto]]`       | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.         |
| `[[categoria-produto]]`  | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado. |
| `[[preco-produto]]`      | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.     |
| `[[coupon]]`      | 'rchlo123' etc.                      | Cupom utilizado na compra.      |
| `[[currency]]`      | 'USD' etc.                      | Moeda no formato ISO_4217..      |
| `[[valor-total]]`      | '355.40', etc.                      | Valor total da compra.      |
| `[[quantidade-produto]]` | '1', '2', '3', etc.                                   | Retornar a quantidade do produto selecionado. |

<br />

<script>
  document.addEventListener('DOMContentLoaded', function(event) {
    document.querySelectorAll('h1 a')[0].style.display = 'none';
  });
</script>

