---
title: Manuale dell’integrazione di Amazon FireOS
description: Manuale dell’integrazione di Amazon FireOS
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1424'
ht-degree: 0%

---

# Manuale dell’integrazione di Amazon FireOS {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

</br>


## Introduzione {#intro}

Questo documento descrive i flussi di lavoro per l&#39;adesione che un&#39;applicazione di livello superiore di un programmatore può implementare tramite le API esposte dalla libreria `AccessEnabler` di Amazon FireOS.

La soluzione di autenticazione Adobe Pass per Amazon FireOS è infine suddivisa in due domini:

- Dominio dell&#39;interfaccia utente: è il livello superiore dell&#39;applicazione che implementa l&#39;interfaccia utente e utilizza i servizi forniti dalla libreria `AccessEnabler` per fornire l&#39;accesso al contenuto con restrizioni.
- Dominio `AccessEnabler`: in questo caso i flussi di lavoro per l&#39;adesione vengono implementati sotto forma di:
   - Chiamate di rete effettuate ai server back-end di Adobe
   - Regole della logica di business relative ai flussi di lavoro di autenticazione e autorizzazione
   - Gestione di varie risorse ed elaborazione dello stato del flusso di lavoro (ad esempio la cache dei token)

L&#39;obiettivo del dominio `AccessEnabler` è nascondere tutte le complessità dei flussi di lavoro di adesione e fornire all&#39;applicazione di livello superiore (tramite la libreria `AccessEnabler`) un set di semplici primitive di adesione. Questo processo consente di implementare i flussi di lavoro per l’adesione:

1. Imposta l&#39;identità del richiedente.
1. Verifica e ottieni l’autenticazione per un determinato provider di identità.
1. Controlla e ottieni l’autorizzazione per una particolare risorsa.
1. Disconnetti.

L&#39;attività di rete di `AccessEnabler` viene eseguita in un thread diverso, pertanto il thread dell&#39;interfaccia utente non viene mai bloccato. Di conseguenza, il canale di comunicazione bidirezionale tra i due domini dell’applicazione deve seguire un modello completamente asincrono:

- Il livello applicazione dell&#39;interfaccia utente invia messaggi al dominio `AccessEnabler` tramite le chiamate API esposte dalla libreria `AccessEnabler`.
- `AccessEnabler` risponde al livello dell&#39;interfaccia utente tramite i metodi di callback inclusi nel protocollo `AccessEnabler` che il livello dell&#39;interfaccia utente registra con la libreria `AccessEnabler`.

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

      - Attivato da `setRequestor()`, restituisce esito positivo o negativo.     Il successo indica che puoi procedere con le chiamate di adesione.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - Attivato da `getAuthentication()` solo se l&#39;utente non ha selezionato un provider (MVPD) e non è ancora autenticato. Il parametro `mvpds` è un array di provider disponibili per l&#39;utente.

   - [&quot;setAuthenticationStatus(status, reason)&quot;](#$setAuthNStatus)

      - Attivato da `checkAuthentication()` ogni volta. Attivato da `getAuthentication()` solo se l&#39;utente è già autenticato e ha selezionato un provider.

      - Lo stato restituito è autenticato o non autenticato. Il motivo descrive un errore di autenticazione o un&#39;azione di disconnessione.

   - [navigateToUrl(url)](#$navigateToUrl)

      - Ignorato nell&#39;SDK di AmazonFireOS, il metodo viene utilizzato sulle piattaforme Android in cui viene attivato da `getAuthentication()` dopo che l&#39;utente ha selezionato un MVPD.  Il parametro `url` fornisce il percorso della pagina di accesso di MVPD.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

      - Attivato da `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
Il parametro `event` indica quale evento di adesione si è verificato; il parametro `data` è un elenco di valori relativi all&#39;evento.

   - [&quot;setToken(token, resource)&quot;](#$setToken)

      - Attivazione eseguita da `checkAuthorization()` e `getAuthorization()` dopo un&#39;autorizzazione di visualizzazione di una risorsa completata.
      - Il parametro `token` è il token multimediale di breve durata; il parametro `resource` è il contenuto che l&#39;utente è autorizzato a visualizzare.

   - [&quot;tokenRequestFailed(resource, code, description)&quot;](#$tokenRequestFailed)

      - Attivato da `checkAuthorization()` e `getAuthorization()` dopo un&#39;autorizzazione non riuscita.
      - Il parametro `resource` è il contenuto che l&#39;utente stava tentando di visualizzare. Il parametro `code` è il codice di errore che indica il tipo di errore che si è verificato. Il parametro `description` descrive l&#39;errore associato al codice di errore.

   - [&quot;selectedProvider(mvpd)&quot;](#$selectedProvider)

      - Attivato da `getSelectedProvider()`.
      - Il parametro `mvpd` fornisce informazioni sul provider selezionato dall&#39;utente.

   - [&quot;setMetadataStatus(metadata, key, arguments)&quot;](#$setMetadataStatus)

      - Attivato da `getMetadata().`
      - Il parametro `metadata` fornisce i dati specifici richiesti; il parametro `key` è la chiave utilizzata nella richiesta `getMetadata()` e il parametro `arguments` è lo stesso dizionario passato a `getMetadata()`.

   - [&quot;preauthorizedResources(resources)&quot;](#$preauthResources)

      - Attivato da `checkPreauthorizedResources()`.
      - Il parametro `authorizedResources` presenta le risorse che l&#39;utente è autorizzato a visualizzare.


![](../../../../assets/android-entitlement-flows.png)


### B. Flusso di avvio {#startup_flow}

1. Avvia l&#39;applicazione di livello superiore.
1. Avvia l&#39;autenticazione Adobe Pass.

   1. Chiamare [`getInstance`](#$getInstance) per creare una singola istanza dell&#39;autenticazione Adobe Pass `AccessEnabler`.

      - **Dipendenza:** Libreria Amazon FireOS nativa per l&#39;autenticazione di Adobe Pass (`AccessEnabler`)

   1. Chiamare ` setRequestor()` per stabilire l&#39;identità del programmatore; passare `requestorID` del programmatore e (facoltativamente) un array di endpoint di autenticazione Adobe Pass.

      - **Dipendenza:** ID richiedente autenticazione Adobe Pass valido (rivolgiti al tuo Adobe Pass Authentication Account Manager per risolvere il problema).

      - **Trigger:** callback setRequestorComplete()

   Non è possibile completare alcuna richiesta di adesione finché non viene stabilita l&#39;identità completa del richiedente. Ciò significa che mentre setRequestor() è ancora in esecuzione, tutte le richieste di adesione successive (ad esempio,`checkAuthentication()`) sono bloccate.

   Sono disponibili due opzioni di implementazione: una volta inviate le informazioni di identificazione del richiedente al server backend, il livello applicazione dell’interfaccia utente può scegliere uno dei due approcci seguenti:</p>

   1. Attendere l&#39;attivazione del callback `setRequestorComplete()` (parte del delegato `AccessEnabler`).  Questa opzione fornisce la massima certezza che `setRequestor()` è stato completato, quindi è consigliata per la maggior parte delle implementazioni.
   1. Continuare senza attendere l&#39;attivazione del callback `setRequestorComplete()` e iniziare a emettere richieste di adesione. Queste chiamate (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) sono in coda dalla libreria `AccessEnabler`, che eseguirà le chiamate di rete effettive dopo `setRequestor()`. Questa opzione può talvolta essere interrotta se, ad esempio, la connessione di rete è instabile.

1. Chiamare [checkAuthentication()](#$checkAuthN) per verificare la presenza di un&#39;autenticazione esistente senza avviare il flusso di autenticazione completo.  Se la chiamata ha esito positivo, puoi passare direttamente al flusso di autorizzazione.  In caso contrario, passare al flusso di autenticazione.

- **Dipendenza:** Chiamata riuscita a `setRequestor()` (questa dipendenza si applica anche a tutte le chiamate successive).

- **Trigger:** callback setAuthenticationStatus()

### C. Flusso di autenticazione {#authn_flow}

1. Chiamare [`getAuthentication()`](#$getAuthN) per avviare il flusso di autenticazione o per ottenere la conferma che l&#39;utente è già autenticato.

   **Trigger:**

   - Il callback setAuthenticationStatus(), se l&#39;utente è già autenticato.  In questo caso, passare direttamente al [Flusso di autorizzazione](#authz_flow).
   - Il callback displayProviderDialog(), se l&#39;utente non è ancora autenticato.

1. Presentare all&#39;utente l&#39;elenco dei provider inviati a `displayProviderDialog()`.
1. Dopo che l&#39;utente ha selezionato un provider, WebView aprirà la pagina del provider per l&#39;accesso dell&#39;utente

   >[!NOTE]
   >
   >A questo punto, l’utente ha la possibilità di annullare il flusso di autenticazione. In questo caso, `AccessEnabler` rimuoverà il proprio stato interno e reimposterà il flusso di autenticazione.

1. Dopo che l&#39;utente avrà eseguito correttamente l&#39;accesso, WebView verrà chiuso.
1. chiamare `getAuthenticationToken(),` che indica a `AccessEnabler` di recuperare il token di autenticazione dal server back-end.
1. [Facoltativo] Chiamare [`checkPreauthorizedResources(resources)`](#$checkPreauth) per verificare quali risorse l&#39;utente è autorizzato a visualizzare. Il parametro `resources` è un array di risorse protette associate al token di autenticazione dell&#39;utente.

   **Trigger:** `preAuthorizedResources()` callback\
   **Punto di esecuzione:** dopo il completamento del flusso di autenticazione

1. Se l’autenticazione ha esito positivo, procedi al flusso di autorizzazione.


### D. Flusso di autorizzazione {#authz_flow}

1. Chiamare [`getAuthorization()`](#$getAuthZ) per avviare il flusso di autorizzazione.

   Dipendenza: ID risorsa validi concordati con gli MVPD.

   **Nota:** i ResourceID devono essere gli stessi utilizzati su qualsiasi altro dispositivo o piattaforma e saranno gli stessi in tutti gli MVPD.

1. Convalida autenticazione e autorizzazione.

   - Se la chiamata `getAuthorization()` ha esito positivo: l&#39;utente dispone di token AuthN e AuthZ validi (l&#39;utente è autenticato e autorizzato a guardare il contenuto multimediale richiesto).
   - Se `getAuthorization()` ha esito negativo: esaminare l&#39;eccezione generata per determinarne il tipo (AuthN, AuthZ o altro):
      - In caso di errore di autenticazione (AuthN), riavvia il flusso di autenticazione.
      - Se si trattava di un errore di autorizzazione (AuthZ), l’utente non è autorizzato a guardare il contenuto multimediale richiesto e deve visualizzare all’utente un qualche tipo di messaggio di errore.
      - Se si è verificato un altro tipo di errore (errore di connessione, errore di rete, ecc.), visualizza un messaggio di errore appropriato.

1. Convalida il token multimediale breve.

   Utilizza la libreria Adobe Pass Authentication Media Token Verifier per verificare il token multimediale di breve durata restituito dalla chiamata `getAuthorization()` precedente:

   - Se la convalida ha esito positivo: riproduci il file multimediale richiesto per l’utente.
   - Se la convalida non riesce: il token AuthZ non è valido, la richiesta di supporto deve essere rifiutata e l’utente deve visualizzare un messaggio di errore.

1. Tornare al normale flusso dell&#39;applicazione.

### E. Visualizza flusso multimediale {#media_flow}

1. L’utente seleziona il file multimediale da visualizzare.
1. Il supporto è protetto?  L&#39;applicazione verifica se il supporto selezionato è protetto:
   - Se il supporto selezionato è protetto, l&#39;applicazione avvia il [flusso di autorizzazione](#authz_flow).
   - Se il supporto selezionato non è protetto, riprodurlo per l&#39;utente.

### F. Flusso di disconnessione {#logout_flow}

1. Chiamare [`logout()`](#$logout) per disconnettersi. `AccessEnabler` cancella tutti i valori e i token memorizzati in cache ottenuti dall&#39;utente per l&#39;MVPD corrente su tutti i richiedenti che condividono l&#39;accesso tramite Single Sign-On. Dopo aver cancellato la cache, `AccessEnabler` effettua una chiamata al server per pulire le sessioni lato server.  Poiché la chiamata al server potrebbe causare un reindirizzamento SAML all’IdP (consentendo la pulizia della sessione sul lato IdP), questa chiamata deve seguire tutti i reindirizzamenti. Per questo motivo, questa chiamata verrà gestita all&#39;interno di un controllo WebView, invisibile per l&#39;utente.

   **Nota:** il flusso di disconnessione è diverso dal flusso di autenticazione in quanto l&#39;utente non è tenuto a interagire in alcun modo con WebView. È quindi possibile (e consigliato) rendere il controllo WebView invisibile (ovvero nascosto) durante il processo di logout.
