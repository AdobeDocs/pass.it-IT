---
title: Preautorizzazione API iOS/tvOS
description: Preautorizzazione API iOS/tvOS
exl-id: 79c596a4-0e38-4b6c-bb85-f97c6af45ed8
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 0%

---

# (Legacy) Preautorizza {#preauthorize}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

L’API di preautorizzazione può essere utilizzata per ottenere una decisione di preautorizzazione per una o più risorse, in questo modo l’applicazione può implementare hint di interfaccia utente e/o filtri dei contenuti.

>[!IMPORTANT]
>
>Prima di concedere all&#39;utente l&#39;accesso alle risorse specificate, è necessario utilizzare l&#39;API di autorizzazione **1&rbrace;.**

Nel caso in cui il risultato della risposta API di preautorizzazione contenga una o più risorse con una decisione di preautorizzazione negata, è possibile includere ulteriori informazioni sull&#39;errore **(vedi la nota seguente)** per ciascuna risorsa interessata.

>[!IMPORTANT]
>
>La funzione avanzata di segnalazione degli errori, che aggiunge informazioni di errore aggiuntive per le decisioni di preautorizzazione negate, è disponibile su richiesta in quanto deve essere abilitata sul lato configurazione autenticazione di Adobe Pass.

Nel caso in cui la richiesta API di preautorizzazione non possa essere gestita a causa di un errore di Adobe Pass Authentication SDK o nel caso in cui si verifichi un errore di Adobe Pass Authentication Services, verranno incluse ulteriori informazioni sull’errore (indipendentemente dalla configurazione di cui sopra) e nessuna risorsa come parte del risultato della risposta API di preautorizzazione.

</br>

## `- (void) preauthorize:(nonnull PreauthorizeRequest *)request didCompleteWith:(nonnull AccessEnablerCallback<PreauthorizeResponse *> *)callback;`


**Disponibilità:** v3.6.0+

**Parametri:**

- PreauthorizeRequest: oggetto della richiesta utilizzato per trasmettere il contenuto della richiesta API;
- AccessEnablerCallback: oggetto callback utilizzato per restituire la risposta API;
- PreauthorizeResponse: oggetto di risposta utilizzato per restituire il contenuto della risposta API;


</br>

## `class PreauthorizeRequest`{#androidpreauthorizerequest}

### **class PreauthorizeRequest.Builder**

```
    ///
    /// Sets the `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// Each element in the list must be a `String` representing either the resource ID value or the media RSS fragment which must be agreed with the MVPD.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    ///
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter resources: The `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
    public func setResources(resources: [String]) -> PreauthorizeRequest.Builder

 

    ///
    /// Sets the features which you want to have them disabled when obtaining preauthorization decisions.
    ///
    /// The list of available features are provided by `PreauthorizeRequest.Feature` enumeration. All features are enabled by default.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    ///
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter features: The set of features which you want to have them disabled.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
    public func disableFeatures(features: Set<PreauthorizeRequest.Feature>) -> PreauthorizeRequest.Builder

 

    ///
    /// Creates and retrieves the reference of a new `PreauthorizeRequest` object instance.
    ///
    /// This function instantiates a new `PreauthorizeRequest` object every time it is called.
    ///
    /// This function uses the values set in advance in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// Bear in mind that this function does not produce any side effects, therefore it does not alter the state of the SDK or the state of the `Builder` object instance which is the receiver of this function call.
    ///
    /// It means that successive calls of this function for the same receiver will create different new `PreauthorizeRequest` object instances, but having the same information, in case the values set to the `Builder` where not modified between the calls.
    ///
    /// In case you do not need to update any of the provided information (resources, features, etc.) you may reuse the `PreauthorizeRequest` instance for multiple uses of the `preauthorize` API.
    ///
    /// - Returns: The reference to a new `PreauthorizeRequest` object instance.
    ///
    public func build() -> PreauthorizeRequest
```


## **enum PreauthorizeRequest.Feature**

```
    ///
    /// This feature controls whether to use the information from the AccessEnabler SDK cache or to bypass it and
    /// rely on Adobe Pass server information via a network call.
    ///
    LOCAL_CACHE

    ///
    /// This feature controls whether to use the information from the Adobe Pass server cache or to bypass it and
    /// rely on MVPD server information via a network call.
    ///
    REMOTE_CACHE
```

</br>

## `interface AccessEnablerCallback<PreauthorizeResponse>` {#accessenablercallback}

```
    /// Response callback called by the SDK when the preauthorize API request was fulfilled. The result is either a successful or an error result containing a status.
    public func onResponse(result: PreauthorizeResponse)


    /// Failure callback called by the SDK when the preauthorize API request could not be serviced. The result is a failure result containing a status. 
    public func onFailure(result: PreauthorizeResponse)
```

</br>


## `class PreauthorizeResponse` {#preauthorizeresponse}

```
    ///
    /// - Returns: Additional status (state) information in case of error or failure.
    ///   Might hold a `nil` value.
    ///
    public Status getStatus()

    ///
    /// - Returns: The list of preauthorization decisions. One decision for each resource.
    ///            The list might be empty in case of error or failure.
    ///
    public List<Decision> getDecisions()
```

### Esempi:

Questa sezione evidenzia la struttura JSON di alcuni possibili oggetti PreauthorizeResponse.

>[!IMPORTANT]
>
>I JSON presentati dagli esempi seguenti sono accessibili solo tramite le classi modello mostrate in questo documento. Potrai accedere alle proprietà di tali JSON solo tramite i metodi pubblici.

>[!IMPORTANT]
>
>L&#39;elenco dei possibili errori aggiuntivi recuperati tramite la funzionalità di segnalazione errori avanzata è documentato in [Segnalazione errori avanzata](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

#### Completato

Tutte le risorse richieste hanno una decisione positiva di autorizzazione

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": true 
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


Una o più risorse hanno una decisione di preautorizzazione negata e la funzione avanzata di segnalazione degli errori non è abilitata nella configurazione di autenticazione di Adobe Pass

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": false,
                 
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


Una o più risorse hanno una decisione di preautorizzazione negata e la funzione avanzata di segnalazione degli errori è abilitata nella configurazione di autenticazione di Adobe Pass

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": false,
                "error" : {
                   "status" : 403,
                   "code" : "authorization_denied_by_mvpd",
                   "message" : "User not authorized",
                   "details" : "Your subscription package does not include the "TestStream3" channel.",
                   "helpUrl" : "https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=it",
                   "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
                   "action" : "none"
                }
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


#### Errore



I servizi di autenticazione di Adobe Pass hanno riscontrato un errore durante la manutenzione della richiesta API di preautorizzazione

```JSON
    {
        "resources": [],
        "status": {
            "status": 400,
            "code" : "bad_request",
            "message": "Missing required parameter : deviceId",
            "details": "",
            "helpUrl" : "https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=it",
            "trace" : "9f115e1c-0158-4a41-8805-9f68923f3646",
            "action" : "none"
        }
    }
```


#### Errore

Adobe Pass Authentication SDK restituisce un errore durante la manutenzione della richiesta API di preautorizzazione

```JSON
    {
        "status": 0,
        "code": "requestor_not_configured",
        "message": "The requestor is not yet configured which is a prerequisite for using any API apart from the setRequestor API.",
        "action": "retry"
    }
    
    {
        "status": 0,
        "code": "authentication_session_expired",
        "message": "The current authentication session has expired. The user must re-authenticate with a supported MVPD in order to continue.",
        "action": "authentication"
    }
    
    {
        "status": 0,
        "code": "authentication_session_missing",
        "message": "The authentication session associated with this request could not be retrieved. The user must re-authenticate with a supported MVPD in order to continue.",
        "action": "authentication"
    }
    
    {
        "status": 0,
        "code": "access_token_unavailable",
        "message": "The request failed due to an unexpected error while retrieving the access token. Please check the validity of your software statement and custom scheme.",
        "action": "none"
    }
    
    {
        "status": 0,
        "code": "server_response_format_unknown",
        "message": "The request failed due to an unknown server response format. Please capture the device console logs and contact support.",
        "action": "none"
    }
    
    {
        "status": 0,
        "code": "network_error",
        "message": "The request failed due to a network error. If the issue persists over several retries, please capture the device console logs and contact support.",
        "action": "none"
    }
```

</br>

## **stato classe** {#status}

```
    ///
    /// - Returns: The HTTP response status code as documented in RFC 7231.
    ///            Might be 0 in case the `Status` comes from the SDK instead of Adobe Pass Authentication services.
    ///
    public int getStatus()

    ///
    /// - Returns: The standard Adobe Pass Authentication services error code.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getCode()

    ///
    /// - Returns: The human readable message which can be displayed to the end user.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getMessage()

    ///
    /// - Returns: The detailed message which in some cases is provided by the MVPD authorization endpoints or by Programmer degradation rules.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getDetails()

    ///
    /// - Returns: The URL that links to more information about why this state/error occurred and possible solutions.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getHelpUrl()

    ///
    /// - Returns: The unique identifier for this response, which can be used when contacting support to identify specific issues in more complex scenarios.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getTrace()

    ///
    /// - Returns: The recommended action to remediate the situation.
    ///             - none: Unfortunately there is no predefined action to remediate this issue. This might indicate an improper invocation of the public API
    ///             - configuration: A configuration change is needed through TVE dashboard or by contacting support.
    ///             - application-registration: The application must register itself again.
    ///             - authentication: The user must authenticate or re-authenticate.
    ///             - authorization: The user must obtain authorization for the specific resource.
    ///             - degradation: Some form of degradation should be applied.
    ///             - retry: Retrying the request might solve the issue
    ///             - retry-after: Retrying the request after the indicated period of time might solve the issue.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getAction()
```

<br>

## **decisione classe** {#decision}

```
    ///
    /// This is a getter function.
    ///
    /// - Returns: The resource id for which the decision was obtained.
    ///
    public Status getId()

    ///
    /// This is a getter function.
    ///
    /// - Returns: The value of the flag indicating if the decision is successful or not.
    ///
    public boolean isAuthorized()

    ///
    /// This is a getter function.
    ///
    /// - Returns: Additional status (state) information in case some error has occurred.
    ///            Might hold a `nil` value.
    ///
    public Status getError()
```

</br>


## **Codice di esempio** {#sample}

```
let resources: [String] = ["resource_1", "resource_2", "resource_3"];

let disabledFeatures: Set<PreauthorizationRequest.Feature> = [PreauthorizationRequest.Feature.LOCAL_CACHE];

// Build the Preauthorization API request using the builder from PreauthorizationRequest class`

let request: PreauthorizationRequest = PreauthorizationRequest.Builder()

                  .setResources(resources: resources)


                  .disableFeatures(features: disabledFeatures)  // It is **optional** to disable features. If not used all features are enabled by default.

                  .build();

// Build the AccessEnablerCallback by providing the constructor two callbacks for onResponse and onFailure handling  
func onResponseCallback(result: PreauthorizeResponse) -> Void {  //
TODO };

func onFailureCallback(result: PreauthorizeResponse) -> Void {

// TODO

};

let callback: AccessEnablerCallback<PreauthorizeResponse> = AccessEnablerCallback<PreauthorizeResponse>(onResponse: onResponseCallback, onFailure: onFailureCallback);

// Use the preauthorize API
accessEnabler.preauthorize(request, callback);
```
