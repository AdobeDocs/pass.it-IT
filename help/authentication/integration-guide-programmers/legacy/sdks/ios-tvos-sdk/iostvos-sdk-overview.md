---
title: Panoramica di iOS/tvOS SDK
description: Panoramica di iOS/tvOS SDK
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3754'
ht-degree: 0%

---

# (Legacy) Panoramica di iOS/tvOS SDK {#iostvos-sdk-overview}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

</br>


## Introduzione {#intro}

iOS AccessEnabler è una libreria Objective C iOS/tvOS che consente alle app mobili di utilizzare l’autenticazione Adobe Pass per i servizi di adesione di TV Everywhere. L&#39;implementazione è costituita dall&#39;interfaccia *AccessEnabler* che definisce l&#39;API di adesione e dai protocolli *EntitlementDelegate* e *[EntitlementStatus](#ios%20entitlement%20status)* che descrivono i callback attivati dalla libreria. L’interfaccia e il protocollo sono denominati con un nome comune: libreria AccessEnabler.

## Requisiti di iOS e tvOS {#reqs}

Per i requisiti tecnici correnti relativi alla piattaforma iOS e tvOS e all&#39;autenticazione di Adobe Pass, consulta [Requisiti per piattaforma/dispositivo/strumento](#ios) e le note sulla versione incluse nel download di SDK. Nel resto di questa pagina, sono disponibili sezioni in cui si notano le modifiche che si applicano a particolari versioni di SDK e successive. Ad esempio, la seguente è una nota legittima relativa alla versione 1.7.5 di SDK:

## Flussi di lavoro dei client nativi {#flows}

I flussi di lavoro dei client nativi sono in genere identici o molto simili a quelli dei client di autenticazione Adobe Pass basati su browser. Tuttavia, esistono alcune eccezioni, come descritto di seguito.

- [Flusso di lavoro di post-inizializzazione](#post-init)
- [Flusso di lavoro di autenticazione iniziale generico](#generic)
- [Flusso di lavoro disconnessione](#logout)


### Flusso di lavoro di post-inizializzazione {#post-init}

Tutti i flussi di lavoro relativi ai diritti supportati da AccessEnabler presuppongono che in precedenza sia stato chiamato [`setRequestor()`](#setReq) per stabilire l&#39;identità. Effettua questa chiamata per fornire l&#39;ID richiedente una sola volta, in genere durante la fase di inizializzazione/configurazione dell&#39;applicazione.


Con un client nativo di iOS, dopo la chiamata iniziale a [`setRequestor()`](#setReq), puoi scegliere come procedere:

- Puoi iniziare a effettuare immediatamente le chiamate di adesione e, se necessario, metterle in coda in modo invisibile all’utente.

- È possibile ricevere una conferma dell&#39;esito positivo/negativo di [`setRequestor()`](#setReq) implementando il callback [`setRequestorComplete()`](#setReqComplete).

- Puoi eseguire entrambe le operazioni descritte sopra.

È possibile scegliere di fare in modo che l&#39;app attenda la notifica del completamento di [`setRequestor()`](#setReq) oppure di fare affidamento sul meccanismo di coda delle chiamate di AccessEnabler. Poiché tutte le richieste di autorizzazione e autenticazione successive richiedono l&#39;ID richiedente e le informazioni di configurazione associate, il metodo [`setRequestor()`](#setReq) blocca in modo efficace tutte le chiamate API di autenticazione e autorizzazione fino al completamento dell&#39;inizializzazione.



### Flusso di lavoro di autenticazione iniziale generico {#generic}

Lo scopo di questo flusso di lavoro è quello di accedere a un utente con il suo MVPD. Dopo aver eseguito correttamente l’accesso, il server backend invia all’utente un token di autenticazione. Sebbene l’autenticazione venga in genere eseguita come parte del processo di autorizzazione, quanto segue descrive come può funzionare in isolamento e non include alcun passaggio di autorizzazione.

Questo flusso di lavoro differisce per i client nativi dal flusso di lavoro tipico di autenticazione basato su browser, ma i passaggi da 1 a 5 sono gli stessi sia per i client nativi che per i client basati su browser.

1. L&#39;applicazione avvia il flusso di lavoro di autenticazione con una chiamata al metodo API `getAuthentication() ` di AccessEnabler, che verifica la presenza di un token di autenticazione nella cache valido.
1. Se l&#39;utente è attualmente autenticato, AccessEnabler chiama la funzione di callback [`setAuthenticationStatus()`](#setAuthNStatus), passando uno stato di autenticazione che indica l&#39;esito positivo e terminando il flusso.
1. Se l&#39;utente non è attualmente autenticato, AccessEnabler continua il flusso di autenticazione determinando se l&#39;ultimo tentativo di autenticazione dell&#39;utente è stato eseguito correttamente con un determinato MVPD. Se un MVPD ID è memorizzato nella cache E il flag `canAuthenticate` è true OPPURE è stato selezionato un MVPD utilizzando [`setSelectedProvider()`](#setSelProv), all&#39;utente non viene visualizzata la finestra di dialogo di selezione di MVPD. Il flusso di autenticazione continua a utilizzare il valore memorizzato nella cache di MVPD, ovvero lo stesso MVPD utilizzato durante l’ultima autenticazione riuscita. Viene effettuata una chiamata di rete al server backend e l’utente viene reindirizzato alla pagina di accesso di MVPD (passaggio 6 di seguito).
1. Se non è stato selezionato alcun MVPD ID nella cache E non è stato selezionato alcun MVPD utilizzando [`setSelectedProvider()`](#setSelProv) OPPURE se il flag `canAuthenticate` è impostato su false, viene chiamato il callback [`displayProviderDialog()`](#dispProvDialog). Questo callback indica all&#39;applicazione di creare l&#39;interfaccia utente che presenta all&#39;utente un elenco di MVPD tra cui scegliere. Viene fornito un array di oggetti MVPD, contenente le informazioni necessarie per creare il selettore MVPD. Ogni oggetto MVPD descrive un&#39;entità MVPD e contiene informazioni come l&#39;ID del MVPD (ad esempio, XFINITY, AT\&amp;T, ecc.) e l&#39;URL in cui è possibile trovare il logo MVPD.
1. Una volta selezionato un determinato MVPD, l&#39;applicazione deve informare AccessEnabler della scelta dell&#39;utente. Dopo che l&#39;utente ha selezionato il MVPD desiderato, si informa AccessEnabler della selezione dell&#39;utente tramite una chiamata al metodo [`setSelectedProvider()`](#setSelProv).
1. IOS AccessEnabler chiama il callback `navigateToUrl:` o `navigateToUrl:useSVC:` per reindirizzare l&#39;utente alla pagina di accesso di MVPD. Attivando una delle due, AccessEnabler invia una richiesta all&#39;applicazione per creare un controller `UIWebView/WKWebView or SFSafariViewController` e caricare l&#39;URL fornito nel parametro `url` del callback. Questo è l’URL dell’endpoint di autenticazione sul server back-end. Per tvOS AccessEnabler, il callback [status()](#status_callback_implementation) viene chiamato con un parametro `statusDictionary` e il polling per la seconda autenticazione dello schermo viene avviato immediatamente. `statusDictionary` contiene `registration code` che deve essere utilizzato per la seconda autenticazione dello schermo.
1. Nel caso di iOS AccessEnabler, l&#39;utente accede alla pagina di accesso di MVPD per immettere le credenziali tramite il controller `UIWebView/WKWebView or SFSafariViewController ` dell&#39;applicazione. Durante il trasferimento si verificano diverse operazioni di reindirizzamento e l&#39;applicazione deve monitorare gli URL caricati dal controller durante le operazioni di reindirizzamento multiple.
1. Nel caso di iOS AccessEnabler, quando il controller `UIWebView/WKWebView or SFSafariViewController` carica un URL personalizzato specifico, l&#39;applicazione deve chiudere il controller e chiamare il metodo API `handleExternalURL:url ` di AccessEnabler. Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall&#39;applicazione come un segnale del completamento del flusso di autenticazione e della sicurezza della chiusura del controller `UIWebView/WKWebView or SFSafariViewController`. Nel caso in cui l&#39;applicazione debba utilizzare un controller `SFSafariViewController `, l&#39;URL personalizzato specifico è definito da `application's custom scheme` (esempio: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), altrimenti questo URL personalizzato specifico è definito dalla costante `ADOBEPASS_REDIRECT_URL` (esempio: `adobepass://ios.app`).
1. Dopo che l&#39;applicazione chiude il controller `UIWebView/WKWebView or SFSafariViewController` e chiama il metodo API `handleExternalURL:url `di AccessEnabler, AccessEnabler recupera il token di autenticazione dal server back-end e informa l&#39;applicazione che il flusso di autenticazione è completo. AccessEnabler chiama il callback [`setAuthenticationStatus()`](#setAuthNStatus) con il codice di stato 1, che indica l&#39;esito positivo. Se si verifica un errore durante l&#39;esecuzione di questi passaggi, il callback [`setAuthenticationStatus()`](#setAuthNStatus) viene attivato con il codice di stato 0, che indica un errore di autenticazione, nonché un codice di errore corrispondente.


>[!WARNING]
>
> Durante i passaggi in cui AccessEnabler cede il controllo all&#39;app, ad esempio quando viene visualizzata la finestra di dialogo di selezione del provider o quando viene visualizzato UIWebView/WKWebView o SFSafariViewController, l&#39;utente ha l&#39;opportunità di annullare il flusso di autenticazione. In queste situazioni, l&#39;app è responsabile dell&#39;informazione di AccessEnabler su questo evento e della chiamata al metodo API [`setSelectedProvider()`](#setSelProv), passando null come parametro. In questo modo AccessEnabler può ripulire il proprio stato interno e ripristinare il flusso di autenticazione. Su tvOS, puoi utilizzare lo stesso metodo per annullare il polling di autenticazione.


### Flusso di lavoro disconnessione {#logout}

Per i client nativi, la disconnessione viene gestita in modo simile al processo di autenticazione descritto in precedenza.

1. L&#39;applicazione avvia il flusso di lavoro di disconnessione con una chiamata al metodo API `logout() ` di AccessEnabler. La disconnessione è il risultato di una serie di operazioni di reindirizzamento HTTP dovute al fatto che l’utente deve essere disconnesso sia dai server di autenticazione di Adobe Pass che dai server di MVPD. Poiché non è possibile completare il flusso con una semplice richiesta HTTP emessa dalla libreria AccessEnabler, è necessario creare un&#39;istanza di un controller `UIWebView/WKWebView or SFSafariViewController` per poter seguire le operazioni di reindirizzamento HTTP.

1. Viene utilizzato un modello simile al flusso di autenticazione. IOS AccessEnabler attiva il callback `navigateToUrl:` o `navigateToUrl:useSVC:` per creare un controller `UIWebView/WKWebView or SFSafariViewController` e caricare l&#39;URL fornito nel parametro `url` del callback. Questo è l&#39;URL dell&#39;endpoint di disconnessione sul server back-end. Per tvOS AccessEnabler non viene chiamato né il callback `navigateToUrl:` né quello `navigateToUrl:useSVC:`.

1. Durante l&#39;esecuzione di diversi reindirizzamenti, l&#39;applicazione deve monitorare l&#39;attività del controller `UIWebView/WKWebView or SFSafariViewController ` e rilevare il momento in cui carica un URL personalizzato specifico. Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall&#39;applicazione come un segnale che il flusso di logout è stato completato e che è sicuro chiudere il controller. Quando il controller carica questo URL personalizzato specifico, l&#39;applicazione deve chiudere il controller e chiamare il metodo API `handleExternalURL:url ` di AccessEnabler. Nel caso in cui l&#39;applicazione debba utilizzare un controller `SFSafariViewController `, l&#39;URL personalizzato specifico è definito da `application's custom scheme` (esempio: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), altrimenti questo URL personalizzato specifico è definito dalla costante ` ADOBEPASS_REDIRECT_URL  ` (esempio: `adobepass://ios.app`).

1. Alla fine AccessEnabler chiamerà il callback [`setAuthenticationStatus()`](#setAuthNStatus) con il codice di stato 0, che indica il completamento del flusso di logout.

Il flusso di disconnessione è diverso dal flusso di autenticazione in quanto l&#39;utente non è tenuto a interagire in alcun modo con il controller `UIWebView/WKWebView or SFSafariViewController`. Pertanto, Adobe consiglia di rendere il controllo invisibile (ovvero nascosto) durante la procedura di disconnessione.

## Token {#tokens}

- [Definizioni e utilizzo](#definitions)
- [Linee guida per la memorizzazione in cache](#caching)
- [Persistenza](#persistence)
- [Formato](#format)
- [Binding dispositivo](#device_binding)


### Definizioni e utilizzo {#definitions}

La soluzione di adesione all’autenticazione di Adobe Pass ruota attorno alla generazione di parti specifiche di dati (token) generati dall’autenticazione di Adobe Pass dopo il completamento corretto dei flussi di lavoro di autenticazione e autorizzazione. Questi token vengono memorizzati localmente sul dispositivo iOS del client.



I token hanno una durata limitata; alla scadenza, i token devono essere riemessi tramite la riattivazione dei flussi di lavoro di autenticazione e/o autorizzazione.



Esistono tre tipi di token rilasciati durante i flussi di lavoro di adesione:

- **Token di autenticazione:** Il risultato finale del flusso di lavoro di autenticazione utente sarà un GUID di autenticazione che AccessEnabler può utilizzare per eseguire query di autorizzazione per conto dell&#39;utente. A questo GUID di autenticazione verrà associato un valore TTL (time-to-live) che potrebbe essere diverso dalla sessione di autenticazione dell&#39;utente. Verrà generato un token di autenticazione associando il GUID di autenticazione alla periferica che avvia le richieste di autenticazione.
- **Token di autorizzazione:** consente l&#39;accesso a una risorsa protetta specifica identificata da un resourceID univoco. È costituito da una sovvenzione di autorizzazione rilasciata dalla parte autorizzatrice insieme all’ID risorsa originale. Queste informazioni sono associate al dispositivo che avvia la richiesta.
- **Token multimediale di breve durata:** AccessEnabler concede l&#39;accesso all&#39;applicazione di hosting per una determinata risorsa restituendo un token multimediale di breve durata. Questo token viene generato in base al token di autorizzazione ottenuto in precedenza per quella specifica risorsa. Inoltre, questo token non è associato al dispositivo e la durata associata è significativamente più breve (impostazione predefinita: 5 minuti).

In caso di autenticazione e autorizzazione corrette, l’autenticazione Adobe Pass rilascia l’autenticazione, l’autorizzazione e i token multimediali di breve durata. Questi token devono essere memorizzati nella cache del dispositivo dell’utente e utilizzati per la durata della vita associata.



### Linee guida per la memorizzazione in cache {#caching}

- Token di autenticazione
- Token di autorizzazione
- Token multimediale di breve durata


#### Token di autenticazione

- **AccessEnabler 1.7:** Questo SDK introduce un nuovo metodo di archiviazione dei token, che abilita più bucket Programmer-MVPD e, di conseguenza, più token di autenticazione. Ora viene utilizzato lo stesso layout di archiviazione sia per lo scenario &quot;Autenticazione per richiedente&quot; che per il normale flusso di autenticazione. L&#39;unica differenza tra le due è nel modo in cui viene eseguita l&#39;autenticazione: &quot;Autenticazione per richiedente&quot; contiene un nuovo miglioramento (Autenticazione passiva) che consente ad AccessEnabler di eseguire l&#39;autenticazione back-channel, in base all&#39;esistenza di un token di autenticazione nell&#39;archiviazione (per un programmatore diverso). L&#39;utente deve eseguire l&#39;autenticazione una sola volta e questa sessione verrà utilizzata per ottenere i token di autenticazione in app aggiuntive. Questo flusso back-channel si verifica durante la chiamata [`setRequestor()`](#setReq) ed è per lo più trasparente per il programmatore. **Esiste tuttavia un requisito importante: il programmatore DEVE chiamare setRequestor() dal thread dell&#39;interfaccia utente principale.**
- **AccessEnabler 1.6 e versioni precedenti:** Il modo in cui i token di autenticazione vengono memorizzati nella cache del dispositivo dipende dal flag &quot;**Authentication per Requestor&quot;** associato al MVPD corrente:

<!-- end list -->

1. Se la funzione &quot;Autenticazione per richiedente&quot; è disabilitata, nel tavolo di montaggio globale verrà memorizzato localmente un singolo token di autenticazione, che verrà condiviso tra tutte le applicazioni integrate con il MVPD corrente.
1. Se la funzione &quot;Autenticazione per richiedente&quot; è abilitata, al programmatore che ha eseguito il flusso di autenticazione verrà associato in modo esplicito un token (il token non verrà memorizzato nel tavolo di montaggio globale, ma in un file privato visibile solo all&#39;applicazione del programmatore). In particolare, il Single Sign-On (SSO) tra diverse applicazioni verrà disattivato; l’utente dovrà eseguire esplicitamente il flusso di autenticazione al passaggio a una nuova app (a condizione che il programmatore della seconda app sia integrato con l’attuale MVPD e che non esista alcun token di autenticazione per tale programmatore nella cache locale).



#### Token di autorizzazione

In qualsiasi momento viene memorizzato nella cache da AccessEnabler UN solo token di autorizzazione PER RISORSA. Possono essere memorizzati nella cache più token di autorizzazione, ma sono associati a risorse diverse. Ogni volta che viene rilasciato un nuovo token di autorizzazione e ne esiste già uno precedente per *la stessa risorsa*, il nuovo token sovrascrive il valore memorizzato nella cache esistente.



#### Token multimediale

Il token multimediale di breve durata NON deve essere memorizzato nella cache. Il token multimediale deve essere recuperato dal server ogni volta che viene chiamata un’API di autorizzazione, perché è limitato all’uso una tantum.



### Persistenza {#persistence}

I token devono essere persistenti su esecuzioni consecutive della stessa applicazione. Ciò significa che una volta acquisiti i token di autenticazione e autorizzazione e che l’utente chiude l’applicazione, gli stessi token sono disponibili per l’applicazione quando l’utente riapre l’applicazione. Inoltre, è auspicabile che tali token siano persistenti in più applicazioni. In altre parole, dopo che un utente utilizza un’applicazione per accedere con un provider di identità specifico (ottenendo con successo i token di autenticazione e autorizzazione), gli stessi token possono essere utilizzati tramite un’applicazione diversa e all’utente non vengono più richieste le credenziali quando esegue l’accesso tramite lo stesso provider di identità. Questo tipo di flusso di lavoro di autenticazione/autorizzazione senza soluzione di continuità è ciò che rende la soluzione di autenticazione Adobe Pass una vera TV-Everywhere
implementazione.



## iOS

La libreria iOS AccessEnabler risolve i problemi di condivisione dei dati tra più applicazioni memorizzando i dati del token in una struttura di dati &quot;simile agli Appunti&quot; denominata *incolla bacheca*. Questa risorsa condivisa a livello di sistema fornisce gli ingredienti chiave che consentono l’implementazione del caso di utilizzo dei token persistenti desiderati:

- **Supporto per l&#39;archiviazione strutturata** - La scheda Incolla non è solo una struttura di memoria semplice e lineare simile a un buffer. Fornisce un meccanismo di archiviazione simile al dizionario che consente l’indicizzazione dei dati in base a valori chiave specificati dall’utente.
- **Supporto per la persistenza dei dati utilizzando il file system sottostante**. Il contenuto della struttura della scheda Incolla può essere contrassegnato come persistente. In tal caso i dati vengono salvati nella memoria interna del dispositivo.



Una volta inserito un token specifico nella cache dei token, la sua validità verrà verificata in momenti diversi dalla libreria AccessEnabler.  Un token valido è definito come:

- Il TTL del token non è scaduto.
- L’autorità di certificazione del token è inclusa nell’elenco dei provider di identità consentiti.



## tvOS

Poiché su tvOS il tavolo di montaggio non è disponibile, la libreria AccessEnabler di tvOS utilizza NsUserDefaults come opzione di archiviazione. In questo modo è possibile risolvere il problema dei token di autenticazione e autorizzazione persistenti, ma le informazioni archiviate non possono essere condivise tra applicazioni diverse.



**Modifiche al tavolo di montaggio di iOS 10 -** Quando si esegue l&#39;aggiornamento da una versione precedente di iOS, il tavolo di montaggio verrà cancellato. Ciò significa che tutte le applicazioni devono essere autenticate nuovamente.



**Modifiche al tavolo di montaggio di iOS 7 -** A causa di modifiche nel funzionamento dei pannelli di montaggio in iOS 7, l&#39;SSO incrociato tra le applicazioni in esecuzione in iOS 7 sarà limitato. Le applicazioni che hanno lo stesso `<Bundle Seed ID>` (noto anche come `<Team ID>`) condivideranno i token, il che significa che le app A1 e A2 dello stesso Programmatore X condivideranno i token, mentre le app A1 (Programmatore X) e A3 (Programmatore Y) non condivideranno i token.

- L’ID di seed bundle/ID team è lo stesso tra 2 app, se queste sono generate dallo stesso profilo di provisioning. Per ulteriori informazioni, consulta questo collegamento:
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- Questa limitazione &quot;Cross SSO&quot; sarà presente in iOS 7 indipendentemente dal SDK di autenticazione di Adobe Pass utilizzato.

Leggere questa nota tecnica per ulteriori informazioni sulla configurazione di SSO in iOS 7 e versione superiore (la nota tecnica si applica per Access Enabler v1.8 e versione successiva): <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### Archiviazione token (AccessEnabler 1.7)

A partire da AccessEnabler 1.7, l&#39;archiviazione dei token può supportare più combinazioni Programmatore-MVPD, basandosi su una struttura di mappe nidificate a più livelli in grado di contenere più token di autenticazione. Questa nuova archiviazione non influisce in alcun modo sull&#39;API pubblica di AccessEnabler e non richiede modifiche da parte del programmatore. Ecco un esempio che
illustra questa nuova funzionalità:

1. Apri App1 (sviluppato da Programmer1).
1. Autentica con MVPD1 (integrato con Programmer1).
1. Sospendi/Chiudi l’applicazione corrente e apri App2 (sviluppata da Programmer2).
1. Supponiamo che Programmer2 non sia integrato con MVPD2; pertanto, l’utente NON sarà autenticato in App2.
1. Autentica con MVPD2 (integrato con Programmer2) in App2.
1. Torna all’app1; l’utente sarà ancora autenticato con Programmer1.

Nelle versioni precedenti di AccessEnabler, nel passaggio 6 l&#39;utente non viene autenticato perché in precedenza l&#39;archiviazione dei token supportava un solo token di autenticazione.



La disconnessione da una sessione Programmer/MVPD cancellerà l&#39;intero storage sottostante, inclusi tutti gli altri token di autenticazione Programmer/MVPD sul dispositivo. D&#39;altra parte, l&#39;annullamento del flusso di autenticazione (richiamando [`setSelectedProvider(null)`](#setSelProv)) NON cancellerà l&#39;archiviazione sottostante, ma influirà solo sul tentativo di autenticazione corrente di Programmatore/MVPD (cancellando il MVPD per il Programmatore corrente).



### Importazione token (AccessEnabler 1.7)

Un&#39;altra funzionalità relativa allo storage inclusa in AccessEnabler 1.7 consente di importare token di autenticazione da aree di storage meno recenti. Questo &quot;Token Importer&quot; aiuta a ottenere la compatibilità tra versioni consecutive di AccessEnabler, mantenendo lo stato SSO anche quando la versione di archiviazione viene aggiornata. L&#39;importazione viene eseguita durante il flusso [`setRequestor()`](#setReq) e si verifica nei due scenari seguenti (supponendo che nell&#39;archiviazione corrente non sia presente alcun token di autenticazione valido per il programmatore corrente):

- La prima installazione di un&#39;app 1.7 sviluppata da un programmatore specifico
- Percorso di aggiornamento a un AccessEnabler futuro che utilizza un nuovo storage

L&#39;operazione di importazione è trasparente per il programmatore e non richiede alcuna modifica del codice nell&#39;applicazione client.



### Sanitizzatore token (AccessEnabler 1.7.5)

Da AccessEnabler 1.7.5 in avanti, questo servizio viene eseguito su [`setRequestor()`](#setReq)`. `È stato sviluppato in seguito al passaggio di iOS 7 dall&#39;indirizzo MAC WiFi all&#39;IDFA per il tracciamento. Lo strumento di pulizia assicura che l’archiviazione corrente contenga solo token di autenticazione validi (validi in termini di ID dispositivo, precedentemente calcolati utilizzando l’indirizzo MAC, prima di iOS7). Token Sanitizer rimuove tutti i token non validi.



Il servizio Token Sanitizer è più utile quando si utilizza un’applicazione AccessEnabler 1.7.5 in iOS 6 e successivamente l’utente si aggiorna a iOS 7. In questo caso, tutti i token di autenticazione ottenuti su iOS 6 non saranno più validi (a causa del passaggio dell’algoritmo Device ID dall’utilizzo dell’indirizzo MAC all’IDFA). Lo strumento di pulizia eliminerà tutti i token non validi e l’utente verrà quindi non autenticato.



Se Token Sanitizer non rimuove i token non validi, AccessEnabler non riuscirà a ottenere le autorizzazioni a causa della presenza di token di autenticazione non validi. Questo potrebbe rivelarsi difficile da eseguire per l’utente finale, in quanto i messaggi di errore di autorizzazione possono essere criptici e non eccessivamente utili per determinare la causa del problema.



### Formato {#format}

- [Token AuthN](#authn_token)
- [Token AuthZ](#authz_token)
- [Token file multimediali brevi](#short_token)
- [Binding dispositivo](#device_binding)


Tieni presente che il formato dei token AuthN e AuthZ è incluso qui solo per informazioni di base. La struttura di questi token può essere modificata dall’autenticazione di Adobe Pass in qualsiasi momento. I programmatori non devono conoscere la struttura esatta dei token AuthN e AuthZ per implementare le loro app, in quanto i token AuthN e AuthZ non sono esposti sul dispositivo locale. Il token per contenuti multimediali brevi *è* esposto all&#39;applicazione del programmatore.



#### Token di autenticazione {#authn_token}

L’elenco seguente presenta il formato del token di autenticazione:

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthenticationToken>
      <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
      <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>   
  </simpleAuthenticationToken>
```


#### Token di autorizzazione {#authz_token}

L’elenco seguente presenta il formato del token di autorizzazione:

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthorizationToken>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
      <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>
  </simpleAuthorizationToken>
```


#### Token file multimediali brevi {#short_token}

L’elenco seguente presenta il formato del token multimediale breve. Questo token viene esposto nell’applicazione del programmatore. Viene inviato all’applicazione del programmatore al termine di un processo di adesione riuscito:

```
  <signatureInfo>signature<signatureInfo>
  <shortAuthorizationToken>
    <sessionGUID>session_guid</sessionGUID>
    <requestorID>requestor_id</requestorID>
    <resourceID>resource_id</resourceID>
    <ttl>ttl_in_ms</ttl>
    <issueTime>issue_time</issueTime>
    <mvpdId>mvpd_id</mvpdId>
    <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
  </shortAuthorizationToken>
```


### Binding dispositivo {#device_binding}

Negli elenchi XML riportati sopra, prendere nota del tag con titolo `simpleTokenFingerprint`. Lo scopo di questo tag è quello di contenere le informazioni di personalizzazione degli ID dispositivo nativi. La libreria AccessEnabler è in grado di ottenere tali informazioni di personalizzazione e di renderle disponibili ai servizi di autenticazione di Adobe Pass durante le chiamate di adesione. Il servizio utilizzerà queste informazioni e le incorporerà nei token effettivi, associando quindi in modo efficace i token a un dispositivo specifico. L’obiettivo finale è quello di rendere i token non trasferibili tra i dispositivi.



Poiché si tratta ovviamente di una funzione correlata alla sicurezza, queste informazioni sono intrinsecamente &quot;sensibili&quot; dal punto di vista della sicurezza. Di conseguenza, queste informazioni devono essere protette sia dalle manomissioni che dalle intercettazioni. Il problema dell’intercettazione viene risolto inviando le richieste di autenticazione/autorizzazione tramite il protocollo HTTPS. La protezione contro le manomissioni viene gestita firmando digitalmente le informazioni di identificazione del dispositivo. La libreria AccessEnabler calcola un ID dispositivo in base alle informazioni fornite dal dispositivo, quindi invia l’ID dispositivo &quot;in chiaro&quot; ai server di autenticazione di Adobe Pass come parametro della richiesta. I server di autenticazione di Adobe Pass firmano digitalmente l’ID dispositivo con la chiave privata di Adobe e lo aggiungono al token di autenticazione restituito ad AccessEnabler. Pertanto, l’ID dispositivo è associato al token di autenticazione. Durante il flusso di autorizzazione, AccessEnabler invia nuovamente l&#39;ID dispositivo in chiaro, insieme al token di autenticazione. Il fallimento del processo di convalida causerà automaticamente il fallimento dei flussi di lavoro di autenticazione/autorizzazione. I server di autenticazione di Adobe Pass applicano la chiave privata all’ID del dispositivo e la confrontano con il valore nel token di autenticazione. Se non corrispondono, il flusso di adesione non riesce.



**Nota sull&#39;associazione dei dispositivi in AccessEnabler 1.7.5:** A partire da AccessEnabler 1.7.5, viene modificata la modalità di calcolo dell&#39;ID dispositivo per fornire informazioni di personalizzazione per un dispositivo iOS. Questa modifica riflette un cambiamento in iOS 7: da iOS 7, Apple non fornisce più l&#39;indirizzo MAC WiFi come opzione di tracciamento, a favore del suo identificatore per gli inserzionisti (IDFA). Poiché le informazioni di personalizzazione per un’app in esecuzione su iOS 7 si basano sull’identificatore IDFA e sono incorporate nei token del flusso di adesione, ciò significa che questa modifica può avere diversi effetti sull’esperienza utente. I diversi effetti possibili si basano sulla versione di iOS da cui l’utente esegue l’aggiornamento e sulla versione di AccessEnabler da cui il programmatore esegue l’aggiornamento. Per informazioni dettagliate su questa modifica, consulta le note sulla versione incluse in AccessEnabler SDK 1.7.5.

<!--
## Related Information {#related}


- [iOS/tvOS Integration Cookbook](#)
- [iOS/tvOS API Reference](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [SSO on iOS when using the Adobe Pass Authentication Access
  Enabler](#)
-->
