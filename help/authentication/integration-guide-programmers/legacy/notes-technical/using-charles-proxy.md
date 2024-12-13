---
title: Utilizzo di Charles Proxy
description: Utilizzo di Charles Proxy
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# (Legacy) Utilizzo di Charles Proxy {#using-charles-proxy}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.


**Carlo:** <http://charlesproxy.com>


## Download, installazione e guida introduttiva a Charles Proxy {#download-install-and-get-stared-with-charles-proxy}

- **Scarica** - <http://www.charlesproxy.com/download/>
- **Installa** - <http://www.charlesproxy.com/documentation/installation/>
- **Guida introduttiva** - <http://www.charlesproxy.com/documentation/getting-started/>


## Schede Struttura e Sequenza {#structure-vs-sequence-tabs}

Esistono due modi diversi per visualizzare il traffico:

1. **Struttura** - Le richieste sono raggruppate per host
1. **Sequenza** - Le richieste sono elencate nell&#39;ordine in cui vengono chiamate


## SSL e certificati {#ssl-and-certificates}

Abilita proxy SSL `\[ *Proxy -\> Proxy Settings... -\> SSL* \]`

Seleziona la casella di controllo &quot;Abilita proxy SSL&quot; e aggiungi tutte le posizioni HTTPS.


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)



- Proxy SSL - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- Certificati SSL - <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- Proxy SSL da dispositivi mobili: vedi di seguito.


## Ignora/Escludi host {#ignore-/-exclude-hosts}

Se l&#39;output risulta troppo ingombrante, è possibile scegliere di ignorare o escludere le posizioni. È possibile ignorare o escludere le posizioni eseguendo una delle operazioni seguenti:

- Fai clic con il pulsante destro del mouse sulle richieste che desideri ignorare e seleziona &quot;Ignora&quot;
- Aggiungi manualmente i percorsi da escludere da `\[ *Proxy -\> Recording Settings... -\> Exclude* \]`


## Spoofing DNS {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



Lo spoofing DNS è molto utile quando si tenta di reindirizzare una richiesta a un IP diverso, soprattutto quando si lavora con dispositivi mobili:

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## Mappa remoto {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



Con map remote puoi reindirizzare una richiesta &quot;in arrivo&quot; a un endpoint diverso. Il caso d&#39;uso più comune per questa funzione è quello di &quot;mappare&quot; `AccessEnabler.swf` a `AccessEnablerDebug.swf:`

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## Inverti proxy {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## Dispositivi mobili {#mobile}

### Usa Charles su un dispositivo iOS (iPhone / iPad) {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### Connessione SSL da iPhone {#ssl-connection-from-iphone}

Passa a <http://charlesproxy.com/charles.crt> dal tuo dispositivo iOS.  Verrà avviata la finestra di dialogo per l’installazione del certificato:

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)

</br>

Fare clic su `\[ *Install*... *Install*... *Done* \]` per completare l&#39;installazione del certificato.

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### Utilizzo di Charles da un dispositivo iOS {#using-charles-from-an-ios-device}

Sul tuo dispositivo iOS seleziona `\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]`. Fai clic sulla piccola freccia blu accanto alla rete, quindi vai su Proxy HTTP e seleziona &quot;Manuale&quot;:


</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)


</br>
Qui devi specificare l’IP e la porta del computer su cui stai eseguendo Charles. <span style="line-height: 1.6em;">Se ora apri Safari sul tuo dispositivo iOS e tenti di aprire una pagina web, dovresti visualizzare il seguente messaggio a comparsa sul computer su cui è in esecuzione Charles:

</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)

</br>
Fare clic su "Consenti" per consentire al dispositivo di utilizzare Charles per eseguire il proxy di tutti i
richieste.

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS - Considera attendibili tutti i certificati {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### Errore di autenticazione iOS - impossibile trovare adobepass.ios.app

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Usa Charles per Android

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Passa al [proxy Charles](http://charlesproxy.com/charles.crt) dal tuo dispositivo Android.
