---
title: Note sulla versione di Adobe Pass Authentication 3.7.0
description: Note sulla versione di Adobe Pass Authentication 3.7.0
source-git-commit: 89b5fbd8e8510cbf84ce7908e8cf86551e7a0cb9
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 3.7.0 {#authn-370-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-370}

* [Numero build](#build-number-370)
* [Panoramica sulla versione](#release-overview-370)

### Numero build {#build-number-370}

Autenticazione Adobe Pass: adobe-pass-**3.7.0.2**\
Data di rilascio: **05/12/2026 - 05/14/2026**

### Panoramica sulla versione {#release-overview-370}

Questa versione si concentra sugli aggiornamenti dell’integrazione MVPD, sulle correzioni di bug e sui miglioramenti del dashboard TVE.

#### Integrazioni MVPD

* È stato aggiunto il supporto PKCE per l’autenticazione MVPD basata su OAuth2.

#### Miglioramenti

* È stata rilasciata la versione 1.5.1 del dashboard di TVE.

#### Correzioni di bug

* È stato risolto un problema che causava l&#39;esclusione di Apple SSO da alcune mancate corrispondenze di configurazione di MVPD.
* È stato risolto un problema che causava errori HTTP 500 in caso di negazione dell’autorizzazione a causa di caratteri non validi nell’intestazione della risposta.
