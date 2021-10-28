![Riachuelo](https://www.riachuelo.com.br/static/version1623357894/frontend/Corra/webjump/pt_BR/images/logo.svg)

> Área - Data Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação de Tags Firebase - Projeto Riachuelo APP

Última atualização: 28/10/2021. <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />

## Sumário 

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Definições Globais](#defini%c3%a7%c3%b5es-globais)
- [Telas](#telas)
- [Eventos - Geral](#eventos---geral)
- [Onboarding](#onboarding)
- [Eventos - Home](#eventos---home)
- [Eventos - Busca](#eventos---busca)
- [Eventos - Vitrine](#eventos---vitrine)
- [Eventos - Produto](#eventos---produto)
- [Eventos - Detalhes de produto](#eventos---detalhes-do-produto)
- [Eventos - Provador virtual](#eventos---provador-virtual)
- [Eventos - Perfil](#eventos---perfil)
- [Eventos - Sacola](#eventos---sacola)
- [Eventos - Lista de desejos](#eventos---lista-de-desejo)
- [Eventos - Login](#eventos---login)
- [Eventos - Minha Conta](#eventos---minha-conta)
- [Eventos - Painel de Notificações](#eventos---painel-de-notifica&#231;&#245;es)
- [Eventos - Checkout](#eventos---checkout)
- [Eventos - Checkout - Cupom](#eventos---checkout---cupom)
- [Eventos - Checkout - SuperApp](#eventos---checkout---superapp)
- [Eventos - Confirmação de pedido](#eventos---confirma%c3%a7%c3%a3o-de-pedido)
- [Eventos - Funil de Vendas](#eventos---funil-de-vendas)
  - [Visualização da Lista de Produtos](#visualiza%c3%a7%c3%a3o-da-lista-de-produtos)
  - [Adição à lista de desejos](#adi%c3%a7%c3%a3o-%c3%a0-lista-de-desejos)
  - [Visualização do Produto Selecionado](#visualiza%c3%a7%c3%a3o-do-produto-selecionado)
  - [Add To Cart](#add-to-cart)
  - [Begin Checkout](#begin-checkout)
  - [Checkout Progress:](#checkout-progress)
  - [Purchase](#purchase)
- [Eventos - Cartões - Super APP](#eventos---cart&#245;es---super-app)
- [Cartões - Novo Fluxo 2 Cartões SICC e TSYS ](#cart&#245;es---novo-fluxo-2-cart&#245;es-sicc-e-tsys)
- [Cartões - Antecipar Pagamento](#cart&#245;es---antecipar-pagamento)
- [Eventos - Cartões - Super APP Guideline](#eventos---cart&#245;es---super-app-guideline)
- [Eventos - Super App - Declarações Anuais](#eventos---super-app---declara&#231;&#245;es-anuais)
- [Eventos - Super App - Assistências e Seguros](#eventos---super-app---assist&#234;ncias-e-seguros)
- [Eventos - Cadastro Biometria Facial](#eventos---cadastro-biometria-facial)
- [Considerações Finais](#considera&#231;&#245;es-finais)


## Objetivo

Este documento tem como objetivo instruir a implementação de Tags de Firebase Analytics para o aplicativo de Riachuelo E-commerce.


## Implementação

> Os links abaixo descrevem como realizar a implementação do Firebase em seu aplicativo.

[Link - Adicionar o Firebase ao projeto do Android e iOS](https://rnfirebase.io/).

<br />

## Especificações Globais

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[ ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores;<br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais;<br />
Caso a informação solicitada não estiver disponível retornar tipagem `undefined`.

---

### Definições Globais

**Definições Customizadas para todas as telas:**<br />
Quando ocorrer as trocas de telas o código abaixo deve ser definido para que os hits sejam enviados ao Firebase Analytics.<br />

```javascript
Analytics.setUserId("[[UserID]]");
Analytics.setUserProperty("login_status", "[[logado-ou-nao-logado]]");
Analytics.setUserProperty("tipo_cartao", "[[tipo-do-cartao]]");
Analytics.setUserProperty("cartID", "[[id-do-carrinho]]");
```

**OBS:** Esta definição global deve ser disparada em todo momento que o usuário estiver logado.

| Nome                | Variável                   | Exemplo                  | Descrição                          |
| ------------------- | -------------------------- | ------------------------ | ---------------------------------- |
| Custumer ID Magento | `[[UserID]]`               | '9876542'                | ID único do usuário                |
| login_status         | `[[logado-ou-nao-logado]]` | ‘logado’ ou ‘nao-logado’ | Identificação de Login do usuário. |
| tipo_cartao    | `[[tipo-do-cartao]]` | 'cartao-midway-master', 'cartao-midway-visa', 'cartao-midway-pl' e etc |Deve retornar o tipo do cartão |
| cartID    | `[[id-do-carrinho]]` |  '04245fsdf4fsdaf32fsd' |Deve retornar o ID do carrinho do usuário |

--- 

### Telas

> No carregamento de todas as telas devem ser disparados os eventos de _Screenview_ com os nomes das telas do aplicativo conforme a seguir:


> A variável `[[currentClass]]` é opcional. É recomendável, no entanto, enviar o **nome da classe atual**.

---


### Eventos - Geral


- **Quando:** Clique em algum dos elementos Menu Inferior.
- **Onde:** Em todas as telas do app com Menu.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:geral",
  eventAction: "clique:menu-footer",
  eventLabel: "[[item-selecionado]]"
});
```

| Variável               | Exemplo                                  | Descrição                                 |
| :--------------------- | :--------------------------------------- | :---------------------------------------- |
| `[[item-selecionado]]` | 'home', 'comprar', 'conta', 'busca', etc | Retornar o item clicado do menu inferior. |


<br />

- **Quando:** Clique em algum dos elementos do Menu superior.
- **Onde:** Em todas as telas do app.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:geral",
  eventAction: "clique:menu-header",
  eventLabel: "[[item-clicado]]"
});
```

| Variável           | Exemplo                         | Descrição                                  |
| :----------------- | :------------------------------ | :----------------------------------------- |
| `[[item-clicado]]` | 'logo', 'buscar', 'sacola', etc | Deve retornar o título do elemento clicado |

<br />


- **Quando:** Clique no ícone de fechar, localizado na parte superior da tela.
- **Onde:** Em todas as telas do app.


```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:geral",
  eventAction: "clique:icone:fechar",
  eventLabel: "[[nome-tela]]"
});
```

| Variável           | Exemplo                         | Descrição                                  |
| :----------------- | :------------------------------ | :----------------------------------------- |
| `[[nome-tela]]` | 'explicacao-uso:consultar-limites', 'explicacao-uso:pagar-boleto' e etc |  Deve retornar o nome da tela que o botão foi clicado.|


- **Onde:** Visualização das possíveis telas de sucesso ou erro de validação de etapas de análise

```javascript
Analytics.setCurrentScreen("/[[nome-fluxo]]/[[nome-tela]]/");
```
-
| Variável           | Exemplo                         | Descrição                                  |
| :----------------- | :------------------------------ | :----------------------------------------- |
| `[[nome-fluxo]]` | 'super-app-solicitacao-cartao' e etc |  Deve retornar o nome do fluxo.|
| `[[nome-tela]]` | 'ops-tivemos-problemas', 'desculpa-nao-foi-dessa-vez', 'estamos-analisando-seu-cadastro' e etc |  Deve retornar o nome da tela apresentada.|

<br />

- **Quando:** No clique do botão
- **Onde:** Nas possíveis telas de sucesso e erro de validação de etapas de análise


```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:[[[nome-fluxo]]",
  eventAction: "clique:botao:[[nome-tela]]",
  eventLabel: "tentar-novamente"
});
```

| Variável           | Exemplo                         | Descrição                                  |
| :----------------- | :------------------------------ | :----------------------------------------- |
| `[[nome-fluxo]]` | 'super-app-solicitacao-cartao' e etc |  Deve retornar o nome do fluxo.|
| `[[nome-tela]]` | 'ops-tivemos-problemas', 'desculpa-nao-foi-dessa-vez', 'estamos-analisando-seu-cadastro' e etc |  Deve retornar o nome da tela apresentada.|

<br />

### Onboarding

**Onde:** Visualização das telas de Onboarding<br />

```javascript
    Analytics.logScreenView("/onboarding/[[titulo-da-tela]]/")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[titulo-da-tela]] | 'use-o-cartao-riachuelo', 'acesse-seu-cartao-riachuelo', 'promocoes-exclusivas', 'retire-na-loja-com-frete-gratis' e etc |Deve retornar o título da tela que o usuário visualizou.|

<br />

- **Quando:** No clique nos botões "Avançar" ou "Pular"
- **Onde:** Nas telas de Onboarding

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:onboarding",
        	eventAction: "clique:botao",
        	eventLabel: "[[titulo-da-tela]]:[[nome-botao]]"
		})
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[titulo-da-tela]] | 'use-o-cartao-riachuelo', 'acesse-seu-cartao-riachuelo', 'promocoes-exclusivas', 'retire-na-loja-com-frete-gratis' e etc |Deve retornar o título da tela que o usuário visualizou.|
| [[nome-botao]] | 'avancar' ou 'pular' | Deve retornar o nome do botão. |


<br />

---

### Eventos - Home

- **Onde:** Visualização da tela principal Bem-vindo / Home do app.

```javascript
Analytics.setCurrentScreen("/riachuelo-app/home/", "[[currentClass]]");
```

<br />

- **Quando:** No clique dos itens do header.
- **Onde:** Na home.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:home",
  eventAction: "clique:header",
  eventLabel: "[[item-clicado]]"
});
```

| Variável           | Exemplo                               | Descrição                            |
| :----------------- | :------------------------------------ | :----------------------------------- |
| `[[item-clicado]]` | 'riachuelo', 'jeans', 'plus-size' etc | Deve retornar o nome do item clicado |

<br />

- **Quando:** No clique dos banners.
- **Onde:** Na home.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:home",
  eventAction: "clique:[[nome-botao]]:banner",
  eventLabel: "[[nome-banner]]"
});
```

| Variável           | Exemplo                               | Descrição                            |
| :----------------- | :------------------------------------ | :----------------------------------- |
| `[[nome-botao]]`  | 'saiba-mais', 'conheca', 'eu-quero', etc                                     | Deve retornar o nome do botão clicado da home      |
| `[[nome-banner]]` | 'primavera-do-seu-jeito', 'stranger-things-3' 'militar-para-apostar-ja', etc | Deve retornar o nome do banner apresentado na home |

<br />

---

### Eventos - Busca


- **Onde:** Na visualização da tela de Busca.

```javascript
Analytics.setCurrentScreen("/riachuelo-app/busca/", "[[currentClass]]");
```

<br />

- **Quando:** Na interação com o campo de pesquisa.
- **Onde:** Na tela de pesquisa.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:busca",
  eventAction: "interacao:campo",
  eventLabel: "pesquisar"
});
```

<br />

- **Quando:** No clique em algum item da lista de "sugestão de busca" ou "Buscas Recentes"
- **Onde:** Na tela de pesquisa.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:busca",
  eventAction: "clique:lista:[[sugestao-recentes]]",
  eventLabel: "[[item-clicado]]"
});
```

| Variável           | Exemplo                                  | Descrição                                                           |
| :----------------- | :--------------------------------------- | :------------------------------------------------------------------ |
| `[[sugestao-recentes]]` | 'sugestao' ou 'busca-recente' | Deve retornar se o clique foi em "Buscas recentes" ou "Sugestões" |
| `[[item-clicado]]` | 'camisa-em-homens', 'camisa-em-mulheres' | Deve retornar o nome do item clicado da lista de sugestões de busca |

<br />

- **Quando:** No callback da tentativa de uma busca.
- **Onde:** Na tela de pesquisa.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:busca",
  eventAction: "tentativa:busca",
  eventLabel: "[[sucesso ou tipo-erro]]:[[termo-buscado ou termo-clicado]]"
});
```

| Variável           | Exemplo                                  | Descrição                                                           |
| :----------------- | :--------------------------------------- | :------------------------------------------------------------------ |
| `[[sucesso ou tipo-erro]]`  | 'sucesso', 'nao-encontramos-o-termo-buscado', etc | Deve retornar o sucesso ou descrição do erro       |
| `[[termo-buscado ou termo-clicado]]` | 'camisa', 'calca-jeans' | Deve retornar o termo buscado ou clicado pelo usuário |

<br />

- **Quando:** No clique do ícone de QRCode, para scanear um código do produto
- **Onde:** Na tela de pesquisa.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:busca",
  eventAction: "clique:icone",
  eventLabel: "qrcode"
});
```

<br />

- **Quando:** No callback para "Permitir" ou "Não" o acesso a câmera
- **Onde:** Na tela de pesquisa.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:busca",
  eventAction: "permissao:busca:qrcode",
  eventLabel: "[[status]]"
});
```

| Variável           | Exemplo                                  | Descrição                                                           |
| :----------------- | :--------------------------------------- | :------------------------------------------------------------------ |
| `[[status]]`  | 'permitir' ou 'nao-permitir' | Deve retornar o status do callback       |

<br />

- **Quando:** No callback do produto scaneado
- **Onde:** Na tela de pesquisa.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:busca",
  eventAction: "callback:busca-qrcode",
  eventLabel: "[[status]]:[[produto-buscado]]"
});
```

| Variável           | Exemplo                                  | Descrição                                                           |
| :----------------- | :--------------------------------------- | :------------------------------------------------------------------ |
| `[[status]]`  | 'sucesso' ou 'erro:nao-encontrado' | Deve retornar o status do callback       |
| `[[produto-buscado]]`  | 'camiseta-polo-abc', 'calca-jeans' e etc | Deve retornar o produto buscado pelo qrcode       |

<br />

---

### Eventos - Vitrine

- **Onde:** Na visualização da tela PLP de vitrine de produtos - Comprar.

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/plp/vitrine/[[categoria]]/"
);
```

| Variável        | Exemplo                              | Descrição                                         |
| :-------------- | :----------------------------------- | :------------------------------------------------ |
| `[[categoria]]` | 'jeans', 'plus-size', 'carters' etc. | Deve retornar, quando houver, a categoria do PDL. |

<br />

- **Quando:** Clique para alternar a ordenação na visualização da lista de produtos.
- **Onde:** Na tela PLP de vitrine de produtos.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:vitrine",
  eventAction: "clique:botao:modo-exibicao",
  eventLabel: "ordenacao:[[opcao-selecionada]]"
});
```

| Variável                | Exemplo                              | Descrição                                     |
| :---------------------- | :----------------------------------- | :-------------------------------------------- |
| `[[opcao-selecionada]]` | 'um-por-coluna' ou 'dois-por-coluna' | Deve retornar o item selecionado pelo usuário |

<br />

- **Quando:** Clique para selecionar os filtros da visualização da lista de produtos.
- **Onde:** Na tela PLP de vitrine de produtos.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:vitrine",
  eventAction: "clique:botao:modo-exibicao",
  eventLabel: "filtrar:[[opcao-selecionada]]"
});
```

| Variável                | Exemplo                        | Descrição                                      |
| :---------------------- | :----------------------------- | :--------------------------------------------- |
| `[[opcao-selecionada]]` | 'tamanho', 'cor', 'manga', etc | Deve retornar a opção selecionada pelo usuário |

<br />

- **Quando:** Clique para ir para a sacola com produtos.
- **Onde:** Na tela PLP de vitrine de produtos.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:vitrine",
  eventAction: "clique:header",
  eventLabel: "sacola"
});
```

<br />

- **Quando:** Clique em algum dos produtos disponíveis.
- **Onde:** Na tela PLP de vitrine de produtos.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:vitrine",
  eventAction: "clique:card-produto",
  eventLabel: "[[id-produto]]:[[nome-produto]]"
});
```

| Variável           | Exemplo                                               | Descrição                             |
| :----------------- | :---------------------------------------------------- | :------------------------------------ |
| `[[id-produto]]`   | '12344', '312333'                                     | Retornar o id do produto selecionado. |
| `[[nome-produto]]` | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.  |

<br />

---

### Eventos - Produto

- **Onde:** Visualização da PDP carregado

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/produto/"
);
```

<br />

- **Quando:** Clique para do 'X' para fechar a exibição do produto.
- **Onde:** Na tela de exibição do produto.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto",
  eventAction: "clique:header",
  eventLabel: "fechar"
});
```

<br />

- **Quando:** Clique para ver as informações do produto.
- **Onde:** Na tela de exibição do produto.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto",
  eventAction: "clique:botao-descricao",
  eventLabel: "[[id-produto]]:informacoes"
});
```

| Variável         | Exemplo                | Descrição                            |
| :--------------- | :--------------------- | :----------------------------------- |
| `[[id-produto]]` | '12344', '312333', etc | Retornar o id do produto selecionado |

<br />

- **Quando:** Clique em Adicionar a Sacola.
- **Onde:** Na tela de exibição do produto escolhido.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto",
  eventAction: "clique:botao",
  eventLabel: "[[id-produto]]:adicionar-a-sacola"
});
```

| Variável         | Exemplo           | Descrição                             |
| :--------------- | :---------------- | :------------------------------------ |
| `[[id-produto]]` | '12344', '312333' | Retornar o id do produto selecionado. |

<br />

- **Quando:** Clique em Comprar.
- **Onde:** Na tela de exibição do produto escolhido.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto",
  eventAction: "clique:botao",
  eventLabel: "[[id-pai]]:[[id-filho]]:comprar-agora",
  mktplace_seller: "[[mktplace-seller]]"
});
```

| Variável       | Exemplo                | Descrição                                                          |
| :------------- | :--------------------- | :----------------------------------------------------------------- |
| `[[id-pai]]`   | '12344', '312333', etc | Retornar o id do produto selecionado                               |
| `[[id-filho]]` | '1478', '1456', etc    | Retornar o id que representar as categorias do produto selecionado |
| `[[mktplace-seller]]` | 'amazon', 'mercado-livre' e etc   | Deve retornar o nome da loja da produto |

<br />

- **Quando:** Clique em Consulte o frete.
- **Onde:** Na tela de exibição do produto escolhido.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto",
  eventAction: "clique:botao",
  eventLabel: "[[id-pai]]:[[id-filho]]:consulte-o-frete"
});
```

| Variável       | Exemplo                | Descrição                                                          |
| :------------- | :--------------------- | :----------------------------------------------------------------- |
| `[[id-pai]]`   | '12344', '312333', etc | Retornar o id do produto selecionado                               |
| `[[id-filho]]` | '1478', '1456', etc    | Retornar o id que representar as categorias do produto selecionado |

<br />

- **Onde:** Visualização da tela "Consulte o frete".

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/produto/consulte-frete/"
);
```

<br />

- **Quando:** Na interação com o campo "CEP".
- **Onde:** Na tela de "Consulte o frete"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto",
  eventAction: "interacao:consulte-frete:campo",
  eventLabel: "[[id-pai]]:[[id-filho]]:preencheu:cep"
});
```

| Variável       | Exemplo                | Descrição                                                          |
| :------------- | :--------------------- | :----------------------------------------------------------------- |
| `[[id-pai]]`   | '12344', '312333', etc | Retornar o id do produto selecionado                               |
| `[[id-filho]]` | '1478', '1456', etc    | Retornar o id que representar as categorias do produto selecionado |

<br />

- **Quando:** No clique dos links ou botões.
- **Onde:** Na tela de "Consulte o frete"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto",
  eventAction: "clique:[[botao ou link]]:consulte-frete",
  eventLabel: "[[id-pai]]:[[id-filho]]:[[nome-item]]"
});
```

| Variável       | Exemplo                | Descrição                                                          |
| :------------- | :--------------------- | :----------------------------------------------------------------- |
| `[[botao ou link]]`   | 'botao' ou 'link' | Deve retornar o tipo de elemento clicado                             |
| `[[id-pai]]`   | '12344', '312333', etc | Retornar o id do produto selecionado                               |
| `[[id-filho]]` | '1478', '1456', etc    | Retornar o id que representar as categorias do produto selecionado |
| `[[nome-item]]`   | 'nao-sei-o-cep', 'consultar' | Deve retornar o nome do item clicado.             |

<br />

- **Onde:** Visualização da tela "Selecione onde deseja receber a entrega".

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/produto/consulte-frete/selecione-entrega/"
);
```

<br />

- **Quando:** No clique dos botões.
- **Onde:** Na tela "Selecione onde deseja receber a entrega".

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto",
  eventAction: "clique:botao:selecione-entrega",
  eventLabel: "[[id-pai]]:[[id-filho]]:[[nome-botao]]"
});
```

| Variável       | Exemplo                | Descrição                                                          |
| :------------- | :--------------------- | :----------------------------------------------------------------- |
| `[[id-pai]]`   | '12344', '312333', etc | Retornar o id do produto selecionado                               |
| `[[id-filho]]` | '1478', '1456', etc    | Retornar o id que representar as categorias do produto selecionado |
| `[[nome-botao]]`   | 'editar-endereco', 'cadastrar-outro-endereco'. |Deve retornar o nome do item selecionado. |

<br />

- **Onde:** Visualização da tela "Selecione o modo de envio".

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/produto/consulte-frete/selecione-entrega/selecione-envio/"
);
```

<br />


- **Quando:** Ao selecionar alguma opção do modo de envio.
- **Onde:** Na tela "Selecione o modo de envio"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto",
  eventAction: "interacao:selecione-envio:selecionou",
  eventLabel: "[[id-pai]]:[[id-filho]]:[[nome-item]]:[[valor]]:[[data-prevista]]"
});
```

| Variável       | Exemplo                | Descrição                                                          |
| :------------- | :--------------------- | :----------------------------------------------------------------- |
| `[[id-pai]]`   | '12344', '312333', etc | Retornar o id do produto selecionado                               |
| `[[id-filho]]` | '1478', '1456', etc    | Retornar o id que representar as categorias do produto selecionado |
| `[[nome-item]]`   |'retirar-na-loja', 'normal', 'agendada' e etc | Deve retornar o nome do item selecionado.  |
| `[[valor]]`   |  '10.00', 'gratis' e etc|Deve retornar o valor do frete selecionado.  |
| `[[data-prevista]]`   |  '05/06/2019', '10/06/2019' e etc |Deve retornar a data prevista do frete selecionado |

<br />


---


### Eventos - Detalhes de produto

- **Onde:** Na visualização dos detalhes de um produto.

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/produto/detalhes/"
);
```

<br />

- **Quando:** No clique em algum dos botões.
- **Onde:** Na tela de detalhes de um produto [Detalhes do Produto 1 - zeplin] e [Detalhes do Produto 2 - 1 - zeplin].

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto:detalhes",
  eventAction: "clique:botao",
  eventLabel: "[[id-pai]]:[[nome-botao]]"
});
```

| Variável         | Exemplo    | Descrição                                                     |
| :--------------- | :--------------------------------------- | :------------------------------ |
| `[[id-pai]]`     | '12344', '312333', etc         | Retornar o id do produto selecionado      |
| `[[nome-botao]]` | 'sacola', 'voltar', 'adicionar-a-sacola', 'sair', 'selecionar-cor-preto', 'selecionar-tamanho-xxg', 'comprar' etc. | Deve retornar o nome do botão clicado.                             |

<br />

- **Quando:** No clique em um dos produtos apresentados nas recomendações de produto.
- **Onde:** Na tela de detalhes de um produto [Detalhes do Produto 2 - 1 - zeplin], abaixo de informações.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto:detalhes",
  eventAction: "clique:botao",
  eventLabel: "[[id-pai]]:[[titulo-recomendacao]]"
});
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[id-pai]]`     | '12344', '312333', etc            | Retornar o id do produto selecionado                               |
| `[[titulo-recomendacao]]` | 'esse-look-combina-com', 'quem-viu-tambem-viu', 'voce-pode-gostar', etc |  Deve retornar o título da seção de recomendação clicacada pelo usuário |

<br />

- **Quando:** No clique em um botão "Ver tudo".
- **Onde:** Na tela de detalhes de um produto [Detalhes do Produto 2 - 1 - zeplin].

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:produto:detalhes",
  eventAction: "clique:botao",
  eventLabel: "recomendacao-ver-tudo"
});
```

<br />

---

### Eventos - Provador virtual

- **Onde:** Visualização da tela de 'Provador virtual'.

```javascript
Analytics.setCurrentScreen("/riachuelo-app/produto/detalhes/provador-virtual/");
Analytics.logEvent("event",{"dimension3":"[[recomendacao-provador-virtual]]}");
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[recomendacao-provador-virtual]]`    |  'm', 'g'  e etc | Deve retornar o tamanho dado na recomendação do provador virtual.  |

<br />

---

### Eventos - Perfil

- **Onde:** Na visualização da tela de conta usuário no app.

```javascript
Analytics.setCurrentScreen("/riachuelo-app/conta/", "[[currentClass]]");
```

<br />

- **Onde:** Na visualização da tela de Meus dados.

```javascript
Analytics.setCurrentScreen("/riachuelo-app/meus-dados/", "[[currentClass]]");
```

<br />

- **Onde:** Na visualização de uma das telas apresentadas dentro de Meus dados.

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/conta/meus-dados/[[tela]]/"
);
```

| Variável             | Exemplo                                           | Descrição                       |
| :------------------- | :------------------------------------------------ | :------------------------------ |
| `[[tela]]` | 'dados-pessoais', 'seguranca', 'redefinicao-de-senha', 'meus-enderecos', 'meus-pedidos', 'meus-cartoes', 'comprar-agora', etc | Deve retornar o título da tela acessada pelo usuário |

<br />


- **Onde:** Visualização da tela de 'Perfil'.

```javascript
Analytics.setCurrentScreen("/riachuelo-app/perfil/", "[[currentClass]]");
Analytics.logEvent("event",{"dimension4":"[[quantidade-perfis]]}");

```

| Variável             | Exemplo                                           | Descrição                       |
| :------------------- | :------------------------------------------------ | :------------------------------ |
| `[quantidade-perfis]]` | '1', '2'  e etc | Deve retornar a quantidade de perfis que o usuário possui de medidas |


<br />

- **Quando:** No clique nos botões.
- **Onde:** Na tela de Perfil.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:provador-virtual:perfil",
  eventAction: "clique:botao",
  eventLabel: "[[nome-botao]]"
});
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[nome-botao]]`     |  'fechar', 'gerenciar-perfis', 'minhas-medidas', 'privacidade', 'fornecido-pela-sizebay', 'ver-todas' e etc   | Deve retornar o nome do botão clicado.   |

<br />

---


### Eventos - Sacola

- **Onde:** Na visualização da tela de sacola - carrinho.

```javascript
Analytics.setCurrentScreen("/riachuelo-app/sacola/", "[[currentClass]]");
```

<br />

- **Quando:** No callback de sincronização.
- **Onde:** Na tela da sacola - carrinho

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:sacola",
  eventAction: "callback:sincronizacao",
  eventLabel: "[[status]]"
});
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[status]]`     | 'sucesso:outros-produtos-foram-adicionados-a-sacola' , 'erro: voce-atingiu-o-numero-maximo-para-este-produto' e etc  | Deve retornar a mensagem de sucesso ou tipo de erro  |


<br />

- **Quando:** No clique dos sellers em "Vendido e entregue por", "vendidor por", "entregue por"
- **Onde:** Na tela da sacola - carrinho

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:sacola",
  eventAction: "clique:link",
  eventLabel: "[[tipo]]:[[seller]]:[[nome-produto]]",
  mktplace_seller: "[[mktplace_seller]]"
});
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[tipo]]`     | 'vendido-entregue', 'vendido', 'entregue'  |  Deve retornar o tipo do link.  |
| `[[seller]]`     |  'riachuelo' e etc  | Deve retornar o nome do seller clicado.   |
| `[[nome-produto]]`     | 'camisa-basica-preta' e etc  |  Deve retornar o nome do produto |
| `[[mktplace_seller]]`     | amazon', 'mercado-livre' e etc | Deve retornar o nome da loja da produto |


<br />

- **Quando:**  No clique para selecionar o modo de envio
- **Onde:** Na tela da sacola - carrinho

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:sacola",
  eventAction: "clique:opcao:modo-envio",
  eventLabel: "[[tipo-envio]]:[[nome-produto]]",
});
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[tipo-envio]]`     | 'normal' e etc |  Deve retornar o tipo de envio escolhido. |
| `[[nome-produto]]`     | 'camisa-basica-preta' e etc  |  Deve retornar o nome do produto |


<br />

- **Quando:**   No clique em "Cupom"
- **Onde:** Na tela da sacola - carrinho

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:sacola",
  eventAction: "clique:opcao",
  eventLabel: "cupom",
});
```

<br />

- **Quando:**   Na tentiva de callback para adicionar cupom
- **Onde:** Na tela da sacola - carrinho

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:sacola",
  eventAction: "callback:cupom",
  eventLabel: "[[status]]",
});
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[status]]`     | 'sucesso', 'erro:cupom-removido' e etc |  Deve retornar a mensagem de sucesso ou tipo de erro.  |


<br />


- **Quando:**   No clique dos botões
- **Onde:**  Na tela da sacola - carrinho

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:sacola",
  eventAction: "clique:botao",
  eventLabel: "[[nome-botao]]",
});
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[nome-botao]]`     | 'ir-para-pagamento', 'continuar-comprando' |  Deve retornar o nome do botão clicado. |


<br />

- **Onde:** Visualização da tela dos ligthbox de atenção'

```javascript
Analytics.setCurrentScreen("/riachuelo-app/sacola/erro-atencao/", "[[currentClass]]");
```

<br />

- **Quando:**   No clique dos botões
- **Onde:**  Na tela lightbox de atenção

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:sacola",
  eventAction: "clique:botao:lightbox-atencao",
  eventLabel: "[[nome-lightbox]]:[[nome-botao]]",
});
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[nome-lightbox]]`     | houve-mudancas-nos-valores', 'reajuste-a-quantidade-de-itens' e erc | Deve retornar o nome do lightbox |
| `[[nome-botao]]`     | 'ok-entendi', 'remover-itens' e etc | Deve retornar o nome do botão clicado. |


<br />


---

### Eventos - Lista de desejos

- **Onde:** Visualização da tela de 'Lista de desejos'

```javascript
Analytics.setCurrentScreen("/riachuelo-app/lista-de-desejos/", "[[currentClass]]");
```

<br />

- **Quando:** No clique do botão 'Ir para home' para quando a lista de desejos estiver vazia 
- **Onde:** Na tela de Lista de desejos

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:lista-de-desejos",
  eventAction: "clique:botao",
  eventLabel: "ir-para-home"
});
```

<br />

- **Quando:** No clique em um dos botões apresentadas para quando existem itens na lista de desejos; 
- **Onde:** Na tela de Lista de desejos

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:lista-de-desejos",
  eventAction: "clique:botao",
  eventLabel: "[[id-pai]]:[[id-filho]]:[[titulo-botao]]"
});
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[id-pai]]`     | '12344', '312333', etc            | Retornar o id do produto selecionado                               |
| `[[id-filho]]`   | '1478', '1456', etc               | Retornar o id que representar as categorias do produto selecionado |
| `[[titulo-botao]]` | 'remover', 'adicionar-a-sacola', etc |   Retornar o título do botão clicado, exemplo |

<br />


- **Quando:** No clique do ícone de 'Compartilhar'
- **Onde:** Na tela de Lista de desejos


```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:lista-de-desejos",
  eventAction: "clique:icone",
  eventLabel: "compatilhar-lista-de-desejos"
});
```

<br />

- **Quando:** Na interação com o lightbox após clicar no ícone ou botão de Compartilhar
- **Onde:** Na tela de Lista de desejos

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:lista-de-desejos",
  eventAction: "lightbox:clique:lista",
  eventLabel: "compartilhar-por",
  dimension1: "[[compartilhar-por]]"
});
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[compartilhar-por]]`     | 'whatsapp', 'facebook', 'bluetooth' e etc      | Deve retornar o nome da aplicação que o usuário selecionou para compartilhar            |

<br />

- **Quando:**  No retorno de sucesso para Adicionar o produto à sacola
- **Onde:** Na tela de Lista de desejos após o clique em Adicionar à sacola

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:lista-de-desejos",
  eventAction: "tentativa:adicionar-a-sacola",
  eventLabel: "sucesso"
});
```

- **Quando:** Na interação com os botões sim ou não no modal 'Tem certeza que deseja excluir o item da Lista de dessejos?'
- **Onde:** Na tela de Lista de desejos após o clique em 'remover'

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:lista-de-desejos",
  eventAction: "modal:clique:botao",
  eventLabel: "excluir-lista-de-desejo:[[sim-nao]]"
});
```

| Variável         | Exemplo                           | Descrição                                                          |
| :--------------- | :-------------------------------- | :----------------------------------------------------------------- |
| `[[sim-nao]]`     | 'sim' ou 'nao'            | Retornar o item clicado.

<br />

- **Onde:** Visualização dos detalhes de um produto vindo da Lista de Desejos

```javascript
Analytics.setCurrentScreen("/riachuelo-app/produto/detalhes-wish-list/", "[[currentClass]]");
```

<br />

- **Quando:**  No clique do ícone de 'Adicionar aos favoritos' representado por um coração
- **Onde:** Na página PLP, PDP e nos rotates de produtos

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:lista-de-desejos",
  eventAction: "clique:icone",
  eventLabel: "adicionar-a-lista-de-desejos"
});
```
<br />

---

### Eventos - Login

- **Onde:** Visualização da tela de autenticação - cpf e senha

```javascript
Analytics.setCurrentScreen("/riachuelo-app/autenticacao/[[nome-step]]/", "[[currentClass]]");
```

<br />


- **Quando:** Na interação com os campos do formulário de login.
- **Onde:** Na tela da login.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:login",
  eventAction: "interacao:campo:formulario",
  eventLabel: "[[nome-campo]]"
});
```

| Variável         | Exemplo        | Descrição                                |
| :--------------- | :------------- | :--------------------------------------- |
| `[[nome-campo]]` | 'cpf', 'senha' | Deve retornar o nome do campo preenchido |

<br />

- **Quando:** No clique dos elementos (botões e links) do formulário de login.
- **Onde:** Na tela da login.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:login",
  eventAction: "clique:[[tipo-elemento]]:formulario",
  eventLabel: "[[nome-elemento]]"
});
```

| Variável            | Exemplo                                              | Descrição                                     |
| :------------------ | :--------------------------------------------------- | :-------------------------------------------- |
| `[[tipo-elemento]]` | 'botao' ou 'link'                                    | Deve retornar o tipo do elemento clicado      |
| `[[nome-elemento]]` | 'entrar', 'continuar', 'esqueci-a-senha', 'cancelar' | Deve retornar o nome do botão ou link clicado |

<br />

- **Quando:** No callback da tentativa de login;
- **Onde:** Na tela da login.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:login",
  eventAction: "envio:formulario",
  eventLabel: "[[sucesso ou tipo-erro]]"
});
```

| Variável                   | Exemplo                                      | Descrição                                             |
| :------------------------- | :------------------------------------------- | :---------------------------------------------------- |
| `[[sucesso ou tipo-erro]]` | 'sucesso', 'erro:favor-digitar-seu-cpf', etc | Deve retornar o sucesso ou descrição do erro de login |

<br />

### Eventos - Minha Conta

**Visualização da tela de "Minha Conta"**<br />

```javascript
Analytics.setCurrentScreen("/riachuelo-app/minha-conta:[[login_status]]/");
```

| Variável        | Exemplo         | Descrição           |
| :-------------- | :-------------- | :------------------ |
| [[login_status]] | 'logado' ou 'nao-logado' | Identificação de Login do usuário |

<br />

- **Quando:** Na clique das opções
- **Onde:** Na tela de "Minha Conta"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo-app:minha-conta",
        	"eventAction": "clique:opcao:[[secao]]",
        	"eventLabel": "[[opcao-selecionada]]"
        })
```

| Variável        | Exemplo         | Descrição           |
| :-------------- | :-------------- | :------------------ |
| [[secao]] | 'minha-conta', 'produtos-riachuelo', 'suporte' e etc | Retorna a seção que o usuário interagir. |
| [[opcao-selecionada]] | Logado ou Não Logado | Identificação de Login do usuário |

<br />

### Eventos - Painel de Notificações

**Visualização da tela &quot;Minha conta&quot;, com a opção de mensagens ainda não visualizadas.**<br />


```javascript
Analytics.setCurrentScreen("/riachuelo-app/minha-conta/mensagens:[[quantidade]]/", "[[currentClass]]");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[quantidade]] | &#039;1&#039;, &#039;2&#039; e etc | Deve retornar a quantidade de mensagens não visualizadas. |

<br />

- **Quando:** No clique da opção &quot;Mensagens&quot;
- **Onde:** Na tela de &quot;Minha Conta&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo-app:minha-conta",
        	"eventAction": "clique:opcao",
        	"eventLabel": "mensagens"
        })
```


<br />

**Visualização da tela de &quot;Mensagens&quot;, quando o cliente não autorizou o recebimento de notificações**<br />


```javascript
Analytics.setCurrentScreen("/riachuelo-app/minha-conta/mensagens/nao-autorizou-notificacoes/", "[[currentClass]]");
```

<br />

- **Quando:** No clique do botão &quot;Ativar Mensagens&quot;
- **Onde:** Na tela de &quot;Mensagens&quot;, seção &quot;Não autorizou o recebimento de notificações&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo-app:notificacoes",
        	"eventAction": "clique:botao",
        	"eventLabel": "ativar-mensagens"
        })
```


<br />

**Visualização da tela de &quot;Mensagens&quot;, quando o cliente ainda não possui mensagens**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/minha-conta/mensagens/ainda-nao-possui-mensagens/", "[[currentClass]]")
```

<br />

**Visualização da tela de &quot;Mensagens&quot;, quando o cliente possui mensagens para ler**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/minha-conta/mensagens/mensagens-nao-lidas/", "[[currentClass]]")
```

<br />

- **Quando:** No clique para ler alguma &quot;Mensagem&quot; exibida
- **Onde:** Na tela de &quot;Mensagens para ler&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo-app:notificacoes",
        	"eventAction": "clique:mensagem",
        	"eventLabel": "[[nome-mensagem]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-mensagem]] | &#039;ja-recebemos-o-seu-pedido&#039;, &#039;25-so-no-app&#039; e etc. | Deve retornar o título da mensagem clicada. |

<br />

---

### Eventos - Checkout

- **Onde:** Na visualização das telas relacionadas a checkout.

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/checkout/[[nome-step]]"
);
```

| Variável        | Exemplo                                                                                | Descrição                  |
| :-------------- | :------------------------------------------------------------------------------------- | :------------------------- |
| `[[nome-step]]` | 'entregar-em', 'modo-envio', 'cupom', 'cartao-presente' e etc | Nome do passo do checkout. |

<br />


- **Quando:** No clique dos botões
- **Onde:** Nas telas relacionadas a checkout

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:botao",
  eventLabel: "[[nome-botao]]:[[step]]"
});
```

| Variável        | Exemplo                                    | Descrição                            |
| :-------------- | :----------------------------------------- | :----------------------------------- |
| `[[nome-botao]]` |  'selecione-ou-altere-endereco', 'selecione-modo-envio', 'insira-um-cupom' e etc |  Deve retornar o nome do botão clicado. |
| `[[step]]` |  'entregar-em', 'modo-envio', 'cupom', 'cartao-presente' e etc | Deve retornar o nome da etapa de checkout. |

<br />

- **Quando:** Nos ícones para "Voltar"
- **Onde:** Nas telas relacionadas a checkout

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:icone",
  eventLabel: "voltar:[[step]]"
});
```

| Variável        | Exemplo                                    | Descrição                            |
| :-------------- | :----------------------------------------- | :----------------------------------- |
| `[[step]]` |  'entregar-em', 'modo-envio', 'cupom', 'cartao-presente' e etc | Deve retornar o nome da etapa de checkout. |

<br />

- **Quando:** Ao selecionar um endereço ja cadastrado
- **Onde:** Na tela "Entregar em"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:opcao:entregar-em",
  eventLabel: "[[posicao-endereco]]"
});
```

| Variável        | Exemplo                                    | Descrição                            |
| :-------------- | :----------------------------------------- | :----------------------------------- |
| `[[posicao-endereco]]` | 'posicao-1', 'posicao-2' e etc | Deve retornar a posição do endereço selecionado. |

<br />

- **Quando:** No clique do botão "Novo endereço"
- **Onde:** Na tela "Entregar em"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:botao:entregar-em",
  eventLabel: "novo-endereco"
});
```

<br />

- **Quando:** Ao selecionar uma opção de entrega.
- **Onde:** Na tela de entrega.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "interacao:modo-de-envio:[[opcao-selecionada]]",
  eventLabel: "[[previsao]]:[[valor]]",
  mktplace_seller: "[[mktplace_seller]]"
});
```

| Variável                | Exemplo                                       | Descrição                                          |
| :---------------------- | :-------------------------------------------- | :------------------------------------------------- |
| `[[opcao-selecionada]]` | 'normal', 'expresso', 'entrega-agendada', etc | Deve retornar o nome da opção de frete selecionada |
| `[[previsao]]`          | '10-dias', '03-dias ' etc                     | Deve retornar a quantidade de dias para a entrega  |
| `[[valor]]`             | '10.59', '14.90' etc                          | Deve retornar o valor do frete                     |
| `[[mktplace_seller]]`   | amazon', 'mercado-livre' e etc   | Deve retornar o nome da loja da produto                         |

<br />


- **Quando:** No clique em uma loja para retirada de pedido.
- **Onde:** Na tela de entrega, após escolher "Retirar em loja".

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:lojas-proximas:retirada-em-loja",
  eventLabel: "[[nome-loja]]",
  mktplace_seller: "[[mktplace_seller]]"
});
```

| Variável        | Exemplo                 | Descrição                            |
| :-------------- | :---------------------- | :----------------------------------- |
| `[[nome-loja]]` | 'bourbon-shopping', etc | Deve retornar o nome da loja clicada |
| `[[mktplace_seller]]`   | amazon', 'mercado-livre' e etc   | Deve retornar o nome da loja da produto                         |

<br />

- **Quando:** No clique para trocar a loja selecionada para retirada do pedido.
- **Onde:** Na tela de entrega, após escolher uma loja para retirada.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:botao:entrega",
  eventLabel: "alterar-loja"
});
```

<br />

- **Quando:** Ao selecionar opções de entrega agendada.
- **Onde:** Na tela de entrega agendada.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "interacao:entrega-agendada",
  eventLabel: "[[data ou periodo]]:[[opcao-selecionada]]",
  mktplace_seller: "[[mktplace_seller]]"
});
```

| Variável                | Exemplo                        | Descrição                         |
| :---------------------- | :----------------------------- | :-------------------------------- |
| `[[data ou periodo]]`   | 'data' ou 'periodo'            | Deve retornar o campo preenchido  |
| `[[opcao-selecionada]]` | '10-12-2019', 'manha', 'tarde' | Deve retornar a opção selecionada |
| `[[mktplace_seller]]`   | amazon', 'mercado-livre' e etc   | Deve retornar o nome da loja da produto                         |

<br />

- **Quando:** No callback da tentativa de agendamento de entrega.
- **Onde:** Na tela de entrega agendada.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "tentativa:entrega-agendada",
  eventLabel: "[[sucesso ou tipo-erro]]",
  mktplace_seller: "[[mktplace_seller]]"
});
```

| Variável                   | Exemplo                               | Descrição                                    |
| :------------------------- | :------------------------------------ | :------------------------------------------- |
| `[[sucesso ou tipo-erro]]` | 'sucesso', 'data-nao-disponivel', etc | Deve retornar o sucesso ou descrição do erro |
| `[[mktplace_seller]]`   | amazon', 'mercado-livre' e etc   | Deve retornar o nome da loja da produto                         |

<br />

- **Quando:** No clique dos itens da página de opções de pagamento.
- **Onde:** Na tela de pagamento.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:[[secao-pagamento]]:pagamento",
  eventLabel: "[[opcao-selecionada]]"
});
```

| Variável                   | Exemplo                               | Descrição                                    |
| :------------------------- | :------------------------------------ | :------------------------------------------- |
| `[[secao-pagamento]]`   | 'seus-cartoes' ou 'forma-de-pagamento', 'escolha-outra-forma-de-pagamento'       | Deve retornar o nome da seção do item clicado |
| `[[opcao-selecionada]]` | 'boleto', 'cartao-de-credito', 'outro-cartao-de-credito' 'cartao-riachuelo', etc | Deve retornar a opção selecionada             |

<br />

- **Quando:** Na exibição de um alerta de pagamento indisponível.
- **Onde:** Na tela de pagamento.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "exibicao:erro",
  eventLabel: "ops-no-momento-a-forma-de-pagamento-selecionada-nao-esta-disponivel"
});
```

<br />

- **Quando:** Na interação com os campos de "Adicionar cartão".
- **Onde:** Na tela para adicionar cartão.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "interacao:adicionar-cartao:pagamento",
  eventLabel: "[[nome-campo]]"
});
```

| Variável         | Exemplo                                                   | Descrição                                |
| :--------------- | :-------------------------------------------------------- | :--------------------------------------- |
| `[[nome-campo]]` | 'numero-do-cartao', 'nome-impresso-no-cartao', 'cvv', etc | Deve retornar o nome do campo preenchido |

<br />

- **Quando:** Ao selecionar uma opção em "Adicionar cartão";
- **Onde:** Na tela para adicionar cartão.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "interacao:adicionar-cartao:pagamento",
  eventLabel: "[[nome-campo]]:[[opcao-selecionada]]"
});
```

| Variável                | Exemplo                                    | Descrição                                 |
| :---------------------- | :----------------------------------------- | :---------------------------------------- |
| `[[nome-campo]]`        | 'tipo-do-cartao', 'quantidade-de-parcelas' | Deve retornar o nome do campo selecionado |
| `[[opcao-selecionada]]` | 'visa', 'american-express', 1x-74.96       | Deve retornar o nome da opção selecionada |

<br />

- **Quando:** Ao selecionar se deseja ou não salvar o cartão  para as próximas compras;
- **Onde:**  Na tela para adicionar cartão em pagamento.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "interacao:opcao",
  eventLabel: "salvar-cartao:[[sim-nao]]"
});
```

| Variável                | Exemplo                                    | Descrição                                 |
| :---------------------- | :----------------------------------------- | :---------------------------------------- |
| `[[sim-nao]]`        | 'sim' ou 'nao' |  Deve retornar a opção selecionada pelo usuario. |


<br />

- **Quando:** No clique em "Anterior", "Próximo".
- **Onde:** Na tela para adicionar cartão.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:adicionar-cartao:pagamento",
  eventLabel: "[[nome-item]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[nome-item]]` | 'anterior', 'proximo' | Deve retornar o nome do item clicado |

<br />


- **Quando:** No callback de erro de pagamento.
- **Onde:** Na tela de pagamento.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "tentativa:pagamento:[[tipo-pagamento]]",
  eventLabel: "erro:[[tipo-erro]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[tipo-pagamento]]` | 'cartao-de-credito' | Deve retornar o nome do tipo do pagamento relacionado ao erro |
| `[[tipo-erro]]`      | 'infelizmente-sua-compra-nao-pode-ser-realizada', 'sua-compra-nao-pode-ser-realizado-entre-em-contato-com-a-operadora-do-seu-cartao', etc | Deve retornar a descrição do erro |

<br />

- **Quando:** No clique em "Alterar forma de pagamento".
- **Onde:** Na tela de pagamento.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:pagamento:erro",
  eventLabel: "alterar-forma-de-pagamento"
});
```

<br />

- **Onde:** Visualização do lightbox de 'Cartão Presente'

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/checkout/cartao-presente/"
);
```

<br />

- **Quando:** Após clicar nos botões para aplicar ou excluir o cartão presente.
- **Onde:** Na tela para adicionar cartão.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:lightbox:cartao-presente:callback",
  eventLabel: "[[nome-botao]]:[[status]]",
  metric1: "[[valor-cartao-presente]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[nome-botao]]` |  'aplicar', 'excluir' e etc |  Deve retornar o nome do botão clicado. |
| `[[status]]` | 'sucesso', 'erro-numero-invalido', 'cartao-presente-sucesso', 'erro-cartao-nao-possui-saldo', 'cartao-presente-removido' e etc | Deve retornar o status do callback.  |
| `[[valor-cartao-presente]]` | '1200.00' e etc | Deve retornar valor em reais do cartão presente inserido. |


<br />


- **Quando:** No clique em "Finalizar Compra.
- **Onde:** Na tela de resumo do pedido.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:botao:resumo-do-pedido",
  eventLabel: "finalizar-compra"
});
```

<br />

- **Quando:** No callback, após carregar a tela de purchase incompleto
- **Onde:** Na tela de Purchase

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "callback:purchase",
  eventLabel: "purchase-incompleto"
  transaction_id: "[[transaction_id]]" 
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[transaction_id]]` | '44955789' e etc |  Deve retornar ID da transação |

<br />


### Eventos - Checkout - Cupom

- **Onde:** Na visualização da tela de Cupom.

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/checkout/cupom/"
);
```

<br />

- **Quando:** No clique em 'Cupom'.
- **Onde:** Na tela de 'Compra'.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:opcao",
  eventLabel: "cupom"
});
```

<br />

- **Quando:** Após o preenchimento do campo 'insira um cupom'.
- **Onde:** Na tela de 'Cupom'.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "preencheu:campo",
  eventLabel: "[[texto-cupom]]"
});
```

| Variável          | Exemplo                      | Descrição                                            |
| :---------------- | :--------------------------- | :--------------------------------------------------- |
| `[[texto-cupom]]` | 'rchl10', 'blackfriday' etc. | Deve retornar o nome do cupom inserido pelo usuário. |


<br />


- **Quando:** No clique em um dos botões.
- **Onde:** Na tela de 'Cupom'.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:botao",
  eventLabel: "cupom:[[nome-botao]]"
});
```

| Variável         | Exemplo                             | Descrição                              |
| :--------------- | :---------------------------------- | :------------------------------------- |
| `[[nome-botao]]` | 'voltar', 'aplicar', 'excluir' etc. | Deve retornar o nome do botão clicado. |

<br />


- **Quando:** No callback de sucesso ou erro de inserção de cupom.
- **Onde:** Na tela de 'Cupom'.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "tentativa:inserir-cupom",
  eventLabel: "sucesso:[[mensagem]] ou erro:[[mensagem]]",
  dimension5: "[[codigo-do-cupom]]"
});
```

| Variável          | Exemplo                      | Descrição                                            |
| :---------------- | :--------------------------- | :--------------------------------------------------- |
| `[[mensagem]]`  | 'sucesso:desconto-aplicado-com-sucesso', 'erro:para-inserir-outro-cupom-e-preciso-excluir-o-cupom-acima', 'erro:nao-foi-possivel-aplicar' etc. | Deve retornar a mensagem de sucesso ou erro ocorrido.      |
| `[[codigo-do-cupom]]` | 'rchlo123', etc.                                 | Deve retornar o cupom adicionado no carrinho. |

<br />

---

### Eventos - Checkout - SuperApp

- **Quando:**  No clique dos botões , no modal de 'vimos que você possui um cartão riachuelo' e no modal de erro 'Algo deu errado'.
- **Onde:**  Nas telas de pagamento.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "clique:botao",
  eventLabel: "[[botao-clicado]]"
});
```

| Variável          | Exemplo                      | Descrição                                            |
| :---------------- | :--------------------------- | :--------------------------------------------------- |
| `[[botao-clicado]]`  | 'preencher-cartao-pl-automaticamente:cancelar', 'preencher-cartao-manualmente', 'preencher-cartao-pl-automaticamente:sim' |Deve retornar o botão clicado.       |

<br />


- **Quando:**   Ao realizar a foto de reconhecimento facial, após o usuario aceitar preencher o cartão automaticamente.
- **Onde:**  Nas telas de pagamento.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:checkout",
  eventAction: "reconhecimento-facial:cartao-rchlo",
  eventLabel: "[[mensagem-tela]]"
});
```

| Variável          | Exemplo                      | Descrição                                            |
| :---------------- | :--------------------------- | :--------------------------------------------------- |
| `[[mensagem-tela]]`  |  'nao-se-mexa', 'enquadre-rosto-no-contorno', 'de-um-sorriso' e etc.| Deve retornar a mensagem exibida na tela .|


<br />


- **Quando:** Callback de sucesso ( Disparar junto ao purchase)
- **Onde:** Na tela de pedido finalizado

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo-app:checkout" ,
        	eventAction: "callback:pedido-finalizado",
        	eventLabel: "sucesso" ,
        	metodo_pagamento: "[[metodo_pagamento]]" ,
		tipo_entrega: "[[tipo_entrega]]" ,
        	qtdeParcelas: "[[qtdeParcelas]]",
		cartao_presente: "[[cartao_presente]]",
		vale_troca:"[[vale_troca]]",
		pagamento_comum:"[[pagamento_comum]]"
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
|[[metodo_pagamento]]|  &#039;rchlo-pl&#039;, &#039;rchlo-master&#039;, &#039;boleto&#039;, &#039;cartao-presente-e-vale-troca&#039;, &#039;boleto-e-cartao-presente&#039;, &#039;cartao-de-credito-e-vale-troca&#039; e etc etc |Deve retornar o metodo de pagamento escolhido, no caso de mais de uma forma de pagamento, retornar os pagamentos escolhidos de forma concatenada|
| [[tipo_entrega]] | &#039;normal&#039;, &#039;expressa&#039; &#039;normal-e-retira-em-loja&#039;, &#039;normal-normal-e-expressa&#039; e etc |Deve retornar o tipo de entrega escolhido, no caso de mais de um tipo de entrega, retornar as formas de entrega escolhidas de forma concatenada |
| [[qtdeParcelas]] | &#039;1&#039;, &#039;2&#039;, &#039;12&#039; e etc |Deve retornar a quantidade da parcela do plano selecionado dos produto de assistencia ou seguro |
| [[cartao_presente]] |  &#039;100.00 &#039;,  &#039;50.00 &#039;,  &#039;20.00 &#039; e etc |Deve retornar qual o valor do cartão presente( Caso não tenha cartão presente, retornar undefined)|
| [[vale_troca]] |  &#039;100.00 &#039;,  &#039;50.00 &#039;,  &#039;20.00 &#039; e etc |Deve retornar qual o valor do vale troca( Caso não tenha vale troca, retornar undefined)|
| [[pagamento_comum]] |  &#039;100.00 &#039;,  &#039;50.00 &#039;,  &#039;20.00 &#039; e etc |Deve retornar qual o valor do pagamento comum sem valor do vale troca ou cartão presente|





<br />


### Eventos - Confirmação de pedido

- **Onde:** Visualização da tela de Confirmação de pedido.

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/checkout/sucesso/"
);
```

<br />

- **Quando:** No clique dos botões da tela de confirmação de pedido.
- **Onde:** Na tela de confirmação de pedido.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:confirmacao",
  eventAction: "clique:botao:pedido",
  eventLabel: "[[nome-botao]]"
});
```

| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[nome-botao]]` | 'copiar-codigo', 'compartilhar', 'acompanhe-seu-pedido' | Deve retornar o nome do botão clicado |

<br />

- **Quando:** Ao abrir ou fechar uma seção de informações do pedido.
- **Onde:** Na tela de confirmação de compra.

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:confirmacao",
  eventAction: "interacao:[[nome-secao]]",
  eventLabel: "[[abrir ou fechar]]"
});
```

| Variável              | Exemplo                                               | Descrição                             |
| :-------------------- | :---------------------------------------------------- | :------------------------------------ |
| `[[nome-secao]]`      | 'endereco-para-entrega', 'descontos', 'pagamento' etc | Deve retornar o nome da seção clicada |
| `[[abrir ou fechar]]` | 'abrir' ou 'fechar'                                   | Deve retornar a ação do usuário       |


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

| Variável                | Exemplo                                               | Descrição                                    |
| :---------------------- | :---------------------------------------------------- | :------------------------------------------- |
| `[[nome-lista]]`        |  'produtos', 'busca', 'camisas', 'tenis' e etc.                                     | Deve retornar a lista de itens.        |
 `[[id-produto]]`        | '12344', '312333'                                     | Retornar o id do produto selecionado.        |
| `[[categoria-produto]]` | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado. |
| `[[nome-produto]]`      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.         |
| `[[preco-produto]]`     | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.     |

### Adição à lista de desejos

- **Quando:** Clique no 'like' de cada produto.
- **Onde:** Na tela PLP de vitrine de produtos.

```javascript
Analytics.logAddToWishlist({
    item_id: '[[id-produto]]',
    item_name: '[[nome-produto]]',
    item_category: '[[categoria-produto]]',
    price: '[[preco-produto]'
}
```

| Variável                | Exemplo                                               | Descrição                                    |
| :---------------------- | :---------------------------------------------------- | :------------------------------------------- |
| `[[id-produto]]`        | '12344', '312333'                                     | Retornar o id do produto selecionado.        |
| `[[nome-produto]]`      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.         |
| `[[categoria-produto]]` | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado. |
| `[[preco-produto]]`     | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.     |

### Visualização do Produto Selecionado

- **Quando:** Na visualização/carregamento do produto carregado.
- **Onde:** Na tela de exibição do produto escolhido.

```javascript
Analytics.logViewItem({
  item_id: "[[id-produto]]",
  item_name: "[[nome-produto]]",
  item_category: "[[categoria-produto]]",
  price: "[[preco-produto]"
});
```

| Variável                | Exemplo                                               | Descrição                                    |
| :---------------------- | :---------------------------------------------------- | :------------------------------------------- |
| `[[id-produto]]`        | '12344', '312333'                                     | Retornar o id do produto selecionado.        |
| `[[nome-produto]]`      | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.         |
| `[[categoria-produto]]` | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado. |
| `[[preco-produto]]`     | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.     |

### Add To Cart

- **Quando:** Clique em Comprar,  Adicionar a Sacola e no sinal de adição (+), dentro do carrinho
- **Onde:** Nos botões inferiores da tela de exibição do produto escolhido e na tela de sacola.

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

| Variável                 | Exemplo                                               | Descrição                                     |
| :----------------------- | :---------------------------------------------------- | :-------------------------------------------- |
| `[[id-produto]]`         | '12344', '312333'                                     | Retornar o id do produto selecionado.         |
| `[[nome-produto]]`       | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.          |
| `[[categoria-produto]]`  | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado.  |
| `[[preco-produto]]`      | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.      |
| `[[coupon]]`      | 'rchlo123' etc.                      | Cupom utilizado na compra.      |
| `[[valor-total]]`      | '355.40', etc.                      | Valor total da compra.      |
| `[[quantidade-produto]]` | '1', '2', '3', etc.                                   | Retornar a quantidade do produto selecionado. |

### Remove From Cart

- **Quando:** Clique no ícone X e no sinal de subtração (-), dentro do carrinho
- **Onde:** Na tela de sacola

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

| Variável                 | Exemplo                                               | Descrição                                     |
| :----------------------- | :---------------------------------------------------- | :-------------------------------------------- |
| `[[id-produto]]`         | '12344', '312333'                                     | Retornar o id do produto selecionado.         |
| `[[nome-produto]]`       | 'camisa-polo-preta-basic', 'camisa-slim-basica', etc. | Retornar o nome produto selecionado.          |
| `[[categoria-produto]]`  | 'camisa-polo', 'sapato-mocasin', 'calça-jeans'        | Retornar a categoria do produto selecionado.  |
| `[[preco-produto]]`      | '29.99', '80.00', '129.99', etc.                      | Retornar o preço do produto selecionado.      |
| `[[coupon]]`      | 'rchlo123' etc.                      | Cupom utilizado na compra.      |
| `[[valor-total]]`      | '355.40', etc.                      | Valor total da compra.      |
| `[[quantidade-produto]]` | '1', '2', '3', etc.                                   | Retornar a quantidade do produto selecionado. |

### Begin Checkout

- **Quando:** Na visualização/carregamento da sacola de produtos.
- **Onde:** Na tela de exibição da sacola carregada.

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

| Variável                 | Exemplo                                               | Descrição                                    |
| :----------------------- | :---------------------------------------------------- | :------------------------------------------- |
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

| Variável                 | Exemplo                                               | Descrição                                    |
| :----------------------- | :---------------------------------------------------- | :------------------------------------------- |
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

| Variável                 | Exemplo                                               | Descrição                                    |
| :----------------------- | :---------------------------------------------------- | :------------------------------------------- |
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

| Variável                         | Exemplo           | Descrição                      |
| :------------------------------- | :---------------- | :----------------------------- |
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

---

### Eventos - Cartões - Super APP

- **Onde:** Visualização do modal de "ATENÇÃO! Não foi possível autenticar com segurança neste momento" logo após de tentar acessar a aba de cartões do App

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/cartoes/atencao-nao-foi-possivel-autenticar/"
);
```

<br />

- **Quando:** No clique dos botões e icones
- **Onde:** No modal "ATENÇÃO! Não foi possível autenticar com segurança neste momento" logo após de tentar acessar a aba cartões do App

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:modal-atencao-nao-foi-possivel-autenticar",
  eventAction: "clique:[[botao-icone]]",
  eventLabel: "[[nome-item]]"
});
```

| Variável         | Exemplo         | Descrição          |
| :--------------- | :-------------- | :----------------- |
| `[[botao-icone]]` | 'botao' ou 'icone' | Deve retornar o elemento clicado. |
| `[[nome-item]]` | 'voltar', 'tentar-novamente', 'fechar' ou 'clique-fora' e etc | Deve retornar o nome do item. |

<br />

- **Onde:** Visualização da tela "Midway".

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/cartoes/midway/"
);
```

<br />

- **Quando:** No clique dos botões
- **Onde:** Na tela "Midway"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "clique:botao:cartao-rchlo",
  eventLabel: "[[nome-botao]]"
});
```

| Variável         | Exemplo         | Descrição          |
| :--------------- | :-------------- | :----------------- |
| `[[nome-botao]]` | &#039;fazer-login&#039;, &#039;solicitar-cartao&#039; e etc | Deve retornar o nome do botão clicado |

<br />

**Visualização do modal de &quot;Sobre o cartão Riachuelo&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/modal-sobre-cartao-riachuelo/");
```

<br />

- **Quando:** No clique dos botões
- **Onde:** No modal de &quot;Sobre o cartão Riachuelo&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "clique:botao:modal-sobre-cartao-riachuelo",
  eventLabel: "[[nome-botao]]"
});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-botao]]` | &#039;entendi-vamos-la&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

**Visualização da tela de &quot;Onboarding&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/onboarding-[[numero-tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[numero-tela]]` | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o número da tela de explicação. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nas telas  de &quot;Onboarding&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:onboarding",
  eventAction: "clique:[[botao-link]]:tela-[[numero-tela]]",
  eventLabel: "[[nome-item]]"
});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[numero-tela]]` | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o número da tela de explicação. |
| `[[botao-link]]` | &#039;botao &#039;ou &#039;link&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;saiba-mais&#039;, &#039;pular&#039;, &#039;avancar&#039;, &#039;entendi&#039; e etc  | Deve retornar o nome do item clicado. |


<br />

**Visualização dos modais de &quot;Saiba mais&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/modal-saiba-mais-[[numero-tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[numero-tela]]` | &#039;1&#039; ou &#039;3&#039; | Deve retornar o número da tela de saiba mais. |

<br />

- **Quando:** No clique dos botões
- **Onde:** Nos modais de &quot;Saiba mais&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:onboarding",
  eventAction: "clique:botao:saiba-mais-[[numero-tela]]",
  eventLabel: "fechar"
});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[numero-tela]]` | &#039;1&#039; ou &#039;3&#039; | Deve retornar o número da tela de saiba mais. |

<br />

**Visualização da tela de &quot;Preciso para cartão riachuelo&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/preciso-para-cartao-riachuelo/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nas telas de &quot;Preciso para cartão riachuelo&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:preciso-para-cartao-riachuelo",
  eventAction: "clique:[[botao-icone]]",
  eventLabel: "[[nome-item]]"
});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;voltar&#039; ou &#039;avancar&#039; | Deve retornar o nome do item. |

<br />

**Visualização das telas de &quot;Bem vindo&quot; e depois &quot;Contato&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/[[nome-tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-tela]]` | &#039;bem-vindo&#039;, &#039;contato&#039; e etc | Deve retornar o nome da tela. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nas telas de &quot;Bem vindo&quot; e &quot;Contato&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:[[nome-tela]]",
  eventAction: "clique:[[botao-icone]]",
  eventLabel: "[[nome-item]]"
});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-tela]]` | &#039;bem-vindo&#039;, &#039;contato&#039; e etc | Deve retornar o nome da tela. |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;voltar&#039; ou &#039;avancar&#039; | Deve retornar o nome do item. |

<br />

**Visualização da tela de "Bem vindo"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/possui-cartao-riachuelo/");
```

<br />

- **Quando:** No clique dos botões
- **Onde:** Nas tela de "Bem vindo"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:possui-cartao-riachuelo",
  eventAction: "clique:botão",
  eventLabel: "vincular-cartao"
});
```

<br />

**Visualização da tela de "Possui solicitação proposta ativa"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/possui-solicitacao-proposta-ativa/");
```

<br />

- **Quando:** No clique dos botões
- **Onde:** Nas tela de "Possui solicitação proposta ativa"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:cadastro-possui-solicitacao-proposta-ativa",
  eventAction: "clique:botão",
  eventLabel: "acompanhar-status"
});
```

<br />

**Na tela de "Não será possível prosseguir solicitação de fraude"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/nao-sera-possivel-prosseguir-solicitacao-fraude/");
```

<br />

- **Quando:** No clique dos botões
- **Onde:** Nas tela de "Não será possível prosseguir solicitação de fraude"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:nao-sera-possivel-prosseguir-solicitacao-fraude",
  eventAction: "clique:botão",
  eventLabel: "ok-entendi"
});
```

<br />

**Na tela de "Desculpe não foi dessa vez"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/desculpe-nao-foi-dessa-vez/");
```

<br />

- **Quando:** No clique dos botões
- **Onde:** Nas tela de "Desculpe não foi dessa vez"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:desculpe-nao-foi-dessa-vez",
  eventAction: "clique:botão",
  eventLabel: "ok-entendi"
});
```

<br />

**Nos possíveis callbacks de erros ao "Solicitar cartão"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/ops-tivemos-um-problema/");
```

<br />

- **Quando:** Callback de erro ao solicitar cartão
- **Onde:** Nos possíveis callbacks de erros ao "Solicitar cartão"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:solicitacao-cartao",
  eventAction: "callback",
  eventLabel: "[[erro]]"
});
```
| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[erro]]` | 'possui-cartao-riachuelo', 'possui-solicitacao-proposta-ativa', 'nao-sera-possivel-prosseguir-solicitacao-fraude', 'desculpe-nao-foi-dessa-vez', 'erro-tivemos-um-problema' e etc. | Deve retornar o erro. |

<br />

- **Quando:** Após o preenchimento dos campos.
- **Onde:** Nas telas de &quot;Bem vindo&quot; e &quot;Contato&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:[[nome-tela]]",
  eventAction: "interacao:campo",
  eventLabel: "[[nome-campo]]"
});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-tela]]` | &#039;bem-vindo&#039;, &#039;contato&#039; e etc | Deve retornar o nome da tela. |
| `[[nome-campo]]` | &#039;cpf&#039;, &#039;nome-completo&#039;, &#039;data-nascimento&#039;, &#039;celular&#039;, &#039;email&#039; e etc | Deve retornar o nome do campo. |

<br />

- **Quando:** Callback de erro do preenchimento dos campos.
- **Onde:** Nas telas de &quot;Bem vindo&quot; e &quot;Contato&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:[[nome-tela]]",
  eventAction: "interacao:campo:callback",
  eventLabel: "[[erro-campo]]"
});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-tela]]` | &#039;bem-vindo&#039;, &#039;contato&#039; e etc | Deve retornar o nome da tela. |
| `[[erro-campo]]` | &#039;erro:cpf-invalido&#039;, &#039;erro:nome-incompleto&#039;, &#039;erro:cpf-invalido/data-invalida&#039;, &#039;erro:celular-invalido&#039;, &#039;erro:email-invalido&#039;, &#039;erro:celular-invalido/email-invalido&#039; e etc | Deve retornar o erro encontrado nos campos. |

<br />

**Visualização da tela de &quot;Código SMS&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/codigo-sms/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;Código SMS&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:codigo-sms",
  eventAction: "clique:[[botao-icone]]",
  eventLabel: "[[nome-item]]"
});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;avancar&#039;, &#039;enviar-codigo-novamente&#039; e etc | Deve retornar o nome do item. |

<br />

- **Quando:** Após o preenchimento do campo
- **Onde:** Na tela de &quot;Código SMS&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:codigo-sms",
  eventAction: "interacao:campo",
  eventLabel: "codigo"
});
```

<br />

- **Quando:** Callback de erro do preenchimento do campo
- **Onde:** Na tela de &quot;Código SMS&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:codigo-sms",
  eventAction: "interacao:campo:callback",
  eventLabel: "[[erro-codigo]]"
});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[erro-codigo]]` | &#039;erro:codigo-invalido&#039;, &#039;erro:codigo-expirado&#039; e etc | Deve retornar o erro encontrado no campo. |

<br />

**Visualização da tela de &quot;Termos de privacidade&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/termos-privacidade/");
```

<br />

**Visualização da tela de &quot;informações adicionais&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/informacoes-adicionais/");
```


<br />

- **Quando:** Após o preenchimento ou seleção do campo
- **Onde:** Na tela de &quot;informações adicionais&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:informacoes-adicionais",
        	eventAction: "interacao:campo",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-campo]]` | &#039;rg-rne&#039;, &#039;nome-mae&#039;, &#039;renda&#039;, &#039;nacionalidade:brasileiro(a)&#039;, &#039;profissao:assalariado&#039;, &#039;nif&#039;, &#039;pais:estados-unidos&#039; e etc | Deve retornar o nome do campo preenchido ou selecionado. |

<br />

- **Quando:** Callback de erro do preenchimento do campo
- **Onde:** Na tela de &quot;informações adicionais&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:informacoes-adicionais",
        	eventAction: "interacao:campo:callback",
        	eventLabel: "[[erro-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[erro-campo]]` | &#039;erro:rg-invalido&#039;, &#039;erro:nome-mae-incorreto&#039;, &#039;erro:nif-nao-encontrado&#039; e etc | Deve retornar o erro encontrado no campo. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;informações adicionais&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:informacoes-adicionais",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]`] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;avancar&#039; e etc | Deve retornar o nome do item. |

<br />

**Visualização da tela de &quot;Endereço&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/endereco/");
```


<br />

- **Quando:** Após o preenchimento do campo
- **Onde:** Na tela de &quot;Endereço&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:endereco",
        	eventAction: "interacao:campo",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-campo]]` | &#039;cep&#039;, &#039;endereco&#039;, &#039;numero&#039;, &#039;complemento&#039; e etc | Deve retornar o nome do campo preenchido, em alguns casos os campos já viram preenchidos, retornar da mesma maneira. |

<br />

- **Quando:** Callback de erro do preenchimento do campo
- **Onde:** Na tela de &quot;Endereço&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:endereco",
        	eventAction: "interacao:campo:callback",
        	eventLabel: "[[erro-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[erro-campo]]` | &#039;erro:cep-invalido&#039;, &#039;erro:endereco-nao-encontrado&#039; e etc | Deve retornar o erro encontrado no campo. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;Endereço&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:endereco",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]`] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;avancar&#039; e etc | Deve retornar o nome do item. |

<br />

**Visualização da tela de &quot;Acesso a câmera&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/acesso-camera/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;Acesso a câmera&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:acesso-camera",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]`] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;habilitar-camera&#039; e etc | Deve retornar o nome do item. |

<br />

**Visualização das telas de dicas de &quot;Foto do rosto&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/foto-rosto-[[num-tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[num-tela]]`| &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o número da tela. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nas telas de dicas de &quot;Foto do rosto&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:foto-rosto-[[num-tela]]",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[num-tela]]` | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o número da tela. |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;pular-dicas&#039;, &#039;avancar&#039; e etc | Deve retornar o nome do item. |

<br />

**Visualização da tela de &quot;Tire a foto&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/tire-foto/");
```

<br />

- **Quando:** No clique do botão "Tire foto"
- **Onde:** Na tela de "Tire a foto"

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:tire-foto",
        	eventAction: "clique:botao",
        	eventLabel: "tirar-foto"
        });
```

<br />

**Visualização da tela de "Carregando sua selfie"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/carregando-selfie/");
```

<br />

**Visualização das telas de possíveis callbacks de &quot;Tire a foto&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/[[callback-possivel]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[callback-possivel]]` | &#039;selfie-aprovada&#039;, &#039;ops-selfie-recusada&#039;, &#039;atencao&#039; e etc | Deve retornar o callback. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nas telas de possíveis callbacks de &quot;Tire a foto&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:[[callback-possivel]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[callback-possivel]]` | &#039;selfie-aprovada&#039;, &#039;ops-selfie-recusada&#039;, &#039;atencao&#039; e etc | Deve retornar o callback. |
| `[[nome-item]]` | &#039;continuar&#039;, &#039;tirar-novamente&#039;, &#039;fechar&#039; e etc | Deve retornar o nome do item. |

<br />

**Visualização da tela de "Reenvio foto"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/tire-foto/reenvio/");
```

<br />

- **Quando:** No clique do botão "Tire foto"
- **Onde:** Na tela de "Reenvio foto"

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:tire-foto-reenvio",
        	eventAction: "clique:botao",
        	eventLabel: "tirar-foto"
        });
```

<br />

**Visualização da tela de &quot;Documentos&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/documentos/");
```


<br />

- **Quando:** Após selecionar um dos elementos
- **Onde:** Na tela de &quot;Documentos&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:documentos",
        	eventAction: "selecionou:opcao",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-item]] `| &#039;rg&#039;, &#039;cnh&#039;, &#039;rne&#039; e etc | Deve retornar o nome do item. |

<br />

**Visualização das telas de dicas de &quot;Foto do documento&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/foto-documento-[[num-tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[num-tela]]` | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o número da tela. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nas telas de dicas de &quot;Foto do documento&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:foto-documento-[[num-tela]]",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[num-tela]]` | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o número da tela. |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;pular-dicas&#039;, &#039;avancar&#039; e etc | Deve retornar o nome do item. |

<br />

**Visualização das telas de &quot;Foto RG Frente&quot;, &quot;Foto RG Verso&quot;, &quot;Foto RNE verso&quot;, &quot;Foto CNH&quot; e etc**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/[[tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[tela]]` | &#039;foto-rg-frente&#039;, &#039;foto-rg-verso&#039;, &#039;foto-rne-verso&#039;, &#039;foto-cnh&#039; e etc | Deve retornar a tela apresentada. |

<br />

**Visualização da tela de "Tire a foto"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/tire-foto/[[nome-item]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-item]]` |'foto-rg-frente', 'foto-rg-verso', 'foto-rne-verso', 'foto-cnh' e etc | Deve retornar a tela apresentada. |

<br />

- **Quando:** No clique do botão "Tire foto"
- **Onde:** Na tela de "Foto do documento"

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:foto-documento-[[num-tela]]",
        	eventAction: "clique:botao",
        	eventLabel: "tirar-foto"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[num-tela]]` | '1', '2', '3' e etc | Deve retornar o número da tela. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nas telas de &quot;Foto RG Frente&quot;, &quot;Foto RG Verso&quot;, &quot;Foto RNE verso&quot;, &quot;Foto CNH&quot; e etc

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:[[tela]]",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[tela]]` | &#039;foto-rg-frente&#039;, &#039;foto-rg-verso&#039;, &#039;foto-rne-verso&#039;, &#039;foto-cnh&#039; e etc | Deve retornar a tela apresentada. |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;capturar-novamente&#039;, &#039;continuar&#039; e etc | Deve retornar o nome do item. |

<br />

**Visualização da tela de &quot;Vencimento da fatura&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/vencimento-fatura/");
```


<br />

- **Quando:** Na interação dos campos
- **Onde:** Na tela de &quot;Vencimento da fatura&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:vencimento-fatura",
        	eventAction: "interacao:campo",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-item]]` | 'vencimento-05', 'vencimento-06', vencimento-07' e etc. | Deve retornar o nome do item selecionado |

<br />

- **Quando:** No clique dos botões ou ícones
- **Onde:** Na tela de &quot;Vencimento da fatura&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:vencimento-fatura",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;avancar:05&#039;, &#039;avancar:08&#039; e etc | Deve retornar o nome do item. |

<br />

**Visualização da tela de &quot;Termos e condições&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/termos-condicoes/");
```

<br />

- **Quando:** No clique do botão em "Termos e condições"
- **Onde:** Na tela de "Termos e condições"

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:termos-condicoes",
        	eventAction: "clique:botao",
        	eventLabel: "li-e-concordo-com-as-condicoes"
        });
```

<br />

**Visualização da tela de "Tire foto validação"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/tire-foto-validacao/");
```

<br />

- **Quando:** No clique do botão "Tire foto"
- **Onde:** Na tela de "Tire foto validação"

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:tire-foto-validacao",
        	eventAction: "clique:botao",
        	eventLabel: "tirar-foto"
        });
```

<br />

**Visualização da tela de "Carregando selfie validação"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/carregando-selfie-validacao/");
```

<br />

**Visualização da tela de "Selfie aprovada validação"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/selfie-aprovada-validacao/");
```

<br />

- **Quando:** No clique do botão
- **Onde:** Na tela de "Selfie aprovada validação"

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:selfie-aprovada-validacao",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```

<br />

**Visualização da tela de "Enviando dados"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/enviando-dados-short/");
```

<br />

**Visualização da tela de "Reenviando dados"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/reenvio-dados-short/");
```

<br />

- **Quando:** No clique do botão
- **Onde:** Na tela de "Reeviando dados"

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:reenvio-de-documentos",
        	eventAction: "clique:botao",
        	eventLabel: "enviar-documento"
        });
```

<br />

**Visualizção da tela de "Recuperando dados"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/recuperando-dados-solicitacao/");
```

<br />

**Visualização da tela de &quot;Em análise&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/em-analise-[[long ou short]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[long ou short]]` | 'long' ou 'short' | Deve retornar o fluxo percorrido. |

<br />

- **Quando:** No clique do link
- **Onde:** Na tela de &quot;Em análise&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:em-analise-[[long ou short]]",
        	eventAction: "clique:link",
        	eventLabel: "como-funciona-analise"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[long ou short]]` | 'long' ou 'short' | Deve retornar o fluxo percorrido. |

<br />

**Visualização do modal de &quot;Como funciona a análise&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/modal-como-funciona-analise/");
```


<br />

- **Quando:** No clique do botão
- **Onde:** No modal de &quot;Como funciona a análise&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:modal-como-funciona-analise",
        	eventAction: "clique:botao",
        	eventLabel: "fechar"
        });
```


<br />

**Visualização das telas de &quot;Acompanhe seu status&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/acompanhe-seu-status-[[num-tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[num-tela]]` | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o número da tela. |

<br />

- **Quando:** No clique do botão
- **Onde:** Nas telas de &quot;Acompanhe seu status&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:acompanhe-seu-status",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-botao]]` | &#039;ok&#039;, &#039;continuar&#039; e etc | Deve retornar o nome do botão clicado. |



<br />

**Visualização das telas de &quot;Precisar de uma nova foto&quot; (identificação, residência ou renda)**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/precisamos-nova-foto-[[tipo]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[tipo]]` | &#039;identificacao&#039;, &#039;residencia&#039;, &#039;renda&#039; e etc | Deve retornar o tipo da tela apresentada. |

<br />

- **Quando:** No clique do botão
- **Onde:** Nas telas de &quot;Precisar de uma nova foto&quot; (identificação, residência ou renda)

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:precisamos-nova-foto-[[tipo]]",
        	eventAction: "clique:botao",
        	eventLabel: "tirar-foto"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[tipo]]` | &#039;identificacao&#039;, &#039;residencia&#039;, &#039;renda&#039; e etc | Deve retornar o tipo da tela apresentada. |

<br />

**Visualização da tela de &quot;Documento enviado, analisando novamente&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/documento-reenviado-analisando-novamente/");
```

<br />

**Visualização da tela de "Parabéns! Solicitação aprovada"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/parabens-solicitacao-aprovada/");
```

<br />

- **Quando:** Após o callback de sucesso ou erro na solicitação do cartão
- **Onde:** Na tela de “Parabéns! Solicitação aprovada” após solicitar o fluxo de cartão

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:solicitacao-cartao",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[status]]` | &#039;sucesso&#039;, &#039;nao-foi-desta-vez&#039; e etc | Deve retornar o status do callback. |

<br />

- **Quando:** Após o callback de sucesso ou erro de callback de vinculo
- **Onde:** Na telas de “Parabéns! Solicitação aprovada”

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:parabens-solicitacao-aprovada",
        	eventAction: "auto-vinculo:callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[status]]` | &#039;sucesso&#039;, &#039;nao-foi-desta-vez&#039; e etc | Deve retornar o status do callback. |

<br />

- **Quando:** No clique dos botões
- **Onde:** Na telas de “Parabéns! Solicitação aprovada”

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes:parabens-solicitacao-aprovada",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-botao]]` | &#039;ver-limite&#039;, &#039;ir-para-loja-online&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

**Visualização do modal de &quot;Atenção oferta especial cartão&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/modal-atencao-oferta-especial-cartao/")
```

<br />

- **Quando:** No clique dos botões
- **Onde:** No modal de &quot;Atenção oferta especial cartão&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:modal-atencao-oferta-especial-cartao",
  eventAction: "clique:botao",
  eventLabel: "[[nome-botao]]"
});
```

| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[nome-botao]]` | &#039;agora-nao&#039;, &#039;tenho-interesse&#039; e etc | Deve retornar o nome do botão clicado |

<br />

- **Quando:** Após o callback de sucesso
- **Onde:** No modal de &quot;Atenção oferta especial cartão&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:modal-atencao-oferta-especial-cartao",
  eventAction: "callback",
  eventLabel: "[[status]]"
});
```

| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[status]]` | 'obrigado:em-breve-entraremos-em-contato' ou 'erro:sem-servico' e etc | Deve retornar o status do callback |

<br />

- **Quando:** No clique do botão de "Acessar cartões"
- **Onde:** Na terceira tela  de onboarding (Pagar boleto)".

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:onboarding",
  eventAction: "clique:botao:cartao-rchlo",
  eventLabel: "pagar-boleto:acessar-cartoes"
});
```

<br />


- **Onde:** Visualização da tela "Associe a o seu cartão Riachuelo".

```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/cartoes/midway/associe-riachuelo/"
);
```

<br />

- **Quando:** Na interação com o campo 4 ultimos digitos
- **Onde:** Na tela "Associe a o seu cartão Riachuelo"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "interacao:campo:app-cartao-rchlo",
  eventLabel: "associe-cartao-riachuelo:preencheu:4-ultimos-digitos"
});
```

<br />

- **Quando:** No clique dos botões
- **Onde:** Na tela "Associe a o seu cartão Riachuelo"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "clique:botao:app-cartao-rchlo",
  eventLabel: "associe-cartao-riachuelo:[[nome-botao]]"
});
```

| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[nome-botao]]` | 'vincular-cartao', 'gerar-boleto' | Deve retornar o nome do botão clicado |

<br />

- **Quando:** Na tentativa de callback para vincular cartão
- **Onde:** Na tela "Associe a o seu cartão Riachuelo"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "callback:app-cartao-rchlo",
  eventLabel: "associe-cartao-riachuelo:[[status]]"
});
```

| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[status]]` | 'sucesso', 'erro:numero-errado' e etc | Deve retornar a mensagem de sucesso ou tipo de erro. |

<br />

- **Onde:** Na visualização do modal, logo após vincular o cartão"

```javascript
Analytics.setCurrentScreen("/riachuelo-app/mini-onboarding-[[passo]]-super-app");

```

| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[passo]]` | &#039;1&#039;, &#039;2&#039;, &#039;3&#039;, &#039;4&#039; e etc.| Deve retornar em qual das 4 telas do modal o usuario esta |


<br />


- **Quando:** Ao clicar no botão fechar
- **Onde:** No modal de miniOnboarding, logo após vincular o cartão

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:super-app:mini-onboarding-[[passo]]",
  eventAction: "clique:botao",
  eventLabel: "fechar"
});
```

| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[passo]]` |  &#039;1&#039;, &#039;2&#039;, &#039;3&#039;, &#039;4&#039; e etc. | Deve retornar em qual das 4 telas do modal o usuario esta  |


<br />


- **Quando:** Ao clicar no botão "acessar cartões."
- **Onde:** No modal de miniOnboarding, logo após vincular o cartão 

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:super-app:mini-onboarding-[[passo]]",
  eventAction: "clique:botao",
  eventLabel: "acessar-cartoes"
});
```

| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[passo]]` | &#039;1&#039;, &#039;2&#039;, &#039;3&#039;, &#039;4&#039; e etc.| Deve retornar em qual das 4 telas do modal o usuario esta |


<br />

- **Quando:** Na visualização da tela de "Sistema de indisponivel" na seção dos cartões
```javascript
Analytics.setCurrentScreen("/riachuelo-app/sistema-indisponivel-super-app");
```

<br />

- **Quando:** Visualização da tela "Limite"

```javascript
Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/associe-riachuelo/limite/");
Analytics.logEvent("event",{"status_fatura":"[[status_fatura]]"});
```

<br />

- **Quando:** No clique dos itens ao lado do limite disponivel
- **Onde:** Na tela &quot;Limite&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes",
        	"eventAction": "clique:limite-disponivel",
        	"eventLabel": "[[nome-item]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;ver-detalhes&#039;, &#039;visualizar-saldo&#039;, &#039;esconder-saldo&#039;, &#039;erro-ao-carregar-limite&#039;, &#039;declaracoes-anuais&#039; e etc. | Deve retornar o nome do  item clicado. |

<br />

- **Quando:** No clique dos botões
- **Onde:** Na tela &quot;Limite&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes",
        	"eventAction": "clique:botao:app-cartao-rchlo",
        	"eventLabel": "limite:[[nome-secao]]:[[nome-botao]]" as })
        ])
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-secao]] | &#039;fatura-aberta&#039;, &#039;fatura-fechada&#039;, &#039;fatura-em-atraso&#039; e etc | Deve retornar o nome da seção que o usuário está interagindo. |
| [[nome-botao]] | &#039;fatura&#039;, &#039;acessar-app-cartoes-rchlo&#039;, &#039;ver-lancamentos&#039;, &#039;ver-boleto&#039;, &#039;ver-extrato&#039;, &#039;opcoes-pagamento&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique em copiar boleto
- **Onde:** Na tela &quot;Limite&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes",
        	"eventAction": "clique:copiar-boleto",
        	"eventLabel": "[[periodo]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[periodo]] | &#039;antes-data-vencimento&#039;, &#039;depois-data-vencimento&#039; | Deve retornar se o clique ocorreu antes ou depois do vencimento. |

<br />

- **Quando:** No clique dos ícones de &quot;Expandir&quot;, &quot;Recolher&quot; e &quot;Abrir&quot; para visualização da fatura aberta, fechada ou em atraso ou em pagamento identificado.
- **Onde:** Na tela &quot;Limite&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes",
        	"eventAction": "clique:icone",
        	"eventLabel": "[[acao-icone]]-[[nome-dropdown]]" as })
        ])
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[acao-icone]] | &#039;expandir&#039;, &#039;recolher&#039; ou &#039;abrir&#039; | Deve retornar a ação da interação. |
| [[nome-dropdown]] | &#039;fatura-aberta&#039;, &#039;fatura-fechada&#039;, &#039;fatura-em-atraso&#039;, &#039;pagamento-identificado&#039; e etc | Deve retornar o nome do dropdown no qual ocorreu a interação. |

<br />

- **Quando:** No retorno de callback após interagir com a "fatura fechada" ou "copiar o código do boleto com sucesso".
- **Onde:** Na tela "Limite"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "clique:botao:app-cartao-rchlo:callback",
  eventLabel: "limite:[[status]]"
});
```


| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[status]]` |  'sem-fatura', 'codigo-copiado-sucesso' e etc|Deve retornar o status do callback.|


<br />


- **Quando:** Acontecer qualquer erro como, por exemplo, falha na conexão, boleto indisponivel e etc. 
- **Onde:** Na tela "Limite"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "callback:erro",
  eventLabel: "[[tipo-erro]]"
});
```


| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[tipo-erro]]` |  boleto-indisponivel', 'nao-foi-possivel-carregar-dados', 'nao-foi-possivel-carregar-limites' e etc|Deve retornar o tipo de erro apresentado.|


<br />

- **Quando:** No clique no botão  'Tentar novamente'
- **Onde:** Na tela "Limite"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "clique:[[tipo-fatura]]",
  eventLabel: "erro:tentar-novamente"
});
```


| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[tipo-fatura]]` |   'fatura-fechada-indisponivel', 'fatura-aberta-indisponivel', 'fatura-anterior-indisponivel' e etc| Deve retornar o tipo de fatura.|


<br />

- **Quando:** Visualização da tela "Empréstimos"

```javascript
Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/associe-riachuelo/limite/emprestimos");
```

<br />

- **Quando:** Nos cliques dos botões ou ícones na tela de emprestimos
- **Onde:** Na tela de emprestimos

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:emprestimos",
        	"eventAction": "clique:[[elemento]]",
        	"eventLabel": "[[item-clicado]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[elemento]]` |  'botao', 'link', 'icone' e etc | Deve retornar o nome do elemento. |
| `[[item-clicado]]` |  'baixar-o-app-midway', 'clique-aqui', 'voltar:emprestimos', 'simular-agora', 'contratar-no-app-midway' e etc | Deve retornar o nome do item clicado. |

<br />

- **Quando:** Visualização do modal "App Midway", para baixar o aplicativo App Midway

```javascript
Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/associe-riachuelo/limite/emprestimos/modal:baixe-o-app-da-midway/");
```

<br />

- **Quando:** Nos cliques dos botões ou ícones do modal para baixar o novo App da Midway
- **Onde:** No modal "App Midway", para baixar o aplicativo App Midway

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:emprestimos:modal:baixe-o-app-midway",
        	"eventAction": "clique:[[elemento]]",
        	"eventLabel": "[[item-clicado]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[elemento]]` |  'botao', 'link', 'icone' e etc | Deve retornar o nome do elemento. |
| `[[item-clicado]]` |  'play-store', 'app-store', 'fechar:modal-baixe-o-app' e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização das telas dos lightboxes "Sem fatura", "App cartões RCHLO"



```javascript
Analytics.setCurrentScreen(
  "/riachuelo-app/cartoes/midway/associe-riachuelo/limite/[[nome-lightbox]]/");
```

| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[nome-lightbox]]` |  'cartoes-rchlo', 'sem-fatura' e etc| Deve retornar o nome do lightbox.|


<br />

- **Quando:** No clique dos botões
- **Onde:** : Nas telas dos ligthboxes "App cartões RCHLO" e "Sem fatura"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "clique:botao:lightbox-[[nome-lightbox]]",
  eventLabel: "[[nome-botao]]"
});
```

| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
|`[[nome-lightbox]]`|'sem-fatura' e etc | Deve retornar o nome do lightbox. |
| `[[nome-botao]]` |   'ok', 'voltar', 'entendi' e etc| Deve retornar o nome do botão clicado. |


<br />


- **Onde:** Visualização da tela "Lançamentos da fatura"

```javascript
Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/associe-riachuelo/limite/lancamento-fatura/")
```
<br />


- **Quando:** No clique dos links ou botão
- **Onde:** : Na tela de Lançamento da fatura

```javascript
  Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "clique:[[botao-link]]:lancamentos-fatura",
  eventLabel: "[[nome-item]]"
});
```

| Variável         | Exemplo                                                 | Descrição                             |
| :--------------- | :------------------------------------------------------ | :------------------------------------ |
| `[[botao-link]]` | 'botao' ou 'link' |  Deve retornar o elemento clicado.  |
| `[[nome-item]]` |'titular', 'adicional', 'fatura-pdf' e etc|  Deve retornar o nome do item clicado.  |


<br />


- **Quando:** Ao selecionar uma forma de pagamento
- **Onde:**  No modal em tela de pagamento

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "selecionou:opcao-de-pagamento",
  eventLabel: "[[opcao-selecionada]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[opcao-selecionada]]` | 'parcelamento-fatura', 'pagamento-total', 'fechar' e etc| Deve retornar a opção selecionada. |
 

<br />



- **Onde:** Visualização da tela de "pagamento de fatura total" ou "pagamento de fatura parcelado"

```javascript
Analytics.setCurrentScreen("/riachuelo-app/cartoes/[[opcao-selecionada]]/");
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[opcao-selecionada]]` | 'parcelamento-fatura', 'pagamento-total' e etc| Deve retornar a opção selecionada. |

<br />


- **Quando:** Após interagir com o campo de &quot;Valor de entrada&quot;
- **Onde:** Na tela de &quot;parcelamento-de-fatura&quot;
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes",
        	eventAction: "interacao:campo:parcelamento-fatura",
        	eventLabel: "entrada:[[entrada-preenchida]]:total:[[valor-total]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[entrada-preenchida]] | &#039;500,00&#039;, &#039;200,00&#039; e etc | Deve retornar o valor de entrada. |
| [[valor-total]] | &#039;1107,84&#039; e etc | Deve retornar o valor total da fatura. |

<br />


- **Quando:** Ao clicar em algum elemento da pagina.
- **Onde:**  Na tela de "pagamento de fatura" ou "parcelamento-de-fatura"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:cartoes",
  eventAction: "[[opcao-selecionada]]:clique",
  eventLabel: "[[nome-item]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[opcao-selecionada]]` | 'parcelamento-fatura', 'pagamento-total' e etc| Deve retornar a opção selecionada. |
| `[[nome-item]]` |  'copiar-boleto', 'ver-boleto', 'copiar-codigo', 'voltar', 'ver-todas-as-opcoes', 'fechar' e etc| Deve retornar o nome do item. |
 

<br />



- **Quando:** Ao clicar nos botões do modal
- **Onde:** Na tela de &quot;parcelamento-de-fatura&quot;, no modal de &quot;Parcelamento&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes" ,
        	eventAction: "parcelamento-fatura:clique",
        	eventLabel: "modal-parcelamento:[[nome-botao]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;, &#039;entendi&#039; e etc | Deve retornar o nome do botão. |

<br />

- **Quando:** Acontecer qualquer erro como, por exemplo, falha na conexão, boleto indisponivel e etc.
- **Onde:** Na tela &quot;Parcelamento de Fatura&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "riachuelo:app:cartoes",
        	eventAction: "parcelamento-fatura:callback:erro" ,
        	eventLabel: "[[tipo-erro]]" 
          })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-erro]] | &#039;boleto-indisponivel&#039;, &#039;nao-foi-possivel-carregar-dados&#039;, &#039;nao-foi-possivel-carregar-limites&#039; e etc. | Deve retornar o tipo de erro apresentado. |

<br />

- **Quando:** No clique no botão  &#039;Tentar novamente&#039;
- **Onde:** Na tela &quot;Parcelamento de Fatura&quot;


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:cartoes" ,
        	eventAction: "parcelamento-fatura:clique:botao" ,
        	eventLabel: "erro:tentar-novamente" 
        })
```


<br />


- **Quando:** No sucesso ao copiar o codigo do boleto
- **Onde:**  Na tela de "pagamento de fatura"  ou "parcelamento-de-fatura"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:cartoes",
  eventAction: "callback:[[opcao-selecionada]]",
  eventLabel: "copiar-[[codigo-ou-boleto]]-boleto:sucesso"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| [[codigo-ou-boleto]] | &#039;codigo&#039; ou &#039;boleto&#039; | Deve retornar o tipo copiado. |
| [[opcao-selecionada]] | &#039;parcelamento-fatura&#039;, &#039;pagamento-total&#039; e etc | Deve retornar a opção selecionada. |

<br />


- **Quando:** Ao selecionar a quantidade de parcelas.
- **Onde:** Na tela  "parcelamento-de-fatura"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:cartoes",
  eventAction: "selecionou:parcelas",
  eventLabel: "[[quantidade-de-parcelas]]:[[valor-parcela]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[quantidade-de-parcelas]]` | '2-parcelas', '3-parcelas', '5-parcelas' e etc.| Deve retornar a quantidade de parcelas selecionada. |
| `[[valor-parcela]]` |  '114.56', '256.43' etc| Deve retornar o valor da parcela. |
 

<br />

**Visualização do lightbox de erro em tela de &quot;pagamento de fatura total&quot; ou &quot;pagamento de fatura parcelado&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/[[opcao-selecionada]]/erro")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] | &#039;parcelamento-fatura&#039;, &#039;pagamento-total&#039; e etc | Deve retornar a opção selecionada. |

<br />

- **Quando:** Na interação com os itens no lightbox de erro
- **Onde:** Na tela de &quot;pagamento de fatura&quot; ou &quot;parcelamento-de-fatura&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes",
        	"eventAction": "clique:lightbox-erro:[[opcao-selecionada]]",
        	"eventLabel": "[[nome-item]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] | &#039;parcelamento-fatura&#039;, &#039;pagamento-total&#039; e etc | Deve retornar a opção selecionada. |
| [[nome-item]] | &#039;fechar&#039;, &#039;tentar-novamente&#039; | Deve retornar o nome do item. |

<br />


- **Onde:** Visualização da tela de "Desbloquear cartão"

```javascript
Analytics.setCurrentScreen("/riachuelo-app/desbloquear-cartao/", "[[currentClass]]");
```

<br />

- **Quando:** No clique do botão de voltar
- **Onde:**  Na tela de "Desbloquear cartão"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes",
  eventAction: "clique:botao:desbloquear-cartao",
  eventLabel: "voltar"
});
```

<br />


- **Quando:** Na interação com os cartões, na seção de "Titular" ou "Adicional"
- **Onde:**  Na tela de "Desbloquear cartão"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:cartoes",
  eventAction: "interacao:cartoes-desbloquear-cartao",
  eventLabel: "[[secao]]:[[posicao-cartao]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[secao]]` | 'titular', 'adicional' e etc| Deve retornar a seção interagida. |
| `[[posicao-cartao]]` | 'cartao-1', 'cartao-2' e etc | Deve retornar a posição do cartão. |
 

<br />


- **Quando:** No clique do botão de "Prosseguir"
- **Onde:**  Na tela de "Desbloquear cartão"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo-app:cartoes",
  eventAction: "clique:botao:desbloquear-cartao",
  eventLabel: "prosseguir:[[tipo-cartao]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[tipo-cartao]]` | 'titular', 'adicional' e etc| Deve retornar o tipo do cartão. |
 

<br />

- **Onde:** Visualização da tela de "Cartão desbloqueado com sucesso!"

```javascript
Analytics.setCurrentScreen("/riachuelo-app/desbloquear-cartao/sucesso/");
```

<br />


- **Quando:** No clique dos botões
- **Onde:**  Na tela de "Cartão desbloqueado com sucesso!"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:desbloqueio-sucesso",
  eventAction: "clique:botao",
  eventLabel: "[[nome-botao]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[nome-botao]]` | 'ir-loja-online', 'fechar' e etc| Deve retornar o nome do botão clicado.  |
 

<br />

- **Onde:** Visualização da tela de "Callback de erro" com o tipo do cartão

```javascript
Analytics.setCurrentScreen("/riachuelo-app/desbloquear-cartao/erro:[[tipo-cartao]]/");
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[tipo-cartao]]` | 'adicional', 'titular' | Deve retornar o tipo do cartão  |


<br />

- **Quando:** No clique do botão
- **Onde:**  Na tela de "Callback de erro" com o tipo do cartão

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:cartoes:desbloqueio-erro",
  eventAction: "clique:botao",
  eventLabel: "voltar:[[tipo-cartao]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[tipo-cartao]]` | 'adicional', 'titular' | Deve retornar o tipo do cartão  |

<br />

- **Onde:** Visualização da tela de aparição de "Seu Upgrade de cartão está disponível"

```javascript
Analytics.setCurrentScreen("/riachuelo-app/modal-sem-problemas-super-app/");
```

<br />

- **Quando:** No clique dos botões em "Seu Upgrade de cartão está disponível"
- **Onde:**  Na home de Catões. 

```javascript
Analytics.logEvent("event", {
  eventCategory: "app-midway:super-app:modal-oferta-especial-cartao",
  eventAction: "clique:botao",
  eventLabel: "[[nome-botao]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[nome-botao]]` | 'ver-mais', 'fechar' e etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique dos botões em "Sem problemas"
- **Onde:** No clique em recusar "Seu Upgrade de cartão está disponível"

```javascript
Analytics.logEvent("event", {
  eventCategory: "app-midway:super-app:modal-sem-problemas",
  eventAction: "clique:botao",
  eventLabel: "[[nome-botao]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[nome-botao]]` | 'ja-tenho-um-cartao', 'valor-da-anuidade', 'valor-do-limite-pre-aprovado', 'nao-vi-beneficios' e etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Onde: Visualização da tela "Agradecimento"

```javascript
Analytics.setCurrentScreen("/cartoes/midway/modal-sem-problemas-agradecimento-super-app/");
```

<br />

- **Quando:** No clique do botão em "Agradecimento"
- **Onde:** Na tela de "Agradecimento".

```javascript
Analytics.logEvent("event", {
  eventCategory: "app-midway:super-app:modal-sem-problemas-agradecimento",
  eventAction: "clique:botao",
  eventLabel: "fechar"
});
```

<br />

- **Onde:** Onde: Visualização da tela de "Upgrade de cartão disponível"

```javascript
Analytics.setCurrentScreen("/cartoes/home/upgrade-cartao-super-app/");
```

<br />

- **Quando:** No clique dos botões em "Upgrade de cartão disponível"
- **Onde:** Na tela de "Upgrade de cartão disponível"

```javascript
Analytics.logEvent("event", {
  eventCategory: "app-midway:super-app:upgrade-cartao-riachuelo",
  eventAction: "clique:botao",
  eventLabel: "[[nome-botao]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[nome-botao]]` | 'voltar', 'fazer-upgrade' e etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique dos ícones
- **Onde:** Em "Upgrade de cartão disponível" 

```javascript
Analytics.logEvent("event", {
  eventCategory: "app-midway:super-app:upgrade-cartao-riachuelo",
  eventAction: "clique:icone",
  eventLabel: "[[nome-icone]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[nome-icone]]` | 'expandir-saiba-mais', 'recolher-saiba-mais', 'aceito-os-termos-e-condicoes-do-cartao-riachuelo', nao-aceito-os-termos-e-condicoes-do-cartao-riachuelo' e etc. | Deve retornar o nome do ícone clicado. |

<br />

- **Quando:** No clique do link
- **Onde:** Em "Upgrade de cartão disponível"

```javascript
Analytics.logEvent("event", {
  eventCategory: "app-midway:super-app:upgrade-cartao-riachuelo",
  eventAction: "clique:link",
  eventLabel: "[[nome-link]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[nome-link]]` | 'termos-e-condicoes' e etc | Deve retornar o nome do link clicado. |

<br />

- **Onde:** Visualização da tela de "Termos e Condições Cartão Riachuelo"

```javascript
Analytics.setCurrentScreen("/cartoes/contrato-cartao-bandeira-super-app/");
```

<br />

- **Quando:** No clique do botão em "Termos e Condições Cartão Riachuelo"
- **Onde:** Na tela de "Termos e Condições Cartão Riachuelo"

```javascript
Analytics.logEvent("event", {
  eventCategory: "app-midway:super-app:contrato-cartao-bandeira",
  eventAction: "clique:botao",
  eventLabel: "voltar"
});
```

<br />

- **Onde:** Visualização da tela de "Upgrade realizado com sucesso"

```javascript
Analytics.setCurrentScreen("/cartoes/sucesso-upgrade-super-app/");
```

<br />

- **Quando:** No clique do botão
- **Onde:** Na tela de "Upgrade realizado com sucesso"

```javascript
Analytics.logEvent("event", {
  eventCategory: "app-midway:super-app:sucesso-upgrade",
  eventAction: "clique:botao",
  eventLabel: "[[nome-botao]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[nome-botao]]` | 'ok', 'fechar' e etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No callback de erro em "Fazer Upgrade"
- **Onde:** Na tela de "Upgrade de cartão"

```javascript
Analytics.logEvent("event", {
  eventCategory: "app-midway:super-app:upgrade-cartao:modal-atencao",
  eventAction: "callback:[[erro]]",
  eventLabel: "[[nome-botao]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[erro]]` | 'nao-foi-possivel-realizar-upgrade' | Deve retornar o nome do sucesso ou erro. |
| `[[nome-botao]]` | 'tentar-novamente', 'voltar', 'fechar' e etc. | Deve retornar o nome do botão clicado. |

<br />

### Cartões - Novo Fluxo 2 Cartões SICC e TSYS 

- **Onde:** **Visualização da tela de "Limite dos Cartões" (Cartão Riachuelo)**<br />

**Obs:** Para os casos que o usuário tem 2 cartões disponiveis, retornar na seguinte estrutura: '1-mastercard:cartao-cancelado', '1-visa:cartao-ativo' e etc<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/associe-riachuelo/limite/[[qtd+bandeira-cartao]]:[[status-cartao]]/")
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| [[qtd+bandeira-cartao]] | '1-mastercard', '1-rchlo' e etc | Retorna a quantidade de cartões + a bandeira do cartão apresentada na tela de limite. |
| [[status-cartao]] | 'cartao-bloqueado', 'cartao-cancelado', 'cartao-ativo' e etc | Retorna o status do cartão para cada cartão apresentado na tela de limite. |

<br />

- **Quando:** No clique nas opções de "Ações Rápidas"
- **Onde:** Na tela de "Limite dos Cartões"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:limite-disponivel",
        	"eventAction": "clique:botao:acao-rapida",
        	"eventLabel": "[[nome-botao]]"
        })
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| [[nome-botao]] | 'desbloquear-cartao', 'minhas-faturas', 'emprestimo-pessoal', 'seguros-e-assistencias', 'upgrade-do-cartao' e etc | Retorna o nome do botão clicado. |

<br />

- **Quando:** No clique do icone para mostrar ou esconder o valor do limite
- **Onde:** Na tela de "Limite dos Cartões"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:limite-disponivel",
        	"eventAction": "clique:icone",
        	"eventLabel": "[[mostrar-ou-esconder]]:limite",
                "status_fatura": "[[status_fatura]]",
                "status_cartao": "[[status_cartao]]",
                "tipo_cartao": "[[tipo_cartao]]"
        })
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| [[mostrar-ou-esconder]] | 'mostrar' ou 'esconder' | Retorna a ação do usuário. |
| [[status_fatura]] | 'fatura-aberta', 'fatura-fechada', 'fatura-em-atraso', 'pagamento-nao-identificado', 'pagamento-identificado' e etc. | Deve retornar o status da fatura. |
| [[status_cartao]] | 'cartao-cancelado', 'cartao-bloqueado', 'cartao-ativo' e etc | Retorna o Status do cartão |
| [[tipo_cartao]] | 'cartao-midway-master', 'cartao-midway-visa', 'cartao-midway-pl' e etc | Deve retornar o tipo do cartão |

<br />

- **Quando:** No clique do botão "Carregar Novamente" (Ultimos Lançamentos)
- **Onde:** Na tela de "Limite dos Cartões", quando não foi possível carregar as informações dos seus últimos lançamentos

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:limite-disponivel",
        	"eventAction": "clique:botao:nao-carregou-ultimos-lancamentos",
        	"eventLabel": "ultimos-lancamentos:carregar-novamente",
                "status_fatura": "[[status_fatura]]",
                "status_cartao": "[[status_cartao]]",
                "tipo_cartao": "[[tipo_cartao]]"
        })
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| [[status_fatura]] | 'fatura-aberta', 'fatura-fechada', 'fatura-em-atraso', 'pagamento-nao-identificado', 'pagamento-identificado' e etc. | Deve retornar o status da fatura. |
| [[status_cartao]] | 'cartao-cancelado', 'cartao-bloqueado', 'cartao-ativo' e etc | Retorna o Status do cartão |
| [[tipo_cartao]] | 'cartao-midway-master', 'cartao-midway-visa', 'cartao-midway-pl' e etc | Deve retornar o tipo do cartão |

<br />

- **Quando:** No clique do botão "Carregar Novamente" (Informações do Cartão)
- **Onde:** Na tela de "Limite dos Cartões", quando não foi possível carregar as informações do seu Cartão

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:limite-disponivel",
        	"eventAction": "clique:botao:nao-carregou-dados-cartao",
        	"eventLabel": "dados-cartao:carregar-novamente",
                "status_fatura": "[[status_fatura]]",
                "status_cartao": "[[status_cartao]]",
                "tipo_cartao": "[[tipo_cartao]]"
        })
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| [[status_fatura]] | 'fatura-aberta', 'fatura-fechada', 'fatura-em-atraso', 'pagamento-nao-identificado', 'pagamento-identificado' e etc. | Deve retornar o status da fatura. |
| [[status_cartao]] | 'cartao-cancelado', 'cartao-bloqueado', 'cartao-ativo' e etc | Retorna o Status do cartão |
| [[tipo_cartao]] | 'cartao-midway-master', 'cartao-midway-visa', 'cartao-midway-pl' e etc | Deve retornar o tipo do cartão |

<br />

- **Onde:** **Visualização da tela de "Minhas Faturas"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/associe-riachuelo/limite/minhas-faturas/")
```

<br />

- **Quando:** Na escolha de "Faturas que deseja consultar"
- **Onde:** Na tela de "Minhas Faturas"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:minhas-faturas",
        	"eventAction": "clique:consultar-fatura",
        	"eventLabel": "[[opcao-selecionada]]"
        })
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| [[opcao-selecionada]] |'faturas-fechadas', 'faturas-abertas', 'fatura-em-atraso' e etc | Retorna a opção selecionada do usuário. |

<br />

- **Quando:** No clique dos botões da tela
- **Onde:** Na tela de "Minhas Faturas"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:minhas-faturas",
        	"eventAction": "clique:botao",
        	"eventLabel": "[[nome-botao]]",
                "status_fatura": "[[status_fatura]]",
                "status_cartao": "[[status_cartao]]",
                "tipo_cartao": "[[tipo_cartao]]",
                "data_vencimento": "[[data_vencimento]]"
        })
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| [[nome-botao]] |'voltar:minhas-faturas', 'ver-extrato', 'opcoes-de-pagamento', 'copiar-codigo', 'ver-boleto' e et | Retorna o nome do botão clicado. |
| [[status_fatura]] | 'fatura-aberta', 'fatura-fechada', 'fatura-em-atraso', 'pagamento-nao-identificado', 'pagamento-identificado' e etc. | Deve retornar o status da fatura. |
| [[status_cartao]] | 'cartao-cancelado', 'cartao-bloqueado', 'cartao-ativo' e etc | Retorna o Status do cartão |
| [[tipo_cartao]] | 'cartao-midway-master', 'cartao-midway-visa', 'cartao-midway-pl' e etc | Deve retornar o tipo do cartão |
| [[data_vencimento]] | '15/11' e etc | Retorna a data de vencimento da fatura |

<br />

- **Quando:** No callback das faturas que informam alguma indisponibilidade para o usuário
- **Onde:** Na tela de "Minhas Faturas"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:minhas-faturas",
        	"eventAction": "callback:[[tipo-de-fatura]]",
        	"eventLabel": "[[callback-retornado]]",
                "status_fatura": "[[status_fatura]]",
                "status_cartao": "[[status_cartao]]",
                "tipo_cartao": "[[tipo_cartao]]",
                "data_vencimento": "[[data_vencimento]]"
        })
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| [[tipo-de-fatura]] |'fatura-anterior', 'fatura-aberta' e etc | Retorna o tipo de fatura consultado. |
| [[callback-retornado]] |'voce-nao-possui-nenhuma-fatura-anterior-em-seu-cartao', 'funcionalidade-nao-disponivel-para-este-cartao', 'em-breve-numero-do-boleto-estara-disponivel', 'voce-nao-possui-nenhuma-fatura-fechada-em-seus-cartoes' e etc | Retorna o callback que foi apresentado ao usuário. |
| [[status_fatura]] | 'fatura-aberta', 'fatura-fechada', 'fatura-em-atraso', 'pagamento-nao-identificado', 'pagamento-identificado' e etc. | Deve retornar o status da fatura. |
| [[status_cartao]] | 'cartao-cancelado', 'cartao-bloqueado', 'cartao-ativo' e etc | Retorna o Status do cartão |
| [[tipo_cartao]] | 'cartao-midway-master', 'cartao-midway-visa', 'cartao-midway-pl' e etc | Deve retornar o tipo do cartão |
| [[data_vencimento]] | '15/11' e etc | Retorna a data de vencimento da fatura |

<br />

- **Onde:** **Visualização do modal de "Desbloqueio do Cartão"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/associe-riachuelo/limite/modal:desbloqueio-do-cartao/")
```

<br />

- **Quando:** No clique dos botões
- **Onde:** No modal de "Desbloqueio do cartão"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:modal:desbloqueio-do-cartao",
        	"eventAction": "clique:botao",
        	"eventLabel": "[[nome-botao]]",
                "tipo_cartao": "[[tipo_cartao]]",
        })
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| [[nome-botao]] |'fechar:modal-desbloqueio-do-cartao', 'clicou-fora:modal-desbloqueio-do-cartao', 'desbloquear-o-cartao-agora' e etc | Retorna o nome do botão clicado. |
| [[tipo_cartao]] | 'cartao-midway-master', 'cartao-midway-visa', 'cartao-midway-pl' e etc | Deve retornar o tipo do cartão |

<br />

- **Onde:** **Visualização do modal de "Cartão Cancelado"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/associe-riachuelo/limite/modal:cartao-cancelado/")
```

<br />

- **Quando:** No clique dos botões
- **Onde:** No modal de "Cartão Cancelado"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:modal:cartao-cancelado",
        	"eventAction": "clique:botao",
        	"eventLabel": "[[nome-botao]]",
                "tipo_cartao": "[[tipo_cartao]]",
        })
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| [[nome-botao]] |'fechar:modal-cartao-cancelado', 'clicou-fora:modal-cartao-cancelado', 'entendi' e etc | Retorna o nome do botão clicado. |
| [[tipo_cartao]] | 'cartao-midway-master', 'cartao-midway-visa', 'cartao-midway-pl' e etc | Deve retornar o tipo do cartão |

<br />

- **Onde:** **Visualização do modal de "Pagamento em Atraso"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/associe-riachuelo/limite/modal:pagamento-em-atraso/")
```

<br />

- **Quando:** No clique dos botões
- **Onde:** No modal de "Pagamento em Atraso"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:modal:pagamento-em-atraso",
        	"eventAction": "clique:botao",
        	"eventLabel": "[[nome-botao]]",
                "tipo_cartao": "[[tipo_cartao]]",
        })
```

| Variável        | Exemplo          | Descrição          |
| :-------------- | :--------------- | :----------------- |
| [[nome-botao]] |'fechar:modal-pagamento-em-atraso', 'clicou-fora:modal-pagamento-em-atraso', 'entendi' e etc | Retorna o nome do botão clicado. |
| [[tipo_cartao]] | 'cartao-midway-master', 'cartao-midway-visa', 'cartao-midway-pl' e etc | Deve retornar o tipo do cartão |

<br />

### Cartões - Antecipar Pagamento

- **Quando:** No clique do botão "Extrato"
- **Onde:** Na tela "Limite disponível", seção "Fatura aberta"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes",
        	"eventAction": "clique:botao",
        	"eventLabel": "extrato",
                "status_fatura": "[[status_fatura]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status_fatura]] | 'fatura-aberta', 'fatura-fechada', 'fatura-em-atraso' e etc. | Deve retornar o status da fatura. |

<br />

**Visualização da tela de "Extrato da fatura"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/limite/extrato-fatura:[[mes]]/")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes]] | 'setembro', 'novembro' e etc | Deve retornar o mês que o usuário está visualizando o extrato. |

<br />

- **Quando:** No clique nos botões "Voltar" e "Antecipar Fatura"
- **Onde:** Na tela "Extrato da Fatura"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:extrato-fatura:[[mes]]",
        	"eventAction": "clique:botao",
        	"eventLabel": "[[nome-botao]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes]] | 'setembro', 'novembro' e etc | Deve retornar o mês que o usuário está visualizando o extrato. |
| [[nome-botao]] | 'voltar' ou 'antecipar-fatura' | Deve retornar o nome do botão clicado. |

<br />

**Visualização da tela de "Antecipação de Fatura"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/limite/extrato-fatura:[[mes]]/antecipar-fatura/")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes]] | 'setembro', 'novembro' e etc | Deve retornar o mês que o usuário está visualizando o extrato. |

<br />

- **Quando:** No clique nos botões "Voltar" e "Gerar Boleto"
- **Onde:** Na tela "Antecipação de Fatura"

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:antecipar-fatura:[[mes]]",
        	"eventAction": "clique:botao",
        	"eventLabel": "[[nome-botao]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes]] | 'setembro', 'novembro' e etc | Deve retornar o mês que o usuário está visualizando o extrato. |
| [[nome-botao]] | 'voltar' ou 'gerar-boleto' | Deve retornar o nome do botão clicado. |

<br />

**Visualização da tela de "Antecipação de Fatura", com o código de barras disponível para pagamento.**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/limite/extrato-fatura:[[mes]]/antecipar-fatura/codigo-de-barras/")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes]] | 'setembro', 'novembro' e etc | Deve retornar o mês que o usuário está visualizando o extrato. |

<br />

- **Quando:** No clique nos botões "Voltar", "Ver Boleto" ou "Copiar Código"
- **Onde:** Na tela "Antecipação de Fatura", com o código de barras disponível para pagamento

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:antecipar-fatura:[[mes]]:codigo-de-barras",
        	"eventAction": "clique:botao",
        	"eventLabel": "[[nome-botao]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes]] | 'setembro', 'novembro' e etc | Deve retornar o mês que o usuário está visualizando o extrato. |
| [[nome-botao]] | 'voltar', 'ver-boleto' ou 'copiar-codigo' | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No callback ao copiar o código de barras.
- **Onde:** Na tela "Antecipação de Fatura", com o código de barras disponível para pagamento

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:antecipar-fatura:[[mes]]:codigo-de-barras",
        	"eventAction": "callback:codigo-de-barras",
        	"eventLabel": "[[sucesso-ou-erro]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes]] | 'setembro', 'novembro' e etc | Deve retornar o mês que o usuário está visualizando o extrato. |
| [[sucesso-ou-erro]] | 'sucesso', 'erro:codigo-de-barras-vencido' e etc | Deve retornar o callback após o usuário copiar o código de barras. |

<br />

**Visualização da tela de "Boleto"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/limite/extrato-fatura:[[mes]]/antecipar-fatura/codigo-de-barras/boleto/")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes]] | 'setembro', 'novembro' e etc | Deve retornar o mês que o usuário está visualizando o extrato. |

<br />

- **Quando:** No clique nos botões "Voltar" ou "Compartilhar"
- **Onde:** Na tela de "Boleto".

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:antecipar-fatura:[[mes]]:boleto",
        	"eventAction": "clique:botao",
        	"eventLabel": "[[nome-botao]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes]] | 'setembro', 'novembro' e etc | Deve retornar o mês que o usuário está visualizando o extrato. |
| [[nome-botao]] | 'voltar' ou 'compatilhar' | Deve retornar o nome do botão clicado. |

<br />

**Visualização da tela de "Ops, tivemos um problema!"**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/limite/extrato-fatura/antecipar-fatura/erro:tivemos-um-problema")
```

<br />

- **Quando:** No clique nos botões "Voltar" ou "Tentar Novamente"
- **Onde:** Na tela de "Ops, tivemos um problema!".

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:antecipar-fatura:erro:tivemos-um-problema",
        	"eventAction": "clique:botao",
        	"eventLabel": "[[nome-botao]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes]] | 'setembro', 'novembro' e etc | Deve retornar o mês que o usuário está visualizando o extrato. |
| [[nome-botao]] | 'voltar' ou 'tentar-novamente' | Deve retornar o nome do botão clicado. |

<br />

### Eventos - Cartões - Super APP Guideline

**Visualização das telas dentro de Super app Guideline**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/[[nome-tela]]-super-app/")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-tela]] | &#039;midway&#039;, &#039;onboarding&#039;, &#039;cartoes&#039;, &#039;limite&#039;, &#039;limite/mais-credito&#039; e etc | Deve retornar a tela apresentada. |

<br />

- **Quando:** No clique dos botões ou links
- **Onde:** Nas telas que estiverem disponíveis

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:super-app:[[nome-do-fluxo]]",
        	"eventAction": "clique:[[botao-link]]",
        	"eventLabel": "[[nome-item]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o elemento clicado. |
| [[nome-item]] | &#039;voltar&#039;, &#039;fechar&#039;, &#039;tentar-novamente&#039; e etc | Deve retornar o nome do item clicado. |
| [[nome-do-fluxo]] | &#039;cartoes&#039; , &#039;desbloqueio&#039; e etc | Deve retornar o nome do fluxo relacionado a tela que usuário se encontra. |

<br />

- **Quando:** No clique dos ícones de &quot;Expandir&quot;, &quot;Recolher&quot; e &quot;Abrir&quot; para visualização da fatura aberta, fechada ou em atraso ou em pagamento identificado.
- **Onde:** Nas telas que estiverem disponíveis

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:super-app:[[nome-do-fluxo]]",
        	"eventAction": "clique:icone",
        	"eventLabel": "[[acao-icone]]-[[nome-dropdown]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[acao-icone]] | &#039;expandir&#039;, &#039;recolher&#039; ou &#039;abrir&#039; | Deve retornar a ação da interação. |
| [[nome-dropdown]] | &#039;fatura-aberta&#039;, &#039;fatura-fechada&#039;, &#039;fatura-em-atraso&#039;, &#039;pagamento-identificado&#039; e etc | Deve retornar o nome do dropdown no qual ocorreu a interação. |
| [[nome-do-fluxo]] | &#039;cartoes&#039; , &#039;desbloqueio&#039; e etc | Deve retornar o nome do fluxo relacionado a tela que usuário se encontra. |

<br />

- **Quando:** Após o preenchimento dos campos (disparar a ação caso o texto seja modificado ou caso haja uma nova entrada de texto.)
- **Onde:** Nas telas que estiverem disponíveis

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:super-app:[[nome-do-fluxo]]",
        	"eventAction": "interacao",
        	"eventLabel": "preencheu:[[nome-campo]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome&#039;, &#039;cpf&#039; e etc | Deve retornar o nome do campo preenchido. |
| [[nome-do-fluxo]] | &#039;cartoes&#039; , &#039;desbloqueio&#039; e etc | Deve retornar o nome do fluxo relacionado a tela que usuário se encontra. |

<br />

- **Quando:** Após callback de sucesso ou erro
- **Onde:** Nas telas que estiverem disponíveis

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:super-app:[[nome-do-fluxo]]",
        	"eventAction": "callback",
        	"eventLabel": "[[status]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039;, &#039;erro:nao-foi-possivel-concluir&#039; e etc | Deve retornar o status do callback. |
| [[nome-do-fluxo]] | &#039;cartoes&#039; , &#039;desbloqueio&#039; e etc | Deve retornar o nome do fluxo relacionado a tela que usuário se encontra. |

---

### Eventos - Super App - Declarações Anuais

- **Onde:** Visualização da tela de &quot;Declarações Anuais&quot;

```javascript
Analytics.setCurrentScreen("/riachuelo-app/declaracoes-anuais/");
```

<br />

 - **Quando:** No clique do ícone de voltar
- **Onde:** Na tela de Declarações Anuais

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:declaracoes-anuais",
  eventAction: "clique:icone",
  eventLabel: "voltar"
});
```

<br />

 - **Quando:** No clique do botão de Abrir
- **Onde:** Na tela de Declarações Anuais

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:declaracoes-anuais",
  eventAction: "clique:botao",
  eventLabel: "abrir:[[opcao-selecionada]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[opcao-selecionada]]` | 'declaracao-quitacao', 'taxas-anuais' e etc | Deve retornar a opção selecionada  |

<br />

 - **Onde:** Visualização das telas de "Declaração de Quitação" ou "Taxas Anuais"

 ```javascript
Analytics.setCurrentScreen("/riachuelo-app/declaracoes-anuais/[[tela]]");
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[tela]]` | 'declaracao-quitacao', 'taxas-anuais' e etc | Deve retornar o nome da tela apresentada  |

<br />

 - **Quando:** No clique do ícone de voltar
- **Onde:** Nas telas de &quot;Declaração Anual&quot; ou &quot;Extrato consolidado Anual&quot;

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:declaracoes-anuais",
  eventAction: "clique:icone:[[opcao-selecionada]]",
  eventLabel: "voltar"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[opcao-selecionada]]` | 'declaracao-quitacao', 'taxas-anuais' e etc | Deve retornar a opção selecionada  |

<br />

- **Quando:** No clique das abas
- **Onde:** Na tela de &quot;Extrato consolidado Anual&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:declaracao-quitacao",
        	"eventAction": "clique:aba:extrato-consolidado-anual",
        	"eventLabel": "[[semestre]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[semestre]] | &#039;1-semestre&#039;, &#039;2-semestre&#039; e etc | Deve retornar em qual semestre o usuário clicou. |

 - **Quando:** No clique do botão de &quot;Baixar Declaração&quot;
- **Onde:** Nas telas de "Declaração de Quitação" ou "Taxas Anuais"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:declaracoes-anuais",
  eventAction: "clique:botao",
  eventLabel: "baixar-declaracao:[[nome-tela]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[nome-tela]]` | 'declaracao-quitacao', 'taxas-anuais' e etc | Deve retornar o nome da tela apresentada  |

<br />

 - **Quando:** Acontecer qualquer erro como, por exemplo, falha na conexão, boleto indisponivel e etc. 
- **Onde:** Nas telas de "Declaração de Quitação" ou "Taxas Anuais"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:declaracoes-anuais",
  eventAction: "callback:erro:[[tela]]",
  eventLabel: "[[tipo-erro]]"
});
```

| Variável        | Exemplo               | Descrição                            |
| :-------------- | :-------------------- | :----------------------------------- |
| `[[tela]]` | 'declaracao-quitacao', 'taxas-anuais' e etc | Deve retornar o nome da tela apresentada  |
| `[[tipo-erro]]` | 'nao-foi-possivel-carregar-dados', 'nao-foi-possivel-carregar-informacoes-quitacao', 'nao-foi-possivel-carregar-informacoes-taxas-anuais' e etc | Deve retornar o tipo de erro apresentado  |

<br />

 - **Quando:** No clique no link &#039;Tentar novamente&#039;
- **Onde:** Nas telas de "Declaração de Quitação" ou "Taxas Anuais"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:declaracoes-anuais",
  eventAction: "clique:link:[[tela]]",
  eventLabel: "tentar-novamente"
});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[tela]]` | 'declaracao-quitacao', 'taxas-anuais' e etc | Deve retornar o nome da tela apresentada  |

<br />

- **Onde:**  Visualização do modal de "Você não tem uma declaração de quitação disponível"

```javascript
Analytics.setCurrentScreen("/riachuelo-app/declaracao-quitacao/modal-declaracao-nao-disponivel/");
```

<br />

 - **Quando:** No clique nos botões &#039;Entendi&#039; e &#039;Fechar&#039;
- **Onde:** No modal de "Você não tem uma declaração de quitação disponível"

```javascript
Analytics.logEvent("event", {
  eventCategory: "riachuelo:app:declaracoes-anuais",
  eventAction: "clique:botao:modal-declarao-nao-disponivel",
  eventLabel: "[[nome-botao]]"
});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-botao]]` | 'fechar' ou 'entendi' | Deve retornar o nome do botão  

<br />

---

### Eventos - Super App - Assistências e Seguros

**Visualização da tela de assistencia e seguros**


```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/")
```


<br />

- **Quando:** No clique nos botões &#039;Simule e contrate&#039;
- **Onde:** Na tela de &quot;Seguros e assitências&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory : "riachuelo:app:seguros-e-assistencias" ,
        	eventAction: "clique:botao" ,
        	eventLabel : "simule-e-contrate"
        	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]"
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 

<br />

- **Quando:** No clique nos botões
- **Onde:** Nas telas &quot;Seguros e assitências&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "riachuelo:app:seguros-e-assistencias",
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]"
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;cancelamento&#039;, &#039;meus-numeros-da-sorte&#039;, &#039;produtos-contratados&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />

**Visualização da tela de produtos de  assistencia e seguros**

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/[[nome-tela-modal]]/")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-tela-modal]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; , &#039;modal/moto-premiavel&#039;, &#039;modal/mais-saude&#039; e etc&quot; | Deve retornar a tela de qual opção o usuario escolheu. |

<br />

- **Quando:** No clique nos botões
- **Onde:** Nas telas de detalhes do produtos de &quot;Seguros e assitências&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:seguros-e-assistencias:detalhe-produto" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]" ,
        	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;entendi-vamos-la&#039;, &#039;fechar&#039;, &#039;continuar&#039; e etc. | Deve retornar o nome do botão clicado. |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 

<br />

**Visualização da tela home de produtos de  assistencia e seguros**

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/[[nome-tela]]/home/")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-tela]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039;e etc&quot; | Deve retornar a tela de qual opção o usuario escolheu. |

<br />

- **Quando:** No clique nos botões
- **Onde:** Nas telas de home do produtos de &quot;Seguros e assitências&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "riachuelo:app:seguros-e-assistencias:home-produto" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]" ,
        	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]" 
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;simular-e-contratar&#039;, &#039;saiba-mais&#039;, &#039;area-do-cliente&#039; e etc | Deve retornar o nome do botão clicado. |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 

<br />

**Visualização do modal que aparece após clicar em &#039;área do cliente&#039; e em &#039;saiba mais &#039; na home de produtos de &#039; assistência e seguros**


```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/[[nome-tela]]/home/[[nome-modal]]")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-tela]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039;  e etc&quot; | Deve retornar a tela de qual opção o usuario escolheu. |
| [[nome-modal]] | &#039;saiba-mais&#039;, &#039;parceiro-vale-saude&#039; e etc | Deve retornar o nome do modal . |

<br />

- **Quando:** No clique nos botões
- **Onde:** No modal que aparece após clicar em &#039;área do cliente&#039; e em &#039;saiba mais &#039; na home de produtos de &#039; assistência e seguros

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "riachuelo:app:seguros-e-assistencias:home-produto" ,
        	eventAction: "[[nome-modal]]:clique-botao" ,
        	eventLabel: "[[nome-botao]]" ,
        	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]" 
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] |  &#039;fechar&#039;, &#039;entendi&#039; , &#039;ok-continuar&#039; e etc | Deve retornar o nome do botão clicado. |
| [[nome-modal]] | &#039;saiba-mais&#039;, &#039;parceiro-vale-saude&#039; e etc | Deve retornar o nome do modal . |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 

<br />

**Visualização da tela de cadastro de dados para os produtos de  assistencia e seguros**

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/[[nome-produto]]/cadastro-de-dados/[[etapa-form]]")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039;  e etc&quot; | Deve retornar a tela de qual opção o usuario escolheu. |
| [[etapa-form]] | &#039;etapa-1&#039;, &#039;etapa-2&#039; e etc | Caso o formulario de cadastro tenha mais de uma etapa , essa variavel deve retornar a etapa do formulario em que o usuario esta, caso contrario retornar a informação vazia. |

<br />

- **Quando:** No callback dos campos
- **Onde:** Na tela de cadastro de dados para os produtos de  assistencia e seguros

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:seguros-e-assistencias:cadastro" ,
        	eventAction: "callback:campo:[[etapa-form]]",
        	eventLabel: "[[nome-campo]]:[[sucesso-erro]]" ,
        	produtoAssistenciaSeguro: "produtoAssistenciaSeguro" 
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[etapa-form]] | &#039;etapa-1&#039;, &#039;etapa-2&#039; e etc | Caso o formulario de cadastro tenha mais de uma etapa , essa variavel deve retornar a etapa do formulario em que o usuario esta, caso contrario retornar a informação vazia. |
| [[nome-campo]] | &#039;nome&#039;, &#039;cpf&#039;, &#039;celular&#039; e etc | Deve retornar o nome do campo preenchido. |
| [[sucesso-erro]] | &#039;sucesso&#039;, &#039;campo-invalido&#039; e etc | Deve retornar se o campo foi preenchido com sucesso ou o tipo de erro . |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 

<br />

- **Quando:** No clique nos botões
- **Onde:** Na tela de cadastro de dados para os produtos de  assistencia e seguros

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:seguros-e-assistencias:cadastro",
        	eventAction: "clique:botao" ,
        	eventLabel: "[[etapa-form]]:[[nome-botao]]" ,
        	produtoAssistenciaSeguro: "produtoAssistenciaSeguro"
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[etapa-form]] | &#039;etapa-1&#039;, &#039;etapa-2&#039; e etc | Caso o formulario de cadastro tenha mais de uma etapa , essa variavel deve retornar a etapa do formulario em que o usuario esta, caso contrario retornar a informação vazia. |
| [[nome-botao]] |  &#039;continuar&#039;, &#039;cancelar&#039; , &#039;voltar&#039; e etc | Deve retornar o nome do botão clicado. |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 


<br />

**Visualização da tela de planos após finalizar o preenchimento do formulario de cadastro**

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/[[nome-produto]]/planos")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039;  e etc&quot; | Deve retornar a tela de qual opção o usuario escolheu. |

<br />

- **Quando:** No clique nos botões( OBS: Os extra parameters só devem retornar preenchidos, quando o usuario selecionar o plano)
- **Onde:** Na tela de planos para os produtos de  assistencia e seguros e detalhes de planos

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "riachuelo:app:seguros-e-assistencias:planos" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]" ,
        	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]", 
        	qtdeParcelas: "[[qtdeParcelas]]" ,
			planoAssistenciaSeguro:"[[planoAssistenciaSeguro]]",
			valorParcela:"[[valorParcela]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] |  &#039;continuar&#039;, &#039;contratar&#039; , &#039;voltar&#039; e etc | Deve retornar o nome do botão clicado. |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 
| [[qtdeParcelas]] | &#039;1&#039;, &#039;2&#039;, &#039;12&#039; e etc |Deve retornar a quantidade da parcela do plano selecionado dos produto de assistencia ou seguro |
| [[[planoAssistenciaSeguro]] | &#039;essencial&#039;, &#039;mega&#039; e etc |Deve retornar o plano do produto de assistencia ou seguro | 
| [[valorParcela]] | &#039;24.90&#039;, &#039;15.90&#039; e etc |Deve retornar o valor da parcela do plano selecionado dos produto de assistencia ou seguro |

<br />

**Visualização da tela de cartão riachuelo, após o usuario escolher o plano do produto de assistencia e seguros**

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/[[nome-produto]]/cartao-riachuelo")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039;  e etc | Deve retornar a tela de qual opção o usuario escolheu. |

<br />

- **Quando:** No clique nos botões e links(Os extraparameters planoAssistenciaSeguro, qtdeParcelas e valorParcela só devem ser retornados quando o clique for no botão &quot;continuar&quot; ou &quot;contratar&quot;).
- **Onde:**  Na tela de cartão riachuelo, após o usuario escolher o plano do produto de assistencia e seguros

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:seguros-e-assistencias:cartao-riachuelo" ,
        	eventAction: "clique:[[botao-link]]" ,
        	eventLabel: "[[nome-item]]" ,
        	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]",
        	qtdeParcelas: "[[qtdeParcelas]]" ,
			    planoAssistenciaSeguro:"[[planoAssistenciaSeguro]]",
			    valorParcela:"[[valorParcela]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] |  &#039;cancelar&#039;, &#039;continuar&#039;, &#039;contratar&#039; e etc | Deve retornar o nome do item clicado(Os extraparameters planoAssistenciaSeguro, qtdeParcelas e  |
| [[botao-link]] | &#039;botao&#039; ou &#039;link&#039;  . | Deve retornar se o item clicado foi um link ou um botão. |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 
| [[qtdeParcelas]] | &#039;1&#039;, &#039;2&#039;, &#039;12&#039; e etc |Deve retornar a quantidade da parcela do plano selecionado dos produto de assistencia ou seguro |
| [[[planoAssistenciaSeguro]] | &#039;essencial&#039;, &#039;mega&#039; e etc |Deve retornar o plano do produto de assistencia ou seguro | 
| [[valorParcela]] | &#039;24.90&#039;, &#039;15.90&#039; e etc |Deve retornar o valor da parcela do plano selecionado dos produto de assistencia ou seguro |


<br />

**Ao visualizar a tela de &quot;Plano escolhido&quot;**


```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/[[nome-produto]]/[[nome-plano]]")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039;  e etc&quot; | Deve retornar a tela de qual opção o usuario escolheu. |
| [[nome-plano]] | &#039;pacote-mega&#039;, &#039;mais-saude-premiavel&#039; e etc | Deve retornar o nome do plano escolhido. |

<br />

- **Quando:** No clique nos botões e links(Os extraparameters planoAssistenciaSeguro, qtdeParcelas e valorParcela só devem ser retornados quando o clique for no botão &quot;Finalizar Contratação&quot;)
- **Onde:** Na tela de &#039;Plano Escolhido&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:seguros-e-assistencias:plano-escolhido",
        	eventAction: "clique:[[botao-link]]" ,
        	eventLabel: "[[nome-item]]" ,
        	produtoAssistenciaSeguro:"[[produtoAssistenciaSeguro]]",
			planoAssistenciaSeguro : "[[planoAssistenciaSeguro]]",
	    	qtdeParcelas :" [[qtdeParcelas ]]",
			valorParcela: "[[valorParcela]] 
			})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] |  &#039;cancelar, &#039;finalizar-contratacao&#039; e etc | Deve retornar o nome do item clicado (Os extraparameters planoAssistenciaSeguro, qtdeParcelas e  |
| [[botao-link]] | &#039;botao&#039; ou &#039;link&#039;  . | Deve retornar se o item clicado foi um link ou um botão. |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 
| [[qtdeParcelas]] | &#039;1&#039;, &#039;2&#039;, &#039;12&#039; e etc |Deve retornar a quantidade da parcela do plano selecionado dos produto de assistencia ou seguro |
| [[[planoAssistenciaSeguro]] | &#039;essencial&#039;, &#039;mega&#039; e etc |Deve retornar o plano do produto de assistencia ou seguro | 
| [[valorParcela]] | &#039;24.90&#039;, &#039;15.90&#039; e etc |Deve retornar o valor da parcela do plano selecionado dos produto de assistencia ou seguro |

<br />

- **Quando:** Ao aceitar os termos listados na tela
- **Onde:** Na tela de plano escolhido, após o usuario escolher o plano do produto de assistencia e seguros

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:seguros-e-assistencias:plano-escolhido" ,
        	eventAction: "[[acao]]:termos" ,
        	eventLabel: "[[termo-aceito]]",
        	produtoAssistenciaSeguro: "produtoAssistenciaSeguro"
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[acao]] | &#039;marcou&#039; ou &#039;desmarcou&#039; . | Deve retornar se o usuario marcou ou desmarcou o termo. |
| [[termo-aceito]] | &#039;termos-e-condicoes&#039;, &#039;compartilhar-dados-com-vale-saude&#039; e etc | Deve retornar o nome do termo aceito. |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 

<br />

**Visualização da tela de &#039;Termos e Condições&#039;**

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/[[nome-produto]]/[[nome-plano]]/termos-e-condicoes")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039;  e etc&quot; | Deve retornar a tela de qual opção o usuario escolheu. |
| [[nome-plano]] | &#039;pacote-mega&#039;, &#039;mais-saude-premiavel&#039; e etc | Deve retornar o nome do plano escolhido. |

<br />

- **Quando:** No clique nos botões .
- **Onde:** Na tela de  &#039;Termos e Condições&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:seguros-e-assistencias:termos-e-condicoes" ,
        	eventAction: "clique:botao" ,
       		eventLabel: "[[nome-item]]" ,
        	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]" 
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] |  &#039;entendi&#039;, &#039;fechar&#039; e etc | Deve retornar o nome do botão clicado. |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 


<br />

**Visualização do modal de confirmação de adesão do plano**

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/[[nome-produto]]/confirmacao-de-adesao")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039;  e etc&quot; | Deve retornar a tela de qual opção o usuario escolheu. |

<br />

- **Quando:** No clique nos botões .
- **Onde:** No modal de confirmação de adesão do plano.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:seguros-e-assistencias:cartao-riachuelo",
        	eventAction: "modal:clique:botao" ,
        	eventLabel: "confirmar-adesao:[[nome-botao]]",
        	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]" 
		})
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039;  e etc&quot; | Deve retornar a tela de qual opção o usuario escolheu. |
| [[nome-botao]] |  &#039;fechar&#039; &#039;sim-confirmar-e-contratar&#039;  e etc | Deve retornar o nome do botão clicado. |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 


<br />

**Visualização da tela de sucesso ou erro ao tentar contratar o plano**

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/[[nome-produto]]/contratacao-plano/[[sucesso-erro]]")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039;  e etc | Deve retornar a tela de qual opção o usuario escolheu. |
| [[sucesso-erro]] | &#039;sucesso&#039; ou &#039;erro&#039; | Deve retornar se a tela é de erro ou sucesso. |

<br />

- **Quando:** No callback de sucesso.
- **Onde:** Na tela de sucesso da contratação do produto de &#039;seguros e assistencia&#039;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "riachuelo:app:seguros-e-assistencias",
        	eventAction: "callback:contratacao",
        	eventLabel: "sucesso",
        	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]",
        	qtdeParcelas: "[[qtdeParcelas]]",            
			    planoAssistenciaSeguro:"[[planoAssistenciaSeguro]]",
			    valorParcela:"[[valorParcela]]"
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 
| [[qtdeParcelas]] | &#039;1&#039;, &#039;2&#039;, &#039;12&#039; e etc |Deve retornar a quantidade da parcela do plano selecionado dos produto de assistencia ou seguro |
| [[[planoAssistenciaSeguro]] | &#039;essencial&#039;, &#039;mega&#039; e etc |Deve retornar o plano do produto de assistencia ou seguro | 
| [[valorParcela]] | &#039;24.90&#039;, &#039;15.90&#039; e etc |Deve retornar o valor da parcela do plano selecionado dos produto de assistencia ou seguro |

<br />

- **Quando:** No callback de erro.
- **Onde:** Na tela de erro da contratação do produto de &#039;seguros e assistencia&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:seguros-e-assistencias",
        	eventAction: "callback:contratacao",
        	eventLabel: "erro:[[tipo-erro]]",
        	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]" ,
        	qtdeParcelas: "[[qtdeParcelas]]",
          planoAssistenciaSeguro:"[[planoAssistenciaSeguro]]",
			    valorParcela:"[[valorParcela]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-erro]] | &#039;cadastro-invalido&#039; e etc. | Deve retornar  o tipo de erro . |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 
| [[qtdeParcelas]] | &#039;1&#039;, &#039;2&#039;, &#039;12&#039; e etc |Deve retornar a quantidade da parcela do plano selecionado dos produto de assistencia ou seguro |
| [[[planoAssistenciaSeguro]] | &#039;essencial&#039;, &#039;mega&#039; e etc |Deve retornar o plano do produto de assistencia ou seguro | 
| [[valorParcela]] | &#039;24.90&#039;, &#039;15.90&#039; e etc |Deve retornar o valor da parcela do plano selecionado dos produto de assistencia ou seguro |

<br />

- **Quando:** No clique nos botões .
- **Onde:** Na tela de sucesso da contratação do produto de &#039;seguros e assistencia&#039; ou cancelamento da contratação

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "riachuelo:app:seguros-e-assistencias:[[tela]]" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]" ,
        	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]" 
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;ok&#039;, &#039;fechar&#039;, &#039;sim&#039;, &#039;nao&#039; e &#039;continuar&#039; e etc | Deve retornar o nome do botão. |
| [[tela]] | &#039;cancelamento&#039;, &#039;conclusao&#039; e etc | Deve retornar se o usuario esta na tela de conclusao ou de cancelamento. |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 


<br />

**Visualização da tela de cancelar contratação do plano**

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-e-assistencias/[[nome-produto]]/contratacao-plano/cancelar")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039;  e etc&quot; | Deve retornar a tela de qual opção o usuario escolheu. |

<br />

**Visualização das telas de "Produtos Contratados" (contratados ou cancelados)**

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/seguros-assistencias/lista-produtos/", "[[currentClass]]");
    Analytics.logEvent("event", {
         	produtoAssistenciaSeguro: "[[produtoAssistenciaSeguro]]" ,
        	qtdeParcelas: "[[qtdeParcelas]]",
          planoAssistenciaSeguro:"[[planoAssistenciaSeguro]]",
			    valorParcela:"[[valorParcela]]"
        })

```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[produtoAssistenciaSeguro]] | &#039;automovel-premiavel&#039;, &#039;moto-premiavel&#039;, &#039;mais-saude&#039; e etc | Deve retornar o nome do produto de assistencia ou seguro | 
| [[qtdeParcelas]] | &#039;1&#039;, &#039;2&#039;, &#039;12&#039; e etc |Deve retornar a quantidade da parcela do plano selecionado dos produto de assistencia ou seguro |
| [[[planoAssistenciaSeguro]] | &#039;essencial&#039;, &#039;mega&#039; e etc |Deve retornar o plano do produto de assistencia ou seguro | 
| [[valorParcela]] | &#039;24.90&#039;, &#039;15.90&#039; e etc |Deve retornar o valor da parcela do plano selecionado dos produto de assistencia ou seguro |


<br />


- **Quando:** Após interagir com as abas (também disparar quando a tela carregar já com a aba por padrão selecionada).
- **Onde:** Nas telas de "Produtos Contratados" (contratados ou cancelados).

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:seguros-e-assistencias:produtos-contratados",
        	eventAction: "clique:aba" ,
        	eventLabel: "[[aba]]:[[qtd-itens]]",
		})
```

<br />


- **Quando:** No clique do ícone de voltar
- **Onde:** Nas telas de "Produtos Contratados" (contratados ou cancelados).

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "riachuelo:app:seguros-e-assistencias:produtos-contratados",
        	eventAction: "clique:botao" ,
        	eventLabel: "voltar",
		})
```

<br />


### Eventos - Cadastro Biometria Facial

**Visualização das telas de &quot;Selfie&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/selfie/")
```


<br />

- **Quando:** No callback, após a leitura da selfie
- **Onde:** Nas telas de &quot;Selfie&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:selfie",
        	"eventAction": "callback:biometria-facial",
        	"eventLabel": "[[status]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;ops-nao-deu-certo&#039;, &#039;em-analise&#039; e etc | Deve retornar o status apresentado. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nas telas de &quot;Selfie&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:selfie:[[nome-tela]]",
        	"eventAction": "clique:[[botao-icone]]",
        	"eventLabel": "[[nome-item]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-tela]] | &#039;ops-nao-deu-certo&#039;, &#039;em-analise&#039; e etc | Deve retornar o nome da tela. |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| [[nome-item]] | &#039;voltar&#039;, &#039;tentar-novamente&#039; e etc | Deve retornar o nome do item. |

<br />

**Visualização dos modais de callback de &quot;Selfie aprovada&quot; ou &quot;Selfie recusada&quot;**<br />

```javascript
    Analytics.setCurrentScreen("/riachuelo-app/cartoes/midway/modal-callback-aprovacao-selfie-[[status]]/")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;aprovada&#039; ou &#039;recusada&#039; | Deve retornar o status apresentado. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nos modais de callback de &quot;Selfie aprovada&quot; ou &quot;Selfie recusada&quot;

```javascript
        Analytics.logEvent("event", {
        	"eventCategory": "riachuelo:app:cartoes:callback-aprovacao-selfie-[[nome-modal]]",
        	"eventAction": "clique:[[botao-icone]]:modal",
        	"eventLabel": "[[nome-item]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-modal]] | &#039;aprovada&#039; ou &#039;recusada&#039; | Deve retornar o nome do modal. |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o elemento clicado. |
| [[nome-item]] | &#039;continuar&#039;, &#039;fechar&#039; e etc | Deve retornar o nome do item. |

<br />
<br />

## Considerações Finais


> Documentações Oficiais do Google: <br />
> [Google Analytics - Avaliação de comércio eletrônico ](https://developers.google.com/analytics/devguides/collection/analyticsjs/ecommerce?hl=pt-br) <br />
> [Google Tag Manager - Guia do desenvolvedor ](https://developers.google.com/tag-manager/enhanced-ecommerce)


<script>
  document.addEventListener('DOMContentLoaded', function(event) {
    document.querySelectorAll('h1 a')[0].style.display = 'none';
  });
</script>
