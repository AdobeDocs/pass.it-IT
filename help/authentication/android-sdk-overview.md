---
title: Panoramica dell’SDK per Android
description: Panoramica dell’SDK per Android
exl-id: a1d98325-32a1-4881-8635-9a3c38169422
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '2720'
ht-degree: 0%

---

# Panoramica dell’SDK per Android {#android-sdk-overview}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#intro}

Android AccessEnabler è una libreria Java Android che consente alle app mobili di utilizzare l&#39;autenticazione Adobe Pass per i servizi di adesione di TV Everywhere. Un’implementazione Android è costituita dall’interfaccia AccessEnabler che definisce l’API di adesione e da un protocollo EntitlementDelegate che descrive i callback attivati dalla libreria. L’interfaccia e il protocollo sono indicati con un nome comune: libreria Android di AccessEnabler.

## Requisiti Android {#reqs}

Per i requisiti tecnici correnti relativi alla piattaforma Android e all’autenticazione Adobe Pass, consulta [Requisiti di piattaforma/dispositivo/strumento](#android), oppure consulta le note sulla versione incluse nel download dell’SDK per Android.

## Flussi di lavoro dei client nativi {#native_client_workflows}

I flussi di lavoro dei client nativi sono in genere identici o molto simili a quelli dei client di autenticazione Adobe Pass basati su browser. Tuttavia, esistono alcune eccezioni, come descritto di seguito.

- [Flusso di lavoro di post-inizializzazione](#post-init)
- [Flusso di lavoro di autenticazione iniziale generico](#generic)
- [Flusso di lavoro disconnessione](#logout)

### Flusso di lavoro di post-inizializzazione {#post-init}

Tutti i flussi di lavoro relativi ai diritti supportati da AccessEnabler presuppongono che sia stato precedentemente chiamato [`setRequestor()`](#setRequestor) per stabilire la tua identità. Effettua questa chiamata per fornire l&#39;ID richiedente una sola volta, in genere durante la fase di inizializzazione/configurazione dell&#39;applicazione.


Con i client nativi (ad esempio, Android), dopo la chiamata iniziale a [`setRequestor()`](#setRequestor), puoi scegliere come procedere:

- Puoi iniziare a effettuare immediatamente le chiamate di adesione e, se necessario, metterle in coda in modo invisibile all’utente.

- In alternativa, puoi ricevere una conferma del successo/fallimento di [`setRequestor()`](#setRequestor) implementando il callback setRequestorComplete().

- Oppure, fate entrambe le cose.

Sta a te decidere se attendere la notifica del completamento di [`setRequestor()`](#setRequestor) o per utilizzare il meccanismo di coda delle chiamate di AccessEnabler. Poiché tutte le successive richieste di autorizzazione e autenticazione richiedono l’ID richiedente e le informazioni di configurazione associate, [`setRequestor()`](#setRequestor) Il metodo blocca efficacemente tutte le chiamate API di autenticazione e autorizzazione fino al completamento dell’inizializzazione.

### Flusso di lavoro di autenticazione iniziale generico {#generic}

Lo scopo di questo flusso di lavoro è quello di accedere a un utente con il proprio MVPD.  Dopo aver eseguito correttamente l’accesso, il server backend invia all’utente un token di autenticazione. Sebbene l’autenticazione venga in genere eseguita come parte del processo di autorizzazione, quanto segue descrive come può funzionare in isolamento e non include alcun passaggio di autorizzazione.

Sebbene il seguente flusso di lavoro client nativo sia diverso dal flusso di lavoro di autenticazione tipico basato su browser, i passaggi da 1 a 5 sono gli stessi sia per i client nativi che per i client basati su browser:

1. La pagina o il lettore avvia il flusso di lavoro di autenticazione con una chiamata a [getAuthentication()](#getAuthN), che verifica la presenza di un token di autenticazione nella cache valido. Questo metodo ha un `redirectURL` parametro; se non si specifica un valore per `redirectURL`, dopo una corretta autenticazione, l&#39;utente viene riportato all&#39;URL da cui è stata inizializzata l&#39;autenticazione.
1. AccessEnabler determina lo stato di autenticazione corrente. Se l&#39;utente è attualmente autenticato, AccessEnabler chiama il `setAuthenticationStatus()` funzione di callback, il passaggio di uno stato di autenticazione che indica il successo (passaggio 7 di seguito).
1. Se l&#39;utente non è autenticato, AccessEnabler continua il flusso di autenticazione determinando se l&#39;ultimo tentativo di autenticazione dell&#39;utente è stato eseguito correttamente con un determinato MVPD. Se un ID MVPD è memorizzato nella cache E il `canAuthenticate` il flag è true OPPURE è stato selezionato un MVPD utilizzando [`setSelectedProvider()`](#setSelectedProvider), all’utente non viene richiesta la finestra di dialogo per selezione MVPD. Il flusso di autenticazione continua a utilizzare il valore memorizzato nella cache di MVPD (ovvero lo stesso MVPD utilizzato durante l’ultima autenticazione riuscita). Viene effettuata una chiamata di rete al server backend e l’utente viene reindirizzato alla pagina di accesso MVPD (passaggio 6 di seguito).
1. Se non è stato selezionato alcun ID MVPD nella cache E non è stato selezionato alcun MVPD utilizzando [`setSelectedProvider()`](#setSelectedProvider) OPPURE `canAuthenticate` è impostato su false, il [`displayProviderDialog()`](#displayProviderDialog) chiamata di callback. Questo callback indirizza la pagina o il lettore a creare l’interfaccia utente che presenta all’utente un elenco di MVPD tra cui scegliere. Viene fornito un array di oggetti MVPD, contenente le informazioni necessarie per creare il selettore MVPD. Ogni oggetto MVPD descrive un&#39;entità MVPD e contiene informazioni quali l&#39;ID MVPD (ad esempio, XFINITY, AT\&amp;T, ecc.) e l’URL in cui è possibile trovare il logo MVPD.
1. Una volta selezionato un MVPD specifico, la pagina o il lettore devono informare AccessEnabler della scelta dell&#39;utente. Per i client non di Flash, una volta che l&#39;utente seleziona il MVPD desiderato, si informa AccessEnabler della selezione dell&#39;utente tramite una chiamata al [`setSelectedProvider()`](#setSelectedProvider) metodo. i client di Flash invece inviano un `MVPDEvent` di tipo &quot;`mvpdSelection`&quot;, passando il provider selezionato.
1. Per le applicazioni Android, se com.android.chrome è disponibile, l’URL di autenticazione verrà caricato nelle schede personalizzate di Chrome.
1. Tramite le schede personalizzate di Chrome, l’utente arriva alla pagina di accesso di MVPD e inserisce le proprie credenziali. Durante questo trasferimento si verificano diverse operazioni di reindirizzamento.
1. Quando le schede personalizzate di Chrome rilevano che un URL corrisponde allo schema (adobepass://) e un collegamento profondo dalla risorsa &quot;redirect\_uri&quot; (ad esempio adobepass://com.adobepass ), AccessEnabler recupera il token di autenticazione effettivo dai server back-end. Gli URL di reindirizzamento finali non sono effettivamente validi e non devono essere caricati dalle schede personalizzate di Chrome. Devono essere interpretati solo dall’SDK come un segnale del completamento del flusso di autenticazione.
1. AccessEnabler informa l&#39;applicazione del completamento del flusso di autenticazione. AccessEnabler chiama [`setAuthenticationStatus()`](#setAuthNStatus) callback con codice di stato 1, che indica l&#39;esito positivo. Se si verifica un errore durante l&#39;esecuzione di questi passaggi, il [`setAuthenticationStatus()`](#setAuthNStatus) il callback viene attivato con un codice di stato pari a 0, insieme al codice di errore corrispondente, che indica un errore di autenticazione.

### Flusso di lavoro disconnessione {#logout}

Per i client nativi, i loghi vengono gestiti in modo simile al processo di autenticazione descritto in precedenza. In base a questo modello, AccessEnabler apre le schede personalizzate di Chrome e carica l&#39;URL dell&#39;endpoint di disconnessione sul server back-end.



**Nota:** La disconnessione da una sessione Programmatore/MVPD cancellerà lo storage sottostante per quello specifico MVPD, inclusi tutti gli altri token di autenticazione Programmatore ottenuti tramite SSO su quel dispositivo. I token ottenuti per altri MVPD o non tramite SSO non verranno eliminati.


## Token {#tokens}

- [Definizioni e utilizzo](#definitions)
- [Linee guida per la memorizzazione in cache](#caching)
- [Persistenza](#persistence)
- [Formato](#format)
- [Binding dispositivo](#device_binding)



### Definizioni e utilizzo {#definitions}

La soluzione di adesione all’autenticazione di Adobe Pass ruota attorno alla generazione di parti specifiche di dati (token) generati dall’autenticazione di Adobe Pass dopo il completamento corretto dei flussi di lavoro di autenticazione e autorizzazione. Questi token vengono memorizzati localmente sul dispositivo Android del client.

I token hanno una durata limitata; alla scadenza, è necessario riemettere i token riavviando i flussi di lavoro di autenticazione e/o autorizzazione.

Esistono tre tipi di token rilasciati durante i flussi di lavoro di adesione:

- **Token di autenticazione** - Il risultato finale del flusso di lavoro di autenticazione utente sarà un GUID di autenticazione che AccessEnabler può utilizzare per eseguire query di autorizzazione per conto dell&#39;utente. A questo GUID di autenticazione verrà associato un valore TTL (time-to-live) che potrebbe essere diverso dalla sessione di autenticazione dell&#39;utente. L’autenticazione di Adobe Pass genera un token di autenticazione associando il GUID di autenticazione alla periferica che avvia le richieste di autenticazione.
- **Token di autorizzazione** - Consente l&#39;accesso a una risorsa protetta specifica identificata da un `resourceID`. Si tratta di una concessione di autorizzazione rilasciata dalla parte autorizzatrice insieme all&#39;originale `resourceID`. Queste informazioni sono associate al dispositivo che avvia la richiesta.
- **Token multimediale di breve durata** : AccessEnabler concede l’accesso all’applicazione di hosting per una determinata risorsa restituendo un Media Token di breve durata. Questo token viene generato in base al token di autorizzazione ottenuto in precedenza per quella specifica risorsa. Inoltre, questo token non è associato al dispositivo e la durata associata è significativamente più breve (impostazione predefinita: 5 minuti).

In caso di autenticazione e autorizzazione corrette, l’autenticazione Adobe Pass rilascia l’autenticazione, l’autorizzazione e i token multimediali di breve durata. Questi token devono essere memorizzati nella cache del dispositivo dell’utente e utilizzati per la durata della vita associata.



### Linee guida per la memorizzazione in cache {#caching}

- Token di autenticazione
- Token di autorizzazione
- Token multimediale


#### Token di autenticazione

- **AccessEnabler 1.6 e versioni precedenti** - Il modo in cui i token di autenticazione vengono memorizzati nella cache del dispositivo dipende dalla &quot;**Autenticazione per richiedente&quot;** Flag associato all&#39;MVPD corrente:


1. Se la funzione &quot;Autenticazione per richiedente&quot; è *disabilitato*, quindi un singolo token di autenticazione verrà archiviato localmente nel tavolo di montaggio globale. Questo token verrà condiviso tra tutte le applicazioni integrate con l&#39;MVPD corrente.
1. Se la funzione &quot;Autenticazione per richiedente&quot; è *abilitato*, quindi un token verrà associato in modo esplicito al programmatore che ha eseguito il flusso di autenticazione (il token non verrà memorizzato nel tavolo di montaggio globale, ma in un file privato visibile solo all&#39;applicazione del programmatore). In particolare, il Single Sign-On (SSO) tra diverse applicazioni verrà disattivato; l&#39;utente dovrà eseguire esplicitamente il flusso di autenticazione quando passa a una nuova app (a condizione che il programmatore della seconda app sia integrato con l&#39;MVPD corrente e che non esista alcun token di autenticazione per tale programmatore nella cache locale).

   **Nota:** AE 1.6 Google GSON Nota tecnica: [Come risolvere le dipendenze Gson](https://tve.zendesk.com/entries/22902516-Android-AccessEnabler-1-6-How-to-resolve-Gson-dependencies)

- **AccessEnabler 1.7** : questo SDK introduce un nuovo metodo di archiviazione dei token, consentendo l’utilizzo di più bucket Programmatore-MVPD e, di conseguenza, di più token di autenticazione. A partire da AE 1.7, lo stesso layout di archiviazione viene utilizzato sia per lo scenario &quot;Autenticazione per richiedente&quot; che per il normale flusso di autenticazione. L&#39;unica differenza tra le due è nel modo in cui viene eseguita l&#39;autenticazione: &quot;Authentication per Requestor&quot; contiene un nuovo miglioramento (autenticazione passiva) che consente ad AccessEnabler di eseguire l&#39;autenticazione back-channel, basata sull&#39;esistenza di un token di autenticazione nell&#39;archivio (per un programmatore diverso). L’utente deve autenticare una sola volta e questa sessione verrà utilizzata per ottenere i token di autenticazione nelle app successive. Questo flusso di back-channel avviene durante [`setRequestor()`](#setRequestor) ed è trasparente per il programmatore. C&#39;è tuttavia un requisito importante qui: il programmatore DEVE chiamare [`setRequestor()`](#setRequestor) dal thread dell’interfaccia utente principale e dall’interno di un’attività.


#### Token di autorizzazione

In qualsiasi momento viene memorizzato nella cache da AccessEnabler UN solo token di autorizzazione per risorsa. Nella cache possono essere memorizzati più token di autorizzazione, ma sono associati a risorse diverse. Ogni volta che viene rilasciato un nuovo token di autorizzazione e ne esiste già uno per la stessa risorsa, il nuovo token sovrascrive il valore memorizzato nella cache esistente.



#### Token multimediale

Il token multimediale di breve durata NON deve essere memorizzato nella cache. Il token multimediale deve essere recuperato dal server ogni volta che viene chiamata un’API di autorizzazione, perché è limitato all’uso una tantum.



### Persistenza {#persistence}

I token devono essere persistenti su esecuzioni consecutive della stessa applicazione. Ciò significa che una volta acquisiti i token di autenticazione e autorizzazione e che l’utente chiude l’applicazione, gli stessi token sono disponibili per l’applicazione quando l’utente riapre l’applicazione. Inoltre, è auspicabile che tali token siano persistenti in più applicazioni. In altre parole, dopo che un utente utilizza un’applicazione per accedere con un provider di identità specifico (ottenendo con successo i token di autenticazione e autorizzazione), gli stessi token possono essere utilizzati tramite un’applicazione diversa e all’utente non vengono più richieste le credenziali quando esegue l’accesso tramite lo stesso provider di identità.



Questo tipo di flusso di lavoro di autenticazione/autorizzazione senza soluzione di continuità è ciò che rende la soluzione di autenticazione Adobe Pass una vera implementazione TV-Everywhere. Dal punto di vista puramente tecnico, la libreria Android AccessEnabler risolve i problemi di condivisione dei dati tra le applicazioni memorizzando i dati del token in un file di database che si trova sull’archiviazione esterna. Queste risorse condivise a livello di sistema forniscono gli ingredienti chiave che consentono l’implementazione del caso d’uso dei token persistenti desiderati:

- Supporto per l’archiviazione strutturata: l’archiviazione dei token di autenticazione Adobe Pass non è solo una semplice struttura lineare di memoria simile a un buffer. Fornisce un meccanismo di archiviazione simile al dizionario che consente l’indicizzazione dei dati in base a valori chiave specificati dall’utente.
- Supporto per la persistenza dei dati utilizzando il file system sottostante: per impostazione predefinita, il contenuto del file di database viene mantenuto e i dati vengono salvati nella memoria esterna del dispositivo.



Una volta inserito un token specifico nella cache dei token, la sua validità verrà verificata in momenti diversi dalla libreria AccessEnabler.  Un token valido è definito come:

- Il TTL del token non è scaduto
- L’emittente del token è incluso nell’elenco dei provider di identità consentiti



A partire da AccessEnabler 1.7, l&#39;archiviazione dei token può supportare più combinazioni Programmatore-MVPD, basandosi su una struttura di mappe nidificate a più livelli in grado di contenere più token di autenticazione. Questa nuova archiviazione non influisce in alcun modo sull&#39;API pubblica di AccessEnabler e non richiede modifiche da parte del programmatore. Di seguito è riportato un esempio che illustra questa nuova funzionalità:

1. Apri App1 (sviluppato da Programmer1).
1. Autentica con MVPD1 (integrato con Programmer1).
1. Sospendi/Chiudi l’applicazione corrente e apri App2 (sviluppata da Programmer2).
1. Supponiamo che Programmer2 non sia integrato con MVPD2; pertanto, l&#39;utente NON sarà autenticato in App2.
1. Autentica con MVPD2 (integrato con Programmer2) in App2.
1. Torna all’app1; l’utente sarà ancora autenticato con Programmer1.

Nelle versioni precedenti di AccessEnabler, nel passaggio 6 l&#39;utente non viene autenticato perché in precedenza l&#39;archiviazione dei token supportava un solo token di autenticazione.


**NOTA:** La disconnessione da una sessione Programmatore/MVPD cancellerà lo storage sottostante, inclusi tutti gli altri token di autenticazione Programmatore sul dispositivo con SSO. I token ottenuti per altri MVPD o non tramite SSO non verranno eliminati. Annullamento del flusso di autenticazione (richiamo [`setSelectedProvider(null)`](#setSelectedProvider)) NON cancella l&#39;archiviazione sottostante, ma influisce solo sull&#39;attuale tentativo di autenticazione Programmatore/MVPD (cancellando MVPD per il Programmatore corrente).


Un&#39;altra funzionalità relativa allo storage inclusa in AccessEnabler 1.7 consente di importare token di autenticazione da aree di storage meno recenti. Questo &quot;Token Importer&quot; aiuta a ottenere la compatibilità tra versioni consecutive di AccessEnabler, mantenendo lo stato SSO anche quando la versione di archiviazione viene aggiornata.

L&#39;importazione viene eseguita durante [`setRequestor()`](#setRequestor) ed viene eseguito nei due scenari seguenti (supponendo che non sia presente alcun token di autenticazione valido per il programmatore corrente nell’archiviazione corrente):

- La prima installazione di un&#39;app 1.7 sviluppata da un programmatore specifico
- Percorso di aggiornamento a un AccessEnabler futuro che utilizza un nuovo storage

L&#39;operazione di importazione è trasparente per il programmatore e non richiede alcuna modifica del codice nell&#39;applicazione client.



### Formato {#format}

- [Token di autenticazione](#authn_token)
- [Token di autorizzazione](#authz_token)
- [Token file multimediali brevi](#short_media_token)
- [Binding dispositivo](#device_binding)

#### Token di autenticazione {#authn_token}

L’elenco seguente presenta il formato del token di autenticazione:

```XML
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

```XML
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


#### Token file multimediali brevi {#short_media_token}

L’elenco seguente presenta il formato del token multimediale breve.  Questo token viene esposto nell’applicazione del programmatore.  Viene passato all’applicazione del programmatore al termine di un processo di adesione riuscito:

```XML
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


#### Binding dispositivo {#device_binding}

Negli elenchi XML riportati sopra, prendere nota del tag intitolato `simpleTokenFingerprint`. Lo scopo di questo tag è quello di contenere le informazioni di personalizzazione degli ID dispositivo nativi. La libreria AccessEnabler è in grado di ottenere tali informazioni di personalizzazione e di renderle disponibili ai servizi di autenticazione di Adobe Pass durante le chiamate di adesione. Il servizio utilizzerà queste informazioni e le incorporerà nei token effettivi, associando quindi in modo efficace i token a un dispositivo specifico. L’obiettivo finale è quello di rendere i token non trasferibili tra i dispositivi.



Negli elenchi XML riportati sopra, si noti il tag simpleTokenFingerprint. Lo scopo di questo tag è quello di contenere le informazioni di personalizzazione degli ID dispositivo nativi. La libreria AccessEnabler è in grado di ottenere tali informazioni di personalizzazione e di renderle disponibili ai servizi di autenticazione di Adobe Pass durante le chiamate di adesione. Il servizio utilizzerà queste informazioni e le incorporerà nei token effettivi, associando quindi in modo efficace i token a un dispositivo specifico. L’obiettivo finale è quello di rendere i token non trasferibili tra i dispositivi.



Poiché si tratta ovviamente di una funzione correlata alla sicurezza, queste informazioni sono intrinsecamente &quot;sensibili&quot; dal punto di vista della sicurezza. Di conseguenza, queste informazioni devono essere protette sia dalle manomissioni che dalle intercettazioni. Il problema dell’intercettazione viene risolto inviando le richieste di autenticazione/autorizzazione tramite il protocollo HTTPS. La protezione contro le manomissioni viene gestita firmando digitalmente le informazioni di identificazione del dispositivo. La libreria AccessEnabler calcola un ID dispositivo in base alle informazioni fornite dal dispositivo, quindi invia l’ID dispositivo &quot;in chiaro&quot; ai server di autenticazione di Adobe Pass come parametro della richiesta.  I server di autenticazione di Adobe Pass firmano digitalmente l’ID dispositivo con la chiave privata di Adobe e lo aggiungono al token di autenticazione restituito ad AccessEnabler. Pertanto, l’ID dispositivo è associato al token di autenticazione.  Durante il flusso di autorizzazione, AccessEnabler invia nuovamente l&#39;ID dispositivo in chiaro, insieme al token di autenticazione.  Il fallimento del processo di convalida causerà automaticamente un errore dei flussi di lavoro di autenticazione/autorizzazione.  I server di autenticazione di Adobe Pass applicano la chiave privata all’ID del dispositivo e la confrontano con il valore nel token di autenticazione.  Se non corrispondono, il flusso di adesione non riesce.


<!--
## Related Information {#related}

- [Android Integration Cookbook](#)
- [Android API](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass Authentication native SDK (Tech Note)](#)
- [AE 1.6: How to resolve Gson dependencies (Tech Note)](#)
-->
