---
title: Scambia un token SSO di Platform con un token di Adobe
description: Scambia un token SSO di Platform con un token di Adobe
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Scambia un token SSO di Platform con un token di Adobe {#exchange-a-platform-sso-token-for-an-adobe-token}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

>[!NOTE]
>
> L&#39;implementazione REST API è limitata dal [meccanismo di limitazione](/help/authentication/throttling-mechanism.md)

## Endpoint REST API {#clientless-endpoints}

&lt;FQDN_REGGIE>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrizione {#description}

Consente lo &quot;scambio&quot; di un profilo SSO di Platform con un token di Adobe.

| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/token/author | Servizio programmatore </br></br>o</br></br>app in streaming | 1. richiedente (obbligatorio)</br>    </br>2.  deviceId (obbligatorio)</br>    </br>3.  mvpd (obbligatorio)</br>    </br>4.  deviceType (obbligatorio)</br>    </br>5.  SAMLResponse (obbligatorio)</br>    </br>6.  deviceUser (obsoleto)</br>    </br>7.  appId (obsoleto) | POST | In caso di esito positivo, la risposta sarà No Content (Nessun contenuto) 204, che indica che il token è stato creato correttamente ed è pronto per l’utilizzo per i flussi di autenticazione. | 204 - Nessun contenuto   </br>400 - Richiesta non valida |


| Parametro di input | Descrizione |
| --- | --- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| mvpd | L&#39;ID MVPD per il quale è valida questa operazione. |
| deviceType | La piattaforma Apple per la quale stiamo tentando di ottenere una richiesta di profilo.  **iOS** o **tvOS**. |
| SAMLResponse | Profilo effettivo restituito dall’SSO di Platform. |
| _utenteDispositivo_ | L’identificatore utente del dispositivo. |
| _appId_ | ID/nome dell’applicazione. |
