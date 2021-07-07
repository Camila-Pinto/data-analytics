![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados - Header, Menu Superior, Footer e Newsletter Footer
Última atualização: 18/02/2020 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes, para utilização dos recursos de monitoramento do Google Analytics, referente ao ambiente de [Riachuelo](https://www.riachuelo.com.br/).

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

A documentação foi descrita para algumas áreas especificas do ambiente [Riachuelo](https://www.riachuelo.com.br/).


### Especificações Globais:

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores; <br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais; <br />
Caso a informação solicitada não estiver disponível retornar tipagem `undefined`.


----

### Especificação de Micro-conversões:

#### Stick Bar

**Quando: No clique dos links do Stick Bar (pré header)**

- **Onde:** Em todas as páginas em que estiverem disponíveis
- **Título ou nome do botão/link:** "Carter's", "Pool", "Jeans", "Geek" etc

```html
<div 	
    data-gtm-event-category="riachuelo:geral"
 	data-gtm-event-action="clique:header-stick-bar"
 	data-gtm-event-label="[[nome-clicado]]"
 >
	Texto do elemento
</div>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-clicado]]		| 'cartes', 'pool', 'plus-size' e etc	| Deve retornar o nome do link do stick bar clicado |

<br />


#### Header

**Quando: No clique dos links do header**

- **Onde:** Em todas as páginas em que estiverem disponíveis
- **Título ou nome do botão/link:** "riachuelo"(logo), "login", "minha-conta" etc 

```html
<div 	
    data-gtm-event-category="riachuelo:geral"
 	data-gtm-event-action="clique:header"
 	data-gtm-event-label="[[nome-clicado]]"
 >
	Texto do elemento
</div>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-clicado]]		| 'riachuelo' (logo), 'login', 'minha-conta' e etc	| Deve retornar o nome do link do header e no logo clicado |

<br />

#### Menu Superior

**Quando: No clique dos links do menu superior**

- **Onde:** Em todas as páginas em que estiverem disponíveis
- **Título ou nome do botão/link:** "Novidades", "Masculino", "Feminino", "Calça" etc 

```html
<div 	
    data-gtm-event-category="riachuelo:geral"
 	data-gtm-event-action="clique:menu-superior"
 	data-gtm-event-label="[[nome-clicado]]"
 >
	Texto do elemento
</div>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-clicado]]		| 'novidades', 'masculino', 'feminino', 'alfaiataria', 'outlet', 'busca', 'camisetas', 'tenis', 'polo_manga_curta', 'super_skinny', 'regular', 'feminino:vestido_feminino_colecao_feminina', 'feminino:busca_feminino_colecao_feminina' e etc	| Deve retornar o nome do link principal do menu |

**Obs: ignorar a categoria e subcategoria quando o clique for diretamente nos links principais do menu superior. Ignorar subcategoria quando o clique for apenas no link de categoria.**

<br />

#### Footer

**Quando: No clique dos ícones de " + "  e " - " do footer para abrir as categorias**

- **Onde:** Em todas as páginas em que estiverem disponíveis

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:geral',
		'eventAction': 'clique:footer:categoria',
		'eventLabel': '[[nome-categoria]]:[[abriu-ou-fechou]]'
	});
</script>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-categoria]]	| 'cartao-riachuelo', 'sobre-a-riachuelo', 'moda-que-transforma' e etc	| Deve retornar o nome da categoria do footer |
| [[abriu-ou-fechou]]	| 'abriu' ou 'fechou'	| Deve retornar o status da categoria |

<br />

**Quando: No clique dos links interno do footer**

- **Onde:** Em todas as páginas em que estiverem disponíveis
- **Título ou nome do botão/link:** "Carter's", "Pool", "Jeans", "Geek" etc

```html
<div 	
    data-gtm-event-category="riachuelo:geral"
 	data-gtm-event-action="clique:footer:link"
 	data-gtm-event-label="[[nome-categoria]]:[[nome-link]]"
 >
	Texto do elemento
</div>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-categoria]]	| 'cartao-riachuelo', 'sobre-a-riachuelo', 'moda-que-transforma' e etc	| Deve retornar o nome da categoria do footer |
| [[nome-link]]	| 'saiba-como-adquirir 'a-empresa', 'entre-costuras'  e etc	| Deve retornar o nome do link do footer clicado |

<br />

### Newsletter Footer

**Quando: Após a seleção de interesse dentro do checkbox para se cadastrar em newsletter**

- **Onde:** No cadastro de newsletter, localizado no footer em todas as páginas

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:geral',
		'eventAction': 'selecionou-interesse:newsletter-footer',
		'eventLabel': '[[interesse-selecionado]]'
	});
</script>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[interesse-selecionado]]	| 'novidades','selecionar-todos','infantil' e etc	| Deve retornar o interesse selecionado no checkbox |

**OBS: Caso seja selecionado mais de uma opção, deve retornar todas as opções concatenadas, separando por ponto e vírgula ( ; ). Ex: "novidades;feminino;" etc**

<br />

**Quando: Após o preenchimento do campo do newsletter**

- **Onde:** No cadastro de newsletter, localizado no footer em todas as páginas

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:geral',
		'eventAction': 'preencheu-campo:cadastro-newsletter-footer',
		'eventLabel': 'email'
	});
</script>
```

<br />

**Quando: No sucesso ou erro da tentativa de cadastro de newsletter**

- **Onde:** No cadastro de newsletter, localizado no footer em todas as páginas

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:geral',
		'eventAction': 'enviar:cadastro-newsletter-footer',
		'eventLabel': '[[sucesso ou erro]]'
	});
</script>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[sucesso ou erro]]	| 'sucesso' ou 'erro'	| Deve retornar o status do envio |

<br />

**Quando: No clique nos links de redes sociais**

- **Onde:** Localizado no footer em todas as páginas
- **Título ou nome do botão/link:** "riachuelo-facebook", "riachuelo-youtube" etc

```html
<div 	
    data-gtm-event-category="riachuelo:geral"
 	data-gtm-event-action="clique:link:rede-social:footer"
 	data-gtm-event-label="[[nome-link]]"
 >
	Texto do elemento
</div>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-link]]	| 'riachuelo-facebook','riachuelo-youtube','riachuelo-linkedin' e etc	| Deve retornar o nome do link da rede social clicada |


## Considerações Finais:

> Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/quickstart)

> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    document.querySelectorAll("h1 a")[0].style.display = 'none';
  });
</script>
