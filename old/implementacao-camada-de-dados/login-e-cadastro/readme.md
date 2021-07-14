![Zoly](http://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics<br />
> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados - Login e Cadastro
Última atualização: 27/11/19 <br />
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
Caso a informação solicitada não estiver disponível retornar 'nao_disponivel'.

---

### Especificação de Micro-conversões:

## Login

**Ao clicar no botão de “Cadastre-se usando suas Redes Sociais”**<br />

- **Onde:** Modal de Cadastro/Login;

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:login" 
     data-gtm-event-action="clique:cadastre-com-redes-sociais" 
     data-gtm-event-label="[[nome-botao]]"
>
Botão
</div>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-botao]]		| 'facebook, google'.   | Deve retornar o botão clicado.		        |

<br />

**No preenchimento do campo "Enter Cpf" e "Senha"**<br />

- **Onde:** No modal Entrar/Criar Conta;

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:login',
		'eventAction': 'preencheu:campo',
		'eventLabel': '[[nome-campo]]'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-campo]]		|  'cpf, senha'.   | Deve retornar o nome do campo preenchido.		        |

<br />


**No clique do link "Esqueceu a sua senha? Clique aqui"**<br />

- **Onde:** No modal Entrar/Criar Conta;

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:login" 
     data-gtm-event-action="clique:link" 
     data-gtm-event-label="esqueceu-senha"
>
Botão
</div>
```



<br />


**No callback da tentativa de login**<br />

- **Onde:** No modal Entrar/Criar Conta;

```html
<script>
	dataLayer.push({
		'event': 'login',
		'eventCategory': 'riachuelo:login',
		'eventAction': 'tentativa:callback',
		'eventLabel': '[[sucesso ou tipo-erro]]',
		'dimension5': '[[Login Status]]',
		'dimension7': '[[User ID]]',
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[sucesso ou tipo-erro]]		|  'sucesso, voce-nao-digitou-os-dados-corretamente' etc.   | Deve retornar o sucesso ou descrição do erro da tentativa de login.		        |
| [[Login Status]]		|  'Logged in', 'Logged out'  | Deve retornar o status do login do usuário.		        |
| [[User ID]]		|  '0123456' etc.   | Deve retornar o ID único do usuário definido após cadastro (MagentoID)		        |

<br />


## Cadastro

**No preenchimento dos campos do formulário de cadastro.**<br />

- **Onde:** Na página de cadastro.
```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:cadastro',
		'eventAction': 'preencheu-campo:cadastro',
		'eventLabel': '[[nome-campo]]'
	});
</script>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-campo]]		|  'nome-completo, e-mail, data-de-nascimento' e etc. | Deve retornar o nome do campo preenchido.|

<br />

**Ao clicar no botão continuar.**<br />

- **Onde:** Na página de cadastro;

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:cadastro" 
     data-gtm-event-action="clique:botao" 
     data-gtm-event-label="continuar"
>
Botão
</div>
```

<br />

**No sucesso ou erro da tentativa de cadastro.**<br />

- **Onde:** Na página de cadastro.

```html
<script>
	dataLayer.push({
		'event': 'event',
		'eventCategory': 'riachuelo:cadastro',
		'eventAction': 'enviar:cadastro',
		'eventLabel': '[[sucesso ou tipo-erro]]'
	});
</script>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[sucesso ou tipo-erro]] | 'sucesso' ou 'dados-invalidos' | Deve retornar o status do envio.  |

<br />

**No clique dos links da página de formulário**<br />

- **Onde:** Na página de cadastro;

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="riachuelo:cadastro" 
     data-gtm-event-action="clique:link" 
     data-gtm-event-label="[[nome-link]]"
>
Botão
</div>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[nome-link]]		| 'politica-de-privacidade, nao-sei-meu-cep, termos-e-condicoes-de-uso'.| Deve retornar o link clicado. |

<br />

**Ao efetivar as alterações cadastrais em "Informações da conta"**<br />

- **Onde:** Na página de "Minha conta" (painel do cliente)

```html
<script>
	dataLayer.push({
		'event': 'r_alteracao_dados_cadastrais',
		'eventCategory': 'riachuelo:minha-conta',
		'eventAction': 'clique:editar:informacoes-da-conta',
		'eventLabel': 'sucesso'
	});
</script>
```

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
