![Zoly](https://lucida-brasil.github.io/public/Images/zoly-logo.png)

> Área - Digital Analytics <br />
> Documento de Especificação Técnica

<br />

## Implementação de Tags Firebase - Riachuelo Midway APP

Última atualização: 24/06/2021 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)


## Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Definições Globais](#defini%c3%a7%c3%b5es-globais)
- [Eventos Globais](#eventos-globais)
- [Biometria Facial](#biometria-facial)
- [Cadastro Endereço](#cadastro-endereco)
- [Primeiro Acesso](#primeiro-acesso)
- [Cadastro](#cadastro)
- [Transparência e Proteção de Dados](#transpar&#234;ncia-e-prote&#231;&#227;o-de-dados)
- [Login](#login)
- [Abertura Conta Corrente](#abertura-conta-corrente)
- [Pagamento Boleto](#pagamento-boleto)
- [Extrato](#extrato)
- [Transferência](#transferencia)
- [Perfil Canais Digitais 2.2.5](#perfil-canais-digitais-2.2.5)
- [Conta Remunerada](#conta-remunerada)
- [Encerramento de conta](#encerramento-de-conta)
- [Cheque Especial](#cheque-especial)
- [Desbloqueio de Cartão](#desbloqueio-de-cartao)
- [Home](#home)
- [Home - Migração de Conta](#home-migra&#231;&#227;o-de-conta)
- [Cotação de câmbio](cota&#231;&#227;o-de-c&#226;mbio)
- [Credito Pré Aprovado](credito-pr&#233;-aprovado)
- [Cartões - Fase 2.1](#cartoes-fase-2.1)
- [Pagar Fatura - Fase 2.2](#pagar-fatura-fase.2.2)
- [Fase 2.3](#fase-2.3)
- [Fase 2.2.5 - Conta Pagamento - Depósito via Boleto](#fase-2.2.5-conta-pagamento-deposito-via-boleto)
- [Fase 2.4 - Conta Pagamento - Extrato](#fase2.4-conta-pagamento-extrato)
- [Fase 2.5 - Conta corrente - Parcelamento Cheque Especial](#fase2.5-conta-corrente-parcelamento-cheque-especial)
- [Fase 2.5 - Conta Pagamento - Migração para Conta Corrente](#fase2.5-conta-pagamento-migra%c3%a7%c3%a3o-para-conta-corrente)
- [Fase 2.6 - Conta Pagamento - Encerrar Conta](#fase2.6-conta-pagamento-encerrar-conta)
- [Fase 2.6 - Conta Pagamento - Bloquear e desbloquear cartão](#fase2.6-conta-pagamento-bloquear-e-desbloquear-cartao)
- [Fase 2.7 - Conta Pagamento - Transferênica Midway para Midway](#fase2.7-conta-pagamento-transferencia-midway-para-midway)
- [Fase 2.8 - Conta Pagamento - Transferência Midway - Outros Bancos](#fase2.8-conta-pagamento-transferencia-midway-para-outros-bancos)
- [Outros Serviços](#outros-servi&#231;os)
- [MarketPlace Financeiro - Geral](#marketplace-financeiro---geral)
- [Empréstimo Contratação](#empr&#233;stimo-contrata&#231;&#227;o)
- [Empréstimo Consulta](#empr&#233;stimo-consulta)
- [Empréstimo Cancelamento](#empr&#233;stimo-cancelamento)
- [Empréstimo Liquidação](#empr&#233;stimo-liquida&#231;&#227;o)
- [Home Pix](#home-pix)
- [Cadastrar Pix](#cadastrar-pix)
- [Minhas chaves Pix](#minhas-chaves-pix)
- [Devolução Pix](#devolu&#231;&#227;o-pix)
- [Exclusão Pix](#exclus&#227;o-pix)
- [Qr Code Pix](#qr-code-pix)
- [Pagamentos e Transferências Pix](#pagamentos-e-transfer&#234;ncias-pix)
- [Extrato Pix](#extrato-pix)
- [Alterar Senha](#alterar-senha)
- [Desbloqueio de Senha Cartão](#desbloqueio-de-senha-cart&#227;o)
- [Eventos - Midway - Assistências e Seguros](#eventos---midway---assist&ecirc;ncias-e-seguros)
- [Eventos - Saque Digital](#eventos---saque-digital)
- [Contato](#contato)



## Objetivo

Este documento tem como objetivo instruir a implementação de Tags de Firebase Analytics para o aplicativo de Riachuelo Midway App.


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
Analytics.setUserProperty("dimension1", "[[Bandeira do Cartão]]");


```

**OBS:** Esta definição global deve ser disparada em todo momento que o usuário estiver logado.

| Nome         | Variável                 | Exemplo                                 | Descrição                               |
| :---------   | :---------------------   | :-----------------------------------    | :-----------------------------------    |
| [[userID]]       | [[UserID]]               | '01234'                                 | ID único de usuário definido após o cadastro (ID próprio da Midway).|
| dimension1   | [[Bandeira do Cartão]]   | "pl", "visa", "mastercard", etc | Deve retornar a bandeira do cartão utilizada pelo usuário |


### Eventos Globais
- **Quando:** Na abertura de modal de erro.
- **Onde:** Em todas as telas em que esta opção estiver disponível.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:erro",
        	eventAction: "abriu:modal",
        	eventLabel: "[[nome-erro]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;cadastro&#039;, &#039;conta-ilimitada&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |
| [[nome-erro]] | &#039;algo-deu-errado&#039;, &#039;nao-conseguimos-conexao-com-a-internet&#039; etc. | Deve retornar a mensagem do erro exibido. |

<br />

- **Quando:** No clique de um dos links.
- **Onde:** Em um modal de erro, em todas as telas em que esta opção estiver disponível.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:erro",
        	eventAction: "clique:link",
        	eventLabel: "[[nome-link]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;cadastro&#039;, &#039;conta-ilimitada&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |
| [[nome-link]] | &#039;tentar-novamente&#039;, &#039;voltar-para-o-inicio&#039;, &#039;recarregar&#039; etc. | Deve retornar nome do link clicado. |

<br />

- **Onde:** Visualização das possíveis telas de sucesso ou erro de validação de etapas de análise.


```javascript
    Analytics.logScreenView("/[[nome-fluxo]]/[[nome-tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;cadastro&#039;, &#039;abertura-conta-ilimitada&#039;, &#039;conta-remunerada&#039;, &#039;emprestimo-pessoal&#039;, &#039;cancelamento-emprestimo-pessoal&#039; e etc | Deve retornar o nome do fluxo. |
| [[nome-tela]] | &#039;salvando-dados&#039;, &#039;em-analise&#039;, &#039;analise-concluida&#039;, &#039;algo-deu-errado&#039; e etc | Deve retornar o nome da tela apresentada. |

<br />

- **Quando:** No clique nos botões de &quot;Continuar&quot;, &quot;Fechar&quot; e &quot;OK&quot;
- **Onde:**  Nas possíveis telas de sucesso e erro de validação de etapas de análise

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[[nome-fluxo]]",
        	eventAction: "clique:botao:[[nome-tela]]",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;cadastro&#039;, &#039;abertura-conta-ilimitada&#039;, &#039;conta-remunerada&#039;, &#039;emprestimo-pessoal&#039;, &#039;cancelamento-emprestimo-pessoal&#039; e etc | Deve retornar o nome do fluxo. |
| [[nome-tela]] | &#039;salvando-dados&#039;, &#039;em-analise&#039;, &#039;analise-concluida&#039;, &#039;algo-deu-errado&#039; e etc | Deve retornar o nome da tela apresentada. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;fechar&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />

### Biometria Facial

- **Onde:** Visualização da tela de dicas para o cadastro da biometria facial (em todos os fluxos em que aparece).


```javascript
    Analytics.logScreenView("/[[nome-fluxo]]/biometria-facial/dicas/[[passo]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[passo]] | &#039;1&#039;,&#039;2&#039;,&#039;3&#039; | Deve retornar o numero do passo das dicas que está sendo visto de acordo com os bullets inferiores. |
| [[nome-fluxo]] | &#039;cadastro&#039;, &#039;abertura-conta-ilimitada&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |

<br />

- **Quando:** Ao clicar no botão apresentado na última etapa das dicas para biometria facial.
- **Onde:** Na tela de dicas para biometria facial.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:biometria-facial:dicas",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;cadastro&#039;, &#039;abertura-conta-ilimitada&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |

<br />

- **Quando:** Ao clicar em um dos links do modal de Permissão.
- **Onde:** Modal de Permissão que aparece na tela de dicas de biometria facial.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:biometria-facial:dicas",
        	eventAction: "clique:link",
        	eventLabel: "[[nome-link]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;cadastro&#039;, &#039;abertura-conta-ilimitada&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |
| [[nome-link]] | &#039;nao-permito&#039; ou &#039;ok&#039;. | Deve retornar o título do link clicado. |

<br />

- **Onde:** Visualização da tela de captura da biometria facial (em todos os fluxos em que aparece).


```javascript
    Analytics.logScreenView("/[[nome-fluxo]]/biometria-facial/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;cadastro&#039;, &#039;abertura-conta-ilimitada&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |

<br />

- **Quando:** Ao clicar em um dos botões apresentados na tela de confirmação da imagem para biometria facial.
- **Onde:** Na tela de confirmação da imagem para biometria facial.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:biometria-facial",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;cadastro&#039;, &#039;abertura-conta-ilimitada&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |
| [[nome-botao]] | &#039;tirar-outra-foto&#039; ou &#039;continuar&#039;. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No callback de sucesso ou erro do cadastro de biometria facial.
- **Onde:** Na tela de confirmação da imagem para biometria facial.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:biometria-facial",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:nao-foi-possivel-salvar-a-imagem&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

### Cadastro Endereço

- **Onde:** Visualização da tela &#039;Endereço&#039;.

```javascript
    Analytics.logScreenView("/[[nome-fluxo]]/endereco");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |

<br />

- **Quando:** Após o preenchimento do campo de &#039;CEP&#039;.
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:cadastro-endereco",
        	eventAction: "preencheu:campo",
        	eventLabel: "cep"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |

<br />

- **Quando:** No callback de preenchimento do campo &#039;CEP&#039;.
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:cadastro-endereco",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:cep-invalido&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** Ao clicar em um dos botões.
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:cadastro-endereco",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;voltar&#039;, &#039;editar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Ao clicar no link &#039;Terminar Depois&#039;.
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:cadastro-endereco",
        	eventAction: "clique:link",
        	eventLabel: "terminar-depois"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |

<br />

- **Quando:** Após o preenchimento de um dos campos.
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:cadastro-endereco",
        	eventAction: "preencheu:campo",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |
| [[nome-campo]] | &#039;endereco&#039;, &#039;numero&#039;, &#039;complemento&#039;, &#039;bairro&#039; etc. | Deve retornar o nome do campo com o qual o usuário interagiu. |

<br />

- **Quando:** No callback de erro após interagir com os campos
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:cadastro-endereco",
        	eventAction: "preencheu:campo:callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |
| [[status]] | &#039;erro:endereco-invalido&#039; e etc. | Deve retornar a mensagem de erro apresentada para o usuário. |

<br />

- **Onde:** Visualização da tela &#039;Endereço (4/5)&#039;, que exibe o endereço após conclusão do preenchimento do formulário de endereço.


```javascript
    Analytics.logScreenView("/[[nome-fluxo]]/endereco/cadastro-concluido/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |

<br />

- **Quando:** Ao clicar em um dos botões.
- **Onde:** Na tela &#039;Endereço (4/5)&#039;, que exibe o endereço após conclusão do preenchimento do formulário de endereço.

Endereço
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:cadastro-endereco:cadastro-concluido",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;editar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Na abertura do box &#039;Endereço de entrega&#039;, após preenchimento do formulário de endereço.
- **Onde:** Na tela &#039;Endereço (4/5)&#039;, que exibe o endereço após conclusão do preenchimento do formulário de endereço.

Endereço
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:cadastro-endereco:cadastro-concluido",
        	eventAction: "abriu:box",
        	eventLabel: "endereco-entrega"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |

<br />

- **Quando:** No clique do link &#039;Adicionar novo endereço&#039;.
- **Onde:** No box &#039;Endereço de entrega&#039;, na tela &#039;Endereço (4/5)&#039;, que exibe o endereço após conclusão do preenchimento do formulário de endereço.

Endereço
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:cadastro-endereco:cadastro-concluido",
        	eventAction: "clique:link",
        	eventLabel: "adicionar-novo-endereco"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |

<br />

- **Quando:** No clique do botão &#039;Não, obrigado&#039;.
- **Onde:** No box &#039;Endereço de entrega&#039;, na tela &#039;Endereço (4/5)&#039;, que exibe o endereço após conclusão do preenchimento do formulário de endereço.

Endereço
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-fluxo]]:cadastro-endereco:cadastro-concluido",
        	eventAction: "clique:botao",
        	eventLabel: "nao-obrigado"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-fluxo]] | &#039;abertura-conta-ilimitada&#039;, &#039;cadastro&#039; etc. | Deve retornar o nome do fluxo em que o usuário está. |

<br />

### Primeiro Acesso

- **Onde:** Visualização da tela 'Abra sua conta Midway'.

```javascript
    Analytics.logScreenView("/primeiro-acesso/abra-sua-conta-midway/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Abra sua conta Midway&#039;.

Onboarding App - Início
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:abra-sua-conta-midway",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;abrir-minha-conta-midway&#039;, &#039;fechar&#039; etc. | Deve retornar o nome do botão clicado pelo usuário. |

<br />

- **Quando:** No clique em um dos links &#039;Detalhes&#039;.
- **Onde:** Na tela &#039;Abra sua conta Midway&#039;.

Onboarding App - Início
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:abra-sua-conta-midway",
        	eventAction: "clique:link",
        	eventLabel: "detalhes:[[nome-secao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-secao]] | &#039;abertura-rapida-conta-simples&#039;, &#039;possivel-upgrade-para-conta-ilimitada&#039; etc. | Deve retornar o nome da seção do link de detalhes clicado pelo usuário. |

<br />

- **Onde:** Visualização da tela 'Vantagens Midway'.

```javascript
    Analytics.logScreenView("/primeiro-acesso/vantagens-midway/");
```


<br />

- **Quando:** Na visualizaçao de uma das opções do carrossel.
- **Onde:** Na tela &#039;Vantagens Midway&#039;.

Onboarding App - Início
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:vantagens-midway",
        	eventAction: "mudou:carrossel",
        	eventLabel: "[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;beneficios-ideais-para-voce&#039;, &#039;dinheiro-livre-e-facil&#039;, &#039;principais-descontos&#039; etc. | Deve retornar o nome do card exibido no carrossel. |

<br />

- **Quando:** No clique em uma dos cards do carrossel.
- **Onde:** Na tela &#039;Vantagens Midway&#039;.

Onboarding App - Início
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:vantagens-midway",
        	eventAction: "clique:card",
        	eventLabel: "[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;beneficios-ideais-para-voce&#039;, &#039;dinheiro-livre-e-facil&#039;, &#039;principais-descontos&#039; etc. | Deve retornar o nome do card exibido no carrossel. |

<br />

- **Quando:** No clique do botão &#039;Abrir minha conta Midway&#039;.
- **Onde:** Na tela &#039;Vantagens Midway&#039;.

Onboarding App - Início
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:vantagens-midway",
        	eventAction: "clique:botao",
        	eventLabel: "abrir-minha-conta-midway"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Benefício ideal&#039;.

```javascript
    Analytics.logScreenView("/primeiro-acesso/beneficio-ideal/");
```


<br />

- **Quando:** Na visualizaçao de uma das opções do carrossel.
- **Onde:** Na tela &#039;Benefício ideal&#039;.

Onboarding App - Início
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:beneficio-ideal",
        	eventAction: "mudou:carrossel",
        	eventLabel: "[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;pacote-bacen&#039;, &#039;conta-simples&#039;, &#039;pacote-premium&#039; etc. | Deve retornar o nome do card exibido no carrossel. |

<br />

- **Quando:** No clique do botão &#039;voltar&#039;.
- **Onde:** Na tela &#039;Benefício ideal&#039;.

Onboarding App - Início
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:beneficio-ideal",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```


<br />

- **Onde:** Visualização da tela de  &#039;Primeiro Acesso&#039; assim que o usuário abrir o aplicativo.


```javascript
    Analytics.logScreenView("/primeiro-acesso/");
```


<br />

- **Quando:** Ao clicar no botão &#039;Abrir conta&#039;.
- **Onde:** Na tela de &#039;Primeiro Acesso&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso",
        	eventAction: "clique:botao",
        	eventLabel: "abrir-conta"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Bem vindo&#039;.


```javascript
    Analytics.logScreenView("/primeiro-acesso/bem-vindo/");
```


<br />

- **Quando:** Ao clicar em um dos botões dos modais de Permissão.
- **Onde:** Modais de Permissão que aparecem na tela &#039;Boas Vindas&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:boas-vindas",
        	eventAction: "modal:[[nome-modal]]:clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-modal]] | &#039;permissao-localizacao&#039;, &#039;permissao-notificacao&#039; e etc | Deve retornar o nome do modal de permissão. |
| [[nome-botao]] | &#039;nao-permito&#039; ou &#039;ok&#039;. | Deve retornar o título do botão clicado. |

<br />

- **Quando:** Ao clicar em um dos botões.
- **Onde:** Tela de &#039;Boas Vindas&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:boas-vindas",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;entrar&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Ao clicar em um dos links.
- **Onde:** Tela de &#039;Boas Vindas&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:boas-vindas",
        	eventAction: "clique:link",
        	eventLabel: "[[nome-link]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;como-cuidamos-dos-seus-dados&#039; etc. | Deve retorna o nome do link clicado. |

<br />

- **Quando:** Ao clicar no botão &#039;Entendi&#039; apresentado no alerta.
- **Onde:** No alerta de &#039;Como cuidamos dos seus dados&#039; aberto na tela &#039;Boas Vindas&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:boas-vindas",
        	eventAction: "alerta:como-cuidamos-dos-seus-dados:clique:botao",
        	eventLabel: "entendi"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Políticas de privacidade&#039;.

```javascript
    Analytics.logScreenView("/primeiro-acesso/termo-uso-privacidade/");
```


<br />

- **Quando:** Ao clicar em um dos botões.
- **Onde:** Nas Telas de &#039;Parceiros Digitais&#039; e &#039;Políticas de Privacidade&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:[[nome-tela]]",
        	eventAction: "clique:[[botao-ou-link]]",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-tela]] | &#039;parceiros-digitais&#039;, &#039;politicas-privacidade&#039; e etc  | Deve retornar o nome da tela. |
| [[botao-ou-link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o tipo do elemento. |
| [[nome-botao]] | &#039;li-e-concordo&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização do primeiro passo da tela &#039;Bem-vindo(a)&#039; para inserir o CPF.

```javascript
    Analytics.logScreenView("/primeiro-acesso/cpf/");
```


<br />

- **Quando:** Ao clicar no botão de voltar
- **Onde:** Na tela &#039;Bem-vindo(a)&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:bem-vindo",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```


<br />

- **Quando:** Ao preencher o campo de &#039;CPF&#039;.
- **Onde:** Na tela &#039;Bem-vindo(a)&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:bem-vindo",
        	eventAction: "interacao:preencheu",
        	eventLabel: "cpf"
        });
```


<br />

- **Quando:** No callback do preenchimento do campo de &#039;CPF&#039;, quando o foco sai do campo.
- **Onde:** No primeiro passo do login.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:primeiro-acesso:bem-vindo",
        	eventAction: "callback",
        	eventLabel: "[[status]]",
        	userID: "[[userID]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:cpf-invalido&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |
| [[userID]] | &quot;01234&quot; | ID único de usuário definido após o cadastro (ID próprio da Midway). |

<br />

### Cadastro

- **Onde:** Visualização do segundo passo da tela &#039;Bem-vindo(a)&#039; para inserir o Nome Completo.

```javascript
    Analytics.logScreenView("/cadastro/nome-completo/");
```


<br />

- **Quando:** Ao clicar no botão de voltar
- **Onde:** Na tela &#039;Bem-vindo(a)&#039; para preencher o nome completo.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```


<br />

- **Quando:** Ao preencher o campo de &#039;Nome Completo&#039;.
- **Onde:** No início do cadastro, quando o CPF não é encontrado.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro",
        	eventAction: "interacao:preencheu",
        	eventLabel: "nome-completo"
        });
```


<br />

- **Quando:** No callback para &#039;Nome Completo&#039; preenchido.
- **Onde:** No início do cadastro, quando o CPF não é encontrado.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro",
        	eventAction: "callback",
        	eventLabel: "nome-completo:[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:nome-incompleto&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** Ao clicar no botão &#039;Continuar&#039;.
- **Onde:** No início do cadastro, quando o CPF não é encontrado.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro",
        	eventAction: "clique:botao",
        	eventLabel: "nome-completo:continuar"
        });
```


<br />

- **Onde:**  Visualização da tela de cadastro de &#039;Dados pessoais&#039;.

```javascript
    Analytics.logScreenView("/cadastro/dados-pessoais/");
```


<br />

- **Quando:** Ao clicar no botão de voltar
- **Onde:** Na tela &#039;Dados Pessoais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:dados-pessoais",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```


<br />

- **Quando:** Após o preenchimento dos campos no formulário de &#039;Dados pessoais&#039;.
- **Onde:** Na etapa de cadastro &#039;Dados pessoais&#039;


No app, não chegou essa etapa de biometria
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:dados-pessoais",
        	eventAction: "interacao:preencheu",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;como-quer-ser-chamado&#039;, &#039;celular&#039;, &#039;e-mail&#039; ou &#039;data-de-nascimento&#039;. | Deve retornar o nome do campo preenchido. |

<br />

- **Quando:** No callback de erro, após a saída do campo, caso ele esteja preenchido incorretamente
- **Onde:** Na etapa de cadastro &#039;Dados pessoais&#039;
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:dados-pessoais",
        	eventAction: "interacao:preencheu:callback",
        	eventLabel: "[[nome-campo]]:[[erro]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;como-quer-ser-chamado&#039;, &#039;celular&#039;, &#039;e-mail&#039; ou &#039;data-de-nascimento&#039;. | Deve retornar o nome do campo preenchido. |
| [[erro]] | &#039;erro:celular-incorreto&#039;, &#039;erro:preencha-data&#039; e etc | Deve retornar o erro apresentado. |

<br />

- **Quando:** Ao selecionar ou desselecionar o checkbox para afirmar que &quot;Sou PcD&quot; ou que &quot;Aceita compartilhar meus dados com os principais....&quot;
- **Onde:** Na etapa de cadastro &quot;Dados pessoais&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:dados-pessoais",
        	eventAction: "[[selecionado ou desselecionado]]:checkbox",
        	eventLabel: "[[tipo-checkbox]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-checkbox]] | &#039;sou-pcd&#039;, &#039;aceito-compartilhar-meus-dados-com-parceiros-banco-midway&#039; e etc | Deve retornar o nome do checkbox selecionado. |
| [[selecionado ou desselecionado]] | &#039;selecionado&#039; ou &#039;desselecionado&#039; | Deve retornar o status do checkbox, se está selecionado ou não. |

<br />

- **Quando:** Após preencher o campo de &quot;Tipo de deficiência&quot;
- **Onde:** Na etapa de cadastro &quot;Dados pessoais&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:dados-pessoais",
        	eventAction: "interacao:campo",
        	eventLabel: "deficiencia"
        });
```


<br />

- **Quando:** Na interação com as opções de &quot;Nacionalidade&quot;
- **Onde:** Na etapa de cadastro &quot;Dados pessoais&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:dados-pessoais",
        	eventAction: "interacao:opcao:[[tipo-opcao]]",
        	eventLabel: "[[nacionalidade-ou-pais]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-opcao]] | &#039;nacionalidade&#039;, &#039;pais&#039; e etc | Deve retornar o tipo de opção interagida. |
| [[nacionalidade-ou-pais]] | &#039;nacionalidade:brasileira&#039;, &#039;nacionalidade:estrangeira&#039;, &#039;pais:estados-unidos-america&#039; e etc | Deve retornar a opção selecionada. |

<br />

- **Quando:** Após preencher o campo de &quot;NIF&quot;
- **Onde:** Na etapa de cadastro &quot;Dados pessoais&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:dados-pessoais",
        	eventAction: "interacao:campo",
        	eventLabel: "nif"
        });
```


<br />

- **Quando:** Ao clicar no botão de &#039;Continuar&#039;.
- **Onde:** Na etapa de cadastro &#039;Dados pessoais&#039;

No app, não chegou essa etapa de biometria
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:dados-pessoais",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />


- **Onde:** Visualização da tela para validação do &#039;Código de Verificação&#039; 

```javascript
    Analytics.logScreenView("/cadastro/codigo-verificacao/");
```


<br />

- **Quando:** No clique no botão de &#039;Reenviar código&#039; ou &quot;Voltar&quot;
- **Onde:** Na etapa de validação do Código de verificação

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:codigo-verificacao",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;reenviar-codigo&#039;, &#039;voltar&#039; e etc | Deve retornar o nome do botão clicado. |

<br />



- **Onde:** Visualização da tela &#039;Antes de criar sua senha do aplicativo...&#039;, de dicas de criação de senha.


```javascript
    Analytics.logScreenView("/cadastro/antes-de-criar-senha/");
```


<br />

- **Quando:** No clique do botão &#039;Continuar&#039;.
- **Onde:** Na tela de &#039;Antes de criar sua senha do aplicativo...&#039;, na etapa de criação de senha.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:criar-senha",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />


- **Onde:** Visualização da tela para cadastro de uma nova senha de acesso ao aplicativo.

```javascript
    Analytics.logScreenView("/cadastro/criar-senha/");
```


<br />


- **Onde:** Visualização da tela para confirmação da senha cadastrada para acesso ao aplicativo.

```javascript
    Analytics.logScreenView("/cadastro/confirmar-senha/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Nas telas de &#039;Senha do Aplicativo&#039; e &#039;Confirme sua senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:[[nome-tela]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-tela]] | &#039;senha-aplicativo&#039;, &#039;confirmar-senha&#039; e etc | Deve retornar o nome da tela. |
| [[nome-botao]] | &#039;voltar&#039;, &#039;visualizar-senha&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />



- **Onde:** Visualização da tela de &#039;Seu cadastro foi criado com sucesso!&#039;.

```javascript
    Analytics.logScreenView("/cadastro/cadastro-criado-com-sucesso/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela de &#039;Seu cadastro foi criado com sucesso!&#039;.

Onboarding App - Fim
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:completo",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;aceitar-conta-simples&#039;, &#039;continuar-para-conta-ilimitada&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No callback para a conclusão do cadastro.
- **Onde:** Na tela de &#039;Seu cadastro foi criado com sucesso!&#039;.

Onboarding App - Fim
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:completo",
        	eventAction: "callback:cadastro-completo",
        	eventLabel: "[[status]]",
        	userID: "[[userID]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:senha-digitada-diferente-da-anterior&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |
| [[userID]] | &quot;01234&quot; | ID único de usuário definido após o cadastro (ID próprio da Midway). |

<br />

- **Quando:** Na abertura do box &#039;Sua Cesta de Benefícios inclui&#039;.
- **Onde:** Na tela de &#039;Seu cadastro foi criado com sucesso!&#039;.

Onboarding App - Fim
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:completo",
        	eventAction: "abriu:box",
        	eventLabel: "sua-cesta-de-beneficios-inclui"
        });
```


<br />

- **Quando:** No clique do botão &#039;Continuar&#039;.
- **Onde:** No box &#039;Sua Cesta de Benefícios inclui&#039;, na tela &#039;Seu cadastro foi criado com sucesso!&#039;.

Onboarding App - Fim
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:completo",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />



- **Onde:** Visualização da tela &#039;Contrato de Conta Simples&#039;.

```javascript
    Analytics.logScreenView("/cadastro/contrato-de-conta-simples/");
```

<br />


- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Contrato de Conta Simples&#039;.

Onboarding App - Fim
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:contrato-de-conta-simples",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;li-e-concordo&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique do link &#039;Ver tarifas da cesta de benefícios&#039;.
- **Onde:** Na tela &#039;Contrato de Conta Simples&#039;.

Onboarding App - Fim
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:contrato-de-conta-simples",
        	eventAction: "clique:link",
        	eventLabel: "ver-tarifas-da-cesta-de-beneficios"
        });
```


<br />


- **Onde:** Visualização da tela &#039;Conta Simples aberta com sucesso!&#039;.

```javascript
    Analytics.logScreenView("/cadastro/abertura-conta-simples/sucesso/");
```


<br />

- **Quando:** No clique do botão &#039;Entrar no aplicativo&#039;.
- **Onde:** Na tela &#039;Conta Simples aberta com sucesso&#039;.

Onboarding App - Fim
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:abertura-conta-simples:sucesso",
        	eventAction: "clique:botao",
        	eventLabel: "entrar-no-aplicativo"
        });
```


<br />

- **Quando:** No clique do link &#039;Saiba mais&#039;.
- **Onde:** Na tela &#039;Conta Simples aberta com sucesso&#039;.

Onboarding App - Fim
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:abertura-conta-simples:sucesso",
        	eventAction: "clique:link",
        	eventLabel: "saiba-mais"
        });
```


<br />

- **Quando:** No callback para a conclusão da abertura de conta simples.
- **Onde:** Na tela &#039;Conta Simples aberta com sucesso&#039;.

Onboarding App - Fim
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:abertura-conta-simples:sucesso",
        	eventAction: "callback:abertura-conta-simples-concluida",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:algo-deu-errado&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />


- **Onde:** Visualização da tela de &#039;Notificações&#039;.

```javascript
    Analytics.logScreenView("/cadastro/confirmar-senha/notificacoes/");
```


<br />

- **Quando:** No clique no botão de &quot;Entendi&quot;
- **Onde:** Na tela de Notificações

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:notificacoes",
        	eventAction: "clique:botao",
        	eventLabel: "entendi"
        });
```


<br />


- **Onde:** Visualização da tela de &#039;Contrato de Conta Digital Midway&#039;

```javascript
    Analytics.logScreenView("/cadastro/confirmar-senha/notificacoes/contrato-conta-digital/");
```


<br />

- **Quando:** No clique nos botões de &quot;Li e Concordo&quot; e &quot;Voltar&quot;
- **Onde:** Na tela de &quot;Contrato de Conta Digital Midway&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:contrato-conta-digital",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;li-e-concordo&#039; etc. | Deve retornar o nome do botão clicado. |

<br />



- **Onde:**  Visualização da tela de &#039;Estamos Felizes em ter você aqui!&#039;.

```javascript
    Analytics.logScreenView("/cadastro/confirmar-senha/notificacoes/contrato-conta-digital/feliz-ter-voce-aqui/");
```


<br />

- **Quando:** No clique no botão de &quot;Continuar&quot; e no link &quot;Acesse sua conta Midway&quot;
- **Onde:** Na tela de &quot;Estamos felizes em ter você aqui!&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:feliz-ter-voce-aqui",
        	eventAction: "clique:[[botao-ou-link]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-ou-link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o elemento clicado. |
| [[nome-item]] | &#039;continuar&#039;, &#039;acesse-conta-midway&#039; e etc. | Deve retornar o nome do item clicado. |

<br />


- **Onde:** Visualização do modal de &#039;Cesta de Benefícios&#039;.

```javascript
    Analytics.logScreenView("/cadastro/confirmar-senha/notificacoes/contrato-conta-digital/feliz-ter-voce-aqui/cesta-beneficios/");
```


<br />

- **Quando:** No clique nos botões de &quot;Continuar&quot; e &quot;Fechar&quot;
- **Onde:** Na tela de &quot;Cesta de Benefícios&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:feliz-ter-voce-aqui",
        	eventAction: "clique:botao:modal-beneficios",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;fechar&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela de &#039;Conta Digital aberta com sucesso!&#039;.

```javascript
    Analytics.logScreenView("/cadastro/confirmar-senha/notificacoes/contrato-conta-digital/feliz-ter-voce-aqui/sucesso/");
```


<br />

- **Quando:** No clique no botão de &quot;Entrar na minha conta Midway&quot; e no link &quot;Saiba mais&quot;
- **Onde:** Na tela de &quot;Conta Digital aberta com sucesso!&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:sucesso",
        	eventAction: "clique:[[botao-ou-link]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-ou-link]] | &#039;botao&#039;, &#039;link&#039; e etc | Deve retornar o elemento clicado. |
| [[nome-item]] | &#039;entrar-conta-midway&#039;, &#039;saiba-mais&#039; e etc. | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização do modal de &#039;Valor mínimo para cartão de débito&#039;.

```javascript
    Analytics.logScreenView("/cadastro/confirmar-senha/notificacoes/contrato-conta-digital/feliz-ter-voce-aqui/sucesso/modal-valor-min-cartao-debito/");
```


<br />

- **Quando:** No clique nos botões de &quot;Continuar&quot; e &quot;Fechar&quot;
- **Onde:** No modal de &#039;Valor mínimo para cartão de débito&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:sucesso",
        	eventAction: "clique:botao:modal-valor-min-cartao-debito",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;fechar&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Na abertura do box &#039;Valor mínimo para cartão de débito&#039;.
- **Onde:** Na tela &#039;Conta Simples aberta com sucesso&#039;.

Onboarding App - Fim
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:abertura-conta-simples:sucesso",
        	eventAction: "abriu:box",
        	eventLabel: "valor-minimo-para-cartao-de-debito"
        });
```


<br />

- **Quando:** No clique do botão &#039;Ok&#039;.
- **Onde:** No box &#039;Valor mínimo para cartão de débito&#039; na tela &#039;Conta Simples aberta com sucesso&#039;.

Onboarding App - Fim
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:abertura-conta-simples:sucesso",
        	eventAction: "clique:botao",
        	eventLabel: "ok"
        });
```


<br />

### Transparência e Proteção de Dados

- **Onde:**  Visualização da tela &#039;Transparência e Proteção de Dados&#039;.


```javascript
    Analytics.logScreenView("/cadastro/transparencia-protecao-dados/");
```


<br />

- **Quando:** Ao selecionar ou desselecionar o checkbox para afirmar que &quot;Aceito compartilhar informações com Grupo Guararapes&quot; ou que &quot;Aceita que o banco consulte minhas informações no SCR&quot;
- **Onde:** Na tela de cadastro &quot;Transparência e Proteção de Dados&quot;
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:transparencia-protecao-dados",
        	eventAction: "[[selecionado ou desselecionado]]:checkbox",
        	eventLabel: "[[tipo-checkbox]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-checkbox]] | &#039;aceito-compartilhar-com-grupo-guararapes&#039;, aceito-consulte-minhas-informacoes-scr&#039; e etc | Deve retornar o nome do checkbox selecionado. |
| [[selecionado ou desselecionado]] | &#039;selecionado&#039; ou &#039;desselecionado&#039; | Deve retornar o status do checkbox, se está selecionado ou não. |

<br />

- **Quando:** No clique nos links de &quot;Grupo Guararapes&quot; e &quot;SCR&quot;
- **Onde:** Na tela de cadastro &quot;Transparência e Proteção de Dados&quot;
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:transparencia-protecao-dados",
        	eventAction: "clique:link",
        	eventLabel: "[[nome-link]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;gurpo-guararapes&#039;, &#039;scr&#039; e etc. | Deve retornar o nome do link clicado. |

<br />

- **Onde:** Visualização dos modais &#039;SCR&#039; e &#039;Grupo Guararapes&#039;


```javascript
    Analytics.logScreenView("/cadastro/transparencia-protecao-dados/modal-[[nome-modal]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-modal]] | &#039;scr&#039;, &#039;grupo-guararapes&#039; e etc. | Deve retornar o nome do modal exibido. |

<br />

- **Quando:** No clique no botão de &quot;Entendi&quot; e no link de &quot;Políticas de privacidade das empresas do grupo&quot;
- **Onde:** Nos modais de &quot;SCR&quot; e &quot;Grupo Guararapes&quot;
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:transparencia-protecao-dados",
        	eventAction: "clique:[[botao-ou-link]]:modal-[[nome-modal]]",
        	eventLabel: "[[item-clicado]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-ou-link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o elemento clicado. |
| [[nome-modal]] | &#039;scr&#039;, &#039;grupo-guararapes&#039; e etc. | Deve retornar o nome do modal exibido. |
| [[item-clicado]] | &#039;entendi&#039;, &#039;politicas-privacidade-empresas-grupo&#039; e etc | Deve retornar o item clicado. |

<br />

- **Onde:** Visualização do modal &#039;Poíticas de privacidade das empresas do grupo&#039;, dentro do moal de &#039;Grupo Guararapes&#039;


```javascript
    Analytics.logScreenView("/cadastro/transparencia-protecao-dados/modal-grupo-guararapes/politicas-privacidade-empresas-grupo/");
```


<br />

- **Quando:** No clique no botão de &quot;Li e concordo&quot; ou &quot;Voltar&quot;
- **Onde:** No modal &quot;Poíticas de privacidade das empresas do grupo&quot;, dentro do modal de &quot;Grupo Guararapes&quot;
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:transparencia-protecao-dados",
        	eventAction: "clique:botao:modal-politicas-privacidade-empresas-grupo",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;li-concordo&#039;, &#039;volta&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique no botão de &quot;Continuar&quot;
- **Onde:** Na tela &#039;Transparência e Proteção de Dados&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastro:transparencia-protecao-dados",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

### Login

- **Onde:** Visualização da tela &#039;Login&#039;.


```javascript
    Analytics.logScreenView("/login/");
```


<br />

- **Quando:** Após o preenchimento do campo &#039;senha&#039;.
- **Onde:** Na tela &#039;Login&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login",
        	eventAction: "preencheu:campo",
        	eventLabel: "senha"
        });
```


<br />

- **Quando:** No callback de sucesso/erro no preenchimento do campo de senha.
- **Onde:** Na tela &#039;Login&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:senha-incorreta&#039;, &#039;erro:senha-bloqueada&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada pelo usuário. |

<br />

- **Quando:** No clique dos links &#039;Esqueci minha senha&#039;, &#039;token&#039;, &#039;criar nova conta&#039;
- **Onde:** Na tela &#039;Login&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login",
        	eventAction: "clique:link",
        	eventLabel: "[[nome-link]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;esqueci-minha-senha&#039;, &#039;token&#039;, &#039;criar-nova-conta&#039; e etc | Deve retornar o nome do link clicado. |

<br />

- **Quando:** No clique do botão &#039;Entrar&#039;.
- **Onde:** Na tela &#039;Login&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login",
        	eventAction: "clique:botao",
        	eventLabel: "entrar"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Sucesso&#039; ou &#039;Erro&#039; após tentar realizar o login.


```javascript
    Analytics.logScreenView("/login/[[status]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:senha-incorreta&#039; e etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** No clique do botão de OK no callback de erro
- **Onde:** Na tela &#039;Login&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login",
        	eventAction: "clique:botao:callback",
        	eventLabel: "ok"
        });
```


<br />

- **Quando:** No clique do botão &#039;Ok&#039;.
- **Onde:** No modal &#039;Recuperar Senha&#039; aberto na tela &#039;Login&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha",
        	eventAction: "clique:botao",
        	eventLabel: "ok"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Esqueci minha senha&#039;.


```javascript
    Analytics.logScreenView("/login/recuperar-senha/");
```


<br />

- **Quando:** Após o preenchimento do campo &#039;cpf&#039;.
- **Onde:** Na tela &#039;Esqueci minha senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha",
        	eventAction: "preencheu:campo",
        	eventLabel: "cpf"
        });
```


<br />

- **Quando:** No clique do botão &#039;voltar&#039;.
- **Onde:** Na tela &#039;Esqueci minha senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```


<br />

- **Quando:** No callback de sucesso/erro no preenchimento do campo de &#039;cpf&#039;.
- **Onde:** Na tela &#039;Esqueci minha senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:senha-incorreta&#039;, &#039;erro:cpf-invalido&#039;, &#039;erro:cpf-nao-confere&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />


- **Onde:** Visualização das telas &#039;Token&#039;.


```javascript
    Analytics.logScreenView("/login/recuperar-senha/token-[[tipo-token]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-token]] | &#039;sms&#039;, &#039;email&#039; etc. | Deve retornar o tipo de token a ser enviado. |

<br />

- **Quando:** No clique do link &#039;Receber por e-mail&#039;.
- **Onde:** Nas telas &#039;Token&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:token-[[tipo-token]]",
        	eventAction: "clique:link",
        	eventLabel: "receber-por-email"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-token]] | &#039;sms&#039;, &#039;email&#039; etc. | Deve retornar o tipo de token a ser enviado. |

<br />

- **Quando:** No clique do botão &#039;voltar&#039;.
- **Onde:** Nas telas &#039;Token&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:token-[[tipo-token]]",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-token]] | &#039;sms&#039;, &#039;email&#039; etc. | Deve retornar o tipo de token a ser enviado. |

<br />

- **Quando:** Após o preenchimento do campo &#039;Código de Verificação&#039;.
- **Onde:** Nas telas &#039;Token&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:token-[[tipo-token]]",
        	eventAction: "preencheu:campo",
        	eventLabel: "codigo-verificacao"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-token]] | &#039;sms&#039;, &#039;email&#039; etc. | Deve retornar o tipo de token a ser enviado. |

<br />

- **Quando:** No callback de sucesso/erro após preenchimento do campo &#039;Código de Verificação&#039;.
- **Onde:** Nas telas &#039;Token&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:token-[[tipo-token]]",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-token]] | &#039;sms&#039;, &#039;email&#039; etc. | Deve retornar o tipo de token a ser enviado. |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:token-invalido&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Onde:** Visualização da  tela &#039;Token Inválido&#039;.


```javascript
    Analytics.logScreenView("/login/recuperar-senha/token-[[tipo-token]]/token-invalido/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-token]] | &#039;sms&#039;, &#039;email&#039; etc. | Deve retornar o tipo de token a ser enviado. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Token Inválido&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:token-[[tipo-token]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-token]] | &#039;sms&#039;, &#039;email&#039; etc. | Deve retornar o tipo de token a ser enviado. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;agora-nao&#039; etc. | Deve retornar o nome do botão clicado pelo usuário. |

<br />

- **Quando:** Na abertura do modal &#039;Atenção&#039;.
- **Onde:** Na tela &#039;Token Inválido&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:token-[[tipo-token]]",
        	eventAction: "abriu:modal",
        	eventLabel: "atencao"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-token]] | &#039;sms&#039;, &#039;email&#039; etc. | Deve retornar o tipo de token a ser enviado. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** No modal &#039;Atenção&#039; da tela &#039;Token Inválido&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:token-[[tipo-token]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-token]] | &#039;sms&#039;, &#039;email&#039; etc. | Deve retornar o tipo de token a ser enviado. |
| [[nome-botao]] | &#039;sair&#039;, &#039;cancelar&#039; etc. | Deve retornar o nome do botão clicado pelo usuário. |

<br />

- **Onde:**  Visualização de uma tela de erro.

```javascript
    Analytics.logScreenView("/login/recuperar-senha/[[status]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;cancelamento-por-politicas-internas&#039;, &#039;cancelamento-por-erro-sistemico&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** No clique do botão &#039;ok&#039;.
- **Onde:** Em uma das telas de erro.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:erro",
        	eventAction: "clique:botao",
        	eventLabel: "ok"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-erro]] | &#039;cancelamento-por-politicas-internas&#039;, &#039;cancelamento-por-erro-sistemico&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Onde:** Visualização da tela &#039;Regras de Senha&#039;.

```javascript
    Analytics.logScreenView("/login/recuperar-senha/regras-de-senha/");
```


<br />

- **Quando:** No clique do botão &#039;continuar&#039;.
- **Onde:** Na tela &#039;Regras de Senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:regras-de-senha",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Nova Senha&#039;.

```javascript
    Analytics.logScreenView("/login/recuperar-senha/nova-senha/");
```


<br />

- **Quando:** Após preenchimento do campo &#039;nova senha&#039;.
- **Onde:** Na tela &#039;Nova Senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:nova-senha",
        	eventAction: "preencheu:campo",
        	eventLabel: "nova-senha"
        });
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Nova Senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:nova-senha",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;visualizar-senha&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Na abertura do modal &#039;Senha Fraca&#039;.
- **Onde:** Na tela &#039;Nova Senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:nova-senha",
        	eventAction: "abriu:modal",
        	eventLabel: "senha-fraca"
        });
```


<br />

- **Quando:** No clique do botão &#039;ok&#039;.
- **Onde:** No modal &#039;Senha Fraca&#039; da tela &#039;Nova Senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:nova-senha",
        	eventAction: "clique:botao",
        	eventLabel: "ok"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Confirmação de Senha&#039;.

```javascript
    Analytics.logScreenView("/login/recuperar-senha/confirmacao-senha/");
```


<br />

- **Quando:** Após preenchimento do campo &#039;confirmar senha&#039;.
- **Onde:** Na tela &#039;Confirmação de Senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:confirmacao-senha",
        	eventAction: "preencheu:campo",
        	eventLabel: "confirmar-senha"
        });
```


<br />

- **Quando:** No clique de um dos botões.
- **Onde:** Na tela &#039;Confirmação de Senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:confirmacao-senha",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;visualizar-senha&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No callback de sucesso/erro após preenchimento do campo &#039;confirmar senha&#039;.
- **Onde:** Na tela &#039;Confirmação de Senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:confirmacao-senha",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:senha-digitada-diferente-da-anterior&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Onde:** Visualização da tela  &#039;Sua senha foi criada com sucesso &#039;.

```javascript
    Analytics.logScreenView("/login/recuperar-senha/senha-criada/");
```


<br />

- **Quando:** No clique do botão &#039;fazer login&#039;.
- **Onde:** Na tela &#039;Sua senha foi criada com sucesso&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:senha-criada",
        	eventAction: "clique:botao",
        	eventLabel: "fazer-login"
        });
```


<br />

- **Quando:** No callback de sucesso/erro na recuperação de senha.
- **Onde:** Na tela &#039;Sua senha foi criada com sucesso&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:recuperar-senha:senha-criada",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:nao-foi-possivel-recuperar-senha&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** Na abertura do modal &#039;Autorize seu Aparelho&#039;.
- **Onde:** Na tela &#039;Login&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:autorizacao-aparelho",
        	eventAction: "abriu:modal",
        	eventLabel: "autorize-seu-aparelho"
        });
```


<br />

- **Quando:** No clique do botão &#039;Continuar&#039;.
- **Onde:** No modal &#039;Autorize seu Aparelho&#039; na tela &#039;Login&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:autorizacao-aparelho",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Autorize seu Aparelho&#039;.

```javascript
    Analytics.logScreenView("/login/autorize-seu-aparelho/");
```


<br />

- **Quando:** No clique de algum dos botões.
- **Onde:** Na tela &#039;Autorize seu Aparelho&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:autorizacao-aparelho",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;agora-nao&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização do modal  &#039;Deletar dispositivos&#039;.

```javascript
    Analytics.logScreenView("/login/autorize-seu-aparelho/deletar/");
```


<br />

- **Quando:** No clique de algum dos botões.
- **Onde:** No modal de &quot;Deletar dispositivos&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:autorizacao-aparelho",
        	eventAction: "clique:botao:modal-deletar-dispositivo",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;sim-deletar&#039;, &#039;cancelar&#039;  etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;Dispositivo Desautorizado&#039;.

```javascript
    Analytics.logScreenView("/login/autorize-seu-aparelho/deletar/desautorizado/");
```


<br />

- **Quando:** No clique de algum dos botões.
- **Onde:** No modal de &quot;Dispositivo Desautorizado&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:dispositivo-desautorizado",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;autorizar&#039;  etc. | Deve retornar o nome do botão clicado. |

<br />


- **Onde:** Na tela &#039;Dispositivo Autorizado&#039;.

```javascript
    Analytics.logScreenView("/login/autorize-seu-aparelho/autorizado/");
```


<br />

- **Quando:** No clique do botão &#039;Fazer Login&#039;.
- **Onde:** Na tela &#039;Dispositivo Autorizado&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:autorizacao-aparelho",
        	eventAction: "clique:botao",
        	eventLabel: "fazer-login"
        });
```


<br />

- **Quando:** No callback de sucesso/erro na autorização do dispositivo.
- **Onde:** Na tela &#039;Dispositivo Autorizado&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:login:autorizacao-aparelho",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:nao-foi-possivel-autorizar-aparelho&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

### Abertura Conta Completa Ilimitada


- **Onde:** Visualização da tela de &#039;falta pouco&#039;.

```javascript
    Analytics.logScreenView("/conta-ilimitada/falta-pouco/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela de &#039;falta pouco&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:falta-pouco",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;abrir-conta&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />


- **Onde:** Visualização da tela de cadastramento de documento.

```javascript
    Analytics.logScreenView("/conta-ilimitada/falta-pouco/cadastro-documento/");
```


<br />

- **Quando:** Ao clicar em uma das opções para cadastrar o documento.
- **Onde:** Na tela de cadastramento de documento.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:falta-pouco:cadastro-documento",
        	eventAction: "clique:card",
        	eventLabel: "[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento escolhido. |

<br />


- **Onde:**  Visualização da tela do documento escolhido para ser cadastrado.

```javascript
    Analytics.logScreenView("/conta-ilimitada/falta-pouco/cadastro-documento/[[tipo-documento]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento selecionado pelo usuário. |

<br />

- **Quando:** Ao preencher os campos do formulário.
- **Onde:** Na tela do documento escolhido para ser cadastrado.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cadastro-documento:[[tipo-documento]]",
        	eventAction: "preencheu:campo",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o documento selecionado pelo usuário. |
| [[nome-campo]] |  | Deve retornar o nome do campo preenchido. Ex.: &#039;numero-documento&#039;, &#039;data-expedicao&#039;, &#039;orgao-emissor&#039;, &#039;data-validade&#039; etc. |

<br />

- **Quando:** Na interação com o select de &#039;Estado Expedidor&#039; do formulário.
- **Onde:** Na tela do documento escolhido para ser cadastrado.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cadastro-documento:[[tipo-documento]]",
        	eventAction: "select:opcao",
        	eventLabel: "estado-expedidor"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o documento selecionado pelo usuário. |

<br />

- **Quando:** Ao clicar no botão &#039;Tirar foto do documento&#039; após preencher os dados do formulário.
- **Onde:** Na tela do documento escolhido para ser cadastrado.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cadastro-documento:[[tipo-documento]]",
        	eventAction: "clique:botao",
        	eventLabel: "tirar-foto-documento"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento selecionado pelo usuário. |

<br />

- **Quando:** Ao clicar em um dos botões.
- **Onde:** Na tela do documento escolhido para ser cadastrado.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cadastro-documento:[[tipo-documento]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento selecionado pelo usuário. |
| [[nome-botao]] | &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Ao clicar em um dos links.
- **Onde:** Na tela do documento escolhido para ser cadastrado.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cadastro-documento:[[tipo-documento]]",
        	eventAction: "clique:link",
        	eventLabel: "[[nome-link]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento selecionado pelo usuário. |
| [[nome-link]] | &#039;terminar-depois&#039; etc. | Deve retornar o nome do link clicado. |

<br />


- **Onde:** Visualização da tela de dicas para a captura da imagem do documento.


```javascript
    Analytics.logScreenView("/conta-ilimitada/abertura/cadastro-documento/[[tipo-documento]]/dicas-captura-imagem/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento selecionado pelo usuário. |

<br />

- **Quando:** Ao clicar no botão apresentado na última etapa das dicas para a captura da imagem do documento.
- **Onde:** Na tela de dicas para captura da imagem do documento.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cadastro-documento:[[tipo-documento]]:dicas-captura-imagem",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento selecionado pelo usuário. |

<br />


- **Onde:** Visualização da tela de captura de documento (frente e verso).

```javascript
    Analytics.logScreenView("/conta-ilimitada/abertura/cadastro-documento/[[tipo-documento]]/captura-imagem/[[lado-documento]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento selecionado pelo usuário. |
| [[lado-documento]] | &#039;frente&#039; ou &#039;verso&#039;. | Deve retornar se a captura é da frente ou do verso do documento. |

<br />

- **Quando:** Ao clicar em um dos botões.
- **Onde:** Na tela de captura de documento (frente e verso).

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cadastro-documento:[[tipo-documento]]:captura-imagem/[[lado-documento]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento selecionado pelo usuário. |
| [[lado-documento]] | &#039;frente&#039; ou &#039;verso&#039;. | Deve retornar se a captura é da frente ou do verso do documento. |
| [[nome-botao]] | &#039;voltar&#039;, &#039;capturar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />



- **Onde:** Visualização da tela de confirmação da captura de documento (frente e verso).

```javascript
    Analytics.logScreenView("/conta-ilimitada/abertura/cadastro-documento/[[tipo-documento]]/captura-imagem/[[lado-documento]]/confirmacao/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento selecionado pelo usuário. |
| [[lado-documento]] | &#039;frente&#039; ou &#039;verso&#039;. | Deve retornar se a captura é da frente ou do verso do documento. |

<br />

- **Quando:** Ao clicar em um dos botões.
- **Onde:** Na tela de confirmação da captura de documento (frente ou verso).

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cadastro-documento:[[tipo-documento]]:captura-imagem/[[lado-documento]]:confirmacao",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento selecionado pelo usuário. |
| [[lado-documento]] | &#039;frente&#039; ou &#039;verso&#039;. | Deve retornar se a captura é da frente ou do verso do documento. |
| [[nome-botao]] | &#039;voltar&#039;, &#039;continuar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Ao clicar em um dos links.
- **Onde:** Na tela de confirmação da captura de documento (frente ou verso).

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cadastro-documento:[[tipo-documento]]:captura-imagem/[[lado-documento]]:confirmacao",
        	eventAction: "clique:link",
        	eventLabel: "[[nome-link]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-documento]] | &#039;rg&#039;, &#039;cnh&#039; ou &#039;rne&#039;. | Deve retornar o nome do documento selecionado pelo usuário. |
| [[lado-documento]] | &#039;frente&#039; ou &#039;verso&#039;. | Deve retornar se a captura é da frente ou do verso do documento. |
| [[nome-link]] | &#039;capturar-novamente&#039; etc. | Deve retornar o nome do link clicado. |

<br />


- **Onde:** Visualização da tela &#039;Dados Cadastrais&#039;.

```javascript
    Analytics.logScreenView("/conta-ilimitada/abertura/dados-cadastrais/");
```


<br />

- **Quando:** Após preencher os campos do formulário de &#039;Dados Cadastrais&#039;.
- **Onde:** Na tela &#039;Dados Cadastrais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "preencheu:campo",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;data-nascimento&#039;, &#039;nome-mae&#039;, &#039;celular&#039; etc. | Deve retornar o nome do campo preenchido. |

<br />

- **Quando:** Na interação com um dos selects do formulário.
- **Onde:** Na tela &#039;Dados Cadastrais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "select:opcao",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;genero&#039;, &#039;nacionalidade&#039;, &#039;uf-naturalidade&#039;, &#039;naturalidade&#039; etc. | Deve retornar o nome do select com o qual o usuário interagiu. |

<br />

- **Quando:** Ao selecionar o checkbox para afirmar que é &#039;Pessoa com Deficiência&#039;.
- **Onde:** Na tela &#039;Dados Cadastrais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "select:checkbox",
        	eventLabel: "sou-pcd"
        });
```


<br />

- **Quando:** Ao clicar em um dos botões.
- **Onde:** Na tela &#039;Dados Cadastrais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Ao clicar no link &#039;Terminar Depois&#039;.
- **Onde:** Na tela &#039;Dados Cadastrais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "clique:link",
        	eventLabel: "terminar-depois"
        });
```


<br />

- **Quando:** No callback para a conclusão de preenchimento do cadastro.
- **Onde:** Na tela &#039;Dados Cadastrais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "callback:cadastro-completo",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:data-de-nascimento-invalida&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Onde:** Visualização da tela &#039;Dados Adicionais&#039;.



```javascript
    Analytics.logScreenView("/abertura-conta-ilimitada/dados-adicionais/");
```


<br />

- **Quando:** Após preencher os campos do formulário.
- **Onde:** Na tela &#039;Dados Adicionais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "preencheu:campo",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;renda-mensal&#039;, &#039;patrimonio&#039; etc. | Deve retornar o nome do campo preenchido. |

<br />

- **Quando:** Na interação com um dos selects do formulário.
- **Onde:** Na tela &#039;Dados Adicionais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "select:opcao",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;estado-civil&#039;, &#039;profissao&#039; etc. | Deve retornar o nome do select com o qual o usuário interagiu. |

<br />

- **Quando:** Ao clicar em um dos botões &#039;Sim&#039; ou &#039;Não&#039;.
- **Onde:** Na tela &#039;Dados Adicionais&#039;, na seção &#039;Sou ou tenho alguma relação com uma pessoa exposta publicamente (PEP)&#039;.

Dados adicionais
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;sim&#039; ou &#039;nao&#039;. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Ao clicar em um dos botões.
- **Onde:** Na tela &#039;Dados Adicionais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;continuar&#039;, &#039;info&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Ao clicar em um dos links.
- **Onde:** Na tela &#039;Dados Adicionais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "clique:link",
        	eventLabel: "[[nome-link]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;saiba-mais&#039;, &#039;terminar-depois&#039; etc. | Deve retornar o nome do link clicado. |

<br />

- **Quando:** Na abertura de um dos modais.
- **Onde:** Na tela &#039;Dados Adicionais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-cadastrais",
        	eventAction: "abriu:modal",
        	eventLabel: "[[nome-modal]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-modal]] | &#039;pessoa-exposta-politicamente&#039;, &#039;patrimonio&#039; etc. | Deve retornar o nome do modal aberto. |

<br />

- **Quando:** No clique do botão &#039;Entendi&#039;.
- **Onde:** Em um dos modais da tela &#039;Dados Adicionais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-adicionais:[[nome-modal]]",
        	eventAction: "clique:botao",
        	eventLabel: "entendi"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-modal]] | &#039;pessoa-exposta-politicamente&#039;, &#039;patrimonio&#039; etc. | Deve retornar o nome do modal aberto. |

<br />

- **Quando:** No callback para a conclusão de preenchimento do cadastro.
- **Onde:** Na tela &#039;Dados Adicionais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:dados-adicionais",
        	eventAction: "callback:cadastro-completo",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:preencha-o-estado-civil&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Onde:** Visualização da tela &#039;Salvando seus dados&#039;.


```javascript
    Analytics.logScreenView("/abertura-conta-ilimitada/salvando-seus-dados/");
```


<br />

- **Quando:** No callback de finalização de cadastro.
- **Onde:** Na tela &#039;Salvando seus dados&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:salvando-seus-dados",
        	eventAction: "callback:salvando-seus-dados",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:nao-foi-possivel-realizar-seu-cadastro&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Onde:** Visualização da tela &#039;Em Análise&#039;.


```javascript
    Analytics.logScreenView("/abertura-conta-ilimitada/em-analise/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Em Análise&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:em-analise",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;ok&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela inicial de &#039;Cestas de Serviços&#039;.


```javascript
    Analytics.logScreenView("/abertura-conta-ilimitada/cesta-servicos/inicio/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela inicial de &#039;Cesta de Serviços&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cesta-servicos:inicio",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique do link &#039;Terminar Depois&#039;.
- **Onde:** Na tela inicial de &#039;Cesta de Serviços&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cesta-servicos:inicio",
        	eventAction: "clique:link",
        	eventLabel: "terminar-depois"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Cesta de Serviços&#039;.


```javascript
    Analytics.logScreenView("/abertura-conta-ilimitada/falta-pouco/cesta-beneficios/");
```


<br />

- **Quando:** Ao clicar em uma das opções de &#039;Cestas de Serviços Midway&#039;.
- **Onde:** Na tela &#039;Cesta de Serviços&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cesta-servicos",
        	eventAction: "clique:card",
        	eventLabel: "[[nome-pacote]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-pacote]] | &#039;pacote-basico&#039;, &#039;pacote-light&#039;, &#039;pacote-premium&#039; etc. | Deve retornar o nome do pacote selecionado. |

<br />

- **Quando:** Ao clicar no botão
- **Onde:** Na tela &#039;Cesta de Serviços&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:falta-pouco:cesta-servicos",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

- **Quando:** Ao clicar em um dos links.
- **Onde:** Na tela &#039;Cesta de Serviços&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:falta-pouco:cesta-servicos",
        	eventAction: "clique:link",
        	eventLabel: "[[nome-link]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;saiba-mais&#039;, &#039;terminar-depois&#039; etc. | Deve retornar o nome do link clicado. |

<br />

- **Onde:** Visualização da  tela &#039;Data de Cobrança&#039;.


```javascript
    Analytics.logScreenView("/abertura-conta-ilimitada/data-cobranca/");
```


<br />

- **Quando:** Na interação com o slider de data de cobrança.
- **Onde:** Na tela &#039;Data de Cobrança&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:data-cobranca",
        	eventAction: "interacao:slider",
        	eventLabel: "[[dia-cobranca]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[dia-cobranca]] | &#039;14&#039;, &#039;25&#039; etc. | Deve retornar o dia escolhido pelo cliente. |

<br />

- **Quando:** Ao clicar em um dos botões.
- **Onde:** Na tela &#039;Data de Cobrança&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:data-cobranca",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;selecionar&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado pelo usuário. |

<br />

- **Onde:** Visualização da  tela de oferta de cartão.

Oferta de cartão
```javascript
    Analytics.logScreenView("/abertura-conta-ilimitada/oferta-cartao/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela de oferta de cartão.

Oferta de cartão
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:oferta-cartao",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;agora-nao&#039;, &#039;voltar&#039;, &#039;compartilhar&#039; etc. | Deve retornar o nome do botão clicado pelo usuário. |

<br />

- **Quando:** Após o clique no botão &quot;aceito&quot; na página de proposta ou &quot;continuar&quot; em &quot;Produtos Contratados&quot;
- **Onde:** Na tela de oferta de cartão.

Oferta de cartão
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:oferta-cartao",
        	eventAction: "produto:[[contrato-proposta]]",
        	eventLabel: "[[produto-contratado]]:[[valor]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[contrato-proposta]] |  | Deve retornar entre proposta para o clique no aceito e contrato para o clique no continuar  |
| [[produto-contratado]] |  | Deve retornar o qual o produto que esta sendo contratado pelo cliente. Ex.: &#039;cartao-de-credito&#039;, &#039;limite-emergencial&#039;, etc |
| [[valor]] |  | Deve retornar o valor do produto que esta sendo contratado pelo cliente. Ex.: &#039;2000&#039;, &#039;3000&#039;, cesta-de-servico&#039;, etc |

<br />

- **Quando:** No clique do link &#039;Consulte taxas e condições&#039;.
- **Onde:** Na tela de oferta de cartão.

Oferta de cartão
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:oferta-cartao",
        	eventAction: "clique:link",
        	eventLabel: "consulte-taxas-e-condicoes"
        });
```


<br />

- **Quando:** Na abertura do box &#039;Cartão de crédito&#039;.
- **Onde:** Na tela de oferta de cartão.

Oferta de cartão
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:oferta-cartao",
        	eventAction: "box:abriu",
        	eventLabel: "cartao-de-credito"
        });
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** No box &#039;Cartão de crédito&#039;, na tela de oferta de cartão.

Oferta de cartão
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:oferta-cartao",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;sim&#039; ou &#039;agora-nao&#039;. | Deve retornar o nome do botão clicado pelo usuário. |

<br />


- **Onde:** Visualização da tela &#039;Produtos contratados&#039;.

```javascript
    Analytics.logScreenView("/abertura-conta-ilimitada/produtos-contratados/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Produtos contratados&#039;.

Resumo de produtos cadastrados
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:produtos-contratados",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;compartilhar&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado pelo usuário. |

<br />

- **Quando:** No clique em um dos cards de produto.
- **Onde:** Na tela de &#039;Produtos contratados&#039;.

Resumo de produtos cadastrados
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:produtos-contratados",
        	eventAction: "clique:card",
        	eventLabel: "[[nome-produto]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;cesta-de-servicos&#039;, &#039;limite-emergencial&#039;, &#039;cartao-de-credito&#039; etc. | Deve retornar o nome do produto do card clicado. |

<br />


- **Onde:** Visualização da tela de &#039;Termo de Aceite&#039;.



```javascript
    Analytics.logScreenView("/abertura-conta-ilimitada/termo-de-aceite/");
```


<br />

- **Quando:** Ao clicar em um dos botões.
- **Onde:** Na tela de &#039;Termo de Aceite&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:termo-de-aceite",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;li-e-concordo&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado pelo usuário. |

<br />


- **Onde:** Visualização da tela &#039;Seu cartão está a caminho&#039; ou &#039;Seu cartão será enviado&#039;


```javascript
    Analytics.logScreenView("/abertura-conta-ilimitada/cartao-enviado/");
```

<br />

- **Quando:** Ao clicar no botão de &#039;OK&#039;.
- **Onde:** Na tela &#039;Seu cartão está a caminho&#039; ou &#039;Seu cartão será enviado&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cartao-enviado",
        	eventAction: "clique:botao",
        	eventLabel: "ok"
        });
```


<br />

- **Quando:** No clique em um dos cards de endereco.
- **Onde:** Na tela &#039;Seu cartão está a caminho&#039; ou &#039;Seu cartão será enviado&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:cartao-enviado",
        	eventAction: "clique:card",
        	eventLabel: "endereco-selecionado"
        });
```


<br />


- **Onde:** Visualização da tela &#039;Conta Digital Aberta com Sucesso&#039;.


```javascript
    Analytics.logScreenView("/abertura-conta-ilimitada/falta-pouco/cesta-beneficios/sucesso/");
```


<br />

- **Quando:** Ao clicar no botão de &#039;Entrar na minha conta Midway&#039;.
- **Onde:** Na tela &#039;Conta Digital Aberta com Sucesso&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:falta-pouco:sucesso",
        	eventAction: "clique:botao",
        	eventLabel: "entrar-conta-midway"
        });
```


<br />

- **Quando:** Ao clicar no link &#039;Compartilhar&#039;.
- **Onde:** Na tela &#039;Conta ilimitada Aberta com Sucesso&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:sucesso",
        	eventAction: "clique:link",
        	eventLabel: "compartilhar"
        });
```


<br />

- **Quando:** No callback de sucesso/erro na abertura da conta ilimitada.
- **Onde:** Na tela &#039;Conta ilimitada Aberta com Sucesso&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-ilimitada:abertura:sucesso",
        	eventAction: "callback:abertura-conta-ilimitada",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:nao-foi-possivel-abrir-conta-ilimitada&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />


### Pagamento Boleto

- **Onde:** Visualização das telas de 'Camêra de Pagamento de Boleto', 'Digite codigo de barras', 'Pagamento de boleto' , do modal de 'Saldo Insuficiente', 'Pagamento Fora do Horario', 'Pagamento Realizado', 'Pagamento Agendado', 'Comprovante de pagamento', e 'Comprovante de Agendamento'



```javascript
    Analytics.logScreenView("/boleto/[[tela]]");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tela]] | 'digite-codigo-de-barras', 'camera', 'pagamento', 'modal/saldo-insuficiente', 'pagamento-fora-do-horario', 'pagamento-agendado', 'pagamento-realizado', 'comprovante-pagamento', 'comprovante-agendamento' | Deve retornar a tela em que o usuario esta|

<br />

- **Quando:** No callback de sucesso ou erro ao realizar o scanner da foto do código de barras
- **Onde:** Na tela de 'Pagamento de Boleto por Camera'.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:boleto-camera",
        	eventAction: "callback:pagamento",
        	eventLabel: "[[sucesso-erro]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso-erro]] | 'sucesso', 'erro:codigo-de-barras-invalido' e etc |Deve retorna se o scanner do código de barras foi realizado com sucesso ou não. . |

<br />

- **Quando:** No callback de sucesso ou erro ao realizar o preenchimento do código de barras
- **Onde:** Na tela de 'Pagamento de Boleto por Codigo de barras'.
- 
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:boleto-digite-codigo-de-barras",
        	eventAction: "callback:codigo-de-barras",
        	eventLabel: "[[sucesso-erro]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso-erro]] | 'sucesso', 'erro:codigo-de-barras-invalido' e etc |Deve retorna se o scanner do código de barras foi realizado com sucesso ou não. . |

<br />

- **Quando:** No clique de todos os botões, icones e links 
- **Onde:** o fluxo de 'Pagamento de Boleto'.
- 
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:boleto-[[secao]]",
        	eventAction: "clique:[[tipo-item]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[secao]] | 'camera', 'codigo-de-barras',  'saldo-insuficiente', 'pagamento-de-boleto', 'pagamento-fora-do-horario', 'comprovante-pagamento', 'comprovante-agendamento', 'pagamento-realizado', 'pagamento-agendado' e etc.|Deve retornar o nome da seção em que ocorreu o clique. |
| [[tipo-item]] |  'botao', 'link' ou 'icone' .|Deve retornar se o item é um botão, link ou icone. |
| [[nome-item]] |  'digite-o-codigo-de-barras', 'continuar', 'voltar', 'limpar', 'camera', 'pagar', 'ok-entendi', 'agendar-um-pagamento', 'compartilhar', 'ver-comprovante', 'fazer-outro-pagamento' e etc | Deve retornar o nome do item clicado. |

<br />



- **Quando:** No clique do botão 'Pagar'
- **Onde:** No tela de 'Pagamento de Boleto'.
- 
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:boleto-pagamento",
        	eventAction: "clique:pagar",
        	eventLabel: "[[situacao-pagamento]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[situacao-pagamento]] | 'pagamento-realizado-hoje', 'pagamento-agendado-para-12-02-2021'| Deve retornar se o pagamento foi realizado hoje ou foi agendado, se foi agendado trazer a data de agendamento.|

<br />


- **Quando:** No preenchimento dos campos
- **Onde:** No fluxo de 'Pagamento de Boleto'.
- 
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:boleto-[[secao]]",
        	eventAction: "preencheu:campo",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[secao]] |'codigo-de-barras',  'pagamento' e etc.|Deve retornar o nome da seção em que ocorreu o clique. |
| [[nome-campo]]  |  'codigo-de-barras', 'identificacao-extrato' e etc . |Deve retornar o nome do campo preenchido.  |

<br />


### Extrato

- **Onde:** Visualização da tela &#039;Extrato&#039;.

```javascript
    Analytics.logScreenView("/[[corrente ou pagamento]]/extrato/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos cards.
- **Onde:** Na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato",
        	eventAction: "clique:card",
        	eventLabel: "servicos:[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |
| [[nome-card]] | &#039;pagamento&#039;, &#039;transferencia&#039;, &#039;desbloquear-cartao&#039; etc. | Deve retornar o nome do card clicado. |

<br />

- **Quando:** No clique em uma das abas.
- **Onde:** Na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato",
        	eventAction: "clique:aba",
        	eventLabel: "[[nome-aba]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |
| [[nome-aba]] | &#039;tudo&#039;, &#039;entradas&#039;, &#039;saidas&#039;, &#039;futuro&#039; etc. | Deve retornar o nome da aba clicada. |

<br />

- **Quando:** No clique dos botões &#039;expandir&#039; ou &#039;reduzir&#039; os detalhes do limite.
- **Onde:** Na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;expandir&#039; ou &#039;reduzir&#039; | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique em um dos botões da tela de &#039;Comprovante de transferência&#039;
- **Onde:** Na tela referente ao Detalhe da transferência após o clique em uma das opções no Extrato

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:comprovante-da-transferencia",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;compartilhar&#039; ou &#039;fazer-outra-transferencia&#039; | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique em um dos botões da tela de compartilhamento de comprovante.
- **Onde:** Na tela de compartilhamento após o clique em &#039;Compartilhar&#039; em Detalhe da transferência.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:compartilhar",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;fechar&#039; ou &#039;enviar-comprovante&#039; | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique do botão &#039;informação&#039;.
- **Onde:** Na tela &#039;Extrato&#039;, após clique em &#039;expandir&#039; detalhes do limite.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:limite-emergencial",
        	eventAction: "clique:botao",
        	eventLabel: "informacao-limite-emergencial"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do link do dia de &#039;cobrança mensal do uso&#039;.
- **Onde:** Na tela &#039;Extrato&#039;, após clique em &#039;expandir&#039; detalhes do limite.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway[[corrente ou pagamento]]:extrato:limite-emergencial",
        	eventAction: "clique:link",
        	eventLabel: "cobranca-mensal-uso"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />

- **Onde:** Visualização da tela  &#039;Data de Cobrança &#039;.

```javascript
    Analytics.logScreenView("/[[corrente ou pagamento]]/extrato/limite-emergencial/data-de-cobranca/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** Na interação com o slider de &#039;dia&#039;.
- **Onde:** Na tela &#039;Escolha a data de cobrança mensal do seu limite emergencial&#039;, após clique do link do dia de &#039;cobrança mensal do uso&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:limite-emergencial:data-de-cobranca",
        	eventAction: "interacao:slider",
        	eventLabel: "[[dia-escolhido]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |
| [[dia-escolhido]] | &#039;1&#039;, &#039;20&#039;, &#039;15&#039; etc. | Deve retornar o novo dia escolhido para cobrança mensal do limite emergencial. |

<br />

- **Quando:** No callback de sucesso ou erro para a validação do token .
- **Onde:** Na tela de validação após a seleção da nova data de cobrança.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:limite-emergencial:data-de-cobranca",
        	eventAction: "callback:validacao-token",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |
| [[status]] | &#039;sucesso&#039;, &#039;erro:token-invalido&#039;, etc | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** No clique do botão concluir.
- **Onde:** Na tela de &#039;Data de cobrança de limite alterada ;)&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:limite-emergencial:data-de-cobranca",
        	eventAction: "clique:botao",
        	eventLabel: "concluir"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do link da quantidade de dias de uso.
- **Onde:** Na tela &#039;Extrato&#039;, após clique em &#039;expandir&#039; detalhes do limite..

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:limite-emergencial:data-de-cobranca",
        	eventAction: "clique:link",
        	eventLabel: "dias-de-uso-limite-emergencial"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />

- **Onde:** Visualização da tela de 'Dias de uso do limite emergencial'.

```javascript
    Analytics.logScreenView("/[[corrente ou pagamento]]/extrato/limite-emergencial/dias-de-uso/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do botão &#039;Gerar PDF&#039;
- **Onde:** Na tela de &#039;Dias de uso do limite emergencial&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:limite-emergencial:dias-de-uso",
        	eventAction: "clique:botao",
        	eventLabel: "gerar-pdf"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />

- **Onde:** Visualização da tela de detalhes de uma determinado data.



```javascript
    Analytics.logScreenView("/[[corrente ou pagamento]]/extrato/limite-emergencial/dias-de-uso/detalhes/[[data-clicada]]");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |
| [[data-clicada]] |  | Deve retornar a data referente ao detalhe do dia de uso do limite emergencial que foi exibido para o cliente. Ex.: &#039;12.04.2020&#039;, &#039;15.11.2020&#039;, etc |

<br />

- **Quando:** No clique do link &#039;filtrar&#039;.
- **Onde:** Na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato",
        	eventAction: "clique:link",
        	eventLabel: "filtrar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />


- **Onde:** Visualização da tela de &#039;Filtro&#039;.


```javascript
    Analytics.logScreenView("/[[corrente ou pagamento]]/extrato/filtro/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em uma das opções de &#039;período em dias&#039;.
- **Onde:** No modal &#039;Filtro&#039;, aberto após clique do link &#039;filtrar&#039; na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:filtro",
        	eventAction: "clique:opcao",
        	eventLabel: "[[periodo-em-dias]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |
| [[periodo-em-dias]] | &#039;30&#039;, &#039;60&#039;, &#039;90&#039;, &#039;180&#039;, &#039;365&#039; etc. | Deve retornar o período em dias clicado. |

<br />

- **Quando:** Após o preenchimento de um dos campos.
- **Onde:** No modal &#039;Filtro&#039;, aberto após clique do link &#039;filtrar&#039; na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:filtro",
        	eventAction: "preencheu:campo",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |
| [[nome-campo]] | &#039;de&#039;, &#039;ate&#039; etc. | Deve retornar o nome do campo preenchido. |

<br />

- **Quando:** No clique do botão &#039;filtrar&#039;.
- **Onde:** No modal &#039;Filtro&#039;, aberto após clique do link &#039;filtrar&#039; na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:filtro",
        	eventAction: "clique:botao",
        	eventLabel: "filtrar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />



- **Quando:** No clique do botão &#039;Entendi&#039; na tela de &#039;Comprovante de transferência&#039; com Conversão de moeda
- **Onde:** Na tela referente ao Detalhe da transferência para uma compra internacional
- **Link:** 6

```javascript

        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:comprovante-da-transferencia:conversao-de-moeda",
        	eventAction: "clique:botao" ,
        	eventLabel: "entendi" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do botão para ver &quot;informações do Cambio&quot;, na tela de &quot;Comprovante de transferencia&quot; com conversão de moedo
- **Onde:** Na tela referente ao Detalhe da transferência para uma compra internacional

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:comprovante-da-transferencia:conversao-de-moeda" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "informacoes-cambio" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |


<br />

-**Na visualização do modal de &quot;Cotação&quot; que aparece após clicar em  &quot;informações do Cambio&quot;.**

```javascript
    Analytics.logScrenView("/[[conta simples ou conta completa]]/cambio/cotacao")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |


<br />

- **Quando:** No clique em todos os botões do modal de &quot;Cotação&quot;
- **Onde:** No  modal de &quot;Cotação&quot; que aparece após clicar em  &quot;informações do Cambio&quot;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[corrente ou pagamento]]:extrato:comprovante-da-transferencia:conversao-de-moeda" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;, &#039;ok&#039;, e etc | deve retornar o nome do botão clicado. |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />



### Transferência

- **Onde:** Visualização da tela &#039;Para que conta quer transferir?&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/transferencia/destino-transferencia/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** Na escolha de uma das opções do select &#039;Para quem é a transferência?&#039;.
- **Onde:** Na tela &#039;Para que conta quer transferir?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:destino-transferencia",
        	eventAction: "select:opcao",
        	eventLabel: "para-quem-e-a-transferencia"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** Na interação com o select &#039;Instituição Financeira&#039;.
- **Onde:** Na tela &#039;Para que conta quer transferir?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:destino-transferencia",
        	eventAction: "clique:select",
        	eventLabel: "instituicao-financeira"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em uma das instituições financeiras.
- **Onde:** No modal &#039;Listagem de Bancos&#039; aberto após clique no select &#039;Instituição Financeira&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:destino-transferencia",
        	eventAction: "clique:lista",
        	eventLabel: "[[nome-instituicao-financeira]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-instituicao-financeira]] | &#039;banco-itau&#039;, &#039;nubank&#039;, &#039;banco-do-brasil&#039; etc. | Deve retornar o nome da instituição financeira selecionada. |

<br />

- **Quando:** No callback de sucesso após selecionar uma instituição financeira.
- **Onde:** No modal &#039;Listagem de Bancos&#039; aberto após clique no select &#039;Instituição financeira&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:destino-transferencia",
        	eventAction: "callback",
        	eventLabel: "sucesso"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** Após preenchimento do campo de busca.
- **Onde:** No modal &#039;Listagem de Bancos&#039; aberto após clique no select &#039;Instituição Financeira&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:destino-transferencia",
        	eventAction: "preencheu:campo",
        	eventLabel: "busca"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** Após o preenchimento de um dos campos do formulário.
- **Onde:** Na tela &#039;Para que conta quer transferir?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:destino-transferencia",
        	eventAction: "preencheu:campo",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039; | Deve retornar o tipo da conta. |
| [[nome-campo]] | &#039;agencia&#039;, &#039;conta-e-digito&#039;, &#039;nome-razao-social&#039;, &#039;cpf-cnpj&#039; etc. | Deve retornar o nome do campo preenchido. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Para que conta quer transferir?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:destino-transferencia",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No callback de sucesso/erro após preenchimento de um dos campos.
- **Onde:** Na tela &#039;Para que conta quer transferir?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:destino-transferencia",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:conta-inexistente&#039;, &#039;erro:conta-impossibilitada-de-receber-transacoes&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Onde:**  Visualização da tela &#039;Qual valor quer transferir?&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/transferencia/valor-transferencia/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** Após preenchimento do campo &#039;valor&#039;.
- **Onde:** Na tela &#039;Qual valor quer transferir?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway[[conta simples ou conta completa]]:transferencia:valor-transferencia",
        	eventAction: "preencheu:campo",
        	eventLabel: "valor"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Qual valor quer transferir?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway[[conta simples ou conta completa]]:transferencia:valor-transferencia",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela de data de transferência &#039;E quando?&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/transferencia/data-transferencia/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela de data de transferência &#039;E quando?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:data-transferencia",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique em um dos links.
- **Onde:** Na tela de data de transferência &#039;E quando?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:data-transferencia",
        	eventAction: "clique:link",
        	eventLabel: "agendar-outra-data"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** Na interação de selecionar ou desselecionar o checkbox
- **Onde:** Na tela de data de transferência &#039;E quando?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:data-transferencia",
        	eventAction: "interecao:checkbox",
        	eventLabel: "[[acao]]:repetir-essa-transferencia"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[acao]] | &#039;selecionar&#039;, &#039;desselecionar&#039; e etc | Deve retornar a ação do usuário. |

<br />

- **Quando:** Na escolha da opção de &quot;Por quanto tempo?&quot;
- **Onde:** Na tela de data de transferência &#039;E quando?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:data-transferencia",
        	eventAction: "clique:opcao:por-quanto-tempo",
        	eventLabel: "[[opcao-selecionada]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[opcao-selecionada]] | &#039;4-meses&#039; e etc | Deve retornar o período selecionado. |

<br />

- **Quando:** Após o preenchimento do campo &#039;Data de transferência&#039;.
- **Onde:** Na tela de data de transferência &#039;E quando?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:data-transferencia",
        	eventAction: "preencheu:campo",
        	eventLabel: "data-de-transferencia"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** Na abertura de algum modal.
- **Onde:** Na tela de data de transferência &#039;E quando?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:data-transferencia",
        	eventAction: "abriu:modal",
        	eventLabel: "[[nome-modal]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-modal]] | &#039;horario-para-efetivacao-da-transferencia&#039;, &#039;este-valor-nao-esta-disponivel-na-sua-conta-hoje&#039; etc. | Deve retornar o nome do modal aberto. |

<br />

- **Quando:** No clique do botão &#039;sim&#039;.
- **Onde:** No modal &#039;Este valor não está disponível na sua conta hoje&#039; da tela &#039;E quando?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:data-transferencia",
        	eventAction: "clique:botao",
        	eventLabel: "sim"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do link &#039;não, ir para o início&#039;.
- **Onde:** No modal &#039;Este valor não está disponível na sua conta hoje&#039; da tela &#039;E quando?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:data-transferencia",
        	eventAction: "clique:link",
        	eventLabel: "ir-para-o-inicio"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />


- **Onde:**  Visualização da tela &#039;Confirmação&#039;.

```javascript
    Analytics.logScreenview("/[[conta simples ou conta completa]]/transferencia/confirmacao/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** Após o preenchimento do campo &#039;Identificação do meu extrato&#039;.
- **Onde:** Na tela &#039;Confirmação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:confirmacao",
        	eventAction: "preencheu:campo",
        	eventLabel: "identificacao-do-meu-extrato"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Confirmação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:confirmacao",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;confirmar&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Após seleção de uma opção de um dos selects.
- **Onde:** Na tela &#039;Confirmação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:confirmacao",
        	eventAction: "select:opcao",
        	eventLabel: "[[nome-select]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-select]] | &#039;tipo-de-conta&#039;, &#039;finalidade-da-transferencia&#039; etc. | Deve retornar o nome do select com o qual o usuário interagiu. |

<br />

- **Quando:** Na abertura de algum modal.
- **Onde:** Na tela &#039;Confirmação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:confirmacao",
        	eventAction: "abriu:modal",
        	eventLabel: "[[nome-modal]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-modal]] | &#039;este-valor-ultrapassa-o-limite-diario-de-transferencia&#039;, &#039;antes-de-continuar-com-sua-transferencia&#039; etc. | Deve retornar o nome do modal aberto. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Em algum dos modais da tela &#039;Confirmação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:confirmacao",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;sim&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique do link &#039;voltar para o início&#039;.
- **Onde:** Em algum dos modais da tela &#039;Confirmação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:confirmacao",
        	eventAction: "clique:link",
        	eventLabel: "voltar-para-o-inicio"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Onde:**Visualização  da tela &#039;Efetivação&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/transferencia/efetivacao/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões de fechar.
- **Onde:** Na tela &#039;Efetivação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:efetivacao",
        	eventAction: "clique:botao",
        	eventLabel: "fechar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do link &#039;ver comprovante&#039;.
- **Onde:** Na tela &#039;Efetivação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:efetivacao",
        	eventAction: "clique:link",
        	eventLabel: "ver-comprovante"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Onde:** Visualização da tela &#039;Comprovante de transferência&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/transferencia/comprovante/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Comprovante de transferência&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:transferencia:comprovante",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;compartilhar&#039;, &#039;fechar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:**  Visualização da tela &#039;Para quem quer transferir?&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/transferencia/para-quem-transferir/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões ou links
- **Onde:** Na tela &#039;Para quem quer transferir?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:para-quem-transferir",
        	eventAction: "clique:[[botao-ou-link]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[botao-ou-link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o elemento clicado. |
| [[nome-item]] | &#039;voltar&#039;, &#039;editar-contato&#039;, &#039;transferir-novo-contato&#039;, &#039;buscar-contato&#039; etc. | Deve retornar o nome do item clicado. |

<br />

- **Quando:** No clique para &quot;favoritar&quot;, &quot;editar&quot; e &quot;excluir&quot; um contato.
- **Onde:** Na tela &#039;Para quem quer transferir?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:para-quem-transferir",
        	eventAction: "clique:opcao",
        	eventLabel: "[[nome-opcao]]:contato-[[posicao-contato]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-opcao]] | &#039;favoritar&#039;, &#039;excluir&#039;, &#039;editar&#039; e etc. | Deve retornar a opção clicada. |
| [[posicao-contato] | &#039;1&#039;, &#039;2&#039; e etc | Deve retornar a posição que o contato aparece. |

<br />

- **Onde:** Visualização do modal de &#039;transferência&#039;

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/transferencia/para-quem-transferir/modal-transferencia/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** No modal de &#039;transferência&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:para-quem-transferir",
        	eventAction: "clique:opcao:modal-transferencia",
        	eventLabel: "[[nome-opcao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-opcao]] | &#039;fechar&#039;, &#039;conta-banco-midway&#039;, &#039;conta-nu-pagamentos&#039;, &#039;editar-contato&#039; e etc. | Deve retornar a opção clicada. |

<br />

- **Onde:** Visualização do modal de &#039;Este valor ultrapassa seu limite diário de transferência&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/transferencia/para-quem-transferir/modal-transferencia/ultrapassa-valor-diario/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** No modal de &#039;Este valor ultrapassa seu limite diário de transferência&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:para-quem-transferir",
        	eventAction: "clique:opcao:modal-ultrapassa-valor-diario",
        	eventLabel: "[[nome-opcao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-opcao]] | &#039;alterar-limite-diario&#039;, &#039;entendi&#039; e etc. | Deve retornar a opção clicada. |

<br />

- **Onde:** Visualização do modal de &#039;Ajuste de limite de transferência&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/transferencia/para-quem-transferir/modal-transferencia/ultrapassa-valor-diario/ajuste-limite-transferencia/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** No modal de &#039;Ajuste de limite de transferência&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:para-quem-transferir",
        	eventAction: "clique:opcao:modal-ajuste-limite-transferencia",
        	eventLabel: "[[nome-opcao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-opcao]] | &#039;voltar&#039;, &#039;confirmar&#039;, &#039;ajuste-limite-65.000,00&#039; e etc. | Deve retornar a opção clicada. |

<br />

- **Onde:**  Visualização da tela de &#039;Limite diário alterado&#039;.


```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/transferencia/para-quem-transferir/modal-transferencia/ultrapassa-valor-diario/ajuste-limite-transferencia/limite-diario-alterado/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** No modal de &#039;Limite diário alterado&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:limite-diario-alterado",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;fechar&#039; e etc. | Deve retornar o botão clicado. |

<br />

### Perfil / Canais Digitais 2.2.5

### Perfil / Canais Digitais 2.2.5

- **Onde:** Visualização da tela &quot;Perfil&quot;.

```javascript
    Analytics.logScreenView("/perfil/")
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &quot;Perfil&quot;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;sair&#039;, &#039;meus-dados&#039;, &#039;configurar-conta&#039;, &#039;configurar-cartao&#039;, &#039;traga-seu-salario-para-midway&#039; e etc | deve retornar o nome do botão clicado. |

<br />



- **Onde:** Visualização da tela &#039;Alterar Dados&#039;, na funcionalidade de Canais Digitais.

```javascript
    Analytics.logScreenView("/perfil/alterar-dados/");
```


<br />

- **Quando:** No clique do botão de &quot;Confirmar Alteração&quot; na funcionalidade de Canais Digitais.
- **Onde:** Na tela &#039;Alterar Dados&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:alterar-dados",
        	eventAction: "clique:botao",
        	eventLabel: "confirmar-alteracao"
        });
```


<br />

- **Quando:** No callback de sucesso ou erro na após clicar no botão de &quot;Confirmar Alteração&quot;, na funcionalidade de Canais Digitais.
- **Onde:** Na tela &#039;Alterar Dados&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:alterar-dados",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso:dados-alterados&#039; ou &#039;erro:nao-foi-possivel-alterar-dados&#039; e  etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Onde:** Visualização da tela &#039;Dados alterados com sucesso&#039;, na funcionalidade de Canais Digitais.

```javascript
    Analytics.logScreenView("/perfil/alterar-dados/dados-alterados-com-sucesso/");
```


<br />

- **Onde:** Visualização da tela &#039;Gerenciar conta corrente&#039;, na funcionalidade de Canais Digitais.

```javascript
    Analytics.logScreenView("/perfil/gerenciar-conta-corrente/");
```


<br />

- **Quando:** No clique das opções de &quot;Cancelar conta corrente&quot;,  &quot;Cancelar limite emergencial&quot; ou &quot;Ativar limite emergencial&quot; na funcionalidade de Canais Digitais.
- **Onde:** Na tela &#039;Gerenciar conta corrente&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:gerenciar-conta-corrente",
        	eventAction: "clique:lista",
        	eventLabel: "[[nome-opcao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-opcao]] | &#039;cancelar-conta-corrente&#039;, &#039;cancelar-limite-emergencial&#039;, &#039;ativar-limite-emergencial&#039; e etc | Deve retornar o nome da opção selecionada pelo usuário. |

<br />

- **Quando:** Na abertura do modal &#039;Quer mesmo cancelar o seu limite emergencial?&#039;.
- **Onde:** Na tela &#039;Gerenciar conta corrente&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:gerenciar-conta-corrente:limite-emergencial:cancelamento",
        	eventAction: "modal:cancelar-limite-emergencial:clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;nao&#039; ou &#039;sim&#039; e etc | Deve retornar o nome do botão clicado pelo usuário. |

<br />

- **Quando:** No callback de sucesso ou erro na após clicar no botão de &quot;Sim&quot;, na funcionalidade de Canais Digitais.
- **Onde:** No modal &#039;Quer mesmo cancelar o seu limite emergencial&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:gerenciar-conta-corrente:limite-emergencial:cancelamento",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso:emergencial-cancelado&#039; ou &#039;erro:no-momento-nao-e-possivel-cancelar&#039; e  etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Onde:** Visualização da tela &#039;Limite emergencial cancelado com sucesso&#039;, na funcionalidade de Canais Digitais.

```javascript
    Analytics.logScreenView("/perfil/gerenciar-conta-corrente/limite-emergencial-cancelado-com-sucesso/");
```


<br />

- **Onde:** Visualização da tela &#039;No momento não é possível cancelar o seu limite emergencial&#039;, na funcionalidade de Canais Digitais.

```javascript
    Analytics.logScreenView("/perfil/gerenciar-conta-corrente/nao-foi-possivel-cancelar-limite-emergencial/");
```


<br />

- **Quando:** No clique do botão de &quot;Voltar&quot; ou &quot;Acesse o chat&quot; na funcionalidade de Canais Digitais.
- **Onde:** No modal &#039;No momento, não é possível cancelar o seu limite emergencial&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:gerenciar-conta-corrente:limite-emergencial:cancelamento",
        	eventAction: "modal:nao-foi-possivel-cancelar-limite-emergencial:clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039; ou &#039;acesse-o-chat&#039; e etc | Deve retornar o nome do botão clicado pelo usuário. |

<br />

### Perfil / Bloqueio Cartão

**Ao visualizar a tela de &quot;Configurar Cartão&quot;**
- **Link:** 5

```javascript
    Analytics.logScreenView("/perfil/configurar-cartao")
```


<br />

- **Quando:** Ao interagir com os botões.
- **Onde:** Na tela de &quot;Configurar Cartão&quot;
- **Link:** 5

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:configurar-cartao",
        	eventAction: "clique:botão",
        	eventLabel: "[[nome-botao]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;ajustar-limite&#039;,  &#039;bloqueio-definitivo&#039;, &#039;configuracao-de-senha&#039; e etc  | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Ao &quot;ativar&quot; e &quot;desativar&quot; o bloqueio temporario do cartão
- **Onde:** Na tela de &quot;Configurar Cartão&quot;
- **Link:** 5

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:configurar-cartao" ,
        	eventAction: "bloqueio-temporario" ,
        	eventLabel: "[[opcao]]:configuracao" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao]] | &#039;ativou&#039; ou &#039;desativou&#039; | Deve retornar se o usuario ativou e desativou o bloqueio temporario do cartão. |

<br />

**Ao visualizar o modal de Bloqueio temporario, após clicar em &#039;ativar / desativar bloqueio temporario na tela  &quot;Configurar Cartão&quot;**


```javascript
    Analytics.logScreenView("/perfil/configurar-cartao/[[opcao]]-bloqueio-temporario")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao]] | &#039;ativou&#039; ou &#039;desativou&#039; | Deve retornar se o usuario ativou e desativou o bloqueio temporario do cartão. |

<br />

- **Quando:** No clique dos botões
- **Onde:** N o modal de Bloqueio temporario, após clicar em &#039;ativar/desativar bloqueio temporario&#039; na tela  &quot;Configurar Cartão&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:modal-bloqueio-temporario" ,
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;bloquear&#039;, &#039;desbloquear&#039;, &#039;cancelar&#039;, &#039;fechar&#039; | deve retornar o nome do botão clicado. |

<br />

**Ao visualizar a tela de sucesso, após ativar ou desativar  bloqueio temporario**

```javascript
    Analytics.logScreenView("/perfil/configurar-cartao/[[opcao]]-bloqueio-temporario/sucesso")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao]] | &#039;ativou&#039; ou &#039;desativou&#039; | Deve retornar se o usuario ativou e desativou o bloqueio temporario do cartão. |

<br />

- **Quando:** No clique dos botões ( a variavel [[motivo]] deve retornar preenchida apenas caso o usuario clique em Bloquear)
- **Onde:** N o modal de Bloqueio Definitivo

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:modal-bloqueio-definitivo",
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]:[[motivo]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;bloquear&#039;, &#039;fechar&#039;,  | Deve retornar o nome do botão clicado. |
| [[motivo]] | perda&#039;, &#039;roubo&#039;, &#039;furto&#039;, &#039;cartao-danificado&#039; e etc | deve retornar preenchida com o motivo que o usuario selecionou para o bloqueio, apenas caso  o usuario clique em &quot;Bloquear&quot;, caso contrario deve retornar vazio. |

<br />

**Ao visualizar a tela de &quot;Validação&quot;, após clicar em &quot;Bloquear&quot;, para bloquear o cartão definitivamente**

```javascript
    Analytics.logScreenView("/perfil/configurar-cartao/bloqueio-definitivo/validacao")
```


<br />

**Ao visualizar a tela de &quot;Sucesso &quot; ao bloquear o cartão definitivamente**

```javascript
    Analytics.logScreenView("/perfil/configurar-cartao/bloqueio-definitivo/sucesso")
```


<br />



- **Quando:** No clique dos botões na tela de cartao bloqueado definitivamente
- **Onde:** No modal de Bloqueio Definitivo

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:modal-bloqueio-definitivo",
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;solicitar-agora&#039;, &#039;solicitar-depois&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique dos botões &#039;solicitar agora&#039; na tela de cartao bloqueado 
- **Onde:**  Na tela de &#039;Configurar cartão&#039; 

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:modal-bloqueio-definitivo",
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;solicitar-agora&#039;, &#039;solicitar-depois&#039; e etc | Deve retornar o nome do botão clicado. |

<br />


**Ao visualizar a tela de confirmaçao do endereço após o clique de  solicitaçao de um novo cartao**

```javascript
    Analytics.logScreenView("/perfil/configurar-cartao/2-via-cartao/confirmar-endereco")
```


<br />


- **Quando:** No clique dos botões na tela de Confirmaçao de endereço
- **Onde:**  Na tela de confirmaçao de endereço

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:2-via-cartao:confirmar-endereco",
        	eventAction: "clique:elementos" ,
        	eventLabel: "[[nome-item]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;voltar&#039;, &#039;editar endereço&#039;, &#039;concluir&#039; e etc | Deve retornar o nome do botão clicado. |

<br />


**Ao visualizar a tela de ediçao de endereço após a clique no botão edição de endereço**

```javascript
    Analytics.logScreenView("/perfil/configurar-cartao/2-via-cartao/confirmar-endereco/editar")
```


<br />

- **Quando:** No preenchimento dos campos na tela de ediçao de endereço
- **Onde:**  Na tela de confirmaçao de endereço

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:2-via-cartao:confirmar-endereco",
        	eventAction: "preencheu:campo" ,
        	eventLabel: "[[nome-campo]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;CEP&#039;, &#039;endereco&#039; e etc | Deve retornar o nome do campo preenchido. |

<br />

**Ao visualizar a tela de sucesso após a conclusão da solicitação de 2ª via do cartão**

```javascript
    Analytics.logScreenView("/perfil/configurar-cartao/2-via-cartao/confirmar-endereco/sucesso")
```


<br />



- **Quando:** No clique dos bot&#245;es na tela de Sucesso de solicita&#231;&#227;o da  2&#170; via do cart&#227;o
- **Onde:**  Na tela de confirmaçao de endereço

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:perfil:2-via-cartao:confirmar-endereco",
        	eventAction: "clique:elementos" ,
        	eventLabel: "[[nome-item]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;voltar&#039;, &#039;fechar&#039; e etc | Deve retornar o nome do campo preenchido. |

<br />


### Conta Remunerada

- **Onde:** Visualização da tela &#039;Remuneração de Conta&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/remuneracao-conta/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do botão &#039;voltar&#039;.
- **Onde:** Na tela &#039;Remuneração de Conta&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:remuneracao-conta",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do link &#039;como funciona&#039;.
- **Onde:** Na tela &#039;Remuneração de Conta&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:remuneracao-conta",
        	eventAction: "clique:link",
        	eventLabel: "como-funciona"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do botão &#039;fechar&#039;.
- **Onde:** No modal &#039;Como funciona a Remuneração de conta?&#039; após clique no link &#039;como funciona&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:remuneracao-conta",
        	eventAction: "clique:botao",
        	eventLabel: "fechar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique dos botões
- **Onde:** Na tela &#039;Conta Remunerada&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:remuneracao-conta",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;voltar&#039;, &#039;desativar-conta-remunerada&#039;, &#039;ativar-conta-remunerada&#039; e etc | Deve retornar o botão clicado. |

<br />

- **Onde:** Visualização dos modais  &#039;Deseja cancelar conta remunerada? &#039;,  &#039;Deseja reativar sua conta remunerada? &#039;

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/remuneracao-conta/[[cancelar-ou-reativar]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[cancelar-ou-reativar]] | &#039;cancelar&#039;, &#039;reativar&#039; | Deve retornar o modal apresentado. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Nos modais &#039;Deseja cancelar conta remunerada?&#039;, &#039;Deseja reativar sua conta remunerada?&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:remuneracao-conta:[[cancelar-ou-reativar]]",
        	eventAction: "clique:botao:modal",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[cancelar-ou-reativar]] | &#039;cancelar&#039;, &#039;reativar&#039; | Deve retornar o modal apresentado. |
| [[nome-botao]] | &#039;voltar&#039;, &#039;cancelar&#039;, &#039;cancelar-conta-remunerada&#039;, &#039;reaitvar-conta-remunerada&#039; e etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** No modal &#039;Quer mesmo desativar a sua Remuneração da Conta?&#039; na tela &#039;Perfil&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:remuneracao-conta:desativacao",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;voltar&#039;, &#039;sim&#039;, &#039;nao&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:**  Visualização da tela &#039;Autenticação&#039;, aberta após confirmação da desativação da conta remunerada.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/remuneracao-conta/desativacao/autenticacao/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do botão &#039;voltar&#039;.
- **Onde:** Na tela &#039;Autenticação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:remuneracao-conta:desativacao",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No callback de sucesso/erro na finalização da operação.
- **Onde:** No fluxo da operação de desativação da conta remunerada.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:remuneracao-conta:desativacao",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[corrente ou pagamento]] | &#039;corrente&#039; ou &#039;pagamento&#039;. | Deve retornar o tipo da conta. |
| [[status]] | &#039;sucesso:remuneracao-de-conta-desativada&#039; ou &#039;erro:nao-foi-possivel-desativar-remuneracao-de-conta&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

### Encerramento de conta

- **Onde:** Visualização da tela &#039;Cancelamento de Conta Completa&#039;

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/cancelamento/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** Na seleção de uma das opções do select &#039;motivo do cancelamento&#039;.
- **Onde:** Na tela &#039;Cancelamento de Conta Completa&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:cancelamento",
        	eventAction: "select:opcao",
        	eventLabel: "[[nome-opcao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-opcao]] | &#039;tarifas-altas&#039;, &#039;trabalho-com-outros-bancos&#039;, &#039;aplicativo-nao-tem-uma-navegacao-intuitiva&#039; etc. | Nome da opção de motivo de cancelamento escolhida. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Cancelamento de Conta Completa&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:cancelamento",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;continuar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Na abertura de um modal.
- **Onde:** Na tela &#039;Cancelamento de Conta Completa&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:cancelamento",
        	eventAction: "abriu:modal",
        	eventLabel: "[[nome-modal]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-modal]] | &#039;confirmacao-cancelamento-conta-corrente&#039;, &#039;solicitacao-enviada&#039; etc. | Deve retornar o nome do modal aberto. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Em um dos modais da tela &#039;Cancelamento de Conta Completa&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:cancelamento:[[nome-modal]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;agora-nao&#039;, &#039;voltar-para-home&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;Condições do Cancelamento&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/cancelamento/condicoes-do-cancelamento/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Condições do Cancelamento&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:cancelamento:condicoes-cancelamento",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;voltar&#039;, &#039;continuar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;Autenticação&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/cancelamento/autenticacao/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do botão &#039;voltar&#039;.
- **Onde:** Na tela &#039;Autenticação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:cancelamento:autenticacao",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Onde:** Visualização da tela &#039;Sua solicitação já foi enviada&#039;.

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/cancelamento/solicitacao-ja-enviada/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique do botão &#039;voltar&#039;.
- **Onde:** Na tela &#039;Sua solicitação já foi enviada&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:cancelamento:solicitacao-ja-enviada",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

### Cheque Especial
- **Quando:** Na abertura do modal &#039;Alterar data de cobrança mensal do limite emergencial&#039;.
- **Onde:** Na tela &#039;Perfil&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:limite-emergencial:alteracao-data-cobranca",
        	eventAction: "abriu:modal",
        	eventLabel: "alterar-data-cobranca-limite-emergencial"
        });
```


<br />

- **Quando:** Na interação com o slider de range de dia.
- **Onde:** No modal &#039;Alterar data de cobrança mensal do limite emergencial&#039; na tela &#039;Perfil&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:limite-emergencial:alteracao-data-cobranca",
        	eventAction: "interacao:slider",
        	eventLabel: "[[dia-cobranca]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[dia-cobranca]] | &#039;14&#039;, &#039;25&#039; etc. | Deve retornar o dia escolhido pelo cliente. |

<br />

- **Quando:** Após preenchimento do campo &#039;dia&#039;.
- **Onde:** No modal &#039;Alterar data de cobrança mensal do limite emergencial&#039; na tela &#039;Perfil&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:limite-emergencial:alteracao-data-cobranca",
        	eventAction: "preencheu:campo",
        	eventLabel: "[[dia-escolhido]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[dia-escolhido]] | &#039;1&#039;, &#039;20&#039;, &#039;15&#039; etc. | Deve retornar o novo dia escolhido para cobrança mensal do limite emergencial. |

<br />

- **Quando:** No clique do botão &#039;alterar&#039;.
- **Onde:** No modal &#039;Alterar data de cobrança mensal do limite emergencial&#039; na tela &#039;Perfil&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:limite-emergencial:alteracao-data-cobranca",
        	eventAction: "clique:botao",
        	eventLabel: "alterar"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Autenticação&#039;, aberta após confirmação da alteração de data de cobrança.

```javascript
    Analytics.logScreenView("/conta-corrente/limite-emergencial/alteracao-data-cobranca/autenticacao/");
```


<br />

- **Quando:** No clique do botão &#039;voltar&#039;.
- **Onde:** Na tela &#039;Autenticação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:limite-emergencial:alteracao-data-cobranca",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```


<br />

- **Quando:** No callback de sucesso/erro na finalização da operação.
- **Onde:** No fluxo da operação de alteração de data de cobrança do limite emergencial.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:limite-emergencial:alteracao-data-cobranca",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso:data-alterada-com-sucesso&#039; ou &#039;erro:algo-deu-errado&#039;, &#039;erro:nao-foi-possivel-alterar-a-data&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** Na abertura do modal &#039;Quer mesmo cancelar o seu limite emergencial?&#039;.
- **Onde:** Na tela &#039;Perfil&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:limite-emergencial:cancelamento",
        	eventAction: "abriu:modal",
        	eventLabel: "cancelar-limite-emergencial"
        });
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** No modal &#039;Quer mesmo cancelar seu limite emergencial?&#039; na tela &#039;Perfil&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:limite-emergencial:cancelamento",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;sim&#039;, &#039;nao&#039; etc. | Deve retornar o nome do botão clicado. |

<br />


- **Onde:** Visualização da tela &#039;Autenticação&#039;, aberta após confirmação da alteração de data de cobrança.

```javascript
    Analytics.logScreenView("/conta-corrente/limite-emergencial/cancelamento/autenticacao/");
```


<br />

- **Quando:** No clique do botão &#039;voltar&#039;.
- **Onde:** Na tela &#039;Autenticação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:limite-emergencial:cancelamento",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```


<br />

- **Quando:** No callback de sucesso/erro na finalização da operação.
- **Onde:** No fluxo da operação de cancelamento do limite emergencial.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:limite-emergencial:cancelamento",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso:limite-emergencial-cancelado&#039; ou &#039;erro:algo-deu-errado&#039;, &#039;erro:nao-foi-possivel-cancelar-limite-emergencial&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** Na abertura do modal &#039;No momento não é possível cancelar o seu limite emergencial&#039;.
- **Onde:** Na tela &#039;Perfil&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:limite-emergencial:cancelamento",
        	eventAction: "abriu:modal",
        	eventLabel: "nao-e-possivel-cancelar-limite-especial"
        });
```


<br />

### Desbloqueio de Cartão
 - **Onde:** Visualização da tela &#039;Confirme o CVV do seu novo cartão&#039;.

```javascript
    Analytics.logScreenView("/desbloqueio-cartao/confirmacao-cvv/");
```


<br />

- **Quando:** No clique do link &#039;O número do cartão não é esse&#039;.
- **Onde:** Na tela &#039;Confirme o CVV do seu novo cartão&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:confirmacao-cvv",
        	eventAction: "clique:link",
        	eventLabel: "o-numero-do-cartao-nao-e-esse"
        });
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Confirme o CVV do seu novo cartão&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:confirmacao-cvv",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Após o preenchimento do campo &#039;CVV&#039;.
- **Onde:** Na tela &#039;Confirme o CVV do seu novo cartão&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:confirmacao-cvv",
        	eventAction: "preencheu:campo",
        	eventLabel: "cvv"
        });
```


<br />

- **Quando:** No callback do preenchimento do campo de &#039;CVV&#039;.
- **Onde:** Na tela &#039;Confirme o CVV do seu novo cartão&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:confirmacao-cvv",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:erro-de-preenchimento&#039;, &#039;erro:numero-invalido&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** Na abertura do modal &#039;Número do cartão diferente&#039;.
- **Onde:** Na tela &#039;Confirme o CVV do seu novo cartão&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:confirmacao-cvv",
        	eventAction: "abriu:modal",
        	eventLabel: "numero-do-cartao-diferente"
        });
```


<br />

- **Quando:** No clique do botão &#039;fechar&#039;.
- **Onde:** No modal &#039;Número do cartão diferente&#039; na tela &#039;Confirme o CVV do seu novo cartão&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:confirmacao-cvv",
        	eventAction: "clique:botao",
        	eventLabel: "fechar"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Digite uma senha para seu cartão&#039;.

```javascript
    Analytics.logScreenView("/desbloqueio-cartao/criacao-senha/");
```

<br />

- **Quando:** Após o preenchimento do campo de &#039;senha&#039;.
- **Onde:** Na tela &#039;Digite uma senha para seu cartão&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:criacao-senha",
        	eventAction: "preencheu:campo",
        	eventLabel: "senha"
        });
```


<br />

- **Quando:** No callback do preenchimento do campo de senha.
- **Onde:** Na tela &#039;Digite uma senha para seu cartão&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:criacao-senha",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:repeticao-de-numeros-seguidos&#039;, &#039;erro:data-de-nascimento&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Digite uma senha para seu cartão&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:criacao-senha",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;ver-senha&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Na tela &#039;Confirme sua senha&#039;.



```javascript
    Analytics.logScreenView("/desbloqueio-cartao/confirmacao-senha/");
```


<br />

- **Quando:** Após o preenchimento do campo de &#039;confirmar senha&#039;.
- **Onde:** Na tela &#039;Confirme sua senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:confirmacao-senha",
        	eventAction: "preencheu:campo",
        	eventLabel: "confirmar-senha"
        });
```


<br />

- **Quando:** No callback do preenchimento do campo de confirmação de senha.
- **Onde:** Na tela &#039;Confirme sua senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:confirmacao-senha",
        	eventAction: "callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:senha-diferente-da-anterior&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** No clique de um dos botões.
- **Onde:** Na tela &#039;Confirme sua senha&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:confirmacao-senha",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;ver-senha&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;Confirme sua senha&#039;.

```javascript
    Analytics.logScreenView("/desbloqueio-cartao/autenticacao/");
```


<br />

- **Quando:** No clique do botão &#039;confirmar&#039;.
- **Onde:** Na tela &#039;Autenticação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:autenticacao",
        	eventAction: "clique:botao",
        	eventLabel: "confirmar"
        });
```


<br />

- **Onde:**  Visualização da tela 'Você já pode começar a usar seu cartão de débito Midway'

```javascript
    Analytics.logScreenView("/desbloqueio-cartao/cartao-desbloqueado/");
```


<br />

- **Quando:** No clique em um dos botões de &#039;fechar&#039;.
- **Onde:** Na tela &#039;Você já pode começar a usar seu cartão de débito Midway&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:cartao-desbloqueado",
        	eventAction: "clique:botao",
        	eventLabel: "fechar"
        });
```


<br />

- **Quando:** No callback da operação de desbloqueio do cartão.
- **Onde:** No fluxo de desbloqueio de cartão.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:desbloqueio-cartao:cartao-desbloqueado",
        	eventAction: "callback",
        	eventLabel: "[[status]]",
        	dimension2: "[[dimension2]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:nao-foi-possivel-realizar-o-desbloqueio&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |
| [[dimension2]] | &quot;conta-corrente&quot;, &quot;cartao-de-credito&quot; ou &quot;conta-corrente-e-cartao-de-credito&quot; | Deve retornar se o usuário possui uma conta corrente, cartão de crédito ou as duas opções |

<br />

### Home 

- **Onde:** VIsualização da tela &#039;Home&#039;.

```javascript
    Analytics.logScreenView("/home/");
```


<br />

- **Quando:** Na interação com o carrossel de cards de &#039;saldo&#039; e &#039;conta completa&#039;
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home",
        	eventAction: "interagiu:card",
        	eventLabel: "[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;saldo-positivo-limite-emergencial&#039;, &#039;uso-limite-emergencial&#039;, &#039;limite-emergencial-nao-contratado&#039;, &#039;limite-emergencial-cancelado&#039; etc. | Deve retornar o nome do card do carrossel exibido. |

<br />

- **Quando:** No clique em um dos cards de atalho.
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home",
        	eventAction: "clique:card",
        	eventLabel: "[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;extrato&#039;, &#039;pagamento&#039;, &#039;transferencia&#039;, &#039;desbloqueio-de-cartao&#039; etc. | Deve retornar o nome do card de atalho clicado. |

<br />

- **Quando:** No clique do link &#039;ver extrato&#039;.
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home",
        	eventAction: "clique:link",
        	eventLabel: "ver-extrato"
        });
```


<br />

- **Quando:** No clique dos botões de &#039;expandir&#039; e &#039;reduzir&#039; de acesso ao &#039;menu perfil&#039;.
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;expandir&#039; ou &#039;reduzir&#039;. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique em uma das opções do menu inferior
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home",
        	eventAction: "clique:menu-inferior",
        	eventLabel: "[[nome-menu]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-menu]] | &#039;home&#039;, &#039;cartoes&#039;, &#039;outros&#039;, &#039;ajuda&#039; e etc | Deve retornar o nome do menu clicado. |

<br />

- **Quando:** No clique do botão de informação de limite emergencial.
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home",
        	eventAction: "clique:botao",
        	eventLabel: "informacao-limite-emergencial"
        });
```


<br />

- **Quando:** Na abertura de um modal de &#039;Como funciona o limite emergencial?&#039;.
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home",
        	eventAction: "abriu:modal",
        	eventLabel: "[[tipo-limite-emergencial]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-limite-emergencial]] | &#039;sem-cheque-especial-contratado&#039;, &#039;com-cheque-especial-contratado&#039; etc. | Deve retornar se o cliente tem cheque especial contratado ou não. |

<br />

- **Quando:** No clique dos contatos para transferência rápida, na seção de &quot;Transferir para&quot;
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home",
        	eventAction: "clique:opcao:transferir-para",
        	eventLabel: "[[tipo-contato]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-contato]] | &#039;contato-favorito&#039;, &#039;outros&#039; e etc | Deve retornar o contato selecionado. |

<br />

- **Quando:** No clique dos banners de informações
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home",
        	eventAction: "clique:banner-informativo",
        	eventLabel: "[[banner-clicado]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[banner-clicado]] | &#039;habilite-token-no-app&#039;, &#039;assistencia-automovel-midway&#039; e etc | Deve retornar o nome do banner clicado. |

<br />

- **Onde:** VIsualização do modal de &quot;Notificação de bloqueio&quot;.

```javascript
    Analytics.logScreenView("/home/notificacao-bloqueio/");
```


<br />




- **Quando:** Na interação com os banners de &quot;Limite pre aprovado&quot; que aparece abaixo ou acima do menu de opções ou ao lado da informação do saldo
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home" ,
        	eventAction: "clique:[[local]]" ,
        	eventLabel: "credito-pre-aprovado" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[local]] | &#039;banner-superior&#039;, &#039;banner-inferior&#039;, &#039;banner-lateral&#039; | Deve retornar se o banner esta localizado na parte superior, inferior, ou lateral da tela. |

<br />

- **Visualização do modal de &#039;Notificação de bloqueio&#039;**

```javascript
    Analytics.logScreenView("/home/notificacao-bloqueio/")
```


<br />

- **Quando:** No clique dos botões
- **Onde:** No modal de &quot;Notificação de bloqueio&quot;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home",
        	eventAction: "clique:botao:modal-notificacao-bloqueio" ,
        	eventLabel: "[[nome-botao]]"
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;atendimento-via-chat&#039;, &#039;atendimento-telefonico&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Ao clicar no card de &quot;Cotação diaria&quot;
- **Onde:** Na home

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home",
        	eventAction: "clique:card" ,
        	eventLabel: "[[nome-card]]" 
        ])
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;cotacao-diaria-euro&#039;, &#039;cotacao-diaria-dolar&#039; e etc | Deve retornar o nome do card clicado. |

<br />

### Home - Migração de Conta


- **Visualização das telas de onboarding em &#039;Migração de conta&#039;** 

```javascript
    Analytics.logScreenView("/home/onboarding-migracao-conta-[[numero-tela]]/")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[numero-tela]] | &#039;1&#039;, &#039;2&#039; e etc | deve retornar o numero da tela. |


<br />


- **Quando:** No clique em todos os botões
- **Onde:**  Nas telas de &quot;Migração de conta&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:onboarding-migracao-conta-[[numero-tela]]",
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]"
        ])
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[numero-tela]] | &#039;1&#039;, &#039;2&#039; e etc | deve retornar o numero da tela. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;fechar&#039; e etc | Deve retornar o  nome do botão clicado. |

<br />

- **Visualização das telas de envio do cartão em &#039;Migração de conta&#039;** 

```javascript
    Analytics.logScreenView("/home/migracao-conta/envio-do-cartao/")
```
<br />


- **Quando:** No clique em todos os botões
- **Onde:**  Na tela de envio do cartão em &quot;Migração de conta&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:migracao-conta",
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]"
        ])
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;editar-endereco&#039; e etc | Deve retornar o  nome do botão clicado. |

<br />

- **Visualização da tela de sucesso em &#039;Migração de conta&#039;** 

```javascript
    Analytics.logScreenView("/home/migracao-conta/sucesso/")
```
<br />

- **Quando:** No clique em todos os botões
- **Onde:** Na tela de sucesso em &quot;Migração de conta&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:migracao-conta:sucesso",
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]"
        ])
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;acessar-conta&#039; e etc | Deve retornar o  nome do botão clicado. |

<br />



### Cotação de câmbio


-**VIsualização da tela de Cotações**

```javascript
    Analytics.logScreenView("/cotacao/cambio/[[moeda]]")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[moeda]] | &#039;dolar&#039;, &#039;euro&#039; e etc | deve retornar o nome da moeda. |

<br />

- **Quando:** No clique em todos os botões da tela de &#039;Cotação&#039;
- **Onde:** Na tela de &quot;Cotações&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:cotacao-cambio",
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]"
        ])
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;filtrar&#039; e etc | Deve retonrar o  nome do botão clicado. |

<br />

-**VIsualização do modal de &#039;Filtros &#039; em &#039;Cotações&#039;**

```javascript
    Analytics.logScreenView("/cotacao/cambio/[[moeda]]/filtrar")
```


<br />

- **Quando:** No clique no botão &quot;Filtrar&quot;  no modal de &quot;Filtrar Cotações&quot;
- **Onde:** No modal de &quot;Filtrar&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cotacao-cambio" ,
        	eventAction: "filtrar:cotacoes" ,
        	eventLabel: "[[filtros-selecionado]][[opcao-selecionada]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[filtros-selecionado]] | &#039;filtrar-data&#039;, &#039;filtrar-dias&#039; e etc | Deve retornar se o usuario selecionou filtrar por data ou por dias. |
| [[opcao-selecionada]] | &#039;30-dias&#039;. &#039;60-dias&#039;, &#039;12-04-2020-ate-12-04-2021&#039; e etc | Deve retornar a opcao selecionada. |

<br />

### Credito Pré Aprovado 

-**VIsualização da tela de Crédito Pré Aprovado**

```javascript
    Analytics.logScreenView("/credito-pre-aprovado")
```


<br />

- **Quando:** No clique em todos os botões
- **Onde:** Na tela de Crédito Pré Aprovado

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:credito-pre-aprovado",
        	eventAction: "clique:botao" ,
        	eventLabel: "[[nome-botao]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;ativar-funcao-credito&#039;, &#039;voltar&#039;, &#039;taxas-e-encargos&#039;, &#039;consulte-aqui-o-contrato-do-seu-cargo&#039; e etc | Deve retornar o nome do botão clicado. |

<br />


- **VIsualização da tela de &#039;Taxas e encargos&#039; e &#039;Contrato&#039;**

```javascript
    Analytics.logScreenView("/credito-pre-aprovado/[[tela]]")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tela]] |&#039;taxas-e-encargos&#039;, &#039;contrato&#039; | deve retornar o nome da tela em que o usuario esta. |

<br />


- **VIsualização das telas de  &#039;Biometria &#039;**

```javascript
    Analytics.logScreenView("/credito-pre-aprovado/validacao/[[etapa]]")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[etapa]] | &#039;orientacoes&#039;, &#039;foto&#039;, &#039;segunda-foto&#039; e etc | deve retornar a etapa em que o usuario esta. |

<br />

- **VIsualização das telas de &#039;Dia de vencimento da fatura&#039;**

```javascript
    Analytics.logScreenView("/credito-pre-aprovado/vencimento-da-fatura")
```


<br />

- **Quando:** No clique do botão de &quot;Continuar&quot;
- **Onde:** Na tela de &#039; Dia de vencimento da fatura&#039;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:credito-pre-aprovado",
        	eventAction: "clique:continuar",
        	eventLabel: "selecionou-dia-vencimento:[[dia-selecionado]]" 
        })
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[dia-selecionado]] | &#039;15&#039;, &#039;20&#039;, &#039;30&#039; e etc | Deve retornar o dia selecionado pelo usuario. |

<br />


- **VIsualização das telas de &#039;Avaliacao Emergencial de credito&#039;**

```javascript
    Analytics.logScreenView("/credito-pre-aprovado/avaliacao-emergencial-de-credito")
```


<br />

- **VIsualização das telas de 'Sucesso'**

```javascript
    Analytics.logScreenView("/credito-pre-aprovado/sucesso")
```


<br />




### Cartões - Fase 2.1 - Cartões - Home

- **Onde:** Visualização da tela &#039;Home&#039; para Cartões.

```javascript
    Analytics.logScreenView("/cartoes/home/");
```


<br />

- **Quando:** Na interação com os botões &quot;Ver lançamento&quot; e &quot;Ver fatura&quot;
- **Onde:** Na home de Cartões

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cartoes-home",
        	eventAction: "clique:botao",
        	eventLabel: "[[titulo-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[titulo-botao]] | &#039;ver-lancamento&#039; e &#039;ver-fatura&#039; | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Ao selecionar uma das opções de cartão em &quot;Últimas Movimentações&quot;
- **Onde:** Na home de Cartões

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cartoes-home",
        	eventAction: "selecionou:opcao",
        	eventLabel: "ultimas-movimentacoes:[[opcao-selecionada]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] | &#039;titular&#039; ou &#039;adicional&#039; | Deve retornar a opção selecionada pelo usuário. |

<br />

- **Quando:** No clique do botão para ver &quot;informações do Cambio&quot;, na tela de &quot;Comprovante de transferencia&quot; com conversão de moeda
- **Onde:** Na tela referente ao Detalhe da transferência para uma compra internacional

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cartoes-home:comprovante-da-transferencia:conversao-de-moeda",
        	eventAction: "clique:botao",
        	eventLabel: "informacoes-cambio"
        });
```



<br />

- **Quando:** No clique em &quot;Comprovante de transferencia&quot;
- **Onde:** Na tela referente ao Detalhe da transferência para uma compra internacional

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cartoes-home",
        	eventAction: "clique:botao",
        	eventLabel: "ultimas-movimentacoes:internacional"
        });
```


<br />

- **Onde:**  Na visualização do modal de "Cotação" que aparece após clicar em "informações do Cambio"..

```javascript
    Analytics.logScreenView("/cartoes/home/cotacao/cambio/");
```


<br />

- **Quando:** No clique em todos os botões do modal de "Cotação"
- **Onde:** No  modal de "Cotação" que aparece após clicar em  "informações do Cambio".

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cartoes-home:comprovante-da-transferencia:conversao-de-moeda",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;, &#039;ok&#039; e etc. | deve retornar o nome do botão clicado. |

<br />


### Cartões - Fase 2.1 - Cartões - Termo e adesão

- **Onde:** VIsualização da tela &#039;Termo e adesão&#039; para Cartões.

```javascript
    Analytics.logScreenView("/cartoes/termo-de-adesao/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[userID]] | &quot;01234&quot; | ID único de usuário definido após o cadastro (ID próprio da Midway). |

<br />

- **Onde:** VIsualização da tela de um termos acessados pelo usuário em &#039;Termo e adesão&#039;.

```javascript
    Analytics.logScreenView("/cartoes/termo-de-adesao/[[tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tela]] |  | Deve retornar o título da tela acessada pelo usuário. Ex.: &#039;termo-de-uso-da-conta-pagamento&#039;, &#039;termo-de-uso-da-conta-corrente&#039;, &#039;termo-de-uso-cartoes-rchlo&#039;, &#039;parcelamento-de-limite-emergencial&#039;, &#039;cancelamento-de-limite-emergencial&#039;, &#039;termo-de-privacidade-app-midway&#039;, etc |

<br />

- **Quando:** Na interação com um dos botões de &quot;Termo e adesão&quot; apresentados na página
- **Onde:** Na tela de Termo e adesão

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cartoes-termo-de-adesao",
        	eventAction: "clique:botao",
        	eventLabel: "[[titulo-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[titulo-botao]] |  | Deve retornar o nome do botão clicado. Ex.: &#039;termo-de-uso-da-conta-pagamento&#039;, &#039;termo-de-uso-da-conta-corrente&#039;, &#039;termo-de-uso-cartoes-rchlo&#039;, &#039;parcelamento-de-limite-emergencial&#039;, &#039;cancelamento-de-limite-emergencial&#039;, &#039;termo-de-privacidade-app-midway&#039;, etc |

<br />

- **Quando:** Na interação com o botão &quot;Assinar&quot;
- **Onde:** Após acessar um dos termos apresentados em &quot;Termo e adesão&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cartoes-termo-de-adesao",
        	eventAction: "clique:botao",
        	eventLabel: "assinar"
        });
```


<br />

- **Quando:** No callback de sucesso ou erro após o clique em assinar
- **Onde:** Na tela de sucesso ou erro para o termo assinado

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cartoes-termo-de-adesao",
        	eventAction: "callback",
        	eventLabel: "[[status]]",
        	dimension2: "[[dimension2]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:falha-ao-autenticar-a-assinatura&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |
| [[dimension2]] | &quot;conta-corrente&quot;, &quot;cartao-de-credito&quot; ou &quot;conta-corrente-e-cartao-de-credito&quot; | Deve retornar se o usuário possui uma conta corrente, cartão de crédito ou as duas opções |

<br />

- **Quando:** Na interação com botão &quot;Voltar para Termos e Adesão&#039;
- **Onde:** Na tela de sucesso para assinar um termo

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cartoes-termo-de-adesao",
        	eventAction: "clique:botao",
        	eventLabel: "voltar-para-termos-e-adesao"
        });
```


<br />

### Cartões - Fase 2.1 - Cartões - Notificações

- **Onde:** VIsualização da tela &#039;Notificações&#039;.

```javascript
    Analytics.logScreenView("/cartoes/notificacoes/");
```


<br />

- **Onde:** Visualização das telas internas de &#039;Notificações&#039;.

```javascript
    Analytics.logScreenView("/cartoes/notificacoes/[[tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tela]] |  | Deve retornar o o título da tela acessada pelo usuário. Ex.: &#039;configuracao&#039;, &#039;melhor-dia-de-compra&#039;, &#039;vencimento-de-fatura&#039;, &#039;voce-nao-vai-querer-perder&#039;, &#039;compra-aprovada&#039;, &#039;fatura-disponivel-para-pagamento&#039;, etc |

<br />

- **Quando:** Na interação com os cards ou o botão de engrenagem
- **Onde:** Na tela de &#039;Notificações&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cartoes-notificacoes",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;melhor-dia-de-compra&#039;, &#039;vencimento-de-fatura&#039;, &#039;voce-nao-vai-querer-perceber&#039;, &#039;compra-aprovada&#039;, etc | Deve retornar o título do botão clicado; |

<br />

- **Quando:** Na interação com um dos botões apresentados nas telas internas de Notificações
- **Onde:** Nas telas internas de &#039;Notificações&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cartoes-notificacoes",
        	eventAction: "clique:botao",
        	eventLabel: "[[tela]]:[[titulo-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tela]] |  | Deve retornar o o título da tela acessada pelo usuário. Ex.: &#039;configuracao&#039;, &#039;melhor-dia-de-compra&#039;, &#039;vencimento-de-fatura&#039;, &#039;voce-nao-vai-querer-perder&#039;, &#039;compra-aprovada&#039;, &#039;fatura-disponivel-para-pagamento&#039;, etc |
| [[titulo-botao]] |  | Deve retornar o título do botões clicado. Ex.: &#039;voltar&#039;, &#039;aviso-de-compras-ativado&#039;, &#039;aviso-de-compras-desativado&#039;, &#039;melhor-dia-de-compra-ativado&#039;, &#039;melhor-dia-de-compra-desativado&#039;, &#039;vencimento-de-fatura-ativado&#039;, &#039;vencimento-de-fatura-desativado&#039;, &#039;descontos-e-promocoes-ativado&#039;, &#039;descontos-e-promocoes-desativado&#039;, &#039;apagar-mensagem&#039;, &#039;ir-para-pagamento-de-fatura&#039;, &#039;ir-para-loja-online&#039;,  |

<br />

### Cartões - Fase 2.1 - Cartões - Consulta de Fatura

- **Onde:**  Visualização da tela de &#039;Consulta de Fatura&#039;

```javascript
    Analytics.logScreenView("/cartoes/consulta-de-fatura/");
```


<br />

- **Quando:** Na interação com um dos botões referente ao mês da fatura
- **Onde:** Na tela de Faturas no gráfico de total da fatura

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:consulta-de-fatura",
        	eventAction: "clique:botao",
        	eventLabel: "grafico-fatura:[[mes]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[mes]] |  | Deve retornar o título do mês clicado pelo usuário. Ex.: &#039;set&#039;, &#039;out&#039;, &#039;nov&#039;, etc |

<br />

- **Quando:** Na interação com os botões &quot;Pagar em PDF&quot;, &quot;Visualizar a fatura em PDF&quot; e &quot;Pagar Fatura&quot;
- **Onde:** Na tela de Faturas

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:consulta-de-fatura",
        	eventAction: "clique:botao",
        	eventLabel: "[[titulo-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[titulo-botao]] |  | Deve retornar o título do botões clicado. Ex.:  &#039;pagar-em-pdf&#039;, &#039;visualizar-a-fatura-em-pdf&#039; e &#039;pagar-fatura&#039; |

<br />

- **Quando:** Ao selecionar uma das opções de cartão
- **Onde:** Na tela de Faturas

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:consulta-de-fatura",
        	eventAction: "select:opcao",
        	eventLabel: "[[opcao-selecionada]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] |  | Deve retornar a opção selecionada pelo usuário. Ex.: &#039;titular&#039; ou &#039;adicional&#039; |

<br />

- **Quando:** No callback de sucesso ou erro após gerar o boleto
- **Onde:** Na tela de sucesso ou erro para gerar o boleto

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:consulta-de-fatura",
        	eventAction: "callback:boleto",
        	eventLabel: "[[consulta-pagamento]]:[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[consulta-pagamento]] |  | Deve retornar se o boleto que esta sendo gerado pelo usuário é para Pagamento de fatura ou apenas visualização. Ex.: &#039;visualizar-fatura&#039; quando o boleto é gerado a partir do clique em Visualizar a fatura em PDF ou &#039;pagar-fatura&#039; quando o boleto é gerado a partir do clique em Pagar fatura |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:falha-ao-gerar-o-boleto&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

### Pagar Fatura - Fase 2.2

- **Onde:** Visualização da tela de &#039;Pagamento de Fatura&#039;

```javascript
    Analytics.logScreenView("/pagamento-de-fatura/");
```


<br />

- **Quando:** Na interação com o card &quot;Pagar total&quot;
- **Onde:** Na primeira tela de Pagamento de fatura

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:pagamento-de-fatura",
        	eventAction: "interagiu:card",
        	eventLabel: "pagar-total"
        });
```


<br />

- **Quando:** Após a seleção de um valor de parcela
- **Onde:** Na primeira tela de Pagamento de fatura dentro do card &quot;Parcelar Fatura&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:pagamento-de-fatura",
        	eventAction: "interagiu:card",
        	eventLabel: "parcelar-fatura:[[valor-parcela]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[valor-parcela]] |  | Deve retornar o valor da parcela selecionada pelo usuário. Ex.: &#039;10x-de-70.00&#039;, &#039;6x-de-110.00&#039;, &#039;2x-de-320.00&#039;, etc |

<br />

- **Quando:** Na interação com o card &quot;Pagar outro valor&quot; após a seleção ou preenchimento do valor que será pago
- **Onde:** Na primeira tela de Pagamento de fatura

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:pagamento-de-fatura",
        	eventAction: "interagiu:card",
        	eventLabel: "outro-valor"
        });
```


<br />

- **Quando:** No clique do link &quot;Consultar condições e tarifas&quot;
- **Onde:** Na primeira tela de Pagamento de fatura

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:pagamento-de-fatura",
        	eventAction: "clique:link",
        	eventLabel: "consultar-condicoes-e-tarifas"
        });
```


<br />


```javascript
    Analytics.logScreenView("/pagamento-de-fatura/condicoes-e-tarifas/");
```


<br />

- **Quando:** No clique do botão continuar
- **Onde:** Na primeira tela de Pagamento de fatura

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:pagamento-de-fatura",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

- **Onde:**  Visualização da tela de &#039;Pagamento de Fatura&#039; após a seleção do tipo de pagamento

```javascript
    Analytics.logScreenView("/pagamento-de-fatura/[[tipo-pagamento]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-pagamento]] |  | Deve retornar o valor conforme a seleção de tipo de pagamento efetuada pelo cliente. Ex.: &#039;pagar-total&#039;, &#039;parcelar-fatura&#039; ou &#039;pagar-outro-valor&#039; |

<br />

- **Quando:** No clique dos botões &quot;Pagar com a conta Midway&quot; ou &quot;Gerar boleto&quot; após o usuário selecionar uma das opções de pagamento
- **Onde:** Na tela de Pagamento de fatura exibidos após o clique no botão continuar

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:pagamento-de-fatura:[[tipo-pagamento]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-pagamento]] |  | Deve retornar o valor conforme a seleção de tipo de pagamento efetuada pelo cliente. Ex.: &#039;pagar-total&#039;, &#039;parcelar-fatura&#039; ou &#039;pagar-outro-valor&#039; |
| [[nome-botao]] |  | Deve retornar o título do botão clicado pelo usuário. Ex.: &#039;pagar-com-a-conta-midway&#039; ou &#039;gerar-boleto&#039; |

<br />

- **Quando:** No callback de sucesso ou erro para continuar com o pagamento
- **Onde:** Após o clique dos botões &quot;Pagar com a conta Midway&quot; ou &quot;Gerar boleto&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:pagamento-de-fatura:[[tipo-pagamento]]",
        	eventAction: "callback:escolha-pagamento",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-pagamento]] |  | Deve retornar o valor conforme a seleção de tipode pagamento efetuada pelo cliente. Ex.: &#039;pagar-total&#039;, &#039;parcelar-fatura&#039; ou &#039;pagar-outro-valor&#039; |
| [[status]] | &#039;sucesso:recebemos-o-seu-pagamento&#039;, &#039;sucesso:seu-boleto-foi-gerado&#039;, &#039;erro:falha-ao-gerar-o-boleto&#039;, &#039;erro:saldo-insuficiente&#039;, &#039;erro:falha-no-servico&#039;, etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |

<br />

- **Quando:** No clique em um dos botões do modal de saldo insuficiente
- **Onde:** No modal de &quot;Saldo insuficiente&quot; exibido após o clique em &quot;Pagar com a conta Midway&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:pagamento-de-fatura:pagar-total",
        	eventAction: "clique:botao",
        	eventLabel: "saldo-insuficiente:[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] |  | Deve retornar o título do botão clicado pelo usuário. Ex.: &#039;gerar-boleto&#039; ou &#039;voltar&#039; |

<br />

- **Onde:** Visualização da tela de  &#039;Recebemos o seu pagamento &#039;

```javascript
    Analytics.logScreenView("/pagamento-de-fatura/[[tipo-pagamento]]/recebemos-o-seu-pagamento/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-pagamento]] |  | Deve retornar o valor conforme a seleção de tipode pagamento efetuada pelo cliente. Ex.: &#039;pagar-total&#039;, &#039;parcelar-fatura&#039; ou &#039;pagar-outro-valor&#039; |

<br />

- **Quando:** No clique do botão Comprovante
- **Onde:** Na tela de &quot;Recebemos o seu pagamento&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:pagamento-de-fatura:[[tipo-pagamento]]",
        	eventAction: "clique:botao",
        	eventLabel: "comprovante"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-pagamento]] |  | Deve retornar o valor conforme a seleção de tipode pagamento efetuada pelo cliente. Ex.: &#039;pagar-total&#039;, &#039;parcelar-fatura&#039; ou &#039;pagar-outro-valor&#039; |

<br />

- **Onde:**  Visulização da tela de &#039;Comprovante de Pagamento&#039;.

```javascript
    Analytics.logScreenView("/pagamento-de-fatura/[[tipo-pagamento]]/comprovante-de-pagamento/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-pagamento]] |  | Deve retornar o valor conforme a seleção de tipode pagamento efetuada pelo cliente. Ex.: &#039;pagar-total&#039;, &#039;parcelar-fatura&#039; ou &#039;pagar-outro-valor&#039; |

<br />

- **Quando:** No clique do botão Compartilhar
- **Onde:** Na tela de &quot;Comprovante de Pagamento&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:pagamento-de-fatura:[[tipo-pagamento]]",
        	eventAction: "clique:botao",
        	eventLabel: "compartilhar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-pagamento]] |  | Deve retornar o valor conforme a seleção de tipode pagamento efetuada pelo cliente. Ex.: &#039;pagar-total&#039;, &#039;parcelar-fatura&#039; ou &#039;pagar-outro-valor&#039; |

<br />

- **Onde:** Visualização da tela de &#039;Pagamento por boleto&#039;

```javascript
    Analytics.logScreenView("/pagamento-de-fatura/[[tipo-pagamento]]/pagamento-boleto/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-pagamento]] |  | Deve retornar o valor conforme a seleção de tipode pagamento efetuada pelo cliente. Ex.: &#039;pagar-total&#039;, &#039;parcelar-fatura&#039; ou &#039;pagar-outro-valor&#039; |

<br />

- **Quando:** No clique dos botões &quot;Copiar código&quot; ou &quot;Compartilhar&quot;
- **Onde:** Na tela de &quot;Pagamento por boleto&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:pagamento-de-fatura:[[tipo-pagamento]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-pagamento]] |  | Deve retornar o valor conforme a seleção de tipode pagamento efetuada pelo cliente. Ex.: &#039;pagar-total&#039;, &#039;parcelar-fatura&#039; ou &#039;pagar-outro-valor&#039; |
| [[nome-botao]] | &#039;copiar-codigo&#039; ou &#039;compartilhar&#039; | Deve retornar o nome do botão clicado. |

<br />

### Fase 2.3 - Conta Pagamento - Solicitar Cartão

- **Onde:**  Visualização da tela &#039;Cartão de débito&#039;.

```javascript
    Analytics.logScreenView("/conta-pagamento/solicitar-cartao/cartao-de-debito/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Cartão de débito&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;aceito&#039; etc. | Deve retornar o nome do botão clicado. |

<br />


- **Onde:**  Visualização da  tela &#039;Seu cartão será enviado&#039;.

```javascript
    Analytics.logScreenView("/conta-pagamento/solicitar-cartao/cartao-de-debito/seu-cartao-sera-enviado/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Seu cartão será enviado&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito:seu-cartao-sera-enviado",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;cancelar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique do link &#039;Adicionar endereço de entrega&#039;.
- **Onde:** Na tela &#039;Seu cartão será enviado&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito:seu-cartao-sera-enviado",
        	eventAction: "clique:link",
        	eventLabel: "adicionar-endereco"
        });
```


<br />

 - **Onde:** Visualização da tela &#039;Endereço&#039;.

```javascript
    Analytics.logScreenView("/conta-pagamento/solicitar-cartao/cartao-de-debito/seu-cartao-sera-enviado/endereco/");
```


<br />

- **Quando:** Após preenchimento do campo &#039;CEP&#039;.
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito:seu-cartao-sera-enviado:endereco",
        	eventAction: "preencheu:campo",
        	eventLabel: "cep"
        });
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito:seu-cartao-sera-enviado:endereco",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;nao-sei-meu-cep&#039;, &#039;continuar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique do link &#039;Terminar depois&#039;.
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito:seu-cartao-sera-enviado:endereco",
        	eventAction: "clique:link",
        	eventLabel: "terminar-depois"
        });
```


<br />

- **Quando:** No callback de sucesso ou erro após preenchimento do campo &#039;CEP&#039;.
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito:seu-cartao-sera-enviado:endereco",
        	eventAction: "callback:cep",
        	eventLabel: "sucesso ou erro:[[tipo-erro]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-erro]] | &#039;erro:cep-nao-existe&#039;, &#039;erro:preencha-cep&#039; etc. | Deve retornar o tipo do erro ocorrido. |

<br />

- **Quando:** Após preenchimento de um dos campos do formulário de endereço.
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito:seu-cartao-sera-enviado:endereco",
        	eventAction: "preencheu:campo",
        	eventLabel: "[[nome-campo]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;endereco&#039;, &#039;numero&#039;, &#039;complemento&#039;, &#039;bairro&#039;, &#039;cidade&#039;, &#039;estado&#039; etc. | Deve retornar o nome do campo preenchido pelo usuário. |

<br />

- **Quando:** No callback de sucesso ou erro após preenchimento dos campos de endereço.
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito:seu-cartao-sera-enviado:endereco",
        	eventAction: "callback:[[nome-campo]]",
        	eventLabel: "sucesso ou erro:[[tipo-erro]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-erro]] | &#039;erro:preencha-bairro&#039;, &#039;erro:numero-invalido&#039; etc. | Deve retornar o tipo do erro ocorrido. |

<br />

- **Onde:**  Visualização da  tela &#039;Endereço adicionado&#039;.



```javascript
    Analytics.logScreenView("/conta-pagamento/solicitar-cartao/cartao-de-debito/seu-cartao-sera-enviado/endereco/sucesso/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Endereço adicionado&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito:seu-cartao-sera-enviado:endereco-adicionado",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;continuar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:**  Visualização da  tela &#039;Cartão solicitado com sucesso!&#039;.

```javascript
    Analytics.logScreenView("/conta-pagamento/solicitar-cartao/cartao-de-debito/seu-cartao-sera-enviado/sucesso/");
```


<br />

- **Quando:** No clique do botão &#039;Continuar&#039;.
- **Onde:** Na tela &#039;Cartão solicitado com sucesso!&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito:sucesso",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

- **Quando:** No callback de sucesso após solicitar o cartão.
- **Onde:** Na tela &#039;Cartão solicitado com sucesso!&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:solicitar-cartao:cartao-de-debito:sucesso",
        	eventAction: "callback:cartao",
        	eventLabel: "sucesso",
        	dimension2: "[[dimension2]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[dimension2]] | &quot;conta-corrente&quot;, &quot;cartao-de-credito&quot; ou &quot;conta-corrente-e-cartao-de-credito&quot; | Deve retornar se o usuário possui uma conta corrente, cartão de crédito ou as duas opções |

<br />

### Conta Pagamento - Home

- **Onde:**  Visualização da  tela &#039;Home&#039;.

```javascript
    Analytics.logScreenView("/conta-pagamento/home/");
```


<br />

- **Quando:** No clique no nome do cliente, que abre a tela de perfil.
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:home",
        	eventAction: "clique:nome-cliente",
        	eventLabel: "abrir-perfil"
        });
```


<br />

- **Quando:** No clique em um dos cards.
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:home",
        	eventAction: "clique:card",
        	eventLabel: "[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;deposito-por-boleto&#039;, &#039;indicar-amigos&#039;, &#039;pagamentos&#039;, &#039;mudar-para-conta-corrente&#039; etc. | Deve retornar o nome do card clicado. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:home",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;notificacao&#039;, &#039;mostrar-saldo&#039;, &#039;ocultar-saldo&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** Na visualização de uma das opções do carrossel.
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:home",
        	eventAction: "mudou:carrossel",
        	eventLabel: "[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;saldo-disponivel&#039;, &#039;cartao-de-debito-midway&#039;, &#039;atualize-seus-dados-cadastrais&#039; etc. | Deve retornar o nome do card exibido no carrossel. |

<br />

- **Quando:** No clique em um dos links.
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:home:[[nome-card]]",
        	eventAction: "clique:link",
        	eventLabel: "[[nome-link]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;ver-extrato&#039;, &#039;adquirir&#039;, &#039;acompanhar-pedido&#039;, &#039;atualizar&#039; etc. | Deve retornar o texto do link clicado. |

<br />

- **Quando:** No clique do banner.
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:home",
        	eventAction: "clique:banner",
        	eventLabel: "[[nome-banner]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-banner]] | &#039;assistencia-automovel-midway&#039; etc. | Deve retornar o nome do banner atualmente exibito na tela (acredito que ele varie. Se não variar, retorne &#039;assistencia-automovel-midway&#039;). |

<br />

- **Quando:** No clique do botão &#039;Desbloquear&#039; que aparece após solicitação de cartão de débito Midway efetuada com sucesso.
- **Onde:** No card &#039;Cartão de débito Midway&#039; na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:home:cartao-de-debito-midway",
        	eventAction: "clique:botao",
        	eventLabel: "desbloquear"
        });
```


<br />

### Conta Pagamento - Perfil

- **Onde:**  Visualização da  tela &#039;Perfil&#039;.

```javascript
    Analytics.logScreenView("/conta-pagamento/perfil/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Perfil&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:perfil",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039; ou &#039;sair&#039;. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique em uma das ações possíveis.
- **Onde:** Na tela &#039;Perfil&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:perfil",
        	eventAction: "clique:opcao",
        	eventLabel: "[[nome-opcao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-opcao]] | &#039;alterar-senha&#039;, &#039;bloquear-cartao&#039;, &#039;desbloquear-cartao&#039;, &#039;alterar-data-de-vencimento-do-cartao&#039;, &#039;cancelar-conta-simples&#039; etc. | Deve retornar o nome da opção clicada. |

<br />

### Fase 2.2.5 - Conta Simples e Conta Completa - Depósito via Boleto

- **Onde:**  Visualização da tela &#039;Qual o valor do depósito&#039;, na funcionalidade de &quot;Conta Simples&quot; e &quot;Conta Completa&quot;

```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/deposito/qual-o-valor-do-deposito/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No callback de erro após preencher um campo com erro
- **Onde:** Na tela &#039;Qual o valor do depósito&#039;, na funcionalidade de &quot;Conta Simples&quot; e &quot;Conta Completa&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:deposito-via-boleto",
        	eventAction: "interacao:campo:callback",
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[status]] | &#039;valor-maximo-boleto-r$...&#039;, &#039;valor-minimo-boleto-r$...&#039; e etc | Deve retornar o status do callback após preencher o campo. |

<br />

- **Quando:** No clique do botão de &quot;Gerar Boleto&quot; e no &quot;Voltar&quot;
- **Onde:** Na tela &#039;Qual o valor do depósito&quot;, na funcionalidade de &quot;Conta Simples&quot; e &quot;Conta Completa&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:deposito-via-boleto",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;gerar-boleto&#039;, &#039;voltar&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização dos &#039;modais de atenção&#039;

```javascript
    Analytics.logScreenView("/conta simples/deposito/qual-o-valor-do-deposito/modal-[[tipo-atencao]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-atencao]] | &#039;atencao-primeira&#039;, &#039;atencao-irregular&#039;, &#039;atencao-abertos&#039; e etc | Deve retornar o tipo de atenção apresentado. |

<br />

- **Quando:** No clique dos botões de &quot;Continuar&quot; ou &quot;Fechar&quot;
- **Onde:** Nos modais de atenção

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:deposito-via-boleto",
        	eventAction: "clique:botao:modal-[[tipo-atencao]]",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-atencao]] | &#039;atencao-primeira&#039;, &#039;atencao-irregular&#039;, &#039;atencao-abertos&#039; e etc | Deve retornar o tipo de atenção apresentado. |
| [[nome-botao]] | &#039;continuar&#039;, &#039;voltar&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização dos modais de &quot;Mudar para conta completa&quot;.


```javascript
    Analytics.logScreenView("/conta simples/deposito/qual-o-valor-do-deposito/modal-[[tipo-atencao]]/modal-mudar-conta-completa/passo-[[numero passo]]");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tipo-atencao]] | &#039;atencao-primeira&#039;, &#039;atencao-irregular&#039;, &#039;atencao-abertos&#039; e etc | Deve retornar o tipo de atenção apresentado. |
| [[numero passo]] | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o passo que o usuário está. |

<br />

- **Quando:** No clique nos botões de &quot;Fechar&quot; e &quot;Mudar para conta completa&quot;
- **Onde:** Nos modais de &quot;Mudar para conta completa&quot;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:mudar-conta-completa",
        	eventAction: "clique:botao:modal-passo-[[numero-passo]]",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[numero-passo]] | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o passo que o usuário está. |
| [[nome-botao]] | &#039;mudar-conta-completa&#039;, &#039;fechar&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique do card &#039;Boleto bancário&#039;.
- **Onde:** Na tela &#039;Depósito via boleto&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:deposito-via-boleto",
        	eventAction: "clique:card",
        	eventLabel: "boleto-bancario"
        });
```


<br />

- **Quando:** No clique do botão &#039;voltar&#039;.
- **Onde:** Na tela &#039;Depósito via boleto&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:deposito-via-boleto",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Boleto gerado com sucesso&#039;, na funcionalidade de &#039;Conta Simples&#039; e &#039;Conta Completa&#039;.

```javascript
    Analytics.logScreenView("/conta-simples/deposito-via-boleto/boleto-gerado/");
```


<br />

- **Onde:** Na tela &quot;Seu boleto foi gerado!&quot;.


```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/deposito/qual-o-valor-do-deposito/boleto-gerado-com-sucesso/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No callback de sucesso após solicitar o boleto.
- **Onde:** Na tela &#039;Seu boleto foi gerado!&#039; e na tela &quot;Boleto gerado com sucesso!&quot;, na funcionalidade &quot;Conta Simples&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:deposito-via-boleto:boleto-gerado",
        	eventAction: "callback:boleto",
        	eventLabel: "sucesso"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Seu boleto foi gerado!&#039; e na tela &quot;Boleto gerado com sucesso!&quot;, na funcionalidade &quot;Conta Simples&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:deposito-via-boleto:boleto-gerado",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;fechar&#039;, &#039;copiar-codigo&#039;, &#039;visualizar-boleto&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização do modal de &#039;Código de barras copiado&#039;.


```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/deposito/qual-o-valor-do-deposito/boleto-gerado-com-sucesso/codigo-barras-copiado/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique dos botões de &quot;Ok&quot; ou &quot;Fechar&quot;
- **Onde:** No modal de &quot;Código de barras copiado&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:deposito-via-boleto:boleto-gerado",
        	eventAction: "clique:botao:modal-codigo-barras-copiado",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;fechar&#039;, &#039;ok&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique do botão &quot;Compartilhar&quot;, na funcionalidade de Conta Simples
- **Onde:** Na tela &quot;Boleto gerado com sucesso!&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:deposito-via-boleto:boleto-gerado",
        	eventAction: "clique:botao",
        	eventLabel: "compartilhar"
        });
```


<br />

- **Onde:** Visualização da tela do boleto.


```javascript
    Analytics.logScreenView("/[[conta simples ou conta completa]]/deposito-via-boleto/boleto/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela que exibe o boleto.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:deposito-via-boleto:boleto",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-botao]] | &#039;voltar&#039;, &#039;compartilhar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique em uma das opções do submenu &#039;compartilhar&#039;.
- **Onde:** Na tela que exibe o boleto, no submenu aberto pelo botão &#039;compartilhar&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[conta simples ou conta completa]]:deposito-via-boleto:boleto:compartilhar",
        	eventAction: "clique:submenu",
        	eventLabel: "[[nome-opcao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[conta simples ou conta completa]] | &#039;simples&#039; ou &#039;completa&#039;. | Deve retornar o tipo da conta. |
| [[nome-opcao]] | &#039;compartilhar&#039;, &#039;salvar&#039;, &#039;imprimir&#039;, &#039;usar-como&#039;, &#039;detalhes&#039; etc. | Deve retornar o nome do item clicado do submenu. |

<br />

- **Quando:** No callback de sucesso ou erro após realização de alguma das ações do submenu &#039;compartilhar&#039;.
- **Onde:** Na tela que exibe o boleto, no submenu aberto pelo botão &#039;compartilhar&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:deposito-via-boleto:boleto:compartilhar",
        	eventAction: "callback:compartilhar",
        	eventLabel: "sucesso ou erro:[[nome-erro]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-erro]] | &#039;erro:nao-foi-possivel-salvar-boleto&#039;, &#039;erro:nao-foi-possivel-compartilhar&#039; etc. | Deve retornar o nome do erro ocorrido. |

<br />

### Fase 2.4 - Conta Simples - Extrato

- **Onde:** Visualização da tela &#039;Extrato&#039;.

```javascript
    Analytics.logScreenView("/conta-simples/extrato/");
```

<br />


- **Quando:** No clique no link
- **Onde:** Na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato",
        	eventAction: "clique:link",
        	eventLabel: "saldo-bloqueado"
        });
```


<br />


- **Onde:** Visualização do modal de &quot;Valor bloqueado&quot;

```javascript
    Analytics.logScreenView("/conta-simples/extrato/modal-valor-bloqueado/");
```


<br />

- **Quando:** No clique no botão e nos links
- **Onde:** No modal de &quot;Valor bloqueado&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato",
        	eventAction: "clique:[[botao-ou-link]]:modal-valor-bloqueado",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-ou-link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o elemento clicado. |
| [[nome-item]] | &#039;fechar&#039;, &#039;atendimento-telefonico&#039;, &#039;atendimento-via-chat&#039;, &#039;ok&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Quando:** No clique e um dos cards de &#039;Serviços&#039;.
- **Onde:** Na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato",
        	eventAction: "clique:card",
        	eventLabel: "[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;pagamento&#039;, &#039;transferencia&#039;, &#039;desbloquear-cartao&#039; etc. | Deve retornar o nome do card clicado. |

<br />

- **Quando:** No clique do botão de expandir/recolher informações.
- **Onde:** Na tela &#039;Extrato&#039;, abaixo do &#039;Saldo&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato",
        	eventAction: "clique:botao",
        	eventLabel: "expandir-informacoes-extrato ou recolher-informacoes-extrato"
        });
```


<br />

- **Quando:** No clique do botão de informação sobre o Limite emergencial.
- **Onde:** Na tela &#039;Extrato&#039;, após clique do botão de expandir informações.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato",
        	eventAction: "clique:botao",
        	eventLabel: "informacoes-limite-emergencial"
        });
```


<br />

- **Quando:** No clique do link do dia de &#039;Cobrança mensal do uso&#039;.
- **Onde:** Na tela &#039;Extrato&#039;, após clique do botão de expandir informações.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato",
        	eventAction: "clique:link",
        	eventLabel: "limite-emergencial:alterar-data-cobranca"
        });
```


<br />

- **Quando:** No clique do link &#039;Filtrar&#039;.
- **Onde:** Na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato",
        	eventAction: "clique:link",
        	eventLabel: "filtrar"
        });
```


<br />

- **Quando:** No clique em uma das abas da exibição do extrato.
- **Onde:** Na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato",
        	eventAction: "clique:aba",
        	eventLabel: "[[nome-aba]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-aba]] | &#039;tudo&#039;, &#039;entradas&#039;, &#039;saidas&#039; ou &#039;futuro&#039;. | Deve retornar o nome da aba clicada. |

<br />

- **Quando:** No clique em uma das operações exibidas no extrato para abrir comprovante.
- **Onde:** Na tela &#039;Extrato&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato",
        	eventAction: "clique:operacao",
        	eventLabel: "abriu-comprovante"
        });
```


<br />

- **Onde:** Visualização da tela de comprovante de operação.

```javascript
    Analytics.logScreenView("/conta-simples/extrato/comprovante-[[operacao]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[operacao]] | &#039;transferencia&#039;, &#039;compra&#039;, &#039;deposito&#039; etc. | Deve retornar o tipo de operação referente à tela exibida. |

<br />

- **Quando:** No clique do link &#039;Compartilhar&#039;.
- **Onde:** Na tela de comprovante de operação.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato:comprovante-[[operacao]]",
        	eventAction: "clique:link",
        	eventLabel: "compartilhar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[operacao]] | &#039;transferencia&#039;, &#039;compra&#039;, &#039;deposito&#039; etc. | Deve retornar o tipo de operação referente à tela exibida. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela de comprovante de operação.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato:comprovante-[[operacao]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[operacao]] | &#039;transferencia&#039;, &#039;compra&#039;, &#039;deposito&#039; etc. | Deve retornar o tipo de operação referente à tela exibida. |
| [[nome-botao]] | &#039;fazer-outro-pagamento&#039;, &#039;voltar-para-conta-corrente&#039;. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;Alterar data de cobrança mensal do limite emergencial&#039;.

```javascript
    Analytics.logScreenView("/conta-simples/extrato/limite-emergencial/alterar-data-cobranca/");
```


<br />

- **Quando:** Na interação com o slider de &#039;dia&#039;.
- **Onde:** Na tela &#039;Alterar data de cobrança mensal do limite emergencial&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato:limite-emergencial",
        	eventAction: "interacao:slider",
        	eventLabel: "[[dia-escolhido]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[dia-escolhido]] | &#039;1&#039;, &#039;20&#039;, &#039;15&#039; etc. | Deve retornar o novo dia escolhido para cobrança mensal do limite emergencial. |

<br />

- **Quando:** Após preenchimento do campo &#039;dia&#039;.
- **Onde:** Na tela &#039;Alterar data de cobrança mensal do limite emergencial&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato:limite-emergencial",
        	eventAction: "preencheu:campo",
        	eventLabel: "[[dia-escolhido]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[dia-escolhido]] | &#039;1&#039;, &#039;20&#039;, &#039;15&#039; etc. | Deve retornar o novo dia escolhido para cobrança mensal do limite emergencial. |

<br />

- **Quando:** No clique do botão &#039;Alterar&#039;.
- **Onde:** Na tela &#039;Alterar data de cobrança mensal do limite emergencial&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato:limite-emergencial",
        	eventAction: "clique:botao",
        	eventLabel: "alterar"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Filtro&#039;, aberta após clique no link &#039;Filtrar&#039; na tela &#039;Extrato&#039;.

```javascript
    Analytics.logScreenView("/conta-simples/extrato/filtro/");
```


<br />

- **Quando:** No clique em uma das opções de &#039;Selecione um período em dias&#039;.
- **Onde:** Na tela &#039;Filtro&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato:filtro",
        	eventAction: "clique:opcao",
        	eventLabel: "[[periodo-em-dias]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[periodo-em-dias]] | &#039;30&#039;, &#039;60&#039;, &#039;90&#039;, &#039;180&#039;, &#039;365&#039;. | Deve retornar o período em dias clicado. |

<br />

- **Quando:** Após o preenchimento de um dos campos.
- **Onde:** Na tela &#039;Filtro&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato:filtro",
        	eventAction: "preencheu:campo",
        	eventLabel: "[[nome-campo]]:[[data]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-campo]] | &#039;de:29-02-2020&#039;, &#039;ate:31-03-2020&#039; etc. | Deve retornar o nome do campo preenchido. |

<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Filtro&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-simples:extrato:filtro",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;fechar&#039;, &#039;filtrar&#039;. | Deve retornar o nome do botão clicado. |

<br />

### Fase 2.5 - Conta corrente - Parcelamento Cheque Especial


- **Onde:** Visualização da tela &#039;Parcelamento limite emergencial&#039;.

```javascript
    Analytics.logScreenView("/conta-corrente/parcelamento-limite-emergencial/");
```


<br />

- **Quando:** No clique na opção de &quot;Escolha uma data&quot;
- **Onde:** Na tela &#039;Parcelamento limite emergencial&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:parcelamento-limite-emergencial",
        	eventAction: "clique:opcao",
        	eventLabel: "data-selecionada:[[data-selecionada]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[data-selecionada]] | &#039;13/04/2017&#039;, &#039;15/02/2020&#039; e etc | Deve retornar a data selecionada. |

<br />

- **Quando:** No clique na opção de &quot;Escolha as parcelas&quot;
- **Onde:** Na tela &#039;Parcelamento limite emergencial&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:parcelamento-limite-emergencial",
        	eventAction: "clique:opcao",
        	eventLabel: "quantidade-parcelas:[[quantidade-parcelas]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[quantidade-parcelas]] | &#039;10x&#039;, &#039;8x&#039; e etc | Deve retornar a quantidade de parcelas selecionadas. |

<br />

- **Quando:** No clique no botão de &quot;Continuar&quot;
- **Onde:** Na tela &#039;Parcelamento limite emergencial&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:parcelamento-limite-emergencial",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

- **Onde:**  Visualização da tela &#039;Sua simulação está pronta&#039;.

```javascript
    Analytics.logScreenView("/conta-corrente/parcelamento-limite-emergencial/sua-simulacao-esta-pronta/");
```


<br />

- **Quando:** No clique no botão de &quot;Contratar&quot;
- **Onde:** Na tela &#039;Sua simulação está pronta&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:parcelamento-limite-emergencial:sua-simulacao-esta-pronta",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;detalhamento-do-cet&#039;, &#039;contratar&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela  &#039;Detalhamento do CET &#039;.

```javascript
    Analytics.logScreenView("/conta-corrente/parcelamento-limite-emergencial/sua-simulacao-esta-pronta/detalhamento-do-cet/");
```


<br />

- **Onde:** Visualização da tela &#039;Termos e Condições Gerais&#039;

```javascript
    Analytics.logScreenView("/conta-corrente/parcelamento-limite-emergencial/sua-simulacao-esta-pronta/termos-condicoes-gerais/");
```


<br />

- **Quando:** No clique no botão de &quot;Li e Concordo&quot;
- **Onde:** Na tela &#039;Termos e Condições Gerais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:parcelamento-limite-emergencial:sua-simulacao-esta-pronta",
        	eventAction: "clique:botao",
        	eventLabel: "termo-condicoes-gerais:li-e-concordo"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Token&#039;

```javascript
    Analytics.logScreenView("/conta-corrente/parcelamento-limite-emergencial/token/");
```


<br />

- **Onde:** Visualização da tela &#039;Contratação Realizada com Sucesso&#039;


```javascript
    Analytics.logScreenView("/conta-corrente/parcelamento-limite-emergencial/contratacao-realizada-com-sucesso/");
```


<br />

- **Quando:** No clique nos botões de &quot;Compartilhar Contrato&quot; ou &quot;Ok&quot;
- **Onde:** Na tela &#039;Contratação Realizada com Sucesso&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-corrente:parcelamento-limite-emergencial:contratacao-realizada-sucesso",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;compartilhar-contrato&#039;, &#039;ok&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

### Fase 2.5 - Conta Pagamento - Migração para Conta Corrente
- **Quando:** No clique no botão de &quot;Começar&quot;, no card de &quot;Mudar para Conta Completa&quot;
- **Onde:** Na tela &#039;Home&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:home",
        	eventAction: "clique:botao",
        	eventLabel: "comecar"
        });
```


<br />


- **Onde:** Visualização da tela &#039;Não podemos prosseguir&#039;.

```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/nao-podemos-prosseguir/");
```


<br />

- **Quando:** No clique no botão de &quot;Ok&quot;
- **Onde:** Na tela &#039;Não podemos prosseguir&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:nao-podemos-prosseguir",
        	eventAction: "clique:botao",
        	eventLabel: "ok"
        });
```


<br />


- **Onde:** Visualização da tela &#039;Abra sua Conta Completa&#039;

```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/abra-sua-conta-completa/");
```


<br />


- **Onde:** Visualização da tela &#039;Aqui sua Conta rende mais&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/aqui-sua-conta-rende-mais/");
```


<br />

- **Onde:** Visualização da tela &#039;Limite emergencial ideal para você&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/limite-emergencial-ideal-para-voce/");
```


<br />

- **Quando:** No clique no botão de &quot;Abrir conta Completa&quot;
- **Onde:** Na tela &#039;Limite emergencial ideal para você&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:limite-emergencial-ideal-para-voce",
        	eventAction: "clique:botao",
        	eventLabel: "abrir-conta-completa"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Dados Cadastrais&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/dados-cadastrais/");
```


<br />

- **Quando:** No clique na opção de &quot;Gênero&quot; e &quot;Nacionalidade&quot;
- **Onde:** Na tela &#039;Dados Cadastrais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:dados-cadastrais",
        	eventAction: "clique:opcao",
        	eventLabel: "[[opcao-selecionada]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] | &#039;genero&#039;, &#039;nacionalidade&#039; e etc | Deve retornar a opção selecionada. |

<br />

- **Quando:** No clique no botão de &quot;Continuar&quot; ou &quot;Terminar Depois&quot;
- **Onde:** Na tela &#039;Dados Cadastrais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:dados-cadastrais",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039; , &#039;terminar-depois&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;Endereço&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/endereco/");
```


<br />

- **Quando:** No clique no botão de &quot;Continuar&quot;, &quot;Não sei meu Cep&quot; ou &quot;Terminar Depois&quot;
- **Onde:** Na tela &#039;Endereço&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:endereco",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039; , &#039;nao-sei-meu-cep&#039;, &#039;terminar-depois&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No clique no checkbox de &quot;Sou ou tenho alguma relação com uma Pessoa Exposta Politicamente&quot;
- **Onde:** Na tela &#039;Dados Cadastrais&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:dados-cadastrais",
        	eventAction: "clique:checkbox",
        	eventLabel: "sou-ou-tenho-relacao-com-pep:checkbox-selecionado"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Pessoa Exposta Politicamente&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/dados-cadastrais/pessoa-exposta-politicamente/");
```


<br />

- **Onde:** Visualização da tela &#039;Documento&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/documento/");
```


<br />

- **Quando:** No clique em um dos cards.
- **Onde:** Na tela &#039;Documento&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:documento",
        	eventAction: "clique:card",
        	eventLabel: "[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;rg&#039;, &#039;cnh&#039;, &#039;rne&#039; e etc. | Deve retornar o nome do card clicado. |

<br />

- **Onde:** Visualização da tela &#039;RG&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/documento/rg/");
```


<br />

- **Quando:** No clique no botão de &quot;Tirar foto do documento&quot; ou &quot;Terminar Depois&quot;
- **Onde:** Na tela &#039;RG&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:documento:rg",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;tirar-foto-documento&#039;, &#039;terminar-depois&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;CNH&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/documento/cnh/");
```


<br />

- **Quando:** No clique no botão de &quot;Tirar foto do documento&quot; ou &quot;Terminar Depois&quot;
- **Onde:** Na tela &#039;CNH&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:documento:cnh",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;tirar-foto-documento&#039;, &#039;terminar-depois&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;RNE&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/documento/rne/");
```


<br />

- **Quando:** No clique no botão de &quot;Tirar foto do documento&quot; ou &quot;Terminar Depois&quot;
- **Onde:** Na tela &#039;RNE&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:documento:rne",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;tirar-foto-documento&#039;, &#039;terminar-depois&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;Foto do documento&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/documento/[[modo-de-documento]]/foto-do-documento/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[modo-de-documento]] | &#039;rg&#039;, &#039;cnh&#039;, &#039;rne&#039; e etc | Deve retornar o nome do documento selecionado. |

<br />

- **Quando:** No clique no botão de &quot;Continuar&quot;
- **Onde:** Na tela &#039;Foto do documento&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:documento:[[modo-de-documento]]:foto-do-documento",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[modo-de-documento]] | &#039;rg&#039;, &#039;cnh&#039;, &#039;rne&#039; e etc | Deve retornar o nome do documento selecionado. |

<br />

- **Onde:** Visualização da tela &#039;Tiraremos uma foto da frente&#039;do documento selecionado


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/documento/[[modo-de-documento]]/foto-do-documento/tiraremos-uma-foto-da-frente/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[modo-de-documento]] | &#039;rg&#039;, &#039;cnh&#039;, &#039;rne&#039; e etc | Deve retornar o nome do documento selecionado. |

<br />

- **Quando:** No clique no botão de &quot;Continuar&quot; ou &quot;Capturar novamente&quot;
- **Onde:** Na tela &#039;Tiraremos uma foto da frente&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:documento:[[modo-de-documento]]:foto-do-documento:foto-da-frente",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;capturar-novamente&#039; e etc | Deve retornar o nome do botão clicado. |

<br />


- **Onde:** Visualização da tela &#039;Tiraremos uma foto do verso&#039;do documento selecionado


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/documento/[[modo-de-documento]]/foto-do-documento/tiraremos-uma-foto-do-verso/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[modo-de-documento]] | &#039;rg&#039;, &#039;cnh&#039;, &#039;rne&#039; e etc | Deve retornar o nome do documento selecionado. |

<br />

- **Quando:** No clique no botão de &quot;Continuar&quot; ou &quot;Capturar novamente&quot;
- **Onde:** Na tela &#039;Tiraremos uma foto do verso&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:documento:[[modo-de-documento]]:foto-do-documento:foto-do-verso",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;capturar-novamente&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;salvando-seus-dados&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/documento/[[modo-de-documento]]/foto-do-documento/salvando-seus-dados/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[modo-de-documento]] | &#039;rg&#039;, &#039;cnh&#039;, &#039;rne&#039; e etc | Deve retornar o nome do documento selecionado. |

<br />

- **Onde:** Visualização da tela &#039;algo-deu-errado'&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/documento/[[modo-de-documento]]/foto-do-documento/algo-deu-errado/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[modo-de-documento]] | &#039;rg&#039;, &#039;cnh&#039;, &#039;rne&#039; e etc | Deve retornar o nome do documento selecionado. |

<br />

- **Quando:** No clique no botão de &quot;Ok&quot;
- **Onde:** Na tela &#039;Algo deu errado&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:documento:[[modo-de-documento]]:algo-deu-errado",
        	eventAction: "clique:botao",
        	eventLabel: "ok"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Cesta de Serviços&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/cesta-de-servicos/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[modo-de-documento]] | &#039;rg&#039;, &#039;cnh&#039;, &#039;rne&#039; e etc | Deve retornar o nome do documento selecionado. |

<br />

- **Quando:** No clique no botão de &quot;Continuar&quot; ou &quot;Capturar novamente&quot;
- **Onde:** Na tela &#039;Cesta de Serviços&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:cesta-de-servicos",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;terminar-depois&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização dos cards &#039;pacote basico&#039;, &#039;pacote light&#039; ou &#039;pacote premium&#039; na tela de &#039;Cesta de Serviços&#039;

```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/cesta-de-servicos/[[card-visualizado]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-visualizado]] | &#039;pacote-basico&#039;, &#039;pacote-light&#039;, &#039;pacote-premium&#039; e etc | Deve retornar o nome do card visualizado. |

<br />

- **Quando:** No clique no botão de &quot;Escolher este pacote&quot; ou &quot;Terminar Depois&quot;
- **Onde:** Na tela &#039;Cesta de Serviços&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:cesta-de-servicos",
        	eventAction: "clique:botao",
        	eventLabel: "[[pacote-selecionado]]:[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pacote-selecionado]] | &#039;pacote-basico&#039;, &#039;pacote-light&#039;, &#039;pacote-premium&#039; e etc | Deve retornar o nome do pacote selecionado . |
| [[nome-botao]] | &#039;escolher-este-pacote&#039;, &#039;terminar-depois&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;Limite Emergencial&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/cesta-de-servicos/limite-emergencial/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-visualizado]] | &#039;pacote-basico&#039;, &#039;pacote-light&#039;, &#039;pacote-premium&#039; e etc | Deve retornar o nome do card visualizado. |

<br />

- **Quando:** No clique nos botões de &quot;Aceito&quot;, &quot;Consulte taxas e condições&quot; ou &quot;Agora não&quot;
- **Onde:** Na tela &#039;Limite Emergencial&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:cesta-de-servicos:limite-emergencial",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;aceito&#039;, &#039;agora-nao&#039;, &#039;consulte-taxas-e-condicoes&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;Data de Cobrança&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/cesta-de-servicos/limite-emergencial/data-de-cobranca/");
```


<br />

- **Quando:** Na interação com o slider de data de cobrança.
- **Onde:** Na tela &#039;Data de Cobrança&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:cesta-de-servicos:limite-emergencial:data-cobranca",
        	eventAction: "interacao:slider",
        	eventLabel: "[[dia-cobranca]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[dia-cobranca]] | &#039;14&#039;, &#039;25&#039; etc. | Deve retornar o dia escolhido pelo cliente. |

<br />

- **Quando:** No clique no botão de &quot;Selecionar&quot;.
- **Onde:** Na tela &#039;Data de Cobrança&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:cesta-de-servicos:limite-emergencial:data-cobranca",
        	eventAction: "clique:botao",
        	eventLabel: "selecionar"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Você pode usar o mesmo cartão que já possui&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/cartao/voce-pode-usar-o-mesmo-cartao-que-ja-possui/");
```


<br />

- **Quando:** No clique no botão de &quot;OK&quot;.
- **Onde:** Na tela &#039;Você pode usar o mesmo cartão que já possui&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:cartao:voce-pode-usar-o-mesmo-cartao-que-ja-possui",
        	eventAction: "clique:botao",
        	eventLabel: "ok"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Seu cartão está a caminho&#039; caso o usuário ainda não tenha cartão.


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/cartao/seu-cartao-esta-a-caminho/");
```


<br />

- **Quando:** No clique no botão de &quot;OK&quot;ou &quot;Adicionar endereço&quot;.
- **Onde:** Na tela &#039;Seu cartão está a caminho&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:cartao:seu-cartao-esta-a-caminho",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;adicionar-endereco&#039;, &#039;ok&#039; e etc | Deve retornar o nome do botão selecionado. |

<br />

- **Onde:** Visualização da tela &#039;Endereço de Entrega&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/cartao/seu-cartao-esta-a-caminho/endereco-de-entrega");
```


<br />

- **Quando:** No clique no botão de &quot;Não sei meu cep&quot;, &quot;Continuar&quot;ou &quot;Terminar depois&quot;.
- **Onde:** Na tela &#039;Endereço de Entrega&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:cartao:seu-cartao-esta-a-caminho:endereco-de-entrega",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;nao-sei-meu-cep&#039;, &#039;continuar&#039;, &#039;terminar-depois&#039; e etc | Deve retornar o nome do botão selecionado. |

<br />

- **Onde:** Visualização da tela &#039;Produtos Contratados&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/produtos-contratados");
```


<br />

- **Quando:** No clique no ícone de &quot;Editar&quot; em cada card
- **Onde:** Na tela &#039;Produtos Contratados&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:produtos-contratados",
        	eventAction: "clique:icone",
        	eventLabel: "editar:[[nome-card]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-card]] | &#039;cesta-de-servico&#039;, &#039;limite-emergencial&#039;, &#039;cartao-credito&#039; e etc | Deve retornar o nome do card selecionado para editar. |

<br />

- **Quando:** No clique no botão de &quot;Continuar&quot; ou &quot;Compartilhar&quot;.
- **Onde:** Na tela &#039;Produtos Contratados&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:produtos-contratados",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;compartilhar&#039; e etc | Deve retornar o nome do botão selecionado. |

<br />

- **Onde:** Visualização da tela &#039;Termo de Aceite&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/termo-de-aceite/");
```


<br />

- **Quando:** No clique no botão de &quot;Li e Concordo&quot; ou &quot;Compartilhar&quot;.
- **Onde:** Na tela &#039;Termo de Aceite&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:termo-de-aceite",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;li-e-concordo&#039;, &#039;compartilhar&#039; e etc | Deve retornar o nome do botão selecionado. |

<br />

- **Onde:** Visualização da tela &#039;Conta Completa concluída com Sucesso&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/migracao-conta-corrente/conta-completa-concluida-sucesso/");
```


<br />

- **Quando:** Ao clicar no botão de &quot;Entrar no aplicativo&quot; ou &quot;Compartilhar&quot;
- **Onde:** Na tela &#039;Conta Completa Concluída com Sucesso&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:migracao-conta-corrente:conta-completa-sucesso",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;entrar-no-aplicativo&#039;, &#039;compartilhar&#039; e etc | Deve retornar o nome do botão selecionado. |

<br />

### Fase 2.6 - Conta Pagamento - Encerrar Conta

- **Onde:** Visualização da tela &#039;Cancelamento de conta pagamento&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/cancelamento/");
```


<br />

- **Quando:** Na seleção de uma das opções do select &#039;Motivo de cancelamento&#039;.
- **Onde:** Na tela &#039;Cancelamento de conta pagamento&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:cancelamento",
        	eventAction: "selecao:opcao",
        	eventLabel: "[[motivo-cancelamento]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[motivo-cancelamento]] | &#039;nao-uso-muito&#039;, &#039;trabalho-com-outros-bancos&#039;, &#039;aplicativo-nao-tem-uma-navegacao-intuitiva&#039; etc. | Deve retornar a opção selecionada pelo usuário no select de &#039;Motivo do cancelamento&#039;. |

<br />

- **Quando:** No clique em algum dos botões.
- **Onde:** Na tela &#039;Cancelamento de conta pagamento&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:cancelamento",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;Condições de cancelamento&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/cancelamento/condicoes/");
```


<br />

- **Quando:** No clique em algum dos botões.
- **Onde:** Na tela &#039;Condições de cancelamento&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:cancelamento:condicoes",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;continuar&#039;, &#039;voltar&#039; etc. | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela de validação do cancelamento.


```javascript
    Analytics.logScreenView("/conta-pagamento/cancelamento/validacao/");
```


<br />

- **Quando:** Na abertura do modal de confirmação de cancelamento.
- **Onde:** Na tela de validação do cancelamento.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:cancelamento:validacao",
        	eventAction: "abriu:modal",
        	eventLabel: "confirmar-cancelamento"
        });
```


<br />

- **Quando:** No clique em algum dos botões.
- **Onde:** No modal de confirmação de cancelamento na tela de validação do cancelamento.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:cancelamento:validacao",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;confirmar&#039; ou &#039;agora-nao&#039;. | Deve retornar o nome do botão clicado. |

<br />

- **Quando:** No callback de sucesso ou erro no cancelamento da conta.
- **Onde:** Na tela de validação do cancelamento.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:cancelamento:validacao",
        	eventAction: "callback:cancelamento",
        	eventLabel: "sucesso ou erro:[[nome-erro]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-erro]] | &#039;sucesso&#039;, &#039;erro:nao-foi-possivel-realizar-cancelamento&#039;, &#039;erro:tente-novamente-mais-tarde&#039; etc. | Deve retornar o nome do erro ocorrido. |

<br />

- **Quando:** Na abertura do modal &#039;Sua solicitação foi enviada. Entraremos em contato&#039;.
- **Onde:** Na tela de validação do cancelamento.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:cancelamento:validacao",
        	eventAction: "abriu:modal",
        	eventLabel: "solicitacao-enviada"
        });
```


<br />

- **Quando:** No clique do botão &#039;Voltar para Home&#039;.
- **Onde:** No modal &#039;Sua solicitação foi enviada. Entraremos em contato.&#039; na tela de validação do cancelamento.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:cancelamento:validacao",
        	eventAction: "clique:botao",
        	eventLabel: "voltar-para-home"
        });
```


<br />

### Fase 2.6 - Conta Pagamento - Bloquear e desbloquear cartão

- **Onde:** Na abertura da tela &#039;Seu cartão foi desbloqueado!&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/desbloquear-cartao/");
```


<br />

- **Quando:** No clique do botão &#039;Voltar para Home&#039;.
- **Onde:** Na tela &#039;Seu cartão foi desbloqueado!&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:desbloquear-cartao",
        	eventAction: "clique:botao",
        	eventLabel: "voltar-para-home"
        });
```


<br />

- **Quando:** No callback de sucesso ou erro no desbloqueio do cartão.
- **Onde:** Na tela &#039;Seu cartão foi desbloqueado!&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:desbloquear-cartao",
        	eventAction: "callback:desbloqueio",
        	eventLabel: "sucesso ou erro:[[nome-erro]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-erro]] | &#039;sucesso&#039;, &#039;erro:nao-foi-possivel-realizar-o-desbloqueio&#039;, &#039;erro:tente-novamente-mais-tarde&#039; etc. | Deve retornar o nome do erro ocorrido. |

<br />


- **Onde:** Na abertura da tela &#039;Seu cartão foi bloqueado, clique para desbloquear&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/bloquear-cartao/");
```


<br />

- **Quando:** No clique em um dos botões.
- **Onde:** Na tela &#039;Seu cartão foi bloqueado, clique para desbloquear&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:bloquear-cartao",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;desbloquear-cartao&#039; ou &#039;voltar-para-home&#039;. | Deve retornar o nome do botão clicado pelo usuário. |

<br />

- **Quando:** No callback de sucesso ou erro no bloqueio do cartão.
- **Onde:** Na tela &#039;Seu cartão foi bloqueado, clique para desbloquear&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:bloquear-cartao",
        	eventAction: "callback:bloqueio",
        	eventLabel: "sucesso ou erro:[[nome-erro]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-erro]] | &#039;sucesso&#039;, &#039;erro:nao-foi-possivel-realizar-o-bloqueio&#039;, &#039;erro:tente-novamente-mais-tarde&#039; etc. | Deve retornar o nome do erro ocorrido. |

<br />

### Fase 2.7 - Conta Pagamento - Transferência Midway para Midway


- **Onde:** Visualização da tela &#039;Para quem quer transferir?&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/transferencia-midway-para-midway/para-quem-transferir/");
```


<br />

- **Quando:** No clique nos ícones de &quot;Contato Favorito&quot;
- **Onde:** Na tela &#039;Para quem quer transferir&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway",
        	eventAction: "clique:icone",
        	eventLabel: "contato-favorito:[[posicao-contato]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[posicao-contato]] | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar a posição do contato escolhido pelo usuário. |

<br />

- **Quando:** No clique no botão de &quot;Transferir para novo contato&quot;
- **Onde:** Na tela &#039;Para quem quer transferir&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway",
        	eventAction: "clique:botao",
        	eventLabel: "transferir-para-novo-contato"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Dados do contato&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/transferencia-midway-para-midway/para-quem-transferir/novo-contato/dados-contato/");
```


<br />

- **Quando:** No clique nas opções de &quot;Instituições Financeiras&quot;
- **Onde:** Na tela &#039;Dados do contato&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:dados-do-contato",
        	eventAction: "clique:opcoes",
        	eventLabel: "instituicao-financeira:[[opcao-selecionada]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] | &#039;000-midway&#039;, &#039;341-itau&#039; e etc | Deve retornar o nome da instituição escolhida pelo usuário. |

<br />

- **Quando:** Na interação com a opção para selecionar se quer &quot;Salvar favorecido na lista de contatos&quot;
- **Onde:** Na tela &#039;Dados do contato&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:dados-do-contato",
        	eventAction: "clique:opcao",
        	eventLabel: "salvar-favorecido-nos-contatos:[[status]]"
        });
```


<br />

- **Quando:** No clique no botão de &quot;Continuar&quot;
- **Onde:** Na tela &#039;Dados do contato&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:dados-do-contato",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Seu saldo disponível&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/transferencia-midway-para-midway/para-quem-transferir/seu-saldo-disponivel/");
```


<br />

- **Quando:** No clique no botão de &quot;editar contato&quot;
- **Onde:** Na tela &#039;Seu saldo disponível&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:seu-saldo-disponivel",
        	eventAction: "clique:botao",
        	eventLabel: "editar-contato"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Qual valor quer transferir&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/transferencia-midway-para-midway/para-quem-transferir/qual-valor-quer-transferir/");
```


<br />

- **Quando:** No clique no botão de &quot;Continuar&quot; para validar o campo preenchido
- **Onde:** Na tela &#039;Qual valor quer transferir&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:qual-valor-quer-transferir",
        	eventAction: "clique:botao:callback",
        	eventLabel: "continuar:[[sucesso-ou-erro]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[sucesso-ou-erro]] | &#039;sucesso&#039; ou &#039;erro:saldo-nao-disponivel&#039; e etc | Deve retornar o status após clicar no botão. |

<br />

- **Onde:** Visualização da tela &#039;Este valor não está disponível na sua conta&#039;. dentro de &#039;Qual valor quer transferir&#039;

```javascript
    Analytics.logScreenView("/conta-pagamento/transferencia-midway-para-midway/para-quem-transferir/qual-valor-quer-transferir/valor-nao-disponivel-na-conta/");
```


<br />

- **Quando:** No clique no botão de &quot;Não, ir para o início&quot;  ou &quot;Sim&quot; caso queira tentar outro valor ou data
- **Onde:** Na tela &#039;Este valor não está disponível na sua conta&#039;, dentro de &quot;Qual valor quer transferir&quot;
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:qual-valor-quer-transferir:valor-nao-disponivel-na-conta",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;sim&#039;, &#039;nao-ir-para-inicio&#039; e etc | Deve retornar o nome do botão clidado pelo usuário. |

<br />

- **Onde:** Visualização da tela &#039;Para quando é sua transferência&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/transferencia-midway-para-midway/para-quem-transferir/para-quando-e-sua-transferencia/");
```


<br />

- **Onde:** Visualização da tela &#039;Escolher a data&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/transferencia-midway-para-midway/para-quem-transferir/para-quando-e-sua-transferencia/escolher-data/");
```


<br />

- **Quando:** No clique do calendário para selecionar uma data
- **Onde:** Na tela &#039;Escolher a data&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:para-quando-sua-transferencia:escolher-data",
        	eventAction: "clique:botao",
        	eventLabel: "data-selecionada:[[data]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[data]] | &#039;13/04/2020&#039;, &#039;03/05/2020&#039; e etc | Deve retornar data selecionada pelo usuário. |

<br />

- **Quando:** No clique no botão de &quot;Ok&quot; ou &quot;Cancelar&quot;
- **Onde:** Na tela &#039;Escolher a data&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:para-quando-sua-transferencia:escolher-data",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;cancelar&#039;, &#039;ok&#039; e etc | Deve retornar o nome do botão clidado pelo usuário. |

<br />

- **Quando:** Na interação a opção &quot;Repetir essa transferência&quot;
- **Onde:** Na tela &#039;Para quando é sua transferência&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:para-quando-sua-transferencia",
        	eventAction: "clique:opcao",
        	eventLabel: "repetir-essa-transferencia:[[status]]"
        });
```


<br />

- **Quando:** No clique nas opções de &quot;Por quanto tempo?&quot;
- **Onde:** Na tela &#039;Para quando é sua transferência&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:para-quando-sua-transferencia",
        	eventAction: "clique:opcoes",
        	eventLabel: "por-quanto-tempo:[[opcao-selecionada]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] | &#039;1-mes&#039;, &#039;2-meses&#039; e etc | Deve retornar a opção selecionada pelo usuário. |

<br />

- **Quando:** No clique no botão de &quot;Continuar&quot;
- **Onde:** Na tela &#039;Para quando é sua transferência&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:para-quando-sua-transferencia",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Confirme os dados da sua transferência&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/transferencia-midway-para-midway/para-quem-transferir/confirme-os-dados-da-sua-transferencia/");
```


<br />

- **Quando:** No clique no botão de &quot;Confirmar transferência&quot;
- **Onde:** Na tela &#039;Confirme os dados da sua transferência&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:confirme-os-dados-da-sua-transferencia",
        	eventAction: "clique:botao",
        	eventLabel: "confirmar-transferencia"
        });
```


<br />

- **Onde:** Visualização da tela de sucesso &#039;Transferência feita&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/transferencia-midway-para-midway/para-quem-transferir/transferencia-feita/");
```


<br />

- **Quando:** No clique nos botões de &quot;Concluir&quot; ou &quot;Compartilhar comprovante&quot;
- **Onde:** Na tela &#039;Transferência feita&#039;.
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-pagamento:transferencia-midway-para-midway:transferencia-feita",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;concluir&#039;, &#039;compartilhar-comprovante&#039; e etc | Deve retornar o nome do botão clicado. |

<br />

- **Onde:** Visualização da tela &#039;Comprovante de transferência entre contas Midwaya&#039;


```javascript
    Analytics.logScreenView("/conta-pagamento/transferencia-midway-para-midway/para-quem-transferir/transferencia-feita/comprovante-de-transferencia-entre-contas-midway/");
```


<br />

### Fase 2.8 - Conta Pagamento - Transferência Midway - Outros Bancos

- **Onde:** Na visualização da tela inicial de Transferência.


```javascript
    Analytics.logScreenView("/transferencia/para-quem-quer-transferir/");
```


<br />

- **Quando:** Na interação com os botões &#039;Editar contato&#039; e &#039;transferir para novo contato&#039;
- **Onde:** Na tela da primeira etapa de transferências &#039;Para quem quer Transferir?&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia:para-quem-quer-transferir",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;editar-contato&#039; ou &#039;transferir-para-novo-contato&#039; | Deve retornar o nome do botão selecionado. |

<br />

- **Onde:** Visualização da tela &#039;Dados do Contato&#039;


```javascript
    Analytics.logScreenView("/transferencia/dados-do-contato/");
```


<br />

- **Quando:** Após a seleção da &#039;Instituição da conta&#039;
- **Onde:** Na tela de &#039;Dados do Contato&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia:dados-do-contato",
        	eventAction: "select:opcao",
        	eventLabel: "instituicao-da-conta:[[opcao-selecionada]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] |  | Deve retornar a opção selecionada pelo usuário. Ex.: &#039;000-midway&#039;, &#039;341-itau-unibanco&#039;, &#039;001-banco-do-brasil&#039;, etc |

<br />

- **Quando:** No clique do botão &#039;Continuar&#039;
- **Onde:** Na tela de &#039;Dados do Contato&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia:dados-do-contato",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

- **Quando:** Na interação para ativar a opção de &#039;Salvar favorecido na lista de contato&#039;
- **Onde:** Na tela de &#039;Dados do Contato&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia:dados-do-contato",
        	eventAction: "select:opcao",
        	eventLabel: "salvar-favorecido-na-lista-de-contato"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Qual o valor quer transferir para&#039;


```javascript
    Analytics.logScreenView("/transferencia/valor-transferencia/");
```


<br />

- **Quando:** No clique do botão &#039;Continuar&#039;
- **Onde:** Na tela de &#039;Qual o valor quer transferir para &#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia:valor-transferencia",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

- **Quando:** Na interação com um dos botões do modal &#039;Este valor não esta disponível na sua conta hoje&#039;
- **Onde:** Após o clique no botão conlcuir na tela de &#039;Qual o valor quer transferir para &#039; em caso de saldo insuficiente

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia",
        	eventAction: "modal:clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;sim&#039; ou &#039;nao-ir-para-o-inicio&#039; | Deve retornar o nome do botão selecionado. |

<br />

- **Onde:** Visualização da tela &#039;Para quando é a sua transferência?&#039;


```javascript
    Analytics.logScreenView("/transferencia/para-quando-e-a-sua-transferencia/");
```


<br />

- **Quando:** No clique do link &#039;Agendar outra data&#039;
- **Onde:** Na tela &#039;Para quando é a sua transferência?&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia:para-quando-e-a-sua-transferencia",
        	eventAction: "clique:link",
        	eventLabel: "agendar-outra-data"
        });
```


<br />

- **Quando:** Na interação para ativar o checkbox &#039;Repetir esta transferência&#039;
- **Onde:** Na tela &#039;Para quando é a sua transferência?&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia:para-quando-e-a-sua-transferencia",
        	eventAction: "select:checkbox",
        	eventLabel: "repetir-essa-transferencia"
        });
```


<br />

- **Quando:** Após a seleção de uma quantidade de meses em &#039;Por quanto tempo?&#039;
- **Onde:** Na tela &#039;Para quando é a sua transferência?&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia:para-quando-e-a-sua-transferencia",
        	eventAction: "select:opcao",
        	eventLabel: "por-quanto-tempo:[[opcao-selecionada]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] |  | Deve retornar a opção selecionada pelo usuário. Ex.: &#039;1-mes&#039;, &#039;2-meses&#039;, &#039;3-meses&#039;, etc |

<br />

- **Quando:** No clique do botão continuar
- **Onde:** Na tela &#039;Para quando é a sua transferência?&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia:para-quando-e-a-sua-transferencia",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });
```


<br />

- **Onde:** Visualização da tela &#039;Confirme os dados da transferência&#039;


```javascript
    Analytics.logScreenView("/transferencia/confirme-os-dados-da-transferencia/");
```


<br />

- **Quando:** No clique do botão &#039;Confirmar Transferência&#039;
- **Onde:** Na tela &#039;Confirme os dados da transferência&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia:confirme-os-dados-da-transferencia",
        	eventAction: "clique:botao",
        	eventLabel: "confirmar-transferencia"
        });
```


<br />

- **Quando:** No callback de sucesso ou erro da transferência
- **Onde:** Após o clique em Confirmar Transferência na tela &#039;Confirme os dados da transferência&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia:confirme-os-dados-da-transferencia",
        	eventAction: "callback:cadastro-completo",
        	eventLabel: "[[status]]:[[data-transferencia]]:[[banco]]",
        	userID: "[[userID]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[status]] | &#039;sucesso&#039; ou &#039;erro:senha-digitada-diferente-da-anterior&#039; etc. | Deve retornar a mensagem de sucesso ou erro apresentada para o usuário. |
| [[data-transferencia]] |  | Deve retornar a data selecionada pelo usuário para efetuar a transferência. Ex.: &#039;10.04.2019&#039;, &#039;05.05.2020&#039;, etc |
| [[banco]] |  | Deve informar o banco que vai receber a transferência. Ex.: &#039;midway&#039;, &#039;itau-unibanco&#039;, &#039;banco-do-brasil&#039;, etc |
| [[userID]] | &quot;01234&quot; | ID único de usuário definido após o cadastro (ID próprio da Midway). |

<br />

- **Onde:** Visualização da tela &#039;Transferencia feita para&#039;


```javascript
    Analytics.logScreenView("/transferencia/transferencia-concluida/");
```


<br />

- **Quando:** No clique do link &#039;Compartilhar comprovante&#039;
- **Onde:** Na tela de &#039;Transferencia feita para &#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia-concluida",
        	eventAction: "clique:link",
        	eventLabel: "compartilhar-comprovante"
        });
```


<br />

- **Quando:** No clique do botão &#039;Concluir&#039;
- **Onde:** Na tela de &#039;Transferencia feita para &#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:transferencia-concluida",
        	eventAction: "clique:botao",
        	eventLabel: "conclluir"
        });
```


<br />

### Outros Serviços

- **Onde:** Na visuliazação da tela de &#039;Outros serviços&#039;, após clicar no botão do menu inferior


```javascript
    Analytics.logScreenView("/home/outros-servicos/");
```


<br />

- **Quando:** No clique das opções
- **Onde:** Na tela de &#039;Outros serviços&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:outros-servicos",
        	eventAction: "clique:opcao",
        	eventLabel: "[[opcao-selecionada]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-selecionada]] | &#039;conta-premiada&#039;, &#039;emprestimos&#039;, &#039;assistencias&#039;, &#039;seguros&#039;, &#039;voltar&#039; e etc | Deve retornar o nome da opção selecionada. |

<br />

- **Onde:** Visualização da tela &#039;Conta Premiada&#039;


```javascript
    Analytics.logScreenView("/home/outros-servicos/conta-premiada/");
```


<br />

- **Quando:** No clique dos botões ou links
- **Onde:** Na tela de &#039;Conta Premiada&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-premiada",
        	eventAction: "clique:[[botao-ou-link]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;veja-aqui-todos-ultimos-sorteios&#039;, &#039;saiba-mais&#039;, &#039;voltar&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Quando:** No clique dos links
- **Onde:** Na tela de &#039;Conta Premiada&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:conta-premiada",
        	eventAction: "clique:link",
        	eventLabel: "[[nome-link]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-link]] | &#039;veja-aqui-todos-ultimos-sorteios&#039;, &#039;saiba-mais&#039; e etc | Deve retornar o nome do link clicado. |

<br />

### MarketPlace Financeiro - Geral

- **Onde:** Na visuliazação da tela de 'Soluções para você' dentro de Outros serviços', após clicar no botão do menu inferior


```javascript
    Analytics.logScreenView("/home/mktplace-financeiro/");
```


<br />

- **Onde:** Na visuliazação da tela de produtos dentro de 'Outros serviços', após clicar no botão do menu inferior


```javascript
    Analytics.logScreenView("/home/mktplace-financeiro/[[nome-produto]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;gift-card&#039;, &#039;emprestimo-pessoal&#039; e etc. | Deve retornar o nome do produto. |

<br />

- **Quando:** No clique dos botoes ou cards
- **Onde:** Na tela de 'Soluções para você';

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro",
        	eventAction: "clique:[[botao-card]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-card]] | &#039;botao&#039; ou &#039;card&#039; | Deve retornar o elemento clicado. |
| [[nome-item]] | &#039;voltar&#039;, &#039;seus-pedidos&#039;, &#039;gift-card&#039;, &#039;play-store&#039;, &#039;ifood&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Quando:** Após acessar um card específico
- **Onde:** Na tela do Seller


```javascript
    Analytics.logScreenView("/home/mktplace-financeiro/[[nome-produto]]//home/mktplace-financeiro/[[nome-produto]]/[[seller]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[seller]] | &#039;play-store&#039;, &#039;ifood&#039; e etc. | Deve retornar o gift card escolhido. |

<br />

- **Quando:** Nos cliques após acessar um card específico
- **Onde:** Na tela de 'gift cards';

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:[[card-seller]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-seller]] | &#039;play-store&#039;, &#039;ifood&#039;, &#039;creditas&#039; e etc | Deve retornar o nome do card clicado.  |
| [[nome-botao]] | &#039;voltar&#039;, &#039;valor-20&#039;, &#039;valor-10&#039;, &#039;valor-do-emprestimo-2000&#039; e etc | Deve retornar qual foi o botão clicado. |


<br />

- **Quando:** Após acessar o resumo de um card específico 
- **Onde:** Na tela de resumo de um Seller


```javascript
    Analytics.logScreenView("/home/mktplace-financeiro/[[nome-produto]]/[[seller]]/resumo/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;gift-card&#039;, &#039;emprestimo-pessoal&#039; e etc. | Deve retornar o nome do produto. |
| [[seller]] | &#039;play-store&#039;, &#039;ifood&#039; e etc. | Deve retornar o gift card escolhido. |

<br />

- **Quando:** Nos cliques após acessar o resumo de um card específico
- **Onde:** Na tela de resumo do Seller;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:[[card-seller]]:resumo",
        	eventAction: "clique:[[botao-link]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-seller]] | &#039;play-store&#039;, &#039;ifood&#039;, &#039;creditas&#039; e etc | Deve retornar o nome do card clicado.  |
| [[botao-link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar se foi clicado num botao ou link. |
| [[nome-item]] | &#039;voltar&#039;, &#039;ver-detalhes&#039;, &#039;termos-e-condicoes&#039;, &#039;proximo&#039; e etc | Deve retornar o nome do item clicado. |


<br />

- **Quando:** Após interagir com o checkbox
- **Onde:** Na tela de resumo do Seller;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:[[card-seller]]:resumo",
        	eventAction: "interacao:checkbox",
        	eventLabel: "[[ativar-desativar]]:li-aceito-termos"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-seller]] | &#039;ifood&#039;, &#039;uber&#039; e etc | Deve retornar o nome do card clicado.  |
| [[ativar-desativar]] | &#039;ativar&#039; ou &#039;desativar&#039; | Deve retornar a ação do usuário. |


<br />

- **Quando:** Após clicar em próximo e acessar a tela de pagamento de um card específico
- **Onde:** Na tela de resumo do Seller


```javascript
    Analytics.logScreenView("/home/mktplace-financeiro/[[nome-produto]]/[[seller]]/resumo/pagamento/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;gift-card&#039;, &#039;emprestimo-pessoal&#039; e etc. | Deve retornar o nome do produto. |
| [[seller]] | &#039;play-store&#039;, &#039;ifood&#039;, &#039;creditas&#039; e etc. | Deve retornar o gift card escolhido. |

<br />


- **Quando:** Nos cliques após acessar a tela de pagamento de um card específico
- **Onde:** Na tela de resumo de um Seller;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:[[card-seller]]:pagamento",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-seller]] | &#039;ifood&#039;, &#039;uber&#039;, &#039;creditas&#039; e etc | Deve retornar o nome do card clicado.  |
| [[nome-botao]] | &#039;voltar&#039;, &#039;ver-detalhes&#039; e etc | Deve retornar qual foi o botão clicado. |


<br />


- **Quando:** No callback após clicar no botão "próximo" na tela de pagamento
- **Onde:** Na tela de pagamento de um Seller;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:[[card-seller]]:pagamento",
        	eventAction: "calback",
        	eventLabel: "proximo:[[forma-pagamento]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-seller]] | &#039;ifood&#039;, &#039;uber&#039;, &#039;creditas&#039; e etc | Deve retornar o nome do card clicado.  |
| [[forma-pagamento]] | &#039;saldo&#039;, &#039;cartao-rchlo&#039; e etc | Deve retornar qual foi a forma de pagamento escolhida. |


<br />

- **Quando:** Ao acessar a tela de Seller solicitado com sucesso
- **Onde:** Na tela de sucesso de pedido de Seller


```javascript
    Analytics.logScreenView("/home/mktplace-financeiro/[[nome-produto]]/[[seller]]/sucesso/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;gift-card&#039;, &#039;emprestimo-pessoal&#039; e etc. | Deve retornar o nome do produto. |
| [[seller]] | &#039;play-store&#039;, &#039;ifood&#039;, &#039;creditas&#039; e etc. | Deve retornar o gift card escolhido. |

<br />

- **Quando:** Nos cliques nos botões
- **Onde:** Na tela de sucesso de pedido de Seller;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:[[card-seller]]:sucesso",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-seller]] | &#039;ifood&#039;, &#039;uber&#039;, &#039;creditas&#039; e etc | Deve retornar o nome do card clicado.  |
| [[nome-botao]] | &#039;voltar&#039;, &#039;ver-pedido&#039; e etc | Deve retornar qual foi o botão clicado. |


<br />

- **Quando:** Ao acessar a tela de 'seus pedidos' 
- **Onde:** Na tela de pedidos de marketplace financeiro


```javascript
    Analytics.logScreenView("/home/mktplace-financeiro/seus-pedidos/");
```

<br />


- **Quando:** Nos cliques após acessar a tela de 'seus pedidos'
- **Onde:** Na tela de Seus Pedidos;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:seus-pedidos",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-botao]] | &#039;voltar&#039;, &#039;uber&#039;, &#039;emprestimo-pessoal&#039; e etc | Deve retornar qual foi o botão clicado. |


<br />

- **Quando:** Ao acessar a tela de 'seus pedidos' dentro do seller selecionado
- **Onde:** Na tela de pedido selecionado


```javascript
    Analytics.logScreenView("/home/mktplace-financeiro/seus-pedidos/[[nome-produto]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;gift-card&#039;, &#039;emprestimo-pessoal&#039; e etc. | Deve retornar o nome do produto. |

<br />

- **Quando:** Nos cliques após acessar a tela de pedidos dentro do seller selecionado
- **Onde:** Na tela de pedido selecionado

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:pedidos-[[card-seller]]",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-seller]] | &#039;ifood&#039;, &#039;uber&#039;, &#039;creditas&#039; e etc | Deve retornar o nome do card clicado.  |
| [[nome-botao]] | &#039;voltar&#039;, &#039;10.00:15/11/2021&#039;, &#039;2000&#039; e etc | Deve retornar qual foi o botão clicado. |


<br />

- **Quando:** Ao acessar a tela de detalhes em 'seus pedidos'
- **Onde:** Na tela de detalhes do pedido


```javascript
    Analytics.logScreenView("/home/mktplace-financeiro/[[nome-produto]]/[[seller]]/detalhes-pedido/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;gift-card&#039;, &#039;emprestimo-pessoal&#039; e etc. | Deve retornar o nome do produto. |
| [[seller]] | &#039;play-store&#039;, &#039;ifood&#039;, &#039;creditas&#039; e etc. | Deve retornar o seller escolhido.  |


<br />

- **Quando:** No carregamento da tela de detalhes em 'seus pedidos'
- **Onde:** Na tela de detalhes do pedido

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:pedidos-[[card-seller]]-detalhes",
        	eventAction: "callback:etapa",
        	eventLabel: "[[status-etapa]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-seller]] | &#039;ifood&#039;, &#039;uber&#039;, &#039;creditas&#039; e etc | Deve retornar o nome do card clicado.  |
| [[status-etapa]] | &#039;pedido-feito&#039;, &#039;pagamento&#039;, &#039;confirmacao-com-parceiro&#039;, &#039;finalizando&#039;, &#039;pedido-cancelado&#039; e etc | Deve retornar em qual etapa do status do pedido. |


<br />

- **Quando:** Nos cliques nos botões após acessar a tela de detalhes em 'seus pedidos'
- **Onde:** Na tela de detalhes do pedido

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:pedidos-[[card-seller]]-detalhes",
        	eventAction: "clique:[[botao-ou-link]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-seller]] | &#039;ifood&#039;, &#039;uber&#039;, &#039;creditas&#039; e etc | Deve retornar o nome do card clicado.  |
| [[botao-ou-link]] | &#039;botao&#039; ou &#039;link&#039; | Deve retornar se foi clicado num botao ou link.  |
| [[nome-item]] | &#039;voltar&#039;, &#039;como-ativar&#039;, &#039;termos-e-condicoes&#039; e &#039;abrir-reclamacao&#039; | Deve retornar o nome do item clicado.  |


<br />


- **Quando:** No calback de insucesso para possíveis motivos de não finalizar o pedido. tela de detalhes em 'seus pedidos'
- **Onde:** Na tela de detalhes do pedido

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:pedidos-[[card-seller]]-detalhes",
        	eventAction: "callback:erro",
        	eventLabel: "[[status-mensagem]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card]] | &#039;ifood&#039;, &#039;uber&#039; e etc | Deve retornar o nome do card clicado.  |
| [[status-mensagem]] | &#039;nao-foi-possivel-etc&#039;, &#039;nao-foi-possivel-confirmar-pagamento&#039; e etc | Deve retornar qual status de insucesso. |



<br />

- **Onde:** Na visualização do modal de "Termos e Condições" de marketplace financeiro


```javascript
    Analytics.logScreenView("/home/mktplace-financeiro/[[nome-produto]]/[[seller]]/modal-termos-condicoes/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-produto]] | &#039;gift-card&#039;, &#039;emprestimo-pessoal&#039; e etc. | Deve retornar o nome do produto. |
| [[seller]] | &#039;play-store&#039;, &#039;ifood&#039;, &#039;creditas&#039; e etc. | Deve retornar o seller escolhido.  |


<br />

- **Quando:** No clique do botão
- **Onde:** No modal de "Termos e condições" de marketplace financeiro

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:mktplace-financeiro:[[card-seller]]:modal-termos-condicoes",
        	eventAction: "clique:botao",
        	eventLabel: "fechar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[card-seller]] | &#039;ifood&#039;, &#039;uber&#039;, &#039;creditas&#039; e etc | Deve retornar o nome do card clicado.  |



<br />

### Empréstimo Contratação

- **Onde:** Visualização da tela de &#039;Emprestimo&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Empréstimo&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[opcao-icone]]",
        	eventLabel: "[[nome-item]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-icone]] | &#039;opcao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;emprestimo-pessoal&#039;, &#039;preventivo&#039;, &#039;voltar&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela de &#039;Emprestimo Pessoal&#039; ou &#039;Preventivo&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/[[tela]]/");
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[tela]] | &#039;emprestimo-pessoal&#039;, &#039;preventivo&#039; e etc. | Deve retornar nome da tela. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Emprestimo Pessoal&#039; ou &#039;Preventivo&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[opcao-icone]]:[[tela]]",
        	eventLabel: "[[nome-item]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-icone]] | &#039;opcao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[tela]] | &#039;emprestimo-pessoal&#039;, &#039;preventivo&#039; e etc. | Deve retornar nome da tela. |
| [[nome-item]] | &#039;meus-contratos&#039;, &#039;solicitar-emprestimo-pessoal&#039;, &#039;voltar&#039;, &#039;cancelamento&#039; e etc | Deve retornar o nome do item clicado. |

<br />


- **Onde:** Visualização da tela de  &#039;Quanto você precisa &#039;.

```javascript
    Analytics.logScreenView("/emprestimos/quanto-voce-precisa/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Quanto você precisa&#039;.


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[opcao-icone]]:quanto-voce-precisa",
        	eventLabel: "[[nome-item]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[opcao-icone]] | &#039;opcao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;voltar&#039;, &#039;continuar&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Quando:** Após interagir com o campo e perder o foco(onChange)
- **Onde:** Na tela de &#039;Quanto você precisa&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "interacao:campo:quanto-voce-precisa",
        	eventLabel: "valor"
        });		
```

<br />

- **Onde:** Visualização da tela de &#039;Sem limite para empréstimo pessoal&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/sem-limite-emprestimo-pessoal/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Sem limite para empréstimo pessoal&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:sem-limite-emprestimo-pessoal",
        	eventLabel: eventLabel,"[[nome-item]]"
        });		
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;fechar&#039;, &#039;continuar&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela de &#039;Empréstimo pessoal valores&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/emprestimo-pessoal-valores/");
```

<br />

- **Quando:** No clique dos elementos **OBS: Retornar as dimensões somente após clicar no botão &quot;Continuar&quot;**
- **Onde:** Na tela de &#039;Empréstimo pessoal valores&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:emprestimo-pessoal-valores",
        	eventLabel: "[[nome-item]]",
		valorSolicitado:"[[valorSolicitado]]",
		valorTotalFinanciado:"[[valorTotalFinanciado]]",
		qtdeParcelas:"[[qtdeParcelas]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;voltar&#039;, &#039;continuar&#039; e etc | Deve retornar o nome do item clicado. |
| [[valorSolicitado]] |  &#039;1000&#039;, &#039;2000&#039; e etc | Deve retornar o valor solicitado para empréstimo pessoal |
| [[valorTotalFinanciado]] |  &#039;1305.00&#039; e etc | Deve retornar o valor total do financiamento |
| [[qtdeParcelas]] |  &#039;10x-130,50&#039; e etc | Deve retornar a quantidade de parcelas |


<br />

- **Onde:** Visualização da tela de &#039;Simulação Pronta&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/simulacao-pronta/");
```

<br />

- **Quando:** No clique dos elementos **OBS: Retornar as dimensões somente após clicar no botão &quot;Contratar&quot;**
- **Onde:** Na tela de &#039;Simulação Pronta&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-link-icone]]:simulacao-pronta",
        	eventLabel: "[[nome-item]]",
			valorSolicitado:"[[valorSolicitado]]",
			valorTotalFinanciado:"[[valorTotalFinanciado]]",
			qtdeParcelas:"[[qtdeParcelas]]"
        });		
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-link-icone]] | &#039;botao&#039;, &#039;icone&#039; ou &#039;link&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;voltar&#039;, &#039;fechar&#039;, &#039;contratar&#039;, &#039;detalhamento-cet&#039; e etc | Deve retornar o nome do item clicado. |
| [[valorSolicitado]] |  &#039;1000&#039;, &#039;2000&#039; e etc | Deve retornar o valor solicitado para empréstimo pessoal |
| [[valorTotalFinanciado]] |  &#039;1305.00&#039; e etc | Deve retornar o valor total do financiamento |
| [[qtdeParcelas]] |  &#039;'10x-130,50&#039; e etc | Deve retornar a quantidade de parcelas |

<br />

- **Onde:** Visualização da tela de &#039;Detalhamento do CET&#039;. 

```javascript
    Analytics.logScreenView("/emprestimos/detalhamento-cet/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Detalhamento do CET&#039;.


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:detalhamento-cet",
        	eventLabel: "[[nome-item]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;fechar&#039;, &#039;entendi&#039; e etc | Deve retornar o nome do item clicado. |

<br />


- **Onde:** Visualização da tela de &#039;Termos e condições gerais&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/termos-condicoes-gerais/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Termos e condições gerais&#039;.


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:termos-condicoes-gerais",
        	eventLabel: "[[nome-item]]"
        });		
```



| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;voltar&#039;, &#039;li-concordo&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela de &#039;Contratação realizada com sucesso&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/contratacao-realizada-sucesso/");
```

<br />

- **Quando:** No clique dos elementos. **OBS: Retornar as dimensões somente após clicar no ícone de &quot;Fechar&quot; ou botão &quot;Concluir&quot;**
- **Onde:** Na tela de &#039;Contratação realizada com sucesso&#039;.


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo:sucesso",
        	eventAction: "clique:[[botao-link-icone]]:contratacao-realizada",
        	eventLabel: "[[nome-item]]",
			valorSolicitado:"[[valorSolicitado]]",
			valorTotalFinanciado:"[[valorTotalFinanciado]]",
			qtdeParcelas:"[[qtdeParcelas]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-link-icone]] | &#039;botao&#039; , &#039;link&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;fechar&#039;, &#039;compartilhar-contrato&#039;, &#039;concluir&#039; e etc | Deve retornar o nome do item clicado. |
| [[valorSolicitado]] |  &#039;1000&#039;, &#039;2000&#039; e etc | Deve retornar o valor solicitado para empréstimo pessoal |
| [[valorTotalFinanciado]] |  &#039;1305.00&#039; e etc | Deve retornar o valor total do financiamento |
| [[qtdeParcelas]] |  &#039;'10x-130,50&#039; e etc | Deve retornar a quantidade de parcelas |

<br />

### Empréstimo Consulta

- **Onde:** Visualização da tela de &#039;meus contratos&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/meus-contratos/");
```

<br />

- **Quando:** No carregamento das abas
- **Onde:** Na tela de &#039;meus contratos&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "carregamento:aba:meus-contratos",
        	eventLabel: "[[aba]]:[[qtd-contratos]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[aba]] | &#039;em-aberto&#039; ou &#039;pagos&#039; | Deve retornar o nome da aba carregada. |
| [[qtd-contratos]] | &#039;2-contratos&#039;, &#039;nenhum-contrato&#039; e etc | Deve retornar a quantidade de contratos. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;meus contratos&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:meus-contratos",
        	eventLabel: "[[aba]]:[[nome-item]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[aba]] | &#039;em-aberto&#039; ou &#039;pagos&#039; | Deve retornar o nome da aba clicada. |
| [[nome-item]] | &#039;voltar&#039;, &#039;ver-detalhes-contrato-1&#039;, &#039;ver-detalhes-contrato-2&#039; e etc | Deve retornar o nome do item clicado. |

<br />


- **Onde:** Visualização da tela de &#039;Detalhes do contrato&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/detalhes-contrato/");
	Analytics.logEvent("event",{"numeroContrato": "[[numeroContrato]]","statusContrato": "[[statusContrato]]"});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[numeroContrato]] |  &#039;124327632&#039; e etc | Deve retornar o número do contrato |
| [[statusContrato]] |  &#039;ativo&#039;,&#039;cancelado&#039;,&#039;liquidado&#039; e etc | Deve retornar o status do contrato |


<br />

- **Quando:** No clique dos elementos. **OBS: Retornar a dimensão somente após clicar nos botões de &quot;Amortizar&quot;, &quot;Liquidar&quot; e &quot;Cancelar contrato&quot;**
- **Onde:** Na tela de &#039;Detalhes do contrato&#039;


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:detalhes-contrato",
        	eventLabel: "[[nome-item]]",
			numeroContrato:"[[numeroContrato]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;voltar&#039;, &#039;liquidar-parcelas&#039;, &#039;amortizar&#039;, &#039;cancelar-contrato&#039; e etc | Deve retornar o nome do item clicado. |
| [[numeroContrato]] |  &#039;124327632&#039; e etc | Deve retornar o número do contrato |

<br />

### Empréstimo Cancelamento

- **Onde:** Visualização do modal de &#039;Cancelar contrato&#039;

```javascript
    Analytics.logScreenView("/emprestimos/modal-cancelar-contrato/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** No modal de &#039;Cancelar contrato&#039;


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:botao:modal-cancelar-contrato",
        	eventLabel: "[[nome-item]]"
        });		
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-item]] | &#039;quero-cancelar&#039;, &#039;agora-nao&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela de &#039;Termos e Condições de cancelamento&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/termos-condicoes-cancelamento/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Termos e condições de cancelamento&#039;


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:termos-condicoes-cancelamento",
        	eventLabel: "[[nome-item]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;voltar&#039;, &#039;li-concordo&#039; e etc | Deve retornar o nome do item clicado. |

<br />


- **Onde:**  Visualização da tela de &#039;Empréstimo pessoal cancelado&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/emprestimo-pessoal-cancelado/");
	Analytics.logEvent("event",{"numeroContrato": "[[numeroContrato]]"});
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| numeroContrato |  &#039;124327632&#039; e etc | Deve retornar o número do contrato |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Empréstimo pessoal cancelado&#039;


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo-cancelado:sucesso",
        	eventAction: "clique:botao",
        	eventLabel: "continuar"
        });		
```

<br />

### Empréstimo Liquidação

- **Onde:** Onde: Visualização da tela de &#039;Liquidação de parcela&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/liquidacao-parcela/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Liquidação de parcela&#039;


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:liquidacao-parcela",
        	eventLabel: "[[nome-item]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;voltar&#039;, &#039;liquidar-parcelas:2:valor-261.00&#039; e etc | Deve retornar o nome do item clicado. |

<br />


- **Onde:** Visualização da tela de &#039;Forma de pagamento&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/forma-pagamento/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Forma de pagamento&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:forma-pagamento",
        	eventLabel: "[[nome-item]]"
        });		
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;voltar&#039;, &#039;boleto-bancario&#039;, &#039;debito-conta&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela de &#039;Débito em conta&#039;

```javascript
    Analytics.logScreenView("/emprestimos/debito-conta/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Débito em conta&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:debito-conta",
        	eventLabel: "[[nome-item]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;voltar&#039;, &#039;confirmar-pagamento&#039; e etc | Deve retornar o nome do item clicado. |

<br />


- **Onde:** Visualização da tela de &#039;Pagamento realizado&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/pagamento-realizado/");
	Analytics.logEvent("event",{"numeroContrato": "[[numeroContrato]]"});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| numeroContrato |  &#039;124327632&#039; e etc | Deve retornar o número do contrato |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Pagamento realizado&#039;


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:pagamento-realizado",
        	eventLabel: "[[nome-item]]"
        });		
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;fechar&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela de &#039;Pagamento por boleto&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/pagamento-boleto/");
```

<br />

- **Quando:** No clique dos elementos.
- **Onde:** Na tela de &#039;Pagamento por boleto&#039;


```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:pagamento-boleto",
        	eventLabel: "[[nome-item]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;voltar&#039;, &#039;gerar-boleto&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela de &#039;Boleto gerado&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/boleto-gerado/");
	Analytics.logEvent("event",{"numeroContrato": "[[numeroContrato]]","dataVencimento": "[[dataVencimento]]"});
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[numeroContrato]] |  &#039;124327632&#039; e etc | Deve retornar o número do contrato |
| [[dataVencimento]] |  &#039;15/11&#039; e etc | Deve retornar a data de vencimento selecionada para o boleto |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Boleto gerado&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:boleto-gerado",
        	eventLabel: "[[nome-item]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;fechar&#039;, &#039;visualizar-boleto&#039;, &#039;copiar-codigo&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização do modal de &#039;Código de barras copiado&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/modal-codigo-barras-copiado/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** No modal de &#039;Código de barras copiado&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:[[botao-icone]]:modal-codigo-barras-copiado",
        	eventLabel: "[[nome-item]]"
        });		
```


| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-icone]] | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| [[nome-item]] | &#039;fechar&#039;, &#039;ok&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:**Visualização da tela &#039;Visualização Boleto&#039;.

```javascript
    Analytics.logScreenView("/emprestimos/visualizacao-boleto/");
```

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Visualização Boleto&#039;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:emprestimo",
        	eventAction: "clique:icone",
        	eventLabel: "voltar"
        });		
```

<br />


### Seguros e Assistências

- **Onde:** Visualização da tela "Seguros e Assistências"

```javascript
    Analytics.logScreenView("/seguros-assistencias/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &#039;Seguros e Assistências&#039;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:seguros-assistencias",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;continuar&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização das telas de "Produtos Contratados" (contratados ou cancelados)

```javascript
    Analytics.logScreenView("/seguros-assistencias/produtos-contratados-aba-[[contratados-cancelados]]-qtd-[[qtd-itens]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[contratados-cancelados]]` | &#039;contratados&#039;, &#039;cancelados&#039; e etc | Deve retornar a aba carregada. |
| `[[qtd-itens]]` | &#039;2&#039;, &#039;0&#039;, &#039;5&#039; e etc | Deve retornar a quantidade de itens. |

<br />

- **Quando:** Após interagir com as abas (também disparar quando a tela carregar já com a aba por padrão selecionada)
- **Onde:** Nas telas de &quot;Produtos Contratados&quot; (contratados ou cancelados)

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:produtos-contratados",
        	eventAction: "clique:aba",
        	eventLabel: "[[aba]]:[[qtd-itens]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[aba]]` | &#039;contratados&#039;, &#039;cancelados&#039; e etc | Deve retornar a aba carregada. |
| `[[qtd-itens]]` | &#039;2&#039;, &#039;0&#039;, &#039;5&#039; e etc | Deve retornar a quantidade de itens. |

<br />

- **Quando:** No clique do ícone de voltar
- **Onde:** Nas telas de &quot;Produtos Contratados&quot; (contratados ou cancelados)

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:produtos-contratados",
        	eventAction: "clique:icone",
        	eventLabel: "voltar"
        });
```


<br />

### Segunda via comprovante

- **Onde:** Visualização da tela "Comprovantes e agendamentos"

```javascript
    Analytics.logScreenView("/seguros-assistencias/comprovantes-agendamentos/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;Comprovantes e agendamentos&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:comprovantes-agendamentos",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;cancelar-recorrencia&#039;, &#039;segunda-via-comprovante&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela "Comprovantes"

```javascript
    Analytics.logScreenView("/seguros-assistencias/comprovantes/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;Comprovantes&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:comprovantes",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
|`[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]`| &#039;voltar&#039;, &#039;nova-transferencia&#039;, &#039;recorrente&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela "Transferência"

```javascript
    Analytics.logScreenView("/seguros-assistencias/transferencia/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;Transferência&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:transferencia",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;fechar&#039;, &#039;concluir&#039;, &#039;ver-comprovante&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela "Comprovante de transferência"

```javascript
    Analytics.logScreenView("/seguros-assistencias/comprovante-transferencia/");
```


<br />

- **Quando:** No clique dos botões
- **Onde:** Na tela de &quot;Comprovante de transferência&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:transferencia",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-item]]` | &#039;compartilhar-comprovante&#039;, &#039;ir-para-home&#039; e etc | Deve retornar o nome do item clicado. |

<br />

### Recarga Celular

- **Onde:** Visualização da tela "Recarga de Celular"

```javascript
    Analytics.logScreenView("/recarga-celular/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;Recarga de Celular&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:recarga-celular",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;continuar:tim:20,00&#039;, &#039;continuar:vivo:25,00&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização do modal de "Selecione o valor"

```javascript
    Analytics.logScreenView("/recarga-celular/modal-selecione-valor/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** No modal de &quot;Selecione o valor&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:modal-selecione-valor",
        	eventAction: "clique:[[opcao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[opcao-icone]]` | &#039;opcao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;fechar&#039;, &#039;R$10,00&#039;, &#039;R$25,00&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela "Confirmação de dados de recarga"

```javascript
    Analytics.logScreenView("/recarga-celular/confirmacao-dados-recarga/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;Confirmação de dados de recarga&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:confirmacao-dados-recarga",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;pagar:saldo-conta&#039;, &#039;pagar:cartao&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização do callback do modal "Saldo insuficiente"

```javascript
    Analytics.logScreenView("/recarga-celular/modal-saldo-insuficiente/");
```


<br />

- **Quando:** No clique do botão
- **Onde:** No modal &quot;Saldo insuficiente&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:modal-saldo-insuficiente",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```


<br />

- **Onde:** Visualização das telas de callback de erros "Algo deu errado", "Cartão sem limite" e etc

```javascript
    Analytics.logScreenView("/recarga-celular/[[tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[tela]]` | &#039;algo-deu-errado&#039;, &#039;cartao-sem-limite&#039; e etc | Deve retornar o nome da tela. |

<br />

- **Quando:** No clique dos botões
- **Onde:** Nas telas de callback de erros &quot;Algo deu errado&quot;, &quot;Cartão sem limite&quot; e etc

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:[[tela]]",
        	eventAction: "clique:botao",
        	eventLabel: "voltar"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[tela]]` | &#039;algo-deu-errado&#039;, &#039;cartao-sem-limite&#039; e etc | Deve retornar o nome da tela. |

<br />

- **Onde:** Visualização da tela de "Recarga realizada com sucesso"

```javascript
    Analytics.logScreenView("/recarga-celular/realizada-sucesso:op-[[operadora]]:val-[[valor]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[operadora]]` | &#039;tim&#039;, &#039;claro&#039; e etc | Deve retornar a operadora que foi a recarga. |
| `[[valor]]` | &#039;R$15,00&#039;, &#039;R$20,00&#039; e etc | Deve retornar o valor da recarga. |

<br />

- **Quando:** No clique dos botões
- **Onde:** Na tela de &quot;Recarga realizada com sucesso&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:recarga-realizada-sucesso",
        	eventAction: "clique:botao",
        	eventLabel: "[[nome-botao]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-botao]]` | &#039;compartilhar&#039;, &#039;ver-comprovante&#039; e etc | Deve retornar o nome do botão. |

<br /> 

- **Onde:** Visualização da tela de "comprovante de recarga"

```javascript
    Analytics.logScreenView("/recarga-celular/comprovante-recarga:op-[[operadora]]:val-[[valor]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[operadora]]` | &#039;tim&#039;, &#039;claro&#039; e etc | Deve retornar a operadora que foi a recarga. |
| `[[valor]]` | &#039;R$15,00&#039;, &#039;R$20,00&#039; e etc | Deve retornar o valor da recarga. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;comprovante de recarga&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:comprovante-recarga",
        	eventAction: "clique:[[botao-icone]]",
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;fechar&#039;, &#039;compartilhar&#039; e etc | Deve retornar o nome do item clicado. |

<br />

### Home Pix

- **Onde:** Visualização das telas de "Bem vindo Pix"

```javascript
    Analytics.logScreenView("/bem-vindo-pix-[[numero-tela]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[numero-tela]]` | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o número da tela. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nas telas de &quot;Bem vindo Pix&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:bem-vindo-pix-[[numero-tela]]"
        	eventAction: "clique:[[botao-icone]]"
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[numero-tela]]` | &#039;1&#039;, &#039;2&#039;, &#039;3&#039; e etc | Deve retornar o número da tela. |
| `[[botao-icone]]` | &#039;botao&#039; ou &#039;icone&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;fechar&#039;, &#039;proximo&#039;, &#039;ok&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização da tela de "Home Pix"

```javascript
    Analytics.logScreenView("/home-pix-[[tem|nao-chave-registrada]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[tem\|nao-chave-registrada]]` | &#039;nao-tem-chave-registrada&#039; ou &#039;tem-chave-registrada&#039; | Deve retornar se o usuário possui ou não chave registrada. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;Home Pix&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home-pix"
        	eventAction: "clique:[[botao-icone-link-card-carrossel]]"
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-icone-link-card-carrossel]]` | &#039;botao&#039;, &#039;icone&#039;, &#039;link&#039; ou &#039;card-carrossel&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;todas-funcionalidades-pix&#039;, &#039;cadastrar-chave&#039;, &#039;ver-extrato&#039;, &#039;transferencia-pix&#039;, &#039;leitor-qrcode&#039;, &#039;copia-cola&#039;, &#039;minhas-chaves&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;Home Pix&quot;, na seção de Extrato

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:home-pix"
        	eventAction: "clique:botao:extrato"
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-item]]` | &#039;cancelar&#039;, &#039;devolver&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização do modal de "Sobre o Pix"

```javascript
    Analytics.logScreenView("/modal-sobre-pix/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** No modal de &quot;Sobre o Pix&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:modal-sobre-pix"
        	eventAction: "clique:[[botao-link]]"
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-link]]` | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;central-atendimento&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Onde:** Visualização do modal de "Notificação de Bloqueio"

```javascript
    Analytics.logScreenView("/modal-notificacao-bloqueio/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** No modal de &quot;Notificação de Bloqueio&quot;
```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:modal-notificacao-bloqueio"
        	eventAction: "clique:[[botao-link]]"
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-link]]` | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;central-atendimento&#039; e etc | Deve retornar o nome do item clicado. |

<br />

### Cadastrar Pix

- **Onde:** Visualização da tela de "Cadastrar chave Pix"

```javascript
    Analytics.logScreenView("/cadastrar-chave-pix/");
```


<br />

- **Quando:** No clique dos elementos. OBS: Retornar o extraParam somente quando for clicado em &quot;Cadastrar Chave&quot;
- **Onde:** Na tela de &quot;Cadastrar chave Pix&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastrar-chave-pix"
        	eventAction: "clique:[[botao-link]]"
        	eventLabel: "[[nome-item]]"
        	TipoChavePix: "[[TipoChavePix]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[botao-link]]` | &#039;botao&#039; ou &#039;link&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;mais-chave&#039;, &#039;cadastrar-chave&#039; e etc | Deve retornar o nome do item clicado. |
| `[[TipoChavePix]]` |  &#039;cpf&#039;, &#039;email&#039;, &#039;celular&#039;, &#039;cpf/email&#039; e etc | Deve retornar o(s) tipo(s) de pix selecionado(s) |

<br />

- **Quando:** No callback de erro de preenchimento dos campos
- **Onde:** Na tela &#039;Cadastrar chave Pix&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:cadastrar-chave-pix"
        	eventAction: "interacao:campo:callback"
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[status]]` | &#039;erro:campo-obrigatorio&#039;, &#039;erro:formato-email-invalido&#039; e etc | Deve retornar o status do callback. |

<br />

- **Onde:** Visualização dos modais de "Chave existente" ou "Limite 5 chaves conta"

```javascript
    Analytics.logScreenView("/modal-[[nome-modal]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-modal]]` | &#039;chave-existente&#039;, &#039;limite-5-chaves-conta&#039; e etc | Deve retornar o nome do modal. |

<br />

- **Onde:** Visualização da tela de "Código de verificação"

```javascript
    Analytics.logScreenView("/cadastrar-chave-pix/codigo-verificacao/");
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela &#039;Código de verificação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:codigo-verificacao"
        	eventAction: "clique:[[icone-link]]"
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[icone-link]]` | &#039;icone&#039; ou &#039;link&#039; | Deve retornar o nome da opção clicada. |
| `[[nome-item]]` | &#039;voltar&#039;, &#039;reenviar&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Quando:** No callback de erro de preenchimento dos campos
- **Onde:** Na tela &#039;Código de verificação&#039;.

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:codigo-verificacao"
        	eventAction: "interacao:campo:callback"
        	eventLabel: "[[status]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[status]]` | &#039;erro:codigo-incorreto&#039; e etc | Deve retornar o status do callback. |

<br />

- **Onde:** Visualização dos modais de "Solicitação chave pendente", "Portabilidade de chave", "reivindicação de chave", "Chave com solicitação em aberto"

```javascript
    Analytics.logScreenView("/modal-[[nome-modal]]/");
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-modal]]` | &#039;solicitacao-chave-pendente&#039;, &#039;portabilidade-chave&#039;, reivindicaoca-chave&#039;, &#039;chave-solicitacao-aberto&#039; e etc | Deve retornar o nome do modal. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nos modais de &quot;Solicitação chave pendente&quot;, &quot;Portabilidade de chave&quot;, &quot;reivindicação de chave&quot;, &quot;Chave com solicitação em aberto&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:modal-[[nome-modal]]"
        	eventAction: "clique:botao"
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-modal]]` | &#039;solicitacao-chave-pendente&#039;, &#039;portabilidade-chave&#039;, reivindicaoca-chave&#039;, &#039;chave-solicitacao-aberto&#039; e etc | Deve retornar o nome do modal. |
| `[[nome-item]]` | &#039;continuar&#039;, &#039;voltar&#039; e etc | Deve retornar o nome do item clicado. |


<br />

- **Onde:** Visualização da tela de erro "Algo deu errado" para criar a chave 

```javascript
    Analytics.logScreenView("/algo-deu-errado/");
	Analytics.logEvent("event",{"TipoChavePix": "[[TipoChavePix]]"});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[TipoChavePix]]` |  &#039;cpf&#039;, &#039;email&#039;, &#039;celular&#039;, &#039;cpf/email&#039; e etc | Deve retornar o(s) tipo(s) de pix selecionado(s) |

<br />

- **Onde:** Visualização das telas de sucesso e em análise

```javascript
    Analytics.logScreenView("/[[nome-tela]]/");
	Analytics.logEvent("event",{"TipoChavePix": "[[TipoChavePix]]"});
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-tela]]` | &#039;chave-pix-cadastrada-sucesso&#039;, &#039;solicitacao-em-analise&#039; e etc | Deve retornar o nome da tela. |
| `[[TipoChavePix]]` |  &#039;cpf&#039;, &#039;email&#039;, &#039;celular&#039;, &#039;cpf/email&#039; e etc | Deve retornar o(s) tipo(s) de pix selecionado(s) |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nas telas de sucesso e em análise

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:[[nome-tela]]"
        	eventAction: "clique:[[botao-icone-link]]"
        	eventLabel: "[[nome-item]]"
        });
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| `[[nome-tela]]` | &#039;chave-pix-cadastrada-sucesso&#039;, &#039;solicitacao-em-analise&#039; e etc | Deve retornar o nome da tela. |
| `[[botao-icone-link]]` | &#039;botao&#039;, &#039;icone&#039; ou &#039;link&#039; | Deve retornar o elemento clicado. |
| `[[nome-item]]` | &#039;fechar&#039;, &#039;minhas-chaves&#039;, &#039;atendimento&#039; e etc | Deve retornar o nome do item clicado. |

<br />


<br />


### Minhas chaves Pix

- **Onde:** Visualização da tela de &#039;Minhas chaves&#039;

```jaavscript
    Analytics.logScreenView("/minhas-chaves/")
```


<br />

- **Quando:** No clique dos elementos
- **Onde:** Na tela de &quot;Minhas chaves&quot;

```javascript
        Analytics.logEvent("event",{
        	eventCategory: "app-midway:minhas-chaves" ,
        	eventAction: "clique:[[botao-aba-link]]" ,
        	eventLabel: "[[nome-item]]" 
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-aba-link]] | &#039;botao&#039;, &#039;aba&#039; ou &#039;link&#039; | Deve retornar o elemento clicado. |
| [[nome-item]] | &#039;voltar&#039;, &#039;cadastrar-nova-chave&#039;, &#039;todos:2-chaves&#039;, &#039;ativas:4-chaves&#039;, &#039;em-analise:5-chaves&#039; e etc | Deve retornar o nome do item clicado. |

<br />

- **Quando:** No clique para &quot;Excluir&quot; e &quot;ver mais&quot;
- **Onde:** Na tela de &quot;Minhas chaves&quot;

```javascript
        Analytics.logEvent("event", {
        	eventCategory: "app-midway:minhas-chaves" ,
        	eventAction: "clique:[[botao-link]]" ,
        	eventLabel: "[[nome-item]]",
        	TipoChavePix: "[[TipoChavePix]]" 
		})
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[botao-link]] | &#039;botao&#039;ou &#039;link&#039; | Deve retornar o elemento clicado. |
| [[nome-item]] | &#039;ver-mais&#039;, &#039;excluir&#039; e etc | Deve retornar o nome do item clicado. |
| [[TipoChavePix]] |  &#039;cpf&#039;, &#039;email&#039;, &#039;celular&#039;, &#039;cpf/email&#039;, &#039;pix-qrcode-criado:cpf/celular&#039;, &#039;pix-qrcode-criado:cpf&#039; e etc | Deve retornar o(s) tipo(s) de pix selecionado(s) |

<br />

- **Onde:** Visualização dos modais de  &#039;Solicitação portabilidade &#039; e  &#039;Solicitação reivindicação &#039;

```javascript
    Analytics.logScreenView("/minhas-chaves/modal-[[nome-modal]]/")
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[nome-modal]] | &#039;solicitacao-portabilidade&#039;, &#039;solicitacao-reivindicacao&#039; e etc | Deve retornar o nome do modal. |

<br />

- **Quando:** No clique dos elementos
- **Onde:** Nos modais de &quot;Solicitação portabilidade&quot; e &quot;Solicitação reivindicação&quot;

```javascript
        Analytics.logEvent("event", {
   