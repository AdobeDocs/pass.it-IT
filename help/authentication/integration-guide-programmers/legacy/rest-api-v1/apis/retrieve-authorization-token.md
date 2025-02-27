---
title: Recupera token di autorizzazione
description: Recupera token di autorizzazione
exl-id: 0b010958-efa8-4dd9-b11b-5d10f51f5680
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 0%

---

# (Legacy) Recupera token di autorizzazione {#retrieve-authorization-token}

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

Recupera il token di autorizzazione (AuthZ).


| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authz</br></br>Esempio:</br></br>&lt;SP_FQDN>/api/v1/tokens/authz | Servizio programmatore </br></br>o</br></br>app in streaming | 1. richiedente (obbligatorio)</br>2.  deviceId (obbligatorio)</br>3.  resource (obbligatorio)</br>4.  device_info/X-Device-Info (obbligatorio)</br>5.  _tipoDispositivo_</br> 6.  _deviceUser_ (obsoleto)</br>7.  _appId_ (obsoleto) | GET | 1. Operazione completata</br>2.  Token di autenticazione </br>    non trovato o scaduto:   </br>    XML che spiega il motivo </br>    token di autenticazione non trovato</br>3.  Token di autorizzazione </br>    non trovato: </br>    spiegazione XML</br>4.  Token di autorizzazione </br>    scaduto: </br>    Spiegazione XML | 200 - Operazione completata </br>412 - Nessuna AuthN</br></br>404 - Nessuna AuthZ</br></br>410 - AuthZ scaduta |

{style="table-layout:auto"}

</br>

| Parametro di input | Descrizione |
| --- | --- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| resource | Una stringa che contiene un resourceId (o frammento MRSS), identifica il contenuto richiesto da un utente ed è riconosciuta dagli endpoint di autorizzazione di MVPD. |
| device_info/</br></br>X-Device-Info | Informazioni sul dispositivo di streaming.</br></br>**Nota**: questo parametro POTREBBE essere trasmesso come parametro URL_device, ma a causa delle dimensioni potenziali del parametro e delle limitazioni alla lunghezza di un URL di GET, DOVREBBE essere trasmesso come X-Device-Info nell&#39;intestazione http. </br></br>Visualizza tutti i dettagli in [Trasmissione delle informazioni sul dispositivo e sulla connessione](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _tipoDispositivo_ | Il tipo di dispositivo (ad esempio, Roku, PC).</br></br>Se questo parametro è impostato correttamente, ESM offre metriche [suddivise per tipo di dispositivo](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) quando si utilizza senza client, in modo che possano essere eseguiti diversi tipi di analisi, ad esempio Roku, AppleTV e Xbox.</br></br>Consulta, [Vantaggi dell&#39;utilizzo del parametro del tipo di dispositivo senza client nelle metriche di passaggio ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: il parametro device_info verrà sostituito. |
| _utenteDispositivo_ | L’identificatore utente del dispositivo. |
| _appId_ | ID/nome dell’applicazione. </br></br>**Nota**: device_info sostituisce questo parametro. |

{style="table-layout:auto"}


### Risposta di esempio {#response}



#### Completato

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authorization>
        <expires>1348148289000</expires>
        <mvpd>sampleMvpdId</mvpd>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <proxyMvpd>sampleProxyMvpdId</proxyMvpd>
    </authorization>
```



**JSON:**

```JSON
    {
        "mvpd": "sampleMvpdId",
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348148289000",
        "proxyMvpd": "sampleProxyMvpdId"
    }
```

</br>


#### Token di autenticazione non trovato o scaduto:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>412</status>
        <message>User not authenticated</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 412,
        "message": "User not authenticated",
        "details": null
    }
```

</br>


#### Token di autorizzazione non trovato:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>404</status>
        <message>Not found</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 404,
        "message": "Not Found",
        "details": null
    }
```

</br>



#### Token di autorizzazione scaduto:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>410</status>
        <message>Gone</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 410,
        "message": "Gone",
        "details": null
    }
```
