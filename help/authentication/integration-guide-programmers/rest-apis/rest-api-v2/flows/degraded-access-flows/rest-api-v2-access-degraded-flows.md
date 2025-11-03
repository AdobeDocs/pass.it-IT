---
title: Flussi di accesso danneggiati
description: REST API V2 - Flussi di accesso danneggiati
exl-id: 9276f5d9-8b1a-4282-8458-0c1e1e06bcf5
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '1615'
ht-degree: 0%

---

# Flussi di accesso degradati {#degraded-access-flows}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione REST API V2 è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Visita anche le [Domande frequenti su REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

Degradazione consente di bypassare temporaneamente endpoint specifici di autenticazione e autorizzazione di MVPD. Di solito, il programmatore avvia questa azione, ma indipendentemente da chi attiva un evento di degrado, l’azione dipende da accordi precedenti conclusi con gli MVPD interessati.

Per ulteriori dettagli sulla funzione di degradazione, consulta la documentazione di [degradazione](/help/premium-workflow/degraded-access/degradation-feature.md).

I flussi di accesso danneggiati consentono di eseguire query per i seguenti scenari:

* [Eseguire l&#39;autenticazione durante l&#39;applicazione della degradazione](#perform-authentication-while-degradation-is-applied)
* [Recupera le decisioni di autorizzazione durante l&#39;applicazione della riduzione del livello di priorità](#retrieve-authorization-decisions-while-degradation-is-applied)
* [Recupera le decisioni di pre-autorizzazione durante l’applicazione della degradazione](#retrieve-preauthorization-decisions-while-degradation-is-applied)
* [Recupera il profilo durante l&#39;applicazione della degradazione](#retrieve-profile-while-degradation-is-applied)

## Eseguire l&#39;autenticazione durante l&#39;applicazione della degradazione {#perform-authentication-while-degradation-is-applied}

### Prerequisiti {#prerequisites-perform-authentication-while-degradation-is-applied}

Prima di eseguire il flusso di autenticazione durante l’applicazione della degradazione, verifica che siano soddisfatti i seguenti prerequisiti:

* L&#39;applicazione di streaming deve avviare una sessione di autenticazione quando deve accedere con MVPD.

>[!IMPORTANT]
> 
> Presupposti
> 
> <br/>
> 
> * L’applicazione di streaming non dispone di un profilo valido per quel MVPD specifico salvato nel backend di Adobe Pass.
> * Regola di degradazione AuthNAll applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

### Flusso di lavoro {#workflow-perform-authentication-while-degradation-is-applied}

Segui i passaggi forniti per implementare il flusso di autenticazione mentre viene applicata la degradazione, come illustrato nel diagramma seguente.

![Eseguire l&#39;autenticazione durante l&#39;applicazione della degradazione](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-perform-authentication-while-degradation-is-applied-flow.png)

*Eseguire l&#39;autenticazione durante l&#39;applicazione della degradazione*

1. **Crea sessione di autenticazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per avviare una sessione di autenticazione chiamando l&#39;endpoint Sessions.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Crea sessione di autenticazione](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md), consulta la documentazione API di:
   > 
   > * Tutti i parametri _required_, come `serviceProvider`, `mvpd`, `domainName` e `redirectUrl`
   > * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_

1. **Verifica regole di degradazione:** Il server Adobe Pass verifica se è stata applicata una regola di degradazione AuthNAll all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

1. **Indicare l&#39;azione successiva:** La risposta dell&#39;endpoint Sessions contiene i dati necessari per guidare l&#39;applicazione di streaming per quanto riguarda l&#39;azione successiva:
   * L&#39;attributo `actionName` è impostato su &quot;authorize&quot;.
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
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   > 
   > L’endpoint Sessions utilizza i dati della richiesta per verificare se sono soddisfatte condizioni di accesso degradate:
   >
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve avere una regola di degradazione AuthNAll applicata.
   >
   > <br/>
   > 
   > Se la convalida dell’accesso degradato non riesce, la risposta utilizza per impostazione predefinita il flusso di autenticazione di base.

1. **Procedi con i flussi di decisioni:** L&#39;applicazione di streaming può continuare con i flussi di decisioni successivi.

## Recupera le decisioni di autorizzazione durante l&#39;applicazione della riduzione del livello di priorità {#retrieve-authorization-decisions-while-degradation-is-applied}

### Prerequisiti {#prerequisites-retrieve-authorization-decisions-while-degradation-is-applied}

Prima di recuperare le decisioni di autorizzazione durante l’applicazione della degradazione, assicurati di soddisfare i seguenti prerequisiti:

* L’applicazione di streaming deve recuperare una decisione di autorizzazione prima di riprodurre una risorsa selezionata dall’utente.

>[!IMPORTANT]
>
> Presupposti
> 
> <br/>
> 
> * L’applicazione di streaming non dispone di un profilo valido per quel MVPD specifico.
> * Regola di degradazione AuthZAll o AuthNAll applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

### Flusso di lavoro {#workflow-retrieve-authorization-decisions-while-degradation-is-applied}

Segui i passaggi forniti per implementare il flusso di autorizzazione durante l’applicazione della degradazione, come illustrato nel diagramma seguente.

![Recuperare le decisioni di autorizzazione durante l&#39;applicazione della riduzione di livello](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-authorization-decisions-while-degradation-is-applied-flow.png)

*Recuperare le decisioni di autorizzazione durante l&#39;applicazione della riduzione di livello*

1. **Recupera decisione di autorizzazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per ottenere una decisione di autorizzazione per una risorsa specifica chiamando l&#39;endpoint Decisions Authorize.

   >[!IMPORTANT]
   > 
   > Per informazioni dettagliate su: [Recuperare le decisioni di autorizzazione utilizzando la documentazione API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifica:
   >
   > * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   > * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_

1. **Verifica regole di degradazione:** Il server Adobe Pass verifica se è stata applicata una regola di degradazione AuthZAll o AuthNAll all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

1. **Restituisci decisione `Permit` con token multimediale:** La risposta dell&#39;endpoint Decisions Authorize contiene una decisione `Permit` e un token multimediale.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta alla decisione, consulta la [documentazione API MVPD](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifica per il recupero delle decisioni di autorizzazione.
   >
   > <br/>
   > 
   > L’endpoint Decisions Authorize convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > L’endpoint Decisions Authorize utilizza i dati della richiesta per verificare se sono soddisfatte condizioni di accesso degradate:
   >
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve avere una regola di degradazione AuthZAll o AuthNAll applicata.
   >
   > <br/>
   > 
   > Se la convalida dell’accesso degradato non riesce, la risposta assume come valore predefinito il flusso di autorizzazione di base.

1. **Avvia flusso con token multimediale:** L&#39;applicazione di streaming utilizza il token multimediale per riprodurre il contenuto.

## Recupera le decisioni di pre-autorizzazione durante l’applicazione della degradazione {#retrieve-preauthorization-decisions-while-degradation-is-applied}

### Prerequisiti {#prerequisites-retrieve-preauthorization-decisions-while-degradation-is-applied}

Prima di recuperare le decisioni di preautorizzazione durante l’applicazione della degradazione, verifica che siano soddisfatti i seguenti prerequisiti:

* L’applicazione di streaming desidera recuperare le decisioni di preautorizzazione per visualizzare un elenco di risorse e i relativi stati associati.

>[!IMPORTANT]
>
> Presupposti
>
> <br/>
> 
> * L’applicazione di streaming non dispone di un profilo valido per quel MVPD specifico.
> * Regola di degradazione AuthZAll o AuthNAll applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

### Flusso di lavoro {#workflow-retrieve-preauthorization-decisions-while-degradation-is-applied}

Segui i passaggi forniti per implementare il flusso di preautorizzazione mentre viene applicata la degradazione, come mostrato nel diagramma seguente.

![Recupera le decisioni di preautorizzazione durante l&#39;applicazione della degradazione](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-preauthorization-decisions-while-degradation-is-applied-flow.png)

*Recupera le decisioni di preautorizzazione durante l&#39;applicazione della degradazione*

1. **Recupera decisioni di preautorizzazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per ottenere le decisioni di preautorizzazione per un elenco di risorse chiamando l&#39;endpoint di preautorizzazione delle decisioni.

   >[!IMPORTANT]
   >
   > Consulta la [Documentazione API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) specifica per il recupero delle decisioni di preautorizzazione, per informazioni dettagliate su:
   >
   > * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   > * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_

1. **Verifica regole di degradazione:** Il server Adobe Pass verifica se è stata applicata una regola di degradazione AuthZAll o AuthNAll all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

1. **Restituire decisioni di preautorizzazione:** La risposta dell&#39;endpoint di preautorizzazione delle decisioni contiene una decisione `Permit` per ogni risorsa.

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
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > L’endpoint di preautorizzazione delle decisioni utilizza i dati della richiesta per verificare se sono soddisfatte condizioni di accesso degradate:
   >
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve avere una regola di degradazione AuthZAll o AuthNAll applicata.
   >
   > <br/>
   > 
   > Se la convalida degli accessi danneggiati non riesce, la risposta assume come valore predefinito il flusso di preautorizzazione di base.

1. **Gestione delle decisioni di preautorizzazione:** L&#39;applicazione di streaming elabora la risposta e può utilizzarla per visualizzare facoltativamente lo stato appropriato per ogni risorsa nell&#39;interfaccia utente.

## Recupera il profilo durante l&#39;applicazione della degradazione {#retrieve-profile-while-degradation-is-applied}

>[!IMPORTANT]
>
> La query dell’endpoint Profiles è facoltativa durante l’applicazione della degradazione.
>
> <br/>
> 
> La risposta dell’endpoint Sessions indica all’applicazione di procedere con i flussi di decisioni mentre viene applicata la degradazione. Per ulteriori dettagli, consulta la sezione [Eseguire l&#39;autenticazione durante l&#39;applicazione della degradazione](#perform-authentication-while-degradation-is-applied).

### Prerequisiti {#prerequisites-retrieve-profile-while-degradation-is-applied}

Prima di recuperare il profilo per un MVPD specifico durante l’applicazione della degradazione, verifica che siano soddisfatti i seguenti prerequisiti:

* L&#39;applicazione di streaming, che ha un identificatore `mvpd` selezionato o memorizzato nella cache, desidera recuperare il profilo per un MVPD specifico.

>[!IMPORTANT]
>
> Presupposti
>
> <br/>
> 
> * L’applicazione di streaming non dispone di un profilo valido per quel MVPD specifico.
> * Regola di degradazione AuthNAll applicata all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

### Flusso di lavoro {#workflow-retrieve-profile-while-degradation-is-applied}

Segui i passaggi forniti per implementare il flusso di recupero del profilo per un MVPD specifico mentre viene applicata la degradazione, come illustrato nel diagramma seguente.

![Recupera il profilo durante l&#39;applicazione della degradazione](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-profile-while-degradation-is-applied-flow.png)

*Recupera il profilo durante l&#39;applicazione della degradazione*

1. **Recupera profilo per mvpd specifico:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare le informazioni sul profilo per tale MVPD specifico inviando una richiesta all&#39;endpoint Profili.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recupera profilo per documentazione API mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) specifica, consulta:
   >
   > * Tutti i parametri _required_, come `serviceProvider` e `mvpd`
   > * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_

1. **Verifica regole di degradazione:** Il server Adobe Pass verifica se è stata applicata una regola di degradazione AuthNAll all&#39;integrazione tra `serviceProvider` e `mvpd` forniti.

1. **Restituire informazioni sul profilo danneggiato:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo danneggiato, incluso l&#39;attributo `type` impostato su &quot;danneggiato&quot;.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione API [Recupera profilo per mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) specifico.
   >
   > <br/>
   >
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   > 
   > L’endpoint Profili utilizza i dati della richiesta per verificare se sono soddisfatte condizioni di accesso degradate:
   >
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve avere una regola di degradazione AuthNAll applicata.
   >
   > <br/>
   > 
   > Se la convalida degli accessi danneggiati non riesce, la risposta utilizza come valore predefinito il flusso di recupero dei profili di base.

1. **Procedi con i flussi di decisioni:** Se la risposta dell&#39;endpoint Profili contiene un profilo, l&#39;applicazione di streaming utilizza le informazioni del profilo degradate per continuare con i flussi di decisioni successivi.

1. **Indicare un nuovo flusso di autenticazione di base:** Se la risposta dell&#39;endpoint Profiles non contiene un profilo, l&#39;applicazione di streaming indica all&#39;utente di avviare un nuovo flusso di autenticazione di base.

>[!NOTE]
>
> I passaggi per il flusso di recupero del profilo per un codice di autenticazione specifico sono gli stessi di cui sopra, tranne per il fatto che l&#39;endpoint utilizzato è quello descritto nella documentazione di [Recupera profilo per codice specifico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).
