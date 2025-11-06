---
title: Note sulla versione di Adobe Pass Authentication 3.1.0
description: Note sulla versione di Adobe Pass Authentication 3.1.0
exl-id: cf9fc8e2-4b37-4b0a-a6ed-cda1b6738e76
source-git-commit: 0c6aec04ae9df410228730b5bce6ced1aeecd312
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 3.1.0 {#authn-310-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-310}

* [Numero build](#build-number-310)
* [Panoramica sulla versione](#release-overview-310)

### Numero build {#build-number-310}

Autenticazione Adobe Pass: adobe-pass-**3.1.0**

Data di rilascio: **02/25/2025 - 02/27/2025**

### Panoramica sulla versione {#release-overview-310}

#### API REST v2

* Nuovo nome azione `partner_logout` e tipo azione `partner_interactive` per distinguere tra disconnessione regolare e disconnessione single sign-on partner nella risposta API REST v2 [API Logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md).
* Nuovo campo `reason` per fornire ulteriori informazioni sul nome dell&#39;azione nelle risposte API REST v2 [Sessions API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) e [Sessions SSO API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md).

#### Correzioni di bug

* È stato risolto un problema che impediva ai sottoscrittori Spectrum di eseguire l&#39;autenticazione tramite l&#39;API REST v2 [Autentica API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).
* È stato risolto un problema che impediva la corretta aggregazione in ESM degli eventi generati tramite REST API V2 [Authenticate API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).
* È stato risolto un problema che causava un calcolo errato della marca temporale `notBefore` per i profili utente nella risposta dell&#39;API REST v2 [Profiles API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md).

#### JavaScript SDK

* Rimossa versione 3.5.0 di [AccessEnabler JavaScript SDK](authn-rn-javascript-471.md).
