---
title: Note sulla versione di Adobe Pass Authentication 2.63
description: Note sulla versione di Adobe Pass Authentication 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 2.63 {#pt-authn-263-rn}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-263}

* [Numero build](#build-number)
* [Nuove funzioni](#new-features)

### Numero build {#build-number-263}

Autenticazione Adobe Pass: adobe-pass-**2,63**
Data di rilascio: **09/20/2022 - 09/22/2022**

### Nuove funzioni {#new-features-263}

#### Miglioramento del meccanismo di identificazione delle piattaforme {#pf-identification-mech}

* A partire da questa versione, è stato migliorato il meccanismo utilizzato per identificare un dispositivo che non dipenderà più dall’implementazione lato client. Questo fornirà una granularità più precisa quando si applicano regole aziendali a livello di piattaforma e una migliore comprensione dei valori del traffico nei rapporti ESM.

* A breve seguirà una nuova versione di ESM, con rapporti nuovi e migliorati che espongono i campi relativi alla piattaforma.

* Per ulteriori dettagli sulle modifiche pianificate, contatta il tuo TAM.

#### Autodegradazione MVPD {#mvpd-self-degradation}

Questa funzione consente agli MVPD di ignorare temporaneamente i propri endpoint di autenticazione e autorizzazione per scenari di traffico elevato quando il carico su tali rispettivi endpoint diventa troppo elevato.


#### Aggiungi l’ID proxy nell’intestazione delle chiamate di autorizzazione {#add-proxied-id}

Questa funzione aggiunge l’ID di un MVPD proxy di Synacor nell’intestazione della chiamata di autorizzazione. Ciò consente a Synacor di impostare regole di business per ogni singolo proxy (ad esempio routing a domini diversi per MVPD proxy).


#### Dashboard TVE {#tve-dashboard}

In questa versione è stato risolto un problema a causa del quale il TTL authN o authZ impostato a livello di MVPD non veniva calcolato correttamente nei rapporti di configurazione.


#### SDK JavaScript 4.6.0 {#js-sdk}

* È stato rimosso l’utilizzo di `eval` rendendo l’SDK conforme alle direttive sulla sicurezza dei contenuti.
* È stato risolto un problema che impediva il completamento corretto del flusso di autenticazione quando l’archiviazione locale del browser veniva esplicitamente cancellata da un’applicazione partner.
