---
title: Controlla il flusso di autenticazione tramite l’app web Second Screen
description: Controlla il flusso di autenticazione tramite l’app web Second Screen
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 0%

---

# (Legacy) Verifica il flusso di autenticazione tramite Second Screen Web App {#check-authentication-flow-by-second-screen-web-app}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

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

Questa API deve essere utilizzata dalla seconda app web di accesso schermata per confermare che l’autenticazione Adobe Pass ha confermato il corretto accesso da MVPD. È consigliabile chiamare questa API prima di mostrare un messaggio di successo all’utente finale che gli indica di passare alla console del dispositivo per continuare con i flussi di lavoro.


| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{codice di registrazione} | Accedi all’app web | 1. codice di registrazione </br>    (componente percorso)</br>2.  richiedente </br>    (Obbligatorio) | GET | XML o JSON contenente i dettagli dell’errore in caso di esito negativo. | 200 - Operazione completata   </br>403 - Non consentito |

</br>

| Parametro di input | Descrizione |
| ----------------- | --------------------------------------------------------------------------------------------- |
| codice di registrazione | Il valore del codice di registrazione fornito dall’utente all’inizio del flusso di autenticazione. |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |


### Risposta di esempio (in caso di errore) {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

### [Torna al riferimento API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
