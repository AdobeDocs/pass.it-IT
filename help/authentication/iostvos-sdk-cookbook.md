---
title: Manuale di iOS/tvOS
description: Manuale di iOS/tvOS
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '2404'
ht-degree: 0%

---

# Manuale dell’SDK iOS/tvOS {#iostvos-sdk-cookbook}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#intro}

Questo documento descrive i flussi di lavoro di adesione che l’applicazione di livello superiore di un programmatore può implementare tramite le API esposte dalla libreria AccessEnabler di iOS/tvOS.

La soluzione di autenticazione Adobe Pass per iOS/tvOS è infine suddivisa in due domini:

* Dominio dell’interfaccia utente: livello applicativo superiore che implementa l’interfaccia utente e utilizza i servizi forniti dalla libreria AccessEnabler per fornire accesso a contenuti con restrizioni.

* Dominio AccessEnabler: qui vengono implementati i flussi di lavoro per l’adesione sotto forma di:

   * Chiamate di rete effettuate ai server back-end di Adobe
   * Regole della logica di business relative ai flussi di lavoro di autenticazione e autorizzazione
   * Gestione di varie risorse ed elaborazione dello stato del flusso di lavoro (ad esempio la cache dei token)

L&#39;obiettivo del dominio AccessEnabler è nascondere tutte le complessità dei flussi di lavoro di adesione e fornire all&#39;applicazione di livello superiore (tramite la libreria AccessEnabler) un set di semplici primitive di adesione con cui implementare i flussi di lavoro di adesione:

1. Imposta l&#39;identità del richiedente
1. Verifica e ottieni l’autenticazione per un determinato provider di identità
1. Controllare e ottenere l’autorizzazione per una particolare risorsa
1. Disconnetti
1. Flussi SSO Apple tramite proxy del framework Apple VSA

L&#39;attività di rete di AccessEnabler si svolge nel proprio thread, pertanto il thread dell&#39;interfaccia utente non viene mai bloccato. Di conseguenza, il canale di comunicazione bidirezionale tra i due domini dell’applicazione deve seguire un modello completamente asincrono:

* Il livello applicazione dell’interfaccia utente invia messaggi al dominio AccessEnabler tramite le chiamate API esposte dalla libreria AccessEnabler.
* AccessEnabler risponde al livello dell&#39;interfaccia utente tramite i metodi di callback inclusi nel protocollo AccessEnabler che il livello dell&#39;interfaccia utente registra con la libreria AccessEnabler.

## Configurazione del servizio ID Experience Cloud (ID visitatore) {#visitorIDSetup}

Configurazione di [ID EXPERIENCE CLOUD](https://experienceleague.adobe.com/docs/id-service/using/home.html) valore è importante dal [!DNL Analytics] punto di vista. Una volta al `visitorID` viene impostato, l&#39;SDK invia queste informazioni insieme a ogni chiamata di rete e [!DNL Adobe Pass] Il server di autenticazione raccoglie queste informazioni. Puoi correlare le analisi del servizio di autenticazione di Adobe Pass con qualsiasi altro rapporto di analisi disponibile in altre applicazioni o siti web. Puoi trovare informazioni su come impostare visitorID [qui](#setOptions).

## Flussi di diritti {#entitlement}

R.  [Prerequisiti](#prereqs) </br>
B.  [Flusso di avvio](#startup_flow) </br>
C.  [Flusso di autenticazione con Apple SSO](#authn_flow_wo_applesso)  </br>
D.  [Flusso di autenticazione con Apple SSO su iOS](#authn_flow_with_applesso) </br>
E.  [Flusso di autenticazione con Apple SSO su tvOS](#authn_flow_with_applesso_tvOS) </br>
F.  [Flusso di autorizzazione](#authz_flow) </br>
G.  [Visualizza flusso multimediale](#media_flow) </br>
H.  [Flusso di disconnessione senza Apple SSO](#logout_flow_wo_AppleSSO) </br>
I.  [Flusso di disconnessione con Apple SSO](#logout_flow_with_AppleSSO) </br>


### A. Prerequisiti {#prereqs}

1. Creare le funzioni di callback:
   * `setRequestorComplete()` </br>
   * Attivato da [setRequestor()](#$setReq), restituisce l&#39;esito positivo o negativo. </br>
   * Il successo indica che puoi procedere con le chiamate di adesione.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Attivato da [`getAuthentication()`](#$getAuthN) solo se l’utente non ha selezionato un provider (MVPD) e non è ancora autenticato. </br>
      * Il `mvpds` Il parametro è un array di provider disponibili per l&#39;utente.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Attivato da `checkAuthentication()` ogni volta. </br>
      * Attivato da [`getAuthentication()`](#$getAuthN) solo se l’utente è già autenticato e ha selezionato un provider. </br>
      * Lo stato restituito è success o failure, il codice di errore descrive il tipo di errore.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Attivato da [`getAuthentication()`](#$getAuthN) dopo che l’utente ha selezionato un MVPD. Il `url` Il parametro fornisce la posizione della pagina di accesso di MVPD.

   * `sendTrackingData(event, data)` </br>
      * Attivato da `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`.
      * Il `event` il parametro indica quale evento di adesione si è verificato; il `data` Il parametro è un elenco di valori relativi all’evento.

   * `setToken(token, resource)`

      * Attivato da [checkAuthorization()](#checkAuthZ) e [getAuthorization()](#$getAuthZ) dopo un’autorizzazione riuscita a visualizzare una risorsa.
      * Il `token` è il token multimediale di breve durata; il `resource` Il parametro è il contenuto che l’utente è autorizzato a visualizzare.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Attivato da [checkAuthorization()](#checkAuthZ) e [getAuthorization()](#$getAuthZ) dopo un’autorizzazione non riuscita.
      * Il `resource` parametro è il contenuto che l&#39;utente stava tentando di visualizzare; il `code` parametro è il codice di errore che indica il tipo di errore che si è verificato; il `description` Il parametro descrive l’errore associato al codice di errore.

   * `selectedProvider(mvpd)` </br>
      * Attivato da [`getSelectedProvider()`](#getSelProv).
      * Il `mvpd` Il parametro fornisce informazioni sul provider selezionato dall&#39;utente.

   * `setMetadataStatus(metadata, key, arguments)`
      * Attivato da `getMetadata().`
      * Il `metadata` Il parametro fornisce i dati specifici richiesti; il `key` Il parametro è la chiave utilizzata nel [getMetadata()](#getMeta) richiesta; e `arguments` Il parametro è lo stesso dizionario che è stato passato a [getMetadata()](#getMeta).

   * [&quot;preauthorizedResources(authorizedResources)&quot;](#preauthResources)

      * Attivato da [`checkPreauthorizedResources()`](#checkPreauth).

      * Il `authorizedResources` Il parametro presenta le risorse che l’utente è autorizzato a visualizzare.

   * [&quot;presentTvProviderDialog(viewController)&quot;](#presentTvDialog)

      * Attivato da [getAuthentication()](#getAuthN) quando il richiedente corrente supporta almeno su MVPD che dispone di supporto SSO.
      * Il parametro viewController è la finestra di dialogo SSO di Apple e deve essere presentato sul controller della visualizzazione principale.

   * [&quot;dismissTvProviderDialog(viewController)&quot;](#dismissTvDialog)

      * Attivato da un’azione dell’utente (selezionando &quot;Annulla&quot; o &quot;Altri provider TV&quot; dalla finestra di dialogo SSO di Apple).
      * Il parametro viewController è la finestra di dialogo SSO di Apple e deve essere chiuso dal controller della visualizzazione principale.

![](assets/iOS-flows.png)

### B. Flusso di avvio {#startup_flow}

1. Avvia l&#39;applicazione di livello superiore.</br>
1. Avvia autenticazione Adobe Pass </br>

   a. Chiamata [`init`](#$init) per creare una singola istanza di Adobe Pass Authentication AccessEnabler.
   * **Dipendenza:** Libreria iOS/tvOS nativa per l’autenticazione di Adobe Pass (AccessEnabler)

   b. Chiamata `setRequestor()` per stabilire l&#39;identità del programmatore; passare nel `requestorID` e (facoltativamente) un array di endpoint di autenticazione Adobe Pass. Per tvOS dovrai anche fornire la chiave pubblica e il segreto. Consulta [Documentazione senza client](#create_dev) per i dettagli.

   * **Dipendenza:** ID richiedente autenticazione Adobe Pass valido (rivolgiti al tuo Adobe Pass Authentication Account Manager per ottenere questo risultato).

   * **Trigger:**
     [setRequestorComplete()](#$setReqComplete) callback.

   >[!NOTE]
   >
   >Non è possibile completare alcuna richiesta di adesione finché non viene stabilita l&#39;identità completa del richiedente. Ciò significa che mentre [`setRequestor()`](#$setReq)  è ancora in esecuzione, tutte le richieste di adesione successive. Ad esempio: [`checkAuthentication()`](#checkAuthN) sono bloccati.

   Sono disponibili due opzioni di implementazione: una volta inviate le informazioni di identificazione del richiedente al server backend, il livello applicazione dell’interfaccia utente può scegliere uno dei due approcci seguenti: </br>

   1. Attendi l&#39;attivazione di [`setRequestorComplete()`](#setReqComplete) callback (parte del delegato AccessEnabler). Questa opzione offre la massima certezza che [`setRequestor()`](#$setReq) è stato completato, quindi è consigliato per la maggior parte delle implementazioni.

   1. Continua senza attendere l&#39;attivazione di [`setRequestorComplete()`](#setReqComplete) richiamata e inizia a emettere richieste di adesione. Queste chiamate (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) sono in coda dalla libreria AccessEnabler, che effettua le chiamate di rete effettive dopo [`setRequestor()`](#$setReq). Questa opzione può talvolta essere interrotta se, ad esempio, la connessione di rete è instabile.

1. Chiamata `checkAuthentication()` per verificare la presenza di un&#39;autenticazione esistente senza avviare il flusso di autenticazione completo.  Se la chiamata ha esito positivo, puoi passare direttamente al flusso di autorizzazione. In caso contrario, passare al flusso di autenticazione.

   * **Dipendenza:** Chiamata a riuscita [setRequestor()](#$setReq) (questa dipendenza si applica anche a tutte le chiamate successive).

   * **Trigger:** [setAuthenticationStatus()](#$setAuthNStatus) callback.


### C. Flusso di autenticazione senza Apple SSO {#authn_flow_wo_applesso}

1. Chiamata [`getAuthentication()`](#$getAuthN) per avviare il flusso di autenticazione o per ottenere la conferma che l’utente è già autenticato.

   **Trigger:**

   * Il [setAuthenticationStatus()](#$setAuthNStatus) callback, se l&#39;utente è già autenticato. In questo caso, passare direttamente al [Flusso di autorizzazione](#authz_flow).

   * Il [displayProviderDialog()](#$dispProvDialog) callback, se l&#39;utente non è ancora autenticato.

1. Presenta all’utente l’elenco dei provider inviati a
   [`displayProviderDialog()`](#dispProvDialog).

1. Dopo che l’utente ha selezionato un provider, ottiene l’URL del MVPD dell’utente da `navigateToUrl:` o `navigateToUrl:useSVC:` callback e apertura di una `UIWebView/WKWebView` o `SFSafariViewController` e indirizzarlo all&#39;URL.

1. Attraverso il `UIWebView/WKWebView` o `SFSafariViewController` istanziato nel passaggio precedente, l’utente arriva alla pagina di accesso di MVPD e immette le credenziali di accesso. Diverse operazioni di reindirizzamento si svolgono all’interno del controller.</br>

>[!NOTE]
>
>A questo punto, l’utente ha la possibilità di annullare il flusso di autenticazione. In questo caso, il livello dell’interfaccia utente è responsabile dell’informazione di AccessEnabler su questo evento chiamando [setSelectedProvider()](#setSelProv) con `null` come parametro. Ciò consente ad AccessEnabler di ripulire il proprio stato interno e reimpostare il flusso di autenticazione.

1. Dopo che l’utente ha effettuato l’accesso, il livello dell’applicazione rileva il caricamento di un URL personalizzato specifico. Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall’applicazione come un segnale che il flusso di autenticazione è stato completato e che è sicuro chiudere `UIWebView/WKWebView` o `SFSafariViewController` controller. Nel caso in cui `SFSafariViewController`è richiesto l&#39;utilizzo del controller. L&#39;URL personalizzato specifico è definito da **`application's custom scheme`** (ad es.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), altrimenti questo URL personalizzato specifico è definito da **`ADOBEPASS_REDIRECT_URL`** costante (ovvero `adobepass://ios.app`).

1. Chiudere il controller UIWebView/WKWebView o SFSafariViewController e chiamare AccessEnabler `handleExternalURL:url` Metodo API che indica ad AccessEnabler di recuperare il token di autenticazione dal server back-end.

1. (Facoltativo) Chiamata [`checkPreauthorizedResources(resources)`](#$checkPreauth) per verificare quali risorse l’utente è autorizzato a visualizzare. Il `resources` Il parametro è un array di risorse protette associate al token di autenticazione dell’utente. Un uso per le informazioni di autorizzazione ottenute dal MVPD dell’utente è quello di decorare l’interfaccia utente (ad esempio, simboli bloccati/sbloccati accanto al contenuto protetto).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) callback
   * **Punto di esecuzione:** Dopo il completamento del flusso di autenticazione

1. Se l’autenticazione ha esito positivo, procedi al flusso di autorizzazione.

### D. Flusso di autenticazione con Apple SSO su iOS {#authn_flow_with_applesso}

1. Chiamata [`getAuthentication()`](#$getAuthN) per avviare il flusso di autenticazione o per ottenere la conferma che l’utente è già autenticato.
   **Trigger:**

   * Il [presentTvProviderDialog()](#presentTvDialog) callback, se l&#39;utente non è autenticato e il richiedente corrente dispone almeno di un MVPD che supporta l&#39;SSO. Se nessun MVPD supporta l&#39;SSO, verrà utilizzato il flusso di autenticazione classico.

1. Dopo aver selezionato un provider, la libreria AccessEnabler otterrà un token di autenticazione con le informazioni fornite dal framework VSA di Apple.

1. Il [setAuthenticationsStatus()](#setAuthNStatus) verrà attivato il callback. A questo punto l’utente deve essere autenticato con l’SSO di Apple.

1. [Facoltativo] Chiamata [`checkPreauthorizedResources(resources)`](#$checkPreauth) per verificare quali risorse l’utente è autorizzato a visualizzare. Il `resources` Il parametro è un array di risorse protette associate al token di autenticazione dell’utente. Un uso per le informazioni di autorizzazione ottenute dal MVPD dell’utente è quello di decorare l’interfaccia utente (ad esempio, simboli bloccati/sbloccati accanto al contenuto protetto).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) callback
   * **Punto di esecuzione:** Dopo il completamento del flusso di autenticazione

1. Se l’autenticazione ha esito positivo, procedi al flusso di autorizzazione.

### E. Flusso di autenticazione con Apple SSO su tvOS {#authn_flow_with_applesso_tvOS}

1. Chiamata [`getAuthentication()`](#$getAuthN) per avviare il flusso di autenticazione o per ottenere la conferma che l’utente è già autenticato.
   **Trigger:**
   * Il [`presentTvProviderDialog()`](#presentTvDialog) callback, se l&#39;utente non è autenticato e il richiedente corrente dispone almeno di un MVPD che supporta l&#39;SSO. Se nessun MVPD supporta l&#39;SSO, verrà utilizzato il flusso di autenticazione classico.

1. Dopo che l’utente ha selezionato un provider, [`status()`](#status_callback_implementation) verrà chiamato il callback. Verrà fornito un codice di registrazione e la libreria AccessEnabler inizierà a eseguire il polling del server per un&#39;autenticazione a seconda schermata riuscita.

1. Se il codice di registrazione fornito è stato utilizzato per autenticare correttamente nella seconda schermata, il [`setAuthenticatiosStatus()`](#setAuthNStatus) verrà attivato il callback. A questo punto l’utente deve essere autenticato con l’SSO di Apple.
1. [Facoltativo] Chiamata [`checkPreauthorizedResources(resources)`](#$checkPreauth) per verificare quali risorse l’utente è autorizzato a visualizzare. Il `resources` Il parametro è un array di risorse protette associate al token di autenticazione dell’utente. Un uso per le informazioni di autorizzazione ottenute dal MVPD dell’utente è quello di decorare l’interfaccia utente (ad esempio, simboli bloccati/sbloccati accanto al contenuto protetto).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) callback

   * **Punto di esecuzione:** Dopo il completamento del flusso di autenticazione
1. Se l’autenticazione ha esito positivo, procedi al flusso di autorizzazione.

### F. Flusso di autorizzazione {#authz_flow}

1. Chiamata [getAuthorization()](#$getAuthZ) per avviare il flusso di autorizzazione.

   * **Dipendenza:** ResourceID validi concordati con gli MVPD.
   * Gli ID delle risorse devono essere gli stessi utilizzati su qualsiasi altro dispositivo o piattaforma e devono essere gli stessi in tutti gli MVPD. Per informazioni sugli ID risorsa, consulta [Identificazione delle risorse protette](/help/authentication/identify-protected-resources.md)

1. Convalida autenticazione e autorizzazione.

   * Se il [getAuthorization()](#$getAuthZ) chiamata riuscita: l’utente dispone di token AuthN e AuthZ validi (l’utente è autenticato e autorizzato a guardare il contenuto multimediale richiesto).

   * Se [getAuthorization()](#$getAuthZ) failed: esamina l’eccezione generata per determinarne il tipo (AuthN, AuthZ o altro):
      * In caso di errore di autenticazione (AuthN), riavvia il flusso di autenticazione.
      * Se si trattava di un errore di autorizzazione (AuthZ), l’utente non è autorizzato a guardare il contenuto multimediale richiesto e deve visualizzare all’utente un qualche tipo di messaggio di errore.
      * Se si è verificato un altro tipo di errore (errore di connessione, errore di rete, ecc.) quindi visualizza un messaggio di errore appropriato.

1. Convalida il token multimediale breve.\
   Utilizza la libreria Adobe Pass Authentication Media Token Verifier per verificare il token multimediale di breve durata restituito da [getAuthorization()](#$getAuthZ) richiamare:

   * Se la convalida ha esito positivo: riproduci il file multimediale richiesto per l’utente.
   * Se la convalida non riesce: il token AuthZ non è valido, la richiesta di supporto deve essere rifiutata e l’utente deve visualizzare un messaggio di errore.


1. Tornare al normale flusso dell&#39;applicazione.

### G. Visualizza flusso multimediale {#media_flow}

1. L’utente seleziona il file multimediale da visualizzare.
1. Il supporto è protetto? L&#39;applicazione verifica se il supporto selezionato è protetto:

   * Se il supporto selezionato è protetto, l&#39;applicazione avvia [Flusso di autorizzazione](#authz_flow) sopra.

   * Se il supporto selezionato non è protetto, riprodurlo per l&#39;utente.

### H. Flusso di disconnessione senza Apple SSO {#logout_flow_wo_AppleSSO}

1. Chiamata [`logout()`](#$logout) per disconnettere l&#39;utente. AccessEnabler cancella tutti i valori e i token memorizzati nella cache. Dopo aver cancellato la cache, AccessEnabler effettua una chiamata al server per pulire le sessioni lato server. Poiché la chiamata al server potrebbe causare un reindirizzamento SAML all’IdP (consentendo la pulizia della sessione sul lato IdP), questa chiamata deve seguire tutti i reindirizzamenti. Per questo motivo, questa chiamata deve essere gestita all&#39;interno di un controller UIWebView/WKWebView o SFSafariViewController.

   a. Seguendo lo stesso modello del flusso di lavoro di autenticazione, il dominio AccessEnabler invia una richiesta al livello dell’applicazione dell’interfaccia utente tramite `navigateToUrl:` o `navigateToUrl:useSVC:` callback, per creare un controller UIWebView/WKWebView o SFSafariViewController e istruire tale utente a caricare l&#39;URL fornito nel `url` parametro. Questo è l&#39;URL dell&#39;endpoint di disconnessione sul server back-end.

   b. L&#39;applicazione deve monitorare l&#39;attività del `UIWebView/WKWebView or SFSafariViewController` e rilevare il momento in cui carica un URL personalizzato specifico, mentre passa attraverso diversi reindirizzamenti. Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall&#39;applicazione come un segnale del completamento del flusso di logout e della sicurezza della chiusura di `UIWebView/WKWebView` o `SFSafariViewController` controller. Quando il controller carica questo URL personalizzato specifico, l&#39;applicazione deve chiudere `UIWebView/WKWebView or SFSafariViewController` e chiama AccessEnabler `handleExternalURL:url`metodo API. Nel caso in cui `SFSafariViewController`è richiesto l&#39;utilizzo del controller. L&#39;URL personalizzato specifico è definito da **`application's custom scheme`** (ad esempio, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), altrimenti questo URL personalizzato specifico è definito da **`ADOBEPASS_REDIRECT_URL`**  costante (ovvero `adobepass://ios.app`).

   >[!NOTE]
   >
   >Il flusso di disconnessione è diverso dal flusso di autenticazione in quanto l&#39;utente non è tenuto a interagire in alcun modo con UIWebView/WKWebView o SFSafariViewController. Il livello dell’applicazione UI utilizza un UIWebView/WKWebView o SFSafariViewController per assicurarsi che tutti i reindirizzamenti siano seguiti. Pertanto, è possibile (e consigliato) rendere il controller invisibile (cioè nascosto) durante il processo di logout.


### I. Flusso di disconnessione con Apple SSO {#logout_flow_with_AppleSSO}

1. Chiamata [`logout()`](#$logout) per disconnettere l&#39;utente.
1. Il [status()](#status_callback_implementation) il callback verrà chiamato con id VSA203.
1. L’utente deve essere informato di effettuare l’accesso anche dalle impostazioni di sistema. In caso contrario, si verificherà una nuova autenticazione al riavvio dell&#39;applicazione.



<!--
### Related Information {#related}


- [iOS API Reference](#)

- [iOS Technical Overview](#)

- [Generating Digital Certificates](#)

- [Identifying Protected Resources](#)

- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)

- [iOS Authentication error - adobepass.ios.app cannot be found (Tech
  Note)](#)
-->
