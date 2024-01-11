---
title: Note sulla versione di JavaScript 4.0.0 per l’autenticazione di Adobe Pass
description: Note sulla versione di JavaScript 4.0.0 per l’autenticazione di Adobe Pass
source-git-commit: 7057aeda34b4fe0d059912ab0a71ea856427654c
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Note sulla versione di JavaScript 4.0.0 per l’autenticazione di Adobe Pass {#javascript-sdk-400-release-notes}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Numero build {#build-no-javascript-sdk-400}

Autenticazione Adobe Pass: JavaScript 4.0.0

Data di rilascio: **07/05/2018**


## Panoramica sulla versione {#overview-javascript-sdk-400}

* È stato implementato il supporto per il meccanismo di registrazione client dinamico, un meccanismo di sicurezza unificato su più piattaforme. La gestione dell’accesso di sicurezza tra piattaforme può essere eseguita tramite il dashboard TVE, dal programmatore stesso.
* Elimina l’utilizzo di tutti i cookie di terze parti e di quasi tutti i cookie di prima parte per tutte le funzionalità (ad eccezione dell’individualizzazione in cui viene ancora utilizzato un cookie di prima parte, che verrà rimosso in futuro), rendendolo così più compatibile e a prova di futuro con le politiche emergenti del browser relative ai cookie di terze parti o ai cookie in generale (esempio. Safari (ITP). Tieni presente che la versione 3.x precedente funziona come previsto per i flussi felici utilizzando solo cookie di prima parte, ma questo potrebbe cambiare con il rilascio di nuove versioni del browser.
* Supporto per la disattivazione della memorizzazione in cache per i controlli di autorizzazione della verifica preliminare.
* Questa versione NON è compatibile con le versioni precedenti ed è ospitata in un nuovo percorso: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js . Per beneficiare di questo nuovo SDK JS è necessario un processo di migrazione da parte dei programmatori per registrare le loro applicazioni web utilizzando il nuovo meccanismo di registrazione client dinamica e per puntare al nuovo URL SDK JS.


## Pacchetto di rilascio {#rel-pkg-javascript-sdk-400}

L’URL di produzione è: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

L’URL di staging è: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
