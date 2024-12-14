---
title: Manuale Apple SSO (iOS/tvOS SDK)
description: Manuale Apple SSO (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%

---

# (Legacy) Manuale Apple SSO (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Adobe Pass Authentication AccessEnabler iOS/tvOS SDK supporta il Single Sign-On (SSO) per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS.

Questo documento funge da estensione della documentazione esistente di AccessEnabler iOS/tvOS SDK, disponibile [qui](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md).

## Manuale {#apple-sso-cookbook-iostvos-sdk-cookbook}

Per beneficiare dell’esperienza utente SSO di Apple, l’applicazione deve integrare AccessEnabler iOS/tvOS SDK e seguire la sequenza di passaggi descritta di seguito.

### Prerequisiti {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### Autorizzazione {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>Suggerimento pro:</u>** L&#39;applicazione di streaming deve richiedere l&#39;accesso alle informazioni di abbonamento dell&#39;utente salvate a livello di dispositivo, per le quali l&#39;utente deve concedere all&#39;applicazione l&#39;autorizzazione per continuare, in modo analogo a fornire l&#39;accesso alla fotocamera o al microfono del dispositivo. Questa autorizzazione deve essere richiesta per applicazione utilizzando il framework dell&#39;account dell&#39;utente con sottoscrizione video [di Apple](https://developer.apple.com/documentation/videosubscriberaccount) e il dispositivo salverà la selezione dell&#39;utente.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** si consiglia di incentivare gli utenti che rifiutano di concedere l&#39;autorizzazione per accedere alle informazioni sull&#39;abbonamento illustrando i vantaggi dell&#39;esperienza utente Single Sign-On di Apple, ma è bene tenere presente che l&#39;utente può modificare la propria decisione accedendo alle impostazioni dell&#39;applicazione (autorizzazione del provider TV) o a *`Settings -> TV Provider`* su iOS e iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** l&#39;applicazione di streaming può richiedere l&#39;autorizzazione dell&#39;utente quando l&#39;applicazione entra in primo piano, perché l&#39;applicazione può controllare [l&#39;autorizzazione di accesso](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente in qualsiasi momento prima di richiedere l&#39;autenticazione dell&#39;utente.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Nel caso in cui l&#39;utente non conceda l&#39;accesso alle sue informazioni di abbonamento o nel caso in cui la comunicazione con il framework dell&#39;account del sottoscrittore video non riesca, il SDK AccessEnabler iOS/tvOS tornerà al normale flusso di autenticazione.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                   // Do nothing.
                
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                   // Incentivize users who refuse to give permission to access subscription information by explaining the benefits of the Single Sign-On (SSO) user experience. Please bear in mind that the user can change its decision by going to the application settings (TV Provider permission access) or to the section from Settings -> TV Provider on iOS/iPadOS or Settings -> Accounts -> TV Provider on tvOS.
                   ...
                }
    }
    ... 
```

#### Callback {#apple-sso-cookbook-iostvos-sdk-callbacks}

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Implementa il seguente elenco di [callback](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) specifici per il flusso di lavoro SSO di Apple.

* [*presentTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Callback attivato quando verrà aperto il selettore MVPD di Apple.
* [*dismissTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Callback attivato quando il selettore MVPD di Apple verrà chiuso.

#### Segnalazione errori {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Implementare il seguente elenco di [codici di errore avanzati](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) specifici per il flusso di lavoro SSO di Apple.

* ***N003*** - L&#39;utente ha selezionato l&#39;opzione &quot;Altro provider TV&quot; dal selettore MVPD di Apple.
* ***N004*** - L&#39;utente ha selezionato un provider TV dal selettore MVPD di Apple, che non è supportato (integrazione o Single Sign-On disabilitato) dal richiedente corrente.
* ***N005*** - L&#39;utente ha deciso di annullare il selettore MVPD normale o il selettore MVPD Apple.
* ***VSA403*** - Autorizzazione del provider TV dell&#39;utente negata per l&#39;applicazione.
* ***VSA404*** - L&#39;autorizzazione del provider TV dell&#39;utente non è determinata per l&#39;applicazione.
* ***VSA503*** - Richiesta metadati account sottoscrittore video non riuscita. Nel campo *message* è fornito più contesto.
* ***AAPL / APPL_ERROR*** - La richiesta dei metadati dell&#39;account del sottoscrittore video non è riuscita. Nel campo *details* viene fornito più contesto.

### Autenticazione {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>Suggerimento:</u>** Segui i passaggi seguenti per l&#39;implementazione iOS/iPadOS/tvOS.

1. L&#39;applicazione dovrà [inizializzare](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) AccessEnabler iOS/tvOS SDK.


1. L&#39;applicazione deve [impostare l&#39;identificatore del richiedente corrente](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Importante:** questo secondo passaggio potrebbe attivare un [codice di errore avanzato](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) specifico per il flusso di lavoro SSO di Apple, nel caso in cui **uno dei seguenti sia true**:

   * ***VSA403*** - Autorizzazione del provider TV dell&#39;utente negata per l&#39;applicazione.
   * ***VSA404*** - L&#39;autorizzazione del provider TV dell&#39;utente non è determinata per l&#39;applicazione.
   * ***APPL*** - Errore durante la comunicazione tra AccessEnabler iOS/tvOS SDK e il framework dell&#39;account del sottoscrittore video.

   Nel secondo passaggio si tenta di scambiare automaticamente il profilo SSO di Apple con un token di autenticazione di Adobe, nel caso in cui **tutte le impostazioni precedenti siano false** e **tutte le impostazioni seguenti siano vere**:

   * L&#39;autorizzazione del provider TV dell&#39;utente è concessa per l&#39;applicazione.
   * L&#39;utente ha effettuato l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo.
   * Il SDK AccessEnabler iOS/tvOS ha ricevuto l&#39;identificatore del provider TV dell&#39;utente dal framework dell&#39;account del sottoscrittore video.
   * L’integrazione del fornitore TV dell’utente con l’applicazione è abilitata tramite Adobe Primetime TVE Dashboard.
   * Il Single Sign-On del provider TV dell’utente con l’applicazione è abilitato tramite Adobe Primetime TVE Dashboard.
   * Il provider TV dell&#39;utente non è danneggiato tramite Adobe Primetime TVE Dashboard.
   * Il SDK AccessEnabler iOS/tvOS ha ricevuto la risposta SAML del provider TV dell’utente dal framework dell’account dell’abbonato video.

   **<u>Suggerimento pro:</u>** questo secondo passaggio non attiverà altri callback, oltre al callback [setRequestorComplete](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete), poiché l&#39;autenticazione non è stata avviata in modo esplicito dall&#39;applicazione.


1. L&#39;applicazione dovrebbe [controllare lo stato di autenticazione](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Importante:** questo terzo passaggio potrebbe attivare un [codice di errore avanzato](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) specifico per il flusso di lavoro SSO di Apple, nel caso in cui **uno dei seguenti sia true**:

   * ***VSA403** - L&#39;utente ha effettuato l&#39;accesso al proprio account di provider TV in
il livello di sistema del dispositivo, ma l&#39;autorizzazione del provider TV dell&#39;utente è
negato per l’applicazione.
   * ***VSA404** - L&#39;utente ha effettuato l&#39;accesso al proprio account di provider TV in
il livello di sistema del dispositivo, ma con l&#39;autorizzazione del provider TV dell&#39;utente
è indeterminato per l’applicazione.
   * ***APPL\_ERROR** - L&#39;utente ha effettuato l&#39;accesso al proprio provider TV
a livello di sistema del dispositivo, ma la comunicazione tra
SDK di AccessEnabler iOS/tvOS e l’account del sottoscrittore video
il framework ha rilevato un errore.

   **Importante:** questo terzo passaggio attiverà il callback [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) con *status* uguale a 0, nel caso in cui **uno dei seguenti valori sia true**:

   * L&#39;utente non ha eseguito l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo o tramite un flusso di autenticazione regolare.
   * L&#39;utente ha eseguito l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo o tramite un flusso di autenticazione regolare, ma il token di autenticazione di provider TV dell&#39;utente è stato passato.
   * L’utente ha effettuato l’accesso al proprio account di provider TV a livello di sistema del dispositivo o tramite un flusso di autenticazione regolare, ma l’integrazione del provider TV dell’utente con l’applicazione è disabilitata tramite Adobe Primetime TVE Dashboard.
   * L&#39;utente ha eseguito l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo, ma il Single Sign-On del provider TV dell&#39;utente con l&#39;applicazione è disabilitato tramite Adobe Primetime TVE Dashboard.
   * L&#39;utente ha eseguito l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo, ma l&#39;autorizzazione del provider TV dell&#39;utente è stata negata per l&#39;applicazione.
   * L&#39;utente ha eseguito l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo, ma l&#39;autorizzazione del provider TV dell&#39;utente non è determinata per l&#39;applicazione.
   * L&#39;utente ha eseguito l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo, ma la comunicazione tra AccessEnabler iOS/tvOS SDK e il framework dell&#39;account del sottoscrittore video ha rilevato un errore.

   **Importante:** questo terzo passaggio attiverà il callback [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) con *status* uguale a 1, nel caso in cui **tutti i precedenti siano false.**


1. L&#39;applicazione dovrebbe [inizializzare l&#39;autenticazione](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) nel caso in cui il precedente controllo dello stato di autenticazione abbia attivato il callback [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) con *status* uguale a 0.

   **<u>Suggerimento pro:</u>** Implementare una delle seguenti API SDK di AccessEnabler iOS/tvOS [getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) o [getAuthentication:filter](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Importante:** questo quarto passaggio potrebbe attivare un [codice di errore avanzato](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) specifico per il flusso di lavoro SSO di Apple, nel caso in cui **uno dei seguenti sia true**:

   * ***VSA403*** - Autorizzazione del provider TV dell&#39;utente negata per l&#39;applicazione.
   * ***VSA404*** - L&#39;autorizzazione del provider TV dell&#39;utente non è determinata per l&#39;applicazione.
   * ***VSA503*** - La comunicazione tra AccessEnabler iOS/tvOS SDK e il framework dell&#39;account del sottoscrittore video ha rilevato un errore.
   * ***N003*** - L&#39;utente ha selezionato l&#39;opzione &quot;Altro provider TV&quot; dal selettore MVPD di Apple.
   * ***N004*** - L&#39;utente ha selezionato un provider TV dal selettore MVPD di Apple, che non è supportato (integrazione o Single Sign-On disabilitato) dal richiedente corrente.
   * ***N005*** - L&#39;utente ha deciso di annullare il selettore MVPD normale o il selettore MVPD Apple.

   **Importante:** questo quarto passaggio tornerebbe al normale flusso di autenticazione, attivando il callback [displayProviderDialog](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dispProvDialog) e **one** dei [codici di errore avanzati](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) precedenti, nel caso in cui **uno dei precedenti sia true**.

   **Importante:** questo quarto passaggio tornerebbe al normale flusso di autenticazione, attivando il callback [navigateToUrl](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2url) o [navigateToUrl:useSVC](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2urlSVC) e **none** dei [codici di errore avanzati](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) di cui sopra, nel caso in cui l&#39;utente abbia selezionato un provider TV che non supporta Apple SSO, ma è presente nel selettore di Apple MVPD.

   **<u>Suggerimento pro:</u>** AccessEnabler iOS/tvOS SDK chiama automaticamente l&#39;API [setSelectedProrovder](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) se l&#39;utente ha selezionato un provider TV che non supporta l&#39;SSO di Apple, ma è presente nel selettore di Apple MVPD.

   **Importante:** questo quarto passaggio tenterebbe di scambiare automaticamente il profilo SSO di Apple con un token di autenticazione Adobe, nel caso in cui **tutti i precedenti siano false** e **tutti i seguenti siano true**:

   * L&#39;autorizzazione del provider TV dell&#39;utente è concessa per l&#39;applicazione.
   * L&#39;utente è connesso/attualmente accede al proprio account di provider TV a livello di sistema del dispositivo.
   * Il SDK AccessEnabler iOS/tvOS ha ricevuto l&#39;identificatore del provider TV dell&#39;utente dal framework dell&#39;account del sottoscrittore video.
   * L’integrazione del fornitore TV dell’utente con l’applicazione è abilitata tramite Adobe Primetime TVE Dashboard.
   * Il Single Sign-On del provider TV dell’utente con l’applicazione è abilitato tramite Adobe Primetime TVE Dashboard.
   * Il provider TV dell&#39;utente non è danneggiato tramite Adobe Primetime TVE Dashboard.
   * Il SDK AccessEnabler iOS/tvOS ha ricevuto la risposta SAML del provider TV dell’utente dal framework dell’account dell’abbonato video.

   **<u>Suggerimento pro:</u>** questo quarto passaggio attiverà il callback [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setAuthNStatus), indipendentemente dal risultato *status*, poiché l&#39;autenticazione è stata avviata in modo esplicito dall&#39;applicazione.

### Metadati {#apple-sso-cookbook-iostvos-sdk-metadata}

L&#39;applicazione dispone dell&#39;opzione per determinare se l&#39;autenticazione è avvenuta a seguito di un accesso tramite Partner SSO o meno, utilizzando l&#39;API &quot;*tokenSource&quot;* [metadati utente](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) di AccessEnabler iOS/tvOS SDK.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### Disconnetti {#apple-sso-cookbook-iostvos-sdk-logout}

Il framework dell&#39;account del sottoscrittore video [1} non fornisce un&#39;API per disconnettere a livello di programmazione gli utenti che hanno effettuato l&#39;accesso al proprio account del provider TV a livello di sistema del dispositivo. ](https://developer.apple.com/documentation/videosubscriberaccount) Pertanto, affinché la disconnessione diventi effettiva, l&#39;utente finale dovrà disconnettersi esplicitamente da *`Settings -> TV Provider`* su iOS/iPadOS o da *`Settings -> Accounts -> TV Provider`* su tvOS. L&#39;altra opzione che l&#39;utente avrebbe è quella di revocare l&#39;autorizzazione per accedere alle informazioni sull&#39;abbonamento dell&#39;utente dalla sezione delle impostazioni specifiche dell&#39;applicazione (autorizzazione di accesso del provider TV).

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite l&#39;API AccessEnabler iOS/tvOS SDK [logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout).

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi seguenti per le implementazioni tvOS.

* L&#39;applicazione deve [avviare la disconnessione](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) dal SDK AccessEnabler iOS/tvOS. Questo non faciliterebbe la pulizia delle sessioni sul lato MVPD.
* L&#39;applicazione dovrebbe indicare/richiedere all&#39;utente di disconnettersi esplicitamente da *`Settings -> Accounts -> TV Provider`* su tvOS solo se il codice di stato [*VSA203* è attivato](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md).

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi indicati di seguito per l&#39;implementazione iOS/iPadOS.

* L&#39;applicazione deve [avviare la disconnessione](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) dal SDK AccessEnabler iOS/tvOS. Questo faciliterebbe la pulizia delle sessioni sul lato MVPD.
* L&#39;applicazione dovrebbe indicare/richiedere all&#39;utente di disconnettersi esplicitamente da *`Settings -> TV Provider`* su iOS/iPadOS solo se il codice di stato [*VSA203* è attivato](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md).
