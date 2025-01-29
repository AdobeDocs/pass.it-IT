---
title: Flusso adesione programmatore
description: Flusso adesione programmatore
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '1823'
ht-degree: 0%

---

# Flusso adesione programmatore {#prog-entitlement-flow}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

Questo documento descrive il flusso di adesione di base dal punto di vista del programmatore.  Per informazioni sulle funzioni e sui casi d&#39;uso oltre l&#39;integrazione TVE di base qui descritta, vedi [Casi d&#39;uso per programmatori](/help/authentication/integration-guide-programmers/programmer-use-cases.md).

L&#39;autenticazione Adobe Pass media il flusso di adesione tra Programmatori e MVPD fornendo interfacce sicure e coerenti per entrambe le parti.  Dal lato del programmatore, Adobe Pass Authentication fornisce due tipi generali di interfaccia di adesione:

1. AccessEnabler: componente client che fornisce una libreria di API per le app su dispositivi che possono eseguire il rendering di pagine web (ad esempio, app web, app per smartphone/tablet).
2. API senza client: servizi web RESTful per dispositivi che non possono eseguire il rendering di pagine web (ad esempio set-top box, console per giochi, smart TV). Il requisito per il rendering delle pagine web deriva dal requisito di MVPD che impone agli utenti di autenticare sul sito web di MVPD.

Oltre alla panoramica indipendente dalla piattaforma presentata qui, è disponibile una panoramica specifica per le API senza client qui: documentazione API senza client. AccessEnabler viene eseguito in modalità nativa sulle piattaforme supportate (AS/JS sul Web, Objective-C su iOS e Java su Android). Le API di AccessEnabler sono coerenti tra le piattaforme supportate. Tutte le piattaforme che non supportano AccessEnabler utilizzano la stessa API senza client.

Per entrambi i tipi di interfaccia, l’autenticazione Adobe Pass media in modo sicuro il flusso di adesione tra l’app del programmatore e il MVPD dell’utente:

![](../assets/prog-entitlement-flow.png)


*Figura: Ecosistema di autenticazione Adobe Pass*

>[!IMPORTANT]
>
>Nel diagramma precedente è presente una parte del flusso di adesione che non passa attraverso i server di autenticazione di Adobe Pass: l’accesso a MVPD. Gli utenti devono accedere alla pagina di accesso del proprio MVPD. A causa di questo requisito, sui dispositivi che non possono riprodurre pagine web, l’app del programmatore deve indirizzare gli utenti a passare a un dispositivo compatibile con il web per accedere con il proprio MVPD, dopodiché devono tornare al dispositivo originale per il resto del flusso di adesione.

## Flusso diritto {#entitlement-flow}

Esistono quattro flussi secondari distinti che costituiscono il diritto di base
flusso:

1. [Flusso di avvio](/help/authentication/integration-guide-programmers/entitlement-flow.md#startup)
1. [Flusso di autenticazione](/help/authentication/integration-guide-programmers/entitlement-flow.md#authentication)
1. [Flusso di autorizzazione](/help/authentication/integration-guide-programmers/entitlement-flow.md#authorization)
1. [Flusso disconnessione](/help/authentication/integration-guide-programmers/entitlement-flow.md#logout)

Durante la visita iniziale di un utente al sito di un programmatore, il flusso di adesione procede nell’ordine indicato sopra. Tuttavia, nelle visite successive, a seconda che i token di autenticazione e autorizzazione siano scaduti o meno, o a seconda dei criteri di visualizzazione, un utente potrebbe passare attraverso solo uno o due dei flussi secondari.

### Flusso di avvio {#startup}

Stabilisce l&#39;identità del programmatore e del dispositivo, esegue le attività di inizializzazione. Questo è un prerequisito per tutte le chiamate di adesione successive.

**AccessEnabler**

* **`setRequestor()`** - Stabilisce l&#39;identità con AccessEnalber e, per estensione, con i server di autenticazione Adobe Pass. Questa chiamata è un precursore del resto del flusso di adesione. Ad esempio, in JavaScript:

  ```JavaScript
    /* Define the requestor ID (Programmer/aggregator ID). */
      var requestorID = "sample_requestor_Id";
      ...
      // Callback indicating that the AccessEnabler swf has initialized
      function swfLoaded() {
          // AccessEnabler is loaded so we can use the API function it provides
          accessEnablerObject.setRequestor(requestorID); 
      ...
      }
  ```

**API senza client**

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`** - A seconda della piattaforma, ci possono essere attività preliminari da completare prima che l&#39;app chiami regcode. Per informazioni dettagliate, consulta la **documentazione API senza client**. Ad esempio, le piattaforme Xbox richiedono di completare i passaggi di sicurezza prescritti prima di chiamare regcode.

### Flusso di autenticazione {#authentication}

Se l&#39;autenticazione ha esito positivo, viene generato un token AuthN associato al dispositivo e al richiedente. La corretta autenticazione è un prerequisito per l’autorizzazione.

**AccessEnabler**

* `checkAuthentication()` - Controlla se nella cache dei token locali esiste un token di autenticazione nella cache valido, senza attivare effettivamente il flusso di autenticazione completo. Questa operazione attiva la funzione di callback `setAuthenticationStatus()`.
* `getAuthentication()` - Avvia il flusso di autenticazione completo. In caso di esito positivo, Adobe Pass Authentication genera un token AuthN e lo memorizza in cache sul client. L’utente accede al sito MVPD selezionato, presentato in un iFrame, una finestra a comparsa o una visualizzazione web, a seconda della piattaforma. Questo attiva displayProviderDialog().

**API senza client**

* `<FQDN>/.../checkauthn` - Versione del servizio Web di `checkAuthentication()` sopra.
* `<FQDN>/.../config` - Restituisce l&#39;elenco di MVPD nell&#39;app di secondo schermo.
* `<FQDN>/.../authenticate` - Avvia il flusso di autenticazione dall&#39;app del secondo schermo, reindirizzando gli utenti al MVPD selezionato per l&#39;accesso. In caso di esito positivo, l’autenticazione Adobe Pass genera un token AuthN e lo memorizza sul server; l’utente ritorna al dispositivo originale per completare il flusso dei diritti.

Un token AuthN è considerato valido se sono soddisfatte le due condizioni seguenti:

* Il token di autenticazione non è scaduto
* Il MVPD associato al token AuthN è incluso nell’elenco degli MVPD consentiti per l’ID richiedente corrente

#### Flusso di lavoro di autenticazione iniziale di AccessEnabler generico {#generic-ae-initial-authn-flow}

1. L&#39;app avvia il flusso di lavoro di autenticazione con una chiamata a `getAuthentication()`, che verifica la presenza di un token di autenticazione nella cache valido. Questo metodo ha un parametro facoltativo `redirectURL`; se non si specifica un valore per `redirectURL`, dopo una corretta autenticazione l&#39;utente viene restituito all&#39;URL da cui è stata inizializzata l&#39;autenticazione.
1. AccessEnabler determina lo stato di autenticazione corrente. Se l&#39;utente è attualmente autenticato, AccessEnabler chiama la funzione di callback `setAuthenticationStatus()`, passando uno stato di autenticazione che indica il completamento dell&#39;operazione.
1. Se l&#39;utente non è autenticato, AccessEnabler continua il flusso di autenticazione determinando se l&#39;ultimo tentativo di autenticazione dell&#39;utente è stato eseguito correttamente con un determinato MVPD. Se un MVPD ID è memorizzato nella cache E il flag `canAuthenticate` è true OPPURE è stato selezionato un MVPD utilizzando `setSelectedProvider()`, all&#39;utente non viene visualizzata la finestra di dialogo di selezione di MVPD. Il flusso di autenticazione continua a utilizzare il valore memorizzato nella cache di MVPD, ovvero lo stesso MVPD utilizzato durante l’ultima autenticazione riuscita. Viene effettuata una chiamata di rete al server backend e l’utente viene reindirizzato alla pagina di accesso di MVPD.

1. Se non è stato selezionato alcun MVPD ID nella cache E non è stato selezionato alcun MVPD utilizzando `setSelectedProvider()` OPPURE se il flag `canAuthenticate` è impostato su false, viene chiamato il callback `displayProviderDialog()`. Questo callback indirizza l&#39;app a creare l&#39;interfaccia utente che presenta all&#39;utente un elenco di MVPD tra cui scegliere. Viene fornito un array di oggetti MVPD, contenente le informazioni necessarie per creare il selettore MVPD. Ogni oggetto MVPD descrive un’entità MVPD e contiene informazioni quali l’ID del MVPD e l’URL in cui è possibile trovare il logo MVPD.

1. Una volta selezionato un MVPD, l’app deve informare AccessEnabler della scelta dell’utente. Per i client non di Flash, una volta selezionato il MVPD desiderato, si informa AccessEnabler della selezione dell&#39;utente tramite una chiamata al metodo `setSelectedProvider()`. I client di Flash inviano invece un `MVPDEvent` condiviso di tipo &quot;`mvpdSelection`&quot;, passando il provider selezionato.

1. Quando AccessEnabler viene informato della selezione MVPD dell&#39;utente, viene effettuata una chiamata di rete al server back-end e l&#39;utente viene reindirizzato alla pagina di accesso di MVPD.

1. All’interno del flusso di lavoro di autenticazione, AccessEnabler comunica con Adobe Pass Authentication e il MVPD selezionato per richiedere le credenziali dell’utente (ID utente e password) e verificarne l’identità. Mentre alcuni MVPD reindirizzano al proprio sito per l&#39;accesso, altri richiedono di visualizzare la propria pagina di accesso all&#39;interno di un iFrame. La pagina deve includere il callback che crea un iFrame, nel caso il cliente scelga uno di questi MVPD.<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)-->.

1. Dopo che l’utente ha effettuato l’accesso, AccessEnabler recupera il token di autenticazione e informa l’app che il flusso di autenticazione è completo. AccessEnabler chiama il callback `setAuthenticationStatus()` con il codice di stato 1, che indica l&#39;esito positivo. Se si verifica un errore durante l&#39;esecuzione di questi passaggi, il callback `setAuthenticationStatus()` viene attivato con il codice di stato 0, che indica un errore di autenticazione, nonché un codice di errore corrispondente.

>[!IMPORTANT]
>Al momento, Comcast è l&#39;unico MVPD che non fornisce un URL statico per il logo. I programmatori devono richiamare i logo aggiornati più recenti dal [portale per sviluppatori XFINITY](https://developers.xfinity.com/products/tv-everywhere).
>

### Flusso di autorizzazione {#authorization}

L’autorizzazione è un prerequisito per la visualizzazione di contenuti protetti. Se l’autorizzazione è andata a buon fine, viene generato un token AuthZ insieme a un token multimediale di breve durata fornito all’app del programmatore a scopo di sicurezza. Per supportare il flusso di lavoro di autorizzazione, è necessario aver precedentemente eseguito la configurazione del richiedente necessaria e aver integrato [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier). Una volta completati, puoi avviare l’autorizzazione.

L’app avvia l’autorizzazione quando un utente richiede l’accesso a una risorsa protetta. Trasmetti un ID risorsa specificando la risorsa richiesta (ad esempio, un canale, un episodio, ecc.). L&#39;app verifica innanzitutto la presenza di un token di autenticazione archiviato. Se non ne viene trovata una, viene avviato il processo di autenticazione.

**AccessEnabler**

* `checkAuthorization()` - Controlla l&#39;autorizzazione senza avviare il flusso di autorizzazione completo. Questa funzione viene spesso utilizzata per aggiornare le informazioni sullo stato visualizzate nell’interfaccia utente dell’app Programmatore.

* `getAuthorization()` - Avvia il flusso di autorizzazione completo.

Fornisci le seguenti funzioni di callback per gestire i risultati di
la chiamata di autorizzazione:

* `setToken()` - Se l&#39;autenticazione ha avuto esito positivo in precedenza e l&#39;autorizzazione ha esito positivo, AccessEnabler chiama la funzione di callback `setToken()`, passando il token multimediale di breve durata, che indica la conclusione del flusso di adesione all&#39;autenticazione Adobe Pass. (Prima di consentire all’utente di visualizzare il contenuto protetto, l’app del programmatore controlla la validità del Media Token utilizzando il Media Token Verifier.

* `tokenRequestFailed()` - Se l&#39;utente non è autorizzato per la risorsa richiesta (o se la query non riesce per altri motivi), AccessEnabler chiama questa funzione di callback (oltre alle proprie funzioni di segnalazione degli errori), trasmettendo i dettagli dell&#39;errore.

**API senza client**

* `\<FQDN\>/.../authorize` - Avvia il flusso di autorizzazione completo.

#### Flusso di lavoro di autorizzazione di AccessEnabler generico {#generic-ae-authr-wf}

1. Fornire una funzione di callback che registri il GUID programmatore assegnato con l&#39;Access Enabler, utilizzando `setReqestor()`. Questa funzione di callback viene chiamata quando AccessEnabler è stato
scaricato correttamente.

1. Chiamare `getAuthorization()` quando un utente richiede l&#39;accesso a una risorsa protetta. Utilizzando `getAuthorization()`, passa un ID risorsa specificando la risorsa richiesta (ad esempio, un canale, un episodio, ecc.). AccessEnabler cerca un token di autenticazione memorizzato nella cache da passare con la richiesta di autorizzazione. Se non ne viene trovato uno, avvia il flusso di autenticazione.
1. Fornisci funzioni di callback per gestire i risultati dell’autorizzazione:

   * `setToken()` - Se l&#39;autorizzazione ha esito positivo o se l&#39;utente è stato precedentemente autorizzato, Access Enabler continua il processo di autorizzazione chiamando la funzione di callback `setToken()` e passando il token di autorizzazione di breve durata.

   * `tokenRequestFailed()` - Se l&#39;utente non è autorizzato per la risorsa richiesta (o se la query non riesce per altri motivi), AccessEnabler richiama tutte le funzioni di segnalazione errori registrate, più il callback `tokenRequestFailed()`, trasmettendo i dettagli sull&#39;errore.

### Flusso di disconnessione {#logout}

Cancella i token e gli altri dati associati al flusso di adesione dell&#39;utente corrente.

**AccessEnabler**

* `logout()`

**API senza client**

* `\<FQDN\>/.../logout`

## Comprendere il comportamento di AccessEnabler {#ae-behavior}

Tutte le chiamate API di AccessEnabler sono asincrone (con una eccezione, indicata nei riferimenti API). Puoi chiamare un’API un numero arbitrario di volte, tuttavia non c’è una solida garanzia che le azioni siano state attivate
dalle chiamate saranno completate nello stesso ordine in cui sono state effettuate le chiamate. (Fa eccezione il runtime di Flash Player corrente; non essendo multi-thread, le chiamate *do* verranno completate nell&#39;ordine
vengono chiamati.)

Per distinguere le risposte ed essere in grado di associare le risposte alle chiamate, tutti i callback eseguono l&#39;echo dei parametri di input. Sono inclusi `setToken()` e `tokenRequestFailed()`, attivati in ultima istanza da `checkAuthorization()`. (Per `checkAuthorization()` callback, la risorsa utilizzata viene ripetuta). Sfruttando questa funzione, puoi distinguere quale risposta corrisponde a quale chiamata. Per utilizzare questa funzione è possibile codificare un codice simile al seguente:

```JavaScript
    for each (resource in ["TNT", "CNN", "TBS", "AdultSwim"] ) {
         ae.checkAuthorization(resource);
    }
    
    // Success callback
    function setToken(resource, token) {
         // Use "resource" to figure 
         // out which checkAuthorization
         // call triggered this response
    }
    
    // Old error callback
    function tokenRequestFailed(resource, error, details) {
         // use "resource" to figure
         // out in response to which
         // checkAuthorization call
         // this was triggered
    }
    
    // Error callback using new error api
    ae.bind("errorEvent',"errorHandler");
    
    function errorHandler(error) {
         if(error.resource) {        
              // Use error.resource to figure
              // which checkAuthorization call
              // triggered this response
         }
    }
```

### Domande frequenti sul comportamento AE {#ae-beh-faq}

**Domanda. Cosa succede se effettuo una seconda chiamata di AccessEnabler prima del termine della prima chiamata?**

La prima chiamata continua a essere eseguita durante l’esecuzione della seconda chiamata (comunicazioni asincrone).

**Domanda. Esiste un numero massimo di chiamate simultanee che AccessEnabler può supportare?**

Poiché nel codice AccessEnabler non viene impostato esplicitamente alcun limite, l&#39;utente è limitato solo dalle risorse di sistema disponibili e dalla capacità di MVPD.

<!--

>[!MORELIKETHIS]
>
>*   [Programmer use cases](/help/authentication/programmer-use-cases.md)
>*   Error Reporting
>**Platform Cookbooks:**
>*   Clientless integration cookbook
>*   iOS Integration Cookbook
>*   Android Integration Cookbook
>*   JavaScript Integration Cookbook
>*   ActionScript Integration Cookbook
>*   Windows 8 Integration Cookbook
>*   AIR Native Extension Overview
-->
