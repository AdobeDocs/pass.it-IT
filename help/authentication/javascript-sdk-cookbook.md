---
title: Manuale dell’SDK JavaScript
description: Manuale dell’SDK JavaScript
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# Manuale dell’SDK JavaScript {#javascript-sdk-cookbook}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#intro}

Questo documento descrive i flussi di lavoro di adesione implementati dall’applicazione di livello superiore di un programmatore per un’integrazione JavaScript con il servizio di autenticazione di Adobe Pass. I collegamenti alla documentazione API di riferimento per JavaScript sono inclusi in.

Si noti inoltre che la [Informazioni correlate](#related) include un collegamento a un set di esempi di codice JavaScript.

## Flussi di diritti {#entitlement}

1. [Prerequisiti](#prereq)
2. [Flusso di avvio](#startup)
3. [Flusso di autenticazione](#authn)
4. [Flusso di autorizzazione](#authz)
5. [Visualizza flusso multimediale](#logout)

</br>

![](assets/javascript-flows.png)


## Prerequisiti {#prereq}

**Dipendenze:**

- Libreria di autenticazione di Adobe Pass (AccessEnabler). Collabora con il tuo Adobe Pass Authentication Account Manager per risolvere il problema.
- RequestorId autenticazione Adobe Pass valido. Per risolvere il problema, rivolgiti al tuo Adobe Pass Authentication Account Manager.

Creare le funzioni di callback:

- `entitlementLoaded`
</br>

**Attivatore:** L&#39;inizializzazione di AccessEnabler è stata caricata e completata.

- `displayProviderDialog(mvpds)`

  **Attivatore:** `getAuthentication(),` solo se l&#39;utente non ha selezionato un provider (un MVPD) e non è ancora autenticato Il parametro mvpds è un array di provider disponibili per l&#39;utente.

- `setAuthenticationStatus(status, errorcode)`

  **Attivatore:**
   - `checkAuthentication()`ogni volta.
   - `getAuthentication()` solo se l’utente è già autenticato e ha selezionato un provider.

  Lo stato restituito è success o failure; il codice di errore descrive il tipo di errore.

- `createIFrame(width, height)`

  **Attivatore:** `setSelectedProvider(providerID)`, solo se il provider selezionato è configurato per la visualizzazione in un IFrame.

  >[!NOTE]
  >
  >Un provider è configurato per eseguire il rendering della schermata di autenticazione come reindirizzamento o in un iFrame e il programmatore deve tenere conto di entrambi.

- `sendTrackingData(event, data)`

  **Trigger:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  Il `event` il parametro indica quale evento di adesione si è verificato; il `data` Il parametro è un elenco di valori relativi all’evento.
- `setToken(token, resource)`
  **Attivatore:** `checkAuthorization()`e `getAuthorization()` dopo un’autorizzazione riuscita a visualizzare una risorsa.   Il `token` è il token multimediale di breve durata; il `resource` Il parametro è il contenuto che l’utente è autorizzato a visualizzare.

- `tokenRequestFailed(resource, code, description)`
  **Attivatore:**`checkAuthorization()` e`getAuthorization()`  dopo un’autorizzazione non riuscita.\
  Il `resource` parametro è il contenuto che l&#39;utente stava tentando di visualizzare; il `code` parametro è il codice di errore che indica il tipo di errore che si è verificato; il `description` Il parametro descrive l’errore associato al codice di errore.

- `selectedProvider(mvpd)`

  **Attivatore:** [`getSelectedProvider()`](#$getSelProv `mvpd` Il parametro fornisce informazioni sul provider selezionato dall&#39;utente.

- `setMetadataStatus(metadata, key, arguments)`

  **Attivatore:** `getMetadata().`\
  Il `metadata` Il parametro fornisce i dati specifici richiesti; il parametro chiave è la chiave utilizzata nel `getMetadata()`richiesta; e `arguments` Il parametro è lo stesso dizionario che è stato passato a `getMetadata()`.


## 2. Flusso di avvio

**I. Caricare AccessEnabler JavaScript:**

**Per il profilo di staging**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

o...

**Per il profilo di produzione**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Trigger:** Al termine dell’inizializzazione, l’autenticazione Adobe Pass chiama il `entitlementLoaded()` callback. Questo è il punto di ingresso per la comunicazione dell&#39;applicazione con AccessEnabler.


**II.** Chiamata `setRequestor()`per stabilire l&#39;identità del programmatore; passare nel `requestorID` e (facoltativamente) un array di endpoint di autenticazione Adobe Pass.

**Trigger:** Nessuno, ma abilita `displayProviderDialog()` da chiamare quando necessario.


**III.** Chiamata `checkAuthentication()` per verificare la presenza di un&#39;autenticazione esistente senza avviare il [flusso di autenticazione].  Se la chiamata ha esito positivo, puoi passare direttamente al `authorization flow`.  In caso contrario, passare alla `authentication flow`.

**Dipendenza:** Chiamata a riuscita `setRequestor()`(questa dipendenza si applica anche a tutte le chiamate successive).

**Trigger:** `setAuthenticationStatus()` callback

</br>

## 3. Flusso di autenticazione</span>


**Dipendenza:** Chiamata a riuscita `setRequestor()`(questa dipendenza si applica anche a tutte le chiamate successive).


Chiamata `getAuthentication()` per ottenere lo stato di autenticazione OPPURE per attivare il flusso di autenticazione del provider.

**Trigger:**

- `displayProviderDialog()`se l’utente non è ancora stato autenticato
- `setAuthenticationStatus()` se l’autenticazione ha già avuto luogo

Il completamento del flusso di autenticazione viene raggiunto quando AccessEnabler chiama `setAuthenticationStatus()`con `isAuthenticated == 1`.

## 4. Flusso di autorizzazione {#authz}

**Dipendenze:**

- Chiamata a riuscita `setRequestor()` (questa dipendenza si applica anche a tutte le chiamate successive).
- ResourceID validi concordati con gli MVPD. Tieni presente che gli ID risorsa devono essere gli stessi utilizzati su qualsiasi altro dispositivo o piattaforma e che saranno gli stessi in tutti gli MVPD.

Chiamata `getAuthorization()` e passa il ResourceID per il file multimediale richiesto. In caso di esito positivo, la chiamata restituisce un token multimediale breve, che conferma che l’utente è autorizzato a visualizzare il contenuto multimediale richiesto.

- Se la chiamata viene superata: l’utente dispone di un token di autenticazione valido e di un’autorizzazione alla visualizzazione del contenuto multimediale richiesto.
- Se la chiamata non riesce: esamina l’eccezione generata per determinarne il tipo (AuthN, AuthZ o altro):
- Se la chiamata era un errore AuthN, riavvia il flusso AuthN.
- Se la chiamata era un errore AuthZ, l’utente non è autorizzato a guardare il contenuto multimediale richiesto e deve visualizzare un qualche tipo di messaggio di errore.
- In caso di altri errori (errore di connessione, errore di rete, ecc.) quindi visualizza un messaggio di errore appropriato.

Utilizza il Media Token Verifier per convalidare il shortMediaToken restituito da un `getAuthorization()` chiamare.


**Dipendenza:** Il verificatore short media token (incluso nella libreria AccessEnabler)

- Se la convalida viene superata: visualizza/riproduce il supporto richiesto per l’utente.
- In caso contrario: il token AuthZ non è valido, la richiesta del supporto deve essere rifiutata e deve essere visualizzato un messaggio di errore.

## 5. Visualizza flusso multimediale {#logout}

- L’utente seleziona il file multimediale da visualizzare.
   - Il supporto è protetto?
      - L’app controlla se il contenuto multimediale è protetto:
         - Se il supporto è protetto, l’app avvia il flusso di autorizzazione (AuthZ) qui sopra.
         - Se il supporto non è protetto, procedere con il flusso Visualizza supporto.
         - Supporti di riproduzione

## Configurazione dell’ID visitatore {#visitorID}

Configurazione di un [ID visitatore Experience Cloud](https://experienceleague.adobe.com/docs/id-service/using/home.html) il valore è molto importante dal punto di vista di analytics. Una volta impostato il valore EC visitorID, l’SDK invierà queste informazioni insieme a ogni chiamata di rete e il servizio di autenticazione di Adobe Pass le raccoglierà. In questo modo potrai correlare i dati di analisi del servizio di autenticazione di Adobe Pass con qualsiasi altro rapporto di analisi disponibile in altre applicazioni o siti Web. Puoi trovare informazioni su come impostare EC visitorID [qui](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en).


>[!NOTE]
>
>Questa funzionalità è disponibile a partire dalla versione 3.1.0 dell’SDK JS.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
