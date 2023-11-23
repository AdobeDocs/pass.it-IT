---
title: Ripristina passaggio temporaneo
description: Ripristina passaggio temporaneo
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Ripristina passaggio temporaneo {#reset-temp-pass}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.
>
>Per utilizzare l’API Reset Temp Pass, è necessario:
>- chiedere al team di supporto un rendiconto software per l&#39;applicazione registrata
>- ottenere un token di accesso basato su [Registrazione client dinamici](dynamic-client-registration.md)
> 

Per **reimpostare un valore Temp Pass specifico**, l’autenticazione Adobe Pass fornisce ai programmatori un *pubblico* API web:

- **Ambiente:** specifica l&#39;endpoint Adobe del server di pass Pay-TV che riceverà la chiamata di rete Reset Temp Pass. Valori possibili: **Prequal** (*mgmt-prequal.auth.adobe.com*), **Versione** (*mgmt.auth.adobe.com*) o **Personalizzato** (riservato ad Adobe ai test interni).
- **Token di accesso OAuth2:** il token OAuth2 è necessario per autorizzare il programmatore, ad Adobe per l’autenticazione a pagamento. Tale token può essere ottenuto da [Registrazione client dinamici](dynamic-client-registration.md).
- **ID passaggio temporaneo:** l’ID univoco dell’MVPD di passaggio temporaneo da reimpostare.(un programmatore può utilizzare più MVPD Temp Pass e vuole reimpostare uno specifico)
- **Chiave generica:** alcuni MVPD di Temp Pass (ad es. [Passaggio temporaneo promozionale](promotional-temp-pass.md)).

Tutti i parametri di cui sopra (ad eccezione di *Chiave generica*) sono obbligatori. Ecco un esempio di parametri e la chiamata di rete associata (l&#39;esempio è sotto forma di un comando *curl *):

- **Ambiente:** Versione (*mgmt.auth.adobe.com*)
- **Token di accesso OAuth2:** &lt;access_token> da [Registrazione client dinamici](dynamic-client-registration.md)
- **ID programmatore:** RIF
- **ID passaggio temporaneo:** TempPassREF
- **Chiave generica:** null (nessun valore specificato)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

verrà effettuata una richiesta HTTP di DELETE al **/reset** endpoint, passaggio di *Token di accesso OAuth2* nell’intestazione Autorizzazione e nella sezione *ID dispositivo*, *ID richiedente* e *ID passaggio temporaneo (ID MVPD)* come parametri.

Se il programmatore fornisce un valore per *Chiave generica*, verrà eseguita un&#39;altra chiamata HTTP (questa volta al **/reset/generic** endpoint), passando il *Chiave generica* all&#39;interno del *chiave* parametro di richiesta.

Ad esempio, impostando *Chiave generica* a un hash di indirizzo e-mail (per gli MVPD che supportano questo tipo di funzionalità) genererà la seguente chiamata HTTP (l’e-mail è `user@domain.com` il relativo hash SHA-256 è `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Per **reimpostare un passaggio temporaneo specifico per tutti i dispositivi**, l’autenticazione Adobe Pass fornisce ai programmatori un *pubblico* API web:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>L’URL precedente sostituisce l’API di ripristino precedente. La vecchia API di ripristino (v1) non è più supportata.

- **Protocollo:** HTTPS
- **Host:**
   - Versione - mgmt.auth.adobe.com
   - Prequal - mgmt-prequal.auth.adobe.com
- **Percorso:** /reset-tempass/v3/reset
- **Parametri query:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
- **Intestazioni:** Autorizzazione: Bearer &lt;access_token_here>
- **Risposta:**
   - Operazione completata - HTTP 204
   - Errore:
      - HTTP 400 per una richiesta non corretta
      - HTTP 401 se l’accesso è negato. Il client DEVE richiedere un nuovo access_token.
      - HTTP 403 se all’ID client non è più consentito eseguire richieste. È necessario generare nuove credenziali client.


Ad esempio:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
