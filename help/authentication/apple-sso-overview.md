---
title: Panoramica di Apple SSO
description: Panoramica di Apple SSO
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1452'
ht-degree: 0%

---

# Panoramica di Apple SSO {#apple-sso-overview}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#Introduction}

Apple fornisce un’API che consente agli utenti di accedere al proprio account di provider TV a livello di sistema del dispositivo, eliminando la necessità di eseguire l’autenticazione app per app.

Apple e l’autenticazione di Adobe Pass hanno quindi collaborato alla creazione dell’esperienza utente Single Sign-On (SSO) nell’ecosistema TV Everywhere per i proprietari di iPhone, iPad e Apple TV.

Per beneficiare dell’esperienza utente Single Sign-On (SSO) su un dispositivo Apple, è necessario compilare un elenco di prerequisiti.

</br>

## Prerequisiti {#Prerequisites}

Il prerequisito può essere applicato a una o più entità coinvolte nel business TVE, ad esempio Programmatori, MVPD, Autenticazione Adobe Pass o Apple.

</br>

### Programmatore {#Programmer}

Per beneficiare dell&#39;esperienza utente Single Sign-On (SSO), un programmatore deve:

1. Utilizza almeno Xcode versione 8 e iOS/tvOS versione 10.

1. Avere [Iscrizione al servizio Single Sign-On per il sottoscrittore video](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) configurati sul rispettivo account Apple Developer. Contatta Apple per abilitare [Framework account sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount) per il tuo Apple Team ID.

1. Abilita Single Sign-On (YES) per ciascuna integrazione desiderata (Channel x MVPD) e piattaforma desiderata (iOS/tvOS) tramite [Dashboard TVE di Adobe Primetime](https://console.auth.adobe.com/).

1. Integra i flussi di lavoro SSO di Apple utilizzando una delle due soluzioni seguenti offerte dal team di autenticazione di Adobe Pass:

   - L’API REST per l’autenticazione di Adobe Pass può supportare l’autenticazione Single Sign-On (SSO) della piattaforma per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS. Vedi anche [Manuale Apple SSO (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md).

   - L’SDK iOS/tvOS di Adobe Pass Authentication AccessEnabler può supportare l’autenticazione Single Sign-On (SSO) della piattaforma per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS. Vedi anche [Manuale Apple SSO (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md).

   - **<u>Suggerimento pro:</u>** Per poter accedere alle informazioni di abbonamento dell’utente, l’utente deve concedere all’applicazione l’autorizzazione per procedere, in modo analogo a fornire l’accesso alla fotocamera o al microfono del dispositivo. Questa autorizzazione deve essere richiesta per applicazione e il dispositivo salverà la selezione dell&#39;utente. È importante ricordare che l&#39;utente può modificare la propria decisione accedendo alle impostazioni dell&#39;applicazione (autorizzazione di accesso del provider TV) o alla sezione da *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS.

   - **<u>Suggerimento pro:</u>** È consigliabile richiedere l&#39;autorizzazione dell&#39;utente quando l&#39;applicazione entra in primo piano, ma si tratta solo di un suggerimento, in quanto l&#39;applicazione può verificare [autorizzazione di accesso](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) le informazioni di abbonamento dell’utente in qualsiasi momento prima di richiedere l’autenticazione dell’utente. Inoltre, le API SDK di AccessEnabler iOS/tvOS richiederanno automaticamente l’autorizzazione dell’utente quando necessario.

   - **<u>Suggerimento pro:</u>** Consigliamo di incentivare gli utenti che rifiutano di concedere l’autorizzazione per accedere alle informazioni sull’abbonamento spiegando i vantaggi dell’esperienza utente Single Sign-On (SSO). È importante ricordare che l&#39;utente può modificare la propria decisione accedendo alle impostazioni dell&#39;applicazione (autorizzazione di accesso del provider TV) o alla sezione da *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS.

Il risultato dovrebbe creare un’esperienza in linea con i seguenti flussi di utenti, che consigliamo di consultare prima di iniziare a sviluppare le applicazioni:

- [iPhone/iPad](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) flussi utente
- [APPLE TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) flussi utente


>[!IMPORTANT]
>
> Quando la funzione Single Sign-On è **abilitato** per iOS/tvOS **e** nel caso di Apple **onboarded (supportato) o selettore** MVPDs i flussi di autenticazione/disconnessione dai flussi di lavoro SSO di Apple coinvolgeranno sia le soluzioni di autenticazione di Apple che quelle di Adobe Pass, mentre tutti gli altri flussi (autorizzazione, preautorizzazione, metadati, ecc.) saranno serviti esclusivamente dall’autenticazione di Adobe Pass.


>[!IMPORTANT]
>
> Quando la funzione Single Sign-On è **disabilitato** per iOS/tvOS **o** nel caso di Apple **non integrato (non supportato)** MVPDs i flussi di autenticazione/logout eseguiranno il fallback dai flussi di lavoro SSO di Apple a quelli regolari serviti esclusivamente dall’autenticazione di Adobe Pass.


>[!IMPORTANT]
>
> Un vantaggio principale del flusso di lavoro SSO di Apple è rappresentato dal flusso di utenti con autenticazione a schermata singola, che può essere distribuito anche sui televisori Apple quando la funzione Single Sign-On è **abilitato** per tvOS **e** nel caso di Apple **onboarded (supportato)** MVPD.


### MVPD {#MVPD}

Per beneficiare dell&#39;esperienza utente Single Sign-On (SSO), un MVPD deve:



1. Puoi essere integrato nel flusso di lavoro SSO di Apple sul lato Apple. Contatta Apple per facilitare il processo di onboarding.
1. Fornisci un’applicazione JavaScript TVML in grado di gestire il modulo di accesso utente. Contatta Apple per ricevere la documentazione corretta.
1. Specifica un valore stringa che rappresenta l’identificatore del provider assegnato da Apple durante il processo di onboarding. Contatta l’autenticazione di Adobe Pass per eseguire le modifiche alla configurazione.

</br>

## Domande frequenti {#FAQ}

1. Nel caso in cui si verifichi un errore con il flusso di lavoro SSO di Apple, l’applicazione che utilizza l’SDK di AccessEnabler iOS/tvOS può effettuare il fallback a un flusso di autenticazione regolare?
   - Questo è possibile, ma richiede una modifica alla configurazione eseguita sul [Dashboard TVE di Adobe Primetime](https://console.auth.adobe.com/). Il *Abilita Single Sign-On* deve essere impostato su *NO* per l’integrazione desiderata (Channel x MVPD) e la piattaforma desiderata (iOS/tvOS).
   - L’applicazione riconosce la modifica della configurazione solo dopo aver chiamato [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) nel caso in cui utilizzi l’SDK AccessEnabler iOS/tvOS.
1. L’applicazione saprà quando si è verificata un’autenticazione a seguito di un accesso tramite l’SSO della piattaforma su un altro dispositivo o un’altra applicazione?
   - Queste informazioni non saranno disponibili.
1. L’applicazione saprà quando si è verificata un’autenticazione a seguito di un accesso tramite l’SSO della piattaforma sullo stesso dispositivo?
   - Queste informazioni sono disponibili come parte della chiave dei metadati utente: *tokenSource*, che in questo caso deve restituire il valore stringa &quot;Apple&quot;.
1. Cosa succede se un utente accede al *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* nella sezione tvOS utilizzando un MVPD non integrato con l’applicazione?
   - Quando l’utente avvia l’applicazione, non viene autenticato tramite il flusso di lavoro SSO di Apple. Pertanto, l’applicazione deve eseguire il fallback al flusso di autenticazione regolare e presentare il proprio selettore MVPD.
1. Cosa succede se un utente accede al *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* nella sezione tvOS utilizzando un MVPD con il *Abilita Single Sign-On* impostato su *NO* il [Dashboard TVE di Adobe Primetime](https://console.auth.adobe.com/) per la piattaforma iOS/tvOS?
   - Quando l’utente avvia l’applicazione, non viene autenticato tramite il flusso di lavoro SSO di Apple. Pertanto, l’applicazione deve eseguire il fallback al flusso di autenticazione regolare e presentare il proprio selettore MVPD.
1. Cosa succede se un utente dispone di un MVPD che non è integrato (non supportato) da Apple, ma è presente nel selettore di Apple?
   - Quando l’utente avvia l’applicazione, seleziona l’MVPD solo tramite il flusso di lavoro SSO di Apple senza completare il flusso di autenticazione. Pertanto, l’applicazione dovrebbe effettuare il fallback al flusso di autenticazione regolare, ma potrebbe utilizzare l’MVPD già selezionato.
1. Cosa succede se un utente dispone di un MVPD che non è integrato (non supportato) da Apple?
   - Quando l&#39;utente avvia l&#39;applicazione, seleziona l&#39;opzione di selezione &quot;Altri provider TV&quot; tramite il flusso di lavoro SSO di Apple. Pertanto, l’applicazione deve eseguire il fallback al flusso di autenticazione regolare e presentare il proprio selettore MVPD.
1. Cosa succede se un utente ha un MVPD che viene degradato attraverso il mezzo di [Dashboard TVE di Adobe Primetime](https://console.auth.adobe.com/)?
   - Quando l’utente avvia l’applicazione, viene autenticato tramite il meccanismo di degradazione e non tramite il flusso di lavoro SSO di Apple.
   - L’esperienza deve essere diretta all’utente, mentre l’applicazione verrà informata tramite *N010* codice di avviso nel caso in cui utilizzi l’SDK AccessEnabler iOS/tvOS.
1. L’ID utente MVPD cambierà tra il flusso di autenticazione SSO Apple e SSO non Apple?
   - Ci si aspetta che l’ID utente non cambi, ma che debba essere verificato per ogni provider selezionato.
1. Verranno modificati i TTL di autenticazione?
   - L’autenticazione Adobe Pass continuerà a rispettare i TTL richiesti dai programmatori per la loro integrazione con ogni MVPD.
   - Quando si passa da un’applicazione Programmer a un’altra applicazione Programmer tramite Apple SSO, la seconda applicazione avrà il TTL della corrispondente integrazione Programmer x MVPD (non condividerà il TTL della prima applicazione che si autentica)

|                                      | TTL di autenticazione Adobe Pass scaduto | TTL di autenticazione Adobe Pass valido |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **TTL del token dispositivo di Apple scaduto** | L&#39;utente NON è autenticato (dovrebbe apparire il selettore MVPD) | L’utente è autenticato e il TTL è il tempo rimanente del token di autenticazione di Adobe Pass |
| **TTL token dispositivo di Apple valido** | l’utente viene autenticato in modo invisibile all’utente e ottiene un altro token di autenticazione Adobe Pass con il TTL specificato nel dashboard TVE | L’utente è autenticato e il TTL è il tempo rimanente del token di autenticazione di Adobe Pass |

<!--

## Resources {#Resources}

- [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)
- [Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)
- [Sign in with your TV provider on your iPhone, iPad, or iPod touch](https://support.apple.com/en-us/HT207035)
- [Use your pay TV or cable provider with Apple TV](https://support.apple.com/en-us/HT207035)
- [TV providers that let you sign in on your iPhone, iPad, or Apple TV](https://support.apple.com/en-us/HT208084)
- [TV Provider Authentication](https://developer.apple.com/design/human-interface-guidelines/tvos/system-capabilities/tv-provider-authentication/)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
