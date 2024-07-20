---
title: Avvia autenticazione
description: Avvia autenticazione
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---

# Avvia autenticazione {#initiate-authentication}

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

Avvia il processo di autenticazione informando di un evento di selezione MVPD. Crea un record nel database di autenticazione di Adobe Pass che viene riconciliato quando viene ricevuta una risposta di successo da MVPD.



| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authenticate | Modulo AuthN | 1. requestor_id (obbligatorio)</br>2.  mso_id (obbligatorio)</br>3.  reg_code (obbligatorio)</br>4.  nome_dominio (obbligatorio)</br>5.  noflash=true - </br>    (Obbligatorio, parametro residuo)</br>6.  no_iframe=true (obbligatorio, parametro residuo)</br>7.  parametri aggiuntivi (facoltativo)</br>8.  redirect_url (obbligatorio) | GET | L&#39;app Web di accesso viene reindirizzata alla pagina di accesso MVPD. | 302 per implementazioni di reindirizzamento complete |

{style="table-layout:auto"}


| Parametro di input | Descrizione |
| --- | --- |
| requestor_id | Il richiedente Programmer per il quale è valida questa operazione. |
| mso_id | ID MVPD per il quale è valida questa operazione. |
| reg_code | Il codice di registrazione generato dal servizio Reggie. |
| nome_dominio | Il dominio di origine. |
| redirect_url | URL di reindirizzamento dell&#39;app Web di accesso al termine dell&#39;autenticazione. |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**Importante: parametri obbligatori -** Indipendentemente dall&#39;implementazione lato client, tutti i parametri di cui sopra sono obbligatori.
>
>
>Esempio:
>
>```
>domain_name=loginwebapp.com
>mso_id=sampleMvpdId
>reg_code=RO0885W
>requestor_id=sampleRequestorId
>noflash=true
>redirect_url=http://loginwebapp.com
>```

>[!IMPORTANT]
> 
>**Importante: parametri facoltativi**
>
>La chiamata di può anche contenere parametri opzionali che abilitano altre funzionalità come:
>
> * generic\_data - abilita l&#39;utilizzo di [Promotional TempPass](/help/authentication/promotional-temp-pass.md)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Note** {#notes}

* Il valore del parametro `domain_name` deve essere impostato su uno dei nomi di dominio registrati con l&#39;autenticazione Adobe Pass. Per ulteriori dettagli, consultare [Registrazione e inizializzazione](/help/authentication/programmer-overview.md).

* [Evita di usare &#39;&amp;&#39;reg\_code nella richiesta /authenticate (nota tecnica)](/help/authentication/clientless-avoid-using-reg-code-in-authenticate-request.md)

* Il parametro `redirect_url` deve essere l&#39;ultimo nell&#39;ordine

* Il valore del parametro `redirect_url` deve essere codificato in URL
