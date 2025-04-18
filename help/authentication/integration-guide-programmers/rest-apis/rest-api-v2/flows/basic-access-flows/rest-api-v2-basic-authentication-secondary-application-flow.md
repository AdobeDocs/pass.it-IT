---
title: Autenticazione di base - Applicazione secondaria - Flusso
description: REST API V2 - Autenticazione di base - Applicazione secondaria - Flusso
exl-id: 83bf592e-c679-4cfe-984d-710a9598c620
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2006'
ht-degree: 0%

---

# Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria {#basic-authentication-flow-performed-within-secondary-application}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione REST API V2 è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Visita anche le [Domande frequenti su REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

Il **flusso di autenticazione** all&#39;interno del diritto di autenticazione di Adobe Pass consente all&#39;applicazione di streaming di verificare che un utente disponga di un account MVPD valido. Questo processo richiede che l’utente disponga di un account MVPD attivo e immetta credenziali di accesso valide nella pagina di accesso di MVPD.

Il flusso di autenticazione è necessario nei seguenti casi:

* Quando l’utente apre un’applicazione per la prima volta.
* Quando l’autenticazione precedente dell’utente è scaduta.
* Quando l’utente si disconnette dall’account MVPD.
* Quando l’utente desidera eseguire l’autenticazione con un altro MVPD.

In tutti questi casi, l’applicazione che chiama uno qualsiasi degli endpoint &quot;Profiles&quot; riceve una risposta vuota o uno o più profili, ma per MVPD diversi.

Il **flusso di autenticazione** richiede che un agente utente (browser) completi una serie di chiamate dall&#39;applicazione al backend di Adobe Pass, quindi alla pagina di accesso di MVPD e infine all&#39;applicazione. Questo flusso può includere diversi reindirizzamenti ai sistemi MVPD e la gestione di cookie o sessioni archiviate per ciascun dominio, che può essere difficile da raggiungere e proteggere senza un agente utente.

In base alle funzionalità dell’applicazione principale (applicazione di streaming) per supportare l’interazione dell’utente per selezionare un MVPD e per l’autenticazione con il MVPD selezionato in un agente utente, gli scenari di autenticazione sono:

* [Eseguire l&#39;autenticazione nell&#39;applicazione principale](rest-api-v2-basic-authentication-primary-application-flow.md)
* [Eseguire l&#39;autenticazione nell&#39;applicazione secondaria con mvpd preselezionato](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Eseguire l&#39;autenticazione nell&#39;applicazione secondaria senza mvpd preselezionato](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## Eseguire l&#39;autenticazione nell&#39;applicazione secondaria con mvpd preselezionato {#perform-authentication-within-secondary-application-with-preselected-mvpd}

### Prerequisiti {#prerequisites-perform-authentication-within-secondary-application-with-preselected-mvpd}

Prima di avviare il flusso di autenticazione all’interno di un’applicazione primaria e di completarlo tramite l’interazione dell’utente all’interno di un’applicazione secondaria, assicurati di soddisfare i seguenti prerequisiti:

* L&#39;applicazione di streaming deve selezionare un MVPD.
* L&#39;applicazione di streaming deve avviare una sessione di autenticazione per accedere con il MVPD selezionato.
* L’applicazione secondaria deve eseguire l’autenticazione con il MVPD selezionato in un agente utente.

>[!IMPORTANT]
>
> Presupposti
>
> <br/>
> 
> * L’applicazione di streaming supporta l’interazione dell’utente per selezionare un MVPD.
> * L’applicazione secondaria (in genere su un dispositivo secondario) supporta l’interazione dell’utente per l’autenticazione con il MVPD selezionato in un agente utente.

### Flusso di lavoro {#workflow-perform-authentication-within-secondary-application-with-preselected-mvpd}

Segui i passaggi forniti per implementare il flusso di autenticazione di base eseguito all’interno di un’applicazione secondaria con un MVPD preselezionato, come illustrato nel diagramma seguente.

![Eseguire l&#39;autenticazione nell&#39;applicazione secondaria con mvpd preselezionato](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-with-preselected-mvpd.png)

*Eseguire l&#39;autenticazione nell&#39;applicazione secondaria con mvpd preselezionato*

1. **Crea sessione di autenticazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per avviare una sessione di autenticazione chiamando l&#39;endpoint Sessions.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Crea sessione di autenticazione](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md), consulta la documentazione API di:
   > 
   > * Tutti i parametri _required_, come `serviceProvider`, `mvpd`, `domainName` e `redirectUrl`
   > * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_
   >
   > <br/>
   > 
   > Durante la creazione della sessione di autenticazione, l’applicazione di streaming deve fornire tutti i parametri richiesti in una singola chiamata.

1. **Indicare l&#39;azione successiva:** La risposta dell&#39;endpoint Sessions contiene i dati necessari per guidare l&#39;applicazione di streaming per quanto riguarda l&#39;azione successiva.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta di sessione, consulta la documentazione API [Crea sessione di autenticazione](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).
   > 
   > <br/>
   > 
   > L’endpoint Sessions convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Procedi con i flussi di decisioni:** La risposta dell&#39;endpoint Sessions contiene i dati seguenti:
   * L&#39;attributo `actionName` è impostato su &quot;authorize&quot;.
   * L&#39;attributo `actionType` è impostato su &quot;direct&quot;.

   Se il backend di Adobe Pass identifica un profilo valido, non è necessario che l’applicazione di streaming autentichi nuovamente con il MVPD selezionato, in quanto esiste già un profilo che può essere utilizzato per i flussi decisionali successivi.

1. **Visualizza codice di autenticazione:** La risposta dell&#39;endpoint Sessions contiene i dati seguenti:
   * `code` che può essere utilizzato per riprendere la sessione di autenticazione in un&#39;applicazione secondaria.
   * L&#39;attributo `actionName` è impostato per l&#39;autenticazione.
   * L&#39;attributo `actionType` è impostato su &quot;interactive&quot;.

   Se il backend di Adobe Pass non identifica un profilo valido, l&#39;applicazione di streaming visualizza `code` che può essere utilizzato per riprendere la sessione di autenticazione all&#39;interno di un&#39;applicazione secondaria.

1. **Convalida codice di autenticazione:** L&#39;applicazione secondaria convalida l&#39;utente fornito `code` per assicurarsi che possa procedere con l&#39;autenticazione di MVPD nell&#39;agente utente.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recuperare le informazioni sulla sessione di autenticazione](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md), consulta la documentazione API di:
   >
   > * Tutti i parametri _required_, come `serviceProvider` e `code`
   > * Tutte le intestazioni _required_, come `Authorization`
   > * Tutti i parametri e le intestazioni _optional_

1. **Informazioni restituite sulla sessione di autenticazione:** La risposta dell&#39;endpoint Sessions contiene i dati seguenti:
   * L&#39;attributo `existing` contiene i parametri esistenti già specificati.
   * L&#39;attributo `missing` contiene i parametri mancanti che devono essere forniti per completare il flusso di autenticazione.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta di convalida della sessione, consultare la documentazione API [Recupera informazioni sulla sessione di autenticazione](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).
   >
   > <br/>
   >
   > L’endpoint Sessions convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!TIP]
   >
   > L&#39;applicazione secondaria può informare gli utenti che `code` utilizzato non è valido nel caso in cui una risposta di errore indichi una sessione di autenticazione mancante e consigliare loro di riprovare a utilizzarne una nuova.

1. **Apri URL nell&#39;agente utente:** L&#39;applicazione secondaria apre un agente utente per caricare `url` calcolato automaticamente, effettuando una richiesta all&#39;endpoint Authenticate. Questo flusso può includere diversi reindirizzamenti, che portano l’utente alla pagina di accesso di MVPD e forniscono credenziali valide.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Eseguire l&#39;autenticazione nella documentazione API dell&#39;agente utente](../../apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md):
   >
   > * Tutti i parametri _required_, come `serviceProvider` e `code`
   > * Tutti i parametri e le intestazioni _optional_

1. **Autenticazione MVPD completa:** se il flusso di autenticazione ha esito positivo, l&#39;interazione dell&#39;agente utente salva un profilo regolare nel backend di Adobe Pass e raggiunge il `redirectUrl` fornito.

1. **Recupera profilo per codice specifico:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare le informazioni sul profilo inviando una richiesta all&#39;endpoint Profili.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recupera profilo per codice specifico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md), consulta la documentazione API per:
   > 
   > * Tutti i parametri _required_, come `serviceProvider` e `code`
   > * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_

   >[!TIP]
   >
   > L&#39;applicazione di streaming deve implementare un meccanismo di polling utilizzando `code` per verificare se il profilo regolare è stato generato e salvato correttamente.

1. **Restituisci informazioni sul profilo regolare:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo regolare associato ai parametri e alle intestazioni ricevuti.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione API [Recupera profilo per codice specifico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).
   > 
   > <br/>
   > 
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).

## Eseguire l&#39;autenticazione nell&#39;applicazione secondaria senza mvpd preselezionato {#perform-authentication-within-secondary-application-without-preselected-mvpd}

### Prerequisiti {#prerequisites-perform-authentication-within-secondary-application-without-preselected-mvpd}

Prima di avviare il flusso di autenticazione all’interno di un’applicazione primaria e di completarlo tramite l’interazione dell’utente all’interno di un’applicazione secondaria, assicurati di soddisfare i seguenti prerequisiti:

* L&#39;applicazione di streaming deve avviare una sessione di autenticazione quando deve accedere.
* L&#39;applicazione secondaria deve selezionare un MVPD.
* L’applicazione secondaria deve eseguire l’autenticazione con il MVPD selezionato in un agente utente.

>[!IMPORTANT]
>
> Presupposti
>
> <br/>
> 
> * L’applicazione secondaria (in genere su un dispositivo secondario) supporta l’interazione dell’utente per selezionare un MVPD.
> * L’applicazione secondaria (in genere su un dispositivo secondario) supporta l’interazione dell’utente per l’autenticazione con il MVPD selezionato in un agente utente.

### Flusso di lavoro {#workflow-perform-authentication-within-secondary-application-without-preselected-mvpd}

Segui i passaggi forniti per implementare il flusso di autenticazione di base eseguito all’interno di un’applicazione secondaria senza un MVPD preselezionato, come illustrato nel diagramma seguente.

![Eseguire l&#39;autenticazione nell&#39;applicazione secondaria senza mvpd preselezionato](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-without-preselected-mvpd.png)

*Eseguire l&#39;autenticazione nell&#39;applicazione secondaria senza mvpd preselezionato*

1. **Crea sessione di autenticazione:** L&#39;applicazione di streaming raccoglie alcuni dei dati necessari per avviare una sessione di autenticazione chiamando l&#39;endpoint Sessions.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Crea sessione di autenticazione](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md), consulta la documentazione API di:
   >
   > * Tutti i parametri _required_, come `serviceProvider`
   > * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_
   >
   > <br/>
   > 
   > L’applicazione di streaming non è in grado di fornire tutti i parametri richiesti in una singola chiamata durante la creazione della sessione di autenticazione.

1. **Indicare l&#39;azione successiva:** La risposta dell&#39;endpoint Sessions contiene i dati necessari per guidare l&#39;applicazione di streaming per quanto riguarda l&#39;azione successiva:
   * `code` che può essere utilizzato per riprendere la sessione di autenticazione in un&#39;applicazione secondaria.
   * L&#39;attributo `actionName` è impostato su &quot;resume&quot;.
   * L&#39;attributo `actionType` è impostato su &quot;direct&quot;.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta di sessione, consulta la documentazione API [Crea sessione di autenticazione](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).
   > 
   > <br/>
   > 
   > L’endpoint Sessions convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Visualizza codice di autenticazione:** L&#39;applicazione di streaming visualizza l&#39;elemento `code` che può essere utilizzato per riprendere la sessione di autenticazione all&#39;interno di un&#39;applicazione secondaria.

1. **Specificare parametri mancanti per la sessione di autenticazione:** L&#39;applicazione secondaria raccoglie tutti i dati mancanti necessari per riprendere la sessione di autenticazione e chiama l&#39;endpoint Sessions.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Riprendi sessione di autenticazione](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md), consulta la documentazione API di:
   >
   > * Tutti i parametri _required_, come `serviceProvider`, `mvpd`, `domainName` e `redirectUrl`
   > * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_

1. **Indicare l&#39;azione successiva:** La risposta dell&#39;endpoint Sessions contiene i dati necessari per guidare l&#39;applicazione di streaming per quanto riguarda l&#39;azione successiva.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta di sessione, consultare la documentazione API [Riprendi sessione di autenticazione](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md).
   > 
   > <br/>
   > 
   > L’endpoint Sessions convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!TIP]
   >
   > L&#39;applicazione secondaria può informare gli utenti che `code` utilizzato non è valido nel caso in cui una risposta di errore indichi una sessione di autenticazione mancante e consigliare loro di riprovare a utilizzarne una nuova.

1. **Indicare il profilo esistente:** La risposta dell&#39;endpoint Sessions contiene i dati seguenti:
   * L&#39;attributo `actionName` è impostato su &quot;authorize&quot;.
   * L&#39;attributo `actionType` è impostato su &quot;direct&quot;.

   Se il backend di Adobe Pass identifica un profilo valido, non è necessario che l’applicazione di streaming autentichi nuovamente con il MVPD selezionato, in quanto esiste già un profilo che può essere utilizzato per i flussi decisionali successivi.

1. **Apri URL nell&#39;agente utente:** La risposta dell&#39;endpoint Sessions contiene i dati seguenti:
   * `url` che può essere utilizzato per avviare l&#39;autenticazione interattiva nella pagina di accesso di MVPD.
   * L&#39;attributo `actionName` è impostato per l&#39;autenticazione.
   * L&#39;attributo `actionType` è impostato su &quot;interactive&quot;.

   Se il backend di Adobe Pass non identifica un profilo valido, l&#39;applicazione secondaria apre un agente utente per caricare l&#39;elemento `url` fornito, effettuando una richiesta all&#39;endpoint Authenticate. Questo flusso può includere diversi reindirizzamenti, che portano l’utente alla pagina di accesso di MVPD e forniscono credenziali valide.

1. **Autenticazione MVPD completa:** se il flusso di autenticazione ha esito positivo, l&#39;interazione dell&#39;agente utente salva un profilo regolare nel backend di Adobe Pass e raggiunge il `redirectUrl` fornito.

1. **Recupera profilo per codice specifico:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare le informazioni sul profilo inviando una richiesta all&#39;endpoint Profili.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recupera profilo per codice specifico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md), consulta la documentazione API per:
   >
   > * Tutti i parametri _required_, come `serviceProvider` e `code`
   > * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_

   >[!TIP]
   >
   > L&#39;applicazione di streaming deve implementare un meccanismo di polling utilizzando `code` per verificare se il profilo regolare è stato generato e salvato correttamente.

1. **Restituisci informazioni sul profilo regolare:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo regolare associato ai parametri e alle intestazioni ricevuti.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione API [Recupera profilo per codice specifico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).
   > 
   > <br/>
   > 
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).
