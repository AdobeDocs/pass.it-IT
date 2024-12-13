---
title: Panoramica di Apple SSO
description: Panoramica di Apple SSO
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1260'
ht-degree: 0%

---

# Panoramica di Apple SSO {#apple-sso-overview}

>[!IMPORTANT]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Apple offre agli utenti la possibilità di accedere al proprio account di provider TV a livello di sistema del dispositivo, eliminando la necessità di eseguire l’autenticazione app per app.

Adobe Pass Authentication ha collaborato con Apple per creare l&#39;esperienza utente Single Sign-On (SSO) del partner nell&#39;ecosistema TV Everywhere per i proprietari di iPhone, iPad e Apple TV.

Per beneficiare dell’esperienza utente Single Sign-On (SSO) su un dispositivo Apple, di seguito è riportato un elenco di prerequisiti da completare.

Il risultato finale dovrebbe creare un’esperienza in linea con i seguenti flussi di utenti, che consigliamo di consultare prima di iniziare a sviluppare l’applicazione:

* Flussi di utenti Single Sign-On (SSO) [per dispositivi iPhone e iPad](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf).
* Flussi di utenti Single Sign-On (SSO) [per dispositivi Apple TV](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf).

## Prerequisiti {#apple-sso-prerequisites}

I prerequisiti per l’onboarding possono essere applicati a una o più entità coinvolte nel business TVE, come programmatori, MVPD, autenticazione Adobe Pass o Apple.

### Programmatore {#apple-sso-prerequisites-programmer}

Per beneficiare dell&#39;esperienza utente Single Sign-On (SSO), un programmatore deve:

* Contatta Apple per abilitare il [framework dell&#39;account del sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount) come parte del tuo ID team Apple e configura il [diritto Single Sign-On del sottoscrittore video](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) come parte dell&#39;account sviluppatore Apple.

   * Utilizza Xcode versione 8 o successiva e iOS/tvOS versione 10 o successiva.

* Abilitare il Single Sign-On (SSO) per ogni integrazione e piattaforma desiderata (iOS/tvOS) tramite il [dashboard TVE di Adobe Pass](https://experience.adobe.com/#/pass/authentication) impostando la proprietà `Enable Single Sign On` su `Yes`.

| Adobe - Abilita Single Sign-On | Apple **MVPD onboarded (supportati)** | MVPD di Apple **Selettore** | Apple **Non integrato (non supportato)** MVPDs |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Sì (abilitato) | I flussi di autenticazione e disconnessione coinvolgeranno sia le soluzioni di autenticazione di Apple che di Adobe Pass, mentre tutti gli altri flussi (autorizzazione, preautorizzazione, metadati, ecc.) saranno serviti esclusivamente dall’autenticazione di Adobe Pass. | I flussi di autenticazione e disconnessione torneranno a quelli regolari serviti esclusivamente dall’autenticazione di Adobe Pass. | I flussi di autenticazione e disconnessione torneranno a quelli regolari serviti esclusivamente dall’autenticazione di Adobe Pass. |
| No (disabilitato) | I flussi di autenticazione e disconnessione torneranno a quelli regolari serviti esclusivamente dall’autenticazione di Adobe Pass. | I flussi di autenticazione e disconnessione torneranno a quelli regolari serviti esclusivamente dall’autenticazione di Adobe Pass. | I flussi di autenticazione e disconnessione torneranno a quelli regolari serviti esclusivamente dall’autenticazione di Adobe Pass. |

* Integra i flussi di utenti Single Sign-On (SSO) utilizzando una delle seguenti soluzioni offerte da Adobe Pass Authentication per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS.

   * L’API REST per l’autenticazione di Adobe Pass V2 supporta il Single Sign-On (SSO) dei partner.

     Consulta la documentazione [Apple SSO Cookbook (REST API V2)](apple-sso-cookbook-rest-api-v2.md).

   * L’API REST per l’autenticazione Adobe Pass versione precedente v1 supporta l’SSO (Single Sign-On) dei partner.

     Consulta la documentazione [(Legacy) Apple SSO Cookbook (REST API V1)](../../../../legacy/sso-access/apple-sso-cookbook-rest-api-v1.md).

   * Il SDK legacy Adobe Pass Authentication AccessEnabler iOS/tvOS supporta il Single Sign-On (SSO) dei partner.

     Consulta la documentazione [(Legacy) Apple SSO Cookbook (iOS/tvOS SDK)](../../../../legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md).

### MVPD {#apple-sso-prerequisites-mvpd}

Per poter usufruire dell’esperienza utente Single Sign-On (SSO), un MVPD deve:

* Contatta Apple per avviare il processo di onboarding lato Apple.

   * Richiedi la documentazione tecnica su come integrare e sviluppare un’applicazione TVML di JavaScript in grado di gestire il modulo di accesso utente.

* Per avviare il processo di onboarding sul lato Adobe, contatta l’autenticazione di Adobe Pass.

   * Immetti il valore stringa che rappresenta l’identificatore del fornitore TV assegnato da Apple durante il processo di onboarding.

## Domande frequenti {#FAQ}

* In caso di problemi con il flusso di lavoro SSO di Apple, l’applicazione che utilizza Adobe Pass Authentication AccessEnabler iOS/tvOS SDK può effettuare il regresso al flusso di autenticazione normale?

  Questo è possibile ma richiede una modifica della configurazione eseguita tramite [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) per impostare **Enable Single Sign-On** su **NO** per l&#39;integrazione e la piattaforma desiderate (iOS/tvOS). Tenere presente che l&#39;applicazione client riconoscerà la modifica della configurazione solo dopo aver richiamato l&#39;API [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3).


* L’applicazione saprà quando si è verificata un’autenticazione a seguito di un accesso tramite Apple SSO?

  Queste informazioni sono disponibili come parte della chiave dei metadati utente: *tokenSource*, che in questo caso deve restituire il valore stringa &quot;Apple&quot;.


* L’applicazione saprà quando si è verificata un’autenticazione a seguito di un accesso tramite Apple SSO su un’altra applicazione?

  Informazioni non disponibili.


* Cosa succede se un utente accede alla sezione *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS utilizzando un MVPD non integrato con l&#39;applicazione?

  Quando l’utente avvia l’applicazione, non viene autenticato tramite il flusso di lavoro SSO di Apple. Pertanto, l’applicazione deve tornare al flusso di autenticazione normale e presentare il proprio selettore MVPD.


* Cosa succede se un utente accede a *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS utilizzando un MVPD in cui l&#39;opzione **Abilita Single Sign On** è impostata su **NO** tramite la [dashboard di Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) per la piattaforma iOS/tvOS?

  Quando l’utente avvia l’applicazione, non viene autenticato tramite il flusso di lavoro SSO di Apple. Pertanto, l’applicazione deve tornare al flusso di autenticazione normale e presentare il proprio selettore MVPD.


* Cosa succede se un utente dispone di un MVPD non integrato (non supportato) da Apple, ma presente nel selettore di Apple?

  Quando l’utente avvia l’applicazione, seleziona MVPD solo tramite il flusso di lavoro SSO di Apple senza completare il flusso di autenticazione. Pertanto, l’applicazione dovrebbe tornare al normale flusso di autenticazione, ma potrebbe utilizzare il MVPD già selezionato.


* Cosa succede se un utente dispone di un MVPD non integrato (non supportato) da Apple?

  Quando l&#39;utente avvia l&#39;applicazione, seleziona l&#39;opzione di selezione &quot;Altri provider TV&quot; tramite il flusso di lavoro SSO di Apple. Pertanto, l’applicazione deve tornare al flusso di autenticazione normale e presentare il proprio selettore MVPD.


* Cosa succede se un utente ha un MVPD danneggiato tramite [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)?

  Quando l’utente avvia l’applicazione, viene autenticato tramite il meccanismo di degradazione e non tramite il flusso di lavoro SSO di Apple. L&#39;esperienza deve essere perfetta per l&#39;utente, mentre l&#39;applicazione verrà informata tramite il codice di avviso *N010* nel caso in cui utilizzi Adobe Pass Authentication AccessEnabler iOS/tvOS SDK.


* L’ID utente di MVPD cambierà tra i flussi di autenticazione SSO di Apple e SSO non di Apple?

  Ci si aspetta che l’ID utente non cambi, ma che debba essere verificato per ogni provider selezionato.


* Verranno modificati i TTL di autenticazione?

  L’autenticazione Adobe Pass continuerà a rispettare i TTL richiesti dai programmatori per la loro integrazione con ogni MVPD. Quando si passa da un’applicazione Programmer a un’altra applicazione Programmer tramite Apple SSO, la seconda applicazione avrà il TTL della corrispondente integrazione Programmer x MVPD (non condividerà il TTL della prima applicazione che si autentica)

|                                      | TTL di autenticazione Adobe Pass scaduto | TTL di autenticazione Adobe Pass valido |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **TTL token dispositivo di Apple scaduto** | L&#39;utente NON è autenticato (dovrebbe essere visualizzato il selettore MVPD) | l’utente è autenticato e il TTL è il tempo rimanente del token/profilo di autenticazione di Adobe Pass |
| **TTL token dispositivo Apple valido** | l’utente viene autenticato in modo invisibile all’utente e ottiene un altro token/profilo di autenticazione Adobe Pass con il TTL specificato nel dashboard TVE | l’utente è autenticato e il TTL è il tempo rimanente del token/profilo di autenticazione di Adobe Pass |
