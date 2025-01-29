---
title: Risorse protette
description: Risorse protette
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# Risorse protette {#protected-resources}

>[!IMPORTANT]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Le risorse protette rappresentano i contenuti che gli utenti possono inviare in streaming e sono identificate da valori univoci stabiliti tramite accordi tra MVPD e i programmatori partecipanti.

Le risorse protette seguono una struttura gerarchica ad albero, in cui ogni livello fornisce maggiore granularità per l’autorizzazione dei contenuti:

* Rete
   * Canale
      * Spettacolo
         * Episodio
            * Risorsa

## Identificatori di risorse protette {#identifiers}

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

## API REST V2 {#rest-api-v2}

Le API di autenticazione di Adobe Pass per il recupero delle decisioni di pre-autorizzazione e autorizzazione richiedono che l’applicazione client includa gli identificatori delle risorse protette come parametri.

Le decisioni di pre-autorizzazione e autorizzazione possono essere recuperate utilizzando le seguenti API:

* [Recuperare le decisioni di pre-autorizzazione utilizzando mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Recuperare le decisioni di autorizzazione utilizzando mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Consulta le sezioni **Request** e **Samples** delle API di cui sopra per comprendere il formato richiesto per fornire gli identificatori univoci delle risorse protette.

Gli identificatori univoci sono opachi all’autenticazione di Adobe Pass, in quanto vengono passati direttamente al MVPD. Se MVPD non è in grado di riconoscere o analizzare un identificatore di risorsa, restituisce un errore a Adobe Pass Authentication, che quindi inoltra l&#39;errore all&#39;applicazione client utilizzando un [Codice di errore avanzato](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

Per ulteriori dettagli su come e quando integrare le API di cui sopra, consulta i seguenti documenti:

* [Flusso di pre-autorizzazione di base eseguito nell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
