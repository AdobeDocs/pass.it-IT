---
title: Accesso a Single Sign-On (SSO) SDK per Android su app Android 10
description: Accesso a Single Sign-On (SSO) SDK per Android su app Android 10
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# Accesso a Single Sign-On (SSO) SDK per Android su app Android 10 {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica

Il Single Sign-On (SSO) tra app basate sull’autenticazione di Adobe Pass è disponibile sui dispositivi che utilizzano il sistema operativo Android tramite l’SDK per Android di Access Enabler. Per offrire il Single Sign-On (SSO) sui dispositivi Android, l’SDK per Android Access Enabler versione 3.2.1 (più recente) e le versioni precedenti utilizzano un file di database condiviso salvato in un’implementazione di archiviazione Android, accessibile da tutte le app basate sull’autenticazione di Adobe Pass.

Tuttavia, nell’ultima versione di Android 10, Google ha prodotto alcune modifiche &quot;per dare agli utenti un maggiore controllo sui loro file e limitare l’ingombro dei file, per impostazione predefinita alle app destinate ad Android 10 (livello API 29) e versioni successive viene concesso l’accesso con ambito in un dispositivo di archiviazione esterno, o archiviazione con ambito. Tali app possono visualizzare solo la directory specifica dell’app `\[...\]`&quot;. Maggiori dettagli relativi a queste modifiche all’archiviazione Android 10 sono presentati in [Documentazione sull’archiviazione di dati e file per Android](https://developer.android.com/training/data-storage/files/external-scoped).

In seguito a queste modifiche, il Single Sign-On (SSO) offerto dalla versione di Access Enabler Android **3.2.1 SDK (più recente)** e le versioni precedenti possono essere influenzate su dispositivi Android 10 come spiegato nella sezione successiva.

Consulta [Panoramica di Roku SSO](/help/authentication/roku-sso-overview.md).

## Comportamento

A seconda dell’app **[!UICONTROL target SDK level]** o l&#39;utilizzo di **android:requestLegacyExternalStorage** attributo manifest Il Single Sign-On (SSO) offerto dall&#39;SDK Access Enabler Android versione 3.2.1 (più recente) e dalle versioni precedenti si comporta attualmente come segue:

- Destinazioni app **Android 9 (livello API 28)** o inferiore **-\>** Single Sign-On (SSO) **funzionerà**
- Destinazioni app **Android 10** **(livello API 29)** e fa **set** il valore di **requestLegacyExternalStorage su true** nel file manifesto dell’app **-\>** Single Sign-On (SSO) **funzionerà**
- Destinazioni app **Android 10** **(livello API 29)** e fa **non impostato** il valore di **requestLegacyExternalStorage su true** nel file manifesto dell’app **-\>** Single Sign-On (SSO) **non funzionerà**


>[!TIP]
>
> Prima che l’SDK Android di Adobe Pass Authentication Access Enabler sia completamente compatibile con l’archiviazione nell’ambito, è possibile rinunciare temporaneamente in base al livello SDK di destinazione dell’app o all’attributo del manifesto requestLegacyExternalStorage come spiegato in pubblico [Documentazione Android](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage).
