---
title: Note sulla versione di Adobe Pass Authentication 2.64
description: Note sulla versione di Adobe Pass Authentication 2.64
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%
---
# Note sulla versione di Adobe Pass Authentication 2.64 {#authn-264-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-264}

* [Numero build](#build-number-264)
* [Panoramica sulla versione](#release-overview-264)

### Numero build {#build-number-264}

Autenticazione Adobe Pass: adobe-pass-**2.64**

Data di rilascio: **11/08/2022 - 11/10/2022**

### Panoramica sulla versione {#release-overview-264}

* Aggiornamenti dell&#39;infrastruttura, con l&#39;obiettivo di ridurre i tempi di risposta dei server e migliorare le prestazioni complessive del sistema.
* Miglioramenti al nuovo meccanismo di identificazione delle piattaforme.
* Possibilità di bloccare le risposte di autenticazione non richieste provenienti da MVPD in cui il parametro &quot;in_response_to&quot; non è presente nell’asserzione SAML.

#### Correzioni di bug

* È stata corretta la formattazione errata di alcuni token TempPass legacy.
* Sono stati risolti alcuni problemi minori relativi al flusso di autenticazione nella seconda schermata.
