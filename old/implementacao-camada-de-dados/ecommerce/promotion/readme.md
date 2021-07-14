![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Enhanced Ecommerce

Última atualização: 20/01/2020  <br />
Em caso de dúvidas, entrar em contato com: [](mailto:@zoly.com.br)

<br />

## Objetivo

Este documento tem como objetivo instruir a implementação da camada de dados para utilização de recursos do enhanced ecommerce referentes ao ambiente [Riachuelo](http://riachuelo.com.br/).

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

A documentação foi descrita para algumas áreas especificas do ambiente [Riachuelo](http://riachuelo.com.br/).

---

## Especificações Globais:

**Itens Gerais:**<br />

Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores;<br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais;<br />
Caso a informação solicitada não estiver disponível retornar `undefined`.

---

### Promotion Impression:

**Na visualização de cada banner exibido durante a navegação - Em todas as páginas que exibirem banners (home, clp, plp)**<br />

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez na visualização de um banner.
 
```html
<script>
dataLayer.push({
	'event': 'r_promoView',
	'eventCategory': 'enhanced-ecommerce',
	'eventAction': 'promoView',
	'eventLabel': '',
	'ecommerce': {
		'promoView': {
			'promotions': [{
				'id': '[[id-banner]]',				//Nome ou ID do banner é obrigatório
				'name': '[[nome-banner]]',
				'creative': '[[criativo-banner]]',
				'position': '[[posicao-banner]]'
			}]
		}
	}
});
</script>
```

<br />

| Nome 			| Variável 					| Exemplo 					| Descrição 								|
| :------------	| :------------------------ | :------------------------ | :---------------------------------------- |
| id			| [[id-banner]] 			| 'https://midia.fotos-riachuelo.com.br/fotos/megamenu/home/janeiro2020/ADS-5506/Carrossel-home-mcasa-kit-colcha_.jpg' e etc				| Deve retornar o link da imagem do banner          	|
| name 			| [[nome-banner]] 			| 'polos-diferenciadas' e etc	| Deve retornar o nome amigável do banner 				|
| creative		| [[criativo-banner]]		| 'hero;clp;banner:clicavel', 'mosaico;blog;banner:nao-clicavel' e etc	| Deve retornar o formato, o tipo de página que o banner foi clicado e o tipo do banner<br /> **OBS: Deve retornar separado por " ; " (ponto e vírgula)**  				|
| position 		| [[posicao-banner]] 		| '1'	 					| Posição que o banner aparece na lista		|
 
<br />

### Promotion Click:

**No clique dos banners - Em todas as páginas que exibirem banners (home, clp, plp)**<br />

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no clique de um banner.
 
```html
<script>
dataLayer.push({
	'event': 'r_promoClick',
	'eventCategory': 'enhanced-ecommerce',
	'eventAction': 'promoClick',
	'eventLabel': '',
	'ecommerce': {
		'promoClick': {
			'promotions': [{
				'id': '[[id-banner]]',				//Nome ou ID do produto é obrigatório
				'name': '[[nome-banner]]',
				'creative': '[[criativo-banner]]',
				'position': '[[posicao-banner]]'
			}]
		}
	}
});
</script>
```

<br />
 
| Nome 			| Variável 					| Exemplo 					| Descrição 								|
| :------------	| :------------------------ | :------------------------ | :---------------------------------------- |
| id			| [[id-banner]] 			| 'https://midia.fotos-riachuelo.com.br/fotos/megamenu/home/janeiro2020/ADS-5506/Carrossel-home-mcasa-kit-colcha_.jpg' e etc				| Deve retornar o link da imagem do banner          	|
| name 			| [[nome-banner]] 			| 'polos-diferenciadas' e etc	| Deve retornar o nome amigável do banner 				|
| creative		| [[criativo-banner]]		| 'hero;clp;banner:clicavel', 'mosaico;blog;banner:nao-clicavel' e etc	| Deve retornar o formato, o tipo de página que o banner foi clicado e o tipo do banner<br /> **OBS: Deve retornar separado por " ; " (ponto e vírgula)** 				|
| position 		| [[posicao-banner]] 		| '1'	 					| Posição que o banner aparece na lista		|
 

<br />

 
## Considerações Finais
 
> Recomendações do Google:

> 1. [Enhanced Ecommerce - Promotion Impression](https://developers.google.com/tag-manager/enhanced-ecommerce#promo-impressions)
> 2. [Enhanced Ecommerce - Promotion Click](https://developers.google.com/tag-manager/enhanced-ecommerce#promo-clicks)
>
> Em caso de dúvidas, entrar em contato com: [](mailto:@zoly.com.br)
 
<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    document.querySelectorAll("h1 a")[0].style.display = 'none';
  });
</script>
