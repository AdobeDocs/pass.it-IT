---
title: Autorizzazione di verifica preliminare MVPD
description: Autorizzazione di verifica preliminare MVPD
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 0%

---

# Autorizzazione di verifica preliminare MVPD

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#mvpd-preflight-authz-intro}

&quot;Autorizzazione di verifica preliminare&quot; è un controllo di autorizzazione semplificato per più risorse. I programmatori lo utilizzano principalmente per decorare le loro UI (ad esempio, indicando lo stato di accesso con le icone di blocco e sblocco).

Al momento l’autenticazione Adobe Pass supporta la verifica preliminare delle autorizzazioni per gli MVPD in due modi, tramite gli attributi di risposta AuthN o una richiesta AuthZ multicanale.  I seguenti scenari descrivono il rapporto costi/benefici dei diversi modi in cui è possibile implementare l&#39;autorizzazione di verifica preliminare:

* **Scenario migliore** - MVPD fornisce l&#39;elenco delle risorse preautorizzate durante la fase di autorizzazione (Multi-channel AuthZ).
* **Scenario del caso peggiore** - Se un MVPD non supporta alcuna forma di autorizzazione per più risorse, il server di autenticazione di Adobe Pass esegue una chiamata di autorizzazione al MVPD per ogni risorsa nell&#39;elenco delle risorse. Questo scenario ha un impatto (proporzionale al numero di risorse) sul tempo di risposta per la richiesta di autorizzazione di verifica preliminare. Può aumentare il carico sui server Adobe e MVPD causando problemi di prestazioni. Inoltre, genera richieste di autorizzazione/eventi di risposta senza l’effettiva necessità di un gioco.
* **Obsoleto** - MVPD fornisce l&#39;elenco delle risorse preautorizzate durante la fase di autenticazione, pertanto non saranno necessarie chiamate di rete, nemmeno la richiesta di verifica preliminare, poiché l&#39;elenco è memorizzato nella cache del client.

Sebbene gli MVPD non debbano supportare l’autorizzazione di verifica preliminare, le sezioni seguenti descrivono alcuni metodi di autorizzazione supportati dall’autenticazione di Adobe Pass prima di ricorrere allo scenario più sfavorevole descritto sopra.

## Verifica preliminare in Autenticazione {#preflight-authn}

Questa verifica preliminare è compatibile con OLCA (Cableab). La sezione 7.5.2 delle specifiche di Authentication and Authorization Interface 1.0, intitolata &quot;Attribute Statement Within Authentication Assertion&quot;, descrive come una risposta di autenticazione SAML possa contenere un elenco di risorse preautorizzate. Se un IdP supporta questa impostazione, il server di autenticazione di Adobe Pass sarà in grado di generare l’elenco delle risorse predefinite al momento dell’autenticazione e di memorizzarlo nella cache del client insieme al token di autenticazione. Questo metodo offre anche lo scenario migliore e non verranno eseguite chiamate di rete quando il programmatore chiama checkPreauthorizedResources(), poiché tutto si trova già sul client.

### Elenco risorse personalizzato nell&#39;istruzione dell&#39;attributo SAML {#custom-res-saml-attr}

La risposta di autenticazione SAML dell&#39;IdP deve includere un AttributeStatement contenente i nomi delle risorse che AdobePass deve autorizzare.  Alcuni MVPD forniscono questo nel seguente formato:

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

L’esempio precedente presenta un elenco contenente due risorse preautorizzate: &quot;MMOD&quot; e &quot;Olympics2012&quot;.

In questo modo viene raggiunto lo scenario migliore e non verranno eseguite chiamate di rete quando il programmatore chiama checkPreauthorizedResources(), in quanto tutto si trova già sul client.

## Verifica preliminare multicanale in AuthZ {#preflight-multich-authz}

Questa implementazione preliminare è anche compatibile OLCA (Cablelab).  Le specifiche dell&#39;interfaccia di autenticazione e autorizzazione 1.0 (sezioni 7.5.3 e 7.5.4) descrivono i metodi per la richiesta di informazioni di autorizzazione da un MVPD utilizzando le asserzioni SAML o XACML. Questo è il metodo consigliato per eseguire query sullo stato di autorizzazione per gli MVPD che non lo supportano come parte del flusso di autenticazione. L’autenticazione di Adobe Pass invia una singola chiamata di rete al MVPD per recuperare l’elenco delle risorse autorizzate.


Adobe Pass Authentication riceve l&#39;elenco delle risorse dall&#39;applicazione del programmatore. L’integrazione MVPD dell’autenticazione di Adobe Pass può quindi effettuare una chiamata AuthZ includendo tutte queste risorse, analizzare la risposta ed estrarre le decisioni relative a più autorizzazioni/negazioni.  Il flusso per la verifica preliminare con lo scenario AuthZ multicanale funziona come segue:

1. L’app del programmatore invia un elenco separato da virgole di risorse tramite l’API client di verifica preliminare, ad esempio: &quot;TestChannel1,TestChannel2,TestChannel3&quot;.
1. La chiamata di richiesta AuthZ di verifica preliminare di MVPD contiene più risorse e presenta la seguente struttura:

```XML
<?xml version="1.0" encoding="UTF-8"?><soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/"> 
<soap11:Header/> 
<soap11:Body> 
  <xacml-samlp:XACMLAuthzDecisionQuery xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol" 
                                       CombinePolicies="false" Destination="https://login.idpexmaple.net/" ID="_3576604f382455d6495f342d9e07b69c" 
                                       IssueInstant="2013-02-07T10:31:40.333Z" Version="2.0"> 
  <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com/on-behalf-of/TestDistributors</saml2:Issuer> 
  <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os"> 
  <xacml-context:Subject SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject"> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VFZTAQEAABQCe[...]</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Subject> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel2</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:type="xacml-context:AttributeValueType">TestChannel3</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Action> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VIEW</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Action> 
  <xacml-context:Environment> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address" 
                           DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">127.0.0.1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Environment> 
  </xacml-context:Request> 
  </xacml-samlp:XACMLAuthzDecisionQuery> 
</soap11:Body> 
</soap11:Envelope>
```

## Autorizzazione Personalizzata Per Più Risorse {#custom-authz}

Alcuni MVPD dispongono di endpoint di autorizzazione che supportano l&#39;autorizzazione per più risorse in una richiesta, ma non rientrano nello scenario descritto in AuthZ multicanale. Questi MVPD specifici richiedono un lavoro personalizzato.

Adobe può inoltre supportare l’autorizzazione di più canali senza apportare modifiche all’implementazione esistente.  Questo approccio deve essere rivisto tra Adobe e il team tecnico di MVPD per garantire che funzioni come previsto.

## MVPD che supportano l&#39;autorizzazione di verifica preliminare {#mvpds-supp-preflight-authz}

Nella tabella seguente sono elencati gli MVPD che supportano l&#39;autorizzazione di verifica preliminare, insieme al tipo di verifica preliminare supportato e alle limitazioni note:

| Approccio di verifica preliminare | MVPD | Note |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| AuthZ multicanale | Comcast AT&amp;T Proxy Clearleap Charter_Direct Proxy GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| Linea di canali nei metadati dell’utente | Suddenlink HTC | Tutte le integrazioni dirette Synacor supportano anche questo approccio. |
| Fork e join | Tutti gli altri non elencati sopra | Numero massimo predefinito di risorse controllate = 5. |

