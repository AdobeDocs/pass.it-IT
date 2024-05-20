---
title: Rapporti
description: Scopri come i dati vengono aggregati nei rapporti della dashboard TVE.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Rapporti {#Reports}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Il **Rapporti** sezione del dashboard TVE fornisce accesso ai dati aggregati per i rapporti AuthN TTL, AuthZ TTL e SSO. Questi rapporti includono le integrazioni di canale con diversi MVPD in tutti [piattaforme](#platforms).

I rapporti ti consentono di filtrare i dati e raccogliere informazioni in [canali o MVPD specifici](#selecting-specific-channels-mvpds). Puoi anche esportare i rapporti in un file CSV per ulteriori analisi.

## Visualizzare i rapporti {#view-reports}

Per visualizzare un rapporto specifico, segui la procedura riportata di seguito.

1. Seleziona la **Rapporti** nel pannello sinistro.
1. Seleziona una delle seguenti schede per visualizzare ed esportare i dati aggregati dei canali e degli MVPD inclusi:
   * [Rapporti TTL AuthN](#authn-ttl-reports)
   * [Rapporti TTL AuthZ](#authz-ttl-reports)
   * [Rapporti SSO](#sso-reports)

   ![Tipo di report](assets/type-of-reports.png)

   *Tipo di report*

### Rapporti TTL AuthN {#authn-ttl-reports}

I rapporti TTL di autenticazione, denominati anche TTL (Authentication Time-To-Live), visualizzano la durata per la quale i token di autenticazione sono configurati per le integrazioni dei canali con vari MVPD in tutti [piattaforme](#platforms). Questi rapporti ti consentono di verificare per quanto tempo un utente rimane autenticato per un MVPD e una piattaforma specifici. I valori di durata sono presentati in formati facili da usare, come, **giorni**, **ore**, **minuti**, e **secondi**. La tabella Rapporti TTL AuthN presenta uno scorrimento orizzontale e verticale per adattarsi a schermi di dimensioni diverse.

Puoi anche visualizzare e scaricare i dati per [canali specifici o MVPD](#selecting-specific-channels-mvpds).

![Esporta rapporti TTL AuthN](assets/authn-ttl-reports.png)

*Esporta rapporti TTL AuthN*

>[!IMPORTANT]
>
> Il **Impostato da MVPD** Il segnaposto viene utilizzato quando MVPD applica il valore TTL AuthN anziché la configurazione di autenticazione di Adobe Pass.

Seleziona **Esportare rapporti** per salvare i dati come file CSV sul computer locale.

### Rapporti TTL AuthZ {#authz-ttl-reports}

I rapporti TTL di AuthZ, denominati anche TTL (Authorization Time-To-Live), visualizzano la durata del token di autorizzazione configurato per le integrazioni dei canali con vari MVPD in tutti [piattaforme](#platforms). Questi rapporti ti consentono di verificare per quanto tempo un utente rimane autorizzato a guardare contenuti per un MVPD e una piattaforma specifici. I valori di durata sono presentati in formati facili da usare, come, **giorni**, **ore**, **minuti**, e **secondi**. La tabella Rapporti TTL AuthZ presenta uno scorrimento orizzontale e verticale per adattarsi a schermi di dimensioni diverse.

Puoi anche visualizzare e scaricare i dati per [canali specifici o MVPD](#selecting-specific-channels-mvpds).

![Esporta rapporti TTL AuthZ](assets/authz-ttl-reports.png)

*Esporta rapporti TTL AuthZ*

>[!IMPORTANT]
>
> Il **Impostato da MVPD** Il segnaposto viene utilizzato quando MVPD applica il valore TTL AuthZ anziché la configurazione di autenticazione di Adobe Pass.

Seleziona **Esportare rapporti** per salvare i dati come file CSV sul computer locale.

### Rapporti SSO {#sso-reports}

I rapporti SSO, denominati anche single sign-on, visualizzano lo stato single sign-on configurato per le integrazioni dei canali con vari MVPD in tutti [piattaforme](#platforms). Questi rapporti ti consentono di verificare l’esperienza SSO di autenticazione utente prevista per un MVPD e una piattaforma specifici. I valori sono presentati in formati facili da usare, come, **SSO disabilitato**, **SSO abilitato**, e **SSO incerto**. La tabella Rapporti SSO presenta uno scorrimento orizzontale e verticale per adattarsi a schermi di dimensioni diverse.

Puoi anche visualizzare e scaricare i dati per [canali specifici o MVPD](#selecting-specific-channels-mvpds).

![Esporta rapporti SSO](assets/sso-reports.png)

*Esporta rapporti SSO*

>[!IMPORTANT]
>
> Il **SSO incerto** Il segnaposto indica che Single Sign-On (SSO) è abilitato e potenzialmente operativo. Tuttavia, le impostazioni elencate di seguito possono impedire l&#39;autenticazione SSO, come spiegato nei seguenti esempi:
>
> * Impostazioni piattaforma utente: opzione per bloccare i cookie di terze parti.
> * Decisioni dell’utente: gli utenti negano l’accesso alla piattaforma al proprio abbonamento al provider TV.
> * Impostazioni MVPD: MVPD richiede l’autenticazione per ogni canale.

Seleziona **Esportare rapporti** per salvare i dati come file CSV sul computer locale.

## Piattaforme {#platforms}

Il [Rapporti TTL AuthN](#authn-ttl-reports), [Rapporti TTL AuthZ](#authz-ttl-reports), e [Rapporti SSO](#sso-reports) presentare i dati su varie piattaforme, ad esempio:

* **Desktop**: visualizza i valori applicati alle implementazioni del programmatore tramite l’SDK JavaScript di autenticazione di Adobe Pass.

* **Dispositivi mobili**

  **iOS**: visualizza i valori applicati utilizzando l’SDK iOS per l’autenticazione di Adobe Pass.

  **Android**: visualizza i valori applicati tramite l’SDK Android per l’autenticazione di Adobe Pass.

  **Altro**: visualizza i valori applicati utilizzando l’API REST di autenticazione di Adobe Pass sviluppata per i dispositivi mobili.

* **TVCD**

  **Roku**: visualizza i valori applicati tramite l’API REST di autenticazione di Adobe Pass, identificando Roku come tipo di dispositivo.

  **FireTV**: visualizza i valori applicati tramite l’SDK FireTV per l’autenticazione di Adobe Pass.

  **AppleTV**: visualizza i valori applicati tramite l’SDK tvOS per l’autenticazione di Adobe Pass.

  **Altro**: visualizza i valori applicati utilizzando l’API REST di autenticazione di Adobe Pass per i dispositivi connessi al televisore.

* **Piattaforma non identificato**: visualizza i valori applicati alle implementazioni del programmatore quando i servizi di autenticazione di Adobe Pass rilevano un tipo di dispositivo sconosciuto.

Per ulteriori informazioni su come condividere il tipo di dispositivo desiderato, ad esempio **Roku** con le API REST o gli SDK di autenticazione di Adobe Pass, puoi visualizzare il meccanismo di [trasmissione delle informazioni client](/help/authentication/passing-client-information-device-connection-and-application.md).

>[!IMPORTANT]
>
> I dati aggregati si basano sulla configurazione specifica di ciascun ambiente di autenticazione di Adobe Pass. Quando si passa da un ambiente all’altro del dashboard TVE, si prevedono varianti nei dati tra i rapporti. Consulta la sezione [Ambienti di autenticazione Adobe Pass](/help/authentication/tve-dashboard-environments.md) per saperne di più.

## Selezione di canali e MVPD specifici {#selecting-specific-channels-mvpds}

Il [Rapporti TTL AuthN](#authn-ttl-reports), [Rapporti TTL AuthZ](#authz-ttl-reports), e [Rapporti SSO](#sso-reports) presentare i dati per **Tutti i canali** integrazioni con **Tutti gli MVPD** per impostazione predefinita.

>[!NOTE]
>
> Se si deseleziona **Tutti i canali** o **Tutti gli MVPD** nei rispettivi menu a discesa, viene visualizzato un messaggio per effettuare una selezione e visualizzare i rapporti significativi.

Per generare un rapporto per canali specifici:

1. Seleziona la **Canali inclusi** menu a discesa nella parte superiore del rapporto selezionato.

   ![Menu a discesa dei canali inclusi](assets/include-channels.png)

   *Menu a discesa dei canali inclusi*

1. Deseleziona **Tutti i canali**.
1. Seleziona i canali richiesti dalla **Canali inclusi** menu a discesa per il quale si desidera generare i dati.

>[!NOTE]
>
> Per rendere disponibili le opzioni in **MVPD inclusi** menu a discesa, è necessario selezionare almeno un canale nel **Canali inclusi** menu a discesa.

Per generare un rapporto per MVPD specifici:

1. Seleziona la **MVPD inclusi** menu a discesa nella parte superiore del rapporto selezionato.

   ![Menu a discesa MVPD incluso](assets/include-mvpds.png)

   *Menu a discesa MVPD incluso*

1. Deseleziona **Tutti gli MVPD**.
1. Seleziona gli MVPD richiesti dalla **MVPD inclusi** menu a discesa per il quale si desidera generare i dati.
