---
title: Meccanismo di limitazione
description: Meccanismo di limitazione
exl-id: 15236570-1a75-42fb-9bba-0e2d7a59c9f6
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 1%

---

# Meccanismo di limitazione {#throttling-mechanism}

## Introduzione {#introduction}

Adobe, in qualità di titolare del trattamento dei dati, deve adottare misure adeguate per garantire che gli utenti dei nostri clienti utilizzino equamente le risorse e che il servizio non sia inondato da richieste API non necessarie. Per questo abbiamo messo in atto un meccanismo di limitazione.
Un&#39;applicazione di monitoraggio della concorrenza può essere utilizzata da più utenti e un utente può avere più sessioni. Pertanto, il servizio avrà limiti configurati per il numero di chiamate accettate per utente/sessione entro un intervallo di tempo specifico.
Una volta raggiunto il limite, le richieste verranno contrassegnate con uno stato di risposta specifico (HTTP 429 - Troppe richieste). Qualsiasi chiamata successiva effettuata dopo aver ricevuto una risposta &quot;429 Troppe richieste&quot; deve essere effettuata con un periodo di raffreddamento di almeno un minuto per garantire che venga ricevuta una risposta aziendale valida.

## Panoramica sul meccanismo {#mechanism-overview}

Il meccanismo determina il numero massimo di chiamate accettate per ogni endpoint di monitoraggio della concorrenza entro un intervallo di tempo specifico.
Una volta raggiunto il numero massimo di chiamate, il nostro servizio risponderà con &quot;429 Troppe richieste&quot;. L’intestazione &quot;Scade&quot; della risposta 429 include il timestamp del momento in cui la chiamata successiva verrà considerata valida o il momento in cui scade la limitazione. Al momento, la limitazione scade dopo una   minuto dalla prima risposta 429.

Gli endpoint configurati con limitazione sono:
1. Crea una nuova sessione: POST /session/{idp}/{subject}
2. Chiamata heartbeat: POST /session/{idp}/{subject}/{sessionId}
3. Termina una sessione: DELETE /session/{idp}/{subject}/{sessionId}

La limitazione è configurata su due livelli:
1. sessione: stesso parametro univoco {sessionId} inviato nella chiamata `Heartbeat` e nella chiamata `Terminate a session`.
2. utente: stesso parametro univoco {subject} inviato nella chiamata `Create a new session`.

Il limite per la limitazione a livello di sessione è impostato su 200 richieste in un minuto.\
Il limite per la limitazione a livello utente è impostato su 200 richieste in un minuto.\
Entrambi questi limiti (limitazione a livello di sessione e limitazione a livello di utente) sono configurabili e verranno aggiornati nel caso in cui vengano raggiunti tramite scenari di integrazione validi. Per questo consigliamo di contattare il team di supporto.

**Scenario per limitazione a livello di sessione:**

| Ora | Invio richiesta a CM | Numero di richieste | Risposta ricevuta da CM | Spiegazione |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Secondo 10 | POST /session/idp1/subject1/session1 | 50 | Tutte le chiamate ricevono ‘202 Accepted’ | 50 chiamate consumate dal limite |
| Secondo 50 | POST /session/idp1/subject1/session1 | 151 | 150 chiamate ricevono &quot;202 Accettate&quot; e 1 chiamata riceve &quot;429 Troppe richieste&quot; | 200 chiamate consumate dal limite e 1 chiamata riceverà una risposta di 429 |
| Secondo 61 | DELETE /session/idp1/subject1/session1 | 1 | 1 chiamata riceve ‘429 Troppe richieste’ | Nessuna chiamata nel limite ancora disponibile |
| Secondo 70 | DELETE /session/idp1/subject1/session1 | 1 | 1 chiamata riceve &quot;202 Accepted&quot; (Accettato) | Limite impostato su 200 chiamate disponibili perché sono trascorsi 60 secondi dal secondo 10 |

**Scenario per limitazione a livello utente:**

| Ora | Invio richiesta a CM | Numero di richieste | Risposta ricevuta da CM | Spiegazione |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Secondo 10 | POST /session/idp1/subject1 | 50 | 50 chiamate ricevono &quot;202 Accettato&quot; | 50 chiamate consumate dal limite |
| Secondo 50 | POST /session/idp1/subject1 | 151 | 150 chiamate ricevono &quot;202 Accettate&quot; e 1 chiamata riceve &quot;429 Troppe richieste&quot; | 200 chiamate consumate dal limite e 1 chiamata riceverà una risposta di 429 |
| Secondo 61 | POST /session/idp1/subject1 | 1 | 1 chiamata riceve ‘429 Troppe richieste’ | Nessuna chiamata nel limite ancora disponibile |
| Secondo 70 | POST /session/idp1/subject1 | 1 | 1 chiamata riceve &quot;202 Accepted&quot; (Accettato) | Limite impostato su 200 chiamate disponibili perché sono trascorsi 60 secondi dal secondo 10 |

**429 Esempio di risposta:**

```
HTTP/2 429
date: Thu, 15 Feb 2024 07:54:20 GMT
content-length: 0
vary: Origin
vary: Access-Control-Request-Method
vary: Access-Control-Request-Headers
cache-control: no-store
expires: Thu, 15 Feb 2024 07:54:41 GMT
strict-transport-security: max-age=31536000 ; includeSubDomains
x-xss-protection: 1; mode=block
x-frame-options: DENY
x-content-type-options: nosniff
```

## Consigli per l’integrazione dei clienti {#customer-integration-recommendations}

Se l’implementazione è corretta, i clienti non riceveranno la risposta &quot;429 Troppe richieste&quot;.
Tuttavia, Adobe consiglia a ogni cliente di gestire in modo appropriato la risposta &quot;429 Troppe richieste&quot; utilizzando i dettagli tecnici presentati in precedenza. Quando si gestisce la risposta, deve essere utilizzata l’intestazione &quot;Scade&quot; per determinare quando inviare la successiva richiesta valida.
