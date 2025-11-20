---
title: Esempi di utilizzo API
description: Utilizzo dell’endpoint API di Monitoraggio concorrenza
exl-id: eb232926-9c68-4874-b76d-4c458d059f0d
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '2052'
ht-degree: 0%

---

# Panoramica API {#api-overview}

Per ulteriori dettagli, consulta la [documentazione API online](https://streams-stage.adobeprimetime.com/swagger-ui/index.html).

## Finalità e prerequisiti {#purpose-prerequisites}

Questo documento aiuta gli sviluppatori di applicazioni a utilizzare le nostre specifiche API Swagger durante l’implementazione di un’integrazione con il monitoraggio della concorrenza. Prima di seguire questa linea guida, si consiglia vivamente al lettore di avere una precedente comprensione dei concetti definiti dal servizio. Per ottenere questa comprensione, è necessario avere una panoramica della [documentazione del prodotto](../cm-home.md) e della [specifica API Swagger](https://streams-stage.adobeprimetime.com/swagger-ui/index.html).

## Introduzione {#api-overview-intro}

Durante il processo di sviluppo, la documentazione pubblica di Swagger rappresenta la linea guida di riferimento per comprendere e testare i flussi API. Questo è un ottimo punto di partenza per avere un approccio pratico e familiarizzare con il modo in cui le applicazioni del mondo reale si comporterebbero in diversi scenari di interazione dell’utente.

Invia un ticket in [Zendesk](mailto:tve-support@adobe.com) per registrare la tua azienda e le tue applicazioni nel monitoraggio della concorrenza. Adobe assegnerà un ID applicazione a ogni entità. In questa guida utilizzeremo due applicazioni di riferimento con ID **demo-app** e **demo-app-2** che saranno sotto il tenant Adobe.

### Prima applicazione {#first-app-use-cases}

All&#39;applicazione con ID **demo-app** è stato assegnato dal team Adobe un criterio con una regola che limita a 3 il numero di flussi simultanei. Una polizza viene assegnata a una specifica applicazione in base alla richiesta presentata in Zendesk.

#### Recupero metadati {#retrieve-metadata-use-case}

La prima chiamata che effettuiamo è per la risorsa Metadati al fine di ottenere l’elenco degli attributi di metadati che devono essere trasmessi come dati del modulo durante l’inizializzazione della sessione. Questi metadati verranno utilizzati per valutare i criteri assegnati a questa applicazione.

```http
# Request
user = 'demo-app'
pass = ''
curl -i -u ${user}:%{pass} http://streams-stage.adobeprimetime.com/v2/metadata

# Response Code
200
# Response Body
[]
```

Come è possibile vedere dal campo corpo della risposta, l’elenco degli attributi dei metadati è vuoto. Ciò significa che gli attributi richiesti dalla progettazione sono sufficienti per valutare il criterio dei 3 flussi assegnato a questa applicazione. Consulta anche la [documentazione sui campi di metadati standard](../technical/standard-metadata-attributes.md). Dopo questa chiamata, possiamo continuare e creare una nuova sessione sulla risorsa REST Sessioni.

#### Inizializzazione della sessione {#session-initial}

La chiamata di inizializzazione della sessione viene eseguita da un’applicazione dopo l’acquisizione di tutte le informazioni necessarie per eseguirla.

```http
# Request
user = 'demo-app'
pass = ''
curl -i -X POST -u ${user}:%{pass} http://streams-stage.adobeprimetime.com/v2/sessions/adobe/12345
```


Non è necessario fornire codice di terminazione alla prima chiamata perché non sono presenti altri flussi attivi. E nessun attributo di metadati perché non ne è stato restituito alcuno dalla chiamata di recupero dei metadati.

I parametri **subject** e **idp** sono obbligatori e verranno specificati come variabili di percorso URI. Puoi ottenere i parametri **subject** e **idp** effettuando una chiamata per i campi di metadati **mvpd** e **upstreamUserID** dall&#39;autenticazione di Adobe Pass. Vedi anche la [panoramica delle API dei metadati](https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/user-metadat/user-metadata-feature.html?lang=en#). In questo esempio forniremo il valore &quot;12345&quot; come soggetto e &quot;adobe&quot; come idp.

```
# Response Code
  202
# Response Body
  no content
# Response Headers
  {
    "cache-control": "no-store",
    "content-length": "0",
    "date": "Tue, 01 Jan 2022 12:00:00 GMT",
    "expires": "Tue, 01 Jan 2022 12:01:00 GMT",
    "location": "76378b50-4eb0-43b4-b144-51cb62d85563", 
    "content-type": null
  }
```

Tutti i dati necessari sono contenuti nelle intestazioni di risposta. L&#39;intestazione **Location** rappresenta l&#39;ID della nuova sessione creata e le intestazioni **Date** e **Expires** rappresentano i valori utilizzati per pianificare l&#39;applicazione in modo che esegua l&#39;heartbeat successivo per mantenere attiva la sessione.

Con ogni chiamata è possibile inviare i metadati necessari, non solo quelli obbligatori per l’applicazione. L’invio di metadati può essere ottenuto in due modi:
* utilizzo di **query** **parametri**:

  ```sh
  curl -i -XPOST -u "user:pass" "https://streams-stage.adobeprimetime.com/v2/sessions/some_idp/some_user?metadata1=value1&metadata2=value2"
  ```

* utilizzo di **richiesta** **corpo**:

  ```sh
  curl -i -XPOST -u "user:pass" https://streams-stage.adobeprimetime.com/v2/sessions/some_idp/some_user -d "metadata1=value1" -d "metadata2=value2" -H "Content-Type=application/x-www-form-urlencoded"
  ```

#### Heartbeat {#heartbeat}

Effettuare una chiamata heartbeat. Fornisci l&#39;**ID sessione** ottenuto nella chiamata di inizializzazione della sessione, insieme ai parametri **subject** e **idp** utilizzati.

```http
# Request
user = 'demo-app'
pass = ''
curl -i -X POST -u ${user}:%{pass} http://streams-stage.adobeprimetime.com/v2/sessions/adobe/12345/76378b50-4eb0-43b4-b144-51cb62d85563
```

Per le chiamate heartbeat è consentito inviare metadati nello stesso modo in cui si esegue la sessione iniziale. È possibile aggiungere nuovi metadati in qualsiasi momento e aggiornare i valori inviati in precedenza con alcune **eccezioni**. I seguenti valori, una volta impostati, non possono essere modificati: **pacchetto**, **canale**, **piattaforma**, **assetId**, **idp**, **mvpd**, **hba_status**, **hba**,
**dispositivo mobile**

Se la sessione è ancora valida (non è scaduta o è stata eliminata manualmente), riceverai un risultato positivo:

```
# Response Code
  202
# Response Body
  no content
# Response Headers
  {
    "cache-control": "no-store",
    "content-length": "0",
    "date": "Tue, 01 Jan 2022 12:00:00 GMT",
    "expires": "Tue, 01 Jan 2022 12:01:00 GMT",
    "content-type": null
  }
```

Come nel primo caso, utilizzeremo le intestazioni **Data** e **Scade** per pianificare un altro heartbeat per questa particolare sessione. Se la sessione non è più valida, la chiamata avrà esito negativo con il codice di stato HTTP 410 GONE.

Puoi utilizzare l’opzione &quot;Keep the stream alive&quot; disponibile nell’interfaccia utente di Swagger per eseguire heartbeat automatici in una sessione specifica, questo può aiutarti a testare una regola senza doverti preoccupare della piattaforma standard necessaria per eseguire heartbeat di sessione puntuali. Questo pulsante è posizionato accanto al pulsante &quot;Prova&quot; nella scheda Swagger Heartbeat. Per impostare un heartbeat automatico per tutte le sessioni create, è necessario che siano programmate in un’interfaccia utente Swagger separata aperta in una scheda del browser web.

![](../assets/keep-stream-alive.png)

#### Terminazione sessione {#session-termination}

Il caso aziendale della tua azienda potrebbe richiedere il monitoraggio della concorrenza per terminare una sessione specifica quando, ad esempio, un utente smette di guardare un video. Per eseguire questa operazione, effettua una chiamata DELETE alla risorsa Sessioni.


```http
# Request
user = 'demo-app'
pass = ''
curl -i -X DELETE -u ${user}:%{pass} http://streams-stage.adobeprimetime.com/v2/sessions/adobe/12345/76378b50-4eb0-43b4-b144-51cb62d85563
```

Utilizza per la chiamata gli stessi parametri utilizzati per l’heartbeat di sessione. I codici di stato HTTP della risposta sono:

* 202 ACCETTATO per una risposta corretta
* 410 GONE se la sessione era già stata interrotta.

#### Ottieni tutti i flussi in esecuzione {#get-all-running-streams}

Questo endpoint offre tutte le sessioni attualmente in esecuzione per un tenant specifico in tutte le relative applicazioni. Utilizza i parametri **subject** e **idp** per la chiamata:

```http
# Request
user = 'demo-app'
pass = ''
curl -i -X GET -u ${user}:%{pass} http://streams-stage.adobeprimetime.com/v2/runningStreams/{idp}/{user}
```

Quando effettui la chiamata riceverai la seguente risposta:

```http
# Response Code
  200 
# Response Body
  {
    "runningStreams": [
      {
        "sessionId": "76378b50-4eb0-43b4-b144-51cb62d85563",
        "startTime": 1738760521421,
        "applicationId": "demo-app",
        "applicationName": "Demo application",
        "terminationCode": "94c8f7d9",
        "metadata": {
          "package": "premium"  
        },
      }
    ]
  }
# Response Headers
  {
    "cache-control": "no-store",
    "content-type": "application/json;charset=utf-8",
    "date": "Tue, 01 Jan 2022 12:00:00 GMT",
    "expires": "Tue, 01 Jan 2022 12:01:00 GMT",
  }
```

Per ogni sessione si otterrà il **terminationCode** e si completeranno i metadati.

Nota l&#39;intestazione **Scade**. Questo è il momento in cui la prima sessione deve scadere a meno che non venga inviato un heartbeat.
Nel campo metadati verranno inseriti tutti i metadati inviati all’avvio della sessione. Non lo filtriamo, riceverai tutto quello che hai inviato.
La risposta include tutti i flussi in esecuzione sulle app di altri tenant, purché le app condividano lo stesso criterio.
Se non sono presenti sessioni in esecuzione per un utente specifico quando effettui la chiamata, riceverai questa risposta:

```http
# Response Code
  200 
# Response Body
  {
    "runningStreams": [],
    "otherStreams": 0
  }
# Response Headers
  {
    "cache-control": "no-store",
    "content-type": "application/json;charset=utf-8",
    "date": "Tue, 01 Jan 2022 12:00:00 GMT",
  }
```

Inoltre, in questo caso l&#39;intestazione **Scade** non è presente.

Nel caso in cui sia stata creata una sessione che causa la morte di un&#39;altra, utilizzando l&#39;intestazione **X-Terminate**, sotto i metadati troverai il campo **sostituito**. Il suo valore è un indicatore della sessione terminata per fare spazio a quella corrente.

```http
# Response Code
  200 
# Response Body
  {
    "runningStreams": [
      {
        "sessionId": "76378b50-4eb0-43b4-b144-51cb62d85563",
        "startTime": 1738760521421,
        "applicationId": "demo-app",
        "applicationName": "Demo application",
        "terminationCode": "c424312e",
        "metadata": {
          "superseded": "ab1a9d54",
          "package": "premium"  
        },
      }
    ]
  }
# Response Headers
  {
    "cache-control": "no-store",
    "content-type": "application/json;charset=utf-8",
    "date": "Tue, 01 Jan 2022 12:00:00 GMT",
    "expires": "Tue, 01 Jan 2022 12:01:00 GMT",
  }
```

#### Interruzione del criterio {#breaking-policy-app-first}

Per simulare il comportamento della nostra applicazione quando il criterio dei 3 flussi ad essa assegnato è interrotto, è necessario effettuare 3 chiamate per l’inizializzazione della sessione. Affinché il criterio entri in vigore, le chiamate devono essere effettuate prima della scadenza di una sessione a causa della mancanza di heartbeat. Vedremo che tutte queste chiamate avranno esito positivo, ma se effettuiamo una quarta chiamata non riuscirà e verrà visualizzato il seguente errore:

```http
# Response Code
409 
# Response Body
  {
    "associatedAdvice": [
      {
        "type": "rule-violation",
        "message": "Number of active streams exceeded",
        "policyName": "demo-policy",
        "threshold": 4,
        "ruleName": "3 streams cap",
        "conflicts": {
          "76378b50-4eb0-43b4-b144-51cb62d85563" : [
            { 
              "terminationCode": "51fd351f", 
              "metadata": {
                "package": "premium",
                "show": "Friends" 
              },
              "channel": "Unknown",
              "startedAt": "2024-11-25T09:06:12.951Z",
              "deviceName": "Unknown",
              "applicationName": "Demo application"
            }
          ]
        }
      }
    ]
  }
```

Nel payload viene visualizzata una risposta 409 CONFLICT insieme a un oggetto risultato della valutazione. Ciò indica che i criteri lato server non consentono la creazione o la continuazione di questa sessione. Il corpo della risposta conterrà un oggetto EvaluationResult con AssociatedAdvice non vuoto, ovvero l&#39;elenco di oggetti Advice contenenti le spiegazioni per ogni violazione della regola.

L’applicazione deve richiedere all’utente i messaggi di errore riportati da ogni istanza di Advice. Inoltre, ogni consiglio indica anche i dettagli della regola come i nomi di attributi, soglie, regole e criteri. Inoltre, i valori in conflitto verranno inclusi anche nell’elenco delle sessioni attive per ciascun valore.

Queste informazioni sono destinate alla formattazione avanzata dei messaggi di errore e consentono all’utente di intervenire sulle sessioni in conflitto.

Ogni sessione in conflitto conterrà un **terminationCode** che può essere utilizzato per **uccidere** quel flusso. In questo modo, l’applicazione potrebbe consentire all’utente di scegliere le sessioni da terminare per tentare di accedere alla sessione corrente.

L’applicazione può utilizzare le informazioni del risultato della valutazione per visualizzare un determinato messaggio all’utente quando interrompe il video e per intraprendere ulteriori azioni, se necessario. Un caso d’uso può essere quello di interrompere altri flussi esistenti per avviarne uno nuovo. Questa operazione viene eseguita utilizzando il valore **terminationCode** presente nel campo **conflitti** per uno specifico attributo in conflitto. Il valore viene fornito come intestazione HTTP X-Terminate nella chiamata per una nuova inizializzazione di sessione.

![](../assets/session-init-termination-code.png)

Quando fornisci uno o più codici di terminazione all’inizializzazione della sessione, la chiamata avrà esito positivo e verrà generata una nuova sessione. Quindi, se proviamo a fare un heartbeat con una delle sessioni che sono state interrotte in remoto, otterremo una risposta 410 GONE con un payload dei risultati della valutazione che descrive il fatto che la sessione è stata terminata in remoto, come nell’esempio:


```http
# Response Code
  410 
# Response Body
  {
    "associatedAdvice": [
      {
        "type": "remote-termination",
        "message": "This session was terminated by a remote user",
        "terminator": {
          "channel": "Unknown",
          "startedAt": "2024-11-25T09:06:12.951Z",
          "deviceName": "Unknown",
          "applicationName": "Demo application"
        }     
      }
    ],
    "obligations": []
  }
```

È possibile restituire 410 con o senza un corpo, in base alla causa dell&#39;interruzione della sessione corrente.

Quando la risposta non ha corpo, 410 significa che si tenta una chiamata heartbeat (o interruzione) per una sessione non più attiva (a causa di un timeout o di un conflitto precedente o di altro tipo). L’unico modo per ripristinare da questo stato è che l’applicazione avvii una nuova sessione. Poiché non è presente alcun corpo, l’applicazione deve gestire questo errore senza che l’utente ne sia a conoscenza.

D&#39;altra parte, quando viene fornito un corpo di risposta, l&#39;applicazione deve cercare nell&#39;attributo **associatedAdvice** per trovare un avviso di **terminazione remota** che indica la sessione remota avviata con l&#39;intenzione esplicita di **uccidere** quella corrente. Questo dovrebbe causare un messaggio di errore come &quot;La sessione è stata annullata da dispositivo/applicazione&quot;.


### Corpo risposta {#response-body}

Per tutte le chiamate API del ciclo di vita della sessione, il corpo della risposta (se presente) sarà un oggetto JSON contenente i campi seguenti:

![](../assets/body_small.png)

**Avviso**
**EvaluationResult** includerà un array di oggetti Advice in **associatedAdvice**. Gli avvisi sono destinati all’applicazione per visualizzare un messaggio di errore completo per l’utente e (potenzialmente) consentire all’utente di intervenire.

Attualmente esistono due tipi di avvisi (specificati dal valore dell&#39;attributo **type**): **rule-law** e **remote-termination**. Il primo fornisce dettagli relativi a una regola interrotta e alle sessioni in conflitto con quella corrente (incluso l&#39;attributo terminate che può essere utilizzato per terminare tale sessione in remoto). La seconda afferma semplicemente che la sessione corrente è stata volutamente terminata da una sessione remota, in modo che gli utenti sapranno chi li ha cacciati quando sono stati raggiunti i limiti. Se **ha sostituito** è incluso nei metadati, la sessione in questione è stata creata utilizzando l&#39;intestazione **X-Terminate**.

![](../assets/advices.png)

**Obbligo**
La valutazione può inoltre contenere una o più azioni predefinite che devono essere attivate dall’applicazione in seguito a tale valutazione.

![](../assets/obligation.png)

### Seconda applicazione {#second-application}

L&#39;altra applicazione di esempio che utilizzeremo è quella con ID **demo-app-2**. A questo è stato assegnato un criterio con una regola che limita a un massimo di 2 il numero di flussi disponibili per un canale.   Devi fornire la variabile del canale per valutare questo criterio.

#### Recupero metadati {#retrieving-metadata}

Imposta il nuovo ID applicazione nell’angolo in alto a destra della pagina ed effettua una chiamata alla risorsa Metadati. Riceverai la seguente risposta:


```http
# Request
user = 'demo-app-2'
pass = ''
curl -i -u ${user}:%{pass} http://streams-stage.adobeprimetime.com/v2/metadata

# Response Code
200
# Response Body
[
  "channel"
]
```

Questa volta, il corpo della risposta non è più un elenco vuoto, come nell’esempio della prima applicazione. Ora il servizio di monitoraggio della concorrenza indica nel corpo della risposta che i metadati **channel** sono necessari all&#39;inizializzazione della sessione per valutare il criterio.

Se effettui una chiamata senza fornire un valore per il parametro **channel**, otterrai:

* Codice risposta - RICHIESTA 400 BAD
* Corpo di risposta: payload dei risultati di valutazione che descrive nel campo **requirements** ciò che è previsto nella richiesta di inizializzazione della sessione affinché l&#39;operazione venga completata correttamente.


```http
# Response Code
  400 Bad Request
# Response Body
  {
    "associatedAdvice": [],
    "obligations": {
      "namespace": "adobe.primetime.cm",
      "action": "refresh",
      "arguments": [
        "metadata"
      ]   
    }
  }
```

#### Inizializzazione della sessione {#session-init}

Assegna un valore per la chiave di metadati richiesta e impostala come parametro di modulo nella richiesta di inizializzazione della sessione, come illustrato di seguito:

```http
# Request
user = 'demo-app-2'
pass = ''
curl -i -X POST -u ${user}:%{pass} http://streams-stage.adobeprimetime.com/v2/sessions/adobe/12345?channel=channel-1
```

Ora la chiamata avrà esito positivo e verrà generata una nuova sessione.

#### Interruzione del criterio {#breaking-policy-second-app}

Per interrompere la regola presente nel criterio assegnato a questa applicazione, è necessario effettuare 2 chiamate con lo stesso valore di canale. Come nel primo esempio, la seconda chiamata deve essere eseguita mentre la prima sessione generata è ancora valida.


```http
# Response Code
  409 
# Response Body
  {
    "associatedAdvice": [
      {
        "type": "rule-violation",
        "message": "Number of streams per channel exceeded",
        "policyName": "Adobe/demo-policy-2",
        "ruleName": "2 per channel",
        "conflicts": {
          "76378b50-4eb0-43b4-b144-51cb62d85563" : [
            { 
              "terminationCode": "51fd351f", 
              "channel": "Unknown",
              "startedAt": "2024-11-25T09:06:12.951Z",
              "deviceName": "Unknown",
              "applicationName": "Demo application"
            }
          ]
        }
      }
    ]
  }
```

Se utilizziamo valori diversi per i metadati del canale ogni volta che creiamo una nuova sessione, tutte le chiamate avranno esito positivo perché la soglia di 2 ha come ambito ogni singolo valore.

Come nel primo esempio, possiamo utilizzare il codice di terminazione per interrompere in remoto i flussi in conflitto o possiamo aspettare che uno dei flussi scada, supponendo che non verrà operato alcun heartbeat su di essi.
