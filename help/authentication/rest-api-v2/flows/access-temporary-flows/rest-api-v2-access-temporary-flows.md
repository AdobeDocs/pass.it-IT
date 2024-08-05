---
title: Flussi di accesso temporanei
description: REST API V2 - Flussi di accesso temporanei
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '3173'
ht-degree: 0%

---


# Flussi di accesso temporanei {#temporary-access-flows}

TempPass consente ai programmatori di fornire accesso temporaneo ai propri contenuti protetti senza richiedere agli utenti di autenticarsi con un account MVPD valido.

Per ulteriori dettagli sulla funzione TempPass, consulta la documentazione di [TempPass](../../../temp-pass.md).

I flussi di accesso temporanei consentono di eseguire query per i seguenti scenari:

* [Recuperare le decisioni di autorizzazione tramite TempPass di base](#retrieve-authorization-decisions-using-basic-temppass)
* [Recupera le decisioni di autorizzazione tramite TempPass promozionale](#retrieve-authorization-decisions-using-promotional-temppass)
* [Utilizzo del numero massimo di risorse tramite TempPass promozionale](#consume-maximum-number-of-resources-using-promotional-temppass)
* [Recupera le decisioni di autorizzazione alla scadenza del TempPass di base o promozionale](#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires)
* [Recupera profilo per TempPass di base](#retrieve-profile-for-basic-temppass)
* [Recupera profilo per TempPass promozionale](#retrieve-profile-for-promotional-temppass)

## Recuperare le decisioni di autorizzazione tramite TempPass di base {#retrieve-authorization-decisions-using-basic-temppass}

### Prerequisiti {#prerequisites-retrieve-authorization-decisions-using-basic-temppass}

Prima di recuperare le decisioni di autorizzazione tramite TempPass di base, verifica che siano soddisfatti i seguenti prerequisiti:

* L’applicazione di streaming vuole fornire un accesso temporaneo per riprodurre i contenuti senza chiedere all’utente di autenticarsi.
* L’applicazione di streaming deve recuperare una decisione di autorizzazione prima di riprodurre una risorsa selezionata dall’utente.

>[!IMPORTANT]
>
> Presupposti
> 
> <br/>
> 
> * È necessario impostare una configurazione valida di TempPass di base applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.
> * Il valore TTL (Time-To-Live) configurato per TempPass di base non è scaduto.

### Flusso di lavoro {#workflow-retrieve-authorization-decisions-using-basic-temppass}

Segui i passaggi forniti per implementare il flusso di autorizzazione utilizzando TempPass di base, come illustrato nel diagramma seguente.

![Recupera le decisioni di autorizzazione tramite TempPass di base](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-retrieve-authorization-decisions-using-basic-temppass-flow.png)

*Recupera le decisioni di autorizzazione tramite TempPass di base*

1. **Recupera decisione di autorizzazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per ottenere una decisione di autorizzazione per una risorsa specifica chiamando l&#39;endpoint Decisions Authorize.

   Per informazioni dettagliate su: [Recuperare le decisioni di autorizzazione utilizzando la documentazione API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifica:
   * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

1. **Convalida TempPass di base:** Il server Adobe Pass verifica se è presente una configurazione valida di TempPass di base applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

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
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > L’endpoint Decisions Authorize utilizza i dati della richiesta per verificare se sono soddisfatte le condizioni di accesso temporanee:
   >
   > * Il valore TTL (Time-To-Live) configurato per TempPass di base non deve essere scaduto.
   >
   > <br/>
   > 
   > Se la convalida dell&#39;accesso temporaneo non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Avvia flusso con token multimediale:** L&#39;applicazione di streaming utilizza il token multimediale per riprodurre il contenuto.

## Recupera le decisioni di autorizzazione tramite TempPass promozionale {#retrieve-authorization-decisions-using-promotional-temppass}

### Prerequisiti {#prerequisites-retrieve-authorization-decisions-using-promotional-temppass}

Prima di recuperare le decisioni di autorizzazione tramite TempPass promozionale, verifica che siano soddisfatti i seguenti prerequisiti:

* L’applicazione di streaming desidera fornire un accesso temporaneo per riprodurre un numero massimo di risorse senza richiedere all’utente di eseguire l’autenticazione.
* L’applicazione di streaming deve includere informazioni univoche sull’identità dell’utente al momento di recuperare una decisione di autorizzazione.
* L’applicazione di streaming deve recuperare una decisione di autorizzazione prima di riprodurre una risorsa selezionata dall’utente.

>[!IMPORTANT]
>
> Presupposti
>
> <br/>
> 
> * È necessario impostare una configurazione valida di Promotion TempPass applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.
> * Il valore TTL (Time-To-Live) configurato per il TempPass promozionale non è scaduto.
> * Non è stato utilizzato il numero massimo di risorse configurate per il TempPass promozionale.

### Flusso di lavoro {#workflow-retrieve-authorization-decisions-using-promotional-temppass}

Segui i passaggi forniti per implementare il flusso di autorizzazione utilizzando il TempPass promozionale come mostrato nel diagramma seguente.

![Recupera le decisioni di autorizzazione tramite TempPass promozionale](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-retrieve-authorization-decisions-using-promotional-temppass-flow.png)

*Recupera le decisioni di autorizzazione tramite TempPass promozionale*

1. **Recupera decisione di autorizzazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per ottenere una decisione di autorizzazione per una risorsa specifica chiamando l&#39;endpoint Decisions Authorize.

   Per informazioni dettagliate su: [Recuperare le decisioni di autorizzazione utilizzando la documentazione API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifica:
   * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

   >[!IMPORTANT]
   >
   > L&#39;endpoint Decisions Authorize richiede la presenza di un&#39;intestazione `AP-TempPass-Identity` quando si utilizza TempPass promozionale. L’intestazione include informazioni univoche sull’identità dell’utente che accede al contenuto.
   > 
   > <br/>
   > 
   > Per ulteriori dettagli sull&#39;intestazione `AP-TempPass-Identity`, consulta la documentazione [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md).

1. **Convalida TempPass promozionale:** Il server Adobe Pass verifica se è presente una configurazione valida di TempPass promozionale applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

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
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > L’endpoint Decisions Authorize utilizza i dati della richiesta per verificare se sono soddisfatte le condizioni di accesso temporanee:
   >
   > * Il valore TTL (Time-To-Live) configurato per il TempPass promozionale non deve essere scaduto.
   > * Non è necessario utilizzare il numero massimo di risorse configurate per il TempPass promozionale.
   >
   > <br/>
   > 
   > Se la convalida dell&#39;accesso temporaneo non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Avvia flusso con token multimediale:** L&#39;applicazione di streaming utilizza il token multimediale per riprodurre il contenuto.

## Utilizzo del numero massimo di risorse tramite TempPass promozionale {#consume-maximum-number-of-resources-using-promotional-temppass}

### Prerequisiti {#prerequisites-consume-maximum-number-of-resources-using-promotional-temppass}

Prima di utilizzare un numero massimo di risorse con TempPass promozionale, verifica che siano soddisfatti i seguenti prerequisiti:

* L’applicazione di streaming desidera fornire un accesso temporaneo per riprodurre un numero massimo di risorse senza richiedere all’utente di eseguire l’autenticazione.
* L’applicazione di streaming deve includere informazioni univoche sull’identità dell’utente al momento di recuperare una decisione di autorizzazione.
* L’applicazione di streaming deve recuperare una decisione di autorizzazione prima di riprodurre una risorsa selezionata dall’utente.

>[!IMPORTANT]
>
> Presupposti
>
> <br/>
> 
> * È necessario impostare una configurazione valida di Promotion TempPass applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.
> * Il valore TTL (Time-To-Live) configurato per il TempPass promozionale non è scaduto.
> * Il numero massimo di risorse configurate per il TempPass promozionale è 1.

### Flusso di lavoro {#workflow-consume-maximum-number-of-resources-using-promotional-temppass}

Segui i passaggi forniti per implementare il flusso di autorizzazione quando utilizzi un numero massimo di risorse utilizzando il TempPass promozionale, come illustrato nel diagramma seguente.

![Consumo massimo di risorse tramite TempPass promozionale](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-consume-maximum-number-of-resources-using-promotional-temppass-flow.png)

*Consumo massimo di risorse tramite TempPass promozionale*

1. **Recupera profilo per TempPass promozionale:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare le informazioni del profilo per TempPass promozionale inviando una richiesta all&#39;endpoint Profili.

   Per informazioni dettagliate su [Recupera profilo per documentazione API mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) specifica, consulta:
   * Tutti i parametri _required_, come `serviceProvider` e `mvpd`
   * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

   >[!IMPORTANT]
   >
   > La query dell’endpoint &quot;Profiles&quot; è facoltativa e può essere utilizzata per determinare quante risorse possono ancora essere riprodotte utilizzando il file TempPass promozionale.

1. **Convalida TempPass promozionale:** Il server Adobe Pass verifica se è presente una configurazione valida di TempPass promozionale applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

1. **Restituire informazioni sul profilo temporaneo:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo temporaneo, incluso l&#39;attributo `type` impostato su &quot;temporary&quot;.

   Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione API [Recupera profilo per mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) specifico.

   >[!IMPORTANT]
   >
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   > 
   > <br/>
   >
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > L’endpoint Profili utilizza i dati della richiesta per verificare se sono soddisfatte le condizioni di accesso temporanee:
   >
   > * Il valore TTL (Time-To-Live) configurato per il TempPass promozionale non deve essere scaduto.
   > * Non è necessario utilizzare il numero massimo di risorse configurate per il TempPass promozionale.
   >
   > <br/>
   > 
   > Se la convalida dell&#39;accesso temporaneo non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Procedi con i flussi di decisioni:** Se la risposta dell&#39;endpoint Profili contiene un profilo, l&#39;applicazione di streaming utilizza le informazioni del profilo temporaneo per continuare con i flussi di decisioni successivi.

1. **Recupera decisione di autorizzazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per ottenere una decisione di autorizzazione per una risorsa specifica chiamando l&#39;endpoint Decisions Authorize.

   Per informazioni dettagliate su: [Recuperare le decisioni di autorizzazione utilizzando la documentazione API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifica:
   * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

   >[!IMPORTANT]
   >
   > L&#39;endpoint Decisions Authorize richiede la presenza di un&#39;intestazione `AP-TempPass-Identity` quando si utilizza TempPass promozionale. L’intestazione include informazioni univoche sull’identità dell’utente che accede al contenuto.
   > 
   > <br/>
   > 
   > Per ulteriori dettagli sull&#39;intestazione `AP-TempPass-Identity`, consulta la documentazione [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md).

1. **Convalida TempPass promozionale:** Il server Adobe Pass verifica se è presente una configurazione valida di TempPass promozionale applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

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
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).
   > 
   > <br/>
   > 
   > L’endpoint Decisions Authorize utilizza i dati della richiesta per verificare se sono soddisfatte le condizioni di accesso temporanee:
   >
   > * Il valore TTL (Time-To-Live) configurato per il TempPass promozionale non deve essere scaduto.
   > * Non è necessario utilizzare il numero massimo di risorse configurate per il TempPass promozionale.
   >
   > <br/>
   > 
   > Se la convalida dell&#39;accesso temporaneo non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Recupera decisione di autorizzazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per ottenere una decisione di autorizzazione per una risorsa specifica chiamando l&#39;endpoint Decisions Authorize.

   Per informazioni dettagliate su: [Recuperare le decisioni di autorizzazione utilizzando la documentazione API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifica:
   * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

   >[!IMPORTANT]
   >
   > L&#39;endpoint Decisions Authorize richiede la presenza di un&#39;intestazione `AP-TempPass-Identity` quando si utilizza TempPass promozionale. L’intestazione include informazioni univoche sull’identità dell’utente che accede al contenuto.
   >
   > <br/>
   > 
   > Per ulteriori dettagli sull&#39;intestazione `AP-TempPass-Identity`, consulta la documentazione [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md).

1. **Convalida TempPass promozionale:** Il server Adobe Pass verifica se è presente una configurazione valida di TempPass promozionale applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

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
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > L’endpoint Decisions Authorize utilizza i dati della richiesta per verificare se sono soddisfatte le condizioni di accesso temporanee:
   >
   > * Il valore TTL (Time-To-Live) configurato per il TempPass promozionale non deve essere scaduto.
   > * Non è necessario utilizzare il numero massimo di risorse configurate per il TempPass promozionale.
   >
   > <br/>
   > 
   > Se la convalida dell&#39;accesso temporaneo non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Gestire i dettagli della decisione `Deny`:** L&#39;applicazione di streaming elabora le informazioni sull&#39;errore dalla risposta e può utilizzarle per visualizzare facoltativamente un messaggio specifico nell&#39;interfaccia utente.

   >[!NOTE]
   >
   > Suggerimento: l’applicazione di streaming può informare gli utenti che è stato superato il numero massimo di risorse e consigliarli di avviare un flusso di autenticazione di base utilizzando un MVPD regolare per continuare a monitorare.

## Recupera le decisioni di autorizzazione alla scadenza del TempPass di base o promozionale {#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

### Prerequisiti {#prerequisites-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

Prima di recuperare le decisioni di autorizzazione alla scadenza di TempPass di base o promozionale, verifica che siano soddisfatti i seguenti prerequisiti:

* [Prerequisiti prima di recuperare le decisioni di autorizzazione tramite TempPass](#prerequisites-retrieve-authorization-decisions-using-basic-temppass) di base.
* [Prerequisiti prima di recuperare le decisioni di autorizzazione tramite TempPass promozionale](#prerequisites-retrieve-authorization-decisions-using-promotional-temppass).

>[!IMPORTANT]
>
> Presupposti
> 
> <br/>
> 
> * È necessario impostare una configurazione valida di TempPass di base o promozionale applicato all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.
> * Il valore TTL (Time-To-Live) configurato per il TempPass di base o promozionale è scaduto.

### Flusso di lavoro {#workflow-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

Segui i passaggi forniti per implementare il flusso di autorizzazione quando il TempPass di base o promozionale scade, come illustrato nel diagramma seguente.

![Recupera le decisioni di autorizzazione alla scadenza del TempPass di base o promozionale](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires-flow.png)

*Recupera le decisioni di autorizzazione alla scadenza del TempPass di base o promozionale*

1. **Recupera decisione di autorizzazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per ottenere una decisione di autorizzazione per una risorsa specifica chiamando l&#39;endpoint Decisions Authorize.

   Per informazioni dettagliate su: [Recuperare le decisioni di autorizzazione utilizzando la documentazione API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifica:
   * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

   >[!IMPORTANT]
   >
   > L&#39;endpoint Decisions Authorize richiede la presenza di un&#39;intestazione `AP-TempPass-Identity` quando si utilizza TempPass promozionale. L’intestazione include informazioni univoche sull’identità dell’utente che accede al contenuto.
   > 
   > <br/>
   > 
   > Per ulteriori dettagli sull&#39;intestazione `AP-TempPass-Identity`, consulta la documentazione [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md).

1. **Convalida TempPass di base o promozionale:** Il server Adobe Pass verifica se è presente una configurazione valida di TempPass di base o promozionale applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

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
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > L’endpoint Decisions Authorize utilizza i dati della richiesta per verificare se sono soddisfatte le condizioni di accesso temporanee:
   >
   > * Il valore TTL (Time-To-Live) configurato per il TempPass di base o promozionale non deve essere scaduto.
   > * Non è necessario utilizzare il numero massimo di risorse configurate per il TempPass promozionale.
   >
   > <br/>
   > 
   > Se la convalida dell&#39;accesso temporaneo non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Gestire i dettagli della decisione `Deny`:** L&#39;applicazione di streaming elabora le informazioni sull&#39;errore dalla risposta e può utilizzarle per visualizzare facoltativamente un messaggio specifico nell&#39;interfaccia utente.

   >[!NOTE]
   >
   > Suggerimento: l’applicazione di streaming può informare gli utenti che l’accesso temporaneo è scaduto e consigliarli di avviare un flusso di autenticazione di base utilizzando un MVPD regolare per continuare a monitorare.

## Recupera profilo per TempPass di base {#retrieve-profile-for-basic-temppass}

>[!IMPORTANT]
>
> La query dell&#39;endpoint Profiles è facoltativa per TempPass di base.

### Prerequisiti {#prerequisites-retrieve-profile-for-basic-temppass}

Prima di recuperare il profilo per TempPass di base, verificare che siano soddisfatti i seguenti prerequisiti:

* L’applicazione di streaming desidera recuperare il profilo temporaneo per garantire che l’accesso temporaneo non sia scaduto.

>[!IMPORTANT]
>
> Presupposti
> 
> <br/>
> 
> * È necessario impostare una configurazione valida di TempPass di base applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.
> * Il valore TTL (Time-To-Live) configurato per TempPass di base non deve essere scaduto.

### Flusso di lavoro {#workflow-retrieve-profile-information-for-basic-temppass}

Segui i passaggi forniti per implementare il flusso di recupero del profilo per TempPass di base, come illustrato nel diagramma seguente.

![Recupera profilo per TempPass di base](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-retrieve-profile-for-basic-temppass-flow.png)

*Recupera profilo per TempPass di base*

1. **Recupera profilo per TempPass di base:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare le informazioni del profilo per TempPass di base inviando una richiesta all&#39;endpoint Profili.

   Per informazioni dettagliate su [Recupera profilo per documentazione API mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) specifica, consulta:
   * Tutti i parametri _required_, come `serviceProvider` e `mvpd`
   * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

1. **Convalida TempPass di base:** Il server Adobe Pass verifica se è presente una configurazione valida di TempPass di base applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

1. **Restituire informazioni sul profilo temporaneo:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo temporaneo, incluso l&#39;attributo `type` impostato su &quot;temporary&quot;.

   Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione API [Recupera profilo per mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) specifico.

   >[!IMPORTANT]
   >
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > L’endpoint Profili utilizza i dati della richiesta per verificare se sono soddisfatte le condizioni di accesso temporanee:
   >
   > * Il valore TTL (Time-To-Live) configurato per TempPass di base non deve essere scaduto.
   >
   > <br/>
   > 
   > Se la convalida dell&#39;accesso temporaneo non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Procedi con i flussi di decisioni:** Se la risposta dell&#39;endpoint Profili contiene un profilo, l&#39;applicazione di streaming utilizza le informazioni del profilo temporaneo per continuare con i flussi di decisioni successivi.

## Recupera profilo per TempPass promozionale {#retrieve-profile-for-promotional-temppass}

>[!IMPORTANT]
>
> La query dell’endpoint Profiles è facoltativa per TempPass promozionale.

### Prerequisiti {#prerequisites-retrieve-profile-for-promotional-temppass}

Prima di recuperare il profilo per TempPass promozionale, accertati che siano soddisfatti i seguenti prerequisiti:

* L’applicazione di streaming desidera recuperare il profilo temporaneo per garantire che l’accesso temporaneo non sia scaduto o per determinare quante risorse possono ancora essere riprodotte.

>[!IMPORTANT]
>
> Presupposti
>
> <br/>
> 
> * È necessario impostare una configurazione valida di Promotion TempPass applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.
> * Il valore TTL (Time-To-Live) configurato per il TempPass promozionale non è scaduto.
> * Non è stato utilizzato il numero massimo di risorse configurate per il TempPass promozionale.

### Flusso di lavoro {#workflow-retrieve-profile-information-for-promotional-temppass}

Segui i passaggi forniti per implementare il flusso di recupero del profilo per il TempPass promozionale, come illustrato nel diagramma seguente.

![Recupera profilo per TempPass promozionale](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-retrieve-profile-for-promotional-temppass-flow.png)

*Recupera profilo per TempPass promozionale*

1. **Recupera profilo per TempPass promozionale:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare le informazioni del profilo per TempPass promozionale inviando una richiesta all&#39;endpoint Profili.

   Per informazioni dettagliate su [Recupera profilo per documentazione API mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) specifica, consulta:
   * Tutti i parametri _required_, come `serviceProvider` e `mvpd`
   * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

1. **Convalida TempPass promozionale:** Il server Adobe Pass verifica se è presente una configurazione valida di TempPass promozionale applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

1. **Restituire informazioni sul profilo temporaneo:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo temporaneo, incluso l&#39;attributo `type` impostato su &quot;temporary&quot;.

   Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione API [Recupera profilo per mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) specifico.

   >[!IMPORTANT]
   >
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > L’endpoint Profili utilizza i dati della richiesta per verificare se sono soddisfatte le condizioni di accesso temporanee:
   >
   > * Il valore TTL (Time-To-Live) configurato per il TempPass promozionale non deve essere scaduto.
   > * Non è necessario utilizzare il numero massimo di risorse configurate per il TempPass promozionale.
   >
   > <br/>
   > 
   > Se la convalida dell&#39;accesso temporaneo non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Procedi con i flussi di decisioni:** Se la risposta dell&#39;endpoint Profili contiene un profilo, l&#39;applicazione di streaming utilizza le informazioni del profilo temporaneo per continuare con i flussi di decisioni successivi.
