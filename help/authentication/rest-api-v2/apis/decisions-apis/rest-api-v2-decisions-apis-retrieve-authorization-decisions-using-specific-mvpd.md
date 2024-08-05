---
title: Recuperare le decisioni di autorizzazione utilizzando mvpd specifico
description: REST API V2 - Recuperare le decisioni di autorizzazione utilizzando mvpd specifico
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 1%

---


# Recuperare le decisioni di autorizzazione utilizzando mvpd specifico {#retrieve-authorization-decisions-using-specific-mvpd}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Richiesta {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">metodo</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Parametri percorso</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identificatore univoco interno associato al provider di servizi durante il processo di onboarding.</td>
      <td><i>obbligatorio</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>Identificatore univoco interno associato al provider di identità durante il processo di onboarding.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Parametri corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">risorse</td>
      <td>L’elenco delle risorse che richiedono una decisione MVPD prima di poter essere riprodotte.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorizzazione</td>
      <td>La generazione del payload del token Bearer è descritta nella documentazione di <a href="../../../dynamic-client-registration-api.md">Registrazione client dinamica</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         Tipo di file multimediale accettato per le risorse inviate.
         <br/><br/>
         Deve essere application/json.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Id</td>
      <td>La generazione del payload dell'identificatore del dispositivo è descritta nella documentazione di <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La generazione del payload di informazioni sul dispositivo è descritta nella documentazione di <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>.
         <br/><br/>
         Si consiglia vivamente di utilizzarlo sempre quando la piattaforma del dispositivo dell’applicazione consente di fornire esplicitamente valori validi.
         <br/><br/>
         Se fornito, il backend di autenticazione di Adobe Pass unirà in modo esplicito i valori con quelli estratti in modo implicito (per impostazione predefinita).
         <br/><br/>
         Se non viene fornito, il backend di autenticazione Adobe Pass utilizzerà i valori estratti in modo implicito (per impostazione predefinita).
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         Indirizzo IP del dispositivo di streaming.
         <br/><br/>
         Si consiglia vivamente di utilizzarlo sempre per le implementazioni server-to-server, in particolare quando la chiamata viene effettuata dal servizio del programmatore anziché dal dispositivo di streaming.
         <br/><br/>
         Per le implementazioni client-server, l’indirizzo IP del dispositivo di streaming viene inviato in modo implicito.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Adobe-Subject-Token</td>
      <td>
        La generazione del payload Single Sign-On per il metodo Platform Identity è descritta nella documentazione di <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a>.
        <br/><br/>
        Per ulteriori dettagli sui flussi abilitati per il Single Sign-On che utilizzano un'identità di piattaforma, fare riferimento alla documentazione <a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">Single Sign-On che utilizza flussi di identità di piattaforma</a>.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        La generazione del payload Single Sign-On per il metodo Service Token è descritta nella documentazione di <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>.
        <br/><br/>
        Per ulteriori dettagli sui flussi abilitati per il Single Sign-On tramite un token di servizio, fare riferimento alla documentazione <a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-service-token-flows.md">Single Sign-On tramite flussi di token di servizio</a>.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        La generazione del payload Single Sign-On per il metodo Partner è descritta nella documentazione di <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a>.
        <br/><br/>
        Per ulteriori dettagli sui flussi abilitati per il Single Sign-On tramite un partner, fare riferimento alla documentazione <a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-partner-flows.md">Single Sign-On tramite flussi partner</a>.</td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-TempPass-Identity</td>
      <td>La generazione del payload dell'identificatore univoco utente è descritta nella documentazione di <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md">AP-TempPass-Identity</a>.</td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accetta</td>
      <td>
         Tipo di supporto accettato dall'applicazione client.
         <br/><br/>
         Se specificato, deve essere application/json.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>Agente utente dell’applicazione client.</td>
      <td>facoltativo</td>
   </tr>
</table>

## Risposta {#response}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 10%;">Codice</th>
      <th style="background-color: #EFF2F7; width: 20%;">Testo</th>
      <th style="background-color: #EFF2F7;">Descrizione</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Il corpo della risposta contiene un elenco di decisioni con informazioni aggiuntive.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Richiesta non valida</td>
      <td>
        Richiesta non valida. Il client deve correggere la richiesta e riprovare. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="../../../enhanced-error-codes.md">Codici di errore avanzati</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non autorizzato</td>
      <td>
        Il token di accesso non è valido, il client deve ottenere un nuovo token di accesso e riprovare. Per ulteriori dettagli, consulta la documentazione di <a href="../../../dynamic-client-registration-api.md">Registrazione client dinamica</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Metodo non consentito</td>
      <td>
        Metodo HTTP non valido. Il client deve utilizzare un metodo HTTP consentito per la risorsa richiesta e riprovare. Per ulteriori dettagli, consulta la sezione <a href="#request">Richiesta</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Errore interno del server</td>
      <td>
        Si è verificato un problema sul lato server. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="../../../enhanced-error-codes.md">Codici di errore avanzati</a>.
      </td>
   </tr>
</table>

### Completato {#success}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Intestazioni</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>200</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">decisioni</td>
      <td>
         JSON contenente un elenco di elementi, ciascuno con i seguenti attributi:
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Attributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">resource</td>
                <td>L’identificatore della risorsa per cui viene restituita la decisione di autorizzazione.</td>
                <td><i>obbligatorio</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">serviceProvider</td>
                <td>Identificatore univoco interno associato al provider di servizi durante il processo di onboarding.</td>
                <td><i>obbligatorio</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">mvpd</td>
                <td>Identificatore univoco interno associato al provider di identità durante il processo di onboarding.</td>
                <td><i>obbligatorio</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">authorized</td>
                <td>Lo stato della decisione per la risorsa, che può essere "true" o "false".</td>
                <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sorgente</td>
               <td>
                  Informazioni sulla fonte della decisione:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Valore</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">mvpd</td>
                        <td>Decisione emessa dall'endpoint di autorizzazione MVPD.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">degradazione</td>
                        <td>La decisione è emessa in conseguenza di un accesso deteriorato.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">temppass</td>
                        <td>La decisione è emessa a seguito dell’accesso temporaneo.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">fittizio</td>
                        <td>La decisione viene emessa come risultato della funzione di autorizzazione fittizia.</td>
                     </tr>
                  </table>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token</td>
               <td>
                  Informazioni sul token multimediale:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Attributo</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">notBefore</td>
                        <td>Il timestamp prima del quale il token multimediale non è valido.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">notAfter</td>
                        <td>Il timestamp dopo il quale il token multimediale non è valido.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">serializedToken</td>
                        <td>Il token multimediale con codifica Base64.</td>
                     </tr>
                  </table>
               <td>facoltativo</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>Il timestamp prima del quale la decisione non è valida.</td>
               <td>facoltativo</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Il timestamp dopo il quale la decisione non è valida.</td>
               <td>facoltativo</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">errore</td>
               <td>L'errore fornisce informazioni aggiuntive sulla decisione 'Nega' che è conforme alla documentazione di <a href="../../../enhanced-error-codes.md">Codici di errore avanzati</a>.</td>
               <td>facoltativo</td>
            </tr>
         </table>
      </td>
      <td><i>obbligatorio</i></td>
</table>

### Errore {#error}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>400, 401, 405, 500</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">errore</td>
      <td>L'errore fornisce informazioni aggiuntive conformi alla documentazione di <a href="../../../enhanced-error-codes.md">Codici di errore avanzati</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

## Esempi {#samples}

### 1. Recuperare le decisioni di autorizzazione utilizzando mvpd regolare

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
POST /api/v2/REF30/decisions/authorize/Cablevision

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Risposta - Disponibile]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8     
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "Cablevision",
            "source": "mvpd",
            "authorized": true,
            "token": {
                "issuedAt": 1697094207324,
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            }
        }
    ]
}
```

>[!ENDTABS]

### 2. Recuperare le decisioni di autorizzazione mediante passaggio temporaneo

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
POST /api/v2/apasstest1/decisions/authorize/TempPass_TEST40 HTTP/1.1

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Risposta - Disponibile]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8     
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": true,
            "token": {
                "issuedAt": 1697094207324,
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            }
        }
    ]
}
```

>[!TAB Risposta - Avviata]

```JSON
HTTP/1.1 200 OK
Content-Type: applicationjson
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": true,
            "token": {
                "issuedAt": 1695360527896,
                "notBefore": 1695360527896,
                "notAfter": 1695360707896,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            }
        }
    ]
}
```

>[!TAB Risposta - Scaduta]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_expired",
                "message": "TempPass has expired.",
                 "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB Risposta - Configurazione non valida]

```JSON
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8
 
{
    "status": 500,
    "code": "temppass_invalid_configuration",
    "message": "TempPass configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 3. Recuperare le decisioni di autorizzazione mediante l&#39;autorizzazione temporanea promozionale

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
POST /api/v2/apasstest1/decisions/authorize/flexibleTempPass HTTP/1.1

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Risposta - Disponibile]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
    ]
}
```

>[!TAB Risposta - Avviata]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
    ]
}
```

>[!TAB Risposta - Scaduta]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_expired",
                "message": "TempPass has expired.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB Risposta - Utilizzata]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_max_resources_exceeded",
                "message": "Flexible TempPass maximum resources exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB Risposta - Configurazione non valida]

```JSON
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8      
 
{
    "status": 500,
    "code": "temppass_invalid_configuration",
    "message": "TempPass configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!TAB Risposta - Identità non valida]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8 
 
{
    "status": 400,
    "code": "temppass_invalid_identity",
    "message": "TempPass is not available for the specified identity.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 4. Recuperare le decisioni di autorizzazione utilizzando mvpd degradato

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
POST /api/v2/REF30/decisions/authorize/degradedMvpd

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30", "apasstest1"]
}
```

>[!TAB Risposta - Degradazione AuthNAll]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            }
        }
    ]
}
```

**Nota:** In questo caso, la regola AuthZAll ha &quot;channel&quot;: &quot;apasstest1&quot;

>[!TAB Risposta - Degrado AuthZAll]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            }
        }
    ]
}
```

**Nota:** In questo caso, la regola AuthZAll ha &quot;channel&quot;: &quot;apasstest1&quot;

>[!TAB Risposta - Degradazione AuthZNone]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
 
    ]
}
```

**Nota:** In questo caso, la regola AuthZNone ha &quot;channel&quot;: &quot;apasstest1&quot;

>[!TAB Risposta - Regola di degradazione scaduta]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_configuration_change",
                "message": "AuthXAll degradation configuration changed, please try again!",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]
