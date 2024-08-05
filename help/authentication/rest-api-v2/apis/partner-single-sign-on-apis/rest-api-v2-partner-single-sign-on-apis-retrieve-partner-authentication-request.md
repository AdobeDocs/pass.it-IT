---
title: Recupera richiesta di autenticazione partner
description: REST API V2 - Recupera richiesta di autenticazione partner
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '1104'
ht-degree: 0%

---


# Recupera richiesta di autenticazione partner {#retrieve-partner-authentication-request}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione REST API V2 è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/throttling-mechanism.md).

## Richiesta {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/api/v2/{serviceProvider}/session/sso/{partner}</td>
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
      <td style="background-color: #DEEBFF;">partner</td>
      <td>Il nome del partner (ad esempio, Apple) che fornisce il framework single sign-on integrato con i flussi di autenticazione di Adobe Pass.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Parametri corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Dominio di origine dell'applicazione che esegue l'accesso MVPD.
        <br/><br/>
        Se la piattaforma del dispositivo di streaming presenta limitazioni nell’offerta di un valore, un’applicazione dovrà riprendere la sessione di autenticazione e fornire un valore valido.
        <br/><br/>
        Questo verrà utilizzato in caso di scenari di fallback in cui la risposta indica che l’applicazione di streaming deve procedere con il flusso di autenticazione di base.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        L’URL di reindirizzamento finale a cui si sposta l’agente utente al completamento del flusso di autenticazione per MVPD.
        <br/><br/>
        Il valore deve essere codificato in URL.
        <br/><br/>
        Se la piattaforma del dispositivo di streaming presenta limitazioni nell’offerta di un valore, un’applicazione dovrà riprendere la sessione di autenticazione e fornire un valore valido.
        <br/><br/>
        Questo verrà utilizzato in caso di scenari di fallback in cui la risposta indica che l’applicazione di streaming deve procedere con il flusso di autenticazione di base.
      </td>
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
         Deve essere codificata in application/x-www-form-urlencoded.
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
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        La generazione del payload Single Sign-On per il metodo Partner è descritta nella documentazione di <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a>.
        <br/><br/>
        Per ulteriori dettagli sui flussi abilitati per il Single Sign-On tramite un partner, fare riferimento alla documentazione <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md">Single Sign-On tramite flussi partner</a>.</td>
      <td>facoltativo</td>
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
        Il corpo della risposta contiene informazioni sulle azioni successive necessarie per eseguire l’autenticazione.
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
      <td style="background-color: #DEEBFF;"></td>
      <td>
         Oggetto JSON con i seguenti attributi:
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Attributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Azione che il dispositivo di streaming deve eseguire per completare il flusso di autenticazione.
                  <br/><br/>
                  I valori possibili sono:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Valore</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">partner_profile</td>
                        <td>Il dispositivo di streaming può utilizzare la richiesta di autenticazione partner fornita per ottenere una risposta di autenticazione partner che può essere utilizzata per recuperare un profilo.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">autenticare</td>
                        <td>
                            Quando il flusso Single Sign-On del partner non riesce a procedere, il dispositivo di streaming può tornare al flusso di autenticazione di base.
                            <br/><br/>
                            Il dispositivo di streaming o un altro dispositivo deve aprire l’URL fornito in un agente utente.
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">ripresa</td>
                        <td>
                            Quando il flusso Single Sign-On del partner non riesce a procedere, il dispositivo di streaming può tornare al flusso di autenticazione di base.
                            <br/><br/>
                            Il dispositivo di streaming o un altro dispositivo deve fornire i parametri mancanti e riprendere la sessione di autenticazione utilizzando il codice.
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">autorizzare</td>
                        <td>Il dispositivo di streaming può procedere direttamente con i flussi decisionali.</td>
                     </tr>
                  </table>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  Tipo di interazione che il dispositivo di streaming deve eseguire per continuare il flusso con l’azione specificata dall’attributo "actionName".
                  <br/><br/>
                  I valori possibili sono:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Valore</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">diretto</td>
                        <td>Il flusso continua con una chiamata diretta all’URL fornito tramite un client HTTP disponibile per l’implementazione del client.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">interattivo</td>
                        <td>Il flusso continua con la navigazione all’URL specificato tramite un agente utente.</td>
                     </tr>
                  </table>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>
                    Parametri mancanti da fornire per completare il flusso di autenticazione di base.
                    <br/><br/>
                    Questo campo è presente quando il flusso Single Sign-On del partner non può procedere.
               </td>
               <td>facoltativo</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>URL in cui l'applicazione client deve spostarsi.</td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">codice</td>
               <td>
                    Codice di autenticazione che può essere utilizzato in un'applicazione secondaria per riprendere la sessione di autenticazione.
                    <br/><br/>
                    Questo campo è presente quando il flusso Single Sign-On del partner non può procedere.
               </td>
               <td>facoltativo</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">authenticationRequest</td>
               <td>
                    La richiesta di autenticazione del partner da utilizzare nel flusso di autenticazione con il partner al di fuori del sistema di autenticazione di Adobe Pass.
                    <br/><br/>
                    Questo campo è presente quando il flusso Single Sign-On del partner può procedere.
                    <br/><br/>
                    Oggetto JSON con i seguenti attributi:
                    <table>
                        <tr>
                            <th style="background-color: #EFF2F7; width: 30%;">Attributo</th>
                            <th style="background-color: #EFF2F7;"></th>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">tipo</td>
                            <td>
                                Indica il tipo di protocollo supportato da MVPD.
                                <br/><br/>
                                I valori possibili sono:
                                <table>
                                    <tr>
                                        <th style="background-color: #EFF2F7; width: 30%;">Valore</th>
                                        <th style="background-color: #EFF2F7;"></th>
                                    </tr>
                                    <tr>
                                        <td style="background-color: #DEEBFF;">saml</td>
                                        <td>MVPD supporta il protocollo SAML.</td>
                                    </tr>
                                </table>
                            </td>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">richiesta</td>
                            <td>La richiesta SAML.</td>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">attributi</td>
                            <td>Attributi della richiesta SAML.</td>
                        </tr>
                    </table>
               </td>
               <td>facoltativo</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>Identificatore opaco che può essere utilizzato per monitorare l’attività dell’utente.</td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Identificatore univoco interno associato al provider di identità durante il processo di onboarding.</td>
               <td>facoltativo</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>Identificatore univoco interno associato al provider di servizi durante il processo di onboarding.</td>
               <td><i>obbligatorio</i></td>
            </tr>
         </table>
      </td>
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

### 1. SSO partner valido abilitato

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Risposta]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"partner_profile",
   "actionType":"direct",
   "url":"/v2/REF30/profiles/sso/Apple/Cablevision",
   "sessionId":"83c046be-ea4b-4581-b5f2-13e56e69dee9",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30",
   "authenticationRequest":{
      "type":"saml",
      "request":"PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRG...."
   }
}    
```

>[!ENDTABS]

### 2. MVPD degradato

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Risposta]

```JSON
HTTP/1.1 200 OK
 
{          
   "actionName" : "authorize",
   "actionType" : "direct",
   "url" : "/api/v2/REF30/decisions",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30",
   "sessionId":"14d4f239-e3b1-4a4a-b8b3-6395b968a260"
}
```

>[!ENDTABS]

### 3. Integrazione disabilitata

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Risposta]

```JSON
HTTP/1.1 403
Content-Type: application/json; charset=utf-8
 
{
    "errors" : [
        {
            "code": "unknown_integration",
            "message": "The integration between the specified programmer and identity provider doesn't exist or it's disabled. Use the TVE Dashboard to register or enable the required integration.",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"        
        } 
    ]
}
```

>[!ENDTABS]

### 4. SSO partner non abilitato (nella configurazione Pass o su VSA): fallback all’autenticazione regolare, tutti i parametri richiesti presenti

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status : ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Risposta]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"authenticate",
   "actionType":"interactive",
   "url":"/v2/authenticate/REF30/OKTWW2W",
   "code":"OKTWW2W",
   "sessionId":"748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}  
```

>[!ENDTABS]

### 5. SSO partner non abilitato (nella configurazione Pass o su VSA): fallback all’autenticazione regolare, parametri richiesti mancanti, la sessione di autenticazione deve riprendere

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:
        
domainName=adobe.com
```

>[!TAB Risposta]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"resume",
   "actionType":"direct",
   "missingParameters":[
      "redirectUrl"
   ],
   "url":"/v2/REF30/sessions/SB7ZRIO",
   "code":"SB7ZRIO",
   "sessionId":"1476173f-5088-43b8-b7c3-8cf3a185de0a",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}
```

>[!ENDTABS]
