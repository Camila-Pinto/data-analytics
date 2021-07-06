![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação Firebase - Riachuelo APP - Android

Última atualização: 07/05/2020 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação do Firebase, e o disparo de eventos para utilização de recursos de monitoramento do Firebase Analytics referentes ao aplicativo Riachuelo.

<br />

## Implementação

> Os links abaixo descrevem como realizar a implementação do Google Tag Manager + Firebase em seu aplicativo.

[Link 1 - Gerenciador de tags + Firebase: primeiros passos](https://developers.google.com/tag-manager/android/v5/)

[Link 2 - Adicionar o Firebase ao projeto do Android](https://firebase.google.com/docs/android/setup)

[Link 3 - Boas vindas ao Firebase](https://console.firebase.google.com/u/0/?hl=pt-br)

[Link 4 - Primeiros passos para usar o Firebase Analytics para Android](https://firebase.google.com/docs/analytics/android/start/)

[Link 5 - Enhanced Ecommerce](https://developers.google.com/tag-manager/android/v5/enhanced-ecommerce)

<br />

## Especificações Globais:

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores;<br />
Todos os valores enviados ao Firebase Analytics devem estar **sanitizados**, ou seja, sem espaços, acentuação ou caracteres especiais;<br />
Caso a informação solicitada não estiver disponível retornar 'nao_disponivel'.

---

### Definições Globais:

**Definições Customizadas para todas as telas:**<br />
Quando ocorrer as trocas de telas o código abaixo deve ser definido para que os hits sejam enviados ao 
.<br />

```java
        mFirebaseAnalytics.setUserID([[UserID]]);
        mFirebaseAnalytics.setUserProperty("login_status", "[[logado-ou-nao-logado]]");
```

**OBS:** Esta definição global deve ser disparada em todo momento que o usuário estiver logado.

| Variável         | Exemplo                | Descrição           |
| :--------------- | :----------            | :-------------------|
| [[UserID]]       | 'abc1234'              | Deve retornar o ID único do usuário |
| [[logado-ou-nao-logado] | 'logado' ou 'nao-logado'  | Identificação de Login do usuário. |

<br />

### ScreenView:

> O valor de Screenview deve ser configurado ao entrar em uma nova tela, independente de ser uma nova activity, e este valor irá junto com todos os eventos disparados até que se configure um novo valor ou troque de activity. 

<br />

**Tela principal Home**<br />

- **Onde:** Visualização da tela inicial do app assim que o usuário logar;

```java
    mFirebaseAnalytics.setCurrentScreen(this, "/riachuelo-app/home/", null);
```

<br />

**Tela de Itens favoritos**<br />

- **Onde:** Visualização da tela de itens favoritos do app;

```java
    mFirebaseAnalytics.setCurrentScreen(this, "/riachuelo-app/favoritos/", null);
```

<br />

**Tela de Mais opções**<br />

- **Onde:** Visualização da tela de mais opcoes no app;

```java
    mFirebaseAnalytics.setCurrentScreen(this, "/riachuelo-app/mais/", null);
```

<br />

**Tela de Conta do usuário**<br />

- **Onde:** Visualização da tela de conta usuário no app;

```java
    mFirebaseAnalytics.setCurrentScreen(this, "/riachuelo-app/conta/", null);
```

<br />

**Tela PLP e CLP - Vitrine**<br />

- **Onde:** Visualização da tela PLP e CLP de vitrine de produtos - Comprar

```java
    mFirebaseAnalytics.setCurrentScreen(this, "/riachuelo-app/vitrine/", null);
```

<br />

**Visualização da tela de Produto**<br />

- **Onde:** Visualização da tela de produto carregado

```java
    mFirebaseAnalytics.setCurrentScreen(this, "/riachuelo-app/produto/", null);
```

<br />

**Tela de sacola**<br />

- **Onde:** Visualização da tela de sacola/carrinho

```java
    mFirebaseAnalytics.setCurrentScreen(this, "/riachuelo-app/sacola/", null);
```
<br />

---


### Eventos:


### Menu Inferior

- **Quando:** Clique em algum dos elementos Menu Inferior.
- **Onde:** Em todas as telas do app com Menu.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:geral");
        params.putString("eventAction","clique:menu");
        params.putString("eventLabel","[[item-selecionado]]");
        mFirebaseAnalytics.logEvent("event", params);
```

| Variável         | Exemplo                | Descrição           |
| :--------------- | :----------            | :-------------------       |
| [[item-selecionado]] | 'home', 'favoritos', 'mais', 'comprar', 'conta', etc. | Retornar o item clicado do menu inferior. |


<br />


### Header

- **Quando:** Clique no icone de busca no header.
- **Onde:** Em todas as telas do app.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:geral");
        params.putString("eventAction","clique:header");
        params.putString("eventLabel","buscar");
        mFirebaseAnalytics.logEvent("event", params);
```

<br />

- **Quando:** Clique no icone de sacola no header.
- **Onde:** Em todas as telas do app.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:geral");
        params.putString("eventAction","clique:header");
        params.putString("eventLabel","sacola");
        mFirebaseAnalytics.logEvent("event", params);
```

<br />

### Vitrine

- **Quando:** Clique para alternar a visualização da lista de produtos.
- **Onde:** Na tela PLP e CLP de vitrine de produtos.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:vitrine");
        params.putString("eventAction","clique:botao:modo-exibicao");
        params.putString("eventLabel","alterar");
        mFirebaseAnalytics.logEvent("event", params);
```

<br />

- **Quando:** Clique para selecionar os filtros da visualização da lista de produtos.
- **Onde:** Na tela PLP e CLP de vitrine de produtos.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:vitrine");
        params.putString("eventAction","clique:botao:modo-exibicao");
        params.putString("eventLabel","filtrar:[[modo-ordenacao]]");
        mFirebaseAnalytics.logEvent("event", params);
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[modo-ordenacao]]    | 'grade' ou 'lista'.  |  Deve retornar o modo de ordenação selecionado. |

<br />

- **Quando:** Clique para ir para a sacola com produtos.
- **Onde:** Na tela PLP e CLP de vitrine de produtos.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:vitrine");
        params.putString("eventAction","clique:botao-flutante");
        params.putString("eventLabel","ir-para-sacola");
        mFirebaseAnalytics.logEvent("event", params);
```

<br />

- **Quando:** Clique em algum dos produtos disponiveis.
- **Onde:** Na tela PLP e CLP de vitrine de produtos.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:vitrine");
        params.putString("eventAction","clique:card-produto");
        params.putString("eventLabel","[[id-produto]]:[[nome-produto]]");
        mFirebaseAnalytics.logEvent("event", params);
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[nome-produto]]      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc.  |  Retornar o nome produto selecionado. |

<br />

### Página de produto

- **Quando:**  Clique para do 'X' para fechar a exibição do produto.
- **Onde:** Na tela de exibição do produto.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:produto");
        params.putString("eventAction","clique:header");
        params.putString("eventLabel","fechar");
        mFirebaseAnalytics.logEvent("event", params);
```

<br />

- **Quando:** Clique para ver as opções do produto.
- **Onde:** Na tela de exibição do produto.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:produto");
        params.putString("eventAction","clique:botao-descricaor");
        params.putString("eventLabel","[[id-produto]]:opcoes");
        mFirebaseAnalytics.logEvent("event", params);
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |

<br />

- **Quando:**  Clique para ver os detalhes do produto. 
- **Onde:** Na tela de exibição do produto escolhido.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:produto");
        params.putString("eventAction","clique:botao-descricao");
        params.putString("eventLabel","[[id-produto]]:detalhes");
        mFirebaseAnalytics.logEvent("event", params);
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |

<br />

- **Quando:** Clique em Adicionar a Sacola.
- **Onde:** Na tela de exibição do produto escolhido.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:produto");
        params.putString("eventAction","clique:botao");
        params.putString("eventLabel","[[id-produto]]:adicionar-a-sacola");
        mFirebaseAnalytics.logEvent("event", params);
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |

<br />

- **Quando:**  Clique em Comprar Agora.
- **Onde:** Na tela de exibição do produto escolhido.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:produto");
        params.putString("eventAction","clique:botao");
        params.putString("eventLabel","[[id-produto]]:comprar-agora");
        mFirebaseAnalytics.logEvent("event", params);
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |

<br />

- **Quando:** Na seleção de tamanho do produto.
- **Onde:** Na tela de exibição do produto escolhido.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:produto");
        params.putString("eventAction","clique:botao");
        params.putString("eventLabel","[[id-produto]]:tamanho:[[tamanho-selecionado]]");
        mFirebaseAnalytics.logEvent("event", params);
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[tamanho-selecionado]]  | 'P', 'M', 'GG'            |  Retornar o tamanho do produto selecionado.        |


<br />

- **Quando:** Na seleção da cor do produto.
- **Onde:** Na tela de exibição do produto escolhido.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:produto");
        params.putString("eventAction","clique:botao");
        params.putString("eventLabel","[[id-produto]]:cor:[[cor-selecionada]]");
        mFirebaseAnalytics.logEvent("event", params);
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[cor-selecionada]]   | 'vermelho', 'verde', 'preto' |  Retornar a cor do produto selecionado.         |

<br />

- **Quando:** Clique para consultar o CEP.
- **Onde:** Na tela de exibição do produto escolhido.

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:produto");
        params.putString("eventAction","clique:botao");
        params.putString("eventLabel","consultar-cep");
        mFirebaseAnalytics.logEvent("event", params);
```

<br />

#### Sacola


- **Quando:** Clique em Finalizar Pedido.
- **Onde:** a tela da sacola/carrinho

```java
        Bundle params = new Bundle();
        params.putString("eventCategory","riachuelo-app:sacola");
        params.putString("eventAction","clique:botao");
        params.putString("eventLabel","finalizar-pedido");
        mFirebaseAnalytics.logEvent("event", params);
```

<br />

## Ecommerce:


### Visualização da Lista de Produtos:

- **Quando:** Na visualização/carregamento da tela de lista de produtos <br />
- **Onde:** Na tela PLP e CLP de vitrine de produtos <br />

```java
        Bundle product1 = new Bundle();
            product1.putString( Param.ITEM_ID, "[[id-produto]]");
            product1.putString( Param.ITEM_NAME, "[[nome-produto]]");
            product1.putString( Param.ITEM_CATEGORY, "[[categoria-produto]]");
            product1.putDouble( Param.PRICE, "[[preco-produto]" );

            Bundle product2 = new Bundle();
            product1.putString( Param.ITEM_ID, "[[id-produto]]");
            product1.putString( Param.ITEM_NAME, "[[nome-produto]]");
            product1.putString( Param.ITEM_CATEGORY, "[[categoria-produto]]");
            product1.putDouble( Param.PRICE, "[[preco-produto]]" );

            ArrayList items = new ArrayList();
            items.add(product1);
            items.add(product2);


        Bundle ecommerceBundle = new Bundle();
        ecommerceBundle.putBundle( "items", items );

        ecommerceBundle.putString( Param.ITEM_LIST, [[nome-lista-produto]] ); 

        mFirebaseAnalytics.logEvent( Event.VIEW_ITEM_LIST, ecommerceBundle );
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[nome-produto]]      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc.  |  Retornar o nome produto selecionado. |
| [[categoria-produto]] | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'            |  Retornar a categoria do produto selecionado.        |
| [[preco-produto]]     | '29.99', '80.00', '129.99', etc.            |  Retornar o preço do produto selecionado.        |
| [[nome-lista-produto]]| 'sapato', 'camiseta', 'calça', etc.           |  Retornar o nome da lista da qual o produto selecionado pertence.        |

<br />

### Adição a lista de desejos:

- **Quando:** Clique no 'like' de cada produto. <br />
- **Onde:** Na tela PLP e CLP de vitrine de produtos. <br />

```java
        Bundle product1 = new Bundle();
            product1.putString( Param.ITEM_ID, "[[id-produto]]");
            product1.putString( Param.ITEM_NAME, "[[nome-produto]]");
            product1.putString( Param.ITEM_CATEGORY, "[[categoria-produto]]");
            product1.putDouble( Param.PRICE, "[[preco-produto]" );

            Bundle product2 = new Bundle();
            product1.putString( Param.ITEM_ID, "[[id-produto]]");
            product1.putString( Param.ITEM_NAME, "[[nome-produto]]");
            product1.putString( Param.ITEM_CATEGORY, "[[categoria-produto]]");
            product1.putDouble( Param.PRICE, "[[preco-produto]]" );

            ArrayList items = new ArrayList();
            items.add(product1);
            items.add(product2);


        Bundle ecommerceBundle = new Bundle();
        ecommerceBundle.putBundle( "items", items );

        mFirebaseAnalytics.logEvent( Event.ADD_TO_WISHLIST, ecommerceBundle );
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[nome-produto]]      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc.  |  Retornar o nome produto selecionado. |
| [[categoria-produto]] | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'            |  Retornar a categoria do produto selecionado.        |
| [[preco-produto]]     | '29.99', '80.00', '129.99', etc.            |  Retornar o preço do produto selecionado.        |
| [[nome-lista-produto]]| 'sapato', 'camiseta', 'calça', etc.           |  Retornar o nome da lista da qual o produto selecionado pertence.        |

<br />

### Visualização do Produto Selecionado:

- **Quando:** Na visualização/carregamento do produto carregado. <br />
- **Onde:** Na tela de exibição do produto escolhido. <br />

```java
        Bundle product1 = new Bundle();
            product1.putString( Param.ITEM_ID, "[[id-produto]]");
            product1.putString( Param.ITEM_NAME, "[[nome-produto]]");
            product1.putString( Param.ITEM_CATEGORY, "[[categoria-produto]]");
            product1.putDouble( Param.PRICE, "[[preco-produto]" );

            Bundle product2 = new Bundle();
            product1.putString( Param.ITEM_ID, "[[id-produto]]");
            product1.putString( Param.ITEM_NAME, "[[nome-produto]]");
            product1.putString( Param.ITEM_CATEGORY, "[[categoria-produto]]");
            product1.putDouble( Param.PRICE, "[[preco-produto]]" );

            ArrayList items = new ArrayList();
            items.add(product1);
            items.add(product2);


        Bundle ecommerceBundle = new Bundle();
        ecommerceBundle.putBundle( "items", items );

        mFirebaseAnalytics.logEvent( Event.VIEW_ITEM, ecommerceBundle );
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[nome-produto]]      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc.  |  Retornar o nome produto selecionado. |
| [[categoria-produto]] | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'            |  Retornar a categoria do produto selecionado.        |
| [[preco-produto]]     | '29.99', '80.00', '129.99', etc.            |  Retornar o preço do produto selecionado.        |
| [[nome-lista-produto]]| 'sapato', 'camiseta', 'calça', etc.           |  Retornar o nome da lista da qual o produto selecionado pertence.        |

<br />

### Add To Cart:

- **Quando:** Clique em Comprar Agora e  Adicionar a Sacola <br />
- **Onde:**  Nos botões inferiores da tela de exibição do produto escolhido. <br />

```java
        Bundle product1 = new Bundle();
            product1.putString( Param.ITEM_ID, "[[id-produto]]");
            product1.putString( Param.ITEM_NAME, "[[nome-produto]]");
            product1.putString( Param.ITEM_CATEGORY, "[[categoria-produto]]");
            product1.putDouble( Param.PRICE, "[[preco-produto]" );
            product1.putLong( Param.QUANTITY, "[[quantidade-produto]]" );


            Bundle product2 = new Bundle();
            product1.putString( Param.ITEM_ID, "[[id-produto]]");
            product1.putString( Param.ITEM_NAME, "[[nome-produto]]");
            product1.putString( Param.ITEM_CATEGORY, "[[categoria-produto]]");
            product1.putDouble( Param.PRICE, "[[preco-produto]]" );
            product1.putLong( Param.QUANTITY, "[[quantidade-produto]]");


            ArrayList items = new ArrayList();
            items.add(product1);
            items.add(product2);


        Bundle ecommerceBundle = new Bundle();
        ecommerceBundle.putBundle( "items", items );

        mFirebaseAnalytics.logEvent( Event.ADD_TO_CART, ecommerceBundle );
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[nome-produto]]      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc.  |  Retornar o nome produto selecionado. |
| [[categoria-produto]] | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'            |  Retornar a categoria do produto selecionado.        |
| [[preco-produto]]     | '29.99', '80.00', '129.99', etc.            |  Retornar o preço do produto selecionado.        |
| [[nome-lista-produto]]| 'sapato', 'camiseta', 'calça', etc.           |  Retornar o nome da lista da qual o produto selecionado pertence.        |
| [[quantidade-produto]]     | '1', '2', '3', etc.            |  Retornar a quantidade do produto selecionado.        |


<br />



### Begin Checkout:

- **Quando:**  Na visualização/carregamento da sacola de produtos. 
 - **Onde:** Na tela de exibição da sacola carregada.

```java
        Bundle product1 = new Bundle();
            product1.putString( Param.ITEM_ID, "[[id-produto]]");
            product1.putString( Param.ITEM_NAME, "[[nome-produto]]");
            product1.putString( Param.ITEM_CATEGORY, "[[categoria-produto]]");
            product1.putDouble( Param.PRICE, "[[preco-produto]]" );
            product1.putLong( Param.QUANTITY, 2 );


        Bundle product2 = new Bundle();
            product2.putString( Param.ITEM_ID, "[[id-produto]]");
            product2.putString( Param.ITEM_NAME, "[[nome-produto]]");
            product2.putString( Param.ITEM_CATEGORY, "[[categoria-produto]]");
            product2.putDouble( Param.PRICE, "[[preco-produto]]" );
            product2.putLong( Param.QUANTITY, 3 );  

        ArrayList items = new ArrayList();
        items.add( product1 );
        items.add( product2 );

        Bundle ecommerceBundle = new Bundle();
        ecommerceBundle.putBundle( "items", items );

        ecommerceBundle.putLong( Param.CHECKOUT_STEP, 1 );
        mFirebaseAnalytics.logEvent( Event.BEGIN_CHECKOUT, ecommerceBundle );
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[nome-produto]]      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc.  |  Retornar o nome produto selecionado. |
| [[categoria-produto]] | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'            |  Retornar a categoria do produto selecionado.        |
| [[preco-produto]]     | '29.99', '80.00', '129.99', etc.            |  Retornar o preço do produto selecionado.        |
| [[nome-lista-produto]]| 'sapato', 'camiseta', 'calça', etc.           |  Retornar o nome da lista da qual o produto selecionado pertence.        |
| [[quantidade-produto]]     | '1', '2', '3', etc.            |  Retornar a quantidade do produto selecionado.        |Quantidade do produto.                                       |

<br />


### Considerações Finais:

> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    document.querySelectorAll("h1 a")[0].style.display = 'none';
  });
</script>
