---
title: Recupera elenco risorse autorizzate
description: Recupera elenco risorse autorizzate
exl-id: 3821378c-bab5-4dc9-abd7-328df4b60cc3
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 0%

---

# (Legacy) Recuperare l’elenco delle risorse pre-autorizzate {#retrieve-list-of-preauthorized-resources}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

>[!NOTE]
>
> L&#39;implementazione REST API è limitata dal [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Endpoint REST API {#clientless-endpoints}

&lt;FQDN_REGGIE>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrizione {#description}

Una richiesta all’autenticazione di Adobe Pass per ottenere l’elenco delle risorse preautorizzate.

Esistono due set di API: un set per Streaming App o Programmer Service e uno per Second Screen Web App. Questa pagina descrive l’API per Streaming App o Programmer Service.


| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize | Servizio programmatore </br></br>o</br></br>app in streaming | &#x200B;1. richiedente (obbligatorio)</br>2.  deviceId (obbligatorio)</br>3.  resource (obbligatorio)</br>4.  device_info/X-Device-Info (obbligatorio)</br>5.  _tipoDispositivo_</br> 6.  _deviceUser_ (obsoleto)</br>7.  _appId_ (obsoleto) | GET | XML o JSON contenente singole decisioni di pre-autorizzazione o dettagli sull’errore. Vedi gli esempi di seguito. | 200 - Operazione completata</br></br>400 - Richiesta non valida</br></br>401 - Non autorizzato</br></br>405 - Metodo non consentito </br></br>412 - Precondizione non riuscita</br></br>500 - Errore interno del server |


| Parametro di input | Descrizione |
| --- | --- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| resource | Una stringa che contiene un elenco delimitato da virgole di resourceIds che identifica il contenuto che potrebbe essere accessibile a un utente ed è riconosciuto dagli endpoint di autorizzazione di MVPD. |
| device_info/</br></br>X-Device-Info | Informazioni sul dispositivo di streaming.</br></br>**Nota**: questo parametro POTREBBE essere trasmesso come parametro URL_device, ma a causa delle dimensioni potenziali del parametro e delle limitazioni alla lunghezza di un URL GET, DOVREBBE essere trasmesso come X-Device-Info nell&#39;intestazione http. </br></br>Visualizza tutti i dettagli in [Trasmissione delle informazioni sul dispositivo e sulla connessione](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _tipoDispositivo_ | Il tipo di dispositivo (ad esempio, Roku, PC).</br></br>Se questo parametro è impostato correttamente, ESM offre metriche [suddivise per tipo di dispositivo](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) quando si utilizza senza client, in modo che possano essere eseguiti diversi tipi di analisi, ad esempio Roku, AppleTV e Xbox.</br></br>Vedi, [vantaggi dell&#39;utilizzo del parametro del tipo di dispositivo senza client nelle metriche di passaggio ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: `device_info` sostituirà questo parametro. |
| _utenteDispositivo_ | L’identificatore utente del dispositivo. |
| _appId_ | ID/nome dell’applicazione. </br></br>**Nota**: device_info sostituisce questo parametro. |



### Risposta di esempio {#sample-response}



**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
  <resource>
    <id>TestStream1</id>
    <authorized>true</authorized>
  </resource>
  <resource>
    <id>TestStream2</id>
    <authorized>false</authorized>
    <error>
      <status>403</status>
      <code>authorization_denied_by_mvpd</code>
      <message>User not authorized</message>
      <details>Your subscription package does not include the "TestStream3" channel.</details>
      <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
      <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
      <action>none</action>
    </error>
  </resource>
</resources>
```

</br>

**JSON:**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
