---
title: Recupera sessione di autenticazione tramite codice
description: REST API V2 - Recupera la sessione di autenticazione tramite il codice
exl-id: 5cc209eb-ee6b-4bb9-9c04-3444408844b7
source-git-commit: 110e8519d6c042cc38de3fbefcd34297b6edcfad
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 2%

---

# Recupera sessione di autenticazione tramite codice {#retrieve-authentication-session-using-code}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione REST API V2 è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Visita anche le [Domande frequenti su REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

## Richiesta {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/api/v2/{serviceProvider}/session/{code}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">metodo</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parametri percorso</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identificatore univoco interno associato al provider di servizi durante il processo di onboarding.</td>
      <td><i>obbligatorio</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">codice</td>
      <td>Il codice di autenticazione ottenuto dopo la creazione della sessione di autenticazione sul dispositivo di streaming.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorizzazione</td>
      <td>La generazione del payload del token Bearer è descritta nella documentazione dell'intestazione <a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a>.</td>
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
      <td style="background-color: #DEEBFF;">AP-Visitor-Identifier</td>
      <td>
        La generazione del payload dell'identificatore del visitatore è descritta nella documentazione dell'intestazione <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md">AP-Visitor-Identifier</a>.
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accetta</td>
      <td>
         Tipo di supporto accettato dall'applicazione client.
         <br/><br/>
         Se specificato, deve essere application/json;charset=utf-8.
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

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Codice</th>
      <th style="background-color: #EFF2F7;">Testo</th>
      <th style="background-color: #EFF2F7;">Descrizione</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Il corpo della risposta contiene informazioni sulla sessione di autenticazione.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Richiesta non valida</td>
      <td>
        Richiesta non valida. Il client deve correggere la richiesta e riprovare. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Codici di errore avanzati</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non autorizzato</td>
      <td>
        Il token di accesso non è valido, il client deve ottenere un nuovo token di accesso e riprovare. Per ulteriori dettagli, consulta la documentazione <a href="../../../rest-api-dcr/dynamic-client-registration-overview.md">Panoramica registrazione client dinamico</a>.
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
        Si è verificato un problema sul lato server. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Codici di errore avanzati</a>.
      </td>
   </tr>
</table>

### Completato {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>200</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         Oggetto JSON con i seguenti attributi:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">existingParameters</td>
               <td>I parametri esistenti già forniti.</td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>Parametri mancanti da fornire per completare il flusso di autenticazione.</td>
               <td>facoltativo</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">dispositivo</td>
               <td>Le informazioni sul dispositivo relative al dispositivo di streaming effettivo.</td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>Il timestamp in millisecondi prima del quale il codice di autenticazione non è valido.</td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Il timestamp in millisecondi dopo il quale il codice di autenticazione non è valido.</td>
               <td><i>obbligatorio</i></td>
            </tr>
         </table>
      </td>
      <td><i>obbligatorio</i></td>
</table>

### Errore {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
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
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
            Il corpo della risposta può fornire informazioni di errore aggiuntive conformi alla documentazione di <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Codici di errore avanzati</a>.
            <br/><br/>
            L’applicazione client deve implementare un meccanismo di gestione degli errori in grado di elaborare correttamente i codici di errore più comunemente restituiti da questa API:
            <ul>
                <li>invalid_authentication_session</li>
                <li>invalid_parameter_code</li>
                <li>ecc.</li>
            </ul>
            L’elenco di cui sopra non è esaustivo. L'applicazione client deve essere in grado di gestire tutti i codici di errore avanzati definiti nella <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">documentazione pubblica</a>.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

## Esempi {#samples}

### &#x200B;1. Recuperare la sessione di autenticazione senza parametri mancanti

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
GET /api/v2/sessions/REF30/8BLW4RW HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Risposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{        
    "existingParameters": {
        "mvpd": "apassidp",
        "domain": "adobe.com"
        "redirectUrl": "https://www.adobe.com",
        "serviceProvider": "REF30"        
    },
    "device": {
        "type": "Desktop",
        "model": null,
        "version": {
            "major": 0,
            "minor": 0,
            "patch": 0,
            "profile": ""
        },
    "hardware": {
      "name": null,
      "vendor": "Apple",
      "version": {
        "major": 0,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "manufacturer": "Apple"
    },
    "operatingSystem": {
      "name": "macOS",
      "family": "macOS",
      "vendor": "Apple",
      "version": {
        "major": 10,
        "minor": 15,
        "patch": 7,
        "profile": ""
      }
    },
    "browser": {
      "name": "Chrome",
      "vendor": "Google",
      "version": {
        "major": 140,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36",
      "originalUserAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36"
    },
    "display": {
      "width": 0,
      "height": 0,
      "ppi": 0,
      "name": "DISPLAY",
      "vendor": null,
      "version": null,
      "diagonalSize": null
    },
    "applicationId": null,
    "connection": {
      "ipAddress": "...",
      "port": "55161",
      "secure": false,
      "type": null
    }
    }
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"    
}
```

>[!ENDTABS]

### &#x200B;1. Recuperare la sessione di autenticazione con parametri mancanti

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
GET /api/v2/sessions/REF30/8BLW4RW HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Risposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
  "missingParameters": [
    "mvpd"
  ],
  "existingParameters": {
    "redirectUrl": "https://adobe.com",
    "domainName": "adobe.com",
    "serviceProvider": "REF30"
  },
  "device": {
    "type": "Desktop",
    "model": null,
    "version": {
      "major": 0,
      "minor": 0,
      "patch": 0,
      "profile": ""
    },
    "hardware": {
      "name": null,
      "vendor": "Apple",
      "version": {
        "major": 0,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "manufacturer": "Apple"
    },
    "operatingSystem": {
      "name": "macOS",
      "family": "macOS",
      "vendor": "Apple",
      "version": {
        "major": 10,
        "minor": 15,
        "patch": 7,
        "profile": ""
      }
    },
    "browser": {
      "name": "Chrome",
      "vendor": "Google",
      "version": {
        "major": 140,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36",
      "originalUserAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36"
    },
    "display": {
      "width": 0,
      "height": 0,
      "ppi": 0,
      "name": "DISPLAY",
      "vendor": null,
      "version": null,
      "diagonalSize": null
    },
    "applicationId": null,
    "connection": {
      "ipAddress": "...",
      "port": "3061",
      "secure": false,
      "type": null
    }
  },
  "notBefore": "1761299929958",
  "notAfter": "1761301729958"
}
```

>[!ENDTABS]
