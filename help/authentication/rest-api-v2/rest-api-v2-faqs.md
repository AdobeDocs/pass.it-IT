---
title: Domande frequenti su REST API V2
description: Domande frequenti su REST API V2
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '7304'
ht-degree: 0%

---

# Domande frequenti su REST API V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questo documento fornisce risposte generali elevate alle domande frequenti sull’adozione dell’API REST V2 per l’autenticazione di Adobe Pass.

Per ulteriori informazioni generali sull&#39;API REST V2, consulta la documentazione [Panoramica API REST V2](./rest-api-v2-overview.md).

## Domande frequenti generali {#general-faqs}

Inizia con questa sezione se stai lavorando su un&#39;applicazione che deve integrare l&#39;API REST V2, sia che si tratti di una nuova applicazione o di una esistente che esegue la migrazione da [API REST V1](#migrate-rest-api-v1-to-rest-api-v2) o [SDK](#migrate-sdk-to-rest-api-v2).

Per ulteriori informazioni sui dettagli e i passaggi della migrazione, consulta anche le sezioni successive.

+++Domande frequenti sulla fase di registrazione

### 1. Qual è lo scopo della fase di registrazione? {#registration-phase-faq1}

Lo scopo della fase di registrazione è registrare l&#39;applicazione client rispetto all&#39;autenticazione Adobe Pass tramite il processo [Dynamic Client Registration (DCR)](./rest-api-v2-glossary.md#dcr).

Il processo Dynamic Client Registration (DCR) richiede che l&#39;applicazione client ottenga una coppia di credenziali client e recuperi un token di accesso come obiettivo finale della fase di registrazione.

Per ulteriori informazioni, consulta la documentazione [Panoramica registrazione client dinamico](/help/authentication/dcr-api/dynamic-client-registration-overview.md).

### 2. La fase di registrazione è obbligatoria? {#registration-phase-faq2}

La fase di registrazione è obbligatoria, ma l’applicazione client può saltare questa fase se dispone di una coppia memorizzata nella cache di credenziali client e di un token di accesso ancora valido.

### 3. Che cos&#39;è un&#39;istruzione software e per quanto tempo è valida? {#registration-phase-faq3}

L&#39;istruzione software è un termine definito nella [documentazione](./rest-api-v2-glossary.md#software-statement).

L&#39;istruzione software è costituita da un JSON Web Token (JWT) che può essere generato e scaricato dal dashboard di Adobe Pass [TVE](./rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

L’istruzione software è valida per un periodo di tempo illimitato, ma puoi scegliere di chiedere a un rappresentante dell’autenticazione di Adobe Pass di revocarla in qualsiasi momento.

L&#39;applicazione client deve memorizzare l&#39;istruzione software e utilizzarla quando è necessario recuperare le credenziali client.

Per ulteriori dettagli, consulta la documentazione [Panoramica registrazione client dinamico](/help/authentication/dcr-api/dynamic-client-registration-overview.md).

### 4. Come generare e scaricare un rendiconto software? {#registration-phase-faq4}

Questa operazione può essere completata tramite Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consultare la [Guida utente canali dashboard TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) o la [Guida utente programmatori dashboard TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

### 5. Cosa succede se viene revocata un&#39;istruzione software? {#registration-phase-faq5}

Quando l&#39;istruzione software viene revocata, è importante considerare una delle seguenti conseguenze:

* Le applicazioni client che utilizzano l&#39;istruzione software revocata non saranno più in grado di eseguire i flussi [adesione](./rest-api-v2-glossary.md#entitlement), il che significa che gli utenti non potranno più riprodurre il contenuto.

### 6. Cosa sono le credenziali del client e per quanto tempo sono valide? {#registration-phase-faq6}

Le credenziali client sono un termine definito nella documentazione di [Glossary](./rest-api-v2-glossary.md#client-credentials).

Le credenziali client sono costituite da un identificatore client e da una coppia di segreto client che è possibile recuperare dall&#39;endpoint Registro client.

Le credenziali client sono valide per un periodo di tempo illimitato.

L’applicazione client deve memorizzare a tempo indeterminato le credenziali client e utilizzarle quando è necessario recuperare un token di accesso.

Per ulteriori informazioni, consulta la documentazione di [Recupero credenziali client](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

### 7. Come gestire le credenziali del client? {#registration-phase-faq7}

È consigliabile che l’applicazione client gestisca una coppia univoca di credenziali client per ogni istanza dell’applicazione utente in caso di integrazioni client-server e server-to-server con l’autenticazione di Adobe Pass.

L’applicazione client deve memorizzare a tempo indeterminato le credenziali client e utilizzarle quando è necessario recuperare un token di accesso.

### 8. Cosa succede se le credenziali del client memorizzate nella cache vengono perse? {#registration-phase-faq8}

Quando le credenziali del client memorizzate nella cache vengono perse, è necessario considerare tre conseguenze importanti:

* L&#39;applicazione client deve ottenere una nuova coppia di credenziali client.
* L&#39;applicazione client deve ottenere un nuovo token di accesso utilizzando la nuova coppia di credenziali client.
* L’applicazione client dovrà chiedere all’utente di autenticare nuovamente, in quanto perderà l’accesso ai profili autenticati ottenuti in precedenza.

### 9. Cos’è un token di accesso e per quanto tempo è valido? {#registration-phase-faq9}

Il token di accesso è un termine definito nella documentazione di [Glossary](./rest-api-v2-glossary.md#access-token).

Il token di accesso è costituito da un [token bearer](./appendix/headers/rest-api-v2-appendix-headers-authorization.md) che può essere recuperato dall&#39;endpoint del token client.

Il token di accesso è valido per un periodo di tempo limitato e breve specificato al momento del rilascio.

L’applicazione client deve memorizzare il token di accesso e utilizzarlo fino alla scadenza quando viene eseguita la destinazione dell’API REST V2.

Per evitare richieste non autorizzate, l&#39;applicazione client deve ottenere un nuovo token di accesso prima della scadenza di quella corrente.

Per ulteriori informazioni, consulta la documentazione [Recuperare il token di accesso](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).

### 10. Come può l’applicazione client aggiornare un token di accesso? {#registration-phase-faq10}

L’applicazione client deve aggiornare un token di accesso nello stesso modo in cui recupera un nuovo token di accesso, ma utilizzando le credenziali client memorizzate nella cache.

L’applicazione client non deve registrarsi nuovamente per aggiornare un token di accesso, ma deve utilizzare le credenziali client memorizzate; in caso contrario, gli utenti dovranno ripetere l’autenticazione.

Per ulteriori informazioni, consulta la documentazione [Recuperare il token di accesso](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).

+++

+++Domande frequenti sulla fase di configurazione

### 1. Qual è lo scopo della fase di configurazione? {#configuration-phase-faq1}

Lo scopo della fase di configurazione è quello di fornire all’applicazione client l’elenco di MVPD con cui è attivamente integrata, insieme ai dettagli di configurazione salvati dall’autenticazione di Adobe Pass per ogni MVPD.

La fase di configurazione funge da passaggio preliminare per la fase di autenticazione quando l&#39;applicazione client deve richiedere all&#39;utente di selezionare il proprio provider TV.

### 2. La fase di configurazione è obbligatoria? {#configuration-phase-faq2}

La fase di configurazione non è obbligatoria, l&#39;applicazione client può saltare questa fase nei seguenti scenari:

* Utente già autenticato.
* All&#39;utente viene offerto un accesso temporaneo tramite la funzione TempPass di base o promozionale.
* L’autenticazione dell’utente è scaduta, ma l’applicazione client ha memorizzato nella cache l’MVPD selezionato in precedenza come scelta motivata dall’esperienza utente e richiede semplicemente all’utente di confermare di essere ancora abbonato a tale MVPD.

### 3. Che cos’è una configurazione e per quanto tempo è valida? {#configuration-phase-faq3}

La configurazione è un termine definito nella documentazione di [Glossary](./rest-api-v2-glossary.md#configuration).

La configurazione è costituita da un elenco di MVPD che possono essere recuperati dall’endpoint Configuration.

L’applicazione client può utilizzare la configurazione per presentare un componente dell’interfaccia utente denominato &quot;Selettore&quot; quando si richiede all’utente di selezionare il proprio MVPD.

L’applicazione client deve aggiornare la configurazione prima che l’utente passi attraverso la fase di autenticazione.

Per ulteriori informazioni, consulta la documentazione di [Recupero configurazione](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

### 4. L&#39;applicazione client può gestire il proprio elenco di MVPD? {#configuration-phase-faq4}

L’applicazione client può gestire il proprio elenco di MVPD, ma si consiglia di utilizzare la configurazione fornita dall’autenticazione di Adobe Pass per garantire che l’elenco sia aggiornato e accurato.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/enhanced-error-codes.md) dall&#39;API REST di autenticazione di Adobe Pass V2 se l&#39;utente selezionato non dispone di un&#39;integrazione attiva con il [provider di servizi](./rest-api-v2-glossary.md#service-provider) specificato.

### 5. L&#39;applicazione client è in grado di filtrare l&#39;elenco di MVPD? {#configuration-phase-faq5}

L’applicazione client può filtrare l’elenco di MVPD forniti nella risposta di configurazione implementando un meccanismo personalizzato in base alla propria logica di business e ai requisiti, ad esempio la posizione dell’utente o la cronologia degli utenti della selezione precedente.

### 6. Cosa succede se l’integrazione con un MVPD è disabilitata e contrassegnata come inattiva? {#configuration-phase-faq6}

Quando l’integrazione con un MVPD è disabilitata e contrassegnata come inattiva, l’MVPD viene rimosso dall’elenco di MVPD fornito nelle risposte di configurazione successive e ci sono due importanti conseguenze da considerare:

* Gli utenti non autenticati di tale MVPD non saranno più in grado di completare la fase di autenticazione utilizzando tale MVPD.
* Gli utenti autenticati di tale MVPD non saranno più in grado di completare le fasi di pre-autorizzazione, autorizzazione o disconnessione utilizzando tale MVPD.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/enhanced-error-codes.md) dall&#39;API REST di autenticazione di Adobe Pass V2 se l&#39;utente selezionato non dispone più di un&#39;integrazione attiva con il [provider di servizi](./rest-api-v2-glossary.md#service-provider) specificato.

### 7. Cosa succede se l’integrazione con un MVPD è nuovamente abilitata e contrassegnata come attiva? {#configuration-phase-faq7}

Quando l’integrazione con un MVPD viene nuovamente abilitata e contrassegnata come attiva, l’MVPD viene nuovamente incluso nell’elenco di MVPD fornito nelle risposte di configurazione successive e ci sono due importanti conseguenze da considerare:

* Gli utenti non autenticati di tale MVPD saranno nuovamente in grado di completare la fase di autenticazione utilizzando tale MVPD.
* Gli utenti autenticati di tale MVPD saranno nuovamente in grado di completare le fasi di pre-autorizzazione, autorizzazione o disconnessione utilizzando tale MVPD.

### 8. Come abilitare o disabilitare l’integrazione con un MVPD? {#configuration-phase-faq8}

Questa operazione può essere completata tramite Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

+++Domande frequenti sulla fase di autenticazione

### 1. Qual è lo scopo della fase di autenticazione? {#authentication-phase-faq1}

Lo scopo della fase di autenticazione è quello di fornire all&#39;applicazione client la capacità di verificare l&#39;identità dell&#39;utente e ottenere le informazioni sui metadati dell&#39;utente.

La fase di autenticazione funge da passaggio preliminare per la fase di pre-autorizzazione o di autorizzazione quando l’applicazione client deve riprodurre il contenuto.

### 2. In che modo l’applicazione client può sapere se l’utente è già autenticato? {#authentication-phase-faq2}

L’applicazione client può eseguire query su uno dei seguenti endpoint in grado di verificare se un utente è già autenticato e restituire informazioni sul profilo:

* [API dell’endpoint &quot;Profiles&quot;](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint &quot;profile&quot; per API MVPD specifica](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint &quot;profile&quot; per API di codice specifiche (di autenticazione)](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Per ulteriori informazioni, consulta i seguenti documenti:

* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 3. In che modo l&#39;applicazione client può ottenere le informazioni sui metadati dell&#39;utente? {#authentication-phase-faq3}

L&#39;applicazione client può eseguire una query su uno dei seguenti endpoint in grado di restituire [informazioni sui metadati dell&#39;utente](/help/authentication/user-metadata-feature.md) come parte delle informazioni del profilo:

* [API dell’endpoint &quot;Profiles&quot;](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint &quot;profile&quot; per API MVPD specifica](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint &quot;profile&quot; per API di codice specifiche (di autenticazione)](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Per ulteriori informazioni, consulta i seguenti documenti:

* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 4. Cos’è una sessione di autenticazione e per quanto tempo è valida? {#authentication-phase-faq4}

La sessione di autenticazione è un termine definito nella documentazione di [Glossary](./rest-api-v2-glossary.md#session).

La sessione di autenticazione memorizza informazioni sul processo di autenticazione avviato che possono essere recuperate dall’endpoint Sessioni.

La sessione di autenticazione è valida per un periodo di tempo limitato e breve specificato al momento del problema, che indica il periodo di tempo necessario all’utente per completare il processo di autenticazione prima di richiedere il riavvio del flusso.

L&#39;applicazione client può utilizzare la risposta della sessione di autenticazione per sapere come procedere con il processo di autenticazione. Tieni presente che in alcuni casi non è necessario eseguire l’autenticazione dell’utente, ad esempio fornendo un accesso temporaneo o ridotto oppure quando l’utente è già autenticato.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Crea API sessione di autenticazione](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Riprendi API sessione di autenticazione](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 5. Che cos’è un codice di autenticazione e per quanto tempo è valido? {#authentication-phase-faq5}

Il codice di autenticazione è un termine definito nella documentazione di [Glossary](./rest-api-v2-glossary.md#code).

Il codice di autenticazione memorizza un valore univoco generato quando un utente avvia il processo di autenticazione e identifica in modo univoco la sessione di autenticazione di un utente fino al completamento del processo o alla scadenza della sessione di autenticazione.

Il codice di autenticazione è valido per un periodo di tempo limitato e breve specificato al momento dell’avvio della sessione di autenticazione, che indica il tempo necessario all’utente per completare il processo di autenticazione prima di richiedere il riavvio del flusso.

L’applicazione client può utilizzare il codice di autenticazione per consentire all’utente di completare o riprendere il processo di autenticazione sullo stesso dispositivo o su un secondo, considerando che la sessione di autenticazione non è scaduta.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Crea API sessione di autenticazione](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Riprendi API sessione di autenticazione](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 6. Come può l&#39;applicazione client sapere se l&#39;utente ha digitato un codice di autenticazione valido e se la sessione di autenticazione non è ancora scaduta? {#authentication-phase-faq6}

L’applicazione client può convalidare il codice di autenticazione digitato dall’utente in un’applicazione secondaria (a schermo) inviando una richiesta all’endpoint sessioni responsabile per recuperare le informazioni sulla sessione di autenticazione associate al codice di autenticazione.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/enhanced-error-codes.md) se il codice di autenticazione fornito fosse digitato in modo errato o se la sessione di autenticazione fosse scaduta.

Per ulteriori informazioni, consulta la documentazione [Recuperare la sessione di autenticazione](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

### 7. Che cos’è un profilo e per quanto tempo è valido? {#authentication-phase-faq7}

Il profilo è un termine definito nella documentazione di [Glossary](./rest-api-v2-glossary.md#profile).

Il profilo memorizza informazioni sulla validità dell’autenticazione dell’utente, sui metadati e molto altro ancora che può essere recuperato dall’endpoint &quot;Profiles&quot;.

L’applicazione client può utilizzare il profilo per conoscere lo stato di autenticazione dell’utente, accedere alle informazioni sui metadati dell’utente o trovare il metodo utilizzato per l’autenticazione.

Per ulteriori informazioni, consulta i seguenti documenti:

* [API dell’endpoint &quot;Profiles&quot;](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint &quot;profile&quot; per API MVPD specifica](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint &quot;profile&quot; per API di codice specifiche (di autenticazione)](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Il profilo è valido per un periodo di tempo limitato specificato al momento della query, che indica per quanto tempo l’utente dispone di un’autenticazione valida prima di dover ripetere la fase di autenticazione.

Questo intervallo di tempo limitato, noto come autenticazione (authN) [TTL](./rest-api-v2-glossary.md#ttl), può essere visualizzato e modificato tramite Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

+++

+++Domande frequenti sulla fase di preautorizzazione

### 1. Qual è lo scopo della fase di pre-autorizzazione? {#preauthorization-phase-faq1}

Lo scopo della fase di pre-autorizzazione è quello di fornire all’applicazione client la capacità di presentare un sottoinsieme di risorse dal suo catalogo a cui l’utente avrebbe diritto di accedere.

La fase di preautorizzazione può migliorare l’esperienza utente quando l’utente apre l’applicazione client per la prima volta o passa a una nuova sezione.

### 2. La fase di pre-autorizzazione è obbligatoria? {#preauthorization-phase-faq2}

La fase di pre-autorizzazione non è obbligatoria, l’applicazione client può saltare questa fase se desidera presentare un catalogo di risorse senza filtrarle prima in base al diritto dell’utente.

### 3. Che cos’è una decisione di preautorizzazione? {#preauthorization-phase-faq3}

La pre-autorizzazione è un termine definito nella documentazione di [Glossary](./rest-api-v2-glossary.md#preauthorization), mentre il termine della decisione si trova anche nel [Glossary](./rest-api-v2-glossary.md#decision).

La decisione di pre-autorizzazione memorizza informazioni sul risultato del processo di pre-autorizzazione MVPD che possono essere recuperate dall’endpoint di pre-autorizzazione delle decisioni.

L’applicazione client può utilizzare le decisioni di preautorizzazione per presentare un sottoinsieme di risorse a cui l’utente può accedere in base alle decisioni (informative) del fornitore TV.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Recuperare l’API per le decisioni di preautorizzazione](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Flusso di pre-autorizzazione di base eseguito nell’applicazione principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

### 4. Perché nella decisione di preautorizzazione manca un token multimediale? {#preauthorization-phase-faq4}

Nella decisione di pre-autorizzazione manca un token multimediale perché la fase di pre-autorizzazione non deve essere utilizzata per riprodurre le risorse, in quanto questo è lo scopo della fase di autorizzazione.

### 5. Cos’è una risorsa e quali formati sono supportati? {#preauthorization-phase-faq5}

La risorsa è un termine definito nella documentazione di [Glossary](./rest-api-v2-glossary.md#resource).

La risorsa è un identificatore univoco concordato con gli MVPD ed è associata a un contenuto che l’applicazione client può inviare in streaming.

L’identificatore univoco della risorsa può avere due formati:

* Un formato di stringa semplice, ad esempio un identificatore univoco per un canale (brand).
* Un formato RSS per contenuti multimediali (MRSS) contenente informazioni aggiuntive come il titolo, le valutazioni e i metadati per il controllo genitori.

Per ulteriori dettagli, consulta la documentazione [Identificazione delle risorse protette](/help/authentication/identify-protected-resources.md).

### 6. Per quante risorse l&#39;applicazione client può ottenere una decisione di preautorizzazione alla volta? {#preauthorization-phase-faq6}

L’applicazione client può ottenere una decisione di preautorizzazione per un numero limitato di risorse in una singola richiesta API, di solito fino a 5, a causa delle condizioni imposte dagli MVPD.

Questo numero massimo di risorse può essere visualizzato e modificato dopo aver concordato con gli MVPD tramite Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di autenticazione Adobe Pass che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

+++

+++Domande frequenti sulla fase di autorizzazione

### 1. Qual è lo scopo della fase di autorizzazione? {#authorization-phase-faq1}

Lo scopo della fase di autorizzazione è quello di fornire all&#39;applicazione client la capacità di riprodurre le risorse richieste dall&#39;utente dopo aver convalidato i propri diritti con l&#39;MVPD.

### 2. La fase di autorizzazione è obbligatoria? {#authorization-phase-faq2}

La fase di autorizzazione è obbligatoria, l’applicazione client non può saltare questa fase se desidera riprodurre le risorse richieste dall’utente, in quanto richiede di verificare con l’MVPD che l’utente abbia diritto prima di rilasciare il flusso.

### 3. Cos’è una decisione di autorizzazione e per quanto tempo è valida? {#authorization-phase-faq3}

L&#39;autorizzazione è un termine definito nella documentazione di [Glossary](./rest-api-v2-glossary.md#authorization), mentre il termine della decisione si trova anche nel [Glossary](./rest-api-v2-glossary.md#decision).

La decisione di autorizzazione memorizza informazioni sul risultato dell&#39;interrogazione del processo di autorizzazione MVPD che possono essere recuperate dall&#39;endpoint Autorizzazione decisioni.

L’applicazione client può utilizzare la decisione di autorizzazione contenente un token multimediale per riprodurre il flusso di risorse nel caso in cui la decisione del fornitore TV (autorevole) consenta all’utente di accedervi.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Recuperare l’API per le decisioni di autorizzazione](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

La decisione di autorizzazione è valida per un periodo di tempo limitato e breve specificato al momento del rilascio, che indica il periodo di tempo in cui verrà memorizzata nella cache dall’autenticazione di Adobe Pass prima che venga richiesto di eseguire nuovamente la query MVPD.

Questo periodo di tempo limitato, noto come autorizzazione (authZ) [TTL](./rest-api-v2-glossary.md#ttl), può essere visualizzato e modificato tramite Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

### 4. Cos’è un token multimediale e per quanto tempo è valido? {#authorization-phase-faq4}

Il token multimediale è un termine definito nella documentazione di [Glossary](./rest-api-v2-glossary.md#media-token).

Il token multimediale è costituito da una stringa firmata inviata in testo non crittografato che può essere recuperata dall’endpoint Autorizzazione decisioni.

Per ulteriori informazioni, consulta la documentazione [Integrazione del Media Token Verifier](/help/authentication/media-token-verifier-int.md).

Il token multimediale è valido per un periodo di tempo limitato e breve specificato al momento del rilascio, che indica il periodo di tempo che deve essere utilizzato dall’applicazione client prima di richiedere di nuovo la query sull’endpoint Decisions Authorize.

L’applicazione client può utilizzare il token multimediale per riprodurre un flusso di risorse nel caso in cui la decisione del fornitore TV (autorevole) consenta all’utente di accedervi.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Recuperare l’API per le decisioni di autorizzazione](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

### 5. Cos’è una risorsa e quali formati sono supportati? {#authorization-phase-faq5}

La risorsa è un termine definito nella documentazione di [Glossary](./rest-api-v2-glossary.md#resource).

La risorsa è un identificatore univoco concordato con gli MVPD ed è associata a un contenuto che l’applicazione client può inviare in streaming.

L’identificatore univoco della risorsa può avere due formati:

* Un formato di stringa semplice, ad esempio un identificatore univoco per un canale (brand).
* Un formato RSS per contenuti multimediali (MRSS) contenente informazioni aggiuntive come il titolo, le valutazioni e i metadati per il controllo genitori.

Per ulteriori dettagli, consulta la documentazione [Identificazione delle risorse protette](/help/authentication/identify-protected-resources.md).

### 6. Per quante risorse la richiesta del cliente può ottenere una decisione di autorizzazione alla volta? {#authorization-phase-faq6}

L’applicazione client può ottenere una decisione di autorizzazione per un numero limitato di risorse in una singola richiesta API, di solito fino a 1, a causa delle condizioni imposte dagli MVPD.

+++

+++Domande frequenti sulla fase di disconnessione

### 1. Qual è lo scopo della fase di disconnessione? {#logout-phase-faq1}

Lo scopo della fase di disconnessione è fornire all’applicazione client la funzionalità per terminare il profilo autenticato dell’utente nell’ambito dell’autenticazione di Adobe Pass su richiesta dell’utente.

+++

+++Domande frequenti sulle intestazioni

### 1. Come calcolare il valore per l’intestazione Autorizzazione? {#headers-faq1}

L&#39;intestazione della richiesta [Authorization](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) contiene il token di accesso `Bearer` richiesto dall&#39;applicazione client per accedere alle API protette di Adobe Pass.

Il valore dell’intestazione Authorization deve essere ottenuto dall’autenticazione di Adobe Pass durante la fase di registrazione.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Panoramica di Registrazione client dinamici](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [Recupera API credenziali client](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recupera API token di accesso](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flusso di registrazione client dinamici](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

Se l&#39;applicazione client esegue la migrazione da REST API V1 a REST API V2, l&#39;applicazione client può continuare a utilizzare lo stesso metodo per ottenere il token di accesso `Bearer` utilizzato in precedenza.

### 2. Come calcolare il valore per l’intestazione AP-Device-Identifier? {#headers-faq2}

L&#39;intestazione della richiesta [AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) contiene l&#39;identificatore del dispositivo di streaming creato dall&#39;applicazione client.

La documentazione dell&#39;intestazione [AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) fornisce alcuni esempi su come calcolare il valore per piattaforme diverse, ma l&#39;applicazione client può scegliere di utilizzare un metodo diverso in base alla propria logica di business e ai propri requisiti.

Nel caso in cui l’applicazione client stia eseguendo la migrazione dall’API REST V1 all’API REST V2, l’applicazione client può continuare a utilizzare lo stesso metodo per calcolare l’identificatore del dispositivo come prima.

### 3. Come calcolare il valore per l’intestazione X-Device-Info? {#headers-faq3}

L&#39;intestazione della richiesta [X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) contiene le informazioni client (dispositivo, connessione e applicazione) relative al dispositivo di streaming effettivo.

Nella documentazione dell&#39;intestazione [X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) sono disponibili alcuni esempi di come calcolare il valore per piattaforme diverse, ma l&#39;applicazione client può scegliere di utilizzare un metodo diverso in base alla propria logica di business e ai propri requisiti.

Se l’applicazione client esegue la migrazione da REST API V1 a REST API V2, può continuare a utilizzare lo stesso metodo utilizzato in precedenza per calcolare le informazioni sul dispositivo.

+++

## Domande frequenti sulla migrazione {#migration-faqs}

Continuare con questa sezione se si sta lavorando su un&#39;applicazione che deve migrare un&#39;applicazione esistente all&#39;API REST V2.

+++Domande frequenti sulla migrazione generali

### 1. È necessario eseguire il rollout di una nuova applicazione client migrata all’API REST V2 per tutti gli utenti contemporaneamente? {#migration-faq1}

No.

Non è necessario che l’applicazione client distribuisca una nuova versione che integra l’API REST V2 a tutti gli utenti simultaneamente.

L’autenticazione di Adobe Pass continuerà a supportare le versioni precedenti delle applicazioni client che integrano l’API REST V1 o l’SDK fino alla fine del 2025.

### 2. È necessario implementare una nuova applicazione client migrata all’API REST V2 in tutte le API e i flussi contemporaneamente? {#migration-faq2}

Sì.

L’applicazione client deve distribuire una nuova versione che integri l’API REST V2 in tutte le API e i flussi di autenticazione di Adobe Pass simultaneamente.

Nel caso del flusso di autenticazione della seconda schermata, l&#39;applicazione client deve eseguire il rollout di una nuova versione che integra l&#39;API REST V2 per le applicazioni [primary](./rest-api-v2-glossary.md#primary-application) e [secondary](./rest-api-v2-glossary.md#secondary-application) simultaneamente.

L’autenticazione Adobe Pass non supporterà implementazioni &quot;ibride&quot; che integrano sia l’API REST V2 che l’API REST V1/SDK tra API e flussi.

### 3. L’autenticazione utente verrà mantenuta durante l’aggiornamento a una nuova applicazione client migrata all’API REST V2? {#migration-faq3}

No.

L’autenticazione utente ottenuta nelle versioni precedenti dell’applicazione client che integrano l’API REST V1 o l’SDK non verrà mantenuta.

Pertanto, all’utente verrà richiesto di autenticare nuovamente all’interno della nuova applicazione client migrata all’API REST V2.

### 4. L’applicazione client può utilizzare le applicazioni registrate esistenti (istruzioni software)? {#migration-faq4}

L’applicazione client non può riutilizzare le applicazioni registrate esistenti (istruzioni software), pertanto deve generare e scaricare una nuova applicazione registrata (istruzioni software) dedicata all’utilizzo dell’API REST V2.

Questa operazione può essere completata tramite Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consultare la [Guida utente canali dashboard TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) o la [Guida utente programmatori dashboard TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

Per il momento ti verrà richiesto di chiedere a un rappresentante dell&#39;autenticazione di Adobe Pass di abilitare l&#39;utilizzo dell&#39;API REST V2 per le nuove applicazioni registrate (istruzioni software), fino a quando la dashboard di Adobe Pass [TVE](./rest-api-v2-glossary.md#tve-dashboard) non verrà aggiornata per consentire l&#39;autogestione di questa operazione.

Per distinguere le applicazioni registrate (istruzioni software) utilizzate nelle applicazioni client che utilizzano l’API REST V2, è necessario aggiungere un suffisso specifico al nome dell’applicazione registrata, ad esempio &quot;RESTV2&quot;.

### 5. L’applicazione client può utilizzare gli schemi personalizzati esistenti? {#migration-faq5}

L&#39;applicazione client può riutilizzare gli schemi personalizzati esistenti generati tramite Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard).

Per ulteriori dettagli, consultare la [Guida utente canali dashboard TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#custom-schemes) o la [Guida utente programmatori dashboard TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#custom-schemes).

### 6. I codici di errore avanzati sono abilitati per impostazione predefinita nell’API REST V2? {#migration-faq6}

Sì.

Le applicazioni client che integrano l’API REST V2 beneficiano della funzione di codici di errore avanzati abilitata per impostazione predefinita.

Per ulteriori dettagli, consulta la documentazione sui [codici di errore avanzati](/help/authentication/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

+++

### Migrazione da REST API V1 a REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Continua con questa sottosezione se stai lavorando su un’applicazione che deve migrare dall’API REST V1 all’API REST V2.

+++Domande frequenti sulla fase di registrazione

#### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di registrazione? {#registration-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 non vi sono modifiche di alto livello per quanto riguarda la fase di registrazione.

L&#39;applicazione client può continuare a utilizzare lo stesso flusso per eseguire la registrazione in base all&#39;autenticazione Adobe Pass tramite il processo [Dynamic Client Registration (DCR)](./rest-api-v2-glossary.md#dcr) e ottenere un token di accesso.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Panoramica di Registrazione client dinamici](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [Recupera API credenziali client](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recupera API token di accesso](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flusso di registrazione client dinamici](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

+++

+++Domande frequenti sulla fase di configurazione

#### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di configurazione? {#configuration-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|------------------------------------------------|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera elenco di MVPD con integrazione attiva | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configurazione](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++Domande frequenti sulla fase di autenticazione

#### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di autenticazione? {#authentication-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera codice di registrazione (codice di autenticazione) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verifica il codice di registrazione (codice di autenticazione) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessioni/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Riprendi autenticazione (MVPD) nella seconda schermata (applicazione) | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Avvia autenticazione (MVPD) | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [GET <br/> /api/v1/checkauthn (prima schermata)](/help/authentication/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (seconda schermata)](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare il token di autenticazione utente (profilo) | [GET <br/> /api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++Domande frequenti sulla fase di preautorizzazione

#### 1. Quali sono le migrazioni API di alto livello richieste per la fase di pre-autorizzazione? {#preauthorization-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [GET <br/> /api/v1/preauthorize (prima schermata)](/help/authentication/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (seconda schermata)](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++Domande frequenti sulla fase di autorizzazione

#### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di autorizzazione? {#authorization-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|-------------------------------------------------------|----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autorizzazione (MVPD) | [GET <br/> /api/v1/authorize](/help/authentication/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recupera token di autorizzazione (decisione di autorizzazione) | [GET <br/> /api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recupera token di autorizzazione breve (token multimediale) | [GET <br/> /api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++Domande frequenti sulla fase di disconnessione

#### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di disconnessione? {#logout-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|-----------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia disconnessione | [GET <br/> /api/v1/logout](/help/authentication/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di logout di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migrazione dall’SDK all’API REST V2 {#migrate-sdk-to-rest-api-v2}

Continua con questa sottosezione se stai lavorando su un’applicazione che deve migrare dall’SDK all’API REST V2.

+++Domande frequenti sulla fase di registrazione

#### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di registrazione? {#registration-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

##### SDK di AccessEnabler per JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrazione client dinamica (DCR) completa | Fornitura di istruzioni software al costruttore | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Panoramica sulla registrazione client dinamica](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Flusso di registrazione client dinamico](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrazione client dinamica (DCR) completa | Fornitura di istruzioni software al costruttore | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Panoramica sulla registrazione client dinamica](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Flusso di registrazione client dinamico](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### SDK di AccessEnabler per Android

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrazione client dinamica (DCR) completa | Fornitura di istruzioni software al costruttore | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Panoramica sulla registrazione client dinamica](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Flusso di registrazione client dinamico](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrazione client dinamica (DCR) completa | Fornitura di istruzioni software al costruttore | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Panoramica sulla registrazione client dinamica](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Flusso di registrazione client dinamico](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

+++Domande frequenti sulla fase di configurazione

#### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di configurazione? {#configuration-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

##### SDK di AccessEnabler per JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------------------|--------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera elenco di MVPD con integrazione attiva | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configurazione](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera elenco di MVPD con integrazione attiva | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configurazione](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### SDK di AccessEnabler per Android

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera elenco di MVPD con integrazione attiva | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configurazione](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera elenco di MVPD con integrazione attiva | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configurazione](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++Domande frequenti sulla fase di autenticazione

#### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di autenticazione? {#authentication-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

##### SDK di AccessEnabler per JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/javascript-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/javascript-sdk-api-reference.md#getMeta) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler per iOS

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera codice di registrazione (codice di autenticazione) | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verifica il codice di registrazione (codice di autenticazione) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessioni/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Riprendi autenticazione (MVPD) nella seconda schermata (applicazione) | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler per Android

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/android-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/android-sdk-api-reference.md#getMetadata) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera codice di registrazione (codice di autenticazione) | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verifica il codice di registrazione (codice di autenticazione) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessioni/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Riprendi autenticazione (MVPD) nella seconda schermata (applicazione) | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/amazon-fireos-native-client-api-reference.md#getMetadata) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++Domande frequenti sulla fase di preautorizzazione

#### 1. Quali sono le migrazioni API di alto livello richieste per la fase di pre-autorizzazione? {#preauthorization-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

##### SDK di AccessEnabler per JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler per Android

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++Domande frequenti sulla fase di autorizzazione

#### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di autorizzazione? {#authorization-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

##### SDK di AccessEnabler per JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler per Android

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++Domande frequenti sulla fase di disconnessione

#### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di disconnessione? {#logout-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

##### SDK di AccessEnabler per JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|-----------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Avvia disconnessione | [AccessEnabler.logout](/help/authentication/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di logout di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Avvia disconnessione | [AccessEnabler.logout](/help/authentication/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di logout di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler per Android

| Ambito | SDK | API REST V2 | Osservazioni |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Avvia disconnessione | [AccessEnabler.logout](/help/authentication/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di logout di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|-----------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Avvia disconnessione | [AccessEnabler.logout](/help/authentication/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di logout di base eseguito nell&#39;applicazione primaria](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
