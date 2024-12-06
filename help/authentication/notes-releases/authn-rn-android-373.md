---
title: Note sulla versione di Adobe Pass Authentication Android 3.7.3
description: Note sulla versione di Adobe Pass Authentication Android 3.7.3
exl-id: f335357e-c209-428d-af2a-2181551447d4
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication Android 3.7.3 {#android-sdk-373-release-notes}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Numero build {#build-no-android-sdk-373}

Autenticazione Adobe Pass: Android 3.7.3

Data di rilascio: 09/19/2023



## Panoramica sulla versione {#overview-android-sdk-373}

* Modifiche per supportare Android 14 e le applicazioni che eseguono il targeting dell’API al livello 34
   * Aggiungi il flag richiesto da [ricevitori di trasmissioni registrate in runtime di Android 14](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported).
* Correzione di ChromeCustomTabs che non apre per l’accesso MVPD sull’API dell’emulatore 32+
   * Nota: una soluzione alternativa per questo problema sull’SDK &lt;3.7.3 consiste nell’aprire l’app Chrome sull’emulatore e completarne la configurazione prima di tentare l’accesso MVPD


## Pacchetto di rilascio {#rel-pkg-android373}

Puoi scaricare Android SDK v3.7.3 da [qui](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
