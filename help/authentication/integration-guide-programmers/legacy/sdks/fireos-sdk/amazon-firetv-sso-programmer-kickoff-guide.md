---
title: Amazon fireTV SSO - Guida introduttiva per programmatori
description: Amazon fireTV SSO - Guida introduttiva per programmatori
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 0%

---

# (Legacy) Amazon fireTV SSO - Guida introduttiva per programmatori {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

</br>

## Introduzione {#intro}

In questo documento vengono descritte le informazioni necessarie per integrare il nuovo SDK FireTV **di autenticazione di** Adobe Pass nell&#39;applicazione fireTV. Questo nuovo SDK sfrutta l&#39;integrazione a livello di sistema operativo sulla piattaforma FireTV di Amazon, offrendo così il supporto di **Single Sign On**. Per beneficiare del Single Sign-On è necessario un po’ di impegno da parte tua per migrare l’applicazione dall’API Clientless al nuovo SDK fireTV. Di seguito sono riportate alcune modifiche nei flussi di autenticazione.

## Architettura di alto livello e integrazione a livello di sistema operativo {#high}

Per ottenere il Single Sign On tra le applicazioni TV Everywhere sulla piattaforma Amazon fireTV e migliorare l’esperienza complessiva su questa piattaforma abbiamo deciso di integrare il nostro SDK principale a livello di sistema operativo fireTV. I programmatori dovranno eseguire la compilazione in base a una libreria stub fornita da Adobe. Le funzionalità effettive verranno fornite dalla libreria di Adobe presente nel sistema operativo FireTV di Amazon.

Finché Amazon non fornirà un simulatore fireTV che incorpori la nostra libreria a livello di sistema operativo, lo sviluppo sarà possibile solo utilizzando veri dispositivi fireTV.

## Vantaggi {#bene}

* Single Sign-On tra tutte le applicazioni TV Everywhere alimentate da Adobe sulla piattaforma Amazon fireTV con tutti gli MVPD integrati.
* Possibilità di usufruire di HBA (con MVPD supportati).
* Possibilità di utilizzare la versione più recente di fireTV SDK senza dover aggiornare le applicazioni ogni volta che viene rilasciata una nuova versione di SDK.
* Tutte le app TVE traggono vantaggio dall&#39;utilizzo della libreria di sistema condivisa, eliminando la necessità di disporre di una copia locale della libreria AccessEnabler. Questo assicura anche che tutte le applicazioni utilizzino la stessa versione di SDK.
* Autenticazione a schermo singolo: non è necessario inserire il codice di registrazione e i flussi di lavoro di seconda schermata.

## Migrazione dall’app basata su API client all’app basata su SDK fireTV {#migra1}

Per migrare dall’API Clientless a fireTV SDK è necessario rimuovere la base di codice relativa all’API Clientless e integrare il nuovo SDK fireTV.

Rispetto all’app basata su API Clientless, con il nuovo SDK fireTV l’autenticazione passa alla prima schermata, non è più necessaria una seconda autenticazione a schermo.

Questo richiede ai programmatori di aggiungere un selettore MVPD nelle loro app in modo che gli utenti possano scegliere il proprio provider TV direttamente sul dispositivo FireTV. Dopo aver selezionato MVPD, all&#39;utente viene visualizzata la pagina di accesso MVPD sul dispositivo fireTV.

I wireframe dei flussi utente che descrivono gli scenari regolari, HBA e SSO su fireTV sono disponibili all&#39;indirizzo [Flusso utente di accesso MVPD di Amazon Fire TV](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## Migrazione dall’app basata su SDK Android all’app basata su SDK fireTV {#migra2}

Questo nuovo SDK fireTV è molto simile al nostro SDK Android esistente e la documentazione corrente di cui disponiamo per l&#39;**integrazione del nostro SDK Android** <!--http://tve.helpdocsonline.com/android-technical-overview-->può essere utilizzata fino a quando non saranno pronti i documenti SDK fireTV. Se disponi già di applicazioni Android che utilizzano Android SDK, l’integrazione di fireTV SDK nell’applicazione fireTV dovrebbe essere semplice.

Rispetto al SDK Android esistente, su fireTV SDK il processo di autenticazione sarà più semplice da sviluppare, in quanto le attività di gestione/presentazione della pagina di accesso di MVPD e recupero del token AuthN verranno eseguite internamente dalla libreria AccessEnabler.

## Domande frequenti {#faq}

1. Come funzionerà **SSO**?

   * SSO funziona su tutte le applicazioni per programmatori con autenticazione Adobe Pass che utilizzano il nuovo SDK fireTV sullo stesso dispositivo Amazon fireTV
   * L&#39;SSO tra le app programmatrici implementate nell&#39;API REST senza client e le app implementate in FireTV SDK **NON sarà supportato**

1. Qual è la copertura MVPD di FireTV SSO?

   * **Tutti gli MVPD** integrati tramite l&#39;autenticazione Adobe Pass saranno tecnicamente supportati dal SDK FireTV.

1. Oltre a utilizzare il nuovo SDK, quali altre **modifiche del flusso di lavoro** devono essere rilevate dai programmatori?

   * I programmatori devono implementare un selettore MVPD per la piattaforma FireTV.

1. L&#39;autenticazione **TTLs** verrà modificata?

   * Non cambia il comportamento relativo ai TTL di autenticazione.
   * Il primo token di autenticazione valido verrà utilizzato per l&#39;esecuzione dell&#39;SSO e in questo caso tutte le altre applicazioni che verranno autenticate tramite SSO utilizzeranno lo stesso TTL fino alla scadenza. Pertanto, quando si passa da un’applicazione all’altra, la seconda applicazione condividerà il TTL della prima applicazione che si autentica.

1. Funzionamento dell&#39;**API di degradazione**

   * Non sono necessarie modifiche per l’API di degradazione, l’esperienza utente sarà la stessa dei dispositivi Android.

1. Effetti sui flussi **TempPass**

   * I flussi di TempPass sono a schermo singolo e si comportano come su qualsiasi altro dispositivo nativo.

1. Altre funzionalità di Adobe funzioneranno come prima?

   * Tutte le funzionalità di autenticazione di Adobe Pass funzioneranno su fireTV come sui dispositivi Android.
