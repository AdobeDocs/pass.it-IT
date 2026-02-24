---
title: Note sulla versione di Adobe Pass Authentication 3.5.0
description: Scopri le nuove funzioni, le modifiche e i problemi noti di questa versione.
exl-id: b196f636-26a5-4974-903e-40b5f8b93a24
source-git-commit: 1cbddf081fc7d57a187c9701e4ade8593baf8759
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 3.5.0

Ultimo aggiornamento: martedì 9 dicembre 2025 00:00:00 GMT+0000 (Coordinated Universal Time)

* Argomenti:
* Autenticazione

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-350}

* [Numero build](#build-number-350)
* [Panoramica sulla versione](#release-overview-350)

### Numero build {#build-number-350}

Autenticazione Adobe Pass: adobe-pass-**3.5.0**\
Data di rilascio: **12/09/2025 - 12/11/2025**

### Panoramica sulla versione {#release-overview-350}

#### API REST v2

* È stata aggiunta una nuova dimensione in ESM (&quot;api&quot;) per consentire ai clienti di creare rapporti che distinguano gli eventi in base al tipo di implementazione: SDK, REST V1 o REST V2.

#### Correzioni di bug

* È stato risolto un problema che impediva la visualizzazione dei messaggi di degradazione personalizzati impostati in Dashboard TVE nei dettagli dell’errore REST API V2.
* È stato risolto un problema nell’API REST V2 a causa del quale il codice di errore `authenticated_profile_expired` non veniva restituito alla scadenza dei profili autenticati.
* È stato risolto un problema a causa del quale i calcoli della latenza di autorizzazione e i valori TTL di verifica preliminare non erano corretti nell’API REST V2.
* È stato risolto un problema a causa del quale veniva restituito un formato di risposta incoerente alla scadenza del token DCR.

## Aggiornamento di manutenzione - febbraio 2026 {#maintenance-update-february-2026}

Autenticazione Adobe Pass: adobe-pass-**3.5.0.5**\
Data di rilascio: **02/24/2026 - 02/26/2026**

Questa versione dell’aggiornamento di manutenzione include miglioramenti importanti per migliorare l’affidabilità e la sicurezza del sistema:

### Miglioramenti

* È stata migliorata la gestione del deterioramento dell’autenticazione per le configurazioni MVPD proxy nell’API REST V2, garantendo un comportamento più coerente durante le interruzioni dei servizi MVPD.
* Sono state migliorate la convalida dei parametri URL e la gestione dei reindirizzamenti per rafforzare i controlli di sicurezza e migliorare l’integrità complessiva del sistema.
