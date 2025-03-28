---
title: Intestazione - X-Device-Info
description: REST API V2 - Intestazione - X-Device-Info
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: 42df16e34783807e1b5eb1a12ca9db92f4e4c161
workflow-type: tm+mt
source-wordcount: '1133'
ht-degree: 2%

---

# Intestazione - X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>X-Device-Info</b> contiene le informazioni client (dispositivo, connessione e applicazione) relative al dispositivo di streaming effettivo e viene utilizzata per determinare le regole specifiche della piattaforma che gli MVPD possono applicare.

## Sintassi {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;informazioni_dispositivo&gt;</td>
   </tr>
   <tr>
      <td>Tipo di intestazione</td>
      <td>Intestazione richiesta</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>No</td>
   </tr>
</table>

## Direttive {#directives}

<b>&lt;informazioni_dispositivo></b>

Il valore `Base64-encoded` dell&#39;elemento JSON contenente almeno gli attributi contrassegnati come richiesto dalla tabella seguente.

<table style="table-layout:auto">
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Presenza</th>
        <th style="background-color: #EFF2F7; width: 15%;">Chiave</th>
        <th style="background-color: #EFF2F7;">Descrizione</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Limitato</th>
        <th style="background-color: #EFF2F7;">Valori possibili</th>
    </tr>
    <tr>
        <td></td>
        <td>primaryHardwareType</td>
        <td>Il tipo di hardware principale del dispositivo.</td>
        <td>&amp;check;</td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Fotocamera</li>
                <li>DataCollectionTerminal</li>
                <li>Desktop</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>Console giochi</li>
                <li>GeolocationTracker</li>
                <li>Occhiali</li>
                <li>MediaPlayer</li>
                <li>Cellulare</li>
                <li>PaymentTerminal</li>
                <li>PluginModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>Tablet</li>
                <li>Punto attivo wireless</li>
                <li>Orologio</li>
                <li>Sconosciuto</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>obbligatorio</i></td>
        <td>modello</td>
        <td>Il nome del modello del dispositivo.</td>
        <td></td>
        <td>ad es. iPhone, SM-G930V, AppleTV, ecc.</td>
    </tr>
    <tr>
        <td><i>obbligatorio</i></td>
        <td>version</td>
        <td>Versione del dispositivo.</td>
        <td></td>
        <td>ad esempio 2.0.1, ecc.</td>
    </tr>
    <tr>
        <td></td>
        <td>produttore</td>
        <td>La società/organizzazione di produzione del dispositivo.</td>
        <td></td>
        <td>ad es. Samsung, LG, ZTE, Huawei, Motorola, Apple, ecc.</td>
    </tr>
    <tr>
        <td></td>
        <td>fornitore</td>
        <td>La società/organizzazione di vendita del dispositivo.</td>
        <td></td>
        <td>ad es. Apple, Samsung, LG, Google, ecc.</td>
    </tr>
    <tr>
        <td><i>obbligatorio</i></td>
        <td>osName</td>
        <td>Il nome del sistema operativo del dispositivo.</td>
        <td>&amp;check;</td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Android</li>
                <li>CHROME OS</li>
                <li>Linux</li>
                <li>MAC OS</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Sistema operativo Roku</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osFamily</td>
        <td>Il nome del gruppo del sistema operativo del dispositivo.</td>
        <td>&amp;check;</td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>Sistema operativo PlayStation</li>
                <li>Sistema operativo Roku</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osVendor</td>
        <td>Il fornitore del sistema operativo del dispositivo.</td>
        <td>&amp;check;</td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Progetto Tizen</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>obbligatorio</i></td>
        <td>osVersion</td>
        <td>Versione del sistema operativo del dispositivo.</td>
        <td></td>
        <td>ad esempio 10.2, 9.0.1, ecc.</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>Nome del browser.</td>
        <td>&amp;check;</td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Browser Android</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonkey</li>
                <li>Browser Symbian</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>La società/organizzazione di costruzione del browser.</td>
        <td>&amp;check;</td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>Motorola</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVersion</td>
        <td>Versione del browser del dispositivo.</td>
        <td></td>
        <td>esempio: 60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>L’agente utente del dispositivo.</td>
        <td></td>
        <td>ad esempio Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, like Gecko) versione/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>displayWidth</td>
        <td>Larghezza fisica dello schermo del dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayHeight</td>
        <td>L’altezza fisica dello schermo del dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayPpi</td>
        <td>Densità fisica dei pixel dello schermo del dispositivo.</td>
        <td></td>
        <td>Esempio: 294</td>
    </tr>
    <tr>
        <td></td>
        <td>diagonalScreenSize</td>
        <td>La dimensione diagonale dello schermo fisico del dispositivo, espressa in pollici.</td>
        <td></td>
        <td>ad es. 5.5, 10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>L’IP del dispositivo utilizzato per inviare richieste HTTP.</td>
        <td></td>
        <td>es. 8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>Porta del dispositivo utilizzata per l’invio di richieste HTTP.</td>
        <td></td>
        <td>ad es. 53124</td>
    </tr>
    <tr>
        <td><i>obbligatorio</i></td>
        <td>connectionType</td>
        <td>Tipo di connessione di rete.</td>
        <td></td>
        <td>ad esempio WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionSecure</td>
        <td>Stato di protezione della connessione di rete.</td>
        <td>&amp;check;</td>
        <td>
            I valori sono limitati:
            <ul>
                <li>true - nel caso di una rete sicura</li>
                <li>false - nel caso di un hotspot pubblico</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>L’identificatore univoco dell’applicazione.</td>
        <td></td>
        <td>es. REF30</td>
    </tr>
</table>


## Esempi {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```

## Cookbook {#cookbooks}

>[!IMPORTANT]
> 
> I frammenti di codice e le risorse della documentazione sono forniti a scopo di riferimento.
> 
> I frammenti di codice non sono esaustivi e potrebbero richiedere ulteriori modifiche per lavorare al progetto.
>
> Indipendentemente dall&#39;implementazione effettiva, l&#39;intestazione `X-Device-Info` deve contenere un valore formattato come descritto nella sezione [Direttive](#directives).

### Browser {#browsers}

Per le applicazioni client in esecuzione in un browser, è possibile omettere l&#39;intestazione `X-Device-Info` in quanto il browser invierà automaticamente un set minimo di informazioni richieste nell&#39;intestazione `User-Agent`.

È comunque possibile utilizzare l&#39;intestazione `X-Device-Info` per fornire informazioni aggiuntive sul dispositivo, sulla connessione e sull&#39;applicazione, nel caso in cui l&#39;applicazione client integri una libreria o un servizio che fornisce un meccanismo di identificazione del dispositivo.

### Dispositivi mobili {#mobile-devices}

#### iOS e iPadOS {#ios-ipados}

Per generare l&#39;intestazione `X-Device-Info` per i dispositivi che eseguono [iOS o iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes), è possibile fare riferimento ai seguenti documenti e frammenti di codice:

* Documentazione per gli sviluppatori di Apple per [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Documentazione per gli sviluppatori di Apple per [Raggiungibilità](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Documentazione manuale Linux per [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|---------------|------------------------|-----------------|
| modello | uname.machine | iPhone |
| fornitore | hardcoded | Apple |
| produttore | hardcoded | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 320 |
| displayHeight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10,2 |

Le informazioni di connessione possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Raggiungibilità currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |


Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|---------------|-----------|-----------------|
| applicationId | hardcoded | REF30 |

#### Android {#android}

Per generare l&#39;intestazione `X-Device-Info` per i dispositivi che eseguono [Android](https://developer.android.com/about/versions), è possibile fare riferimento ai seguenti documenti e frammenti di codice:

* Documentazione per gli sviluppatori di Android per la classe [Build](https://developer.android.com/reference/android/os/Build.html).

```JAVA
private JSONObject computeClientInformation() {
     String LOGGING_TAG = "DefineClass.class";
  
     JSONObject clientInformation = new JSONObject();

     String connectionType;

     try {
          ConnectivityManager cm = (ConnectivityManager) getContext().getSystemService(CONNECTIVITY_SERVICE);
          NetworkInfo activeNetwork = cm.getActiveNetworkInfo();

          if (activeNetwork != null && activeNetwork.isConnectedOrConnecting()) {
              switch (activeNetwork.getType()) {
                    case ConnectivityManager.TYPE_WIFI: {
                        connectionType = "WIFI";
                        break;
                    }
                    case ConnectivityManager.TYPE_BLUETOOTH: {
                        connectionType = "BLUETOOTH";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE: {
                        connectionType = "MOBILE";
                        break;
                    }
                    case ConnectivityManager.TYPE_ETHERNET: {
                        connectionType = "ETHERNET";
                        break;
                    }
                    case ConnectivityManager.TYPE_VPN: {
                        connectionType = "VPN";
                        break;
                    }
                    case ConnectivityManager.TYPE_DUMMY: {
                        connectionType = "DUMMY";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE_DUN: {
                        connectionType = "MOBILE_DUN";
                        break;
                    }
                    case ConnectivityManager.TYPE_WIMAX: {
                        connectionType = "WIMAX";
                        break;
                    }
                    default:
                       connectionType = ConnectivityManager.EXTRA_OTHER_NETWORK_INFO;
              }
          } else {
                connectionType = ConnectivityManager.EXTRA_NO_CONNECTIVITY;
          }
     } catch (Exception e) {
          connectionType = "notAccessible";
     }

     try {
          clientInformation.put("model", Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer", Build.MANUFACTURER);
          clientInformation.put("version", Build.DEVICE);
          clientInformation.put("osName", "Android");
          clientInformation.put("osVersion", Build.VERSION.RELEASE);
          clientInformation.put("connectionType", connectionType);
          clientInformation.put("applicationId", "REF30");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|---------------|-----------------------------|-----------------|
| modello | Build.MODEL | GT-I9505 |
| fornitore | Build.BRAND | samsung |
| produttore | Build.MANUFACTURER | samsung |
| version | Build.DEVICE | jflte |
| displayWidth | DisplayMetrics.widthPixels | 600 |
| displayHeight | DisplayMetrics.heightPixels | 800 |
| osName | hardcoded | Android |
| osVersion | Build.VERSION.RELEASE | 5.0.1. |

Le informazioni di connessione possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|---------------|-----------|-----------------|
| applicationId | hardcoded | REF30 |

### Dispositivi collegati al televisore {#tv-connected-devices}

#### tvOS {#tvos}

Per generare l&#39;intestazione `X-Device-Info` per i dispositivi che eseguono [tvOS](https://developer.apple.com/documentation/tvos-release-notes), è possibile fare riferimento ai seguenti documenti e frammenti di codice:

* Documentazione per gli sviluppatori di Apple per [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Documentazione per gli sviluppatori di Apple per [Raggiungibilità](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Documentazione manuale Linux per [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|---------------|------------------------|-----------------|
| modello | uname.machine | AppleTV |
| fornitore | hardcoded | Apple |
| produttore | hardcoded | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 1920 |
| displayHeight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10,2 |

Le informazioni di connessione possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Raggiungibilità currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|---------------|-----------|-----------------|
| applicationId | hardcoded | REF30 |

#### Fire OS {#fireos}

Per generare l&#39;intestazione `X-Device-Info` per i dispositivi che eseguono [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html), è possibile fare riferimento ai seguenti documenti:

* Documentazione per gli sviluppatori di Android per la classe [Build](https://developer.android.com/reference/android/os/Build.html).
* Documentazione per gli sviluppatori di Amazon per [Identificazione dei dispositivi Fire TV](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html).

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|---------------|-----------------------------|-----------------|
| modello | Build.MODEL | AFTM |
| fornitore | Build.BRAND | Amazon |
| produttore | Build.MANUFACTURER | Amazon |
| version | Build.DEVICE | montoya |
| displayWidth | DisplayMetrics.widthPixels |                 |
| displayHeight | DisplayMetrics.heightPixels |                 |
| osName | hardcoded | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1. |

Le informazioni di connessione possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|---------------|-----------|-----------------|
| applicationId | hardcoded | REF30 |

#### Sistema operativo Roku {#rokuos}

Per generare l&#39;intestazione `X-Device-Info` per i dispositivi che eseguono [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md), è possibile fare riferimento ai seguenti documenti:

* Documentazione per gli sviluppatori Roku per [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md).

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|---------------|--------------------------------------------|-----------------|
| modello | hardcoded | &quot;Roku&quot; |
| fornitore | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| produttore | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| version | ifDeviceInfo.GetModelDetails().ModelNumber | 5303X |
| displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| osName | hardcoded | &quot;Roku&quot; |
| osVersion | ifDeviceInfo.getVersion() |                 |

Le informazioni di connessione possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
| connectionSecure | hardcoded | true se la connessione è cablata |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|---------------|-----------|-----------------|
| applicationId | hardcoded | REF30 |

### Altri {#others}

Per le piattaforme per dispositivi non incluse nella documentazione, le informazioni sul client (dispositivo, connessione e applicazione) devono essere collegate a qualsiasi attributo hardware e sistema operativo disponibile, in genere specificato nei manuali hardware e del sistema operativo del dispositivo.
