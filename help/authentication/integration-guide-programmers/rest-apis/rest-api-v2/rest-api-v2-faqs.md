---
title: Domande frequenti su REST API V2
description: Domande frequenti su REST API V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 44fa75eb7b19ff44a41809d44c171baff6853b52
workflow-type: tm+mt
source-wordcount: '11089'
ht-degree: 1%

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

### Domande frequenti sulla fase di registrazione {#registration-phase-faqs-general}

+++Domande frequenti sulla fase di registrazione

Consulta la documentazione [Domande frequenti sulla registrazione client dinamica (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs).

+++

### Domande frequenti sulla fase di configurazione {#configuration-phase-faqs-general}

+++Domande frequenti sulla fase di configurazione

#### &#x200B;1. Qual è lo scopo della fase di configurazione? {#configuration-phase-faq1}

Lo scopo della fase di configurazione è fornire all&#39;applicazione client l&#39;elenco di MVPD con cui è attivamente integrata insieme ai dettagli di configurazione (ad esempio, `id`, `displayName`, `logoUrl`, ecc.) salvate dall’autenticazione di Adobe Pass per ogni MVPD.

La fase di configurazione funge da passaggio preliminare per la fase di autenticazione quando l&#39;applicazione client deve richiedere all&#39;utente di selezionare il proprio provider TV.

#### &#x200B;2. La fase di configurazione è obbligatoria? {#configuration-phase-faq2}

La fase di configurazione non è obbligatoria, l’applicazione client deve recuperare la configurazione solo quando l’utente deve selezionare il proprio MVPD per l’autenticazione o la nuova autenticazione.

L’applicazione client può saltare questa fase nei seguenti scenari:

* Utente già autenticato.
* All&#39;utente viene offerto l&#39;accesso temporaneo tramite la funzionalità [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) di base o promozionale.
* L’autenticazione dell’utente è scaduta, ma l’applicazione client ha memorizzato nella cache il MVPD selezionato in precedenza come scelta motivata dall’esperienza utente e richiede all’utente di confermare che è ancora un abbonato di quel MVPD.

#### &#x200B;3. Cos’è una configurazione e per quanto tempo è valida? {#configuration-phase-faq3}

La configurazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration).

La configurazione contiene un elenco di MVPD definiti dai seguenti attributi `id`, `displayName`, `logoUrl` e così via, che possono essere recuperati dall&#39;endpoint [Configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

L’applicazione client deve recuperare la configurazione solo quando l’utente deve selezionare il proprio MVPD per autenticarsi o autenticarsi di nuovo.

L’applicazione client può utilizzare la risposta di configurazione per presentare un selettore dell’interfaccia utente con le opzioni MVPD disponibili ogni volta che l’utente deve selezionare il proprio provider TV.

Per procedere con le fasi di autenticazione, preautorizzazione, autorizzazione o disconnessione, l&#39;applicazione client deve memorizzare l&#39;identificatore MVPD selezionato dell&#39;utente, come specificato dall&#39;attributo `id` del livello di configurazione di MVPD.

Per ulteriori informazioni, consulta la documentazione di [Recupero configurazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### &#x200B;4. La configurazione è specifica per un provider di servizi, una piattaforma o un utente? {#configuration-phase-faq4}

La configurazione è specifica per un [provider di servizi](rest-api-v2-glossary.md#service-provider).

La configurazione è specifica per un tipo di piattaforma.

La configurazione non è specifica per un utente.

Per le applicazioni client che utilizzano un’architettura server-to-server, si consiglia di memorizzare nella cache la risposta di configurazione (ad esempio, con un TTL di 2 minuti) per ogni tipo di piattaforma nello storage della memoria lato server. Questo riduce le richieste non necessarie per ogni utente e migliora l’esperienza complessiva dell’utente.

#### &#x200B;5. L&#39;applicazione client deve memorizzare nella cache le informazioni di risposta della configurazione in un archivio persistente? {#configuration-phase-faq5}

>[!IMPORTANT]
> 
> Per le applicazioni client che utilizzano un’architettura server-to-server, si consiglia di memorizzare nella cache la risposta di configurazione (ad esempio, con un TTL di 2 minuti) per ogni tipo di piattaforma nello storage della memoria lato server. Questo riduce le richieste non necessarie per ogni utente e migliora l’esperienza complessiva dell’utente.

L’applicazione client deve recuperare la configurazione solo quando l’utente deve selezionare il proprio MVPD per autenticarsi o autenticarsi di nuovo.

L’applicazione client deve memorizzare nella cache le informazioni sulla risposta di configurazione in un archivio di memoria per evitare richieste non necessarie e migliorare l’esperienza utente nei seguenti casi:

* Utente già autenticato.
* All&#39;utente viene offerto l&#39;accesso temporaneo tramite la funzionalità [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) di base o promozionale.
* L’autenticazione dell’utente è scaduta, ma l’applicazione client ha memorizzato nella cache il MVPD selezionato in precedenza come scelta motivata dall’esperienza utente e richiede all’utente di confermare che è ancora un abbonato di quel MVPD.

#### &#x200B;6. L&#39;applicazione client può gestire il proprio elenco di MVPD? {#configuration-phase-faq6}

L’applicazione client può gestire il proprio elenco di MVPD, ma è necessario mantenere gli identificatori MVPD sincronizzati con l’autenticazione di Adobe Pass. Pertanto, si consiglia di utilizzare la configurazione fornita dall’autenticazione di Adobe Pass per garantire che l’elenco sia aggiornato e accurato.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) dall&#39;API REST di autenticazione di Adobe Pass V2 se l&#39;identificatore MVPD fornito non è valido o se non dispone di un&#39;integrazione attiva con il provider di servizi [specificato](rest-api-v2-glossary.md#service-provider).

#### &#x200B;7. L&#39;applicazione client può filtrare l&#39;elenco di MVPD? {#configuration-phase-faq7}

L’applicazione client può filtrare l’elenco di MVPD forniti nella risposta di configurazione implementando un meccanismo personalizzato in base alla propria logica di business e ai requisiti, ad esempio la posizione dell’utente o la cronologia degli utenti della selezione precedente.

L&#39;applicazione client può filtrare l&#39;elenco di [MVPD TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) o MVPD con la relativa integrazione ancora in fase di sviluppo o test.

#### &#x200B;8. Cosa succede se l’integrazione con un MVPD è disabilitata e contrassegnata come inattiva? {#configuration-phase-faq8}

Quando l’integrazione con un MVPD è disabilitata e contrassegnata come inattiva, il MVPD viene rimosso dall’elenco degli MVPD forniti nelle risposte di configurazione successive e ci sono due importanti conseguenze da considerare:

* Gli utenti non autenticati di quel MVPD non potranno più completare la fase di autenticazione utilizzando quel MVPD.
* Gli utenti autenticati di tale MVPD non saranno più in grado di completare le fasi di pre-autorizzazione, autorizzazione o disconnessione utilizzando tale MVPD.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) dall&#39;API REST di autenticazione di Adobe Pass V2 se l&#39;utente selezionato per MVPD non dispone più di un&#39;integrazione attiva con il [provider di servizi](rest-api-v2-glossary.md#service-provider) specificato.

#### &#x200B;9. Cosa succede se l’integrazione con un MVPD è nuovamente abilitata e contrassegnata come attiva? {#configuration-phase-faq9}

Quando l’integrazione con un MVPD viene nuovamente abilitata e contrassegnata come attiva, il MVPD viene incluso nuovamente nell’elenco di MVPD forniti nelle risposte di configurazione successive e ci sono due importanti conseguenze da considerare:

* Gli utenti non autenticati di quel MVPD saranno nuovamente in grado di completare la fase di autenticazione utilizzando quel MVPD.
* Gli utenti autenticati di tale MVPD potranno nuovamente completare le fasi di pre-autorizzazione, autorizzazione o disconnessione utilizzando tale MVPD.

#### &#x200B;10. Come abilitare o disabilitare l’integrazione con un MVPD? {#configuration-phase-faq10}

Questa operazione può essere completata tramite Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

### Domande frequenti sulla fase di autenticazione {#authentication-phase-faqs-general}

+++Domande frequenti sulla fase di autenticazione

#### &#x200B;1. Qual è lo scopo della fase di autenticazione? {#authentication-phase-faq1}

Lo scopo della fase di autenticazione è quello di fornire all&#39;applicazione client la capacità di verificare l&#39;identità dell&#39;utente e ottenere le informazioni sui metadati dell&#39;utente.

La fase di autenticazione funge da passaggio preliminare per la fase di pre-autorizzazione o di autorizzazione quando l’applicazione client deve riprodurre il contenuto.

#### &#x200B;2. La fase di autenticazione è obbligatoria? {#authentication-phase-faq2}

La fase di autenticazione è obbligatoria; l’applicazione client deve autenticare l’utente quando non dispone di un profilo valido nell’ambito dell’autenticazione di Adobe Pass.

L’applicazione client può saltare questa fase nei seguenti scenari:

* L’utente è già autenticato e il profilo è ancora valido.
* All&#39;utente viene offerto l&#39;accesso temporaneo tramite la funzionalità [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) di base o promozionale.

La gestione degli errori dell&#39;applicazione client richiede la gestione dei codici [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) (ad esempio `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated` e così via), che indicano che l&#39;applicazione client richiede l&#39;autenticazione dell&#39;utente.

#### &#x200B;3. Cos’è una sessione di autenticazione e per quanto tempo è valida? {#authentication-phase-faq3}

La sessione di autenticazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session).

La sessione di autenticazione memorizza informazioni sul processo di autenticazione avviato che possono essere recuperate dall’endpoint Sessioni.

La sessione di autenticazione è valida per un periodo di tempo limitato e breve specificato al momento del problema dalla marca temporale `notAfter`, che indica il tempo necessario all&#39;utente per completare il processo di autenticazione prima di richiedere il riavvio del flusso.

L&#39;applicazione client può utilizzare la risposta della sessione di autenticazione per sapere come procedere con il processo di autenticazione. Tieni presente che in alcuni casi non è necessario eseguire l’autenticazione dell’utente, ad esempio fornendo un accesso temporaneo o ridotto oppure quando l’utente è già autenticato.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Crea API sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Riprendi API sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;4. Che cos’è un codice di autenticazione e per quanto tempo è valido? {#authentication-phase-faq4}

Il codice di autenticazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code).

Il codice di autenticazione memorizza un valore univoco generato quando un utente avvia il processo di autenticazione e identifica in modo univoco la sessione di autenticazione di un utente fino al completamento del processo o alla scadenza della sessione di autenticazione.

Il codice di autenticazione è valido per un periodo di tempo limitato e breve specificato al momento dell&#39;avvio della sessione di autenticazione tramite la marca temporale `notAfter`, che indica il tempo necessario all&#39;utente per completare il processo di autenticazione prima di richiedere il riavvio del flusso.

L’applicazione client può utilizzare il codice di autenticazione per verificare se l’utente ha completato correttamente l’autenticazione e recuperare le informazioni sul profilo dell’utente, in genere tramite un meccanismo di polling.

L’applicazione client può inoltre utilizzare il codice di autenticazione per consentire all’utente di completare o riprendere il processo di autenticazione sullo stesso dispositivo o su un secondo dispositivo (schermo), considerando che la sessione di autenticazione non è scaduta.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Crea API sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Riprendi API sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;5. In che modo l&#39;applicazione client può sapere se l&#39;utente ha digitato un codice di autenticazione valido e se la sessione di autenticazione non è ancora scaduta? {#authentication-phase-faq5}

L’applicazione client può convalidare il codice di autenticazione digitato dall’utente in un’applicazione secondaria (a schermo) inviando una richiesta a uno degli endpoint sessioni responsabili per riprendere la sessione di autenticazione o recuperare le informazioni sulla sessione di autenticazione associate al codice di autenticazione.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) se il codice di autenticazione fornito fosse stato digitato in modo errato o se la sessione di autenticazione fosse scaduta.

Per ulteriori informazioni, consultare i documenti [Riprendi sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) e [Recupera sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### &#x200B;6. Come può l’applicazione client sapere se l’utente è già autenticato? {#authentication-phase-faq6}

L’applicazione client può eseguire query su uno dei seguenti endpoint in grado di verificare se un utente è già autenticato e restituire informazioni sul profilo:

* [API dell’endpoint &quot;Profiles&quot;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint &quot;profile&quot; per API MVPD specifiche](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint &quot;profile&quot; per API di codice specifiche (di autenticazione)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Per ulteriori informazioni, consulta i seguenti documenti:

* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### &#x200B;7. Che cos’è un profilo e per quanto tempo è valido? {#authentication-phase-faq7}

Il profilo è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile).

Il profilo memorizza informazioni sulla validità dell’autenticazione dell’utente, sui metadati e molto altro ancora che può essere recuperato dall’endpoint &quot;Profiles&quot;.

L’applicazione client può utilizzare il profilo per conoscere lo stato di autenticazione dell’utente, accedere alle informazioni sui metadati dell’utente, trovare il metodo utilizzato per l’autenticazione o l’entità utilizzata per fornire l’identità.

Per ulteriori informazioni, consulta i seguenti documenti:

* [API dell’endpoint &quot;Profiles&quot;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint &quot;profile&quot; per API MVPD specifiche](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint &quot;profile&quot; per API di codice specifiche (di autenticazione)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Il profilo è valido per un periodo di tempo limitato specificato quando viene eseguita una query con la marca temporale `notAfter`, che indica il periodo di tempo per cui l&#39;utente dispone di un&#39;autenticazione valida prima di richiedere di ripetere la fase di autenticazione.

Questo intervallo di tempo limitato, noto come autenticazione (authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl), può essere visualizzato e modificato tramite Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### &#x200B;8. L&#39;applicazione client deve memorizzare nella cache le informazioni sul profilo dell&#39;utente in un archivio permanente? {#authentication-phase-faq8}

L’applicazione client deve memorizzare in cache parti delle informazioni del profilo utente in un archivio persistente per evitare richieste non necessarie e migliorare l’esperienza utente, considerando i seguenti aspetti:

| Attributo | Esperienza utente |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `mvpd` | L&#39;applicazione client può utilizzarlo per tenere traccia del provider TV selezionato dall&#39;utente e continuare a utilizzarlo durante le fasi di pre-autorizzazione o autorizzazione.<br/><br/>Alla scadenza del profilo utente corrente, l&#39;applicazione client può utilizzare la selezione di MVPD memorizzata e chiedere conferma all&#39;utente. |
| `attributes` | L&#39;applicazione client può utilizzarlo per personalizzare l&#39;esperienza utente in base a [chiavi di metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) diverse (ad esempio, `zip`, `maxRating`, ecc.).<br/><br/>I metadati utente diventano disponibili al termine del flusso di autenticazione, pertanto l&#39;applicazione client non deve eseguire query su un endpoint separato per recuperare le informazioni di [metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md), in quanto sono già incluse nelle informazioni del profilo.<br/><br/>Alcuni attributi di metadati possono essere aggiornati durante il flusso di autorizzazione, a seconda di MVPD e dell&#39;attributo di metadati specifico. Di conseguenza, l’applicazione client potrebbe dover eseguire nuovamente la query sulle API dei profili per recuperare i metadati dell’utente più recenti. |
| `notAfter` | L&#39;applicazione client può utilizzarla per tenere traccia della data di scadenza del profilo utente.<br/><br/>La gestione degli errori dell&#39;applicazione client richiede la gestione dei codici [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) (ad esempio, `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated`, ecc.), che indicano che l&#39;applicazione client richiede l&#39;autenticazione dell&#39;utente. |

#### &#x200B;9. L’applicazione client può estendere il profilo dell’utente senza richiedere una nuova autenticazione? {#authentication-phase-faq9}

No.

Il profilo utente non può essere esteso oltre la sua validità senza l’interazione dell’utente, in quanto la sua scadenza è determinata dal TTL di autenticazione stabilito con gli MVPD.

Pertanto, l’applicazione client deve richiedere all’utente di autenticarsi nuovamente e interagire con la pagina di accesso di MVPD per aggiornare il proprio profilo sul sistema.

Tuttavia, per gli MVPD che supportano [l&#39;autenticazione basata su home](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) (HBA), l&#39;utente non dovrà immettere le credenziali.

#### &#x200B;10. Quali sono i casi d’uso per ogni endpoint dei profili disponibile? {#authentication-phase-faq10}

Gli endpoint di base per i profili sono progettati per fornire all’applicazione client la funzionalità di conoscere lo stato di autenticazione dell’utente, accedere alle informazioni sui metadati dell’utente, trovare il metodo utilizzato per l’autenticazione o l’entità utilizzata per fornire l’identità.

Ogni endpoint è adatto a un caso d’uso specifico, come segue:

| API | Descrizione | Caso d’uso |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [API endpoint profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | Recupera tutti i profili utente. | **L&#39;utente apre l&#39;applicazione client per la prima volta**<br/><br/> In questo scenario, l&#39;applicazione client non ha l&#39;identificatore MVPD selezionato dell&#39;utente memorizzato nella cache dell&#39;archivio permanente.<br/><br/>Di conseguenza, invierà una singola richiesta per recuperare tutti i profili utente disponibili. |
| [Endpoint dei profili per API MVPD specifica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | Recupera il profilo utente associato a un MVPD specifico. | **L&#39;utente ritorna all&#39;applicazione client dopo l&#39;autenticazione in una visita precedente**<br/><br/> In questo caso, l&#39;applicazione client deve avere l&#39;identificatore MVPD selezionato in precedenza nella cache dell&#39;archivio permanente.<br/><br/>Di conseguenza, invierà una singola richiesta per recuperare il profilo dell&#39;utente per quel MVPD specifico. |
| [Endpoint dei profili per API di codice specifico (autenticazione)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Recupera il profilo utente associato a un codice di autenticazione specifico. | **L&#39;utente avvia il processo di autenticazione**<br/><br/> In questo caso, l&#39;applicazione client deve determinare se l&#39;utente ha completato correttamente l&#39;autenticazione e recuperare le informazioni sul profilo.<br/><br/>Verrà quindi avviato un meccanismo di polling per recuperare il profilo dell&#39;utente associato al codice di autenticazione. |

Per ulteriori dettagli, fare riferimento al [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md) e al [Flusso dei profili di base eseguito all&#39;interno dei documenti dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md).

L’endpoint SSO Profili ha uno scopo diverso e fornisce all’applicazione client la possibilità di creare un profilo utente utilizzando la risposta di autenticazione del partner e di recuperarlo in un’unica operazione una tantum.

| API | Descrizione | Caso d’uso |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Profili API endpoint SSO](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | Crea e recupera il profilo utente utilizzando la risposta di autenticazione del partner. | **L&#39;utente consente all&#39;applicazione di utilizzare il Single Sign-On partner per l&#39;autenticazione**<br/><br/> In questo caso, l&#39;applicazione client deve creare un profilo utente dopo aver ricevuto la risposta di autenticazione partner e recuperarla in un&#39;unica operazione. |

Per qualsiasi query successiva, è necessario utilizzare gli endpoint di base Profiles per determinare lo stato di autenticazione dell’utente, accedere alle informazioni sui metadati dell’utente, trovare il metodo utilizzato per l’autenticazione o l’entità utilizzata per fornire l’identità.

Per ulteriori dettagli, fare riferimento ai documenti [Single Sign-On utilizzando i flussi dei partner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) e [Apple SSO Cookbook (REST API V2)](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md).

#### &#x200B;11. Cosa deve fare l’applicazione client se l’utente dispone di più profili MVPD? {#authentication-phase-faq11}

La decisione di supportare più profili dipende dai requisiti di business dell’applicazione client.

La maggior parte degli utenti avrà un solo profilo. Tuttavia, nei casi in cui esistono più profili (come descritto di seguito), l’applicazione client è responsabile di determinare la migliore esperienza utente per la selezione del profilo.

L’applicazione client può scegliere di richiedere all’utente di selezionare il profilo MVPD desiderato o di effettuare la selezione automaticamente, ad esempio scegliendo il primo profilo utente dalla risposta o quello con il periodo di validità più lungo.

REST API v2 supporta più profili per supportare:

* Utenti che potrebbero dover scegliere tra un profilo MVPD normale e uno ottenuto tramite Single Sign-On (SSO).
* Agli utenti viene offerto l’accesso temporaneo senza dover disconnettersi dal normale MVPD.
* Utenti con abbonamento a MVPD combinato con servizi Direct-to-Consumer (DTC).
* Utenti con più abbonamenti MVPD.

#### &#x200B;12. Cosa succede quando i profili utente scadono? {#authentication-phase-faq12}

Quando i profili utente scadono, non vengono più inclusi nella risposta dall’endpoint &quot;Profiles&quot;.

Se l’endpoint Profili restituisce una risposta di mappa dei profili vuota, l’applicazione client deve creare una nuova sessione di autenticazione e richiedere all’utente di ripetere l’autenticazione.

Per ulteriori informazioni, consulta la documentazione [Creare l&#39;API della sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

#### &#x200B;13. Quando i profili utente non sono più validi? {#authentication-phase-faq13}

I profili utente non sono più validi nei seguenti scenari:

* Quando scade il TTL di autenticazione, come indicato dalla marca temporale `notAfter` nella risposta dell’endpoint Profili.
* Quando l&#39;applicazione client modifica il valore dell&#39;intestazione [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md).
* Quando l&#39;applicazione client aggiorna le credenziali client utilizzate per recuperare il valore dell&#39;intestazione [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md).
* Quando l&#39;applicazione client revoca o aggiorna l&#39;istruzione software utilizzata per ottenere le credenziali del client.

#### &#x200B;14. Quando avviare il meccanismo di polling nell&#39;applicazione client? {#authentication-phase-faq14}

Per garantire l’efficienza ed evitare richieste non necessarie, l’applicazione client deve avviare il meccanismo di polling nelle seguenti condizioni:

**Autenticazione eseguita nell&#39;applicazione primaria (schermata)**

L&#39;applicazione primaria (streaming) deve avviare il polling quando l&#39;utente raggiunge la pagina di destinazione finale, dopo che il componente del browser carica l&#39;URL specificato per il parametro `redirectUrl` nella richiesta dell&#39;endpoint [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

**Autenticazione eseguita all&#39;interno di un&#39;applicazione secondaria (schermo)**

L&#39;applicazione principale (streaming) deve avviare il polling non appena l&#39;utente avvia il processo di autenticazione, subito dopo aver ricevuto la risposta dell&#39;endpoint [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) e aver visualizzato il codice di autenticazione per l&#39;utente.

#### &#x200B;15. Quando arrestare il meccanismo di polling nell&#39;applicazione client? {#authentication-phase-faq15}

Per garantire l’efficienza ed evitare richieste non necessarie, l’applicazione client deve arrestare il meccanismo di polling nelle seguenti condizioni:

**Autenticazione completata**

Le informazioni del profilo dell’utente vengono recuperate correttamente, confermando il loro stato di autenticazione. A questo punto, il polling non è più necessario.

**Sessione di autenticazione e scadenza del codice**

La sessione di autenticazione e il codice scadono, come indicato dalla marca temporale `notAfter` (ad esempio, 30 minuti) nella risposta dell&#39;endpoint [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). In questo caso, l’utente deve riavviare il processo di autenticazione e arrestare immediatamente il polling che utilizza il codice di autenticazione precedente.

**Nuovo codice di autenticazione generato**

Se l’utente richiede un nuovo codice di autenticazione sul dispositivo principale (schermo), la sessione esistente non è più valida e il polling che utilizza il codice di autenticazione precedente deve essere interrotto immediatamente.

#### &#x200B;16. Intervallo tra le chiamate che l&#39;applicazione client deve utilizzare per il meccanismo di polling. {#authentication-phase-faq16}

Per garantire l&#39;efficienza ed evitare richieste non necessarie, l&#39;applicazione client deve configurare la frequenza del meccanismo di polling nelle seguenti condizioni:

| **Autenticazione eseguita nell&#39;applicazione primaria (schermata)** | **Autenticazione eseguita all&#39;interno di un&#39;applicazione secondaria (schermo)** |
|----------------------------------------------------------------------------|----------------------------------------------------------------------------|
| L’applicazione principale (streaming) deve eseguire il polling ogni 3-5 secondi o più. | L’applicazione principale (streaming) deve eseguire il polling ogni 3-5 secondi o più. |

#### &#x200B;17. Qual è il numero massimo di richieste di polling che l&#39;applicazione client può inviare? {#authentication-phase-faq17}

L&#39;applicazione client deve rispettare i limiti correnti definiti dal meccanismo di limitazione dell&#39;autenticazione di Adobe Pass [](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits).

La gestione degli errori dell&#39;applicazione client deve essere in grado di gestire il codice di errore [429 Troppe richieste](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response), che indica che l&#39;applicazione client ha superato il numero massimo di richieste consentito.

Per ulteriori dettagli, consulta la documentazione sul [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

#### &#x200B;18. In che modo l&#39;applicazione client può ottenere le informazioni sui metadati dell&#39;utente? {#authentication-phase-faq18}

L&#39;applicazione client può eseguire una query su uno dei seguenti endpoint in grado di restituire [informazioni sui metadati dell&#39;utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) come parte delle informazioni del profilo:

* [API dell’endpoint &quot;Profiles&quot;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint &quot;profile&quot; per API MVPD specifiche](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint &quot;profile&quot; per API di codice specifiche (di autenticazione)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

I metadati utente diventano disponibili al termine del flusso di autenticazione, pertanto l&#39;applicazione client non deve eseguire una query su un endpoint separato per recuperare le informazioni sui [metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md), in quanto sono già inclusi nelle informazioni del profilo.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Alcuni attributi di metadati possono essere aggiornati durante il flusso di autorizzazione, a seconda di MVPD e dell’attributo di metadati specifico. Di conseguenza, l’applicazione client potrebbe dover eseguire nuovamente la query sulle API di cui sopra per recuperare i metadati dell’utente più recenti.

#### &#x200B;19. In che modo l&#39;applicazione client deve gestire l&#39;accesso danneggiato? {#authentication-phase-faq19}

La [funzionalità di degradazione](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md) consente all&#39;applicazione client di mantenere un&#39;esperienza di streaming senza soluzione di continuità per gli utenti, anche quando i servizi di autenticazione o autorizzazione di MVPD incontrano problemi.

In sintesi, questo può garantire un accesso ininterrotto ai contenuti nonostante interruzioni temporanee del servizio MVPD.

Dato che l’organizzazione intende utilizzare la funzione di degradazione premium, l’applicazione client deve gestire flussi di accesso degradati, che delineano il comportamento degli endpoint REST API v2 in tali scenari.

Per ulteriori dettagli, consulta la documentazione sui [flussi di accesso danneggiati](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).

#### &#x200B;20. In che modo l&#39;applicazione client deve gestire l&#39;accesso temporaneo? {#authentication-phase-faq20}

La funzionalità [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) consente all&#39;applicazione client di fornire accesso temporaneo all&#39;utente.

In sintesi, questo approccio può coinvolgere gli utenti fornendo un accesso limitato nel tempo ai contenuti o a un numero predefinito di titoli VOD per un periodo di tempo specificato.

Dato che la tua organizzazione intende utilizzare la funzione Premium TempPass, l’applicazione client deve gestire flussi di accesso temporanei che delineano il comportamento degli endpoint REST API v2 in tali scenari.

Nelle versioni API precedenti, l’applicazione client doveva disconnettersi da un utente autenticato con il proprio MVPD regolare per offrire un accesso temporaneo.

Con REST API v2, l’applicazione client può passare facilmente da un normale MVPD a un TempPass MVPD durante l’autorizzazione di un flusso, eliminando la necessità di disconnettersi dall’utente.

Per ulteriori dettagli, consulta la documentazione [Flussi di accesso temporanei](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).

#### &#x200B;21. In che modo l&#39;applicazione client deve gestire l&#39;accesso single sign-on tra dispositivi? {#authentication-phase-faq21}

REST API v2 può abilitare il single sign-on tra dispositivi se l’applicazione client fornisce un identificatore utente univoco coerente tra i dispositivi.

Questo identificatore, noto come token di servizio, deve essere generato dall’applicazione client tramite l’implementazione o l’integrazione di un servizio Identity esterno di scelta.

Per ulteriori dettagli, consulta la documentazione [Single Sign-On using Service Token Flow](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

+++

### Domande frequenti sulla fase di pre-autorizzazione {#preauthorization-phase-faqs-general}

+++Domande frequenti sulla fase di pre-autorizzazione

#### &#x200B;1. Qual è lo scopo della fase di pre-autorizzazione? {#preauthorization-phase-faq1}

Lo scopo della fase di pre-autorizzazione è quello di fornire all’applicazione client la capacità di presentare un sottoinsieme di risorse dal suo catalogo a cui l’utente avrebbe diritto di accedere.

La fase di preautorizzazione può migliorare l’esperienza utente quando l’utente apre l’applicazione client per la prima volta o passa a una nuova sezione.

#### &#x200B;2. La fase di pre-autorizzazione è obbligatoria? {#preauthorization-phase-faq2}

La fase di pre-autorizzazione non è obbligatoria, l’applicazione client può saltare questa fase se desidera presentare un catalogo di risorse senza filtrarle prima in base al diritto dell’utente.

#### &#x200B;3. Cos&#39;è una decisione di pre-autorizzazione? {#preauthorization-phase-faq3}

La pre-autorizzazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization), mentre il termine della decisione si trova anche nel [Glossary](rest-api-v2-glossary.md#decision).

La decisione di preautorizzazione memorizza informazioni sul risultato dell’interrogazione del processo di preautorizzazione di MVPD che possono essere recuperate dall’endpoint di preautorizzazione delle decisioni.

L’applicazione client può utilizzare le decisioni di preautorizzazione per presentare un sottoinsieme di risorse a cui l’utente può accedere in base alle decisioni (informative) del fornitore TV.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Recuperare l’API per le decisioni di preautorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Flusso di pre-autorizzazione di base eseguito nell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### &#x200B;4. L&#39;applicazione client deve memorizzare nella cache le decisioni di preautorizzazione in un archivio persistente? {#preauthorization-phase-faq4}

L&#39;applicazione client non è necessaria per archiviare le decisioni di preautorizzazione nell&#39;archiviazione persistente. Tuttavia, si consiglia di memorizzare nella cache le decisioni sulle autorizzazioni per migliorare l’esperienza utente. Questo consente di evitare inutili chiamate all’endpoint Decisions Preauthorize per le risorse che sono già state preautorizzate, riducendo la latenza e migliorando le prestazioni.

#### &#x200B;5. In che modo l&#39;applicazione client può determinare il motivo per cui è stata negata una decisione di preautorizzazione? {#preauthorization-phase-faq5}

L&#39;applicazione client può determinare il motivo di una decisione di preautorizzazione negata esaminando il codice di errore [e il messaggio](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) inclusi nella risposta dall&#39;endpoint di preautorizzazione delle decisioni. Questi dettagli forniscono ad insight il motivo specifico per cui la richiesta di preautorizzazione è stata negata, aiutando a informare l’esperienza utente o a attivare eventuali operazioni necessarie nell’applicazione.

Assicurati che eventuali meccanismi di esecuzione di nuovi tentativi implementati per recuperare le decisioni di preautorizzazione non generino un ciclo infinito se la decisione di preautorizzazione viene negata.

Valuta la possibilità di limitare i nuovi tentativi a un numero ragionevole e di gestire i rifiuti in modo appropriato fornendo all’utente un feedback chiaro.

#### &#x200B;6. Perché nella decisione di preautorizzazione manca un token multimediale? {#preauthorization-phase-faq6}

Nella decisione di pre-autorizzazione manca un token multimediale perché la fase di pre-autorizzazione non deve essere utilizzata per riprodurre le risorse, in quanto questo è lo scopo della fase di autorizzazione.

#### &#x200B;7. È possibile saltare la fase di autorizzazione se esiste già una decisione di preautorizzazione? {#preauthorization-phase-faq7}

No.

La fase di autorizzazione non può essere ignorata anche se è disponibile una decisione di preautorizzazione. Le decisioni di pre-autorizzazione sono solo informative e non concedono i diritti di riproduzione effettivi. La fase di pre-autorizzazione ha lo scopo di fornire indicazioni preliminari, ma la fase di autorizzazione è ancora necessaria prima di riprodurre qualsiasi contenuto.

#### &#x200B;8. Che cos’è una risorsa e quali formati sono supportati? {#preauthorization-phase-faq8}

La risorsa è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

La risorsa è un identificatore univoco concordato con gli MVPD ed è associata a un contenuto che l’applicazione client può inviare in streaming.

L’identificatore univoco della risorsa può avere due formati:

* Un formato di stringa semplice, ad esempio un identificatore univoco per un canale (brand).
* Un formato RSS per contenuti multimediali (MRSS) contenente informazioni aggiuntive come il titolo, le valutazioni e i metadati per il controllo genitori.

Per ulteriori dettagli, consulta la documentazione di [Risorse protette](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### &#x200B;9. Per quante risorse l&#39;applicazione client può ottenere una decisione di preautorizzazione alla volta? {#preauthorization-phase-faq9}

L’applicazione client può ottenere una decisione di preautorizzazione per un numero limitato di risorse in una singola richiesta API, di solito fino a 5, a causa delle condizioni imposte dagli MVPD.

Questo numero massimo di risorse può essere visualizzato e modificato dopo aver concordato con gli MVPD tramite Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di autenticazione Adobe Pass che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

+++

### Domande frequenti sulla fase di autorizzazione {#authorization-phase-faqs-general}

+++Domande frequenti sulla fase di autorizzazione

#### &#x200B;1. Qual è lo scopo della fase di autorizzazione? {#authorization-phase-faq1}

Lo scopo della fase di autorizzazione è quello di fornire all’applicazione client la capacità di riprodurre le risorse richieste dall’utente dopo aver convalidato i propri diritti con MVPD.

#### &#x200B;2. La fase di autorizzazione è obbligatoria? {#authorization-phase-faq2}

La fase di autorizzazione è obbligatoria, l’applicazione client non può saltare questa fase se desidera riprodurre le risorse richieste dall’utente, in quanto richiede la verifica con il MVPD che l’utente abbia diritto prima di rilasciare il flusso.

#### &#x200B;3. Cos&#39;è una decisione di autorizzazione e per quanto tempo è valida? {#authorization-phase-faq3}

L&#39;autorizzazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization), mentre il termine della decisione si trova anche nel [Glossary](rest-api-v2-glossary.md#decision).

La decisione di autorizzazione memorizza informazioni sul risultato della richiesta di informazioni del processo di autorizzazione di MVPD che possono essere recuperate dall’endpoint &quot;Decisions Authorize&quot;.

L’applicazione client può utilizzare la decisione di autorizzazione contenente un token multimediale per riprodurre il flusso di risorse nel caso in cui la decisione del fornitore TV (autorevole) consenta all’utente di accedervi.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Recuperare l’API per le decisioni di autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

La decisione di autorizzazione è valida per un periodo di tempo limitato e breve specificato al momento del rilascio, che indica il periodo di tempo in cui verrà memorizzata nella cache dall’autenticazione di Adobe Pass prima che venga richiesto di eseguire nuovamente la query sul MVPD.

Questo periodo di tempo limitato, noto come autorizzazione (authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl), può essere visualizzato e modificato tramite Adobe Pass [TVE Dashboard](rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### &#x200B;4. L&#39;applicazione client deve memorizzare nella cache le decisioni di autorizzazione in un archivio persistente? {#authorization-phase-faq4}

L&#39;applicazione client non è necessaria per archiviare le decisioni di autorizzazione nell&#39;archivio permanente.

#### &#x200B;5. In che modo l&#39;applicazione client può determinare il motivo per cui è stata negata una decisione di autorizzazione? {#authorization-phase-faq5}

L&#39;applicazione client può determinare il motivo di una decisione di autorizzazione negata esaminando il [codice di errore e il messaggio](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) inclusi nella risposta dall&#39;endpoint Decisions Authorize. Questi dettagli forniscono ad insight il motivo specifico per cui la richiesta di autorizzazione è stata negata, aiutando a informare l’esperienza utente o a attivare eventuali operazioni necessarie nell’applicazione.

Assicurati che eventuali meccanismi di esecuzione di nuovi tentativi implementati per recuperare le decisioni di autorizzazione non generino un loop infinito se la decisione di autorizzazione viene negata.

Valuta la possibilità di limitare i nuovi tentativi a un numero ragionevole e di gestire i rifiuti in modo appropriato fornendo all’utente un feedback chiaro.

#### &#x200B;6. Cos’è un token multimediale e per quanto tempo è valido? {#authorization-phase-faq6}

Il token multimediale è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token).

Il token multimediale è costituito da una stringa firmata inviata in testo non crittografato che può essere recuperata dall’endpoint Autorizzazione decisioni.

Per ulteriori informazioni, consulta la documentazione di [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

Il token multimediale è valido per un periodo di tempo limitato e breve specificato al momento del rilascio, che indica il limite di tempo prima che debba essere verificato e utilizzato dall’applicazione client.

L’applicazione client può utilizzare il token multimediale per riprodurre un flusso di risorse nel caso in cui la decisione del fornitore TV (autorevole) consenta all’utente di accedervi.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Recuperare l’API per le decisioni di autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### &#x200B;7. L’applicazione client deve convalidare il token multimediale prima di riprodurre il flusso di risorse? {#authorization-phase-faq7}

Sì.

L’applicazione client deve convalidare il token multimediale prima di avviare la riproduzione del flusso di risorse. Questa convalida deve essere eseguita utilizzando [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier). Verificando `serializedToken` da `token` restituito, l&#39;applicazione client impedisce l&#39;accesso non autorizzato, ad esempio la copia di flusso, e garantisce che solo gli utenti autorizzati possano riprodurre il contenuto.

#### &#x200B;8. L’applicazione client deve aggiornare un token multimediale scaduto durante la riproduzione? {#authorization-phase-faq8}

No.

Non è necessario che l’applicazione client aggiorni un token multimediale scaduto mentre il flusso è in riproduzione attiva. Se il token multimediale scade durante la riproduzione, il flusso deve poter continuare senza interruzioni. Tuttavia, il client deve richiedere una nuova decisione di autorizzazione e ottenere un nuovo token multimediale la volta successiva che l’utente tenta di riprodurre una risorsa.

#### &#x200B;9. Qual è lo scopo di ogni attributo di marca temporale nella decisione di autorizzazione? {#authorization-phase-faq9}

La decisione di autorizzazione include diversi attributi di marca temporale che forniscono un contesto essenziale sul periodo di validità dell’autorizzazione stessa e del token multimediale associato. Queste marche temporali hanno scopi diversi, a seconda che si riferiscano alla decisione di autorizzazione o al token multimediale.

**Marca temporale a livello di decisione**

Le marche temporali descrivono il periodo di validità della decisione globale di autorizzazione:

| Attributo | Descrizione | Note |
|-------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Tempo in millisecondi in cui è stata rilasciata la decisione di autorizzazione. | Indica l&#39;inizio della finestra di validità dell&#39;autorizzazione. |
| `notAfter` | Tempo in millisecondi in cui scade la decisione di autorizzazione. | Il valore TTL [authorization Time-to-Live](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management) determina per quanto tempo l&#39;autorizzazione rimane valida prima di richiedere una nuova autorizzazione. Questo TTL viene negoziato con i rappresentanti di MVPD. |

**Marca temporale a livello di token**

Queste marche temporali descrivono il periodo di validità del token multimediale associato alla decisione di autorizzazione:

| Attributo | Descrizione | Note |
|-------------|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Tempo in millisecondi in cui è stato emesso il token multimediale. | Indica quando il token diventa valido per la riproduzione. |
| `notAfter` | Tempo in millisecondi in cui scade il token multimediale. | I token multimediali hanno una durata intenzionalmente breve (in genere 7 minuti) per ridurre al minimo i rischi di uso improprio e tenere conto delle potenziali differenze di clock tra il server che genera i token e il server che verifica i token. |

#### &#x200B;10. Che cos’è una risorsa e quali formati sono supportati? {#authorization-phase-faq10}

La risorsa è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

La risorsa è un identificatore univoco concordato con gli MVPD ed è associata a un contenuto che l’applicazione client può inviare in streaming.

L’identificatore univoco della risorsa può avere due formati:

* Un formato di stringa semplice, ad esempio un identificatore univoco per un canale (brand).
* Un formato RSS per contenuti multimediali (MRSS) contenente informazioni aggiuntive come il titolo, le valutazioni e i metadati per il controllo genitori.

Per ulteriori dettagli, consulta la documentazione di [Risorse protette](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### &#x200B;11. Per quante risorse l&#39;applicazione client può ottenere una decisione di autorizzazione alla volta? {#authorization-phase-faq11}

L’applicazione client può ottenere una decisione di autorizzazione per un numero limitato di risorse in una singola richiesta API, di solito fino a 1, a causa delle condizioni imposte dagli MVPD.

+++

### Domande frequenti sulla fase di disconnessione {#logout-phase-faqs-general}

+++Domande frequenti sulla fase di disconnessione

#### &#x200B;1. Qual è lo scopo della fase di disconnessione? {#logout-phase-faq1}

Lo scopo della fase di disconnessione è fornire all’applicazione client la funzionalità per terminare il profilo autenticato dell’utente nell’ambito dell’autenticazione di Adobe Pass su richiesta dell’utente.

#### &#x200B;2. La fase di disconnessione è obbligatoria? {#logout-phase-faq2}

La fase di disconnessione è obbligatoria, l&#39;applicazione client deve fornire all&#39;utente la possibilità di disconnettersi.

+++

### Domande frequenti sulle intestazioni {#headers-faqs-general}

+++Domande frequenti sulle intestazioni

#### &#x200B;1. Come si calcola il valore per l’intestazione Autorizzazione? {#headers-faq1}

>[!IMPORTANT]
>
> Se l&#39;applicazione client esegue la migrazione dall&#39;API REST V1 all&#39;API REST V2, l&#39;applicazione client può continuare a utilizzare lo stesso metodo per ottenere il valore del token di accesso `Bearer` come in precedenza.

L&#39;intestazione della richiesta [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) contiene il token di accesso `Bearer` richiesto dall&#39;applicazione client per accedere alle API protette di Adobe Pass.

Il valore dell’intestazione Authorization deve essere ottenuto dall’autenticazione di Adobe Pass durante la fase di registrazione.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Panoramica di Registrazione client dinamici](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Recupera API credenziali client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recupera API token di accesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flusso di registrazione client dinamici](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### &#x200B;2. Come si calcola il valore per l’intestazione AP-Device-Identifier? {#headers-faq2}

>[!IMPORTANT]
>
> Se l’applicazione client esegue la migrazione dall’API REST V1 all’API REST V2, può continuare a utilizzare lo stesso metodo per calcolare il valore dell’identificatore del dispositivo come in precedenza.

L&#39;intestazione della richiesta [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) contiene l&#39;identificatore del dispositivo di streaming creato dall&#39;applicazione client.

La documentazione dell&#39;intestazione [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) fornisce esempi per le piattaforme principali su come calcolare il valore, ma l&#39;applicazione client può scegliere di utilizzare un metodo diverso in base alla propria logica di business e ai propri requisiti.

#### &#x200B;3. Come si calcola il valore per l’intestazione X-Device-Info? {#headers-faq3}

>[!IMPORTANT]
>
> Se l’applicazione client esegue la migrazione dall’API REST V1 all’API REST V2, può continuare a utilizzare lo stesso metodo per calcolare il valore delle informazioni sul dispositivo.

L&#39;intestazione della richiesta [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) contiene le informazioni client (dispositivo, connessione e applicazione) relative al dispositivo di streaming effettivo e viene utilizzata per determinare le regole specifiche della piattaforma che gli MVPD possono applicare.

La documentazione dell&#39;intestazione [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) fornisce esempi per le piattaforme principali su come calcolare il valore, ma l&#39;applicazione client può scegliere di utilizzare un metodo diverso in base alla propria logica di business e ai propri requisiti.

Se l&#39;intestazione [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) è mancante o contiene valori non corretti, la richiesta potrebbe essere classificata come proveniente da una piattaforma `unknown`.

Questo può comportare che la richiesta venga trattata come non sicura e soggetta a regole più restrittive, ad esempio TTL di autenticazione più brevi. Inoltre, alcuni campi, ad esempio il dispositivo di streaming `connectionIp` e `connectionPort`, sono obbligatori per funzionalità quali [Autenticazione di base di base di Spectrum](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md).

Anche quando la richiesta proviene da un server per conto di un dispositivo, il valore dell&#39;intestazione [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) deve riflettere le informazioni effettive sul dispositivo di streaming.

+++

### Domande frequenti varie {#misc-faqs-general}

+++Domande frequenti varie

#### &#x200B;1. Posso esplorare le richieste e le risposte REST API V2 e testare l’API? {#misc-faq1}

Sì.

Puoi esplorare REST API V2 tramite il nostro sito Web dedicato [Adobe Developer](https://developer.adobe.com/adobe-pass/). Il sito web di Adobe Developer fornisce accesso illimitato a:

* [API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Per interagire con [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/), è necessario includere l&#39;intestazione [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) con un token di accesso `Bearer` ottenuto tramite l&#39;API [DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Per utilizzare l&#39;API [DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), è necessaria un&#39;istruzione software con ambito REST API V2. Per ulteriori dettagli, consulta il documento [Domande frequenti sulla registrazione client dinamica (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

#### &#x200B;2. Posso esplorare richieste e risposte REST API V2 utilizzando uno strumento di sviluppo API con supporto OpenAPI? {#misc-faq2}

Sì.

Puoi scaricare i file delle specifiche OpenAPI per l&#39;[API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) e l&#39;[API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) dal sito Web [Adobe Developer](https://developer.adobe.com/adobe-pass/).

Per scaricare i file delle specifiche OpenAPI, fare clic sui pulsanti di download per salvare i seguenti file nel computer locale:

* [JSON API DCR](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [JSON REST API V2](https://developer.adobe.com/adobe-pass/restApiV2.json)

Puoi quindi importare questi file nello strumento di sviluppo API preferito per esplorare le richieste e le risposte REST API V2 e testare l’API.

#### &#x200B;3. Posso ancora utilizzare lo strumento di test API esistente ospitato su https://sp.auth-staging.adobe.com/apitest/api.html? {#misc-faq3}

No.

Le applicazioni client che eseguono la migrazione all’API REST V2 devono utilizzare il nuovo strumento di test ospitato in https://developer.adobe.com/adobe-pass/. Il sito web di Adobe Developer fornisce accesso illimitato a:

* [API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Per interagire con [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/), è necessario includere l&#39;intestazione [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) con un token di accesso `Bearer` ottenuto tramite l&#39;API [DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Per utilizzare l&#39;API [DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), è necessaria un&#39;istruzione software con ambito REST API V2. Per ulteriori dettagli, consulta il documento [Domande frequenti sulla registrazione client dinamica (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

+++

### Domande frequenti su Apple SSO {#apple-sso-general}

+++Domande frequenti su Apple SSO

#### &#x200B;1. Cos’è Apple SSO e come funziona con l’API REST V2? {#apple-sso-faq1}

Apple SSO (Single Sign-On) consente agli utenti di accedere al proprio account di provider TV a livello di dispositivo utilizzando il [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) di Apple, eliminando la necessità di eseguire l&#39;autenticazione app per app.

REST API V2 supporta Partner Single Sign-On (SSO) per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS tramite i flussi dei partner.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Panoramica di Apple SSO](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
* [Manuale Apple SSO (REST API V2)](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
* [Single sign-on con flussi di partner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)

#### &#x200B;2. Quali sono i prerequisiti per l’implementazione di Apple SSO? {#apple-sso-faq2}

Prima di implementare Apple SSO, verifica che siano soddisfatti i seguenti prerequisiti:

**Requisiti dell&#39;applicazione di streaming:**

* Contatta Apple per abilitare il framework dell&#39;account dell&#39;utente con sottoscrizione video [1} come parte del tuo ID team Apple.](https://developer.apple.com/documentation/videosubscriberaccount)
* Configura l&#39;adesione Single Sign-On del sottoscrittore video come parte dell&#39;account Apple Developer.
* Utilizza Xcode versione 8 o successiva e iOS/tvOS versione 10 o successiva.
* Richiedi l’autorizzazione dell’utente per accedere alle informazioni di abbonamento a livello di dispositivo.
* Includi le intestazioni `X-Device-Info` e/o `User-Agent` in modo che il backend di autenticazione Adobe Pass possa identificare la piattaforma del dispositivo.
* Includi l&#39;intestazione `AP-Partner-Framework-Status` con stato framework partner valido in tutte le richieste API pertinenti.

**Configurazione Adobe Pass:**

* Abilitare il Single Sign-On (SSO) per ogni integrazione e piattaforma desiderata (iOS/tvOS) tramite Adobe Pass TVE Dashboard impostando la proprietà `Enable Single Sign On` su `Yes`.

**Requisiti MVPD:**

* MVPD deve essere integrato con Apple per il supporto SSO di Apple.
* Per la configurazione SSO dei partner, è necessario eseguire l’onboarding di MVPD con l’autenticazione di Adobe Pass.

Per ulteriori dettagli, consulta la [Panoramica di Apple SSO - Prerequisiti](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites).

#### &#x200B;3. Cos’è l’intestazione AP-Partner-Framework-Status e perché è necessaria? {#apple-sso-faq3}

L&#39;intestazione `AP-Partner-Framework-Status` contiene le informazioni sullo stato del framework partner recuperate dal framework dell&#39;account del sottoscrittore video di Apple.

**Sempre richiesto:**

* [Recupera richiesta di autenticazione partner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [Crea e recupera il profilo utilizzando la risposta di autenticazione del partner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

**Obbligatorio in base alle condizioni:**

* [Recuperare i profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recupera profilo per mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recuperare le decisioni di autorizzazione utilizzando mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Recuperare le decisioni di pre-autorizzazione utilizzando mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

L’applicazione di streaming deve recuperare queste informazioni chiamando il Framework dell’account del sottoscrittore video e includerlo come payload JSON con codifica base64 nell’intestazione.

**Best practice:**

* L’applicazione di streaming deve recuperare lo stato del framework del partner quando l’applicazione entra in primo piano.
* Memorizza nella cache le informazioni sullo stato del framework del partner e aggiorna quando l’applicazione passa dal background al primo piano.
* Verificare che lo stato del framework partner contenga valori validi (autorizzazione utente concessa, identificatore provider valido, data di scadenza valida).

Per ulteriori dettagli, consulta la documentazione [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

#### &#x200B;4. Come posso risolvere i problemi di SSO di Apple? {#apple-sso-faq4}

Per la risoluzione dei problemi di SSO di Apple, segui questo approccio generale:

**Passaggio 1: verifica generazione richiesta SAML**

* Verificare che l&#39;endpoint [Recupera richiesta di autenticazione partner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) restituisca una richiesta di autenticazione partner valida (richiesta SAML).
* Verificare che l&#39;attributo `authenticationRequest - request` contenga una richiesta SAML formattata correttamente dopo la decodifica base64.
* Verificare che l&#39;intestazione `AP-Partner-Framework-Status` contenga informazioni valide sullo stato del framework partner.

**Passaggio 2: identificazione degli errori VSA**

* Rivedi le risposte di errore dal framework dell’account del sottoscrittore video.
* Per informazioni sui codici di errore e le descrizioni, consulta la documentazione del [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount/vserror#Error-codes).
* Si noti che la documentazione relativa agli errori VSA è piuttosto generica e potrebbe non fornire informazioni dettagliate sulla causa principale.

**Passaggio 3: verifica problemi comuni**

* È necessario concedere lo stato di accesso alle autorizzazioni utente (selezionare `VSAccountAccessStatus.granted`).
* L&#39;identificatore di mapping del provider utente deve essere presente e valido (`accountProviderIdentifier`).
* La data di scadenza del profilo del provider utente deve essere valida (`authenticationExpirationDate`).
* MVPD deve essere integrato con Apple (controlla `boardingStatus` nella risposta dell&#39;endpoint di configurazione).
* L’integrazione MVPD deve avere Apple SSO abilitato in TVE Dashboard.

**Passaggio 4: coinvolgere MVPD per l&#39;investigazione**

* Se il framework dell’account abbonato video riceve una richiesta SAML valida ma non restituisce una risposta SAML a seguito di un’interazione con MVPD, per la risoluzione dei problemi è necessario coinvolgere il MVPD abilitato SSO di Apple.
* Il problema può essere correlato anche alla configurazione o all’implementazione specifica di MVPD sul lato Apple.

#### &#x200B;5. Quali sono gli errori VSA comuni e come gestirli? {#apple-sso-faq5}

Scenari comuni e relativa gestione:

**Autorizzazione negata dall&#39;utente:**

* `VSAccountAccessStatus` non sarà `.granted`.
* Torna al flusso di autenticazione di base e presenta il selettore MVPD dell’applicazione.

**MVPD non integrato con Apple (codice errore 1):**

* Il framework VSA restituisce un errore con `error.code == 1`.
* `error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"]` contiene l&#39;ID MSO di Apple.
* Torna al flusso di autenticazione di base, ma puoi saltare la richiesta all’utente con il selettore MVPD se puoi mappare l’ID MSO di Apple a un MVPD nella tua configurazione.

**Nessun metadati restituito:**

* `vsaMetadata` è `nil` oppure mancano dei campi obbligatori.
* Ripristino del flusso di autenticazione di base.

**Risposta SAML non restituita:**

* `samlAttributeQueryResponse` è `nil` dopo l&#39;autenticazione di MVPD.
* Questo può indicare un problema con l’implementazione Apple SSO di MVPD.
* Prendi in considerazione la possibilità di contattare Adobe, MVPD e Apple per un’indagine.

Per informazioni dettagliate sull&#39;errore, consulta la documentazione [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).

#### &#x200B;6. Cosa indica il tipo di profilo &quot;appleSSO&quot;? {#apple-sso-faq6}

Un profilo con `type` impostato su &quot;appleSSO&quot; indica che l&#39;utente ha eseguito l&#39;autenticazione tramite il flusso Single Sign-On Partner di Apple utilizzando il framework dell&#39;account dell&#39;utente iscritto video.

Questo tipo di profilo ha requisiti specifici:

* Quando si prendono decisioni (pre-autorizzazione/autorizzazione) sulle richieste con un profilo &quot;appleSSO&quot;, l’applicazione di streaming deve includere un’intestazione `AP-Partner-Framework-Status` valida con lo stato attuale del framework del partner.
* Durante la disconnessione, la risposta includerà `actionName` impostato su &quot;partner_logout&quot; e `actionType` impostato su &quot;partner_interactive&quot;, che indica che l&#39;utente deve completare la disconnessione a livello di partner (sistema).

I profili regolari (SSO non Apple) non hanno questi requisiti e seguono i flussi di autenticazione di base.

#### &#x200B;7. Come posso gestire la disconnessione per i profili SSO di Apple? {#apple-sso-faq7}

Quando si avvia la disconnessione per un utente con un profilo di tipo &quot;appleSSO&quot;:

* La risposta dell’endpoint di disconnessione di Adobe Pass includerà:
   * `actionName` impostato su &quot;partner_logout&quot;
   * `actionType` impostato su &quot;partner_interactive&quot;
   * L&#39;attributo `url` sarà mancante

* L’applicazione di streaming deve richiedere all’utente di completare il processo di disconnessione a livello di partner (sistema) andando in:
   * `Settings -> TV Provider` su iOS/iPadOS
   * `Settings -> Accounts -> TV Provider` su tvOS

* Per completare il processo di disconnessione, l&#39;utente deve disconnettersi manualmente dal proprio provider TV a livello di sistema.

Per ulteriori dettagli, consulta la documentazione [Fase di disconnessione](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md#cookbook) del manuale Apple SSO (REST API V2).

#### &#x200B;8. Posso tornare all’autenticazione di base se l’SSO di Apple non riesce? {#apple-sso-faq8}

Sì, l’API REST per l’autenticazione di Adobe Pass V2 torna automaticamente al flusso di autenticazione di base nei seguenti scenari:

**Fallback automatico:**

* La convalida Single Sign-On dei partner non riesce nel back-end di Adobe Pass
* L’utente nega l’autorizzazione per accedere alle informazioni sull’abbonamento
* MVPD non è integrato con Apple
* Il framework VSA non restituisce metadati validi

**Indicazione risposta:**

Quando si torna all&#39;autenticazione di base, la risposta dell&#39;endpoint [Retrieve partner authentication request](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) conterrà:

* `actionName` impostato per &quot;autenticare&quot; o &quot;riprendere&quot;
* `actionType` impostato su &quot;interattivo&quot; o &quot;diretto&quot;

L’applicazione di streaming deve gestire queste risposte avviando il flusso di autenticazione di base.

**Fallback manuale:**

Per disabilitare Apple SSO per un’integrazione specifica e utilizzare sempre l’autenticazione di base:

* Imposta la proprietà `Enable Single Sign On` su `No` nel dashboard TVE di Adobe Pass per l&#39;integrazione e la piattaforma desiderate (iOS/tvOS).

Per ulteriori dettagli, consulta la documentazione di [Single Sign-on tramite i flussi di partner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).

#### &#x200B;9. Dove posso trovare ulteriori informazioni sul framework dell’account dell’abbonato video? {#apple-sso-faq9}

Per informazioni dettagliate sul framework dell’account Video Subscriber di Apple, compresi i riferimenti API, i codici di errore e le linee guida per l’integrazione, consulta la documentazione ufficiale di Apple:

* [Documentazione framework account abbonato video](https://developer.apple.com/documentation/videosubscriberaccount)

Classi e protocolli chiave da esaminare:

* [VSAccountManager](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager) - Responsabile principale per le operazioni dell&#39;account del sottoscrittore
* [VSAccountMetadataRequest](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) - Richiesta di informazioni account sottoscrittore
* [VSAccountMetadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) - Risposta contenente le informazioni sull&#39;account del sottoscrittore
* [VSAccountManagerDelegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) - Delega del protocollo per gli eventi di account manager
* [VSAccountAccessStatus](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountaccessstatus) - Enumerazione stato autorizzazioni utente

+++

## Domande frequenti sulla migrazione {#migration-faqs}

Continuare con questa sezione se si sta lavorando su un&#39;applicazione che deve migrare un&#39;applicazione esistente all&#39;API REST V2.

>[!MORELIKETHIS]
>
> * [Domande frequenti su Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### Domande frequenti sulla migrazione generali {#general-migration-faqs}

+++Domande frequenti sulla migrazione generali

#### &#x200B;1. È necessario eseguire il rollout di una nuova applicazione client migrata all’API REST V2 per tutti gli utenti contemporaneamente? {#migration-faq1}

No.

Non è necessario che l’applicazione client distribuisca una nuova versione che integra l’API REST V2 a tutti gli utenti simultaneamente.

L’autenticazione di Adobe Pass continuerà a supportare le versioni precedenti delle applicazioni client che integrano l’API REST V1 o SDK fino alla fine del 2025.

#### &#x200B;2. È necessario eseguire il rollout di una nuova applicazione client migrata all’API REST V2 in tutte le API e i flussi contemporaneamente? {#migration-faq2}

Sì.

L’applicazione client deve distribuire una nuova versione che integri l’API REST V2 in tutte le API e i flussi di autenticazione di Adobe Pass simultaneamente.

Nel caso del flusso di autenticazione della seconda schermata, l&#39;applicazione client deve eseguire il rollout di una nuova versione che integra l&#39;API REST V2 per le applicazioni [primary](rest-api-v2-glossary.md#primary-application) e [secondary](rest-api-v2-glossary.md#secondary-application) simultaneamente.

L’autenticazione Adobe Pass non supporterà implementazioni &quot;ibride&quot; che integrano sia l’API REST V2 che l’API REST V1/SDK tra API e flussi.

#### &#x200B;3. L’autenticazione utente verrà mantenuta durante l’aggiornamento a una nuova applicazione client migrata all’API REST V2? {#migration-faq3}

No.

L’autenticazione utente ottenuta nelle versioni precedenti dell’applicazione client che integrano l’API REST V1 o SDK non verrà mantenuta.

Pertanto, all’utente verrà richiesto di autenticare nuovamente all’interno della nuova applicazione client migrata all’API REST V2.

#### &#x200B;4. I codici di errore avanzati sono abilitati per impostazione predefinita nell’API REST V2? {#migration-faq4}

Sì.

Per impostazione predefinita, le applicazioni client che eseguono la migrazione all’API REST V2 beneficiano automaticamente di questa funzione, fornendo informazioni di errore più dettagliate e precise.

Per ulteriori dettagli, consulta la documentazione sui [codici di errore avanzati](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

#### &#x200B;5. Le integrazioni esistenti richiedono modifiche alla configurazione durante la migrazione all’API REST V2? {#migration-faq5}

No.

Le applicazioni client che eseguono la migrazione all’API REST V2 non richiedono modifiche di configurazione per le integrazioni MVPD esistenti. Inoltre, continueranno a utilizzare gli stessi identificatori per [provider di servizi](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider) e [MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd) esistenti.

Inoltre, i protocolli utilizzati dall’autenticazione di Adobe Pass per comunicare con gli endpoint di MVPD rimangono invariati.

+++

### Migrazione da REST API V1 a REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Continua con questa sottosezione se stai lavorando su un’applicazione che deve migrare dall’API REST V1 all’API REST V2.

#### Domande frequenti sulla fase di registrazione {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di registrazione

##### &#x200B;1. Quali sono le migrazioni API di alto livello necessarie per la fase di registrazione? {#registration-phase-v1-to-v2-faq1}

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

##### &#x200B;1. Quali sono le migrazioni API di alto livello necessarie per la fase di configurazione? {#configuration-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recupera elenco di MVPD con integrazione attiva | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configurazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Domande frequenti sulla fase di autenticazione {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di autenticazione

##### &#x200B;1. Quali sono le migrazioni API di alto livello necessarie per la fase di autenticazione? {#authentication-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera codice di registrazione (codice di autenticazione) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verifica il codice di registrazione (codice di autenticazione) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Riprendi autenticazione (MVPD) nella seconda schermata (applicazione) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Avvia autenticazione (MVPD) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [GET <br/> /api/v1/checkauthn (prima schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (seconda schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare il token di autenticazione utente (profilo) | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di pre-autorizzazione {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di pre-autorizzazione

##### &#x200B;1. Quali sono le migrazioni API di alto livello richieste per la fase di pre-autorizzazione? {#preauthorization-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [GET <br/> /api/v1/preauthorize (prima schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (seconda schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di autorizzazione {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di autorizzazione

##### &#x200B;1. Quali sono le migrazioni API di alto livello necessarie per la fase di autorizzazione? {#authorization-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autorizzazione (MVPD) | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recupera token di autorizzazione (decisione di autorizzazione) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recupera token di autorizzazione breve (token multimediale) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di disconnessione {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di disconnessione

##### &#x200B;1. Quali sono le migrazioni API di alto livello necessarie per la fase di disconnessione? {#logout-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia disconnessione | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di logout di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migrazione da SDK all’API REST V2 {#migration-sdk-to-rest-api-v2}

Continua con questa sottosezione se stai lavorando su un’applicazione che deve migrare da SDK all’API REST V2.

#### Domande frequenti sulla fase di registrazione {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Domande frequenti sulla fase di registrazione

##### &#x200B;1. Quali sono le migrazioni API di alto livello necessarie per la fase di registrazione? {#registration-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. Quali sono le migrazioni API di alto livello necessarie per la fase di configurazione? {#configuration-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. Quali sono le migrazioni API di alto livello necessarie per la fase di autenticazione? {#authentication-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

###### SDK di AccessEnabler JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler iOS

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera codice di registrazione (codice di autenticazione) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verifica il codice di registrazione (codice di autenticazione) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Riprendi autenticazione (MVPD) nella seconda schermata (applicazione) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler Android

| Ambito | SDK | API REST V2 | Osservazioni |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera codice di registrazione (codice di autenticazione) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verifica il codice di registrazione (codice di autenticazione) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Riprendi autenticazione (MVPD) nella seconda schermata (applicazione) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Avvia autenticazione (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di pre-autorizzazione {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Domande frequenti sulla fase di pre-autorizzazione

##### &#x200B;1. Quali sono le migrazioni API di alto livello richieste per la fase di pre-autorizzazione? {#preauthorization-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

###### SDK di AccessEnabler JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler Android

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di autorizzazione {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Domande frequenti sulla fase di autorizzazione

##### &#x200B;1. Quali sono le migrazioni API di alto livello necessarie per la fase di autorizzazione? {#authorization-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

###### SDK di AccessEnabler JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler Android

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler FireOS

| Ambito | SDK | API REST V2 | Osservazioni |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recupera token di autorizzazione breve (token multimediale) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di disconnessione {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++Domande frequenti sulla fase di disconnessione

##### &#x200B;1. Quali sono le migrazioni API di alto livello necessarie per la fase di disconnessione? {#logout-phase-sdk-to-v2-faq1}

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
