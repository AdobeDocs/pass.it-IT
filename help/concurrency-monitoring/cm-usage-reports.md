---
title: Report utilizzo monitoraggio concorrenza
description: Report utilizzo monitoraggio concorrenza
exl-id: 20220436-e748-4b22-8e7c-e074e0bfe242
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '757'
ht-degree: 0%

---

# Report utilizzo monitoraggio concorrenza {#cm-usage-reports}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.



## Panoramica {#usage-rep-overview}

Il servizio **Report sull&#39;utilizzo del monitoraggio della concorrenza** è disponibile tramite un&#39;API REST che fornisce informazioni approfondite sull&#39;utilizzo concorrente, come riportato dalle applicazioni del cliente.

## Prerequisiti {#usage-rep-prerequisites}

Per accedere al prodotto Report sull&#39;utilizzo del monitoraggio della concorrenza, un cliente deve prima contattare il [Team di supporto](mailto:tve-support@adobe.com) per il monitoraggio della concorrenza ed eseguire i passaggi necessari per consentire l&#39;accesso al prodotto API.

## Metriche e raggruppamenti generali dei rapporti {#general-rep-metrics-breakdown}

### I report sull’utilizzo possono monitorare le metriche seguenti:{#monitor-metrics}

| Nome | Descrizione |
|:---|:---|
| utenti attivi | numero di account utente distinti che hanno avviato almeno una sessione di streaming durante l’intervallo, suddivisi per granularità temporale |
| sessioni attive | numero di sessioni di streaming che hanno segnalato l’attività durante l’intervallo (non include i tentativi non riusciti, le sessioni che hanno ricevuto una risposta di negazione per la chiamata di inizializzazione) |
| started-session | numero di sessioni di streaming avviate nell&#39;intervallo |
| sessioni completate | numero di sessioni di streaming completate durante l’intervallo (esplicitamente o a causa di timeout) |
| tentativi falliti | numero di tentativi di inizializzazione della sessione che hanno ricevuto una risposta di negazione |
| dismiss-session | numero di sessioni di streaming terminate durante la riproduzione (su heartbeat) a causa di una violazione dei criteri. Questa metrica è significativa solo se viene emesso un criterio FIFO o last_one_wins. |
| sessioni terminate | numero di sessioni di streaming esplicitamente terminate all’avvio di una nuova sessione (tramite intestazione X-Terminate) |
| duration_0-15 | numero di flussi con una durata compresa tra 0 e 15 minuti |
| duration_15-30 | numero di flussi con una durata compresa tra 15 e 30 minuti |
| duration_30-60 | numero di flussi con una durata compresa tra 30 e 60 minuti |
| duration_60-120 | numero di flussi con una durata compresa tra 60 e 120 minuti |
| duration_2h-4h | numero di flussi con una durata compresa tra 2 ore (120 minuti) e 4 ore |
| duration_4h-8h | numero di flussi con una durata compresa tra 4 e 8 ore |
| duration_8h-16h | numero di flussi con una durata compresa tra 8 e 16 ore |
| duration_16h-1d | numero di flussi con una durata compresa tra 16 ore e 1 giorno (24 ore) |
| duration_1d-3d | numero di flussi con una durata compresa tra 1 giorno e 3 giorni |
| duration_3d-7d | numero di flussi con una durata compresa tra 3 giorni e 7 giorni |
| duration_1w-1m | numero di flussi con una durata compresa tra 1 settimana (7 giorni) e 1 mese (30 giorni) |
| duration_over-1m | numero di flussi con una durata superiore a 1 mese (30 giorni) |

### I report sull’utilizzo possono filtrare le metriche elencate in precedenza in base alle seguenti dimensioni: {#dimensions-2-filter-metrics}

| Nome Dimension | Descrizione |
|:---|:---|
| anno | Anno a 4 cifre |
| mese | Il mese dell&#39;anno (1-12) |
| giorno | Il giorno del mese (1-31) |
| ora | L&#39;ora del giorno |
| minuto | Il minuto dell&#39;ora |
| applicazione | Nome dell&#39;applicazione registrato in Monitoraggio concorrenza utilizzato per gestire le sessioni |
| application-id | ID applicazione registrato in Monitoraggio concorrenza utilizzato per gestire le sessioni |
| channel | Metadati del canale inviati durante l’inizializzazione della sessione (contrassegnati come Sconosciuti se non vengono inviati metadati) |
| mvpd | MVPD fornito durante la gestione della sessione |
| piattaforma | Metadati della piattaforma forniti all&#39;inizializzazione della sessione o predefiniti per un&#39;applicazione a livello di configurazione |

## Metriche e raggruppamenti dei rapporti sulla concorrenza {#concurrency-reports-metrics-breakdown}

A partire dalla versione 2.9.0 del monitoraggio della concorrenza è stato introdotto un nuovo rapporto per comprendere l&#39;utilizzo simultaneo: un istogramma per **livello di concorrenza** e **livello attività**.

Lo scopo principale di questo rapporto è quello di aiutarti a comprendere l’impatto dell’impostazione di una policy con un determinato limite di concorrenza e di fornirti informazioni sufficienti per decidere se aumentare il limite.

### I report sull’utilizzo possono monitorare le metriche seguenti: {#metrics-usage-rep-users}

| Nome Dimension | Descrizione |
|:---|:---|
| utenti | numero di utenti che hanno raggiunto ogni livello di concorrenza/attività |

### I report sull’utilizzo possono filtrare le metriche elencate in precedenza in base alle seguenti dimensioni: {#dimensions-to-filter-metrics}

| Nome Dimension | Descrizione |
|:---|:---|
| anno | Anno a 4 cifre |
| mese | Il mese dell&#39;anno (1-12) |
| giorno | Il giorno del mese (1-31) |
| a livello di concorrenza | Rappresenta qualsiasi attività di flusso **distinta approvata durante la fase di inizializzazione della sessione** per un utente per poter osservare quanti flussi simultanei **sono stati aperti** da un utente e capire l&#39;impatto dell&#39;applicazione di un determinato limite di concorrenza |
| livello di attività | Rappresenta qualsiasi **attività di flusso distinta (indipendentemente dal suo stato: avviato, attivo, arrestato, rifiutato)** per un utente per poter osservare quanti flussi simultanei sono stati tentati di essere aperti da un utente e capire l&#39;impatto dell&#39;applicazione di un determinato limite di concorrenza |
| mvpd | MVPD fornito durante la gestione della sessione |
