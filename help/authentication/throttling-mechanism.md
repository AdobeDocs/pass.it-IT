---
title: Meccanismo di limitazione
description: Scopri il meccanismo di limitazione utilizzato nell’autenticazione di Adobe Pass. Per una panoramica di questo meccanismo, consulta questa pagina.
exl-id: f00f6c8e-2281-45f3-b592-5bbc004897f7
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 0%

---

# Meccanismo di limitazione {#throttling-mechanism}

Tutti i clienti di Autenticazione di tipo Pass devono essere in grado di accedere all’API di autenticazione di tipo Pass per ciascuno dei loro utenti, in base alle istruzioni e al loro caso aziendale.

L’autenticazione pass introduce un meccanismo di limitazione per garantire un’equa distribuzione delle risorse tra gli utenti dei nostri clienti.

Questo meccanismo è importante per alcuni motivi:

- Una delle regole d&#39;oro dell&#39;architettura multi-tenant è che il comportamento di un utente non dovrebbe influenzare qualcun altro.
- La limitazione della frequenza è importante per le API in quanto è facile commettere errori durante l’integrazione con un’API. Poiché le API sono programmaticamente, è relativamente facile inviare accidentalmente più richieste di quanto previsto.

## Panoramica sul meccanismo {#mechanism-overview}

### Implementazioni client

Pass Authentication fornisce linee guida e SDK per l’interazione con l’API, ma non controlla come il cliente la utilizza. Alcune implementazioni possono avere un’implementazione rudimentale e potrebbero inondare il servizio di richieste API non necessarie, accidentalmente o meno, potrebbe comportare rallentamenti o problemi di capacità per altri utenti.

Il servizio stesso dovrebbe essere in grado di gestire qualsiasi capacità ragionevole. Ma non importa quanto scalabile o performante sia un servizio, ci sono sempre dei limiti. Di conseguenza, il servizio deve disporre di limiti configurati per il numero di chiamate accettate entro un intervallo di tempo specifico.

### Introduzione alla limitazione

L’autenticazione pass si basa sull’identificazione dell’utente e su un algoritmo di limitazione della frequenza dei bucket dei token con valori predefiniti per controllare l’accesso del dispositivo di ciascun utente all’API.

### Meccanismo di identificazione del dispositivo

Il meccanismo di limitazione proposto utilizza i dispositivi identificati singolarmente, con l’aiuto dell’intestazione &quot;X-Forwarded-For&quot;. I limiti vengono applicati nello stesso modo per ogni dispositivo.

### Aggiornamenti richiesti

Le implementazioni server-to-server devono inoltrare gli indirizzi IP dei propri client utilizzando il meccanismo di intestazione &quot;X-Forwarded-For&quot;.

Ulteriori dettagli su come trasmettere l&#39;intestazione X-Forwarded-For [qui](rest-api-cookbook-servertoserver.md).

### Limiti ed endpoint effettivi

Attualmente, il limite predefinito consente un massimo di 1 richiesta al secondo., con un burst iniziale di 3 richieste (una tantum per la prima interazione del client identificato, che dovrebbe consentire il completamento dell’inizializzazione). Questo non dovrebbe influenzare nessun caso di business regolare tra tutti i nostri clienti.

Il meccanismo di limitazione verrà attivato sui seguenti endpoint:

- /o/client/register
- /o/client/token
- /o/client/ambiti
- /o/client/validate
- /api/v2/
- /api/v1/tokens/usermetadata
- /api/v1/tokens/authn
- /api/v1/token/authz
- /api/v1/tokens/media
- /api/v1/config/
- /api/v1/checkauthn
- /api/v1/logout
- /api/v1/authorize
- /api/v1/preauthorize
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/.+/profile-requests/.+
- /api/v1/identities
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### Disambiguazione nell&#39;implementazione dell&#39;SDK

Poiché i client che utilizzano l’autenticazione di Adobe Pass fornita dagli SDK non interagiscono in modo esplicito con alcun endpoint, questa sezione presenta le funzioni note, il loro comportamento quando incontrano una risposta di limitazione e le azioni da intraprendere.

#### setRequestor

Quando viene raggiunto il limite di velocità utilizzando la funzione `setRequestor` dell&#39;SDK, l&#39;SDK restituirà un codice di errore CFG429 tramite callback `errorHandler`.

#### getAuthorization

Quando viene raggiunto il limite di velocità utilizzando la funzione `getAuthorization` dell&#39;SDK, l&#39;SDK restituirà un codice di errore Z100 tramite callback `errorHandler`.

#### checkPreauthorizedResources

Quando viene raggiunto il limite di velocità utilizzando la funzione `checkPreauthorizedResources` dell&#39;SDK, l&#39;SDK restituirà un codice di errore P100 tramite callback `errorHandler`.

#### getMetadata

Quando viene raggiunto il limite di velocità utilizzando la funzione `getMetadata` dell&#39;SDK, l&#39;SDK restituirà una risposta vuota tramite il callback `setMetadataStatus`.

Per ogni dettaglio di implementazione specifico, consulta la documentazione SDK specifica.

- [Riferimento API di JavaScript SDK](javascript-sdk-api-reference.md)
- [Riferimento API dell&#39;SDK per Android](android-sdk-api-reference.md)
- [Riferimento API per iOS/tvOS](iostvos-sdk-api-reference.md)

### Modifiche alla risposta API e risposta

Quando identifichiamo che il limite è stato superato, contrassegneremo questa richiesta con uno stato di risposta specifico (HTTP 429 Troppe richieste), indicando che hai utilizzato tutti i token assegnati al dispositivo utente (indirizzo IP) per l’intervallo di tempo.

La limitazione scade dopo un secondo delle prime 429 risposte. Ogni applicazione che riceve una risposta 429 deve attendere almeno 1 secondo prima di generare una nuova richiesta.

Tutte le applicazioni del cliente devono gestire in modo appropriato la risposta &quot;429 Troppe richieste&quot;.

Ecco un esempio di messaggio di risposta 429:

```
HTTP/2 429
date: Tue, 20 Feb 2024 11:21:53 GMT
content-type: text/html
content-length: 166
set-cookie: AWSALB=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/
set-cookie: AWSALBCORS=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/; SameSite=None; Secure
server: openresty
access-control-allow-credentials: true
access-control-allow-methods: POST,GET,OPTIONS,DELETE
access-control-allow-headers: ap_11,ap_42,ap_z,ap_19,ap_21,ap_23,authorization,content-type,pass_sfp,AP-Session-Identifier,AP-Device-Identifier,AP-SDK-Identifier,X-Device-Info
access-control-expose-headers: pass_sfp,Authzf-Error-Code,Authzf-Sub-Error-Code,Authzf-Error-Details
p3p: CP="NOI DSP COR CURa ADMa DEVa OUR BUS IND UNI COM NAV STA"

<html>
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## Impatto e modifiche necessarie

### Trasmissione dell’intestazione X-Forwarded-For

I clienti che utilizzano un’implementazione personalizzata (inclusi quelli da server a server) per interagire con l’API di autenticazione pass devono assicurarsi di poter acquisire l’indirizzo IP dell’utente e inoltrarlo correttamente, utilizzando l’intestazione X-Forwarded-For anche per Pass Authentication API.

Vedi [qui](rest-api-cookbook-servertoserver.md) per ulteriori dettagli.

### Reazione al nuovo codice di risposta

I clienti che utilizzano un’implementazione personalizzata (incluse quelle da server a server) per interagire con l’API di autenticazione pass devono assicurarsi che qualsiasi chiamata successiva effettuata dopo la ricezione di una richiesta 429 Troppe includa un periodo di attesa minimo di 1 secondo. Questo periodo di attesa garantisce l&#39;opportunità di modificare questo meccanismo e ottenere una risposta aziendale valida.

## Esempio di scenario per la limitazione

| Tempo dalla prima richiesta | Risposta ricevuta | Spiegazione |
|--------------------------|-----------------------------------|----------------------------------------------------------------------------------------------------------|
| Secondo 0 | La chiamata riceve il codice di stato di esito positivo | 1 chiamate consumate dal limite |
| Secondo 0,3 | La chiamata riceve il codice di stato di esito positivo | 1 chiamate consumate dal limite e 1 chiamate contrassegnate come burst |
| Secondo 0,6 | La chiamata riceve il codice di stato di esito positivo | 1 chiamate consumate dal limite e 2 chiamate contrassegnate come burst |
| Secondo 0,9 | La chiamata riceve il codice di stato di esito positivo | 1 chiamate consumate dal limite e 3 chiamate contrassegnate come burst |
| Secondo 1,2 | La chiamata riceve il codice di stato di esito positivo | 2 chiamate consumate dal limite e 3 chiamate contrassegnate come burst |
| Secondo 1,4 | La chiamata riceve il codice di stato 429 | 2 chiamate consumate dal limite e 3 chiamate contrassegnate come burst e 1 chiamata riceve &quot;429 Troppe richieste&quot; |
| Secondo 1,6 | La chiamata riceve il codice di stato 429 | 2 chiamate consumate dal limite e 3 chiamate contrassegnate come burst e 2 chiamate ricevono ‘429 Troppe richieste’ |
| Secondo 1,8 | La chiamata riceve il codice di stato 429 | 2 chiamate consumate dal limite e 3 chiamate contrassegnate come burst e 3 chiamate ricevono &quot;429 Troppe richieste&quot; |
| Secondo 2.1 | La chiamata riceve il codice di stato di esito positivo | 3 chiamate consumate dal limite e 3 chiamate contrassegnate come burst e 3 chiamate ricevono &quot;429 Troppe richieste&quot; |
