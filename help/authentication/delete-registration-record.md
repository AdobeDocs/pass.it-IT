---
title: Elimina record di registrazione
description: Elimina record di registrazione
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Elimina record di registrazione {#delete-registration-record}

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


## Descrizione {#delete-record}

Elimina il record del codice reg e rilascia il codice reg per il riutilizzo.

| Endpoint | Chiamato  </br>Da | Input   </br>Parametri | HTTP  </br>Metodo | Risposta | HTTP  </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;reggie_fqdn>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Ad esempio:</br></br>&lt;reggie_fqdn>/reggie/v1/regcode/ER45RTY | App di streaming</br></br>o</br></br>Servizio programmatore | 1. ID richiedente  </br>    (componente Percorso)</br>2.  Codice di registrazione  </br>    (componente Percorso) | DELETE | Nessuno | 204 |

{style="table-layout:auto"}

</br>

| Parametro di input | Descrizione |
| --- | --- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| codice di registrazione | Il valore del codice di registrazione che verrebbe visualizzato sul dispositivo di streaming (da inserire nel flusso di autenticazione). |

{style="table-layout:auto"}

</br>

### [Torna a Riferimento API REST](/help/authentication/rest-api-reference.md)
