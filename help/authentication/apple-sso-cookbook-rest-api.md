---
title: Manuale Apple SSO (REST API)
description: Manuale Apple SSO (REST API)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---

# Manuale Apple SSO (REST API) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#Introduction}

L’API REST per l’autenticazione di Adobe Pass Authentication può supportare l’autenticazione Single Sign-On (SSO) della piattaforma per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS tramite quello che chiamiamo flusso di lavoro SSO di Apple.

Tieni presente che questo documento funge da estensione della documentazione API REST esistente, disponibile [qui](/help/authentication/rest-api-reference.md).

## Cookbook {#Cookbooks}

Per beneficiare dell&#39;esperienza utente SSO di Apple, un&#39;applicazione dovrebbe integrare il framework [Video Subscriber Account](https://developer.apple.com/documentation/videosubscriberaccount) sviluppato da Apple, mentre per quanto riguarda la comunicazione API REST per l&#39;autenticazione di Adobe Pass, dovrebbe seguire la sequenza di suggerimenti presentata di seguito.

### Autenticazione {#Authentication}

- [Esiste un token di autenticazione Adobe valido?](#Is_there_a_valid_Adobe_authentication_token)
- [L’utente ha effettuato l’accesso tramite Platform SSO?](#Is_the_user_logged_in_via_Platform_SSO)
- [Recupera configurazione Adobe](#Fetch_Adobe_configuration)
- [Avvia il flusso di lavoro SSO di Platform con la configurazione Adobe](#Initiate_Platform_SSO_workflow_with_Adobe_config)
- [L’accesso dell’utente è riuscito?](#Is_user_login_successful)
- [Ottieni una richiesta di profilo da Adobe per l’MVPD selezionato](#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD)
- [Inoltra la richiesta di Adobe all’SSO della piattaforma per ottenere il profilo](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)
- [Scambia il profilo SSO di Platform con un token di autenticazione Adobe](#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token)
- [Il token di Adobe è stato generato correttamente?](#Is_Adobe_token_generated_successfully)
- [Avvia flusso di lavoro di autenticazione nella seconda schermata](#Initiate_second_screen_authentication_workflow)
- [Procedi con i flussi di autorizzazione](#Proceed_with_authorization_flows)


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/qu/platform-sso.jpeg)


#### Passaggio: &quot;Esiste un token di autenticazione Adobe valido?&quot; {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite il servizio [Autenticazione Adobe Pass](/help/authentication/check-authentication-token.md).


#### Passaggio: &quot;L’utente ha effettuato l’accesso tramite SSO a Platform?&quot; {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite il framework [Account del sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount).

- L&#39;applicazione dovrebbe verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente e procedere solo se l&#39;utente lo consente.
- L&#39;applicazione dovrebbe inviare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per le informazioni sull&#39;account del sottoscrittore.
- L&#39;applicazione dovrebbe attendere ed elaborare le informazioni [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



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
                    
                    // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                            // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Fallback to regular authentication workflow.
                ...
            }
}
...  
```

#### Passaggio: &quot;Recupera configurazione Adobe&quot; {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite il servizio [Autenticazione Adobe Pass](/help/authentication/provide-mvpd-list.md).


>[!TIP]
>
> **<u>Suggerimento pro:</u>** Tieni presente le proprietà MVPD: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* e presta particolare attenzione ai commenti presentati nei frammenti di codice di altri passaggi.

#### Passaggio &quot;Avviare il flusso di lavoro SSO di Platform con la configurazione Adobe&quot; {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite il framework [Account del sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount).

- L&#39;applicazione dovrebbe verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente e procedere solo se l&#39;utente lo consente.
- L&#39;applicazione deve fornire un [delegato](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) per VSAccountManager.
- L&#39;applicazione dovrebbe inviare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per le informazioni sull&#39;account del sottoscrittore.
- L&#39;applicazione dovrebbe attendere ed elaborare le informazioni [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



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
                        
                        // This is going to make the Video Subscriber Account framework to prompt the user with the providers picker at this step. 
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
                                // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
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
                                            // Continue with the "Initiate second screen authentcation workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate second screen authentcation workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate second screen authentcation workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate second screen authentcation workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Passaggio: &quot;L’accesso dell’utente è riuscito?&quot; {#Is_user_login_successful}

>[!TIP]
>
> **<u>Suggerimento pro:</u>** Tieni presente lo snippet di codice del passaggio [&quot;Avvia flusso di lavoro SSO di Platform con configurazione Adobe&quot;](#Initiate_Platform_SSO_workflow_with_Adobe_config). L&#39;accesso utente ha esito positivo nel caso in cui *`vsaMetadata!.accountProviderIdentifier`* contenga un valore valido e la data corrente non abbia superato il valore *`vsaMetadata!.authenticationExpirationDate`*.

#### Passaggio &quot;Ottieni una richiesta di profilo da Adobe per l’MVPD selezionato&quot; {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementalo tramite il servizio [Richiesta profilo](/help/authentication/retrieve-profilerequest.md) di autenticazione Adobe Pass.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** l&#39;identificatore del provider ottenuto dal framework dell&#39;account del sottoscrittore video rappresenta *`platformMappingId`* in termini di configurazione dell&#39;autenticazione Adobe Pass. Pertanto, l&#39;applicazione deve determinare il valore della proprietà ID MVPD, utilizzando il valore *`platformMappingId`*, tramite il servizio Autenticazione Adobe Pass [Fornisci elenco MVPD](/help/authentication/provide-mvpd-list.md).

#### Passaggio: &quot;Inoltra la richiesta di Adobe all’SSO della piattaforma per ottenere il profilo&quot; {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite il framework [Account del sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount).


- L&#39;applicazione dovrebbe verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente e procedere solo se l&#39;utente lo consente.
- L&#39;applicazione dovrebbe inviare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per le informazioni sull&#39;account del sottoscrittore.
- L&#39;applicazione dovrebbe attendere ed elaborare le informazioni [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



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
                        
                        // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                                
                                // Continue with the "Exchange the Platform SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Fallback to regular authentication workflow.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Passaggio: &quot;Exchange the Platform SSO profile for an Adobe authentication token&quot; (Scambia il profilo SSO di Platform per un token di autenticazione di ) {#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementalo tramite il servizio [Token Exchange](/help/authentication/token-exchange.md) di autenticazione Adobe Pass.


>[!TIP]
>
> **<u>Suggerimento pro:</u>** Tieni presente il frammento di codice dal passaggio [&quot;Inoltra la richiesta di Adobe all&#39;SSO della piattaforma per ottenere il profilo&quot;](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile). *`vsaMetadata!.samlAttributeQueryResponse!`* rappresenta *`SAMLResponse`*, che deve essere passato a [Token Exchange](/help/authentication/token-exchange.md) e richiede la manipolazione delle stringhe e la codifica (*Base64* codificata e *URL* codificata successivamente) prima di effettuare la chiamata.

</br>

#### Passaggio: &quot;Il token di Adobe è stato generato correttamente?&quot; {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementalo tramite la risposta di esito positivo [Scambio token](/help/authentication/token-exchange.md) di autenticazione Adobe Pass di livello medio, che sarà *`204 No Content`*, a indicare che il token è stato creato correttamente ed è pronto per essere utilizzato per i flussi di autorizzazione.

</br>

#### Passaggio: &quot;Avvia il flusso di lavoro di autenticazione nella seconda schermata&quot; {#Initiate_second_screen_authentication_workflow}

**Importante:** la terminologia &quot;Flusso di lavoro di autenticazione secondo schermo&quot; è appropriata per AppleTV, mentre la terminologia &quot;Flusso di lavoro di autenticazione primo schermo&quot; / &quot;Flusso di lavoro di autenticazione regolare&quot; è più appropriata per iPhone e iPad.


>[!TIP]
>
> **<u>Suggerimento:</u>** implementalo tramite l&#39;autenticazione di Adobe Pass

[Richiesta codice di registrazione](/help/authentication/registration-code-request.md), [Avvia autenticazione](/help/authentication/initiate-authentication.md) e [Servizi Recupero token di autenticazione API REST](/help/authentication/retrieve-authentication-token.md) o [Controlla token di autenticazione](/help/authentication/check-authentication-token.md).


>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi seguenti per le implementazioni tvOS.

- L&#39;applicazione dovrebbe [ottenere un codice di registrazione](/help/authentication/registration-code-request.md) e presentarlo all&#39;utente finale sul primo dispositivo (schermo).
- L&#39;applicazione deve avviare il [polling per riconoscere lo stato di autenticazione](/help/authentication/retrieve-authentication-token.md) sul primo dispositivo (schermo) dopo l&#39;ottenimento del codice di registrazione.
- Un&#39;altra applicazione dovrebbe [avviare l&#39;autenticazione](/help/authentication/initiate-authentication.md) su un secondo dispositivo (schermo) quando viene utilizzato il codice di registrazione.
- L&#39;applicazione dovrebbe interrompere il [polling](/help/authentication/retrieve-authentication-token.md) sul primo dispositivo (schermo) quando viene generato il token di autenticazione.



>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi indicati di seguito per l&#39;implementazione iOS/iPadOS.

- L&#39;applicazione deve [ottenere un codice di registrazione](/help/authentication/registration-code-request.md) che non deve essere presentato all&#39;utente finale sul primo dispositivo (schermo).
- L&#39;applicazione deve [avviare l&#39;autenticazione](/help/authentication/initiate-authentication.md) sul primo dispositivo (schermo) utilizzando il codice di registrazione e un componente [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
- L&#39;applicazione dovrebbe avviare il [polling per conoscere lo stato di autenticazione](/help/authentication/retrieve-authentication-token.md) sul primo dispositivo (schermo) dopo la chiusura del componente [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
- L&#39;applicazione dovrebbe interrompere il [polling](/help/authentication/retrieve-authentication-token.md) sul primo dispositivo (schermo) quando viene generato il token di autenticazione.

</br>

#### Passaggio: &quot;Procedi con i flussi di autorizzazione&quot; {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>Suggerimento:</u>** implementalo tramite i servizi Autenticazione Adobe Pass [Avvia autorizzazione](/help/authentication/initiate-authorization.md) e [Ottieni token di file multimediali brevi](/help/authentication/obtain-short-media-token.md).

</br>

### Disconnetti {#Logout}

Il framework [Account sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount) non fornisce un&#39;API per disconnettere a livello di programmazione gli utenti che hanno effettuato l&#39;accesso al proprio account del provider TV a livello di sistema del dispositivo. Pertanto, affinché la disconnessione diventi effettiva, l&#39;utente finale dovrà disconnettersi esplicitamente da *`Settings -> TV Provider`* su iOS/iPadOS o da *`Settings -> Accounts -> TV Provider`* su tvOS. L&#39;altra opzione che l&#39;utente avrebbe è quella di revocare l&#39;autorizzazione per accedere alle informazioni sull&#39;abbonamento dell&#39;utente dalla sezione delle impostazioni specifiche dell&#39;applicazione (accesso al provider TV).

>[!TIP]
>
> **<u>Suggerimento:</u>** implementare questa impostazione tramite la [Chiamata metadati utente](/help/authentication/user-metadata.md) e i servizi [Logout](/help/authentication/initiate-logout.md) di autenticazione Adobe Pass.


>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi seguenti per le implementazioni tvOS.


- L&#39;applicazione dovrebbe determinare se l&#39;autenticazione è avvenuta a seguito di un accesso tramite l&#39;SSO della piattaforma o meno, utilizzando &quot;*tokenSource&quot;* [metadati utente](/help/authentication/user-metadata.md) dal servizio di autenticazione di Adobe Pass.
- L&#39;applicazione dovrebbe indicare/richiedere all&#39;utente di disconnettersi esplicitamente da *`Settings -> Accounts -> TV Provider`* in tvOS **only** se il valore *&quot;tokenSource&quot;* è uguale a &quot;*Apple&quot;.*
- L&#39;applicazione dovrebbe [avviare la disconnessione](/help/authentication/initiate-logout.md) dal servizio di autenticazione di Adobe Pass utilizzando una chiamata HTTP diretta. Ciò non faciliterebbe la pulizia delle sessioni da parte di MVPD.



>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi indicati di seguito per l&#39;implementazione iOS/iPadOS.

- L&#39;applicazione dovrebbe determinare se l&#39;autenticazione è avvenuta a seguito di un accesso tramite l&#39;SSO della piattaforma o meno, utilizzando &quot;*tokenSource&quot;* [metadati utente](/help/authentication/user-metadata.md) dal servizio di autenticazione di Adobe Pass.
- L&#39;applicazione dovrebbe indicare/richiedere all&#39;utente di disconnettersi esplicitamente da *`Settings -> TV Provider`* su iOS/iPadOS **only** nel caso in cui il valore *&quot;tokenSource&quot;* sia uguale a *&quot;Apple&quot;*.
- L&#39;applicazione deve [avviare la disconnessione](/help/authentication/initiate-logout.md) dal servizio di autenticazione di Adobe Pass utilizzando un componente [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller). Questo faciliterebbe la pulizia delle sessioni da parte di MVPD.

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
