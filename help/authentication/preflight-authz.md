---
title: Autorizzazione di verifica preliminare
description: Autorizzazione di verifica preliminare
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# Autorizzazione di verifica preliminare {#preflight-authorization}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>

## Panoramica {#overview}

Questa funzione fornisce un controllo di autorizzazione semplificato per più risorse. Lo scopo di questo controllo leggero è decorare l’interfaccia utente (ad esempio, indicando lo stato di accesso con icone di blocco e sblocco). L’autorizzazione di verifica preliminare è il più semplice ed efficiente possibile, in modo che una singola chiamata API generi lo stato di autorizzazione per un elenco di risorse. Tieni presente che questa funzione non è autorevole per quanto riguarda l’autorizzazione di una risorsa.

È ancora NECESSARIO effettuare una chiamata `getAuthorization(resource)` o `checkAuthorization(resource)` prima di consentire la riproduzione.

L’autorizzazione di verifica preliminare fornisce anche il supporto per un diverso caso d’uso, in cui il programmatore deve richiedere l’autorizzazione per più ID di risorsa per consentire la riproduzione di un elemento del contenuto multimediale. Il Programmatore può effettuare un controllo preliminare iniziale sulle risorse richieste e, a seconda della risposta, potrebbe non riuscire in anticipo se le condizioni di business non vengono soddisfatte.

Per un elenco di MVPD che supportano l&#39;autorizzazione di verifica preliminare, vedere la pagina [Autorizzazione di verifica preliminare MVPD](/help/authentication/mvpd-preflight-authz.md#preflight_support_list).

>[!NOTE]
>
> L&#39;utilizzo di questa funzionalità per gli MVPD che non dispongono del supporto completo per l&#39;autorizzazione di verifica preliminare deve essere concordato in anticipo con Adobe e MVPD. L&#39;utilizzo dell&#39;autorizzazione di verifica preliminare per questi MVPD rientra nello &quot;scenario peggiore&quot; descritto [qui](/help/authentication/mvpd-preflight-authz.md#intro) e può causare problemi di prestazioni e rallentamento del tempo di risposta. </br>
> Inoltre, tieni presente che l&#39;utilizzo dell&#39;autorizzazione di verifica preliminare con **più di 5 risorse deve essere esplicitamente accettato dall&#39;Adobe**.

## API di verifica preliminare di AccessEnabler {#AE_pre_api}

AccessEnabler espone una coppia di funzioni API/callback per implementare l&#39;autorizzazione di verifica preliminare. La chiamata API accetta un singolo parametro costituito da un elenco di risorse. La funzione di callback accetta un singolo parametro che rappresenta le risorse autorizzate effettive. Gli esempi seguenti sono in ActionScript, ma le chiamate sono disponibili in tutte le versioni di AccessEnabler.

### checkPreauthorizedResources(Array:resources):void {#checkPreauthRes}

Chiamare questa funzione sull&#39;oggetto AccessEnabler per richiedere lo stato di autorizzazione per un elenco di risorse.

Il parametro resources è l&#39;elenco delle risorse per le quali deve essere controllata l&#39;autorizzazione. Ogni elemento dell’elenco deve essere una stringa che rappresenta l’ID della risorsa. L&#39;ID risorsa è soggetto alle stesse limitazioni dell&#39;ID risorsa nella chiamata `getAuthorization()`, ovvero al valore concordato tra il Programmatore e l&#39;MVPD, o un frammento RSS del supporto. Tieni presente che l’autenticazione Adobe Pass non gestisce le risorse in alcun modo, ad eccezione di un livello di mediazione sottile che può trasformare i formati delle risorse in base a ciò che MVPD supporta effettivamente.

### preauthorizedResources(Array:authorizedResources) {#preauthRes}

Si tratta di una funzione di callback che deve essere implementata nell&#39;applicazione di livello superiore del programmatore. AccessEnabler chiamerà questa funzione dopo aver calcolato l&#39;elenco delle risorse autorizzate.


### Esempio JS

```javascript
    checkPreauthorizedResources(["CNBC","MSNBC"]);
    ...
    preauthorizedResources() {
        var resource = arguments[0];
        for(i in resources) {
            // Do things with resource list
            // such as decorate UI
            ...
        }
    }
```

## Informazioni sull’implementazione {#details}

- [Verifica preliminare tramite ChannelID](#preflight_using_channelID)
- [POST/Preauthorize](#post)
- [Storage](#storage)

La chiamata API tenta di trovare un elenco memorizzato nella cache delle risorse autorizzate per l’utente corrente nell’archiviazione locale del client. Se non è presente alcun elenco memorizzato in cache, viene effettuata una chiamata HTTPS ai server AdobePass per recuperare l’elenco.

Il meccanismo di caching migliora i tempi di prestazione nelle chiamate successive saltando completamente la chiamata di rete. Inoltre, l’elenco memorizzato in cache può essere popolato in anticipo come parte del processo di autenticazione.  (Per informazioni sulla configurazione di questo scenario, vedere [Integrazione autorizzazione verifica preliminare](/help/authentication/authz-usecase.md#preflight_authz_int) nella sezione Autorizzazione della Guida all&#39;integrazione MVPD).

Inoltre, l&#39;elenco delle risorse memorizzate nella cache può essere utilizzato per ottimizzare il flusso di autorizzazione, nel senso che se esiste un elenco di risorse memorizzato nella cache, `checkAuthorization()` può controllarlo prima di eseguire una chiamata di rete. Se la risorsa non è presente nell’elenco delle risorse preautorizzate, il controllo può non riuscire senza dover chiamare i server di autenticazione di Adobe Pass.


### Verifica preliminare tramite ChannelID {#preflight_using_channelID}

A partire dalla versione di Adobe Pass Authentication 2.4.1, il flusso di verifica preliminare funziona come segue:

1. Durante l&#39;autenticazione, Adobe Pass Authentication legge l&#39;elemento `channelIID` dalla risposta SAML di MVPD e utilizza questo valore per impostare l&#39;elemento `authorizedResources` nel token di autenticazione.
1. All&#39;interno della funzione API `checkPreauthorizedResources()`, l&#39;autenticazione di Adobe Pass verifica se l&#39;elemento `authorizedResources` è impostato.
1. Se l&#39;elemento `authorizedResources` è impostato, Adobe Pass Authentication legge tale valore ed esegue un&#39;intersezione tra l&#39;elenco delle risorse dall&#39;elemento `authorizedResources` e l&#39;elenco delle risorse ricevute dal parametro `checkPreauthorizedResources()`.  Il risultato di questa intersezione è l’elenco finale delle risorse preautorizzate.
1. Se l&#39;elemento `authorizedResources` non è impostato, eseguire il flusso implementato in precedenza, in cui l&#39;elenco delle risorse ricevute dal parametro `checkPreauthorizedResources()` viene passato a PreAuthorizationServlet. Questo servlet esegue le chiamate di autorizzazione agli endpoint MVPD e restituisce l’elenco delle risorse preautorizzate.

### Esempio di verifica preliminare con ChannelID

L’esempio seguente mostra un esempio di allineamento del canale. Tieni presente che il nome dell’attributo utilizzato per inviare l’elenco dei canali può differire da un MVPD all’altro:

```XML
    <saml:Attribute Name="visible_channels" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
        <saml:AttributeValue xsi:type="xs:string">MSNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FBN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FNC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TNT</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TBS</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TRUTV</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TOON</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">HBO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">MAX</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">EPIXHD</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">BTN-BTN2GO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">SPEED-SPEED2</saml:AttributeValue>
    </saml:Attribute>
```


L&#39;elemento `authorizedResources` dell&#39;elemento di autenticazione viene visualizzato come segue:

```JSON
    <authorizedResources>
        <authorizedResource resourceID="MSNBC"/>
        <authorizedResource resourceID="CNBC"/>
        <authorizedResource resourceID="FBN"/>
        <authorizedResource resourceID="FNC"/>
        <authorizedResource resourceID="TNT"/>
        <authorizedResource resourceID="TBS"/>
        <authorizedResource resourceID="CNN"/>
        <authorizedResource resourceID="TRUTV"/>
        <authorizedResource resourceID="TOON"/>
        <authorizedResource resourceID="HBO"/>
        <authorizedResource resourceID="MAX"/>
        <authorizedResource resourceID="EPIXHD"/>
        <authorizedResource resourceID="BTN-BTN2GO"/>
        <authorizedResource resourceID="SPEED-SPEED2"/>
    </authorizedResources>
```

Il programmatore esegue la chiamata API `checkPreauthorizedResources()`, passando il seguente elenco di parametri:</span>

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;
- fbc-fox

L&#39;implementazione corrente della verifica preliminare esegue l&#39;intersezione con l&#39;elenco di risorse dell&#39;elemento `authorizedResources` e restituisce questo elenco:

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;



**Nota:** l&#39;intersezione non distingue tra maiuscole e minuscole.



### POST/Preauthorize {#post}

Questa chiamata verrà eseguita automaticamente da AccessEnabler se non esiste alcun elenco di risorse memorizzato nella cache, autorizzato e.


#### Richiesta {#req}

| Parametro | Tipo | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| `authentication_token` | stringa | SÌ | Token di autenticazione. |
| `resource_id` | stringa | SÌ | Una singola risorsa. Questa impostazione può essere specificata più volte, una volta per ogni elemento dell&#39;array di risorse fornito nella chiamata API `checkPreauthorizedResources()`. |


**Nota:** è possibile configurare il numero massimo di risorse richieste.
**Il valore predefinito massimo è 5.**


#### Risposta {#resp}

La risposta inviata dal servlet di preautorizzazione ha il seguente formato:

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <resources>
      <resource>
        <id>TNT</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>TBS</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>CNN</id>
        <authorized>false</authorized>
      </resource>
    </resources>
```

### Storage {#storage}

Elenco di risorse preautorizzate ottenute da AccessEnabler dal provider di servizi. Questo elenco di risorse:

- Viene memorizzato insieme ai token AuthN e AuthZ
- È valido finché l’utente si trova sullo stesso sito web o fino a quando
Scadenza token di autenticazione
- Viene recuperato nuovamente ogni volta che l’utente arriva a una nuova Adobe Pass
sito Web integrato di autenticazione

Ogni voce contiene l&#39;ID risorsa per il quale l&#39;utente è preautorizzato.

Ad esempio:


| ID risorsa | Autorizzato |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


Questo elenco è denominato &quot;cache di preautorizzazione&quot;.

#### Flusso {#flow}

1. L&#39;app/sito del programmatore effettua una chiamata `checkPreauthorizedResources(resourceList)`.
1. AccessEnabler verifica il token di autenticazione per le risorse autorizzate:
   1. Se il token di autenticazione contiene risorse autorizzate: questo elenco è autorevole e non deve essere effettuata alcuna chiamata per ottenere queste informazioni. Le risorse da resourceList vengono cercate nell&#39;elenco delle risorse autorizzate nel token di autenticazione e solo quelle trovate vengono restituite dal callback `preauthorizedResources()`.
   1. Se il token di autenticazione NON contiene risorse autorizzate, `resourceList` viene confrontato con l&#39;elenco di risorse nella cache di preautorizzazione.
      1. Se l’elenco contiene le stesse risorse, significa che è già stata effettuata una chiamata al server e che la risposta si trova già nella cache di preautorizzazione. Solo le risorse autorizzate verranno restituite dal callback `preauthorizedResources()`.
      1. Se l&#39;elenco NON contiene le stesse risorse, il client deve effettuare una chiamata al server per ottenere lo stato di autorizzazione per le risorse in resourceList. La risposta verrà recuperata e memorizzata nella cache di preautorizzazione, sostituendo completamente le risorse precedenti. Solo le risorse autorizzate verranno restituite dal callback `preauthorizedResources()`.


#### Recupero elenco {#listRetrieve}

Ogni volta che si chiama un `checkPreauthorizedResources()`, l&#39;elenco delle risorse da verificare per l&#39;autorizzazione viene controllato rispetto alla cache di preautorizzazione. Se l&#39;elenco contiene lo stesso set di risorse, non verrà effettuata alcuna chiamata al provider di servizi, poiché tutte le risorse necessarie per attivare il callback `preauthorizedResources()` sono già nella cache.


#### logout() {#logout}

La cache di preautorizzazione viene svuotata al momento della disconnessione.


## Dipendenze {#depends}

Le prestazioni dell’API di verifica preliminare dipendono da implementazioni MVPD specifiche.  Per le opzioni di implementazione, vedere [Integrazione autorizzazione verifica preliminare](/help/authentication/authz-usecase.md#preflight_authz_int) nella sezione Autorizzazione della Guida all&#39;integrazione MVPD.


## Sicurezza {#security}

Le API client sono disponibili per tutti i programmatori.

L’implementazione utilizza HTTPS come trasporto, ma per avere una chiamata più leggera non vengono impiegate misure di sicurezza aggiuntive (nessuna firma, nessun FAXS).

**Attenzione:** NON utilizzare questa API in modo autorevole per determinare se a un utente deve essere concesso l&#39;accesso a una risorsa protetta. Lo scopo di questa API è la decorazione dell’interfaccia utente e/o la verifica preliminare delle decisioni aziendali. Le chiamate `getAuthorization()` e `checkAuthorization()` devono sempre essere effettuate prima di consentire la riproduzione.


## Compatibilità {#compat}

Questa funzione è supportata in tutte le versioni di AccessEnabler: AS, JS, AIR, iOS, Android, Xbox (nel flusso AuthN del secondo schermo).

L&#39;autorizzazione di verifica preliminare non supporta le risorse di pre-autorizzazione che contengono sezioni CDATA. Lo scopo del sistema di verifica preliminare corrente è di supportare il filtro a livello di canale. Le risorse con sezioni CDATA sono probabilmente risorse a livello di risorsa. La verifica preliminare supporta risorse `mrss` semplici per la pre-autorizzazione a livello di canale, purché non contengano CDATA.

## Integrazione Con Altre Funzioni {#integ_w_other_features}

È possibile che nel flusso di autorizzazione si verifichi una possibile ottimizzazione per avere esito negativo rapidamente se la risorsa non è inclusa nell’elenco delle risorse preautorizzate.

In questa ottimizzazione, l’endpoint di verifica preliminare del server si integra con il meccanismo di degradazione.

Se è presente una regola &quot;AuthN All&quot; per MVPD e Requestor, l’endpoint di verifica preliminare eseguirà semplicemente il mirroring di tutte le risorse ricevute nella richiesta.

Lo stesso vale se &quot;AuthZ All&quot; è abilitato per almeno una delle risorse presenti nella richiesta.

<!--
## Related Information

  - `checkPreauthorizedResources()`
      - [iOS](#checkPreauth)
      - [Android](#checkPreauth)
      - [JavaScript](#checkPreauthRes)
      - [ActionScript](#checkPreauthRes)
  - [Xbox](#2nd%20Screen%20Authentication)
  - [MVPD Integration Guide: Preflight AuthZ](#)
-->
