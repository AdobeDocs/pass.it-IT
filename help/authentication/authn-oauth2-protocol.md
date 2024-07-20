---
title: Autenticazione tramite il protocollo OAuth 2.0
description: Autenticazione tramite il protocollo OAuth 2.0
exl-id: 0c1f04fe-51dc-4b4d-88e7-66e8f4609e02
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1088'
ht-degree: 0%

---

# Autenticazione tramite il protocollo OAuth 2.0

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

Anche se SAML è ancora il principale protocollo utilizzato per l&#39;autenticazione da parte degli MVPD statunitensi e delle aziende in generale, c&#39;è una chiara tendenza verso il passaggio a OAuth 2.0 come protocollo di autenticazione principale. Il protocollo OAuth 2.0 (https://tools.ietf.org/html/rfc6749) è stato sviluppato principalmente per i siti di consumo ed è stato rapidamente adottato da giganti di Internet come Facebook, Google &amp; Twitter.

OAuth 2.0 ha riscosso un enorme successo e questo ha spinto le aziende ad aggiornare lentamente la propria infrastruttura per supportarla.



## Vantaggi del passaggio a OAuth 2.0 {#adv-oauth2}

Ad alto livello, il protocollo OAuth 2.0 offre le stesse funzionalità del protocollo SAML, ma ci sono alcune importanti distinzioni.

Uno di questi è il fatto che il flusso del token di aggiornamento può essere utilizzato come modo per aggiornare l’autenticazione dietro le quinte. Questo consente all&#39;IdP (gli MVPD in questo caso) di mantenere il controllo pur consentendo una buona esperienza utente, in quanto l&#39;utente non è più tenuto ad accedere spesso a causa di problemi di sicurezza.

Il protocollo offre anche maggiore flessibilità in termini di dati esposti come provider di servizi ora può utilizzare i token per accedere ad altre API per ottenere informazioni aggiuntive. Questo a sua volta si traduce in un protocollo &quot;chattier&quot; per i casi d’uso di TVE, ma consente la flessibilità necessaria per flussi di lavoro complessi.





## Requisiti per il passaggio a OAuth 2.0 {#oauth-req}

Per supportare l’autenticazione con OAuth 2.0, un MVPD deve soddisfare i seguenti prerequisiti:

Innanzitutto, MVPD deve assicurarsi di supportare il flusso [Concessione codice di autorizzazione](https://oauthlib.readthedocs.io/en/latest/oauth2/grants/authcode.html).

Dopo aver confermato che supporta il flusso, il MVPD deve fornirci le seguenti informazioni:

* end-point di autenticazione
   * l’endpoint fornirà il codice di autorizzazione che verrà successivamente utilizzato in cambio del token di aggiornamento e accesso
* end-point /token
   * questo fornirà il token di aggiornamento e il token di accesso
   * il token di aggiornamento deve essere stabile (non deve cambiare ogni volta che si richiede un nuovo token di accesso)
   * MVPD deve consentire diversi token di accesso attivi per ogni token di aggiornamento
   * questo endpoint scambierà anche un token di aggiornamento con un token di accesso
* è necessario un **endpoint per user-profile**
   * questo endpoint fornirà l’identificatore userID, che deve essere univoco per un account e non deve contenere alcuna informazione di identificazione personale
* l&#39;endpoint **/logout** (facoltativo)
   * L’autenticazione Adobe Pass reindirizzerà a questo endpoint, fornirà all’MVPD un URI di reindirizzamento verso il retro; su questo endpoint, l’MVPD può cancellare i cookie sul computer client o applicare qualsiasi logica desiderata per la disconnessione
* si consiglia vivamente di avere supporto per i client autorizzati (app client che non attivano una pagina di autorizzazione utente)
* avremo anche bisogno di:
   * **clientID** e **client secret** per le configurazioni di integrazione
   * **durata** (TTL) valori per token di aggiornamento e token di accesso
   * Possiamo fornire all&#39;MVPD un callback di autorizzazione e un URI di callback di logout. Inoltre, se necessario, possiamo fornire agli MVPD un elenco di IP da inserire nella whitelist delle impostazioni del firewall.


## Flusso di autenticazione {#authn-flow}

Nel flusso di autenticazione, l’autenticazione di Adobe Pass comunicherà con MVPD sul protocollo selezionato nella configurazione. Il flusso OAuth 2.0 è illustrato nell’immagine seguente:



![Diagramma che mostra il flusso di autenticazione in Adobe Authentication che comunica con MVPD sul protocollo selezionato nella configurazione.](assets/authn-flow.png)

**Figura 1: flusso di autenticazione OAuth 2.0**



## Richiesta e risposta di autenticazione {#authn-req-response}

In breve, il flusso di autenticazione per gli MVPD che supportano il protocollo OAuth 2.0 segue questi passaggi:

1. L’utente finale passa al sito del programmatore e seleziona di accedere con le sue credenziali MVPD
1. AccessEnabler installato sul lato del programmatore con l&#39;invio di una richiesta di autenticazione sotto forma di richiesta HTTP all&#39;endpoint di autenticazione Adobe Pass, che l&#39;endpoint di autenticazione Adobe Pass reindirizzerà all&#39;endpoint di autorizzazione MVPD.
1. L’endpoint di autorizzazione MVPD invia un codice di autorizzazione all’endpoint di autenticazione Adobe Pass
1. L’autenticazione di Adobe Pass utilizza il codice di autorizzazione ricevuto per richiedere un token di aggiornamento e un token di accesso dall’endpoint del token MVPD
1. Una chiamata per recuperare informazioni utente e metadati può essere inviata all’endpoint del profilo utente nel caso in cui le informazioni utente non siano incluse nel token
1. Il token di autenticazione viene passato all&#39;utente finale che ora può esplorare correttamente il sito Programmatore

   >[!NOTE]
   >
   >Il token di aggiornamento viene utilizzato per ottenere un nuovo token di accesso dopo che il token di accesso corrente non è più valido o è scaduto.


>[!IMPORTANT]
>
>Il token di aggiornamento non deve cambiare quando viene scambiato con un token di accesso.

Questa limitazione deriva dai flussi client che non consentono al server di aggiornare AuthNToken che, per il protocollo OAuth 2.0, contiene anche il token di aggiornamento.

Un flusso di autorizzazione tipico esegue uno scambio del token di aggiornamento salvato in AuthNToken con un token di accesso che viene successivamente utilizzato per eseguire la chiamata di autorizzazione a nome dell&#39;utente autenticato in primo luogo. Se il server di autorizzazione (MVPD) dovesse modificare il token di aggiornamento e invalidare quello precedente, non sarà possibile aggiornare AuthNToken valido. Per questo motivo, gli MVPD devono supportare token di aggiornamento stabili per poter configurare le integrazioni OAuth 2.0 per essi.


## Migrazione da SAML a OAuth 2.0 {#saml-auth2-migr}

La migrazione delle integrazioni da SAML a OAuth 2.0 verrà eseguita da Adobe e MVPD. Non sono necessarie modifiche tecniche sul lato del programmatore, anche se il programmatore potrebbe voler controllare/testare il co-branding nella pagina di accesso di MVPD. Dal punto di vista dell’MVPD, sono necessari gli endpoint e le altre informazioni richieste nei requisiti di Oauth 2.0.

Per **mantenere SSO**, gli utenti che dispongono già di un token di autenticazione ottenuto tramite SAML verranno comunque considerati autenticati e le loro richieste verranno instradate attraverso la vecchia integrazione SAML.

Da un punto di vista tecnico:

1. Adobe abiliterà un’integrazione OAuth 2.0 tra il programmatore e l’MVPD, SENZA eliminare l’integrazione SAML.
1. Dopo l’abilitazione, tutti i nuovi utenti utilizzeranno i flussi OAuth 2.0.
1. Gli utenti già autenticati, che dispongono già di un token di autenticazione locale contenente l’ID soggetto SAML, verranno instradati automaticamente da Adobe tramite l’integrazione SAML.
1. Per gli utenti nel passaggio numero 3, una volta scaduto il token di autenticazione SAML generato, Adobe li tratterà come nuovi utenti e si comporterà come gli utenti nel passaggio numero 2.
1. Adobe esaminerà i modelli di utilizzo per determinare il momento in cui l’integrazione SAML può essere disattivata in modo sicuro.
