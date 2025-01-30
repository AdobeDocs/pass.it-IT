---
title: Decisioni
description: Decisioni
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# Decisioni {#decisions}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Le decisioni vengono generate dall&#39;autenticazione Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) in base alle richieste di autorizzazione MVPD o di preautorizzazione dell&#39;utente, determinando se l&#39;accesso a [contenuto protetto](#protected-resources) è concesso o negato.

Esistono due tipi di decisioni che vengono fornite, a seconda dell’API richiamata:

* [Decisioni di pre-autorizzazione](#preauthorization-decisions) che sono decisioni informative.
* [Decisioni di autorizzazione](#authorization-decisions) che sono decisioni autorevoli.

## Decisioni di pre-autorizzazione {#preauthorization-decisions}

La decisione di preautorizzazione è una decisione informativa che consente all&#39;applicazione client di essere informata se MVPD può consentire o negare l&#39;accesso dell&#39;utente a una [risorsa protetta](#protected-resources).

Lo scopo della pre-autorizzazione (autorizzazione di verifica preliminare) è quello di consentire all’applicazione di visualizzare informazioni accurate sul contenuto che l’utente può visualizzare. Ciò si ottiene ottimizzando l’interfaccia utente con indicatori, come icone bloccate o sbloccate, per riflettere lo stato di accesso.

>[!IMPORTANT]
>
> La decisione di pre-autorizzazione non deve essere utilizzata in modo autorevole per riprodurre le risorse, poiché questo è lo scopo di una [decisione di autorizzazione](#authorization-decisions).

L’utilizzo dell’API di preautorizzazione non è obbligatorio; l’applicazione client può saltarlo se desidera presentare un catalogo di risorse senza alcun filtro.

Se l’applicazione client intende utilizzare questa funzione, è importante notare che le decisioni di preautorizzazione possono essere ottenute solo per un numero limitato di risorse per richiesta API, in genere fino a 5.

>[!IMPORTANT]
> 
> Il numero massimo di risorse può essere aumentato solo dopo aver raggiunto un accordo con gli MVPD e i rappresentanti di autenticazione di Adobe Pass. Una volta concordate, le modifiche possono essere implementate tramite Adobe Pass TVE Dashboard da un amministratore dell’organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.
> 
> Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

Gli MVPD possono supportare la preautorizzazione tramite vari meccanismi, ciascuno con implicazioni distinte per le prestazioni e il numero massimo di risorse che possono essere gestite in una singola richiesta API.

Per ulteriori dettagli sui meccanismi esistenti che supportano la preautorizzazione, consulta la documentazione [Autorizzazione verifica preliminare MVPD](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md).

>[!IMPORTANT]
>
> Per gli MVPD senza supporto completo per le autorizzazioni di verifica preliminare, l&#39;uso della preautorizzazione deve essere concordato in anticipo con gli MVPD e i rappresentanti di autenticazione di Adobe Pass, in quanto può causare problemi di prestazioni e rallentare i tempi di risposta.

## Decisioni di autorizzazione {#authorization-decisions}

La decisione di autorizzazione è una decisione autorevole che consente all&#39;applicazione client di essere conforme alla decisione di MVPD di consentire o negare l&#39;accesso dell&#39;utente a una [risorsa protetta](#protected-resources).

Lo scopo dell’autorizzazione è consentire all’applicazione di riprodurre le risorse richieste dall’utente, dopo la convalida dei diritti con MVPD e la ricezione di un token multimediale dall’autenticazione di Adobe Pass.

>[!IMPORTANT]
> 
> Adobe Pass Authentication consiglia ai programmatori di utilizzare la libreria Media Token Verifier per convalidare il token multimediale incluso in una decisione di autorizzazione, garantendo l’accesso sicuro prima di avviare il flusso video.
> 
> Per ulteriori dettagli, consulta la documentazione di [Media Tokens](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md).

L’utilizzo dell’API di autorizzazione è obbligatorio; l’applicazione client non può saltare questa fase se desidera riprodurre le risorse richieste dall’utente, in quanto richiede la verifica con il MVPD che l’utente abbia diritto prima di rilasciare il flusso.

È importante notare che le decisioni di autorizzazione possono essere ottenute solo per un numero limitato di risorse per richiesta API, in genere 1.

>[!IMPORTANT]
>
> Il numero massimo di risorse può essere aumentato solo dopo aver raggiunto un accordo con gli MVPD e i rappresentanti di autenticazione di Adobe Pass.

## Risorse protette {#protected-resources}

Le risorse protette si riferiscono a contenuti semplificabili, identificati da valori univoci definiti tramite accordi tra MVPD e programmatori partecipanti.

Le risorse protette seguono una struttura gerarchica ad albero, in cui ogni livello fornisce maggiore granularità per l’autorizzazione dei contenuti:

* Rete
   * Canale
      * Spettacolo
         * Episodio
            * Risorsa

>[!IMPORTANT]
>
> La pre-autorizzazione (autorizzazione di verifica preliminare) è incentrata sulle risorse a livello di canale con identificatori aventi una stringa semplice o in formato MRSS.
> 
> Si consiglia di non utilizzare risorse con identificatori che includono `CDATA` sezioni in caso di pre-autorizzazione, in quanto vengono utilizzate principalmente per risorse a livello di risorsa definite da un MRSS.

### Identificatore risorsa {#resource-identifier}

L’identificatore univoco della risorsa può avere due formati:

* Un formato di stringa semplice, ad esempio un identificatore univoco per un canale (brand).
* Un formato RSS per contenuti multimediali (MRSS) contenente informazioni aggiuntive come il titolo, le valutazioni e i metadati per il controllo genitori.

Nel caso di un identificatore di risorsa semplice, ad esempio &quot;REF30&quot; (che si presume rappresenti un canale), può essere convertito in un identificatore di risorsa RSS come segue:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

Nel caso di un identificatore di risorsa più complesso, l&#39;identificatore di risorsa RSS può includere informazioni di valutazione aggiuntive come indicato di seguito:

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

Gli identificatori univoci sono principalmente opachi all’autenticazione di Adobe Pass, tuttavia i trasformatori possono essere applicati in base alle funzionalità e ai requisiti di MVPD. Se MVPD non è in grado di riconoscere o analizzare un identificatore di risorsa, restituisce un errore ad Adobe Pass Authentication, che successivamente invia l&#39;errore all&#39;applicazione client utilizzando un [Codice di errore avanzato](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

## API REST V2 {#rest-api-v2}

Le decisioni di preautorizzazione possono essere recuperate utilizzando la seguente API:

* [Recuperare le decisioni di pre-autorizzazione utilizzando mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

Le decisioni di autorizzazione possono essere recuperate utilizzando la seguente API:

* [Recuperare le decisioni di autorizzazione utilizzando mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Consulta le sezioni **Response** e **Samples** delle API di cui sopra per comprendere la struttura delle decisioni di preautorizzazione e autorizzazione.

Per ulteriori dettagli su come e quando integrare le API di cui sopra, consulta i seguenti documenti:

* [Flusso di pre-autorizzazione di base eseguito nell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
