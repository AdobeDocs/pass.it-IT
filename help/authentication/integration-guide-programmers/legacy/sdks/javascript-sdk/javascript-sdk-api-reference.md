---
title: Riferimento API di JavaScript SDK
description: Riferimento API di JavaScript SDK
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '2861'
ht-degree: 0%

---

# Riferimento API di JavaScript SDK (legacy) {#javascript-sdk-api-reference}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Riferimento API {#api-reference}

Queste funzioni avviano richieste di interazione con un MVPD. Tutte le chiamate sono asincrone. È necessario implementare [callback](#callbacks) per gestire le risposte:

- [setRequestor()](#setReq)
- [getAuthorization()](#getAuthZ)
- [getAuthentication()](#getAuthN)
- [checkAuthentication()](#checkAuthN)
- [checkAuthorization()](#checkAuthZ)
- [checkPreauthorizedResources()](#checkPreauthRes)
- [getMetadata()](#getMeta)
- [setSelectedProvider()](#setSelProv)
- [logout()](#logout)


## setRequestor (inRequestorID, endpoint, opzioni){#setrequestor(inRequestorID,endpoints,options)}

**Descrizione:** identifica il sito da cui provengono le richieste.  Devi effettuare questa chiamata prima di qualsiasi altra chiamata API in una sessione di comunicazione.

**Parametri:**

- Adobe *inRequestorID* - Identificatore univoco assegnato al sito di origine durante la registrazione.

- *endpoint* - Questo parametro è facoltativo. Può corrispondere a uno dei seguenti valori:

   - Array che consente di specificare gli endpoint per i servizi di autenticazione e autorizzazione forniti da Adobe (a scopo di debug possono essere utilizzate istanze diverse). Nel caso in cui vengano forniti più URL, l’elenco MVPD è composto dagli endpoint di tutti i provider di servizi. Ogni MVPD è associato al provider di servizi più veloce, ovvero il provider che ha risposto per primo e che supporta tale MVPD. Per impostazione predefinita (se non viene specificato alcun valore), viene utilizzato il provider di servizi Adobe (<http://sp.auth.adobe.com/>).

  Esempio:
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *opzioni* - Oggetto JSON contenente il valore ID applicazione, le impostazioni senza aggiornamento del valore ID visitatore (disconnessione in background) e le impostazioni MVPD (iFrame). Tutti i valori sono facoltativi.
   1. Se specificato, l’ID visitatore Experience Cloud viene riportato su tutte le chiamate di rete eseguite dalla libreria. Il valore può essere successivamente utilizzato per i rapporti di analisi avanzati.
   2. Se l&#39;identificatore univoco dell&#39;applicazione è specificato -`applicationId` - il valore verrà aggiunto a tutte le chiamate successive effettuate dall&#39;applicazione come parte dell&#39;intestazione HTTP X-Device-Info. Questo valore può essere recuperato in seguito dai report [ESM](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) utilizzando la query corretta.

  **Nota:** tutte le chiavi JSON fanno distinzione tra maiuscole e minuscole.

  Esempio:

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- Il programmatore può ignorare le impostazioni di MVPD configurate nell&#39;autenticazione di Adobe Pass specificando se un iFrame è necessario o meno per l&#39;accesso (*iFrameRequired* key) e le dimensioni iFrame (*iFrameWidth* e *iFrameHeight* keys). L’oggetto JSON presenta il seguente modello:

```JSON
    {  
       "visitorID": <string>,
       "backgroundLogin": <boolean>,
       "backgroundLogout": <boolean>,
       "mvpdConfig":{  
          "MVPD_ID_1":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          },
          ...
          "MVPD_ID_N":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          }
       }
    }
```


Tutte le chiavi di primo livello nel modello precedente sono facoltative e hanno valori predefiniti (*backgroundLogin*, *backgroundLogut* sono false per impostazione predefinita e mvpdConfig è null, il che significa che nessuna impostazione di MVPD viene ignorata).


- **Nota**: specificando valori o tipi non validi per i parametri di cui sopra si verificherà un comportamento non definito.



Di seguito è riportata una configurazione di esempio per lo scenario seguente: attivazione dell’accesso senza aggiornamento e della disconnessione, modifica di MVPD1 con accesso con reindirizzamento a pagina intera (non iFrame) e accesso di MVPD2 con iFrame con larghezza=500 e altezza=300:

```JSON
    {  
       "backgroundLogin": true,
       "backgroundLogout": true,
       "mvpdConfig":{  
          "MVPD1":{  
             "iFrameRequired": false
          },
          "MVPD2":{  
             "iFrameRequired": true,
             "iFrameWidth": 500,
             "iFrameHeight": 300
          }
       }
    }
```


**Callback attivati:** [setConfig()](#setconfigconfigxml-setconfigconfigxml)
</br>

[Torna all&#39;inizio](#top)

</br>

## getAuthorization(inResourceID, redirect_url) {#getauthorization(inresourceid,redirect_url)}

**Descrizione:** richiede l&#39;autorizzazione per la risorsa specificata. Ogni volta che un cliente tenta di accedere a una risorsa autorizzabile, chiama questa funzione per ottenere un token di autorizzazione di breve durata da Access Enabler. Gli ID risorsa sono concordati con l&#39;autorizzazione fornita da MVPD.

Utilizza il token di autenticazione nella cache per il cliente corrente. Se non viene trovato alcun token di questo tipo, avvia prima il processo di autenticazione, quindi continua con l’autorizzazione.

**Parametri:**

- `inResourceID` - ID della risorsa per la quale l&#39;utente richiede l&#39;autorizzazione.
- `redirect_url` - Fornire facoltativamente un URL di reindirizzamento, in modo che il processo di autorizzazione di MVPD restituisca l&#39;utente alla pagina specificata anziché alla pagina da cui è stata avviata l&#39;autorizzazione.


**Callback attivati:** [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) al completamento, [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) al fallimento

>[!CAUTION]
>
>Se possibile, utilizza checkAuthorization() invece di getAuthorization(). Il metodo getAuthorization() avvierà un flusso di autenticazione completo (se l’utente non è autenticato) e ciò potrebbe comportare una complicata implementazione da parte del programmatore.

</b>

[Torna all&#39;inizio](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**Descrizione:** richiede l&#39;autenticazione per il cliente corrente. Generalmente chiamato in risposta a un clic su un pulsante di accesso. Verifica la presenza di un token di autenticazione memorizzato nella cache per il cliente corrente. Se tale token non viene trovato, avvia il processo di autenticazione. Viene visualizzata la finestra di dialogo di selezione del provider predefinita o personalizzata, quindi viene utilizzato il provider selezionato per reindirizzare all&#39;interfaccia di accesso di MVPD.

In caso di esito positivo, crea e memorizza un token di autenticazione per l’utente. Se l&#39;autenticazione non riesce, il provider restituisce un messaggio di errore appropriato al callback [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode).

**Parametri:**

- redirect_url: se necessario, fornisci un URL di reindirizzamento, in modo che il processo di autenticazione di MVPD restituisca l’utente a quella pagina invece che alla pagina da cui è stata avviata l’autenticazione.

**Callback attivati:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Torna all&#39;inizio](#top)

</br>

## checkAuthN {#checkauthn}

**Descrizione:** verifica lo stato di autenticazione corrente per il cliente corrente.  Non associato ad alcuna interfaccia utente.

**Callback attivati:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

[Torna all&#39;inizio](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**Descrizione:** Questo metodo viene utilizzato dall&#39;applicazione per controllare lo stato di autorizzazione per il cliente corrente e la risorsa specificata. Viene innanzitutto verificato lo stato di autenticazione. Se non viene autenticato, il callback tokenRequestFailed() viene attivato e il metodo viene chiuso. Se l’utente è autenticato, attiva anche il flusso di autorizzazione. Vedi i dettagli sul metodo [getAuthorization()](#getAuthZ.

>[!TIP]
>
> **Utilizzo delle funzioni di verifica dello stato** Non è necessario controllare lo stato dell&#39;autenticazione o dell&#39;autorizzazione prima di richiedere l&#39;autorizzazione. È possibile chiamare queste funzioni, ad esempio, per aggiornare la visualizzazione dello stato. Non utilizzarli quando è necessario che l’utente interagisca.

**Parametri:**

- `inResourceID` - ID della risorsa per la quale l&#39;utente richiede l&#39;autorizzazione.


**Callback attivati:**
[setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken), [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata), [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources(resources) {#checkPreauthorizedResources(resources)}

**Descrizione:** richiede lo stato di autorizzazione &quot;verifica preliminare&quot; per un elenco di
risorse.

**Parametri:**

- *resources*: il parametro resources è una matrice di risorse per le quali deve essere controllata l&#39;autorizzazione. Ogni elemento dell’elenco deve essere una stringa che rappresenta l’ID della risorsa. L&#39;ID risorsa è soggetto alle stesse limitazioni dell&#39;ID risorsa nella chiamata `getAuthorization()`, ovvero è un valore concordato tra il Programmatore e MVPD o un frammento RSS del supporto.

</br>

## checkPreauthorizedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

Questa variante API è disponibile a partire da JS SDK versione 4.0


**Parametri:**

- *resources*: il parametro resources è una matrice di risorse per le quali deve essere controllata l&#39;autorizzazione. Ogni elemento dell’elenco deve essere una stringa che rappresenta l’ID della risorsa. L&#39;ID risorsa è soggetto alle stesse limitazioni dell&#39;ID risorsa nella chiamata `getAuthorization()`, ovvero è un valore concordato tra il Programmatore e MVPD o un frammento RSS del supporto.

- *cache*: per utilizzare la cache interna durante la verifica della presenza di risorse preautorizzate. Parametro facoltativo. Impostazione predefinita: **true**. Se true, il comportamento è identico a quello dell’API precedente, il che significa che le chiamate successive a questa funzione utilizzeranno una cache interna per risolvere le risorse preautorizzate. Se si passa **false** per questo parametro, la cache interna verrà disabilitata e verrà generata una chiamata al server ogni volta che viene chiamata l&#39;API **checkPreauthorizedResources**.

**Callback attivati:** [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[Torna all&#39;inizio](#top)
</br>

## getMetadata(Key) {#getMetadata}

**Descrizione:** recupera le informazioni esposte come metadati dalla libreria di Access Enabler.

Esistono due tipi di metadati:

- **Statico** (TTL token di autenticazione, TTL token di autorizzazione e ID dispositivo)
- **Metadati utente** (include informazioni specifiche dell&#39;utente passate dal MVPD al dispositivo dell&#39;utente durante i flussi di autenticazione e/o autorizzazione)

**Ulteriori informazioni:** [Metadati utente](#UserMetadata)

**Parametri:**

- *chiave*: un ID che specifica i metadati richiesti:
   - Se la chiave è `"TTL_AUTHN",`, viene eseguita la query per ottenere l&#39;ora di scadenza del token di autenticazione.

   - Se la chiave è `"TTL_AUTHZ"` e i parametri sono una matrice contenente l&#39;ID risorsa come stringa, la query viene eseguita per ottenere la scadenza del token di autorizzazione associato alla risorsa specificata.

   - Se la chiave è `"DEVICEID"`, viene eseguita la query per ottenere l&#39;ID dispositivo corrente. Tieni presente che questa funzione è disabilitata per impostazione predefinita e i programmatori devono contattare Adobe per informazioni sull’abilitazione e le tariffe.

   - Se key è incluso nel seguente elenco di tipi di metadati utente, un oggetto JSON contenente i metadati utente corrispondenti viene inviato alla funzione di callback [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata):

   - `"zip"` - Codice postale

   - `"encryptedZip"` - Codice postale crittografato

   - `"householdID"` - Identificatore famiglia. Nel caso in cui un MVPD non supporti account secondari, questo sarà identico a userID.

   - `"maxRating"` - Valutazione genitori massima per l&#39;utente

   - `"userID"` - Identificatore utente. Nel caso in cui un MVPD supporti account secondari e l’utente non sia l’account principale, userID sarà diverso da familyID.

   - `"channelID"` - Elenco dei canali che l&#39;utente può visualizzare

   - `"is_hoh"` - Flag che identifica se un utente è a capo del nucleo familiare

   - `"encryptedZip"` - Codice postale crittografato

   - `"typeID"` - Flag che identifica se l&#39;account utente è primario/secondario

   - `"primaryOID"` - Identificatore famiglia

   - `"postalCode"` - Simile al CAP

   - `"acctID"` - ID account

   - `"acctParentID"` - ID padre account

  **Nota**: i metadati utente effettivi disponibili per un programmatore dipendono da ciò che un MVPD rende disponibile.  Per l&#39;elenco corrente dei metadati utente disponibili, vedere [Metadati utente](#UserMetadata).


Ad esempio:

```JSON
    // Assume that a reference to the AccessEnabler has been previously 
    // obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
        if (status ==  1) {
            //user is authenticated, request metadata
            ae.getMetadata("zip");
            ae.getMetadata("maxRating");
        } else {
            ...
      }
    }
```


**Callback attivati:** [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[Torna all&#39;inizio](#top)

</br>


## setSelectedProvider(providerid) {#setSelectedProvider}

**Descrizione:** Chiama questa funzione quando l&#39;utente ha selezionato un MVPD dall&#39;interfaccia utente di selezione del provider per inviare la selezione del provider all&#39;Access Enabler o chiama questa funzione con un parametro null nel caso in cui l&#39;utente abbia rifiutato l&#39;interfaccia utente di selezione del provider senza selezionare un provider.

**Callback
attivato:**[ setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Torna all&#39;inizio](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**Descrizione:** recupera i risultati della selezione del cliente nella finestra di dialogo di selezione del provider. Può essere utilizzato in qualsiasi momento dopo il controllo di autenticazione iniziale.

Questa funzione è asincrona e restituisce il risultato alla funzione di callback `selectedProvider()`.

- **MVPD** Il MVPD attualmente selezionato o null se non è stato selezionato alcun MVPD.
- **AE_State** Il risultato dell&#39;autenticazione per il cliente corrente è &quot;Nuovo utente&quot;, &quot;Utente non autenticato&quot; o &quot;Utente autenticato&quot;

**Callback attivati:** [selectedProvider()](#getselectedprovider-getselectedprovider)

</br>

[Torna all&#39;inizio](#top)

</br>

## logout {#logout}

**Descrizione:** Disconnette il cliente corrente, cancellando tutte le informazioni di autenticazione e autorizzazione per tale utente. Elimina tutti i token authN e authZ dal sistema del cliente.

**Callback attivati:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
</br>

[Torna all&#39;inizio](#top)

</br>

## Definizione callback {#calllback-definitions}

Devi implementare questi callback per gestire le risposte alle chiamate di richiesta asincrone:

- [entitlementLoaded()](#entitlementloaded-entitlementloaded)
- [setConfig()](#setconfigconfigxml-setconfigconfigxml)
- [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders)
- [createIFrame()](#createiframe-createiframeinwidthminheight)
- [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
- [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)
- [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)
- [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)
- [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)
- [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)
- [selectedProvider()](#selectedproviderresult-selectedprovider)

</br>

## entitlementLoaded() {#entitlementLoaded}

**Descrizione:** attivato quando l&#39;attivatore di accesso ha completato l&#39;inizializzazione ed è pronto per ricevere le richieste. Implementa questo callback per sapere quando avviare la comunicazione con l’API di Access Enabler.
</br>

[Torna all&#39;inizio](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**Descrizione:** implementa questo callback per ricevere le informazioni di configurazione e l&#39;elenco di MVPD.

**Parametri:**

- *configXML*: oggetto xml contenente la configurazione per il RICHIEDENTE corrente, incluso l&#39;elenco di MVPD.


**Attivato da:** [setRequestor()](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[Torna all&#39;inizio](#top)

</br>

## displayProviderDialog(providers) {#displayproviderdialog(providers)}

**Descrizione:** implementa questo callback per richiamare l&#39;interfaccia utente personalizzata per la selezione del provider. La finestra di dialogo deve utilizzare il nome visualizzato (e il logo opzionale) per fornire le scelte del cliente. Quando il cliente ha effettuato una scelta e ha chiuso la finestra di dialogo, invia l&#39;ID associato per il provider scelto nella chiamata a *setSelectedProvider()*.

**Parametri:**

- *provider* - Matrice di oggetti che rappresenta gli MVPD richiesti:

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**Attivato da:** [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[Torna all&#39;inizio](#top)

</br>

## createIFrame(inWidth, inHeight) {#createIFrame(inWidth,inHeight)}

**Descrizione:** implementa questo callback se l&#39;utente ha selezionato un MVPD che richiede un iFrame in cui visualizzare l&#39;interfaccia utente della pagina di accesso per l&#39;autenticazione.

**Attivato da:**[ setSelectedProvider()](#setselectedproviderproviderid-setselectedprovider)

</br> [Torna all&#39;inizio](#top)

</br>

## setAuthenticationStatus(isAuthenticated, errorCode) {#set-authn-status-isauthn-error}

**Descrizione:** implementare questo callback per ricevere lo stato di autenticazione (1=autenticato o 0=non autenticato) e un messaggio di errore descrittivo in caso di errore durante il tentativo di determinare lo stato di autenticazione (stringa vuota al completamento del controllo).

>[!NOTE]
> 
>Se utilizzi il sistema corrente [Segnalazione avanzata errori](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md), puoi ignorare il parametro errorCode inviato a questa funzione.  Tuttavia, i flag isAuthenticated sono ancora utili per monitorare lo stato di autenticazione di un utente nel flusso di adesione


**Parametri:**

- *isAuthenticated* - Fornisce lo stato di autenticazione: 1 (autenticato) o 0 (non autenticato).
- *errorCode* - Qualsiasi errore che si è verificato durante la determinazione dello stato di autenticazione. Stringa vuota se non presente.


**Attivato da:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[Torna all&#39;inizio](#top)

</br>

## sendTrackingData(trackingEventType, trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>Il tipo di dispositivo e il sistema operativo sono derivati dall&#39;utilizzo di una libreria Java pubblica (<http://java.net/projects/user-agent-utils>) e della stringa dell&#39;agente utente. Tieni presente che queste informazioni vengono fornite solo come metodo grezzo per suddividere le metriche operative in categorie di dispositivi, ma che Adobe non può assumersi alcuna responsabilità per risultati errati. Utilizza la nuova funzionalità di conseguenza.

**Descrizione:** implementa questo callback per ricevere i dati di tracciamento quando si verificano eventi specifici. Puoi utilizzarlo, ad esempio, per tenere traccia del numero di utenti che hanno effettuato l’accesso con le stesse credenziali. Il tracciamento non è attualmente configurabile. Con Adobe Pass Authentication 1.6, `sendTrackingData()` segnala inoltre informazioni sul dispositivo, sul client Access Enabler e sul tipo di sistema operativo. Il callback `sendTrackingData()` rimane compatibile con le versioni precedenti.

- Valori possibili per il tipo di dispositivo:
   - computer
   - tablet
   - mobile
   - gameconsole
   - sconosciuto

- Valori possibili per il tipo di client Access Enabler:
   - html5
   - ios
   - androide


Passa il tipo di evento e una matrice di informazioni associate. I tipi di evento sono:

| mvpdSelection | L’utente ha selezionato un MVPD in una finestra di dialogo per selezione del provider. |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | Controllo di autenticazione completato. |
| authorizationDetection | Richiesta di autorizzazione completata. |

</br>
I dati sono specifici per ogni tipo di evento:
</br>

| Tipo evento (stringa) | Dati (array) |
|:--- | :--- |
| mvpdSelection | 0: MVPD selezionato |
|  | 1: Tipo di dispositivo |
|  | 2: tipo di client Access Enabler |
|  | 3: Sistema operativo |
| authenticationDetection | 0: indica se la richiesta del token è riuscita (true/false) |
|  | 1: MVPD ID |
|  | 2: GUID |
|  | 3: token già nella cache (true/false) |
|  | 4: Tipo di dispositivo |
|  | 5: tipo di client Access Enabler |
|  | 6: Sistema operativo |
| authorizationDetection | 0: indica se la richiesta del token è riuscita (true/false) |
|  | 1: MVPD ID |
|  | 2: GUID |
|  | 3: token già nella cache (true/false) |
|  | 4: Errore |
|  | 5: Dettagli |
|  | 6: Tipo di dispositivo |
|  | 7: tipo di client Access Enabler |
|  | 8: Sistema operativo |


**Attivato da:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[Torna all&#39;inizio](#top)

</br>

## setToken(inRequestedResourceID, inToken) {#setToken(inRequestedResourceID,inToken)}

**Descrizione:** implementare questo callback per ricevere il token multimediale di breve durata (inToken) e l&#39;ID della risorsa (inRequestedResourceID) per cui è stata effettuata e completata una richiesta di autorizzazione o di autorizzazione di controllo.

**Attivato da:** [checkAuthorization()](#checkAuthZ), [getAuthorization()](#getAuthZ)
</br>

[Torna all&#39;inizio](#top)

</br>

## tokenRequestFailed(inRequestedResourceID, inRequestErrorCode, inRequestDetailedErrorMessage) {#token-request-failed-error-msg}

**Descrizione:** implementare il callback da segnalare quando una richiesta di autorizzazione o di controllo-autorizzazione non è riuscita. Può essere utilizzato facoltativamente da un MVPD per fornire un messaggio personalizzato che deve essere visualizzato dal programmatore.

>[!IMPORTANT]
>
>Questa funzione di callback fa parte del precedente sistema di segnalazione degli errori di autenticazione di Adobe Pass originale. Viene mantenuto per compatibilità con le versioni precedenti, ma non è necessario utilizzare questa funzione se sono stati implementati callback personalizzati utilizzando il sistema corrente di Segnalazione errori avanzata. Il più recente sistema di segnalazione degli errori fornisce informazioni più dettagliate sul motivo per cui un’autorizzazione (o un’altra operazione) non è riuscita, insieme alle azioni suggerite per ogni tipo di errore o avviso.

**Parametri:**

- *inRequestedResourceID* - Stringa che fornisce l&#39;ID risorsa utilizzato nella richiesta di autorizzazione.
- *inRequestErrorCode* - Stringa che visualizza il codice di errore di autenticazione di Adobe Pass, che indica il motivo dell&#39;errore. I valori possibili sono &quot;Errore utente non autenticato&quot; e &quot;Errore utente non autorizzato&quot;. Per ulteriori dettagli, vedere &quot;Codici di errore di callback&quot; di seguito.
- *inRequestDetailedErrorMessage* - Stringa descrittiva aggiuntiva adatta alla visualizzazione. Se questa stringa descrittiva non è disponibile per alcun motivo, Adobe Pass Authentication invia una stringa vuota **(&quot;)**.  Questo può essere utilizzato da un MVPD per trasmettere messaggi di errore personalizzati o messaggi relativi alle vendite. Se ad esempio a un sottoscrittore viene negata l&#39;autorizzazione per una risorsa, MVPD potrebbe rispondere con un `*inRequestDetailedErrorMessage*`, ad esempio: **&quot;Attualmente non si dispone dell&#39;accesso a questo canale nel pacchetto. Se desideri aggiornare il pacchetto, fai clic su \*qui\*.&quot;** Il messaggio viene passato dall&#39;autenticazione di Adobe Pass tramite questo callback al sito del programmatore. Il programmatore ha quindi la possibilità di visualizzarlo o ignorarlo. L&#39;autenticazione di Adobe Pass può inoltre utilizzare `*inRequestDetailedErrorMessage*` per notificare al programmatore la condizione che potrebbe aver causato un errore. Ad esempio, **&quot;Errore di rete durante la comunicazione con il servizio di autorizzazione del provider&quot;.**



**Attivato da:** [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[Torna all&#39;inizio](#top)

</br>


## preauthorizedResources(authorizedResources) {#preauthorizedResources(authorizedResources)}

**Descrizione:** richiamata attivata da Access Enabler che distribuisce l&#39;elenco delle risorse autorizzate restituito dopo una chiamata a `checkPreauthorizedResources()`.

**Parametri:**

- *authorizedResources*: elenco delle risorse autorizzate.

**Attivato da:** [checkPreauthorizedResources()](#checkPreauthRes)
</br>

[Torna all&#39;inizio](#top)

</br>

## setMetadataStatus(chiave, crittografata, dati) {#setMetadataStatus(key,encrypted,data)}

**Descrizione:** callback attivato dall&#39;Access Enabler che distribuisce i metadati richiesti tramite una chiamata `getMetadata()`.

**Ulteriori informazioni:** [Metadati utente](#userMetadata)

**Parametri:**

- *chiave (stringa)*: chiave dei metadati per cui è stata effettuata la richiesta.
- *crittografato (booleano)*: flag che indica se il &quot;valore&quot; è crittografato o meno. Se questo è &quot;true&quot;, &quot;value&quot; sarà in realtà una rappresentazione JSON Web Encrypted del valore effettivo.
- *dati (oggetto JSON)*: un oggetto JSON con la rappresentazione dei metadati.Per le richieste semplici (&#39;`TTL_AUTHN`&#39;, &#39;`TTL_AUTHZ`&#39;, &#39;`DEVICEID`&#39;), il risultato è un valore String (che rappresenta il TTL di autenticazione, il TTL di autorizzazione o l&#39;ID dispositivo). Nel caso di una richiesta di metadati utente, il risultato può essere un oggetto primitivo o JSON che rappresenta il payload di metadati. La struttura effettiva degli oggetti metadati utente JSON è simile alla seguente:

```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
            zip: ["12345", "34567"],
            maxrating: { 
                "MPAA": "PG-13",
                "VCHIP": "TV-Y", 
                "URL": "http://exam.pl/e/manage/ratings"
            },
            householdid: "3456",
            uid: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
            channelID: ["channel-1", "channel-2"]
    }
```


Ad esempio:

```JSON
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
        if (encrypted) {
            //the metadata value is encrypted
            //needs to be decrypted by the programmer
            data = decrypt(data);
        }
        alert(key + "=" + data);
    }
```


**Attivato da:** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[Torna all&#39;inizio](#top)

</br>

## selectedProvider(result) {#selectedProvider(result)}

**Descrizione:** implementa questo callback per ricevere il MVPD attualmente selezionato e il risultato dell&#39;autenticazione dell&#39;utente corrente racchiuso nel parametro `result`. Il parametro `result` è un oggetto con le seguenti proprietà:

- **MVPD** Il MVPD attualmente selezionato o null se non è stato selezionato alcun MVPD.
- **AE\_State** Il risultato dell&#39;autenticazione per l&#39;utente corrente, uno dei seguenti: &quot;Nuovo utente&quot;, &quot;Utente non autenticato&quot; o &quot;Utente autenticato&quot;

**Attivato da:** [getSelectedProvider()](#getSelProv)

</br>

[Torna all&#39;inizio](#top)

</br>

### Codici di errore di callback {#callback-error-codes}

| Errori generici | |
|:--- | :--- | 
| Errore interno | Si è verificato un errore di sistema durante il tentativo di elaborare la richiesta. |
| Errore provider non selezionato | Si verifica all&#39;annullamento del cliente nella finestra di dialogo di selezione del provider |
| Errore provider non disponibile | Si verifica quando non è disponibile alcun provider. |

| Errori di autenticazione | |
|:--- | :--- | 
| Errore di autenticazione generica | Restituito quando il motivo non è noto o non può essere pubblicato. |
| Errore di autenticazione interna | Si è verificato un errore di sistema durante il tentativo di autenticazione. |
| Errore utente non autenticato | Utente non autenticato. |
| Errore di più richieste di autenticazione | Sono state ricevute ulteriori richieste di autenticazione prima del completamento della prima. |

| Errori di autorizzazione | |
|:--- | :--- | 
| Errore di autorizzazione generica | Restituito quando il motivo non è noto o non può essere pubblicato. |
| Errore di autorizzazione interna | Si è verificato un errore di sistema durante il tentativo di autorizzazione. |
| Errore utente non autorizzato | Il cliente non è autorizzato a visualizzare il contenuto richiesto. |

<!--

### Related Information {#Related-Infornation}

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
* **JavaScript Code Samples**
* [Error Reporting](/help/authentication/error-reporting.md)
* [Understanding Tokens](#understanding_tokens)
* **Tracking Data in Adobe Pass Authentication**
-->

[Torna all&#39;inizio](#top)
