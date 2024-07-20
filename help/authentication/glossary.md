---
title: Glossario
description: Glossario
exl-id: e64a94f6-7460-4aa8-8d6b-e0553ba1e4ec
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Glossario {#glossary}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## AccessEnabler {#accessEnabler}

Il componente client dell’autenticazione di Adobe Pass. Adobe Pass Authentication fornisce una libreria AccessEnabler per ogni piattaforma supportata.

## AuthN {#authn}

Utilizzato come abbreviazione per &quot;authentication&quot; (autenticazione), come in &quot;AuthN Token&quot; (Token di autenticazione) o &quot;AuthN Flow&quot; (Flusso di autenticazione).


## Token AuthN{#authn-token}

Token di autenticazione, generato dall’autenticazione di Adobe Pass dopo che un utente è stato autenticato correttamente con un MVPD. A seconda della piattaforma di integrazione del programmatore, il token viene memorizzato sul dispositivo dell’utente o sui server di autenticazione di Adobe Pass.

## AuthZ {#authz}

Utilizzato come abbreviazione per &quot;autorizzazione&quot;, come in &quot;Token AuthZ&quot; o &quot;Flusso AuthZ&quot;.

## Token AuthZ {#authz-token}

Token di autorizzazione, generato dall’autenticazione di Adobe Pass dopo che un utente è stato autorizzato a visualizzare il contenuto protetto. Il token AuthZ è archiviato nei server di autenticazione Adobe Pass e viene utilizzato per generare un [token multimediale di breve durata](#short-lived-token).

## ID canale (obsoleto) {#channel_id}

Termine precedente per ID risorsa.

## API senza client {#clientless-api}

Soluzione di integrazione Adobe Pass Authentication che utilizza servizi Web anziché il componente client AccessEnabler.

## ID dispositivo {#device-id}

Identifica in modo univoco un dispositivo (ad esempio un telefono, un tablet, ecc.) nell’autenticazione di Adobe Pass. Questo ID viene ottenuto/fornito dall’applicazione del programmatore.


## Flusso diritto{#entitlement_flow}

Il termine utilizzato nella documentazione di autenticazione di Adobe Pass fa riferimento all’intero processo di registrazione di un dispositivo/utente con autenticazione di Adobe Pass, autenticazione di un utente con un MVPD, autorizzazione di una risorsa per un utente e disconnessione di un utente.


## GUID {#guid}

Vedi [ID utente](#user-id).

## IdP {#idp}

Identifica il provider; sinonimo di MVPD nel contesto del ruolo di un MVPD in un’integrazione di autenticazione Adobe Pass. (I clienti devono verificare la propria identificazione tramite la pagina di accesso del provider di Pay TV).

## Media Token Verifier {#media-token-verifier}

Libreria fornita da Adobe e utilizzata dai programmatori per verificare il token multimediale di breve durata generato dall’autenticazione di Adobe Pass dopo il completamento di un flusso di adesione.

## MVPD {#mvpd}

Distributore di programmi video multicanale, sinonimo di &quot;provider di servizi di televisione a pagamento&quot;.

## ID MVPD {#mvpd-id}

Vedi [ID utente](#user-id).

## ID partner {#partner-id}

Un identificatore che Adobe passa agli MVPD, che lo utilizzano per identificare per conto di chi l’autenticazione di Adobe Pass richiede. A volte viene utilizzato per configurare le loro UI per programmatori particolari, a volte è lo stesso in tutti i programmatori, dipende dalle esigenze del MVPD.

## Provider di servizi di televisione a pagamento {#pay-tv-provider}

Sinonimo di [MVPD](#mvpd).

## Programmatore {#programmer}

Sinonimo di &quot;provider di contenuti&quot;, &quot;account&quot;, &quot;canale&quot;, &quot;provider di servizi&quot;, &quot;brand&quot; e così via.

## MVPD proxy {#proxy-mvpd}

MVPD che fornisce servizi di identità per altri MVPD; integrato direttamente con Adobe Pass Authentication.

## MVPD proxy {#proxied-mvpd}

MVPD che non ha un&#39;integrazione diretta con l&#39;SP Adobe, ma è integrato tramite un MVPD proxy.

## ID richiedente {#requestor-id}

Identifica in modo univoco un [programmatore](#programmer) (un account, un marchio, un canale o una proprietà) nell&#39;autenticazione di Adobe Pass. Questo ID viene determinato tra il Programmatore e l’Adobe durante la configurazione iniziale dell’account. Sul web, l’ID richiedente è associato a un set di domini inseriti nella whitelist; tutte le chiamate che utilizzano un ID proveniente da un dominio esterno verranno rifiutate. I programmatori utilizzano anche l’ID richiedente per l’analisi. In genere esiste un solo ID richiedente per programmatore. Una funzione aggiuntiva relativa all’ID richiedente è che il programmatore deve fornire all’Adobe un certificato pubblico, poiché la chiamata API setRequestor prevede l’invio di dati crittografati, utilizzati per autenticare il programmatore nel sistema di autenticazione di Adobe Pass.

## ID risorsa {#resource-id}

Una stringa o una risorsa mRSS che identifica un [Programmatore](#programmer) in MVPDs. È concordato tra il Programmatore e gli MVPD; l&#39;Autenticazione Adobe Pass trasmette l&#39;ID risorsa attraverso intatto, quindi deve essere lo stesso per tutti gli MVPD. Un programmatore può utilizzare più ID di risorse purché gli MVPD siano consapevoli di ciò che ogni ID rappresenta.

## GUID sessione {#sessionGUID}

Vedi [ID utente](#user-id).

## Token multimediale di breve durata {#short-lived-token}

Questo token viene generato tramite l’autenticazione di Adobe Pass al completamento del processo di adesione per un determinato utente. Il token viene passato al programmatore, che utilizza Adobe Pass Authentication Token Verifier sul token multimediale di breve durata per verificare la sicurezza del processo di adesione.

## Smart Device {#smart-device}

Termine utilizzato nella documentazione relativa all’autenticazione di Adobe Pass per fare riferimento a set-top box, console giochi e smart TV. Si tratta di dispositivi che dispongono di funzionalità di rete ma non sono in grado di eseguire il rendering di pagine web.

## SP{#sp}

Provider di servizi; in genere si riferisce al *ruolo* dell&#39;SP, svolto dall&#39;autenticazione Adobe Pass, che agisce per conto di un programmatore in un&#39;integrazione con un [MVPD](#mvpd).

## Passaggio temporaneo {#temp-pass}

Funzione che consente ai programmatori di fornire accesso gratuito temporaneo ai contenuti a pagamento. L&#39;accesso è per richiedente, per un periodo di tempo specificato dal programmatore.

## TTL {#ttl}

Time To Live. Periodo di tempo specificato per la validità di un token.

## TVE {#tve}

Televisione Ovunque.

## ID utente {#user-id}

Identifica in modo univoco l’utente dell’app di un programmatore, ma ha origine dall’MVPD. Disponibile in diversi moduli per diversi casi d’uso. Consulta [Informazioni sugli ID utente nella panoramica del programmatore](/help/authentication/programmer-overview.md#user-ids).

## Elenco Consentiti {#whitelist}

Elenco di domini designati come legittimi ai fini della comunicazione con l’autenticazione di Adobe Pass.

## Token XSTS {#xsts-token}

Un token di sicurezza rilasciato da Microsoft per lo sviluppo di app console Xbox, utilizzato nell&#39;integrazione di autenticazione Xbox/Adobe Pass.
