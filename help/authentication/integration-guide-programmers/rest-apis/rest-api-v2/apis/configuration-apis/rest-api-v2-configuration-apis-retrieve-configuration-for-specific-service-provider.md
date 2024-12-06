---
title: Recupera la configurazione per un provider di servizi specifico
description: 'REST API V2: recupero della configurazione per un provider di servizi specifico'
exl-id: ad7e4c6d-ed96-4ae7-82a9-3c24e5fc9302
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 2%

---

# Recupera la configurazione per un provider di servizi specifico {#retrieve-configuration-for-specific-service-provider}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione REST API V2 è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Richiesta {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/api/v2/{serviceProvider}/configuration</td>
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
      <th style="background-color: #EFF2F7;">Parametri di query</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">profilo</td>
      <td>-</td>
      <td>facoltativo</td>
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
      <td style="background-color: #DEEBFF;">AP-Device-Id</td>
      <td>La generazione del payload dell'identificatore del dispositivo è descritta nella documentazione dell'intestazione <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La generazione del payload di informazioni sul dispositivo è descritta nella documentazione dell'intestazione <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>.
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
        Il corpo della risposta contiene un elenco di MVPD con un'integrazione attiva con il 'serviceProvider'.
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
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
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
         JSON contenente un elenco di elementi, ciascuno con i seguenti attributi:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">dispositivo</td>
                <td>Tipo di dispositivo</td>
                <td><i>obbligatorio</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">clientType</td>
                <td>Tipo di client</td>
                <td></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">errorReporting</td>
                <td>Oggetto</td>
                <td></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">richiedente</td>
                <td>
                    Oggetto JSON con i seguenti attributi:
                    <ul>
                        <li><b>id</b></li>
                        <li><b>name</b></li>
                        <li><b>domini</b></li>
                    </ul>
                </td>
                <td><i>obbligatorio</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">mvpds</td>
                <td>
                    Oggetto JSON con i seguenti attributi:
                    <ul>
                        <li><b>id</b></li>
                        <li><b>displayName</b></li>
                        <li><b>logoUrl</b></li>
                        <li><b>isTempPass</b></li>
                        <li><b>isProxy</b></li>
                        <li><b>boardingStatus</b></li>
                        <li><b>platformMappingId</b></li>
                        <li><b>enablePlatformServices</b></li>
                        <li><b>displayInPlatformPicker</b></li>
                        <li><b>enforcePlatformPermissions</b></li>
                    </ul>
                </td>
                <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">tempo</td>
               <td></td>
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
      <td>Il corpo della risposta può fornire informazioni di errore aggiuntive conformi alla documentazione di <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Codici di errore avanzati</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

## Esempi {#samples}

### 1. Recuperare la configurazione per un provider di servizi specifico

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
GET /api/v2/REF30/configuration/ HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Risposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8
 
{
    "device": "unknown",
    "clientType": "html5",
    "os": "Unknown",
    "requestor": {
        "id": "REF30",
        "name": "Reference site only in 30",
        "domains": [
            {
                "name": "adobe.com",
                "mvpdInitiated": false
            },
            {
                "name": "adobe.io",
                "mvpdInitiated": false
            },
            {
                "name": "adobepass.com",
                "mvpdInitiated": false
            },
            {
                "name": "adobeptime.com",
                "mvpdInitiated": false
            },
            {
                "name": "anvilcreative.com",
                "mvpdInitiated": false
            },
            {
                "name": "testadobe.com",
                "mvpdInitiated": false
            }
        ],
        "mvpds": [
            {
                "id": "AdobePass_SMI",
                "displayName": "Adobe Pass SMI",
                "logoUrl": "https://blogs.adobe.com/conversations/files/2010/08/adobe-logo.jpg",
                "authPerAggregator": false
            },
            {
                "id": "TempPass_TEST40",
                "displayName": "Adobe Temp Pass Test 3 min",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "authPerAggregator": true,
                "isTempPass": true
            },
            {
                "id": "TempPass_TEST44",
                "displayName": "Adobe Temp Pass Test 30 min",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "authPerAggregator": true,
                "isTempPass": true
            },
            {
                "id": "AdobeShibboleth",
                "displayName": "AdobeShibboleth",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/adobe.png"
            },
            {
                "id": "ATTOTT",
                "displayName": "DIRECTV STREAM",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/directvstream.jpg"
            },
            {
                "id": "ElasticSSO",
                "displayName": "ElasticSSO",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "authPerAggregator": false
            },
            {
                "id": "TempPass",
                "displayName": "Temp-Pass",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "passiveAuthnEnabled": false,
                "authPerAggregator": true,
                "isTempPass": true
            },
            {
                "id": "Comcast_SSO_Perf",
                "displayName": "Xfinity Perf",
                "logoUrl": "https://login.comcast.net/static/images/ci/tve/mvpd_comcast_logo112x33.gif",
                "authPerAggregator": true,
                "authPerBrowserSession": true
            }
        ]
    }
}  
```

>[!ENDTABS]
