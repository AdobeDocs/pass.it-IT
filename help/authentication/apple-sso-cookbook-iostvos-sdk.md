---
title: Manuale Apple SSO (iOS/tvOS SDK)
description: Manuale Apple SSO (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1867'
ht-degree: 0%

---

# Manuale Apple SSO (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#Introduction}

L’SDK iOS/tvOS di Adobe Primetime Authentication AccessEnabler può supportare l’autenticazione Single Sign-On (SSO) della piattaforma per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS tramite quello che chiamiamo flusso di lavoro SSO di Apple.

Questo documento funge da estensione della documentazione esistente AccessEnabler iOS/tvOS SDK, reperibile [qui](/help/authentication/iostvos-sdk-api-reference.md).

</br>

## Manuale {#Cookbook}

Per beneficiare dell’esperienza utente SSO di Apple, un’applicazione deve integrare l’SDK iOS/tvOS di AccessEnabler e seguire la sequenza di suggerimenti presentata di seguito.

</br>

### Prerequisiti {#Prerequisites}

</br>

#### Autorizzazione

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Per poter accedere alle informazioni di abbonamento dell’utente, l’utente deve concedere all’applicazione l’autorizzazione per procedere, in modo analogo a fornire l’accesso alla fotocamera o al microfono del dispositivo. Questa autorizzazione deve essere richiesta per applicazione e il dispositivo salverà la selezione dell&#39;utente. È importante ricordare che l&#39;utente può modificare la propria decisione accedendo alle impostazioni dell&#39;applicazione (autorizzazione di accesso del provider TV) o alla sezione da *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** È consigliabile richiedere l&#39;autorizzazione dell&#39;utente quando l&#39;applicazione entra in primo piano, ma si tratta solo di un suggerimento, in quanto l&#39;applicazione può verificare [autorizzazione di accesso](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) le informazioni di abbonamento dell’utente in qualsiasi momento prima di richiedere l’autenticazione dell’utente. Inoltre, le API SDK di AccessEnabler iOS/tvOS richiederanno automaticamente l’autorizzazione dell’utente quando necessario.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Nel caso in cui l’utente non conceda l’accesso alle sue informazioni di abbonamento o nel caso in cui la comunicazione con il framework dell’account del sottoscrittore video non riesca, l’SDK di AccessEnabler iOS/tvOS effettuerà il fallback al flusso di autenticazione regolare.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Consigliamo di incentivare gli utenti che rifiutano di concedere l’autorizzazione per accedere alle informazioni sull’abbonamento spiegando i vantaggi dell’esperienza utente Single Sign-On (SSO). È importante ricordare che l&#39;utente può modificare la propria decisione accedendo alle impostazioni dell&#39;applicazione (autorizzazione di accesso del provider TV) o alla sezione da *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS.


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

</br>

#### Callback

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Implementa il seguente elenco di [callback](/help/authentication/iostvos-sdk-api-reference.md) specifiche del flusso di lavoro SSO di Apple.

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Callback attivato quando il selettore MVPD di Apple sta per aprirsi.
- [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Callback attivato quando il selettore MVPD di Apple sta per chiudersi.

</br>

#### Segnalazione errori

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Implementa il seguente elenco di [codici di errore avanzati](/help/authentication/error-reporting.md) specifiche del flusso di lavoro SSO di Apple.

- ***N003*** - L&#39;utente ha selezionato l&#39;opzione &quot;Altro provider TV&quot; dal selettore MVPD di Apple.
- ***N004*** - L&#39;utente ha selezionato un provider TV dal selettore MVPD di Apple, che non è supportato (integrazione o Single Sign-On disabilitato) dal richiedente corrente.
- ***N005*** : l’utente ha deciso di annullare il selettore MVPD regolare o il selettore MVPD di Apple.
- ***VSA403*** - Autorizzazione del provider TV dell&#39;utente negata per l&#39;applicazione.
- ***VSA404*** - L&#39;autorizzazione per il provider TV dell&#39;utente non è determinata per l&#39;applicazione.
- ***VSA503*** - Richiesta non riuscita dei metadati dell’account del sottoscrittore video. Il contesto fornito è più ampio *messaggio* campo.
- ***AAPL / APPL_ERROR*** - Richiesta non riuscita dei metadati dell’account del sottoscrittore video. Il contesto fornito è più ampio *dettagli* campo.

</br>

### Autenticazione {#Authentication}

>[!TIP]
>
> **<u>Suggerimento</u>** Segui i passaggi seguenti per le implementazioni iOS/iPadOS/tvOS.

1. L&#39;applicazione dovrebbe [inizializzare](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) l’SDK AccessEnabler iOS/tvOS.
1. L&#39;applicazione dovrebbe [imposta l&#39;identificatore del richiedente corrente](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Importante:** Questo secondo passaggio potrebbe attivare un [codice di errore avanzato](/help/authentication/error-reporting.md) specifico del flusso di lavoro SSO di Apple, nel caso in cui **una delle seguenti affermazioni è vera**:

   - ***VSA403*** - Autorizzazione del provider TV dell&#39;utente negata per l&#39;applicazione.
   - ***VSA404*** - L&#39;autorizzazione per il provider TV dell&#39;utente non è determinata per l&#39;applicazione.
   - ***APPL*** - Errore durante la comunicazione tra l’SDK di AccessEnabler iOS/tvOS e il framework dell’account del sottoscrittore video.

   Questo secondo passaggio tenterebbe di scambiare in silenzio il profilo Apple SSO con un token di autenticazione di Adobe, nel caso in cui **tutte le informazioni precedenti sono false** e **tutte le seguenti condizioni sono vere**:

   - L&#39;autorizzazione del provider TV dell&#39;utente è concessa per l&#39;applicazione.
   - L&#39;utente ha effettuato l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo.
   - L’SDK iOS/tvOS di AccessEnabler ha ricevuto l’identificatore del provider TV dell’utente dal framework dell’account dell’abbonato video.
   - L’integrazione del fornitore TV dell’utente con l’applicazione è abilitata tramite Adobe Primetime TVE Dashboard.
   - Il Single Sign-On del provider TV dell’utente con l’applicazione è abilitato tramite Adobe Primetime TVE Dashboard.
   - Il provider TV dell&#39;utente non è danneggiato tramite Adobe Primetime TVE Dashboard.
   - L’SDK iOS/tvOS di AccessEnabler ha ricevuto la risposta SAML del provider TV dell’utente dal framework dell’account dell’abbonato video.

   **<u>Suggerimento pro:</u>** Questo secondo passaggio non attiverà altri callback, oltre al [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) callback, poiché l&#39;autenticazione non è stata avviata esplicitamente dall&#39;applicazione.

1. L&#39;applicazione dovrebbe [controllare lo stato di autenticazione](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Importante:** Questo terzo passaggio potrebbe attivare un [codice di errore avanzato](/help/authentication/error-reporting.md) specifico del flusso di lavoro SSO di Apple, nel caso in cui **una delle seguenti affermazioni è vera**:

   - ***VSA403** - L&#39;utente ha effettuato l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo, ma l&#39;autorizzazione di provider TV dell&#39;utente è stata negata per l&#39;applicazione.
   - ***VSA404** - L&#39;utente ha effettuato l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo, ma l&#39;autorizzazione di provider TV dell&#39;utente non è determinata per l&#39;applicazione.
   - ***APPL\_ERROR** : l’utente ha effettuato l’accesso al proprio account di provider TV a livello di sistema del dispositivo, ma la comunicazione tra l’SDK di AccessEnabler iOS/tvOS e il framework dell’account dell’abbonato video ha rilevato un errore.

   **Importante:** Questo terzo passaggio attiverà [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback con *stato* uguale a 0, nel caso **una delle seguenti affermazioni è vera**:

   - L&#39;utente non ha eseguito l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo o tramite un flusso di autenticazione regolare.
   - L&#39;utente ha eseguito l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo o tramite un flusso di autenticazione regolare, ma il token di autenticazione di provider TV dell&#39;utente è stato passato.
   - L’utente ha effettuato l’accesso al proprio account di provider TV a livello di sistema del dispositivo o tramite un flusso di autenticazione regolare, ma l’integrazione del provider TV dell’utente con l’applicazione è disabilitata tramite Adobe Primetime TVE Dashboard.
   - L&#39;utente ha eseguito l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo, ma il Single Sign-On del provider TV dell&#39;utente con l&#39;applicazione è disabilitato tramite Adobe Primetime TVE Dashboard.
   - L&#39;utente ha eseguito l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo, ma l&#39;autorizzazione del provider TV dell&#39;utente è stata negata per l&#39;applicazione.
   - L&#39;utente ha eseguito l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo, ma l&#39;autorizzazione del provider TV dell&#39;utente non è determinata per l&#39;applicazione.
   - L’utente ha effettuato l’accesso al proprio account di provider TV a livello di sistema del dispositivo, ma la comunicazione tra l’SDK di AccessEnabler iOS/tvOS e il framework dell’account del sottoscrittore di video ha rilevato un errore.

   **Importante:** Questo terzo passaggio attiverà [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback con *stato* uguale a 1, nel caso **tutti i precedenti sono falsi.**


1. L&#39;applicazione dovrebbe [inizializzare l’autenticazione](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) nel caso in cui il precedente controllo dello stato di autenticazione abbia attivato [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback con *stato* uguale a 0.

   **<u>Suggerimento pro:</u>** Implementa una delle seguenti API SDK di AccessEnabler iOS/tvOS [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) o [getAuthentication:filtro](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Importante:** Questo quarto passaggio potrebbe attivare un [codice di errore avanzato](/help/authentication/error-reporting.md) specifico del flusso di lavoro SSO di Apple, nel caso in cui **una delle seguenti affermazioni è vera**:

   - ***VSA403*** - Autorizzazione del provider TV dell&#39;utente negata per l&#39;applicazione.
   - ***VSA404*** - L&#39;autorizzazione per il provider TV dell&#39;utente non è determinata per l&#39;applicazione.
   - ***VSA503*** - Errore durante la comunicazione tra l’SDK di AccessEnabler iOS/tvOS e il framework dell’account del sottoscrittore video.
   - ***N003*** - L&#39;utente ha selezionato l&#39;opzione &quot;Altro provider TV&quot; dal selettore MVPD di Apple.
   - ***N004*** - L&#39;utente ha selezionato un provider TV dal selettore MVPD di Apple, che non è supportato (integrazione o Single Sign-On disabilitato) dal richiedente corrente.
   - ***N005*** : l’utente ha deciso di annullare il selettore MVPD regolare o il selettore MVPD di Apple.

   **Importante:** Questo quarto passaggio tornerebbe al flusso di autenticazione regolare, attivando [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) callback e **uno** di quanto sopra [codici di errore avanzati](/help/authentication/error-reporting.md), nel caso **uno dei precedenti è vero**.

   **Importante:** Questo quarto passaggio tornerebbe al flusso di autenticazione regolare, attivando [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) o [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) callback e **nessuno** di quanto sopra [codici di errore avanzati](/help/authentication/error-reporting.md), se l’utente ha selezionato un provider TV che non supporta Apple SSO, ma è presente nel selettore MVPD di Apple.

   **<u>Suggerimento pro:</u>** L’SDK iOS/tvOS di AccessEnabler chiama automaticamente il [setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) API, nel caso in cui l’utente abbia selezionato un provider TV che non supporta Apple SSO, ma è presente nel selettore MVPD di Apple.

   **Importante:** Questo quarto passaggio tenterebbe di scambiare silenziosamente il profilo SSO di Apple con un token di autenticazione di Adobe, nel caso in cui **tutte le informazioni precedenti sono false** e **tutte le seguenti condizioni sono vere**:

   - L&#39;autorizzazione del provider TV dell&#39;utente è concessa per l&#39;applicazione.
   - L&#39;utente è connesso/attualmente accede al proprio account di provider TV a livello di sistema del dispositivo.
   - L’SDK iOS/tvOS di AccessEnabler ha ricevuto l’identificatore del provider TV dell’utente dal framework dell’account dell’abbonato video.
   - L’integrazione del fornitore TV dell’utente con l’applicazione è abilitata tramite Adobe Primetime TVE Dashboard.
   - Il Single Sign-On del provider TV dell’utente con l’applicazione è abilitato tramite Adobe Primetime TVE Dashboard.
   - Il provider TV dell&#39;utente non è danneggiato tramite Adobe Primetime TVE Dashboard.
   - L’SDK iOS/tvOS di AccessEnabler ha ricevuto la risposta SAML del provider TV dell’utente dal framework dell’account dell’abbonato video.



>**<u>Suggerimento pro:</u>** Questo quarto passaggio attiverà [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus) callback, indipendentemente da *stato* poiché l&#39;autenticazione è stata avviata esplicitamente dall&#39;applicazione.


</br>

### Metadati {#Metadata}

L’applicazione ha la possibilità di determinare se l’autenticazione è avvenuta a seguito di un accesso tramite l’SSO della piattaforma o meno, utilizzando &quot;*tokenSource&quot;* [metadati utente](/help/authentication/iostvos-sdk-api-reference.md#getMeta) API dall’SDK di AccessEnabler iOS/tvOS.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### Disconnetti {#Logout}

Il [Account abbonato video](https://developer.apple.com/documentation/videosubscriberaccount) Il framework non fornisce un&#39;API per disconnettere programmaticamente gli utenti che hanno effettuato l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo. Pertanto, affinché la disconnessione diventi effettiva, l&#39;utente finale dovrà disconnettersi esplicitamente da *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS. L&#39;altra opzione che l&#39;utente avrebbe è quella di revocare l&#39;autorizzazione per accedere alle informazioni sull&#39;abbonamento dell&#39;utente dalla sezione delle impostazioni specifiche dell&#39;applicazione (autorizzazione di accesso del provider TV).

>[!TIP]
>
> **<u>Suggerimento</u>** Implementare questo attraverso l’SDK di AccessEnabler iOS/tvOS [logout](/help/authentication/iostvos-sdk-api-reference.md#logout) API.


>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi seguenti per le implementazioni tvOS.

- L&#39;applicazione dovrebbe [avvia la disconnessione](/help/authentication/iostvos-sdk-api-reference.md#logout) dall’SDK di AccessEnabler iOS/tvOS. Ciò non faciliterebbe la pulizia delle sessioni da parte di MVPD.
- L’applicazione deve indicare/richiedere all’utente di disconnettersi esplicitamente da *`Settings -> Accounts -> TV Provider`* su tvOS solo nel caso [*VSA203* il codice di stato viene attivato](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi seguenti per le implementazioni iOS/iPadOS.

- L&#39;applicazione dovrebbe [avvia la disconnessione](/help/authentication/iostvos-sdk-api-reference.md#logout) dall’SDK di AccessEnabler iOS/tvOS. Questo faciliterebbe la pulizia delle sessioni da parte di MVPD.
- L’applicazione deve indicare/richiedere all’utente di disconnettersi esplicitamente da *`Settings -> TV Provider`* su iOS/iPadOS solo nel caso [*VSA203* il codice di stato viene attivato](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
