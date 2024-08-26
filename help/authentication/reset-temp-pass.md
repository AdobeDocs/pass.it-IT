---
title: Ripristina passaggio temporaneo
description: Ripristina passaggio temporaneo
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# Ripristina passaggio temporaneo {#reset-temp-pass}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Prima di utilizzare l’API Reset Temp Pass, verifica che siano soddisfatti i seguenti prerequisiti:
>
> * Ottenere le credenziali del client come descritto nella [Documentazione API per il recupero delle credenziali del client](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Ottenere il token di accesso come descritto nella documentazione API [Recuperare il token di accesso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Per ulteriori informazioni su come creare un&#39;applicazione registrata e scaricare l&#39;istruzione software, consultare la documentazione [Panoramica registrazione client dinamica](./dcr-api/dynamic-client-registration-overview.md).

Per **reimpostare un passaggio temporaneo specifico**, l&#39;autenticazione Adobe Pass fornisce ai programmatori un&#39;API Web *public*:

* **Ambiente:** specifica l&#39;endpoint Adobe del server di passaggio PayTV che riceverà la chiamata di rete per reimpostare il passaggio Temp. Valori possibili: **Prequal** (*mgmt* prequal.auth.adobe.com*), **Release** (*mgmt.auth.adobe.com*) o **Custom** (riservati, ad Adobe, ai test interni).
* **Token di accesso OAuth2:** il token OAuth2 è necessario per autorizzare il programmatore, ad Adobe per l&#39;autenticazione a pagamento. Tale token può essere ottenuto come descritto nella documentazione API [Recupera token di accesso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
* **ID passaggio temporaneo:** l&#39;ID univoco per l&#39;MVPD del passaggio temporaneo da reimpostare.(un programmatore può utilizzare più MVPD Temp Pass e vuole reimpostare uno specifico)
* **Chiave generica:** alcuni MVPD di passaggio temporaneo (ovvero [Passaggio temporaneo promozionale](promotional-temp-pass.md)).

Tutti i parametri di cui sopra (ad eccezione della *chiave generica*) sono obbligatori. Ecco un esempio di parametri e la chiamata di rete associata (l&#39;esempio è sotto forma di un comando *curl *):

* **Ambiente:** Versione (*mgmt.auth.adobe.com*)
* **Token di accesso OAuth2:** &lt;token di accesso> come descritto nella documentazione dell&#39;API [Recupera token di accesso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* **ID programmatore:** REF
* **ID passaggio temporaneo:** TempPassREF
* **Chiave generica:** null (nessun valore specificato)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

verrà effettuata una richiesta HTTP DELETE all&#39;endpoint **/reset**, passando il *Token di accesso OAuth2* nell&#39;intestazione Autorizzazione e il *ID dispositivo*, *ID richiedente* e *ID passaggio temporaneo (ID MVPD)* come parametri.

Se il programmatore fornisce un valore per la *chiave generica*, verrà eseguita un&#39;altra chiamata HTTP (questa volta all&#39;endpoint **/reset/generic**), passando la *chiave generica* nel parametro della richiesta *chiave*.

Ad esempio, impostando la *chiave generica* su un hash di indirizzo di posta elettronica (per
I file MVPD con passaggio temporaneo che supportano questo tipo di funzionalità) produrranno
seguente chiamata HTTP (l&#39;e-mail è `user@domain.com` il relativo SHA-256
hash `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Per **reimpostare un passaggio temporaneo specifico per tutti i dispositivi**, l&#39;autenticazione Adobe Pass fornisce ai programmatori un&#39;API Web *pubblica*:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>L’URL precedente sostituisce l’API di ripristino precedente. La vecchia API di ripristino (v1) non è più supportata.

* **Protocollo:** HTTPS
* **Host:**
   * Versione - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Percorso:** /reset-tempass/v3/reset
* **Parametri query:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **Intestazioni:** Autorizzazione: Bearer &lt;access_token_here>
* **Risposta:**
   * Operazione completata - HTTP 204
   * Errore:
      * HTTP 400 per una richiesta non corretta
      * HTTP 401 se l’accesso è negato. Il client DEVE richiedere un nuovo access_token.
      * HTTP 403 se all’ID client non è più consentito eseguire richieste. È necessario generare nuove credenziali client.


Ad esempio:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
