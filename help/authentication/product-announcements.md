---
title: Annunci sui prodotti
description: Annunci sui prodotti
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 1%

---

# Annunci sui prodotti {#product-announcements}

## Fine del ciclo di vita (EOL) {#eol}

Questa sezione descrive le date di fine del supporto e del ciclo di vita di alcune funzioni e prodotti di autenticazione di Adobe Pass che si prevede di disattivare.

Assicurati di essere sempre informato sulle tempistiche di disattivazione e di pianificare le azioni descritte di seguito.

| Annuncio | Data notifica | Data di fine del supporto | Data di fine del ciclo di vita | Descrizione |
|-----------------------------------------------------------------|-------------------|---------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autenticazione di Adobe Pass AccessEnabler JavaScript SDK v3.5 EOL | 12/11/2024 | N/D | 01/08/2025 | Pianifichiamo la fine del ciclo di vita di Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5 il **8 gennaio 2025**. Dopo questa data, le funzionalità e i servizi forniti da AccessEnabler JavaScript SDK v3.5 non funzioneranno più, incluse l&#39;autenticazione e l&#39;autorizzazione degli utenti. |
| Autenticazione Adobe Pass per EOL del meccanismo di sicurezza non DCR | 12/11/2024 | N/D | 01/20/2025 | Pianifichiamo la fine del ciclo di vita del meccanismo di sicurezza non DCR per l’autenticazione di Adobe Pass il **20 gennaio 2025**. Questo meccanismo è stato utilizzato per proteggere l’accesso alle seguenti API di autenticazione di Adobe Pass:<ul><li><a href="/help/authentication/integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md">Reimposta API passaggio temporaneo</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md">API di degradazione</a></li><li><a href="/help/authentication/integration-guide-mvpds/proxy-mvpd-webserv.md">API MVPD proxy</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md">API di monitoraggio del servizio di adesione</a></li></ul>Dopo questa data, le funzioni e i servizi forniti dalle API di cui sopra a cui si accede utilizzando questo meccanismo di sicurezza non funzioneranno più.</br></br>Per garantire la continuità del servizio, è necessario eseguire la migrazione di tutte le applicazioni per utilizzare il meccanismo [Registrazione client dinamica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md). |
| Autenticazione Adobe Pass REST API v1 EOL | 12/11/2024 | 12/31/2025 | Fine del 2026 (da definire) | Il supporto per l’API REST di autenticazione di Adobe Pass v1 verrà interrotto il **31 dicembre 2025**. Dopo questa data, non saranno più forniti aggiornamenti o correzioni.</br></br>Per garantire la continuità del supporto, è necessario migrare tutte le applicazioni in <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>.</br></br>Pianifichiamo la fine del ciclo di vita dell&#39;API REST di autenticazione di Adobe Pass v1 entro la **fine del 2026**. Dopo questa data, le funzioni e i servizi forniti dall’API REST v1 non funzioneranno più, inclusa l’autenticazione e l’autorizzazione degli utenti.</br></br>Per garantire la continuità del servizio, è necessario migrare tutte le applicazioni in <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>. |
| Fine del ciclo di vita degli SDK di Adobe Pass Authentication AccessEnabler | 12/11/2024 | 05/31/2026 | Fine del 2026 (da definire) | Il supporto per gli SDK di Adobe Pass Authentication AccessEnabler verrà interrotto il **31 maggio 2026**. Dopo questa data, non saranno più forniti aggiornamenti o correzioni.</br></br>Per garantire la continuità del supporto, è necessario migrare tutte le applicazioni in <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>.</br></br>Pianifichiamo la fine del ciclo di vita degli SDK Adobe Pass Authentication AccessEnabler entro la **fine del 2026**. Dopo questa data, le funzionalità e i servizi forniti dagli SDK di AccessEnabler non funzioneranno più, incluse l’autenticazione e l’autorizzazione degli utenti.</br></br>Per garantire la continuità del servizio, è necessario migrare tutte le applicazioni in <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>. |

## Rilasci di prodotti {#product-releases}

In questa sezione vengono compilati i riferimenti alla cronologia delle versioni e alle relative note sulla versione per l’autenticazione Adobe Pass.

### 2024 {#product-releases-2024}

| Note sulla versione | Date |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Note sulla versione di Adobe Pass Authentication 3.0.3](notes-releases/auth-rn-303.md) | 10/29/2024 - 10/31/2024 |
| [Note sulla versione di Adobe Pass Authentication 3.0](notes-releases/auth-rn-300.md) | 09/10/2024 - 09/12/2024 |
| [Note sulla versione di Adobe Pass Authentication 2.70](notes-releases/auth-rn-270.md) | 04/23/2024 - 04/25/2024 |
| [Note sulla versione di Adobe Pass Authentication 2.69](notes-releases/auth-rn-269.md) | 02/27/2024 - 02/29/2024 |
| [Note sulla versione di Adobe Pass Authentication JavaScript SDK 4.7.0](notes-releases/authn-rn-javascript-470.md) | 02/27/2024 - 02/29/2024 |
| [Note sulla versione di Adobe Pass Authentication iOS/tvOS SDK 3.9.2](notes-releases/authn-rn-ios-tvos-392.md) | 03/26/2024 |
| [Note sulla versione di Adobe Pass Authentication iOS/tvOS SDK 3.8.4](notes-releases/authn-rn-ios-tvos-384.md) | 01/26/2024 |

### 2023 {#product-releases-2023}

| Note sulla versione | Date |
|---------------------------------------------------------------------------------------------------------|-------------------------|
| [Note sulla versione di Adobe Pass Authentication 2.68](notes-releases/auth-rn-268.md) | 12/05/2023 - 12/07/2023 |
| [Note sulla versione di Adobe Pass Authentication 2.67](notes-releases/auth-rn-267.md) | 09/12/2023 - 09/14/2023 |
| [Note sulla versione di Adobe Pass Authentication 2.66](notes-releases/auth-rn-266.md) | 07/11/2023 - 07/13/2023 |
| [Note sulla versione di Adobe Pass Authentication 2.65.1](notes-releases/auth-rn-2651.md) | 06/20/2023 - 06/22/2023 |
| [Note sulla versione di Adobe Pass Authentication 2.65](notes-releases/auth-rn-265.md) | 25/04/2023 - 27/04/2023 |
| [Note sulla versione di Adobe Pass Authentication 2.64.1](notes-releases/auth-rn-2641.md) | 01/31/2023 - 02/02/2023 |
| [Note sulla versione di Adobe Pass Authentication iOS/tvOS SDK 3.8.3](notes-releases/authn-rn-ios-tvos-383.md) | 11/16/2023 |
| [Note sulla versione di Adobe Pass Authentication iOS/tvOS SDK 3.8.2](notes-releases/authn-rn-ios-tvos-382.md) | 02/10/2023 |
| [Note sulla versione di Adobe Pass Authentication iOS/tvOS SDK 3.8.1](notes-releases/authn-rn-ios-tvos-381.md) | 26/05/2023 |
| [Note sulla versione di Adobe Pass Authentication Android SDK 3.7.3](notes-releases/authn-rn-android-373.md) | 09/19/2023 |

### 2022 {#product-releases-2022}

| Note sulla versione | Date |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Note sulla versione di Adobe Pass Authentication 2.64](notes-releases/auth-rn-264.md) | 11/08/2022 - 11/10/2022 |
| [Note sulla versione di Adobe Pass Authentication 2.63](notes-releases/auth-rn-263.md) | 09/20/2022 - 09/22/2022 |
| [Note sulla versione di Adobe Pass Authentication 2.62.1](notes-releases/auth-rn-2621.md) | 08/02/2022 - 08/04/2022 |
| [Note sulla versione di Adobe Pass Authentication JavaScript SDK 4.6.0](notes-releases/authn-rn-javascript-460.md) | 09/20/2022 - 09/22/2022 |

### 2021 {#product-releases-2021}

| Note sulla versione | Date |
|-----------------------------------------------------------------------------------------------------------|------------|
| [Note sulla versione di Adobe Pass Authentication JavaScript SDK 4.4.0](notes-releases/authn-rn-javascript-440.md) | 06/22/2021 |
| [Note sulla versione di Adobe Pass Authentication iOS/tvOS SDK 3.7.0](notes-releases/authn-rn-ios-tvos-370.md) | 09/03/2021 |