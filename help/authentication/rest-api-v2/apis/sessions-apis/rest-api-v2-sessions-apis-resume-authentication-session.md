---
title: Riprendi sessione di autenticazione
description: REST API V2 - Riprendi sessione di autenticazione
source-git-commit: d59afc0384a1c3617143efcef4ab5fb1a323e511
workflow-type: tm+mt
source-wordcount: '753'
ht-degree: 1%

---


# Riprendi sessione di autenticazione {#resume-authentication-session}

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
      <td>/api/v2/{serviceProvider}/session/{code}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">metodo</td>
      <td>POST</td>
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
      <th style="background-color: #EFF2F7;">Parametri corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>
        Identificatore univoco interno associato al provider di identità durante il processo di onboarding.
        <br/><br/>
        Se la piattaforma del dispositivo di streaming presenta limitazioni nell’offerta di un valore, un’applicazione dovrà riprendere la sessione di autenticazione e fornire un valore valido.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Dominio di origine dell'applicazione che esegue l'accesso MVPD.
        <br/><br/>
        Se la piattaforma del dispositivo di streaming presenta limitazioni nell’offerta di un valore, un’applicazione dovrà riprendere la sessione di autenticazione e fornire un valore valido.
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
        </td>
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
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         Tipo di file multimediale accettato per le risorse inviate.
         <br/><br/>
         Deve essere codificata in application/x-www-form-urlencoded.
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
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Azione che il dispositivo di streaming deve eseguire per completare il flusso di autenticazione.
                  <br/><br/>
                  I valori possibili sono:
                  <ul>
                    <li><b>autentica</b><br/>Il dispositivo di streaming o un altro dispositivo deve aprire l'URL specificato in un agente utente.</li>
                    <li><b>riprova</b><br/>Il dispositivo di streaming o un altro dispositivo deve fornire i parametri mancanti e riprovare a riprendere la sessione di autenticazione utilizzando il codice.</li>
                    <li><b>autorizza</b><br/>Il dispositivo di streaming può procedere direttamente con i flussi decisionali.</li>
                  </ul> 
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  Tipo di interazione che il dispositivo di streaming deve eseguire per continuare il flusso con l’azione specificata dall’attributo "actionName".
                  <br/><br/>
                  I valori possibili sono:
                  <ul>
                    <li><b>interattivo</b><br/>Il flusso continua con la navigazione all'URL specificato tramite un agente utente.</li>
                    <li><b>diretto</b><br/>Il flusso continua con una chiamata diretta all'URL fornito tramite un client HTTP disponibile per l'implementazione del client.</li>
                  </ul>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>Parametri mancanti da fornire per completare il flusso di autenticazione di base.</td>
               <td>facoltativo</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>URL in cui l'applicazione client deve spostarsi.</td>
               <td>facoltativo</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">codice</td>
               <td>Codice di autenticazione che può essere utilizzato in un'applicazione secondaria per riprendere la sessione di autenticazione.</td>
               <td><i>obbligatorio</i></td>
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

### 1. Riprendi la sessione di autenticazione senza fornire valori per tutti i parametri richiesti

>[!BEGINTABS]

>[!TAB Richiesta]

```JSON
POST /api/v2/REF30/sessions/8BLW4RW
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com
```

>[!TAB Risposta]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "retry",
   "actionType" : "interactive",
   "url" : "/v2/REF30/sessions/8BLW4RW",
   "missingParameters" : ["redirectUrl"]
   "code" : "8BLW4RW",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]
