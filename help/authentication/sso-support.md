---
title: Supporto Single Sign-On
description: Supporto Single Sign-On
exl-id: edc3719e-c627-464c-9b10-367a425698c6
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1144'
ht-degree: 0%

---

# Supporto Single Sign-On

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview-sso-support}

Questo documento descrive i tipi di Single Sign-On supportati e basati sull’autenticazione Adobe Pass su piattaforme diverse. Lo scopo di questo documento è quello di fare luce su ciò che è supportato e ciò che non lo è, qual è la copertura MVPD per ogni metodo SSO e ciò che è richiesto ai programmatori per poter beneficiare dell&#39;SSO su ogni piattaforma.

Dopo che un utente accede con le proprie credenziali MVPD, l’autenticazione di Adobe Pass genera un token protetto che rappresenta la sessione di autenticazione di MVPD e associa tale token al dispositivo dell’utente utilizzando un ID dispositivo. L’autenticazione Adobe Pass memorizza il token o l’ID dispositivo su un server o sul dispositivo. In questo modo gli utenti possono immettere le credenziali con minore frequenza, mantenendo al contempo sicure le transazioni.

>[!NOTE]
>
>I flussi di lavoro SSO fanno parte del pacchetto Flusso di lavoro Premium. Se sei interessato a utilizzare questa funzionalità, contatta il rappresentante commerciale Adobe Pass.

## Stato corrente per SSO su varie piattaforme {#current-sso-status-platforms}

| Piattaforma/Dispositivo | Supporto SSO | Tipo SSO | Copertura MVPD | Note |
|:-------------------:|:-----------:|:---------------------------------------:|-----------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Web (JavaScript) | Sì | Token di autenticazione condivisa (Adobe SSO) | Tutti | Nessun SSO tra browser Seguire le istruzioni riportate nella Guida all&#39;integrazione dei programmatori per JavaScript. Dopo aver seguito le istruzioni, SSO viene abilitato per impostazione predefinita.  L&#39;abilitazione dell&#39;autenticazione per richiedente interrompe l&#39;SSO |
| iOS | Sì | SSO piattaforma - scambio token | A seconda del supporto di Apple: l’elenco è qui | Da iOS 10, Apple e Adobe hanno introdotto la funzionalità SSO per i programmatori e gli MVPD partecipanti. Utilizzando l’SDK di iOS di Adobe più recente o l’API REST senza client di Adobe e implementando la funzionalità SSO di Apple è possibile beneficiare dell’SSO sui dispositivi iOS. Ulteriori dettagli sull’implementazione dell’SDK qui e ulteriori dettagli sull’implementazione senza client qui. Note aggiuntive: - Se non desideri utilizzare l’SSO di Apple, puoi comunque disporre di un SSO limitato tra le app dello stesso fornitore (stesso ID bundle) che possono condividere lo storage e un ID (IDFV), pertanto l’SSO è limitato solo alle app dello stesso fornitore. |
| Android | Sì | Token di autenticazione condivisa (Adobe SSO) | Tutti | Se l’utente non accetta la richiesta di autorizzazione WRITE_EXTERNAL_STORAGE, la libreria utilizzerà un archivio locale in modalità sandbox. L’implicazione in questo caso è che non ci sarà SSO tra applicazioni diverse quando si utilizza l’archiviazione locale. |
| tvOS - nuova Apple TV | Sì | SSO piattaforma - scambio token | A seconda del supporto di Apple: l’elenco è qui | Da tvOS 10, Apple e Adobe hanno introdotto la funzionalità SSO per i programmatori e gli MVPD partecipanti. Utilizzando l’SDK per tvOS di Adobe più recente o utilizzando l’API REST senza client di Adobe e implementando la funzionalità SSO di Apple è possibile beneficiare dell’SSO sui dispositivi tvOS. Maggiori dettagli su tvOS SDK: qui e qui e ulteriori dettagli sull’implementazione senza client qui. |
| Roku | Sì | Token di autenticazione condivisa (Adobe SSO) | L&#39;elenco completo delle coperture significative sarà fornito a breve. | Roku SSO funziona con l’API senza client per tutti i clienti che rispettano le linee guida Roku, senza richiedere alcuna implementazione speciale. L’SSO si basa sulle informazioni di identificazione del dispositivo che Roku invia in modo sicuro all’Adobe. |
| Amazon FireTV | Sì | Token di autenticazione condivisa (Adobe SSO) | L&#39;elenco completo delle coperture significative sarà fornito a breve. | L’SDK di FireTV fornisce il supporto per Single Sign-On in base alle funzionalità di Android. L’SSO su questa piattaforma è possibile solo tra app che per il momento utilizzano l’SDK Adobe FireTV. Ulteriori informazioni sul nuovo SDK di FireTV qui. Le app FireTV implementate sopra l’API Clientless potranno beneficiare dell’SSO entro EOY 2018. |
| Xbox 360 | No |                                         |                                                     | Nessun ID dispositivo utilizzabile. Esiste un ID app, quindi gli utenti non devono autenticarsi ogni volta. |
| Xbox One | No |                                         |                                                     | Nessun ID dispositivo utilizzabile. Esiste un ID app, quindi gli utenti non devono autenticarsi ogni volta. |
| Windows 8/10 | No |                                         |                                                     | Nessun ID dispositivo utilizzabile. Esiste un ID app, quindi gli utenti non devono autenticarsi ogni volta. |
| TV Samsung | No |                                         |                                                     | Nessun ID dispositivo utilizzabile. Esiste un ID app, quindi gli utenti non devono autenticarsi ogni volta. |

### Note su Xbox 360 e Xbox One {#notes-xbox-360}

* **Xbox 360**- Xbox 360 si basa sul servizio Live per fornire il token che incorpora l&#39;ID dispositivo. Il servizio live si trova nel valore appID per deviceID, e ha ambito solo per l’app. Per Xbox 360, Microsoft ha fornito all&#39;Adobe una libreria Java per facilitare l&#39;analisi del token.

* **Xbox One**- Verrà emesso un token Web JSON crittografato con il certificato/chiave dell&#39;editore e firmato da Microsoft. L&#39;Adobe estrae l&#39;ID dispositivo da un parametro denominato DPI (Device Pairwise ID), diverso dal parametro Xbox 360 PDID (Partner Device ID). Il PDID esiste anche in Xbox One ma deve essere sostituito da questo nuovo parametro &quot;Device Pairwise ID&quot; (DPI).


### Disabilitazione dell’SSO {#disable-sso}

In alcune situazioni, alcune app o alcuni siti vorranno disabilitare l’SSO per soddisfare i casi aziendali avanzati.

* **Per JS e SDK nativi** - Il team di supporto per l&#39;autenticazione di Adobe Pass può disabilitare l&#39;SSO per una coppia ID richiedente/MVPD. Non è necessario lavorare sui siti o nelle app native.  Una volta che l’SSO è stato disabilitato dal team di supporto per l’autenticazione di Adobe Pass, le autenticazioni eseguite utilizzando la coppia RequestorId/MVPD specificata non verranno condivise con siti o app che utilizzano ID richiedente diversi. Inoltre, le autenticazioni esistenti con ID richiedente diversi non saranno valide per la combinazione ID richiedente/MVPD in cui SSO è stato disabilitato. Tecnicamente, la disabilitazione SSO viene eseguita associando il token AuthN alla specifica combinazione ID richiedente/MVPD.
* **Per API senza client** - È possibile disabilitare l&#39;SSO nel flusso di autenticazione senza client specificando un parametro appId non vuoto nelle chiamate REST. Puoi utilizzare qualsiasi stringa come valore, purché tale stringa sia univoca per l’ID richiedente. Tieni presente che per l’API Clientless, il programmatore/implementatore deve modificare il sito o l’app per aggiungere questo parametro specifico per il richiedente.

>[!IMPORTANT]
>
>NOTA IMPORTANTE PER l’SSO API SENZA CLIENT: alcuni MVPD richiedono che ogni rete (ID richiedente) esegua il proprio flusso di autenticazione. Per i flussi basati sull’SDK (iOS, ecc.), questo viene gestito automaticamente dall’SDK. Tuttavia, per le API senza client questo deve essere gestito dal programmatore. Consigliamo vivamente ai programmatori di non abilitare a questo punto i flussi SSO per le API senza client e di utilizzare invece una combinazione ID dispositivo + ID app per l’ID dispositivo. Adobe si occuperà anche di migliorare i flussi API senza client in modo da poter stabilire un SSO corretto.

### Disconnetti {#logout-sso-support}

I programmatori devono essere consapevoli del fatto che l’azione &quot;Logout&quot; nel contesto del Single Sign-On, quando eseguita in un’app/su un sito, eliminerà tutti i token sul dispositivo e l’utente verrà disconnesso da un app/sito all’altro.

Se vengono soddisfatte le condizioni SSO (indipendentemente dal fatto che l&#39;SSO sia abilitato o disabilitato), verrà eseguita la disconnessione e verranno eliminate tutte le informazioni di autenticazione e autorizzazione.
