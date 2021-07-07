![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados Ecommerce - PDP
Última atualização: 11/09/2020 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes, para utilização dos recursos de monitoramento do Google Analytics, referente ao ambiente de [Riachuelo](https://www.riachuelo.com.br).

<br />

## Overview e Descrições Técnicas

### Camada de dados (DataLayer)

> É um array de objetos javascript, utilizado pelo Google Tag Manager, que recebe em seus atributos dados importantes do site.
Para implementar o dataLayer no site, o desenvolvedor pode utilizar formas diferentes para preencher os dados. Essas formas são dependentes da ação estabelecida na documentação e do nível da interação.

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

<br />

## Implementação

A documentação foi descrita para algumas áreas especificas do ambiente [Riachuelo](https://www.riachuelo.com.br).


### Especificações Globais:

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores; <br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais; <br />
Caso a informação solicitada não estiver disponível retornar 'nao_disponivel'.

---

### Enhanced Ecommerce

### Product Detail:

**No carregamento da página de detalhe ou no quickview com informações detalhadas de produto.**<br />

- **Onde:** Na página de detalhes de produto e na página de lista de produtos

```html
<script>
dataLayer.push({
  'event': 'r_product_detail',
  'ecommerce': {
    'detail': {
      'actionField': {'list': '[[sessao-pagina-atual]]'},
      'products': [{
      'name': '[[nome-produto]]',       //Nome ou ID do produto é obrigatório
      'id': '[[sku-produto-pai]]',
      'price': '[[preco-produto]]',
      'category': '[[departamento-categoria-produto]]',
      'variant': '[[codigo-dco]]',
      'dimension13': '[[idade-produto]]',
      'dimension18': '[[departamento-produto]]',
      'dimension19': '[[id-look]]',
      'dimension20': '[[categoria-produto]]',
      'dimension21': '[[look-completo]]',
      'dimension24': '[[ordem-insercao]]',
      'dimension27': '[[padronagem-produto]]',
      'dimension30': '[[sub-categoria]]',
      'dimension32': '[[preco-original]]',
      'dimension38': '[[lifestyle]]',
      'dimension39': '[[gender]]',
      'dimension40': '[[exclusivo-ecommerce+mktplace]]',
      'dimension42': '[[sku-produto-filho]]',
      'dimension58': '[[selo-ou-tag]]'
    }]
    }
  }
 });
</script>
```

| Variável        | Exemplo         | Descrição                   |
| :-------------------- | :-------------------- | :-------------------------------------------  |
| [[sku-produto-filho]]      | '2552' e etc  | Deve retornar o id filho do produto |
| [[nome-produto]] | 'calca-masculina-super-skinny-em-jeans' e etc        | Deve retornar o nome do produto |
| [[sku-produto-pai]]     | '13239635'      | Deve retornar o sku do produto pai                 |
| [[preco-produto]]      | '139.99' e etc     | Deve retornar o preço do produto |
| [[departamento-categoria-produto]]     | 'masculino'       | Deve retornar o Departamento e a Categoria do produto |
| [[codigo-dco]] | '325' e etc        | Deve retornar o código DCO - Categoria do produto|
| [[idade-produto]]      | 'x-day' e etc     | Deve retornar a idade do produto cadastrado no Magento (checkout)|
| [[departamento-produto]]     | 'masculino'       | Deve retornar o departamento da empresa principal do produto|
| [[id-look]] | 'produto-sem-vinculo' e etc        | Deve retornar o id do produto do tipo do look|
| [[categoria-produto]]     | 'acessorio'      | Deve retornar a categoria secundaria  do produto|
| [[look-completo]]      | 'true' ou 'false' | Deve retornar o look completo |
| [[ordem-insercao]]     | '1', '2' e etc       | Deve retornar a ordem de inserção ao carrinho|
| [[padronagem-produto]] | 'florido', 'listrado' e etc        | Deve retornar o padrão da estampa do produto|
| [[sub-categoria]] | '310090' e etc        | Deve retornar o código da categoria GM|
| [[preco-original]]     | '12.00' e etc      | Deve retornar o preço do produto (dê) |
| [[lifestyle]]      | 'casual', 'esportivo' e etc     | Deve retornar o estilo do produto |
| [[gender]]     | 'feminino', unissex' e etc      | Deve retornar o gênero do produto|
| [[exclusivo-ecommerce+mktplace]]      | 'sim:riachuelo', 'nao:riachuelo', 'nao:pontofrio', 'nao:extra' | Deve retornar se o produto é exclusivo do ecommerce e o seu seller/marketplace |
| [[selo-ou-tag]]     | 'selo:black-friday', 'tag:frete-gratis'       | Deve retornar o tipo do selo ou tag e a promoção<br /> **OBS: Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por " ; " (Ponto e vírgula)**|

<br />

### AddToCart:

**No clique para adicionar produto ao carrinho**<br />

- **Onde:** Na página de detalhes de um produto

```html
<script>
dataLayer.push({
  'event': 'r_addtocart',
  'ecommerce': {
    'add': {
      'products': [{
       'name': '[[nome-produto]]',       //Nome ou ID do produto é obrigatório
       'id': '[[sku-produto-pai]]',
       'price': '[[preco-produto]]',
       'category': '[[departamento-categoria-produto]]',
       'variant': '[[codigo-dco]]',
       'quantity': '[[quantidade-produto]]'
       'dimension13': '[[idade-produto]]',
       'dimension18': '[[departamento-produto]]',
       'dimension19': '[[id-look]]',
       'dimension20': '[[categoria-produto]]',
       'dimension21': '[[look-completo]]',
       'dimension24': '[[ordem-insercao]]',
       'dimension25': '[[tamanho-produto]]',
       'dimension26': '[[cor-produto]]',
       'dimension27': '[[padronagem-produto]]',
       'dimension30': '[[sub-categoria]]',
       'dimension32': '[[preco-original]]',
       'dimension38': '[[lifestyle]]',
       'dimension39': '[[gender]]',
       'dimension40': '[[exclusivo-ecommerce+mktplace]]',
       'dimension42': '[[sku-produto-filho]]',
       'dimension58': '[[selo-ou-tag]]'
     }]
    }
  }
  });
</script>
```

| Variável        | Exemplo         | Descrição                   |
| :-------------------- | :-------------------- | :-------------------------------------------  |
| [[sku-produto-filho]]      | '2552' e etc  | Deve retornar o id filho do produto |
| [[nome-produto]] | 'calca-masculina-super-skinny-em-jeans' e etc        | Deve retornar o nome do produto |
| [[sku-produto-pai]]     | '13239635'      | Deve retornar o sku do produto pai                 |
| [[preco-produto]]      | '139.99' e etc     | Deve retornar o preço do produto |
| [[departamento-categoria-produto]]     | 'masculino'       | Deve retornar o Departamento e a Categoria do produto |
| [[codigo-dco]] | '325' e etc        | Deve retornar o código DCO - Categoria do produto|
| [[quantidade-produto]] | '1' e etc        | Deve retornar a quantidade do produto |
| [[idade-produto]]      | 'x-day' e etc     | Deve retornar a idade do produto cadastrado no Magento (checkout)|
| [[departamento-produto]]     | 'masculino'       | Deve retornar o departamento da empresa principal do produto|
| [[id-look]] | 'produto-sem-vinculo' e etc        | Deve retornar o id do produto do tipo do look|
| [[categoria-produto]]     | 'acessorio'      | Deve retornar a categoria secundaria  do produto|
| [[look-completo]]      | 'true' ou 'false' | Deve retornar o look completo |
| [[ordem-insercao]]     | '1', '2' e etc       | Deve retornar a ordem de inserção ao carrinho|
| [[tamanho-produto]]     | 'p', 'm', '8-12', '42' e etc       | Deve retornar o tamanho do produto|
| [[cor-produto]]     | 'vermelho' e etc       | Deve retornar a cor do produto|
| [[padronagem-produto]] | 'florido', 'listrado' e etc        | Deve retornar o padrão da estampa do produto|
| [[sub-categoria]] | '310090' e etc        | Deve retornar o código da categoria GM|
| [[preco-original]]     | '12.00' e etc      | Deve retornar o preço do produto (dê) |
| [[lifestyle]]      | 'casual', 'esportivo' e etc     | Deve retornar o estilo do produto |
| [[gender]]     | 'feminino', unissex' e etc      | Deve retornar o gênero do produto|
| [[exclusivo-ecommerce+mktplace]]      | 'sim:riachuelo', 'nao:riachuelo', 'nao:pontofrio', 'nao:extra' | Deve retornar se o produto é exclusivo do ecommerce e o seu seller/marketplace |
| [[selo-ou-tag]]     | 'selo:black-friday', 'tag:frete-gratis'       | Deve retornar o tipo do selo ou tag e a promoção<br /> **OBS: Caso apareça tanto o selo quanto a tag, disparar os dois na mesma dimensão e separar por " ; " (Ponto e vírgula)**|

<br />

---

## Considerações Finais:

> Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/quickstart)

> Recomendações do Google:
> 1. [Enhanced Ecommerce - Product Detail](https://developers.google.com/tag-manager/enhanced-ecommerce#details)
> 2. [Enhanced Ecommerce - AddToCart / Remove From Cart](https://developers.google.com/tag-manager/enhanced-ecommerce#cart)

> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    document.querySelectorAll("h1 a")[0].style.display = 'none';
  });
</script>
