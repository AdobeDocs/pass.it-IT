---
title: Avvia autorizzazione
description: Avvia autorizzazione
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# (Legacy) Avvia autorizzazione {#initiate-authorization}

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

Ottiene la risposta di autorizzazione.

| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorize | Servizio programmatore </br></br>o</br></br>app in streaming | &#x200B;1. richiedente (obbligatorio)</br>2.  deviceId (obbligatorio)</br>3.  resource (obbligatorio)</br>4.  device_info/X-Device-Info (obbligatorio)</br>5.  _tipoDispositivo_</br> 6.  _deviceUser_ (obsoleto)</br>7.  _appId_ (obsoleto)</br>8.  parametri aggiuntivi (facoltativo) | GET | XML o JSON contenente i dettagli di autorizzazione o di errore se l’operazione non ha esito positivo. Vedi gli esempi di seguito. | 200 - Operazione riuscita </br>403 - Nessuna operazione riuscita |

{style="table-layout:auto"}

</br>


| Parametro di input | Descrizione |
| --- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| resource | Una stringa che contiene un resourceId (o frammento MRSS), identifica il contenuto richiesto da un utente ed è riconosciuta dagli endpoint di autorizzazione di MVPD. |
| device_info/</br></br>X-Device-Info | Informazioni sul dispositivo di streaming.</br></br>**Nota**: questo parametro POTREBBE essere trasmesso come parametro URL_device, ma a causa delle dimensioni potenziali del parametro e delle limitazioni alla lunghezza di un URL GET, DOVREBBE essere trasmesso come X-Device-Info nell&#39;intestazione http. </br></br>Visualizza tutti i dettagli in [Trasmissione delle informazioni sul dispositivo e sulla connessione](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _tipoDispositivo_ | Il tipo di dispositivo (ad esempio, Roku, PC).</br></br>Se questo parametro è impostato correttamente, ESM offre metriche [suddivise per tipo di dispositivo](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) quando si utilizza senza client, in modo che possano essere eseguiti diversi tipi di analisi, ad esempio per Roku, AppleTV, Xbox ecc.</br></br>Vedere [Vantaggi del parametro relativo al tipo di dispositivo senza client nelle metriche di passaggio ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: il parametro device_info verrà sostituito. |
| _utenteDispositivo_ | L’identificatore utente del dispositivo. |
| _appId_ | ID/nome dell’applicazione. </br></br>**Nota**: device_info sostituisce questo parametro. |
| parametri aggiuntivi | La chiamata può inoltre contenere parametri facoltativi che abilitano altre funzionalità come:</br></br>* generic_data - abilita l&#39;utilizzo di [Promotional TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)</br></br>Esempio: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**Indirizzo IP dispositivo di streaming**</br>
>Per le implementazioni client-server, l&#39;indirizzo IP del dispositivo di streaming viene inviato implicitamente con questa chiamata.  Per le implementazioni server-to-server in cui la chiamata **regcode** viene effettuata dal servizio Programmatore e non dal dispositivo di streaming, è necessaria la seguente intestazione per passare l&#39;indirizzo IP del dispositivo di streaming:</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>dove `<streaming\_device\_ip>` è l&#39;indirizzo IP pubblico del dispositivo di streaming.</br></br>
>Esempio: </br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Risposta di esempio {#sample-response}

* **Caso 1: operazione riuscita**
</br>
  * **XML:**
  </br>

    &quot;XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;autorizzazione>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;richiedente>sampleRequestorId&lt;/requestor>
    &lt;risorsa>sampleResourceId&lt;/resource>
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
