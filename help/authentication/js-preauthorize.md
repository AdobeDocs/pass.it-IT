---
title: Autorizza in anticipo
description: JavaScript preauthorize
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1499'
ht-degree: 0%

---

# Autorizza in anticipo {#js-preauthorize}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#preauth-overview}

Il metodo API Preauthorize deve essere utilizzato dalle applicazioni per ottenere decisioni di preautorizzazione per una o più risorse. La richiesta API di pre-autorizzazione deve essere utilizzata per gli hint dell’interfaccia utente e/o per il filtro dei contenuti. È necessario effettuare una richiesta API di autorizzazione effettiva prima di consentire l’accesso degli utenti alle risorse specificate.

Nel caso in cui si verifichi un errore imprevisto (ad esempio, un problema di rete e l’endpoint di autorizzazione MVPD non disponibile) quando una richiesta API di preautorizzazione viene elaborata dai servizi di autenticazione di Adobe Pass, verranno incluse una o più informazioni di errore separate per le risorse interessate come parte del risultato della risposta API di preautorizzazione.

### public preauthorize(request: PreauthorizeRequest, callback: AccessEnablerCallback&lt;any>): void {#preauth-method}

**Descrizione:** Questo metodo deve essere utilizzato dalle applicazioni per ottenere le decisioni di preautorizzazione (informative) dell’utente autenticato dal servizio di autenticazione di Adobe Pass per visualizzare risorse protette specifiche, allo scopo principale di decorare l’interfaccia utente dell’applicazione (ad esempio, indicando lo stato di accesso con le icone di blocco e sblocco).

**Disponibilità:** v4.4.0+

**Parametri:**

* `PreauthorizeRequest`: oggetto Builder utilizzato per definire la richiesta
* `AccessEnablerCallback`: callback utilizzato per restituire la risposta API
* `PreauthorizeResponse`: oggetto utilizzato per restituire il contenuto della risposta API

### class PreauthorizeRequestBuilder {#preath-req-builder-class}

#### setResources(resources: string[]): PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* Imposta l&#39;elenco delle risorse per le quali si desidera ottenere le decisioni di preautorizzazione.
* È obbligatorio impostarlo per l’utilizzo dell’API di preautorizzazione.
* Ogni elemento nell’elenco deve essere un valore String che rappresenta il valore ID della risorsa o il frammento RSS del file multimediale che deve essere concordato con l’MVPD.
* Questo metodo imposta le informazioni solo nel contesto del `PreauthorizeRequestBuilder` istanza dell&#39;oggetto, che è il destinatario di questa chiamata del metodo.

* Per generare un valore effettivo `PreauthorizeRequest` puoi dare un&#39;occhiata a `PreauthorizeRequestBuilder`metodo di:

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` risorse. L’elenco delle risorse per le quali desideri ottenere decisioni di preautorizzazione.
* `@returns {PreauthorizeRequestBuilder}` Il riferimento allo stesso `PreauthorizeRequestBuilder` istanza dell&#39;oggetto, che è il ricevitore della chiamata del metodo.
* Questo consente di creare il concatenamento dei metodi.

#### disableFeatures(...features: stringa[]): PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* Imposta le funzionalità che si desidera disabilitare quando si ottengono decisioni di preautorizzazione.
* Questa funzione imposta le informazioni solo nel contesto corrente. `PreauthorizeRequestBuilder` istanza oggetto, che è il ricevitore di questa chiamata di funzione.
* Per generare un valore effettivo `PreauthorizeRequest` puoi dare un&#39;occhiata a `PreauthorizeRequestBuilder`funzione di:

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}` funzioni. L&#39;insieme di funzionalità per le quali si desidera disattivarle.
* `@returns` Il riferimento allo stesso `PreauthorizeRequestBuilder` istanza oggetto, che è il ricevitore della chiamata di funzione.
* Lo fa per consentire la creazione di concatenamento di funzioni.

#### build(): PreauthorizeRequest {#preauth-req}

* Crea e recupera il riferimento di un nuovo `PreauthorizeRequest` istanza oggetto.
* Questo metodo crea un&#39;istanza di un nuovo `PreauthorizeRequest` ogni volta che viene chiamato.
* Questo metodo utilizza i valori impostati in precedenza nel contesto del `PreauthorizeRequestBuilder` istanza dell&#39;oggetto, che è il destinatario di questa chiamata del metodo.
* Tenere presente che questo metodo non produce effetti collaterali,
* pertanto, non modifica lo stato dell’SDK o dello stato dell’SDK. `PreauthorizeRequestBuilder` istanza dell&#39;oggetto, che è il destinatario di questa chiamata del metodo.
* Significa che le chiamate successive di questo metodo per lo stesso ricevitore creeranno nuove `PreauthorizeRequest` ma con le stesse informazioni, nel caso in cui i valori siano impostati su `PreauthorizeRequestBuilder` dove non viene modificato tra le chiamate di.
* Nel caso in cui non sia necessario aggiornare alcuna delle informazioni fornite (risorse e caching), è possibile riutilizzare l’istanza PreauthorizeRequest per più utilizzi dell’API preauthorize.
* `@returns {PreauthorizeRequest}`

### interfaccia AccessEnablerCallback&lt;t> {#interface-access-enablr-callback}

#### onResponse(risultato: T); {#on-response-result}

* Callback di risposta chiamato dall&#39;SDK quando la richiesta API di preautorizzazione è stata soddisfatta.
* Il risultato è un risultato positivo o un risultato di errore contenente uno stato.
* `@param {T} result`

#### onFailure(risultato: T); {#on-failure-result}

* Errore di callback chiamato dall&#39;SDK quando non è stato possibile soddisfare la richiesta API di preautorizzazione.
* Il risultato è un risultato di errore contenente uno stato.
* `@param {T} result`

### class PreauthorizeResponse {#preauth-response-class}

#### stato pubblico: status; {#public-status}

* Restituisce: informazioni aggiuntive sullo stato (stato) in caso di errore.
* Potrebbe contenere un `null` valore.

#### decisioni pubbliche: decisione[]; {#public-decisions}

* Restituisce: l’elenco delle decisioni di preautorizzazione. Una decisione per ogni risorsa.
* L’elenco potrebbe essere vuoto in caso di errore.

### Stato classe {#class-status}

#### stato pubblico: numero; {#public-status-numbr}

* Il codice di stato della risposta HTTP come documentato nella RFC 7231.
* Può essere 0 nel caso in cui `Status` proviene dall’SDK anziché dai servizi di autenticazione di Adobe Pass.

#### codice pubblico: numero; {#public-code-numbr}

* Codice di errore standard dei servizi di autenticazione di Adobe Pass.
* Potrebbe contenere una stringa vuota o un `null` valore.

#### messaggio pubblico: string; {#public-msg-string}

* Il messaggio dettagliato che in alcuni casi viene fornito dagli endpoint di autorizzazione MVPD o dalle regole di degradazione del programmatore.
* Potrebbe contenere una stringa vuota o un `null` valore.

#### dettagli pubblici: string; {#public-details-strng}

* Contiene un messaggio dettagliato che in alcuni casi è fornito dagli endpoint di autorizzazione MVPD o dalle regole di degradazione del programmatore.
* Potrebbe contenere una stringa vuota o un `null` valore.


#### helpUrl pubblico: string; {#public-help-url-string}

* L’URL che rimanda a ulteriori informazioni sul motivo di questo stato/errore e sulle possibili soluzioni.
* Potrebbe contenere una stringa vuota o un `null` valore.

#### traccia pubblica: string; {#public-trace-string}

* L’identificatore univoco di questa risposta, che può essere utilizzato quando si contatta il supporto per identificare problemi specifici in scenari più complessi.
* Potrebbe contenere una stringa vuota o un `null` valore.

#### azione pubblica: string; {#public-action-string}

* Azione consigliata per risolvere la situazione.
   * **nessuno**: purtroppo non è presente alcuna azione predefinita per risolvere questo problema. Ciò potrebbe indicare una chiamata non corretta dell’API pubblica
   * **configurazione**: è necessaria una modifica della configurazione tramite il dashboard TVE o contattando il supporto.
   * **registrazione dell&#39;applicazione**: l’applicazione deve registrarsi nuovamente.
   * **autenticazione**: l’utente deve autenticare o autenticare di nuovo.
   * **autorizzazione**: l’utente deve ottenere l’autorizzazione per la risorsa specifica.
   * **degradazione**: deve essere applicata una qualche forma di degradazione.
   * **riprova**: un nuovo tentativo di richiesta potrebbe risolvere il problema
   * **riprova dopo**: un nuovo tentativo di richiesta dopo il periodo di tempo indicato potrebbe risolvere il problema.
* Potrebbe contenere una stringa vuota o un `null` valore.

### class Decision {#class-decision}

#### id pubblico: string; {#public-id-string}

* ID risorsa per cui è stata ottenuta la decisione.

#### public authorized: booleano; {#public-auth-boolean}

* Valore del flag che indica se la decisione è stata eseguita con successo o meno.

#### errore pubblico: stato; {#public-error-status}

* Informazioni aggiuntive sullo stato (stato) nel caso si sia verificato un errore. Potrebbe contenere un `null` valore.

## Esempio di implementazione client {#client-imp-example}

```JavaScript
let accessEnablerApi = new window.AccessEnabler.AccessEnabler("software statement");
let accessEnablerModels = window.AccessEnabler.models;



// Build request
let requestBuilder = new accessEnablerModels.PreauthorizeRequest.getBuilder();
let request = requestBuilder
    .setResources(["RES01", "RES02", "RES03"])
    .disableFeatures("LOCAL_CACHE")
    .build();



// Create callback
let callback = {
    onResponse(response) {
        // Handle onResponse
    },
    onFailure(response) {
        // Handle onFailure
    }
};

// Invoke call
accessEnablerApi.preauthorize(request, callback);
```


## Esempi di scenari {#scenario-examples}

### Scenario 1: tutte le risorse richieste sono state autorizzate {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>Flag di codice di errore avanzato</th>
    <th>Risposta</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabilitato</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": true
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }    
```

</td>
  </tr>
</tbody>


### Scenario 2: alcune risorse richieste sono state autorizzate. {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>Flag di codice di errore avanzato</th>
    <th>Risposta</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabilitato</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;decisioni&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: true
    }
    ]
    }
    
    &quot;

</td>
  </tr>

<tr>
    <td>Abilitato</td>
    <td>

    &quot;JavaScript
    {
    &quot;decisioni&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorized_denso_by_mvpd&quot;,
    &quot;messaggio&quot;: &quot;MVPD ha restituito una decisione \&quot;Nega\&quot; durante la richiesta di pre-autorizzazione per la risorsa specificata.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: true
    },
    ]
    }
    
    &quot;

</td>
  </tr>
</tbody>


### Scenario 3: nessuna delle risorse richieste è stata autorizzata. {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>Flag di codice di errore avanzato</th>
    <th>Risposta</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabilitato</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;decisioni&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: false
    }
    ]
    }
    
    &quot;

</td>
  </tr>

<tr>
    <td>Abilitato</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;decisioni&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorized_denso_by_mvpd&quot;,
    &quot;messaggio&quot;: &quot;MVPD ha restituito una decisione \&quot;Nega\&quot; durante la richiesta di pre-autorizzazione per la risorsa specificata.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorized_denso_by_mvpd&quot;,
    &quot;messaggio&quot;: &quot;MVPD ha restituito una decisione \&quot;Nega\&quot; durante la richiesta di pre-autorizzazione per la risorsa specificata.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;maximum_execution_time_exceeded&quot;,
    &quot;message&quot;: &quot;La richiesta non è stata completata entro il tempo massimo consentito. Un nuovo tentativo di richiesta potrebbe risolvere il problema.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot; (riprova)
    }
    }
    ]
    }
    
    &quot;

</td>
  </tr>
</tbody>


### Scenario 4: richiesta client non valida. Nessuna risorsa specificata. {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>Flag di codice di errore avanzato</th>
    <th>Risposta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Disabilitato/abilitato</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 400,
    &quot;code&quot;: &quot;internal_error&quot;,
    &quot;message&quot;: &quot;Richiesta non riuscita a causa di un errore interno.&quot;,
    &quot;details&quot;: &quot;Il parametro obbligatorio String[] &#39;resource&#39; non è presente&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;decisioni&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Scenario 5: richiesta client non valida. Sono state specificate risorse vuote. {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>Flag di codice di errore avanzato</th>
    <th>Risposta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Disabilitato/abilitato</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 412,
    &quot;code&quot;: &quot;missing_resource&quot;,
    &quot;message&quot;: &quot;Parametro della risorsa mancante&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;decisioni&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Scenario 6: errore di rete. {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>Flag di codice di errore avanzato</th>
    <th>Risposta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Abilitato</td>
    <td>

    &quot;JavaScript
    {
    &quot;decisioni&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;Errore di lettura durante il recupero della risposta dal servizio partner associato. Un nuovo tentativo di richiesta potrebbe risolvere il problema.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot; (riprova)
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;Errore di lettura durante il recupero della risposta dal servizio partner associato. Un nuovo tentativo di richiesta potrebbe risolvere il problema.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot; (riprova)
    }
    }
    ]
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Scenario 7: il flusso di preautorizzazione è stato richiamato senza una sessione AuthN valida.

<table>
<thead>
  <tr>
    <th>Flag di codice di errore avanzato</th>
    <th>Risposta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Disabilitato/abilitato</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;authentication_session_missing&quot;,
    &quot;message&quot;: &quot;Impossibile recuperare la sessione di autenticazione associata a questa richiesta. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato.&quot;,
    &quot;action&quot;: &quot;authentication&quot; (autenticazione)
    },
    &quot;decisioni&quot;: []
    }
    
    &quot;

</td>
  </tr>
</tbody>
</table>



### Scenario 8: il flusso di preautorizzazione è stato richiamato prima del completamento della chiamata setRequestor

<table>
<thead>
  <tr>
    <th>Flag di codice di errore avanzato</th>
    <th>Risposta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Disabilitato/abilitato</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;requestor_not_configured&quot;,
    &quot;message&quot;: &quot;Il richiedente non è ancora configurato, il che è un prerequisito per l’utilizzo di qualsiasi API diversa dall’API setRequestor.&quot;,
    &quot;action&quot;: &quot;retry&quot; (riprova)
    },
    &quot;decisioni&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>
