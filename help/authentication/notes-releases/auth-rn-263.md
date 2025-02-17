---
title: Note sulla versione di Adobe Pass Authentication 2.63
description: Note sulla versione di Adobe Pass Authentication 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 2.63 {#authn-263-rn}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-263}

* [Numero build](#build-number-263)
* [Panoramica sulla versione](#release-overview-263)

### Numero build {#build-number-263}

Autenticazione Adobe Pass: adobe-pass-**2.63**

Data di rilascio: **09/20/2022 - 09/22/2022**

### Panoramica sulla versione {#release-overview-263}

#### Miglioramento del meccanismo di identificazione delle piattaforme

* A partire da questa versione, è stato migliorato il meccanismo utilizzato per identificare un dispositivo che non dipenderà più dall’implementazione lato client. Questo fornirà una granularità più precisa quando si applicano regole aziendali a livello di piattaforma e una migliore comprensione dei valori del traffico nei rapporti ESM.

* A breve seguirà una nuova versione di ESM, con rapporti nuovi e migliorati che espongono i campi relativi alla piattaforma.

* Per ulteriori dettagli sulle modifiche pianificate, contatta il tuo TAM.

#### Auto-degradazione MVPD

Questa funzione consente agli MVPD di ignorare temporaneamente i propri endpoint di autenticazione e autorizzazione per scenari di traffico elevato quando il carico su tali rispettivi endpoint diventa troppo elevato.

#### Aggiungi l’ID proxy nell’intestazione delle chiamate di autorizzazione

Questa funzione aggiunge l’ID di un MVPD proxy di Synacor nell’intestazione della chiamata di autorizzazione. Ciò consente a Synacor di impostare regole di business per ogni singolo proxy (ad esempio routing a domini diversi per MVPD proxy).

#### Dashboard TVE

In questa versione è stato risolto un problema a causa del quale il valore TTL authN o authZ impostato a livello di MVPD non veniva calcolato correttamente nei rapporti di configurazione.

#### JavaScript SDK 4.6.0

* È stato rimosso l&#39;utilizzo della funzione `eval`, rendendo il SDK conforme ai criteri sulla sicurezza dei contenuti.
* È stato risolto un problema che impediva il completamento corretto del flusso di autenticazione quando l’archiviazione locale del browser veniva esplicitamente cancellata da un’applicazione partner.
