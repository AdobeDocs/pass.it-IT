---
title: Domande frequenti su REST API V2
description: Domande frequenti su REST API V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '6460'
ht-degree: 0%

---

# Domande frequenti su REST API V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questo documento fornisce risposte generali elevate alle domande frequenti sull’adozione dell’API REST V2 per l’autenticazione di Adobe Pass.

Per ulteriori informazioni generali sull&#39;API REST V2, consulta la documentazione [Panoramica API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

## Domande frequenti generali {#general-faqs}

Inizia con questa sezione se stai lavorando su un&#39;applicazione che deve integrare l&#39;API REST V2, sia che si tratti di una nuova applicazione o di una esistente che esegue la migrazione da [API REST V1](#migration-rest-api-v1-to-rest-api-v2) o [SDK](#migration-sdk-to-rest-api-v2).

Per ulteriori informazioni sui dettagli e i passaggi della migrazione, consulta anche le sezioni successive.

>[!MORELIKETHIS]
>
> * [Domande frequenti su Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#general-faqs)

### Domande frequenti sulla fase di registrazione {#registration-phase-faqs-general}

+++Domande frequenti sulla fase di registrazione

Consulta la documentazione [Domande frequenti sulla registrazione client dinamica (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs).

+++

### Domande frequenti sulla fase di configurazione {#configuration-phase-faqs-general}

+++Domande frequenti sulla fase di configurazione

#### 1. Qual è lo scopo della fase di configurazione? {#configuration-phase-faq1}

Lo scopo della fase di configurazione è quello di fornire all’applicazione client l’elenco di MVPD con cui è attivamente integrata, insieme ai dettagli di configurazione salvati dall’autenticazione di Adobe Pass per ogni MVPD.

La fase di configurazione funge da passaggio preliminare per la fase di autenticazione quando l&#39;applicazione client deve richiedere all&#39;utente di selezionare il proprio provider TV.

#### 2. La fase di configurazione è obbligatoria? {#configuration-phase-faq2}

La fase di configurazione non è obbligatoria, l&#39;applicazione client può saltare questa fase nei seguenti scenari:

* Utente già autenticato.
* All&#39;utente viene offerto un accesso temporaneo tramite la funzione TempPass di base o promozionale.
* L’autenticazione dell’utente è scaduta, ma l’applicazione client ha memorizzato nella cache il MVPD selezionato in precedenza come scelta motivata dall’esperienza utente e richiede all’utente di confermare che è ancora un abbonato di quel MVPD.

#### 3. Che cos’è una configurazione e per quanto tempo è valida? {#configuration-phase-faq3}

La configurazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration).

La configurazione è costituita da un elenco di MVPD che possono essere recuperati dall’endpoint Configuration.

L’applicazione client può utilizzare la configurazione per presentare un componente dell’interfaccia utente denominato &quot;Selettore&quot; quando si richiede all’utente di selezionare il proprio MVPD.

L’applicazione client deve aggiornare la configurazione prima che l’utente passi attraverso la fase di autenticazione.

Per ulteriori informazioni, consulta la documentazione di [Recupero configurazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### 4. L&#39;applicazione client può gestire il proprio elenco di MVPD? {#configuration-phase-faq4}

L’applicazione client può gestire il proprio elenco di MVPD, ma si consiglia di utilizzare la configurazione fornita dall’autenticazione di Adobe Pass per garantire che l’elenco sia aggiornato e accurato.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) dall&#39;API REST di autenticazione di Adobe Pass V2 se l&#39;utente selezionato per MVPD non dispone di un&#39;integrazione attiva con il [provider di servizi](rest-api-v2-glossary.md#service-provider) specificato.

#### 5. L&#39;applicazione client è in grado di filtrare l&#39;elenco di MVPD? {#configuration-phase-faq5}

L’applicazione client può filtrare l’elenco di MVPD forniti nella risposta di configurazione implementando un meccanismo personalizzato in base alla propria logica di business e ai requisiti, ad esempio la posizione dell’utente o la cronologia degli utenti della selezione precedente.

#### 6. Cosa succede se l’integrazione con un MVPD è disabilitata e contrassegnata come inattiva? {#configuration-phase-faq6}

Quando l’integrazione con un MVPD è disabilitata e contrassegnata come inattiva, il MVPD viene rimosso dall’elenco degli MVPD forniti nelle ulteriori risposte di configurazione e ci sono due importanti conseguenze da considerare:

* Gli utenti non autenticati di quel MVPD non potranno più completare la fase di autenticazione utilizzando quel MVPD.
* Gli utenti autenticati di tale MVPD non saranno più in grado di completare le fasi di pre-autorizzazione, autorizzazione o disconnessione utilizzando tale MVPD.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) dall&#39;API REST di autenticazione di Adobe Pass V2 se l&#39;utente selezionato per MVPD non dispone più di un&#39;integrazione attiva con il [provider di servizi](rest-api-v2-glossary.md#service-provider) specificato.

#### 7. Cosa succede se l’integrazione con un MVPD è nuovamente abilitata e contrassegnata come attiva? {#configuration-phase-faq7}

Quando l’integrazione con un MVPD viene nuovamente abilitata e contrassegnata come attiva, il MVPD viene incluso nuovamente nell’elenco di MVPD forniti nelle risposte di configurazione successive e ci sono due importanti conseguenze da considerare:

* Gli utenti non autenticati di quel MVPD saranno nuovamente in grado di completare la fase di autenticazione utilizzando quel MVPD.
* Gli utenti autenticati di tale MVPD potranno nuovamente completare le fasi di pre-autorizzazione, autorizzazione o disconnessione utilizzando tale MVPD.

#### 8. Come abilitare o disabilitare l’integrazione con un MVPD? {#configuration-phase-faq8}

Questa operazione può essere completata tramite Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

### Domande frequenti sulla fase di autenticazione {#authentication-phase-faqs-general}

+++Domande frequenti sulla fase di autenticazione

#### 1. Qual è lo scopo della fase di autenticazione? {#authentication-phase-faq1}

Lo scopo della fase di autenticazione è quello di fornire all&#39;applicazione client la capacità di verificare l&#39;identità dell&#39;utente e ottenere le informazioni sui metadati dell&#39;utente.

La fase di autenticazione funge da passaggio preliminare per la fase di pre-autorizzazione o di autorizzazione quando l’applicazione client deve riprodurre il contenuto.

#### 2. In che modo l’applicazione client può sapere se l’utente è già autenticato? {#authentication-phase-faq2}

L’applicazione client può eseguire query su uno dei seguenti endpoint in grado di verificare se un utente è già autenticato e restituire informazioni sul profilo:

* [API dell’endpoint &quot;Profiles&quot;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint &quot;profile&quot; per API MVPD specifiche](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint &quot;profile&quot; per API di codice specifiche (di autenticazione)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Per ulteriori informazioni, consulta i seguenti documenti:

* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 3. In che modo l&#39;applicazione client può ottenere le informazioni sui metadati dell&#39;utente? {#authentication-phase-faq3}

L&#39;applicazione client può eseguire una query su uno dei seguenti endpoint in grado di restituire [informazioni sui metadati dell&#39;utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) come parte delle informazioni del profilo:

* [API dell’endpoint &quot;Profiles&quot;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint &quot;profile&quot; per API MVPD specifiche](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint &quot;profile&quot; per API di codice specifiche (di autenticazione)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Per ulteriori informazioni, consulta i seguenti documenti:

* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 4. Cos’è una sessione di autenticazione e per quanto tempo è valida? {#authentication-phase-faq4}

La sessione di autenticazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session).

La sessione di autenticazione memorizza informazioni sul processo di autenticazione avviato che possono essere recuperate dall’endpoint Sessioni.

La sessione di autenticazione è valida per un periodo di tempo limitato e breve specificato al momento del problema, che indica il periodo di tempo necessario all’utente per completare il processo di autenticazione prima di richiedere il riavvio del flusso.

L&#39;applicazione client può utilizzare la risposta della sessione di autenticazione per sapere come procedere con il processo di autenticazione. Tieni presente che in alcuni casi non è necessario eseguire l’autenticazione dell’utente, ad esempio fornendo un accesso temporaneo o ridotto oppure quando l’utente è già autenticato.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Crea API sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Riprendi API sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 5. Che cos’è un codice di autenticazione e per quanto tempo è valido? {#authentication-phase-faq5}

Il codice di autenticazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code).

Il codice di autenticazione memorizza un valore univoco generato quando un utente avvia il processo di autenticazione e identifica in modo univoco la sessione di autenticazione di un utente fino al completamento del processo o alla scadenza della sessione di autenticazione.

Il codice di autenticazione è valido per un periodo di tempo limitato e breve specificato al momento dell’avvio della sessione di autenticazione, che indica il tempo necessario all’utente per completare il processo di autenticazione prima di richiedere il riavvio del flusso.

L’applicazione client può utilizzare il codice di autenticazione per consentire all’utente di completare o riprendere il processo di autenticazione sullo stesso dispositivo o su un secondo, considerando che la sessione di autenticazione non è scaduta.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Crea API sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Riprendi API sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 6. Come può l&#39;applicazione client sapere se l&#39;utente ha digitato un codice di autenticazione valido e se la sessione di autenticazione non è ancora scaduta? {#authentication-phase-faq6}

L’applicazione client può convalidare il codice di autenticazione digitato dall’utente in un’applicazione secondaria (a schermo) inviando una richiesta all’endpoint sessioni responsabile per recuperare le informazioni sulla sessione di autenticazione associate al codice di autenticazione.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) se il codice di autenticazione fornito fosse digitato in modo errato o se la sessione di autenticazione fosse scaduta.

Per ulteriori informazioni, consulta la documentazione [Recuperare la sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### 7. Che cos’è un profilo e per quanto tempo è valido? {#authentication-phase-faq7}

Il profilo è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile).

Il profilo memorizza informazioni sulla validità dell’autenticazione dell’utente, sui metadati e molto altro ancora che può essere recuperato dall’endpoint &quot;Profiles&quot;.

L’applicazione client può utilizzare il profilo per conoscere lo stato di autenticazione dell’utente, accedere alle informazioni sui metadati dell’utente o trovare il metodo utilizzato per l’autenticazione.

Per ulteriori informazioni, consulta i seguenti documenti:

* [API dell’endpoint &quot;Profiles&quot;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint &quot;profile&quot; per API MVPD specifiche](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint &quot;profile&quot; per API di codice specifiche (di autenticazione)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Il profilo è valido per un periodo di tempo limitato specificato al momento della query, che indica per quanto tempo l’utente dispone di un’autenticazione valida prima di dover ripetere la fase di autenticazione.

Questo intervallo di tempo limitato, noto come autenticazione (authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl), può essere visualizzato e modificato tramite Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

+++

### Domande frequenti sulla fase di pre-autorizzazione {#preauthorization-phase-faqs-general}

+++Domande frequenti sulla fase di preautorizzazione

#### 1. Qual è lo scopo della fase di pre-autorizzazione? {#preauthorization-phase-faq1}

Lo scopo della fase di pre-autorizzazione è quello di fornire all’applicazione client la capacità di presentare un sottoinsieme di risorse dal suo catalogo a cui l’utente avrebbe diritto di accedere.

La fase di preautorizzazione può migliorare l’esperienza utente quando l’utente apre l’applicazione client per la prima volta o passa a una nuova sezione.

#### 2. La fase di pre-autorizzazione è obbligatoria? {#preauthorization-phase-faq2}

La fase di pre-autorizzazione non è obbligatoria, l’applicazione client può saltare questa fase se desidera presentare un catalogo di risorse senza filtrarle prima in base al diritto dell’utente.

#### 3. Che cos’è una decisione di preautorizzazione? {#preauthorization-phase-faq3}

La pre-autorizzazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization), mentre il termine della decisione si trova anche nel [Glossary](rest-api-v2-glossary.md#decision).

La decisione di preautorizzazione memorizza informazioni sul risultato dell’interrogazione del processo di preautorizzazione di MVPD che possono essere recuperate dall’endpoint di preautorizzazione delle decisioni.

L’applicazione client può utilizzare le decisioni di preautorizzazione per presentare un sottoinsieme di risorse a cui l’utente può accedere in base alle decisioni (informative) del fornitore TV.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Recuperare l’API per le decisioni di preautorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Flusso di pre-autorizzazione di base eseguito nell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### 4. Perché nella decisione di preautorizzazione manca un token multimediale? {#preauthorization-phase-faq4}

Nella decisione di pre-autorizzazione manca un token multimediale perché la fase di pre-autorizzazione non deve essere utilizzata per riprodurre le risorse, in quanto questo è lo scopo della fase di autorizzazione.

#### 5. Cos’è una risorsa e quali formati sono supportati? {#preauthorization-phase-faq5}

La risorsa è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

La risorsa è un identificatore univoco concordato con gli MVPD ed è associata a un contenuto che l’applicazione client può inviare in streaming.

L’identificatore univoco della risorsa può avere due formati:

* Un formato di stringa semplice, ad esempio un identificatore univoco per un canale (brand).
* Un formato RSS per contenuti multimediali (MRSS) contenente informazioni aggiuntive come il titolo, le valutazioni e i metadati per il controllo genitori.

Per ulteriori dettagli, consulta la documentazione di [Risorse protette](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 6. Per quante risorse l&#39;applicazione client può ottenere una decisione di preautorizzazione alla volta? {#preauthorization-phase-faq6}

L’applicazione client può ottenere una decisione di preautorizzazione per un numero limitato di risorse in una singola richiesta API, di solito fino a 5, a causa delle condizioni imposte dagli MVPD.

Questo numero massimo di risorse può essere visualizzato e modificato dopo aver concordato con gli MVPD tramite Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di autenticazione Adobe Pass che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

+++

### Domande frequenti sulla fase di autorizzazione {#authorization-phase-faqs-general}

+++Domande frequenti sulla fase di autorizzazione

#### 1. Qual è lo scopo della fase di autorizzazione? {#authorization-phase-faq1}

Lo scopo della fase di autorizzazione è quello di fornire all’applicazione client la capacità di riprodurre le risorse richieste dall’utente dopo aver convalidato i propri diritti con MVPD.

#### 2. La fase di autorizzazione è obbligatoria? {#authorization-phase-faq2}

La fase di autorizzazione è obbligatoria, l’applicazione client non può saltare questa fase se desidera riprodurre le risorse richieste dall’utente, in quanto richiede la verifica con il MVPD che l’utente abbia diritto prima di rilasciare il flusso.

#### 3. Cos’è una decisione di autorizzazione e per quanto tempo è valida? {#authorization-phase-faq3}

L&#39;autorizzazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization), mentre il termine della decisione si trova anche nel [Glossary](rest-api-v2-glossary.md#decision).

La decisione di autorizzazione memorizza informazioni sul risultato della richiesta di informazioni del processo di autorizzazione di MVPD che possono essere recuperate dall’endpoint &quot;Decisions Authorize&quot;.

L’applicazione client può utilizzare la decisione di autorizzazione contenente un token multimediale per riprodurre il flusso di risorse nel caso in cui la decisione del fornitore TV (autorevole) consenta all’utente di accedervi.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Recuperare l’API per le decisioni di autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

La decisione di autorizzazione è valida per un periodo di tempo limitato e breve specificato al momento del rilascio, che indica il periodo di tempo in cui verrà memorizzata nella cache dall’autenticazione di Adobe Pass prima che venga richiesto di eseguire nuovamente la query sul MVPD.

Questo periodo di tempo limitato, noto come autorizzazione (authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl), può essere visualizzato e modificato tramite Adobe Pass [TVE Dashboard](rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### 4. Cos’è un token multimediale e per quanto tempo è valido? {#authorization-phase-faq4}

Il token multimediale è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token).

Il token multimediale è costituito da una stringa firmata inviata in testo non crittografato che può essere recuperata dall’endpoint Autorizzazione decisioni.

Per ulteriori informazioni, consulta la documentazione di [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

Il token multimediale è valido per un periodo di tempo limitato e breve specificato al momento del rilascio, che indica il periodo di tempo che deve essere utilizzato dall’applicazione client prima di richiedere di nuovo la query sull’endpoint Decisions Authorize.

L’applicazione client può utilizzare il token multimediale per riprodurre un flusso di risorse nel caso in cui la decisione del fornitore TV (autorevole) consenta all’utente di accedervi.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Recuperare l’API per le decisioni di autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 5. Cos’è una risorsa e quali formati sono supportati? {#authorization-phase-faq5}

La risorsa è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

La risorsa è un identificatore univoco concordato con gli MVPD ed è associata a un contenuto che l’applicazione client può inviare in streaming.

L’identificatore univoco della risorsa può avere due formati:

* Un formato di stringa semplice, ad esempio un identificatore univoco per un canale (brand).
* Un formato RSS per contenuti multimediali (MRSS) contenente informazioni aggiuntive come il titolo, le valutazioni e i metadati per il controllo genitori.

Per ulteriori dettagli, consulta la documentazione di [Risorse protette](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 6. Per quante risorse la richiesta del cliente può ottenere una decisione di autorizzazione alla volta? {#authorization-phase-faq6}

L’applicazione client può ottenere una decisione di autorizzazione per un numero limitato di risorse in una singola richiesta API, di solito fino a 1, a causa delle condizioni imposte dagli MVPD.

+++

### Domande frequenti sulla fase di disconnessione {#logout-phase-faqs-general}

+++Domande frequenti sulla fase di disconnessione

#### 1. Qual è lo scopo della fase di disconnessione? {#logout-phase-faq1}

Lo scopo della fase di disconnessione è fornire all’applicazione client la funzionalità per terminare il profilo autenticato dell’utente nell’ambito dell’autenticazione di Adobe Pass su richiesta dell’utente.

+++

### Domande frequenti sulle intestazioni {#headers-faqs-general}

+++Domande frequenti sulle intestazioni

#### 1. Come calcolare il valore per l’intestazione Autorizzazione? {#headers-faq1}

L&#39;intestazione della richiesta [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) contiene il token di accesso `Bearer` richiesto dall&#39;applicazione client per accedere alle API protette di Adobe Pass.

Il valore dell’intestazione Authorization deve essere ottenuto dall’autenticazione di Adobe Pass durante la fase di registrazione.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Panoramica di Registrazione client dinamici](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Recupera API credenziali client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recupera API token di accesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flusso di registrazione client dinamici](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

Se l&#39;applicazione client esegue la migrazione da REST API V1 a REST API V2, l&#39;applicazione client può continuare a utilizzare lo stesso metodo per ottenere il token di accesso `Bearer` utilizzato in precedenza.

#### 2. Come calcolare il valore per l’intestazione AP-Device-Identifier? {#headers-faq2}

L&#39;intestazione della richiesta [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) contiene l&#39;identificatore del dispositivo di streaming creato dall&#39;applicazione client.

La documentazione dell&#39;intestazione [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) fornisce alcuni esempi su come calcolare il valore per piattaforme diverse, ma l&#39;applicazione client può scegliere di utilizzare un metodo diverso in base alla propria logica di business e ai propri requisiti.

Nel caso in cui l’applicazione client stia eseguendo la migrazione dall’API REST V1 all’API REST V2, l’applicazione client può continuare a utilizzare lo stesso metodo per calcolare l’identificatore del dispositivo come prima.

#### 3. Come calcolare il valore per l’intestazione X-Device-Info? {#headers-faq3}

L&#39;intestazione della richiesta [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) contiene le informazioni client (dispositivo, connessione e applicazione) relative al dispositivo di streaming effettivo.

Nella documentazione dell&#39;intestazione [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) sono disponibili alcuni esempi di come calcolare il valore per piattaforme diverse, ma l&#39;applicazione client può scegliere di utilizzare un metodo diverso in base alla propria logica di business e ai propri requisiti.

Se l’applicazione client esegue la migrazione da REST API V1 a REST API V2, può continuare a utilizzare lo stesso metodo utilizzato in precedenza per calcolare le informazioni sul dispositivo.

+++

## Domande frequenti sulla migrazione {#migration-faqs}

Continuare con questa sezione se si sta lavorando su un&#39;applicazione che deve migrare un&#39;applicazione esistente all&#39;API REST V2.

>[!MORELIKETHIS]
>
> * [Domande frequenti su Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### Domande frequenti sulla migrazione generali {#general-migration-faqs}

+++Domande frequenti sulla migrazione generali

#### 1. È necessario eseguire il rollout di una nuova applicazione client migrata all’API REST V2 per tutti gli utenti contemporaneamente? {#migration-faq1}

No.

Non è necessario che l’applicazione client distribuisca una nuova versione che integra l’API REST V2 a tutti gli utenti simultaneamente.

L’autenticazione di Adobe Pass continuerà a supportare le versioni precedenti delle applicazioni client che integrano l’API REST V1 o SDK fino alla fine del 2025.

#### 2. È necessario implementare una nuova applicazione client migrata all’API REST V2 in tutte le API e i flussi contemporaneamente? {#migration-faq2}

Sì.

L’applicazione client deve distribuire una nuova versione che integri l’API REST V2 in tutte le API e i flussi di autenticazione di Adobe Pass simultaneamente.

Nel caso del flusso di autenticazione della seconda schermata, l&#39;applicazione client deve eseguire il rollout di una nuova versione che integra l&#39;API REST V2 per le applicazioni [primary](rest-api-v2-glossary.md#primary-application) e [secondary](rest-api-v2-glossary.md#secondary-application) simultaneamente.

L’autenticazione Adobe Pass non supporterà implementazioni &quot;ibride&quot; che integrano sia l’API REST V2 che l’API REST V1/SDK tra API e flussi.

#### 3. L’autenticazione utente verrà mantenuta durante l’aggiornamento a una nuova applicazione client migrata all’API REST V2? {#migration-faq3}

No.

L’autenticazione utente ottenuta nelle versioni precedenti dell’applicazione client che integrano l’API REST V1 o SDK non verrà mantenuta.

Pertanto, all’utente verrà richiesto di autenticare nuovamente all’interno della nuova applicazione client migrata all’API REST V2.

#### 4. I codici di errore avanzati sono abilitati per impostazione predefinita nell’API REST V2? {#migration-faq4}

Sì.

Le applicazioni client che integrano l’API REST V2 beneficiano della funzione di codici di errore avanzati abilitata per impostazione predefinita.

Per ulteriori dettagli, consulta la documentazione sui [codici di errore avanzati](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

+++

### Migrazione da REST API V1 a REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Continua con questa sottosezione se stai lavorando su un’applicazione che deve migrare dall’API REST V1 all’API REST V2.

#### Domande frequenti sulla fase di registrazione {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di registrazione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di registrazione? {#registration-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 non vi sono modifiche di alto livello per quanto riguarda la fase di registrazione.

L&#39;applicazione client può continuare a utilizzare lo stesso flusso per eseguire la registrazione in base all&#39;autenticazione Adobe Pass tramite il processo [Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) e ottenere un token di accesso.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Panoramica di Registrazione client dinamici](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Recupera API credenziali client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recupera API token di accesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flusso di registrazione client dinamici](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### Domande frequenti sulla fase di configurazione {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di configurazione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di configurazione? {#configuration-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera elenco di MVPD con integrazione attiva | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configurazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Domande frequenti sulla fase di autenticazione {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di autenticazione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di autenticazione? {#authentication-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera codice di registrazione (codice di autenticazione) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verifica il codice di registrazione (codice di autenticazione) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessioni/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Riprendi autenticazione (MVPD) nella seconda schermata (applicazione) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Avvia autenticazione (MVPD) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [GET <br/> /api/v1/checkauthn (prima schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (seconda schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare il token di autenticazione utente (profilo) | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di pre-autorizzazione {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di preautorizzazione

##### 1. Quali sono le migrazioni API di alto livello richieste per la fase di pre-autorizzazione? {#preauthorization-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [GET <br/> /api/v1/preauthorize (prima schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (seconda schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di autorizzazione {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di autorizzazione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di autorizzazione? {#authorization-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autorizzazione (MVPD) | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recupera token di autorizzazione (decisione di autorizzazione) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recupera token di autorizzazione breve (token multimediale) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di disconnessione {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di disconnessione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di disconnessione? {#logout-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia disconnessione | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di logout di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migrazione da SDK all’API REST V2 {#migration-sdk-to-rest-api-v2}

Continua con questa sottosezione se stai lavorando su un’applicazione che deve migrare da SDK all’API REST V2.

#### Domande frequenti sulla fase di registrazione {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Domande frequenti sulla fase di registrazione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di registrazione? {#registration-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

###### SDK di AccessEnabler JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrazione client dinamica (DCR) completa | Fornitura di istruzioni software al costruttore | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Panoramica sulla registrazione client dinamica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flusso di registrazione client dinamico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrazione client dinamica (DCR) completa | Fornitura di istruzioni software al costruttore | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Panoramica sulla registrazione client dinamica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flusso di registrazione client dinamico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### SDK di AccessEnabler Android

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrazione client dinamica (DCR) completa | Fornitura di istruzioni software al costruttore | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Panoramica sulla registrazione client dinamica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flusso di registrazione client dinamico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrazione client dinamica (DCR) completa | Fornitura di istruzioni software al costruttore | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Panoramica sulla registrazione client dinamica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flusso di registrazione client dinamico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di configurazione {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Domande frequenti sulla fase di configurazione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di configurazione? {#configuration-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

###### SDK di AccessEnabler JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera elenco di MVPD con integrazione attiva | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configurazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera elenco di MVPD con integrazione attiva | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configurazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### SDK di AccessEnabler Android

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera elenco di MVPD con integrazione attiva | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configurazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera elenco di MVPD con integrazione attiva | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configurazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Domande frequenti sulla fase di autenticazione {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++Domande frequenti sulla fase di autenticazione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di autenticazione? {#authentication-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

###### SDK di AccessEnabler JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler iOS

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera codice di registrazione (codice di autenticazione) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verifica il codice di registrazione (codice di autenticazione) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessioni/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Riprendi autenticazione (MVPD) nella seconda schermata (applicazione) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler Android

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera codice di registrazione (codice di autenticazione) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verifica il codice di registrazione (codice di autenticazione) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessioni/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Riprendi autenticazione (MVPD) nella seconda schermata (applicazione) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di pre-autorizzazione {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Domande frequenti sulla fase di preautorizzazione

##### 1. Quali sono le migrazioni API di alto livello richieste per la fase di pre-autorizzazione? {#preauthorization-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

###### SDK di AccessEnabler JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler Android

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di autorizzazione {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Domande frequenti sulla fase di autorizzazione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di autorizzazione? {#authorization-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

###### SDK di AccessEnabler JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler Android

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di disconnessione {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++Domande frequenti sulla fase di disconnessione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di disconnessione? {#logout-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

###### SDK di AccessEnabler JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia disconnessione | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di logout di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia disconnessione | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di logout di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler Android

| Ambito | SDK | API REST V2 | Osservazioni |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia disconnessione | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di logout di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia disconnessione | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di logout di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
