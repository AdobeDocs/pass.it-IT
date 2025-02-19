---
title: Note sulla versione di Adobe Pass Authentication Android 3.7.3
description: Note sulla versione di Adobe Pass Authentication Android 3.7.3
exl-id: f335357e-c209-428d-af2a-2181551447d4
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication Android 3.7.3 {#android-sdk-373-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Numero build {#build-number-373}

Autenticazione Adobe Pass: Android 3.7.3

Data di rilascio: **09/19/2023**

## Panoramica sulla versione {#release-overview-373}

* Modifiche per supportare Android 14 e le applicazioni che eseguono il targeting dell’API al livello 34
   * Aggiungi il flag richiesto da [ricevitori di trasmissioni registrate in runtime di Android 14](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported).
* Correzione di ChromeCustomTabs che non apre per l’accesso a MVPD sull’API dell’emulatore 32+
   * Nota: una soluzione alternativa per questo problema in SDK &lt;3.7.3 consiste nell’aprire l’app Chrome nell’emulatore e completarne la configurazione prima di tentare l’accesso a MVPD

## Pacchetto di rilascio {#release-package-373}

Puoi scaricare Android SDK v3.7.3 da [qui](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
