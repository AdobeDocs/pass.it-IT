---
title: Note sulla versione di Adobe Pass Authentication Android 3.8.0
description: Note sulla versione di Adobe Pass Authentication Android 3.8.0
source-git-commit: f7db5b71723c02b2924e73105769ab5b22e05bca
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication Android 3.8.0 {#android-sdk-380-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Numero build {#build-number-380}

Autenticazione Adobe Pass: Android 3.8.0

Data di rilascio: **09/18/2025**

## Panoramica sulla versione {#release-overview-380}

* Correggere una vulnerabilità con il ricevitore di broadcast di archiviazione di SDK. Un’applicazione dannosa può presentare un collegamento falso per interrogare l’archiviazione condivisa dei token Adobe.
Tuttavia, non vengono archiviate informazioni critiche e la probabilità che qualsiasi utente sia stato interessato da questa vulnerabilità è molto bassa.
   * Nota: in seguito alla modifica, gli utenti verranno disconnessi.

## Pacchetto di rilascio {#release-package-380}

Puoi scaricare Android SDK v3.8.0 da [qui](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
