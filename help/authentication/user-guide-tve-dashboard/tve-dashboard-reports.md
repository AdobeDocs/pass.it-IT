---
title: Rapporti
description: Scopri come i dati vengono aggregati nei rapporti della dashboard TVE.
exl-id: d8ba48de-d743-4dc2-866c-7d6e3ff94773
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Rapporti {#Reports}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

La sezione **Report** del dashboard TVE fornisce l&#39;accesso ai dati aggregati per i report TTL AuthN, TTL AuthZ e SSO. Questi rapporti includono le integrazioni dei canali con diversi MVPD su tutte le [piattaforme](#platforms).

I report ti consentono di filtrare i dati e raccogliere informazioni tra [canali o MVPD specifici](#selecting-specific-channels-mvpds). Puoi anche esportare i rapporti in un file CSV per ulteriori analisi.

## Visualizzazione dei rapporti {#view-reports}

Per visualizzare un rapporto specifico, segui la procedura riportata di seguito.

1. Seleziona la scheda **Rapporti** nel pannello a sinistra.
1. Seleziona una delle seguenti schede per visualizzare ed esportare i dati aggregati dei canali e degli MVPD inclusi:
   * [Rapporti TTL AuthN](#authn-ttl-reports)
   * [Rapporti TTL AuthZ](#authz-ttl-reports)
   * [Rapporti SSO](#sso-reports)

   ![Tipo di report](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-tabs-view.png)

   *Tipo di report*

### Rapporti TTL AuthN {#authn-ttl-reports}

I rapporti TTL di autenticazione, denominati anche TTL (Authentication Time-To-Live), visualizzano la durata per la quale i token di autenticazione sono configurati per le integrazioni dei canali con vari MVPD su tutte le [piattaforme](#platforms). Questi rapporti ti consentono di verificare per quanto tempo un utente rimane autenticato per un MVPD e una piattaforma specifici. I valori di durata sono presentati in formati facili da usare, ad esempio **giorni**, **ore**, **minuti** e **secondi**. La tabella Rapporti TTL AuthN presenta uno scorrimento orizzontale e verticale per adattarsi a schermi di dimensioni diverse.

È inoltre possibile visualizzare e scaricare dati per [canali specifici o MVPD](#selecting-specific-channels-mvpds).

![Esporta rapporti TTL AuthN](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-authn-ttl-export-button.png)

*Esporta rapporti TTL AuthN*

>[!IMPORTANT]
>
> Il segnaposto **Impostato da MVPD** viene utilizzato quando MVPD applica il valore TTL AuthN anziché la configurazione di autenticazione Adobe Pass.

Seleziona **Esporta rapporti** per salvare i dati come file CSV nel computer locale.

### Rapporti TTL AuthZ {#authz-ttl-reports}

Nei rapporti TTL di AuthZ, denominati anche TTL (Authorization Time-To-Live), viene visualizzata la durata del token di autorizzazione configurato per le integrazioni dei canali con vari MVPD su tutte le [piattaforme](#platforms). Questi rapporti ti consentono di verificare per quanto tempo un utente rimane autorizzato a guardare contenuti per una piattaforma e un MVPD specifici. I valori di durata sono presentati in formati facili da usare, ad esempio **giorni**, **ore**, **minuti** e **secondi**. La tabella Rapporti TTL AuthZ presenta uno scorrimento orizzontale e verticale per adattarsi a schermi di dimensioni diverse.

Puoi anche visualizzare e scaricare i dati per [canali specifici o MVPD](#selecting-specific-channels-mvpds).

![Esporta rapporti TTL AuthZ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-authz-ttl-export-button.png)

*Esporta rapporti TTL AuthZ*

>[!IMPORTANT]
>
> Il segnaposto **Impostato da MVPD** viene utilizzato quando MVPD applica il valore TTL AuthZ anziché la configurazione di autenticazione Adobe Pass.

Seleziona **Esporta rapporti** per salvare i dati come file CSV nel computer locale.

### Rapporti SSO {#sso-reports}

I report SSO, denominati anche single sign-on, visualizzano lo stato di single sign-on configurato per le integrazioni dei canali con vari MVPD su tutte le [piattaforme](#platforms). Questi rapporti ti consentono di verificare l’esperienza SSO di autenticazione utente prevista per una piattaforma e un MVPD specifici. I valori sono presentati in formati facili da usare, ad esempio **SSO disabilitato**, **SSO abilitato** e **SSO incerto**. La tabella Rapporti SSO presenta uno scorrimento orizzontale e verticale per adattarsi a schermi di dimensioni diverse.

È inoltre possibile visualizzare e scaricare dati per [canali specifici o MVPD](#selecting-specific-channels-mvpds).

![Esporta report SSO](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-sso-export-button.png)

*Esporta report SSO*

>[!IMPORTANT]
>
> Il segnaposto **SSO incerto** indica che Single Sign-On (SSO) è abilitato e potenzialmente operativo. Tuttavia, le impostazioni elencate di seguito possono impedire l&#39;autenticazione SSO, come spiegato nei seguenti esempi:
>
> * Impostazioni piattaforma utente: opzione per bloccare i cookie di terze parti.
> * Decisioni dell’utente: gli utenti negano l’accesso alla piattaforma al proprio abbonamento al provider TV.
> * Impostazioni MVPD: MVPD richiede l’autenticazione per ogni canale.

Seleziona **Esporta rapporti** per salvare i dati come file CSV nel computer locale.

## Piattaforme {#platforms}

I [rapporti TTL AuthN](#authn-ttl-reports), [rapporti TTL AuthZ](#authz-ttl-reports) e [rapporti SSO](#sso-reports) presentano dati su varie piattaforme, ad esempio:

* **Desktop**: visualizza i valori applicati alle implementazioni del programmatore tramite Adobe Pass Authentication JavaScript SDK.

* **Dispositivi mobili**

  **iOS**: visualizza i valori applicati mediante l&#39;iOS SDK di autenticazione di Adobe Pass.

  **Android**: visualizza i valori applicati tramite Adobe Pass Authentication Android SDK.

  **Altri**: visualizza i valori applicati utilizzando l&#39;API REST di autenticazione Adobe Pass sviluppata per i dispositivi mobili.

* **TVCD**

  **Roku**: visualizza i valori applicati tramite l&#39;API REST per l&#39;autenticazione di Adobe Pass, identificando Roku come tipo di dispositivo.

  **FireTV**: visualizza i valori applicati tramite l&#39;autenticazione Adobe Pass FireTV SDK.

  **AppleTV**: visualizza i valori applicati tramite Adobe Pass Authentication tvOS SDK.

  **Altri**: visualizza i valori applicati utilizzando l&#39;API REST di autenticazione Adobe Pass per i dispositivi connessi alla TV.

* **Piattaforma non identificata**: visualizza i valori applicati alle implementazioni del programmatore quando i servizi di autenticazione di Adobe Pass rilevano un tipo di dispositivo sconosciuto.

Per ulteriori informazioni su come condividere il tipo di dispositivo desiderato, ad esempio **Roku** con API REST o SDK di autenticazione Adobe Pass, vedere il meccanismo di [trasmissione delle informazioni client](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

>[!IMPORTANT]
>
> I dati aggregati si basano sulla configurazione specifica di ciascun ambiente di autenticazione di Adobe Pass. Quando si passa da un ambiente all’altro del dashboard TVE, si prevedono varianti nei dati tra i rapporti. Per ulteriori informazioni, consulta [Ambienti di autenticazione Adobe Pass](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md).

## Selezione di canali e MVPD specifici {#selecting-specific-channels-mvpds}

Per impostazione predefinita, i [rapporti TTL AuthN](#authn-ttl-reports), [rapporti TTL AuthZ](#authz-ttl-reports) e [rapporti SSO](#sso-reports) presentano dati per **tutte le integrazioni Canali** con **tutti i MVPD**.

>[!NOTE]
>
> Se deselezioni **Tutti i canali** o **Tutti gli MVPD** nei rispettivi menu a discesa, viene visualizzato un messaggio per effettuare una selezione e visualizzare i rapporti significativi.

Per generare un rapporto per canali specifici:

1. Selezionare il menu a discesa **Canali inclusi** nella parte superiore del report selezionato.

   ![Menu a discesa Canali inclusi](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-included-channels-menu.png)

   *Menu a discesa Canali inclusi*

1. Deseleziona **Tutti I Canali**.

1. Selezionare i canali richiesti dal menu a discesa **Canali inclusi** per i quali si desidera generare i dati.

>[!NOTE]
>
> Per rendere disponibili le opzioni nel menu a discesa **MVPDs** inclusi, è necessario selezionare almeno un canale nel menu a discesa **Canali inclusi**.

Per generare un rapporto per MVPD specifici:

1. Selezionare il menu a discesa **MVPDs** inclusi nella parte superiore del report selezionato.

   ![Menu a discesa MVPD incluso](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-included-mvpds-menu.png)

   *Menu a discesa MVPD incluso*

1. Deselezionare **Tutti gli MVPD**.

1. Seleziona gli MVPD richiesti dal menu a discesa **MVPD inclusi** per i quali desideri generare i dati.
