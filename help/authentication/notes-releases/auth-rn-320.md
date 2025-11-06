---
title: Note sulla versione di Adobe Pass Authentication 3.2.0
description: Note sulla versione di Adobe Pass Authentication 3.2.0
exl-id: 43aee317-dbac-4000-893e-839ee3e9f6ba
source-git-commit: fcdf50b2caad20deef15fceeb3e23f4195c0078d
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 3.2.0 {#authn-320-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-320}

* [Numero build](#build-number-320)
* [Panoramica sulla versione](#release-overview-320)

### Numero build {#build-number-320}

Autenticazione Adobe Pass: adobe-pass-**3.2.0**

Data di rilascio: **06/10/2025 - 06/12/2025**

### Panoramica sulla versione {#release-overview-320}

#### API REST v2

* È stato aggiunto un nuovo motivo `missing_parameters_fallback` per i casi in cui mancano parametri nella risposta [Sessions API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).
* Un nuovo campo &quot;device&quot; è stato aggiunto alla risposta [Sessions API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### Nuove funzioni

* Espandi l’API di degradazione per aggiungere la capacità di applicare le regole di degradazione per più MVPD in una singola chiamata

#### Correzioni di bug

* È stato risolto un problema che impediva il completamento corretto del flusso di autenticazione nei casi in cui l’elaborazione dei metadati dell’utente non fosse riuscita.
* È stato risolto un problema a causa del quale il motivo dell’autorizzazione non veniva calcolato correttamente.
* È stato risolto un problema a causa del quale upstreamUserID non era presente nella risposta del profilo.
* È stato risolto un problema che impediva alla richiesta di autenticazione di includere informazioni sull’ambito per le richieste di autenticazione di avvio dell’API REST V2.
