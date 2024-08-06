---
title: Recuperare i profili
description: REST API V2 - Recupero profili
source-git-commit: dc9fab27c7eced2be5dd9f364ab8f2d64f8e4177
workflow-type: tm+mt
source-wordcount: '824'
ht-degree: 1%

---


# Recuperare i profili {#retrieve-profiles}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione REST API V2 è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/throttling-mechanism.md).

## Richiesta {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/api/v2/{serviceProvider}/profiles</td>
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
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorizzazione</td>
      <td>La generazione del payload del token Bearer è descritta nella documentazione di <a href="../../../dynamic-client-registration-api.md">Registrazione client dinamica</a>.</td>
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
        Per ulteriori dettagli sui flussi abilitati per il Single Sign-On che utilizzano un'identità di piattaforma, fare riferimento alla documentazione <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">Single Sign-On che utilizza flussi di identità di piattaforma</a>.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        La generazione del payload Single Sign-On per il metodo Service Token è descritta nella documentazione di <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>.
        <br/><br/>
        Per ulteriori dettagli sui flussi abilitati per il Single Sign-On tramite un token di servizio, fare riferimento alla documentazione <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md">Single Sign-On tramite flussi di token di servizio</a>.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        La generazione del payload Single Sign-On per il metodo Partner è descritta nella documentazione di <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a>.
        <br/><br/>
        Per ulteriori dettagli sui flussi abilitati per il Single Sign-On tramite un partner, fare riferimento alla documentazione <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md">Single Sign-On tramite flussi partner</a>.</td>
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
        Il corpo della risposta contiene una mappa di profili validi, che può essere vuota.
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
      <td style="background-color: #DEEBFF;">profili</td>
      <td>
        JSON contenente una mappa di coppie chiave-valore.
        <br/><br/>
        L’elemento chiave è definito dal seguente valore:
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Valore</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Identificatore univoco interno associato al provider di identità durante il processo di onboarding.</td>
               <td><i>obbligatorio</i></td>
            </tr>
         </table>
         L'elemento value è definito dai seguenti attributi:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>Il timestamp prima del quale il profilo non è valido.</td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Il timestamp dopo il quale il profilo non è valido.</td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">emittente</td>
               <td>
                  Entità a cui appartiene il profilo.
                  <br/><br/>
                  I valori possibili sono:
                  <ul>
                    <li><b>mvpd (ad esempio, Spectrum, Cablevision e così via)</b><br/>Il profilo è stato creato in seguito a: autenticazione di base, Single Sign-On tramite l'identità della piattaforma o Single Sign-On tramite il token di servizio.</li>
                    <li><b>Apple</b><br/>Il profilo è stato creato in seguito a: single sign-on con il partner Apple.</li>
                  </ul>
               </td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">tipo</td>
               <td>
                  Tipo del profilo.
                  <br/><br/>
                  I valori possibili sono:
                  <ul>
                    <li><b>regolare</b><br/>Il profilo è stato creato come risultato di: autenticazione di base.</li>
                    <li><b>appleSSO</b><br/>Il profilo è stato creato in seguito a: single sign-on con Apple partner.</li>
                    <li><b>platformSSO</b><br/>Il profilo è stato creato come risultato di: single sign-on utilizzando l'identità della piattaforma.</li>
                    <li><b>serviceTokenSSO</b><br/>Il profilo è stato creato in seguito a: Single Sign-On utilizzando un token di servizio.</li>
                  </ul>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">attributi</td>
               <td>
                    L’elenco degli attributi dei metadati utente.
                    <br/><br/>
                    Questi attributi possono essere:
                    <ul>
                        <li>Obbligatorio, come "userId"</li>
                        <li>Non obbligatorio, come "zip", "familyId", "maxRating", ecc.</li>
                    </ul>
                    I valori per gli attributi possono essere:
                    <ul>
                        <li>semplice</li>
                        <li>list</li>
                        <li>mappa</li>
                    </ul>
               </td>
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
      <td style="background-color: #DEEBFF;">errore</td>
      <td>L'errore fornisce informazioni aggiuntive conformi alla documentazione di <a href="../../../enhanced-error-codes.md">Codici di errore avanzati</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

## Esempi {#samples}

### 1. Recupera tutti i profili autenticati esistenti e validi ottenuti tramite autenticazione di base

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
GET /api/v2/REF30/profiles
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Risposta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "profiles" : {
        "Cablevision" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Cablevision",
            "type" : "regular",
            "attributes" : {
                "userId" : {
                    "value" : "BASE64_value_userId",
                    "state" : "plain"
                },
                "householdId" : {
                    "value" : "BASE64_value_householdId",
                    "state" : "plain"
                },
                "zip" : {
                    "value" : "BASE64_value_zip",
                    "state" : "enc"
                },
                "parental-controls" : {
                    "value" : BASE64_value_parental-controls,
                    "state" : "plain"
                }          
            }
        },
        "Spectrum" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Spectrum",
            "type" : "regular",
            "attributes" : {
                "userId" : {
                    "value" : "BASE64_value_userId",
                    "state" : "plain"
                }
            }
        }
     }
}
```

>[!ENDTABS]

### 2. Recupera tutti i profili autenticati esistenti e validi, inclusi quelli ottenuti tramite l’autenticazione single sign-on utilizzando il metodo Service Token

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
GET /api/v2/REF30/profiles
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
AD-Service-Token : eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJkZDNmYWIyN2NmMjg0ZmU2ZWU0ZDY3ZmExZjY4MzE3YyIsImlzcyI6IkFkb2JlIiw.....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Risposta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
   "profiles": {
      "AdobeShibboleth": {
         "notBefore": 1748073636999,
         "notAfter": 1748105173000,
         "issuer": "AdobeShibboleth",
         "type": "serviceTokenSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd1...",
               "state": "plain"
            },
            "userID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd14aTV....",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobeShibboleth",
               "state": "plain"
            }
         }
      },
      "Spectrum": {
         "notBefore": 1623943955,
         "notAfter": 1623951155,
         "issuer": "Spectrum",
         "type": "regular",
         "attributes": {
            "userId": {
               "value": "BASE64_value_userId",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 3. Recupera tutti i profili autenticati esistenti e validi, inclusi quelli ottenuti tramite l’autenticazione single sign-on utilizzando il metodo Platform Identity

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
GET /api/v2/REF30/profiles
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Adobe-Subject-Token : eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiIyMmM4MDU1MjEzMDIwYzhmZGYzOGZkMTI1YWViMzUzYSIsImlzcyI6....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Risposta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8    
 
{
   "profiles": {
      "AdobePass_SMI": {
         "notBefore": 1724337476000,
         "notAfter": 1724345252000,
         "issuer": "AdobePass_SMI",
         "type": "platformSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "userID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobePass_SMI",
               "state": "plain"
            }
         }
      },
      "Cablevision": {
         "notBefore": 1623943955,
         "notAfter": 1623951155,
         "issuer": "Spectrum",
         "type": "regular",
         "attributes": {
            "userId": {
               "value": "BASE64_value_userId",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]
