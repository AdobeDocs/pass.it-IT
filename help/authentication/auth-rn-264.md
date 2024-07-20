---
title: Note sulla versione di Adobe Pass Authentication 2.64
description: Note sulla versione di Adobe Pass Authentication 2.64
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 2.64 {#authn-264-rn}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#ss-web-clients}

* [Numero build](#build-no-264)
* [Nuove funzioni](#new-featres-264)
* [Correzioni di bug](#bug-fixes-264)

### Numero build {#build-no-264}

Autenticazione Adobe Pass: adobe-pass-**2.64**

Data di rilascio: **11/08/2022 - 11/10/2022**

### Nuove funzioni {#new-featres-264}

* Aggiornamenti dell&#39;infrastruttura, con l&#39;obiettivo di ridurre i tempi di risposta dei server e migliorare le prestazioni complessive del sistema.
* Miglioramenti al nuovo meccanismo di identificazione delle piattaforme.
* Possibilità di bloccare le risposte di autenticazione non richieste provenienti da MVPD in cui il parametro &quot;in_response_to&quot; non è presente nell’asserzione SAML.

### Correzioni di bug {#bug-fixes-264}

* È stata corretta la formattazione errata di alcuni token TempPass legacy.
* Sono stati risolti alcuni problemi minori relativi al flusso di autenticazione nella seconda schermata.
