![Zoly](https://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics <br />
> Documento de Especificação Técnica


<br />

## Implementação de Tags Firebase - Riachuelo - APP Simplifica - React Native

Última atualização: 02/06/2021 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)


## Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Definições Globais](#defini%c3%a7%c3%b5es-globais)
- [Eventos Globais](#eventos-globais)
- [Login](#login)
- [Seja Bem Vindo](#seja-bem-vindo)
- [Home](#home)
- [Notificações](#notifica&#231;&#245;es)
- [Perfil](#perfil)
- [Reembolso](#reembolso)
- [NPS](#nps)
- [Holerite](#holerite)
- [Fluxo Covid 19](#fluxo-covid-19)
- [FAQ RH](#faq-rh)
- [Considerações Finais](#considera%c3%a7%c3%b5es-finais)


## Objetivo

Este documento tem como objetivo instruir a implementação de Tags de Firebase Analytics para o aplicativo de Riachuelo - APP Simplifica.


## Implementação

> Os links abaixo descrevem como realizar a implementação do Firebase em seu aplicativo.

[Link - Adicionar o Firebase ao projeto do Android](https://rnfirebase.io/docs/master/installation/android).
[Link - Adicionar o Firebase ao projeto do iOS](https://rnfirebase.io/docs/master/installation/ios).

<br />

## Especificações Globais

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[ ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores;<br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais;<br />
Caso a informação solicitada não estiver disponível retornar tipagem `undefined`.

---

### Definições Globais

**Definições Customizadas para todas as telas:**<br />
Quando ocorrer as trocas de telas o código abaixo deve ser definido para que os hits sejam enviados ao Firebase Analytics.<br />


```javascript
Analytics.setUserId('[[UserID]]');

```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| userID | &quot;01234&quot; | A matrícula do funcionário Riachuelo |


### Eventos Globais

- **Quando:** Na abertura de modal de erro.
- **Onde:** Em todas as telas em que esta opção estiver disponível.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:geral",
        	eventAction: "clique:footer" ,
        	eventLabel: "[[nome-item]]"
        
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;inicio&#039;, &#039;notificacoes&#039;, &#039;perfil&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />


### Login 

- **Onde:** Na visualização da tela de login


```javascript
    Analytics.logScreenView("/login");
```
    

- **Quando:** No preenchimento dos campos
- **Onde:** Na tela de login e recuperar senha

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:[[tela]]",
        	eventAction: "preencheu:campo" ,
        	eventLabel: "[[nome-campo]]:[[etapa]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tela]] | &#039;login&#039;, &#039;recuperar-senha&#039; e etc. | Deve retornar o nome da tela preenchida. |
| [[nome-campo]] | &#039;matricula&#039;, &#039;senha&#039;,matricula&#039;, &#039;cpf&#039;,&#039;data-de-nascimento&#039;,&#039;celular&#039;, &#039;nova-senha&#039;, &#039;confirme-nova-senha&#039;. | Deve retornar se o campo foi preenchido. |
| [[etapa]] | &#039;redefinir-senha&#039;, &#039;celular&#039;, &#039;celular-codigo&#039;, &#039;nova-senha&#039;, &#039;login&#039;. | Deve retornar a etapa de redefinição de senha que o usuário está. |

<br />



- **Quando:** No callback de sucesso/erro ao efetuar o login, redefinir senha, validação do código de celular
- **Onde:** Na tela de login e redefinir senha

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:[[tela]]",
        	eventAction: "callback:[[etapa]]" ,
        	eventLabel: "[[sucesso-erro]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[etapa]] | &#039;redefinir-senha&#039;, &#039;celular&#039;, &#039;celular-codigo&#039;, &#039;nova-senha&#039;, &#039;login&#039;. | Deve retornar a etapa de redefinição de senha que o usuário está. |
| [[sucesso-erro]] | &#039;sucesso&#039;, &#039;erro:acesso-invalido&#039;, &#039;erro:celular-invalido&#039; | Deve retornar se ocorreu sucesso ou erro na solicitação. |
| [[tela]] | &#039;login&#039;, &#039;recuperar-senha&#039; e etc. | Deve retornar o nome da tela preenchida. |

<br />


- **Quando:** No clique em todos os botões e links
- **Onde:** Na tela de login e recuperar senha


```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:[[tela]]",
        	eventAction: "clique:[[botao-link]]" ,
        	eventLabel: "[[nome-item]]:[[etapa]]" 
        })
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-link]] | &#039;botao&#039;, &#039;link&#039; e etc. | Deve retornar se o nome do item clicado é botão ou link. |
| [[etapa]] | &#039;redefinir-senha&#039;, &#039;celular&#039;, &#039;celular-codigo&#039;, &#039;nova-senha&#039;, &#039;login&#039;. | Deve retornar a etapa de redefinição de senha que o usuário está. |
| [[nome-item]] | visualizar-senha&#039;, &#039;ocultar-senha&#039;, &#039;acessar&#039;,&#039;continuar&#039;, &#039;enviar&#039;, &#039;voltar&#039;,&#039;reenviar&#039;, &#039;voltar-ao-login&#039;  | Deve retornar o nome do item clicado. |
| [[tela]] | &#039;login&#039;, &#039;recuperar-senha&#039; e etc. | Deve retornar o nome da tela preenchida. |

<br />


- **Onde:** Na visualização da tela de redefinição de senha


```javascript
    Analytics.logScreenView("/login/recuperar-senha/");
```

<br />

- **Na visualização da tela de redefinição de senha na etapa do codigo do celular**

```javascript
    Analytics.logScreenView("/login/recuperar-senha/codigo/")
```

<br />


- **Na visualização da tela de criar &#039;nova senha&#039;**


```javascript
    Analytics.logScreenView("/login/recuperar-senha/nova-senha/")
```

<br />


### Seja Bem Vindo

- **Onde:** Na visualização da tela de &#039;Seja Bem Vindo&#039;


```javascript
    Analytics.logScreenView("/seja-bem-vindo");
```

<br />


- **Quando:** No clique do botão &#039;continuar&#039;
- **Onde:** Na tela de &#039;Seja Bem Vindo&#039;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:seja-bem-vindo",
        	eventAction: "clique:botao" ,
        	eventLabel: "continuar" 
        })
```


<br />

### Home 

- **Onde:** Na visualização da tela de &#039;Home&#039;


```javascript
    Analytics.logScreenView("/home");
```

<br />

- **Quando:** No clique em todos os botões e links
- **Onde:** Na tela &#039;Home&#039;

```javascript
        Analytics.logEvent("event",  {
        	eventCategory: "app-simplifica:home" ,
        	eventAction: "clique:[[botao-link]]" ,
        	eventLabel: "[[secao]]:[[nome-item]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar se o  item clicado é um &#039;botao&#039; ou &#039;link&#039; .  |
| [[secao]] | &#039;servicos&#039;, &#039;redes&#039; e etc.  | Deve retornar a seção do item clicado. |
| [[nome-item]] |  &#039;reembolso&#039; e etc. | Deve retornar o nome do item clicado. |

<br />

### Notificações

- **Onde:** Na visualização da tela de &#039;Notificações&#039;


```javascript
    Analytics.logScreenView("/notificacoes");
```


<br />

- **Quando:** No clique no botão voltar
- **Onde:** Na tela de &#039;Notificações&#039;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:notificacoes",
        	eventAction: "clique:botao" ,
        	eventLabel: "voltar" 
        })
```


<br />

- **Quando:** No clique nas notificações
- **Onde:** Na tela de &#039;Notificações&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:notificacoes" ,
        	eventAction: "clique:notificacao",
        	eventLabel: "[[nome-notificacao]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-notificacao]] | &#039;reembolso-aprovado&#039;, &#039;reembolso-rejeitado&#039; e etc. | Deve retornar o nome da notificação clicada. |

<br />

### Perfil

- **Onde:** Na visualização da tela de &#039;Perfil&#039;


```javascript
    Analytics.logScreenView("/perfil");
```


<br />

- **Quando:** No clique em todos os botões
- **Onde:** Na tela &#039;Perfil&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:perfil",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]" 
        })
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;sair-do-app&#039;, &#039;sair&#039;, &#039;cancelar&#039; , &#039;alterar-minha-senha&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />

### Reembolso

- **Onde:** Na visualização da tela de &#039;Reembolso&#039;


```javascript
    Analytics.logScreenView("/reembolso");
```

<br />

- **Quando:** No clique em todos os botões
- **Onde:** Na tela &#039;Reembolso&#039; , &#039;Novo Reembolso&#039;, &#039;Progresso Reembolso&#039;, &#039;Confirmação Novo Reembolso&#039;, &#039;Detalhes Reembolso&#039;

```javascript
        Analytics.logEvent("event", 
        	eventCategory: "app-simplifica:[[tela]]" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]" 
        )
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tela]] | &#039;reembolso&#039;, &#039;novo-reembolso&#039;, &#039;detalhes-reembolso&#039;, &#039;reembolso-em-progresso&#039;, &#039;confirmacao-novo-reembolso&#039; e etc | Deve retornar em qual tela o usuario esta. |
| [[nome-botao]] | &#039;voltar&#039;, &#039;filtrar-status&#039;, &#039;duvida&#039;, &#039;filtrar-data&#039;, &#039;novo-reembolso&#039;,  &#039;alterar&#039;, &#039;enviar&#039;, &#039;voltar&#039;, &#039;enviar&#039;, &#039;duvida&#039;,&#039;ok&#039;, &#039;cancelar&#039;  e etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique nos reembolsos
- **Onde:** Na tela &#039;Reembolso&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:reembolso" ,
        	eventAction: "clique" ,
        	eventLabel: "reembolso",
        	dimension1: "[[dimension1]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[dimension1]] | &quot;aprovado&quot;, &quot;processando&quot; e etc | Deve retornar o status do reembolso do usuario |

<br />

- **Quando:** Ao interagir com o  filtro de status
- **Onde:** Na tela &#039;Reembolso&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:reembolso" ,
        	eventAction: "filtrar:status",
        	eventLabel: "[[nome-status]]" 
            })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-status]] | &#039;aprovado&#039;, &#039;concluido&#039; e etc | Deve retornar o status selecionado. |

<br />

- **Quando:** Ao clicar no botão &quot;ok&quot; ou &quot;cancelar&quot; em selecionar data, caso o clique seja em &quot;Ok&quot; retornar a data selecionada
- **Onde:** Na tela &#039;Reembolso&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:reembolso" ,
        	eventAction: "filtrar:data" ,
        	eventLabel: "botao:[[nome-botao]]" ,
        	dimension2: "[[dimension2]]" 
        ])
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;ok&#039;, &#039;cancelar&#039; | Deve retornar o nome do botão clicado. |
| [[dimension2]] | &#039;02-02-2020-ate-10-02-2020&#039; e etc | Deve retornar o periodo de reembolso selecionado pelo usuario no filtro de data. Caso o Usuario clique em cancelar, a dimension2 retorna undefined |

<br />


- **Onde:** Na visualização da tela de &#039;Detalhes&#039; de reembolso


```javascript
    Analytics.logScreenView("/reembolso/detalhes");
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[dimension1]] | &quot;aprovado&quot;, &quot;processando&quot; e etc | Deve retornar o status do reembolso do usuario |

<br />

- **Quando:** No clique nos dropboxs
- **Onde:** Na tela &#039;Detalhes&#039; de reembolso

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:detalhes-reembolso" ,
        	eventAction: "clique:dropbox" ,
        	eventLabel: "[[nome-item]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;em-andamento&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Na visualização da tela de &#039;Novo Reembolso&#039;


```javascript
    Analytics.logScreenView("/reembolso/novo-reembolso");
```

<br />

- **Quando:** No preenchimento dos campos do formulário
- **Onde:** Na tela &#039;Novo Reembolso&#039;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:novo-reembolso" ,
        	eventAction: "preencheu:campo" ,
        	eventLabel: "[[nome-campo]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;motivo-da-nota-fiscal&#039;, &#039;data&#039; &#039;nota-fiscal&#039;, &#039;valor-da-nota-fiscal&#039; e etc. | Deve retornar se o campo foi preenchido. |

<br />

- **Quando:** Ao submeter a foto
- **Onde:** Na tela &#039;Novo Reembolso&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:novo-reembolso" ,
        	eventAction: "enviou:foto" ,
        	eventLabel: "nota-fiscal" 
            })
```


<br />


- **Onde:** Na visualização da tela de &#039;Confirmação de Solicitação de Novo Reembolso&#039;


```javascript
    Analytics.logScreenView("/reembolso/novo-reembolso/confirmacao");
```

<br />

- **Onde:** Na visualização da tela de &#039;Reembolso em progresso&#039;


```javascript
    Analytics.logScreenView("/reembolso/novo-reembolso/progresso");
```


<br />

- **Quando:** No callback de sucesso
- **Onde:** Na tela Sucesso de Novo Reembolso


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:novo-reembolso" ,
        	eventAction: "callback:novo-reembolso" ,
        	eventLabel: "[[status]]" 
            })
```





| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039;, &#039;erro:falha-no-sistema&#039;, &#039;erro:dado-invalido&#039; e etc | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />


- **Onde:** Na visualização da tela de &#039;Reembolso em progresso&#039;


```javascript
    Analytics.logScreenView("novo-reembolso/duvidas-[[tela]]");
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tela]] | &#039;1&#039;, &#039;2&#039; ou &#039;3&#039; | Deve retornar se o usuario esta visualizando a tela 1, 2 ou 3. |

<br />

- **Quando:** No clique do botão de fechar
- **Onde:** Na tela &#039;Duvidas&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:duvidas-novo-reembolso" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-do-botao]]:[[etapa]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;,&#039;proximo&#039;, &#039;anterior&#039; e etc | Deve retornar o nome do botão clicado. |
| [[etapa]] | &#039;1&#039;, &#039;2&#039;,&#039;3&#039;. | Qual etapa o usuário estava quando clicou em fechar. |

<br />

### NPS

- **Onde:** Na visualização da tela de &#039;NPS&#039;


```javascript
    Analytics.logScreenView("/nps");
```


<br />

- **Quando:** No clique do botão de &#039;fechar&#039; e &#039;enviar&#039;
- **Onde:** Na tela &#039;NPS&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:nps" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]" ,
        	dimension3: "[[dimension3]]",
                dimension4 : "[[dimension4]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[dimension3]] | &#039;odiei&#039;, &#039;adorei&#039; e etc | Deve retornar a avaliação do Usuario, se o usuario clicar em fechar retornar undefined |
| [[dimension4]] | &#039;achei-otima-experiencia&#039; e etc | Deve retornar a mensagem de avaliação do Usuario, Se o campo estiver vazio retornar &#039;default&#039;|

<br />


### Holerite

**Visualização das telas de  &quot;Holerite Quinzenal&quot; e &quot;Holerite Mensal&quot;**

```javascript
    Analytics.logScreenView("/holerite/[[periodo]]")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[periodo]] | &#039;mensal&#039;, &#039;quinzenal&#039; e etc | deve retornar se o usuario interagiu com itens do holerite mensal ou quinzenal. |

<br />

- **Quando:** No clique dos botões .
- **Onde:** Na tela de &quot;Holerite Quinzenal&quot; e &quot;Holerite Mensal&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:holerite-[[periodo]]",
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;compartilhar&#039;, &#039;voltar&#039;, &#039;quinzenal&#039;, &#039;mensal&#039;, &#039;compartilhar&#039; e etc | deve retornar o nome do botão clicado. |
| [[periodo]] | &#039;mensal&#039;, &#039;quinzenal&#039; e etc | deve retornar se o usuario interagiu com itens do holerite mensal ou quinzenal. |

<br />

- **Quando:** Ao selecionar o mês, no filtro da tela
- **Onde:** Na tela de &quot;Holerite Quinzenal&quot; e &quot;Holerite Mensal&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:holerite-[[periodo]]",
        	eventAction: "interacao:filtro" ,
        	eventLabel: "[[mes-selecionado]]" 
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes-selecionado]] | &#039;abril/2020&#039; , &#039;dez/2020&#039; e etc | deve retornar o mes selecionado. |
| [[periodo]] | &#039;mensal&#039;, &#039;quinzenal&#039; e etc | deve retornar se o usuario interagiu com itens do holerite mensal ou quinzenal. |

<br />

- **Quando:** Ao abrir e fechar as informações de &quot;salario&quot;, &quot;descontos&quot;
- **Onde:** Na tela de &quot;Holerite Quinzenal&quot; e &quot;Holerite Mensal&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:holerite-mensal",
        	eventAction: "ineracao:[[item]]",
        	eventLabel: "[[abriu-fechou]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[abriu-fechou]] | &quot;abriu&quot;, &quot;fechou&quot; | deve retornar se o usuario abriu ou fechou. |
| [[item]] | &#039;descontos&#039;, &#039;salario&#039; e etc | deve retornar o nome do item clicado. |

<br />

### Fluxo Covid 19


**Visualização da tela de &quot;Ola Covid19&quot; e &quot;Bom te-lo novamento Covid19&quot;**<br />

```javascript
    Analytics.logScreenView("/covid19/")
```


<br />

- **Quando:** No clique dos botões
- **Onde:** Na tela de &quot;Ola Covid19&quot; e &quot;Bom te-lo novamento Covid19&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:covid-19" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]" 
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;voltar&#039; e etc | deve retornar o nome do botão clicado. |

<br />

**Visualização da tela de &quot;Como podemos te ajudar covid19&quot;**

```javascript
    Analytics.logScreenView("/covid19/como-podemos-ajudar/")
```


<br />

- **Quando:** No clique dos botoes
- **Onde:** Na tela de &quot;Como podemos te ajudar covid19&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-simplifica:covid-19:ajuda",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;relatar-covid-19&#039;, &#039;estou-com-duvidas&#039; e etc | deve retornar o nome do botão clicado. |

<br />

**Visualização da tela de &quot;Contato&quot;**

```javascript
    Analytics.logScreenView("/covid19/contato")
```


<br />

- **Quando:** No clique dos botoes
- **Onde:** Na tela de &quot;Contato&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:covid-19:contato",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;continuar&#039; | deve retornar o nome do botão clicado. |

<br />

- **Quando:** No callback de sucesso ou erro ao fornecer o contato
- **Onde:** Na tela de &quot;Contato&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:covid-19:contato" ,
        	eventAction: "callback" ,
        	eventLabel: "[[sucesso-erro]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso-erro]] | &#039;sucesso&#039;, &#039;erro:numero-invalido&#039; e etc | deve retornar se o contato foi enviado com sucesso ou o tipo de erro. |

<br />

### FAQ RH 


**Visualização da tela de "Área"**

```javascript
    Analytics.logScreenView("/faq-rh/area")
```

<br />

- **Quando:** No clique dos filtros para escolha da área após a interação com ícone "FAQ RH".
- **Onde:** No fluxo de "FAQ RH" dentro de "Serviços". 
 
```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:faq-rh" ,
        	eventAction: "clique:filtro" ,
        	eventLabel: "[[nome-filtro]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-filtro]] | &#039;admissao&#039;, &#039;alteracao-e-selecao-de-talentos&#039;, &#039;beneficios&#039; e etc | Deve retornar o nome do filtro clicado pelo usuario.  |

<br />

**Visualização da tela de "Tema"**

```javascript
    Analytics.logScreenView("/faq-rh/tema")
```

<br />

- **Quando:** No clique dos botoes na interação com os temas após ter escolhido a "área"
- **Onde:** No fluxo de "faq RH".
 
```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:faq-rh" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;indicacao&#039;, &#039;carreira&#039; e etc | Deve retornar o nome do botão clicado.  |

<br />

**Visualização da tela de "Perguntas e Respostas"**

```javascript
    Analytics.logScreenView(" /faq-rh/perguntas-respostas")
```

<br />

- **Quando:** No clique do icone para "Abrir" e "Fechar" a interação com as perguntas.
- **Onde:** No fluxo de "FAQ RH".

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-simplifica:faq-rh" ,
        	eventAction: "clique:icone" ,
        	eventLabel: "[[nome-icone]]:[[pergunta]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pergunta]] | &#039;como-faco-para-me-candidatar-a-vagas-internas&#039; e &#039;como-uma-vaga-em-meu-time&#039; e etc | Deve retornar a pergunta selecionada.  |
| [[nome-icone]] | &#039;abrir&#039; e &#039;fechar&#039; e etc | Deve retornar o icone clicado.  |

<br />

## Contato

Em caso de dúvidas, por favor entrar em contato pelo e-mail [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)
<br /><br />
<br />
