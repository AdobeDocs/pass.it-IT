---
title: Note sulla versione di Adobe Pass Authentication JavaScript 4.4.0
description: Note sulla versione di Adobe Pass Authentication JavaScript 4.4.0
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication JavaScript 4.4.0 {#javascript-sdk-440-rn}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Numero build {#build-number-440}

Autenticazione Adobe Pass: JavaScript 4.4.0

Data di rilascio: **06/22/2021**

## Panoramica sulla versione {#release-overview-440}

### Nuove funzioni

Autorizzazione di verifica preliminare

* Nuova API di pre-autorizzazione: si tratta della nuova chiamata API da utilizzare per la funzione di autorizzazione della verifica preliminare, che introduce anche una funzione avanzata di segnalazione degli errori per il flusso di verifica preliminare.
* Questa funzione è disponibile su richiesta in quanto deve essere abilitata nella configurazione di Autenticazione Primetime. Per ulteriori informazioni su come abilitare questa funzione, contatta il TAM.
* L’API checkPreauthorizedResources è obsoleta.

Identificazione della piattaforma

* Aggiunge l’intestazione AP-SDK-Identifier su tutte le chiamate SDK per identificare meglio il tipo e la versione di SDK.

Altri

* Miglioramenti dell&#39;architettura interna.

### Correzioni di bug

* Correzione della race condition generata quando setRequestor e getAuthentication vengono chiamati contemporaneamente.
* È stato risolto un problema che impediva il caricamento corretto dei diritti negli ambienti di staging.
* È stato risolto un problema che impediva il completamento del flusso di logout in background sui browser Safari, così l’utente sembrava ancora autenticato fino a quando non si verificava un aggiornamento della pagina. È stato introdotto un timeout, attualmente impostato a 30 secondi, quindi se non è presente alcuna risposta dal server Autenticazione Primetime durante questo periodo, SDK richiamerà il callback setAuthenticationStatus.

## Pacchetto di rilascio {#release-package-440}

L’URL di produzione è: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

L’URL di staging è: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
