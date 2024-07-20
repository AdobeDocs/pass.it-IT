---
title: Manuale di iOS/tvOS
description: Manuale di iOS/tvOS
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '2402'
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

La configurazione del valore [ID Experience Cloud](https://experienceleague.adobe.com/docs/id-service/using/home.html) è importante dal punto di vista di [!DNL Analytics]. Una volta impostato il valore `visitorID`, l&#39;SDK invia queste informazioni insieme a ogni chiamata di rete e il server di autenticazione [!DNL Adobe Pass] le raccoglie. Puoi correlare le analisi del servizio di autenticazione di Adobe Pass con qualsiasi altro rapporto di analisi disponibile in altre applicazioni o siti web. Le informazioni su come configurare visitorID sono disponibili [qui](#setOptions).

## Flussi di diritti {#entitlement}

A. [Prerequisiti](#prereqs) </br>
B. [Flusso di avvio](#startup_flow) </br>
C. [Flusso di autenticazione senza Apple SSO](#authn_flow_wo_applesso) </br>
D. [Flusso di autenticazione con SSO Apple su iOS](#authn_flow_with_applesso) </br>
E. [Flusso di autenticazione con SSO Apple su tvOS](#authn_flow_with_applesso_tvOS) </br>
F. [Flusso di autorizzazione](#authz_flow) </br>
G. [Visualizza flusso multimediale](#media_flow) </br>
H. [Flusso di disconnessione senza Apple SSO](#logout_flow_wo_AppleSSO) </br>
I. [Flusso di disconnessione con Apple SSO](#logout_flow_with_AppleSSO) </br>


### A. Prerequisiti {#prereqs}

1. Creare le funzioni di callback:
   * `setRequestorComplete()` </br>
   * Attivato da [setRequestor()](#$setReq), restituisce esito positivo o negativo. </br>
   * Il successo indica che puoi procedere con le chiamate di adesione.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Attivato da [`getAuthentication()`](#$getAuthN) solo se l&#39;utente non ha selezionato un provider (MVPD) e non è ancora autenticato. </br>
      * Il parametro `mvpds` è un array di provider disponibili per l&#39;utente.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Attivato da `checkAuthentication()` ogni volta. </br>
      * Attivato da [`getAuthentication()`](#$getAuthN) solo se l&#39;utente è già autenticato e ha selezionato un provider. </br>
      * Lo stato restituito è success o failure, il codice di errore descrive il tipo di errore.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Attivazione eseguita da [`getAuthentication()`](#$getAuthN) dopo la selezione di un MVPD da parte dell&#39;utente. Il parametro `url` fornisce il percorso della pagina di accesso di MVPD.

   * `sendTrackingData(event, data)` </br>
      * Attivato da `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`.
      * Il parametro `event` indica quale evento di adesione si è verificato; il parametro `data` è un elenco di valori relativi all&#39;evento.

   * `setToken(token, resource)`

      * Attivazione da [checkAuthorization()](#checkAuthZ) e [getAuthorization()](#$getAuthZ) dopo un&#39;autorizzazione riuscita per visualizzare una risorsa.
      * Il parametro `token` è il token multimediale di breve durata. Il parametro `resource` è il contenuto che l&#39;utente è autorizzato a visualizzare.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Attivazione da [checkAuthorization()](#checkAuthZ) e [getAuthorization()](#$getAuthZ) dopo un&#39;autorizzazione non riuscita.
      * Il parametro `resource` è il contenuto che l&#39;utente stava tentando di visualizzare. Il parametro `code` è il codice di errore che indica il tipo di errore che si è verificato. Il parametro `description` descrive l&#39;errore associato al codice di errore.

   * `selectedProvider(mvpd)` </br>
      * Attivato da [`getSelectedProvider()`](#getSelProv).
      * Il parametro `mvpd` fornisce informazioni sul provider selezionato dall&#39;utente.

   * `setMetadataStatus(metadata, key, arguments)`
      * Attivato da `getMetadata().`
      * Il parametro `metadata` fornisce i dati specifici richiesti; il parametro `key` è la chiave utilizzata nella richiesta [getMetadata()](#getMeta) e il parametro `arguments` è lo stesso dizionario passato a [getMetadata()](#getMeta).

   * [&quot;preauthorizedResources(authorizedResources)&quot;](#preauthResources)

      * Attivato da [`checkPreauthorizedResources()`](#checkPreauth).

      * Il parametro `authorizedResources` presenta le risorse che l&#39;utente
è autorizzato a visualizzare.

   * [&quot;presentTvProviderDialog(viewController)&quot;](#presentTvDialog)

      * Attivazione eseguita da [getAuthentication()](#getAuthN) quando il richiedente corrente supporta almeno MVPD con supporto SSO.
      * Il parametro viewController è la finestra di dialogo SSO di Apple e deve essere presentato sul controller della visualizzazione principale.

   * [&quot;dismissTvProviderDialog(viewController)&quot;](#dismissTvDialog)

      * Attivato da un’azione dell’utente (selezionando &quot;Annulla&quot; o &quot;Altri provider TV&quot; dalla finestra di dialogo SSO di Apple).
      * Il parametro viewController è la finestra di dialogo SSO di Apple e deve essere chiuso dal controller della visualizzazione principale.

![](assets/iOS-flows.png)

### B. Flusso di avvio {#startup_flow}

1. Avviare l&#39;applicazione di livello superiore.</br>
1. Avvia autenticazione Adobe Pass </br>

   a. Chiamare [`init`](#$init) per creare una singola istanza di Adobe Pass Authentication AccessEnabler.
   * **Dipendenza:** Adobe Pass Authentication Native iOS/tvOS Library (AccessEnabler)

   b. Chiamare `setRequestor()` per stabilire l&#39;identità del programmatore; passare `requestorID` del programmatore e (facoltativamente) un array di endpoint di autenticazione Adobe Pass. Per tvOS dovrai anche fornire la chiave pubblica e il segreto. Per informazioni dettagliate, consulta la [documentazione senza client](#create_dev).

   * **Dipendenza:** ID richiedente autenticazione Adobe Pass valido (utilizzare l&#39;account di autenticazione Adobe Pass)
per il manager).

   * **Trigger:**
     [setRequestorComplete()](#$setReqComplete) callback.

   >[!NOTE]
   >
   >Non è possibile completare alcuna richiesta di adesione finché non viene stabilita l&#39;identità completa del richiedente. Ciò significa che mentre [`setRequestor()`](#$setReq) è ancora in esecuzione, tutte le richieste di adesione successive. [`checkAuthentication()`](#checkAuthN) ad esempio sono bloccati.

   Sono disponibili due opzioni di implementazione: una volta inviate le informazioni di identificazione del richiedente al server back-end, il livello applicazione dell&#39;interfaccia utente può scegliere uno dei due approcci seguenti: </br>

   1. Attendere l&#39;attivazione del callback [`setRequestorComplete()`](#setReqComplete) (parte del delegato AccessEnabler). Questa opzione fornisce la massima certezza che [`setRequestor()`](#$setReq) è stato completato, quindi è consigliata per la maggior parte delle implementazioni.

   1. Continuare senza attendere l&#39;attivazione del callback [`setRequestorComplete()`](#setReqComplete) e iniziare a emettere richieste di adesione. Queste chiamate (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) sono in coda dalla libreria AccessEnabler, che eseguirà le chiamate di rete effettive dopo [`setRequestor()`](#$setReq). Questa opzione può talvolta essere interrotta se, ad esempio, la connessione di rete è instabile.

1. Chiamare `checkAuthentication()` per verificare la presenza di un&#39;autenticazione esistente senza avviare il flusso di autenticazione completo.  Se la chiamata ha esito positivo, puoi passare direttamente al flusso di autorizzazione. In caso contrario, passare al flusso di autenticazione.

   * **Dipendenza:** Chiamata riuscita a [setRequestor()](#$setReq) (questa dipendenza si applica anche a tutte le chiamate successive).

   * **Trigger:** [setAuthenticationStatus()](#$setAuthNStatus) callback.


### C. Flusso di autenticazione senza Apple SSO {#authn_flow_wo_applesso}

1. Chiamare [`getAuthentication()`](#$getAuthN) per avviare il flusso di autenticazione o per ottenere la conferma che l&#39;utente è già
autenticato.

   **Trigger:**

   * Il callback [setAuthenticationStatus()](#$setAuthNStatus), se l&#39;utente è già autenticato. In questo caso, passare direttamente al [Flusso di autorizzazione](#authz_flow).

   * Il callback [displayProviderDialog()](#$dispProvDialog), se l&#39;utente non è ancora autenticato.

1. Presenta all’utente l’elenco dei provider inviati a
   [`displayProviderDialog()`](#dispProvDialog).

1. Dopo che l&#39;utente ha selezionato un provider, ottenere l&#39;URL del MVPD dell&#39;utente dal callback `navigateToUrl:` o `navigateToUrl:useSVC:` e aprire un controller `UIWebView/WKWebView` o `SFSafariViewController` e indirizzare tale controller all&#39;URL.

1. Tramite l&#39;istanza di `UIWebView/WKWebView` o `SFSafariViewController` creata nel passaggio precedente, l&#39;utente arriva alla pagina di accesso di MVPD e immette le credenziali di accesso. Diverse operazioni di reindirizzamento si verificano all&#39;interno del controller.</br>

>[!NOTE]
>
>A questo punto, l’utente ha la possibilità di annullare il flusso di autenticazione. In questo caso, il livello dell&#39;interfaccia utente è responsabile dell&#39;informazione di AccessEnabler su questo evento chiamando [setSelectedProvider()](#setSelProv) con `null` come parametro. Ciò consente ad AccessEnabler di ripulire il proprio stato interno e reimpostare il flusso di autenticazione.

1. Dopo che l’utente ha effettuato l’accesso, il livello dell’applicazione rileva il caricamento di un URL personalizzato specifico. Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall&#39;applicazione come un segnale del completamento del flusso di autenticazione e della sicurezza della chiusura del controller `UIWebView/WKWebView` o `SFSafariViewController`. Nel caso in cui sia necessario utilizzare un controller `SFSafariViewController`, l&#39;URL personalizzato specifico è definito da **`application's custom scheme`** (ad esempio `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), altrimenti questo URL personalizzato specifico è definito dalla costante **`ADOBEPASS_REDIRECT_URL`** (ovvero `adobepass://ios.app`).

1. Chiudere il controller UIWebView/WKWebView o SFSafariViewController e chiamare il metodo API `handleExternalURL:url` di AccessEnabler, che indica ad AccessEnabler di recuperare il token di autenticazione dal server back-end.

1. (Facoltativo) Chiamare [`checkPreauthorizedResources(resources)`](#$checkPreauth) per verificare quali risorse l&#39;utente è autorizzato a visualizzare. Il parametro `resources` è un array di risorse protette associate al token di autenticazione dell&#39;utente. Un uso per le informazioni di autorizzazione ottenute dal MVPD dell’utente è quello di decorare l’interfaccia utente (ad esempio, simboli bloccati/sbloccati accanto al contenuto protetto).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) callback
   * **Punto di esecuzione:** dopo il completamento del flusso di autenticazione

1. Se l’autenticazione ha esito positivo, procedi al flusso di autorizzazione.

### D. Flusso di autenticazione con Apple SSO su iOS {#authn_flow_with_applesso}

1. Chiamare [`getAuthentication()`](#$getAuthN) per avviare il flusso di autenticazione o per ottenere la conferma che l&#39;utente è già autenticato.
   **Trigger:**

   * Il callback [presentTvProviderDialog()](#presentTvDialog), se l&#39;utente non è autenticato e il richiedente corrente dispone almeno di un MVPD che supporta l&#39;SSO. Se nessun MVPD supporta l&#39;SSO, verrà utilizzato il flusso di autenticazione classico.

1. Dopo aver selezionato un provider, la libreria AccessEnabler otterrà un token di autenticazione con le informazioni fornite dal framework VSA di Apple.

1. Verrà attivato il callback [setAuthenticationsStatus()](#setAuthNStatus). A questo punto l’utente deve essere autenticato con l’SSO di Apple.

1. [Facoltativo] Chiamare [`checkPreauthorizedResources(resources)`](#$checkPreauth) per verificare quali risorse l&#39;utente è autorizzato a visualizzare. Il parametro `resources` è un array di risorse protette associate al token di autenticazione dell&#39;utente. Un uso per le informazioni di autorizzazione ottenute dal MVPD dell’utente è quello di decorare l’interfaccia utente (ad esempio, simboli bloccati/sbloccati accanto al contenuto protetto).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) callback
   * **Punto di esecuzione:** dopo il completamento del flusso di autenticazione

1. Se l’autenticazione ha esito positivo, procedi al flusso di autorizzazione.

### E. Flusso di autenticazione con Apple SSO su tvOS {#authn_flow_with_applesso_tvOS}

1. Chiama [`getAuthentication()`](#$getAuthN) per avviare
flusso di autenticazione o per ottenere la conferma che l’utente è già
autenticato.
   **Trigger:**
   * Il callback [`presentTvProviderDialog()`](#presentTvDialog), se l&#39;utente non è autenticato e il richiedente corrente dispone almeno di un MVPD che supporta l&#39;SSO. Se nessun MVPD supporta l&#39;SSO, verrà utilizzato il flusso di autenticazione classico.

1. Dopo che l&#39;utente ha selezionato un provider, verrà chiamato il callback [`status()`](#status_callback_implementation). Verrà fornito un codice di registrazione e la libreria AccessEnabler inizierà a eseguire il polling del server per un&#39;autenticazione a seconda schermata riuscita.

1. Se il codice di registrazione fornito è stato utilizzato per l&#39;autenticazione nella seconda schermata, verrà attivato il callback [`setAuthenticatiosStatus()`](#setAuthNStatus). A questo punto l’utente deve essere autenticato con l’SSO di Apple.
1. [Facoltativo] Chiamare [`checkPreauthorizedResources(resources)`](#$checkPreauth) per verificare quali risorse l&#39;utente è autorizzato a visualizzare. Il parametro `resources` è un array di risorse protette associate al token di autenticazione dell&#39;utente. Un uso per le informazioni di autorizzazione ottenute dal MVPD dell’utente è quello di decorare l’interfaccia utente (ad esempio, simboli bloccati/sbloccati accanto al contenuto protetto).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) callback

   * **Punto di esecuzione:** dopo il completamento del flusso di autenticazione
1. Se l’autenticazione ha esito positivo, procedi al flusso di autorizzazione.

### F. Flusso di autorizzazione {#authz_flow}

1. Chiamare [getAuthorization()](#$getAuthZ) per avviare il flusso di autorizzazione.

   * **Dipendenza:** ID risorsa validi concordati con gli MVPD.
   * Gli ID delle risorse devono essere gli stessi utilizzati su qualsiasi altro dispositivo o piattaforma e devono essere gli stessi in tutti gli MVPD. Per informazioni sugli ID delle risorse, vedere [Identificazione delle risorse protette](/help/authentication/identify-protected-resources.md)

1. Convalida autenticazione e autorizzazione.

   * Se la chiamata [getAuthorization()](#$getAuthZ) ha esito positivo: l&#39;utente dispone di token AuthN e AuthZ validi (l&#39;utente è autenticato e autorizzato a guardare il contenuto multimediale richiesto).

   * Se [getAuthorization()](#$getAuthZ) non riesce: esaminare l&#39;eccezione generata per determinarne il tipo (AuthN, AuthZ o altro):
      * In caso di errore di autenticazione (AuthN), riavvia il flusso di autenticazione.
      * Se si trattava di un errore di autorizzazione (AuthZ), l’utente non è autorizzato a guardare il contenuto multimediale richiesto e deve visualizzare all’utente un qualche tipo di messaggio di errore.
      * Se si è verificato un altro tipo di errore (errore di connessione, errore di rete, ecc.) quindi visualizza un messaggio di errore appropriato.

1. Convalida il token multimediale breve.\
   Utilizza la libreria Adobe Pass Authentication Media Token Verifier per verificare il token multimediale di breve durata restituito dalla chiamata [getAuthorization()](#$getAuthZ) precedente:

   * Se la convalida ha esito positivo: riproduci il file multimediale richiesto per l’utente.
   * Se la convalida non riesce: il token AuthZ non è valido, la richiesta di supporto deve essere rifiutata e l’utente deve visualizzare un messaggio di errore.


1. Tornare al normale flusso dell&#39;applicazione.

### G. Visualizza flusso multimediale {#media_flow}

1. L’utente seleziona il file multimediale da visualizzare.
1. Il supporto è protetto? L&#39;applicazione verifica se il supporto selezionato è protetto:

   * Se il supporto selezionato è protetto, l&#39;applicazione avvia il [flusso di autorizzazione](#authz_flow).

   * Se il supporto selezionato non è protetto, riprodurlo per
utente.

### H. Flusso di disconnessione senza Apple SSO {#logout_flow_wo_AppleSSO}

1. Chiamare [`logout()`](#$logout) per disconnettersi. AccessEnabler cancella tutti i valori e i token memorizzati nella cache. Dopo aver cancellato la cache, AccessEnabler effettua una chiamata al server per pulire le sessioni lato server. Poiché la chiamata al server potrebbe causare un reindirizzamento SAML all’IdP (consentendo la pulizia della sessione sul lato IdP), questa chiamata deve seguire tutti i reindirizzamenti. Per questo motivo, questa chiamata deve essere gestita all&#39;interno di un controller UIWebView/WKWebView o SFSafariViewController.

   a. Seguendo lo stesso modello del flusso di lavoro di autenticazione, il dominio AccessEnabler invia una richiesta al livello dell&#39;applicazione dell&#39;interfaccia utente, tramite il callback `navigateToUrl:` o `navigateToUrl:useSVC:`, per creare un controller UIWebView/WKWebView o SFSafariViewController e istruire tale utente a caricare l&#39;URL fornito nel parametro `url` del callback. Questo è l&#39;URL dell&#39;endpoint di disconnessione sul server back-end.

   b. L&#39;applicazione deve monitorare l&#39;attività del controller `UIWebView/WKWebView or SFSafariViewController` e rilevare il momento in cui viene caricato un URL personalizzato specifico, mentre viene eseguito diversi reindirizzamenti. Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall&#39;applicazione come un segnale del completamento del flusso di disconnessione e della sicurezza della chiusura del controller `UIWebView/WKWebView` o `SFSafariViewController`. Quando il controller carica questo URL personalizzato specifico, l&#39;applicazione deve chiudere il controller `UIWebView/WKWebView or SFSafariViewController` e chiamare il metodo API `handleExternalURL:url` di AccessEnabler. Nel caso in cui sia necessario utilizzare un controller `SFSafariViewController`, l&#39;URL personalizzato specifico è definito da **`application's custom scheme`** (ad esempio, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), altrimenti questo URL personalizzato specifico è definito dalla costante **`ADOBEPASS_REDIRECT_URL`** (ovvero `adobepass://ios.app`).

   >[!NOTE]
   >
   >Il flusso di disconnessione è diverso dal flusso di autenticazione in quanto l&#39;utente non è tenuto a interagire in alcun modo con UIWebView/WKWebView o SFSafariViewController. Il livello dell’applicazione UI utilizza un UIWebView/WKWebView o SFSafariViewController per assicurarsi che tutti i reindirizzamenti siano seguiti. Pertanto, è possibile (e consigliato) rendere il controller invisibile (cioè nascosto) durante il processo di logout.


### I. Flusso di disconnessione con Apple SSO {#logout_flow_with_AppleSSO}

1. Chiamare [`logout()`](#$logout) per disconnettersi.
1. Il callback [status()](#status_callback_implementation) verrà chiamato con ID VSA203.
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
