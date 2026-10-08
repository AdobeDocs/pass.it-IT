---
title: Note sulla versione di Adobe Pass Concurrency Monitoring 2.3.2
description: Note sulla versione di Adobe Pass Concurrency Monitoring 2.3.2
exl-id: 3996da45-498c-482a-b374-3cda1c5df2f7
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 1%
---
# Note sulla versione di Adobe Pass Concurrency Monitoring 2.3.2 {#cm-232}

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

Data di rilascio: 12/11/2015

## Nuove funzioni e miglioramenti {#new-features}

* Sono disponibili nuove suddivisioni nei Rapporti sull’utilizzo. I nuovi raggruppamenti sono disponibili se l’applicazione integrata con Monitoraggio concorrenza invia metadati personalizzati.
  * application: l’ID applicazione riportato nell’URL della chiamata
  * mvpd: il MVPD segnalato nell’URL della chiamata
  * channel: il canale metadati personalizzato
  * Platform: l’applicazione di metadati personalizzata Platform
* Nuove metriche relative alla **durata flusso** disponibili nei report sull&#39;utilizzo. Le nuove metriche possono essere utilizzate per creare un istogramma della durata del flusso. Sono attualmente disponibili i seguenti intervalli in minuti:
  * duration_0-15
  * duration_15-30
  * duration_30-60
  * duration_60-120
  * duration_over-120

## Correzioni di bug {#bug-fixes}

N/D

## Problemi noti {#known-issues}

N/D
