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

## Implementação da Camada de dados
Última atualização: 11/02/2020 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

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

### Especificação de Micro-conversões:


**No carregamento dos tipos de entrega disponíveis**<br />

- **Onde:** Na página de carrinho e checkout

```html
<script>
  dataLayer.push({
    'event': 'event',
    'nonInteraction': 'true',	
    'eventCategory': 'riachuelo:checkout',
    'eventAction': 'tipos-de-entrega:[[cart ou checkout]]',
    'eventLabel': '[{
	"t":"[[nome-entrega]]",
	"d":"[[previsao]]",
	"p":"[[valor]]"
	},
	{
	"t":"[[nome-entrega]]",
	"d":"[[previsao]]",
	"p":"[[valor]]"
	}]'
});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[cart ou checkout]]	| 'cart' ou 'checkout'				| Deve retornar o local onde os tipos de entrega são exibidos				|
| [[nome-entrega]]			| 'normal', 'expresso ', 'entrega-agendada','retirar-em-loja' | Deve retornar o nome dos tipos de entrega.		|
| [[previsao]]			| '10-dias, 03-dias ' etc			| Deve retornar a quantidade de dias para a entrega							|
| [[valor]]			| '2.99', '5.00'				| Deve retornar os valores dos fretes.									|

<br />

---

<br />

### Enhanced Ecommerce

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
        'actionField': {'step':'[[index]]','option':'frete:[[nome-entrega]]:[[previsao]]:[[valor]]'}
      }
    }
  });
</script>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[index]]	| '1' ou '4'				| Deve retornar a etapa onde o frete foi selecionado. Carrinho = 1; Entrega = 4				|
| [[nome-entrega]]			| 'normal'			| Deve retornar o nome do tipo de entrega selecionado.									|
| [[previsao]]			| '03-dias' etc			| Deve retornar a quantidade de dias para a entrega							|
| [[valor]]			| '10.99'				| Deve retornar os valor do frete selecionado						|

---

## Considerações Finais


> Documentações Oficiais do Google: <br />
> [Google Analytics - Avaliação de comércio eletrônico ](https://developers.google.com/analytics/devguides/collection/analyticsjs/ecommerce?hl=pt-br)
> [Google Tag Manager - Guia do desenvolvedor ](https://developers.google.com/tag-manager/enhanced-ecommerce)

<script>
  document.addEventListener('DOMContentLoaded', function(event) {
    document.querySelectorAll('h1 a')[0].style.display = 'none';
  });
</script>
