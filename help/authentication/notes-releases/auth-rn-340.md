---
title: Note sulla versione di Adobe Pass Authentication 3.4.0
description: Note sulla versione di Adobe Pass Authentication 3.4.0
exl-id: 7a9b8c6d-4e5f-4a3b-8c7d-9e0f1a2b3c4d
source-git-commit: 3a275f64f7f8cffa3bdc0d546c8e2db517840069
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 3.4.0 {#authn-340-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-340}

* [Numero build](#build-number-340)
* [Panoramica sulla versione](#release-overview-340)

### Numero build {#build-number-340}

Autenticazione Adobe Pass: adobe-pass-**3.4.0**
Data di rilascio: **09/09/2025 - 09/11/2025**

### Panoramica sulla versione {#release-overview-340}

#### API REST v2

* È stato aggiunto il supporto per [Experience Cloud ID (ECID)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md) per migliorare le funzionalità di identificazione e tracciamento degli utenti.
* Intestazioni AP-Device-Identifier e X-Device-Info modificate in opzionali per REST API V2 [Configuration API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### Correzioni di bug

* È stato risolto un problema che causava l&#39;inversione di MVPD ID e Proxy MVPD ID nel token dalla risposta [Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) dell&#39;API REST V2.
* È stato risolto un problema a causa del quale l&#39;ID richiedente non era presente nel token dalla risposta [Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) dell&#39;API REST V2.
* È stato risolto un problema che causava il passaggio di un numero eccessivo di risorse all&#39;API REST V2 [Preauthorize Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) nella risposta, causando un elenco di decisioni vuoto al posto del [Codice di errore avanzato](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **too_many_resources**.

#### Varie

* Sono state corrette le vulnerabilità di sicurezza.