---
title: Servizio Web MVPD proxy
description: Servizio Web MVPD proxy
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---


# Servizio Web MVPD proxy {#proxy-mvpd-wbservice}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Prima di utilizzare il servizio Web MVPD proxy, verificare che siano soddisfatti i seguenti prerequisiti:
>
> * Ottenere le credenziali del client come descritto nella [Documentazione API per il recupero delle credenziali del client](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Ottenere il token di accesso come descritto nella documentazione API [Recuperare il token di accesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Per ulteriori informazioni su come creare un&#39;applicazione registrata e scaricare l&#39;istruzione software, consultare la documentazione [Panoramica registrazione client dinamica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Panoramica {#overview-proxy-mvpd-webserv}

Un &quot;Proxy MVPD&quot; è un MVPD che, oltre a gestire la propria integrazione con l’autenticazione Adobe Pass, gestisce anche il processo di adesione per conto di un gruppo di &quot;Proxy MVPD&quot; associati. Questa disposizione è trasparente per i programmatori.

Per implementare la funzione ProxyMVPD, Adobe Pass Authentication fornisce servizi web RESTful, con cui i ProxyMVPD possono inviare e recuperare elenchi di ProxiedMVPD. Il protocollo utilizzato per questa API pubblica è REST HTTP, con i seguenti presupposti:

- Proxy MVPD utilizza il metodo HTTP GET per recuperare l&#39;elenco degli MVPD integrati correnti.
- Il MVPD proxy utilizza il metodo HTTP POST per aggiornare l&#39;elenco degli MVPD supportati.

## Servizi MVPD proxy {#proxy-mvpd-services}

- [Recupera MVPD proxy](#retriev-proxied-mvpds)
- [Invia MVPD proxy](#submit-proxied-mvpds)

### Recuperare MVPD proxy {#retriev-proxied-mvpds}

Recupera l&#39;elenco corrente di MVPD proxy integrati con il MVPD proxy identificato.

| Endpoint | Chiamato da | Parametri di richiesta | Intestazioni richiesta | Metodo HTTP | Risposta HTTP |
|--------------------------------------------------------------------------|-----------|-----------------------|---------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Autorizzazione (obbligatoria) | GET | <ul><li> 200 (ok) - La richiesta è stata elaborata correttamente e la risposta contiene un elenco di ProxiedMVPD in formato XML</li><li>401 (non autorizzato) - Indica uno dei seguenti elementi:<ul><li>Il client DEVE richiedere un nuovo access_token</li><li>La richiesta proviene da un indirizzo IP non presente nell’elenco Consentiti</li><li>Token non valido</li></ul></li><li>403 (non consentito): indica che l’operazione non è supportata per i parametri forniti oppure che il proxy MVPD non è impostato come proxy o è mancante</li><li>405 (metodo non consentito): è stato utilizzato un metodo HTTP diverso da GET o POST. Il metodo HTTP non è supportato in genere o non è supportato per questo endpoint specifico.</li><li>500 (errore interno del server) - È stato generato un errore sul lato server durante il processo di richiesta.</li></ul> |

Esempio di curl:

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds"`


Esempio di risposta XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```

### Invia MVPD proxy {#submit-proxied-mvpds}

Invia un array di MVPD integrato con il MVPD proxy identificato.

| Endpoint | Chiamato da | Parametri di richiesta | Intestazioni richiesta | Metodo HTTP | Risposta HTTP |
|:------------------------------------------------------------------------:|:---------:|-----------------------|:---------------------------------------------------:|:-----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Autorizzazione (obbligatoria) proxied-mvpds (obbligatoria) | POST | <ul><li>201 (creato) - Push elaborato correttamente</li><li>400 (richiesta non valida) - Il server non è in grado di elaborare la richiesta:<ul><li>Il codice XML in ingresso non è conforme allo schema pubblicato in questa specifica</li><li>Gli mvpd proxy non hanno ID univoci</li><li>Gli ID richiedente inviati non esistono. Altro motivo del contenitore servlet per il codice di risposta 400.</li></ul><li>401 (non autorizzato) - Indica uno dei seguenti elementi:<ul><li>Il client DEVE richiedere un nuovo access_token</li><li>La richiesta proviene da un indirizzo IP non presente nell’elenco Consentiti</li><li>Token non valido</li></ul></li><li>403 (non consentito): indica che l’operazione non è supportata per i parametri forniti oppure che il proxy MVPD non è impostato come proxy o è mancante</li><li>405 (metodo non consentito): è stato utilizzato un metodo HTTP diverso da GET o POST. Il metodo HTTP non è supportato in genere o non è supportato per questo endpoint specifico.</li><li>500 (errore interno del server) - È stato generato un errore sul lato server durante il processo di richiesta.</li></ul> |

Esempio di curl:

`curl -X POST -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



Esempio XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```


### Frequenza di registrazione {#posting-frequency}

L’autenticazione Adobe Pass consiglia ai ProxyMVPD di inviare l’elenco dei ProxyMVPD solo quando è presente una modifica rispetto al push precedente.

### Eliminazione di MVPD proxy {#delete-proxied-freqency}

Se ProxyMVPD invia un record XML con un elenco ProxiedMVPD vuoto, tale elenco vuoto verrà memorizzato nel nostro sistema come qualsiasi elenco, eliminando di fatto l&#39;elenco precedente.



## Formato XSD {#xsd-format}

Adobe ha definito il seguente formato accettato per la pubblicazione/il recupero di MVPD proxy da/verso il servizio web pubblico:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns:pxm="http://tve.adobe.com/data/proxiedmvpd"
           targetNamespace="http://tve.adobe.com/data/proxiedmvpd"
           elementFormDefault="qualified"
           version="1.0">
    <xs:complexType name="iframeSize">
        <xs:all>
            <xs:element name="iframeHeight" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeWidth" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
        </xs:all>
    </xs:complexType>
    <xs:complexType name="requestorIds">
        <xs:annotation>
            <xs:documentation>List of requestors/programmers integrated with the proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:sequence>
            <xs:element name="requestorId" type="xs:string" minOccurs="1" maxOccurs="unbounded" nillable="false">
                <xs:annotation>
                    <xs:documentation>The requestor/programmer identifier recognized by Adobe</xs:documentation>
                </xs:annotation>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
    <xs:complexType name="proxiedMvpd">
        <xs:all>
            <xs:element name="id" minOccurs="1" maxOccurs="1" nillable="false">
                <xs:annotation>
                    <xs:documentation>The id must conform to the regular expression: ([a-zA-Z0-9]+((\-)|[_])*)</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:simpleContent>
                        <xs:extension base="xs:string">
                            <xs:attribute name="ProviderID">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1"/>
                                        <xs:maxLength value="128"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:attribute>
                        </xs:extension>
                    </xs:simpleContent>
                </xs:complexType>
            </xs:element>
            <xs:element name="displayName" type="xs:string" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="logoURL" type="xs:anyURI" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeSize" type="pxm:iframeSize" minOccurs="0" maxOccurs="1"/>
            <xs:element name="requestorIds" type="pxm:requestorIds" minOccurs="0" maxOccurs="1"/>
        </xs:all>
    </xs:complexType>
    <xs:element name="proxiedMvpds">
        <xs:annotation>
            <xs:documentation>List of Proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:complexType>
            <xs:sequence>
                <xs:element name="proxiedMvpd" type="pxm:proxiedMvpd" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

**Note sugli elementi:**

-   `id` (obbligatorio) - L&#39;ID MVPD proxy deve essere una stringa rilevante per il nome del MVPD, che utilizza uno dei seguenti caratteri (in quanto verrà esposto ai programmatori a scopo di tracciamento):
-   Qualsiasi carattere alfanumerico, carattere di sottolineatura (&quot;_&quot;) e trattino (&quot;-&quot;).
-   L’idID deve essere conforme alla seguente espressione regolare:
`(a-zA-Z0-9((-)|_)*)`

    Deve quindi contenere almeno un carattere, iniziare con una lettera e continuare con qualsiasi lettera, cifra, trattino o carattere di sottolineatura.

-   `iframeSize` (facoltativo) - L&#39;elemento iframeSize è facoltativo e definisce le dimensioni dell&#39;iFrame se la pagina di autenticazione di MVPD deve essere inclusa in un iFrame. In caso contrario, se l’elemento iframeSize non è presente, l’autenticazione verrà eseguita in una pagina di reindirizzamento del browser completa.
-   `requestorIds` (facoltativo) - I valori requestorIds verranno forniti da Adobe. Un requisito è che un MVPD con proxy debba essere integrato con almeno un requestorId. Se il tag &quot;requestorIds&quot; non è presente nell&#39;elemento MVPD proxy, tale MVPD proxy verrà integrato con tutti i richiedenti disponibili integrati nel MVPD proxy.
-   `ProviderID` (facoltativo) - Quando l&#39;attributo ProviderID è presente nell&#39;elemento ID, il valore di ProviderID verrà inviato nella richiesta di autenticazione SAML al MVPD proxy come ID MVPD/SubMVPD proxy (invece del valore ID). In questo caso, il valore di id verrà utilizzato solo nel selettore MVPD presentato nella pagina Programmatore e internamente tramite l’autenticazione di Adobe Pass. La lunghezza dell&#39;attributo ProviderID deve essere compresa tra 1 e 128 caratteri.

## Sicurezza {#security}

Per essere considerata valida, una richiesta deve rispettare le seguenti regole:

- L&#39;intestazione della richiesta deve contenere il token di accesso Oauth2 di sicurezza ottenuto come descritto nella documentazione API [Recupera token di accesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
- La richiesta deve provenire da un indirizzo IP specifico che è stato consentito.
- La richiesta deve essere inviata tramite il protocollo SSL.

Eventuali parametri presenti nell’intestazione della richiesta non elencati sopra verranno ignorati.

Esempio di curl:

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/<proxy-mvpd-identifier>/mvpds"`

## Endpoint del servizio Web MVPD proxy per gli ambienti di autenticazione Adobe Pass {#proxy-mvpd-wevserv-endpoints}

- **URL di produzione:** https://mgmt.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **URL di gestione temporanea:** https://mgmt.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **URL preQual-produzione:** https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **URL preQual-staging:** https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
