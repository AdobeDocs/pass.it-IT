---
title: Controlla il flusso di autenticazione tramite l’app web Second Screen
description: Controlla il flusso di autenticazione tramite l’app web Second Screen
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# Controlla il flusso di autenticazione tramite l’app web Second Screen {#check-authentication-flow-by-second-screen-web-app}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Endpoint REST API {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrizione {#description}

Questa API deve essere utilizzata dalla seconda app web di accesso schermata per confermare che l’autenticazione Adobe Pass ha confermato il corretto accesso da MVPD. È consigliabile chiamare questa API prima di mostrare un messaggio di successo all’utente finale che gli indica di passare alla console del dispositivo per continuare con i flussi di lavoro.


| Endpoint | Chiamato  </br>Da | Input   </br>Parametri | HTTP  </br>Metodo | Risposta | HTTP  </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{codice di registrazione} | Accedi all’app web | 1. codice di registrazione  </br>    (componente Percorso)</br>2.  richiedente  </br>    (Obbligatorio) | GET | XML o JSON contenente i dettagli dell’errore in caso di esito negativo. | 200 - Operazione completata   </br>403 - Non consentito |

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

### [Torna a Riferimento API REST](/help/authentication/rest-api-reference.md)
