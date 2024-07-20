---
title: Note sulla versione di Adobe Pass Concurrency Monitoring 2.5.0
description: Note sulla versione di Adobe Pass Concurrency Monitoring 2.5.0
exl-id: da392b18-a2aa-4f51-a75f-2c5b65b2b073
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Concurrency Monitoring 2.5.0 {#cm-250}

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

Data di rilascio: 04/21/2016

## Nuove funzioni {#new-features}

L’API v2.0 per il monitoraggio della concorrenza è ora disponibile in produzione.

La versione V2 unifica le chiamate heartbeat e query e semplifica notevolmente l’implementazione quando entrambe queste API vengono utilizzate simultaneamente.



### Ciclo di vita della sessione {#session-lifecycle}

* API di sola scrittura per la creazione, il keep-alive e la conclusione di una sessione, ovvero nessuna più query; la decisione viene restituita sia nelle chiamate di inizio sessione che in quelle heartbeat
* L’intervallo heartbeat è ora gestito dal server tramite le intestazioni Expires impostate sia sulle chiamate di inizio sessione che sulle chiamate keep-alive. Questo consente la configurazione lato server degli intervalli di heartbeat e un valore codificato in meno nelle app.
* Il percorso felice (comportamento conforme sia dell’utente che dell’app) è &quot;pavimentato&quot; con codici di stato 2XX

### Gestione degli errori {#error-handling}

* Le decisioni di rifiuto, i metadati mancanti o il comportamento errato dell’applicazione vengono contrassegnati come risposte 4XX (Conflitto, Richiesta non valida, Non autorizzato, Non disponibile, ecc.)

* Quando ha senso, la risposta include:

   * avviso associato — spiegazione dettagliata dell&#39;errore, da richiedere all&#39;utente.

   * obblighi: azioni obbligatorie che l’applicazione deve intraprendere (ad esempio: aggiornamento dei metadati, disconnessione da Adobe Pass).

### Metadati {#metadata}

* Metadati standard anziché personalizzati: consente all’API di identificare se vengono inviati metadati errati o se mancano i metadati richiesti dalle regole.
* Gli attributi richiesti per ciascuna applicazione ora sono forniti da una chiamata API e devono essere memorizzati nella cache dai client.
Note

>[!NOTE]
>
>La versione V1 continua a essere disponibile. Se i clienti devono utilizzare query al di fuori della chiamata heartbeat, è possibile utilizzare la versione V1.




### Correzioni di bug {#bug-fixes}

N/D

### Problemi noti {#known-issues}

N/D
