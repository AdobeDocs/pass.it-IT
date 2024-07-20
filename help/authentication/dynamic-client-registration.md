---
title: Registrazione client dinamici
description: Registrazione client dinamici
exl-id: 9bc2597d-b634-4542-849b-8e91a76cb8da
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Registrazione client dinamici {#dynamic-client-registration}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Contesto {#context}

Per allinearsi alle moderne procedure di sicurezza, miglioramento dell&#39;interfaccia utente e dei proprietari delle piattaforme
requisiti, Adobe Pass Authentication Android SDK e iOS SDK si stanno spostando nella direzione di adottare [schede personalizzate di Android Chrome](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} e [controller di visualizzazione Apple Safari](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}.

L’attuale implementazione di AdobePass utilizza visualizzazioni web specifiche per piattaforma, al fine di fornire l’ambiente web per visualizzare la pagina di accesso MVPD. Queste visualizzazioni Web non condividono la gestione delle credenziali con i browser della piattaforma, pertanto l’utente non può utilizzare una password salvata dal browser quando utilizza un’applicazione di autenticazione Adobe Pass. Inoltre, per motivi di sicurezza, alcune piattaforme stanno per rendere obsoleti i controller WebView per le attività di autenticazione. Sia Google che Apple forniscono opzioni alternative, ad esempio &quot;Chrome Custom Tabs&quot; e &quot;Safari View Controller&quot;. Si tratta fondamentalmente di schede monouso dei rispettivi browser. L’autenticazione Adobe Pass adotterà questi nuovi componenti nel 2018.

## Dettagli {#details}

Attualmente, l’autenticazione Adobe Pass identifica e registra le applicazioni in due modi:

* i client basati su browser vengono registrati tramite l’elenco dei domini consentiti
* i client delle applicazioni native, come le applicazioni iOS e Android, vengono registrati tramite il meccanismo del richiedente firmato

Per gestire i nuovi flussi per le schede personalizzate Chrome e il controller di visualizzazione Safari, Adobe Pass propone un nuovo meccanismo di registrazione client per la registrazione di nuove applicazioni. Questo meccanismo consentirà un controllo più sicuro e granulare delle applicazioni e può essere utilizzato per registrare le applicazioni su tutte le piattaforme.

<!--
## Related Information

- [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
- [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)
-->
