---
title: Autenticazione di Adobe Pass e il nuovo modello di autorizzazioni "Marshmallow" di Android 6
description: Autenticazione di Adobe Pass e il nuovo modello di autorizzazioni "Marshmallow" di Android 6
exl-id: 3c96769e-b25b-48ab-bb74-40f13d4e5a84
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# (Legacy) Autenticazione di Adobe Pass e il nuovo modello di autorizzazioni &quot;Marshmallow&quot; di Android 6 {#adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

</br>

La nuova versione Marshmallow di Android 6 introduce alcuni aggiornamenti al modello di autorizzazioni, che possono influenzare il comportamento delle app che utilizzano l’autenticazione SDK di Adobe Pass versione 1.8 e precedenti.

Il nuovo sistema operativo Android offre un controllo granulare di [sulle autorizzazioni necessarie per le app al momento dell&#39;installazione e in fase di esecuzione](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html).

>[!IMPORTANT]
>
>Le modifiche descritte di seguito **interesseranno solo le applicazioni sviluppate specificamente per Android 6.0** (targetSdkVersion=23). Non influisce sulle applicazioni meno recenti già installate sul dispositivo dell’utente durante l’aggiornamento ad Android 6.0.


In particolare, per le app sviluppate in Android Studio che utilizzano il livello [API 23](http://developer.android.com/sdk/api_diff/23/changes.html) e che utilizzano Adobe Pass Authentication SDK, lo sviluppatore dovrà scrivere un codice personalizzato (vedi frammento di codice di seguito) [per attivare la finestra di dialogo per l&#39;autorizzazione](https://developer.android.com/training/permissions/requesting.html).

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




**Dal punto di vista degli utenti**, al momento dell&#39;installazione gli utenti vengono accolti da una finestra in cui viene richiesto di confermare le autorizzazioni di lettura/scrittura per i file (vedere la figura 2 seguente). Questo porta a uno dei due risultati seguenti:

1. Se l&#39;utente **conferma** le autorizzazioni, il normale flusso di autenticazione verrà mantenuto e i token verranno archiviati nell&#39;archiviazione globale. Gli utenti rimarranno autenticati nell’app e tra le app utilizzando l’autenticazione di Adobe Pass per tutto il tempo in cui i token sono validi.
1. Se l&#39;utente **nega** le autorizzazioni, le azioni di scrittura nell&#39;archivio non riusciranno e gli utenti verranno autenticati solo fino a quando non usciranno dall&#39;app. Tieni presente che alcune applicazioni vengono reinizializzate quando si passa dal primo piano al background, in modo che gli utenti vengano disconnessi quando si esegue questa azione. I token NON sono memorizzati e gli utenti dovranno autenticarsi ogni volta che utilizzano l’app.


>[!TIP]
>
>Una funzione che introduce la resilienza dello storage è attualmente in fase di sviluppo per Adobe Pass Authentication SDK 1.9. Il nuovo SDK verrà rilasciato **nell&#39;ultima settimana di ottobre**. L’applicazione eseguirà il fallback alla scrittura nell’archiviazione sandbox dell’applicazione ogni volta che non è possibile utilizzare l’archiviazione generale. In questo caso, per le applicazioni sviluppate in API di livello 23, gli utenti NON accettano le autorizzazioni di lettura/scrittura nell’archiviazione globale. I token vengono memorizzati singolarmente per app, il che significa che il Single Sign-On tra le app che utilizzano l’autenticazione Adobe Pass verrà disabilitato.


![](/help/authentication/assets/android-permissions-request.png)

*Figura: finestra di dialogo della richiesta di autorizzazione per le app scritte nel targeting API livello 23*

>[!IMPORTANT]
>
> Adobe consiglia ai **partner di sviluppare app utilizzando l&#39;API di livello 22 (targetSdkVersion=22) o versioni precedenti per garantire la migliore esperienza utente possibile nel processo di autenticazione**.
