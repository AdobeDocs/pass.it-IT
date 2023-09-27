---
title: Riferimento API per iOS/tvOS
description: Riferimento API per iOS/tvOS
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '7010'
ht-degree: 0%

---

# Riferimento API per iOS/tvOS SDK {#iostvos-sdk-api-reference}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#intro}

Questa pagina descrive i metodi e le funzioni di callback esposti dal client nativo iOS/tvOS per l’autenticazione di Adobe Pass. I metodi e le funzioni di callback qui descritti sono definiti nel `AccessEnabler.h` e `EntitlementDelegate.h` file di intestazione; li puoi trovare nell’SDK di iOS AccessEnabler qui: `[SDK directory]/AccessEnabler/headers/api/`


Documentazione associata:

* Per una descrizione del flusso di adesioni all’autenticazione di base di Adobe Pass, consulta [Flusso diritto](/help/authentication/entitlement-flow.md).
* Per informazioni dettagliate su come implementare il flusso dei diritti di autenticazione di Adobe Pass utilizzando questa API, consulta [Manuale dell’integrazione di iOS](/help/authentication/iostvos-sdk-cookbook.md).
* Per la versione più recente dell’SDK di iOS AccessEnabler, vedi [Libreria di abilitazione di accesso nativa iOS](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>L’Adobe ti incoraggia a utilizzare solo l’autenticazione Adobe Pass *pubblico* API:
>
>* Le API pubbliche sono disponibili e completamente testate su tutti i tipi di client supportati. Per qualsiasi funzione pubblica, ci assicuriamo che ogni tipo di client abbia una versione corrispondente dei metodi associati.
>* Le API pubbliche devono essere il più stabili possibile, per supportare la compatibilità con le versioni precedenti e garantire che le integrazioni dei partner non si interrompano. Tuttavia, per le API non pubbliche, ci riserviamo il diritto di modificare la loro firma in qualsiasi momento futuro. Se incontri un flusso particolare che non può essere supportato tramite una combinazione delle chiamate API di autenticazione di Adobe Pass pubbliche correnti, l’approccio migliore è quello di farcelo sapere. Tenendo conto delle tue esigenze, possiamo modificare le API pubbliche e fornire una soluzione stabile.

</br>

## Riferimento API {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - Crea un&#39;istanza dell&#39;oggetto AccessEnabler.

* **[OBSOLETO]** [init](#init) - Crea un&#39;istanza dell&#39;oggetto AccessEnabler.

* [setOptions:options:](#setOptions) : configura le opzioni SDK globali come profile o visitorID.

* [setRequestor:](#setReqV3)[requestorID](#setReqV3),[setRequestor:requestorID:serviceProvider:](#setReqV3) - Stabilisce l&#39;identità del programmatore.

* **[OBSOLETO]** [setRequestor:signedRequestorId:](#setReq),[setRequestor:signedRequestorId:serviceProvider:](#setReq) - Stabilisce l&#39;identità del programmatore.

* **[OBSOLETO]** [setRequestor:signedRequestorId:segreto:publicKey](#setReq_tvos), [setRequestor:signedRequestorId:serviceProvider:secret:publicKey](#setReq_tvos)-Stabilisce l&#39;identità del programmatore.

* [setRequestorComplete:](#setReqComplete) - Informa l&#39;applicazione che la fase di configurazione è stata completata.

* [checkAuthentication](#checkAuthN) - Controlla lo stato di autenticazione dell&#39;utente corrente.

* [getAuthentication](#getAuthN), [getAuthentication:withData:](#getAuthN) - Avvia il flusso di lavoro di autenticazione completo.

* [getAuthentication:filtro](#getAuthN_filter),[getAuthentication:withData:](#getAuthN)[andFilter](#getAuthN_filter) - Avvia il flusso di lavoro di autenticazione completo.

* [displayProviderDialog:](#dispProvDialog) : informa l’applicazione per creare un’istanza degli elementi dell’interfaccia utente appropriati affinché l’utente possa selezionare un MVPD.

* [setSelectedProvider:](#setSelProv) - Informa l&#39;AccessEnabler della selezione MVPD dell&#39;utente.

* [navigateToUrl:](#nav2url) - Informa l&#39;applicazione che all&#39;utente deve essere presentata la pagina di accesso MVPD.

* [navigateToUrl:useSVC:](#nav2urlSVC) - Informa l&#39;applicazione che all&#39;utente deve essere presentata la pagina di accesso MVPD, utilizzando SFSafariViewController

* [handleExternalURL:url](#handleExternalURL) - Completa il flusso di autenticazione/disconnessione.

* **[OBSOLETO]** [getAuthenticationToken](#getAuthNToken) - Richiede il token di autenticazione al server back-end.

* [setAuthenticationStatus:errorCode:](#setAuthNStatus) - Informa l&#39;applicazione dello stato del flusso di autenticazione.

* [checkPreauthorizedResources:](#checkPreauth) - Determina se l’utente è già autorizzato a visualizzare risorse protette specifiche.

* [checkPreauthorizedResources:cache:](#checkPreauthCache) - Determina se l’utente è già autorizzato a visualizzare risorse protette specifiche.

* [risorse preautorizzate:](#preauthResources) : fornisce un elenco delle risorse che l’utente è già autorizzato a visualizzare.

* [checkAuthorization:](#checkAuthZ), [checkAuthorization:withData:](#checkAuthZ) - Controlla lo stato di autorizzazione dell&#39;utente corrente.

* [getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ) - Avvia il flusso di autorizzazione.

* [setToken:forResource:](#setToken) - Informa l&#39;applicazione che il flusso di autorizzazione è stato completato correttamente.

* [tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed) - Informa l&#39;applicazione che il flusso di autorizzazione non è riuscito.

* [logout](#logout) - Avvia il flusso di logout.

* [getSelectedProvider](#getSelProv) - Determina il provider attualmente selezionato.

* [selectedProvider:](#selProv) - Fornisce all&#39;applicazione informazioni sul MVPD attualmente selezionato.

* [getMetadata:](#getMeta) : recupera le informazioni esposte come metadati dalla libreria AccessEnabler.

* [presentTvProviderDialog:](#presentTvDialog) : indica all’applicazione di visualizzare la finestra di dialogo SSO di Apple.

* [dismissTvProviderDialog:](#dismissTvDialog) - Informa l&#39;applicazione di nascondere la finestra di dialogo SSO di Apple.

* [setMetadataStatus:encrypted:forKey:andArguments:](#setMetaStatus) - Consegna dei metadati richiesti da [getMetadata:](#getMeta) chiamare.

* [sendTrackingData:forEventType:](#sendTracking) - Fornisce informazioni sui dati di tracciamento.

* [MVPD](#mvpd) - Classe MVPD. [Contiene informazioni sull&#39;MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Crea istanze dell&#39;oggetto AccessEnabler. Deve essere presente una singola istanza di AccessEnabler per ogni istanza dell&#39;applicazione.

| **Chiamata API: costruttore iOS AccessEnabler** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Disponibilità:** v3.0+

**Parametri:**

* **softwareStatement:** Stringa che identifica l’applicazione nel sistema di Adobe. Controllare come ottenere un rendiconto software.

[Torna all&#39;inizio...](#apis)



### init - [OBSOLETO]{#init}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Crea istanze dell&#39;oggetto AccessEnabler. Deve essere presente una singola istanza di AccessEnabler per ogni istanza dell&#39;applicazione.

| Chiamata API: costruttore iOS AccessEnabler |
| --- |
| ```- (id) init;``` |

**Disponibilità:** v1.0+ **Fino a:** v3.0

**Parametri:** Nessuno

[Torna all&#39;inizio...](#apis)

</br>

### setOptions:opzioni {#setOptions}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Configura le opzioni SDK globali. Accetta un NSDictionary come argomento. I valori del dizionario vengono passati al server insieme a ogni chiamata di rete effettuata dall&#39;SDK.

**Nota:** I valori vengono passati al server indipendentemente dal flusso corrente (autenticazione/autorizzazione). Se desideri modificare i valori, puoi chiamare questo metodo in qualsiasi momento.

| Chiamata API: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Disponibilità:** v2.3.0+

**Parametri:**

* *opzioni*: NSDictionary contenente le opzioni SDK globali. Attualmente, sono disponibili le seguenti opzioni:
   * **applicationProfile** : può essere utilizzato per creare configurazioni server basate su questo valore.
   * **visitorID** : servizio ID Experience Cloud. Questo valore può essere utilizzato in seguito per i rapporti di analisi avanzata.
   * **handleSVC** - Valore booleano che indica se il programmatore gestirà i controller SFSafariView. Consulta [Supporto di SFSafariViewController sull&#39;SDK iOS 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) per ulteriori dettagli.
      * Se impostato su **false,** l’SDK presenta automaticamente all’utente finale un SFSafariViewController. L’SDK passerà ulteriormente all’URL della pagina di accesso MVPDs.
      * Se impostato su **vero,** l’SDK **NOT** presenta automaticamente all&#39;utente finale un oggetto SFSafariViewController. L’SDK attiverà ulteriormente **navigate(toUrl:{url}, useSVC:YES)**.
* **device\_info** - Informazioni per il cliente come descritto in [Trasmissione delle informazioni del client](/help/authentication/passing-client-information-device-connection-and-application.md).

[Torna all&#39;inizio...](#apis)


### setRequestor:requestorID, setRequestor:requestorID:serviceProvider: {#setReqV3}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Stabilisce l&#39;identità del programmatore. A ciascun programmatore viene assegnato un ID univoco al momento della registrazione all’Adobe per il sistema di autenticazione di Adobe Pass. Quando si tratta di token SSO e remoti, lo stato di autenticazione può cambiare quando l&#39;applicazione è in background, setRequestor può essere richiamato nuovamente quando l&#39;applicazione viene portata in primo piano per sincronizzarsi con lo stato del sistema (recuperare un token remoto se SSO è abilitato o eliminare il token locale se nel frattempo si è verificato un logout).

La risposta del server contiene un elenco di MVPD insieme ad alcune informazioni di configurazione collegate all&#39;identità del programmatore. La risposta del server viene utilizzata internamente dal codice AccessEnabler. Solo lo stato dell’operazione (ovvero OPERAZIONE RIUSCITA/NON RIUSCITA) viene presentato all’applicazione tramite `setRequestorComplete:` callback.

Se il `urls` non viene utilizzato, la chiamata di rete risultante è destinata all’URL predefinito del fornitore di servizi: l’ambiente Adobe RELEASE/production.


Se per il campo `urls` , la chiamata di rete risultante esegue il targeting di tutti gli URL forniti nel `urls` parametro. Tutte le richieste di configurazione vengono attivate contemporaneamente in thread separati. Il primo responder ha la precedenza quando si compila l’elenco degli MVPD. Per ogni MVPD nell&#39;elenco, AccessEnabler ricorda l&#39;URL del provider di servizi associato. Tutte le richieste di adesione successive vengono indirizzate all’URL associato al provider di servizi che è stato associato all’MVPD di destinazione durante la fase di configurazione.

| Chiamata API: configurazione richiedente |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Disponibilità:** v3.0+

| Chiamata API: configurazione richiedente |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Disponibilità:** v3.0+

**Parametri:**

* *requestorID*: ID univoco associato al programmatore. La prima volta che ti registri al servizio di autenticazione di Adobe Pass, passa l’ID univoco assegnato dall’Adobe al tuo sito.
* *url*: parametro facoltativo; per impostazione predefinita, viene utilizzato il provider di servizi Adobe (http://sp.auth.adobe.com/). Questo array consente di specificare gli endpoint per i servizi di autenticazione e autorizzazione forniti da Adobe (a scopo di debug possono essere utilizzate istanze diverse). È possibile utilizzare questa opzione per specificare più istanze del provider di servizi di autenticazione di Adobe Pass. In tal caso, l&#39;elenco MVPD è composto dagli endpoint di tutti i fornitori di servizi. Ogni MVPD è associato al provider di servizi più veloce, ovvero il provider che ha risposto per primo e che supporta tale MVPD.

>[!NOTE]
>
>Se chiamato senza `serviceProviders` , la libreria recupererà la configurazione dal provider di servizi predefinito (ovvero `https://sp.auth.adobe.com` per il profilo di produzione o `https://sp.auth-staging.adobe.com` per il profilo di staging). Se il `serviceProviders` fornito, deve essere un array di URL. Le informazioni di configurazione vengono recuperate da tutti gli endpoint specificati e unite. Se esistono informazioni duplicate in risposte diverse del provider di servizi, il conflitto viene risolto a favore del server con la risposta più rapida, ovvero il server con il tempo di risposta più breve ha la precedenza.

**Callback attivati:** [`setRequestorComplete:`](#setReqComplete)

[Torna all&#39;inizio...](#apis)

</br>

### setRequestor:setSignedRequestorId:, setRequestor:setSignedRequestorId:serviceProvider: - [OBSOLETO] {#setReq}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Stabilisce l&#39;identità del programmatore. A ciascun programmatore viene assegnato un ID univoco al momento della registrazione all’Adobe per il sistema di autenticazione di Adobe Pass. Quando si tratta di token SSO e remoti, lo stato di autenticazione può cambiare quando l&#39;applicazione è in background, setRequestor può essere richiamato nuovamente quando l&#39;applicazione viene portata in primo piano per sincronizzarsi con lo stato del sistema (recuperare un token remoto se SSO è abilitato o eliminare il token locale se nel frattempo si è verificato un logout).

La risposta del server contiene un elenco di MVPD insieme ad alcune informazioni di configurazione collegate all&#39;identità del programmatore. La risposta del server viene utilizzata internamente dal codice AccessEnabler. Solo lo stato dell’operazione (ovvero OPERAZIONE RIUSCITA/NON RIUSCITA) viene presentato all’applicazione tramite `setRequestorComplete:` callback.

Se il `urls` non viene utilizzato, la chiamata di rete risultante è destinata all’URL predefinito del fornitore di servizi: l’ambiente Adobe RELEASE/production.

Se per il campo `urls` , la chiamata di rete risultante esegue il targeting di tutti gli URL forniti nel `urls` parametro. Tutte le richieste di configurazione vengono attivate contemporaneamente in thread separati. Il primo responder ha la precedenza quando si compila l’elenco degli MVPD. Per ogni MVPD nell&#39;elenco, AccessEnabler ricorda l&#39;URL del provider di servizi associato. Tutte le richieste di adesione successive vengono indirizzate all’URL associato al provider di servizi che è stato associato all’MVPD di destinazione durante la fase di configurazione.

| Chiamata API: configurazione richiedente |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Disponibilità:** v1.0+ **Fino a:** v3.0

| Chiamata API: configurazione richiedente |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Disponibilità:** v1.0+ **Fino a:** v3.0

**Parametri:**

* *requestorID*: ID univoco associato al programmatore. Trasmetti l’ID univoco assegnato da Adobe al sito quando ti sei registrato per la prima volta con il servizio di autenticazione di Adobe Pass.
* *signedRequestorID*: **Questo parametro esiste in iOS AccessEnabler versioni 1.2 e successive.** Una copia dell&#39;ID richiedente con firma digitale della chiave privata. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: parametro facoltativo; per impostazione predefinita, viene utilizzato il provider di servizi Adobe (http://sp.auth.adobe.com/). Questo array consente di specificare gli endpoint per i servizi di autenticazione e autorizzazione forniti da Adobe (a scopo di debug possono essere utilizzate istanze diverse). È possibile utilizzare questa opzione per specificare più istanze del provider di servizi di autenticazione di Adobe Pass. In tal caso, l&#39;elenco MVPD è composto dagli endpoint di tutti i fornitori di servizi. Ogni MVPD è associato al provider di servizi più veloce, ovvero il provider che ha risposto per primo e che supporta tale MVPD.

**Note:** Se chiamato senza `serviceProviders` , la libreria recupererà la configurazione dal provider di servizi predefinito (ovvero`https://sp.auth.adobe.com` per il profilo di produzione o `https://sp.auth-staging.adobe.com` per il profilo di staging). Se il `serviceProviders` fornito, deve essere un array di URL. Le informazioni di configurazione vengono recuperate da tutti gli endpoint specificati e unite. Se esistono informazioni duplicate in risposte diverse del provider di servizi, il conflitto viene risolto a favore del server con la risposta più rapida (ovvero, il server con il tempo di risposta più breve ha la precedenza).

**Callback attivati:** [`setRequestorComplete:`](#setReqComplete)


[Torna all&#39;inizio...](#apis)

### setRequestor:setSignedRequestorId:segreto:publicKey, setRequestor:setSignedRequestorId:serviceProvider:secret:publicKey - [OBSOLETO] {#setReq_tvos}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Stabilisce l&#39;identità del programmatore. A ciascun programmatore viene assegnato un ID univoco al momento della registrazione all’Adobe per il sistema di autenticazione di Adobe Pass. Questa impostazione deve essere eseguita una sola volta durante il ciclo di vita dell&#39;applicazione.

La risposta del server contiene un elenco di MVPD insieme ad alcune informazioni di configurazione collegate all&#39;identità del programmatore. La risposta del server viene utilizzata internamente dal codice AccessEnabler. Solo lo stato dell’operazione (ovvero OPERAZIONE RIUSCITA/NON RIUSCITA) viene presentato all’applicazione tramite `setRequestorComplete:` callback.

Se il `urls` non viene utilizzato, la chiamata di rete risultante è destinata all’URL predefinito del fornitore di servizi: l’ambiente Adobe RELEASE/production.

Se per il campo `urls` , la chiamata di rete risultante esegue il targeting di tutti gli URL forniti nel `urls` parametro. Tutte le richieste di configurazione vengono attivate contemporaneamente in thread separati. Il primo responder ha la precedenza quando si compila l’elenco degli MVPD. Per ogni MVPD nell&#39;elenco, AccessEnabler ricorda l&#39;URL del provider di servizi associato. Tutte le richieste di adesione successive vengono indirizzate all’URL associato al provider di servizi che è stato associato all’MVPD di destinazione durante la fase di configurazione.



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: configurazione richiedente</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID
               secret:(NSString *)secret
            publicKey:(NSString *)publicKey;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilità:** v2.0+ **Fino a:** v3.0

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: configurazione richiedente</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID 
     serviceProviders:(NSArray *)urls</code></pre>
<p><code class="sourceCode objectivec">               secret:(NSString *)secret</code></p>
<p><code class="sourceCode objectivec">            publicKey:(NSString *)publicKey;</code></p></td>
</tr>
</tbody>
</table>

**Disponibilità:** v2.0+ **Fino a:** v3.0

**Parametri:**

* *requestorID*: ID univoco associato al programmatore. Trasmetti l’ID univoco assegnato da Adobe al sito quando ti sei registrato per la prima volta con il servizio di autenticazione di Adobe Pass.
* *signedRequestorID*: **Questo parametro esiste in iOS AccessEnabler versioni 1.2 e successive.** Una copia dell&#39;ID richiedente con firma digitale della chiave privata. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: parametro facoltativo; per impostazione predefinita, viene utilizzato il provider di servizi Adobe (http://sp.auth.adobe.com/). Questo array consente di specificare gli endpoint per i servizi di autenticazione e autorizzazione forniti da Adobe (a scopo di debug possono essere utilizzate istanze diverse). È possibile utilizzare questa opzione per specificare più istanze del provider di servizi di autenticazione di Adobe Pass. In tal caso, l&#39;elenco MVPD è composto dagli endpoint di tutti i fornitori di servizi. Ogni MVPD è associato al provider di servizi più veloce, ovvero il provider che ha risposto per primo e che supporta tale MVPD.
* secret and publicKey: il segreto e la chiave pubblica utilizzati per firmare le chiamate della seconda schermata. Per ulteriori informazioni, consulta [Documentazione senza client](#create_dev).

Se chiamato senza `serviceProviders` , la libreria recupererà la configurazione dal provider di servizi predefinito (ad esempio, `https://sp.auth.adobe.com` per il profilo di produzione o https://sp.auth-staging.adobe.com per il profilo di staging). Se il `serviceProviders` fornito, deve essere un array di URL. Le informazioni di configurazione vengono recuperate da tutti gli endpoint specificati e unite. Se esistono informazioni duplicate in risposte diverse del provider di servizi, il conflitto viene risolto a favore del server con la risposta più rapida (ovvero, il server con il tempo di risposta più breve ha la precedenza).

**Callback attivati:** [`setRequestorComplete:`](#setReqComplete)

[Torna all&#39;inizio...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** Callback attivato da AccessEnabler che informa l&#39;applicazione del completamento della fase di configurazione. Questo è un segnale che l’app può iniziare a emettere richieste di adesione. L’applicazione non può inviare richieste di adesione fino al completamento della fase di configurazione.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: configurazione richiedente completata</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilità:** v1.0+

**Parametri**:

* *stato*: può assumere uno dei seguenti valori:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - fase di configurazione completata
   * `ACCESS_ENABLER_STATUS_ERROR` - fase di configurazione non riuscita

**Attivato da:**
`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Torna all&#39;inizio...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Controlla lo stato di autenticazione dell&#39;utente corrente.
A tale scopo, cerca un token di autenticazione valido nello spazio di archiviazione dei token locale. La chiamata a questo metodo non esegue chiamate di rete. Viene utilizzata dall’applicazione per eseguire una query sullo stato di autenticazione dell’utente e aggiornare di conseguenza l’interfaccia utente (ad esempio, aggiornare l’interfaccia utente di accesso/disconnessione). Lo stato di autenticazione viene comunicato all’applicazione tramite [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) callback.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: controlla lo stato di autenticazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

**Parametri:** Nessuno

**Callback attivati:**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Torna all&#39;inizio...](#apis)

</br>

### getAuthentication, getAuthentication:withData: {#getAuthN}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Avvia il flusso di lavoro di autenticazione completo. Viene avviato controllando lo stato di autenticazione. Se non è già autenticato, viene avviato il computer dello stato del flusso di autenticazione:

* se l&#39;ultimo tentativo di autenticazione ha avuto esito positivo, la fase di selezione MVPD viene saltata e il [`navigateToUrl:`](#nav2url) callback attivato. L&#39;applicazione utilizza questo callback per creare un&#39;istanza del controllo WebView che presenta all&#39;utente la pagina di accesso di MVPD. **[NOTA: a partire da Access Enabler 1.5, questa funzionalità non è disponibile a causa di una limitazione nell’SDK].**
* se l’ultimo tentativo di autenticazione non ha avuto esito positivo o se l’utente si è esplicitamente disconnesso, il [`displayProviderDialog:`](#dispProvDialog) callback attivato. L&#39;applicazione utilizza questo callback per visualizzare l&#39;interfaccia utente di selezione MVPD. Inoltre, l’app deve riprendere il flusso di autenticazione informando la libreria AccessEnabler della selezione MVPD dell’utente tramite [`setSelectedProvider:`](#setSelProv) metodo.

Poiché le credenziali dell&#39;utente vengono verificate nella pagina di accesso MVPD, l&#39;applicazione deve monitorare le operazioni di reindirizzamento multiple che si verificano mentre l&#39;utente si autentica nella pagina di accesso di MVPD. Quando vengono immesse le credenziali corrette, il controllo WebView viene reindirizzato a un URL personalizzato definito da `ADOBEPASS_REDIRECT_URL` costante. Questo URL non deve essere caricato da WebView. L’applicazione deve intercettare questo URL e interpretare questo evento come un segnale del completamento della fase di accesso. Deve quindi passare il controllo ad AccessEnabler per completare il flusso di autenticazione (chiamando il [handleExternalURL](#handleExternalURL) metodo).

Infine, lo stato di autenticazione viene comunicato all’applicazione tramite [setAuthenticationStatus:errorCode:](#setAuthNStatus) callback.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: avvia il flusso di autenticazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: avvia il flusso di autenticazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Disponibilità:** v1.9+

**Parametri:**

* *forceAuthn*: flag che specifica se avviare il flusso di autenticazione, indipendentemente dal fatto che l’utente sia già autenticato o meno.
* *dati*: dizionario costituito da coppie chiave-valore da inviare al servizio di pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare l’SDK.

**Callback attivati:** ` setAuthenticationStatus:errorCode:, `[`displayProviderDialog:`](#dispProvDialog)`,`` sendTrackingData:forEventType:`


[Torna all&#39;inizio...](#apis)

</br>

### getAuthentication:filter, getAuthentication:withData:andFilter {#getAuthN_filter}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Avvia il flusso di lavoro di autenticazione completo. Viene avviato controllando lo stato di autenticazione. Se non è già autenticato, viene avviato il computer dello stato del flusso di autenticazione:

* [presentTvProviderDialog()](#presentTvDialog) verrà chiamato se il richiedente corrente dispone di almeno un MVPD che supporta l&#39;SSO. Se MVPD non supporta l&#39;SSO, verrà avviato il flusso di autenticazione classico e il parametro di filtro verrà ignorato.
* Dopo che l’utente ha completato il flusso SSO di Apple [dismissTvProviderDialog()](#dismissTvDialog) verrà attivato e il processo di autenticazione verrà completato.

Infine, lo stato di autenticazione viene comunicato all’applicazione tramite [setAuthenticationStatus:errorCode:](#setAuthNStatus) callback.

**Disponibilità:** v2.4+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: avvia il flusso di autenticazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSDictionary *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: avvia il flusso di autenticazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSDictionary *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Parametri:**

* *forceAuthn*: flag che specifica se avviare il flusso di autenticazione, indipendentemente dal fatto che l’utente sia già autenticato o meno.
* *dati*: dizionario costituito da coppie chiave-valore da inviare al servizio di pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare l’SDK.
* filter (filtro): dizionario con due elenchi di ID MVPD che devono essere visualizzati nella finestra di dialogo SSO di Apple. Qualsiasi MVPD che non supporta SSO verrà ignorato, ma l&#39;ordine verrà rispettato. Il dizionario deve avere due chiavi:
   * TV\_PROVIDERS: elenco con tutti i MVPD da visualizzare nel selettore
   * IN PRIMO PIANO\_TV\_PROVIDERS: Un elenco con tutti gli MVPD che devono essere contrassegnati come presenti nel selettore. Gli MVPD in questo elenco devono essere specificati anche nell&#39;elenco TV\_PROVIDERS.

**Disponibilità:** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: avvia il flusso di autenticazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSArray *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: avvia il flusso di autenticazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSArray *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Parametri:**

* *forceAuthn*: flag che specifica se avviare il flusso di autenticazione, indipendentemente dal fatto che l’utente sia già autenticato o meno.
* *dati*: dizionario costituito da coppie chiave-valore da inviare al servizio di pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare l’SDK.
* filter (Filtro): elenco di ID MVPD da visualizzare nella finestra di dialogo SSO di Apple. Qualsiasi MVPD che non supporta SSO verrà ignorato, ma l&#39;ordine verrà rispettato.

**Callback attivati:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Torna all&#39;inizio...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** Callback attivato da AccessEnabler per informare l&#39;applicazione che è necessario creare un&#39;istanza degli elementi dell&#39;interfaccia utente appropriati per consentire all&#39;utente di selezionare l&#39;MVPD desiderato. Il callback fornisce un elenco di oggetti MVPD con informazioni aggiuntive che possono aiutare a creare correttamente il pannello dell&#39;interfaccia utente di selezione (come l&#39;URL che punta al logo MVPD, il nome visualizzato intuitivo, ecc.)

Dopo che l’utente ha selezionato l’MVPD desiderato, l’applicazione di livello superiore deve riprendere il flusso di autenticazione chiamando `setSelectedProvider:` e trasmettendogli l’ID del MVPD corrispondente alla selezione dell’utente.

**Interruzione del flusso di autenticazione** - Questo è un punto in cui l’utente ha la possibilità di premere il pulsante &quot;Indietro&quot;, che equivale all’interruzione del flusso di autenticazione. In questo caso, l’applicazione deve chiamare [setSelectedProvider:](#setSelProv) , passando null come parametro, per consentire ad AccessEnabler di reimpostare il computer dello stato di autenticazione.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: visualizzare l'interfaccia utente di selezione MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilità:** v1.0+

**Parametri**:

* *mvpds*: elenco di oggetti MVPD contenenti informazioni relative a MVPD che l’applicazione può utilizzare per creare gli elementi dell’interfaccia utente di selezione MVPD.

**Attivato da:** ` getAuthentication, `[getAuthentication:withData:](#getAuthN),` getAuthorization:, `[getAuthorization:withData:](#getAuthZ)


[Torna all&#39;inizio...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene richiamato dall&#39;applicazione per informare l&#39;Access Enabler della selezione MVPD dell&#39;utente. È possibile utilizzare questo metodo per selezionare o modificare il provider di servizi utilizzato per l&#39;autenticazione.

Se l’MVPD selezionato è un MVPD TempPass, si autenticherà automaticamente con tale MVPD senza dover chiamare in seguito getAuthentication().

Tieni presente che questo non è possibile per il Passaggio temporaneo promozionale in cui vengono forniti parametri aggiuntivi sul metodo getAuthentication().

Quando si passa *nulle* come parametro, Access Enabler presuppone che l’utente abbia annullato il flusso di autenticazione (ovvero premuto il pulsante &quot;Indietro&quot;) e risponde reimpostando lo stato-computer di autenticazione e chiamando [setAuthenticationStatus:errorCode:](#setAuthNStatus) callback con `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` codice di errore.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: imposta il provider attualmente selezionato</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

**Parametri:** Nessuno

**Callback attivati:** ` setAuthenticationStatus:errorCode:,sendTrackingData:forEventType:,  `[`navigateToUrl:`](#nav2url)

[Torna all&#39;inizio...](#apis)

</br>

#### navigateToUrl: {#nav2url}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione:** Callback attivato da AccessEnabler per richiedere all&#39;applicazione di creare un&#39;istanza di un controller UIWebView/WKWebView e di caricare l&#39;URL fornito nel **`url`** parametro. Il callback passa il **`url`** Parametro che rappresenta l&#39;URL dell&#39;endpoint di autenticazione o l&#39;URL dell&#39;endpoint di logout.

Come UIWebView/WKWebView` `il controller passa attraverso diversi reindirizzamenti, l&#39;applicazione deve monitorare l&#39;attività del controller e rilevare il momento in cui carica un URL personalizzato specifico definito dal `ADOBEPASS_REDIRECT_URL `costante (ovvero `adobepass://ios.app`). Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall&#39;applicazione come un segnale che il flusso di autenticazione o disconnessione è stato completato e che è sicuro chiudere il controller. Quando il controller carica questo URL personalizzato specifico, l&#39;applicazione deve chiudere UIWebView/WKWebView e chiamare AccessEnabler `handleExternalURL:url `metodo API.

**Nota:** Tieni presente che nel caso del flusso di autenticazione si tratta di un punto in cui l’utente ha la possibilità di premere il pulsante &quot;Indietro&quot;, che equivale all’interruzione del flusso di autenticazione. In questo caso, l’applicazione deve chiamare [setSelectedProvider:](#setSelProv) metodo di passaggio **`nil`** come parametro e dando la possibilità ad AccessEnabler di reimpostare il computer dello stato di autenticazione.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: visualizzazione della pagina di accesso MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

**Parametri**:

* *url*: URL che punta alla pagina di accesso di MVPD

**Attivato da:** [setSelectedProvider:](#setSelProv)



[Torna all&#39;inizio...](#apis)

</br>

#### navigateToUrl:useSVC: {#nav2urlSVC}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione:** Callback attivato da AccessEnabler anziché da `navigateToUrl:` callback nel caso in cui l&#39;applicazione abbia precedentemente abilitato la gestione manuale di Safari View Controller (SVC) tramite [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) e solo in caso di MVPD che richiedono Safari View Controller (SVC). Per tutti gli altri MVPD `navigateToUrl:` verrà chiamato il callback. Consulta[Supporto di SFSafariViewController sull&#39;SDK iOS 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) per informazioni dettagliate sulla gestione di Safari View Controller (SVC).

Simile a `navigateToUrl:` callback di `navigateToUrl:useSVC:` viene attivato da AccessEnabler per richiedere all&#39;applicazione di creare un&#39;istanza di `SFSafariViewController` e per caricare l&#39;URL fornito nel file del callback **`url`** parametro. Il callback passa il **`url`** parametro che rappresenta l&#39;URL dell&#39;endpoint di autenticazione o l&#39;URL dell&#39;endpoint di logout e **`useSVC`** parametro che specifica che l&#39;applicazione deve utilizzare un `SFSafariViewController`.

Come `SFSafariViewController` il controller viene sottoposto a diversi reindirizzamenti; l&#39;applicazione deve monitorare l&#39;attività del controller e rilevare il momento in cui carica un URL personalizzato specifico definito dal `application's custom scheme` (ad es.** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall&#39;applicazione come un segnale che il flusso di autenticazione o disconnessione è stato completato e che è sicuro chiudere il controller. Quando il controller carica questo URL personalizzato specifico, l&#39;applicazione deve chiudere `SFSafariViewController` e chiama AccessEnabler `handleExternalURL:url `metodo API.

**Nota:** Tieni presente che nel caso del flusso di autenticazione si tratta di un punto in cui l’utente ha la possibilità di premere il pulsante &quot;Indietro&quot;, che equivale all’interruzione del flusso di autenticazione. In questo caso, l’applicazione deve chiamare [setSelectedProvider:](#setSelProv) metodo di passaggio **`nil`** come parametro e dando la possibilità ad AccessEnabler di reimpostare il computer dello stato di autenticazione.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: visualizzazione della pagina di accesso MVPD in SFSafariViewController</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:**v 3.2+

**Parametri**:

* *url:* URL che punta alla pagina di accesso di MVPD
* *useSVC:* se l’URL deve essere caricato in SFSafariViewController.

**Attivato da:**[ setOptions:](#setOptions) prima di [setSelectedProvider:](#setSelProv)

[Torna all&#39;inizio...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene richiamato dall&#39;applicazione per completare il flusso di autenticazione o disconnessione. Questo metodo deve essere chiamato subito dopo che l&#39;applicazione rileva il momento in cui `UIWebView/WKWebView or SFSafariViewController` il controller viene reindirizzato a un URL personalizzato specifico. Nel caso in cui l’applicazione debba utilizzare un `SFSafariViewController `controller l&#39;URL personalizzato specifico è definito dal `application's custom scheme` (ad es.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), altrimenti questo URL personalizzato specifico è definito da `ADOBEPASS_REDIRECT_URL `costante (ovvero `adobepass://ios.app`).

Nel caso del flusso di autenticazione, AccessEnabler completa il flusso recuperando il token di autenticazione dal server back-end e archiviandolo localmente nell&#39;archivio dei token. AccessEnabler informerà l&#39;applicazione che il flusso di autenticazione è completo chiamando `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> callback con codice di stato 1, che indica l&#39;esito positivo. Se si verifica un errore durante l&#39;esecuzione di questi passaggi, il `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> il callback viene attivato con un codice di stato pari a 0, che indica un errore di autenticazione, nonché un codice di errore corrispondente.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: completa il flusso di autenticazione o disconnessione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v3.0+

**Parametri:**

* *url*: l’URL intercettato da ` UIWebView/WKWebView or SFSafariViewController ` come stringa.


**Callback attivati:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Torna all&#39;inizio...](#apis)

</br>

#### getAuthenticationToken - [OBSOLETO] {#getAuthNToken}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Completa il flusso di autenticazione richiedendo il token di autenticazione dal server back-end. Questo metodo deve essere chiamato dall&#39;applicazione solo in risposta all&#39;evento in cui il controllo WebView che ospita la pagina di accesso MVPD viene reindirizzato all&#39;URL personalizzato definito da `ADOBEPASS_REDIRECT_URL` costante.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: recupera il token di autenticazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+ **Fino a:** v3.0

**Parametri:** Nessuno

**Callback attivati:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Torna all&#39;inizio...](#apis)

&lt;/br

#### setAuthenticationStatus:errorCode: {#setAuthNStatus}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** Callback attivato da AccessEnabler che informa l&#39;applicazione dello stato del flusso di autenticazione. In molti casi questo flusso può non riuscire, a causa dell’interazione dell’utente o di altri scenari imprevisti (ad esempio, problemi di connettività di rete, ecc.). Questo callback informa l’applicazione dello stato di esito positivo/negativo del flusso di autenticazione, fornendo al contempo informazioni aggiuntive sul motivo dell’errore, quando necessario.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: segnala lo stato del flusso di autenticazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setAuthenticationStatus:(int)status 
                       errorCode:(NSString *)code; </code></pre></td>
</tr>
</tbody>
</table>


**Disponibilità:** v1.0+

**Parametri**:

* *stato*: può assumere uno dei seguenti valori:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - flusso di autenticazione completato
   * `ACCESS_ENABLER_STATUS_ERROR` - flusso di autenticazione non riuscito
* *codice*: motivo dell’errore. Se *stato* è `ACCESS_ENABLER_STATUS_SUCCESS`, quindi *codice* è una stringa vuota (definita da `USER_AUTHENTICATED` costante). In caso di errore, questo parametro può assumere uno dei seguenti valori:
   * `USER_NOT_AUTHENTICATED_ERROR` : utente non autenticato. In risposta al [checkAuthentication:](#checkAuthN) chiamata al metodo quando non è presente alcun token di autenticazione valido nella cache dei token locale.
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler ha reimpostato il computer dello stato di autenticazione dopo il passaggio dell&#39;applicazione di livello superiore *nulle* a [`setSelectedProvider:`](#setSelProv) per interrompere il flusso di autenticazione.  Presumibilmente l’utente ha annullato il flusso di autenticazione (ovvero ha premuto il pulsante &quot;Indietro&quot;).
   * `GENERIC_AUTHENTICATION_ERROR` - Flusso di autenticazione non riuscito per motivi quali la non disponibilità della rete o l&#39;annullamento esplicito del flusso di autenticazione da parte dell&#39;utente.

**Attivato da:** ` checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN),` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ)

[Torna all&#39;inizio...](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene utilizzato dall’applicazione per determinare se l’utente è già autorizzato a visualizzare risorse protette specifiche. Lo scopo principale di questo metodo è quello di recuperare informazioni da utilizzare per decorare l’interfaccia utente **(ad esempio, per indicare lo stato di accesso con le icone di blocco e sblocco).**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: imposta il provider attualmente selezionato</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.3+

**Parametri:**

* *risorse:* array di risorse per le quali deve essere controllata l’autorizzazione. Ogni elemento dell’elenco deve essere una stringa che rappresenta l’ID della risorsa. L’ID della risorsa è soggetto alle stesse limitazioni dell’ID della risorsa nella chiamata, ovvero deve essere un valore concordato tra il Programmatore e l’MVPD o un frammento RSS del contenuto multimediale.

**Callback attivato:** [`preauthorizedResources:`](#preauthResources)

[Torna all&#39;inizio...](#apis)

</br>

### checkPreauthorizedResources:cache: {#checkPreauthCache}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene utilizzato dall’applicazione per determinare se l’utente è già autorizzato a visualizzare risorse protette specifiche. Lo scopo principale di questo metodo è quello di recuperare informazioni da utilizzare per decorare l’interfaccia utente (ad esempio, per indicare lo stato di accesso con le icone di blocco e sblocco). Il **cache** Il parametro controlla se la cache interna viene utilizzata per la risoluzione delle risorse.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: imposta il provider attualmente selezionato</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**Disponibilità:** v3.1+



**Parametri:**

* *risorse:* array di risorse per le quali deve essere controllata l’autorizzazione. Ogni elemento dell’elenco deve essere una stringa che rappresenta l’ID della risorsa. L’ID risorsa è soggetto alle stesse limitazioni dell’ID risorsa in `getAuthorization:` deve essere un valore concordato tra il programmatore e l’MVPD o un frammento RSS del contenuto multimediale.
* *cache:* Valore booleano che specifica se utilizzare la cache interna per la risoluzione delle risorse. Se false, la cache verrà bypassata, dando luogo a chiamate al server ogni volta che questa API viene chiamata.

**Callback attivato:** [`preauthorizedResources:`](#preauthResources)

[Torna all&#39;inizio...](#apis)

</br>

### risorse preautorizzate: {#preauthResources}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione:** Callback attivato da `checkPreauthorizedResources:`. Fornisce un elenco delle risorse che l’utente è già autorizzato a visualizzare.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: imposta il provider attualmente selezionato</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.3+

**Parametri:**

* `resources`: array di risorse per le quali l’utente è già autorizzato a visualizzare.

**Attivato da:** [`checkPreauthorizedResources:`](#checkPreauth)



[Torna all&#39;inizio...](#apis)

</br>

### checkAuthorization:, checkAuthorization:withData: {#checkAuthZ}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene utilizzato dall&#39;applicazione per controllare lo stato di autorizzazione. Viene innanzitutto verificato lo stato di autenticazione. Se non autenticato, il [tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed) viene attivato il callback e il metodo viene chiuso. Se l’utente è autenticato, attiva anche il flusso di autorizzazione. Vedi i dettagli su [`getAuthorization:`](#getAuthZ) metodo.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: controlla lo stato di autorizzazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: controlla lo stato di autorizzazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource:
                   withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.9+

**Parametri:**

* *resource*: ID della risorsa per la quale l’utente richiede l’autorizzazione.
* *dati*: dizionario costituito da coppie chiave-valore da inviare al servizio di pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare l’SDK.

**Callback attivati:**
[tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed)`,setToken:forResource:, sendTrackingData:forEventType:, setAuthenticationStatus:errorCode:`

[Torna all&#39;inizio...](#apis)

</br>

### getAuthorization:, getAuthorization:withData: {#getAuthZ}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene utilizzato dall&#39;applicazione per avviare il flusso di autorizzazione. Se l’utente non è già autenticato, avvia anche il flusso di autenticazione. Se l&#39;utente viene autenticato, AccessEnabler procede con il rilascio di richieste per il token di autorizzazione (se non è presente alcun token di autorizzazione valido nella cache dei token locale) e per il token multimediale di breve durata. Una volta ottenuto il token multimediale breve, il flusso di autorizzazione viene considerato completo. Il [setToken:forResource:](#setToken) il callback viene attivato e il token multimediale breve viene distribuito come parametro all’applicazione. Se, per qualsiasi motivo, l&#39;autorizzazione non riesce, il [tokenRequestFailed:forEventType:](#tokenReqFailed) viene attivato il callback e vengono forniti il codice di errore o i dettagli.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: avvia il flusso di autorizzazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: avvia il flusso di autorizzazione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource:
                 withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>



**Disponibilità:** v1.9+

**Parametri:**

* *resource*: ID della risorsa per la quale l’utente richiede l’autorizzazione.
* *dati*: dizionario costituito da coppie chiave-valore da inviare al servizio di pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare l’SDK.

**Callback attivati:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Callback aggiuntivi attivati:**\
Questo metodo può anche attivare i seguenti callback (se viene avviato anche il flusso di autenticazione): `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

**NOTA: utilizzare checkAuthorization: / checkAuthorization:withData: invece di getAuthorization: / getAuthorization:withData: quando possibile. Il metodo getAuthorization: / getAuthorization:withData: Il metodo avvierà un flusso di autenticazione completo (se l’utente non è autenticato) e questo potrebbe comportare una complicata implementazione da parte del programmatore.**

[Torna all&#39;inizio...](#apis)

</br>

### setToken:forResource: {#setToken}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** Callback attivato da AccessEnabler che informa l&#39;applicazione che il flusso di autorizzazione è stato completato correttamente. Anche il token multimediale di breve durata viene distribuito come parametro.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: flusso di autorizzazione completato</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setToken:(NSString *)token 
      forResource:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

**Parametri**:

* *token*: token multimediale di breve durata
* *resource*: la risorsa per la quale è stata ottenuta l’autorizzazione

**Attivato da:** [checkAuthorization:](#checkAuthZ)` , `[checkAuthorization:withData:](#checkAuthZ),` `[getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ)

[Torna all&#39;inizio...](#apis)

</br>

### tokenRequestFailed:errorCode:errorDescription: {#tokenReqFailed}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** Callback attivato da AccessEnabler che informa l&#39;applicazione di livello superiore che il flusso di autorizzazione non è riuscito.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: flusso di autorizzazione non riuscito</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) tokenRequestFailed:(NSString *)resource 
                  errorCode:(NSString *)code 
           errorDescription:(NSString *)description; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

**Parametri**:

* *resource*: risorsa per la quale è stata ottenuta l’autorizzazione.
* *codice*: codice di errore associato allo scenario di errore. Valori possibili:
   * `USER_NOT_AUTHORIZED_ERROR` - l&#39;utente non è stato in grado di autorizzare per la risorsa specificata
* *descrizione*: ulteriori dettagli sullo scenario di errore. Se questa stringa descrittiva non è disponibile per alcun motivo, Adobe Pass Authentication invia una stringa vuota **(&quot;&quot;)**.\
  Questa stringa può essere utilizzata da un MVPD per trasmettere messaggi di errore personalizzati o messaggi relativi alle vendite. Ad esempio, se a un abbonato viene negata l’autorizzazione per una risorsa, l’MVPD potrebbe inviare un messaggio del tipo: &quot;Attualmente non hai accesso a questo canale nel pacchetto. Se desideri aggiornare il pacchetto, fai clic su **qui**.&quot; Il messaggio viene passato dall’autenticazione di Adobe Pass tramite questo callback al programmatore, che ha la possibilità di visualizzarlo o ignorarlo. L’autenticazione di Adobe Pass può inoltre utilizzare questo parametro per fornire una notifica della condizione che potrebbe aver causato un errore. Ad esempio, &quot;Si è verificato un errore di rete durante la comunicazione con il servizio di autorizzazione del provider&quot;.

**Attivato da:** ` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ)

[Torna all&#39;inizio...](#apis)

</br>

### logout {#logout}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene richiamato dall&#39;applicazione per avviare il flusso di logout. La disconnessione è il risultato di una serie di operazioni di reindirizzamento HTTP dovute al fatto che l’utente deve essere disconnesso sia dai server di autenticazione di Adobe Pass che dai server MVPD. Poiché questo flusso non può essere completato con una semplice richiesta HTTP emessa dalla libreria AccessEnabler, è possibile che `UIWebView/WKWebView or SFSafariViewController` è necessario creare un&#39;istanza del controller per poter seguire le operazioni di reindirizzamento HTTP.

Il flusso di logout è diverso dal flusso di autenticazione in quanto l’utente non è tenuto a interagire con `UIWebView/WKWebView or SFSafariViewController`  in qualsiasi modo. Pertanto, Adobe consiglia di rendere il controllo invisibile (ovvero nascosto) durante la procedura di disconnessione.

Viene utilizzato un modello simile al flusso di autenticazione. IOS AccessEnabler attiva `navigateToUrl:` callback o `navigateToUrl:useSVC:` per creare un `UIWebView/WKWebView or SFSafariViewController` e per caricare l&#39;URL fornito nel file del callback `url` parametro. Questo è l&#39;URL dell&#39;endpoint di disconnessione sul server back-end. Per tvOS AccessEnabler né il `navigateToUrl:` callback o `navigateToUrl:useSVC:` chiamata di callback.

Durante il trattamento di diversi reindirizzamenti, l’applicazione deve monitorare l’attività del `UIWebView/WKWebView or SFSafariViewController `e rilevare il momento in cui carica un URL personalizzato specifico. Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall&#39;applicazione come un segnale che il flusso di logout è stato completato e che è sicuro chiudere il controller. Quando il controller carica questo URL personalizzato specifico, l&#39;applicazione deve chiudere il controller e chiamare AccessEnabler `handleExternalURL:url `metodo API. Nel caso in cui l’applicazione debba utilizzare un `SFSafariViewController `controller l&#39;URL personalizzato specifico è definito dal `application's custom scheme` (ad es.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), altrimenti questo URL personalizzato specifico è definito da `ADOBEPASS_REDIRECT_URL `costante (ovvero `adobepass://ios.app`).

Alla fine AccessEnabler chiamerà [`setAuthenticationStatus()`](#setAuthNStatus) callback con codice di stato 0, che indica il completamento del flusso di logout.

**Nota:** Se l&#39;utente ha effettuato l&#39;accesso con Apple SSO, verrà attivato lo stato di VSA203. In questo caso, l&#39;utente deve essere informato di disconnettersi anche dalle impostazioni di sistema. In caso contrario, verrà eseguita una nuova autenticazione al riavvio dell&#39;applicazione.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: avvia il flusso di logout</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

**Parametri:** Nessuno

**Callback attivati:** `navigateToUrl:, `[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[Torna all&#39;inizio...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Utilizzare questo metodo per determinare il provider attualmente selezionato.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: determina il MVPD attualmente selezionato</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

**Parametri:** Nessuno

**Callback attivati:** [`selectedProvider:`](#selProv)

[Torna all&#39;inizio...](#apis)

</br>

### selectedProvider {#selProv}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** Callback attivato da AccessEnabler che fornisce all&#39;applicazione informazioni sull&#39;MVPD attualmente selezionato.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: informazioni sull'MVPD attualmente selezionato</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) selectedProvider:(MVPD *)mvpd;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

**Parametri**:

* *mvpd*: oggetto contenente informazioni sull’MVPD attualmente selezionato

**Attivato da:** [`getSelectedProvider`](#getSelProv)

[Torna all&#39;inizio...](#apis)

</br>

### getMetadata: {#getMeta}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Utilizzare questo metodo per recuperare informazioni esposte come metadati dalla libreria AccessEnabler. L’applicazione può accedere a questi dati fornendo un dizionario *chiave* parametro di input.

Esistono due tipi di metadati disponibili per i programmatori:

* Metadati statici (TTL del token di autenticazione, TTL del token di autorizzazione e ID dispositivo)
* Metadati utente (informazioni specifiche dell’utente, come ID utente e codice postale; possono essere trasmesse da un MVPD al dispositivo di un utente durante i flussi di autenticazione e autorizzazione)

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chiamata API: query di AccessEnabler per i metadati</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

**Parametri:**

* *keyDictionary*: una struttura di dati del dizionario con il seguente formato:
   * Se la chiave è `METADATA_OPCODE_KEY` e il valore è `METADATA_AUTHENTICATION`, quindi viene eseguita la query per ottenere l’ora di scadenza del token di autenticazione.
   * Se la chiave è `METADATA_OPCODE_KEY` e il valore è `METADATA_AUTHORIZATION` **e**\
     la chiave è `METADATA_RESOURCE_ID_KEY` e è un ID risorsa specifico, quindi viene eseguita la query per ottenere la scadenza del token di autorizzazione associato alla risorsa specificata.
   * Se la chiave è `METADATA_OPCODE_KEY` e il valore è `METADATA_DEVICE_ID`, quindi viene eseguita la query per ottenere l’id dispositivo corrente. Tieni presente che questa funzione è disabilitata per impostazione predefinita e i programmatori devono contattare l’Adobe per informazioni sull’abilitazione e sulle tariffe.
   * Se la chiave è `METADATA_OPCODE_KEY` e il valore è `METADATA_USER_META` **e** la chiave è `METADATA_USER_META_KEY` e value è il nome dei metadati, quindi viene eseguita la query per i metadati dell’utente. Elenco dei tipi di metadati utente disponibili:
      * `zip` - Elenco dei codici postali
      * `householdID` - Identificatore della famiglia. Nel caso in cui un MVPD non supporti i conti secondari, questo sarà identico a `userID`.
      * `maxRating` - Una raccolta delle valutazioni massime dei genitori per l&#39;utente
      * `userID` : identificatore utente. Se un MVPD supporta gli account secondari e l&#39;utente non è l&#39;account principale, `userID` sarà diverso da `householdID.`
      * `channelID` - Un elenco di canali che un utente ha il diritto di visualizzare.

  >[!NOTE]
  >
  >I metadati utente effettivi disponibili per un programmatore dipendono da ciò che viene reso disponibile da un MVPD. L’elenco verrà espanso man mano che nuovi metadati saranno disponibili e aggiunti al sistema di autenticazione di Adobe Pass.

**Callback attivati:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Ulteriori informazioni:** [Metadati utente](/help/authentication/user-metadata.md)

[Torna all&#39;inizio...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** Callback attivato da AccessEnabler dopo la chiamata[getAuthentication()](#getAuthN) se il richiedente corrente supporta almeno un MVPD con supporto SSO.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: risultato dei flussi SSO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) presentTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v2.0+

**Parametri**:

* viewController: rappresenta la finestra di dialogo SSO di Apple. Questo viewController deve essere visualizzato sullo schermo.

**Attivato da:** [`getAuthentication`](#getAuthN)

**Ulteriori informazioni:** [iOS/tvOS Single Sign On](#presentTvDialog)

[Torna all&#39;inizio...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** Callback attivato da AccessEnabler dopo la chiusura della finestra di dialogo SSO di Apple.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: risultato dei flussi SSO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) dismissTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v2.0+

**Parametri**:

* viewController: rappresenta la finestra di dialogo SSO di Apple. Questo viewController deve essere rimosso dallo schermo.

**Attivato da:** Azione utente

**Ulteriori informazioni:** [iOS/tvOS Single Sign On](#presentTvDialog)

[Torna all&#39;inizio...](#apis)

</br>

### setMetadataStatus:encrypted:forKey:andArguments: {#setMetaStatus}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** Callback attivato da AccessEnabler che distribuisce i metadati richiesti tramite un [`getMetadata:`](#getMeta) chiamare.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: risultato della richiesta di recupero dei metadati</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setMetadataStatus:(id)metadata
                 encrypted:(bool)encrypted 
                    forKey:(int)key 
              andArguments:(NSDictionary *)arguments; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:** v1.0+

**Parametri**:

* *metadati*: i metadati richiesti. Questo valore è un `NSString` nel caso di metadati statici (TTL di autenticazione, TTL di autorizzazione, ID dispositivo).  Si tratta di un oggetto complesso quando si richiedono metadati specifici dell’utente. Questo oggetto complesso è solitamente la rappresentazione Objective-C di un payload JSON (ad es. &quot;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;building&quot;: [&quot;150&quot;, &quot;320&quot;]}&#39; è tradotto in Objective-C come NSDictionary(&quot;strada&quot; -> &quot;Main Avenue&quot;, &quot;edifici&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;)).   Oggetto JSON metadati di esempio:

```JSON
        {
            updated: 1334243471,
            encrypted: ["encryptedProp"],
            data: {
                zip: ["12345", "34567"],
                maxRating: { 
                    "MPAA": "PG-13",
                    "VCHIP": "TV-Y", 
                    "URL": "http://exam.pl/e/manage/ratings"
                },
                householdID: "3456",
                userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
                channelID: ["channel-1", "channel-2"]
            }
```

* *crittografato*: valore booleano che specifica se i metadati recuperati sono crittografati o meno. Questo parametro è significativo solo per le richieste di metadati utente, non ha significato per i metadati statici (ad esempio, TTL di autenticazione) che vengono sempre ricevuti non crittografati. Se questo parametro è impostato su true, spetta al programmatore ottenere il valore dei metadati utente non crittografati eseguendo una decrittografia RSA utilizzando la chiave privata di whitelisting (la stessa chiave privata utilizzata per la firma dell’ID richiedente nel [`setRequestor:setSignedRequestorId:`](#setReq) e `setRequestor:setSignedRequestorId:serviceProviders: `chiamate).

* *chiave*: chiave utilizzata per formulare la richiesta di recupero dei metadati.

* *argomenti*: stesso dizionario passato al [`getMetadata:`](#getMeta) chiamare. Fornito per consentire all’applicazione di far corrispondere le richieste con le risposte.

**Attivato da:** [`getMetadata:`](#getMeta)

**Ulteriori informazioni:** [Metadati utente](/help/authentication/user-metadata.md)


[Torna all&#39;inizio...](#apis)

</br>

### MVPD {#mvpd}

**File:** AccessEnabler/headers/model/MVPD.h

**Descrizione** Descrive l&#39;oggetto MVPD. Può essere utilizzato per ottenere informazioni sulle proprietà dell&#39;MVPD.

**Disponibilità:** v1.0+ [La proprietà boardingStatus è disponibile dalla versione 2.2]

**Proprietà**:

* (NSString) ID: l&#39;ID MVPD.
* (NSString) displayName: il nome MVPD. [Da utilizzare per la visualizzazione nel selettore]
* (NSString) logoURL: l’indirizzo del logo MVPD.
* (BOOL) enablePlatformServices - Se true, MVPD supporta i servizi SSO come [SSO APPLE](#presentTvDialog).
* (NSString) boardingStatus - Può avere 3 valori:
   * nil: MVPD non supporta Apple SSO.
   * SELETTORE: il MVPD può essere visualizzato nel selettore di Apple, ma il flusso di autenticazione viene eseguito per Adobe.
   * SUPPORTATO: MVPD è completamente supportato da Apple e utilizzerà il token SSO di Apple.

[Torna all&#39;inizio...](#apis)

</br>

## Tracciamento degli eventi {#tracking}

AccessEnabler attiva un callback aggiuntivo non necessariamente correlato ai flussi di adesione. Implementazione di [`sendTrackingData()`](#sendTracking) la funzione di callback è facoltativa, ma consente all&#39;applicazione di tenere traccia di eventi specifici e di compilare statistiche quali il numero di tentativi di autenticazione/autorizzazione riusciti/non riusciti.

### sendTrackingData:forEventType: {#sendTracking}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** Callback attivato dal segnale di AccessEnabler all&#39;applicazione al verificarsi di vari eventi come il completamento/il fallimento dei flussi di autenticazione/autorizzazione. Con Adobe Pass Authentication 1.6, il tipo di dispositivo, il tipo di client AccessEnabler e il sistema operativo vengono segnalati da [`sendTrackingData()`](#sendTracking). Il [`sendTrackingData()`](#sendTracking) il callback rimane compatibile con le versioni precedenti.

**Callback: eventi di tracciamento**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Disponibilità:** v1.0+

**Nota:** Il tipo di dispositivo e il sistema operativo sono derivati tramite l&#39;uso di una libreria Java pubblica (<http://java.net/projects/user-agent-utils>) e la stringa dell&#39;agente utente. Tieni presente che queste informazioni vengono fornite solo come metodo grezzo per suddividere le metriche operative in categorie di dispositivi, ma che l’Adobe non può assumersi alcuna responsabilità per risultati errati. Utilizza la nuova funzionalità di conseguenza.

* Valori possibili per il tipo di dispositivo:
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* Valori possibili per il tipo di client AccessEnabler:
   * `flash`
   * `html5`
   * `ios`
   * `android`


**Parametri**:

* *evento*: codice dell’evento di cui viene eseguito il tracciamento. Esistono tre possibili tipi di eventi di tracciamento:
   * **authorizationDetection:** ogni volta che viene restituita una richiesta di token di autorizzazione (l’evento è `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** ogni volta che si verifica un controllo di autenticazione (l’evento è `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** quando l’utente seleziona un MVPD nel modulo di selezione MVPD (l’evento è `TRACKING_GET_SELECTED_PROVIDER`)
* *dati*: dati aggiuntivi associati all’evento segnalato. Questi dati vengono presentati sotto forma di elenco di valori.

**Attivato da:** `checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN), `checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ), `setSelectedProvider:`

Istruzioni per l’interpretazione dei valori in *dati* array:

* Per trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Se la richiesta del token è stata eseguita correttamente (true/false) e in caso di esito positivo:
   * **1** - Stringa ID MVPD
   * **2** - GUID (hash MD5)
   * **3** - Token già nella cache (true/false)
   * **4** - Tipo di dispositivo
   * **5** - Tipo di client AccessEnabler
   * **6** - Tipo di sistema operativo

* Per trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Se la richiesta del token è stata eseguita correttamente (true/false) e in caso di esito positivo:
   * **1** - ID MVPD
   * **2** - GUID (hash MD5)
   * **3** - Token già nella cache (true/false)
   * **4** - Errore
   * **5** - Dettagli
   * **6** - Tipo di dispositivo
   * **7** - Tipo di client AccessEnabler
   * **8** - Tipo di sistema operativo
* Per trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - ID del MVPD attualmente selezionato
   * **1** - Tipo di dispositivo
   * **2** - Tipo di client AccessEnabler
   * **3** - Tipo di sistema operativo

</br>
