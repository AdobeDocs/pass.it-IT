---
title: SDK per Android con registrazione client dinamica
description: SDK per Android con registrazione client dinamica
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 0%

---

# SDK per Android con registrazione client dinamica {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#Intro}

L’SDK Android AccessEnabler per Android è stato modificato per abilitare l’autenticazione senza l’utilizzo dei cookie di sessione. Poiché sempre più browser limitano l’accesso ai cookie, è necessario utilizzare un altro metodo per consentire l’autenticazione.

Per Android, l’utilizzo delle schede personalizzate di Chrome limita l’accesso ai cookie di altre applicazioni.

>**Android SDK 3.0.0** introduce:

- dynamic client registration sostituisce l&#39;attuale meccanismo di registrazione dell&#39;app basato sull&#39;autenticazione con ID richiedente firmato e cookie di sessione
- Schede personalizzate Chrome per i flussi di autenticazione

>[!NOTE]
>
>Per le versioni Android precedenti senza il supporto delle schede personalizzate di Chrome, verrà utilizzata l’autenticazione WebView simile a quella delle versioni precedenti dell’SDK AccessEnabler.


## Registrazione client dinamici {#DCR}

Android SDK v3.0+ utilizzerà la procedura di registrazione del client dinamico definita in [Registrazione client dinamici](/help/authentication/dynamic-client-registration.md).


## Demo sulle funzioni {#Demo}

Osserva [questo webinar](https://my.adobeconnect.com/pzkp8ujrigg1/) che fornisce più contesto della funzione e contiene una demo su come gestire le istruzioni software utilizzando TVE Dashboard e come testare quelle generate utilizzando un’applicazione demo fornita da Adobe come parte dell’SDK per Android.

## Modifiche API {#API}


### Factory.getInstance

**Descrizione:** Crea istanze dell&#39;oggetto Access Enabler. Deve essere presente una singola istanza di Access Enabler per ogni istanza dell&#39;applicazione.

| Chiamata API: costruttore |
| --- |
| getInstance(Context appContext, String softwareStatement, String redirectUrl) statica pubblica di AccessEnabler<br>        genera AccessEnablerException |


**Disponibilità:** v3.0+

**Parametri:**

- *appContext*: contesto dell’applicazione Android
- softwareStatement: valore ottenuto da TVE Dashboard o *nulle* se &quot;software\_statement&quot; è impostato in strings.xml
- redirectUrl : URL univoco, uno dei domini in ordine inverso aggiunto in modo esplicito nel dashboard TVE oppure *nulle* se &quot;redirect\_uri&quot; è impostato in stringhe.xml

Nota: un softwareStatement o redirectUrl non valido impedirà all&#39;applicazione di inizializzare AccessEnabler o di registrare l&#39;applicazione per l&#39;autenticazione e l&#39;autorizzazione di Adobe Pass
</br>
Nota: il parametro redirectUrl o redirect\_uri in strings.xml deve essere il valore del dominio aggiunto nel dashboard TVE per l’applicazione in ordine inverso ( per esempio, per il dominio &quot;adobe.com&quot; aggiunto nel dashboard TVE, il redirectUrl deve essere &quot;com.adobe&quot;.


### setRequestor

**Descrizione:** Stabilisce l’identità del canale. A ciascun canale viene assegnato un ID univoco al momento della registrazione all’Adobe per il sistema di autenticazione di Adobe Pass. Quando si tratta di token SSO e remoti, lo stato di autenticazione può cambiare quando l&#39;applicazione è in background, setRequestor può essere richiamato nuovamente quando l&#39;applicazione viene portata in primo piano per sincronizzarsi con lo stato del sistema (recuperare un token remoto se SSO è abilitato o eliminare il token locale se nel frattempo si è verificato un logout).

La risposta del server contiene un elenco di MVPD insieme ad alcune informazioni di configurazione associate all’identità del canale. La risposta del server viene utilizzata internamente dal codice di Access Enabler. Solo lo stato dell’operazione (ovvero SUCCESS/FAIL) viene presentato all’applicazione tramite il callback setRequestorComplete().

Se il *url* non viene utilizzato, la chiamata di rete risultante esegue il targeting dell’URL predefinito del fornitore di servizi: l’ambiente Adobe Release/Production.

Se per il campo *url* , la chiamata di rete risultante esegue il targeting di tutti gli URL forniti nel *url* parametro. Tutte le richieste di configurazione vengono attivate contemporaneamente in thread separati. Il primo responder ha la precedenza quando si compila l’elenco degli MVPD. Per ogni MVPD nell&#39;elenco, Access Enabler ricorda l&#39;URL del provider di servizi associato. Tutte le richieste di adesione successive vengono indirizzate all’URL associato al provider di servizi che è stato associato all’MVPD di destinazione durante la fase di configurazione.

| Chiamata API: configurazione richiedente |
| --- |
| ```public void setRequestor(String requestorId)``` |

**Disponibilità:** v3.0+

| Chiamata API: configurazione richiedente |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Disponibilità:** v3.0+

**Parametri:**

- *requestorID*: ID univoco associato al canale. Trasmetti l’ID univoco assegnato da Adobe al sito quando ti sei registrato per la prima volta con il servizio di autenticazione di Adobe Pass.
- *url*: parametro facoltativo; per impostazione predefinita, viene utilizzato il provider di servizi Adobe [http://sp.auth.adobe.com/](http://sp.auth.adobe.com/). Questo array consente di specificare gli endpoint per i servizi di autenticazione e autorizzazione forniti da Adobe (a scopo di debug possono essere utilizzate istanze diverse). È possibile utilizzare questa opzione per specificare più istanze del provider di servizi di autenticazione di Adobe Pass. In tal caso, l&#39;elenco MVPD è composto dagli endpoint di tutti i fornitori di servizi. Ogni MVPD è associato al provider di servizi più veloce, ovvero il provider che ha risposto per primo e che supporta tale MVPD.

Obsoleto:

- *signedRequestorID*: copia dell’ID richiedente con firma digitale nella chiave privata. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Callback attivati:** `setRequestorComplete()`

### logout

**Descrizione:** Utilizzare questo metodo per avviare il flusso di logout. La disconnessione è il risultato di una serie di operazioni di reindirizzamento HTTP dovute al fatto che l’utente deve essere disconnesso sia dai server di autenticazione di Adobe Pass che dai server MVPD. Di conseguenza, questo flusso aprirà una finestra ChromeCustomTab per eseguire la disconnessione.

| Chiamata API: avvia il flusso di logout |
| --- |
| public void logout() |

**Disponibilità:** v3.0+

**Parametri:** Nessuno

**Callback attivati:** `setAuthenticationStatus()`
</br></br>

## Flusso di implementazione del programmatore {#Progr}

### **1. Registra applicazione**

a. Ottenere il software\_statement e il reindirizzamento\_uri da Adobe Pass ( Dashboard TVE )

b. Esistono due opzioni per trasmettere questi valori all’SDK di Adobe Pass:

In strings.xml aggiungi:

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

Chiama AccessEnabler.getInstance(appContext,softwareStatement, redirectUrl)


### 2. Configurare l’applicazione

a. setRequestor(requestor\_id)

L’SDK eseguirà le seguenti operazioni:

- registra applicazione: utilizzando **software\_statement**, l’SDK otterrà un **client\_id, client\_secret, client\_id\_issued\_at, redirect\_uris, grant\_types**. Queste informazioni verranno archiviate nell&#39;archivio interno dell&#39;applicazione.

- ottenere un **access\_token** utilizzando client\_id, client\_secret e grant\_type=&quot;client\_credentials&quot; . Questo accesso\_token verrà utilizzato a ogni chiamata effettuata dall&#39;SDK ai server Adobe Pass

**Risposte di errore token:**

| Risposte di errore | | |
| --- | --- | --- |
| HTTP 400 (richiesta non valida) | {&quot;error&quot;: &quot;invalid\_request&quot;} | Nella richiesta manca un parametro obbligatorio, include un valore di parametro non supportato (diverso dal tipo di concessione), ripete un parametro, include più credenziali, utilizza più meccanismi per l’autenticazione del client o ha un formato non valido. |
| HTTP 400 (richiesta non valida) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Autenticazione client non riuscita. Client sconosciuto. L’SDK DEVE registrarsi nuovamente con il server autorizzazioni. |
| HTTP 400 (richiesta non valida) | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | Il client autenticato non è autorizzato a utilizzare questo tipo di concessione di autorizzazione. |

- nel caso in cui un MVPD richieda l’autenticazione passiva, si aprirà una scheda personalizzata di Chrome per eseguire operazioni passive con tale MVPD e si chiuderà una volta completato

b. checkAuthentication()

- true : vai a Autorizzazione
- false : vai a Seleziona MVPD

c. getAuthentication: l&#39;SDK includerà **access_token** nei parametri di chiamata

- mvpd memorizzato: vai a setSelectedProvider(mvpd_id)
- mvpd non selezionato: displayProviderDialog
- mvpd selezionato: vai a setSelectedProvider(mvpd_id)

d. setSelectedProvider

- L&#39;URL di autenticazione mvpd\_id è caricato in ChromeCustomTabs
- accesso riuscito : delegate.setAuthenticationStatus ( SUCCESS )
- accesso annullato: reimpostare la selezione MVPD
- Lo schema URL viene impostato come &quot;adobepass://redirect_uri&quot; da acquisire al termine dell’autenticazione

e. get/checkAuthorization : l’SDK includerà **access_token** nell’intestazione come Autorizzazione: Bearer **access_token**

- se l’autorizzazione viene rilasciata, verrà effettuata una chiamata per ottenere il token multimediale

f. logout:

- L&#39;SDK eliminerà il token valido per il richiedente corrente (le autenticazioni ottenute da altre applicazioni e non tramite SSO rimarranno valide)
- L’SDK aprirà le schede personalizzate di Chrome per raggiungere l’endpoint mvpd_id logout. Una volta completate, le schede personalizzate di Chrome verranno chiuse
- Lo schema URL viene impostato come &quot;adobepass://logout&quot; per acquisire il momento in cui viene completato il logout
- la disconnessione attiverà sendTrackingData(new Event(EVENT_LOGOUT,USER_NOT_AUTHENTICATED_ERROR) e un callback : setAuthenticationStatus(0,&quot;Logout&quot;)

**Nota:** poiché ogni chiamata richiede un **access_token,** i possibili codici di errore seguenti vengono gestiti nell’SDK.


| Risposte di errore | | |
| --- | ---|--- |
| invalid_request | 400 | Richiesta non valida. L’SDK deve interrompere l’esecuzione delle chiamate al server. |
| invalid_client | 403 | L’ID client non è più autorizzato a eseguire richieste. L&#39;SDK DEVE eseguire nuovamente la registrazione client. |
| accesso negato | 401 | Accesso\_token non valido. L’sdk DEVE richiedere un nuovo access_token. |
