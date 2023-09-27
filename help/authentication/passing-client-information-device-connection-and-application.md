---
title: Trasmissione delle informazioni del client (dispositivo, connessione e applicazione)
description: Trasmissione delle informazioni del client (dispositivo, connessione e applicazione)
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1681'
ht-degree: 0%

---

# Trasmissione delle informazioni del client (dispositivo, connessione e applicazione) {#pass-client-info}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.


## Ambito {#pass-client-info-scope}

Questo documento aggrega i dettagli e i manuali per il passaggio di informazioni client (dispositivo, connessione e applicazione) da un’applicazione Programmatore alle API REST o agli SDK di autenticazione di Adobe Pass.

I vantaggi della fornitura di informazioni ai clienti sono:

* La capacità di abilitare correttamente l&#39;autenticazione Home Base (HBA) nel caso di alcuni tipi di dispositivi e MVPD che supportano l&#39;HBA.
* Possibilità di applicare correttamente i TTL nel caso di alcuni tipi di dispositivi (ad esempio, configurare TTL più lunghi per le sessioni di autenticazione su dispositivi connessi alla TV).
* La possibilità di aggregare correttamente le metriche aziendali nei rapporti suddivisi per tipo di dispositivo utilizzando il monitoraggio dei servizi di adesione (ESM, Entitlement Service Monitoring).
* Sblocca la possibilità di applicare correttamente varie regole aziendali (ad es. degradazione) su tipi di dispositivi specifici.

## Panoramica {#pass-client-info-overview}

Le informazioni sul cliente sono costituite da:

* **Dispositivo** informazioni sugli attributi hardware e software del dispositivo da cui l&#39;utente sta tentando di utilizzare il contenuto del programmatore.
* **Connessione** informazioni sugli attributi di connessione del dispositivo da cui l’utente si connette ai servizi di autenticazione e/o ai servizi programmatori di Adobe Pass (ad esempio implementazioni server-to-server).
* **Applicazione** informazioni sull&#39;applicazione registrata da cui l&#39;utente sta tentando di utilizzare il contenuto del programmatore.

Le informazioni client sono un oggetto JSON creato con le chiavi presentate nella tabella seguente.

>[!NOTE]
>
>I seguenti elementi **tasti** sono **obbligatorio** da inviare nell’oggetto JSON di informazioni client: **modello**, **osName**.
>
>I seguenti tasti sono **limitato** valori: `primaryHardwareType`, `osName`, `osFamily`, `browserName`, `browserVendor`, `connectionSecure`.

|   | Chiave | Limitato | Descrizione | Valori possibili |
|---|---|---|---|---|
|            | primaryHardwareType | # Sì | Il tipo di hardware principale del dispositivo. | # I valori sono limitati: Camera DataCollectionTerminal Desktop EmbeddedNetworkModule eReader GamesConsole GeolocationTracker Occhiali MediaPlayer MobilePhone PaymentTerminal PluginModem SetTopBox TV Tablet WirelessHotspot Orologio da polso sconosciuto |
| #mandatory | modello | No | Il nome del modello del dispositivo. | ad es. iPhone, SM-G930V, AppleTV, ecc. |
|            | version | No | Versione del dispositivo. | ad esempio 2.0.1, ecc. |
|            | produttore | No | La società/organizzazione di produzione del dispositivo. | ad es. Samsung, LG, ZTE, Huawei, Motorola, Apple, ecc. |
|            | fornitore | No | Azienda/organizzazione di vendita del dispositivo. | ad es. Apple, Samsung, LG, Google, ecc. |
| #mandatory | osName | # Sì | Il nome del sistema operativo del dispositivo. | # I valori sono limitati: Android Chrome OS Linux Mac OS X OpenBSD Roku OS Windows iOS tvOS webOS |
|            | osFamily | Sì | Il nome del gruppo del sistema operativo del dispositivo. | # I valori sono limitati: Android BSD Linux PlayStation OS Roku OS Symbian Tizen Windows iOS macOS tvOS webOS |
|            | osVendor | No | Il sistema operativo del dispositivo. | Amazon Apple Google LG Microsoft Mozilla Nintendo Nokia Roku Samsung Sony Tizen Project |
|            | osVersion | No | Versione del sistema operativo del dispositivo. | ad esempio 10.2, 9.0.1, ecc. |
|            | browserName | # Sì | Nome del browser. | # I valori sono limitati: Browser Android Chrome Edge Firefox Internet Explorer Opera Safari SeaMonkey Symbian Browser |
|            | browserVendor | # Sì | La società/organizzazione di creazione del browser. | # I valori sono limitati: Amazon Apple Google Microsoft Motorola Mozilla Netscape Nintendo Nokia Samsung Sony Ericsson |
|            | browserVersion | No | Versione del browser del dispositivo. | esempio: 60.0.3112 |
|            | userAgent | No | Agente utente del dispositivo. | ad esempio Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, like Gecko) versione/10.0.3 Safari/602.4.8 |
|            | displayWidth | No | Larghezza fisica dello schermo del dispositivo. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | No | L’altezza fisica dello schermo del dispositivo. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | No | Densità fisica dei pixel dello schermo del dispositivo. | Esempio: 294 |
|            | diagonalScreenSize | No | La dimensione diagonale dello schermo fisico del dispositivo in pollici. | ad es. 5.5, 10.1 |
|            | connectionIp | No | L’IP del dispositivo utilizzato per inviare richieste HTTP. | es. 8.8.4.4 |
|            | connectionPort | No | Porta del dispositivo utilizzata per l’invio di richieste HTTP. | ad es. 53124 |
|            | connectionType | No | Tipo di connessione di rete. | ad esempio WiFi, LAN, 3G, 4G, 5G |
|            | connectionSecure | # Sì | Stato di protezione della connessione di rete. | # I valori sono limitati: true - nel caso di una rete sicura false - nel caso di un hotspot pubblico |
|            | applicationId | No | L’identificatore univoco dell’applicazione. | es. CNN |

## Riferimenti API {#api-ref}

Questa sezione descrive l’API responsabile della gestione delle informazioni client quando si utilizzano le API REST o gli SDK per l’autenticazione di Adobe Pass.

### API REST {#rest-api}

I servizi di autenticazione di Adobe Pass supportano la ricezione delle informazioni client nei modi seguenti:

* As a **intestazione: &quot;X-Device-Info&quot;**
* As a **parametro query: &quot;device_info&quot;**
* As a **parametro post: &quot;device_info&quot;**

>[!IMPORTANT]
>
>In tutti e tre gli scenari, il payload dell’intestazione o del parametro deve essere **Codifica Base64 e codifica URL**.

**SDK**

#### SDK JavaScript {#js-sdk}

L’SDK JavaScript di AccessEnabler crea per impostazione predefinita un oggetto JSON di informazioni client, che verrà passato ai servizi di autenticazione di Adobe Pass, a meno che non venga sostituito.

L’SDK JavaScript di AccessEnabler supporta **solo sostituzione** la chiave &quot;applicationId&quot; dall’oggetto JSON di informazioni client attraverso [setRequestor](/help/authentication/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options))di *applicationId* parametro options.

>[!CAUTION]
>
>Il `applicationId` Il valore del parametro deve essere un valore String di testo normale.
>Se l’applicazione Programmer decide di passare l’applicationId, le altre chiavi di informazioni client verranno comunque calcolate dall’SDK JavaScript di AccessEnabler.

#### SDK per iOS/tvOS {#ios-tvos-sdk}

L’SDK iOS/tvOS di AccessEnabler crea per impostazione predefinita un oggetto JSON di informazioni client, che verrà passato ai servizi di autenticazione di Adobe Pass, a meno che non venga sostituito.

L’SDK di AccessEnabler per iOS/tvOS supporta **sovrascrittura di tutto** informazioni client oggetto JSON tramite [setOptions](/help/authentication/iostvos-sdk-api-reference.md#setoptions)parametro device_info di.

>[!CAUTION]
>
>Il *device_info* il valore del parametro deve essere un **Codifica Base64** *NSString* valore.
>
>Nel caso in cui l’applicazione Programmatore decida di trasmettere il *device_info*, quindi tutte le chiavi di informazioni client calcolate dall’SDK di AccessEnabler iOS/tvOS verranno ignorate. Pertanto, è molto importante calcolare e trasmettere i valori per il maggior numero possibile di chiavi. Per ulteriori dettagli sull’implementazione, vedi [Panoramica](#pass-client-info-overview) tabella e [Manuale iOS/tvOS](#ios-tvos).

#### SDK per Android e FireOS {#and-fire-os-sdk}

Il `AccessEnabler` L’SDK Android/FireOS crea per impostazione predefinita un oggetto JSON di informazioni client, che verrà passato ai servizi di autenticazione di Adobe Pass, a meno che non venga sostituito.

Il `AccessEnabler` L’SDK per Android/FireOS supporta **sovrascrittura di tutto** informazioni client oggetto JSON tramite [setOptions](/help/authentication/android-sdk-api-reference.md#setOptions)s/[setOptions](/help/authentication/amazon-fireos-native-client-api-reference.md#fire_setOption)di `device_info` parametro.

>[!NOTE]
>
>Il `device_info` il valore del parametro deve essere un **Codifica Base64** Valore stringa.

>[!IMPORTANT]
>
>Nel caso in cui l’applicazione Programmatore decida di trasmettere il `device_info`, quindi tutte le chiavi di informazioni client calcolate da `AccessEnabler` L’SDK Android/FireOS verrà sovrascritto. Pertanto, è molto importante calcolare e trasmettere i valori per il maggior numero possibile di chiavi. Per ulteriori dettagli sull’implementazione, vedi [Panoramica](#pass-client-info-overview) tabella e [Android](#android) e [FireOS](#fire-tv) manuale di cucina.

## Cookbook {#cookbooks}

Questa sezione presenta un manuale per la creazione dell’oggetto JSON delle informazioni client in caso di diversi tipi di dispositivi.

>[!IMPORTANT]
>
>Tasti contrassegnati con  **!** sono obbligatori per l’invio.

### Android {#android}

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

|   | Chiave | Sorgente | Valore (esempio) |
|---|---------------|-----------------------------|---------------|
| ! | modello | Build.MODEL | GT-I9505 |
|   | fornitore | Build.BRAND | samsung |
|   | produttore | Build.MANUFACTURER | samsung |
| ! | version | Build.DEVICE | jflte |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | hardcoded | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.0.1 |

Le informazioni di connessione possono essere costruite nel modo seguente:

|   | Chiave | Sorgente | Valore (esempio) |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

|   | Chiave | Sorgente | Valore (esempio) |
|---|---------------|-----------|--------------|
|   | applicationId | hardcoded | CNN |

>[!IMPORTANT]
>
Le informazioni su dispositivo, connessione e applicazione devono essere aggiunte allo stesso oggetto JSON. In seguito, l’oggetto risultante deve essere **Codifica Base64**. Inoltre, nel caso delle API REST di autenticazione di Adobe Pass, il valore deve essere **Codifica URL**.

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
**Risorse:**
* classe pubblica [build](https://developer.android.com/reference/android/os/Build.html){target=_blank} nella documentazione per gli sviluppatori Java.

### FireTV {#fire-tv}

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

|   | Chiave | Sorgente | Valore (ad es.) |
|---|---------------|-----------------------------|--------------|
| ! | modello | Build.MODEL | AFTM |
|   | fornitore | Build.BRAND | Amazon |
|   | produttore | Build.MANUFACTURER | Amazon |
| ! | version | Build.DEVICE | montoya |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | hardcoded | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1 |

Le informazioni di connessione possono essere costruite nel modo seguente:

|   | Chiave | Sorgente | Valore (esempio) |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

|   | Chiave | Sorgente | Valore (esempio) |
|---|---------------|-----------|--------------|
|   | applicationId | hardcoded | CNN |

>[!IMPORTANT]
>
Le informazioni su dispositivo, connessione e applicazione devono essere aggiunte allo stesso oggetto JSON. In seguito, l’oggetto risultante deve essere **Codifica Base64**. Inoltre, nel caso delle API REST di autenticazione di Adobe Pass, il valore deve essere **Codifica URL**.

>[!NOTE]
>
**Risorse:**
* classe pubblica [Genera](https://developer.android.com/reference/android/os/Build.html){target=_blank} nella documentazione per gli sviluppatori di Android.
* [Identificazione dei dispositivi FireTV](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

|   | Chiave | Sorgente | Valore (esempio) |
|---|---------------|------------------------|--------------|
| ! | modello | uname.machine | iPhone |
|   | fornitore | hardcoded | Apple |
|   | produttore | hardcoded | Apple |
| ! | version | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10.2 |

Le informazioni di connessione possono essere costruite nel modo seguente:

|   | Chiave | Sorgente | Valore (esempio) |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [Raggiungibilità currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

|   | Chiave | Sorgente | Valore (esempio) |
|---|---------------|-----------|--------------|
|   | applicationId | hardcoded | CNN |

>[!IMPORTANT]
>
Le informazioni su dispositivo, connessione e applicazione devono essere aggiunte allo stesso oggetto JSON. Successivamente, l&#39;oggetto risultante deve essere codificato in Base64. Inoltre, nel caso delle API REST di autenticazione di Adobe Pass, il valore deve essere codificato in URL.

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
**Risorse:**
* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
* [Informazioni sulla raggiungibilità](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

| Chiave | Sorgente | Valore (esempio) |                 |
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

|   | Chiave | Sorgente | Valore (esempio) |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
|   | connectionSecure | hardcoded | true se la connessione è cablata |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

|   | Chiave | Sorgente | Valore (esempio) |
|---|---------------|-----------|--------------|
|   | applicationId | hardcoded | CNN |

>[!IMPORTANT]
>
Le informazioni su dispositivo, connessione e applicazione devono essere aggiunte allo stesso oggetto JSON. In seguito, l’oggetto risultante deve essere **Codifica Base64**. Inoltre, nel caso delle API REST di autenticazione di Adobe Pass, il valore deve essere codificato in URL.

>[!NOTE]
>
Per ulteriori informazioni, consulta [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)

### XBOX 1/360 {#xbox}

Le informazioni sul dispositivo possono essere costruite nel modo seguente:

|   | Chiave | Sorgente | Valore (esempio) |
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

|   | Chiave | Sorgente | Esempio |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | TipoAutenticazioneRete | &quot;None&quot;, &quot;Wpa&quot; ecc. |

Le informazioni sull&#39;applicazione possono essere costruite nel modo seguente:

| Chiave | Sorgente | Valore (esempio) |
|---|---|---|
| applicationId | hardcoded | CNN |

>[!IMPORTANT]
>
Le informazioni su dispositivo, connessione e applicazione devono essere aggiunte allo stesso oggetto JSON. In seguito, l’oggetto risultante deve essere **Codifica Base64**. Inoltre, nel caso delle API REST di autenticazione di Adobe Pass, il valore deve essere **Codifica URL**.

**Risorse**

* [Classe EasClientDeviceInformation](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [Classe DisplayInformation](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
