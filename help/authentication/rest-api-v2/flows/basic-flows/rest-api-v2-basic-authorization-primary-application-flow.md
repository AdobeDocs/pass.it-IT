---
title: Autorizzazione di base - Applicazione principale - Flusso
description: REST API V2 - Autorizzazione di base - Applicazione principale - Flusso
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 0%

---


# Flusso di autorizzazione di base eseguito nell&#39;applicazione principale {#basic-authorization-flow-performed-within-primary-application}

Il **flusso di autorizzazione** all&#39;interno del diritto di autenticazione di Adobe Pass consente all&#39;applicazione di streaming di determinare se un MVPD consente o nega la richiesta dell&#39;utente di inviare contenuti in streaming. Se la decisione è `Permit`, la risposta include un token multimediale. Il server di Adobe Pass firma il token multimediale e consente all’applicazione di streaming di utilizzare la libreria di verificatori del token multimediale per verificarne l’autenticità prima che il flusso venga rilasciato.

La verifica con la libreria Media Token Verifier deve avvenire sul servizio back-end dell’applicazione di streaming collegato nella catena di autorizzazioni per il rilascio di un flusso dalla rete CDN.

## Recuperare le decisioni di autorizzazione utilizzando mvpd specifico {#retrieve-authorization-decisions-using-specific-mvpd}

### Prerequisiti {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

Prima di recuperare le decisioni di autorizzazione utilizzando un MVPD specifico, assicurati di soddisfare i seguenti prerequisiti:

* L’applicazione di streaming deve disporre di un profilo regolare valido creato correttamente per MVPD utilizzando uno dei flussi di autenticazione di base:
   * [Eseguire l&#39;autenticazione nell&#39;applicazione principale](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Eseguire l&#39;autenticazione nell&#39;applicazione secondaria con mvpd preselezionato](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Eseguire l&#39;autenticazione nell&#39;applicazione secondaria senza mvpd preselezionato](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* L’applicazione di streaming deve recuperare una decisione di autorizzazione prima di riprodurre una risorsa selezionata dall’utente.

### Flusso di lavoro {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

Segui i passaggi forniti per implementare il flusso di autorizzazione di base utilizzando un MVPD specifico eseguito all’interno di un’applicazione primaria, come illustrato nel diagramma seguente.

![Recupera le decisioni di autorizzazione utilizzando mvpd](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png) specifico

*Recupera le decisioni di autorizzazione utilizzando mvpd* specifico

1. **Recupera decisione di autorizzazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per ottenere una decisione di autorizzazione per una risorsa specifica chiamando l&#39;endpoint Decisions Authorize.

   Per informazioni dettagliate su: [Recuperare le decisioni di autorizzazione utilizzando la documentazione API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifica:
   * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

1. **Trova profilo regolare:** Il server Adobe Pass identifica un profilo valido in base ai parametri e alle intestazioni ricevuti.

1. **Recupera decisione MVPD per la risorsa richiesta:** Il server Adobe Pass chiama l&#39;endpoint di autorizzazione MVPD per ottenere una decisione `Permit` o `Deny` per la risorsa specifica ricevuta dall&#39;applicazione di streaming.

1. **Restituisci decisione `Permit` con token multimediale:** La risposta dell&#39;endpoint Decisions Authorize contiene una decisione `Permit` e un token multimediale.

   Per informazioni dettagliate sulle informazioni fornite in una risposta alla decisione, consulta la [documentazione API MVPD](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifica per il recupero delle decisioni di autorizzazione.

   >[!IMPORTANT]
   >
   > L’endpoint Decisions Authorize convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Avvia flusso con token multimediale:** L&#39;applicazione di streaming utilizza il token multimediale per riprodurre il contenuto.

1. **Restituisci decisione `Deny` con dettagli:** La risposta dell&#39;endpoint Decisions Authorize contiene una decisione `Deny` e un payload di errore conformi alla documentazione [Codici di errore migliorati](../../../enhanced-error-codes.md).

   Per informazioni dettagliate sulle informazioni fornite in una risposta alla decisione, consulta la [documentazione API MVPD](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifica per il recupero delle decisioni di autorizzazione.

   >[!IMPORTANT]
   >
   > L’endpoint Decisions Authorize convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Gestire i dettagli della decisione `Deny`:** L&#39;applicazione di streaming elabora le informazioni sull&#39;errore dalla risposta e può utilizzarle per visualizzare facoltativamente un messaggio specifico nell&#39;interfaccia utente.
