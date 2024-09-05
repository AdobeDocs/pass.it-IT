---
title: Note sulla versione di Adobe Pass Authentication 3.0
description: Note sulla versione di Adobe Pass Authentication 3.0
source-git-commit: 82fd63a0e208a90753235b81fa52757283c9d314
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 3.0 {#pt-authn-300-rn}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-300}

* [Numero build](#build-number-300)
* [Panoramica sulla versione](#release-overview-300)

### Numero build {#build-number-2651}

Autenticazione Adobe Pass: adobe-pass-**3.0**
Data di rilascio: **09/10/2024 - 09/12/2024**

### Nuove funzioni {#new-features-300}

#### API REST v2 {#rest-apis}

##### Codice

* A partire dalla versione di adobe-pass-**3.0**, le applicazioni client nuove ed esistenti possono integrarsi o migrare alla nuova API REST v2 per beneficiare delle funzionalità Adobe Pass più recenti.

##### Documentazione

* Per iniziare con la nuova API REST v2, consulta i seguenti documenti:
   * [REST API v2 - API - Panoramica](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [API REST v2 - Flussi - Panoramica](./rest-api-v2/flows/rest-api-v2-flows-overview.md)
* Gli URL per i documenti pubblici dell’API REST v1 sono stati modificati. Fai riferimento ai seguenti documenti:
   * [REST API v1 - API - Panoramica](./rest-api-overview.md)
   * [REST API v1 - API - Riferimento](./rest-api-reference.md)

##### Strumenti

* Per provare la nuova API REST v2, fare riferimento alla nuova pagina di autenticazione di Adobe Pass dal sito Web [Adobe Developer](https://developer.adobe.com/adobe-pass).

### Correzioni di bug {#bug-fixes-300}

* È stato risolto un problema a causa del quale il parametro URL di reindirizzamento non veniva utilizzato se presente nella richiesta di disconnessione.