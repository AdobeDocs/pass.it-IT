---
title: Accesso a Android SDK Single Sign-On (SSO) con attivatore su app Android 10
description: Accesso a Android SDK Single Sign-On (SSO) con attivatore su app Android 10
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 0%

---

# (Legacy) Accesso a Android SDK Single Sign-On (SSO) su app Android 10 {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica

Il Single Sign-On (SSO) tra app basate sull’autenticazione di Adobe Pass è disponibile sui dispositivi che utilizzano il sistema operativo Android tramite Access Enabler Android SDK. Per offrire il Single Sign-On (SSO) sui dispositivi Android, Access Enabler Android SDK versione 3.2.1 (più recente) e le versioni precedenti utilizzano un file di database condiviso salvato in un’implementazione di archiviazione Android, accessibile da tutte le app basate sull’autenticazione di Adobe Pass.

Tuttavia, nell’ultima versione di Android 10, Google ha prodotto alcune modifiche &quot;per dare agli utenti un maggiore controllo sui loro file e limitare l’ingombro dei file. Per impostazione predefinita, alle app destinate a Android 10 (livello API 29) e versioni successive viene assegnato l’accesso con ambito in un dispositivo di archiviazione esterno, o archiviazione con ambito. Tali app possono visualizzare solo la directory `\[...\]` specifica per l&#39;app.&quot; Ulteriori dettagli relativi a queste modifiche all&#39;archiviazione di Android 10 sono presentati in [Documentazione sull&#39;archiviazione dei dati e dei file per Android](https://developer.android.com/training/data-storage/files/external-scoped).

Come conseguenza di queste modifiche, l&#39;SSO (Single Sign-On) offerto da Access Enabler Android versione **3.2.1 SDK (più recente)** e versioni precedenti può essere influenzato sui dispositivi Android 10 come descritto nella sezione successiva.

## Comportamento

A seconda dell&#39;attributo del manifesto **[!UICONTROL target SDK level]** dell&#39;app o dell&#39;utilizzo di **android:requestLegacyExternalStorage**, l&#39;SSO (Single Sign-On) offerto da Access Enabler Android versione 3.2.1 SDK (più recente) e dalle versioni precedenti si comporterà attualmente come segue:

- L&#39;app è destinata a **Android 9 (livello API 28)** o a **-\>** Single Sign-On (SSO) **funzionerà**
- L&#39;app è destinata a **Android 10** **(livello API 29)** e **imposta** il valore di **requestLegacyExternalStorage su true** nel file manifesto dell&#39;app **-\>** Single Sign-On (SSO) **funzionerà**
- L&#39;app è destinata a **Android 10** **(livello API 29)** e **non imposta** il valore di **requestLegacyExternalStorage su true** nel file manifesto dell&#39;app **-\>** Single Sign-On (SSO) **non funzionerà**

>[!TIP]
>
> Prima che Adobe Pass Authentication Access Enabler Android SDK sia completamente compatibile con l&#39;archiviazione con ambito, è possibile rinunciare temporaneamente in base al livello di SDK di destinazione dell&#39;app o all&#39;attributo del manifesto requestLegacyExternalStorage, come spiegato nella [documentazione di Android](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage) pubblica.
