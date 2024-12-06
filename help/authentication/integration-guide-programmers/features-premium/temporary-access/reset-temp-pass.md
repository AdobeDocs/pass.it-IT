---
title: Ripristina passaggio temporaneo
description: Ripristina passaggio temporaneo
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---


# Ripristina passaggio temporaneo {#reset-temp-pass}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Prima di utilizzare l’API Reset Temp Pass, verifica che siano soddisfatti i seguenti prerequisiti:
>
> * Ottenere le credenziali del client come descritto nella [Documentazione API per il recupero delle credenziali del client](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Ottenere il token di accesso come descritto nella documentazione API [Recuperare il token di accesso](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Per ulteriori informazioni su come creare un&#39;applicazione registrata e scaricare l&#39;istruzione software, consultare la documentazione [Panoramica registrazione client dinamica](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

Per **reimpostare un passaggio temporaneo specifico per un dispositivo o per tutti i dispositivi**, l&#39;autenticazione Adobe Pass fornisce ai programmatori un&#39;API Web *pubblica*:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

* **Protocollo:** HTTPS
* **Host:**
   * Versione - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Percorso:** /reset-tempass/v3/reset
* **Parametri query:** device_id (se non viene fornito alcun valore, viene impostato automaticamente su all), requestor_id (obbligatorio), mvpd_id (obbligatorio), appId, deviceUser, environment
* **Intestazioni:** Autorizzazione: Bearer &lt;access_token_here>
* **Risposta:**
   * Operazione completata - HTTP 204
   * Errore:xw
      * HTTP 400 per una richiesta non corretta
      * HTTP 401 se l’accesso è negato. Il client DEVE richiedere un nuovo access_token.
      * HTTP 403 se all’ID client non è più consentito eseguire richieste. È necessario generare nuove credenziali client.


Ad esempio:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

Al fine di **reimpostare Flexible Temp Pass per una chiave generica o tutte le chiavi**, l&#39;autenticazione Adobe Pass fornisce ai programmatori un&#39;API Web *public*:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic
```

* **Protocollo:** HTTPS
* **Host:**
   * Versione - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Percorso:** /reset-tempass/v3/reset/generic
* **Parametri query:** chiave (se non viene fornito alcun valore, viene impostato automaticamente su tutti), requestor_id (obbligatorio), mvpd_id (obbligatorio), ambiente
* **Intestazioni:** Autorizzazione: Bearer &lt;access_token_here>
* **Risposta:**
   * Operazione completata - HTTP 204
   * Errore:
      * HTTP 400 per una richiesta non corretta
      * HTTP 401 se l’accesso è negato. Il client DEVE richiedere un nuovo access_token.
      * HTTP 403 se all’ID client non è più consentito eseguire richieste. È necessario generare nuove credenziali client.


Ad esempio, impostando la *chiave generica* su un hash di indirizzo di posta elettronica (per
I file MVPD con passaggio temporaneo che supportano questo tipo di funzionalità) produrranno
seguente chiamata HTTP (l&#39;e-mail è `user@domain.com` il relativo SHA-256
hash `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```
