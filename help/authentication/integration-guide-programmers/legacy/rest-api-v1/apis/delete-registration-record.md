---
title: Elimina record di registrazione
description: Elimina record di registrazione
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 1%

---

# (Legacy) Elimina record di registrazione {#delete-registration-record}

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


## Descrizione {#delete-record}

Elimina il record del codice reg e rilascia il codice reg per il riutilizzo.

| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Esempio:</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | Servizio programmatore </br></br>o</br></br>app in streaming | &#x200B;1. ID richiedente </br>    (componente percorso)</br>2.  Codice di registrazione </br>    (componente Percorso) | DELETE | Nessuno | 204 |

{style="table-layout:auto"}

</br>

| Parametro di input | Descrizione |
| --- | --- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| codice di registrazione | Il valore del codice di registrazione che verrebbe visualizzato sul dispositivo di streaming (da inserire nel flusso di autenticazione). |

{style="table-layout:auto"}

</br>

### [Torna al riferimento API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
