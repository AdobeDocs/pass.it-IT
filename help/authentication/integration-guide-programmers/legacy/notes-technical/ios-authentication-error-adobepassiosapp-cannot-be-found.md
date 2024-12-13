---
title: Errore di autenticazione iOS - Impossibile trovare adobepass.ios.app
description: Errore di autenticazione iOS - Impossibile trovare adobepass.ios.app
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# (Legacy) Errore di autenticazione iOS - Impossibile trovare adobepass.ios.app {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Problema {#issue}

L&#39;utente sta attraversando il flusso di autenticazione e, dopo aver immesso correttamente le credenziali con il provider, viene reindirizzato a una pagina di errore, a una pagina di ricerca o a un&#39;altra pagina personalizzata per informarlo che `adobepass.ios.app` non è stato trovato/risolto.

## Spiegazione {#explanation}

In iOS, `adobepass.ios.app` viene utilizzato come URL di reindirizzamento finale per indicare che il flusso AuthN è completo. A questo punto, l’app deve effettuare una richiesta all’AccessEnabler per ottenere il token AuthN e finalizzare il flusso AuthN.

Il problema è che `adobepass.ios.app` non esiste e attiverà un messaggio di errore in `webView`. Le versioni precedenti di iOS DemoApp presupponevano che questo errore sarebbe sempre stato attivato alla fine del flusso AuthN ed era configurato per gestirlo di conseguenza (`indidFailLoadWithError`).

**Nota:** questo problema è stato risolto nelle versioni successive di DemoApp (incluse nel download di iOS SDK).

Sfortunatamente, questo presupposto NON è corretto. Alcuni server DNS o proxy cosiddetti &quot;intelligenti&quot; non si limitano a trasferire l&#39;errore generato, ma eseguono una delle operazioni seguenti:

- Creare una pagina di errore personalizzata
- Inoltra a una pagina di ricerca o a un altro tipo di pagina del cliente o di portale.

In questi casi, la risposta che ritorna a iOS webView sarà una risposta perfettamente valida per quanto riguarda webView, e NON attiverà l’errore da cui dipendeva la vecchia DemoApp.

## Soluzione {#solution}

NON fare lo stesso presupposto che fa DemoApp. Intercettare invece la richiesta prima che venga eseguita (in `shouldStartLoadWithRequest`) e gestirla in modo appropriato.

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

- NON utilizzare MAI `adobepass.ios.app` direttamente in nessun punto del codice. Utilizza invece la costante `ADOBEPASS_REDIRECT_URL`
- L&#39;istruzione `return NO;` impedisce il caricamento della pagina
- Assicurati assolutamente che la chiamata `getAuthenticationToken` venga chiamata una sola volta nel codice. Più chiamate a `getAuthenticationToken` produrranno risultati non definiti.
