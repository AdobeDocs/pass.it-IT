---
title: Autenticazione MVPD
description: Autenticazione MVPD
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1908'
ht-degree: 0%

---

# Autenticazione MVPD {#mvpd-authn}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#mvpd-authn-overview}

Il ruolo effettivo di provider di servizi (SP) è detenuto da un programmatore, ma Adobe Pass Authentication funge da proxy SP per tale programmatore. L’utilizzo dell’autenticazione di Adobe Pass come intermediario consente sia agli MVPD che ai programmatori di evitare di dover personalizzare i processi di adesione caso per caso.

I passaggi seguenti presentano la sequenza di eventi, utilizzando l’autenticazione di Adobe Pass, quando un programmatore richiede l’autenticazione da un MVPD che supporta SAML. Il componente Adobe Pass Authentication Access Enabler è attivo nel client dell’utente o del sottoscrittore. Da lì, Access Enabler semplifica tutti i passaggi del flusso di autenticazione.

1. Quando l’utente richiede l’accesso a contenuti protetti, Access Enabler avvia l’autenticazione (AuthN) per conto del programmatore (SP).
1. L’app dell’SP presenta un &quot;selettore MVPD&quot; all’utente per ottenere il proprio provider di Pay TV (MVPD). L&#39;SP reindirizza quindi il browser dell&#39;utente al servizio Identity Provider (IdP) dell&#39;MVPD selezionato.  Questo è &quot;**Accesso avviato dal programmatore**&quot;.  MVPD invia la risposta dell&#39;IdP al servizio consumer di asserzione SAML di Adobe, dove viene elaborata.
1. Infine, Access Enabler reindirizza il browser al sito SP, informandolo dello stato (operazione riuscita/non riuscita) della richiesta AuthN.

## La richiesta di autenticazione {#authn-req}

Come descritto nei passaggi precedenti, durante il flusso di AuthN un MVPD deve accettare una richiesta di AuthN basata su SAML e inviare una risposta di AuthN SAML.

Il [Specifiche dell&#39;interfaccia di autenticazione e autorizzazione OLCA (Online Content Access)](https://www.cablelabs.com/specifications/search?query=&amp;category=&amp;subcat=&amp;doctype=&amp;content=false&amp;archives=false){target=_blanck} presenta una richiesta e una risposta AuthN standard. Anche se l’autenticazione Adobe Pass non richiede che gli MVPD basino i messaggi di adesione su questo standard, l’analisi delle specifiche può fornire informazioni approfondite sugli attributi chiave necessari per una transazione AuthN.

>[!NOTE]
>
>La richiesta AuthN ricevuta da MVPD con l’autenticazione di Adobe Pass contiene una firma digitale. Tuttavia, nell&#39;esempio seguente non viene visualizzata alcuna firma, per motivi di brevità. Per un esempio che mostra una firma digitale, vedere l&#39;esempio in [la risposta di autenticazione](#authn-response) nelle sezioni seguenti.

Esempio di richiesta di autenticazione SAML:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:AuthnRequest  
    AssertionConsumerServiceURL=http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer          
    Destination=http://idp.com/SSOService
    ForceAuthn="false"
    ID="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
    IsPassive="false"
    IssueInstant="2010-08-03T14:14:54.372Z"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
    Version="2.0"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
        http://saml.sp.adobe.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds=_signature_block_goes_here_
    </ds:Signature>
    <samlp:NameIDPolicy
        AllowCreate="true"
        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
        SPNameQualifier="http://saml.sp.adobe.adobe.com"/>
</samlp:AuthnRequest> 
```

La tabella seguente spiega gli attributi e i tag che devono trovarsi in una richiesta di autenticazione, con i valori previsti predefiniti.

**Dettagli della richiesta di autenticazione SAML**

| samlp:AuthnRequest | &lt;authnrequest> rilasciata dal provider di servizi al provider di identità. |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | Questo è l’endpoint Adobe da utilizzare nella risposta successiva. Valore predefinito: **http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| Destinazione | Riferimento URI che indica l&#39;indirizzo a cui è stata inviata la richiesta. Ciò è utile per evitare l’inoltro dannoso di richieste a destinatari non intenzionali, una protezione richiesta da alcune associazioni di protocollo. Se presente, il destinatario effettivo DEVE verificare che il riferimento URI identifichi la posizione in cui è stato ricevuto il messaggio. In caso contrario, la richiesta DEVE essere eliminata. Per alcune associazioni di protocollo può essere necessario utilizzare questo attributo. |
| ForceAuthn | L&#39;attributo ForceAuthn, se presente con il valore true, obbliga il provider di identità a stabilire questa identità in modo più recente, anziché basarsi su una sessione esistente che potrebbe avere con l&#39;entità principale. |
| ID | Identificatore della richiesta. Consulta [Core SAML 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} sezione 1.3.4 per i dettagli. |
| IsPassive | Un valore booleano. Se &quot;true&quot;, il provider di identità e lo stesso agente utente NON DEVONO assumere in modo visibile il controllo dell&#39;interfaccia utente dal richiedente e interagire con il presentatore in modo percepibile. Se non viene fornito un valore, il valore predefinito è &quot;false&quot;. |
| IssueInstant | L’istante di tempo in cui è stato emesso il messaggio di risposta. Il valore temporale è codificato in UTC come descritto in [Core SAML 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} Sezione 1.3.3. |
| ProtocolBinding | Un riferimento URI che identifica un binding del protocollo SAML da utilizzare quando viene restituito &lt;response> messaggio. Consulta [SAMLBind] per ulteriori informazioni sulle associazioni di protocollo e sui riferimenti URI definiti per tali associazioni. Valore predefinito : urn:oasis:nomi:tc:SAML:2.0:bindings:HTTP-POST |
| Versione | Versione della richiesta. |
| saml:Emittente | Identifica l’entità che ha generato il messaggio di risposta. (Per ulteriori informazioni su questo elemento, vedi SAML Core 2.0-os Section 2.2.5) |
| ds:Firma | Firma XML che protegge l&#39;integrità dell&#39;autorità emittente dell&#39;asserzione e ne autentica l&#39;autenticazione, come descritto nella sezione 5 di SAML Core 2.0-os |
| samlp:NameIDPolicy | Specifica i vincoli sull&#39;identificatore del nome da utilizzare per rappresentare il soggetto richiesto. |
| AllowCreate | Valore booleano utilizzato per indicare se al provider di identità è consentito, nel corso dell’esecuzione della richiesta, creare un nuovo identificatore per rappresentare l’entità. Predefinito: true |
| Formato | Specifica il riferimento URI corrispondente al formato dell&#39;identificatore di nome Default: urn:oasis:nomi:tc:SAML:2.0:nameid-format:Adobe transitorio consiglia: urn:oasis:nomi:tc:SAML:2.0:nameid-format:persistente |
| SPNameQualifier | Facoltativamente, specifica che l&#39;identificatore del soggetto dell&#39;asserzione deve essere restituito (o creato) nello spazio dei nomi di un provider di servizi diverso dal richiedente. Predefinito : http://saml.sp.adobe.adobe.com |

## Risposta di autenticazione {#authn-response}

Dopo aver ricevuto e gestito la richiesta di autenticazione, MVPD deve ora inviare una risposta di autenticazione.

**Esempio di risposta di autenticazione SAML**

```XML
<?xml version="1.0" encoding="UTF-8"?> 
<samlp:Response Destination="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_0ac3a9dd5dae0ce05de20912af6f4f83a00ce19587"                             
                InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                IssueInstant="2010-08-17T11:17:50Z" Version="2.0"              
                xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
                xmlns:xs="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
             http://idp.com/SSOService
    </saml:Issuer>
    <samlp:Status>
       <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="pfxb0662d76-17a2-a7bd-375f-c11046a86742"
                   IssueInstant="2010-08-17T11:17:50Z"
                   Version="2.0">
        <saml:Issuer>http://idp.com/SSOService</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod
                     Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod
                     Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
            <ds:Reference URI="#pfxb0662d76-17a2-a7bd-375f-c11046a86742">
              <ds:Transforms>
                 <ds:Transform
                    Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>        
                 <ds:Transform
                            Algorithm=http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <ds:DigestValue>LgaPI2ASx/fHsoq0rB15Zk+CRQ0=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>
                POw/mCKF__shortened_for_brevity__9xdktDu+iiQqmnTs/NIjV5dw==
          </ds:SignatureValue>
          <ds:KeyInfo>
            <ds:X509Data>
                <ds:X509Certificate>
                 MIIDVDCCAjygAwIBA__shortened_for_brevity_utQ==
                </ds:X509Certificate>
            </ds:X509Data>
          </ds:KeyInfo>
      </ds:Signature>
      <saml:Subject>
        <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                     SPNameQualifier="https://saml.sp.auth.adobe.com">
            _5afe9a437203354aa8480ce772acb703e6bbb8a3ad
        </saml:NameID>
        <saml:SubjectConfirmation
                     Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml:SubjectConfirmationData
                  InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                  NotOnOrAfter="2010-08-17T11:22:50Z"                                          
                  Recipient="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"/>
           </saml:SubjectConfirmation>
       </saml:Subject>
       <saml:Conditions NotBefore="2010-08-17T11:17:20Z"
                        NotOnOrAfter="2010-08-17T19:17:50Z">
           <saml:AudienceRestriction>
              <saml:Audience>https://saml.sp.auth.adobe.com</saml:Audience>
           </saml:AudienceRestriction>
       </saml:Conditions>
       <saml:AuthnStatement AuthnInstant="2010-08-17T11:17:50Z"
                   SessionIndex="_1adc7692e0fffbb1f9b944aeafce62aaa7d770cd9e">
        <saml:AuthnContext>
            <saml:AuthnContextClassRef>
                   urn:oasis:names:tc:SAML:2.0:ac:classes:Password
            </saml:AuthnContextClassRef>
        </saml:AuthnContext>
    </saml:AuthnStatement>
  </saml:Assertion>
</samlp:Response>
```


Nell’esempio precedente, l’SP Adobe prevede di recuperare l’ID utente dal Subject/NameId. L’SP di Adobe può essere configurato per ottenere l’ID utente da un attributo personalizzato definito; la risposta deve contenere un elemento come segue:

```XML
<saml:AttributeStatement>
     <saml:Attribute Name="guid" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue xsi:type="xs:string">
               71C69B91-F327-F185-F29E-2CE20DC560F5
         </saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```

**Dettagli risposta autenticazione SAML**
| samlp:Risposta | Risposta ricevuta dall&#39;autenticazione Adobe Pass.                                                                                                                                                                                                                                                                                                                                                                                                                       | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------------ | Destinazione | Riferimento URI che indica l&#39;indirizzo a cui è stata inviata la richiesta. Ciò è utile per evitare l’inoltro dannoso di richieste a destinatari non intenzionali, una protezione richiesta da alcune associazioni di protocollo. Se presente, il destinatario effettivo DEVE verificare che il riferimento URI identifichi la posizione in cui è stato ricevuto il messaggio. In caso contrario, la richiesta DEVE essere eliminata. Per alcune associazioni di protocollo può essere necessario utilizzare questo attributo. | ID | | Identificatore della richiesta. È di tipo xs:ID e DEVE soddisfare i requisiti di cui al punto 1.3.4 della [Core SAML 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} per l’univocità dell’identificatore. I valori dell’attributo ID in una richiesta e dell’attributo InResponseTo nella risposta corrispondente DEVONO corrispondere.                                                                                                                                                                                         | | In risposta a | ID di un messaggio del protocollo SAML in risposta al quale un&#39;entità di attestazione può presentare l&#39;asserzione. Il valore deve essere uguale a quello dell’attributo ID inviato nella richiesta di autenticazione. Vedere SAML core 2.0-os | | IssueInstant | L&#39;ora del rilascio della richiesta.                                                                                                                                                                                                                                                                                                                                                                                                                                  | | Versione | Versione della richiesta.                                                                                                                                                                                                                                                                                                                                                                                                                                                | | saml:Emittente | Identifica l’entità che ha generato il messaggio di richiesta. (Per ulteriori informazioni su questo elemento, cfr. sezione 2.2.5. Core SAML 2.0-os ) | | samlp:Stato | Codice che rappresenta lo stato della richiesta corrispondente.                                                                                                                                                                                                                                                                                                                                                                                                               | | samlp:CodiceStato | Codice che rappresenta lo stato dell&#39;attività svolta in risposta alla richiesta corrispondente.                                                                                                                                                                                                                                                                                                                                                                       | | saml:Asserzione | Questo tipo specifica le informazioni di base comuni a tutte le asserzioni.                                                                                                                                                                                                                                                                                                                                                                                                        | ID | | Identificatore di questa asserzione.                                                                                                                                                                                                                                                                                                                                                                                                                                         | | Versione | Versione di questa asserzione.                                                                                                                                                                                                                                                                                                                                                                                                                                             | | IssueInstant | L&#39;ora del rilascio della richiesta.                                                                                                                                                                                                                                                                                                                                                                                                                                  | | ds:Firma | Firma XML che protegge l&#39;integrità dell&#39;emittente dell&#39;asserzione e ne autentica l&#39;autenticità, come descritto nella sezione 5 di SAML Core 2.0-os | | ds:InformazioniFirmate | La struttura di SignedInfo include l&#39;algoritmo di canonizzazione, un algoritmo di firma e uno o più riferimenti. L&#39;elemento SignedInfo può contenere un attributo ID facoltativo che consentirà di utilizzare altre firme e altri oggetti come riferimento a tale elemento. Consulta Sintassi ed elaborazione della firma XML | | ds:CanonicalizationMethod | CanonicalizationMethod è un elemento obbligatorio che specifica l&#39;algoritmo di canonicalizzazione applicato all&#39;elemento SignedInfo prima di eseguire i calcoli della firma. Consulta Sintassi ed elaborazione della firma XML | | ds:SignatureMethod | SignatureMethod è un elemento obbligatorio che specifica l&#39;algoritmo utilizzato per la generazione e la convalida della firma. Questo algoritmo identifica tutte le funzioni crittografiche coinvolte nell’operazione di firma (ad esempio hashing, algoritmi a chiave pubblica, MAC, padding, ecc.) Consulta Sintassi ed elaborazione della firma XML | | ds:Riferimento | Il riferimento è un elemento che può verificarsi una o più volte. Specifica un algoritmo di digest e un valore di digest, e facoltativamente un identificatore dell&#39;oggetto da firmare, il tipo dell&#39;oggetto e/o un elenco di trasformazioni da applicare prima della digestione. Consulta Sintassi ed elaborazione della firma XML | | ds:Trasformazioni | L’elemento Transforms facoltativo contiene un elenco ordinato di elementi Transform che descrivono il modo in cui il firmatario ha ottenuto l’oggetto dati digerito. L&#39;output di ciascuna trasformazione funge da input per la successiva trasformazione. L&#39;input per la prima trasformazione è il risultato del dereferenziamento dell&#39;attributo URI dell&#39;elemento Reference. L&#39;output dell&#39;ultima trasformazione è l&#39;input per l&#39;algoritmo DigestMethod. Consulta Sintassi ed elaborazione della firma XML | | ds:DigestMethod | DigestMethod è un elemento obbligatorio che identifica l&#39;algoritmo di digest da applicare all&#39;oggetto firmato. Consulta Sintassi ed elaborazione della firma XML | | ds:DigestValue | DigestValue è un elemento che contiene il valore codificato del digest. Il digest viene sempre codificato utilizzando base64. Consulta Sintassi ed elaborazione della firma XML | | ds:SignatureValue | L&#39;elemento SignatureValue contiene il valore effettivo della firma digitale, che viene sempre codificato utilizzando base64. Consulta Sintassi ed elaborazione della firma XML | | ds:InformazioniChiave | KeyInfo è un elemento facoltativo che consente ai destinatari di ottenere la chiave necessaria per convalidare la firma. Consulta Sintassi ed elaborazione della firma XML | | ds:X509Data | Un elemento X509Data in KeyInfo contiene uno o più identificatori di chiavi o certificati X509. Consulta Sintassi ed elaborazione della firma XML | | ds: Certificato X509Certificate | L&#39;elemento X509Certificate, che contiene un codice base64 [X509v3] certificato | | saml:Oggetto | Oggetto delle affermazioni nell’asserzione.                                                                                                                                                                                                                                                                                                                                                                                                                          | | saml:NameID | Il &lt;nameid> è di tipo NameIDType (vedi Sezione 2.2.2 in SAML core 2.0-os) ed è utilizzato in vari costrutti di asserzione SAML come &lt;subject> e &lt;subjectconfirmation> e in vari messaggi di protocollo.                                                                                                                                                                                                                                           | Formato | | Riferimento URI che rappresenta la classificazione delle informazioni dell&#39;identificatore basato su stringhe.                                                                                                                                                                                                                                                                                                                                                                                    | | SPNameQualifier | Può essere altresì indicato un nome con il nome di un fornitore di servizi o di un&#39;affiliazione di fornitori. Questo attributo consente di unire i nomi in base alla parte o alle parti che fanno affidamento sulla certificazione.                                                                                                                                                                                                                                                                      | | saml:SubjectConfirmation | Informazioni che consentono di confermare il soggetto. Se viene fornita più di una conferma dell&#39;oggetto, è sufficiente soddisfarne una per confermare l&#39;oggetto ai fini dell&#39;applicazione dell&#39;asserzione.                                                                                                                                                                                                                                                    | | saml:SubjectConfirmationData | Ulteriori informazioni di conferma da utilizzare con un metodo di conferma specifico. Ad esempio, il contenuto tipico di questo elemento potrebbe essere un <!--<ds:KeyInfo>--> come definito nella specifica di elaborazione e sintassi della firma XML | | NotOnOrAfter | Un momento in cui il soggetto non può più essere confermato.                                                                                                                                                                                                                                                                                                                                                                                                            | | Destinatario | URI che specifica l&#39;entità o la posizione in cui un&#39;entità di attestazione può presentare l&#39;asserzione. Ad esempio, questo attributo potrebbe indicare che l’asserzione deve essere consegnata a un particolare endpoint di rete per impedire a un intermediario di reindirizzarla altrove.                                                                                                                                                                                   | | saml:Condizioni | Il &lt;condition> funge da punto di estensione per le nuove condizioni.                                                                                                                                                                                                                                                                                                                                                                                                   | | NotBefore | L&#39;attributo NotBefore specifica l&#39;istante di inizio dell&#39;intervallo di validità.                                                                                                                                                                                                                                                                                                                                                                                  | | saml:AudienceRestriction | Il &lt;audiencerestriction> specifica che l&#39;asserzione è indirizzata a uno o più tipi di pubblico specifici identificati da &lt;audience> elementi.                                                                                                                                                                                                                                                                                                                           | | saml:Pubblico | Riferimento URI che identifica un pubblico previsto.                                                                                                                                                                                                                                                                                                                                                                                                                      | | saml:AuthnStatement | Un&#39;istruzione di autenticazione.                                                                                                                                                                                                                                                                                                                                                                                                                                               | | AuthnInstant | Specifica l&#39;ora in cui si è verificata l&#39;autenticazione.                                                                                                                                                                                                                                                                                                                                                                                                                 | | SessionIndex | Specifica l&#39;indice di una determinata sessione tra l&#39;entità principale identificata dal soggetto e l&#39;autorità di autenticazione.                                                                                                                                                                                                                                                                                                                                              | | saml:ContestoAutore | Contesto utilizzato dall’autorità di autenticazione fino all’evento di autenticazione che ha prodotto questa dichiarazione e incluso tale evento.                                                                                                                                                                                                                                                                                                                                                 | | saml:AuthnContextClassRef | Riferimento URI che identifica una classe di contesto di autenticazione che descrive la dichiarazione di contesto di autenticazione seguente. |
