---
title: Verificare i flussi di autenticazione e autorizzazione utilizzando il sito di test API di Adobe
description: Verificare i flussi di autenticazione e autorizzazione utilizzando il sito di test API di Adobe
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Verificare i flussi di autenticazione e autorizzazione utilizzando il sito di test API di Adobe {#How-to-test-auth-flows}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Per testare i flussi AuthN e AuthZ, abbiamo preparato un **Sito di test API** che è a tua disposizione. Il nostro team di supporto sarà lieto di fornirti le credenziali. Puoi contattarci all’indirizzo **support@tve.zendesk.com**.


## Parte I {#part-I}

Per i test relativi all’ambiente RELEASE, vai direttamente alla Parte II.  Per eseguire il test nell’ambiente di pre-qualifica, consulta [Configurazione dell’ambiente e test in Pre-Qual](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

## Parte II

Dopo aver completato la parte I, effettuare le seguenti operazioni:


1. Apri pagina Web: [Test API per staging](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Carica enabler di accesso dal seguente URL:
   * [JavaScript per l&#39;attivazione degli accessi per la gestione temporanea](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * OPPURE
   * [JavaScript per l’attivazione degli accessi per la produzione](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * QUINDI fai clic sul pulsante &quot;**Carica Access Enabler**&quot;.
1. Ora imposta il valore ID richiedente su &quot;**requestorID**&quot; e fare clic sul pulsante &quot;setRequestor&quot;.
1. Successivamente, premere il pulsante &quot;getAuthentication&quot; e attendere che venga visualizzato il selettore di visualizzazione.
1. Seleziona la &quot;**MVPD**&quot; dal selettore.
1. Immetti le tue credenziali su &quot;**MVPD**&quot;pagina di accesso.
1. Dopo il reindirizzamento, ripetere i passaggi da 1 a 3
1. Dopo aver ripetuto il passaggio 3 di &quot;setAuthenticationStatus&quot; dovrebbe essere visualizzato il valore &quot;1&quot;. Se l&#39;autenticazione non funziona, viene visualizzata la finestra di dialogo MVPD.
1. Per verificare l’autorizzazione, immetti &quot;**requestorID**&quot; e fare clic sul pulsante &quot;getAuthorization&quot;.
1. Di conseguenza, nella casella di testo &quot;setToken&quot;-\>&quot;id risorsa&quot; verrà visualizzata la risorsa e nella casella di testo &quot;setToken&quot;-\>&quot;token&quot; verrà visualizzato shortAuthorizationToken, il che significa che authZ è stato eseguito correttamente.
1. Ora puoi fare clic sul pulsante &quot;logout&quot; per eliminare i token.
