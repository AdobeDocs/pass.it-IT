---
title: Controlla token di autenticazione
description: Controlla token di autenticazione
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Controlla token di autenticazione {#check-authentication-token}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

>[!NOTE]
>
> L&#39;implementazione REST API è limitata dal [meccanismo di limitazione](/help/authentication/throttling-mechanism.md)

## Endpoint REST API {#clientless-endpoints}

&lt;FQDN_REGGIE>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrizione {#description}

Indica se il dispositivo dispone di un token di autenticazione non scaduto.

| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/checkauthn | Servizio programmatore </br></br>o</br></br>app in streaming | 1. richiedente (obbligatorio)</br>2.  deviceId (obbligatorio)</br>3.  device_info/X-Device-Info (obbligatorio)</br>4.  _tipoDispositivo_ </br>5.  _deviceUser_ (obsoleto)</br>6.  _appId_ (obsoleto) | GET | XML o JSON contenente i dettagli dell’errore in caso di esito negativo. | 200 - Operazione completata   </br>403 - Nessun successo |

{style="table-layout:auto"}


| Parametro di input | Descrizione |
| --- | --- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| device_info/</br></br>X-Device-Info | Informazioni sul dispositivo di streaming.</br></br>**Nota**: questo valore PUÒ essere trasmesso device_info come parametro URL, ma a causa delle dimensioni potenziali di questo parametro e delle limitazioni alla lunghezza di un URL di GET, DEVE essere trasmesso come X-Device-Info nell&#39;intestazione http. </br></br><!--See the full details in [Passing Device and Connection Information](/help/authentication/passing-client-information-device-connection-and-application.md)(/help/authentication/passing-client-information-device-connection-and-application.md)-->. |
| _tipoDispositivo_ | Il tipo di dispositivo (ad esempio, Roku, PC).</br></br>Se questo parametro è impostato correttamente, ESM offre metriche [suddivise per tipo di dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) quando si utilizza senza client, in modo che possano essere eseguiti diversi tipi di analisi, ad esempio per Roku, AppleTV, Xbox ecc.</br></br>Per ulteriori dettagli, vedere [Vantaggi dell&#39;utilizzo del parametro deviceType senza client nelle metriche di autenticazione di Adobe Pass ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Nota**: il parametro device_info verrà sostituito. |
| _utenteDispositivo_ | L’identificatore utente del dispositivo. |
| _appId_ | ID/nome dell’applicazione.</br>**Nota**: device_info sostituisce questo parametro. |

{style="table-layout:auto"}


## Risposta (in caso di esito negativo) {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [Torna al riferimento API REST](/help/authentication/rest-api-reference.md)
