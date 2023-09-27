---
title: Errore di autenticazione iOS - Impossibile trovare adobepass.ios.app
description: Errore di autenticazione iOS - Impossibile trovare adobepass.ios.app
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# Errore di autenticazione iOS - Impossibile trovare adobepass.ios.app {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Problema {#issue}

L’utente sta attraversando il flusso di autenticazione e, dopo aver immesso correttamente le credenziali con il proprio provider, viene reindirizzato a una pagina di errore, a una pagina di ricerca o a un’altra pagina personalizzata che informa che `adobepass.ios.app` non trovato/risolto.

## Spiegazione {#explanation}

Su iOS, `adobepass.ios.app` viene utilizzato come URL di reindirizzamento finale per indicare il completamento del flusso AuthN. A questo punto, l’app deve effettuare una richiesta all’AccessEnabler per ottenere il token AuthN e finalizzare il flusso AuthN.

Il problema è che `adobepass.ios.app` non esiste e attiverà un messaggio di errore nel `webView`. Le versioni precedenti di iOS DemoApp presupponevano che questo errore sarebbe sempre stato attivato alla fine del flusso di autenticazione ed era configurato per gestirlo di conseguenza (`indidFailLoadWithError`).

**Nota:** Questo problema è stato risolto nelle versioni successive di DemoApp (incluse con il download dell’SDK per iOS).

Sfortunatamente, questo presupposto NON è corretto. Alcuni server DNS o proxy cosiddetti &quot;intelligenti&quot; non si limitano a trasferire l&#39;errore generato, ma eseguono una delle operazioni seguenti:

- Creare una pagina di errore personalizzata
- Inoltra a una pagina di ricerca o a un altro tipo di pagina del cliente o di portale.

In questi casi, la risposta che ritorna a iOS webView sarà una risposta perfettamente valida per quanto riguarda webView, e NON attiverà l’errore da cui dipendeva la vecchia DemoApp.

## Soluzione {#solution}

NON fare lo stesso presupposto che fa DemoApp. Invece, intercetta la richiesta prima di eseguirla (in `shouldStartLoadWithRequest`) e maneggiarla in modo appropriato.

Esempio di come intercettare la richiesta prima che venga eseguita:

```obj-c
- (BOOL)webView:(UIWebView*)localWebView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {

NSString *absolutePath = [[request URL] absoluteString]; 
if ([absolutePath isEqualToString:ADOBEPASS_REDIRECT_URL] && ![APP_DELEGATE getAuthenticationWasCalled]) {

// user was logged ok => call getAuthenticationToken() 
[APP_DELEGATE setGetAuthenticationWasCalled:YES]; 
[[APP_DELEGATE accessEnabler] getAuthenticationToken];
return NO;

}

return YES;

}
```

Alcune cose da notare:

- NON usare MAI `adobepass.ios.app` direttamente ovunque nel codice. Utilizza invece la costante `ADOBEPASS_REDIRECT_URL`
- Il `return NO;` impedisce il caricamento della pagina
- Assicurati assolutamente che il `getAuthenticationToken` La chiamata di viene chiamata una sola volta nel codice. Più chiamate a `getAuthenticationToken` risulterà in risultati non definiti.
