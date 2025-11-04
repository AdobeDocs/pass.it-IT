---
title: Manuale di JavaScript SDK
description: Manuale di JavaScript SDK
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '957'
ht-degree: 0%

---

# Manuale di JavaScript SDK (legacy) {#javascript-sdk-cookbook}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Introduzione {#intro}

Questo documento descrive i flussi di lavoro di adesione implementati dall’applicazione di livello superiore di un programmatore per un’integrazione di JavaScript con il servizio di autenticazione di Adobe Pass. I collegamenti alle API di riferimento di JavaScript sono inclusi in.

Inoltre, la sezione [Informazioni correlate](#related) include
collegamento a un set di esempi di codice JavaScript.

## Flussi di diritti {#entitlement}

1. [Prerequisiti](#prereq)
2. [Flusso di avvio](#startup)
3. [Flusso di autenticazione](#authn)
4. [Flusso di autorizzazione](#authz)
5. [Visualizza flusso multimediale](#logout)

</br>

![](/help//authentication/assets/javascript-flows.png)


## Prerequisiti {#prereq}

**Dipendenze:**

- Libreria di autenticazione di Adobe Pass (AccessEnabler). Collabora con il tuo Adobe Pass Authentication Account Manager per risolvere il problema.
- RequestorId autenticazione Adobe Pass valido. Per risolvere il problema, rivolgiti al tuo Adobe Pass Authentication Account Manager.

Creare le funzioni di callback:

- `entitlementLoaded`
</br>

**Trigger:** L&#39;inizializzazione di AccessEnabler è stata caricata e completata.

- `displayProviderDialog(mvpds)`

  **Trigger:** `getAuthentication(),` solo se l&#39;utente non ha selezionato un provider (un MVPD) e non è ancora autenticato
Il parametro mvpds è un array di provider disponibili per l&#39;utente.

- `setAuthenticationStatus(status, errorcode)`

  **Attivatore:**
   - `checkAuthentication()` ogni volta.
   - `getAuthentication()` solo se l&#39;utente è già autenticato e ha selezionato un provider.

  Lo stato restituito è success o failure; il codice di errore descrive il tipo di errore.

- `createIFrame(width, height)`

  **Trigger:** `setSelectedProvider(providerID)`, solo se il provider selezionato è configurato per la visualizzazione in un IFrame.

  >[!NOTE]
  >
  >Un provider è configurato per eseguire il rendering della schermata di autenticazione come reindirizzamento o in un iFrame e il programmatore deve tenere conto di entrambi.

- `sendTrackingData(event, data)`

  **Trigger:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  Il parametro `event` indica quale evento di adesione si è verificato; il parametro `data` è un elenco di valori relativi all&#39;evento.
- `setToken(token, resource)`
  **Trigger:** `checkAuthorization()` e `getAuthorization()` dopo un&#39;autorizzazione riuscita per visualizzare una risorsa.   Il parametro `token` è il token multimediale di breve durata. Il parametro `resource` è il contenuto che l&#39;utente è autorizzato a visualizzare.

- `tokenRequestFailed(resource, code, description)`
  **Trigger:**`checkAuthorization()` e`getAuthorization()` dopo un&#39;autorizzazione non riuscita.\
  Il parametro `resource` è il contenuto che l&#39;utente stava tentando di visualizzare. Il parametro `code` è il codice di errore che indica il tipo di errore che si è verificato. Il parametro `description` descrive l&#39;errore associato al codice di errore.

- `selectedProvider(mvpd)`

  **Trigger:** [`getSelectedProvider()`]&#x200B;(#$getSelProv Il parametro `mvpd` fornisce informazioni sul provider selezionato da
utente.

- `setMetadataStatus(metadata, key, arguments)`

  **Attivatore:** `getMetadata().`\
  Il parametro `metadata` fornisce i dati specifici richiesti; il parametro chiave è la chiave utilizzata nella richiesta `getMetadata()` e il parametro `arguments` è lo stesso dizionario passato a `getMetadata()`.


## &#x200B;2. Flusso di avvio

**I. Caricare AccessEnabler JavaScript:**

**Per Il Profilo Di Staging**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

o...

**Per Il Profilo Di Produzione**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Triggers:** Al termine dell&#39;inizializzazione, Adobe Pass
l&#39;autenticazione chiama la funzione di callback `entitlementLoaded()`. Questo è il punto di ingresso per la comunicazione dell&#39;applicazione con AccessEnabler.


**II.** Chiama `setRequestor()` per stabilire
identità del programmatore; passare il `requestorID` del programmatore e
(facoltativamente) un array di endpoint di autenticazione Adobe Pass.

**Trigger:** Nessuno, ma consente di chiamare `displayProviderDialog()` quando necessario.


**III.** Chiamare `checkAuthentication()` per verificare la presenza di un&#39;autenticazione esistente senza avviare il [flusso di autenticazione] completo.  Se la chiamata ha esito positivo, puoi passare direttamente a `authorization flow`.  In caso contrario, passare a `authentication flow`.

**Dipendenza:** Chiamata riuscita a `setRequestor()` (questa dipendenza si applica anche a tutte le chiamate successive).

**Trigger:** `setAuthenticationStatus()` callback

</br>

## &#x200B;3. Flusso di autenticazione</span>


**Dipendenza:** Chiamata riuscita a `setRequestor()` (questa dipendenza si applica anche a tutte le chiamate successive).


Chiamare `getAuthentication()` per ottenere lo stato di autenticazione OPPURE per attivare il flusso di autenticazione del provider.

**Trigger:**

- `displayProviderDialog()`se l&#39;utente non è ancora stato autenticato
- `setAuthenticationStatus()` se l&#39;autenticazione è già stata eseguita

Il completamento del flusso di autenticazione viene raggiunto quando AccessEnabler chiama `setAuthenticationStatus()` con `isAuthenticated == 1`.

## &#x200B;4. Flusso di autorizzazione {#authz}

**Dipendenze:**

- Chiamata a `setRequestor()` riuscita (questa dipendenza si applica anche a tutte le chiamate successive).
- ID risorsa validi concordati con il MVPD. Tieni presente che gli ID risorsa devono essere gli stessi utilizzati su qualsiasi altro dispositivo o piattaforma e che saranno gli stessi in tutti gli MVPD.

Chiamare `getAuthorization()` e passare ResourceID per il supporto richiesto. In caso di esito positivo, la chiamata restituisce un token multimediale breve, che conferma che l’utente è autorizzato a visualizzare il contenuto multimediale richiesto.

- Se la chiamata viene superata: l’utente dispone di un token di autenticazione valido e di un’autorizzazione alla visualizzazione del contenuto multimediale richiesto.
- Se la chiamata non riesce: esamina l’eccezione generata per determinarne il tipo (AuthN, AuthZ o altro):
- Se la chiamata era un errore AuthN, riavvia il flusso AuthN.
- Se la chiamata era un errore AuthZ, l’utente non è autorizzato a guardare il contenuto multimediale richiesto e deve visualizzare un qualche tipo di messaggio di errore.
- Se si è verificato un altro errore (errore di connessione, errore di rete, ecc.), visualizza un messaggio di errore appropriato.

Utilizza Media Token Verifier per convalidare il shortMediaToken restituito da una chiamata `getAuthorization()` riuscita.


**Dipendenza:** Il verificatore del token multimediale breve (incluso con il
libreria AccessEnabler)

- Se la convalida viene superata: visualizza/riproduce il supporto richiesto per l’utente.
- In caso contrario: il token AuthZ non è valido, la richiesta del supporto deve essere rifiutata e deve essere visualizzato un messaggio di errore.

## &#x200B;5. Visualizza flusso multimediale {#logout}

- L’utente seleziona il file multimediale da visualizzare.
   - Il supporto è protetto?
      - L’app controlla se il contenuto multimediale è protetto:
         - Se il supporto è protetto, l’app avvia il flusso di autorizzazione (AuthZ) qui sopra.
         - Se il supporto non è protetto, procedere con il flusso Visualizza supporto.
         - Supporti di riproduzione

## Configurazione dell’ID visitatore {#visitorID}

La configurazione di un valore [Experience Cloud visitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=it) è molto importante dal punto di vista analitico. Una volta impostato il valore EC visitorID, SDK invierà queste informazioni insieme a ogni chiamata di rete e il servizio di autenticazione di Adobe Pass le raccoglierà. In questo modo potrai correlare i dati di analisi del servizio di autenticazione di Adobe Pass con qualsiasi altro rapporto di analisi disponibile in altre applicazioni o siti Web. Le informazioni su come impostare EC visitorID sono disponibili [qui](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=it).


>[!NOTE]
>
>Questa funzionalità è disponibile a partire dalla versione 3.1.0 di JS SDK.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
