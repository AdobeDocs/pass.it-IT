---
title: Guida alla migrazione ad iOS/tvOS v3.x
description: Guida alla migrazione ad iOS/tvOS v3.x
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%

---

# Guida alla migrazione ad iOS/tvOS v3.x {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

>[!TIP]
> 
> **Note:**
>
> - A partire dall’sdk di iOS versione 3.1, gli implementatori possono ora utilizzare WKWebView o UIWebView in modo intercambiabile. Poiché UIWebView è obsoleto, le app devono migrare a WKWebView per evitare problemi con le versioni future di iOS.
> - Tieni presente che la migrazione implicherebbe semplicemente il passaggio della classe UIWebView a WKWebView; non è necessario eseguire alcun lavoro specifico in merito all’AccessEnabler di Adobe.

</br>

## Aggiornare le impostazioni della build {#update}

Questa versione contiene funzionalità scritte in linguaggio SWIFT. Se la tua app è completamente Objective-C, devi impostare su &quot;Sì&quot; la casella di controllo &quot;Incorpora sempre le librerie Swift standard&quot; nelle impostazioni di build della tua destinazione. Quando questa opzione è impostata, Xcode analizza i framework inclusi nell’app e, se uno di essi contiene codice Swift, copia le librerie pertinenti nel bundle dell’app. Se non aggiorni le impostazioni della build, l’app potrebbe bloccarsi a causa di errori che indicano che non è possibile caricare AccessEnabler.framework o vari `ibswift*` librerie.

</br>

## Aggiunta dell&#39;istruzione software {#add}

> Per informazioni su come ottenere l&#39;informativa sul software, vedere
> pagina:
> [Registrazione applicazione](/help/authentication/iostvos-application-registration.md)

Una volta ottenuta l&#39;istruzione software, si consiglia di ospitarla su un server remoto in modo da poterla revocare o modificare facilmente senza distribuire una nuova versione dell&#39;applicazione in App Store. All&#39;avvio dell&#39;applicazione, ottenere l&#39;istruzione software dalla posizione remota e trasmetterla nel costruttore AccessEnabler:

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> Informazioni API qui: [Riferimento API per iOS/tvOS](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## Aggiungere lo schema URL personalizzato {#add-custom}

> Per informazioni su come ottenere uno schema URL personalizzato, consulta questa pagina: [Ottenere uno schema URL cliente](/help/authentication/iostvos-application-registration.md)

Dopo aver ottenuto lo schema URL personalizzato, devi aggiungerlo al file info.plist dell’applicazione. Lo schema personalizzato ha il seguente formato: `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. È necessario omettere i due punti e le barre in avanti quando lo si aggiunge al file. L’esempio precedente diventerà `adbe.u-XFXJeTSDuJiIQs0HVRAg`.

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>CUSTOM_URL_SCHEME_HERE</string>
            </array>
        </dict>
    </array>
```

</br>

## Intercettazione delle chiamate sullo schema URL personalizzato {#intercept}

Questo vale solo nel caso in cui l&#39;applicazione abbia precedentemente abilitato la gestione manuale di Safari View Controller (SVC) tramite [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](/help/authentication/iostvos-sdk-api-reference.md) e per MVPD specifici che richiedono Safari View Controller (SVC), richiedendo pertanto il caricamento degli URL degli endpoint di autenticazione e disconnessione da parte di un controller SFSafariViewController anziché da un controller UIWebView/WKWebView.

Durante i flussi di autenticazione e disconnessione, l&#39;applicazione deve monitorare l&#39;attività del `SFSafariViewController `il controller durante diversi reindirizzamenti. L&#39;applicazione deve rilevare il momento in cui carica un URL personalizzato specifico definito dal `application's custom URL scheme` (ad es.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`. Quando il controller carica questo URL personalizzato specifico, l&#39;applicazione deve chiudere `SFSafariViewController` e chiama AccessEnabler `handleExternalURL:url `metodo API.

Nel tuo `AppDelegate` aggiungi il seguente metodo:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> Informazioni API qui: [Gestisci URL esterno](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## Aggiorna la firma del metodo setRequestor {#update-setreq}

Poiché il nuovo SDK utilizza un nuovo meccanismo di autenticazione, non sono necessari il parametro signedRequestId o la chiave pubblica e il segreto (per tvOS). Il `setRequestor` Il metodo è semplificato e richiede solo il valore requestorID.

### iOS

Questo codice:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

diventa:

```swift
    accessEnabler.setRequestor(requestorId)
```

</br>

### tvOS

Questo codice:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId,
                    secret: "secret", publicKey: "public_key")
```

diventa:

```swift
    accessEnabler.setRequestor(requestorId)
```

> Informazioni API qui: [Imposta richiedente](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## Sostituisci il metodo getAuthenticationToken con il metodo handleExternalURL {#replace}

`getAuthentication` è stato utilizzato in passato per completare il flusso di autenticazione. Poiché il suo nome era fuorviante, è stato rinominato in `handleExternalURL` e considera l’url come un parametro.

Modifica tutte le occorrenze di questo:

```swift
    accessEnabler.getAuthenticationToken()
```

in questo:

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> Informazioni API qui: [Gestisci URL esterno](/help/authentication/iostvos-sdk-api-reference.md)
