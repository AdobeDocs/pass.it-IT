---
title: Anteprima gratuita per Passaggio temporaneo e Passaggio temporaneo promozionale
description: Anteprima gratuita per Passaggio temporaneo e Passaggio temporaneo promozionale
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# (Legacy) Anteprima gratuita per Passaggio temporaneo e Passaggio temporaneo promozionale {#free-preview-for-temp-pass-and-promotional-temp-pass}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

>[!NOTE]
>
> L&#39;implementazione REST API è limitata dal [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Endpoint REST API {#clientless-endpoints}

&lt;FQDN_REGGIE>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrizione {#description}

Consente di creare un token di autenticazione per Passaggio temporaneo e Passaggio temporaneo promozionale senza la necessità di una seconda schermata.


| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freepreview | Servizio programmatore </br></br>o</br></br>app in streaming | &#x200B;1. requestor_id (obbligatorio)</br>    </br>2.  deviceId (obbligatorio)</br>    </br>3.  mso_id (obbligatorio)</br>    </br>4.  nome_dominio (obbligatorio)</br>    </br>5.  device_info/X-Device-Info (obbligatorio)</br>6.  deviceType</br>    </br>7.  deviceUser (obsoleto)</br>    </br>8.  appId (obsoleto)</br>    </br>9.  generic_data (facoltativo) | POST | In caso di esito positivo, la risposta sarà No Content (Nessun contenuto) 204, che indica che il token è stato creato correttamente ed è pronto per l’utilizzo per i flussi di autenticazione. | 204 - Nessun contenuto   </br>400 - Richiesta non valida |

<div>


| Parametro di input | Descrizione |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| requestor_id | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| mso_id | L&#39;ID MVPD per il quale è valida questa operazione. |
| nome_dominio | Il nome di dominio per il quale verrà concesso un token. Questo viene confrontato con i domini del fornitore di servizi quando viene concesso un token di autorizzazione. |
| device_info/</br></br>X-Device-Info | Informazioni sul dispositivo di streaming.</br></br>**Nota**: questo parametro POTREBBE essere trasmesso come parametro URL_device, ma a causa delle dimensioni potenziali del parametro e delle limitazioni alla lunghezza di un URL GET, DOVREBBE essere trasmesso come X-Device-Info nell&#39;intestazione http. </br></br>Visualizza tutti i dettagli in [Trasmissione delle informazioni sul dispositivo e sulla connessione](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _tipoDispositivo_ | Il tipo di dispositivo (ad esempio, Roku, PC).</br></br>Se questo parametro è impostato correttamente, ESM offre metriche [suddivise per tipo di dispositivo](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) quando si utilizza senza client, in modo che possano essere eseguiti diversi tipi di analisi, ad esempio per Roku, AppleTV, Xbox ecc.</br></br>Consulta [Vantaggi dell&#39;utilizzo di parametri di tipo di dispositivo senza client &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info sostituirà questo parametro. |
| _utenteDispositivo_ | L’identificatore utente del dispositivo.</br></br>**Nota**: se utilizzato, deviceUser deve avere gli stessi valori della richiesta [Crea codice di registrazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | ID/nome dell’applicazione. </br></br>**Nota**: device_info sostituisce questo parametro. Se utilizzato, `appId` deve avere gli stessi valori della richiesta [Crea codice di registrazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| generic_data | Utilizzato per limitare l’ambito del token per il passaggio temporaneo promozionale. |


### [Torna al riferimento API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
