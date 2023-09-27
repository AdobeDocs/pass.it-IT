---
title: Autenticazione Adobe Pass e il nuovo modello di autorizzazioni "Marshmallow" per Android 6
description: Autenticazione Adobe Pass e il nuovo modello di autorizzazioni "Marshmallow" per Android 6
exl-id: 3c96769e-b25b-48ab-bb74-40f13d4e5a84
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 0%

---

# Autenticazione Adobe Pass e il nuovo modello di autorizzazioni &quot;Marshmallow&quot; per Android 6 {#adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>

La nuova versione di Marshmallow per Android 6 introduce alcuni aggiornamenti al modello di autorizzazioni, che possono influenzare il comportamento delle app che utilizzano l’SDK per l’autenticazione di Adobe Pass versione 1.8 e precedenti.

La nuova funzionalità del sistema operativo Android offre [controllo granulare sulle autorizzazioni necessarie per le app al momento dell’installazione e in fase di runtime](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html).

>[!IMPORTANT]
>
>Le modifiche descritte di seguito **ha effetto solo sulle applicazioni sviluppate specificamente per Android 6.0** (targetSdkVersion=23). Non influisce sulle applicazioni precedenti già installate sul dispositivo dell’utente durante l’aggiornamento ad Android 6.0.


In particolare, per le app sviluppate in Android Studio utilizzando [Livello API 23](http://developer.android.com/sdk/api_diff/23/changes.html) e che utilizzano l’SDK di autenticazione di Adobe Pass, lo sviluppatore dovrà scrivere un codice personalizzato (vedi frammento di codice di seguito) [per attivare la finestra di dialogo consenti/nega autorizzazioni](https://developer.android.com/training/permissions/requesting.html).

Di seguito è riportato un estratto di codice utilizzato per richiedere l’accesso in scrittura allo storage esterno del dispositivo:

```java
// Here, thisActivity is the current activity
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.WRITE_EXTERNAL_STORAGE)
        != PackageManager.WRITE_EXTERNAL_STORAGE) {

    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.WRITE_EXTERNAL_STORAGE)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE);

        // MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```




**Dal punto di vista degli utenti** Al momento dell&#39;installazione, gli utenti vengono accolti da una finestra che richiede di confermare le autorizzazioni di lettura/scrittura per i file (vedere la figura 2 di seguito). Questo porta a uno dei due risultati seguenti:

1. Se l&#39;utente **conferma** le autorizzazioni, il flusso di autenticazione regolare verranno conservati e i token verranno memorizzati nell’archiviazione globale. Gli utenti rimarranno autenticati nell’app e tra le app utilizzando l’autenticazione di Adobe Pass per tutto il tempo in cui i token sono validi.
1. Se l&#39;utente **nega** le autorizzazioni, le azioni di scrittura nell’archiviazione non riusciranno e gli utenti verranno autenticati solo fino a quando non usciranno dall’app. Tieni presente che alcune applicazioni vengono reinizializzate quando si passa dal primo piano al background, in modo che gli utenti vengano disconnessi quando si esegue questa azione. I token NON sono memorizzati e gli utenti dovranno autenticarsi ogni volta che utilizzano l’app.


>[!TIP]
>
>Una funzione che introduce la resilienza dello storage è attualmente in fase di sviluppo per l’SDK 1.9 per l’autenticazione di Adobe Pass. Il nuovo SDK è destinato a **rilascio nell’ultima settimana di ottobre**. L’applicazione eseguirà il fallback alla scrittura nell’archiviazione sandbox dell’applicazione ogni volta che non è possibile utilizzare l’archiviazione generale. In questo caso, per le applicazioni sviluppate in API di livello 23, gli utenti NON accettano le autorizzazioni di lettura/scrittura nell’archiviazione globale. I token vengono memorizzati singolarmente per app, il che significa che il Single Sign-On tra le app che utilizzano l’autenticazione Adobe Pass verrà disabilitato.


![](assets/android-permissions-request.png)

*Figura: Finestra di dialogo per la richiesta di autorizzazione per le app scritte nel targeting dell’API di livello 23*

>[!IMPORTANT]
>
> Adobi consigliati **i suoi partner a sviluppare app utilizzando l’API livello 22 (targetSdkVersion=22) o versioni precedenti per garantire la migliore esperienza utente possibile nel processo di autenticazione**.
