---
title: Single Sign-On - Token di servizio - Flussi
description: REST API V2 - Single Sign-On - Token di servizio - Flussi
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '1837'
ht-degree: 0%

---


# Single sign-on con flussi di token di servizio{#single-sign-on-service-token-full-flows}

>[!NOTE]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Il metodo Service Token consente a più applicazioni di utilizzare un identificatore utente univoco per ottenere un Single Sign-On (SSO) su più dispositivi e piattaforme quando si utilizzano i servizi Adobe Pass.

Le applicazioni sono responsabili del recupero del payload dell’identificatore utente univoco utilizzando servizi di identità esterni, al di fuori dei sistemi Adobe Pass, ad esempio:

* Un servizio DTC (Direct-to-Consumer), in cui gli utenti accedono a ogni dispositivo utilizzando le stesse credenziali e sono associati allo stesso ID utente o nome account utente.
* Servizio di autenticazione di terze parti, ad esempio Google o Facebook, in cui gli utenti accedono a ciascun dispositivo utilizzando le stesse credenziali e sono associati allo stesso indirizzo e-mail.

Le applicazioni sono responsabili dell&#39;inclusione di questo payload di identificatore utente univoco nell&#39;intestazione `AD-Service-Token` per tutte le richieste che lo specificano.

Per ulteriori dettagli sull&#39;intestazione `AD-Service-Token`, consulta la documentazione [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

## Eseguire l&#39;autenticazione tramite Single Sign-On utilizzando il token di servizio {#performing-authentication-flow-using-service-token-single-sign-on-method}

### Prerequisiti {#prerequisites-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Prima di eseguire il flusso di autenticazione tramite Single Sign-On utilizzando un token di servizio, verificare che siano soddisfatti i seguenti prerequisiti:

* Il servizio Identity esterno deve restituire informazioni coerenti come payload `JWS` in tutte le applicazioni su più dispositivi e piattaforme.
* La prima applicazione di streaming deve recuperare l&#39;identificatore utente univoco e includere il payload `JWS` come parte dell&#39;intestazione [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) per tutte le richieste che lo specificano.
* La prima applicazione di streaming deve selezionare un MVPD.
* La prima applicazione di streaming deve avviare una sessione di autenticazione per accedere con l&#39;MVPD selezionato.
* La prima applicazione di streaming deve effettuare l’autenticazione con l’MVPD selezionato in un agente utente.
* La seconda applicazione di streaming deve recuperare l&#39;identificatore utente univoco e includere il payload `JWS` come parte dell&#39;intestazione [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) per tutte le richieste che lo specificano.

>[!IMPORTANT]
>
> Presupposti
> 
> <br/>
> 
> * La prima applicazione di streaming supporta l’interazione dell’utente per selezionare un MVPD.
> * La prima applicazione di streaming supporta l’interazione dell’utente per l’autenticazione con l’MVPD selezionato in un agente utente.

### Flusso di lavoro {#workflow-steps-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Per implementare il flusso di autenticazione tramite single sign-on, esegui i passaggi indicati utilizzando un token di servizio, come illustrato nel diagramma seguente.

![Eseguire l&#39;autenticazione tramite Single Sign-On utilizzando il token di servizio](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-service-token-flow.png)

*Eseguire l&#39;autenticazione tramite Single Sign-On utilizzando il token di servizio*

1. **Autentica con il servizio Identity:** La prima applicazione di streaming chiama il servizio Identity, al di fuori dei sistemi Adobe Pass, per ottenere il payload `JWS` associato all&#39;identificatore utente univoco.

1. **Restituire un identificatore utente univoco come JWS:** La prima applicazione di streaming convalida i dati di risposta per garantire il rispetto delle condizioni di sicurezza di base:
   * Payload non scaduto.
   * Il payload è firmato.

1. **Crea sessione di autenticazione:** La prima applicazione di streaming raccoglie tutti i dati necessari per avviare una sessione di autenticazione chiamando l&#39;endpoint Sessions.

   Per informazioni dettagliate su [Crea sessione di autenticazione](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md), consulta la documentazione API di:
   * Tutti i parametri _required_, come `serviceProvider`, `mvpd`, `domainName` e `redirectUrl`
   * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

   >[!IMPORTANT]
   >
   > Prima di effettuare una richiesta, l’applicazione di streaming deve verificare di includere un valore valido per l’identificatore utente univoco.
   >
   > <br/>
   > 
   > Per ulteriori dettagli sull&#39;intestazione `AD-Service-Token`, consulta la documentazione [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Indicare l&#39;azione successiva:** La risposta dell&#39;endpoint Sessions contiene i dati necessari per guidare la prima applicazione di streaming per quanto riguarda l&#39;azione successiva.

   Per informazioni dettagliate sulle informazioni fornite in una risposta di sessione, consulta la documentazione API [Crea sessione di autenticazione](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

   >[!IMPORTANT]
   >
   > L’endpoint Sessions convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Apri URL nell&#39;agente utente:** La risposta dell&#39;endpoint Sessions contiene i dati seguenti:
   * `url` che può essere utilizzato per avviare l&#39;autenticazione interattiva nella pagina di accesso di MVPD.
   * L&#39;attributo `actionName` è impostato per l&#39;autenticazione.
   * L&#39;attributo `actionType` è impostato su &quot;interactive&quot;.

   Se il backend di Adobe Pass non identifica un profilo valido, la prima applicazione di streaming apre un agente utente per caricare l&#39;elemento `url` fornito, effettuando una richiesta all&#39;endpoint Authenticate. Questo flusso può includere diversi reindirizzamenti, che portano l’utente alla pagina di accesso MVPD e forniscono credenziali valide.

1. **Autenticazione MVPD completa:** Se il flusso di autenticazione ha esito positivo, l&#39;interazione dell&#39;agente utente salva un profilo regolare nel backend di Adobe Pass e raggiunge il `redirectUrl` fornito.

1. **Recupera profilo per codice specifico:** La prima applicazione di streaming raccoglie tutti i dati necessari per recuperare le informazioni sul profilo inviando una richiesta all&#39;endpoint Profili.

   Per informazioni dettagliate su [Recupera profilo per codice specifico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md), consulta la documentazione API per:
   * Tutti i parametri _required_, come `serviceProvider`, `code`
   * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

   >[!NOTE]
   >
   > Suggerimento: l&#39;applicazione di streaming può attendere che l&#39;agente utente raggiunga il `redirectUrl` fornito per verificare se il profilo regolare è stato generato e salvato correttamente.

1. **Trova profilo regolare:** Il server Adobe Pass identifica un profilo valido in base ai parametri e alle intestazioni ricevuti.

1. **Restituisci informazioni sul profilo regolare:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo trovato associato ai parametri e alle intestazioni ricevuti.

   Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione API [Recupera profilo per codice specifico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md).

   >[!IMPORTANT]
   >
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Procedi con i flussi di decisioni:** La prima applicazione di streaming può continuare con i flussi di decisioni successivi.

   >[!IMPORTANT]
   >
   > Prima di effettuare una richiesta, l’applicazione di streaming deve verificare di includere un valore valido per l’identificatore utente univoco.
   >
   > <br/>
   > 
   > Per ulteriori dettagli sull&#39;intestazione `AD-Service-Token`, consulta la documentazione [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Autentica con il servizio Identity:** La seconda applicazione di streaming chiama il servizio Identity, al di fuori dei sistemi Adobe Pass, per ottenere il payload `JWS` associato all&#39;identificatore utente univoco.

1. **Restituire un identificatore utente univoco come JWS:** La seconda applicazione di streaming convalida i dati di risposta per garantire il rispetto delle condizioni di sicurezza di base:
   * Payload non scaduto.
   * Il payload è firmato.

1. **Recupera profili:** La seconda applicazione di streaming raccoglie tutti i dati necessari per recuperare tutte le informazioni sui profili inviando una richiesta all&#39;endpoint Profili.

   Per informazioni dettagliate su [Recuperare i profili](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md), consulta la documentazione API di:
   * Tutti i parametri _required_, come `serviceProvider`
   * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

   >[!IMPORTANT]
   >
   > Prima di effettuare una richiesta, l’applicazione di streaming deve verificare di includere un valore valido per l’identificatore utente univoco.
   >
   > <br/>
   > 
   > Per ulteriori dettagli sull&#39;intestazione `AD-Service-Token`, consulta la documentazione [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Trova profilo Single Sign-On:** Il server Adobe Pass identifica un profilo Single Sign-On valido in base ai parametri e alle intestazioni ricevuti.

1. **Restituire informazioni sul profilo Single Sign-On:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo trovato associato ai parametri e alle intestazioni ricevuti.

   Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione dell&#39;API [Recupera profili](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md).

   >[!IMPORTANT]
   >
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Procedi con i flussi di decisioni:** La seconda applicazione di streaming può continuare con i flussi di decisioni successivi.

   >[!IMPORTANT]
   >
   > Prima di effettuare una richiesta, l’applicazione di streaming deve verificare di includere un valore valido per l’identificatore utente univoco.
   >
   > <br/>
   > 
   > Per ulteriori dettagli sull&#39;intestazione `AD-Service-Token`, consulta la documentazione [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

## Recuperare le decisioni di autorizzazione tramite Single Sign-On utilizzando il token di servizio {#performing-authorization-flow-using-service-token-single-sign-on-method}

### Prerequisiti {#prerequisites-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Prima di eseguire il flusso di autorizzazione tramite Single Sign-On utilizzando un token di servizio, verificare che siano soddisfatti i seguenti prerequisiti:

* Il servizio Identity esterno deve restituire informazioni coerenti come payload `JWS` in tutte le applicazioni su più dispositivi e piattaforme.
* La prima applicazione di streaming deve recuperare l&#39;identificatore utente univoco e includere il payload `JWS` come parte dell&#39;intestazione [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) per tutte le richieste che lo specificano.
* La seconda applicazione di streaming deve recuperare una decisione di autorizzazione prima di riprodurre una risorsa selezionata dall’utente.

>[!IMPORTANT]
>
> Presupposti
>
> <br/>
> 
> * La prima applicazione di streaming ha eseguito l&#39;autenticazione e ha incluso un valore valido per l&#39;intestazione di richiesta [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

### Flusso di lavoro {#workflow-steps-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Eseguire i passaggi specificati per implementare il flusso di autorizzazione tramite Single Sign-On utilizzando un token di servizio come illustrato nel diagramma seguente.

![Recuperare le decisioni di autorizzazione tramite Single Sign-On utilizzando il token di servizio](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-service-token-flow.png)

*Recuperare le decisioni di autorizzazione tramite Single Sign-On utilizzando il token di servizio*

1. **Autentica con il servizio Identity:** La seconda applicazione di streaming chiama il servizio Identity, al di fuori dei sistemi Adobe Pass, per ottenere il payload `JWS` associato all&#39;identificatore utente univoco.

1. **Restituire un identificatore utente univoco come JWS:** La seconda applicazione di streaming convalida i dati di risposta per garantire il rispetto delle condizioni di sicurezza di base:
   * Payload non scaduto.
   * Il payload è firmato.

1. **Recupera decisione di autorizzazione:** La seconda applicazione di streaming raccoglie tutti i dati necessari per ottenere una decisione di autorizzazione per una risorsa specifica chiamando l&#39;endpoint Decisions Authorize.

   Per informazioni dettagliate su: [Recuperare le decisioni di autorizzazione utilizzando la documentazione API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifica:
   * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

   >[!IMPORTANT]
   >
   > Prima di effettuare una richiesta, l’applicazione di streaming deve verificare di includere un valore valido per l’identificatore utente univoco.
   >
   > <br/>
   > 
   > Per ulteriori dettagli sull&#39;intestazione `AD-Service-Token`, consulta la documentazione [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Trova profilo Single Sign-On:** Il server Adobe Pass identifica un profilo Single Sign-On valido in base ai parametri e alle intestazioni ricevuti.

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

1. **Avvia flusso con token multimediale:** La seconda applicazione di streaming utilizza il token multimediale per riprodurre il contenuto.

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

1. **Gestire i dettagli della decisione `Deny`:** La seconda applicazione di streaming elabora le informazioni sull&#39;errore dalla risposta e può utilizzarle per visualizzare facoltativamente un messaggio specifico nell&#39;interfaccia utente.

>[!NOTE]
>
> I passaggi per il flusso di preautorizzazione sono gli stessi del flusso di autorizzazione, tranne per il fatto che l&#39;endpoint utilizzato è quello descritto nella [Documentazione di recupero delle decisioni di preautorizzazione utilizzando mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) specifico.
