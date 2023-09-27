---
title: Note sulla versione di Adobe Pass Concurrency Monitoring 2.3.2
description: Note sulla versione di Adobe Pass Concurrency Monitoring 2.3.2
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# Note sulla versione di Adobe Pass Concurrency Monitoring 2.3.2 {#cm-232}

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

Data di rilascio: 12/11/2015

## Nuove funzioni e miglioramenti {#new-features}

* Sono disponibili nuove suddivisioni nei Rapporti sull’utilizzo. I nuovi raggruppamenti sono disponibili se l’applicazione integrata con Monitoraggio concorrenza invia metadati personalizzati.
   * application: l’ID applicazione riportato nell’URL della chiamata
   * mvpd: MVPD segnalato nell’URL della chiamata
   * channel: il canale metadati personalizzato
   * Platform: l’applicazione di metadati personalizzata Platform
* Nuove metriche relative a **durata flusso** disponibile nei report di utilizzo. Le nuove metriche possono essere utilizzate per creare un istogramma della durata del flusso. Sono attualmente disponibili i seguenti intervalli in minuti:
   * duration_0-15
   * duration_15-30
   * duration_30-60
   * duration_60-120
   * duration_over-120

## Correzioni di bug {#bug-fixes}

N/D

## Problemi noti {#known-issues}

N/D
