---
title: SDK Amazon FireOS con registrazione client dinamica
description: SDK Amazon FireOS con registrazione client dinamica
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1145'
ht-degree: 0%

---

# SDK Amazon FireOS con registrazione client dinamica {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>

## <span id=""></span>Introduzione {#Intro}

L’SDK di FireOS AccessEnabler per FireTV è stato modificato per abilitare l’autenticazione senza utilizzare i cookie di sessione. Poiché sempre più browser limitano l’accesso ai cookie, era necessario un altro metodo per consentire l’autenticazione.

**FireOS SDK 3.0.4** sostituisce il meccanismo di registrazione dell&#39;app corrente basato sull&#39;ID richiedente firmato e sull&#39;autenticazione dei cookie di sessione con [Registrazione client dinamica](/help/authentication/dynamic-client-registration.md).


## Modifiche API {#API}

### Factory.getInstance

**Descrizione:** crea istanze dell&#39;oggetto Access Enabler. Deve essere presente una singola istanza di Access Enabler per ogni istanza dell&#39;applicazione.

| Chiamata API: costruttore |
| --- |
| getInstance(Context appContext, String softwareStatement, String redirectUrl)<br> statico pubblico di AccessEnabler        genera AccessEnablerException |

**Disponibilità:** v3.0+

**Parametri:**

- *appContext*: contesto dell&#39;applicazione Android
- *softwareStatement*: valore ottenuto dal dashboard TVE o *null* se &quot;software\_statement&quot; è impostato in stringhe.xml
- *redirectUrl*: per le implementazioni FireTV questo parametro deve essere null. Qualsiasi impostazione su questo attributo verrà ignorata.

**Note**

- softwareStatement non valido impedirà all&#39;applicazione di inizializzare AccessEnabler o di registrare l&#39;applicazione per l&#39;autenticazione e l&#39;autorizzazione di Adobe Pass
- Il parametro redirectUrl per FireTV è impostato dall’SDK su adobepass://android.app poiché l’autenticazione è gestita da un’istanza AccessEnabler univoca.

### setRequestor

**Descrizione:** stabilisce l&#39;identità del canale. A ciascun canale viene assegnato un ID univoco al momento della registrazione all’Adobe per il sistema di autenticazione di Adobe Pass. Quando si tratta di token SSO e remoti, lo stato di autenticazione può cambiare quando l&#39;applicazione è in background, setRequestor può essere richiamato nuovamente quando l&#39;applicazione viene portata in primo piano per sincronizzarsi con lo stato del sistema (recuperare un token remoto se SSO è abilitato o eliminare il token locale se nel frattempo si è verificato un logout).

La risposta del server contiene un elenco di MVPD insieme ad alcune informazioni di configurazione associate all’identità del canale. La risposta del server viene utilizzata internamente dal codice di Access Enabler. Solo lo stato dell’operazione (ovvero SUCCESS/FAIL) viene presentato all’applicazione tramite il callback setRequestorComplete().

Se il parametro *urls* non viene utilizzato, la chiamata di rete risultante esegue il targeting dell&#39;URL del provider di servizi predefinito: l&#39;ambiente di produzione Adobe Release.

Se viene fornito un valore per il parametro *urls*, la chiamata di rete risultante eseguirà il targeting di tutti gli URL forniti nel parametro *urls*. Tutte le richieste di configurazione vengono attivate contemporaneamente in thread separati. Il primo responder ha la precedenza quando si compila l’elenco degli MVPD. Per ogni MVPD nell&#39;elenco, Access Enabler ricorda l&#39;URL del provider di servizi associato. Tutte le richieste di adesione successive vengono indirizzate all’URL associato al provider di servizi che è stato associato all’MVPD di destinazione durante la fase di configurazione.

| Chiamata API: configurazione richiedente |
| --- |
| void pubblico setRequestor(String requestorId) |

**Disponibilità:** v3.0+

| Chiamata API: configurazione richiedente |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Disponibilità:** v3.0+

**Parametri:**

- *requestorID*: ID univoco associato al canale. La prima volta che ti registri al servizio di autenticazione di Adobe Pass, passa l’ID univoco assegnato dall’Adobe al tuo sito.
- *url*: parametro facoltativo; per impostazione predefinita, viene utilizzato il provider di servizi Adobe (http://sp.auth.adobe.com/). Questo array consente di specificare gli endpoint per i servizi di autenticazione e autorizzazione forniti da Adobe (a scopo di debug possono essere utilizzate istanze diverse). È possibile utilizzare questa opzione per specificare più istanze del provider di servizi di autenticazione di Adobe Pass. In tal caso, l&#39;elenco MVPD è composto dagli endpoint di tutti i fornitori di servizi. Ogni MVPD è associato al provider di servizi più veloce, ovvero il provider che ha risposto per primo e che supporta tale MVPD.

Obsoleto:

- *signedRequestorID*: copia dell&#39;ID richiedente con firma digitale nella chiave privata. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Callback attivati:** `setRequestorComplete()`

</br>

### logout

**Descrizione:** Utilizzare questo metodo per avviare il flusso di disconnessione. La disconnessione è il risultato di una serie di operazioni di reindirizzamento HTTP dovute al fatto che l’utente deve essere disconnesso sia dai server di autenticazione di Adobe Pass che dai server MVPD. Di conseguenza, questo flusso aprirà una finestra ChromeCustomTab per eseguire la disconnessione.

| Chiamata API: avvia il flusso di logout |
| --- |
| public void logout() |

**Disponibilità:** v3.0+

**Parametri:** Nessuno

**Callback attivati:** `setAuthenticationStatus()`

## Flusso di implementazione del programmatore {#Progr}

### **1. Registra applicazione**

1. Ottenere il software\_statement da Adobe Pass ( Dashboard TVE )
1. Esistono due opzioni per trasmettere questi valori all’SDK di Adobe Pass:
   - In strings.xml aggiungi:

     ```
     <string name>"software\_statement">[softwarestatement value]</string>
     ```

   - Chiama AccessEnabler.getInstance(appContext,softwareStatement, null)



### **2. Configura applicazione**

- a. setRequestor(requestor\_id)

  L’SDK eseguirà le seguenti operazioni:

   - registra applicazione: utilizzando **software\_statement**, l&#39;SDK otterrà un **client\_id, client\_secret, client\_id\_issued\_at, redirect\_uris, grant\_types**. Queste informazioni verranno archiviate nell&#39;archivio interno dell&#39;applicazione.
   - ottieni un **accesso\_token** utilizzando client\_id, client\_secret e grant\_type=&quot;client\_credentials&quot; . Questo accesso\_token verrà utilizzato a ogni chiamata effettuata dall&#39;SDK ai server Adobe Pass.

| Risposte di errore token: |  |  |
|--- | --- | --- |
| HTTP 400 (richiesta non valida) | {&quot;error&quot;: &quot;invalid\_request&quot;} | Nella richiesta manca un parametro obbligatorio, include un valore di parametro non supportato (diverso dal tipo di concessione), ripete un parametro, include più credenziali, utilizza più meccanismi per l’autenticazione del client o ha un formato non valido. |
| HTTP 400 (richiesta non valida) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Autenticazione client non riuscita. Client sconosciuto. L&#39;SDK *DEVE* registrarsi nuovamente con il server autorizzazioni. |
| HTTP 400 (richiesta non valida) | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | Il client autenticato non è autorizzato a utilizzare questo tipo di concessione di autorizzazione. |

- nel caso in cui un MVPD richieda l&#39;autenticazione passiva, WebView si aprirà per l&#39;esecuzione passiva con tale MVPD e si chiuderà una volta completato

- b. checkAuthentication()

   - *true*: vai ad autorizzazione
   - *false* : vai a Seleziona MVPD

- c. getAuthentication: l&#39;SDK includerà **access_token** nei parametri di chiamata

   - mvpd memorizzato: vai a setSelectedProvider(mvpd\_id)
   - mvpd non selezionato: displayProviderDialog
   - mvpd selezionato: vai a setSelectedProvider(mvpd\_id)

- d. setSelectedProvider

   - L&#39;URL di autenticazione mvpd\_id è caricato in ChromeCustomTabs
   - accesso riuscito : delegate.setAuthenticationStatus ( SUCCESS )
   - accesso annullato: reimpostare la selezione MVPD
   - Lo schema URL viene impostato come &quot;adobepass://android.app&quot; da acquisire al termine dell’autenticazione

- e. get/checkAuthorization : l&#39;SDK includerà **access\_token **nell&#39;intestazione come Authorization: Bearer **access\_token**

- se l’autorizzazione viene rilasciata, verrà effettuata una chiamata per ottenere il token multimediale

- f. logout:

   - L&#39;SDK eliminerà il token valido per il richiedente corrente (le autenticazioni ottenute da altre applicazioni e non tramite SSO rimarranno valide)
   - L&#39;SDK aprirà le schede personalizzate di Chrome per raggiungere l&#39;endpoint mvpd\_id logout. Al termine, le schede personalizzate Chrome verranno chiuse
   - Lo schema URL viene impostato come &quot;adobepass://logout&quot; per acquisire il momento in cui viene completato il logout
   - la disconnessione attiva sendTrackingData(new Event(EVENT\_LOGOUT,USER\_NOT\_AUTHENTICATED\_ERROR) e un callback : setAuthenticationStatus(0,&quot;Logout&quot;)



**Nota:** poiché ogni chiamata richiede un **access_token**, i possibili codici di errore seguenti vengono gestiti nell&#39;SDK.

| Risposte di errore |  |  |
|--- | --- | --- |
| invalid_request | 400 | Richiesta non valida. L’SDK deve interrompere l’esecuzione delle chiamate al server. |
| invalid_client | 403 | L’ID client non è più autorizzato a eseguire richieste. L&#39;SDK DEVE eseguire nuovamente la registrazione client. |
| accesso negato | 401 | Access_token non valido. L’sdk DEVE richiedere un nuovo access_token. |
