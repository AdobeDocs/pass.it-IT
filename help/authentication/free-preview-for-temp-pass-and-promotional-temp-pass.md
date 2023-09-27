---
title: Anteprima gratuita per Passaggio temporaneo e Passaggio temporaneo promozionale
description: Anteprima gratuita per Passaggio temporaneo e Passaggio temporaneo promozionale
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 1%

---

# Anteprima gratuita per Passaggio temporaneo e Passaggio temporaneo promozionale {#free-preview-for-temp-pass-and-promotional-temp-pass}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Endpoint REST API {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrizione {#description}

Consente di creare un token di autenticazione per Passaggio temporaneo e Passaggio temporaneo promozionale senza la necessità di una seconda schermata.


| Endpoint | Chiamato  </br>Da | Input   </br>Parametri | HTTP  </br>Metodo | Risposta | HTTP  </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate/freepreview | App di streaming</br></br>o</br></br>Servizio programmatore | 1. requestor_id (obbligatorio)</br>    </br>2.  deviceId (obbligatorio)</br>    </br>3.  mso_id (obbligatorio)</br>    </br>4.  domain_name (obbligatorio)</br>    </br>5.  device_info/X-Device-Info (Obbligatorio)</br>6.  deviceType</br>    </br>7.  deviceUser (obsoleto)</br>    </br>8.  appId (obsoleto)</br>    </br>9.  generic_data (facoltativo) | POST | In caso di esito positivo, la risposta sarà No Content (Nessun contenuto) 204, che indica che il token è stato creato correttamente ed è pronto per l’utilizzo per i flussi di autenticazione. | 204 - Nessun contenuto   </br>400 - Richiesta non valida |

<div>


| Parametro di input | Descrizione |
| --- | --- |
| requestor_id | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| mso_id | L&#39;ID MVPD per il quale è valida questa operazione. |
| nome_dominio | Il nome di dominio per il quale verrà concesso un token. Questo viene confrontato con i domini del fornitore di servizi quando viene concesso un token di autorizzazione. |
| device_info/</br></br>X-Device-Info | Informazioni sul dispositivo di streaming.</br></br>**Nota**: questo PUÒ essere trasmesso device_info come parametro URL, ma a causa delle dimensioni potenziali del parametro e delle limitazioni alla lunghezza di un URL GET, DEVE essere trasmesso come X-Device-Info nell’intestazione http. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | Il tipo di dispositivo (ad esempio, Roku, PC).</br></br>Se questo parametro è impostato correttamente, ESM offre metriche che sono [suddiviso per tipo di dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) quando utilizzi Clientless, in modo che possano essere eseguiti diversi tipi di analisi per, ad esempio, Roku, AppleTV, Xbox, ecc.</br></br>Consulta [Vantaggi dell’utilizzo di parametri per tipi di dispositivi senza client ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info sostituirà questo parametro. |
| _deviceUser_ | L’identificatore utente del dispositivo.</br></br>**Nota**: se utilizzato, deviceUser deve avere gli stessi valori di [Crea codice di registrazione](/help/authentication/registration-code-request.md) richiesta. |
| _appId_ | ID/nome dell’applicazione. </br></br>**Nota**: device_info sostituisce questo parametro. Se utilizzato, `appId` deve avere gli stessi valori di [Crea codice di registrazione](/help/authentication/registration-code-request.md) richiesta. |
| generic_data | Utilizzato per limitare l’ambito del token per il passaggio temporaneo promozionale. |


### [Torna a Riferimento API REST](/help/authentication/rest-api-reference.md)
