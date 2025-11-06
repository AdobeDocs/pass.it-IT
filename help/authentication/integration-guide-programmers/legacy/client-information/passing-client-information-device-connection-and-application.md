---
title: Trasmissione delle informazioni del client (dispositivo, connessione e applicazione)
description: Trasmissione delle informazioni del client (dispositivo, connessione e applicazione)
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 1%

---

# (Legacy) Trasmissione delle informazioni del client (dispositivo, connessione e applicazione) {#pass-client-info}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Ambito {#pass-client-info-scope}

Questo documento aggrega i dettagli e i manuali per il passaggio di informazioni client (dispositivo, connessione e applicazione) da un’applicazione Programmatore alle API REST o agli SDK di autenticazione di Adobe Pass.

I vantaggi della fornitura di informazioni ai clienti sono:

* La capacità di abilitare correttamente l&#39;autenticazione Home Base (HBA) nel caso di alcuni tipi di dispositivi e MVPD che supportano l&#39;HBA.
* Possibilità di applicare correttamente i TTL nel caso di alcuni tipi di dispositivi (ad esempio, configurare TTL più lunghi per le sessioni di autenticazione su dispositivi connessi alla TV).
* La possibilità di aggregare correttamente le metriche aziendali nei rapporti suddivisi per tipo di dispositivo utilizzando il monitoraggio dei servizi di adesione (ESM, Entitlement Service Monitoring).
* Sblocca la possibilità di applicare correttamente varie regole aziendali (ad es. degradazione) su tipi di dispositivi specifici.

## Panoramica {#pass-client-info-overview}

Le informazioni sul cliente sono costituite da:

* **Informazioni sul dispositivo** relative agli attributi hardware e software del dispositivo da cui l&#39;utente sta tentando di utilizzare il contenuto del programmatore.
* **Connessione** informazioni sugli attributi di connessione del dispositivo da cui l&#39;utente si connette ai servizi di autenticazione e/o ai servizi Programmer di Adobe Pass (ad esempio, implementazioni server-to-server).
* **Applicazione** informazioni sull&#39;applicazione registrata da cui l&#39;utente sta tentando di utilizzare il contenuto del programmatore.

Le informazioni client sono un oggetto JSON creato con le chiavi presentate nella tabella seguente.

>[!NOTE]
>
>Le **chiavi** seguenti sono **obbligatorie** da inviare nell&#39;oggetto JSON delle informazioni client: **model**, **osName**.
>
>Le chiavi seguenti hanno **valori limitati**: `primaryHardwareType`, `osName`, `osFamily`, `browserName`, `browserVendor`, `connectionSecure`.

|   | Chiave | Limitato | Descrizione | Valori possibili |
|---|---|---|---|---|
|            | primaryHardwareType | # Sì | Il tipo di hardware principale del dispositivo. | # I valori sono limitati:                                                                     Fotocamera                                                      DataCollectionTerminal                                                      Desktop                                                      EmbeddedNetworkModule                                                      eReader                                                      Console giochi                                                      GeolocationTracker                                                      Occhiali                                                      MediaPlayer                                                      Cellulare                                                      PaymentTerminal                                                      PluginModem                                                      SetTopBox                                                      TV                                                      Tablet                                                      Punto attivo wireless                                                      Orologio                                                      Sconosciuto |
| #mandatory | modello | No | Il nome del modello del dispositivo. | ad es. iPhone, SM-G930V, AppleTV, ecc. |
|            | version | No | Versione del dispositivo. | ad esempio 2.0.1, ecc. |
|            | produttore | No | La società/organizzazione di produzione del dispositivo. | ad es. Samsung, LG, ZTE, Huawei, Motorola, Apple, ecc. |
|            | fornitore | No | Azienda/organizzazione di vendita del dispositivo. | ad es. Apple, Samsung, LG, Google, ecc. |
| #mandatory | osName | # Sì | Il nome del sistema operativo del dispositivo. | # I valori sono limitati:                                                   Android                   CHROME OS                   Linux                   MAC OS                   OS X                   OpenBSD                   Sistema operativo Roku                   Windows                   iOS                   tvOS                   webOS |
|            | osFamily | Sì | Il nome del gruppo del sistema operativo del dispositivo. | # I valori sono limitati:                                                   Android                   BSD                   Linux                   Sistema operativo PlayStation                   Sistema operativo Roku                   Symbian                   Tizen                   Windows                   iOS                   macOS                   tvOS                   webOS |
|            | osVendor | No | Il sistema operativo del dispositivo. | Amazon                   Apple                   Google                   LG                   Microsoft                   Mozilla                   Nintendo                   Nokia                   Roku                   Samsung                   Sony                   Progetto Tizen |
|            | osVersion | No | Versione del sistema operativo del dispositivo. | ad esempio 10.2, 9.0.1, ecc. |
|            | browserName | # Sì | Nome del browser. | # I valori sono limitati:                                                   Browser Android                   Chrome                   Edge                   Firefox                   Internet Explorer                   Opera                   Safari                   SeaMonkey                   Browser Symbian |
|            | browserVendor | # Sì | La società/organizzazione di creazione del browser. | # I valori sono limitati:                                                   Amazon                   Apple                   Google                   Microsoft                   Motorola                   Mozilla                   Netscape                   Nintendo                   Nokia                   Samsung                   Sony Ericsson |
|            | browserVersion | No | Versione del browser del dispositivo. | esempio: 60.0.3112 |
|            | userAgent | No | Agente utente del dispositivo. | ad esempio Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, like Gecko) versione/10.0.3 Safari/602.4.8 |
|            | displayWidth | No | Larghezza fisica dello schermo del dispositivo. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | No | L’altezza fisica dello schermo del dispositivo. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | No | Densità fisica dei pixel dello schermo del dispositivo. | Esempio: 294 |
|            | diagonalScreenSize | No | La dimensione diagonale dello schermo fisico del dispositivo in pollici. | ad es. 5.5, 10.1 |
|            | connectionIp | No | L’IP del dispositivo utilizzato per inviare richieste HTTP. | Esempio: 8.8.4.4 |
|            | connectionPort | No | Porta del dispositivo utilizzata per l’invio di richieste HTTP. | ad es. 53124 |
|            | connectionType | No | Tipo di connessione di rete. | ad esempio WiFi, LAN, 3G, 4G, 5G |
|            | connectionSecure | # Sì | Stato di protezione della connessione di rete. | # I valori sono limitati:                                                   true - nel caso di una rete sicura                   false - nel caso di un hotspot pubblico |
|            | applicationId | No | L’identificatore univoco dell’applicazione. | es. CNN |

## Riferimenti API {#api-ref}

Questa sezione descrive l’API responsabile della gestione delle informazioni client quando si utilizzano le API REST o gli SDK per l’autenticazione di Adobe Pass.

### API REST {#rest-api}

I servizi di autenticazione di Adobe Pass supportano la ricezione delle informazioni client nei modi seguenti:

* Come **intestazione: &quot;X-Device-Info&quot;**
* Come parametro **query: &quot;device_info&quot;**
* Come **parametro post: &quot;device_info&quot;**

>[!IMPORTANT]
>
>In tutti e tre gli scenari, il payload dell&#39;intestazione o del parametro deve essere **Codificato Base64 e Codificato URL**.

**SDK**

#### JavaScript SDK {#js-sdk}

AccessEnabler JavaScript SDK crea per impostazione predefinita un oggetto JSON di informazioni client, che verrà passato ai servizi di autenticazione di Adobe Pass, a meno che non venga sostituito.

AccessEnabler JavaScript SDK supporta **l&#39;override solo** della chiave &quot;applicationId&quot; dall&#39;oggetto JSON delle informazioni client tramite il parametro [applicationId](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options)) delle opzioni *setRequestor*.

>[!CAUTION]
>
>Il valore del parametro `applicationId` deve essere un valore String di testo normale.
>Se l&#39;applicazione Programmer decide di passare l&#39;applicationId, le altre chiavi di informazioni client verranno comunque calcolate da AccessEnabler JavaScript SDK.

#### iOS/tvOS SDK {#ios-tvos-sdk}

AccessEnabler iOS/tvOS SDK crea per impostazione predefinita un oggetto JSON di informazioni client, che verrà passato ai servizi di autenticazione di Adobe Pass, a meno che non venga sostituito.

Il SDK AccessEnabler iOS/tvOS supporta **l&#39;override dell&#39;intero oggetto JSON delle informazioni client** tramite il parametro device_info di [setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setoptions).

>[!CAUTION]
>
>Il valore del parametro *device_info* deve essere un valore **Base64 codificato** *NSString*.
>
>Nel caso in cui l&#39;applicazione Programmer decida di passare *device_info*, tutte le chiavi di informazioni client calcolate dal SDK AccessEnabler iOS/tvOS verranno ignorate. Pertanto, è molto importante calcolare e trasmettere i valori per il maggior numero possibile di chiavi. Per ulteriori dettagli sull&#39;implementazione, consulta la tabella [Panoramica](#pass-client-info-overview) e il [manuale iOS/tvOS](#ios-tvos).

#### Android/FireOS SDK {#and-fire-os-sdk}

Il SDK Android/FireOS `AccessEnabler` crea per impostazione predefinita un oggetto JSON di informazioni client, che verrà passato ai servizi di autenticazione di Adobe Pass, a meno che non venga sostituito.

Il SDK di Android/FireOS `AccessEnabler` supporta **l&#39;override dell&#39;intero oggetto JSON delle informazioni client** tramite il parametro [&#x200B; di &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setOptions)setOptions[&#39;s/](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#fire_setOption)setOptions`device_info`.

>[!NOTE]
>
>Il valore del parametro `device_info` deve essere un valore stringa **Base64 codificato**.

>[!IMPORTANT]
>
>Nel caso in cui l&#39;applicazione Programmer decida di passare `device_info`, tutte le chiavi di informazioni client calcolate dal SDK Android/FireOS `AccessEnabler` verranno ignorate. Pertanto, è molto importante calcolare e trasmettere i valori per il maggior numero possibile di chiavi. Per ulteriori dettagli sull&#39;implementazione, consulta la tabella [Panoramica](#pass-client-info-overview) e il manuale di istruzioni [Android](#android) e [FireOS](#fire-tv).

## Cookbook {#cookbooks}

Questa sezione presenta un manuale per la creazione dell’oggetto JSON delle informazioni client in caso di diversi tipi di dispositivi.

>[!IMPORTANT]
>
>Le chiavi contrassegnate con **.** sono obbligatori per essere inviati.

### Android {#android}

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (esempio) |
|---|---------------|-----------------------------|---------------|
| ! | modello | Build.MODEL | GT-I9505 |
|   | fornitore | Build.BRAND | samsung |
|   | produttore | Build.MANUFACTURER | samsung |
| ! | version | Build.DEVICE | jflte |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | hardcoded | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.0.1. |

Le informazioni di connessione possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (esempio) |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (esempio) |
|---|---------------|-----------|--------------|
|   | applicationId | hardcoded | CNN |

>[!IMPORTANT]
>
>Le informazioni su dispositivo, connessione e applicazione devono essere aggiunte allo stesso oggetto JSON. In seguito, l&#39;oggetto risultante deve essere **Codificato Base64**. Inoltre, nel caso delle API REST di autenticazione di Adobe Pass, il valore deve essere **URL codificato**.

**Codice di esempio**

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
          clientInformation.put("model",Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer",Build.MANUFACTURER);
          clientInformation.put("version",Build.DEVICE);
          clientInformation.put("osName","Android");
          clientInformation.put("osVersion",Build.VERSION.RELEASE);
          clientInformation.put("connectionType",connectionType);
          clientInformation.put("applicationId","CNN");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

>[!NOTE]
>
>**Risorse:**
>* classe pubblica [build](https://developer.android.com/reference/android/os/Build.html){target=_blank} nella documentazione per gli sviluppatori Java.

### FireTV {#fire-tv}

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (ad es.) |
|---|---------------|-----------------------------|--------------|
| ! | modello | Build.MODEL | AFTM |
|   | fornitore | Build.BRAND | Amazon |
|   | produttore | Build.MANUFACTURER | Amazon |
| ! | version | Build.DEVICE | montoya |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | hardcoded | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1. |

Le informazioni di connessione possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (esempio) |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (esempio) |
|---|---------------|-----------|--------------|
|   | applicationId | hardcoded | CNN |

>[!IMPORTANT]
>
>Le informazioni su dispositivo, connessione e applicazione devono essere aggiunte allo stesso oggetto JSON. In seguito, l&#39;oggetto risultante deve essere **Codificato Base64**. Inoltre, nel caso delle API REST di autenticazione di Adobe Pass, il valore deve essere **URL codificato**.

>[!NOTE]
>
>**Risorse:**
>* classe pubblica [Build](https://developer.android.com/reference/android/os/Build.html){target=_blank} nella documentazione per gli sviluppatori di Android.
>* [Identificazione dispositivi FireTV](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (esempio) |
|---|---------------|------------------------|--------------|
| ! | modello | uname.machine | iPhone |
|   | fornitore | hardcoded | Apple |
|   | produttore | hardcoded | Apple |
| ! | version | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10,2 |

Le informazioni di connessione possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (esempio) |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [Raggiungibilità currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (esempio) |
|---|---------------|-----------|--------------|
|   | applicationId | hardcoded | CNN |

>[!IMPORTANT]
>
>Le informazioni su dispositivo, connessione e applicazione devono essere aggiunte allo stesso oggetto JSON. Successivamente, l&#39;oggetto risultante deve essere codificato in Base64. Inoltre, nel caso delle API REST di autenticazione di Adobe Pass, il valore deve essere codificato in URL.

**Codice di esempio**

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
                @"applicationId": @"CNN" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

>[!NOTE]
>
>**Risorse:**
>* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
>* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
>* [Informazioni sulla raggiungibilità](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | modello | hardcoded | &quot;Roku&quot; |
|     | fornitore | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
|     | produttore | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| ! | version | ifDeviceInfo.GetModelDetails().ModelNumber | 5303X |
|     | displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ! | osName | hardcoded | &quot;Roku&quot; |
| ! | osVersion | ifDeviceInfo.getVersion() |                 |

Le informazioni di connessione possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (esempio) |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
|   | connectionSecure | hardcoded | true se la connessione è cablata |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (esempio) |
|---|---------------|-----------|--------------|
|   | applicationId | hardcoded | CNN |

>[!IMPORTANT]
>
>Le informazioni su dispositivo, connessione e applicazione devono essere aggiunte allo stesso oggetto JSON. In seguito, l&#39;oggetto risultante deve essere **Codificato Base64**. Inoltre, nel caso delle API REST di autenticazione di Adobe Pass, il valore deve essere codificato in URL.

>[!NOTE]
>
>Per ulteriori informazioni, vedere [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)

### XBOX 1/360 {#xbox}

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

|   | Chiave | Source | Valore (esempio) |
|---|---|---|---|
| ! | modello | EasClientDeviceInformation.SystemProductName |                 |
|   | fornitore | hardcoded | Microsoft |
|   | produttore | hardcoded | Microsoft |
| ! | version | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

Le informazioni di connessione possono essere costruite nel modo seguente:

|   | Chiave | Source | Esempio |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | TipoAutenticazioneRete | &quot;None&quot;, &quot;Wpa&quot; ecc. |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

| Chiave | Source | Valore (esempio) |
|---|---|---|
| applicationId | hardcoded | CNN |

>[!IMPORTANT]
>
>Le informazioni su dispositivo, connessione e applicazione devono essere aggiunte allo stesso oggetto JSON. In seguito, l&#39;oggetto risultante deve essere **Codificato Base64**. Inoltre, nel caso delle API REST di autenticazione di Adobe Pass, il valore deve essere **URL codificato**.

**Risorse**

* [Classe EasClientDeviceInformation](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [Classe DisplayInformation](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
