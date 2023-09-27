---
title: API di monitoraggio del servizio di adesione
description: API di monitoraggio del servizio di adesione
exl-id: a9572372-14a6-4caa-9ab6-4a6baababaa1
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '2026'
ht-degree: 0%

---

# API di monitoraggio del servizio di adesione {#entitlement-service-monitoring-api}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica API {#api-overview}

Il monitoraggio del servizio di titolarità (ESM) è implementato come WOLAP (basato sul Web [Elaborazione analitica online](https://en.wikipedia.org/wiki/Online_analytical_processing){target=_blank}). ESM è un’API web generica per la generazione di rapporti di business supportata da un data warehouse. Funge da linguaggio di query HTTP che consente l&#39;esecuzione completa di operazioni OLAP tipiche.

>[!NOTE]
>
>L’API ESM non è generalmente disponibile. Per domande sulla disponibilità, contatta il rappresentante di Adobe.

L&#39;API ESM fornisce una visualizzazione gerarchica dei cubi OLAP sottostanti. Ogni risorsa ([dimensione](#esm_dimensions) nella gerarchia delle dimensioni, mappato come segmento di percorso URL) genera rapporti con (aggregato) [metriche](#esm_metrics) per la selezione corrente. Ogni risorsa punta alla relativa risorsa padre (per l’aggregazione) e alle relative risorse secondarie (per l’espansione). Le operazioni di suddivisione in porzioni e dicing vengono eseguite tramite parametri di stringa di query che fissano dimensioni a valori o intervalli specifici.

L’API REST fornisce i dati disponibili entro un intervallo di tempo specificato nella richiesta (fallback ai valori predefiniti se non ne viene fornito alcuno), in base al percorso della dimensione, ai filtri forniti e alle metriche selezionate. L’intervallo di tempo non verrà applicato ai rapporti che non contengono dimensioni temporali (anno, mese, giorno, ora, minuto, secondo).

Il percorso radice dell’URL dell’endpoint restituisce le metriche aggregate complessive all’interno di un singolo record, insieme ai collegamenti alle opzioni di drill-down disponibili. La versione API è mappata come segmento finale del percorso URI dell’endpoint. Ad esempio: `https://mgmt.auth.adobe.com/*v2*` significa che i client accederanno a WOLAP versione 2.

I percorsi URL disponibili sono individuabili tramite i collegamenti contenuti nella risposta. I percorsi URL validi vengono mantenuti per mappare un percorso all’interno della struttura di drill-down sottostante che contiene le metriche (pre)aggregate. Un percorso nel modulo `/dimension1/dimension2/dimension3` rifletterà una preaggregazione di queste tre dimensioni (l&#39;equivalente di un SQL `clause GROUP` DA `dimension1`, `dimension2`, `dimension3`). Se tale preaggregazione non esiste e il sistema non è in grado di calcolarla immediatamente, l’API restituirà una risposta 404 Not Found.

## Albero drill-down {#drill-down-tree}

Le seguenti strutture di espansione illustrano le dimensioni (risorse) disponibili in ESM 2.0 per [Programmatori] (#esm_dimensions) e [MVPD](#esm_dimensions_mvpd).


### Dimension disponibili per i programmatori {#progr-dimensions}

![](assets/esm-progr-dimensions.png)

### Dimension disponibili per gli MVPD {#mvpd-dimensions}

![](assets/esm-mvpd-dimensions.png)

Un GET al `https://mgmt.auth.adobe.com/v2` L’endpoint API restituirà una rappresentazione contenente:

* Collegamenti ai percorsi di drill-down radice disponibili:

   * `<link rel="drill-down" href="/v2/dimensionA"/>`

   * `<link rel="drill-down" href="/v2/dimensionB"/>`

* Un riepilogo (valori aggregati) per tutte le metriche (nell’intervallo predefinito, poiché non sono forniti parametri della stringa di query, vedi di seguito).


Seguire un percorso di espansione (passo dopo passo):
`/dimensionA/year/month/day/dimensionX` recupera la seguente risposta:

* Collegamenti a &quot;`dimensionY`&quot; e &quot;`dimensionZ`&quot; opzioni di espansione

* Un report contenente gli aggregati giornalieri per ogni valore di `dimensionX`


### Filtri

Ad eccezione delle dimensioni data/ora, qualsiasi dimensione disponibile per la proiezione corrente (percorso dimensione) può essere filtrata utilizzando il suo nome come parametro della stringa di query.

Sono disponibili le seguenti opzioni di filtro:

* **Uguale a** I filtri vengono forniti impostando il nome della dimensione su un valore particolare nella stringa query.

* **IN** è possibile specificare i filtri aggiungendo più volte lo stesso parametro nome-dimensione con valori diversi: dimension=value1\&amp;dimension=value2

* **Non è uguale a** i filtri devono utilizzare &#39;\!&#39; dopo il nome della quota, risultante in &quot;\!=&#39; &quot;operator&quot;: dimension\!=valore

* **NOT IN** i filtri richiedono &#39;\!Operatore =&#39; da utilizzare più volte, una volta per ogni valore nel set: dimension\!=valore1\&amp;dimensione\!=valore2&amp;...

Esiste anche un utilizzo speciale per i nomi delle dimensioni nella stringa di query: se il nome della dimensione viene utilizzato come parametro della stringa di query senza valore, questo indicherà all’API di restituire una proiezione che include tale dimensione nel rapporto.

### Esempio di query ESM

| *URL* | *Equivalente SQL* |
|---|---|
| /dimension1/dimension2/dimension3?dimension1=value1 | SELECT * from projection WHERE dimensione1 = &#39;valore1&#39; </br> GROUP BY dimensione1, dimensione2, dimensione3 |
| /dimension1/dimension2/dimension3?dimension1=value1&amp;dimension1=value2 | SELECT * from projection WHERE dimensione1 IN (&#39;valore1&#39;, &#39;valore2&#39;) </br> GROUP BY dimensione1, dimensione2, dimensione3 |
| /dimension1/dimension2/dimension3?dimension1!=value1 | SELECT * from proiezione WHERE dimensione1 &lt;> &#39;valore1&#39; | </br> GROUP BY dimensione1, dimensione2, dimensione3 |
| /dimension1/dimension2/dimension3?dimension1!=valore1&amp;dimensione2!=value2 | SELECT * from projection WHERE dimensione1 NOT IN (&#39;valore1&#39;, &#39;valore2&#39;) | </br> GROUP BY dimensione1, dimensione2, dimensione3 |
| Supponendo che non vi sia un percorso diretto: /dimension1/dimension3 </br> ma esiste un percorso: /dimension1/dimension2/dimension3 </br> </br> /dimension1?dimension3 | SELECT * from proiezione GROUP BY dimensione1, dimensione3 |

>[!NOTE]
>
>Nessuna di queste tecniche di filtro funzionerà per `date/time` dimensioni. L’unico modo per filtrare `date/time` dimensioni è quello di impostare `start` e `end` parametri della stringa di query (descritti di seguito) ai valori richiesti.

I seguenti parametri della stringa di query hanno significati riservati per l’API (e pertanto non possono essere utilizzati come nomi di dimensioni, altrimenti non sarebbe possibile alcun filtro per tale dimensione).

### Parametri stringa query riservata API ESM

| Parametro | Facoltativo | Descrizione | Valore predefinito | Esempio |
| --- | ---- | --- | ---- | --- |
| access_token | Sì | Se la protezione OAuth IMS è abilitata, il token IMS può essere trasmesso come token Bearer di autorizzazione standard o come parametro della stringa di query. | Nessuno | access_token=XXXXXX |
| dimension-name | Sì | Qualsiasi nome di dimensione - presente nel percorso URL corrente o in un percorso secondario valido; il valore verrà trattato come filtro &quot;è uguale a&quot;. Se non viene fornito alcun valore, la dimensione specificata verrà inclusa nell&#39;output anche se non è inclusa o adiacente al percorso corrente | Nessuno | someDimension=someValue&amp;someOtherDimension |
| fine | Sì | Ora di fine del rapporto in millisecondi | Ora corrente del server | fine=30/07/2012 |
| formato | Sì | Utilizzato per la negoziazione dei contenuti (con lo stesso effetto ma precedenza inferiore rispetto al percorso &quot;estensione&quot; - vedi di seguito). | Nessuno: la negoziazione dei contenuti proverà le altre strategie | format=json |
| limit | Sì | Numero massimo di righe da restituire | Valore predefinito segnalato dal server nel collegamento autonomo se nella richiesta non è specificato alcun limite | limit=1500 |
| metriche | Sì | Elenco separato da virgole di nomi di metriche da restituire; deve essere utilizzato sia per filtrare un sottoinsieme delle metriche disponibili (per ridurre la dimensione del payload) che per forzare l’API a restituire una proiezione che contiene le metriche richieste (anziché la proiezione ottimale predefinita). | Se questo parametro non viene fornito, verranno restituite tutte le metriche disponibili per la proiezione corrente. | metriche=m1,m2 |
| inizio | Sì | Ora di inizio per il report come ISO8601; il server compilerà la parte rimanente se viene fornito solo un prefisso: ad esempio, start=2012 si tradurrà in start=2012-01-01:00:00:00 | Segnalato dal server nel collegamento autonomo; il server tenta di fornire valori predefiniti ragionevoli in base alla granularità temporale selezionata | start=15/07/2012 |

Al momento l’unico metodo HTTP disponibile è GET. Il supporto per i metodi OPTIONS/HEAD può essere fornito nelle versioni future.

## Codici di stato API ESM {#esm-api-status-codes}

| Codice di stato | Frase motivo | Descrizione |
|---|---|---|
| 200 | OK | La risposta conterrà collegamenti &quot;roll-up&quot; e &quot;drill-down&quot; (se applicabile). Il report verrà renderizzato come attributo della risorsa: un elemento/proprietà &quot;report&quot; nidificato. |
| 400 | Richiesta non valida | Il corpo della risposta conterrà un messaggio di testo che spiega cosa non funziona con la richiesta. </br> </br> Lo stato 400 Richiesta non valida è accompagnato da un testo esplicativo nel corpo della risposta (tipo di file multimediale normale/di testo) che fornisce informazioni utili sull’errore del client. Oltre agli scenari banali come formati di data non validi o filtri applicati a dimensioni non esistenti, il sistema rifiuterà anche di rispondere a query che richiedono la restituzione o l’aggregazione immediata di un volume massiccio di dati. |
| 401 | Non autorizzato | Causata da una richiesta che non contiene le intestazioni OAuth appropriate per l’autenticazione dell’utente |
| 403 | Non consentito | Indica che la richiesta non è consentita nel contesto di sicurezza corrente; ciò si verifica quando l’utente è autenticato ma non è autorizzato ad accedere alle informazioni richieste |
| 404 | Non trovato | Si verifica nel caso in cui con la richiesta venga fornito un percorso URL non valido. Ciò non dovrebbe mai verificarsi se il client segue i collegamenti &quot;drill-down&quot;/&quot;roll-up&quot; forniti con 200 risposte |
| 405 | Metodo non consentito | Segnala che nella richiesta è stato utilizzato un metodo non supportato. Anche se attualmente è supportato solo il metodo GET, le versioni future potrebbero consentire HEAD o OPTIONS |
| 406 | Non accettabile | Segnala che il client ha richiesto un tipo di file multimediale non supportato |
| 500 | Errore interno del server | &quot;Questo non dovrebbe mai accadere&quot; |
| 503 | Servizio non disponibile | Segnala un errore all’interno dell’applicazione o nelle sue dipendenze |

## Formati dati {#data-formats}

I dati sono disponibili nei seguenti formati:

* JSON (predefinito)
* XML
* CSV
* HTML (a scopo dimostrativo)

I client possono utilizzare le seguenti strategie di negoziazione dei contenuti (la precedenza è data dalla posizione nell&#39;elenco, prima le cose):

1. &quot;Estensione file&quot; aggiunta all’ultimo segmento del percorso URL: ad esempio, `/esm/v2/media-company/year/month/day.xml`. Se l’URL contiene una stringa di query, l’estensione deve precedere il punto interrogativo: `/esm/v2/media-company/year/month/day.csv?mvpd= SomeMVPD`
1. Un parametro stringa query formato: ad esempio, `/esm/report?format=json`
1. L’intestazione HTTP Accept standard: ad esempio, `Accept: application/xml`

Sia l’&quot;estensione&quot; che il parametro query supportano i seguenti valori:

* xml
* json
* csv
* html

Se nessuna delle strategie specifica un tipo di file multimediale, l’API produrrà il contenuto JSON per impostazione predefinita.

## Lingua applicazione ipertesto {#hypertext-application-language}

Per JSON e XML, il payload verrà codificato come HAL, come descritto qui:  <http://stateless.co/hal_specification.html>.

Il rapporto effettivo (un tag/proprietà nidificata denominata &quot;report&quot;) sarà costituito dall’elenco effettivo di record contenenti tutte le dimensioni e le metriche selezionate/applicabili con i relativi valori, codificati come segue:

### JSON

```JSON
 "report": [
  {
    "dimension1": "d1",
    ...
    "metric1": "m1",
    ...
  }, {
    ...
  }
]
```

### XML

```XML
 <report>
  <record dimension1="d1" ... metric1="m1" ... />
  ...
</report
```

Per i formati XML e JSON, l’ordine dei campi (dimensioni e metriche) all’interno di un record non è specificato, ma è coerente (l’ordine è lo stesso in tutti i record). Tuttavia, i clienti non devono basarsi su un ordine particolare dei campi all’interno di un record.

Il collegamento della risorsa (il rel &quot;self&quot; in JSON e l’attributo di risorsa &quot;href&quot; in XML) contiene il percorso corrente e la stringa di query utilizzati per il rapporto in linea. La stringa di query rivelerà tutti i parametri impliciti ed espliciti, in modo che il payload indichi esplicitamente l’intervallo di tempo utilizzato, i filtri impliciti (se presenti) e così via. Gli altri collegamenti all’interno della risorsa conterranno tutti i segmenti disponibili che possono essere seguiti per eseguire il drill-down dei dati correnti. Verrà inoltre fornito un collegamento per il rollup che punterà al percorso principale (se presente). Il `href` il valore per i collegamenti di espansione/rollup contiene solo il percorso URL (non include la stringa di query, pertanto se necessario il client deve accodarlo). Nota che non tutti i parametri della stringa di query utilizzati (o impliciti) dalla risorsa corrente saranno applicabili per i collegamenti &quot;roll-up&quot; o &quot;drill-down&quot; (ad esempio, i filtri potrebbero non essere applicabili alle risorse secondarie o super).

Esempio (supponendo che sia presente una singola metrica denominata `clients` ed è disponibile una preaggregazione per `year/month/day/...`):

* https://mgmt.auth.adobe.com/esm/v2/year/month.xml

```XML
   <resource href="/esm/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21">
   <links>
   <link rel="roll-up" href="/esm/v2/year"/>
   <link rel="drill-down" href="/esm/v2/year/month/day"/>
   </links>
   <report>
   <record month="6" year="2012" clients="205"/>
   <record month="7" year="2012" clients="466"/>
   </report>
   </resource>
```

* https://mgmt.auth.adobe.com/esm/v2/year/month.json

  ```JSON
      {
        "_links" : {
          "self" : {
            "href" : "/esm/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21"
          },
          "roll-up" : {
            "href" : "/esm/v2/year"
          },
          "drill-down" : {
            "href" : "/esm/v2/year/month/day"
          }
        },
        "report" : [ {
          "month" : "6",
          "year" : "2012",
          "clients" : "205"
        }, {
          "month" : "7",
          "year" : "2012",
          "clients" : "466"
        } ]
      }
  ```

### CSV

Nel formato dati CSV, non verranno forniti collegamenti o altri metadati (tranne la riga di intestazione) in linea; invece, i metadati di selezione verranno forniti nel nome file, che seguirà questo pattern:

```CSV
    esm__<start-date>_<end-date>_<filter-values,...>.csv
```

Il file CSV conterrà una riga di intestazione e quindi i dati del rapporto come righe successive. La riga di intestazione conterrà tutte le dimensioni seguite da tutte le metriche. L’ordinamento dei dati del rapporto si rifletterà nell’ordine delle dimensioni. Pertanto, se i dati sono ordinati per `D1` e quindi per `D2`, l’intestazione CSV sarà simile a: `D1, D2, ...metrics...`.

L’ordine dei campi nella riga di intestazione rifletterà l’ordinamento dei dati della tabella.


Esempio: https://mgmt.auth.adobe.com/v2/year/month.csv produrrà un file denominato `report__2012-07-20_2012-08-20_1000.csv` con il seguente contenuto:


| Anno | Mese | Client |
| ---- | :---: | ------- |
| 2012 | 6 | 580 |
| 2012 | 7 | 231 |

## Aggiornamento dei dati {#data-freshness}

Le risposte HTTP riuscite contengono una `Last-Modified` intestazione che indica l’ora dell’ultimo aggiornamento del rapporto nel corpo. L’assenza di un’intestazione Ultima modifica indica che i dati del rapporto vengono calcolati in tempo reale.

In genere, i dati a grana grossa vengono aggiornati meno frequentemente dei dati a grana fine (ad esempio, i valori orari o di by-the-minuti possono essere più aggiornati dei valori giornalieri, soprattutto per le metriche che non possono essere calcolate in base a granularità più piccole, come i conteggi univoci).

Le versioni future di ESM possono consentire ai client di eseguire GET condizionali fornendo l’intestazione &quot;If-Modified-Since&quot; standard.

## Compressione GZIP {#gzip-compression}

L’Adobe consiglia vivamente di abilitare il supporto gzip nei client che recuperano i rapporti ESM. In questo modo si riduce notevolmente la dimensione della risposta, con conseguente riduzione dei tempi di risposta. Il rapporto di compressione per i dati ESM è compreso nell&#39;intervallo 20-30.

Per abilitare la compressione Gzip nel client, impostare `Accept-Encoding:` intestazione come segue:

* Accept-Encoding: gzip, deflate


<!--
## Related Information {#related-information}

- [ESM Overview](/help/authentication/entitlement-service-monitoring-overview.md)
- [Degradation API Overview](/help/authentication/degradation-api-overview.md)
- [Understanding Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
