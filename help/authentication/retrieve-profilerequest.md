---
title: Recuperare la richiesta del profilo SSO di Platform
description: Recuperare la richiesta del profilo SSO di Platform
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# Recuperare la richiesta del profilo SSO di Platform {#retrieve-platform-sso-profile-request}

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

Questa risorsa genera richieste di profilo per un ID richiedente e una tupla MVPD.


| Endpoint | Chiamato  </br>Da | Input   </br>Parametri | HTTP  </br>Metodo | Risposta | HTTP  </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/{requestor}/profile-requests/{mvpd} | App di streaming</br></br>o</br></br>Servizio programmatore | 1. richiedente (parametro percorso)</br>2. mvpd (parametro percorso)</br>3. deviceType (obbligatorio) | GET | Il Content-Type di risposta sarà application/octet-stream, poiché il payload effettivo è opaco per l’applicazione client.</br></br>La risposta deve essere inoltrata dall’applicazione alla piattaforma</br></br>Motore SSO per ottenere un SSO profilo. | 200 - Operazione completata   </br>400 - Richiesta non valida |


| Parametro di input | Descrizione |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| mvpd | L&#39;ID MVPD per il quale è valida questa operazione. |
| deviceType | La piattaforma Apple per la quale stiamo tentando di ottenere una richiesta di profilo.  o **iOS** o **tvOS**. |
