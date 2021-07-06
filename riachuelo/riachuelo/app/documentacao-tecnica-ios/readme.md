![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação Firebase - Riachuelo APP - IOS

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

```swift
        Analytics.setUserID([[UserID]]);
        Analytics.setUserProperty("login_status", "[[logado-ou-nao-logado]]");
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

- **Onde:** Visualização da tela inicial do app;

```swift
    Analytics.setScreenName("/riachuelo-app/home/", screenClass: screenClass)
```

<br />

**Tela de Itens favoritos**<br />

- **Onde:** Visualização da tela de itens favoritos do app;

```swift
    Analytics.setScreenName("/riachuelo-app/favoritos/", screenClass: screenClass)
```

<br />

**Tela de Mais opções**<br />

- **Onde:** Visualização da tela de mais opcoes no app;

```swift
    Analytics.setScreenName("/riachuelo-app/mais/", screenClass: screenClass)
```

<br />

**Tela de Conta do usuário**<br />

- **Onde:** Visualização da tela de conta usuário no app;

```swift
    Analytics.setScreenName("/riachuelo-app/conta/", screenClass: screenClass)
```

<br />

**Tela PLP e CLP - Vitrine**<br />

- **Onde:** Visualização da tela PLP e CLP de vitrine de produtos - Comprar

```swift
    Analytics.setScreenName("/riachuelo-app/vitrine/", screenClass: screenClass)
```

<br />

**Visualização da tela de Produto**<br />

- **Onde:** Visualização da tela de produto carregado

```swift
    Analytics.setScreenName("/riachuelo-app/produto/", screenClass: screenClass)
```

<br />

**Tela de sacola**<br />

- **Onde:** Visualização da tela de sacola/carrinho

```swift
    Analytics.setScreenName("/riachuelo-app/sacola/", screenClass: screenClass)
```
<br />

---


### Eventos:


### Menu Inferior

- **Quando:** Clique em algum dos elementos Menu Inferior.
- **Onde:** Em todas as telas do app com Menu.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:geral" as NSObject,
        "eventAction": "clique:menu" as NSObject,
        "eventLabel": "[[item-selecionado]]" as NSObject
        ])
```

| Variável         | Exemplo                | Descrição           |
| :--------------- | :----------            | :-------------------|
| [[item-selecionado]] | 'home', 'favoritos', 'mais', 'comprar', 'conta', etc. | Retornar o item clicado do menu inferior. |


<br />


### Header

- **Quando:** Clique no icone de busca no header.
- **Onde:** Em todas as telas do app.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:geral" as NSObject,
        "eventAction": "clique:header" as NSObject,
        "eventLabel": "buscar" as NSObject
        ])
```

<br />

- **Quando:** Clique no icone de sacola no header.
- **Onde:** Em todas as telas do app.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:geral" as NSObject,
        "eventAction": "clique:header" as NSObject,
        "eventLabel": "sacola" as NSObject
        ])
```

<br />

### Vitrine

- **Quando:** Clique para alternar a visualização da lista de produtos.
- **Onde:** Na tela PLP e CLP de vitrine de produtos.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:vitrine" as NSObject,
        "eventAction": "clique:botao:modo-exibicao" as NSObject,
        "eventLabel": "alterar" as NSObject
        ])
```

<br />

- **Quando:** Clique para selecionar os filtros da visualização da lista de produtos.
- **Onde:** Na tela PLP e CLP de vitrine de produtos.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:vitrine" as NSObject,
        "eventAction": "clique:botao:modo-exibicao" as NSObject,
        "eventLabel": "filtrar:[[modo-ordenacao]]" as NSObject
        ])
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[modo-ordenacao]]    | 'grade' ou 'lista'.          |  Deve retornar o modo de ordenação selecionado. |

<br />

- **Quando:** Clique para ir para a sacola com produtos.
- **Onde:** Na tela PLP e CLP de vitrine de produtos.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:vitrine" as NSObject,
        "eventAction": "clique:botao-flutante" as NSObject,
        "eventLabel": "ir-para-sacola" as NSObject
        ])
```

<br />

- **Quando:** Clique em algum dos produtos disponiveis.
- **Onde:** Na tela PLP e CLP de vitrine de produtos.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:vitrine" as NSObject,
        "eventAction": "clique:card-produto" as NSObject,
        "eventLabel": "[[id-produto]]:[[nome-produto]]" as NSObject
        ])
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[nome-produto]]      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc.  |  Retornar o nome produto selecionado. |

<br />

### Página de produto

- **Quando:**  Clique para do 'X' para fechar a exibição do produto.
- **Onde:** Na tela de exibição do produto.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:produto" as NSObject,
        "eventAction": "clique:header" as NSObject,
        "eventLabel": "fechar" as NSObject
        ])
```

<br />

- **Quando:** Clique para ver as informações do produto.
- **Onde:** Na tela de exibição do produto.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:produto" as NSObject,
        "eventAction": "clique:botao-descricao" as NSObject,
        "eventLabel": "[[id-produto]]:informacoes" as NSObject
        ])
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |

<br />

- **Quando:**  Clique para ver os detalhes do produto. 
- **Onde:** Na tela de exibição do produto escolhido.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:produto" as NSObject,
        "eventAction": "clique:botao-descricao" as NSObject,
        "eventLabel": "[[id-produto]]:detalhes" as NSObject
        ])
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |

<br />

- **Quando:** Clique em Adicionar a Sacola.
- **Onde:** Na tela de exibição do produto escolhido.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:produto" as NSObject,
        "eventAction": "clique:botao" as NSObject,
        "eventLabel": "[[id-produto]]:adicionar-a-sacola" as NSObject
        ])
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |

<br />

- **Quando:**  Clique em Comprar Agora.
- **Onde:** Na tela de exibição do produto escolhido.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:produto" as NSObject,
        "eventAction": "clique:botao" as NSObject,
        "eventLabel": "[[id-produto]]:comprar-agora" as NSObject
        ])
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |

<br />

- **Quando:** Clique em Comprar Agora.
- **Onde:** Na tela de exibição do produto escolhido.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:produto" as NSObject,
        "eventAction": "clique:botao" as NSObject,
        "eventLabel": "[[id-produto]]:tamanho:[[tamanho-selecionado]]" as NSObject
        ])
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[tamanho-selecionado]]  | 'P', 'M', 'GG'            |  Retornar o tamanho do produto selecionado.   |


<br />

- **Quando:** Clique em Comprar Agora.
- **Onde:** Na tela de exibição do produto escolhido.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:produto" as NSObject,
        "eventAction": "clique:botao" as NSObject,
        "eventLabel": "[[id-produto]]:cor:[[cor-selecionada]]" as NSObject
        ])
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[cor-selecionada]]   | 'vermelho', 'verde', 'preto' |  Retornar a cor do produto selecionado.       |

<br />

- **Quando:** Clique para consultar o CEP.
- **Onde:** Na tela de exibição do produto escolhido.

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:produto" as NSObject,
        "eventAction": "clique:botao" as NSObject,
        "eventLabel": "consultar-cep" as NSObject
        ])
```

<br />

### Sacola


- **Quando:** Clique em Finalizar Pedido.
- **Onde:** a tela da sacola/carrinho

```swift
        Analytics.logEvent("event", parameters: [
        "eventCategory": "riachuelo-app:sacola" as NSObject,
        "eventAction": "clique:botao" as NSObject,
        "eventLabel": "finalizar-pedido" as NSObject
        ])
```

<br />

## Ecommerce:


### Visualização da Lista de Produtos:

- **Quando:** Na visualização/carregamento da tela de lista de produtos <br />
- **Onde:** Na tela PLP e CLP de vitrine de produtos <br />

```swift
    NSDictionary *product1 = @{
       kFIRParameterItemID : @"[[id-produto]]"
       kFIRParameterItemName : @"[[nome-produto]]",
       kFIRParameterItemCategory : @"[[categoria-produto]]",
       kFIRParameterPrice : @"[[preco-produto]",
    };

    NSDictionary *product2 = @{
       kFIRParameterItemID : @"[[id-produto]]"
       kFIRParameterItemName : @"[[nome-produto]]",
       kFIRParameterItemCategory : @"[[categoria-produto]]",
       kFIRParameterPrice : @"[[preco-produto]",
    };

    // Prepare ecommerce dictionary.
    NSArray *items = @[product1, product2];

    NSDictionary *ecommerce = @{
       @"items" : items,
       kFIRParameterItemList : @"[[nome-lista-produto]]"
    };

    [FIRAnalytics logEventWithName:kFIREventViewSearchResults
                    parameters:ecommerce];
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[nome-produto]]      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc.  |  Retornar o nome produto selecionado. |
| [[categoria-produto]] | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'            |  Retornar a categoria do produto selecionado.        |
| [[preco-produto]]     | '29.99', '80.00', '129.99', etc.            |  Retornar o preço do produto selecionado.        |
| [[nome-lista-produto]]| 'sapato', 'camiseta', 'calça', etc.           |  Retornar o nome da lista da qual o produto selecionado pertence.        |

<br />

### Adição a lista de desejos: ----

- **Quando:** Clique no 'like' de cada produto. <br />
- **Onde:** Na tela PLP e CLP de vitrine de produtos. <br />

```swift
    NSDictionary *product1 = @{
       kFIRParameterItemID : @"[[id-produto]]"
       kFIRParameterItemName : @"[[nome-produto]]",
       kFIRParameterItemCategory : @"[[categoria-produto]]",
       kFIRParameterPrice : @"[[preco-produto]",
    };

    NSDictionary *product2 = @{
       kFIRParameterItemID : @"[[id-produto]]"
       kFIRParameterItemName : @"[[nome-produto]]",
       kFIRParameterItemCategory : @"[[categoria-produto]]",
       kFIRParameterPrice : @"[[preco-produto]",
    };

    NSArray *items = @[product1, product2];

    NSDictionary *ecommerce = @{
       @"items" : items,
       kFIRParameterItemList : @"[[nome-lista-produto]]"
    };

    [FIRAnalytics logEventWithName:kFIREventAddToWishList
                    parameters:ecommerce];
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

```swift
    NSDictionary *product1 = @{
       kFIRParameterItemID : @"[[id-produto]]"
       kFIRParameterItemName : @"[[nome-produto]]",
       kFIRParameterItemCategory : @"[[categoria-produto]]",
       kFIRParameterPrice : @"[[preco-produto]",
    };

    NSDictionary *product2 = @{
       kFIRParameterItemID : @"[[id-produto]]"
       kFIRParameterItemName : @"[[nome-produto]]",
       kFIRParameterItemCategory : @"[[categoria-produto]]",
       kFIRParameterPrice : @"[[preco-produto]",
    };

    NSArray *items = @[product1, product2];

    NSDictionary *ecommerce = @{
       @"items" : items,
       kFIRParameterItemList : @"[[nome-lista-produto]]"
    };

    [FIRAnalytics logEventWithName:kFIREventViewItem
                        parameters:ecommerce];
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

```swift
    NSDictionary *product1 = @{
       kFIRParameterItemID : @"[[id-produto]]"
       kFIRParameterItemName : @"[[nome-produto]]",
       kFIRParameterItemCategory : @"[[categoria-produto]]",
       kFIRParameterPrice : @"[[preco-produto]",
    };

    NSDictionary *product2 = @{
       kFIRParameterItemID : @"[[id-produto]]"
       kFIRParameterItemName : @"[[nome-produto]]",
       kFIRParameterItemCategory : @"[[categoria-produto]]",
       kFIRParameterPrice : @"[[preco-produto]",
    };

    NSArray *items = @[product1, product2];

    NSDictionary *ecommerce = @{
       @"items" : items
    };

    [FIRAnalytics logEventWithName:kFIREventAddToCart
                        parameters:ecommerce];
```

| Variável              | Exemplo                      | Descrição                                     |
| :-----------------    | :------------------          | :-------------------------------------------  |
| [[id-produto]]        | '12344', '312333'            |  Retornar o id do produto selecionado.        |
| [[nome-produto]]      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc.  |  Retornar o nome produto selecionado. |
| [[categoria-produto]] | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'            |  Retornar a categoria do produto selecionado.        |
| [[preco-produto]]     | '29.99', '80.00', '129.99', etc.            |  Retornar o preço do produto selecionado.        |
| [[nome-lista-produto]]| 'sapato', 'camiseta', 'calça', etc.           |  Retornar o nome da lista da qual o produto selecionado pertence.        |
| [[quantidade-produto]]     | '1', '2', '3', etc.            |  Retornar a quantidade do produto selecionado.    |


<br />



### Begin Checkout:

- **Quando:**  Na visualização/carregamento da sacola de produtos. 
 - **Onde:** Na tela de exibição da sacola carregada.

```swift
    NSDictionary *product1 = @{
       kFIRParameterItemID : @"[[id-produto]]"
       kFIRParameterItemName : @"[[nome-produto]]",
       kFIRParameterItemCategory : @"[[categoria-produto]]",
       kFIRParameterPrice : @"[[preco-produto]",
    };

    NSDictionary *product2 = @{
       kFIRParameterItemID : @"[[id-produto]]"
       kFIRParameterItemName : @"[[nome-produto]]",
       kFIRParameterItemCategory : @"[[categoria-produto]]",
       kFIRParameterPrice : @"[[preco-produto]",
    };

    NSArray *items = @[product1, product2];

    NSDictionary *ecommerce = @{
       @"items" : items
    };

    [FIRAnalytics logEventWithName:kFIREventBeginCheckout
                        parameters:ecommerce];
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
