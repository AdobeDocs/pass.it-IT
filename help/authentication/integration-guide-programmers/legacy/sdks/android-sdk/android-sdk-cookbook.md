---
title: Manuale dell’SDK di Android
description: Manuale dell’SDK di Android
exl-id: 7f66ab92-f52c-4dae-8016-c93464dd5254
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1681'
ht-degree: 0%

---

# Manuale dell’SDK di Android {#android-sdk-cookbook}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

</br>

## Introduzione {#intro}

Questo documento descrive i flussi di lavoro per l&#39;adesione che l&#39;applicazione di livello superiore di un programmatore può implementare tramite le API esposte dalla libreria Android AccessEnabler.


La soluzione di autenticazione Adobe Pass per Android è infine suddivisa in due domini:

- Dominio dell’interfaccia utente: livello applicativo superiore che implementa l’interfaccia utente e utilizza i servizi forniti dalla libreria AccessEnabler per fornire accesso a contenuti con restrizioni.
- Dominio AccessEnabler: qui vengono implementati i flussi di lavoro per l’adesione sotto forma di:
   - Chiamate di rete effettuate ai server back-end di Adobe
   - Regole della logica di business relative ai flussi di lavoro di autenticazione e autorizzazione
   - Gestione di varie risorse ed elaborazione dello stato del flusso di lavoro (ad esempio la cache dei token)

L&#39;obiettivo del dominio AccessEnabler è nascondere tutte le complessità dei flussi di lavoro di adesione e fornire all&#39;applicazione di livello superiore (tramite la libreria AccessEnabler) un set di semplici primitive di adesione con cui implementare i flussi di lavoro di adesione:

1. Imposta l&#39;identità del richiedente.

1. Verifica e ottieni l’autenticazione per un determinato provider di identità.

1. Controlla e ottieni l’autorizzazione per una particolare risorsa.

1. Disconnetti.

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

     Attivato da `setRequestor()`, restituisce esito positivo o negativo.\
     Il successo indica che puoi procedere con le chiamate di adesione.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

     Attivato da `getAuthentication()` solo se l&#39;utente non ha selezionato un provider (MVPD) e non è ancora autenticato.\
     Il parametro `mvpds` è un array di provider disponibili per l&#39;utente.

   - [&quot;setAuthenticationStatus(status, errorcode)&quot;](#$setAuthNStatus)

     Attivato da `checkAuthentication()` ogni volta.\
     Attivato da `getAuthentication()` solo se l&#39;utente è già autenticato e ha selezionato un provider.

     Lo stato restituito è success o failure, il codice di errore descrive il tipo di errore.

   - [navigateToUrl(url)](#$navigateToUrl)

     Attivazione eseguita da `getAuthentication()` dopo la selezione di un MVPD da parte dell&#39;utente. Il parametro `url` fornisce il percorso della pagina di accesso di MVPD.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

     Attivato da `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.\
     Il parametro `event` indica quale evento di adesione si è verificato; il parametro `data` è un elenco di valori relativi all&#39;evento.

   - [&quot;setToken(token, resource)&quot;](#$setToken)

     Attivazione eseguita da `checkAuthorization()` e `getAuthorization()` dopo un&#39;autorizzazione di visualizzazione di una risorsa completata.\
     Il parametro `token` è il token multimediale di breve durata. Il parametro `resource` è il contenuto che l&#39;utente è autorizzato a visualizzare.

   - [&quot;tokenRequestFailed(resource, code, description)&quot;](#$tokenRequestFailed)

     Attivato da `checkAuthorization()` e `getAuthorization()` dopo un&#39;autorizzazione non riuscita.\
     Il parametro `resource` è il contenuto che l&#39;utente stava tentando di visualizzare. Il parametro `code` è il codice di errore che indica il tipo di errore che si è verificato. Il parametro `description` descrive l&#39;errore associato al codice di errore.

   - [&quot;selectedProvider(mvpd)&quot;](#$selectedProvider)

     Attivato da `getSelectedProvider()`.\
     Il parametro `mvpd` fornisce informazioni sul provider selezionato dall&#39;utente.

   - [&quot;setMetadataStatus(metadata, key, arguments)&quot;](#$setMetadataStatus)

     Attivato da `getMetadata().`\
     Il parametro `metadata` fornisce i dati specifici richiesti; il parametro `key` è la chiave utilizzata nella richiesta `getMetadata()` e il parametro `arguments` è lo stesso dizionario passato a `getMetadata()`.

   - [&quot;preauthorizedResources(resources)&quot;](#$preauthResources)

     Attivato da `checkPreauthorizedResources()`.\
     Il parametro `authorizedResources` presenta le risorse che l&#39;utente è autorizzato a visualizzare.


![](../../../../assets/android-entitlement-flows.png)


### B. Flusso di avvio {#startup_flow}

1. Avvia l&#39;applicazione di livello superiore.
1. Avvia autenticazione Adobe Pass

   a. Chiamare [`getInstance`](#$getInstance) per creare una singola istanza di Adobe Pass Authentication AccessEnabler.

   - **Dipendenza:** Autenticazione Adobe Pass Nativa
Libreria Android (AccessEnabler)

   b. Chiamare ` setRequestor()` per stabilire l&#39;identità del programmatore; passare `requestorID` del programmatore e (facoltativamente) un array di endpoint di autenticazione Adobe Pass.

   - **Dipendenza:** ID richiedente autenticazione Adobe Pass valido\
     Per risolvere il problema, rivolgiti al tuo Adobe Pass Authentication Account Manager.

   - **Trigger:** callback setRequestorComplete()

   | NOTA |     |
   | --- | --- |  
   | ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/icons/1313859077_lightbulb.png) | Non è possibile completare alcuna richiesta di adesione finché non viene stabilita l&#39;identità completa del richiedente. Ciò significa che mentre setRequestor() è ancora in esecuzione, tutte le richieste di adesione successive (ad esempio, `checkAuthentication()`) sono bloccate.<br><br>Sono disponibili due opzioni di implementazione: una volta inviate le informazioni di identificazione del richiedente al server back-end, il livello dell&#39;applicazione dell&#39;interfaccia utente può scegliere uno dei due approcci seguenti:<br><br>1.  Attendere l&#39;attivazione del callback `setRequestorComplete()` (parte del delegato AccessEnabler).  Questa opzione fornisce la massima certezza che `setRequestor()` è stato completato, quindi è consigliata per la maggior parte delle implementazioni.<br>2.  Continuare senza attendere l&#39;attivazione del callback `setRequestorComplete()` e iniziare a emettere richieste di adesione. Queste chiamate (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) sono in coda dalla libreria AccessEnabler, che effettuerà le chiamate di rete effettive dopo `setRequestor(). `Questa opzione può essere interrotta occasionalmente se, ad esempio, la connessione di rete è instabile. |

1. Chiamare [checkAuthentication()](#$checkAuthN) per verificare la presenza di un&#39;autenticazione esistente senza avviare il flusso di autenticazione completo.   Se la chiamata ha esito positivo, puoi passare direttamente al flusso di autorizzazione.  In caso contrario, passare al flusso di autenticazione.

   - **Dipendenza:** Chiamata riuscita a `setRequestor()` (questa dipendenza si applica anche a tutte le chiamate successive).

   - **Trigger:** callback setAuthenticationStatus()

### C. Flusso di autenticazione {#authn_flow}

1. Chiamare [`getAuthentication()`](#$getAuthN) per avviare il flusso di autenticazione o per ottenere la conferma che l&#39;utente è già autenticato.\
   **Trigger:**
   - Il callback setAuthenticationStatus(), se l&#39;utente è già autenticato.  In questo caso, passare direttamente al [Flusso di autorizzazione](#authz_flow).
   - Il callback displayProviderDialog(), se l&#39;utente non è ancora autenticato.

1. Presentare all&#39;utente l&#39;elenco dei provider inviati a `displayProviderDialog()`.

1. Dopo che l&#39;utente ha selezionato un provider, ottenere l&#39;URL del MVPD dell&#39;utente dal callback `navigateToUrl()`.  Aprire un controllo WebView e indirizzarlo all&#39;URL.

1. Tramite il WebView creato nel passaggio precedente, l&#39;utente arriva alla pagina di accesso di MVPD e immette le credenziali di accesso. Diverse operazioni di reindirizzamento si verificano all&#39;interno di WebView.

   **Nota:** A questo punto, l&#39;utente ha la possibilità di annullare il flusso di autenticazione. In questo caso, il livello dell&#39;interfaccia utente è responsabile dell&#39;informazione di AccessEnabler su questo evento chiamando `setSelectedProvider()` con `null` come parametro. Ciò consente ad AccessEnabler di ripulire il proprio stato interno e reimpostare il flusso di autenticazione.

1. Dopo che l&#39;utente ha effettuato l&#39;accesso, il livello dell&#39;applicazione rileva il caricamento di un &quot;URL di reindirizzamento personalizzato&quot; (ovvero `http://adobepass.android.app`). Questo URL personalizzato è in realtà un URL non valido che non deve essere caricato da WebView. È un segnale che indica che il flusso di autenticazione è stato completato e che WebView deve essere chiuso.

1. Chiudere il controllo WebView e chiamare `getAuthenticationToken()`, che indica ad AccessEnabler di recuperare il token di autenticazione dal server back-end.

1. [Facoltativo] Chiamare [`checkPreauthorizedResources(resources)`](#$checkPreauth) per verificare quali risorse l&#39;utente è autorizzato a visualizzare. Il parametro `resources` è un array di risorse protette associate al token di autenticazione dell&#39;utente.\
   **Trigger:** `preAuthorizedResources()` callback\
   **Punto di esecuzione:** dopo il completamento del flusso di autenticazione

1. Se l’autenticazione ha esito positivo, procedi al flusso di autorizzazione.



### D. Flusso di autorizzazione {#authz_flow}

1. Chiama [getAuthorization()](#$getAuthZ) per avviare l&#39;autorizzazione
flusso.

   Dipendenza: ID risorsa validi concordati con gli MVPD.

   **Nota:** i ResourceID devono essere gli stessi utilizzati su qualsiasi altro dispositivo o piattaforma e saranno gli stessi in tutti gli MVPD.

1. Convalida autenticazione e autorizzazione.

   - Se la chiamata `getAuthorization()` ha esito positivo: l&#39;utente dispone di token AuthN e AuthZ validi (l&#39;utente è autenticato e autorizzato a guardare il contenuto multimediale richiesto).
   - Se `getAuthorization()` ha esito negativo: esaminare l&#39;eccezione generata per determinarne il tipo (AuthN, AuthZ o altro):
      - In caso di errore di autenticazione (AuthN), riavvia il flusso di autenticazione.
      - Se si trattava di un errore di autorizzazione (AuthZ), l’utente non è autorizzato a guardare il contenuto multimediale richiesto e deve visualizzare all’utente un qualche tipo di messaggio di errore.
      - Se si è verificato un altro tipo di errore (errore di connessione, errore di rete, ecc.), visualizza un messaggio di errore appropriato.

1. Convalida il token multimediale breve.\
   Utilizza la libreria Adobe Pass Authentication Media Token Verifier per verificare il token multimediale di breve durata restituito dalla chiamata `getAuthorization()` precedente:

   - Se la convalida ha esito positivo: riproduci il file multimediale richiesto per l’utente.
   - Se la convalida non riesce: il token AuthZ non è valido, la richiesta di supporto deve essere rifiutata e l’utente deve visualizzare un messaggio di errore.

1. Tornare al normale flusso dell&#39;applicazione.

### E. Visualizza flusso multimediale {#media_flow}

1. L’utente seleziona il file multimediale da visualizzare.
2. Il supporto è protetto?  L&#39;applicazione verifica se il supporto selezionato è protetto:
- Se il supporto selezionato è protetto, l&#39;applicazione avvia il [flusso di autorizzazione](#authz_flow).
- Se il supporto selezionato non è protetto, riprodurlo per l&#39;utente.



### F. Flusso di disconnessione {#logout_flow}

1. Chiamare [`logout()`](#$logout) per disconnettersi.\
   AccessEnabler cancella tutti i valori e i token memorizzati nella cache per il MVPD corrente per il richiedente corrente e anche per i richiedenti con Single Sign On. Dopo aver cancellato la cache, AccessEnabler effettua una chiamata al server per pulire le sessioni lato server.  Poiché la chiamata al server potrebbe causare un reindirizzamento SAML all’IdP (consentendo la pulizia della sessione sul lato IdP), questa chiamata deve seguire tutti i reindirizzamenti. Per questo motivo, è necessario gestire questa chiamata all&#39;interno di un controllo WebView.

   a. Seguendo lo stesso modello del flusso di lavoro di autenticazione, il dominio AccessEnabler invia una richiesta al livello applicazione dell&#39;interfaccia utente (tramite il callback `navigateToUrl()`) per creare un controllo WebView e istruire tale controllo a caricare l&#39;URL dell&#39;endpoint di disconnessione sul server back-end.

   b. Di nuovo, l&#39;interfaccia utente deve monitorare l&#39;attività del controllo WebView e rilevare il momento in cui il controllo, durante diversi reindirizzamenti, carica l&#39;URL personalizzato dell&#39;applicazione (ovvero `http://adobepass.android.app/`). Una volta eseguito questo evento, il livello dell&#39;applicazione dell&#39;interfaccia utente chiude WebView e il processo di disconnessione viene completato.

   **Nota:** il flusso di disconnessione è diverso dal flusso di autenticazione in quanto l&#39;utente non è tenuto a interagire in alcun modo con WebView. Il livello dell’applicazione UI utilizza una visualizzazione web per assicurarsi che tutti i reindirizzamenti siano seguiti. È quindi possibile (e consigliato) rendere il controllo WebView invisibile (ovvero nascosto) durante il processo di logout.



### Flussi di utenti per l’accesso con più MVPD e disconnessione {#user_flows}

[Qui](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AndroidSSOUserFlows.pdf) hai un documento che descrive il comportamento quando utilizzi più MVPD e cosa accade quando l&#39;utente si disconnette da un&#39;applicazione.

Il comportamento descritto è disponibile quando si utilizza la versione SDK di Android >= 2.0.0.
