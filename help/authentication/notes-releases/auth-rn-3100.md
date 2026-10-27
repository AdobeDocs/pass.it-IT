---
title: Note sulla versione di Adobe Pass Authentication 3.10.0
description: Note sulla versione di Adobe Pass Authentication 3.10.0
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: d60b5256f5809b52122d70d04eb873018707fcaf
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%
---
# Note sulla versione di Adobe Pass Authentication 3.10.0 {#authn-3100-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-3100}

* [Numero build](#build-number-3100)
* [Panoramica sulla versione](#release-overview-3100)

### Numero build {#build-number-3100}

Autenticazione Adobe Pass: adobe-pass-**3.10.0**\
Data di rilascio: **10/27/2026 - 10/29/2026**

### Panoramica sulla versione {#release-overview-3100}

Questa versione si concentra sui controlli Single Sign-On, sui miglioramenti dei metadati utente e sui miglioramenti dell’affidabilità per la gestione della configurazione di MVPD e l’API REST V2.

#### Miglioramenti

* È stato aggiunto il controllo a livello di integrazione per il Single Sign-On (SSO) del token di servizio. L’SSO del token di servizio viene utilizzato solo quando è abilitato in modo esplicito per l’integrazione.
* È stato aggiunto il supporto per i metadati utente personalizzati (ad esempio, `packageType`) durante l&#39;autenticazione, l&#39;autorizzazione e la verifica preliminare. Le decisioni di autorizzazione e verifica preliminare dell’API REST V2 ora includono metadati utente configurati, incluse le risposte memorizzate nella cache, nel rispetto delle impostazioni di accesso ai metadati e crittografia.

#### Correzioni di bug

* È stato risolto un problema che causava il mancato funzionamento delle richieste di profilo Apple SSO REST API V2 con un errore di risposta SAML non valido per le integrazioni che utilizzavano MVPD OAuth2 tramite SAML.
* È stato risolto un problema a causa del quale le decisioni di verifica preliminare e di autorizzazione per la stessa risorsa potevano sovrascrivere l’una l’altra i tempi di scadenza memorizzati nella cache e i metadati utente.
* È stato risolto un problema che impediva i successivi salvataggi della configurazione a causa dell’eliminazione di MVPD proxy e di impostazioni di piattaforma prive di riferimenti.
