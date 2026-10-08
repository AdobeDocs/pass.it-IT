---
title: Note sulla versione di Adobe Pass Authentication 3.8.0
description: Note sulla versione di Adobe Pass Authentication 3.8.0
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%
---
# Note sulla versione di Adobe Pass Authentication 3.8.0 {#authn-380-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-380}

* [Numero build](#build-number-380)
* [Panoramica sulla versione](#release-overview-380)

### Numero build {#build-number-380}

Autenticazione Adobe Pass: adobe-pass-**3.8.0**\
Data di rilascio: **08/11/2026 - 08/13/2026**

### Panoramica sulla versione {#release-overview-380}

Questa versione di introduce miglioramenti a livello di stabilità e sicurezza e gli aggiornamenti di sicurezza per tutti i servizi di autenticazione di Adobe Pass.

#### Correzioni di bug

* È stato risolto un problema che causava errori HTTP 500 sulle API V2 a causa di alcuni caratteri non validi nel deviceId.

#### Miglioramenti

* È stata migliorata la gestione dei token di aggiornamento per supportare il rinnovo dei token in sequenza.
* Migliore riconoscimento visitorId sui dispositivi secondari per l’analisi.
* È stata migliorata la convalida dei parametri URL per rafforzare i controlli di sicurezza e migliorare l’integrità complessiva del sistema.
* Dashboard TVE versione 1.5.2 con miglioramenti minori all’interfaccia utente.
