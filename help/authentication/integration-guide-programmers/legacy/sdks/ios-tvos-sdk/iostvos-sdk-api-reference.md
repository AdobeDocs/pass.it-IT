---
title: Riferimento API per iOS/tvOS
description: Riferimento API per iOS/tvOS
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '6942'
ht-degree: 0%

---

# (Legacy) Riferimento API SDK per iOS/tvOS {#iostvos-sdk-api-reference}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Introduzione {#intro}

Questa pagina descrive i metodi e le funzioni di callback esposti dal client nativo iOS/tvOS per l’autenticazione di Adobe Pass. I metodi e le funzioni di callback qui descritti sono definiti nei file di intestazione `AccessEnabler.h` e `EntitlementDelegate.h`. Sono disponibili in iOS AccessEnabler SDK qui: `[SDK directory]/AccessEnabler/headers/api/`


Documentazione associata:

* Per una descrizione dettagliata di come implementare Adobe Pass
flusso di autenticazione dei diritti tramite questa API, consulta il [Manuale dell&#39;integrazione di iOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md).
* Per la versione più recente di iOS AccessEnabler SDK, vedi [Libreria iOS Native Access Enabler](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobe ti incoraggia a utilizzare solo le API *public* di autenticazione Adobe Pass:
>
>* Le API pubbliche sono disponibili e completamente testate su tutti i tipi di client supportati. Per qualsiasi funzione pubblica, ci assicuriamo che ogni tipo di client abbia una versione corrispondente dei metodi associati.
>* Le API pubbliche devono essere il più stabili possibile, per supportare la compatibilità con le versioni precedenti e garantire che le integrazioni dei partner non si interrompano. Tuttavia, per le API non pubbliche, ci riserviamo il diritto di modificare la loro firma in qualsiasi momento futuro. Se incontri un flusso particolare che non può essere supportato tramite una combinazione delle chiamate API di autenticazione di Adobe Pass pubbliche correnti, l’approccio migliore è quello di farcelo sapere. Tenendo conto delle tue esigenze, possiamo modificare le API pubbliche e fornire una soluzione stabile.

</br>

## Riferimento API {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - Crea un&#39;istanza dell&#39;oggetto AccessEnabler.

* **[OBSOLETO]** [init](#init) - Crea un&#39;istanza dell&#39;oggetto AccessEnabler.

* [`setOptions:options:`](#setOptions) - Configura le opzioni globali di SDK come profile o visitorID.

* [`setRequestor:`](#setReqV3) [`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - Stabilisce l&#39;identità del programmatore.

* **[OBSOLETO]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - Stabilisce l&#39;identità del programmatore.

* **[OBSOLETO]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos)-Stabilisce l&#39;identità del programmatore.

* [`setRequestorComplete:`](#setReqComplete) - Informa l&#39;applicazione che la fase di configurazione è stata completata.

* [`checkAuthentication`](#checkAuthN) - Controlla lo stato di autenticazione dell&#39;utente corrente.

* [`getAuthentication`](#getAuthN), [`getAuthentication:withData:`](#getAuthN) - Avvia il flusso di lavoro di autenticazione completo.

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN) [andFilter](#getAuthN_filter) - Avvia il flusso di lavoro di autenticazione completo.

* [`displayProviderDialog:`](#dispProvDialog) - Informa l&#39;applicazione per creare un&#39;istanza degli elementi dell&#39;interfaccia utente appropriati affinché l&#39;utente possa selezionare un MVPD.

* [`setSelectedProvider:`](#setSelProv) - Informa AccessEnabler della selezione MVPD dell&#39;utente.

* [`navigateToUrl:`](#nav2url) - Informa l&#39;applicazione che all&#39;utente deve essere presentata la pagina di accesso di MVPD.

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - Informa l&#39;applicazione che all&#39;utente deve essere presentata la pagina di accesso di MVPD, utilizzando SFSafariViewController

* [`handleExternalURL:url`](#handleExternalURL) - Completa il flusso di autenticazione/disconnessione.

* **[OBSOLETO]** [`getAuthenticationToken`](#getAuthNToken) - Richiede il token di autenticazione al server back-end.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) - Informa l&#39;applicazione dello stato del flusso di autenticazione.

* [`checkPreauthorizedResources:`](#checkPreauth) - Determina se l&#39;utente è già autorizzato a visualizzare risorse protette specifiche.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) - Determina se l&#39;utente è già autorizzato a visualizzare risorse protette specifiche.

* [`preauthorizedResources:`](#preauthResources) - Fornisce un elenco di risorse che l&#39;utente è già autorizzato a visualizzare.

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) - Controlla lo stato di autorizzazione dell&#39;utente corrente.

* [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ) - Avvia il flusso di autorizzazione.

* [`setToken:forResource:`](#setToken) - Informa l&#39;applicazione che il flusso di autorizzazione è stato completato.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) - Informa l&#39;applicazione che il flusso di autorizzazione non è riuscito.

* [`logout`](#logout) - Avvia il flusso di disconnessione.

* [`getSelectedProvider`](#getSelProv) - Determina il provider attualmente selezionato.

* [`selectedProvider:`](#selProv) - Fornisce all&#39;applicazione informazioni sul MVPD attualmente selezionato.

* [`getMetadata:`](#getMeta) - Recupera le informazioni esposte come metadati dalla libreria AccessEnabler.

* [`presentTvProviderDialog:`](#presentTvDialog) - Informa l&#39;applicazione per visualizzare la finestra di dialogo SSO di Apple.

* [`dismissTvProviderDialog:`](#dismissTvDialog) - Informa l&#39;applicazione per nascondere la finestra di dialogo SSO di Apple.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - Consegna i metadati richiesti da una chiamata [`getMetadata:`](#getMeta).

* [`sendTrackingData:forEventType:`](#sendTracking) - Fornisce informazioni sui dati di tracciamento.

* [`MVPD`](#mvpd) - Classe MVPD. [Contiene informazioni su MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** crea istanze dell&#39;oggetto AccessEnabler. Deve essere presente una singola istanza di AccessEnabler per ogni istanza dell&#39;applicazione.

| **Chiamata API: costruttore iOS AccessEnabler** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Disponibilità:** v3.0+

**Parametri:**

* **softwareStatement:** Stringa che identifica l&#39;applicazione nel sistema di Adobe. Controllare come ottenere un rendiconto software.

[Torna all&#39;inizio...](#apis)



### init - [OBSOLETO]{#init}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** crea istanze dell&#39;oggetto AccessEnabler. Deve essere presente una singola istanza di AccessEnabler per ogni istanza dell&#39;applicazione.

| Chiamata API: costruttore iOS AccessEnabler |
| --- |
| ```- (id) init;``` |

**Disponibilità:** v1.0+ **Fino a:** v3.0

**Parametri:** Nessuno

[Torna all&#39;inizio...](#apis)

</br>

### setOptions:opzioni {#setOptions}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Configura le opzioni globali di SDK. Accetta un NSDictionary come argomento. I valori del dizionario vengono passati al server insieme a ogni chiamata di rete effettuata dal SDK.

**Nota:** i valori verranno passati al server indipendentemente dal flusso corrente (autenticazione/autorizzazione). Se desideri modificare i valori, puoi chiamare questo metodo in qualsiasi momento.

| Chiamata API: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Disponibilità:** v2.3.0+

**Parametri:**

* *options*: NSDictionary contenente le opzioni globali di SDK. Attualmente, sono disponibili le seguenti opzioni:
   * **applicationProfile** - Può essere utilizzato per creare configurazioni server basate su questo valore.
   * **visitorID** - Servizio ID Experience Cloud. Questo valore può essere utilizzato in seguito per i rapporti di analisi avanzata.
   * **handleSVC** - Valore booleano che indica se il programmatore gestirà i controller SFSafariView. Per ulteriori informazioni, vedere il supporto di [SFSafariViewController in iOS SDK 3.2+](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md).
      * Se è impostato su **false,** SDK presenterà automaticamente all&#39;utente finale un oggetto SFSafariViewController. Il SDK passerà ulteriormente all’URL della pagina di accesso MVPDs.
      * Se è impostato su **true,** SDK **NOT** presenterà automaticamente all&#39;utente finale un oggetto SFSafariViewController. SDK attiverà ulteriormente **navigate(toUrl:{url}, useSVC:YES)**.
* **dispositivo\_info** - Informazioni client come descritto in [Trasmissione delle informazioni client](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

[Torna all&#39;inizio...](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** stabilisce l&#39;identità del programmatore. A ciascun programmatore viene assegnato un ID univoco al momento della registrazione a Adobe per il sistema di autenticazione di Adobe Pass. Quando si tratta di token SSO e remoti, lo stato di autenticazione può cambiare quando l&#39;applicazione è in background, setRequestor può essere richiamato nuovamente quando l&#39;applicazione viene portata in primo piano per sincronizzarsi con lo stato del sistema (recuperare un token remoto se SSO è abilitato o eliminare il token locale se nel frattempo si è verificato un logout).

La risposta del server contiene un elenco di MVPD insieme ad alcune informazioni di configurazione collegate all&#39;identità del programmatore. La risposta del server viene utilizzata internamente dal codice AccessEnabler. Solo lo stato dell&#39;operazione (ovvero OPERAZIONE RIUSCITA/NON RIUSCITA) viene presentato all&#39;applicazione tramite il callback `setRequestorComplete:`.

Se il parametro `urls` non viene utilizzato, la chiamata di rete risultante esegue il targeting dell&#39;URL predefinito del provider di servizi: l&#39;ambiente Adobe RELEASE/production.


Se viene fornito un valore per il parametro `urls`, la chiamata di rete risultante eseguirà il targeting di tutti gli URL forniti nel parametro `urls`. Tutte le richieste di configurazione vengono attivate contemporaneamente in thread separati. Il primo responder ha la precedenza quando si compila l’elenco degli MVPD. Per ogni MVPD nell&#39;elenco, AccessEnabler ricorda l&#39;URL del provider di servizi associato. Tutte le richieste di adesione successive vengono indirizzate all’URL associato al provider di servizi che è stato associato al MVPD di destinazione durante la fase di configurazione.

| Chiamata API: configurazione richiedente |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Disponibilità:** v3.0+

| Chiamata API: configurazione richiedente |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Disponibilità:** v3.0+

**Parametri:**

* *requestorID*: ID univoco associato al programmatore. La prima volta che ti registri al servizio di autenticazione di Adobe Pass, passa l’ID univoco assegnato da Adobe al tuo sito.
* *url*: parametro facoltativo; per impostazione predefinita, viene utilizzato il provider di servizi Adobe (http://sp.auth.adobe.com/). Questo array consente di specificare gli endpoint per i servizi di autenticazione e autorizzazione forniti da Adobe (a scopo di debug possono essere utilizzate istanze diverse). È possibile utilizzare questa opzione per specificare più istanze del provider di servizi di autenticazione di Adobe Pass. In questo caso, l’elenco MVPD è composto dagli endpoint di tutti i provider di servizi. Ogni MVPD è associato al provider di servizi più veloce, ovvero il provider che ha risposto per primo e che supporta tale MVPD.

>[!NOTE]
>
>Se chiamata senza il parametro `serviceProviders`, la libreria recupererà la configurazione dal provider di servizi predefinito (ovvero `https://sp.auth.adobe.com` per il profilo di produzione o `https://sp.auth-staging.adobe.com` per il profilo di staging). Se viene fornito il parametro `serviceProviders`, deve essere una matrice di URL. Le informazioni di configurazione vengono recuperate da tutti gli endpoint specificati e unite. Se esistono informazioni duplicate in risposte diverse del provider di servizi, il conflitto viene risolto a favore del server con la risposta più rapida, ovvero il server con il tempo di risposta più breve ha la precedenza.

**Callback attivati:** [`setRequestorComplete:`](#setReqComplete)

[Torna all&#39;inizio...](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [OBSOLETO] {#setReq}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** stabilisce l&#39;identità del programmatore. A ciascun programmatore viene assegnato un ID univoco al momento della registrazione a Adobe per il sistema di autenticazione di Adobe Pass. Quando si tratta di token SSO e remoti, lo stato di autenticazione può cambiare quando l&#39;applicazione è in background, setRequestor può essere richiamato nuovamente quando l&#39;applicazione viene portata in primo piano per sincronizzarsi con lo stato del sistema (recuperare un token remoto se SSO è abilitato o eliminare il token locale se nel frattempo si è verificato un logout).

La risposta del server contiene un elenco di MVPD insieme ad alcune informazioni di configurazione collegate all&#39;identità del programmatore. La risposta del server viene utilizzata internamente dal codice AccessEnabler. Solo lo stato dell&#39;operazione (ovvero OPERAZIONE RIUSCITA/NON RIUSCITA) viene presentato all&#39;applicazione tramite il callback `setRequestorComplete:`.

Se il parametro `urls` non viene utilizzato, la chiamata di rete risultante esegue il targeting dell&#39;URL predefinito del provider di servizi: l&#39;ambiente Adobe RELEASE/production.

Se viene fornito un valore per il parametro `urls`, la chiamata di rete risultante eseguirà il targeting di tutti gli URL forniti nel parametro `urls`. Tutte le richieste di configurazione vengono attivate contemporaneamente in thread separati. Il primo responder ha la precedenza quando si compila l’elenco degli MVPD. Per ogni MVPD nell&#39;elenco, AccessEnabler ricorda l&#39;URL del provider di servizi associato. Tutte le richieste di adesione successive vengono indirizzate all’URL associato al provider di servizi che è stato associato al MVPD di destinazione durante la fase di configurazione.

| Chiamata API: configurazione richiedente |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Disponibilità:** v1.0+ **Fino a:** v3.0

| Chiamata API: configurazione richiedente |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Disponibilità:** v1.0+ **Fino a:** v3.0

**Parametri:**

* *requestorID*: ID univoco associato al programmatore. Passa l’ID univoco assegnato da Adobe al sito alla prima registrazione al servizio di autenticazione di Adobe Pass.
* *signedRequestorID*: **Questo parametro esiste in iOS AccessEnabler versioni 1.2 e successive.** Copia dell&#39;ID richiedente con firma digitale della chiave privata. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: parametro facoltativo; per impostazione predefinita, viene utilizzato il provider di servizi Adobe (http://sp.auth.adobe.com/). Questo array consente di specificare gli endpoint per i servizi di autenticazione e autorizzazione forniti da Adobe (a scopo di debug possono essere utilizzate istanze diverse). È possibile utilizzare questa opzione per specificare più istanze del provider di servizi di autenticazione di Adobe Pass. In questo caso, l’elenco MVPD è composto dagli endpoint di tutti i provider di servizi. Ogni MVPD è associato al provider di servizi più veloce, ovvero il provider che ha risposto per primo e che supporta tale MVPD.

**Note:** Se chiamata senza il parametro `serviceProviders`, la libreria recupererà la configurazione dal provider di servizi predefinito, ovvero `https://sp.auth.adobe.com` per il profilo di produzione o `https://sp.auth-staging.adobe.com` per il profilo di gestione temporanea. Se viene fornito il parametro `serviceProviders`, deve essere una matrice di URL. Le informazioni di configurazione vengono recuperate da tutti gli endpoint specificati e unite. Se esistono informazioni duplicate in risposte diverse del provider di servizi, il conflitto viene risolto a favore del server con la risposta più rapida (ovvero, il server con il tempo di risposta più breve ha la precedenza).

**Callback attivati:** [`setRequestorComplete:`](#setReqComplete)


[Torna all&#39;inizio...](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [OBSOLETO] {#setReq_tvos}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** stabilisce l&#39;identità del programmatore. A ciascun programmatore viene assegnato un ID univoco al momento della registrazione a Adobe per il sistema di autenticazione di Adobe Pass. Questa impostazione deve essere eseguita una sola volta durante il ciclo di vita dell&#39;applicazione.

La risposta del server contiene un elenco di MVPD insieme ad alcune informazioni di configurazione collegate all&#39;identità del programmatore. La risposta del server viene utilizzata internamente dal codice AccessEnabler. Solo lo stato dell&#39;operazione (ovvero OPERAZIONE RIUSCITA/NON RIUSCITA) viene presentato all&#39;applicazione tramite il callback `setRequestorComplete:`.

Se il parametro `urls` non viene utilizzato, la chiamata di rete risultante esegue il targeting dell&#39;URL predefinito del provider di servizi: l&#39;ambiente Adobe RELEASE/production.

Se viene fornito un valore per il parametro `urls`, la chiamata di rete risultante eseguirà il targeting di tutti gli URL forniti nel parametro `urls`. Tutte le richieste di configurazione vengono attivate contemporaneamente in thread separati. Il primo responder ha la precedenza quando si compila l’elenco degli MVPD. Per ogni MVPD nell&#39;elenco, AccessEnabler ricorda l&#39;URL del provider di servizi associato. Tutte le richieste di adesione successive vengono indirizzate all’URL associato al provider di servizi che è stato associato al MVPD di destinazione durante la fase di configurazione.



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

* *requestorID*: ID univoco associato al programmatore. La prima volta che trasmetti l&#39;ID univoco assegnato da Adobe al tuo sito   registrati con il servizio di autenticazione di Adobe Pass.
* *signedRequestorID*: **Questo parametro esiste in iOS AccessEnabler   versioni 1.2 e successive.** Copia dell&#39;ID richiedente con firma digitale della chiave privata. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: parametro facoltativo; per impostazione predefinita, il provider di servizi Adobe   (http://sp.auth.adobe.com/). Questo array consente di specificare gli endpoint per i servizi di autenticazione e autorizzazione forniti da Adobe (a scopo di debug possono essere utilizzate istanze diverse). È possibile utilizzare questa opzione per specificare più istanze del provider di servizi di autenticazione di Adobe Pass. In questo caso, l’elenco MVPD è composto dagli endpoint di tutti i provider di servizi. Ogni MVPD è associato al provider di servizi più veloce, ovvero il provider che ha risposto per primo e che supporta tale MVPD.
* secret and publicKey: il segreto e la chiave pubblica utilizzati per firmare le chiamate della seconda schermata. Per ulteriori informazioni, consulta la [documentazione senza client](#create_dev).

Se chiamata senza il parametro `serviceProviders`, la libreria recupererà la configurazione dal provider di servizi predefinito (ovvero `https://sp.auth.adobe.com` per il profilo di produzione o https://sp.auth-staging.adobe.com per il profilo di staging). Se viene fornito il parametro `serviceProviders`, deve essere una matrice di URL. Le informazioni di configurazione vengono recuperate da tutti gli endpoint specificati e unite. Se esistono informazioni duplicate in risposte diverse del provider di servizi, il conflitto viene risolto a favore del server con la risposta più rapida (ovvero, il server con il tempo di risposta più breve ha la precedenza).

**Callback attivati:** [`setRequestorComplete:`](#setReqComplete)

[Torna all&#39;inizio...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** callback attivato da AccessEnabler che informa l&#39;applicazione del completamento della fase di configurazione. Questo è un segnale che l’app può iniziare a emettere richieste di adesione. L’applicazione non può inviare richieste di adesione fino al completamento della fase di configurazione.

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

* *status*: può assumere uno dei seguenti valori:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - fase di configurazione completata
   * `ACCESS_ENABLER_STATUS_ERROR` - fase di configurazione non riuscita

**Attivato da:**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Torna all&#39;inizio...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** verifica lo stato di autenticazione dell&#39;utente corrente.
A tale scopo, cerca un token di autenticazione valido nella directory locale
spazio di archiviazione dei token. Questo metodo non esegue chiamate di rete e si consiglia di chiamarlo sul thread principale.
Viene utilizzato dall’applicazione per eseguire una query sullo stato di autenticazione dell’utente e
aggiorna di conseguenza l’interfaccia utente (ad esempio, aggiorna l’interfaccia utente di accesso/disconnessione). Il
lo stato di autenticazione viene comunicato all’applicazione tramite
il callback [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).


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

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** avvia il flusso di lavoro di autenticazione completo. Viene avviato controllando lo stato di autenticazione. Se non è già autenticato, viene avviato il computer dello stato del flusso di autenticazione:

* se l&#39;ultimo tentativo di autenticazione ha avuto esito positivo, MVPD   la fase di selezione viene saltata e   il callback [`navigateToUrl:`](#nav2url) è attivato. Il   L&#39;applicazione utilizza questo callback per creare un&#39;istanza del controllo WebView che presenta all&#39;utente la pagina di accesso di MVPD. **[NOTA: a partire da Access Enabler 1.5, questa funzionalità non è disponibile a causa di una limitazione in SDK].**
* se l&#39;ultimo tentativo di autenticazione non è riuscito o se l&#39;utente si è disconnesso in modo esplicito, il callback [`displayProviderDialog:`](#dispProvDialog) è   attivato. L&#39;applicazione utilizza questo callback per visualizzare l&#39;interfaccia utente di selezione di MVPD. Anche l&#39;app deve riprendere il flusso di autenticazione informando la libreria AccessEnabler della selezione MVPD dell&#39;utente tramite il metodo [`setSelectedProvider:`](#setSelProv).

Man mano che le credenziali dell’utente vengono verificate nella pagina di accesso di MVPD, l’applicazione deve monitorare le diverse operazioni di reindirizzamento che si verificano durante l’autenticazione dell’utente nella pagina di accesso di MVPD. Quando vengono immesse le credenziali corrette, il controllo WebView viene reindirizzato a un URL personalizzato definito dalla costante `ADOBEPASS_REDIRECT_URL`. Questo URL non deve essere caricato da WebView. L’applicazione deve intercettare questo URL e interpretare questo evento come un segnale del completamento della fase di accesso. Deve quindi passare il controllo ad AccessEnabler per completare il flusso di autenticazione chiamando il metodo [handleExternalURL](#handleExternalURL).

Infine, lo stato di autenticazione viene comunicato all&#39;applicazione tramite il callback [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

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

* *forceAuthn*: flag che specifica se avviare il flusso di autenticazione, indipendentemente dal fatto che l&#39;utente sia già autenticato o meno.
* *dati*: dizionario costituito da coppie chiave-valore da inviare al servizio di pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare il SDK.

**Callback attivati:** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[Torna all&#39;inizio...](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** avvia il flusso di lavoro di autenticazione completo. Viene avviato controllando lo stato di autenticazione. Se non è già autenticato, viene avviato il computer dello stato del flusso di autenticazione:

* [presentTvProviderDialog()](#presentTvDialog) verrà chiamato se il richiedente corrente dispone di almeno un MVPD che supporta l&#39;SSO. Se MVPD non supporta l&#39;SSO, verrà avviato il flusso di autenticazione classico e il parametro di filtro verrà ignorato.
* Dopo il completamento del flusso SSO di Apple [`dismissTvProviderDialog()`](#dismissTvDialog) verrà attivato e il processo di autenticazione verrà completato.

Infine, lo stato di autenticazione viene comunicato all&#39;applicazione tramite il callback [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

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

* *forceAuthn*: flag che specifica se avviare il flusso di autenticazione, indipendentemente dal fatto che l&#39;utente sia già autenticato o meno.
* *dati*: dizionario costituito da coppie chiave-valore da inviare al servizio di pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare il SDK.
* filter (filtro): dizionario con due elenchi di ID MVPD che devono essere visualizzati nella finestra di dialogo SSO di Apple. Qualsiasi MVPD che non supporta l&#39;SSO verrà ignorato, ma l&#39;ordine verrà rispettato. Il dizionario deve avere due chiavi:
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

* *forceAuthn*: flag che specifica se avviare il flusso di autenticazione, indipendentemente dal fatto che l&#39;utente sia già autenticato o meno.
* *dati*: dizionario costituito da coppie chiave-valore da inviare al servizio di pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare il SDK.
* filter: elenco di ID MVPD da visualizzare nella finestra di dialogo SSO di Apple. Qualsiasi MVPD che non supporta l&#39;SSO verrà ignorato, ma l&#39;ordine verrà rispettato.

**Callback attivati:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Torna all&#39;inizio...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** Il callback è stato attivato da AccessEnabler per informare l&#39;applicazione che è necessario creare un&#39;istanza degli elementi dell&#39;interfaccia utente appropriati per consentire all&#39;utente di selezionare il MVPD desiderato. Il callback fornisce un elenco di oggetti MVPD con informazioni aggiuntive che possono aiutare a creare correttamente il pannello dell’interfaccia utente di selezione (come l’URL che punta al logo MVPD, il nome visualizzato descrittivo, ecc.)

Dopo che l&#39;utente ha selezionato il MVPD desiderato, l&#39;applicazione di livello superiore deve riprendere il flusso di autenticazione chiamando `setSelectedProvider:` e trasmettendogli l&#39;ID del MVPD corrispondente alla selezione dell&#39;utente.

**Interruzione del flusso di autenticazione** - Questo è un punto in cui l&#39;utente può premere il pulsante &quot;Indietro&quot;, che equivale all&#39;interruzione del flusso di autenticazione. In questo scenario, è necessario che l&#39;applicazione chiami il metodo [setSelectedProvider:](#setSelProv), passando null come parametro, per consentire ad AccessEnabler di reimpostare il computer dello stato di autenticazione.

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

* *mvpds*: elenco di oggetti MVPD contenenti informazioni relative a MVPD che l&#39;applicazione può utilizzare per creare gli elementi dell&#39;interfaccia utente di selezione di MVPD.

**Attivato da:** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN),`getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[Torna all&#39;inizio...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** questo metodo viene chiamato dall&#39;applicazione per informare Access Enabler della selezione MVPD dell&#39;utente. È possibile utilizzare questo metodo per selezionare o modificare il provider di servizi utilizzato per l&#39;autenticazione.

Se il MVPD selezionato è un MVPD TempPass, effettuerà automaticamente l’autenticazione con tale MVPD senza dover successivamente chiamare getAuthentication().

Tieni presente che questo non è possibile per il Passaggio temporaneo promozionale in cui vengono forniti parametri aggiuntivi sul metodo getAuthentication().

Quando si passa *null* come parametro, Access Enabler presume che l&#39;utente abbia annullato il flusso di autenticazione (ovvero ha premuto il pulsante &quot;Indietro&quot;) e risponde reimpostando lo stato-computer di autenticazione e richiamando il callback [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) con il codice di errore `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR`.

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

**Callback attivati:** `setAuthenticationStatus:errorCode:`,`sendTrackingData:forEventType:`, [`navigateToUrl:`](#nav2url)

[Torna all&#39;inizio...](#apis)

</br>

#### navigateToUrl: {#nav2url}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione:** callback attivato da AccessEnabler per richiedere all&#39;applicazione di creare un&#39;istanza di un controller UIWebView/WKWebView e di caricare l&#39;URL fornito nel parametro **`url`** del callback. Il callback passa il parametro **`url`** che rappresenta l&#39;URL dell&#39;endpoint di autenticazione o l&#39;URL dell&#39;endpoint di disconnessione.

Poiché il controller UIWebView/WKWebView` ` viene sottoposto a diversi reindirizzamenti, l&#39;applicazione deve monitorare l&#39;attività del controller e rilevare il momento in cui carica un URL personalizzato specifico definito dalla costante `ADOBEPASS_REDIRECT_URL ` (ovvero `adobepass://ios.app`). Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall&#39;applicazione come un segnale che il flusso di autenticazione o disconnessione è stato completato e che è sicuro chiudere il controller. Quando il controller carica questo URL personalizzato specifico, l&#39;applicazione deve chiudere UIWebView/WKWebView e chiamare il metodo API `handleExternalURL:url ` di AccessEnabler.

**Nota:** Tieni presente che nel caso del flusso di autenticazione si tratta di un punto in cui l&#39;utente può premere il pulsante &quot;Indietro&quot;, che equivale all&#39;interruzione del flusso di autenticazione. In questo caso, è necessario che l&#39;applicazione chiami il metodo [setSelectedProvider:](#setSelProv) passando **`nil`** come parametro e dando la possibilità ad AccessEnabler di reimpostare il computer dello stato di autenticazione.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: visualizzazione della pagina di accesso di MVPD</th>
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

* *url*: l&#39;URL che punta alla pagina di accesso di MVPD

**Attivato da:** [setSelectedProvider:](#setSelProv)



[Torna all&#39;inizio...](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione:** callback attivato da AccessEnabler anziché dal callback `navigateToUrl:` nel caso in cui l&#39;applicazione abbia precedentemente abilitato la gestione manuale di Safari View Controller (SVC) tramite la chiamata [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) e solo nel caso di MVPD che richiedono Safari View Controller (SVC). Per tutti gli altri MVPD verrà chiamato il callback `navigateToUrl:`. Per informazioni dettagliate sulla gestione di SVC (Safari View Controller), vedere il supporto di SFSafariViewController in iOS SDK 3.2+[&#128279;](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md).

Simile al callback `navigateToUrl:`, il `navigateToUrl:useSVC:` viene attivato da AccessEnabler per richiedere all&#39;applicazione di creare un&#39;istanza di un controller `SFSafariViewController` e caricare l&#39;URL fornito nel parametro **`url`** del callback. Il callback passa il parametro **`url`** che rappresenta l&#39;URL dell&#39;endpoint di autenticazione o l&#39;URL dell&#39;endpoint di disconnessione e il parametro **`useSVC`** che specifica che l&#39;applicazione deve utilizzare un `SFSafariViewController`.

Poiché il controller `SFSafariViewController` viene sottoposto a diversi reindirizzamenti, l&#39;applicazione deve monitorare l&#39;attività del controller e rilevare il momento in cui carica un URL personalizzato specifico definito dal `application's custom scheme` (ad esempio **&#x200B; **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall&#39;applicazione come un segnale che il flusso di autenticazione o disconnessione è stato completato e che è sicuro chiudere il controller. Quando il controller carica questo URL personalizzato specifico, l&#39;applicazione deve chiudere `SFSafariViewController` e chiamare il metodo API `handleExternalURL:url ` di AccessEnabler.

**Nota:** Tieni presente che nel caso del flusso di autenticazione si tratta di un punto in cui l&#39;utente può premere il pulsante &quot;Indietro&quot;, che equivale all&#39;interruzione del flusso di autenticazione. In questo caso, è necessario che l&#39;applicazione chiami il metodo [setSelectedProvider:](#setSelProv) passando **`nil`** come parametro e dando la possibilità ad AccessEnabler di reimpostare il computer dello stato di autenticazione.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: visualizzazione della pagina di accesso di MVPD in SFSafariViewController</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
&#x200B;- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilità:**&#x200B;v 3.2+

**Parametri**:

* *url:* l&#39;URL che punta alla pagina di accesso di MVPD
* *useSVC:* se l&#39;URL deve essere caricato in SFSafariViewController.

**Attivato da:**&#x200B;[ setOptions:](#setOptions) prima di [setSelectedProvider:](#setSelProv)

[Torna all&#39;inizio...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene chiamato dall&#39;applicazione per completare il flusso di autenticazione o disconnessione. Questo metodo deve essere chiamato subito dopo che l&#39;applicazione rileva il momento in cui il controller `UIWebView/WKWebView or SFSafariViewController` viene reindirizzato a un URL personalizzato specifico. Nel caso in cui l&#39;applicazione debba utilizzare un controller `SFSafariViewController `, l&#39;URL personalizzato specifico è definito da `application's custom scheme` (ad esempio `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), altrimenti questo URL personalizzato specifico è definito dalla costante `ADOBEPASS_REDIRECT_URL ` (ad esempio `adobepass://ios.app`).

Nel caso del flusso di autenticazione, AccessEnabler completa il flusso recuperando il token di autenticazione dal server back-end e archiviandolo localmente nell&#39;archivio dei token. AccessEnabler informerà l&#39;applicazione che il flusso di autenticazione è stato completato chiamando il callback `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> con il codice di stato 1, che indica l&#39;esito positivo. Se si verifica un errore durante l&#39;esecuzione di questi passaggi, il callback `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> viene attivato con il codice di stato 0, che indica un errore di autenticazione, nonché un codice di errore corrispondente.

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

* *url*: l&#39;URL intercettato dal controllo ` UIWebView/WKWebView or SFSafariViewController ` come stringa.


**Callback attivati:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Torna all&#39;inizio...](#apis)

</br>

#### getAuthenticationToken - [OBSOLETO] {#getAuthNToken}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** completa il flusso di autenticazione richiedendo il token di autenticazione al server back-end. Questo metodo deve essere chiamato dall&#39;applicazione solo in risposta all&#39;evento in cui il controllo WebView che ospita la pagina di accesso di MVPD viene reindirizzato all&#39;URL personalizzato definito dalla costante `ADOBEPASS_REDIRECT_URL`.


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

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** callback attivato da AccessEnabler che informa l&#39;applicazione dello stato del flusso di autenticazione. In molti casi questo flusso può non riuscire, a causa dell’interazione dell’utente o di altri scenari imprevisti (ad esempio, problemi di connettività di rete, ecc.). Questo callback informa l’applicazione dello stato di esito positivo/negativo del flusso di autenticazione, fornendo al contempo informazioni aggiuntive sul motivo dell’errore, quando necessario.

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

* *status*: può assumere uno dei seguenti valori:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - flusso di autenticazione completato
   * `ACCESS_ENABLER_STATUS_ERROR` - flusso di autenticazione non riuscito
* *codice*: motivo errore. Se *status* è `ACCESS_ENABLER_STATUS_SUCCESS`, *code* è una stringa vuota, ovvero definita dalla costante `USER_AUTHENTICATED`. In caso di errore, questo parametro può assumere uno dei seguenti valori:
   * `USER_NOT_AUTHENTICATED_ERROR` - Utente non autenticato. In risposta alla chiamata al metodo [checkAuthentication:](#checkAuthN) quando non è presente alcun token di autenticazione valido nella cache dei token locale.
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler ha reimpostato       macchina dello stato di autenticazione dopo l&#39;applicazione di livello superiore       ha passato *null* a [`setSelectedProvider:`](#setSelProv) per interrompere il flusso di autenticazione.  Presumibilmente l’utente ha annullato il flusso di autenticazione (ovvero ha premuto il pulsante &quot;Indietro&quot;).
   * `GENERIC_AUTHENTICATION_ERROR` - Flusso di autenticazione non riuscito per motivi quali la non disponibilità della rete o l&#39;annullamento esplicito del flusso di autenticazione da parte dell&#39;utente.

**Attivato da:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[Torna all&#39;inizio...](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene utilizzato dall&#39;applicazione per determinare se l&#39;utente è già autorizzato a visualizzare risorse protette specifiche. Lo scopo principale di questo metodo consiste nel recuperare informazioni da utilizzare per decorare l&#39;interfaccia utente **(ad esempio, per indicare lo stato di accesso con le icone di blocco e sblocco).**

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

* *risorse:* array di risorse per cui controllare l&#39;autorizzazione. Ogni elemento dell’elenco deve essere una stringa che rappresenta l’ID della risorsa. Il     l’ID risorsa è soggetto alle stesse limitazioni dell’ID risorsa nella chiamata, ovvero deve essere un valore concordato tra il Programmatore e MVPD o un frammento RSS per contenuti multimediali.

**Callback attivato:** [`preauthorizedResources:`](#preauthResources)

[Torna all&#39;inizio...](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene utilizzato dall&#39;applicazione per determinare se l&#39;utente è già autorizzato a visualizzare risorse protette specifiche. Lo scopo principale di questo metodo è quello di recuperare informazioni da utilizzare per decorare l’interfaccia utente (ad esempio, per indicare lo stato di accesso con le icone di blocco e sblocco). Il parametro **cache** controlla se la cache interna viene utilizzata per la risoluzione delle risorse.

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

* *risorse:* array di risorse per cui controllare l&#39;autorizzazione. Ogni elemento dell’elenco deve essere una stringa che rappresenta l’ID della risorsa. L&#39;ID risorsa è soggetto alle stesse limitazioni dell&#39;ID risorsa nella chiamata `getAuthorization:`, ovvero deve essere un valore concordato tra il Programmatore e MVPD o un frammento RSS multimediale.
* *cache:* Valore booleano che specifica se utilizzare la cache interna per la risoluzione delle risorse. Se false, la cache verrà bypassata, dando luogo a chiamate al server ogni volta che questa API viene chiamata.

**Callback attivato:** [`preauthorizedResources:`](#preauthResources)

[Torna all&#39;inizio...](#apis)

</br>

### risorse preautorizzate: {#preauthResources}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione:** richiamata attivata da `checkPreauthorizedResources:`. Fornisce un elenco delle risorse che l’utente è già autorizzato a visualizzare.

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

* `resources`: array di risorse per le quali l&#39;utente è già autorizzato a visualizzare.

**Attivato da:** [`checkPreauthorizedResources:`](#checkPreauth)



[Torna all&#39;inizio...](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene utilizzato dall&#39;applicazione per verificare lo stato dell&#39;autorizzazione. Viene innanzitutto verificato lo stato di autenticazione. Se non è autenticato, il callback [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) viene attivato e il metodo viene chiuso. Se l’utente è autenticato, attiva anche il flusso di autorizzazione. Visualizzare i dettagli sul metodo [`getAuthorization:`](#getAuthZ).


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

* *resource*: l&#39;ID della risorsa per cui l&#39;utente richiede l&#39;autorizzazione.
* *dati*: dizionario costituito da coppie chiave-valore da inviare al servizio di pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare il SDK.

**Callback attivati:**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[Torna all&#39;inizio...](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene utilizzato dall&#39;applicazione per avviare il flusso di autorizzazione. Se l’utente non è già autenticato, avvia anche il flusso di autenticazione. Se l&#39;utente viene autenticato, AccessEnabler procede con il rilascio di richieste per il token di autorizzazione (se non è presente alcun token di autorizzazione valido nella cache dei token locale) e per il token multimediale di breve durata. Una volta ottenuto il token multimediale breve, il flusso di autorizzazione viene considerato completo. Il callback [`setToken:forResource:`](#setToken) viene attivato e il token multimediale breve viene distribuito come parametro all&#39;applicazione. Se per qualsiasi motivo l&#39;autorizzazione non riesce, viene attivato il callback [`tokenRequestFailed:forEventType:`](#tokenReqFailed) e vengono forniti il codice di errore o i dettagli.

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

* *resource*: l&#39;ID della risorsa per cui l&#39;utente richiede l&#39;autorizzazione.
* *dati*: dizionario costituito da coppie chiave-valore da inviare al servizio di pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare il SDK.

**Callback attivati:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Callback aggiuntivi attivati:**\
Questo metodo può anche attivare i seguenti callback (se viene avviato anche il flusso di autenticazione): `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>Se possibile, usa `checkAuthorization:` / `checkAuthorization:withData:` invece di `getAuthorization:` / `getAuthorization:withData:`. Il metodo `getAuthorization:` / `getAuthorization:withData:` avvierà un flusso di autenticazione completo (se l&#39;utente non è autenticato) e ciò potrebbe comportare una complicata implementazione da parte del programmatore.

[Torna all&#39;inizio...](#apis)

</br>

### `setToken:forResource:` {#setToken}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** callback attivato da AccessEnabler che informa l&#39;applicazione che il flusso di autorizzazione è stato completato correttamente. Anche il token multimediale di breve durata viene distribuito come parametro.


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
* *resource*: la risorsa per la quale è stata ottenuta l&#39;autorizzazione

**Attivato da:** [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[Torna all&#39;inizio...](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

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

* *resource*: la risorsa per la quale è stata ottenuta l&#39;autorizzazione.
* *code*: codice di errore associato allo scenario di errore. Valori possibili:
   * `USER_NOT_AUTHORIZED_ERROR` - l&#39;utente non è riuscito ad autorizzare
per la risorsa specificata
* *descrizione*: ulteriori dettagli sullo scenario di errore. Se questa stringa descrittiva non è disponibile per alcun motivo, Adobe Pass Authentication invia una stringa vuota **(&quot;)**.\
  Questa stringa può essere utilizzata da un MVPD per trasmettere messaggi di errore personalizzati o messaggi relativi alle vendite. Ad esempio, se a un abbonato viene negata l’autorizzazione per una risorsa, MVPD potrebbe inviare un messaggio del tipo: &quot;Attualmente non hai accesso a questo canale nel tuo pacchetto. Se desideri aggiornare il pacchetto, fai clic **qui**.&quot; Il messaggio viene passato dall’autenticazione di Adobe Pass tramite questo callback al programmatore, che ha la possibilità di visualizzarlo o ignorarlo. L’autenticazione di Adobe Pass può inoltre utilizzare questo parametro per fornire una notifica della condizione che potrebbe aver causato un errore. Ad esempio, &quot;Si è verificato un errore di rete durante la comunicazione con il servizio di autorizzazione del provider&quot;.

**Attivato da:** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[Torna all&#39;inizio...](#apis)

</br>

### logout {#logout}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Questo metodo viene chiamato dall&#39;applicazione per avviare il flusso di disconnessione. La disconnessione è il risultato di una serie di operazioni di reindirizzamento HTTP dovute al fatto che l’utente deve essere disconnesso sia dai server di autenticazione di Adobe Pass che dai server di MVPD. Poiché non è possibile completare il flusso con una semplice richiesta HTTP emessa dalla libreria AccessEnabler, è necessario creare un&#39;istanza di un controller `UIWebView/WKWebView or SFSafariViewController` per poter seguire le operazioni di reindirizzamento HTTP.

Il flusso di disconnessione è diverso dal flusso di autenticazione in quanto l&#39;utente non è tenuto a interagire in alcun modo con il controller `UIWebView/WKWebView or SFSafariViewController`. Pertanto, Adobe consiglia di rendere il controllo invisibile (ovvero nascosto) durante la procedura di disconnessione.

Viene utilizzato un modello simile al flusso di autenticazione. IOS AccessEnabler attiva il callback `navigateToUrl:` o `navigateToUrl:useSVC:` per creare un controller `UIWebView/WKWebView or SFSafariViewController` e caricare l&#39;URL fornito nel parametro `url` del callback. Questo è l&#39;URL dell&#39;endpoint di disconnessione sul server back-end. Per tvOS AccessEnabler non viene chiamato né il callback `navigateToUrl:` né quello `navigateToUrl:useSVC:`.

Durante l&#39;esecuzione di diversi reindirizzamenti, l&#39;applicazione deve monitorare l&#39;attività del controller `UIWebView/WKWebView or SFSafariViewController ` e rilevare il momento in cui carica un URL personalizzato specifico. Questo URL personalizzato specifico non è valido e non è destinato al controller per caricarlo effettivamente. Deve essere interpretato solo dall&#39;applicazione come un segnale che il flusso di logout è stato completato e che è sicuro chiudere il controller. Quando il controller carica questo URL personalizzato specifico, l&#39;applicazione deve chiudere il controller e chiamare il metodo API `handleExternalURL:url ` di AccessEnabler. Nel caso in cui l&#39;applicazione debba utilizzare un controller `SFSafariViewController `, l&#39;URL personalizzato specifico è definito da `application's custom scheme` (ad esempio `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), altrimenti questo URL personalizzato specifico è definito dalla costante `ADOBEPASS_REDIRECT_URL ` (ad esempio `adobepass://ios.app`).

Alla fine AccessEnabler chiamerà il callback [`setAuthenticationStatus()`](#setAuthNStatus) con il codice di stato 0, che indica il completamento del flusso di logout.

**Nota:** se l&#39;utente ha eseguito l&#39;accesso con Apple SSO, verrà attivato lo stato di VSA203. In questo caso, l&#39;utente deve essere informato di disconnettersi anche dalle impostazioni di sistema. In caso contrario, verrà eseguita una nuova autenticazione al riavvio dell&#39;applicazione.


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

**Callback attivati:** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



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

**Descrizione** callback attivato da AccessEnabler che fornisce all&#39;applicazione informazioni sul MVPD attualmente selezionato.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: informazioni sul MVPD attualmente selezionato</th>
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

* *mvpd*: oggetto contenente informazioni sul MVPD attualmente selezionato

**Attivato da:** [`getSelectedProvider`](#getSelProv)

[Torna all&#39;inizio...](#apis)

</br>

### getMetadata: {#getMeta}

**File:** AccessEnabler/headers/AccessEnabler.h

**Descrizione:** Utilizzare questo metodo per recuperare informazioni esposte come metadati dalla libreria AccessEnabler. L&#39;applicazione può accedere a questi dati fornendo un parametro di input *key* basato su dizionario.

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

* *keyDictionary*: una struttura dati del dizionario con le seguenti caratteristiche
formato:
   * Se la chiave è `METADATA_OPCODE_KEY` e il valore è `METADATA_AUTHENTICATION`, viene eseguita la query per ottenere la data di scadenza del token di autenticazione.
   * Se la chiave è `METADATA_OPCODE_KEY` e il valore è `METADATA_AUTHORIZATION` **e**\
     la chiave è `METADATA_RESOURCE_ID_KEY` e il valore è un ID risorsa particolare, quindi viene eseguita la query per ottenere la scadenza del token di autorizzazione associato alla risorsa specificata.
   * Se la chiave è `METADATA_OPCODE_KEY` e il valore è `METADATA_DEVICE_ID`, viene eseguita la query per ottenere l&#39;ID dispositivo corrente. Tieni presente che questa funzione è disabilitata per impostazione predefinita e i programmatori devono contattare Adobe per informazioni sull’abilitazione e le tariffe.
   * Se la chiave è `METADATA_OPCODE_KEY` e il valore è `METADATA_USER_META` **e** la chiave è `METADATA_USER_META_KEY` e il valore è il nome dei metadati, la query viene eseguita per i metadati utente. Elenco dei tipi di metadati utente disponibili:
      * `zip` - Elenco codici postali
      * `householdID` - Identificatore famiglia. Se un MVPD non supporta account secondari, sarà identico a `userID`.
      * `maxRating` - Una raccolta di valutazioni parentali massime per l&#39;utente
      * `userID` - Identificatore utente. Se un MVPD supporta gli account secondari e l&#39;utente non è l&#39;account principale, `userID` sarà diverso da `householdID.`
      * `channelID` - Elenco di canali che un utente può visualizzare.

  >[!NOTE]
  >
  >I metadati utente effettivi disponibili per un programmatore dipendono da ciò che viene reso disponibile da MVPD. L’elenco verrà espanso man mano che nuovi metadati saranno disponibili e aggiunti al sistema di autenticazione di Adobe Pass.

**Callback attivati:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Ulteriori informazioni:** [Metadati utente](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)

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

**Ulteriori informazioni:** [Single Sign-On iOS/tvOS](#presentTvDialog)

[Torna all&#39;inizio...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** callback attivato da AccessEnabler dopo la chiusura della finestra di dialogo SSO di Apple.

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

**Attivazione da parte di:** azione utente

**Ulteriori informazioni:** [Single Sign-On iOS/tvOS](#presentTvDialog)

[Torna all&#39;inizio...](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** callback attivato da AccessEnabler che distribuisce i metadati richiesti tramite una chiamata [`getMetadata:`](#getMeta).

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

* *metadati*: i metadati richiesti. Questo valore è `NSString` nel caso di metadati statici (TTL autenticazione, TTL autorizzazione, ID dispositivo).  Si tratta di un oggetto complesso quando si richiedono metadati specifici dell’utente. Questo oggetto complesso è in genere la rappresentazione Objective-C di un payload JSON (ad esempio, &quot;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;building&quot;: [&quot;150&quot;, &quot;320&quot;]}&quot; è tradotto in Objective-C come NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;building&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;)).   Oggetto JSON metadati di esempio:

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

* *crittografato*: valore booleano che specifica se i metadati recuperati sono crittografati o meno. Questo parametro è significativo solo per le richieste di metadati utente, non ha significato per i metadati statici (ad esempio, TTL di autenticazione) che vengono sempre ricevuti non crittografati. Se questo parametro è impostato su true, spetta al programmatore ottenere il valore dei metadati utente non crittografati eseguendo una decrittografia RSA utilizzando la chiave privata della whitelist (la stessa chiave privata utilizzata per la firma dell&#39;ID richiedente nelle chiamate [`setRequestor:setSignedRequestorId:`](#setReq) e `setRequestor:setSignedRequestorId:serviceProviders: `).

* *chiave*: chiave utilizzata per formulare la richiesta di recupero dei metadati.

* *argomenti*: lo stesso dizionario passato alla chiamata [`getMetadata:`](#getMeta). Fornito per consentire all’applicazione di far corrispondere le richieste con le risposte.

**Attivato da:** [`getMetadata:`](#getMeta)

**Ulteriori informazioni:** [Metadati utente](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)


[Torna all&#39;inizio...](#apis)

</br>

### MVPD {#mvpd}

**File:** AccessEnabler/headers/model/MVPD.h

**Descrizione** Descrive l&#39;oggetto MVPD. Può essere utilizzato per ottenere informazioni sulle proprietà di MVPD.

**Disponibilità:** v1.0+ [la proprietà boardingStatus è disponibile dalla v2.2]

**Proprietà**:

* (NSString) ID: l’ID MVPD.
* (NSString) displayName: il nome del MVPD. [Da utilizzare per la visualizzazione nel selettore]
* (NSString) logoURL: l’indirizzo del logo MVPD.
* (BOOL) enablePlatformServices - Se true, MVPD supporta i servizi SSO come [Apple SSO](#presentTvDialog).
* (NSString) boardingStatus - Può avere 3 valori:
   * nil: MVPD non supporta Apple SSO.
   * SELETTORE: il MVPD può essere visualizzato nel selettore Apple, ma il flusso di autenticazione viene eseguito da Adobe.
   * SUPPORTATO: MVPD è completamente supportato da Apple e utilizzerà il token SSO di Apple.

[Torna all&#39;inizio...](#apis)

</br>

## Tracciamento degli eventi {#tracking}

AccessEnabler attiva un callback aggiuntivo non necessariamente correlato ai flussi di adesione. L&#39;implementazione della funzione di callback [`sendTrackingData()`](#sendTracking) è facoltativa, ma consente all&#39;applicazione di tenere traccia di eventi specifici e di compilare statistiche quali il numero di tentativi di autenticazione/autorizzazione riusciti/non riusciti.

### sendTrackingData:forEventType: {#sendTracking}

**File:** AccessEnabler/headers/EntitlementDelegate.h

**Descrizione** callback attivato da AccessEnabler che segnala all&#39;applicazione l&#39;occorrenza di vari eventi, ad esempio il completamento/errore dei flussi di autenticazione/autorizzazione. Con Adobe Pass Authentication 1.6, il tipo di dispositivo, il tipo di client AccessEnabler e il sistema operativo sono segnalati da [`sendTrackingData()`](#sendTracking). Il callback [`sendTrackingData()`](#sendTracking) rimane compatibile con le versioni precedenti.

**Callback: eventi di tracciamento**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Disponibilità:** v1.0+

**Nota:** il tipo di dispositivo e il sistema operativo sono derivati dall&#39;utilizzo di una libreria Java pubblica (<http://java.net/projects/user-agent-utils>) e della stringa dell&#39;agente utente. Tieni presente che queste informazioni vengono fornite solo come metodo grezzo per suddividere le metriche operative in categorie di dispositivi, ma che Adobe non può assumersi alcuna responsabilità per risultati errati. Utilizza la nuova funzionalità di conseguenza.

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

* *event*: codice dell&#39;evento di cui viene eseguito il rilevamento. Esistono tre possibili tipi di eventi di tracciamento:
   * **authorizationDetection:** ogni volta che viene restituita una richiesta del token di autorizzazione (evento: `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** ogni volta che si verifica un controllo di autenticazione (evento: `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** quando l&#39;utente seleziona un MVPD nel modulo di selezione MVPD (evento: `TRACKING_GET_SELECTED_PROVIDER`)
* *dati*: dati aggiuntivi associati all&#39;evento segnalato. Questi dati vengono presentati sotto forma di elenco di valori.

**Attivato da:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

Istruzioni per l&#39;interpretazione dei valori nell&#39;array *data*:

* Per trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Se la richiesta del token è stata completata correttamente (true/false) e in caso di esito positivo:
   * **1** - Stringa MVPD ID
   * **2** - GUID (hash md5)
   * **3** - Token già nella cache (true/false)
   * **4** - Tipo di dispositivo
   * **5** - Tipo client AccessEnabler
   * **6** - Tipo di sistema operativo

* Per trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Se la richiesta del token è stata completata correttamente (true/false) e in caso di esito positivo:
   * **1** - ID MVPD
   * **2** - GUID (hash md5)
   * **3** - Token già nella cache (true/false)
   * **4** - Errore
   * **5** - Dettagli
   * **6** - Tipo di dispositivo
   * **7** - Tipo client AccessEnabler
   * **8** - Tipo di sistema operativo
* Per trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - ID del MVPD attualmente selezionato
   * **1** - Tipo di dispositivo
   * **2** - Tipo client AccessEnabler
   * **3** - Tipo di sistema operativo

</br>
