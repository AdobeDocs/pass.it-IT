---
title: API di registrazione client dinamica
description: API di registrazione client dinamica
exl-id: 06a76c71-bb19-4115-84bc-3d86ebcb60f3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# API di registrazione client dinamica {#dynamic-client-registration-api}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

Attualmente, l’autenticazione Adobe Pass identifica e registra le applicazioni in due modi:

* i client basati su browser sono registrati tramite consentiti [elenco domini](/help/authentication/programmer-overview.md)
* i client delle applicazioni native, come le applicazioni iOS e Android, vengono registrati tramite il meccanismo del richiedente firmato.

Adobe Pass Authentication propone un nuovo meccanismo per la registrazione delle applicazioni. Questo meccanismo è descritto nei paragrafi seguenti.

## Il meccanismo di registrazione delle domande {#appRegistrationMechanism}

### Motivi tecnici {#reasons}

Il meccanismo di autenticazione nell’autenticazione di Adobe Pass si basava sui cookie di sessione, ma a causa di [Schede personalizzate Android Chrome](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} and [Apple Safari View Controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}, questo obiettivo non può più essere raggiunto.

Date queste limitazioni, Adobe introduce un nuovo meccanismo di registrazione per tutti i suoi clienti. Si basa sulla RFC di OAuth 2.0 ed è costituito dai seguenti passaggi:

1. Recupero dell&#39;istruzione software da TVE Dashboard
1. Ottenimento delle credenziali del client
1. Ottenimento del token di accesso

### Recupero del rendiconto software dal dashboard TVE {#softwareStatement}

Per ogni applicazione rilasciata, è necessario ottenere un rendiconto software. Tutte le istruzioni software vengono fornite tramite la dashboard TVE, una volta create le applicazioni. L&#39;istruzione software deve essere distribuita insieme all&#39;applicazione sul dispositivo dell&#39;utente.

>[!IMPORTANT]
>
>Quando si utilizza un&#39;istruzione software, il meccanismo dell&#39;ID richiedente firmato non sarà più necessario.

Per ulteriori dettagli su come creare istruzioni software, visitare il sito Web all&#39;indirizzo [Registrazione client nel dashboard TVE](/help/authentication/dynamic-client-registration.md).

### Ottenimento delle credenziali client {#clientCredentials}

Dopo aver recuperato un&#39;istruzione software da TVE Dashboard, è necessario registrare l&#39;applicazione con il server di autorizzazione Adobe Pass. A tale scopo, esegui una chiamata /register e recupera l’identificatore client univoco.

**Richiesta**

| Chiamata HTTP |                    |
|-----------|--------------------|
| percorso | /o/client/register |
| metodo | POST |

| campi |                                                                           |           |
|--------------------|---------------------------------------------------------------------------|-----------|
| software_statement | L&#39;istruzione software creata in TVE Dashboard. | obbligatorio |
| redirect_uri | URI utilizzato dall&#39;applicazione per completare il flusso di autenticazione. | facoltativo |

| intestazioni di richiesta |                                                                                |           |
|-----------------|--------------------------------------------------------------------------------|-----------|
| Content-Type | application/json | obbligatorio |
| X-Device-Info | Le informazioni sul dispositivo definite in Passing Device and Connection Information | obbligatorio |
| User-Agent | L’agente utente | obbligatorio |

**Risposta**

| intestazioni di risposta |                  |           |
|------------------|------------------|-----------|
| Content-Type | application/json | obbligatorio |

| campi di risposta |                 |                            |
|---------------------|-----------------|----------------------------|
| client_id | Stringa | obbligatorio |
| client_secret | Stringa | obbligatorio |
| client_id_issue_at | long | obbligatorio |
| redirect_uris | elenco di stringhe | obbligatorio |
| grant_types | elenco di stringhe<br/> **valore accettato**<br/> `client_credentials`: utilizzato da client non sicuri, ad esempio SDK per Android. | obbligatorio |
| errore | **valori accettati**<ul><li>invalid_request</li><li>invalid_redirect_uri</li><li>invalid_software_statement</li><li>unauthorized_software_statement</li></ul> | obbligatorio in un flusso di errore |


#### Risposta di errore {#error-response}

In caso di errore, il server di registrazione risponde con un codice di stato HTTP 400 (Richiesta non valida) e include i seguenti parametri nella risposta:

| codice di stato | corpo della risposta | descrizione |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | Nella richiesta manca un parametro obbligatorio, include un valore di parametro non supportato, ripete un parametro o il formato non è corretto. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_redirect_uri&quot;} | Redirect_uri non consentito per questo client in base alla sua applicazione registrata. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_software_statement&quot;} | Dichiarazione software non valida. |
| HTTP 400 | {&quot;error&quot;: &quot;unapprove_software_statement&quot;} | Software_id non trovato nella configurazione. |

#### Esempio di credenziali client {#client-credentials-example}

**Richiesta:**

```HTTPS
POST /o/client/register HTTP/1.1
    User-Agent: Android
    X-Device-Info: ew0KICAibW9kZWwiOiAiVFYiLA0KICAidmVuZG9yIjogIkFwcGxlIiwNCiAgIm1hbnVmYWN0dXJlciI6ICJBcHBsZSIsDQogICJvc05hbWUiOiAidHZPUyIsDQogICJvc1ZlbmRvciI6ICJBcHBsZSIsDQogICJvc1ZlcnNpb24iOiAiMTAuMiIsDQogICJicm93c2VyVmVuZG9yIjogIkFwcGxlIiwNCiAgImJyb3dzZXJOYW1lIjogIlNhZmFyaSINCn0
    Content-Type: application/json
    Accept: application/json
    Host: sp.auth.adobe.com
 {
        "software_statement": "eyJhbGciOiJSUzI1NiJ9.
    eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
    bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
    aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
    GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
    zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
    5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
    fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
    U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
    IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
  "redirect_uri": "adobepass://com.programmer"  }
```

**Risposta:**

```HTTPS
HTTP/1.1 201 Created
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
    
{
 "client_id": "s6BhdRkqt3",
 "client_secret": "t7AkePiru4",
 "client_id_issued_at": 2893256800,
 "redirect_uris": [
   "app://com.programmer.adobe#sdasdsadas"],
 "grant_types": ["client_credentials"]
}
```

**Risposta di errore:**

```HTTPS
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

### Ottenimento del token di accesso {#accessToken}

Dopo aver recuperato l’identificatore client univoco (ID client e segreto client) per l’applicazione, devi ottenere un token di accesso. Il token di accesso è un token OAuth 2.0 obbligatorio, utilizzato per chiamare le API di autenticazione di Adobe Pass.

>[!NOTE]
>
>Attualmente, i token di accesso hanno un tempo di durata di 24 ore.

**Richiesta**


| **Chiamata HTTP** | |
| --- | --- |
| percorso | `/o/client/token` |
| metodo | POST |

| **parametri di richiesta** | |
| --- | --- |
| `grant_type` | Ricevuto nel processo di registrazione del client.<br/> **Valore accettato**<br/>`client_credentials`: utilizzato per client non sicuri, come l’SDK per Android. |
| `client_id` | Identificatore client ottenuto nel processo di registrazione client. |
| `client_secret` | Identificatore client ottenuto nel processo di registrazione client. |

**Risposta**

| campi di risposta | | |
| --- | --- | --- |
| `access_token` | Il valore del token di accesso da utilizzare per chiamare le API di Adobe Pass | obbligatorio |
| `expires_in` | Il tempo in secondi che deve trascorrere prima della scadenza del token di accesso | obbligatorio |
| `token_type` | Tipo del token **portatore** | obbligatorio |
| `created_at` | Ora di emissione del token | obbligatorio |
| **intestazioni di risposta** | | |
| `Content-Type` | application/json | obbligatorio |

**Risposta di errore**

In caso di errore, il server autorizzazioni risponde con un codice di stato HTTP 400 (Richiesta non valida) e include i seguenti parametri nella risposta:

| codice di stato | corpo della risposta | descrizione |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | Nella richiesta manca un parametro obbligatorio, include un valore di parametro non supportato (diverso dal tipo di concessione), ripete un parametro, include più credenziali, utilizza più meccanismi per l’autenticazione del client o ha un formato non valido. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_client&quot;} | Autenticazione client non riuscita. Client sconosciuto. L’SDK DEVE registrarsi nuovamente con il server autorizzazioni. |
| HTTP 400 | {&quot;error&quot;: &quot;unauthorized_client&quot;} | Il client autenticato non è autorizzato a utilizzare questo tipo di concessione di autorizzazione. |

#### Esempio di ottenimento del token di accesso: {#obt-access-token}

**Richiesta:**

```HTTPS
POST o/client/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
    
grant_type=client_credentials&client_id=AAA&client_secret=SSS
```

**Risposta:**

```JSON
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"bearer",
  "expires_in":3600,
  "created_at":123456789
}
```

**Risposta di errore:**

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

## Esecuzione di richieste di autenticazione {#autheticationRequests}

Utilizza il token di accesso per eseguire Adobe Pass [Chiamate API di autenticazione](/help/authentication/initiate-authentication.md). A questo scopo, è necessario aggiungere il token di accesso alla richiesta API in uno dei seguenti modi:

* aggiungendo un nuovo parametro di query alla richiesta. Questo nuovo parametro viene chiamato **access_token**.

* aggiungendo una nuova intestazione HTTP alla richiesta: Authorization: Bearer. È consigliabile utilizzare l’intestazione HTTP, in quanto le stringhe di query tendono a essere visibili nei registri del server.

In caso di errore, è possibile restituire le risposte di errore seguenti:

| Risposte di errore |     |                                                                                                        |
|-----------------|-----|--------------------------------------------------------------------------------------------------------|
| invalid_request | 400 | Richiesta non valida. |
| invalid_client | 403 | L’ID client non è più autorizzato a eseguire richieste. È necessario generare nuove credenziali client. |
| accesso negato | 401 | Access_token non valido (scaduto o non valido). Il client DEVE richiedere un nuovo access_token. |

### Esempi di esecuzione di richieste di autenticazione:

**Invio del token di accesso come parametro della richiesta:**

```HTTPS
GET adobe-services/config?access_token=<access_token>&requestor_id=... HTTP/1.1
    
Host: sp.auth.adobe.com
```

**Invio del token di accesso come intestazione HTTP:**

```HTTPS
POST adobe-services/sessionDevice?device_id=platformDeviceId HTTP/1.1
    
Authorization: Bearer <access_token>
    
Host: sp.auth.adobe.com
```

**Risposte di errore come corpo della risposta:**

```HTTPS
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error":"invalid_client" }
```

**Risposte di errore come parametri URL:**

```HTTPS
HTTP/1.1 302 Found
    
Location: adobepass://com.programmer.adobe?error=access_denied
```
