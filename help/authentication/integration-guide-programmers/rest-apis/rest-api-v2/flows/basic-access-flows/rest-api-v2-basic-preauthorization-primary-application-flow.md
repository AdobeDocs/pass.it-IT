---
title: Preautorizzazione di base - Applicazione principale - Flusso
description: REST API V2 - Preautorizzazione di base - Applicazione principale - Flusso
exl-id: f557f6c3-d5b2-4ec8-be51-91a90fbd31c0
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# Flusso di pre-autorizzazione di base eseguito nell’applicazione principale {#basic-preauthorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione REST API V2 è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

Il **flusso di preautorizzazione** all&#39;interno del diritto di autenticazione di Adobe Pass consente all&#39;applicazione di streaming di determinare se un MVPD può consentire o negare l&#39;accesso dell&#39;utente a un elenco di risorse. Questa verifica assicura che l’applicazione possa presentare all’utente informazioni accurate sul contenuto che potrebbe essere idoneo per visualizzare.

## Recuperare le decisioni di pre-autorizzazione utilizzando mvpd specifico {#retrieve-preauthorization-decisions-using-specific-mvpd}

### Prerequisiti {#prerequisites-retrieve-preauthorization-decisions-using-specific-mvpd}

Prima di recuperare le decisioni di preautorizzazione utilizzando un MVPD specifico, assicurati che siano soddisfatti i seguenti prerequisiti:

* L’applicazione di streaming deve disporre di un profilo regolare valido creato correttamente per MVPD utilizzando uno dei flussi di autenticazione di base:
   * [Eseguire l&#39;autenticazione nell&#39;applicazione principale](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Eseguire l&#39;autenticazione nell&#39;applicazione secondaria con mvpd preselezionato](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Eseguire l&#39;autenticazione nell&#39;applicazione secondaria senza mvpd preselezionato](rest-api-v2-basic-authentication-secondary-application-flow.md)
* L’applicazione di streaming desidera recuperare le decisioni di preautorizzazione per visualizzare un elenco di risorse e i relativi stati associati.

### Flusso di lavoro {#workflow-retrieve-preauthorization-decisions-using-specific-mvpd}

Segui i passaggi forniti per implementare il flusso di preautorizzazione di base utilizzando un MVPD specifico eseguito all’interno di un’applicazione principale, come illustrato nel diagramma seguente.

![Recupera le decisioni di preautorizzazione utilizzando mvpd](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-preauthorization-decisions-within-primary-application-using-specific-mvpd.png) specifico

*Recupera le decisioni di preautorizzazione utilizzando mvpd* specifico

1. **Recupera decisioni di preautorizzazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per ottenere le decisioni di preautorizzazione per un elenco di risorse chiamando l&#39;endpoint di preautorizzazione delle decisioni.

   >[!IMPORTANT]
   >
   > Consulta la [Documentazione API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) specifica per il recupero delle decisioni di preautorizzazione, per informazioni dettagliate su:
   >
   > * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   > * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_

1. **Trova profilo regolare:** Il server Adobe Pass identifica un profilo valido in base ai parametri e alle intestazioni ricevuti.

1. **Recupera le decisioni di MVPD per le risorse richieste:** Il server Adobe Pass chiama l&#39;endpoint di preautorizzazione di MVPD per ottenere una decisione `Permit` o `Deny` per ogni risorsa ricevuta dall&#39;applicazione di streaming.

1. **Decisioni di pre-autorizzazione restituite:** La risposta dell&#39;endpoint di preautorizzazione delle decisioni contiene una decisione `Permit` o `Deny` per ogni risorsa:
   * Una decisione `Permit` indica che la risorsa è riproducibile. La risposta non include un token multimediale, poiché il flusso di preautorizzazione non deve essere utilizzato per riprodurre le risorse.
   * Una decisione `Deny` indica che la risorsa non è riproducibile. La risposta include un payload di errore conforme alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta alla decisione, consulta la [Documentazione API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) specifica per il recupero delle decisioni di preautorizzazione.
   > 
   > <br/>
   > 
   > L’endpoint di preautorizzazione delle decisioni convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Gestione delle decisioni di preautorizzazione:** L&#39;applicazione di streaming elabora la risposta e può utilizzarla per visualizzare facoltativamente lo stato appropriato per ogni risorsa nell&#39;interfaccia utente.
