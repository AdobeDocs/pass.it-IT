---
title: Avvia autorizzazione
description: Avvia autorizzazione
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# Avvia autorizzazione {#initiate-authorization}

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

Ottiene la risposta di autorizzazione.

| Endpoint | Chiamato  </br>Da | Input   </br>Parametri | HTTP  </br>Metodo | Risposta | HTTP  </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authorize | App di streaming</br></br>o</br></br>Servizio programmatore | 1. richiedente (obbligatorio)</br>2.  deviceId (obbligatorio)</br>3.  resource (obbligatorio)</br>4.  device_info/X-Device-Info (Obbligatorio)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Obsoleto)</br>7.  _appId_ (Obsoleto)</br>8.  parametri aggiuntivi (facoltativo) | GET | XML o JSON contenente i dettagli di autorizzazione o di errore se l’operazione non ha esito positivo. Vedi gli esempi di seguito. | 200 - Operazione completata  </br>403 - Nessun successo |

{style="table-layout:auto"}

</br>


| Parametro di input | Descrizione |
| --- | --- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| resource | Una stringa che contiene un resourceId (o frammento MRSS), identifica il contenuto richiesto da un utente ed è riconosciuta dagli endpoint di autorizzazione MVPD. |
| device_info/</br></br>X-Device-Info | Informazioni sul dispositivo di streaming.</br></br>**Nota**: questo PUÒ essere trasmesso device_info come parametro URL, ma a causa delle dimensioni potenziali del parametro e delle limitazioni alla lunghezza di un URL GET, DEVE essere trasmesso come X-Device-Info nell’intestazione http. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | Il tipo di dispositivo (ad esempio, Roku, PC).</br></br>Se questo parametro è impostato correttamente, ESM offre metriche che sono [suddiviso per tipo di dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) quando utilizzi Clientless, in modo che possano essere eseguiti diversi tipi di analisi per, ad esempio, Roku, AppleTV, Xbox, ecc.</br></br>Consulta [Vantaggi del parametro del tipo di dispositivo senza client nelle metriche di passaggio ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info sostituirà questo parametro. |
| _deviceUser_ | L’identificatore utente del dispositivo. |
| _appId_ | ID/nome dell’applicazione. </br></br>**Nota**: device_info sostituisce questo parametro. |
| parametri aggiuntivi | La chiamata di può anche contenere parametri opzionali che abilitano altre funzionalità come:</br></br>* generic_data - consente l’utilizzo di [TempPass promozionale](/help/authentication/promotional-temp-pass.md)</br></br>Esempio: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**Indirizzo IP dispositivo di streaming**</br>
>Per le implementazioni client-server, l&#39;indirizzo IP del dispositivo di streaming viene inviato implicitamente con questa chiamata.  Per le implementazioni server-to-server, in cui **regcode** La chiamata viene effettuata dal servizio Programmatore e non dal dispositivo di streaming. Per passare l&#39;indirizzo IP del dispositivo di streaming è necessaria la seguente intestazione:</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>dove `<streaming\_device\_ip>` è l&#39;indirizzo IP pubblico del dispositivo di streaming.</br></br>
>Esempio:</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Risposta di esempio {#sample-response}

* **Caso 1: successo**
</br>
  * **XML:**
  </br>

    &quot;XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
    &quot;



* **JSON:**

  ```JSON
  {
    "mvpd": "sampleMvpdId",
    "resource": "sampleResourceId",
    "requestor": "sampleRequestorId",
    "expires": "1348148289000"
  }
  ```

>[!IMPORTANT]
>
>Quando la risposta proviene da un MVPD proxy, può includere un elemento aggiuntivo denominato `proxyMvpd`.



* **Caso 2: autorizzazione negata**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
