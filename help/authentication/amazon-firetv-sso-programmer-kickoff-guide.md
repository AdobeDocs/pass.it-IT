---
title: Amazon fireTV SSO - Guida introduttiva per programmatori
description: Amazon fireTV SSO - Guida introduttiva per programmatori
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 0%

---

# Amazon fireTV SSO - Guida introduttiva per programmatori {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>

## Introduzione {#intro}

Questo documento descrive le informazioni necessarie per integrare il nuovo **SDK fireTV per l’autenticazione Adobe Pass** nell&#39;applicazione fireTV. Questo nuovo SDK sfrutta l’integrazione a livello di sistema operativo sulla piattaforma FireTV di Amazon, offrendo così **Single Sign-On** supporto. Per beneficiare del Single Sign-On è necessario un po’ di impegno da parte tua per migrare l’applicazione dall’API senza client al nuovo SDK fireTV. Di seguito sono riportate alcune modifiche nei flussi di autenticazione.

## Architettura di alto livello e integrazione a livello di sistema operativo {#high}

Per ottenere il Single Sign On tra le applicazioni TV Everywhere sulla piattaforma Amazon fireTV e migliorare l’esperienza complessiva su questa piattaforma abbiamo deciso di integrare il nostro SDK principale a livello di sistema operativo fireTV. I programmatori dovranno eseguire la compilazione in base a una libreria stub fornita da Adobe. Le funzionalità effettive verranno fornite dalla libreria di Adobe presente nel sistema operativo FireTV di Amazon.

Finché Amazon non fornirà un simulatore fireTV che incorpori la nostra libreria a livello di sistema operativo, lo sviluppo sarà possibile solo utilizzando veri dispositivi fireTV.

## Vantaggi {#bene}

* Single Sign-On tra tutte le applicazioni TV Everywhere alimentate da Adobi sulla piattaforma Amazon fireTV con tutti gli MVPD integrati.
* Possibilità di usufruire di HBA (con MVPD supportati).
* Possibilità di utilizzare la versione più recente dell’SDK fireTV senza dover aggiornare le applicazioni ogni volta che viene rilasciata una nuova versione SDK.
* Tutte le app TVE traggono vantaggio dall&#39;utilizzo della libreria di sistema condivisa, eliminando la necessità di disporre di una copia locale della libreria AccessEnabler. Questo assicura anche che tutte le applicazioni utilizzino la stessa versione SDK.
* Autenticazione a schermo singolo: non è necessario inserire il codice di registrazione e i flussi di lavoro di seconda schermata.

## Migrazione dall’app basata su API client all’app basata su SDK fireTV {#migra1}

Per migrare dall’API client all’SDK fireTV è necessario rimuovere la base di codice relativa all’API client e integrare il nuovo SDK fireTV.

Rispetto all’app basata su API Clientless, con il nuovo SDK fireTV l’autenticazione passa alla prima schermata, non è più necessaria una seconda autenticazione schermata.

Questo richiede ai programmatori di aggiungere un selettore MVPD nelle loro app in modo che gli utenti possano scegliere il proprio provider TV direttamente sul dispositivo FireTV. Dopo la selezione di MVPD, all&#39;utente viene visualizzata la pagina di accesso MVPD sul dispositivo fireTV.

I wireframe dei flussi utente che descrivono gli scenari regolari, HBA e SSO su fireTV sono disponibili all&#39;indirizzo [Flusso utente per l&#39;accesso a Amazon Fire TV - MVVPD](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## Migrazione dall’app basata su SDK per Android all’app basata su SDK fireTV {#migra2}

Questo nuovo SDK fireTV è molto simile al nostro SDK Android esistente e alla documentazione corrente disponibile per **integrazione dell’SDK per Android** <!--http://tve.helpdocsonline.com/android-technical-overview-->può essere utilizzato finché non sono pronti i documenti SDK di fireTV. Se disponi già di applicazioni Android che utilizzano il nostro SDK Android, l’integrazione dell’SDK fireTV nell’applicazione fireTV dovrebbe essere semplice.

Rispetto all’SDK Android esistente, su FireTV SDK il processo di autenticazione sarà più semplice da sviluppare, poiché le attività di gestione/presentazione della pagina di accesso MVPD e recupero del token AuthN verranno eseguite internamente dalla libreria AccessEnabler.

## Domande frequenti {#faq}

1. In che modo **SSO** lavoro?

   * SSO funziona su tutte le applicazioni per programmatori con autenticazione Adobe Pass che utilizzano il nuovo SDK fireTV sullo stesso dispositivo Amazon fireTV
   * SSO tra le app programmatrici implementate nell’API REST senza client e le app implementate nell’SDK fireTV **NON sarà supportato**

1. Qual è la copertura MVPD di FireTV SSO?

   * **Tutti gli MVPD** integrato tramite l’autenticazione di Adobe Pass sarà tecnicamente supportato SSO sull’SDK di fireTV.

1. Oltre a utilizzare il nuovo SDK, quali altri **modifiche al flusso di lavoro** I programmatori dovrebbero esserne a conoscenza?

   * I programmatori devono implementare un selettore MVPD per la piattaforma FireTV.

1. L’autenticazione verrà modificata **TTL**?

   * Non cambia il comportamento relativo ai TTL di autenticazione.
   * Il primo token di autenticazione valido verrà utilizzato per l&#39;esecuzione dell&#39;SSO e in questo caso tutte le altre applicazioni che verranno autenticate tramite SSO utilizzeranno lo stesso TTL fino alla scadenza. Pertanto, quando si passa da un’applicazione all’altra, la seconda applicazione condividerà il TTL della prima applicazione che si autentica.

1. Come **API di degradazione** lavoro?

   * Non sono necessarie modifiche per l’API di degradazione, l’esperienza utente sarà la stessa dei dispositivi Android.

1. Come **TempPass** I flussi sono interessati?

   * I flussi di TempPass sono a schermo singolo e si comportano come su qualsiasi altro dispositivo nativo.

1. Altre funzionalità di Adobe funzioneranno come prima?

   * Tutte le funzionalità di autenticazione di Adobe Pass funzioneranno su fireTV come su dispositivi Android.
