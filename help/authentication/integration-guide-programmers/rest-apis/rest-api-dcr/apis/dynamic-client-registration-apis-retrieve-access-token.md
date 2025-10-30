---
title: Recupera token di accesso
description: API Dynamic Client Registration - Recupera token di accesso
exl-id: 23287acf-5d56-46f0-b65e-79bf7d667708
source-git-commit: 110e8519d6c042cc38de3fbefcd34297b6edcfad
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 1%

---

# Recupera token di accesso {#retrieve-access-token}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione dell&#39;API Dynamic Client Registration è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Richiesta {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/o/client/token</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">metodo</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parametri corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_id</td>
      <td>
            Stringa dell'identificatore dell'applicazione client.
            <br/><br/>
            Per ulteriori informazioni su come ottenere la stringa dell'identificatore client, consultare la documentazione dell'API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recupera credenziali client</a>.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_secret</td>
      <td>
            Stringa segreta dell'applicazione client.
            <br/><br/>
            Per ulteriori informazioni su come ottenere la stringa del segreto client, consulta la documentazione API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recuperare le credenziali del client</a>.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">grant_type</td>
      <td>
            Stringa del tipo di concessione (ad esempio, "client_credentials") che l’applicazione client può utilizzare per l’endpoint del token client.
            <br/><br/>
            Per ulteriori informazioni su come ottenere la stringa del tipo di concessione, consulta la documentazione API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recuperare le credenziali del client</a>.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
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
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La generazione del payload di informazioni sul dispositivo è descritta nella documentazione di <a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>.
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

### Completato {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>201</td>
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
         Oggetto JSON con i seguenti attributi:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">id</td>
               <td>Identificatore opaco che può essere utilizzato per monitorare l’attività dell’utente.</td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">access_token</td>
               <td>Il valore del token di accesso che l’applicazione client deve utilizzare per l’intestazione Autorizzazione.</td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">created_at</td>
               <td>Tempo in millisecondi in cui è stato emesso il token di accesso.</td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">expires_in</td>
               <td>Il tempo in secondi che deve trascorrere prima della scadenza del token di accesso.</td>
               <td><i>obbligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token_type</td>
               <td>Il tipo di token (ad esempio, "bearer").</td>
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
      <td>400</td>
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
      <td>
        I valori possibili sono:
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Valore</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_request</td>
               <td>
                    La richiesta non è valida per uno dei motivi seguenti:
                    <ul>
                        <li>Nella richiesta manca un parametro obbligatorio.</li>
                        <li>La richiesta include un valore di parametro non supportato (diverso dal tipo di concessione).</li>
                        <li>La richiesta ripete un parametro.</li>
                        <li>La richiesta include più credenziali.</li>
                        <li>La richiesta utilizza più di un meccanismo per l’autenticazione del client.</li>
                        <li>Richiesta non valida.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_client</td>
               <td>Credenziali client non valide. Il client deve ottenere nuove credenziali client e riprovare. Per ulteriori dettagli, consulta la documentazione API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recuperare le credenziali del client</a>.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unauthorized_client</td>
               <td>L'applicazione client non è autorizzata a utilizzare questo tipo di concessione di autorizzazione.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unsupported_grant_type</td>
               <td>Il tipo di concessione di autorizzazione non è supportato dal server autorizzazioni.</td>
            </tr>
         </table>
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

## Esempi {#samples}

### Recupera token di accesso {#samples-retrieve-access-token}

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
POST /o/client/token HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
    
Body:
    
client_id=s6BhdRkqt3&client_secret=t7AkePiru4&grant_type=client_credentials
```

>[!TAB Risposta - Completata]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
  "id": "a932f8f0-210a-41a4-b2a8-377751f6b76f",  
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "created_at": 1752148106221,
  "expires_in": 21600,
  "token_type": "bearer"
}
```

>[!TAB Risposta - Errore]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
