---
title: Manuale Apple SSO (REST API)
description: Manuale Apple SSO (REST API)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1435'
ht-degree: 0%

---

# Manuale Apple SSO (REST API) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#Introduction}

L’API REST per l’autenticazione di Adobe Pass Authentication può supportare l’autenticazione Single Sign-On (SSO) della piattaforma per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS tramite quello che chiamiamo flusso di lavoro SSO di Apple.

Questo documento funge da estensione della documentazione REST API esistente, accessibile [qui](/help/authentication/rest-api-reference.md).

## Cookbook {#Cookbooks}

Per beneficiare dell’esperienza utente SSO di Apple, un’applicazione dovrebbe integrare [Account abbonato video](https://developer.apple.com/documentation/videosubscriberaccount) framework sviluppato da Apple, per quanto riguarda la comunicazione API REST per l’autenticazione di Adobe Pass, dovrebbe seguire la sequenza di suggerimenti presentata di seguito.

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
> **<u>Suggerimento</u>** Implementare questa soluzione tramite [Autenticazione Adobe Pass](/help/authentication/check-authentication-token.md) servizio.


#### Passaggio: &quot;L’utente ha effettuato l’accesso tramite SSO a Platform?&quot; {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>Suggerimento</u>** Implementare questa soluzione tramite [Account abbonato video](https://developer.apple.com/documentation/videosubscriberaccount) infrastruttura.

- L’applicazione dovrebbe verificare la presenza di [autorizzazione di accesso](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) le informazioni di abbonamento dell’utente e procedi solo se l’utente lo ha consentito.
- La domanda dovrebbe presentare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per informazioni sull’account dell’abbonato.
- L’applicazione deve attendere ed elaborare il [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informazioni.



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
> **<u>Suggerimento</u>** Implementare questa soluzione tramite [Autenticazione Adobe Pass](/help/authentication/provide-mvpd-list.md) servizio.


>[!TIP]
>
> **<u>Suggerimento pro:</u>** Tieni presente le proprietà MVPD: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* e prestare particolare attenzione ai commenti presentati nei frammenti di codice di altri passaggi.

#### Passaggio &quot;Avviare il flusso di lavoro SSO di Platform con la configurazione Adobe&quot; {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>Suggerimento</u>** Implementare questa soluzione tramite [Account abbonato video](https://developer.apple.com/documentation/videosubscriberaccount) infrastruttura.

- L’applicazione dovrebbe verificare la presenza di [autorizzazione di accesso](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) le informazioni di abbonamento dell’utente e procedi solo se l’utente lo ha consentito.
- La domanda dovrebbe fornire un [delegare](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) per VSAccountManager.
- La domanda dovrebbe presentare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per informazioni sull’account dell’abbonato.
- L’applicazione deve attendere ed elaborare il [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informazioni.



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
> **<u>Suggerimento pro:</u>** Tieni presente il frammento di codice da [&quot;Avvia il flusso di lavoro SSO di Platform con la configurazione Adobe&quot;](#Initiate_Platform_SSO_workflow_with_Adobe_config) passaggio. L’accesso utente ha esito positivo se *`vsaMetadata!.accountProviderIdentifier`* contiene un valore valido e la data corrente non ha superato il *`vsaMetadata!.authenticationExpirationDate`* valore.

#### Passaggio &quot;Ottieni una richiesta di profilo da Adobe per l’MVPD selezionato&quot; {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>Suggerimento</u>** Implementare questa impostazione tramite l’autenticazione di Adobe Pass [Richiesta profilo](/help/authentication/retrieve-profilerequest.md) servizio.

>[!TIP]
>
> **<u>Suggerimento pro:</u>** L’identificatore del provider ottenuto dal framework dell’account dell’abbonato video rappresenta *`platformMappingId`* in termini di configurazione dell’autenticazione di Adobe Pass. Pertanto, l&#39;applicazione deve determinare il valore della proprietà ID MVPD, utilizzando *`platformMappingId`* tramite Adobe Pass Authentication [Fornisci elenco MVPD](/help/authentication/provide-mvpd-list.md) servizio.

#### Passaggio: &quot;Inoltra la richiesta di Adobe all’SSO della piattaforma per ottenere il profilo&quot; {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>Suggerimento</u>** Implementare questa soluzione tramite [Account abbonato video](https://developer.apple.com/documentation/videosubscriberaccount) infrastruttura.


- L’applicazione dovrebbe verificare la presenza di [autorizzazione di accesso](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) le informazioni di abbonamento dell’utente e procedi solo se l’utente lo ha consentito.
- La domanda dovrebbe presentare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per informazioni sull’account dell’abbonato.
- L’applicazione deve attendere ed elaborare il [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) informazioni.



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
> **<u>Suggerimento</u>** Implementare questa impostazione tramite l’autenticazione di Adobe Pass [Scambio di token](/help/authentication/token-exchange.md) servizio.


>[!TIP]
>
> **<u>Suggerimento pro:</u>** Tieni presente il frammento di codice da [&quot;Inoltra la richiesta di Adobe all’SSO della piattaforma per ottenere il profilo&quot;](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile) passaggio. Questo *`vsaMetadata!.samlAttributeQueryResponse!`* rappresenta il *`SAMLResponse`*, che deve essere trasmesso [Scambio di token](/help/authentication/token-exchange.md) e richiede la manipolazione e la codifica delle stringhe (*Base64* codificato e *URL* codificati successivamente) prima di effettuare la chiamata.

</br>

#### Passaggio: &quot;Il token di Adobe è stato generato correttamente?&quot; {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>Suggerimento</u>** Implementarlo tramite l’autenticazione Adobe Pass di supporto [Scambio di token](/help/authentication/token-exchange.md) risposta corretta, che sarà un *`204 No Content`*, che indica che il token è stato creato correttamente ed è pronto per essere utilizzato per i flussi di autorizzazione.

</br>

#### Passaggio: &quot;Avvia il flusso di lavoro di autenticazione nella seconda schermata&quot; {#Initiate_second_screen_authentication_workflow}

**Importante:** La terminologia &quot;Flusso di lavoro di autenticazione secondo schermo&quot; è appropriata per AppleTV, mentre la terminologia &quot;Flusso di lavoro di autenticazione primo schermo&quot; / &quot;Flusso di lavoro di autenticazione regolare&quot; sarebbe più appropriata per iPhone e iPad.


>[!TIP]
>
> **<u>Suggerimento</u>** Implementare questa impostazione tramite l’autenticazione di Adobe Pass

[Richiesta codice di registrazione](/help/authentication/registration-code-request.md), [Avvia autenticazione](/help/authentication/initiate-authentication.md) e [Recupero token di autenticazione API REST](/help/authentication/retrieve-authentication-token.md) o [Controlla token di autenticazione](/help/authentication/check-authentication-token.md) servizi.


>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi seguenti per le implementazioni tvOS.

- L&#39;applicazione dovrebbe [ottenere un codice di registrazione](/help/authentication/registration-code-request.md) e presentarlo all&#39;utente finale sul primo dispositivo (schermo).
- L&#39;applicazione dovrebbe iniziare [polling per riconoscere lo stato di autenticazione](/help/authentication/retrieve-authentication-token.md) sul primo dispositivo (schermo) dopo aver ottenuto il codice di registrazione.
- Un&#39;altra applicazione dovrebbe [avvia autenticazione](/help/authentication/initiate-authentication.md) su un secondo dispositivo (schermo) quando viene utilizzato il codice di registrazione.
- L&#39;applicazione dovrebbe essere interrotta [sondaggio](/help/authentication/retrieve-authentication-token.md) sul primo dispositivo (schermata) quando viene generato il token di autenticazione.



>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi seguenti per le implementazioni iOS/iPadOS.

- L&#39;applicazione dovrebbe [ottenere un codice di registrazione](/help/authentication/registration-code-request.md) che non deve essere presentato all&#39;utente finale sul primo dispositivo (schermo).
- L&#39;applicazione dovrebbe [avvia autenticazione](/help/authentication/initiate-authentication.md) sul primo dispositivo (schermo) utilizzando il codice di registrazione e un [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o un [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) componente.
- L&#39;applicazione dovrebbe iniziare [polling per conoscere lo stato di autenticazione](/help/authentication/retrieve-authentication-token.md) sul primo dispositivo (schermo) dopo [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) il componente viene chiuso.
- L&#39;applicazione dovrebbe essere interrotta [sondaggio](/help/authentication/retrieve-authentication-token.md) sul primo dispositivo (schermata) quando viene generato il token di autenticazione.

</br>

#### Passaggio: &quot;Procedi con i flussi di autorizzazione&quot; {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>Suggerimento</u>** Implementare questa impostazione tramite l’autenticazione di Adobe Pass [Avvia autorizzazione](/help/authentication/initiate-authorization.md) e [Ottieni token multimediale breve](/help/authentication/obtain-short-media-token.md) servizi.

</br>

### Disconnetti {#Logout}

Il [Account abbonato video](https://developer.apple.com/documentation/videosubscriberaccount) Il framework non fornisce un&#39;API per disconnettere programmaticamente gli utenti che hanno effettuato l&#39;accesso al proprio account di provider TV a livello di sistema del dispositivo. Pertanto, affinché la disconnessione diventi effettiva, l&#39;utente finale dovrà disconnettersi esplicitamente da *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS. L&#39;altra opzione che l&#39;utente avrebbe è quella di revocare l&#39;autorizzazione per accedere alle informazioni sull&#39;abbonamento dell&#39;utente dalla sezione delle impostazioni specifiche dell&#39;applicazione (accesso al provider TV).

>[!TIP]
>
> **<u>Suggerimento</u>** Implementare questa impostazione tramite l’autenticazione di Adobe Pass [Chiamata metadati utente](/help/authentication/user-metadata.md) e [Disconnetti](/help/authentication/initiate-logout.md) servizi.


>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi seguenti per le implementazioni tvOS.


- L’applicazione dovrebbe determinare se l’autenticazione è avvenuta a seguito di un accesso tramite l’SSO della piattaforma o meno, utilizzando il comando &quot;*tokenSource&quot;* [metadati utente](/help/authentication/user-metadata.md) dal servizio Adobe Pass Authentication.
- L’applicazione deve indicare/richiedere all’utente di disconnettersi esplicitamente da *`Settings -> Accounts -> TV Provider`* su tvOS **solo** se il *&quot;tokenSource&quot;* il valore è uguale a &quot;*Apple&quot;.*
- L&#39;applicazione dovrebbe [avvia la disconnessione](/help/authentication/initiate-logout.md) dal servizio Adobe Pass Authentication tramite una chiamata HTTP diretta. Ciò non faciliterebbe la pulizia delle sessioni da parte di MVPD.



>[!TIP]
>
> **<u>Suggerimento pro:</u>** Segui i passaggi seguenti per le implementazioni iOS/iPadOS.

- L’applicazione dovrebbe determinare se l’autenticazione è avvenuta a seguito di un accesso tramite l’SSO della piattaforma o meno, utilizzando il comando &quot;*tokenSource&quot;* [metadati utente](/help/authentication/user-metadata.md) dal servizio Adobe Pass Authentication.
- L’applicazione deve indicare/richiedere all’utente di disconnettersi esplicitamente da *`Settings -> TV Provider`* su iOS/iPadOS **solo** se il *&quot;tokenSource&quot;* il valore è uguale a *&quot;Apple&quot;*.
- L&#39;applicazione dovrebbe [avvia la disconnessione](/help/authentication/initiate-logout.md) dal servizio di autenticazione di Adobe Pass tramite un [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o un [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) componente. Questo faciliterebbe la pulizia delle sessioni da parte di MVPD.

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
