---
title: Riferimento API dell'SDK per Android
description: Riferimento API dell'SDK per Android
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: 854698397d9d14c1bfddcc10eecc61c7e3c32b71
workflow-type: tm+mt
source-wordcount: '4537'
ht-degree: 0%

---

# Riferimento API dell&#39;SDK per Android {#android-sdk-api-reference}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#intro}

Questo documento descrive i metodi e i callback esposti dall’SDK di Android per l’autenticazione di Adobe Pass, supportati con le versioni di autenticazione di Adobe Pass 1.7 e successive. I metodi e le funzioni di callback qui descritti sono definiti nei file di intestazione AccessEnabler.h e EntitlementDelegate.h.

Fai riferimento a [https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) per la versione più recente dell&#39;SDK di Android AccessEnabler.


**Nota:** il team di autenticazione di Adobe Pass ti incoraggia a utilizzare solo le API di autenticazione di Adobe Pass *public*:

- Le API pubbliche sono disponibili *e completamente testate* su tutti i tipi di client supportati. Per qualsiasi funzione pubblica, ci assicuriamo che ogni tipo di client abbia una versione corrispondente dei metodi associati.</span>
- Le API pubbliche devono essere il più stabili possibile, per supportare la compatibilità con le versioni precedenti e garantire che le integrazioni dei partner non si interrompano. Tuttavia, per *API non* pubbliche, ci riserviamo il diritto di modificare la loro firma in qualsiasi momento futuro. Se incontri un flusso particolare che non può essere supportato tramite una combinazione delle chiamate API di autenticazione di Adobe Pass pubbliche correnti, l’approccio migliore è quello di farcelo sapere. Tenendo conto delle tue esigenze, possiamo modificare le API pubbliche e fornire una soluzione stabile.

## API ANDROID {#api}

- [getInstance](#getInstance)
- [setOptions](#setOptions)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- preautorizza
- [checkAuthorization](#checkAuthZ)
- [getAuthorization](#getAuthZ)
- [setToken](#setToken)
- [tokenRequestFailed](#tokenRequestFailed)
- [logout](#logout)
- [getSelectedProvider](#getSelectedProvider)
- [selectedProvider](#selectedProvider)
- [getMetadata](#getMetadata)
- [setMetadataStatus](#setMetadaStatus)
- [getVersion](#getVersion)

### Factory.getInstance {#getInstance}

**Descrizione:** crea istanze dell&#39;oggetto Access Enabler. Deve essere presente una singola istanza di Access Enabler per ogni istanza dell&#39;applicazione.

| Chiamata API: costruttore |
| --- |
| **statico pubblico** AccessEnabler **getInstance**(ContextAppContext, String softwareStatement, String redirectUrl)<br>        **genera** AccessEnablerException <br><br>**statico pubblico** AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl)<br>**genera** AccessEnablerException |

**Disponibilità:** v3.1.2+

**Parametri:**

- *appContext*: contesto dell&#39;applicazione Android.
- env\_url: per eseguire test utilizzando l’ambiente di staging Adobe, env\_url può essere impostato su &quot;sp.auth-staging.adobe.com&quot;

**Obsoleto:**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**Descrizione:** stabilisce l&#39;identità del programmatore. A ciascun programmatore viene assegnato un ID univoco al momento della registrazione all’Adobe per il sistema di autenticazione di Adobe Pass. Quando si tratta di token SSO e remoti, lo stato di autenticazione può cambiare quando l&#39;applicazione è in background, setRequestor può essere richiamato nuovamente quando l&#39;applicazione viene portata in primo piano per sincronizzarsi con lo stato del sistema (recuperare un token remoto se SSO è abilitato o eliminare il token locale se nel frattempo si è verificato un logout).

La risposta del server contiene un elenco di MVPD insieme ad alcune informazioni di configurazione collegate all&#39;identità del programmatore. La risposta del server viene utilizzata internamente dal codice di Access Enabler. Solo lo stato dell’operazione (ovvero SUCCESS/FAIL) viene presentato all’applicazione tramite il callback setRequestorComplete().

Se il parametro *urls* non viene utilizzato, la chiamata di rete risultante esegue il targeting dell&#39;URL del provider di servizi predefinito: l&#39;ambiente Adobe Release/Production.

Se viene fornito un valore per il parametro *urls*, la chiamata di rete risultante eseguirà il targeting di tutti gli URL forniti nel parametro *urls*. Tutte le richieste di configurazione vengono attivate contemporaneamente in thread separati. Il primo responder ha la precedenza quando si compila l’elenco degli MVPD. Per ogni MVPD nell&#39;elenco, Access Enabler ricorda l&#39;URL del provider di servizi associato. Tutte le richieste di adesione successive vengono indirizzate all’URL associato al provider di servizi che è stato associato all’MVPD di destinazione durante la fase di configurazione.

| Chiamata API: configurazione richiedente |
| --- |
| ```public void setRequestor(String requestorId)``` |

**Disponibilità:** v3.0+

| Chiamata API: configurazione richiedente |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |
**Disponibilità:** v3.0+


**Parametri:**

- *requestorID*: ID univoco associato al programmatore. Trasmetti l’ID univoco assegnato da Adobe al sito quando ti sei registrato per la prima volta con il servizio di autenticazione di Adobe Pass.

- *signedRequestorID*: copia dell&#39;ID richiedente con firma digitale nella chiave privata. <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

- *url*: parametro facoltativo; per impostazione predefinita, viene utilizzato il provider di servizi Adobe (http://sp.auth.adobe.com/). Questo array consente di specificare gli endpoint per i servizi di autenticazione e autorizzazione forniti da Adobe (a scopo di debug possono essere utilizzate istanze diverse). È possibile utilizzare questa opzione per specificare più istanze del provider di servizi di autenticazione di Adobe Pass. In tal caso, l&#39;elenco MVPD è composto dagli endpoint di tutti i fornitori di servizi. Ogni MVPD è associato al provider di servizi più veloce, ovvero il provider che ha risposto per primo e che supporta tale MVPD.

**Callback attivati:** `setRequestorComplete()`

Obsoleto:

    public void setRequestor(String requestorId, String signedRequestorId)
    
    public void setRequestor (String requestorId, String signedRequestorId, ArrayList&lt;String> urls)

[Torna all’API di Android...](#api)

### setRequestorComplete {#setRequestorComplete}

**Descrizione:** richiamata attivata da Access Enabler che informa l&#39;applicazione che la fase di configurazione è stata completata. Questo è un segnale che l’app può iniziare a emettere richieste di adesione. L’applicazione non può inviare richieste di adesione fino al completamento della fase di configurazione.

| Callback: configurazione richiedente completata |
| --- |
| java public void setRequestorComplete(int status) |

**Disponibilità:** v1.0+

**Parametri:**

- *status*: può assumere uno dei seguenti valori:
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - fase di configurazione completata
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - fase di configurazione non riuscita
   - SDK \&lt; 3,4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - fase di configurazione completata
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - fase di configurazione non riuscita

**Attivato da:** `setRequestor()`

[Torna all’API di Android...](#api)

### setOptions {#setOptions}

**Descrizione:** configura le opzioni SDK globali. Accetta un **Map\&lt;String, String\>** come argomento. I valori della mappa verranno passati al server insieme a ogni chiamata di rete effettuata dall&#39;SDK.

I valori vengono passati al server indipendentemente dal flusso corrente (autenticazione/autorizzazione). Se desideri modificare i valori, puoi chiamare questo metodo in qualsiasi momento.

| Chiamata API: setOptions |
| --- |
| public void setOptions(HashMap&lt;String,String> options) |

**Disponibilità:** v1.9.2+

**Parametri:**

- *options*: una mappa&lt;String, String> contenente le opzioni SDK globali. Attualmente, sono disponibili le seguenti opzioni:
   - **applicationProfile** - Può essere utilizzato per creare configurazioni server basate su questo valore.
   - **ap_vi** - ID Experience Cloud (visitorID). Questo valore può essere utilizzato in seguito per i rapporti di analisi avanzata.
   - **ap_ai** - L&#39;ID Advertising
   - **informazioni_dispositivo** - Informazioni client come descritto qui: [Passaggio della connessione e dell&#39;applicazione del dispositivo per le informazioni client](/help/authentication/passing-client-information-device-connection-and-application.md).

[Torna all&#39;inizio...](#apis)


### checkAuthentication {#checkAuthN}

**Descrizione:** verifica lo stato di autenticazione. A tale scopo, cerca un token di autenticazione valido nello spazio di archiviazione dei token locale. Questo metodo non esegue chiamate di rete e si consiglia di chiamarlo sul thread principale. Viene utilizzata dall’applicazione per eseguire una query sullo stato di autenticazione dell’utente e aggiornare di conseguenza l’interfaccia utente (ad esempio, aggiornare l’interfaccia utente di accesso/disconnessione). Lo stato di autenticazione viene comunicato all&#39;applicazione tramite il callback [*setAuthenticationStatus()*](#setAuthNStatus).

Se un MVPD supporta la funzione &quot;Autenticazione per richiedente&quot;, è possibile memorizzare su un dispositivo più token di autenticazione.  Per informazioni dettagliate su questa funzione, consulta la sezione [Linee guida per il caching](#$caching) nella Panoramica tecnica di Android.

| Chiamata API: controlla lo stato di autenticazione |
| --- |
| public void checkAuthentication() |

**Disponibilità:** v1.0+

**Parametri:** Nessuno

**Callback attivati:** `setAuthenticationStatus()`

[Torna all’API di Android...](#api)


### getAuthentication {#getAuthN}

**Descrizione:** avvia il flusso di lavoro di autenticazione completo. Viene avviato controllando lo stato di autenticazione. Se non è già autenticato, viene avviato il computer dello stato del flusso di autenticazione:

- Se l&#39;ultimo tentativo di autenticazione ha avuto esito positivo, la fase di selezione MVPD viene ignorata e viene attivato il callback [*navigateToUrl()*](#navigagteToUrl). L&#39;applicazione utilizza questo callback per creare un&#39;istanza del controllo WebView che presenta all&#39;utente la pagina di accesso di MVPD.
- Se l&#39;ultimo tentativo di autenticazione non è riuscito o se l&#39;utente si è disconnesso in modo esplicito, viene attivato il callback [*displayProviderDialog()*](#displayProviderDialog). L&#39;applicazione utilizza questo callback per visualizzare l&#39;interfaccia utente di selezione MVPD. L&#39;app deve inoltre riprendere il flusso di autenticazione informando la libreria di Access Enabler della selezione MVPD dell&#39;utente tramite il metodo [setSelectedProvider()](#setSelectedProvider).

Poiché le credenziali dell&#39;utente vengono verificate nella pagina di accesso MVPD, l&#39;applicazione deve monitorare le operazioni di reindirizzamento multiple che si verificano mentre l&#39;utente si autentica nella pagina di accesso di MVPD. Quando vengono immesse le credenziali corrette, il controllo WebView viene reindirizzato a un URL personalizzato definito dalla costante *AccessEnabler.ADOBEPASS\_REDIRECT\_URL*. Questo URL non deve essere caricato da WebView. L’applicazione deve intercettare questo URL e interpretare questo evento come un segnale del completamento della fase di accesso. Dovrebbe quindi passare il controllo all&#39;Access Enabler per completare il flusso di autenticazione chiamando il metodo *getAuthenticationToken()*.

Se un MVPD supporta la funzione &quot;Autenticazione per richiedente&quot;, è possibile memorizzare su un dispositivo più token di autenticazione (uno per programmatore).  Per informazioni dettagliate su questa funzione, consulta la sezione [Linee guida per il caching](#$caching) nella Panoramica tecnica di Android.

Infine, lo stato di autenticazione viene comunicato all&#39;applicazione tramite il callback *setAuthenticationStatus()*.



| Chiamata API: avvia il flusso di autenticazione |
| --- |
| public void getAuthentication() |

**Disponibilità:** v1.0+

| Chiamata API: avvia il flusso di autenticazione |
| --- |
| public void getAuthentication(boolean forceAuthN, Map&lt;String, Object> genericData) |

**Disponibilità:** v1.8+

**Parametri:**

- *forceAuthn*: flag che specifica se avviare il flusso di autenticazione, indipendentemente dal fatto che l&#39;utente sia già autenticato o meno.
- *dati*: una mappa costituita da coppie chiave-valore da inviare al servizio pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare l’SDK.

**Callback attivati:** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Torna all’API di Android...](#api)

### displayProviderDialog {#displayProviderDialog}

**Descrizione** Il callback è stato attivato da Access Enabler per informare l&#39;applicazione che è necessario creare un&#39;istanza degli elementi dell&#39;interfaccia utente appropriati per consentire all&#39;utente di selezionare il MVPD desiderato. Il callback fornisce un elenco di oggetti MVPD con informazioni aggiuntive che possono aiutare a creare correttamente il pannello dell&#39;interfaccia utente di selezione (come l&#39;URL che punta al logo MVPD, il nome visualizzato intuitivo, ecc.)

Dopo che l&#39;utente ha selezionato il MVPD desiderato, l&#39;applicazione di livello superiore deve riprendere il flusso di autenticazione chiamando *setSelectedProvider()* e trasmettendogli l&#39;ID del MVPD corrispondente alla selezione dell&#39;utente.

>[!NOTE]
>
> Interruzione del flusso di autenticazione
> </br></br>
> Tieni presente che è un punto in cui l’utente può premere il pulsante &quot;Indietro&quot;, che equivale all’interruzione del flusso di autenticazione. In questo caso, è necessario che l&#39;applicazione chiami il metodo `setSelectedProvider()`, passando *null* come parametro, per consentire a Access Enabler di reimpostare il computer dello stato di autenticazione.

| Callback: visualizzare l&#39;interfaccia utente di selezione MVPD |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**Disponibilità:** v1.0+

**Parametri**:

- *mvpds*: elenco di oggetti MVPD contenenti informazioni relative a MVPD che l&#39;applicazione può utilizzare per creare gli elementi dell&#39;interfaccia utente di selezione MVPD.

**Attivato da:** `getAuthentication(), getAuthorization()`

[Torna all’API di Android...](#api)


### setSelectedProvider {#setSelectedProvider}

**Descrizione:** questo metodo viene chiamato dall&#39;applicazione per informare Access Enabler della selezione MVPD dell&#39;utente. È possibile utilizzare questo metodo per selezionare o modificare il provider di servizi utilizzato per l&#39;autenticazione.

Se l’MVPD selezionato è un MVPD TempPass, si autenticherà automaticamente con tale MVPD senza dover chiamare in seguito getAuthentication().

Tieni presente che questo non è possibile per il Passaggio temporaneo promozionale in cui vengono forniti parametri aggiuntivi sul metodo getAuthentication().

Quando si passa *null* come parametro, Access Enabler presume che l&#39;utente abbia annullato il flusso di autenticazione (ovvero ha premuto il pulsante &quot;Indietro&quot;) e risponde reimpostando lo stato-computer di autenticazione e chiamando il callback *setAuthenticationStatus()* con il codice di errore `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR`.

| Chiamata API: imposta il provider attualmente selezionato |
| --- |
| void pubblico setSelectedProvider(String mvpdId) |

**Disponibilità:** v1.0+

**Parametri:** Nessuno

**Callback attivati:** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Torna all’API di Android...](#api)


### navigateToUrl {#navigagteToUrl}

**Obsoleto:** A partire da Android SDK 3.0, navigateToUrl viene utilizzato solo se la scheda personalizzata di Chrome non è presente nel dispositivo

**Descrizione:** richiamata attivata da Access Enabler che informa l&#39;applicazione che è necessario presentare all&#39;utente la pagina di accesso MVPD per immettere le credenziali. Access Enabler trasmette come parametro l&#39;URL della pagina di accesso MVPD. L&#39;applicazione deve creare un&#39;istanza di un controllo WebView e indirizzarlo a questo URL. È inoltre necessario che l&#39;applicazione monitori gli URL caricati dal controllo WebView e intercetti l&#39;operazione di reindirizzamento dell&#39;URL personalizzato definito dalla costante `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)`. In seguito a questo evento, è necessario che l&#39;applicazione chiuda o nasconda il controllo WebView e restituisca il controllo alla libreria Access Enabler chiamando il metodo *getAuthenticationToken()*. L’Access Enabler completa il flusso di autenticazione recuperando il token di autenticazione dal server back-end e archiviandolo localmente nell’archivio dei token.

>[!WARNING]
>
> **Interruzione del flusso di autenticazione** <br>Tieni presente che questo è un punto in cui l&#39;utente può premere il pulsante &quot;Indietro&quot;, che equivale all&#39;interruzione del flusso di autenticazione. In questo caso, è necessario che l&#39;applicazione chiami il metodo _setSelectedProvider()_ passando _null_ come parametro e dando la possibilità a Access Enabler di reimpostare il computer dello stato di autenticazione.

| Callback: visualizzazione della pagina di accesso MVPD |
| --- |
| public void navigateToUrl(String url) |

**Disponibilità:** v1.0+

**Parametri:**

- *url*: URL che punta alla pagina di accesso di MVPD

**Attivato da:** `getAuthentication(), setSelectedProvider()`

[Torna all’API di Android...](#api)


### getAuthenticationToken {#getAuthNToken}

**Obsoleto:** A partire da Android SDK 3.0, poiché per l&#39;autenticazione viene utilizzata la scheda personalizzata Chrome, questo metodo non viene più utilizzato dall&#39;applicazione.

**Descrizione:** completa il flusso di autenticazione richiedendo il token di autenticazione al server back-end. Questo metodo deve essere chiamato dall&#39;applicazione solo in risposta all&#39;evento in cui il controllo WebView che ospita la pagina di accesso MVPD viene reindirizzato all&#39;URL personalizzato definito dalla costante `AccessEnabler.ADOBEPASS_REDIRECT_URL`.

| Chiamata API: recupera il token di autenticazione |
| --- |
| void pubblico getAuthenticationToken() |

**Disponibilità:** v1.0+

**Parametri:**

- *cookie*: cookie impostati sul dominio di destinazione (vedi l&#39;applicazione demo nell&#39;SDK per un&#39;implementazione di riferimento).

**Callback attivati:** `setAuthenticationStatus()`, `sendTrackingData()`

[Torna all’API di Android...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**Descrizione:** callback attivato dall&#39;attivatore di accesso che informa
l’applicazione dello stato del flusso di autenticazione. Ce ne sono molti
luoghi in cui questo flusso può non riuscire, come risultato del
interazioni o a causa di altri scenari imprevisti (ad es.
problemi di connettività, ecc.). Questo callback informa l&#39;applicazione di
lo stato di esito positivo/negativo del flusso di autenticazione, e
fornendo informazioni aggiuntive sul motivo dell’errore, se necessario.

| Callback: segnala lo stato del flusso di autenticazione |
| --- |
| void pubblico setAuthenticationStatus(int status, String errorCode) |

**Disponibilità:** v1.0+

**Parametri:**

- *status*: può assumere uno dei seguenti valori:
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - flusso di autenticazione completato
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - flusso di autenticazione non riuscito
- *codice*: motivo errore. Se *status* è `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`, *code* è una stringa vuota, ovvero definita dalla costante `AccessEnablerConstants.USER_AUTHENTICATED`. In caso di errore, questo parametro può assumere uno dei seguenti valori:
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` - Utente non autenticato. In risposta alla chiamata al metodo *checkAuthentication()* quando non è presente alcun token di autenticazione valido nella cache dei token locale.
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler ha reimpostato il computer dello stato di autenticazione dopo che l&#39;applicazione del livello superiore ha passato *null* a `setSelectedProvider()` per interrompere il flusso di autenticazione.  Presumibilmente l’utente ha annullato il flusso di autenticazione (ovvero ha premuto il pulsante &quot;Indietro&quot;).
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` - Flusso di autenticazione non riuscito per motivi quali la non disponibilità della rete o l&#39;annullamento esplicito del flusso di autenticazione da parte dell&#39;utente.

**Attivato da:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Torna all’API di Android...](#api)


### checkPreauthorizedResources {#checkPreauth}

>**Obsoleto:** A partire da Android SDK 3.6, l&#39;API di preautorizzazione sostituisce checkPreauthorizedResources, fornendo codici di errore estesi.

**Descrizione:** Questo metodo viene utilizzato dall&#39;applicazione per determinare se l&#39;utente è già autorizzato a visualizzare risorse protette specifiche. Lo scopo principale di questo metodo è quello di recuperare informazioni da utilizzare per decorare l’interfaccia utente (ad esempio, per indicare lo stato di accesso con le icone di blocco e sblocco).

| Chiamata API: imposta il provider attualmente selezionato |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Disponibilità:** v1.3+

**Parametri:** Il parametro `resources` è una matrice di risorse per le quali deve essere controllata l&#39;autorizzazione. Ogni elemento dell’elenco deve essere una stringa che rappresenta l’ID della risorsa. L&#39;ID risorsa è soggetto alle stesse limitazioni dell&#39;ID risorsa nella chiamata `getAuthorization()`, ovvero deve essere un valore concordato tra il Programmatore e l&#39;MVPD o un frammento RSS del supporto.

**Callback attivato:** `preauthorizedResources()`

[Torna all’API di Android...](#api)


### checkPreauthorizedResources {#checkPreauth2}

**Obsoleto:** A partire da Android SDK 3.6, l&#39;API di preautorizzazione sostituisce checkPreauthorizedResources, fornendo codici di errore estesi.

**Descrizione:** Questo metodo viene utilizzato dall&#39;applicazione per determinare se l&#39;utente è già autorizzato a visualizzare risorse protette specifiche. Lo scopo principale di questo metodo è quello di recuperare informazioni da utilizzare per decorare l’interfaccia utente (ad esempio, per indicare lo stato di accesso con le icone di blocco e sblocco).

| Chiamata API: imposta il provider attualmente selezionato |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**Disponibilità:** v3.1+

**Parametri:** Il parametro `resources` è una matrice di risorse per le quali deve essere controllata l&#39;autorizzazione. Ogni elemento dell’elenco deve essere una stringa che rappresenta l’ID della risorsa. L&#39;ID risorsa è soggetto alle stesse limitazioni dell&#39;ID risorsa nella chiamata `getAuthorization()`, ovvero deve essere un valore concordato tra il Programmatore e l&#39;MVPD o un frammento RSS del supporto.

Il parametro `cache` specifica se è possibile utilizzare o meno la risposta di preautorizzazione memorizzata nella cache. Per impostazione predefinita, la cache è true, l&#39;SDK restituirà una risposta precedentemente memorizzata nella cache, se disponibile.

**Callback attivato:** `preauthorizedResources()`

[Torna all’API di Android...](#api)

### risorse preautorizzate {#preauthResources}

**Obsoleto:** A partire da Android SDK 3.6, l&#39;API di preautorizzazione sostituisce checkPreauthorizedResources, fornendo codici di errore estesi. Il callback preauthorizedResources non verrà chiamato sulla nuova API.


**Descrizione:** richiamata attivata da checkPreauthorizedResources(). Fornisce un elenco delle risorse che l’utente è già autorizzato a visualizzare.

| Chiamata API: imposta il provider attualmente selezionato |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Disponibilità:** v1.3+

**Parametri:** Il parametro `resources` è un array di risorse per le quali l&#39;utente è già autorizzato a visualizzare.

**Attivato da:** `checkPreauthorizedResources()`

[Torna all’API di Android...](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**Descrizione:** Questo metodo viene utilizzato dall&#39;applicazione per verificare lo stato dell&#39;autorizzazione. Viene innanzitutto verificato lo stato di autenticazione. Se non è autenticato, il callback *setTokenRequestFailed()* viene attivato e il metodo viene chiuso. Se l’utente è autenticato, attiva anche il flusso di autorizzazione. Vedi i dettagli sul metodo *getAuthorization()*.

| Chiamata API: controlla lo stato di autorizzazione |
| --- |
| public void checkAuthorization(String resourceId) |

**Disponibilità:** v1.0+

| Chiamata API: controlla lo stato di autorizzazione |
| --- |
| public void checkAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Disponibilità:** v1.8+

**Parametri:**

- *resourceId*: l&#39;ID della risorsa per cui l&#39;utente richiede l&#39;autorizzazione.
- *dati*: una mappa costituita da coppie chiave-valore da inviare al servizio pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare l’SDK.

**Callback attivati:** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Torna all’API di Android...](#api)


### <span id="getAuthZ"></span>getAuthorization

**Descrizione:** Questo metodo viene utilizzato dall&#39;applicazione per avviare il flusso di autorizzazione. Se l’utente non è già autenticato, avvia anche il flusso di autenticazione. Se l’utente viene autenticato, Access Enabler procede con il rilascio di richieste per il token di autorizzazione (se non è presente alcun token di autorizzazione valido nella cache dei token locale) e per il token multimediale di breve durata. Una volta ottenuto il token multimediale breve, il flusso di autorizzazione viene considerato completo. Il callback *setToken()* viene attivato e il token multimediale breve viene consegnato come parametro all&#39;applicazione. Se per qualsiasi motivo l&#39;autorizzazione non riesce, viene attivato il callback *tokenRequestFailed()* e vengono forniti il codice di errore e i dettagli.

| Chiamata API: avvia il flusso di autorizzazione |
| --- |
| void pubblico getAuthorization(String resourceId) |

**Disponibilità:** v1.0+

| Chiamata API: avvia il flusso di autorizzazione |
| --- |
| public void getAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Disponibilità:** v1.8+

**Parametri:**

- *resourceId*: l&#39;ID della risorsa per cui l&#39;utente richiede l&#39;autorizzazione.
- *dati*: una mappa costituita da coppie chiave-valore da inviare al servizio pass Pay-TV. Adobe può utilizzare questi dati per abilitare funzionalità future senza modificare l’SDK.

**Callback attivati:** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **Callback aggiuntivi attivati** <br> Questo metodo può anche attivare i seguenti callback (se è stato avviato anche il flusso di autenticazione): *setAuthenticationStatus()*, *displayProviderDialog()*, *navigateToUrl()*

**NOTA: utilizzare checkAuthorization() invece di getAuthorization() quando possibile. Il metodo getAuthorization() avvierà un flusso di autenticazione completo (se l&#39;utente non è autenticato) e ciò potrebbe comportare una complicata implementazione da parte del programmatore.**

[Torna all’API di Android...](#api)


### setToken {#setToken}

**Descrizione:** richiamata attivata da Access Enabler che informa l&#39;applicazione che il flusso di autorizzazione è stato completato correttamente. Anche il token multimediale di breve durata viene distribuito come parametro.

| Callback: flusso di autorizzazione completato |
| --- |
| void setToken(Token stringa, ResourceId stringa) pubblico |

**Disponibilità:** v1.0+

**Parametri:**

- *token*: token multimediale di breve durata
- *resourceId*: la risorsa per la quale è stata ottenuta l&#39;autorizzazione

**Attivato da:** `checkAuthorization()`, `getAuthorization()`


[Torna all’API di Android...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**Descrizione:** callback attivato dall&#39;Access Enabler che informa l&#39;applicazione di livello superiore che il flusso di autorizzazione non è riuscito.

| Callback: flusso di autorizzazione non riuscito |
| --- |
| public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription) |

**Disponibilità:** v1.0+

**Parametri:**

- *resourceId*: la risorsa per la quale è stata ottenuta l&#39;autorizzazione
- *errorCode*: codice di errore associato allo scenario di errore. Valori possibili:
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` - L&#39;utente non è stato in grado di autorizzare per la risorsa specificata
- *errorDescription*: ulteriori dettagli sullo scenario di errore. Se questa stringa descrittiva non è disponibile per alcun motivo, Adobe Pass Authentication invia una stringa vuota **(&quot;)**.

  Questa stringa può essere utilizzata da un MVPD per trasmettere messaggi di errore personalizzati o messaggi relativi alle vendite. Ad esempio, se a un abbonato viene negata l’autorizzazione per una risorsa, l’MVPD potrebbe inviare un messaggio del tipo: &quot;Attualmente non hai accesso a questo canale nel pacchetto. Se desideri aggiornare il pacchetto, fai clic qui.&quot; Il messaggio viene passato dall’autenticazione di Adobe Pass tramite questo callback al programmatore, che ha la possibilità di visualizzarlo o ignorarlo. L’autenticazione di Adobe Pass può inoltre utilizzare questo parametro per fornire una notifica della condizione che potrebbe aver causato un errore. Ad esempio, &quot;Si è verificato un errore di rete durante la comunicazione con il servizio di autorizzazione del provider&quot;.

**Attivato da:** `checkAuthorization(), getAuthorization()`

[Torna all’API di Android...](#api)

### logout {#logout}

**Descrizione:** Utilizzare questo metodo per avviare il flusso di disconnessione. La disconnessione è il risultato di una serie di operazioni di reindirizzamento HTTP dovute al fatto che l’utente deve essere disconnesso sia dai server di autenticazione di Adobe Pass che dai server MVPD. Di conseguenza, questo flusso non può essere completato con una semplice richiesta HTTP emessa dalla libreria di Access Enabler. L&#39;SDK utilizza schede personalizzate Chrome per eseguire le operazioni di reindirizzamento HTTP. Questo flusso sarà visibile all’utente e al termine verrà chiuso

| Chiamata API: avvia il flusso di logout |
| --- |
| public void logout() |

**Disponibilità:** v1.0+

**Parametri:** Nessuno

**Callback attivati:**

- `navigateToUrl()` per la versione SDK precedente alla 3.0
- `setAuthenticationStatus()` per SDK versione > 3.0


[Torna all’API di Android...](#api)


### getSelectedProvider {#getSelectedProvider}

**Descrizione:** Utilizzare questo metodo per determinare il provider attualmente selezionato.

| Chiamata API: determina il MVPD attualmente selezionato |
| --- |
| void pubblico getSelectedProvider() |

**Disponibilità:** v1.0+

**Parametri:** Nessuno

**Callback attivati:** `selectedProvider()`

[Torna all’API di Android...](#api)


### <span id="selectedProvider"></span>selectedProvider

**Descrizione:** callback attivato dall&#39;Access Enabler che fornisce all&#39;applicazione informazioni sull&#39;MVPD attualmente selezionato.

| Callback: informazioni sull&#39;MVPD attualmente selezionato |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**Disponibilità:** v1.0+

**Parametri:**

- *mvpd*: oggetto contenente informazioni sull&#39;MVPD attualmente selezionato

**Attivato da:** `getSelectedProvider()`

[Torna all’API di Android...](#api)


### getMetadata {#getMetadata}

**Descrizione:** Utilizzare questo metodo per recuperare informazioni esposte come metadati dalla libreria di Access Enabler. L&#39;applicazione può accedere a queste informazioni fornendo un oggetto MetadataKey composito.

| Chiamata API: query di AccessEnabler per i metadati |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**Disponibilità:** v1.0+

Esistono due tipi di metadati disponibili per i programmatori:

- Metadati statici (TTL del token di autenticazione, TTL del token di autorizzazione e ID dispositivo)
- Metadati utente (informazioni specifiche dell’utente, come ID utente e codice postale; trasmessi da un MVPD al dispositivo di un utente durante i flussi di autenticazione e/o autorizzazione)

**Parametri:**

- *metadataKey*: struttura di dati che incapsula una variabile chiave e args, con il seguente significato:
   - Se la chiave è `METADATA_KEY_USER_META` e l&#39;argomento contiene un oggetto SerializableNameValuePair con nome = `METADATA_ARG_USER_META` e valore = `[metadata_name]`, viene eseguita la query per i metadati utente. L’elenco corrente dei tipi di metadati utente disponibili:
      - `zip` - Codice postale

      - `householdID` - Identificatore famiglia. Se un MVPD non supporta gli account secondari, sarà identico a `userID`.

      - `maxRating` - Valutazione genitori massima per l&#39;utente

      - `userID` - Identificatore utente. Se un MVPD supporta gli account secondari e l&#39;utente non è l&#39;account principale, `userID` sarà diverso da `householdID`.

      - `channelID` - Elenco dei canali che l&#39;utente ha il diritto di visualizzare
   - Se la chiave è `METADATA_KEY_DEVICE_ID`, viene eseguita la query per ottenere l&#39;ID dispositivo corrente. Tieni presente che questa funzione è disabilitata per impostazione predefinita e i programmatori devono contattare l’Adobe per informazioni sull’abilitazione e sulle tariffe.
   - Se la chiave è `METADATA_KEY_TTL_AUTHZ` e l&#39;argomento contiene un oggetto SerializableNameValuePair con nome = `METADATA_ARG_RESOURCE_ID` e valore = `[resource_id]`, viene eseguita la query per ottenere la scadenza del token di autorizzazione associato alla risorsa specificata.
   - Se la chiave è `METADATA_KEY_TTL_AUTHN`, viene eseguita la query per ottenere l&#39;ora di scadenza del token di autenticazione.



>[!NOTE]
>
>Per l&#39;SDK 3.4.0, le costanti: `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` sono disponibili da com.adobe.adobepass.accessenabler.api.profile.UserProfileService.



>[!NOTE]
>
>I metadati utente effettivi disponibili per un programmatore dipendono da ciò che viene reso disponibile da un MVPD.  Questo elenco verrà ulteriormente esteso man mano che nuovi metadati saranno disponibili e aggiunti al sistema di autenticazione di Adobe Pass.

**Callback attivati:** [`setMetadataStatus()`](#setMetadaStatus)

**Ulteriori informazioni:** [Metadati utente](/help/authentication/user-metadata-feature.md)

[Torna all’API di Android...](#api)

### setMetadataStatus {#setMetadaStatus}

**Descrizione:** callback attivato dall&#39;Access Enabler che distribuisce i metadati richiesti tramite una chiamata *getMetadata()*.

| Callback: risultato della richiesta di recupero dei metadati |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Disponibilità:** v1.0+

**Parametri:**

- *chiave*: l&#39;oggetto MetadataKey contenente la chiave per la quale viene richiesto il valore dei metadati e i parametri associati (vedere l&#39;applicazione demo per un&#39;implementazione di riferimento).
- *result*: oggetto composito contenente i metadati richiesti. L’oggetto dispone dei seguenti campi:
   - *simpleResult*: valore String che rappresenta il valore dei metadati quando è stata effettuata la richiesta per Authentication TTL, Authorization TTL o Device ID. Questo valore è nullo se la richiesta è stata effettuata per i metadati utente.

   - *userMetadataResult*: oggetto contenente la rappresentazione Java di un payload di metadati utente JSON.\
     Ad esempio:

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
```

è tradotto in Java come:

```java
          Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
```

**La struttura effettiva degli oggetti metadati utente è simile alla seguente:**

```json
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
          }
```

Questo valore è nullo quando la richiesta è stata effettuata per metadati semplici (TTL di autenticazione, TTL di autorizzazione o ID dispositivo).

- *crittografato*: valore booleano che specifica se i metadati recuperati sono crittografati o meno. Questo parametro è significativo solo per le richieste di metadati utente, non ha significato per i metadati statici (ad esempio, TTL di autenticazione) che vengono sempre ricevuti non crittografati. Se questo parametro è impostato su True, spetta al programmatore ottenere il valore dei metadati utente non crittografati eseguendo una decrittografia RSA utilizzando la chiave privata dell&#39;inserimento nella whitelist (la stessa chiave privata utilizzata per la firma dell&#39;ID richiedente nella chiamata [`setRequestor`](#setRequestor)).

**Attivato da:** [`getMetadata()`](#getMetadata)

**Ulteriori informazioni:** [Metadati utente](/help/authentication/user-metadata-feature.md)


[Torna all’API di Android...](#api)


### getVersion {#getVersion}

**Descrizione:** Questo metodo può essere utilizzato per recuperare la versione della libreria AccessEnabler.

| Chiamata API: ottieni versione di AccessEnabler |
| --- |
| ```public static String getVersion()``` |


[Torna all’API di Android...](#api)

</br>

## Tracciamento degli eventi {#tracking}

L’Access Enabler attiva un callback aggiuntivo che non è necessariamente correlato ai flussi di adesione. L&#39;implementazione della funzione di callback di tracciamento degli eventi denominata *sendTrackingData()* è facoltativa, ma consente all&#39;applicazione di tenere traccia di eventi specifici e di compilare statistiche quali il numero di tentativi di autenticazione/autorizzazione riusciti/non riusciti. Di seguito è riportata la specifica per il callback *sendTrackingData()*:


### sendTrackingData {#sendTrackingData}

**Descrizione:** callback attivato dal servizio Access Enabler che segnala all&#39;applicazione la presenza di vari eventi, ad esempio il completamento/errore dei flussi di autenticazione/autorizzazione. Il tipo di dispositivo, il tipo di client Access Enabler e il sistema operativo vengono inoltre segnalati da sendTrackingData().

>[!WARNING]
>
> Il tipo di dispositivo e il sistema operativo sono derivati dall&#39;utilizzo di una libreria Java pubblica ([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils)) e della stringa dell&#39;agente utente. Tieni presente che queste informazioni vengono fornite solo come metodo grezzo per suddividere le metriche operative in categorie di dispositivi, ma che l’Adobe non può assumersi alcuna responsabilità per risultati errati. Utilizza la nuova funzionalità di conseguenza.


- Valori possibili per il tipo di dispositivo:
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`


- Valori possibili per il tipo di client Access Enabler:
   - `flash`
   - `html5`
   - `ios`
   - `android`

</br>

| Callback: eventi di tracciamento |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**Disponibilità:** v1.0+

**Parametri:**

- *event*: evento di cui viene tenuta traccia. Esistono tre possibili tipi di eventi di tracciamento:
   - **authorizationDetection:** ogni volta che viene restituita una richiesta del token di autorizzazione (tipo di evento: `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** ogni volta che si verifica un controllo di autenticazione (tipo di evento: `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** quando l&#39;utente seleziona un MVPD nel modulo di selezione MVPD (il tipo di evento è `EVENT_MVPD_SELECTION`)
- *dati*: dati aggiuntivi associati all&#39;evento segnalato. Questi dati vengono presentati sotto forma di elenco di valori.

Di seguito sono riportate le istruzioni per l&#39;interpretazione dei valori in *data*
array:

- Per il tipo di evento *`EVENT_AUTHN_DETECTION`:*
   - **0** - Indica se la richiesta del token è riuscita (true/false) e se quanto sopra è vero:
   - **1** - Stringa ID MVPD
   - **2** - GUID (hash md5)
   - **3** - Token già nella cache (true/false)
   - **4** - Tipo di dispositivo
   - **5** - Tipo di client Access Enabler
   - **6** - Tipo di sistema operativo

- Per il tipo di evento `EVENT_AUTHZ_DETECTION`
   - **0** - Se la richiesta del token è stata completata correttamente (true/false) e in caso di esito positivo:
   - **1** - ID MVPD
   - **2** - GUID (hash md5)
   - **3** - Token già nella cache (true/false)
   - **4** - Errore
   - **5** - Dettagli
   - **6** - Tipo di dispositivo
   - **7** - Tipo client Access Enabler
   - **8** - Tipo di sistema operativo

- Per il tipo di evento `EVENT_MVPD_SELECTION`
   - **0** - ID del MVPD attualmente selezionato
   - **1** - Tipo di dispositivo
   - **2** - Tipo di client Access Enabler
   - **3** - Tipo di sistema operativo

**Attivato da:** `checkAuthentication()`, `getAuthentication()`, `checkAuthorization()`, `getAuthorization()`, `setSelectedProvider()`

[Torna all’API di Android...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
