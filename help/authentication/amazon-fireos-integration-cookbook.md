---
title: Manuale dell’integrazione di Amazon FireOS
description: Manuale dell’integrazione di Amazon FireOS
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1432'
ht-degree: 0%

---

# Manuale dell’integrazione di Amazon FireOS {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>


## Introduzione {#intro}

Questo documento descrive i flussi di lavoro di adesione che l’applicazione di livello superiore di un programmatore può implementare tramite le API esposte dalla libreria AccessEnabler di Amazon FireOS.

La soluzione di autenticazione Adobe Pass per Amazon FireOS è infine suddivisa in due domini:

- Dominio dell’interfaccia utente: livello applicativo superiore che implementa l’interfaccia utente e utilizza i servizi forniti dalla libreria AccessEnabler per fornire accesso a contenuti con restrizioni.
- Dominio AccessEnabler: qui vengono implementati i flussi di lavoro per l’adesione sotto forma di:
   - Chiamate di rete effettuate ai server back-end di Adobe
   - Regole della logica di business relative ai flussi di lavoro di autenticazione e autorizzazione
   - Gestione di varie risorse ed elaborazione dello stato del flusso di lavoro (ad esempio la cache dei token)

L&#39;obiettivo del dominio AccessEnabler è nascondere tutte le complessità dei flussi di lavoro di adesione e fornire all&#39;applicazione di livello superiore (tramite la libreria AccessEnabler) un set di semplici primitive di adesione con cui implementare i flussi di lavoro di adesione:

1. Imposta l&#39;identità del richiedente
1. Verifica e ottieni l’autenticazione per un determinato provider di identità
1. Controllare e ottenere l’autorizzazione per una particolare risorsa
1. Disconnetti

L&#39;attività di rete di AccessEnabler si svolge in un thread diverso, pertanto il thread dell&#39;interfaccia utente non viene mai bloccato. Di conseguenza, il canale di comunicazione bidirezionale tra i due domini dell’applicazione deve seguire un modello completamente asincrono:

- Il livello applicazione dell’interfaccia utente invia messaggi al dominio AccessEnabler tramite le chiamate API esposte dalla libreria AccessEnabler.
- AccessEnabler risponde al livello dell&#39;interfaccia utente tramite i metodi di callback inclusi nel protocollo AccessEnabler che il livello dell&#39;interfaccia utente registra con la libreria AccessEnabler.

## Flussi di diritti {#entitlement}

1. [Prerequisiti](#prereqs)
1. [Flusso di avvio](#startup_flow)
1. [Flusso di autenticazione](#authn_flow)
1. [Flusso di autorizzazione](#authz_flow)
1. [Visualizza flusso multimediale](#media_flow)
1. [Flusso di disconnessione](#logout_flow)



### A. Prerequisiti {#prereqs}

1. Creare le funzioni di callback:
   - [&quot;setRequestorComplete()&quot;](#$setRequestorComplete)

      - Attivato da `setRequestor()`, restituisce l&#39;esito positivo o negativo.     Il successo indica che puoi procedere con le chiamate di adesione.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - Attivato da `getAuthentication()` solo se l’utente non ha selezionato un provider (MVPD) e non è ancora autenticato. Il `mvpds` Il parametro è un array di provider disponibili per l&#39;utente.

   - [&quot;setAuthenticationStatus(status, reason)&quot;](#$setAuthNStatus)

      - Attivato da `checkAuthentication()` ogni volta. Attivato da `getAuthentication()` solo se l’utente è già autenticato e ha selezionato un provider.

      - Lo stato restituito è autenticato o non autenticato. Il motivo descrive un errore di autenticazione o un&#39;azione di disconnessione.

   - [navigateToUrl(url)](#$navigateToUrl)

      - Ignorato nell’SDK di AmazonFireOS, il metodo viene utilizzato sulle piattaforme Android dove viene attivato da `getAuthentication()` dopo che l’utente ha selezionato un MVPD.  Il `url` Il parametro fornisce la posizione della pagina di accesso di MVPD.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

      - Attivato da `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
Il `event` il parametro indica quale evento di adesione si è verificato;il `data` Il parametro è un elenco di valori relativi all’evento.

   - [&quot;setToken(token, resource)&quot;](#$setToken)

      - Attivato da `checkAuthorization()` e `getAuthorization()` dopo un’autorizzazione riuscita a visualizzare una risorsa.
      - Il `token` è il token multimediale di breve durata;il `resource` Il parametro è il contenuto che l’utente è autorizzato a visualizzare.

   - [&quot;tokenRequestFailed(resource, code, description)&quot;](#$tokenRequestFailed)

      - Attivato da `checkAuthorization()` e `getAuthorization()` dopo un’autorizzazione non riuscita.
      - Il `resource` parametro è il contenuto che l&#39;utente stava tentando di visualizzare; il `code` parametro è il codice di errore che indica il tipo di errore che si è verificato; il `description` Il parametro descrive l’errore associato al codice di errore.

   - [&quot;selectedProvider(mvpd)&quot;](#$selectedProvider)

      - Attivato da `getSelectedProvider()`.
      - Il `mvpd` Il parametro fornisce informazioni sul provider selezionato dall&#39;utente.

   - [&quot;setMetadataStatus(metadata, key, arguments)&quot;](#$setMetadataStatus)

      - Attivato da `getMetadata().`
      - Il `metadata` Il parametro fornisce i dati specifici richiesti; il `key` Il parametro è la chiave utilizzata nel `getMetadata()` richiesta; e `arguments` Il parametro è lo stesso dizionario che è stato passato a `getMetadata()`.

   - [&quot;preauthorizedResources(resources)&quot;](#$preauthResources)

      - Attivato da `checkPreauthorizedResources()`.
      - Il `authorizedResources` Il parametro presenta le risorse che l’utente è autorizzato a visualizzare.


![](assets/android-entitlement-flows.png)


### B. Flusso di avvio {#startup_flow}

1. Avvia l&#39;applicazione di livello superiore.
1. Avvia l&#39;autenticazione Adobe Pass.

   1. Chiamata [`getInstance`](#$getInstance) per creare una singola istanza di Adobe Pass Authentication AccessEnabler.

      - **Dipendenza:** Libreria Amazon FireOS nativa per l’autenticazione di Adobe Pass (AccessEnabler)

   1. Chiamata` setRequestor()` per stabilire l&#39;identificazione del programmatore; passare nel programma `requestorID` e (facoltativamente) un array di endpoint di autenticazione Adobe Pass.

      - **Dipendenza:** ID richiedente autenticazione Adobe Pass valido (rivolgiti al tuo Adobe Pass Authentication Account Manager per ottenere questo risultato).

      - **Trigger:** callback setRequestorComplete()

   Non è possibile completare alcuna richiesta di adesione finché non viene stabilita l&#39;identità completa del richiedente. Ciò significa che mentre setRequestor() è ancora in esecuzione, tutte le richieste di adesione successive (ad esempio,`checkAuthentication()`) sono bloccati.

   Sono disponibili due opzioni di implementazione: una volta inviate le informazioni di identificazione del richiedente al server backend, il livello applicazione dell’interfaccia utente può scegliere uno dei due approcci seguenti:</p>

   1. Attendi l&#39;attivazione di `setRequestorComplete()` callback (parte del delegato AccessEnabler).  Questa opzione offre la massima certezza che `setRequestor()` è stato completato, quindi è consigliato per la maggior parte delle implementazioni.
   1. Continua senza attendere l&#39;attivazione di `setRequestorComplete()` richiamata e inizia a emettere richieste di adesione. Queste chiamate (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) sono in coda dalla libreria AccessEnabler, che effettua le chiamate di rete effettive dopo `setRequestor()`. Questa opzione può talvolta essere interrotta se, ad esempio, la connessione di rete è instabile.

1. Chiamata [checkAuthentication()](#$checkAuthN) per verificare la presenza di un&#39;autenticazione esistente senza avviare il flusso di autenticazione completo.  Se la chiamata ha esito positivo, puoi passare direttamente al flusso di autorizzazione.  In caso contrario, passare al flusso di autenticazione.

- **Dipendenza:** Chiamata a riuscita `setRequestor()` (questa dipendenza si applica anche a tutte le chiamate successive).

- **Trigger:** callback setAuthenticationStatus()

### C. Flusso di autenticazione {#authn_flow}

1. Chiamata [`getAuthentication()`](#$getAuthN) per avviare il flusso di autenticazione o per ottenere la conferma che l’utente è già autenticato.

   **Trigger:**

   - Il callback setAuthenticationStatus(), se l&#39;utente è già autenticato.  In questo caso, passare direttamente al [Flusso di autorizzazione](#authz_flow).
   - Il callback displayProviderDialog(), se l&#39;utente non è ancora autenticato.

1. Presenta all’utente l’elenco dei provider inviati a `displayProviderDialog()`.
1. Dopo che l&#39;utente ha selezionato un provider, WebView aprirà la pagina del provider per l&#39;accesso dell&#39;utente

   >[!NOTE]
   >
   >A questo punto, l’utente ha la possibilità di annullare il flusso di autenticazione. In questo caso, AccessEnabler rimuoverà il proprio stato interno e reimposterà il flusso di autenticazione.

1. Dopo che l&#39;utente avrà effettuato l&#39;accesso, WebView verrà chiuso.
1. chiamare `getAuthenticationToken(),` che indica ad AccessEnabler di recuperare il token di autenticazione dal server back-end.
1. [Facoltativo] Chiamata [`checkPreauthorizedResources(resources)`](#$checkPreauth) per verificare quali risorse l’utente è autorizzato a visualizzare. Il `resources` Il parametro è un array di risorse protette associate al token di autenticazione dell’utente.

   **Trigger:** `preAuthorizedResources()` callback\
   **Punto di esecuzione:** Dopo il completamento del flusso di autenticazione

1. Se l’autenticazione ha esito positivo, procedi al flusso di autorizzazione.


### D. Flusso di autorizzazione {#authz_flow}

1. Chiamata [`getAuthorization()`](#$getAuthZ) per avviare il flusso di autorizzazione.

   Dipendenza: ID risorsa validi concordati con gli MVPD.

   **Nota:** I ResourceID devono essere gli stessi utilizzati su qualsiasi altro dispositivo o piattaforma e sono gli stessi in tutti gli MVPD.

1. Convalida autenticazione e autorizzazione.

   - Se il `getAuthorization()` chiamata riuscita: l’utente dispone di token AuthN e AuthZ validi (l’utente è autenticato e autorizzato a guardare il contenuto multimediale richiesto).
   - Se `getAuthorization()` failed: esamina l’eccezione generata per determinarne il tipo (AuthN, AuthZ o altro):
      - In caso di errore di autenticazione (AuthN), riavvia il flusso di autenticazione.
      - Se si trattava di un errore di autorizzazione (AuthZ), l’utente non è autorizzato a guardare il contenuto multimediale richiesto e deve visualizzare all’utente un qualche tipo di messaggio di errore.
      - Se si è verificato un altro tipo di errore (errore di connessione, errore di rete, ecc.) quindi visualizza un messaggio di errore appropriato.

1. Convalida il token multimediale breve.

   Utilizza la libreria Adobe Pass Authentication Media Token Verifier per verificare il token multimediale di breve durata restituito da `getAuthorization()` richiamare:

   - Se la convalida ha esito positivo: riproduci il file multimediale richiesto per l’utente.
   - Se la convalida non riesce: il token AuthZ non è valido, la richiesta di supporto deve essere rifiutata e l’utente deve visualizzare un messaggio di errore.

1. Tornare al normale flusso dell&#39;applicazione.

### E. Visualizza flusso multimediale {#media_flow}

1. L’utente seleziona il file multimediale da visualizzare.
1. Il supporto è protetto?  L&#39;applicazione verifica se il supporto selezionato è protetto:
   - Se il supporto selezionato è protetto, l&#39;applicazione avvia [Flusso di autorizzazione](#authz_flow) sopra.
   - Se il supporto selezionato non è protetto, riprodurlo per l&#39;utente.

### F. Flusso di disconnessione {#logout_flow}

1. Chiamata [`logout()`](#$logout) per disconnettere l&#39;utente. AccessEnabler cancella tutti i valori e i token memorizzati nella cache ottenuti dall&#39;utente per l&#39;MVPD corrente su tutti i richiedenti che condividono l&#39;accesso tramite Single Sign-On. Dopo aver cancellato la cache, AccessEnabler effettua una chiamata al server per pulire le sessioni lato server.  Poiché la chiamata al server potrebbe causare un reindirizzamento SAML all’IdP (consentendo la pulizia della sessione sul lato IdP), questa chiamata deve seguire tutti i reindirizzamenti. Per questo motivo, questa chiamata verrà gestita all&#39;interno di un controllo WebView, invisibile per l&#39;utente.

   **Nota:** Il flusso di disconnessione è diverso dal flusso di autenticazione in quanto l&#39;utente non è tenuto a interagire in alcun modo con WebView. È quindi possibile (e consigliato) rendere il controllo WebView invisibile (ovvero nascosto) durante il processo di logout.
