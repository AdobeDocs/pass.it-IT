---
title: Note sulla versione di Adobe Pass Authentication 2.70
description: Note sulla versione di Adobe Pass Authentication 2.70
exl-id: 81713f8e-bc51-4057-9b00-6a2d6c83cd02
source-git-commit: 6d46beb688299aeef2a7dbb013b3eca7b4509593
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 2.70 {#pt-authn-270-rn}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-270}

* [Numero build](#build-number-270)
* [Nuove funzioni](#new-features-270)

### Numero build {#build-number-270}

Autenticazione Adobe Pass: adobe-pass-**2.70**
Data di rilascio: **04/23/2024 - 04/25/2024**

### Nuove funzioni {#new-features-270}

#### Varie {#misc}

* Sono state corrette le vulnerabilità di sicurezza.
* Miglioramenti al servizio Degradation API.
   * Utilizza DCR come meccanismo di sicurezza per l’API di degradazione.
   * Ulteriori dettagli sono disponibili qui: [Panoramica API di degradazione](degradation-api-overview.md)

#### API REST {#rest-apis}

* Sviluppo continuo di nuove API REST.
   * Una prossima versione dedicata introdurrà nuovi endpoint e flussi, che verranno annunciati in una notifica separata.
   * È in corso l’aggiornamento della documentazione per l’utilizzo di queste nuove API.
