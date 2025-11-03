---
title: Manuale Apple SSO (REST API V1)
description: Manuale Apple SSO (REST API V1)
exl-id: 072a011f-e1bb-4d3e-bcb5-697f2d1739cc
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '1496'
ht-degree: 0%

---

# (Legacy) Manuale di Apple SSO (REST API V1) {#apple-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

L’API REST per l’autenticazione di Adobe Pass V1 supporta il Single Sign-On (SSO) dei partner per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS.

Questo documento funge da estensione della documentazione API REST V1 esistente, disponibile [qui](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md).

## Manuale {#apple-sso-cookbook-rest-api-v1-cookbook}

Per beneficiare dell&#39;esperienza utente SSO di Apple, l&#39;applicazione deve integrare il [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) sviluppato da Apple, mentre per la comunicazione REST API V1 di autenticazione Adobe Pass, deve seguire la sequenza di passaggi descritta di seguito.

### Autorizzazione {#apple-sso-cookbook-rest-api-v1-permission}

>[!TIP]
>
> **<u>Suggerimento pro:</u>** L&#39;applicazione di streaming deve richiedere l&#39;accesso alle informazioni di abbonamento dell&#39;utente salvate a livello di dispositivo, per le quali l&#39;utente deve concedere all&#39;applicazione l&#39;autorizzazione per continuare, in modo analogo a fornire l&#39;accesso alla fotocamera o al microfono del dispositivo. Questa autorizzazione deve essere richiesta per applicazione utilizzando il framework dell&#39;account dell&#39;utente con sottoscrizione video [di Apple](https://developer.apple.com/documentation/videosubscriberaccount) e il dispositivo salverà la selezione dell&#39;utente.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** si consiglia di incentivare gli utenti che rifiutano di concedere l&#39;autorizzazione per accedere alle informazioni sull&#39;abbonamento illustrando i vantaggi dell&#39;esperienza utente Single Sign-On di Apple, ma è bene tenere presente che l&#39;utente può modificare la propria decisione accedendo alle impostazioni dell&#39;applicazione (autorizzazione del provider TV) o a *`Settings -> TV Provider`* su iOS e iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** è consigliabile richiedere l&#39;autorizzazione dell&#39;utente quando l&#39;applicazione entra in primo piano, in quanto l&#39;applicazione può verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente in qualsiasi momento prima di richiedere l&#39;autenticazione dell&#39;utente.

### Autenticazione {#apple-sso-cookbook-rest-api-v1-authentication}

* [Esiste un token di autenticazione Adobe valido?](#step1)
* [L’utente ha effettuato l’accesso tramite Partner SSO?](#step2)
* [Recupera la configurazione di Adobe](#step3)
* [Avvia flusso di lavoro SSO partner con configurazione Adobe](#step4)
* [L’accesso dell’utente è riuscito?](#step5)
* [Ottieni una richiesta di profilo da Adobe per il MVPD selezionato](#step6)
* [Inoltrare la richiesta Adobe all’SSO partner per ottenere il profilo](#step7)
* [Scambia il profilo SSO Partner con un token di autenticazione Adobe](#step8)
* [Il token Adobe è stato generato correttamente?](#step9)
* [Avvia flusso di lavoro di autenticazione regolare](#step10)
* [Procedi con i flussi di autorizzazione](#step11)

![](/help/authentication/assets/rest-api-v1/apple-sso-cookbook-rest-api-v1.png)

#### Passaggio: &quot;Esiste un token di autenticazione Adobe valido?&quot; {#step1}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementalo tramite il servizio API [Controlla token di autenticazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) di Adobe Pass Authentication.

#### Passaggio: &quot;L’utente ha effettuato l’accesso tramite SSO partner?&quot; {#step2}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).

* L&#39;applicazione dovrebbe verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente e procedere solo se l&#39;utente lo consente.
* L&#39;applicazione dovrebbe inviare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per le informazioni sull&#39;account del sottoscrittore.
* L&#39;applicazione dovrebbe attendere ed elaborare le informazioni [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui lo snippet di codice e presta particolare attenzione ai commenti.

```swift
...
let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();

videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
            switch (accessStatus) {
            // The user allows the application to access subscription information.
            case VSAccountAccessStatus.granted:
                    // Construct the request for subscriber account information.
                    let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();

                    // This is actually the SAML Issuer not the channel ID.
                    vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAccountProviderIdentifier = true;
                    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                    
                    // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                    vsaMetadataRequest.isInterruptionAllowed = false;
                    
                    // Submit the request for subscriber account information - accountProviderIdentifier.
                    videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in        
                        if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                            // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                            // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                            ...
                            // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                            // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                            ...
                            // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                            // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                            ...
                            // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                            ...
                            // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Initiate regular authentication workflow" step.
                ...
            }
}
...  
```

#### Passaggio: &quot;Recupera la configurazione di Adobe&quot; {#step3}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite il servizio API [Fornisci elenco MVPD](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) di autenticazione di Adobe Pass.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Tieni presente le proprietà di MVPD: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* e presta particolare attenzione ai commenti presentati nei frammenti di codice di altri passaggi.

#### Passaggio &quot;Avviare il flusso di lavoro SSO dei partner con la configurazione di Adobe&quot; {#step4}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).

* L&#39;applicazione dovrebbe verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente e procedere solo se l&#39;utente lo consente.
* L&#39;applicazione deve fornire un [delegato](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) per VSAccountManager.
* L&#39;applicazione dovrebbe inviare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per le informazioni sull&#39;account del sottoscrittore.
* L&#39;applicazione dovrebbe attendere ed elaborare le informazioni [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui lo snippet di codice e presta particolare attenzione ai commenti.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    // This must be a class implementing the VSAccountManagerDelegate protocol.
    let videoSubscriberAccountManagerDelegate: VideoSubscriberAccountManagerDelegate = VideoSubscriberAccountManagerDelegate();
    
    videoSubscriberAccountManager.delegate = videoSubscriberAccountManagerDelegate;
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
                        
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
    
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
                        
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            // This represents the checks for the "Is user login successful?" step.
                            if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                                // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                                // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                                ...
                                // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                                // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                                ...
                                // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                                // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                                ...
                                // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                                ...
                                // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
    
                                        // The requestor.mvpds must be computed during the "Fetch Adobe configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
                                        
                                        if mvpd != nil {
                                            // Continue with the "Initiate regular authentication workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate regular authentication workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate regular authentication workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate regular authentication workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Passaggio: &quot;L’accesso dell’utente è riuscito?&quot; {#step5}

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Tieni presente lo snippet di codice del passaggio [&quot;Avvia il flusso di lavoro SSO del partner con la configurazione di Adobe&quot;](#step4). L&#39;accesso utente ha esito positivo nel caso in cui *`vsaMetadata!.accountProviderIdentifier`* contenga un valore valido e la data corrente non abbia superato il valore *`vsaMetadata!.authenticationExpirationDate`*.

#### Passaggio &quot;Ottenere una richiesta di profilo da Adobe per il MVPD selezionato&quot; {#step6}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementalo tramite il servizio API [Richiesta profilo](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md) di autenticazione Adobe Pass.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** l&#39;identificatore del provider ottenuto dal framework dell&#39;account del sottoscrittore video rappresenta *`platformMappingId`* in termini di configurazione dell&#39;autenticazione Adobe Pass. Pertanto, l&#39;applicazione deve determinare il valore della proprietà MVPD ID, utilizzando il valore *`platformMappingId`*, tramite il servizio API [Fornisci elenco MVPD](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) di autenticazione Adobe Pass.

#### Passaggio: &quot;Inoltra la richiesta Adobe all’SSO partner per ottenere il profilo&quot; {#step7}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).


* L&#39;applicazione dovrebbe verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente e procedere solo se l&#39;utente lo consente.
* L&#39;applicazione dovrebbe inviare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per le informazioni sull&#39;account del sottoscrittore.
* L&#39;applicazione dovrebbe attendere ed elaborare le informazioni [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui lo snippet di codice e presta particolare attenzione ai commenti.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
    
                        // This are the user metadata fields expected to be available on a successful login and are determined from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service. Look for the requiredMetadataFields associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = requiredMetadataFields;
    
                        // This is the payload from [Adobe Pass Authentication](/help/authentication/retrieve-profilerequest.md) service.
                        vsaMetadataRequest.verificationToken = profileRequestPayload;
                        
                        // Submit the request for subscriber account information.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in
                            if (vsaMetadata != nil && vsaMetadata!.samlAttributeQueryResponse != nil) {
                                var samlResponse: String? = vsaMetadata!.samlAttributeQueryResponse!;
                                
                                // Remove new lines, new tabs and spaces.
                                samlResponse = samlResponse?.replacingOccurrences(of: "[ \\t]+", with: " ", options: String.CompareOptions.regularExpression);
                                samlResponse = samlResponse?.components(separatedBy: CharacterSet.newlines).joined(separator: "");
                                samlResponse = samlResponse?.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines);
                                
                                // Base64 encode.
                                samlResponse = samlResponse?.data(using: .utf8)?.base64EncodedString(options: []);
                                
                                // URL encode. Please be aware not to double URL encode it further.
                                samlResponse = samlResponse?.addingPercentEncoding(withAllowedCharacters: CharacterSet.init(charactersIn: "!*'();:@&=+$,/?%#[]").inverted);
                                
                                // Continue with the "Exchange the Partner SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Continue with the "Initiate regular authentication workflow" step.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Passaggio: &quot;Sostituire il profilo SSO partner con un token di autenticazione Adobe&quot; {#step8}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementalo tramite il servizio API [Token Exchange](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) di autenticazione di Adobe Pass.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Tieni presente il frammento di codice del passaggio [&quot;Inoltra la richiesta Adobe all&#39;SSO partner per ottenere il profilo&quot;](#step7). *`vsaMetadata!.samlAttributeQueryResponse!`* rappresenta *`SAMLResponse`*, che deve essere passato a [Token Exchange](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) e richiede la manipolazione delle stringhe e la codifica (*Base64* codificata e *URL* codificata successivamente) prima di effettuare la chiamata.

#### Passaggio: &quot;Il token Adobe è stato generato correttamente?&quot; {#step9}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementalo tramite la risposta corretta di autenticazione di Adobe Pass [Scambio token](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md), che sarà un *`204 No Content`*, che indica che il token è stato creato correttamente ed è pronto per essere utilizzato per i flussi di autorizzazione.

#### Passaggio: &quot;Avvia flusso di lavoro di autenticazione regolare&quot; {#step10}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite l&#39;autenticazione Adobe Pass [Richiesta codice di registrazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md), [Avvia autenticazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) e [Recupera token di autenticazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) o [Controlla i servizi API del token di autenticazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md).

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi seguenti per le implementazioni tvOS.

* L&#39;applicazione dovrebbe [ottenere un codice di registrazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) e presentarlo all&#39;utente finale sul primo dispositivo (schermo).
* L&#39;applicazione deve avviare il [polling per riconoscere lo stato di autenticazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sul primo dispositivo (schermo) dopo l&#39;ottenimento del codice di registrazione.
* Un&#39;altra applicazione dovrebbe [avviare l&#39;autenticazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) su un secondo dispositivo (schermo) quando viene utilizzato il codice di registrazione.
* L&#39;applicazione dovrebbe interrompere il [polling](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sul primo dispositivo (schermo) quando viene generato il token di autenticazione.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi indicati di seguito per l&#39;implementazione iOS/iPadOS.

* L&#39;applicazione deve [ottenere un codice di registrazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) che non deve essere presentato all&#39;utente finale sul primo dispositivo (schermo).
* L&#39;applicazione deve [avviare l&#39;autenticazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) sul primo dispositivo (schermo) utilizzando il codice di registrazione e un componente [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
* L&#39;applicazione dovrebbe avviare il [polling per conoscere lo stato di autenticazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sul primo dispositivo (schermo) dopo la chiusura del componente [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
* L&#39;applicazione dovrebbe interrompere il [polling](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sul primo dispositivo (schermo) quando viene generato il token di autenticazione.

#### Passaggio: &quot;Procedi con i flussi di autorizzazione&quot; {#step11}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementalo tramite l&#39;autenticazione Adobe Pass [Avvia autorizzazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) e [Ottieni token di file multimediali brevi](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) servizi API.

### Disconnetti {#apple-sso-cookbook-rest-api-v1-logout}

Il framework dell&#39;account del sottoscrittore video [1&rbrace; non fornisce un&#39;API per disconnettere a livello di programmazione gli utenti che hanno effettuato l&#39;accesso al proprio account del provider TV a livello di sistema del dispositivo. &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) Pertanto, affinché la disconnessione diventi effettiva, l&#39;utente finale dovrà disconnettersi esplicitamente da *`Settings -> TV Provider`* su iOS/iPadOS o da *`Settings -> Accounts -> TV Provider`* su tvOS. L&#39;altra opzione che l&#39;utente avrebbe è quella di revocare l&#39;autorizzazione per accedere alle informazioni sull&#39;abbonamento dell&#39;utente dalla sezione delle impostazioni specifiche dell&#39;applicazione (accesso al provider TV).

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite la [chiamata metadati utente](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) e i servizi API [Logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) per l&#39;autenticazione di Adobe Pass.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi seguenti per le implementazioni tvOS.

* L&#39;applicazione dovrebbe determinare se l&#39;autenticazione è avvenuta a seguito di un accesso tramite l&#39;SSO partner o meno, utilizzando &quot;*tokenSource&quot;* [metadati utente](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) dal servizio di autenticazione di Adobe Pass.
* L&#39;applicazione dovrebbe indicare/richiedere all&#39;utente di disconnettersi esplicitamente da *`Settings -> Accounts -> TV Provider`* in tvOS **only** se il valore *&quot;tokenSource&quot;* è uguale a &quot;*Apple&quot;.*
* L&#39;applicazione dovrebbe [avviare la disconnessione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) dal servizio di autenticazione di Adobe Pass utilizzando una chiamata HTTP diretta. Questo non faciliterebbe la pulizia delle sessioni sul lato MVPD.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi indicati di seguito per l&#39;implementazione iOS/iPadOS.

* L&#39;applicazione dovrebbe determinare se l&#39;autenticazione è avvenuta a seguito di un accesso tramite l&#39;SSO partner o meno, utilizzando &quot;*tokenSource&quot;* [metadati utente](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) dal servizio di autenticazione di Adobe Pass.
* L&#39;applicazione dovrebbe indicare/richiedere all&#39;utente di disconnettersi esplicitamente da *`Settings -> TV Provider`* su iOS/iPadOS **only** nel caso in cui il valore *&quot;tokenSource&quot;* sia uguale a *&quot;Apple&quot;*.
* L&#39;applicazione deve [avviare la disconnessione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) dal servizio di autenticazione di Adobe Pass utilizzando un componente [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller). Questo faciliterebbe la pulizia delle sessioni sul lato MVPD.
