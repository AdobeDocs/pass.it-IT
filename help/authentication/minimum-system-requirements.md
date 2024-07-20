---
title: Requisiti minimi di sistema
description: Requisiti minimi di sistema
exl-id: 57b21e2a-abd7-4b4b-85f1-25584a850e40
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# Requisiti minimi di sistema {#minimum-system-requirements}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.


## Panoramica {#overview}

Questo documento presenta gli attuali requisiti software e hardware per l’implementazione delle integrazioni di autenticazione Adobe Pass sulle piattaforme supportate. Tutti i browser web/mobili supportati e i sistemi operativi elencati di seguito beneficeranno del supporto completo del team di autenticazione di Adobe Pass, associato agli SLA concordati.

In qualità di team di autenticazione di Adobe Pass, incoraggiamo l’utilizzo delle versioni stabili più recenti dei browser e dei sistemi operativi; riconosciamo anche l’esistenza di piattaforme e browser non conformi/meno recenti attualmente in uso. Questi dispositivi obsoleti possono ancora funzionare senza problemi, ma saranno più soggetti a errori.

L’approccio iniziale per mitigare eventuali problemi che appaiono su queste piattaforme obsolete dovrebbe consistere nell’aggiornamento alle versioni più recenti, che possono essere la versione del sistema operativo, la versione del browser o la versione dell’applicazione installata.

Tutti i problemi che appaiono su queste piattaforme saranno risolti nel miglior modo possibile dal team di autenticazione di Adobe Pass.

Adobe Pass incoraggia i nostri clienti e partner a prendere in considerazione l’aggiornamento alle versioni più recenti per beneficiare del supporto completo di Adobe in tutti i potenziali problemi, oltre a miglioramenti delle prestazioni, dell’efficienza e della sicurezza.


## Requisiti del browser e del sistema operativo {#browser-OS-system-requirements}


| Browser Web/Mobile (†) | Versioni supportate |
|---|---|
| Google Chrome | **70** o versione successiva |
| Mozilla Firefox | **57** o versione successiva |
| Apple Safari | **14** o versione successiva |
| Microsoft Edge | **100** o versione successiva |

(†) L’Adobe sconsiglia di utilizzare la modalità privata o in incognito.

| Sistema operativo | Versioni supportate |
|---|---|
| *Android* | **7.0** (Nougat) o versione successiva |
| *iOS* | **14** o versione successiva |
| *iPadOS* | **14** o versione successiva |
| *tvOS* | **14** o versione successiva |
| *Attiva sistema operativo* | **5 (Android 5.1)** o versione successiva |
| *SO Mac* | **10.13** o versione successiva |
| *Microsoft* | **10** o versione successiva |




>[!NOTE]
>
>Cookie di terze parti - I flussi di adesione all’autenticazione di Adobe Pass potrebbero non riuscire quando i cookie di terze parti sono disabilitati.  Questo problema si verifica solo quando vengono modificate le impostazioni del browser. Per tutti i browser supportati, Autenticazione Adobe Pass deve funzionare con le impostazioni predefinite.


## Requisiti dei dispositivi per le implementazioni senza client (REST) {#general_clientless_reqs}


Qualsiasi dispositivo che utilizzerà i servizi di autenticazione di Adobe Pass tramite implementazioni senza client **deve essere in grado di**:

* Specifica un ID dispositivo con hash univoco. Se il dispositivo non fornisce un ID dispositivo con hash univoco, deve essere in grado di mantenere un ID univoco fornito dall’autenticazione di Adobe Pass. Il dispositivo deve essere in grado di mantenere l’ID univoco in modo permanente nel proprio archivio locale e di fornirlo come ID dispositivo quando effettua chiamate alle API di autenticazione di Adobe Pass.
* Generare firme digitali utilizzando l&#39;algoritmo HMAC-SHA1
* Impostare intestazioni HTTP arbitrarie
* Utilizzo di servizi Web RESTful
* Analizzare i formati di dati XML e JSON
* Inviare il traffico tramite HTTPS
* Gestire i codici di errore HTTP
