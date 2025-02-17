---
title: Note sulla versione di Adobe Pass Authentication 2.69
description: Note sulla versione di Adobe Pass Authentication 2.69
exl-id: d031c4c5-dbd5-4a77-b298-a53b992cc4c5
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 2.69 {#authn-269-rn}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-269}

* [Numero build](#build-number-269)
* [Panoramica sulla versione](#release-overview-269)

### Numero build {#build-number-269}

Autenticazione Adobe Pass: adobe-pass-**2.69**

Data di rilascio: **02/27/2024 - 02/29/2024**

### Panoramica sulla versione {#release-overview-269}

#### Varie

* Sono state corrette le vulnerabilità di sicurezza.
* Miglioramenti al livello di sicurezza Reset Temp Pass con Dynamic Client Registration (DCR).
   * Ulteriori dettagli sono disponibili qui: [Funzionalità TempPass](../integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
* Sono stati apportati miglioramenti al reporting di Identificazione piattaforma.

#### API REST

* Sviluppo continuo di nuove API REST.
   * Una prossima versione dedicata introdurrà nuovi endpoint e flussi, che verranno annunciati in una notifica separata.
   * È in corso l’aggiornamento della documentazione per l’utilizzo di queste nuove API.

#### Dashboard TVE

* Sviluppo continuo nel nuovo dashboard TVE.
   * Una prossima versione dedicata presenterà la nuova dashboard TVE, che verrà annunciata in una notifica separata.
   * È in corso l’aggiornamento della documentazione per l’utilizzo di questo nuovo dashboard TVE.

#### JavaScript SDK 4.7.0

* È stata rimossa la versione obsoleta 2.0.1 di Access Enabler JavaScript SDK a causa di vulnerabilità di sicurezza.
   * Segui il collegamento per ulteriori dettagli: [Note sulla versione di Adobe Pass Authentication JavaScript 4.7.0](authn-rn-javascript-470.md)
