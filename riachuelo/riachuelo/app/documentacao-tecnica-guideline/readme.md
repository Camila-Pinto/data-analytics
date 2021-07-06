![Zoly](https://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics <br />
> Documento de Especificação Técnica

<br />

## Implementação de Tags Firebase - Riachuelo APP - GuideLine Super App

Última atualização: 24/02/2021. <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)


## Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Definições Globais](#defini%c3%a7%c3%b5es-globais)
- [Telas](#telas)
- [Eventos - Cartões - Super APP Guideline](#Eventos---Cart%c3%b5es---Super-APP-Guideline)
- [Considerações Finais](#considera%c3%a7%c3%b5es-finais)


## Objetivo

Este documento tem como objetivo instruir a implementação de Tags de Firebase Analytics para o aplicativo de Riachuelo Super App.


## Implementação

> Os links abaixo descrevem como realizar a implementação do Firebase em seu aplicativo.

[Link 1 - Adicionar o Firebase ao projeto](https://rnfirebase.io/analytics/usage/#installation)

[Link 2 - Definir propriedades do Usuário e Eventos](https://rnfirebase.io/analytics/usage#usage)

[Link 3 - Acompanhamento do fluxo de Telas](https://rnfirebase.io/analytics/screen-tracking)

[Link 4 - Referências do Firebase Analytics](https://rnfirebase.io/reference/analytics)

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
analytics().setUserId("[[UserID]]");
analytics().setUserProperties({'login_status': '[[logado-ou-nao-logado]]','tipo_cartao': '[[tipo-do-cartao]]'});
```

**OBS:** Esta definição global deve ser disparada em todo momento que o usuário estiver logado.

| Nome                | Variável                   | Exemplo                  | Descrição                          |
| ------------------- | -------------------------- | ------------------------ | ---------------------------------- |
| Custumer ID Magento | `[[UserID]]`               | '9876542'                | ID único do usuário                |
| login_status         | `[[logado-ou-nao-logado]]` | ‘logado’ ou ‘nao-logado’ | Identificação de Login do usuário. |
| tipo_cartao    | `[[tipo-do-cartao]]` | 'cartao-midway-master', 'cartao-midway-visa', 'cartao-midway-pl' e etc |Deve retornar o tipo do cartão |

--- 

### Telas

> No carregamento de todas as telas devem ser disparados os eventos de _Screenview_ com os nomes das telas do aplicativo conforme a seguir:


```javascript
analytics().logScreenView("/nome-da-tela/");
```

---

### Eventos - Cartões - Super APP Guideline

- **Onde:** Visualização das telas dentro de Super app Guideline

```javascript
analytics().logScreenView("/riachuelo-app/[[nome-tela]]-super-app/");
```

| Variável           | Exemplo                               | Descrição                            |
| :----------------- | :------------------------------------ | :----------------------------------- |
| `[[nome-tela]]` | 'midway', 'onboarding', 'cartoes', 'limite', 'limite/mais-credito' etc | Deve retornar a tela apresentada |

<br />

- **Quando:** No clique dos botões ou links
- **Onde:** Nas telas que estiverem disponíveis

```javascript
analytics().logEvent("event", {
  eventCategory: "riachuelo:app:super-app:[[nome-do-fluxo]]",
  eventAction: "clique:[[botao-link]]",
  eventLabel: "[[nome-item]]"
});
```

| Variável           | Exemplo                               | Descrição                            |
| :----------------- | :------------------------------------ | :----------------------------------- |
| `[[nome-do-fluxo]]` | 'cartoes' , 'desbloqueio' etc | Deve retornar o nome do fluxo relacionado a tela que usuário se encontra |
| `[[botao-link]]` | 'botao' ou 'link' | Deve retornar o elemento clicado |
| `[[nome-item]]` | 'voltar', 'fechar', 'tentar-novamente' etc | Deve retornar o nome do item clicado |

<br />

- **Quando:** No clique dos ícones de "Expandir", "Recolher" e "Abrir" para visualização da fatura aberta, fechada ou em atraso ou em pagamento identificado.
- **Onde:** Nas telas que estiverem disponíveis

```javascript
analytics().logEvent("event", {
  eventCategory: "riachuelo:app:super-app:[[nome-do-fluxo]]",
  eventAction: "clique:icone",
  eventLabel: "[[acao-icone]]-[[nome-dropdown]]"
});
```

| Variável           | Exemplo                               | Descrição                            |
| :----------------- | :------------------------------------ | :----------------------------------- |
| `[[nome-do-fluxo]]` | 'cartoes' , 'desbloqueio' etc | Deve retornar o nome do fluxo relacionado a tela que usuário se encontra |
| `[[acao-icone]]` | 'expandir', 'recolher' ou 'abrir' | Deve retornar a ação da interação |
| `[[nome-dropdown]]` | 'fatura-aberta', 'fatura-fechada', 'fatura-em-atraso', 'pagamento-identificado' etc | Deve retornar o nome do dropdown no qual ocorreu a interação |

<br />

- **Quando:** Após o preenchimento dos campos (disparar a ação caso o texto seja modificado ou caso haja uma nova entrada de texto.)
- **Onde:** Nas telas que estiverem disponíveis

```javascript
analytics().logEvent("event", {
  eventCategory: "riachuelo:app:super-app:[[nome-do-fluxo]]",
  eventAction: "interacao",
  eventLabel: "preencheu:[[nome-campo]]"
});
```

| Variável           | Exemplo                               | Descrição                            |
| :----------------- | :------------------------------------ | :----------------------------------- |
| `[[nome-do-fluxo]]` | 'cartoes' , 'desbloqueio' etc | Deve retornar o nome do fluxo relacionado a tela que usuário se encontra |
| `[[nome-campo]]` | 'nome', 'cpf' e etc | Deve retornar o nome do campo preenchido |

<br />

- **Quando:** Após callback de sucesso ou erro
- **Onde:** Nas telas que estiverem disponíveis

```javascript
analytics().logEvent("event", {
  eventCategory: "riachuelo:app:super-app:[[nome-do-fluxo]]",
  eventAction: "callback",
  eventLabel: "[[status]]"
});
```

| Variável           | Exemplo                               | Descrição                            |
| :----------------- | :------------------------------------ | :----------------------------------- |
| `[[nome-do-fluxo]]` | 'cartoes' , 'desbloqueio' etc | Deve retornar o nome do fluxo relacionado a tela que usuário se encontra |
| `[[status]]` | 'sucesso', 'erro:nao-foi-possivel-concluir' e etc | Deve retornar o status do callback |

<br />


---

### Considerações Finais

> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<script>
  document.addEventListener('DOMContentLoaded', function(event) {
    document.querySelectorAll('h1 a')[0].style.display = 'none';
  });
</script>
