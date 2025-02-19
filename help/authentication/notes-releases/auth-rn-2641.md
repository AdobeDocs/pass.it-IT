---
title: Note sulla versione di Adobe Pass Authentication 2.64.1
description: Note sulla versione di Adobe Pass Authentication 2.64.1
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 2.64.1 {#authn-264-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-2641}

* [Numero build](#build-number-2641)
* [Panoramica sulla versione](#release-overview-2641)

### Numero build {#build-number-2641}

Autenticazione Adobe Pass: adobe-pass-**2.64.1**

Data di rilascio: **01/31/2023 - 02/02/2023**

### Panoramica sulla versione {#release-overview-2641}

Questa versione aggiunge quanto segue:

* Possibilità di bloccare le risposte di autenticazione non richieste provenienti da MVPD in cui il parametro &quot;in_response_to&quot; non è presente nell’asserzione SAML.
* È stata migliorata la convalida sul parametro redirect_url per garantire la conformità ai requisiti di sicurezza.
* È stata migliorata la registrazione delle informazioni sul dispositivo per le richieste di autenticazione della seconda schermata, utilizzando le informazioni fornite dal dispositivo iniziale.
