---
title: Note sulla versione di Adobe Pass Authentication 2.66
description: Note sulla versione di Adobe Pass Authentication 2.66
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 2.66 {#authn-266-rn}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-266}

* [Numero build](#build-number-266)
* [Panoramica sulla versione](#release-overview-266)

### Numero build {#build-number-266}

Autenticazione Adobe Pass: adobe-pass-**2.66.0.1**

Data di rilascio: **07/11/2023 - 07/13/2023**

### Panoramica sulla versione {#release-overview-266}

Con questa versione abbiamo continuato gli aggiornamenti interni per la nuova API REST.

#### Correzioni di bug

* È stato risolto il flusso di logout per MVPD basati su SAML, in cui il parametro RelayState mancava dalla richiesta di logout. Dopo la versione, eseguiremo il targeting degli aggiornamenti della configurazione per ripristinare il flusso di logout per gli MVPD interessati.
* È stata aggiunta la possibilità di aggiornare i certificati SSL nella configurazione per gli endpoint di autorizzazione SOAP.
* È stato risolto un caso limite in cui dati non corretti venivano registrati nel campo Programmatore in alcuni rapporti ESM.
