---
title: Note sulla versione di Adobe Pass Authentication 3.9.0
description: Note sulla versione di Adobe Pass Authentication 3.9.0
hold: true
source-git-commit: 5ca8f29764a07ddb68abb36accb12cfb3b68b72d
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 3.9.0 {#authn-390-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-390}

* [Numero build](#build-number-390)
* [Panoramica sulla versione](#release-overview-390)

### Numero build {#build-number-390}

Autenticazione Adobe Pass: adobe-pass-**3.9.0.1**\
Data di rilascio: **09/08/2026 - 09/10/2026**

### Panoramica sulla versione {#release-overview-390}

Questa versione si concentra sui miglioramenti dell’API REST V2 e delle metriche ESM.

#### Miglioramenti

* È stato migliorato il Single Sign-On per l’API REST V2 per garantire che venga restituita una richiesta di autenticazione valida per gli MVPD configurati con OAuth2.
* Sono state migliorate le decisioni REST API V2 per restituire una risposta di errore chiara in caso di errore di autorizzazione, invece di una risposta vuota.
* È stata migliorata la generazione del codice di registrazione per evitare caratteri visivamente ambigui, rendendo i codici più facili da leggere e immettere correttamente.
* Miglioramenti al dashboard ESM con supporto per le metriche AuthZ di verifica preliminare.
