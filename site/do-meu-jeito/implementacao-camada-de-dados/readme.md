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

## Implementação da Camada de dados - Riachuelo - Do Meu Jeito
Última atualização: 02/06/2021 <br />
Em caso de dúvidas, entrar em contato com algum desses e-mails: 

[camila.adalgisa@riachuelo.com.br](mailto:camila.adalgisa@riachuelo.com.br) <br />
[guilherme.lacerda@riachuelo.com.br](mailto:guilherme.lacerda@riachuelo.com.br) <br />
[gustavo.pereira@riachuelo.com.br](mailto:gustavo.pereira@riachuelo.com.br) <br />

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [http://domeujeito.com.br/](http://domeujeito.com.br/).

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-T86MKXC');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-T86MKXC" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
  <!-- End Google Tag Manager (noscript) -->
  //...
  </body>
</html>
```

Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/quickstart)


## Observações
> Os valores especificados entre colchetes `[[ ]]` são variáveis dinâmicas e devem ser substituídas por seus respectivos valores.<br />

> Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais. <br />

> Caso o site já possua o Google Analytics instalado, será necessário a remoção do código de **todas as páginas do site**: <br />

```html

<script>
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o), m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)})(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

ga('create', 'UA-XXXXXXXX-X', 'auto');
ga('send', 'pageview');
</script>

```

---

```html

<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-XXXXXXX-X"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){window.dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-XXXXXXX-X');
</script>
```

---

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
  data-gtm-event-category='[[exemplo:valor-categoria]]'
  data-gtm-event-action='[[exemplo:valor-acao]]'
  data-gtm-event-label='[[exemplo:valor-rotulo]]'
 >
  Texto do elemento
</div>
```

#### Importante:
> Também devem ter os data-attributes `data-gtm-event-category`, `data-gtm-event-action` e `data-gtm-event-label`. Preenchidos conforme instruções específicas.

<br />

## Implementação

A documentação foi descrita para algumas áreas especificas do ambiente do site ou aplicativo.


### Especificações Globais:

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores; <br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais; <br />
Caso a informação solicitada não estiver disponivel retornar o valor com tipagem undefined.

---

### Dimensões Globais:

**Dimensões Customizadas para todas as páginas:**<br />
Deve ser disparado um push de dataLayer no momento de carregamento de todas as páginas do site (Considerar também todas as trocas de Path, quando o conteúdo da página é alterado mas a página não recarrega).<br />

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'dimension1': '[[userid]]',
    'dimension13': '[[quantidadepresentes]]',
    'dimension14': '[[tipolista]]',
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[userid]] | &quot;01234&quot; | ID único de usuário definido após o cadastro |
| [[quantidadepresentes]] | &quot;45-itens&quot;, etc | Deve retornar a quantidade de presentes que os noivos adicionaram na lista |
| [[tipolista]] | &quot;cha-de-bebe&quot;, &#039;lista-de-casamento&#039;, &#039;open-house&#039; e etc. | Deve retornar o tipo de lista selecionada |

---


### Geral 


**No clique dos itens do footer.**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='do-meu-jeito:geral'
   data-gtm-event-action='clique:footer'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;como-funciona&#039;, &#039;beneficios&#039;, &#039;quem-somos&#039; e etc. | Deve retornar o nome do item clicado. |

<br />


**No clique do ícone &quot;Acessar sacola&quot;**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='do-meu-jeito:geral'
   data-gtm-event-action='clique:icone'
   data-gtm-event-label='acessar-sacola'
>Botão</div>
```



<br />


**No callback de &quot;Sua conta foi atualizada&quot;**<br />

- **Onde:** Na página de &quot;Sua conta foi atualizada&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'do-meu-jeito:geral',
    'eventAction': 'callback:atualizacao-de-conta',
    'eventLabel': '[[atualizacao]]:conta-atualizada'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[atualizacao]] | &#039;senha&#039;, &#039;dados-cadastrais&#039;, &#039;cadastro-dos-noivos&#039; e etc | Deve retornar o que foi atualizado na conta. |

<br />

**No clique dos botões da pagina, após realizar uma busca.**<br />

- **Onde:** Em todas as páginas  de busca em que estiver disponível;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'do-meu-jeito:geral',
    'eventAction': 'buscar-lista:clique:botao',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;nova-busca&#039;, &#039;ver-lista&#039;, &#039;ver-mais-listas&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

**Na tentativa de callback para enviar o modal Nps.**<br />

- **Onde:** Em todas as páginas que estiver disponível;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'do-meu-jeito:nps',
    'eventAction': 'callback:enviar:modal',
    'eventLabel': '[[opcao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao]] | &#039;nao-gostei&#039;, &#039;gostei&#039;, &#039;sei-la&#039; e etc | Deve retornar o nome da opção clicada. |

<br />



### Home - Sem logar

**No clique dos itens do menu.**<br />

- **Onde:** Na página Home.
    
```html
<div
   data-gtm-event-category='dmj:deslogado:home'
   data-gtm-event-action='clique:menu'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;logo&#039;, &#039;perguntas-frequentes&#039;, &#039;como-funciona&#039;, &#039;beneficios&#039;, &#039;quem-somos&#039; e etc. | Deve retornar o nome do item clicado. |

<br />


**Na interação com o campo &quot;Buscar Lista&quot;.**<br />

- **Onde:** Na página Home.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:deslogado:home',
    'eventAction': 'interacao:campo',
    'eventLabel': 'preencheu:campo:buscar-lista'
  });
</script>
```




<br />


**No clique do Perfil.**<br />

- **Onde:** Na página Home.


```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:deslogado:home',
    'eventAction': 'clique:botao',
    'eventLabel': 'criar-ou-acessar-lista'
  });
</script>
```

<br />


**No clique dos botões.**<br />

- **Onde:** Na página Home.
    
```html
<div
   data-gtm-event-category='dmj:deslogado:home'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;criar-nova-lista&#039;, &#039;buscar-listas&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique dos botões em &quot;como funciona ou perguntas frequentes&quot;**<br />

- **Onde:** Na página Home, no menu &quot;Como funciona&quot;
    
```html
<div
   data-gtm-event-category='dmj:deslogado:home'
   data-gtm-event-action='clique:[[nome-menu]]'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-menu]] | &#039;como-funciona&#039; ou &#039;perguntas-frequentes&#039;. | Deve retornar o nome do menu. |
| [[nome-botao]] | &#039;noivos&#039;, &#039;convidados&#039;. | Deve retornar o nome do botão clicado. |

<br />


**No clique dos botão &quot;Criar nova lista&quot; de cada menu**<br />

- **Onde:** Na página Home, em todos os menus
    
```html
<div
   data-gtm-event-category='dmj:deslogado:home'
   data-gtm-event-action='clique:[[nome-pagina]]'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-pagina]] | &#039;como-funciona&#039;, &#039;beneficios&#039;, &#039;quem-somos&#039;, &#039;perguntas-frequentes&#039; ,  &#039;home&#039; e etc. | Deve retornar o nome da pagina. |
| [[nome-botao]] | &#039;criar-nova-lista&#039;, &#039;buscar-listas&#039;, etc | Deve retornar o nome do botão clicado. |

<br />


**Na interação para abrir ou fechar uma pergunta**<br />

- **Onde:** Na página Home, no menu Perguntas Frequentes
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:deslogado:home',
    'eventAction': 'interacao:perguntas-frequentes',
    'eventLabel': '[[nome-pergunta]]:[[acao]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-pergunta]] | &#039;o-que-e-do-meu-jeito-com-br&#039; e etc. | Deve retornar o nome da pergunta. |
| [[acao]] | &#039;abrir&#039; ou &#039;fechar&#039;. | Deve retornar a ação do usuário. |

<br />


### Login

**Na interação com os campos &quot;Acesse sua lista&quot;.**<br />

- **Onde:** Na página de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:login',
    'eventAction': 'interacao:acesse-sua-lista',
    'eventLabel': 'preencheu:campo:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;email&#039;, &#039;senha&#039;. | Deve retornar o nome do campo preenchido. |

<br />


**Na interação com o checkbox &quot;manter conectado&quot;.**<br />

- **Onde:** Na página de login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:login',
    'eventAction': 'interacao:acesse-sua-lista',
    'eventLabel': 'checkbox:manter-conectado'
  });
</script>
```




<br />


**No clique do botão &quot;entrar&quot;.**<br />

- **Onde:** Na página de login.
    
```html
<div
   data-gtm-event-category='dmj:login'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='entrar'
>Botão</div>
```



<br />


**No clique do links.**<br />

- **Onde:** Na página de login.
    
```html
<div
   data-gtm-event-category='dmj:login'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='[[nome-link]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;esqueceu-sua-senha&#039; ou &#039;quer-criar-uma-lista&#039;. | Deve retornar o nome do link clicado. |

<br />


**No preenchimento do campo e-mail, após clicar em &quot;esqueceu sua senha&quot;.**<br />

- **Onde:** Na página &quot;Esqueceu sua senha?&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:login:esqueceu-sua-senha',
    'eventAction': 'preencheu:campo',
    'eventLabel': 'email'
  });
</script>
```




<br />


**No clique do botão &quot;OK&quot;, após clicar em esqueceu sua senha.**<br />

- **Onde:** Na página de &quot;Esqueceu sua senha?&quot;
    
```html
<div
   data-gtm-event-category='dmj:login:esqueceu-sua-senha'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='ok'
>Botão</div>
```



<br />


**No callback para checar a caixa de entrada do e-mail**<br />

- **Onde:** Na página de &quot;Crie ou acesse a lista do seu evento&quot;
    
```html
<div
   data-gtm-event-category='dmj:login:esqueceu-sua-senha'
   data-gtm-event-action='callback:checar-caixa-de-entrada'
   data-gtm-event-label='[[instrucao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[instrucao]] | &#039;voce-devera-receber-um-email-em-breve-com-mais-instrucoes&#039; e etc | Deve retornar a instrução recebida |

<br />


**No clique do botão &quot;OK&quot;, após ter recebido o link via e-mail para atualização de senha**<br />

- **Onde:** Na página de &quot;Atualização de Senha&quot;
    
```html
<div
   data-gtm-event-category='dmj:login:atualizacao-de-senha'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='ok'
>Botão</div>
```



<br />


**No callback da tentiva de login.**<br />

- **Onde:** Na página Login.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'login',
    'eventCategory': 'dmj:login',
    'eventAction': 'envio:callback',
    'eventLabel': '[[sucesso ou tipo-de-erro]]',
    'dimension1': '[[userid]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;nao-foi-possivel&#039; e etc. | Deve retornar a mensagem de sucesso ou o tipo de erro. |
| [[userid]] | &quot;01234&quot; | ID único de usuário definido após o cadastro |

<br />


### Cadastro Lista

**Na escolha do tipo da sua lista.**<br />

- **Onde:** Na página de cadastro.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:lista:cadastro',
    'eventAction': 'clique:botao',
    'eventLabel': '[[nome-do-botao]]:[[tipo-lista-selecionada]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-do-botao]] | &#039;continuar&#039; | Deve retornar o nome do botão clicado. 
| [[tipo-lista-selecionada]] | &#039;cha-de-bebe&#039;, &#039;open-house&#039;, &#039;lista-de-casamento&#039; e etc. | Deve retornar qual o tipo da lista selecionada. |

<br />


**Na interação com os campos &quot;Criar Lista&quot;.**<br />

- **Onde:** Na página de cadastro.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:cadastro',
    'eventAction': 'interacao:criar-lista',
    'eventLabel': 'preencheu:campo:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome&#039;, &#039;nome-do-seu-amor&#039;, &#039;cpf&#039; e etc. | Deve retornar o nome do campo preenchido. |

<br />


**Ao selecionar a data do casamento**<br />

- **Onde:** Na página de cadastro.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:cadastro',
    'eventAction': 'interacao:criar-lista',
    'eventLabel': 'selecionou:data-de-casamento:[[valor]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor]] | &#039;01-janeiro-2020&#039; e etc. | Deve retornar a data escolhida. |

<br />


**No clique do link &quot;termos de uso&quot;**<br />

- **Onde:** Na página de cadastro.
    
```html
<div
   data-gtm-event-category='dmj:noivos:cadastro'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='termos-de-uso'
>Botão</div>
```



<br />


**No clique do botão &quot;continuar&quot;**<br />

- **Onde:** Na página de cadastro.
    
```html
<div
   data-gtm-event-category='dmj:noivos:cadastro'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='continuar'
>Botão</div>
```



<br />


**No callback da tentiva de cadastro.**<br />

- **Onde:** Na página de cadastro.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'dmj:noivos:cadastro',
    'eventAction': 'envio:callback',
    'eventLabel': '[[sucesso ou tipo-de-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;nao-foi-possivel&#039; e etc. | Deve retornar a mensagem de sucesso ou o tipo de erro. |

<br />


### Geral - Noivos - Aréa Logada

**No clique para editar o perfil, na lateral esquerda**<br />

- **Onde:** Em todas as páginas que estiver disponível
    
```html
<div
   data-gtm-event-category='dmj:noivos:geral-minha-conta'
   data-gtm-event-action='clique:perfil'
   data-gtm-event-label='editar'
>Botão</div>
```



<br />


**No clique do menu lateral**<br />

- **Onde:** Em todas as páginas que estiver disponível.
    
```html
<div
   data-gtm-event-category='dmj:noivos:geral-minha-conta'
   data-gtm-event-action='clique:menu'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;minha-conta&#039;, &#039;sair&#039;, &#039;adicionar-presentes-a-lista&#039; e etc. | Deve retornar o nome do item clicado. |

<br />


**No clique do botões em compartilhe sua lista, na lateral esquerda.**<br />

- **Onde:** Em todas as páginas que estiver disponível.


```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:geral-minha-conta',
    'eventAction': 'clique:compartilhe-sua-lista',
    'eventLabel': 'botao:[[nome-botao]]'
  });
</script>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;copiar-link-do-site&#039;, &#039;facebook&#039;, &#039;whatsapp&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />


### Minha Conta - Noivos - Aréa Logada

**Na interação com os campos do formulário.**<br />

- **Onde:** Na página minha conta.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:acesso:minha-conta',
    'eventAction': 'interacao:formularia',
    'eventLabel': 'preencheu:campo:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome-completo&#039;, &#039;nome-do-amor&#039;, &#039;email&#039;, &#039;cpf&#039; e etc. | Deve retornar o nome do campo preenchido. |

<br />


**No clique dos botões ou links do formulário.**<br />

- **Onde:** Na página minha conta.
    
```html
<div
   data-gtm-event-category='dmj:noivos:acesso:minha-conta'
   data-gtm-event-action='clique:formulario'
   data-gtm-event-label='[[botao ou link]]:[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao ou link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o nome do elemento clicado. |
| [[nome-item]] | &#039;salvar-edicao&#039;,  &#039;excluir-conta&#039;. | Deve retornar o nome do item clicado. |

<br />


**No clique dos botões do modal de exclusão de conta.**<br />

- **Onde:** Na página minha conta.
    
```html
<div
   data-gtm-event-category='dmj:noivos:acesso:minha-conta'
   data-gtm-event-action='clique:modal-excluir-conta'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;excluir&#039;, &#039;cancelar&#039;, &#039;fechar&#039;. | Deve retornar o nome do botão clicado. |

<br />


### Cadastro - Noivos - Área Logada

**No clique do link &quot;Clique aqui para continuar&quot;**<br />

- **Onde:** Ao fim do cadastro dos noivos, após receber o e-mail de verificação
    
```html
<div
   data-gtm-event-category='dmj:noivos:cadastro-noivos'
   data-gtm-event-action='clique:link'
   data-gtm-event-label='clique-aqui-para-continuar'
>Botão</div>
```



<br />


### Personalização - Noivos - Área Logada

**Ao enviar uma imagem para capa, ou selecionar uma existente.**<br />

- **Onde:** Na página de personalização.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:personalizacao',
    'eventAction': 'selecionar:imagem-capa',
    'eventLabel': '[[existente ou anexada]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[existente ou enviada]] | &#039;existente&#039; ou &#039;anexada&#039;. | Deve retornar que tipo de imagem foi utilizada. |

<br />


**Ao inserir uma imagem para noiva e noivo.**<br />

- **Onde:** Na página de personalização.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:personalizacao',
    'eventAction': 'inserir:foto',
    'eventLabel': '[[noivo ou noiva]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[noivo ou noiva]] | &#039;noiva&#039; ou &#039;noivo&#039;. | Deve retornar onde inseriu a foto. |

<br />


**Na seleção dos campos da seção &quot;Edite informações sobre seu casamento&quot;**<br />

- **Onde:** Na página de personalização.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:personalizacao',
    'eventAction': 'interacao:edite-informacoes',
    'eventLabel': 'selecionou:[[nome-filtro]]:[[valor-filtro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-filtro]] | &#039;data-do-casamento&#039; ou &#039;estado&#039;, &#039;cidade&#039;. | Deve retornar o nome do filtro. |
| [[valor-filtro]] | &#039;03-fevereiro-2020&#039;, &#039;sp&#039;, &#039;sao-paulo&#039; e etc. | Deve retornar o valor do filtro. |

<br />


**Na interação com &quot;Disponibilidade da lista&quot;**<br />

- **Onde:** Na página de personalização.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:personalizacao',
    'eventAction': 'interacao:disponibilidade-da-lista',
    'eventLabel': '[[visivel ou indisponivel]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[visivel ou indisponivel]] | &#039;visivel&#039; ou &#039;indisponivel&#039;. | Deve retornar a disponibilidade da lista. |

<br />


**Na interação com o campo &quot;personalize o endereço do seu site&quot;**<br />

- **Onde:** Na página de personalização.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:personalizacao',
    'eventAction': 'interacao:campo',
    'eventLabel': 'personalize-o-endereco-do-seu-site'
  });
</script>
```




<br />


**Na tentativa de callback para personalizar o endereço do seu site**<br />

- **Onde:** Na página de personalização.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:personalizacao',
    'eventAction': 'envio:callback-personalize-o-endereco',
    'eventLabel': '[[sucesso ou tipo-de-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso-url-disponivel&#039;, &#039;erro:url-ja-utilizada&#039; e etc | Deve retornar a mensagem de erro ou de sucesso. |

<br />


**No clique do botão &quot;Salvar Personalização&quot;**<br />

- **Onde:** Na página de personalização.
    
```html
<div
   data-gtm-event-category='dmj:noivos:personalizacao'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='salvar-personalizacao'
>Botão</div>
```



<br />


**No clique dos botões do lightbox**<br />

- **Onde:** Na página de personalização.
    
```html
<div
   data-gtm-event-category='dmj:noivos:personalizacao'
   data-gtm-event-action='clique:lightbox'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;nao&#039;, &#039;sim&#039;, &#039;fechar&#039;. | Deve retornar o nome do botão clicado. |

<br />


###   Adicionar Presentes- Noivos - Aréa Logada

**No clique dos botões do lightbox.**<br />

- **Onde:** Na página Adicionar presentes à lista.
    
```html
<div
   data-gtm-event-category='dmj:noivos:adicionar-presentes'
   data-gtm-event-action='clique:lightbox'
   data-gtm-event-label='botao:[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;comecar-agora&#039; ou &#039;fechar&#039;. | Deve retornar o nome do botão clicado. |

<br />


**Ao clicar em buscar**<br />

- **Onde:** Na página Adicionar presentes à lista.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:adicionar-presentes',
    'eventAction': 'busca',
    'eventLabel': '[[nome-buscado]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-buscado]] | &#039;tapetes&#039; e etc. | Deve o nome buscado. |

<br />


**No callback de erro de busca**<br />

- **Onde:** Na página Adicionar presentes à lista.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'dmj:noivos:adicionar-presentes',
    'eventAction': 'envio:callback-busca',
    'eventLabel': '[[sucesso-erro]]:[[nome-buscado]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso-erro]] | &#039;sucesso&#039;, &#039;erro:ops-nao-encontramos-nenhum-resultado&#039; e etc | Deve retornar a mensagem de sucesso ou o tipo de erro. |
| [[nome-buscado]] | &#039;tapetes&#039; e etc. | Deve o nome buscado. |

<br />


**No retorno de erro ao excluir um dos itens da lista**<br />

- **Onde:** Na página Adicionar presentes à lista.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'dmj:noivos:adicionar-presentes',
    'eventAction': 'envio:callback-excluir-item',
    'eventLabel': '[[sucesso-erro]]',
    'dimension6': '[[product-cor]]',
    'dimension7': '[[product-estampa]]',
    'dimension8': '[[product-estilo]]',
    'dimension9': '[[product-tamanho]]',
    'dimension10': '[[product-skufilho]]',
    'dimension11': '[[product-subcategoria]]',
    'dimension12': '[[vendedor]]',
    'dimension13': '[[quantidadepresentes]]',
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso-erro]] | &#039;sucesso&#039;, &#039;erro:ops-algo-deu-errado&#039; e etc | Deve retornar a mensagem de sucesso ou o tipo de erro. |
| [[product-cor]] | &quot;rosa&quot;, &quot;azul&quot;, etc | Cor do produto |
| [[product-estampa]] | &quot;face-selva&quot;, &quot;floral&quot;, etc | Estampa do produto |
| [[product-estilo]] | &quot;feminino-delicado&quot;, &quot;casual-e-atemporal, etc | Estilo do produto |
| [[product-tamanho]] | p&#039;,&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; e etc | Tamanho do produto |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[product-subcategoria]] | 310090&#039; | Código da categoria GM  |
| [[vendedor]] | &quot;riachuelo&quot;, etc | Deve retornar o nome da loja ques esta vendendo o produto |
| [[quantidadepresentes]] | &quot;45-itens&quot;, etc | Deve retornar a quantidade de presentes que os noivos adicionaram na lista |

<br />


**Após clicar nos botões de &quot;Limpar Filtro&quot; ou &quot;Aplicar&quot; depois de interagir com os filtros.**<br />

- **Onde:** Na página Adicionar presentes à lista.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:adicionar-presentes',
    'eventAction': 'clique:filtro:[[nome-botao]]',
    'eventLabel': '[[nome-fitro]]:[[categoria]]:[[valor-filtro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;, &#039;aplicar-filtro&#039;, &#039;limpar-filtro&#039; . | Deve retornar o nome do botão clicado. |
| [[nome-fitro]] | &#039;departamento&#039;, &#039;filtrar-ordenar&#039;, &#039;filtrar-categorias&#039; e etc | Deve retornar o nome do filtro. |
| [[categoria]] | &#039;cama&#039;, &#039;banho&#039; e etc | Deve retornar a categoria do filtro. |
| [[valor-filtro]] | &#039;menor-preco&#039;, &#039;maior-preco&#039; e etc.  | Deve retornar o valor do filtro selecionado. |

<br />


### Minha Lista de Presentes - Noivos - Aréa Logada

**Ao selecionar a opção de habiliar e desabilitar a lista**<br />

- **Onde:** Na página Minha lista de presentes.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:minha-lista',
    'eventAction': 'interacao',
    'eventLabel': '[[habilitar ou desabilitar]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[habilitar ou desabilitar]] | &#039;habilitar&#039; ou &#039;desabilitar&#039;. | Deve retornar a opção selecionada. |

<br />


**No clique dos botões.**<br />

- **Onde:** Na página Minha lista de presentes.
    
```html
<div
   data-gtm-event-category='dmj:noivos:minha-lista'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='[[nome-botao]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;esvaziar-lista&#039;, &#039;adicionar-mais-presentes&#039;. | Deve retornar o nome do botão clicado. |

<br />


**Na interação com o filtro &quot;Filtrar/ordenar&quot;.**<br />

- **Onde:** Na página Minha lista de presentes.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:minha-lista',
    'eventAction': 'interacao:filtro',
    'eventLabel': 'filtrar-ordenar:[[categoria]]:[[valor-filtro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[categoria]] | &#039;cama&#039;, &#039;banho&#039; e etc | Deve retornar a categoria do filtro. |
| [[valor-filtro]] | &#039;cama&#039;, &#039;toaha-de-rosto&#039;, &#039;ordenar&#039;, &#039;menor-preco&#039; e etc. Obs: se tiver mais de um valor de filtro aplicado, separar por virgula. | Deve retornar o valor do filtro selecionado. |

<br />


**No clique do botão &quot;Excluir da lista&quot; de cada presente**<br />

- **Onde:** Na página Minha lista de presentes.
    
```html
<div
   data-gtm-event-category='dmj:noivos:minha-lista'
   data-gtm-event-action='clique:botao'
   data-gtm-event-label='excluir-da-lista:[[nome-produto]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;conjunto-de-lorem-ipsum-dolor&#039; e etc. | Deve retornar o nome do produto. |

<br />


**No callback de exclusão de produto**<br />

- **Onde:** Na página Minha lista de presentes.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'dmj:noivos:minha-lista',
    'eventAction': 'envio:callback-excluir-produto',
    'eventLabel': '[[sucesso ou tipo-de-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sua-lista-de-presentes-esta-vazia&#039;, &#039;nao-foi-possivel-excluir&#039; e etc. | Deve retornar a mensagem de erro ou de sucesso. |

<br />


**No clique dos botão &quot;Adicionar presentes&quot; , para quando a lista de presentes esta vazia.**<br />

- **Onde:** Na página Minha lista de presentes.
    
```html
<div
   data-gtm-event-category='dmj:noivos:minha-lista'
   data-gtm-event-action='clique:lista-vazia'
   data-gtm-event-label='botao:adicionar-presentes-agora'
>Botão</div>
```



<br />


### Créditos e Mensagens - Noivos - Aréa Logada

**No clique do botão &quot;Ler mensagem&quot;**<br />

- **Onde:** Na página Créditos e Mensagens
   

```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:creditos-e-mensagens',
    'eventAction': 'clique:botao',
    'eventLabel': 'ler-mensagem:[[sku-filho]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sku-filho]] | &#039;13411195001&#039; e etc. | Deve retornar o sku do item clicado |

<br />


**Na interação para visualizar cartão presente**<br />

- **Onde:** Na página Créditos e Mensagens
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:noivos:creditos-e-mensagens',
    'eventAction': 'interacao:opcao:visualizar',
    'eventLabel': 'cartao-presente'
  });
</script>
```




<br />


**No clique do links ou botão**<br />

- **Onde:** Ná pagina de &quot;Créditos e Mensagens&quot;
    
```html
<div
   data-gtm-event-category='dmj:noivos:creditos-e-mensagens'
   data-gtm-event-action='clique:[[elemento]]'
   data-gtm-event-label='[[item-clicado]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[elemento]] | &#039;link&#039; ou &#039;botao&#039; | Deve retornar o elemento clicado. |
| [[item-clicado]] |  | Deve retornar o nome do item clicado. &#039;adicionar-presentes-a-lista&#039;, &#039;clique-e-conheca&#039; ou &#039;adicionar-presentes-agora&#039; |

<br />


### Cadastro de Convidados

**Na interação com os campos de cadastro.**<br />

- **Onde:** Na página de indetificação.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:convidados:cadastro',
    'eventAction': 'interacao:indentificacao',
    'eventLabel': 'preencheu:campo:[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome-completo&#039;, &#039;rg&#039;, &#039;cpf&#039; e etc. | Deve retornar o nome do campo preenchido. |

<br />


**No clique do botão ou link do cadastro.**<br />

- **Onde:** Na página de indetificação.
    
```html
<div
   data-gtm-event-category='dmj:convidados:cadastro'
   data-gtm-event-action='clique:[[botao ou link]]'
   data-gtm-event-label='[[nome-item]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao ou link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o nome do elemento clicado. |
| [[nome-item]] | &#039;continuar&#039; , &#039;nao-sei-meu-cep&#039; e etc. | Deve retornar o nome do item clicado. |

<br />


**No callback da tentiva de cadastro**<br />

- **Onde:** Na página de indetificação.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'dmj:convidados:cadastro',
    'eventAction': 'envio:callback',
    'eventLabel': '[[sucesso ou tipo-de-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso ou tipo-de-erro]] | &#039;sucesso&#039;, &#039;erro:cpf-invalido&#039; e etc. | Deve retornar a mensagem de erro ou de sucesso. |

<br />


### Convidados

**Na interação com o campo busca**<br />

- **Onde:** Na página convidados
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:convidados',
    'eventAction': 'interacao:campo',
    'eventLabel': '[[nome-campo]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;busca-por-nomes&#039;, &#039;data&#039; | Deve retornar o nome do campo que utilizou a busca. |

<br />


**Após clicar no botão &quot;buscar lista&quot;.**<br />

- **Onde:** Na página convidados
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:convidados',
    'eventAction': 'clique:botao:buscar-lista',
    'eventLabel': 'preencheu:[[campos-preenchidos]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[campos-preenchidos]] | &#039;nomes:data-[[data-selecionada]]&#039;, &#039;data-[[data-selecionada]]&#039;, &#039;nomes&#039; e etc  | Deve retornar os campos preenchidos para fazer a pesquisa. |
| [[data-selecionada]] | &#039;11/05/2020&#039; | Deve retornar a data que o usuário selecionou. |

<br />


**No callback de sucesso ao buscar uma lista de presentes**<br />

- **Onde:** Na página convidados após o clique em &#039;buscar lista&#039;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'dmj:convidados',
    'eventAction': 'envio:callback-busca',
    'eventLabel': 'sucesso:[[quantidade-de-listas-encontradas]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[quantidade-de-listas-encontradas]] |  | Deve retornar a quantidade de listas de casamento encontradas após a busca. Ex.: &#039;20&#039;, &#039;15&#039;, etc |

<br />


**No callback de erro ao buscar uma lista de presentes**<br />

- **Onde:** Na página convidados após o clique em &#039;buscar lista&#039;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'dmj:convidados',
    'eventAction': 'envio:callback-busca',
    'eventLabel': '[[mensagem-erro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mensagem-erro]] | &#039;erro:nao-foi-possivel-localizar-a-lista&#039;, etc | Deve retornar o tipo de erro. |

<br />


**Após clicar nos botões de &quot;Limpar Filtro&quot; ou &quot;Aplicar&quot; depois de interagir com os filtros.**<br />

- **Onde:** Na página convidados
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:convidados',
    'eventAction': 'clique:filtro:[[nome-botao]]',
    'eventLabel': '[[nome-fitro]]:[[categoria]]:[[valor-filtro]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;, &#039;aplicar-filtro&#039;, &#039;limpar-filtro&#039; . | Deve retornar o nome do botão clicado. |
| [[nome-fitro]] | &#039;departamento&#039;, &#039;filtrar-ordenar&#039;, &#039;filtrar-categorias&#039; e etc | Deve retornar o nome do filtro. |
| [[categoria]] | &#039;cama&#039;, &#039;banho&#039; e etc | Deve retornar a categoria do filtro. |
| [[valor-filtro]] | &#039;cama&#039;, &#039;toaha-de-rosto&#039;, &#039;ordenar&#039;, &#039;menor-preco&#039; e etc.  | Deve retornar o valor do filtro selecionado. |

<br />


**Na interação com o filtro de quantidade**<br />

- **Onde:** No carrinho, na primeira etapa &quot;Sacola de presentes&quot;
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:convidados',
    'eventAction': 'interacao:sacola-de-presentes',
    'eventLabel': 'filtro:quantidade:[[valor-filtro]]:[[nome-produto]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;conjunto-lorem&#039; e etc. | Deve retornar o nome do produto. |

<br />


**No clique do botão &quot;Comprar mais presentes&quot;**<br />

- **Onde:** No carrinho, na primeira etapa &quot;Sacola de presentes&quot;
    
```html
<div
   data-gtm-event-category='dmj:convidados'
   data-gtm-event-action='clique:sacola-de-presentes'
   data-gtm-event-label='botao:comprar-mais-presentes'
>Botão</div>
```



<br />


### Checkout

**No preenchimento dos campos das etapas do checkout**<br />

- **Onde:** Na página de checkout.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:convidados:checkout',
    'eventAction': 'interacao:campo',
    'eventLabel': '[[nome-campo]]:[[step]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;nome-completo&#039;, &#039;email&#039;, &#039;mensagem-para-os-noivos&#039; e etc | Deve retornar o nome do campo preenchido. |
| [[step]] | &#039;identificacao&#039;, &#039;pagamento&#039; e etc | Deve retornar o nome da etapa do checkout. |

<br />


**No clique do botões ou links nas etapas do checkout**<br />

- **Onde:** Na página de checkout.
    
```html
<div
   data-gtm-event-category='dmj:convidados:checkout'
   data-gtm-event-action='clique:[[botao ou link]]'
   data-gtm-event-label='[[nome-item]]:[[step]]'
>Botão</div>
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao ou link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o tipo de elemento clicado. |
| [[nome-item]] | &#039;continuar&#039;, &#039;continuar-comprando&#039;, &#039;finalizar-compra&#039; e etc | Deve retornar o nome do item clicado. |
| [[step]] | &#039;identificacao&#039;, &#039;pagamento&#039; e etc | Deve retornar o nome da etapa do checkout. |

<br />


**Na interação com o checkbox na etapa pagamento**<br />

- **Onde:** Na página de checkout.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:convidados:checkout',
    'eventAction': 'interacao:checkbox',
    'eventLabel': 'confirmar-presenca:pagamento'
  });
</script>
```




<br />


**Ao selecionar uma opção de pagamento**<br />

- **Onde:** Na página de checkout.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'event',
    'eventCategory': 'dmj:convidados:checkout',
    'eventAction': 'selecionar',
    'eventLabel': 'opcao-de-pagamento:[[nome]]'
  });
</script>
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome]] | &#039;cartao-de-credito&#039; e etc. | Deve retornar o nome da opção de pagamento. |

<br />


### Enhanced E-commerce

**Ao adicionar um produto da lista de presentes ou no filtro de quantidade do carrinho**<br />

- **Onde:** Na página de lista de presentes para comprar um dos itens ou na página sacola
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'addToCart',
  'eventCategory':'dmj:enhanced-ecommerce',
  'eventAction': 'addToCart',
  'dimension12': '[[vendedor]]',
  'dimension13': '[[quantidadepresentes]]',
  'ecommerce': {
    'add': {
      'products': [{
        'dimension6': '[[product-cor]]',
        'dimension7': '[[product-estampa]]',
        'dimension8': '[[product-estilo]]',
        'dimension9': '[[product-tamanho]]',
        'dimension10': '[[product-skufilho]]',
        'dimension11': '[[product-subcategoria]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &quot;cafeteira&quot; | Nome do produto |
| [[id-produto]] | &quot;i17mcjf106-771-2&quot; | SKU do produto |
| [[preco-produto]] | &quot;139,99&quot; | Preço do produto |
| [[categoria-produto]] | &quot;casa&quot;, &quot;moda&quot;, &quot;eletronicos&quot; e etc | Categoria do produto (departamento onde o produto aparece) |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |
| [[marca-produto]] | &quot;nespresso&quot; | Marca do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |
| [[product-cor]] | &quot;rosa&quot;, &quot;azul&quot;, etc | Cor do produto |
| [[product-estampa]] | &quot;face-selva&quot;, &quot;floral&quot;, etc | Estampa do produto |
| [[product-estilo]] | &quot;feminino-delicado&quot;, &quot;casual-e-atemporal, etc | Estilo do produto |
| [[product-tamanho]] | p&#039;,&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; e etc | Tamanho do produto |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[product-subcategoria]] | 310090&#039; | Código da categoria GM  |
| [[vendedor]] | &quot;riachuelo&quot;, etc | Deve retornar o nome da loja ques esta vendendo o produto |
| [[quantidadepresentes]] | &quot;45-itens&quot;, etc | Deve retornar a quantidade de presentes que os noivos adicionaram na lista |

<br />


**Ao remover um produto da lista ou diminuir a quantidade deste no filtro de quantidade**<br />

- **Onde:** Na página de sacola
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'removeFromCart',
  'eventCategory': 'dmj:enhanced-ecommerce',
  'eventAction': 'removeFromCart',
  'ecommerce': {
    'remove': {
      'products': [{
        'dimension6': '[[product-cor]]',
        'dimension7': '[[product-estampa]]',
        'dimension8': '[[product-estilo]]',
        'dimension9': '[[product-tamanho]]',
        'dimension10': '[[product-skufilho]]',
        'dimension11': '[[product-subcategoria]]',
        'dimension12': '[[vendedor]]',
        'dimension13': '[[quantidadepresentes]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &quot;cafeteira&quot; | Nome do produto |
| [[id-produto]] | &quot;i17mcjf106-771-2&quot; | SKU do produto |
| [[preco-produto]] | &quot;139,99&quot; | Preço do produto |
| [[categoria-produto]] | &quot;casa&quot;, &quot;moda&quot;, &quot;eletronicos&quot; e etc | Categoria do produto (departamento onde o produto aparece) |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |
| [[marca-produto]] | &quot;nespresso&quot; | Marca do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |
| [[product-cor]] | &quot;rosa&quot;, &quot;azul&quot;, etc | Cor do produto |
| [[product-estampa]] | &quot;face-selva&quot;, &quot;floral&quot;, etc | Estampa do produto |
| [[product-estilo]] | &quot;feminino-delicado&quot;, &quot;casual-e-atemporal, etc | Estilo do produto |
| [[product-tamanho]] | p&#039;,&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; e etc | Tamanho do produto |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[product-subcategoria]] | 310090&#039; | Código da categoria GM  |
| [[vendedor]] | &quot;riachuelo&quot;, etc | Deve retornar o nome da loja ques esta vendendo o produto |
| [[quantidadepresentes]] | &quot;45-itens&quot;, etc | Deve retornar a quantidade de presentes que os noivos adicionaram na lista |

<br />


**Nas etapas de checkout**<br />

- **Onde:** Nas páginas de checkout
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'checkout',
  'eventCategory': 'dmj:enhanced-ecommerce',
  'eventAction': 'checkout',
  'noInteraction': '1',
  'ecommerce': {
    'checkout': {
      'actionField': {'step': '[[passo-checkout]]'},
      'products': [{
        'dimension6': '[[product-cor]]',
        'dimension7': '[[product-estampa]]',
        'dimension8': '[[product-estilo]]',
        'dimension9': '[[product-tamanho]]',
        'dimension10': '[[product-skufilho]]',
        'dimension11': '[[product-subcategoria]]',
        'dimension12': '[[vendedor]]',
        'dimension13': '[[quantidadepresentes]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[checkout-index]] |  | Retornar &quot;1&quot; ou &quot;2&quot; ou &quot;3&quot; de acordo com a página que o usuário está.  |
| [[nome-produto]] | &quot;cafeteira&quot; | Nome do produto |
| [[id-produto]] | &quot;i17mcjf106-771-2&quot; | SKU do produto |
| [[preco-produto]] | &quot;139,99&quot; | Preço do produto |
| [[categoria-produto]] | &quot;casa&quot;, &quot;moda&quot;, &quot;eletronicos&quot; e etc | Categoria do produto (departamento onde o produto aparece) |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |
| [[marca-produto]] | &quot;nespresso&quot; | Marca do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |
| [[product-cor]] | &quot;rosa&quot;, &quot;azul&quot;, etc | Cor do produto |
| [[product-estampa]] | &quot;face-selva&quot;, &quot;floral&quot;, etc | Estampa do produto |
| [[product-estilo]] | &quot;feminino-delicado&quot;, &quot;casual-e-atemporal, etc | Estilo do produto |
| [[product-tamanho]] | p&#039;,&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; e etc | Tamanho do produto |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[product-subcategoria]] | 310090&#039; | Código da categoria GM  |
| [[vendedor]] | &quot;riachuelo&quot;, etc | Deve retornar o nome da loja ques esta vendendo o produto |
| [[quantidadepresentes]] | &quot;45-itens&quot;, etc | Deve retornar a quantidade de presentes que os noivos adicionaram na lista |
| [[passo-checkout]] | &quot;1&quot; | Página de carrinho de compras |
| [[passo-checkout]] | &quot;2&quot; | Página de confirmação do endereço de entrega |
| [[passo-checkout]] | &quot;3&quot; | Página de seleção do método de pagamento |

<br />


**No carregamento da página de confirmação de pedido**<br />

- **Onde:** Na página de pedido
    

```html
<script>
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': 'purchase',
  'eventCategory': 'dmj:enhanced-ecommerce',
  'eventAction': 'purchase',
  'noInteraction': '1',
  'ecommerce': {
    'purchase': {
      'actionField': {
        'id': '[[id-transacao]]',
        'revenue': '[[valor-total-transacao]]',
        'tax': '[[taxa-transacao]]'
      },
      'products': [{
        'dimension1': '[[userid]]',
        'dimension4': '[[transaction-pagamento]]',
        'dimension5': '[[transaction-parcelas]]',
        'dimension6': '[[product-cor]]',
        'dimension7': '[[product-estampa]]',
        'dimension8': '[[product-estilo]]',
        'dimension9': '[[product-tamanho]]',
        'dimension10': '[[product-skufilho]]',
        'dimension11': '[[product-subcategoria]]',
        'dimension12': '[[vendedor]]',
        'dimension13': '[[quantidadepresentes]]',
        'name': '[[nome-produto]]',
        'id': '[[id-produto]]',
        'price': '[[preco-produto]]',
        'brand': '[[marca-produto]]',
        'category': '[[categoria-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[userid]] | &quot;01234&quot; | ID único de usuário definido após o cadastro |
| [[transaction-pagamento]] | &quot;a-vista&quot;, &quot;cartao-de-credito&quot;, &quot;debito&quot; | Meio de pagamento |
| [[transaction-parcelas]] | &quot;1x&quot;, &quot;4x&quot;, &quot;12x&quot; | Número de parcelas |
| [[product-cor]] | &quot;rosa&quot;, &quot;azul&quot;, etc | Cor do produto |
| [[product-estampa]] | &quot;face-selva&quot;, &quot;floral&quot;, etc | Estampa do produto |
| [[product-estilo]] | &quot;feminino-delicado&quot;, &quot;casual-e-atemporal, etc | Estilo do produto |
| [[product-tamanho]] | p&#039;,&#039;m&#039;,&#039;8-12&#039;,&#039;42&#039; e etc | Tamanho do produto |
| [[product-skufilho]] |  &#039;2552&#039; | ID filho do produto |
| [[product-subcategoria]] | 310090&#039; | Código da categoria GM  |
| [[vendedor]] | &quot;riachuelo&quot;, etc | Deve retornar o nome da loja ques esta vendendo o produto |
| [[quantidadepresentes]] | &quot;45-itens&quot;, etc | Deve retornar a quantidade de presentes que os noivos adicionaram na lista |
| [[id-transacao]] | &quot;000011652&quot; | Número do pedido |
| [[valor-total-transacao]] | &quot;139,99&quot; | Valor total da transação incluindo frete e taxas |
| [[taxa-transacao]] | &quot;0.00&quot; | Valor de taxas da transação |
| [[nome-produto]] | &quot;cafeteira&quot; | Nome do produto |
| [[id-produto]] | &quot;i17mcjf106-771-2&quot; | SKU do produto |
| [[preco-produto]] | &quot;139,99&quot; | Preço do produto |
| [[categoria-produto]] | &quot;casa&quot;, &quot;moda&quot;, &quot;eletronicos&quot; e etc | Categoria do produto (departamento onde o produto aparece) |
| [[variacao-produto]] | &quot;325&quot; | Código DCO - Categoria do produto |
| [[marca-produto]] | &quot;nespresso&quot; | Marca do produto |
| [[quantidade-produto]] | &quot;1&quot; | Quantidade do produto |

<br />


<br />

---


## Considerações Finais


> Documentações Oficiais do Google: <br />
> [Google Analytics - Avaliação de comércio eletrônico ](https://developers.google.com/analytics/devguides/collection/analyticsjs/ecommerce?hl=pt-br) <br />
> [Google Tag Manager - Guia do desenvolvedor ](https://developers.google.com/tag-manager/enhanced-ecommerce))

<script>
  document.addEventListener('DOMContentLoaded', function(event) {
    document.querySelectorAll('h1 a')[0].style.display = 'none';
  });
</script>
