---
title: Autorizza in anticipo
description: JavaScript preautorizza
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1488'
ht-degree: 0%

---

# (Legacy) Preautorizza {#js-preauthorize}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Panoramica {#preauth-overview}

Il metodo API Preauthorize deve essere utilizzato dalle applicazioni per ottenere decisioni di preautorizzazione per una o più risorse. La richiesta API di pre-autorizzazione deve essere utilizzata per gli hint dell’interfaccia utente e/o per il filtro dei contenuti. È necessario effettuare una richiesta API di autorizzazione effettiva prima di consentire l’accesso degli utenti alle risorse specificate.

Nel caso in cui si verifichi un errore imprevisto (ad esempio, un problema di rete e l’endpoint di autorizzazione MVPD non disponibile) quando una richiesta API di preautorizzazione viene elaborata dai servizi di autenticazione di Adobe Pass, verranno incluse una o più informazioni di errore separate per le risorse interessate come parte del risultato della risposta API di preautorizzazione.

### public preauthorize(request: PreauthorizeRequest, callback: AccessEnablerCallback&lt;any>): void {#preauth-method}

**Descrizione:** Questo metodo deve essere utilizzato dalle applicazioni per ottenere le decisioni di preautorizzazione (informative) dell&#39;utente autenticato dal servizio di autenticazione di Adobe Pass per visualizzare risorse protette specifiche, allo scopo principale di decorare l&#39;interfaccia utente dell&#39;applicazione (ad esempio, per indicare lo stato di accesso con le icone di blocco e sblocco).

**Disponibilità:** v4.4.0+

**Parametri:**

* `PreauthorizeRequest`: oggetto Builder utilizzato per definire la richiesta
* `AccessEnablerCallback`: callback utilizzato per restituire la risposta API
* `PreauthorizeResponse`: oggetto utilizzato per restituire il contenuto della risposta API

### class PreauthorizeRequestBuilder {#preath-req-builder-class}

#### setResources(resources: string[]): PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* Imposta l&#39;elenco delle risorse per le quali si desidera ottenere le decisioni di preautorizzazione.
* È obbligatorio impostarlo per l’utilizzo dell’API di preautorizzazione.
* Ogni elemento nell’elenco deve essere un valore String che rappresenta il valore ID della risorsa o il frammento RSS del file multimediale che deve essere concordato con MVPD.
* Questo metodo imposta le informazioni solo nel contesto dell&#39;istanza dell&#39;oggetto `PreauthorizeRequestBuilder` corrente, che è il destinatario di questa chiamata al metodo.

* Per generare un `PreauthorizeRequest` effettivo, puoi dare un&#39;occhiata al metodo di `PreauthorizeRequestBuilder`:

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` risorse. L’elenco delle risorse per le quali desideri ottenere decisioni di preautorizzazione.
* `@returns {PreauthorizeRequestBuilder}` Riferimento alla stessa istanza dell&#39;oggetto `PreauthorizeRequestBuilder`, che è il destinatario della chiamata al metodo.
* Questo consente di creare il concatenamento dei metodi.

#### disableFeatures(...features: string[]): PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* Imposta le funzionalità che si desidera disabilitare quando si ottengono decisioni di preautorizzazione.
* Questa funzione imposta le informazioni solo nel contesto dell&#39;istanza dell&#39;oggetto `PreauthorizeRequestBuilder` corrente, che è il destinatario di questa chiamata di funzione.
* Per generare un `PreauthorizeRequest` effettivo, puoi dare un&#39;occhiata alla funzione di `PreauthorizeRequestBuilder`:

```JavaScript
public func build() -> PreauthorizeRequest
```

* Funzionalità di `@param {string[]}`. L&#39;insieme di funzionalità per le quali si desidera disattivarle.
* `@returns` Il riferimento alla stessa istanza dell&#39;oggetto `PreauthorizeRequestBuilder`, che è il destinatario della chiamata di funzione.
* Lo fa per consentire la creazione di concatenamento di funzioni.

#### build(): PreauthorizeRequest {#preauth-req}

* Crea e recupera il riferimento di una nuova istanza dell&#39;oggetto `PreauthorizeRequest`.
* Questo metodo crea un&#39;istanza di un nuovo oggetto `PreauthorizeRequest` ogni volta che viene chiamato.
* Questo metodo utilizza i valori impostati in precedenza nel contesto dell&#39;istanza dell&#39;oggetto `PreauthorizeRequestBuilder` corrente, che è il destinatario di questa chiamata del metodo.
* Tenere presente che questo metodo non produce effetti collaterali,
* pertanto, non modifica lo stato di SDK o lo stato dell&#39;istanza dell&#39;oggetto `PreauthorizeRequestBuilder`, che è il destinatario di questa chiamata di metodo.
* Ciò significa che le chiamate successive di questo metodo per lo stesso ricevitore creeranno nuove istanze di oggetto `PreauthorizeRequest` diverse, ma con le stesse informazioni, nel caso in cui i valori impostati su `PreauthorizeRequestBuilder` non vengano modificati tra le chiamate.
* Nel caso in cui non sia necessario aggiornare alcuna delle informazioni fornite (risorse e caching), è possibile riutilizzare l’istanza PreauthorizeRequest per più utilizzi dell’API preauthorize.
* `@returns {PreauthorizeRequest}`

### interfaccia AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse(risultato: T); {#on-response-result}

* Callback di risposta chiamato da SDK quando è stata soddisfatta la richiesta API di preautorizzazione.
* Il risultato è un risultato positivo o un risultato di errore contenente uno stato.
* `@param {T} result`

#### onFailure(risultato: T); {#on-failure-result}

* Errore di callback chiamato da SDK quando non è stato possibile soddisfare la richiesta API di preautorizzazione.
* Il risultato è un risultato di errore contenente uno stato.
* `@param {T} result`

### class PreauthorizeResponse {#preauth-response-class}

#### stato pubblico: status; {#public-status}

* Restituisce: informazioni aggiuntive sullo stato (stato) in caso di errore.
* Potrebbe contenere un valore `null`.

#### decisioni pubbliche: Decisione[]; {#public-decisions}

* Restituisce: l’elenco delle decisioni di preautorizzazione. Una decisione per ogni risorsa.
* L’elenco potrebbe essere vuoto in caso di errore.

### Stato classe {#class-status}

#### stato pubblico: numero; {#public-status-numbr}

* Il codice di stato della risposta HTTP come documentato nella RFC 7231.
* Potrebbe essere 0 nel caso in cui `Status` provenga dal servizio SDK anziché dai servizi di autenticazione di Adobe Pass.

#### codice pubblico: numero; {#public-code-numbr}

* Codice di errore standard dei servizi di autenticazione di Adobe Pass.
* Potrebbe contenere una stringa vuota o un valore `null`.

#### messaggio pubblico: string; {#public-msg-string}

* Il messaggio dettagliato che in alcuni casi viene fornito dagli endpoint di autorizzazione di MVPD o dalle regole di degradazione del programmatore.
* Potrebbe contenere una stringa vuota o un valore `null`.

#### dettagli pubblici: string; {#public-details-strng}

* Contiene un messaggio dettagliato che in alcuni casi viene fornito dagli endpoint di autorizzazione di MVPD o dalle regole di degradazione del programmatore.
* Potrebbe contenere una stringa vuota o un valore `null`.


#### helpUrl pubblico: string; {#public-help-url-string}

* L’URL che rimanda a ulteriori informazioni sul motivo di questo stato/errore e sulle possibili soluzioni.
* Potrebbe contenere una stringa vuota o un valore `null`.

#### traccia pubblica: string; {#public-trace-string}

* L’identificatore univoco di questa risposta, che può essere utilizzato quando si contatta il supporto per identificare problemi specifici in scenari più complessi.
* Potrebbe contenere una stringa vuota o un valore `null`.

#### azione pubblica: string; {#public-action-string}

* Azione consigliata per risolvere la situazione.
   * **none**: nessuna azione predefinita per risolvere il problema. Ciò potrebbe indicare una chiamata non corretta dell’API pubblica
   * **configurazione**: è necessario modificare la configurazione tramite il dashboard TVE o contattando il supporto.
   * **registrazione-applicazione**: l&#39;applicazione deve registrarsi nuovamente.
   * **autenticazione**: l&#39;utente deve autenticare o autenticare di nuovo.
   * **autorizzazione**: l&#39;utente deve ottenere l&#39;autorizzazione per la risorsa specifica.
   * **degradazione**: applicare una qualche forma di degradazione.
   * **riprova**: un nuovo tentativo di richiesta potrebbe risolvere il problema
   * **riprova dopo**: il problema potrebbe essere risolto ritentando la richiesta dopo il periodo di tempo indicato.
* Potrebbe contenere una stringa vuota o un valore `null`.

### class Decision {#class-decision}

#### id pubblico: string; {#public-id-string}

* ID risorsa per cui è stata ottenuta la decisione.

#### public authorized: booleano; {#public-auth-boolean}

* Valore del flag che indica se la decisione è stata eseguita con successo o meno.

#### errore pubblico: stato; {#public-error-status}

* Informazioni aggiuntive sullo stato (stato) nel caso si sia verificato un errore. Potrebbe contenere un valore `null`.

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
    &quot;Decisions&quot;: [
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
    &quot;Decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_rejected_by_mvpd&quot;,
    &quot;message&quot;: &quot;Il MVPD ha restituito una decisione \&quot;Deny\&quot; pre-autorizzazione per la risorsa specificata.&quot;,
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
    &quot;Decisions&quot;: [
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
    &quot;Decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_rejected_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPD ha restituito una decisione \&quot;Deny\&quot; quando si richiede la pre-autorizzazione per la risorsa specificata.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_deny_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPD ha restituito una decisione \&quot;Deny\&quot; durante la richiesta di pre-autorizzazione per la risorsa specificata.&quot;,
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
    &quot;message&quot;: &quot;La richiesta non è stata completata entro il tempo massimo consentito. Un nuovo tentativo della richiesta potrebbe risolvere il problema.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
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
    &quot;details&quot;: &quot;Parametro stringa[] obbligatorio &#39;resource&#39; non presente&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;Decisions&quot;: []
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
    &quot;Decisions&quot;: []
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
    &quot;Decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;Errore di lettura durante il recupero della risposta dal servizio partner associato. Un nuovo tentativo di richiesta potrebbe risolvere il problema.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;Errore di lettura durante il recupero della risposta dal servizio partner associato. Un nuovo tentativo della richiesta potrebbe risolvere il problema.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
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
    &quot;message&quot;: &quot;Impossibile recuperare la sessione di autenticazione associata a questa richiesta. Per continuare, l&#39;utente deve ripetere l&#39;autenticazione con un MVPD supportato.&quot;,
    &quot;action&quot;: &quot;authentication&quot;
    },
    &quot;Decisions&quot;: []
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
    &quot;message&quot;: &quot;Il richiedente non è ancora configurato, il che è un prerequisito per l&#39;utilizzo di qualsiasi API a parte l&#39;API setRequestor.&quot;,
    &quot;action&quot;: &quot;retry&quot;
    },
    &quot;Decisions&quot;: []
    
    &quot;

</td>
  </tr>
</tbody>
</table>
