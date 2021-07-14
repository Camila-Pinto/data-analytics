![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Modais Sacola e Login

Última atualização: 26/03/2020  <br />
Em caso de dúvidas, entrar em contato com: [](mailto:@zoly.com.br)

<br />

## Objetivo

Este documento tem como objetivo instruir a implementação da camada de dados para utilização de recursos do virtual pageview referentes ao ambiente [Riachuelo](http://riachuelo.com.br/).

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
Caso a informação solicitada não estiver disponível retornar tipagem `undefined`.

---

### Virtual Pageview PLP e PDP:

**No carregamento do modal lateral  - Sua Sacola com produtos**<br />

- **Onde:** No modal lateral, em todas as páginas
 
```javascript
window.dataLayer = window.dataLayer || [];
dataLayer.push({
    'event'	: 'virtual-pageview',
    'page'	: '/modal-carrinho-com-produtos/',
});
```
<br />

**No carregamento do modal lateral  - Sua Sacola sem produtos**<br />

- **Onde:** No modal lateral, em todas as páginas
 
```javascript
window.dataLayer = window.dataLayer || [];
dataLayer.push({
    'event'	: 'virtual-pageview',
    'page'	: '/modal-carrinho-sem-produtos/',
});
```
<br />

**No carregamento do modal de login para confirmar o cpf**<br />

- **Onde:** No modal lateral, em todas as páginas
 
```javascript
window.dataLayer = window.dataLayer || [];
dataLayer.push({
    'event'	: 'virtual-pageview',
    'page'	: '/modal-confirmacao-cpf/',
});
```
<br />

**No carregamento do modal de login para confirmar a senha**<br />

- **Onde:** No modal lateral, em todas as páginas
 
```javascript
window.dataLayer = window.dataLayer || [];
dataLayer.push({
    'event'	: 'virtual-pageview',
    'page'	: '/modal-confirmacao-senha/',
});
```
<br />

---

**No carregamento do modal lateral - Sua Sacola sem produtos ou com produtos**<br />

- **Onde:** No modal lateral, em todas as páginas.

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:geral',
		'eventAction': 'visualizou-modal-lateral',
		'eventLabel': 'sacola:[[com ou sem]]-produtos'
		'nonInteraction': 'true'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[com ou sem]]|  'sacola:com-produtos', 'sacola:sem-produtos'| Deve retornar se a sacola está com ou sem produtos. |

<br />

**No carregamento do modal de login para confirmar o cpf e senha**<br />

- **Onde:** No modal lateral, em todas as páginas.

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:geral',
		'eventAction': 'visualizou-modal-lateral',
		'eventLabel': '[[cpf ou senha]]'
		'nonInteraction': 'true'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[cpf ou senha]] |'cpf', 'senha'|  Deve retornar a confirmação que o usuário preencheu cpf e senha.|

<br />

 
## Considerações Finais
 

> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)
 
<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    document.querySelectorAll("h1 a")[0].style.display = 'none';
  });
</script>
