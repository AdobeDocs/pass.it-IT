---
title: Guida all’integrazione dei programmatori
description: Guida all’integrazione dei programmatori
exl-id: 51461caf-08ef-459e-b284-8f317f45e7b1
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '2119'
ht-degree: 0%

---

# Guida all’integrazione dei programmatori {#programmer-integration-guide}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questa guida all’integrazione è destinata ai provider di contenuti (programmatori) che intendono integrarsi con Adobe® Pass Authentication.

Nel panorama digitale odierno, gli spettatori possono accedere a Internet in qualsiasi momento e ovunque e richiedere l’accesso ai contenuti protetti. Potrebbero essere alla ricerca di un evento unico o dei diritti per lo streaming di un&#39;intera serie televisiva che stai trasmettendo.

Prima di concedere l’accesso a un contenuto protetto, è necessario determinare se il visualizzatore ne ha diritto. Le domande chiave includono:

* **Il visualizzatore dispone di un abbonamento attivo con un server di distribuzione di programmi video multicanale (MVPD)?**
* **La sottoscrizione include la programmazione?**

## Autenticazione Adobe Pass per TV Everywhere {#adobe-pass-authentication-for-tv-everywhere}

Per i programmatori, la determinazione dell’adesione non è sempre semplice. Gli MVPD sono i custodi dei dati identificativi dei clienti e dei privilegi di accesso. Complicando ulteriormente le cose, i programmatori visualizzatori possono abbonarsi a un&#39;ampia varietà di MVPDs, ognuno con sistemi unici. Queste complessità rendono la verifica del diritto sia tecnicamente impegnativa che ad uso intensivo di risorse.

![Diritto Utente Determinato Direttamente Dal Programmatore](../assets/user-ent-by-progr.png){align="center"}

*Diritto Utente Determinato Direttamente Dal Programmatore*

L’autenticazione Adobe Pass facilita in modo sicuro le transazioni di adesione tra Programmatori e MVPD, rendendo più rapido, semplice e sicuro fornire contenuti protetti ai visualizzatori idonei.

![Diritto utente mediato dall&#39;autenticazione Adobe Pass](../assets/user-ent-mediatedby-authn.png){align="center"}

*Diritto utente mediato dall&#39;autenticazione Adobe Pass*

L’autenticazione Adobe Pass funge da proxy e facilita il flusso dei diritti tra programmatori e MVPD offrendo interfacce sicure e coerenti per entrambe le parti.

Per i programmatori, l&#39;autenticazione Adobe Pass fornisce API come parte di un livello **Standard** o **Premium**:

* API di autenticazione standard di Adobe Pass:
   * [DCR REST API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* API di autenticazione Premium Adobe Pass:
   * [Ripristina API passaggio temporaneo](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [Funzione TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [API di degradazione](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [Funzione di degradazione](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [API di monitoraggio del servizio di adesione](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

### Casi d’uso {#use-cases}

Questa sezione descrive ulteriormente i casi di utilizzo dell’integrazione dei programmatori supportati dall’autenticazione Adobe Pass:

* Applicazione programmatore (TVE) con una rete a canale singolo

  Questo consente al programmatore di fornire ai visualizzatori l&#39;accesso al contenuto da una rete di canali a marchio singolo all&#39;interno di un&#39;applicazione TVE.

* Applicazione programmatore (TVE) con reti multicanale

  Questo consente al programmatore di fornire agli spettatori l&#39;accesso al contenuto da più reti di canali all&#39;interno di una singola applicazione TVE.

* Applicazione per programmatori (TVE) per eventi speciali

  Questo consente al programmatore di fornire ai visualizzatori l’accesso al contenuto di eventi speciali che potrebbero non essere risorse presenti nel database dei diritti di MVPD, come i normali canali.

| **Fase** | **Priorità** | **Caso d&#39;uso** | **Documenti** |
|----------------------|--------------|-------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Autenticazione** | **Alta** | Autenticazione | Per ulteriori dettagli, consulta i documenti aggregati nella sezione [Fase di autenticazione](#authentication-phase). |
|                      | **Alta** | Autenticazione basata sulla home page (HBA) | Per ulteriori dettagli, fare riferimento a [Autenticazione basata sulla home](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md). |
|                      | **Alta** | Single Sign-On (SSO) | Per ulteriori dettagli, consulta i documenti aggregati nella sezione [Single Sign-On (SSO)](#sso). |
|                      | **Alta** | Seleziona MVPD | Per ulteriori dettagli, consulta i documenti aggregati nella sezione [Fase di configurazione](#configuration-phase). |
|                      | **Medium** | Pagina di accesso MVPD con marchio | Consente agli MVPD di fornire pagine di accesso con branding specifico per il programmatore o il provider di servizi, incluso il supporto per le preferenze di lingua predefinite. |
|                      | **Alta** | Configurare i valori TTL (Time-To-Live) per piattaforma | Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione delle dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows). |
| **Preautorizzazione** | **Basso** | Preautorizzazione (autorizzazione di verifica preliminare) | Per ulteriori dettagli, consulta i documenti aggregati nella sezione [Fase di preautorizzazione](#preauthorization-phase). |
|                      | **Medium** | Codici di errore migliorati | Per ulteriori dettagli, fare riferimento a [Codici di errore avanzati](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md). |
| **Autorizzazione** | **Alta** | Autorizzazione | Per ulteriori dettagli, consulta i documenti aggregati nella sezione [Fase di autorizzazione](#authorization-phase). |
|                      | **Alta** | Autorizzazione del canale distinta | Consente agli utenti di accedere ai contenuti da più reti di canali all&#39;interno di una singola applicazione TVE. I programmatori possono effettuare chiamate di autorizzazione specifiche per il canale per verificare l’adesione. |
|                      | **Basso** | Autorizzazione a livello di risorsa | Consente agli MVPD di raccogliere analisi dettagliate per singole risorse di contenuto durante l’autorizzazione. |
|                      | **Medium** | Codici di errore migliorati | Per ulteriori dettagli, fare riferimento a [Codici di errore avanzati](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md). |
|                      | **Alta** | Programmatore Federated Player - Con Autorizzazione A Livello Di Pagina | Per ulteriori dettagli, consulta [Token multimediali](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Medium** | Programmatore Federated Player - Con Autorizzazione Interna Del Lettore | Per ulteriori dettagli, consulta [Token multimediali](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Alta** | Lettore in syndication - ospitato su MVPD Portal con autorizzazione a livello di pagina | Per ulteriori dettagli, consulta [Token multimediali](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Basso** | Controllo genitori - Classificazione dei contenuti nelle richieste di autorizzazione | Consente al programmatore di includere le valutazioni del contenuto come parte della richiesta di autorizzazione al MVPD, che sono utili per l’autorizzazione a livello di risorsa. |
|                      | **Basso** | Controllo genitori: filtro dei contenuti in base agli attributi degli utenti | Consente al programmatore di controllare la classificazione massima del contenuto consentita per un utente e filtrare di conseguenza il contenuto disponibile. |
| **Disconnessione** | **Medium** | Disconnetti | Per ulteriori dettagli, consulta i documenti aggregati nella sezione [Fase di disconnessione](#logout-phase). |

## Flusso diritto {#entitlement-flow}

Il flusso di adesione è una serie di passaggi che un’applicazione Programmatore (TVE) deve completare per inviare in streaming contenuti protetti. Il flusso è costituito dalle seguenti fasi:

* [Fase di registrazione](#registration-phase)
* [Fase di configurazione](#configuration-phase)
* [Fase di autenticazione](#authentication-phase)
* [(Facoltativo) Fase di pre-autorizzazione](#preauthorization-phase)
* [Fase di autorizzazione](#authorization-phase)
* [Fase disconnessione](#logout-phase)

Durante la visita iniziale di un utente a un’applicazione Programmatore (TVE), il flusso di adesione segue la sequenza descritta. Tuttavia, nelle visite successive, l’applicazione può ignorare alcuni passaggi in base allo stato della registrazione o dell’autenticazione e ai criteri di visualizzazione applicabili.

Per un’esplorazione dettagliata del flusso di adesione e delle sue fasi, continua a leggere questo documento e dopo aver fatto riferimento alle guide del manuale di accompagnamento per ulteriori informazioni:

* [Manuale dell’API REST V2 (da client a server)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
* [Manuale dell’API REST V2 (server-to-server)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)

>[!NOTE]
>
> L’applicazione Programmatore (TVE) viene utilizzata in questo documento per fare riferimento collettivamente ai tipi di applicazioni in esecuzione su piattaforme diverse (browser, dispositivi mobili, dispositivi collegati alla TV, ecc.) supportate dall’autenticazione di Adobe Pass.

### Fase di registrazione {#registration-phase}

Lo scopo della fase di registrazione è registrare l&#39;applicazione client rispetto all&#39;autenticazione Adobe Pass tramite il processo [Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

Il processo Dynamic Client Registration (DCR) richiede che l&#39;applicazione client ottenga una coppia di credenziali client e recuperi un token di accesso come obiettivo finale della fase di registrazione.

**API**

* [Recupera credenziali client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recupera token di accesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**Flussi**

* [Flusso di registrazione client dinamico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**Domande frequenti**

* [Domande frequenti sulla fase di registrazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#registration-phase-faqs-general).

### Fase di configurazione {#configuration-phase}

Lo scopo della fase di configurazione è quello di fornire all’applicazione client l’elenco di MVPD con cui è attivamente integrata, insieme ai dettagli di configurazione salvati dall’autenticazione di Adobe Pass per ogni MVPD.

La fase di configurazione funge da passaggio preliminare per la fase di autenticazione quando l&#39;applicazione client deve richiedere all&#39;utente di selezionare il proprio provider TV.

**API**

* [Recupera la configurazione per un provider di servizi specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

**Domande frequenti**

* [Domande frequenti sulla fase di configurazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#configuration-phase-faqs-general).

>[!TIP]
>
> L’applicazione TVE deve includere un’interfaccia di selezione MVPD che consenta agli utenti di identificare e selezionare facilmente il proprio provider TV.

### Fase di autenticazione {#authentication-phase}

Lo scopo della fase di autenticazione è quello di fornire all&#39;applicazione client la capacità di verificare l&#39;identità dell&#39;utente con MVPD e ottenere informazioni sui metadati dell&#39;utente.

La fase di autenticazione funge da passaggio preliminare per la fase di pre-autorizzazione o di autorizzazione quando l’applicazione client deve riprodurre il contenuto.

Se l&#39;autenticazione ha esito positivo, viene generato un profilo associato all&#39;applicazione, al dispositivo e al provider di servizi, contenente anche informazioni sui metadati dell&#39;utente.

**Passaggi di alto livello**

I passaggi seguenti delineano i passaggi di alto livello in caso di integrazione SAML:

1. Caricamento applicazione del programmatore **(sito Web)**\
   L&#39;utente passa all&#39;applicazione del programmatore (sito Web), che integra l&#39;autenticazione Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Richiesta contenuto protetto**\
   Quando l’utente tenta di accedere a contenuti protetti, l’applicazione del Programmatore visualizza un elenco di MVPD tra cui l’utente può effettuare la selezione.

1. **Inizializzazione richiesta autenticazione**\
   Dopo aver selezionato MVPD, l’utente viene reindirizzato a un server di autenticazione Adobe Pass. In questo caso, viene generata una richiesta di autenticazione SAML crittografata per il MVPD selezionato, in caso di integrazione SAML. Questa richiesta viene inviata al MVPD per conto del programmatore. A seconda del sistema di MVPD, il browser dell&#39;utente viene reindirizzato alla pagina di accesso di MVPD oppure un iFrame di accesso è incorporato nell&#39;applicazione del programmatore.

1. **Accesso a MVPD**\
   MVPD accetta la richiesta e presenta la sua interfaccia di accesso tramite reindirizzamento o iFrame.

1. **Accesso e convalida utente**\
   L’utente accede con le proprie credenziali MVPD. MVPD convalida lo stato di abbonamento dell’utente e stabilisce la propria sessione HTTP.

1. **Risposta di MVPD all&#39;autenticazione di Adobe Pass**\
   Una volta completata la convalida, MVPD genera una risposta SAML (crittografata) e la invia nuovamente all’autenticazione di Adobe Pass.

1. **Generazione profilo**\
   L’autenticazione di Adobe Pass verifica la risposta SAML, genera un profilo utente che viene memorizzato in cache e reindirizza l’utente all’applicazione del programmatore (sito web).

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

>[!TIP]
>
> L’applicazione TVE deve comunicare chiaramente lo stato di autenticazione dell’utente, ad esempio visualizzando il logo MVPD insieme alle icone &quot;bloccato&quot; o &quot;sbloccato&quot; per indicare l’accessibilità dei contenuti protetti.

#### Single Sign-On (SSO) {#single-sign-on}

**API**

* [Recupera richiesta di autenticazione partner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [Crea e recupera il profilo utilizzando la risposta di autenticazione del partner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

**Flussi**

* [Single sign-on con flussi di partner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
* [Single sign-on con flussi di identità della piattaforma](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
* [Single sign-on con flussi di token di servizio](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)

### (Facoltativo) Fase di pre-autorizzazione {#preauthorization-phase}

Lo scopo della fase di pre-autorizzazione è quello di fornire all’applicazione client la capacità di presentare un sottoinsieme di risorse dal suo catalogo a cui l’utente avrebbe diritto di accedere.

La fase di preautorizzazione può migliorare l’esperienza utente quando l’utente apre l’applicazione client per la prima volta o passa a una nuova sezione.

**API**

* [Recuperare le decisioni di pre-autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**Flussi**

* [Flusso di pre-autorizzazione di base eseguito nell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**Domande frequenti**

* [Domande frequenti sulla fase di pre-autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

>[!TIP]
>
> L&#39;applicazione TVE deve distinguere chiaramente i contenuti con restrizioni dai contenuti autorizzati mediante indicatori visivi, quali un&#39;icona &quot;bloccato&quot; per i contenuti con restrizioni e un&#39;icona &quot;sbloccato&quot; per i contenuti autorizzati.

### Fase di autorizzazione {#authorization-phase}

Lo scopo della fase di autorizzazione è quello di fornire all’applicazione client la capacità di riprodurre le risorse richieste dall’utente dopo aver convalidato i propri diritti con MVPD.

Un’autorizzazione corretta genera una decisione, contenente anche un token multimediale fornito all’applicazione Programmatore (TVE) a scopo di sicurezza.

**Passaggi di alto livello**

Le seguenti fasi delineano le fasi di alto livello:

1. **Gestione dell&#39;identificatore di risorsa**\
   Il contenuto protetto è identificato da un [identificatore di risorsa](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier), che può essere una stringa semplice o una struttura più complessa. Questo identificatore è predefinito e concordato tra il programmatore e MVPD. L&#39;applicazione del programmatore invia l&#39;identificatore di risorsa all&#39;autenticazione Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Controllo autorizzazione MVPD**\
   Il server di autenticazione di Adobe Pass comunica con l’endpoint di autorizzazione di MVPD utilizzando protocolli standardizzati.

1. **Risposta di MVPD all&#39;autenticazione di Adobe Pass**\
   Una volta completata la convalida, MVPD conferma che l’utente dispone o meno dei diritti necessari per accedere al contenuto e invia nuovamente una risposta all’autenticazione di Adobe Pass.

1. **Generazione di token di decisioni e multimediali**\
   L&#39;autenticazione di Adobe Pass verifica la risposta, genera una [decisione](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) che viene memorizzata nella cache e restituisce la decisione contenente un token multimediale all&#39;applicazione del programmatore (sito Web).

1. **Verifica dell&#39;accesso ai contenuti**\
   L&#39;applicazione del programmatore utilizza il [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) per confermare che l&#39;utente corretto sta accedendo al contenuto corretto. Dopo la convalida, l’utente ottiene l’accesso per visualizzare il contenuto protetto.

**API**

* [Recupera decisioni di autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**Flussi**

* [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**Domande frequenti**

* [Domande frequenti sulla fase di autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

>[!TIP]
>
> L&#39;applicazione TVE deve distinguere chiaramente i contenuti con restrizioni dai contenuti autorizzati mediante indicatori visivi, quali un&#39;icona &quot;bloccato&quot; per i contenuti con restrizioni e un&#39;icona &quot;sbloccato&quot; per i contenuti autorizzati.

### Fase disconnessione {#logout-phase}

Lo scopo della fase di disconnessione è fornire all’applicazione client la funzionalità per terminare il profilo autenticato dell’utente nell’ambito dell’autenticazione di Adobe Pass su richiesta dell’utente.

**API**

* [Avvia disconnessione per mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**Flussi**

* [Flusso di logout di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**Domande frequenti**

* [Domande frequenti sulla fase di disconnessione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

#### Disconnessione singola (SLO) {#single-logout}

**Flussi**

* [Flusso disconnessione singola](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)

## Informazioni sulle adesioni {#understanding-entitlements}

La soluzione di autenticazione Adobe Pass si basa sulla creazione di diritti, ossia parti specifiche di dati generati al completamento dei flussi di lavoro di autenticazione e autorizzazione. Questi diritti consentono l’accesso a contenuti protetti, ma hanno una durata limitata. Una volta scaduto, il diritto deve essere rinnovato avviando nuovamente i processi di autenticazione o autorizzazione.

Per ulteriori informazioni sulle adesioni, consulta i seguenti documenti:

* **Profili**

  Dopo l’autenticazione corretta, Adobe Pass Authentication crea un profilo autenticato (&quot;di lunga durata&quot;) associato all’identificatore dell’applicazione, del dispositivo e del provider di servizi richiedente (identificatore del richiedente).

* **[Metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Dopo l’autenticazione corretta (e in alcuni casi anche dopo l’autorizzazione), Adobe Pass Authentication riceve metadati utente da MVPD che possono esporli all’applicazione richiedente.

* **[Decisioni](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  In caso di esito positivo dell’autorizzazione, Adobe Pass Authentication crea una decisione di autorizzazione (&quot;di lunga durata&quot;) associata all’applicazione, al dispositivo, all’identificatore del provider di servizi (identificatore del richiedente) e a una risorsa protetta specifica (identificatore della risorsa).

* **[Token multimediali](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Dopo aver ottenuto l’autorizzazione, Adobe Pass Authentication crea un token multimediale (&quot;di breve durata&quot;) associato a una richiesta di riproduzione corretta e fornisce supporto per le best practice del settore per mitigare le frodi (ad esempio, la copia in streaming).

I valori TTL (time-to-live) per profili e decisioni sono impostati in base ad accordi tra programmatori e fornitori di servizi di televisione a pagamento, che concordano su un valore che meglio si adatta a tutti gli utenti coinvolti.
