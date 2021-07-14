![Zoly](https://lucida-brasil.github.io/public/Images/zoly-mutant-logo.png)

> Área: Digital Analytics<br />
> Documento de Especificação Técnica


# Instalação do Google Tag Manager for Apps - React Native - Riachuelo APP

Última atualização: 27/03/2020 <br />
Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)

<br />

## **Objetivo**

Esse documento descreve como instalar o Google Tag Manager (GTM) em um aplicativo desenvolvido em React Native [Riachuelo APP] com o intuito de modificar um evento já mapeado e transformá-lo em um evento de conversão do Firebase Analytics.


## **Implementação**

Os links abaixo podem ajudar em caso de dúvidas na implementação do Google Tag Manager em seu aplicativo.

1. [Tag Manager + Firebase: Getting Started - Android](https://developers.google.com/tag-manager/android/v5)
2. [Tag Manager + Firebase: Getting Started - iOS](https://developers.google.com/tag-manager/ios/v5)

<br />

## Android

### **Instalando o Google Tag Manager no projeto**

*Sugestão:* Abra a pasta `android` do seu projeto no *Android Studio*.

No arquivo `app/build.gradle` inclua a seguinte dependência:

```java
dependencies {
  ...
  // inclua essa linha
  implementation 'com.google.android.gms:play-services-tagmanager:17.0.0'
  ...
}
```

**Observação:** Após a alteração dentro de _dependencies_ é necessário sincronizar os arquivos do projeto. Se estiver no *Android Studio*, clique em `Sync Now`. Se não, reinstale o app com o comando `react-native run-android`.


### **Implementação da classe chamada pelo Google Tag Manager**

No diretório `android/app/src/main/java/<nome do seu projeto>`, crie o arquivo `ConversionEventTagProvider.java` e cole o código abaixo. Troque '**XXX**' pelo nome do pacote do seu app - Ex: com.riachuelo.app.

Esse arquivo é responsável pela ponte entre o seu aplicativo e o Google Tag Manager. A função `execute` que a classe `ConversionEventTagProvider` implementa, traz, no parâmetro `map`, os parâmetros (chave - valor) enviados pela Tag do Google Tag Manager. Esses valores são tratados e enviados para o Firebase.

```java
package XXX;

import android.content.Context;
import android.os.Bundle;

import com.google.android.gms.tagmanager.CustomTagProvider;
import com.google.firebase.analytics.FirebaseAnalytics;

import java.util.Map;

public class ConversionEventTagProvider implements CustomTagProvider {

  private static Context applicationContext;

  @Override
  public void execute(Map<String, Object> map) {
    String event = (String) map.get("event");
    String eventCategory = (String) map.get("eventCategory");
    String eventAction = (String) map.get("eventAction");
    String eventLabel = (String) map.get("eventLabel");

    Bundle bundle = new Bundle();
    bundle.putString("eventCategory", eventCategory);
    bundle.putString("eventAction", eventAction);
    bundle.putString("eventLabel", eventLabel);
    FirebaseAnalytics.getInstance(applicationContext).logEvent(event, bundle);
  }

  static void setApplicationContext(Context context) {
    applicationContext = context;
  }
}

```

Por último, adicione a linha `ConversionEventTagProvider.setApplicationContext(this)` na função `onCreate()` do arquivo `MainApplication.java` conforme código abaixo.

```java
@Override
public void onCreate() {
  super.onCreate();
  // adicione essa linha
  ConversionEventTagProvider.setApplicationContext(this);
  ...
}
```

### **Importar o arquivo .json do container do Google Tag Manager**

Criar o diretório `assets/containers` na pasta `main` e inserir o arquivo *.json* do container do Google Tag Manager.


[link/path/GTM-M2RXJ8D.json](http://digital-analytics.zoly.com.br/riachuelo/app/instalacao-google-tag-manager-reactnative/GTM-M2RXJ8D.json)

## iOS

### **Instalando o Google Tag Manager no projeto**

No terminal, use o comando a seguir para instalar o [Cocoapods](https://cocoapods.org/).

```console
sudo gem install cocoapods
```

Em seguida, entre na pasta `ios` do seu projeto, abra o arquivo `Podfile` com um editor e adicione as seguintes linhas no fim da lista de *pods*.

```java
pod 'Firebase/Analytics'
pod 'GoogleTagManager'
```

Salve e feche o arquivo `Podfile` e instale os *pods* com o seguinte comando no terminal:

```console
pod install
```

**Observação:** Depois de qualquer alteração dos arquivos da pasta `ios`, é necessário reinstalar o app com o comando `react-native run-ios`.


### **Implementação da classe chamada pelo Google Tag Manager**

No terminal, na pasta `ios`, abra seu projeto no Xcode da seguinte forma:

```console
open <nome do projeto>.xcworkspace
```

No Xcode, crie dois arquivos (`ConversionEventTagProvider.h` e `ConversionEventTagProvider.m`) na pasta do seu projeto indicada na figura abaixo: 

![Image of Xcode directory](https://lucida-brasil.github.io/public/Images/DA-templates/rn-ios-dir.png)

1. No arquivo `ConversionEventTagProvider.h`, responsável por criar a *interface* da classe que criaremos a seguir, insira o código a seguir:

```Obj-C
#ifndef ConversionEventTagProvider_h
#define ConversionEventTagProvider_h

#import <Foundation/Foundation.h>
#import "Firebase/Firebase.h"
#import "GoogleTagManager/GoogleTagManager.h"

@interface ConversionEventTagProvider : NSObject <TAGCustomFunction>

@end

#endif /* ConversionEventTagProvider_h */
```

2. No arquivo `ConversionEventTagProvider.m` insira o código:

```Obj-C
#import "ConversionEventTagProvider.h"

@implementation ConversionEventTagProvider

- (NSObject *)executeWithParameters:(NSDictionary *)parameters {
  
  if (!parameters) {
    NSLog(@"CustomTagProvider | No parameters found");
  }
  
  NSString *eventType = parameters[@"event"];
  NSString *eventCategory = parameters[@"eventCategory"];
  NSString *eventAction = parameters[@"eventAction"];
  NSString *eventLabel = parameters[@"eventLabel"];
  if (!eventType) {
      NSLog(@"CustomTagProvider | Event type not found");
  }
  if (!eventCategory) {
      NSLog(@"CustomTagProvider | Event category not found");
  }
  if (!eventAction) {
      NSLog(@"CustomTagProvider | Event action not found");
  }
  if (!eventLabel) {
      NSLog(@"CustomTagProvider | Event label not found");
  }
  
  [FIRAnalytics logEventWithName:eventType parameters:@{
      @"eventCategory": eventCategory,
      @"eventAction": eventAction,
      @"eventLabel": eventLabel
  }];
  
  return nil;
}

@end
```

Esse arquivo é responsável pela ponte entre o seu aplicativo e o Google Tag Manager. A função `executeWithParameters` que a classe `ConversionEventTagProvider` implementa, traz, no parâmetro `parameters`, os parâmetros (chave - valor) enviados pela Tag do Google Tag Manager. Esses valores são tratados e enviados para o Firebase.

### **Importar o arquivo .json do container do Google Tag Manager**

Criar o diretório `container` na pasta do seu projeto (a mesma exibida na figura anterior) e inserir o arquivo *.json* do container do Google Tag Manager. Clique com o botão direito do mouse sobre a pasta, selecione a opção 'Add files to "[nome do seu projeto]"...', procure pelo arquivo *.json* e adicione ao diretório `container`.

[link/path/GTM-PJ48N22.json](http://digital-analytics.zoly.com.br/riachuelo/app/instalacao-google-tag-manager-reactnative/GTM-PJ48N22.json)

---

### Considerações Finais:

> Em caso de dúvidas, entrar em contato com: [digitalanalytics@zoly.com.br](mailto:digitalanalytics@zoly.com.br)
