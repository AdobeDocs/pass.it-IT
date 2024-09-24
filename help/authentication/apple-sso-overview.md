---
title: Panoramica di Apple SSO
description: Panoramica di Apple SSO
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 7107d4a915113fb237602143aafc350b776c55d6
workflow-type: tm+mt
source-wordcount: '1417'
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

1. Devi configurare l&#39;autorizzazione Single Sign-On ](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) del Sottoscrittore video[sul suo account sviluppatore Apple. Contatta Apple per abilitare il framework dell&#39;account dell&#39;utente con sottoscrizione video [1} per il tuo ID team Apple.](https://developer.apple.com/documentation/videosubscriberaccount)

1. Abilita Single Sign-On (YES) per ogni integrazione desiderata (Channel x MVPD) e piattaforma desiderata (iOS/tvOS) tramite [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication).

1. Integra i flussi di lavoro SSO di Apple utilizzando una delle due soluzioni seguenti offerte dal team di autenticazione di Adobe Pass:

   - L’API REST per l’autenticazione di Adobe Pass può supportare l’autenticazione Single Sign-On (SSO) della piattaforma per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS. Consulta anche [Manuale Apple SSO (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md).

   - L’SDK iOS/tvOS di Adobe Pass Authentication AccessEnabler può supportare l’autenticazione Single Sign-On (SSO) della piattaforma per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS. Consulta anche [Manuale Apple SSO (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md).

   - **<u>Suggerimento pro:</u>** Per poter accedere alle informazioni di abbonamento dell&#39;utente, l&#39;utente deve concedere all&#39;applicazione l&#39;autorizzazione per continuare, in modo analogo a fornire l&#39;accesso alla fotocamera o al microfono del dispositivo. Questa autorizzazione deve essere richiesta per applicazione e il dispositivo salverà la selezione dell&#39;utente. È importante ricordare che l&#39;utente può modificare la propria decisione accedendo alle impostazioni dell&#39;applicazione (autorizzazione di accesso al provider TV) o alla sezione da *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS.

   - **<u>Suggerimento pro:</u>** si consiglia di richiedere l&#39;autorizzazione dell&#39;utente quando l&#39;applicazione entra in primo piano, ma si tratta solo di un suggerimento, perché l&#39;applicazione può controllare [le autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente in qualsiasi momento prima di richiedere l&#39;autenticazione dell&#39;utente. Inoltre, le API SDK di AccessEnabler iOS/tvOS richiederanno automaticamente l’autorizzazione dell’utente quando necessario.

   - **<u>Suggerimento pro:</u>** è consigliabile incentivare gli utenti che rifiutano di concedere l&#39;autorizzazione per accedere alle informazioni sull&#39;abbonamento illustrando i vantaggi dell&#39;esperienza utente Single Sign-On (SSO). È importante ricordare che l&#39;utente può modificare la propria decisione accedendo alle impostazioni dell&#39;applicazione (autorizzazione di accesso al provider TV) o alla sezione da *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS.

Il risultato dovrebbe creare un’esperienza in linea con i seguenti flussi di utenti, che consigliamo di consultare prima di iniziare a sviluppare le applicazioni:

- [Flussi di utenti iPhone / iPad](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf)
- [Flussi di utenti di Apple TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf)


>[!IMPORTANT]
>
> Quando la funzionalità Single Sign-On è **enabled** per iOS/tvOS **e** nel caso di MVPD con configurazione di Apple **onboarded (supportati) o selettore**, i flussi di autenticazione/disconnessione dai flussi di lavoro SSO di Apple coinvolgeranno sia le soluzioni di autenticazione di Apple che di Adobe Pass, mentre tutti gli altri flussi (autorizzazione, preautorizzazione, metadati, ecc.) saranno serviti esclusivamente dall’autenticazione di Adobe Pass.


>[!IMPORTANT]
>
> Quando la funzione Single Sign-On è **disabilitata** per iOS/tvOS **o** nel caso di Apple **non onboarded (non supportato)** MVPD, i flussi di autenticazione/disconnessione verranno riportati dai flussi di lavoro SSO di Apple a quelli regolari serviti esclusivamente dall&#39;autenticazione di Adobe Pass.


>[!IMPORTANT]
>
> Un vantaggio principale del flusso di lavoro SSO di Apple è rappresentato dal flusso di utenti con autenticazione a schermo singolo, che può essere consegnato anche su Apple TV quando la funzione Single Sign-On è **enabled** per tvOS **e** nel caso di Apple **MVPD onboarded (supportati)**.


### MVPD {#MVPD}

Per beneficiare dell&#39;esperienza utente Single Sign-On (SSO), è necessario
MVPD deve:



1. Puoi essere integrato nel flusso di lavoro SSO di Apple sul lato Apple. Contatta Apple per facilitare il processo di onboarding.
1. Fornire un&#39;applicazione TVML JavaScript in grado di gestire il modulo di accesso utente. Contatta Apple per ricevere la documentazione corretta.
1. Specifica un valore stringa che rappresenta l’identificatore del provider assegnato da Apple durante il processo di onboarding. Contatta l’autenticazione di Adobe Pass per eseguire le modifiche alla configurazione.

</br>

## Domande frequenti {#FAQ}

1. Nel caso in cui si verifichi un errore con il flusso di lavoro SSO di Apple, l’applicazione che utilizza l’SDK di AccessEnabler iOS/tvOS può effettuare il fallback a un flusso di autenticazione regolare?
   - Questo è possibile ma richiede una modifica alla configurazione in esecuzione sul [dashboard TVE di Adobe Pass](https://experience.adobe.com/#/pass/authentication). *Abilita Single Sign-On* deve essere impostato su *NO* per l&#39;integrazione desiderata (Channel x MVPD) e la piattaforma desiderata (iOS/tvOS).
   - L&#39;applicazione riconoscerà la modifica della configurazione solo dopo aver chiamato l&#39;API [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) nel caso in cui utilizzi l&#39;SDK AccessEnabler iOS/tvOS.
1. L’applicazione saprà quando si è verificata un’autenticazione a seguito di un accesso tramite l’SSO della piattaforma su un altro dispositivo o un’altra applicazione?
   - Queste informazioni non saranno disponibili.
1. L’applicazione saprà quando si è verificata un’autenticazione a seguito di un accesso tramite l’SSO della piattaforma sullo stesso dispositivo?
   - Queste informazioni sono disponibili come parte della chiave dei metadati utente: *tokenSource*, che in questo caso deve restituire il valore stringa &quot;Apple&quot;.
1. Cosa succede se un utente accede a *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS utilizzando un MVPD non integrato con l&#39;applicazione?
   - Quando l’utente avvia l’applicazione, non viene autenticato tramite il flusso di lavoro SSO di Apple. Pertanto, l’applicazione deve eseguire il fallback al flusso di autenticazione regolare e presentare il proprio selettore MVPD.
1. Cosa succede se un utente accede a *`Settings -> TV Provider`* su iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* su tvOS utilizzando un MVPD con *Abilita Single Sign-On* impostato su *NO* sulla [Dashboard TVE di Adobe Pass](https://experience.adobe.com/#/pass/authentication) per la piattaforma iOS/tvOS?
   - Quando l’utente avvia l’applicazione, non viene autenticato tramite il flusso di lavoro SSO di Apple. Pertanto, l’applicazione deve eseguire il fallback al flusso di autenticazione regolare e presentare il proprio selettore MVPD.
1. Cosa succede se un utente dispone di un MVPD che non è integrato (non supportato) da Apple, ma è presente nel selettore di Apple?
   - Quando l’utente avvia l’applicazione, seleziona l’MVPD solo tramite il flusso di lavoro SSO di Apple senza completare il flusso di autenticazione. Pertanto, l’applicazione dovrebbe effettuare il fallback al flusso di autenticazione regolare, ma potrebbe utilizzare l’MVPD già selezionato.
1. Cosa succede se un utente dispone di un MVPD che non è integrato (non supportato) da Apple?
   - Quando l&#39;utente avvia l&#39;applicazione, seleziona l&#39;opzione di selezione &quot;Altri provider TV&quot; tramite il flusso di lavoro SSO di Apple. Pertanto, l’applicazione deve eseguire il fallback al flusso di autenticazione regolare e presentare il proprio selettore MVPD.
1. Cosa succede se un utente ha un MVPD degradato tramite [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)?
   - Quando l’utente avvia l’applicazione, viene autenticato tramite il meccanismo di degradazione e non tramite il flusso di lavoro SSO di Apple.
   - L&#39;esperienza deve essere perfetta per l&#39;utente, mentre l&#39;applicazione verrà informata tramite il codice di avviso *N010* nel caso in cui utilizzi l&#39;SDK AccessEnabler iOS/tvOS.
1. L’ID utente MVPD cambierà tra il flusso di autenticazione SSO Apple e SSO non Apple?
   - Ci si aspetta che l’ID utente non cambi, ma che debba essere verificato per ogni provider selezionato.
1. Verranno modificati i TTL di autenticazione?
   - L’autenticazione Adobe Pass continuerà a rispettare i TTL richiesti dai programmatori per la loro integrazione con ogni MVPD.
   - Quando si passa da un’applicazione Programmer a un’altra applicazione Programmer tramite Apple SSO, la seconda applicazione avrà il TTL della corrispondente integrazione Programmer x MVPD (non condividerà il TTL della prima applicazione che si autentica)

|                                      | TTL di autenticazione Adobe Pass scaduto | TTL di autenticazione Adobe Pass valido |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **TTL token dispositivo di Apple scaduto** | L&#39;utente NON è autenticato (dovrebbe apparire il selettore MVPD) | L’utente è autenticato e il TTL è il tempo rimanente del token di autenticazione di Adobe Pass |
| **TTL token dispositivo Apple valido** | l’utente viene autenticato in modo invisibile all’utente e ottiene un altro token di autenticazione Adobe Pass con il TTL specificato nel dashboard TVE | L’utente è autenticato e il TTL è il tempo rimanente del token di autenticazione di Adobe Pass |

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
