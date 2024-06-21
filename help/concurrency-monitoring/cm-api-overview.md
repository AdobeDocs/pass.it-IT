---
title: Panoramica API
description: Panoramica API del monitoraggio della concorrenza
exl-id: eb232926-9c68-4874-b76d-4c458d059f0d
source-git-commit: dd370b231acc08ea0544c0dedaa1bdb0683e378f
workflow-type: tm+mt
source-wordcount: '1556'
ht-degree: 0%

---

# Panoramica API {#api-overview}

Visualizza [documentazione API online](http://docs.adobeptime.io/cm-api-v2/) per ulteriori dettagli.

## Finalità e prerequisiti {#purpose-prerequisites}

Questo documento aiuta gli sviluppatori di applicazioni a utilizzare le nostre specifiche API Swagger durante l’implementazione di un’integrazione con il monitoraggio della concorrenza. Prima di seguire questa linea guida, si consiglia vivamente al lettore di avere una precedente comprensione dei concetti definiti dal servizio. Per avere questa comprensione, è necessario avere una panoramica del [documentazione del prodotto](/help/concurrency-monitoring/cm-home.md) e [Specifica API Swagger](http://docs.adobeptime.io/cm-api-v2/).


## Introduzione {#api-overview-intro}

Durante il processo di sviluppo, la documentazione pubblica di Swagger rappresenta la linea guida di riferimento per comprendere e testare i flussi API. Questo è un ottimo punto di partenza per avere un approccio pratico e familiarizzare con il modo in cui le applicazioni del mondo reale si comporterebbero in diversi scenari di interazione dell’utente.

Inviare un ticket in [Zendesk](mailto:tve-support@adobe.com) per registrare l&#39;azienda e le applicazioni in Monitoraggio concorrenza. Adobe assegnerà un ID applicazione a ogni entità. In questa guida utilizzeremo due applicazioni di riferimento con ID **demo-app** e **demo-app-2** che si troverà nell’Adobe del tenant.


## Casi d’uso {#api-use-case}

Il primo passaggio nel testare un flusso utilizzando Swagger è quello di immettere l’ID dell’applicazione in alto a destra della pagina come segue:

![](assets/setting-app-id.png)

In seguito, premiamo **Esplora** per impostare l’ID che verrà utilizzato nell’intestazione Autorizzazione per tutte le chiamate effettuate all’API REST.  Ogni chiamata API prevede che l’ID applicazione venga trasmesso tramite l’autenticazione HTTP di base. Il nome utente è l&#39;ID applicazione e la password è vuota.


### Prima applicazione {#first-app-use-cases}

Applicazione con ID **demo-app** Il team di Adobe ha assegnato un criterio con una regola che limita a 3 il numero di flussi simultanei. Una polizza viene assegnata a una specifica applicazione in base alla richiesta presentata in Zendesk.


#### Recupero metadati {#retrieve-metadata-use-case}

La prima chiamata che effettuiamo è per la risorsa Metadati al fine di ottenere l’elenco degli attributi di metadati che devono essere trasmessi come dati del modulo durante l’inizializzazione della sessione. Questi metadati verranno utilizzati per valutare i criteri assegnati a questa applicazione.

![](assets/retrieving-metadata.png)

Dopo aver premuto &quot;Try it out&quot; (Prova), per l’applicazione con ID **demo-app** otterremo il seguente risultato:

![](assets/empty-metadata-call.png)

Come è possibile vedere dal campo corpo della risposta, l’elenco degli attributi dei metadati è vuoto. Ciò significa che gli attributi richiesti dalla progettazione sono sufficienti per valutare il criterio dei 3 flussi assegnato a questa applicazione. Vedi anche, [Documentazione sui campi metadati standard](/help/concurrency-monitoring/standard-metadata-attributes.md). Dopo questa chiamata, possiamo continuare e creare una nuova sessione sulla risorsa REST Sessioni.


#### Inizializzazione della sessione {#session-initial}

La chiamata di inizializzazione della sessione viene eseguita da un’applicazione dopo l’acquisizione di tutte le informazioni necessarie per eseguirla.

![](assets/session-init.png)

Non è necessario fornire codice di terminazione alla prima chiamata perché non sono presenti altri flussi attivi. E nessun attributo di metadati perché non ne è stato restituito alcuno dalla chiamata di recupero dei metadati.

Il **oggetto** e **idp** I parametri sono obbligatori e verranno specificati come variabili di percorso URI. È possibile ottenere **oggetto** e **idp** effettuando una chiamata per **mvpd** e **upstreamUserID** campi di metadati da Autenticazione Adobe Pass. Vedi anche, [panoramica delle API di metadati](https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/user-metadat/user-metadata-feature.html?lang=en#). In questo esempio forniremo il valore &quot;12345&quot; come soggetto e &quot;adobe&quot; come idp.


![](assets/session-init-params-frstapp.png)

Eseguire la chiamata di inizializzazione della sessione. Riceverai la seguente risposta:


![](assets/session-init-result-first-app.png)


Tutti i dati necessari sono contenuti nelle intestazioni di risposta. Il **Posizione** rappresenta l’id della nuova sessione creata e il **Data** e **Scade** le intestazioni rappresentano i valori utilizzati per pianificare l’applicazione in modo che esegua l’heartbeat successivo al fine di mantenere attiva la sessione.

#### Heartbeat {#heartbeat}

Effettuare una chiamata heartbeat. Fornisci **id sessione** ottenuto nella chiamata di inizializzazione della sessione, insieme alla **oggetto** e **idp** parametri utilizzati.

![](assets/heartbeat.png)


Se la sessione è ancora valida (non è scaduta o è stata eliminata manualmente), riceverai un risultato positivo:

![](assets/heartbeat-succesfull-result.png)

Come nel primo caso, utilizzeremo **Data** e **Scade** intestazioni per pianificare un altro heartbeat per questa particolare sessione. Se la sessione non è più valida, la chiamata avrà esito negativo con il codice di stato HTTP 410 GONE.

Puoi utilizzare l’opzione &quot;Keep the stream alive&quot; disponibile nell’interfaccia utente di Swagger per eseguire heartbeat automatici in una sessione specifica, questo può aiutarti a testare una regola senza doverti preoccupare della piattaforma standard necessaria per eseguire heartbeat di sessione puntuali. Questo pulsante è posizionato accanto al pulsante &quot;Prova&quot; nella scheda Swagger Heartbeat. Per impostare un heartbeat automatico per tutte le sessioni create, è necessario che siano programmate in un’interfaccia utente Swagger separata aperta in una scheda del browser web.

![](assets/keep-stream-alive.png)

#### Terminazione sessione {#session-termination}

Il caso aziendale della tua azienda potrebbe richiedere il monitoraggio della concorrenza per terminare una sessione specifica quando, ad esempio, un utente smette di guardare un video. Per eseguire questa operazione, devi effettuare una chiamata DELETE sulla risorsa Sessioni.

![](assets/delete-session.png)

Utilizza per la chiamata gli stessi parametri utilizzati per l’heartbeat di sessione. I codici di stato HTTP della risposta sono:

* 202 ACCETTATO per una risposta corretta
* 410 GONE se la sessione era già stata interrotta.

#### Ottieni tutti i flussi in esecuzione {#get-all-running-streams}

Questo endpoint offre tutte le sessioni attualmente in esecuzione per un tenant specifico in tutte le relative applicazioni. Utilizzare **oggetto** e **idp** parametri per la chiamata:

![](assets/get-all-running-streams-parameters.png)

Quando effettui la chiamata riceverai la seguente risposta:

![](assets/get-all-running-streams-success.png)

Tieni presente la **Scade** intestazione. Questo è il momento in cui la prima sessione deve scadere a meno che non venga inviato un heartbeat. OtherStreams ha il valore 0 perché non ci sono altri flussi in esecuzione per questo utente sulle applicazioni di altri tenant.
Nel campo metadati verranno inseriti tutti i metadati inviati all’avvio della sessione. Non lo filtriamo, riceverai tutto quello che hai inviato.
Se non sono presenti sessioni in esecuzione per un utente specifico quando effettui la chiamata, riceverai questa risposta:

![](assets/get-all-running-streams-empty.png)

Si noti inoltre che in questo caso **Scade** l’intestazione non è presente.

#### Interruzione del criterio {#breaking-policy-app-first}


Per simulare il comportamento della nostra applicazione quando il criterio dei 3 flussi ad essa assegnato è interrotto, è necessario effettuare 3 chiamate per l’inizializzazione della sessione. Affinché il criterio entri in vigore, le chiamate devono essere effettuate prima della scadenza di una sessione a causa della mancanza di heartbeat. Vedremo che tutte queste chiamate avranno esito positivo, ma se effettuiamo una quarta chiamata non riuscirà e verrà visualizzato il seguente errore:

![](assets/breaking-policy-frstapp.png)


Nel payload viene visualizzata una risposta 409 CONFLICT insieme a un oggetto risultato della valutazione. Leggere una descrizione completa dei risultati della valutazione in [Specifica API Swagger](http://docs.adobeptime.io/cm-api-v2/#evaluation-result).

L’applicazione può utilizzare le informazioni del risultato della valutazione per visualizzare un determinato messaggio all’utente quando interrompe il video e per intraprendere ulteriori azioni, se necessario. Un caso d’uso può essere quello di interrompere altri flussi esistenti per avviarne uno nuovo. Questa operazione viene eseguita utilizzando **terminationCode** valore presente nel **conflitti** per un attributo in conflitto specifico. Il valore viene fornito come intestazione HTTP X-Terminate nella chiamata per una nuova inizializzazione di sessione.

![](assets/session-init-termination-code.png)

Quando fornisci uno o più codici di terminazione all’inizializzazione della sessione, la chiamata avrà esito positivo e verrà generata una nuova sessione. Quindi, se proviamo a creare un heartbeat con una delle sessioni che sono state interrotte in remoto, otterremo una risposta 410 GONE con un payload dei risultati della valutazione che descrive il fatto che la sessione è stata terminata in remoto, come nell’esempio:

![](assets/remote-termination.png)

### Seconda applicazione {#second-application}

L’altra applicazione di esempio che utilizzeremo è quella con ID **demo-app-2**. A questo è stato assegnato un criterio con una regola che limita a un massimo di 2 il numero di flussi disponibili per un canale.   Devi fornire la variabile del canale per valutare questo criterio.

#### Recupero metadati {#retrieving-metadata}

Imposta il nuovo ID applicazione nell’angolo in alto a destra della pagina ed effettua una chiamata alla risorsa Metadati. Riceverai la seguente risposta:

![](assets/non-empty-metadata-secndapp.png)

Questa volta, il corpo della risposta non è più un elenco vuoto, come nell’esempio della prima applicazione. Ora il Servizio di monitoraggio della concorrenza indica nel corpo della risposta che **channel** i metadati sono necessari all’inizializzazione della sessione per valutare il criterio.

Se effettui una chiamata senza fornire un valore per **channel** parametro, otterrai:

* Codice risposta - RICHIESTA 400 BAD
* Corpo della risposta: un payload dei risultati di valutazione che descrive in **obblighi** inserire il valore previsto nella richiesta di inizializzazione della sessione per consentire la riuscita dell&#39;operazione.

![](assets/metadata-request-secndapp.png)


#### Inizializzazione della sessione {#session-init}

Assegna un valore per la chiave di metadati richiesta e impostala come parametro di modulo nella richiesta di inizializzazione della sessione, come illustrato di seguito:

![](assets/session-init-params-secndapp.png)

Ora la chiamata avrà esito positivo e verrà generata una nuova sessione.


#### Interruzione del criterio {#breaking-policy-second-app}

Per interrompere la regola presente nel criterio assegnato a questa applicazione, è necessario effettuare 2 chiamate con lo stesso valore di canale. Come nel primo esempio, la seconda chiamata deve essere eseguita mentre la prima sessione generata è ancora valida.

![](assets/breaking-policy-secnd-app.png)

Se utilizziamo valori diversi per i metadati del canale ogni volta che creiamo una nuova sessione, tutte le chiamate avranno esito positivo perché la soglia di 2 ha come ambito ogni singolo valore.

Come nel primo esempio, possiamo utilizzare il codice di terminazione per interrompere in remoto i flussi in conflitto o possiamo aspettare che uno dei flussi scada, supponendo che non verrà operato alcun heartbeat su di essi.
