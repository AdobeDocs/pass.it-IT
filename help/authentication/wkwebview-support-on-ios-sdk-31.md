---
title: Supporto WKWebView sull'SDK iOS 3.1+
description: Supporto WKWebView sull'SDK iOS 3.1+
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---

# Supporto WKWebView sull&#39;SDK iOS 3.1+ {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>

**Poiché Apple ha dichiarato obsoleto UIWebView su iOS, abbiamo aggiornato iOS SDK 3.1 con il supporto per WKWebView.**

## Compatibilità {#compatibility}

A partire dalla versione 3.1 dell&#39;SDK per iOS, gli implementatori possono ora utilizzare WKWebView o UIWebView in modo intercambiabile. Poiché UIWebView è stato dichiarato obsoleto da Apple, le app devono migrare a WKWebView per evitare problemi con le versioni future di iOS.

Tieni presente che la migrazione implicherebbe semplicemente il passaggio della classe UIWebView a WKWebView; non è necessario eseguire alcun lavoro specifico in merito all’AccessEnabler di Adobe.

## Problemi noti {#known-issues}

AccessEnabler di Adobe utilizzava un&#39;istanza UIWebView interna nascosta per eseguire &quot;[autenticazione passiva](/help/authentication/sso-passive-authn.md)&quot; per alcuni MVPD. Il flusso &quot;passivo&quot; è stato utile per gli MVPD che richiedono l’autenticazione per ogni ID richiedente e da questo flusso hanno tratto beneficio i programmatori che hanno utilizzato lo stesso ID team in più applicazioni iOS per simulare un’esperienza SSO (Adobe SSO). Questa funzione è attualmente utilizzata da un numero limitato di MVPD.

La funzione utilizzava un comportamento di UIWebView che consentiva all&#39;Adobe di acquisire i cookie di autenticazione e riprodurli durante il flusso &quot;passivo&quot;. WKWebView introduce una maggiore sicurezza che impedisce ad Adobe di acquisire i cookie impostati al momento dell’accesso e di riprodurli utilizzando un’istanza nascosta di WKWebView. A causa di questo miglioramento della sicurezza e considerando che il flusso &quot;passivo&quot; ha beneficiato solo di un set molto limitato di MVPD in uno scenario di implementazione molto specifico (più applicazioni che utilizzano lo stesso ID team), Adobe ha rimosso la funzione di &quot;autenticazione passiva&quot; per MVPD che utilizzano le visualizzazioni web per l’autenticazione.

La funzione è ancora presente per gli MVPD configurati per utilizzare SFSafariViewController, ma in questo caso l’autenticazione &quot;passiva&quot; sarà visibile all’utente poiché SFSafariViewController non può essere utilizzato in modo &quot;nascosto&quot;.
