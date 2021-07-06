![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados
Última atualização: 05/08/2019 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes, para utilização dos recursos de monitoramento do Google Analytics, referente ao recurso de lightbox de abondono do site de Riachulo.

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

### Especificação de Micro-conversões:


**Exibição do LightBox de Frete Grátis**<br />

- **Onde:** Todas as páginas de Checkout;

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:geral',
		'eventAction': 'exibiu:lightbox',
		'eventLabel': '[[id-lightbox]]',
		'nonInteraction': 'true'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[id-lightbox]]		| 'opc-boleto-10', 'opc-boleto-14', 'opc-boleto-16', etc.				| Disparar o ID do lightbox	|

<br />

**Clique em 'ir as compras', 'continuar', 'não, obrigado', 'X' para fechar.**<br />

- **Onde:** Lightbox de frete grátis nas páginas do checkout.

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:geral" 
     data-gtm-event-action="clique:lightbox" 
     data-gtm-event-label="[[id lightbox]]:[[botao-clicado]]"
>
Botão
</div>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[id-lightbox]]		| 'opc-boleto-10', 'opc-boleto-14', 'opc-boleto-16', etc.				| Disparar o ID do lightbox	|
| [[botao-clicado]]		|  'ir-as-compras' ou 'continuar', 'nao-obrigado' |	Retornar o texto do elemento que foi clicado	|

<br />

---

## Considerações Finais:

> Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/quickstart)

> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    document.querySelectorAll("h1 a")[0].style.display = 'none';
  });
</script>
