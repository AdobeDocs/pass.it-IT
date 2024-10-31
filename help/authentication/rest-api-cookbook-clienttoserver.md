---
title: Manuale dell’API REST (da client a server)
description: Client-to-server del manuale API REST.
exl-id: f54a1eda-47d5-4f02-b343-8cdbc99a73c0
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 0%

---

# Manuale dell’API REST (da client a server) {#rest-api-cookbook-client-to-server}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.


## Panoramica {#overview}

Questo documento fornisce istruzioni dettagliate per il team tecnico di un programmatore per integrare uno &quot;smart device&quot; (console giochi, app per smart TV, set top box, ecc.) con l’autenticazione di Adobe Pass tramite i servizi API REST. Questo approccio client-to-server, che utilizza le API REST anziché un SDK client, consente un supporto più ampio di piattaforme diverse per le quali non sarebbe possibile sviluppare un numero significativo di SDK univoci. Per un&#39;ampia panoramica tecnica sul funzionamento della soluzione Clientless, vedere [Panoramica tecnica Clientless](/help/authentication/rest-api-overview.md).


Questo approccio richiede due componenti (streaming app e AuthN app) per completare i flussi richiesti: avvio, registrazione, autorizzazione e flussi view-media nell’app di streaming e il flusso di autenticazione nell’app AuthN.

### Meccanismo di limitazione

L&#39;API REST per l&#39;autenticazione di Adobe Pass è gestita da un [meccanismo di limitazione](/help/authentication/throttling-mechanism.md).

## Componenti {#components}

In una soluzione client-to-server funzionante sono coinvolti i seguenti componenti:



| Tipo | Componente | Descrizione |
| --- | --- | --- |
| Dispositivo di streaming | App di streaming | L&#39;applicazione Programmer che risiede sul dispositivo di streaming dell&#39;utente e riproduce video autenticati. |
| | \[Facoltativo\] Modulo AuthN | Se il dispositivo di streaming dispone di un agente utente (ad esempio, un browser web), il modulo AuthN è responsabile dell’autenticazione dell’utente sull’IdP MVPD. |
| \[Facoltativo\] Dispositivo AuthN | App autenticazione | Se il dispositivo di streaming non dispone di un agente utente (ad esempio, un browser web), l’applicazione AuthN è un’applicazione web Programmer a cui si accede dal dispositivo di un utente separato utilizzando un browser web. |
| Infrastruttura Adobe | Servizio Adobe Pass | Servizio che si integra con MVPD IdP e AuthZ Service e fornisce decisioni di autenticazione e autorizzazione. |
| Infrastruttura MVPD | IdP MVPD | Endpoint MVPD che fornisce un servizio di autenticazione basato su credenziali per convalidare l&#39;identità dell&#39;utente. |
| | Servizio AuthZ MVPD | Endpoint MVPD che fornisce decisioni di autorizzazione basate su abbonamenti dell&#39;utente, controllo genitori, ecc. |



Ulteriori termini utilizzati nel flusso sono definiti nel [Glossario](/help/authentication/glossary.md).

## Flussi{#flows}

### Registrazione Dynamic Client (DCR)

Adobe Pass utilizza il DCR per proteggere le comunicazioni client tra un’applicazione o un server di programmazione e i servizi Adobe Pass. Il flusso DCR è separato ed è descritto nella documentazione [Panoramica registrazione client dinamica](./dcr-api/dynamic-client-registration-overview.md).


### Flussi di app in streaming (per dispositivi avanzati)

![](assets/smart-device-app-flow.png)

#### Flusso di avvio

1. L’app viene avviata e ne viene caricata l’interfaccia utente iniziale.

2. Ottieni/genera un ID dispositivo.

3. Effettua una chiamata di autenticazione tramite assegno per verificare se il dispositivo è già autenticato.  Esempio: [`<SP_FQDN>/api/v1/checkauthn [device ID]`](/help/authentication/check-authentication-token.md)

4. Se la chiamata `checkauthn` ha esito positivo, passare al flusso di autorizzazione dal passaggio 2 in poi.  In caso di errore, avvia il flusso di registrazione.



#### Flusso di registrazione

1. Ottieni un codice di registrazione e un URL da utilizzare per accedere all’app di accesso alla seconda schermata e presentali all’utente:

   a. Invia una richiesta POST al servizio Codice di registrazione di Adobe, trasmettendo un ID dispositivo con hash e un &quot;URL di registrazione&quot;.  Esempio: [`<REGGIE_FQDN>/reggie/v1/[requestorId]/regcode [device ID]`](/help/authentication/registration-code-request.md)

   b. Presenta all’utente il codice di registrazione e l’URL restituiti.

   c. Chiedere all&#39;utente di passare a un dispositivo compatibile con il Web, passare all&#39;URL e quindi immettere il codice di registrazione.



#### Flusso di autorizzazione

1. L’utente ritorna dalla seconda schermata dell’app e preme il pulsante &quot;Continua&quot; sul dispositivo. In alternativa, è possibile implementare un meccanismo di polling per controllare lo stato di autenticazione, ma l’autenticazione Adobe Pass consiglia il metodo del pulsante Continua sopra il polling. <!--(For information on employing a "Continue" button versus polling the Adobe Pass Authentication backend server, see the Clientless Technical Overview: Managing 2nd-Screen Workflow Transition.)--> Ad esempio: [\&lt;SP\_FQDN\>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md)

2. Invia una richiesta di GET al servizio di autorizzazione dell’autenticazione di Adobe Pass per avviare l’autorizzazione. Esempio: `<SP_FQDN>/api/v1/authorize [device ID, Requestor ID, Resource ID]`

<!-- end list -->

* Se la risposta indica che l’operazione è riuscita: l’utente dispone di un token AuthN valido E dell’autorizzazione a guardare il file multimediale richiesto (per questo utente è disponibile un token AuthZ valido).

* Se la risposta indica un errore: esamina l’eccezione generata per determinarne il tipo (AuthN, AuthZ o altro):

   * Se si è verificato un errore di autenticazione, riavviare il flusso di registrazione.

   * Se si è verificato un errore AuthZ, l’utente non è autorizzato a guardare il contenuto multimediale richiesto e deve visualizzare all’utente un qualche tipo di messaggio di errore.

   * In caso di altri errori (errore di connessione, errore di rete, ecc.) quindi visualizza un messaggio di errore appropriato.



#### Visualizza flusso multimediale

1. Presentare le scelte multimediali. L’utente seleziona il file multimediale da visualizzare.

2. Il supporto è protetto?

   a. L’app controlla se il supporto è protetto.

   b. Se il supporto è protetto, l’app avvia l’autorizzazione
(AuthZ) Flusso sopra.

   c. Se il supporto non è protetto, riprodurlo per
utente.

3. Riprodurre il contenuto multimediale.


### Flusso app AuthN (seconda schermata)

![](assets/secnd-screen-authn-flow.png)

1. Ottieni un elenco di MVPD per questo utente. Esempio: [`<SP_FQDN>/api/v1/config/[requestorID]`](/help/authentication/provide-mvpd-list.md)

1. Avvia il flusso di autenticazione.  Esempio: [`<SP_FQDN>/api/v1/authenticate [requestorID, MVPD ID, Redirect URL, Domain name, Registration Code, "noflash=true"]`](/help/authentication/initiate-authentication.md)

1. Verifica se l’autenticazione è riuscita. Esempio:[`<SP_FQDN>/api/v1/checkauthn/[registration code][requestor ID]`](/help/authentication/check-authentication-token.md)

1. Invia nuovamente l&#39;utente all&#39;app Smart Device per completare il flusso di autorizzazione.

## Single Sign-On partner {#partner-sso}

Alcuni dispositivi forniscono supporto dedicato per il Single Sign-On (SSO) dei partner:

* [SSO APPLE](/help/authentication/single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-cookbook-rest-api-v1.md)

## Single Sign-On della piattaforma {#platform-sso}

Alcuni dispositivi forniscono supporto dedicato per l’SSO (Single Sign-On) della piattaforma:

* [SSO AMAZON](./single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v1.md)
* [SSO Roku](./single-sign-on/platform-single-sign-on/roku-single-sign-on/roku-sso-overview.md)

## TempPass e Promotional TempPass per API REST {#temppass}

Per le implementazioni TempPass e Promotional TempPass in cui l’utente non è tenuto a immettere credenziali, l’autenticazione può essere implementata direttamente nell’app di streaming.

**Per utilizzare questa API, l&#39;app di streaming deve verificare l&#39;univocità dell&#39;ID dispositivo, in quanto viene utilizzata per identificare il token, insieme ai dati aggiuntivi facoltativi.**


![](assets/temp-pass-promo-temppass.png)
