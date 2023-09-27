---
title: Note sulla versione di Adobe Pass Authentication Android 3.7.3
description: Note sulla versione di Adobe Pass Authentication Android 3.7.3
source-git-commit: 284ae59837fd42bc6b711b3041eb6b7ef79aaed3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication Android 3.7.3 {#android-sdk-373-release-notes}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Numero build {#build-no-android-sdk-373}

Autenticazione Adobe Pass: Android 3.7.3

Data di rilascio: 09/19/2023



## Panoramica sulla versione {#overview-android-sdk-373}

* Modifiche per il supporto di Android 14 e delle applicazioni che eseguono il targeting dell’API al livello 34
   * Aggiungi il flag richiesto da [Ricevitori di trasmissioni registrati runtime Android 14](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported).
* Correzione di ChromeCustomTabs che non apre per l’accesso MVPD sull’API dell’emulatore 32+
   * Nota: una soluzione alternativa per questo problema sull’SDK &lt;3.7.3 consiste nell’aprire l’app Chrome sull’emulatore e completarne la configurazione prima di tentare l’accesso MVPD


## Pacchetto di rilascio {#rel-pkg-android373}

Puoi scaricare l&#39;SDK Android v3.7.3 da [qui](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
