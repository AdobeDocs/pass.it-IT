---
title: Manuale dell’API REST V2 (da client a server)
description: Manuale dell’API REST V2 (da client a server)
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1833'
ht-degree: 0%

---

# Manuale dell’API REST V2 (da client a server) {#rest-api-v2-cookbook-client-to-server}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione REST API V2 è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

Il documento è destinato agli sviluppatori che stanno integrando [API REST di autenticazione Adobe Pass V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) nelle loro applicazioni di streaming con architettura Client-to-Server (C2S).

## Prerequisiti {#prerequisites}

Per termini e definizioni, consulta la documentazione [REST API V2 Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md).

Per i requisiti obbligatori e le pratiche consigliate, consulta la documentazione [Elenco di controllo API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md).

Per domande frequenti, consulta la [documentazione delle domande frequenti sulle API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md).

## A. Fase di registrazione {#registration-phase}

Lo scopo della fase di registrazione è registrare l’applicazione di streaming rispetto all’autenticazione di Adobe Pass tramite il processo di registrazione client dinamica (DCR, Dynamic Client Registration).

Il processo DCR (Dynamic Client Registration) richiede che l&#39;applicazione di streaming ottenga una coppia di credenziali client e recuperi un token di accesso come obiettivo finale della fase di registrazione.

La fase di registrazione è obbligatoria, ma l’applicazione di streaming può saltare questa fase se dispone di una coppia di credenziali client memorizzate nella cache e di un token di accesso ancora valido.

+++Articoli correlati

**API:**

* [Recupera credenziali client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recupera token di accesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**Flussi:**

* [Flusso di registrazione client dinamici](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**Domande frequenti:**

* [Domande frequenti sulla fase di registrazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### Passaggio 1: registrare l&#39;applicazione {#step-1-register-your-application}

* **Recupera credenziali client:** L&#39;applicazione di streaming recupera le credenziali client chiamando l&#39;endpoint [**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

   * L’applicazione di streaming deve memorizzare le credenziali del client e utilizzarle a tempo indefinito quando è necessario recuperare un token di accesso.


* **Recupera token di accesso:** L&#39;applicazione di streaming recupera il token di accesso chiamando l&#39;endpoint [**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

   * L’applicazione di streaming deve archiviare e utilizzare il token di accesso fino alla scadenza, quindi eliminarlo e ottenerne uno nuovo.

## B. Fase di autenticazione {#authentication-phase}

Lo scopo della fase di autenticazione è quello di fornire all&#39;applicazione di streaming la capacità di verificare l&#39;identità dell&#39;utente e ottenere informazioni sui metadati dell&#39;utente.

La fase di autenticazione funge da passaggio preliminare per la fase di pre-autorizzazione o di autorizzazione quando l’applicazione di streaming deve riprodurre il contenuto.

+++Articoli correlati

**API**

* [Crea sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Riprendi sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Recupera sessione di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Eseguire l’autenticazione nell’agente utente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Recuperare i profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recupera profilo per mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recupera profilo per codice specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**Flussi**

* [Flusso di autenticazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**Domande frequenti**

* [Domande frequenti sulla fase di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### Passaggio 2: verificare la presenza di profili autenticati esistenti {#step-2-check-for-existing-authenticated-profiles}

* **Recupera profili:** L&#39;applicazione di streaming verifica i profili esistenti chiamando l&#39;endpoint [**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md).


* **Scenario 1:** sono presenti profili esistenti. L&#39;applicazione di streaming può passare alla [Fase di preautorizzazione](#preauthorization-phase) o alla [Fase di autorizzazione](#authorization-phase).


* **Scenario 2:** Non sono presenti profili. L&#39;applicazione di streaming potrebbe procedere al passaggio successivo per [Autenticare l&#39;utente](#step-3-authenticate-the-user).


* **Scenario 3:** Non sono presenti profili, l&#39;applicazione di streaming potrebbe continuare a fornire all&#39;utente l&#39;accesso temporaneo tramite la funzionalità [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md).

   * Questo scenario non rientra nell&#39;ambito di questo documento. Per ulteriori informazioni, fare riferimento alla documentazione [Flussi di accesso temporanei](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).

### Passaggio 3: autenticare l’utente {#step-3-authenticate-the-user}

* **Recupera configurazione:** L&#39;applicazione di streaming recupera l&#39;elenco degli MVPD disponibili chiamando l&#39;endpoint [**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

   * L’applicazione di streaming può implementare un meccanismo di filtro personalizzato per perfezionare l’elenco degli MVPD dalla risposta di configurazione, visualizzando solo i fornitori previsti e nascondendo altri (ad esempio, MVPD in fase di sviluppo, test MVPD, TempPass). In questo modo gli utenti ricevono una selezione accurata al momento di scegliere il proprio provider TV.


* **Crea sessione di autenticazione:** L&#39;applicazione di streaming avvia una sessione di autenticazione chiamando l&#39;endpoint [**/api/v2/{serviceProvider}/essions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).


* **Scenario 1:** L&#39;applicazione di streaming può aprire un browser o una visualizzazione Web, pertanto deve caricare l&#39;autenticazione `url`.

   * L’utente invia il proprio nome utente e password all’interno della pagina di accesso di MVPD. Dopo l’autenticazione corretta, il reindirizzamento finale mostra una pagina di successo.


* **Scenario 2:** L&#39;applicazione di streaming non può aprire un browser, pertanto deve visualizzare l&#39;autenticazione `code`. È necessaria un&#39;applicazione Web separata per richiedere all&#39;utente di immettere `code`, creare l&#39;autenticazione `url` e aprire: [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

   * L’utente invia il proprio nome utente e password all’interno della pagina di accesso di MVPD. Dopo l’autenticazione corretta, il reindirizzamento finale mostra una pagina di successo.

### Passaggio 4: verificare la presenza di profili autenticati {#step-4-check-for-authenticated-profiles}

* **Recupera profilo per codice specifico:** L&#39;applicazione di streaming deve implementare un meccanismo di polling utilizzando `code` per verificare se il profilo è stato generato e salvato correttamente chiamando l&#39;endpoint [**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).

   * L&#39;applicazione di streaming deve **avviare il meccanismo di polling** nelle seguenti condizioni:

      * **Autenticazione eseguita all&#39;interno dell&#39;applicazione primaria (schermo):** L&#39;applicazione primaria (streaming) deve avviare il polling quando l&#39;utente raggiunge la pagina di destinazione finale, dopo che il componente del browser carica l&#39;URL specificato per il parametro `redirectUrl` nella richiesta dell&#39;endpoint [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

      * **Autenticazione eseguita all&#39;interno di un&#39;applicazione secondaria (a schermo):** L&#39;applicazione primaria (in streaming) deve avviare il polling non appena l&#39;utente avvia il processo di autenticazione, subito dopo aver ricevuto la risposta dell&#39;endpoint [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) e aver visualizzato il codice di autenticazione per l&#39;utente.

   * L&#39;applicazione di streaming deve **interrompere il meccanismo di polling** nelle seguenti condizioni:

      * **Autenticazione riuscita:** Le informazioni del profilo dell&#39;utente sono state recuperate correttamente, confermando il relativo stato di autenticazione. A questo punto, il polling non è più necessario.

      * **Sessione di autenticazione e scadenza codice:** La sessione di autenticazione e il codice scadono, come indicato dalla marca temporale `notAfter` (ad esempio, 30 minuti) nella risposta dell&#39;endpoint [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). In questo caso, l’utente deve riavviare il processo di autenticazione e arrestare immediatamente il polling che utilizza il codice di autenticazione precedente.

      * **È stato generato un nuovo codice di autenticazione:** Se l&#39;utente richiede un nuovo codice di autenticazione sul dispositivo principale (schermo), la sessione esistente non è più valida e il polling che utilizza il codice di autenticazione precedente deve essere interrotto immediatamente.

   * L&#39;applicazione di streaming deve **configurare la frequenza del meccanismo di polling** nelle seguenti condizioni:

      * **Autenticazione eseguita all&#39;interno dell&#39;applicazione principale (schermo):** L&#39;applicazione principale (streaming) deve eseguire il polling ogni 3-5 secondi o più.

      * **Autenticazione eseguita all&#39;interno di un&#39;applicazione secondaria (schermo):** L&#39;applicazione primaria (streaming) deve eseguire il polling ogni 3-5 secondi o più.

   * L’applicazione di streaming deve memorizzare in cache parti delle informazioni del profilo dell’utente in un archivio persistente per evitare richieste non necessarie e migliorare l’esperienza utente.

## C. Fase di pre-autorizzazione (facoltativa) {#preauthorization-phase}

Lo scopo della fase di pre-autorizzazione è quello di fornire all’applicazione di streaming la capacità di presentare un sottoinsieme di risorse dal suo catalogo a cui l’utente avrebbe diritto di accedere.

La fase di pre-autorizzazione può migliorare l’esperienza utente quando l’utente apre l’applicazione di streaming per la prima volta o passa a una nuova sezione.

La fase di pre-autorizzazione non è obbligatoria, l’applicazione di streaming può saltare questa fase se desidera presentare un catalogo di risorse senza filtrarle prima in base all’adesione dell’utente.

+++Articoli correlati

**API**

* [Recuperare le decisioni di pre-autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**Flussi**

* [Flusso di pre-autorizzazione di base eseguito nell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**Domande frequenti**

* [Domande frequenti sulla fase di pre-autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### Passaggio 5: verificare la presenza di risorse preautorizzate {#step-5-check-for-preauthorized-resources}

* **Recupera decisioni di preautorizzazione:** L&#39;applicazione di streaming recupera le decisioni di preautorizzazione per un elenco di risorse chiamando l&#39;endpoint [**/api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md).

   * L’applicazione di streaming non è necessaria per archiviare le decisioni di preautorizzazione nell’archiviazione persistente. Tuttavia, si consiglia di memorizzare nella cache le decisioni sulle autorizzazioni per migliorare l’esperienza utente. Questo consente di evitare inutili chiamate per risorse già preautorizzate, riducendo la latenza e migliorando le prestazioni.

   * L&#39;applicazione di streaming può determinare il motivo di una decisione di preautorizzazione negata esaminando il [codice di errore e il messaggio](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) inclusi nella risposta dall&#39;endpoint di preautorizzazione delle decisioni. Questi dettagli forniscono ad insight il motivo specifico per cui la richiesta di preautorizzazione è stata negata, aiutando a informare l’esperienza utente o a attivare eventuali operazioni necessarie nell’applicazione. Assicurati che eventuali meccanismi di esecuzione di nuovi tentativi implementati per recuperare le decisioni di preautorizzazione non generino un ciclo infinito se la decisione di preautorizzazione viene negata. Valuta la possibilità di limitare i nuovi tentativi a un numero ragionevole e di gestire i rifiuti in modo appropriato fornendo all’utente un feedback chiaro.

   * L’applicazione di streaming può ottenere una decisione di preautorizzazione per un numero limitato di risorse in una singola richiesta API, di solito fino a 5, a causa delle condizioni imposte dagli MVPD. Questo numero massimo di risorse può essere visualizzato e modificato dopo aver concordato con gli MVPD tramite Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di autenticazione Adobe Pass che agisce per tuo conto.


## D. Fase di autorizzazione {#authorization-phase}

La fase di autorizzazione ha lo scopo di fornire all’applicazione di streaming la funzionalità di riprodurre le risorse richieste dall’utente dopo aver convalidato i propri diritti con MVPD.

La fase di autorizzazione è obbligatoria, l’applicazione di streaming non può saltare questa fase se desidera riprodurre le risorse richieste dall’utente, in quanto richiede la verifica con il MVPD che l’utente abbia diritto prima di rilasciare il flusso.

+++Articoli correlati

**API**

* [Recupera decisioni di autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**Flussi**

* [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**Domande frequenti**

* [Domande frequenti sulla fase di autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### Passaggio 6: verificare la disponibilità di risorse autorizzate {#step-6-check-for-authorized-resources}

* **Recupera decisione di autorizzazione:** L&#39;applicazione di streaming recupera la decisione di autorizzazione per una risorsa specifica chiamando l&#39;endpoint [**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md).

   * L&#39;applicazione di streaming non è necessaria per archiviare le decisioni di autorizzazione nell&#39;archiviazione persistente.

   * L&#39;applicazione di streaming può determinare il motivo di una decisione di autorizzazione negata esaminando il [codice di errore e il messaggio](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) inclusi nella risposta dall&#39;endpoint Decisions Authorize. Questi dettagli forniscono ad insight il motivo specifico per cui la richiesta di autorizzazione è stata negata, aiutando a informare l’esperienza utente o a attivare eventuali operazioni necessarie nell’applicazione. Assicurati che eventuali meccanismi di esecuzione di nuovi tentativi implementati per recuperare le decisioni di autorizzazione non generino un loop infinito se la decisione di autorizzazione viene negata. Valuta la possibilità di limitare i nuovi tentativi a un numero ragionevole e di gestire i rifiuti in modo appropriato fornendo all’utente un feedback chiaro.

   * Non è necessario che l’applicazione di streaming aggiorni un token multimediale scaduto mentre il flusso è in riproduzione attiva. Se il token multimediale scade durante la riproduzione, il flusso deve poter continuare senza interruzioni. Tuttavia, il client deve richiedere una nuova decisione di autorizzazione e ottenere un nuovo token multimediale la volta successiva che l’utente tenta di riprodurre una risorsa.

   * L’applicazione di streaming può ottenere una decisione di autorizzazione per un numero limitato di risorse in una singola richiesta API, di solito fino a 1, a causa delle condizioni imposte dagli MVPD.

## E. Fase di disconnessione {#logout-phase}

Lo scopo della fase di disconnessione è fornire all’applicazione di streaming la funzionalità per terminare il profilo autenticato dell’utente nell’ambito dell’autenticazione di Adobe Pass su richiesta dell’utente.

La fase di disconnessione è obbligatoria, l&#39;applicazione di streaming deve fornire all&#39;utente la possibilità di disconnettersi.

+++Articoli correlati

**API**

* [Avvia disconnessione per mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**Flussi**

* [Flusso di logout di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**Domande frequenti**

* [Domande frequenti sulla fase di disconnessione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### Passaggio 7: disconnessione {#step-7-logout}

* **Avvia disconnessione Adobe Pass:** L&#39;applicazione di streaming avvia il flusso di disconnessione chiamando l&#39;endpoint [**/api/v2/{serviceProvider}/logout/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md).

   * L&#39;applicazione di streaming deve seguire le istruzioni fornite negli attributi `actionName` e `actionType` della risposta dell&#39;endpoint di disconnessione per garantire il completamento corretto del processo di disconnessione.

      * Se l&#39;attributo `actionType` nella risposta è impostato su &quot;interattivo&quot;:

         * **Scenario 1:** L&#39;applicazione di streaming può aprire un browser o una visualizzazione Web, pertanto deve caricare la disconnessione `url`.

         * **Scenario 2:** L&#39;applicazione di streaming non è in grado di aprire un browser, pertanto il processo di disconnessione può essere interrotto in quanto la sessione di MVPD non è persistita in una cache del browser del dispositivo di streaming.
