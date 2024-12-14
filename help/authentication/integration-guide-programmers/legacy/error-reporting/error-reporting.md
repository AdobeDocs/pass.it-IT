---
title: Segnalazione errori
description: Segnalazione errori
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3012'
ht-degree: 0%

---

# (Legacy) Segnalazione errori {#error-reporting}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Panoramica {#overview}

La segnalazione degli errori nell’autenticazione di Adobe Pass è attualmente implementata in due modi diversi:

* **Segnalazione avanzata errori** L&#39;implementatore registra un callback di errore in caso di [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk) o implementa un metodo Interface denominato &quot;`status`&quot; in caso di [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk) e [AccessEnabler Android SDK](#accessenabler-android-sdk), per ricevere la segnalazione avanzata degli errori. Gli errori sono suddivisi in **Informazioni**, **Avviso** e **Errore**. Il sistema di reporting è **asincrono**, in quanto **non è garantito l&#39;ordine in cui verranno attivati più errori**.  Per informazioni dettagliate sul sistema di segnalazione errori avanzato, vedere la sezione [Segnalazione errori avanzata](#advanced-error-reporting).

* **Segnalazione errori originale -** Sistema di segnalazione statico in cui i messaggi di errore vengono trasmessi a funzioni di callback specifiche quando richieste specifiche non riescono. Gli errori sono raggruppati in tipi generici, di autenticazione e di autorizzazione. Per l&#39;elenco degli errori segnalati nel sistema originale, vedere la sezione [Segnalazione errori originale](#original-error-reporting).


## Segnalazione avanzata degli errori {#advanced-error-reporting}

* [SDK di AccessEnabler JavaScript](#accessenabler-javascript-sdk)
* [SDK di AccessEnabler iOS/tvOS](#accessenabler-ios-tvos-sdk)
* [SDK di AccessEnabler Android](#accessenabler-android-sdk)
* [SDK di AccessEnabler FireOS](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>La vecchia API di Segnalazione errori [Originale](#original-error-reporting) continuerà a funzionare come in precedenza, la Segnalazione errori avanzata non interrompe la funzionalità, ma la Segnalazione errori originale NON riceverà più aggiornamenti. Tutti i nuovi errori e aggiornamenti verranno eseguiti dal sistema di segnalazione errori avanzato.

### SDK di AccessEnabler JavaScript {#accessenabler-javascript-sdk}

Il nuovo sistema di segnalazione degli errori rimane facoltativo, pertanto l’implementatore può registrare esplicitamente un callback del gestore degli errori per ricevere segnalazioni avanzate degli errori. Il sistema include la possibilità di registrare e annullare la registrazione di più callback di errore in modo dinamico. Inoltre, è possibile registrare nuovi callback di errore non appena viene caricato AccessEnabler JavaScript SDK, senza dover eseguire altre inizializzazioni (prima di chiamare `setRequestor()`), il che significa che è possibile ricevere report avanzati sugli errori di inizializzazione e configurazione.


#### Implementazione {#access-enab-js-imp}

yourErrorHandler(errorData:Object)


La funzione di callback del gestore degli errori riceverà un singolo oggetto (una mappa) con la seguente struttura:

```JavaScript
    {
      errorId: "CFG410",
      level: "error",
      message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

### 1. Legare {#bind}

**`.bind(eventType:String, handlerName:String):void`**

Collega un gestore per un evento.

**`eventType`** - SOLO il valore &quot;`errorEvent`&quot; genera in AccessEnabler JavaScript SDK l&#39;attivazione di callback di report di errori avanzati.

**`handlerName`** - una stringa che specifica il nome della funzione del gestore errori.


Entrambi i parametri di associazione devono utilizzare solo i caratteri del seguente set: `[0-9a-zA-Z][-._a-zA-Z0-9]`. I parametri devono iniziare con un numero o una lettera e possono quindi includere solo trattini, punti, trattini bassi e caratteri alfanumerici.  Inoltre, i parametri non possono superare i 1024 caratteri.

**Esempio** di gestori errori di associazione:

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

A causa di limitazioni tecniche, non è possibile associare una chiusura o una funzione anonima. È necessario specificare il nome del metodo nel secondo parametro.


### 2. Separa {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

Rimuove un gestore eventi precedentemente associato.

**`eventType`** - SOLO il valore &#39;`errorEvent`&#39; genera in AccessEnabler JavaScript SDK l&#39;attivazione di callback di report di errori avanzati.

**`handlerName`** - una stringa che specifica il nome della funzione del gestore errori, se è null o mancano tutti i gestori associati per il `eventType` specificato verrà rimossa.

Entrambi i parametri di associazione devono utilizzare solo i caratteri del seguente set: `[0-9a-zA-Z][-._a-zA-Z0-9]`. I parametri devono iniziare con un numero o una lettera e possono quindi includere solo trattini, punti, trattini bassi e caratteri alfanumerici.  Inoltre, i parametri non possono superare i 1024 caratteri.

**Esempio** di rimozione di un singolo gestore errori:

`accessEnabler.unbind('errorEvent', 'errorLogger');`

**Esempio** rimozione di tutti i gestori errori:

`accessEnabler.unbind('errorEvent');`


### SDK di AccessEnabler iOS/tvOS {#accessenabler-ios-tvos-sdk}

Il nuovo sistema di segnalazione degli errori è obbligatorio, pertanto l’implementatore deve conformarsi esplicitamente al nuovo protocollo &quot;EntitlementStatus&quot; dell’Obiettivo C. Questo nuovo approccio consente ai programmatori di ricevere report avanzati sugli errori.

#### Implementazione {#accessenab-ios-tvossdk-imp}

Un implementatore deve essere conforme al seguente protocollo **EntitlementStatus**:

**StatoDiritto.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

La funzione **status** riceverà un singolo oggetto (un `NSDictionary`) con la seguente struttura:

```OBJ-C
    {
          errorId: "CFG410",
          level: "error",
          message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

**1. Dichiarazione**

```OBJ-C
    @interface MyEntitlementStatusDelegate : NSObject <EntitlementStatus>
```

**2. Implementazione**

```OBJ-C
    @implementation DemoAppAppDelegate     
    // very simple implementation
    - (void)status:(NSDictionary *)statusDict {
        NSLog(@"%@:\n%@", statusDict[@"level"], statusDict);
    }
```


### SDK di AccessEnabler Android {#accessenabler-android-sdk}

Il nuovo sistema di segnalazione errori è obbligatorio perché l&#39;implementatore deve essere conforme in modo esplicito al protocollo definito dall&#39;interfaccia `IAccessEnablerDelegate`. Questo nuovo approccio consente ai programmatori di ricevere report avanzati sugli errori.

#### Implementazione {#access-enablr-androidsdk-imp}

Un implementatore deve gestire il nuovo metodo `status` dall&#39;interfaccia`IAccessEnablerDelegate`. La funzione **`status`** riceverà un singolo oggetto **`AdvancedStatus`** con il seguente modello:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Esempio**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

### SDK di AccessEnabler FireOS {#accessenabler-fireos-sdk}


Il nuovo sistema di segnalazione errori è obbligatorio perché l&#39;implementatore deve essere conforme in modo esplicito al protocollo definito dall&#39;interfaccia `IAccessEnablerDelegate`. Questo nuovo approccio consente ai programmatori di ricevere report avanzati sugli errori.

#### Implementazione {#access-enab-fireos-sdk-}

Un implementatore deve gestire il nuovo metodo `status` dall&#39;interfaccia`IAccessEnablerDelegate`. La funzione **`status`** riceverà un singolo oggetto **`AdvancedStatus`** con il seguente modello:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Esempio**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

## Riferimento codici di errore avanzati {#advanced-error-codes-reference}

La tabella seguente elenca e descrive i codici di errore esposti dalla nuova API di errore, insieme alle azioni suggerite da intraprendere per correggerli:

| ID | Livello | Descrizione | Azioni sviluppatore | Azioni utente | JavaScript | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| AAPL&amp; AAPL_ERROR | Errore | Errore SSO Apple generico | L’errore contiene un campo dei dettagli con l’errore VSA originale. | n/d | n/d | Sì | n/d |
| VSA203 | Info | L’utente ha deciso di uscire dall’applicazione mentre si verificava l’autenticazione a seguito di un accesso tramite l’SSO della piattaforma. | Indica/richiedi all&#39;utente di disconnettersi esplicitamente da Impostazioni -> Account -> Provider TV su tvOS. <br><br> Indica/richiede all&#39;utente di disconnettersi esplicitamente da Impostazioni -> Provider TV su iOS/iPadOS. | Disconnettersi in modo esplicito da Impostazioni -> Account -> Provider TV in tvOS. <br> <br> Esci esplicitamente da Impostazioni -> Provider TV su iOS/iPadOS | n/d | Sì | n/d |
| VSA404 | Info | L&#39;autorizzazione per l&#39;account sottoscrittore video dell&#39;applicazione non è determinata. | Incentivare gli utenti che rifiutano di concedere l&#39;autorizzazione per accedere alle informazioni sull&#39;abbonamento spiegando i vantaggi dell&#39;esperienza utente Single Sign-On (SSO). | L’utente può modificare la propria decisione andando nelle impostazioni dell’applicazione (accesso al provider TV) o nella sezione da Impostazioni -> Provider TV su iOS/iPadOS o Impostazioni -> Account -> Provider TV su tvOS. | n/d | Sì | n/d |
| VSA503 | Info | Richiesta metadati dell&#39;account del sottoscrittore video dell&#39;applicazione non riuscita. | L’endpoint MVPD non risponde. L’applicazione potrebbe eseguire il fallback a un flusso di autenticazione regolare. | n/d | n/d | Sì | n/d |
| 500 | Errore | Errore interno | Utilizza AccessEnablerDebug ed esamina i registri di debug (output console.log) per determinare cosa è andato storto. | n/d | Sì | Sì | n/d |
| SEC403 | Errore | Errore di sicurezza del dominio. Il richiedente utilizza un dominio non valido. Tutti i domini utilizzati da un particolare ID richiedente devono essere inseriti nella whitelist da Adobe. | - Carica AccessEnabler solo dall&#39;elenco dei domini consentiti <br> <br> - Contatta l&#39;Adobe per gestire la whitelist del dominio per l&#39;ID richiedente utilizzato <br> <br> - iOS: verificare di utilizzare il certificato corretto e che la firma sia stata creata correttamente | n/d | n/d | Sì | n/d |
| SEC412 | Avvertenza | [Disponibile nella versione 2.5] L&#39;ID dispositivo non corrisponde. Questo può accadere ogni volta che la piattaforma sottostante cambia il proprio ID dispositivo. In questo caso, i token esistenti verranno cancellati e l’utente non verrà più autenticato. Tieni presente che ciò accade legittimamente quando l’utente utilizza JS SDK ed è in roaming (su JS l’IP client fa parte dell’ID dispositivo). In caso contrario, potrebbe essere un’indicazione di un tentativo di frode, un tentativo di copiare token da un dispositivo diverso. | - Monitora il numero di avvisi. Se il picco non ha un motivo apparente (nessun aggiornamento recente del browser; nuovi sistemi operativi), potrebbe indicare tentativi di frode.  <br> <br>- Facoltativamente, informare l&#39;utente che deve effettuare di nuovo l&#39;accesso. | Accedi di nuovo. | Sì | Sì | Sì da 3.2 |
| SEC420 | Errore | Errore di sicurezza HTTP durante la comunicazione con i server di autenticazione Adobe Pass. Questo errore si verifica in genere quando sono presenti spoofing o proxy. | - Carica `[https://]{SP_FQDN\}` nel browser e accetta manualmente i certificati SSL, ad esempio **https://api.auth.adobe.com** o **https://api.auth-staging.adobe.com** <br> <br>- Contrassegna i certificati proxy come attendibili | Se questo accade per un utente normale, è un&#39;indicazione di un possibile attacco man-in-the-middle! | Sì | Sì | Sì da 3.2 |
| CFG100 | Avvertenza | Data/ora/fuso orario del computer client non impostato correttamente. Questo potrebbe causare errori di autenticazione/autorizzazione. | - Informa l&#39;utente per impostare l&#39;ora corretta. <br> <br> Consente di eseguire azioni per impedire i flussi di adesione, poiché probabilmente non riusciranno. | Impostare la data/ora corretta. | Sì | Sì | Sì da 3.2 |
| CFG400 | Errore | È stato fornito un ID richiedente non valido. | Lo sviluppatore DEVE specificare un ID richiedente valido. | n/d | Sì | Sì | Sì da 3.2 |
| CFG404 | Errore | Server di autenticazione Adobe Pass non trovati. Ciò può accadere in 3 istanze: <br><br> - Lo sviluppatore dispone di uno spoofing non valido. <br><br> - L&#39;utente ha problemi di rete e non può raggiungere i domini di autenticazione di Adobe Pass. <br><br> - Configurazione errata dei server di autenticazione Adobe Pass. <br><br>  **Nota:** su Firefox, CFG400 verrà visualizzato al posto di CFG404 (limitazione del browser) | - Controllare la falsificazione. <br><br> - Controllare le impostazioni di rete/DNS. <br><br> - Informa Adobe. | Verificare le impostazioni di rete/DNS. | Sì | Sì | Sì da 3.2 |
| CFG410 | Errore | AccessEnabler è troppo vecchio. | Informa l’utente per cancellare le cache. | Cancella la cache del browser. | Sì | n/d | Sì da 3.2 |
| CFG5xx | Errore | Si sono verificati errori interni nei server di autenticazione di Adobe Pass. xx può essere un numero qualsiasi. | - Informa l’utente dell’indisponibilità dell’autenticazione Adobe Pass. <br><br> - Ignora l&#39;autenticazione Adobe Pass. <br> <br> - Informa Adobe. | Riprova più tardi. | Sì | Sì | Sì da 3.2 |
| N000 | Info | Utente non autenticato. | n/d | Accedi. | Sì | Sì | Sì da 3.2 |
| N001 | Info | Tentativo di autenticazione passiva avviato in background. Ciò si verificherà per gli MVPD configurati con &quot;Autenticazione per richiedente&quot;. Anche se si spera che l’utente venga autenticato automaticamente, questo comporta una penalizzazione delle prestazioni al momento dell’inizializzazione. | Facoltativamente, informa l’utente, o presenta l’interfaccia utente che lo avvisa, che &quot;il lavoro è in corso&quot;. | Aspetta. | Sì | Sì | Sì da 3.2 |
| N003 | Info | L’utente seleziona l’opzione &quot;Other TV Provider&quot; (Altro provider TV) dal selettore MVPD di Apple. | Verrà chiamato il callback *displayProviderDialog* e l&#39;applicazione potrebbe eseguire il fallback al normale flusso di autenticazione. | Seleziona MVPD normale e continua con la schermata di accesso. | n/d | Sì | n/d |
| N004 | Info | L&#39;utente seleziona un provider TV non supportato dal richiedente corrente. | Verrà chiamato il callback *displayProviderDialog* e l&#39;applicazione potrebbe eseguire il fallback al normale flusso di autenticazione. | Seleziona MVPD normale e continua con la schermata di accesso. | n/d | Sì | n/d |
| N005 | Info | Il selettore MVPD è stato annullato. | n/d | n/d | Sì | Sì | Sì da 3.2 |
| N010 | Avvertenza | L&#39;utente è stato autenticato mentre era attiva la regola di degradazione autentica-tutto per il MVPD selezionato. | Facoltativamente, informa l’utente che sta ottenendo un accesso gratuito &quot;gratuito&quot; a causa di difficoltà di MVPD. | n/d | Sì | Sì | Sì da 3.2 |
| N011 | Info | L&#39;utente è stato autenticato tramite TempPass. | - Informare l&#39;utente. <br> <br> - Facoltativamente presentare un elenco di MVPD regolari. | Facoltativamente, accedi con il MVPD normale. | Sì | Sì | Sì da 3.2 |
| N111 | Avvertenza | TempPass scaduto. | - Informare l&#39;utente. <br> <br> - Presenta un elenco di MVPD regolari. <br> <br> - Nasconde l&#39;opzione TempPass. | Accedi con il tuo normale MVPD. | Sì | Sì | Sì da 3.2 |
| N130 | Errore | **Token di autenticazione non trovato nella sessione.** Il problema può essere dovuto a uno dei seguenti motivi: <br> <br> 1 Il browser ha i cookie (di terze parti) disabilitati (non applicabile per AccessEnabler JavaScript SDK versione 4.x) <br> <br> 2. Il browser ha abilitato la funzione Impedisci il rilevamento intersito (Safari 11+) <br> <br> 3. La sessione è scaduta <br> <br> 4. Il programmatore chiama le API di autenticazione con una successione errata <br> <br> NOTA: questo codice di errore non è disponibile per i flussi di autenticazione di reindirizzamento a pagina intera. | 1. Richiedere all&#39;utente di abilitare i cookie (di terze parti) <br> <br> 2. Richiede all&#39;utente di disabilitare il rilevamento intersito <br> <br> 3. Richiedi all&#39;utente di riautenticare <br> <br> 4. Chiamare le API nell’ordine corretto | 1. Abilitare i cookie di terze parti <br> <br> 2. Disattiva il rilevamento intersito <br> <br> 3. Autentica di nuovo <br> <br> 4. N/D | Sì | Sì | Sì da 3.2 |
| N500 | Errore | Errore interno. <br> <br> Nota: questo è l&#39;errore originale del sistema &quot;Errore di autenticazione generico&quot; e &quot;Errore di autenticazione interna&quot;. Questo errore verrà gradualmente eliminato. | Utilizza AccessEnablerDebug ed esamina i registri di debug (output console.log) per determinare cosa è andato storto. | n/d | Sì | Sì | n/d |
| R401 | Errore | Errore durante il tentativo di ottenere un token di accesso. <br> <br> Nota: errore irreversibile. Informare l&#39;utente che l&#39;applicazione non è disponibile. | - iOS: controlla l’informativa software e gli schemi personalizzati nell’applicazione. <br> <br> - JavaScript: controllare l&#39;istruzione software nell&#39;applicazione del sito Web. <br> <br> Aprire un ticket utilizzando Zendesk e informare l&#39;utente che il sistema è temporaneamente non disponibile | n/d | Sì dalla versione 4.0 | Sì dalla versione 3.0 | Sì da 3.2 |
| R400 | Errore | Applicazione non registrata. Dichiarazione software non valida o revocata. <br> <br> Nota: errore irreversibile. Informare l&#39;utente che l&#39;applicazione non è disponibile. | - iOS: controlla l’informativa software e gli schemi personalizzati nell’applicazione. <br> <br> - JavaScript: controllare l&#39;istruzione software nell&#39;applicazione del sito Web. <br> <br> Aprire un ticket utilizzando Zendesk e informare l&#39;utente che il sistema è temporaneamente non disponibile | n/d | Sì dalla versione 4.0 | Sì dalla versione 3.0 | Sì da 3.2 |
| REG500 | Errore | Impossibile recuperare il codice di registrazione dal server. <br> <br> Nota: errore irreversibile. Informare l&#39;utente che l&#39;applicazione non è disponibile. | Apri un ticket utilizzando Zendesk e informa l’utente che il sistema è temporaneamente non disponibile. | n/d | Sì dalla versione 4.0 | Sì dalla versione 3.0 | Sì da 3.2 |
| REGCODE | Completato | L’applicazione ha chiamato l’API setSelectedProvider sulla piattaforma tvOS. | Indica/richiede all&#39;utente di utilizzare un secondo dispositivo (schermata) per accedere utilizzando il codice di registrazione fornito. | Utilizza il codice regcode su un secondo dispositivo (schermata) per avviare l’autenticazione. | n/d | Sì Solo per tvOS | n/d |
| Z010 | Avvertenza | L&#39;utente è stato autorizzato mentre era attiva la regola di degradazione authentication-all o authorize-all per il MVPD selezionato. | Facoltativamente, informa l’utente che sta ottenendo un accesso gratuito &quot;gratuito&quot; a causa di difficoltà di MVPD. | n/d | Sì | Sì | Sì da 3.2 |
| Z011 | Info | L&#39;utente è stato autorizzato tramite TempPass | Facoltativamente, informare l&#39;utente | n/d | Sì | Sì | Sì da 3.2 |
| Z100 | Errore | Autorizzazione non riuscita perché l&#39;utente non dispone di un abbonamento per la risorsa richiesta o per altri motivi provenienti da MVPD, ad esempio perché il video non corrisponde alle impostazioni di Controllo genitori per l&#39;account utente | - Non consentire la riproduzione. <br> <br> - Informare l&#39;utente. <br> <br> - La chiave &#39;message&#39; nel messaggio di errore POTREBBE contenere un messaggio più dettagliato fornito da MVPD. | n/d | Sì | Sì | Sì da 3.2 |
| Z110 | Errore | Autorizzazione negata a causa di ripetuti rifiuti MVPD. Possibile tentativo di frode o DOS. | - Non consentire la riproduzione. <br> <br> - Informare l&#39;utente. | n/d | Sì | Sì | Sì da 3.2 |
| Z120 | Errore | Autorizzazione negata per motivi tecnici nella comunicazione con MVPD. Possibile errore di rete. | - Non consentire la riproduzione. <br> <br> - Informare l&#39;utente che si sono verificati problemi in MVPD e riprovare più tardi. | Riprova più tardi. | Sì | Sì | Sì da 3.2 |
| Z130 | Errore | Autorizzazione negata perché è stata utilizzata una risorsa non valida o non valida. | Controlla la stringa di risorsa e correggila. In genere questo errore è dovuto a un MRSS non valido o all&#39;utilizzo di una stringa semplice al posto di MRSS. | n/d | Sì | Sì | Sì da 3.2 |
| Z169 | Errore | Autorizzazione negata perché è stata applicata la regola di degradazione authzNone per la risorsa specificata. | Informa l’utente | n/d | Sì | Sì | Sì da 3.2 |
| Z500 | Errore | Errore interno. <br> <br> Nota: si tratta dell&#39;errore di autenticazione generico e dell&#39;errore di autenticazione interna legacy. Questo errore verrà gradualmente eliminato. | Utilizza AccessEnablerDebug ed esamina i registri di debug (output console.log) per determinare cosa è andato storto. | n/d | Sì | Sì | Sì da 3.2 |
| P100 | Errore | Preautorizzazione non riuscita. Molto probabilmente ciò è dovuto alla richiesta di autorizzazione per troppe risorse. | - NON utilizzare un numero di risorse superiore a quello massimo consentito. <br> <br> - Contattare il supporto Adobe Pass per l&#39;autenticazione per trovare/impostare il numero massimo di risorse consentite. | n/d | Sì dalla versione 3.0 | Sì | Sì da 3.2 |
| IS2XX | Errore | Questi codici di errore vengono restituiti quando i dati di risposta dell&#39;endpoint del server di personalizzazione hanno un formato non valido o mancano le informazioni di personalizzazione richieste. | Apri un ticket utilizzando Zendesk e informa l’utente che il sistema è temporaneamente non disponibile | n/d | Sì dalla versione 3.0 | n/d | n/d |
| IS4XX | Errore | Questi codici di errore vengono restituiti in caso di errore dell’endpoint del server di individualizzazione 4XX: è il codice di stato HTTP della risposta. | Apri un ticket utilizzando Zendesk e informa l’utente che il sistema è temporaneamente non disponibile | n/d | Sì dalla versione 3.0 | n/d | n/d |
| IS5XX | Errore | Questi codici di errore vengono restituiti in caso di errore dell’endpoint del server di individualizzazione 5XX: è il codice di stato HTTP della risposta. | Apri un ticket utilizzando Zendesk e informa l’utente che il sistema è temporaneamente non disponibile | n/d | Sì dalla versione 3.0 | n/d | n/d |
| IS0 | Errore | Questo codice viene restituito quando l&#39;endpoint del server di individuazione non risponde affatto, pertanto la connessione è scaduta | Apri un ticket utilizzando Zendesk e informa l’utente che il sistema è temporaneamente non disponibile | n/d | Sì dalla versione 3.0 | n/d | n/d |
| LS011 | Avvertenza | AccessEnabler utilizza un&#39;archiviazione volatile a causa di problemi LSO/LocalStorage e di problemi di WebStorage (o di indisponibilità). <br> Autenticazione/autorizzazione <br> non persiste oltre la pagina corrente. A ogni caricamento di pagina l’utente dovrà eseguire l’autenticazione. I TTL configurati non vengono applicati nei diversi ricaricamenti delle pagine. | - Informare l&#39;utente delle limitazioni. <br> <br> - Informare l&#39;utente su come aumentare lo spazio di archiviazione disponibile. <br> <br> - In alternativa, disconnettersi per cancellare l&#39;archiviazione. | - Aumentare lo storage. <br> <br> - Disconnettersi per cancellare l&#39;archiviazione. | Sì | n/d | n/d |

<br>

## Segnalazione errori originale {#original-error-reporting}

Questa sezione descrive il sistema di segnalazione degli errori originale e i codici di errore originali. Nel sistema di segnalazione degli errori originale AccessEnabler trasmette gli errori alle due funzioni di callback seguenti: `setAuthenticationStatus()` dopo una chiamata a `checkAuthentication()`; `tokenRequestFailed()`, dopo l&#39;errore di una chiamata a `checkAuthorization()` o `getAuthorization()`.

La segnalazione degli errori originale e le API di stato continuano a funzionare esattamente come prima. Tuttavia, andando avanti le API di segnalazione errori originali non verranno aggiornate. Tutte le nuove segnalazioni di errori e gli aggiornamenti relativi ai vecchi errori verranno rispecchiati SOLO nel nuovo [sistema Avanzato di Segnalazione errori](#advanced-error-reporting).


Per esempi di utilizzo del sistema di segnalazione errori originale, vedere le funzioni [Riferimento API di JavaScript](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md):[setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#set-authn-status-isauthn-error) e [tokenRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#token-request-failed-error-msg), [Riferimento API di iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setAuthNStatus)e [tokentRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#tokenReqFailed), [Riferimento API di Android](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus) e [tokenRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed).

### Codici di errore del callback originale {#original-callback-error-codes}

| **Errori generici** | |
|---|---|
| Errore interno | Si è verificato un errore di sistema durante il tentativo di elaborare la richiesta. |
| Errore provider non selezionato | Si verifica in seguito all&#39;annullamento del cliente nella finestra di dialogo di selezione del provider. |
| Errore provider non disponibile | Si verifica quando non è disponibile alcun provider. |
|  |  |
| **Errori di autenticazione** | |
| Errore di autenticazione generica | Restituito quando il motivo non è noto o non può essere pubblicato. |
| Errore di autenticazione interna | Si è verificato un errore di sistema durante il tentativo di autenticazione. |
| Errore utente non autenticato | Utente non autenticato. |
|  |  |
| **Errori di autorizzazione** |  |
| Errore di autorizzazione generica | Restituito quando il motivo non è noto o non può essere pubblicato. |
| Errore di autorizzazione interna | Si è verificato un errore di sistema durante il tentativo di autorizzazione. |
| Errore utente non autorizzato | Il cliente non è autorizzato a visualizzare il contenuto richiesto. |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
