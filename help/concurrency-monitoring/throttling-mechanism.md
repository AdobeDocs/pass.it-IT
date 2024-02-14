---
title: Meccanismo di limitazione
description: Meccanismo di limitazione
source-git-commit: 1390c09d3de6b4608cdcc97b3f053cce71e84639
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 1%

---


# Meccanismo di limitazione {#throttling-mechanism}

## Introduzione {#introduction}

Ad Adobe, in qualità di responsabile del trattamento dei dati, deve adottare misure adeguate per garantire che gli utenti dei nostri clienti utilizzino equamente le risorse e che il servizio non sia inondato da richieste API non necessarie. Per questo abbiamo messo in atto un meccanismo di limitazione.\
Un&#39;applicazione di monitoraggio della concorrenza può essere utilizzata da più utenti e un utente può avere più sessioni. Pertanto, il servizio avrà limiti configurati per il numero di chiamate accettate per utente/sessione entro un intervallo di tempo specifico.\
Una volta raggiunto il limite, le richieste verranno contrassegnate con uno stato di risposta specifico (HTTP 429 - Troppe richieste). Qualsiasi chiamata successiva effettuata dopo aver ricevuto una risposta &quot;429 Troppe richieste&quot; deve essere effettuata con un periodo di arresto di almeno 1 minuto per garantire che venga ricevuta una risposta aziendale valida.

## Panoramica sul meccanismo {#mechanism-overview}

Il meccanismo determina il numero massimo di chiamate accettate per ogni endpoint di monitoraggio della concorrenza entro un intervallo di tempo specifico.
Una volta raggiunto il numero massimo di chiamate, il nostro servizio risponderà con &quot;429 Troppe richieste&quot;. Il servizio richiede altri 60 secondi per inizializzare di nuovo il limite al suo valore massimo.

Gli endpoint configurati con limitazione sono:
1. Crea una nuova sessione: POST /session/{idp}/{subject}
2. Chiamata Heartbeat: POST /session/{idp}/{subject}/{sessionId}
3. Termina una sessione: DELETE /session/{idp}/{subject}/{sessionId}

La limitazione è configurata su due livelli:
1. session: same unique {sessionId} parametro inviato `Heartbeat` chiama e `Terminate a session` chiamare.
2. utente: stesso univoco {subject} parametro inviato `Create a new session` chiamare.

Il limite per la limitazione a livello di sessione è impostato su 200 richieste in un minuto.\
Il limite per la limitazione a livello utente è impostato su 200 richieste in un minuto.\
Entrambi questi limiti (limitazione a livello di sessione e limitazione a livello di utente) sono configurabili e verranno aggiornati nel caso in cui vengano raggiunti tramite scenari di integrazione validi. Per questo consigliamo di contattare il team di supporto.

Questo è uno scenario per la limitazione a livello di sessione:

| Ora | Invio richiesta a CM | Numero di richieste | Risposta ricevuta da CM | Spiegazione |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Secondo 10 | POST/session/idp1/subject1/session1 | 50 | Tutte le chiamate ricevono ‘202 Accepted’ | 50 chiamate consumate dal limite |
| Secondo 50 | POST/session/idp1/subject1/session1 | 151 | 150 chiamate ricevono &quot;202 Accettate&quot; e 1 chiamata riceve &quot;429 Troppe richieste&quot; | 200 chiamate consumate dal limite e 1 chiamata riceverà una risposta di 429 |
| Secondo 61 | DELETE/session/idp1/subject1/session1 | 1 | 1 chiamata riceve ‘429 Troppe richieste’ | Nessuna chiamata nel limite ancora disponibile |
| Secondo 70 | DELETE/session/idp1/subject1/session1 | 1 | 1 chiamata riceve &quot;202 Accepted&quot; (Accettato) | Limite impostato su 200 chiamate disponibili perché sono trascorsi 60 secondi dal secondo 10 |

e uno scenario per la limitazione a livello di utente:

| Ora | Invio richiesta a CM | Numero di richieste | Risposta ricevuta da CM | Spiegazione |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Secondo 10 | POST/session/idp1/subject1 | 50 | 50 chiamate ricevono &quot;202 Accettato&quot; | 50 chiamate consumate dal limite |
| Secondo 50 | POST/session/idp1/subject1 | 151 | 150 chiamate ricevono &quot;202 Accettate&quot; e 1 chiamata riceve &quot;429 Troppe richieste&quot; | 200 chiamate consumate dal limite e 1 chiamata riceverà una risposta di 429 |
| Secondo 61 | POST/session/idp1/subject1 | 1 | 1 chiamata riceve ‘429 Troppe richieste’ | Nessuna chiamata nel limite ancora disponibile |
| Secondo 70 | POST/session/idp1/subject1 | 1 | 1 chiamata riceve &quot;202 Accepted&quot; (Accettato) | Limite impostato su 200 chiamate disponibili perché sono trascorsi 60 secondi dal secondo 10 |


## Consigli per l’integrazione dei clienti {#customer-integration-recommendations}

Se l’implementazione è corretta, i clienti non riceveranno la risposta &quot;429 Troppe richieste&quot;.\
Tuttavia, Adobe consiglia a ogni cliente di gestire in modo appropriato la risposta &quot;429 Troppe richieste&quot; utilizzando i dettagli tecnici presentati in precedenza.
