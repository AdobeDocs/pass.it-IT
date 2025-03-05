---
title: Domande frequenti su REST API V2
description: Domande frequenti su REST API V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '8198'
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

### Domande frequenti sulla fase di registrazione {#registration-phase-faqs-general}

+++Domande frequenti sulla fase di registrazione

Consulta la documentazione [Domande frequenti sulla registrazione client dinamica (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs).

+++

### Domande frequenti sulla fase di configurazione {#configuration-phase-faqs-general}

+++Domande frequenti sulla fase di configurazione

#### 1. Qual è lo scopo della fase di configurazione? {#configuration-phase-faq1}

Lo scopo della fase di configurazione è quello di fornire all’applicazione client l’elenco di MVPD con cui è attivamente integrata insieme ai dettagli di configurazione (ad esempio, `id`, `displayName`, `logoUrl`, ecc.) salvati dall’autenticazione di Adobe Pass per ogni MVPD.

La fase di configurazione funge da passaggio preliminare per la fase di autenticazione quando l&#39;applicazione client deve richiedere all&#39;utente di selezionare il proprio provider TV.

#### 2. La fase di configurazione è obbligatoria? {#configuration-phase-faq2}

La fase di configurazione non è obbligatoria, l’applicazione client deve recuperare la configurazione solo quando l’utente deve selezionare il proprio MVPD per l’autenticazione o la nuova autenticazione.

L’applicazione client può saltare questa fase nei seguenti scenari:

* Utente già autenticato.
* All&#39;utente viene offerto l&#39;accesso temporaneo tramite la funzionalità [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) di base o promozionale.
* L’autenticazione dell’utente è scaduta, ma l’applicazione client ha memorizzato nella cache il MVPD selezionato in precedenza come scelta motivata dall’esperienza utente e richiede all’utente di confermare che è ancora un abbonato di quel MVPD.

#### 3. Che cos’è una configurazione e per quanto tempo è valida? {#configuration-phase-faq3}

La configurazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration).

La configurazione contiene un elenco di MVPD definiti dai seguenti attributi `id`, `displayName`, `logoUrl` e così via, che possono essere recuperati dall&#39;endpoint [Configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

L’applicazione client deve recuperare la configurazione solo quando l’utente deve selezionare il proprio MVPD per autenticarsi o autenticarsi di nuovo.

L’applicazione client può utilizzare la risposta di configurazione per presentare un selettore dell’interfaccia utente con le opzioni MVPD disponibili ogni volta che l’utente deve selezionare il proprio provider TV.

Per procedere con le fasi di autenticazione, preautorizzazione, autorizzazione o disconnessione, l&#39;applicazione client deve memorizzare l&#39;identificatore MVPD selezionato dell&#39;utente, come specificato dall&#39;attributo `id` del livello di configurazione di MVPD.

Per ulteriori informazioni, consulta la documentazione di [Recupero configurazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### 4. L&#39;applicazione client deve memorizzare nella cache le informazioni di risposta della configurazione in un archivio persistente? {#configuration-phase-faq4}

L’applicazione client deve recuperare la configurazione solo quando l’utente deve selezionare il proprio MVPD per autenticarsi o autenticarsi di nuovo.

L’applicazione client deve memorizzare nella cache le informazioni sulla risposta di configurazione in un archivio di memoria per evitare richieste non necessarie e migliorare l’esperienza utente nei seguenti casi:

* Utente già autenticato.
* All&#39;utente viene offerto l&#39;accesso temporaneo tramite la funzionalità [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) di base o promozionale.
* L’autenticazione dell’utente è scaduta, ma l’applicazione client ha memorizzato nella cache il MVPD selezionato in precedenza come scelta motivata dall’esperienza utente e richiede all’utente di confermare che è ancora un abbonato di quel MVPD.

#### 5. L&#39;applicazione client può gestire il proprio elenco di MVPD? {#configuration-phase-faq5}

L’applicazione client può gestire il proprio elenco di MVPD, ma è necessario mantenere gli identificatori MVPD sincronizzati con l’autenticazione di Adobe Pass. Pertanto, si consiglia di utilizzare la configurazione fornita dall’autenticazione di Adobe Pass per garantire che l’elenco sia aggiornato e accurato.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) dall&#39;API REST di autenticazione di Adobe Pass V2 se l&#39;identificatore MVPD fornito non è valido o se non dispone di un&#39;integrazione attiva con il provider di servizi [specificato](rest-api-v2-glossary.md#service-provider).

#### 6. L&#39;applicazione client è in grado di filtrare l&#39;elenco di MVPD? {#configuration-phase-faq6}

L’applicazione client può filtrare l’elenco di MVPD forniti nella risposta di configurazione implementando un meccanismo personalizzato in base alla propria logica di business e ai requisiti, ad esempio la posizione dell’utente o la cronologia degli utenti della selezione precedente.

L&#39;applicazione client può filtrare l&#39;elenco di [MVPD TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) o MVPD con la relativa integrazione ancora in fase di sviluppo o test.

#### 7. Cosa succede se l’integrazione con un MVPD è disabilitata e contrassegnata come inattiva? {#configuration-phase-faq7}

Quando l’integrazione con un MVPD è disabilitata e contrassegnata come inattiva, il MVPD viene rimosso dall’elenco degli MVPD forniti nelle risposte di configurazione successive e ci sono due importanti conseguenze da considerare:

* Gli utenti non autenticati di quel MVPD non potranno più completare la fase di autenticazione utilizzando quel MVPD.
* Gli utenti autenticati di tale MVPD non saranno più in grado di completare le fasi di pre-autorizzazione, autorizzazione o disconnessione utilizzando tale MVPD.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) dall&#39;API REST di autenticazione di Adobe Pass V2 se l&#39;utente selezionato per MVPD non dispone più di un&#39;integrazione attiva con il [provider di servizi](rest-api-v2-glossary.md#service-provider) specificato.

#### 8. Cosa succede se l’integrazione con un MVPD è nuovamente abilitata e contrassegnata come attiva? {#configuration-phase-faq8}

Quando l’integrazione con un MVPD viene nuovamente abilitata e contrassegnata come attiva, il MVPD viene incluso nuovamente nell’elenco di MVPD forniti nelle risposte di configurazione successive e ci sono due importanti conseguenze da considerare:

* Gli utenti non autenticati di quel MVPD saranno nuovamente in grado di completare la fase di autenticazione utilizzando quel MVPD.
* Gli utenti autenticati di tale MVPD potranno nuovamente completare le fasi di pre-autorizzazione, autorizzazione o disconnessione utilizzando tale MVPD.

#### 9. Come abilitare o disabilitare l’integrazione con un MVPD? {#configuration-phase-faq9}

Questa operazione può essere completata tramite Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

### Domande frequenti sulla fase di autenticazione {#authentication-phase-faqs-general}

+++Domande frequenti sulla fase di autenticazione

#### 1. Qual è lo scopo della fase di autenticazione? {#authentication-phase-faq1}

Lo scopo della fase di autenticazione è quello di fornire all&#39;applicazione client la capacità di verificare l&#39;identità dell&#39;utente e ottenere le informazioni sui metadati dell&#39;utente.

La fase di autenticazione funge da passaggio preliminare per la fase di pre-autorizzazione o di autorizzazione quando l’applicazione client deve riprodurre il contenuto.

#### 2. Cos’è una sessione di autenticazione e per quanto tempo è valida? {#authentication-phase-faq2}

La sessione di autenticazione è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session).

La sessione di autenticazione memorizza informazioni sul processo di autenticazione avviato che possono essere recuperate dall’endpoint Sessioni.

La sessione di autenticazione è valida per un periodo di tempo limitato e breve specificato al momento del problema dalla marca temporale `notAfter`, che indica il tempo necessario all&#39;utente per completare il processo di autenticazione prima di richiedere il riavvio del flusso.

L&#39;applicazione client può utilizzare la risposta della sessione di autenticazione per sapere come procedere con il processo di autenticazione. Tieni presente che in alcuni casi non è necessario eseguire l’autenticazione dell’utente, ad esempio fornendo un accesso temporaneo o ridotto oppure quando l’utente è già autenticato.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Crea API sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Riprendi API sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 3. Che cos’è un codice di autenticazione e per quanto tempo è valido? {#authentication-phase-faq3}

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

#### 4. In che modo l&#39;applicazione client può sapere se l&#39;utente ha digitato un codice di autenticazione valido e se la sessione di autenticazione non è ancora scaduta? {#authentication-phase-faq4}

L’applicazione client può convalidare il codice di autenticazione digitato dall’utente in un’applicazione secondaria (a schermo) inviando una richiesta a uno degli endpoint sessioni responsabili per riprendere la sessione di autenticazione o recuperare le informazioni sulla sessione di autenticazione associate al codice di autenticazione.

L&#39;applicazione client riceverebbe un [errore](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) se il codice di autenticazione fornito fosse stato digitato in modo errato o se la sessione di autenticazione fosse scaduta.

Per ulteriori informazioni, consultare i documenti [Riprendi sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) e [Recupera sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### 5. In che modo l’applicazione client può sapere se l’utente è già autenticato? {#authentication-phase-faq5}

L’applicazione client può eseguire query su uno dei seguenti endpoint in grado di verificare se un utente è già autenticato e restituire informazioni sul profilo:

* [API dell’endpoint &quot;Profiles&quot;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint &quot;profile&quot; per API MVPD specifiche](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint &quot;profile&quot; per API di codice specifiche (di autenticazione)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Per ulteriori informazioni, consulta i seguenti documenti:

* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 6. Che cos’è un profilo e per quanto tempo è valido? {#authentication-phase-faq6}

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

#### 7. L&#39;applicazione client deve memorizzare le informazioni sul profilo dell&#39;utente in una memoria persistente? {#authentication-phase-faq7}

L’applicazione client deve memorizzare nella cache le informazioni del profilo utente in un archivio persistente per evitare richieste non necessarie e migliorare l’esperienza utente, considerando i seguenti aspetti:

| Attributo | Esperienza utente |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `attributes` | L&#39;applicazione client può utilizzarlo per personalizzare l&#39;esperienza utente in base a [chiavi di metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) diverse (ad esempio, `zip`, `maxRating`, ecc.). |
| `mvpd` | L&#39;applicazione client può utilizzarlo per tenere traccia del provider TV selezionato dall&#39;utente.<br/><br/>Alla scadenza del profilo utente corrente, l&#39;applicazione client può utilizzare la selezione di MVPD memorizzata e chiedere conferma all&#39;utente. |
| `notAfter` | L’applicazione client può utilizzarla per tenere traccia della data di scadenza del profilo utente e attivare il processo di riautenticazione alla scadenza, evitando errori durante le fasi di pre-autorizzazione o autorizzazione.<br/><br/>La gestione degli errori dell&#39;applicazione client deve essere in grado di gestire il codice di errore [authenticated_profile_expiry](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2), che indica che l&#39;applicazione client richiede la riautenticazione dell&#39;utente. |

#### 8. L’applicazione client può estendere il profilo dell’utente senza richiedere una nuova autenticazione? {#authentication-phase-faq8}

No.

Il profilo utente non può essere esteso oltre la sua validità senza l’interazione dell’utente, in quanto la sua scadenza è determinata dal TTL di autenticazione stabilito con gli MVPD.

Pertanto, l’applicazione client deve richiedere all’utente di autenticarsi nuovamente e interagire con la pagina di accesso di MVPD per aggiornare il proprio profilo sul sistema.

Tuttavia, per gli MVPD che supportano [l&#39;autenticazione basata su home](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) (HBA), l&#39;utente non dovrà immettere le credenziali.

#### 9. Quali sono i casi d’uso per ogni endpoint dei profili disponibile? {#authentication-phase-faq9}

Gli endpoint Profili sono progettati per fornire all’applicazione client la funzionalità di conoscere lo stato di autenticazione dell’utente, accedere alle informazioni sui metadati dell’utente, trovare il metodo utilizzato per l’autenticazione o l’entità utilizzata per fornire l’identità.

Ogni endpoint è adatto a un caso d’uso specifico, come segue:

| API | Descrizione | Caso d’uso |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [API endpoint profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | Recupera tutti i profili utente. | **L&#39;utente apre l&#39;applicazione client per la prima volta**<br/><br/> In questo scenario, l&#39;applicazione client non ha l&#39;identificatore MVPD selezionato dell&#39;utente memorizzato nella cache dell&#39;archivio permanente.<br/><br/>Di conseguenza, invierà una singola richiesta per recuperare tutti i profili utente disponibili. |
| [Endpoint dei profili per API MVPD specifica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | Recupera il profilo utente associato a un MVPD specifico. | **L&#39;utente ritorna all&#39;applicazione client dopo l&#39;autenticazione in una visita precedente**<br/><br/> In questo caso, l&#39;applicazione client deve avere l&#39;identificatore MVPD selezionato in precedenza memorizzato nella cache dell&#39;archivio permanente.<br/><br/>Di conseguenza, invierà una singola richiesta per recuperare il profilo dell&#39;utente per quel MVPD specifico. |
| [Endpoint dei profili per API di codice specifico (autenticazione)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Recupera il profilo utente associato a un codice di autenticazione specifico. | **L&#39;utente avvia il processo di autenticazione**<br/><br/> In questo caso, l&#39;applicazione client deve determinare se l&#39;utente ha completato correttamente l&#39;autenticazione e recuperare le informazioni sul profilo.<br/><br/>Verrà quindi avviato un meccanismo di polling per recuperare il profilo dell&#39;utente associato al codice di autenticazione. |

#### 10. Cosa deve fare l’applicazione client se l’utente dispone di più profili MVPD? {#authentication-phase-faq10}

Quando l’utente dispone di più profili MVPD, l’applicazione client è responsabile di determinare l’approccio migliore per gestire questo scenario.

L’applicazione client può scegliere di richiedere all’utente di selezionare il profilo MVPD desiderato o di effettuare la selezione automaticamente, ad esempio scegliendo il primo profilo utente dalla risposta o quello con il periodo di validità più lungo.

#### 11. Cosa succede quando i profili utente scadono? {#authentication-phase-faq11}

Quando i profili utente scadono, non vengono più inclusi nella risposta dall’endpoint &quot;Profiles&quot;.

Se l’endpoint Profili restituisce una risposta di mappa dei profili vuota, l’applicazione client deve creare una nuova sessione di autenticazione e richiedere all’utente di ripetere l’autenticazione.

Per ulteriori informazioni, consulta la documentazione [Creare l&#39;API della sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

#### 12. Quando i profili utente non sono più validi? {#authentication-phase-faq12}

I profili utente non sono più validi nei seguenti scenari:

* Quando scade il TTL di autenticazione, come indicato dalla marca temporale `notAfter` nella risposta dell’endpoint Profili.
* Quando l&#39;applicazione client modifica il valore dell&#39;intestazione [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md).
* Quando l&#39;applicazione client aggiorna le credenziali client utilizzate per recuperare il valore dell&#39;intestazione [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md).
* Quando l&#39;applicazione client revoca o aggiorna l&#39;istruzione software utilizzata per ottenere le credenziali del client.

#### 13. Quando l’applicazione client deve avviare il meccanismo di polling? {#authentication-phase-faq13}

Per garantire l’efficienza ed evitare richieste non necessarie, l’applicazione client deve avviare il meccanismo di polling nelle seguenti condizioni:

**Autenticazione eseguita nell&#39;applicazione primaria (schermata)**

L&#39;applicazione primaria (streaming) deve avviare il polling quando l&#39;utente raggiunge la pagina di destinazione finale, dopo che il componente del browser carica l&#39;URL specificato per il parametro `redirectUrl` nella richiesta dell&#39;endpoint [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

**Autenticazione eseguita all&#39;interno di un&#39;applicazione secondaria (schermo)**

L&#39;applicazione principale (streaming) deve avviare il polling non appena l&#39;utente avvia il processo di autenticazione, subito dopo aver ricevuto la risposta dell&#39;endpoint [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) e aver visualizzato il codice di autenticazione per l&#39;utente.

#### 14. Quando arrestare il meccanismo di polling nell&#39;applicazione client? {#authentication-phase-faq14}

Per garantire l’efficienza ed evitare richieste non necessarie, l’applicazione client deve arrestare il meccanismo di polling nelle seguenti condizioni:

**Autenticazione completata**

Le informazioni del profilo dell’utente vengono recuperate correttamente, confermando il loro stato di autenticazione. A questo punto, il polling non è più necessario.

**Sessione di autenticazione e scadenza del codice**

La sessione di autenticazione e il codice scadono, come indicato dalla marca temporale `notAfter` nella risposta dell&#39;endpoint [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). In questo caso, l’utente deve riavviare il processo di autenticazione e arrestare immediatamente il polling che utilizza il codice di autenticazione precedente.

**Nuovo codice di autenticazione generato**

Se l’utente richiede un nuovo codice di autenticazione sul dispositivo principale (schermo), la sessione esistente non è più valida e il polling che utilizza il codice di autenticazione precedente deve essere interrotto immediatamente.

#### 15. Quale intervallo tra le chiamate deve essere utilizzato dall&#39;applicazione client per il meccanismo di polling? {#authentication-phase-faq15}

Per garantire l&#39;efficienza ed evitare richieste non necessarie, l&#39;applicazione client deve configurare la frequenza del meccanismo di polling nelle seguenti condizioni:

| **Autenticazione eseguita nell&#39;applicazione primaria (schermata)** | **Autenticazione eseguita all&#39;interno di un&#39;applicazione secondaria (schermo)** |
|----------------------------------------------------------------------|----------------------------------------------------------------------|
| L’applicazione principale (streaming) deve eseguire il polling ogni 1-5 secondi. | L’applicazione principale (streaming) deve eseguire il polling ogni 3-5 secondi. |

#### 16. Qual è il numero massimo di richieste di polling che l&#39;applicazione client può inviare? {#authentication-phase-faq16}

L&#39;applicazione client deve rispettare i limiti correnti definiti dal meccanismo di limitazione dell&#39;autenticazione di Adobe Pass [](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits).

La gestione degli errori dell&#39;applicazione client deve essere in grado di gestire il codice di errore [429 Troppe richieste](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response), che indica che l&#39;applicazione client ha superato il numero massimo di richieste consentito.

Per ulteriori dettagli, consulta la documentazione sul [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

#### 17. Come può l&#39;applicazione client ottenere le informazioni sui metadati dell&#39;utente? {#authentication-phase-faq17}

L&#39;applicazione client può eseguire una query su uno dei seguenti endpoint in grado di restituire [informazioni sui metadati dell&#39;utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) come parte delle informazioni del profilo:

* [API dell’endpoint &quot;Profiles&quot;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint &quot;profile&quot; per API MVPD specifiche](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint &quot;profile&quot; per API di codice specifiche (di autenticazione)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

L’applicazione client non deve eseguire query su un endpoint separato per recuperare le informazioni sui metadati dell’utente, in quanto questo è già incluso nelle informazioni sul profilo ottenute durante la verifica dell’autenticazione dell’utente.

Per ulteriori informazioni, consulta i seguenti documenti:

* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 18. In che modo l&#39;applicazione client deve gestire l&#39;accesso danneggiato? {#authentication-phase-faq18}

Dato che la tua organizzazione intende utilizzare la funzionalità [degradation](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md), l&#39;applicazione client deve gestire flussi di accesso degradati, che delineano il comportamento degli endpoint REST API v2 in tali scenari.

Per ulteriori dettagli, consulta la documentazione sui [flussi di accesso danneggiati](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).

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

#### 2. Come calcolare il valore per l’intestazione AP-Device-Identifier? {#headers-faq2}

>[!IMPORTANT]
>
> Se l’applicazione client esegue la migrazione dall’API REST V1 all’API REST V2, può continuare a utilizzare lo stesso metodo per calcolare il valore dell’identificatore del dispositivo come in precedenza.

L&#39;intestazione della richiesta [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) contiene l&#39;identificatore del dispositivo di streaming creato dall&#39;applicazione client.

La documentazione dell&#39;intestazione [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) fornisce esempi per le piattaforme principali su come calcolare il valore, ma l&#39;applicazione client può scegliere di utilizzare un metodo diverso in base alla propria logica di business e ai propri requisiti.

#### 3. Come calcolare il valore per l’intestazione X-Device-Info? {#headers-faq3}

>[!IMPORTANT]
>
> Se l’applicazione client esegue la migrazione dall’API REST V1 all’API REST V2, può continuare a utilizzare lo stesso metodo per calcolare il valore delle informazioni sul dispositivo.

L&#39;intestazione della richiesta [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) contiene le informazioni client (dispositivo, connessione e applicazione) relative al dispositivo di streaming effettivo.

La documentazione dell&#39;intestazione [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) fornisce esempi per le piattaforme principali su come calcolare il valore, ma l&#39;applicazione client può scegliere di utilizzare un metodo diverso in base alla propria logica di business e ai propri requisiti.

+++

### Domande frequenti varie {#misc-faqs-general}

+++Domande frequenti varie

#### 1. Posso esplorare le richieste e le risposte REST API V2 e testare l’API? {#misc-faq1}

Sì.

Puoi esplorare REST API V2 tramite il nostro sito Web dedicato [Adobe Developer](https://developer.adobe.com/adobe-pass/). Il sito web di Adobe Developer fornisce accesso illimitato a:

* [API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Per interagire con [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/), è necessario includere l&#39;intestazione [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) con un token di accesso `Bearer` ottenuto tramite l&#39;API [DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Per utilizzare l&#39;API [DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), è necessaria un&#39;istruzione software con ambito REST API V2. Per ulteriori dettagli, consulta il documento [Domande frequenti sulla registrazione client dinamica (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

#### 2. Posso esplorare le richieste e le risposte REST API V2 utilizzando uno strumento di sviluppo API con supporto OpenAPI? {#misc-faq2}

Sì.

Puoi scaricare i file delle specifiche OpenAPI per l&#39;[API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) e l&#39;[API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) dal sito Web [Adobe Developer](https://developer.adobe.com/adobe-pass/).

Per scaricare i file delle specifiche OpenAPI, fare clic sui pulsanti di download per salvare i seguenti file nel computer locale:

* [JSON API DCR](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [JSON REST API V2](https://developer.adobe.com/adobe-pass/restApiV2.json)

Puoi quindi importare questi file nello strumento di sviluppo API preferito per esplorare le richieste e le risposte REST API V2 e testare l’API.

#### 3. Posso continuare a utilizzare lo strumento di test API esistente ospitato su https://sp.auth-staging.adobe.com/apitest/api.html? {#misc-faq3}

No.

Le applicazioni client che eseguono la migrazione all’API REST V2 devono utilizzare il nuovo strumento di test ospitato in https://developer.adobe.com/adobe-pass/. Il sito web di Adobe Developer fornisce accesso illimitato a:

* [API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Per interagire con [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/), è necessario includere l&#39;intestazione [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) con un token di accesso `Bearer` ottenuto tramite l&#39;API [DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Per utilizzare l&#39;API [DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), è necessaria un&#39;istruzione software con ambito REST API V2. Per ulteriori dettagli, consulta il documento [Domande frequenti sulla registrazione client dinamica (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

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

Per impostazione predefinita, le applicazioni client che eseguono la migrazione all’API REST V2 beneficiano automaticamente di questa funzione, fornendo informazioni di errore più dettagliate e precise.

Per ulteriori dettagli, consulta la documentazione sui [codici di errore avanzati](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

#### 5. Le integrazioni esistenti richiedono modifiche alla configurazione durante la migrazione all’API REST V2? {#migration-faq5}

No.

Le applicazioni client che eseguono la migrazione all’API REST V2 non richiedono modifiche di configurazione per le integrazioni MVPD esistenti. Inoltre, continueranno a utilizzare gli stessi identificatori per [provider di servizi](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider) e [MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd) esistenti.

Inoltre, i protocolli utilizzati dall’autenticazione di Adobe Pass per comunicare con gli endpoint di MVPD rimangono invariati.

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
| Verifica il codice di registrazione (codice di autenticazione) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Riprendi autenticazione (MVPD) nella seconda schermata (applicazione) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Avvia autenticazione (MVPD) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificare lo stato di autenticazione dell’utente | [GET <br/> /api/v1/checkauthn (prima schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (seconda schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare il token di autenticazione utente (profilo) | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperare le informazioni sui metadati dell’utente | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Utilizzare uno dei seguenti: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;applicazione client può utilizzare la risposta di queste API per più scopi contemporaneamente: <br/> <ul><li>Verificare lo stato di autenticazione dell’utente</li><li>Recupera profilo utente</li><li>Recuperare le informazioni sui metadati dell’utente</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flusso dei profili di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di pre-autorizzazione {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di preautorizzazione

##### 1. Quali sono le migrazioni API di alto livello richieste per la fase di pre-autorizzazione? {#preauthorization-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [GET <br/> /api/v1/preauthorize (prima schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (seconda schermata)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di autorizzazione {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Domande frequenti sulla fase di autorizzazione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di autorizzazione? {#authorization-phase-v1-to-v2-faq1}

Nella migrazione dall’API REST V1 all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nella tabella seguente:

| Ambito | API REST V1 | API REST V2 | Osservazioni |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Avvia autorizzazione (MVPD) | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recupera token di autorizzazione (decisione di autorizzazione) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recupera token di autorizzazione breve (token multimediale) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;applicazione client può utilizzare la risposta di questa API per più scopi contemporaneamente: <br/> <ul><li>Avvia autorizzazione (MVPD)</li><li>Recupera decisione di autorizzazione</li><li>Recupera token multimediale breve</li></ul> <br/> Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

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

+++Domande frequenti sulla fase di preautorizzazione

##### 1. Quali sono le migrazioni API di alto livello richieste per la fase di pre-autorizzazione? {#preauthorization-phase-sdk-to-v2-faq1}

Nella migrazione dagli SDK all’API REST V2 sono presenti modifiche di alto livello da considerare presentate nelle tabelle seguenti:

###### SDK di AccessEnabler JavaScript

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler iOS/tvOS

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### SDK di AccessEnabler Android

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Ambito | SDK | API REST V2 | Osservazioni |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperare risorse autorizzate (decisioni di preautorizzazione) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [PUBBLICA <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Per ulteriori dettagli, fare riferimento ai seguenti documenti: <br/> <ul><li>[Flusso di pre-autorizzazione di base eseguito nell&#39;applicazione primaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Domande frequenti sulla fase di autorizzazione {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Domande frequenti sulla fase di autorizzazione

##### 1. Quali sono le migrazioni API di alto livello necessarie per la fase di autorizzazione? {#authorization-phase-sdk-to-v2-faq1}

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
