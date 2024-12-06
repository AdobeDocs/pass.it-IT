---
title: Pagina di registrazione
description: Pagina di registrazione
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# Pagina di registrazione {#registration-page}

## Endpoint REST API {#clientless-endpoints}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!NOTE]
>
> L&#39;implementazione REST API è limitata dal [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

&lt;FQDN_REGGIE>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## Descrizione {#create-reg-code-svc}

Restituisce l&#39;URI del codice di registrazione e della pagina di accesso generati in modo casuale.

| Endpoint | Chiamato <br> da | Input   <br>Parametro | Metodo HTTP <br> | Risposta | HTTP <br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br>Esempio:<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Servizio programmatore <br>o<br>app in streaming | 1. richiedente <br>    (componente percorso)<br>2.  deviceId (Hashed)   <br>    (obbligatorio)<br>3.  device_info/X-Device-Info (obbligatorio)<br>4.  mvpd (facoltativo)<br>5.  ttl (facoltativo)<br> | POST | XML o JSON contenente un codice di registrazione e informazioni o dettagli sull’errore in caso di esito negativo. Vedi gli esempi di seguito. | 201 |

{style="table-layout:auto"}

| Parametro di input | Tipo | Descrizione |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autorizzazione | Valore intestazione <br>: Bearer &lt;access_token> | Token di accesso DCR |
| Accetta | Valore intestazione <br>: application/json | indica il tipo di contenuto che il client deve essere in grado di comprendere |
| richiedente | Parametro query | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Parametro query | Byte ID dispositivo. |
| device_info/<br>X-Device-Info | device_info: Body <br> X-Device-Info: Header | Informazioni sul dispositivo di streaming.<br>**Nota**: questo parametro POTREBBE essere trasmesso come parametro URL_device, ma a causa delle dimensioni potenziali del parametro e delle limitazioni alla lunghezza di un URL di GET, DOVREBBE essere trasmesso come X-Device-Info nell&#39;intestazione http. <br>Visualizza tutti i dettagli in [Trasmissione delle informazioni sul dispositivo e sulla connessione](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md). |
| mvpd | Parametro query | ID MVPD per il quale è valida questa operazione. |
| ttl | Parametro query | Durata di questo codice regcode in secondi.<br>**Nota**: il valore massimo consentito per ttl è di 36000 secondi (10 ore). Valori più alti determinano una risposta HTTP 400 (richiesta non valida). Se `ttl` viene lasciato vuoto, Adobe Pass Authentication imposta il valore predefinito di 30 minuti. |
| _tipoDispositivo_ | Parametro query | Obsoleto, non deve essere utilizzato. |
| _utenteDispositivo_ | Parametro query | Obsoleto, non deve essere utilizzato. |
| _appId_ | Parametro query | Obsoleto, non deve essere utilizzato. |

{style="table-layout:auto"}

>[!CAUTION]
>
>**Indirizzo IP dispositivo di streaming**
><br>
>Per le implementazioni client-server, l&#39;indirizzo IP del dispositivo di streaming viene inviato implicitamente con questa chiamata.  Per le implementazioni server-to-server in cui la chiamata **regcode** è impostata sul servizio Programmatore e non sul dispositivo di streaming, per passare l&#39;indirizzo IP del dispositivo di streaming è necessaria la seguente intestazione:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>dove `<streaming\_device\_ip>` è l&#39;indirizzo IP pubblico del dispositivo di streaming.
><br><br>
>Esempio: <br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
<br>

### JSON di risposta


#### ESEMPI JSON PER CODICE DI REGISTRAZIONE

```JSON
{
  "id": "ef5a79e8-7c8a-41d6-a45a-e378c6c7c8b5",
  "code": "IYQD5JQ",
  "requestor": "sampleRequestorId",
  "mvpd": "sampleMvpdId",
  "generated": 1704963921144,
  "expires": 1704965721144,
  "info": {
    "deviceId": "c28tZGV2aWQtMDAz",
    "deviceInfo": "eyJ0eXBlIjoiU2V0VG9wQm94IiwibW9kZWwiOiJBRlRNTSIsInZlcnNpb24iOnsibWFqb3IiOjAsIm1pbm9yIjowLCJwYXRjaCI6MCwicHJvZmlsZSI6IiJ9LCJoYXJkd2FyZSI6eyJuYW1lIjoiQUZUTU0iLCJ2ZW5kb3IiOiJVbmtub3duIiwidmVyc2lvbiI6eyJtYWpvciI6MCwibWlub3IiOjAsInBhdGNoIjowLCJwcm9maWxlIjoiIn0sIm1hbnVmYWN0dXJlciI6IlJva3UifSwib3BlcmF0aW5nU3lzdGVtIjp7Im5hbWUiOiJBbmRyb2lkIiwiZmFtaWx5IjoiQW5kcm9pZCIsInZlbmRvciI6IkFtYXpvbiIsInZlcnNpb24iOnsibWFqb3IiOjcsIm1pbm9yIjoxLCJwYXRjaCI6MiwicHJvZmlsZSI6IiJ9fSwiYnJvd3NlciI6eyJuYW1lIjoiQ2hyb21lIiwidmVuZG9yIjoiR29vZ2xlIiwidmVyc2lvbiI6eyJtYWpvciI6MTEyLCJtaW5vciI6MCwicGF0Y2giOjU2MTUsInByb2ZpbGUiOiIifSwidXNlckFnZW50IjoiTW96aWxsYS81LjAgKExpbnV4OyBBbmRyb2lkIDcuMS4yOyBBRlRNTSBCdWlsZC9OUzYyOTc7IHd2KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBWZXJzaW9uLzQuMCBDaHJvbWUvMTEyLjAuNTYxNS4xOTcgTW9iaWxlIFNhZmFyaS81MzcuMzYgQWRvYmVQYXNzTmF0aXZlRmlyZVRWLzMuMC44Iiwib3JpZ2luYWxVc2VyQWdlbnQiOiJNb3ppbGxhLzUuMCAoTGludXg7IEFuZHJvaWQgNy4xLjI7IEFGVE1NIEJ1aWxkL05TNjI5Nzsgd3YpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIFZlcnNpb24vNC4wIENocm9tZS8xMTIuMC41NjE1LjE5NyBNb2JpbGUgU2FmYXJpLzUzNy4zNiBBZG9iZVBhc3NOYXRpdmVGaXJlVFYvMy4wLjgifSwiZGlzcGxheSI6eyJ3aWR0aCI6MCwiaGVpZ2h0IjowLCJwcGkiOjAsIm5hbWUiOiJESVNQTEFZIiwidmVuZG9yIjpudWxsLCJ2ZXJzaW9uIjpudWxsLCJkaWFnb25hbFNpemUiOm51bGx9LCJhcHBsaWNhdGlvbklkIjpudWxsLCJjb25uZWN0aW9uIjp7ImlwQWRkcmVzcyI6IjE5My4xMDUuMTQwLjEzMSIsInBvcnQiOiI5OTM0Iiwic2VjdXJlIjpmYWxzZSwidHlwZSI6bnVsbH19",
    "userAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "originalUserAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "authorizationType": "OAUTH2",
    "sourceApplicationInformation": {
      "id": "14138364-application-id",
      "name": "application name",
      "version": "1.0.0"
    }
  }
}
```

<br>

| Nome elemento | Descrizione |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| id | UUID generato da Registration Code Service |
| codice | Codice di registrazione generato da Registration Code Service |
| richiedente | ID richiedente |
| mvpd | ID Mvpd |
| generato | Timestamp di creazione del codice di registrazione (in millisecondi dal 1° gennaio 1970 GMT) |
| scade | Timestamp di scadenza del codice di registrazione (in millisecondi dal 1° gennaio 1970 GMT) |
| deviceId | ID dispositivo univoco Base64 |
| info:deviceId | Tipo di dispositivo Base64 |
| info:deviceInfo | Base64 Normalized Device Information si basa sulle informazioni ricevute dall&#39;agente utente, da X-Device-Info o da device_info |
| info:userAgent | Agente utente inviato dall’applicazione |
| info:originalUserAgent | Agente utente inviato dall’applicazione |
| info:authorizationType | OAUTH2 per chiamate con DCR |
| info:sourceApplicationInformation | Informazioni applicazione configurate in DCR |

{style="table-layout:auto"}


<br>

### Messaggio di errore: esempio di risposta JSON (#error-sample-response)

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

