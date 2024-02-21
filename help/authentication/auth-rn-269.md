---
title: Note sulla versione di Adobe Pass Authentication 2.69
description: Note sulla versione di Adobe Pass Authentication 2.69
source-git-commit: 4417c5c50873c73c615f9845cadd4a42c26f3068
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 2.69 {#pt-authn-269-rn}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-269}

* [Numero build](#build-number-269)
* [Nuove funzioni](#new-features-269)

### Numero build {#build-number-269}

Autenticazione Adobe Pass: adobe-pass-**2,69**
Data di rilascio: **02/27/2024 - 02/29/2024**

### Nuove funzioni {#new-features-269}

#### Varie {#misc}

* Sono state corrette le vulnerabilità di sicurezza.
* Miglioramenti al livello di sicurezza Reset Temp Pass con Dynamic Client Registration (DCR).
   * Ulteriori dettagli sono disponibili qui: [Ripristina passaggio temporaneo](reset-temp-pass.md)
* Sono stati apportati miglioramenti al reporting di Identificazione piattaforma.

#### API REST {#rest-apis}

* Sviluppo continuo di nuove API REST.
   * Una prossima versione dedicata introdurrà nuovi endpoint e flussi, che verranno annunciati in una notifica separata.
   * È in corso l’aggiornamento della documentazione per l’utilizzo di queste nuove API.

#### Dashboard TVE {#tve-dashboard}

* Sviluppo continuo nel nuovo dashboard TVE.
   * Una prossima versione dedicata presenterà la nuova dashboard TVE, che verrà annunciata in una notifica separata.
   * È in corso l’aggiornamento della documentazione per l’utilizzo di questo nuovo dashboard TVE.

#### SDK JavaScript 4.7.0 {#js-sdk}

* È stata rimossa la versione obsoleta 2.0.1 dell’SDK JavaScript per Access Enabler a causa di vulnerabilità di sicurezza.
   * Per ulteriori informazioni, segui il collegamento: [Note sulla versione di JavaScript 4.7.0 per l’autenticazione di Adobe Pass](authn-rn-javascript-470.md)
