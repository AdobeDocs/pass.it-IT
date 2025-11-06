---
title: Recupera elenco di risorse preautorizzate tramite l’app web Second Screen
description: Recupera elenco di risorse preautorizzate tramite l’app web Second Screen
exl-id: 78eeaf24-4cc1-4523-8298-999c9effdb7a
source-git-commit: 1c357b918fa4f6d4b92a9055de018c55ee5861e0
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# (Legacy) Recuperare l’elenco delle risorse preautorizzate tramite l’app web Second Screen {#retrieve-list-of-preauthorized-resources-by-second-screen-web-app}

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

Esistono due set di API: un set per Streaming App o Programmer Service e uno per Second Screen Web App. Questa pagina descrive l’API per l’app AuthN.


| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize/{registration code} | Modulo AuthN | &#x200B;1. codice di registrazione </br>    (componente percorso)</br>2.  richiedente (obbligatorio)</br>3.  resource (obbligatorio) | GET | XML o JSON contenente singole decisioni di pre-autorizzazione o dettagli sull’errore. Vedi gli esempi di seguito. | 200 - Operazione completata</br></br>400 - Richiesta non valida</br></br>401 - Non autorizzato</br></br>405 - Metodo non consentito </br></br>412 - Precondizione non riuscita</br></br>500 - Errore interno del server |



| Parametro di input | Descrizione |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| codice di registrazione | Il valore del codice di registrazione fornito dall’utente all’inizio del flusso di autenticazione. |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| resource | Una stringa che contiene un elenco delimitato da virgole di resourceIds che identifica il contenuto che potrebbe essere accessibile a un utente ed è riconosciuto dagli endpoint di autorizzazione di MVPD. |


### Risposta di esempio {#sample-response}

**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4`
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
    <resource>
</resources>
```

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
