---
title: Ottieni token multimediale breve
description: ottieni token multimediale breve
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---

# (Legacy) Ottieni token di file multimediali brevi {#obtain-short-media-token}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!NOTE]
>
> L&#39;implementazione REST API è limitata dal [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Endpoint REST API {#clientless-endpoints}

&lt;FQDN_REGGIE>:

* Produzione - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Gestione temporanea - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produzione - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Gestione temporanea - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## Descrizione {#description}

Ottiene un token multimediale breve.

| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/mediatoken</br></br> o</br></br>&lt;SP_FQDN>/api/v1/tokens/media</br></br>Esempio:</br></br>&lt;SP_FQDN>/api/v1/tokens/media | Servizio programmatore </br></br>o</br></br>app in streaming | 1. richiedente (obbligatorio)</br>2.  deviceId (obbligatorio)</br>3.  resource (obbligatorio)</br>4.  device_info/X-Device-Info (obbligatorio)</br>5.  _tipoDispositivo_</br> 6.  _deviceUser_ (obsoleto)</br>7.  _appId_ (obsoleto) | GET | XML o JSON contenente un token multimediale con codifica Base64 o dettagli dell’errore in caso di esito negativo. | 200 - Operazione riuscita </br>403 - Nessuna operazione riuscita |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| Parametro di input | Descrizione |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| resource | Una stringa che contiene un resourceId (o frammento MRSS), identifica il contenuto richiesto da un utente ed è riconosciuta dagli endpoint di autorizzazione di MVPD. |
| device_info/</br></br>X-Device-Info | Informazioni sul dispositivo di streaming.</br></br>**Nota**: questo parametro POTREBBE essere trasmesso come parametro URL_device, ma a causa delle dimensioni potenziali del parametro e delle limitazioni alla lunghezza di un URL di GET, DOVREBBE essere trasmesso come X-Device-Info nell&#39;intestazione http. </br></br>Visualizza tutti i dettagli in [Trasmissione delle informazioni sul dispositivo e sulla connessione](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _tipoDispositivo_ | Il tipo di dispositivo (ad esempio, Roku, PC).</br></br>Se questo parametro è impostato correttamente, ESM offre metriche [suddivise per tipo di dispositivo]/(help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) quando si utilizza senza client, in modo che possano essere eseguiti diversi tipi di analisi per. Ad esempio, Roku, AppleTV e Xbox.</br></br>Consulta [Vantaggi dell&#39;utilizzo del parametro clientless devicetype ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info sostituirà questo parametro. |
| _utenteDispositivo_ | L’identificatore utente del dispositivo.</br></br>**Nota**: se utilizzato, deviceUser deve avere gli stessi valori della richiesta [Crea codice di registrazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | ID/nome dell’applicazione. </br></br>**Nota**: device_info sostituisce questo parametro. Se utilizzato, `appId` deve avere gli stessi valori della richiesta [Crea codice di registrazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |

{style="table-layout:auto"}

### Risposta di esempio {#response}

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <play>
        <expires>1348649569000</expires>
        <mvpdId>sampleMvpdId</mvpdId>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <serializedToken>PHNpZ25hdH[...]</serializedToken>
        <userId>sampleScrambledUserId</userId>
    </play>
```



**JSON:**

```JSON
    {
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348649614000",
        "serializedToken": "PHNpZ25hdH[...]",
        "userId": "sampleScrambledUserId",
        "mvpdId": "sampleMvpdId"
    }
```



### Compatibilità con Media Verification Library

Il campo `serializedToken` della chiamata &quot;Ottieni token file multimediali brevi&quot; è un token codificato Base64 che può essere verificato in base alla libreria di verifica Adobe Medium.
