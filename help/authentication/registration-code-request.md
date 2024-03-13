---
title: Pagina di registrazione
description: Pagina di registrazione
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# Pagina di registrazione {#registration-page}

## Endpoint REST API {#clientless-endpoints}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

>[!NOTE]
>
> L’implementazione REST API è limitata da [Meccanismo di limitazione](/help/authentication/throttling-mechanism.md)

&lt;reggie_fqdn>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrizione {#create-reg-code-svc}

Restituisce l&#39;URI del codice di registrazione e della pagina di accesso generati in modo casuale.

| Endpoint | Chiamato  </br>Da | Input   </br>Parametro | HTTP  </br>Metodo | Risposta | HTTP  </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;reggie_fqdn>/reggie/v1/{requestor}/regcode</br>Ad esempio:</br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | App di streaming</br>o</br>Servizio programmatore | 1. richiedente  </br>    (componente Percorso)</br>2.  deviceId (Hashed)   </br>    (Obbligatorio)</br>3.  device_info/X-Device-Info (Obbligatorio)</br>4.  mvpd (facoltativo)</br>5.  ttl (facoltativo)</br>6.  _deviceType_</br> 7.  _deviceUser_ (Obsoleto)</br>8.  _appId_ (Obsoleto) | POST | XML o JSON contenente un codice di registrazione e informazioni o dettagli sull’errore in caso di esito negativo. Consulta schemi ed esempi di seguito. | 201 |

{style="table-layout:auto"}

| Parametro di input | Descrizione |
| --- | --- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| device_info/</br>X-Device-Info | Informazioni sul dispositivo di streaming.</br>**Nota**: questo PUÒ essere trasmesso device_info come parametro URL, ma a causa delle dimensioni potenziali del parametro e delle limitazioni alla lunghezza di un URL GET, DEVE essere trasmesso come X-Device-Info nell’intestazione http. </br>Vedi tutti i dettagli in [Trasmissione delle informazioni sul dispositivo e sulla connessione](/help/authentication/passing-client-information-device-connection-and-application.md). |
| mvpd | ID MVPD per il quale è valida questa operazione. |
| ttl | Durata di questo codice regcode in secondi.</br>**Nota**: il valore massimo consentito per ttl è di 36000 secondi (10 ore). Valori più alti determinano una risposta HTTP 400 (richiesta non valida). Se `ttl` Se viene lasciato vuoto, Adobe Pass Authentication imposta un valore predefinito di 30 minuti. |
| _deviceType_ | Il tipo di dispositivo (ad esempio, Roku, PC).</br>Se questo parametro è impostato correttamente, ESM offre metriche che sono [suddiviso per tipo di dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) quando utilizzi Clientless, in modo da poter eseguire diversi tipi di analisi, ad esempio Roku, AppleTV e Xbox.</br>Vedi, [Vantaggi dell’utilizzo del parametro del tipo di dispositivo senza client nelle metriche di passaggio ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Nota**: device_info sostituirà questo parametro. |
| _deviceUser_ | L’identificatore utente del dispositivo. |
| _appId_ | ID/nome dell’applicazione. </br>**Nota**: device_info sostituisce questo parametro. |

{style="table-layout:auto"}


>[!CAUTION]
>
>**Indirizzo IP dispositivo di streaming**
></br>
>Per le implementazioni client-server, l&#39;indirizzo IP del dispositivo di streaming viene inviato implicitamente con questa chiamata.  Per le implementazioni server-to-server, in cui **regcode** Viene effettuata una chiamata al servizio Programmatore e non al dispositivo di streaming, è necessaria la seguente intestazione per trasmettere l&#39;indirizzo IP del dispositivo di streaming:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>dove `<streaming\_device\_ip>` è l&#39;indirizzo IP pubblico del dispositivo di streaming.
></br></br>
>Esempio:</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1</br>X-Forwarded-For:203.45.101.20
>```
>
</br>

### Schema XML di risposta {#xml-schema}


#### Codice di registrazione XSD {#registration-code-xsd}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="model.mvc.reggie.pass.adobe.com"
            targetNamespace="model.mvc.reggie.pass.adobe.com"
            attributeFormDefault="unqualified"
            elementFormDefault="unqualified">
        <xs:element name="regcode">
            <xs:complexType>
                <xs:all>
                    <xs:element name="id" type="xs:string" />
                    <xs:element name="code" type="xs:string" />
                    <xs:element name="requestor" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="mvpd" type="xs:string" minOccurs="1" maxOccurs="1"/
                    <xs:element name="generated" type="xs:long" />
                    <xs:element name="expires" type="xs:long" />
                    <xs:element name="info" type="infoType" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
        <xs:complexType name="infoType">
            <xs:all>
                <xs:element name="deviceId" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
                <xs:element name="deviceType" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="deviceUser" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appId" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appVersion" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="registrationURL" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
            </xs:all>
        </xs:complexType>
    </xs:schema>
```

</br>

| Nome elemento | Descrizione |
| --------------- | ------------------------------------------------------------------------------------ |
| id | UUID generato da Registration Code Service |
| codice | Codice di registrazione generato da Registration Code Service |
| richiedente | ID richiedente |
| mvpd | ID Mvpd |
| generato | Timestamp di creazione del codice di registrazione (in millisecondi dal 1° gennaio 1970 GMT) |
| scade | Timestamp di scadenza del codice di registrazione (in millisecondi dal 1° gennaio 1970 GMT) |
| deviceId | ID dispositivo univoco (o token XSTS) |
| deviceType | Tipo di dispositivo |
| deviceUser | Utente connesso al dispositivo |
| appId | ID applicazione |
| appVersion | Versione applicazione |
| registrationURL | URL dell&#39;app Web di accesso da visualizzare all&#39;utente finale |

{style="table-layout:auto"}


</br>

### Messaggio di errore XSD  {#error-message}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="rest.pass.adobe.com"
               targetNamespace="rest.pass.adobe.com"
               attributeFormDefault="unqualified"
               elementFormDefault="unqualified">
        <xs:element name="error">
            <xs:complexType>
                <xs:all>
                    <xs:element name="status" type="xs:int" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="message" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="details" type="xs:string" minOccurs="0" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
    </xs:schema>
```


### Risposta di esempio {#sample-response}

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <ns2:regcode xmlns:ns2="model.mvc.reggie.pass.adobe.com">
        <id>678f9fea-a1cafec8-1ff0-4a26-8564-f6cd020acf13</id>
        <code>TJJCFK</code>
        <requestor>sampleRequestorId</requestor>
        <mvpd>sampleMvpdId</mvpd>
        <generated>1348039846647</generated>
        <expires>1348043446647</expires>
        <info>
            <deviceId>dGhpc0lkQUR1bW15RGV2aWNlSWQ=</deviceId>
            <deviceType>xbox</deviceType>
            <deviceUser>JD</deviceUser>
            <appId>2345</appId>
            <appVersion>2.0</appVersion>
            <registrationURL>http://loginwebapp.com</registrationURL>
        </info>
    </ns2:regcode>
```

**JSON:**

```JSON
    {
        "id": "678f9fea-9d364b29-246c-488f-b97e-298566d1b9e0",
        "code": "D4BDU2W",
        "requestor": "sampleRequestorId",
        "mvpd": "sampleMvpdId",
        "generated": 1348039555877,
        "expires": 1348043155877,
        "info": {
            "deviceId": "dGhpc0l.kQUR1bW15RGV2.aWNlSWQ=",
            "deviceType": "xboxOne",
            "deviceUser": "JD",
            "appId": "2345",
            "appVersion": "2.0",
            "registrationURL": "http://loginwebapp.com"
        }
    }
```
