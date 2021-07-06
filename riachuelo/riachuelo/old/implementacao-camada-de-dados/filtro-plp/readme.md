![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados
Última atualização: 06/03/2020 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes, para utilização dos recursos de monitoramento do Google Analytics, referente ao filtro na PLP do site de [Riachuelo](https://www.riachuelo.com.br/).

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


**No clique nos filtros nas páginas de Busca e de PLPs**<br />

- **Onde:** Na página de busca, após buscar algum termo e também nas páginas de Listas de Produtos;

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:filtro',
		'eventAction': 'clique:filtro',
		'eventLabel': '[[localizacao-filtro]]:[[filtro]]:[[filtro-selecionado]]'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[localizacao-filtro]]		| 'busca', 'plp' | Deve retornar a localização do filtro. |
| [[filtro]] | 'departamento', 'categorias', 'tamanho', 'ordenar', 'mais-filtros', 'modal-mais-filtros' e etc | Deve retornar o nome do filtro. |
| [[filtro-selecionado]] | 'menor-preco', 'masculino', 'infanto-juvenil', 'list','grid','definicao-valor:0-300' e etc | Deve retornar o label do filtro selecionado. |

**OBS: Caso seja selecionado mais de um filtro, retornar de forma concatenada, separando por ponto e vírgula ( ; ). Ex: 'plp:tamanho:46;48'**

<br />

**No clique do botão "Aplicar" para aplicar os filtros selecionados**<br />

- **Onde:** Na página de busca, após buscar algum termo e também nas páginas de Listas de Produtos
- **Título:** "Aplicar"

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:filtro" 
     data-gtm-event-action="clique:botao" 
     data-gtm-event-label="[[localizacao-filtro]]:[[nome-botao]]"
>
Botão
</div>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[localizacao-filtro]] | 'busca', 'plp', etc. | Deve retornar a localização do filtro. |
| [[nome-botao]] | 'limpar' ou 'aplicar' |	Deve retornar o nome do botão. |

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
