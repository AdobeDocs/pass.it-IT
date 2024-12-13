---
title: Recuperare la richiesta del profilo SSO di Platform
description: Recuperare la richiesta del profilo SSO di Platform
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 1%

---

# (Legacy) Recuperare la richiesta del profilo SSO di Platform {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

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

Questa risorsa genera richieste di profilo per un ID richiedente e una tupla MVPD.


| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | Servizio programmatore </br></br>o</br></br>app in streaming | 1. richiedente (parametro percorso)</br>2. mvpd (parametro percorso)</br>3. deviceType (obbligatorio) | GET | Il Content-Type di risposta sarà application/octet-stream, poiché il payload effettivo è opaco per l’applicazione client.</br></br>La risposta deve essere inoltrata dall&#39;applicazione al motore SSO di Platform</br></br>per ottenere un SSO profilo. | 200 - Operazione completata   </br>400 - Richiesta non valida |


| Parametro di input | Descrizione |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| mvpd | L&#39;ID MVPD per il quale è valida questa operazione. |
| deviceType | La piattaforma Apple per la quale stiamo tentando di ottenere una richiesta di profilo.  **iOS** o **tvOS**. |
