![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Riachuelo - Devolução Online e Retirar em loja
Última atualização: 02/03/2020 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes, para utilização dos recursos de monitoramento do Google Analytics, referente ao ambiente de [Riachuelo - Devolução Online e Retirar em loja](http://riachuelo.com.br).

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

A documentação foi descrita para algumas áreas especificas do ambiente [Riachuelo - Devolução Online e Retirar em loja](http://riachuelo.com.br).


### Especificações Globais:

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores; <br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais; <br />
Caso a informação solicitada não estiver disponível retornar 'nao_disponivel'.

---

### Especificação de Micro-conversões:

### Devolução Online e Retirar em Loja

**No clique em "Visualizar Pedido"**<br />

- **Onde:** Na página de "Meus Pedidos"
- **Título ou nome do botão/link:** "Visualizar Pedido".

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:minha-conta:meus-pedidos" 
     data-gtm-event-action="clique:meus-pedidos" 
     data-gtm-event-label="visualizar-pedido:[[id-pedido]]"
>
Botão
</div>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[id-pedido]]			| '236565' '65264561' etc | Deve retornar o id do pedido.           	|

<br />

**No clique das abas em "Detalhes do pedido"**<br />

- **Onde:** Na página de detalhes do pedido
- **Título ou nome do botão/link:** "Informações do Pedido", "Produtos Comprados", "Postagens do Pedido" 

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:minha-conta:meus-pedidos" 
     data-gtm-event-action="clique:detalhes-do-pedido" 
     data-gtm-event-label="[[nome-item]]"
>
Botão
</div>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-item]]			| 'informacoes-do-pedido', 'produtos-comprados', 'postagens-do-pedido'	| Deve retornar o nome do item clicado	|

<br />

**No clique em "Trocar" ou "Devolver"**<br />

- **Onde:**  Na página de detalhes do pedido, em "Produtos Comprados" e dentro de produto
- **Título ou nome do botão/link:** "Trocar" ou "Devolver" 

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:minha-conta:meus-pedidos" 
     data-gtm-event-action="clique:produto:detalhes-do-pedido" 
     data-gtm-event-label="[[devolver ou trocar]]:[[id-pedido]]:[[id-produto-filho]]
>
Botão
</div>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[devolver ou trocar]]| 'devolver' ou 'trocar'| Deve retornar o nome do item clicado.			|
| [[id-pedido]]			| '236565' '65264561' etc | Deve retornar o id do pedido. 			    |
| [[id-produto-filho]]	| Ex: '0655', '8566' <br/> **Obs: No caso de mais que um ID do produto na devolução enviar '1895/0655/8566'**| Deve retornar os ids dos produtos filhos referente ao pedido de devolução.|


<br/>

**No clique dos botões "Voltar", "Avançar" ou "Fechar" - Disparar clique de "Avançar" apenas quando "Li e Concordo com os termos” estiver selecionado**<br />

- **Onde:** No modal de "Termos e Regras de Devolução"
- **Título ou nome do botão/link:** "Voltar", "Aceito" e "Fechar"

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:minha-conta:meus-pedidos" 
     data-gtm-event-action="clique:termos-e-regras-de-devolucao" 
     data-gtm-event-label="[[nome-botao]]"
>
Botão
</div>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-botao]]		| 'fechar', 'voltar' ou 'aceito' | Deve retornar o nome do botão ou ícone clicado.|

<br />


**Na interação com os itens relacionados à devolução**<br />

- **Onde:** Na página de "Solicitação de Devolução"

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
		'eventAction': 'interacao:solicitacao-de-devolucao',
		'eventLabel': '[[motivo-da-solicitacao]]:[[id-pedido]]:[[id-produto-filho]]'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[motivo-da-solicitacao]]		| 'arrependimento', 'defeito', etc	| Deve retornar a opção selecionada em "Motivo da solicitação"	|
| [[id-pedido]]			| '236565' '65264561' etc| Deve retornar o id do pedido.|
| [[id-produto-filho]]	|  '0655', '8566'  <br /> **Obs: no caso de mais que um ID do produto na devolução enviar '1895/0655/8566'**| Deve retornar o id dos produtos filhos referente ao pedido de devolução.	|

<br />


**Nos botões "Voltar" e "Confirmar"**<br />

- **Onde:** Na página de "Solicitação de Devolução"
- **Título ou nome do botão/link:** "Voltar" e"Confirmar"

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:minha-conta:meus-pedidos" 
     data-gtm-event-action="clique:solicitacao-de-devolucao" 
     data-gtm-event-label="[[nome-botao]]"
>
Botão
</div>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-botao]]		| 'voltar' ou 'confirmar' 	| Deve retornar o nome do botão clicado.	|

<br />

**Na aparição do alerta após o carregamento da página de solicitação de devolução**<br />

- **Onde:** Na página de "Solicitação de Devolução"

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
		'eventAction': 'alerta:solicitacao-de-devolucao',
		'eventLabel': '[[mensagem-alerta]]:[[id-pedido]]:[[id-produto-filho]]'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[mensagem-alerta]]   |  'nao-aceitamos-a-devolucao-via-autoatendimento-com-valor-inferior-a-R$80,90' e etc| Deve retornar a mensagem de alerta. <br /> **Obs: quando a solicitação não for válida por conta de compra com valor inferior ao valor mínimo retornar o valor mínimo na mensagem de erro.** |
| [[id-pedido]]			| '236565' '65264561' etc| Deve retornar o id do pedido.|
| [[id-produto-filho]]	|  '0655', '8566'  <br /> **Obs: no caso de mais que um ID do produto na devolução enviar '1895/0655/8566'**| Deve retornar o id dos produtos filhos referente ao pedido de devolução.	|

<br />

**No clique em "Voltar" ou "Avançar"**<br />

- **Onde:** No modal de "Resumo de Pedido"

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
		'eventAction': 'clique:resumo:solicitacao-de-devolucao',
		'eventLabel': '[[nome-botao]]:[[id-pedido]]:[[id-produto-filho]]'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-botao]]        |   'avancar',  'voltar'| Deve retornar o nome do botão clicado.        |
| [[id-pedido]]			| '236565' '65264561' etc| Deve retornar o id do pedido.                |
| [[id-produto-filho]]	|  '0655', '8566'  <br /> **Obs: no caso de mais que um ID do produto na devolução enviar '1895/0655/8566'**| Deve retornar o id dos produtos filhos referente ao pedido de devolução.	|

<br />


**No callback da tentativa de solicitação de devolução ou troca**<br />

- **Onde:** Na página de "Solicitação de Devolução ou Troca"

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
		'eventAction': 'tentativa:solicitacao-de-devolucao',
		'eventLabel': '[[sucesso ou tipo-erro]]:[[id-pedido]]:[[id-produto-filho]]'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[sucesso ou tipo-erro]]|  'sucesso', 'ocorreu-um-erro' etc| Deve retornar o sucesso ou descrição do erro de solicitação de devolução.|
| [[id-pedido]]			| '236565' '65264561' etc | Deve retornar o id do pedido.		        |
| [[id-produto-filho]]	|  '0655', '8566'<br /> **Obs: No caso de mais que um ID do produto na devolução enviar '1895/0655/8566'**| Deve retornar o id dos produtos filhos referente ao pedido de devolução.|


<br />

**Ao selecionar opção de autorizar retirada de pedido por terceiros**<br />

- **Onde:** Na página de detalhes de pedido

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
		'eventAction': 'interacao:meus-pedidos',
		'eventLabel': 'autorizo-a-retirada-do-meu-pedido-por-outras-pessoas'
	});
</script>
```

<br />

**No clique dos botões e ícones do modal de retirada de pedido por terceiros**<br />

- **Onde:** Na página de detalhes de pedido

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:minha-conta:meus-pedidos" 
     data-gtm-event-action="clique:retirada-por-terceiros" 
     data-gtm-event-label="[[nome-botao]]"
>
Botão
</div>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-botao]]		| 'adicionar', 'fechar' 'salvar-e-finalizar', 'remover'	| Deve retornar o nome do botão ou ícone clicado.|

<br />

**No callback da tentativa de inserção de terceiros para retirar pedido**<br />

- **Onde:** Na página de detalhes de pedido

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:minha-conta:meus-pedidos',
		'eventAction': 'tentativa:retirada-por-terceiros',
		'eventLabel': '[[sucesso ou tipo-erro]]:[[id-pedido]]:[[numero-pessoas]]'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------  | :-------------------------------------------	|
| [[sucesso ou tipo-erro]]|  'sucesso', 'ocorreu-um-erro' etc| Deve retornar o sucesso ou descrição do erro de inserção de terceiros para retirada de pedidos	  |
| [[id-pedido]]			| '236565' '65264561' etc |  Deve retornar o id do pedido a ser devolvido.|
| [[numero-pessoas]]    |  '1' ou '2'		    | Deve retornar o numero de pessoas que farão a retirada.|


<br />

## Considerações Finais:

> Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/quickstart)

> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    document.querySelectorAll("h1 a")[0].style.display = 'none';
  });
</script>
