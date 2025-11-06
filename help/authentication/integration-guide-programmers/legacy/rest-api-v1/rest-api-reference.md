---
title: Riferimento API REST
description: Riferimento API REST
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 2%

---

# Riferimento API REST (legacy) {#rest-api-reference}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Meccanismo di limitazione

L&#39;API REST per l&#39;autenticazione di Adobe Pass è gestita da un [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Formati di risposta {#response-formats}


>[!NOTE]
>
> Le API fornite in questi servizi possono restituire risposte in XML o JSON (per le API che restituiscono una risposta). Esistono 3 modi diversi per specificare il formato della risposta nella richiesta:
>
>* Impostare HTTP Accept Header su `application/xml` o `application/json`.
>* Nel payload della richiesta, specifica il parametro `format=xml` o `format=json`.
>* Chiamare l&#39;endpoint del servizio Web con estensione `.xml` o `.json`. Ad esempio, `/regcode.xml` o `/regcode.json`
>
>È possibile specificare uno qualsiasi dei metodi descritti sopra. La specifica di più metodi con formati in conflitto può causare errori o output indesiderato.

## Endpoint REST API {#clientless-endpoints}

&lt;FQDN_REGGIE>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Riepilogo servizi Web {#web_srvs_summary}

La tabella seguente elenca i servizi web disponibili per l’approccio senza client. Fai clic sugli endpoint dei servizi web per ulteriori informazioni (richiesta e risposta di esempio, parametri di input, metodi HTTP, ecc.)


| Sr | Endpoint servizio Web | Descrizione | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | Ospitato da | Chiamato da |
|-----|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------|
| 1. | [&lt;REGGIE_FQDN>/reggie/v1/ </br>  {requestorId}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | Restituisce l&#39;URI del codice di registrazione e della pagina di accesso generato in modo casuale | 2 | Servizio codice di registrazione </br>Adobe | Smart Device |
| 2. | [&lt;REGGIE_FQDN>/reggie/v1/ </br>  {requestorId}/regcode/ </br>{registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | Restituisce il record del codice di registrazione contenente il codice di registrazione UUID, il codice di registrazione e l&#39;ID dispositivo con hash | 8 | Servizio codice di registrazione </br>Adobe | Autenticazione Adobe Pass |
| 3. | [&lt;SP_FQDN>/api/v1/config/ </br>{requestorId}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | Restituisce l’elenco degli MVPD configurati per il richiedente | 5 | Adobe </br>Adobe Pass </br>autenticazione </br>Servizio | Accedi a </br>Web </br>App |
| 4. | [&lt;SP_FQDN>/api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | Avvia il processo AuthN informando l&#39;evento di selezione MVPD. Crea un record nel database di autenticazione, che viene riconciliato quando viene ricevuta una risposta corretta da MVPD (passaggio 13) | 7 | Adobe </br>Adobe Pass </br>autenticazione </br>Servizio | Accedi a </br>Web </br>App |
| 5. | Consumer asserzione SAML | Flusso di lavoro SAML esistente tra autenticazione Adobe Pass e MVPD | 13 | Servizio di autenticazione </br> di Adobe Pass </br> | Autenticazione Adobe Pass |
| 6. | [&lt;SP_FQDN>/api/v1/checkauthn/ </br>{registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | L’app web di accesso può verificare se il tentativo di flusso di accesso è riuscito |                                                                                             | Autenticazione di Adobe Pass </br>   </br>Servizio | Login   </br>Web   </br>App |
| 7. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Ottiene i metadati relativi al token di autenticazione | 15 | Servizio di autenticazione </br> di Adobe Pass </br> | Smart Device |
| 8. | [&lt;REGGIE_FQDN>/reggie/v1/ </br>  {requestorId}/regcode/ </br>{registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md) | Elimina il record del codice reg e rilascia il codice reg per il riutilizzo | 16 | Servizio codice di registrazione </br>Adobe | Autenticazione Adobe Pass |
| 9. | [&lt;SP_FQDN>/api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | Ottiene la risposta di autorizzazione. | 17 | Servizio di autenticazione </br> di Adobe Pass </br> | Smart Device |
| 10. | [&lt;SP_FQDN>/api/v1/checkauthn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) | Indica se il dispositivo ha un token AuthN non scaduto. |                                                                                             | Servizio di autenticazione </br> di Adobe Pass </br> | Smart Device |
| 11 | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Se trovato, restituisce il token AuthN. |                                                                                             | Servizio di autenticazione </br> di Adobe Pass </br> | Smart Device |
| 12 | [&lt;SP_FQDN>/api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | Se trovato, restituisce il token AuthZ. |                                                                                             | Servizio di autenticazione </br> di Adobe Pass </br> | Smart Device |
| 13 | [&lt;SP_FQDN>/api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | Se trovato, restituisce il token multimediale breve: come /api/v1/mediatoken |                                                                                             | Servizio di autenticazione </br> di Adobe Pass </br> | Smart Device |
| 14 | [&lt;SP_FQDN>/api/v1/mediatoken](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | Ottiene un token multimediale breve |                                                                                             | Servizio di autenticazione </br> di Adobe Pass </br> | Smart Device |
| 15 | [&lt;SP_FQDN>/api/v1/preauthorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) | Recupera l’elenco delle risorse preautorizzate |                                                                                             | Servizio di autenticazione </br> di Adobe Pass </br> | Smart Device |
| 16 | [&lt;SP_FQDN>/api/v1/preauthorize/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | Recupera l’elenco delle risorse preautorizzate |                                                                                             | Servizio di autenticazione </br> di Adobe Pass </br> | Accedi all’app web |
| 17 | [&lt;SP_FQDN>/api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | Rimuovere i token AuthN e AuthZ dall&#39;archiviazione |                                                                                             | Autenticazione di Adobe Pass </br>   </br>Servizio | Smart Device |
| 18 | [&lt;SP_FQDN>/api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Ottiene i metadati utente al termine del flusso di autenticazione | N/D | N/D | Smart Device |
| 19. | [&lt;SP_FQDN>/api/v1/authenticate/freepreview](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Creare un token di autenticazione per Passaggio temporaneo o Passaggio temporaneo promozionale | N/D | Servizio di autenticazione </br> di Adobe Pass </br> | Smart Device |


## Sicurezza REST API {#security}

Tutte le API REST di autenticazione di Adobe Pass devono essere chiamate utilizzando il protocollo HTTPS per una comunicazione sicura. Inoltre, la maggior parte delle API chiamate deve contenere un token di accesso ottenuto come descritto nella documentazione API [Recupera token di accesso](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
