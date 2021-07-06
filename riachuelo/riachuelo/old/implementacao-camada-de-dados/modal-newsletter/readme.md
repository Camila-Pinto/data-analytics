![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados - Newsletter
Última atualização: 02/10/2019 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes, para utilização dos recursos de monitoramento do Google Analytics, referente ao ambiente de [Riachuelo - Modal Newsletter](https://www.riachuelo.com.br/).

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

### Atributos HTML (Data Attributes)

> São atributos customizados inseridos nos elementos HTML da página, permitindo a inclusão de dados adicionais.

**Instalação**
1. Elementos: ```<div>Elemento</div>``` <br />
Todos os elementos do html que serão clicados, deverão ser mapeados recebendo os atributos com sua estrutura no item.

```html
<div 	
     	data-gtm-event-category="[[exemplo:valor-categoria]]"
 	data-gtm-event-action="[[exemplo:valor-acao]]"
 	data-gtm-event-label="[[exemplo:valor-rotulo]]"
 >
	Texto do elemento
</div>
```

#### Importante:
> Também devem ter os data-attributes `data-gtm-event-category`, `data-gtm-event-action` e `data-gtm-event-label`. Preenchidos conforme instruções específicas.

<br />

## Implementação

A documentação foi descrita para algumas áreas especificas do ambiente [Riachuelo - Modal Newsletter](https://www.riachuelo.com.br/).


### Especificações Globais:

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores; <br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais; <br />
Caso a informação solicitada não estiver disponível retornar `undefined`.


----

### Especificação de Micro-conversões:

### Newsletter

**Na aparição do modal de newsletter**<br />

- **Onde:** No modal de cadastro de newsletter flutuante em todas as páginas

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:geral',
		'eventAction': 'visualizou-modal',
		'eventLabel': 'newsletter',
		'nonInteraction': 'true'
	});
</script>
```

<br />

**Na interação para fechar o modal de newsletter**<br />

- **Onde:** No modal de cadastro de newsletter flutuante em todas as páginas

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:geral',
		'eventAction': 'clique:icone',
		'eventLabel': 'fechou'
	});
</script>
```

<br />

**Após o preenchimento do campo de email do newsletter**<br />

- **Onde:** No modal de cadastro de newsletter flutuante em todas as páginas

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:geral',
		'eventAction': 'preencheu-campo:cadastro-newsletter',
		'eventLabel': 'email'
	});
</script>
```

<br />

**No sucesso ou erro da tentativa de cadastro de newsletter**<br />

- **Onde:** No modal de cadastro de newsletter flutuante em todas as páginas

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:geral',
		'eventAction': 'enviar:cadastro-newsletter',
		'eventLabel': '[[sucesso ou erro]]'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[sucesso ou erro]]	| 'sucesso' ou 'erro'	| Deve retornar o status do envio |

<br />


## Considerações Finais:

> Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/quickstart)

> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    document.querySelectorAll("h1 a")[0].style.display = 'none';
  });
</script>
