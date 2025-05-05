---
title: Come effettuare una richiesta di accesso a dati personali
description: Come effettuare una richiesta di accesso a dati personali
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# Come effettuare una richiesta di accesso a dati personali {#howto-make-privacy-request}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Identificatori e namespace {#identifier-namespace}

Quando si invia una richiesta di accesso o di eliminazione dei dati personali, l’applicazione del cliente deve includere i seguenti identificatori:

* **mvpdID** - Identificatore univoco di MVPD.
* **userID** - Identifica in modo univoco l&#39;utente dell&#39;app di un programmatore, ma ha origine da MVPD. Consulta Informazioni sugli ID utente nella Panoramica dei programmatori.
* **IMSOrgID** - L&#39;ID organizzazione del servizio Adobe Experience Cloud Identity Management, che identifica in modo univoco il cliente in Adobe Experience Cloud


Verifica l’esempio seguente:

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>Per poter generare richieste di accesso a dati personali per l’autenticazione Adobe Pass, gli utenti devono essere autenticati. In caso contrario, i programmatori devono trovare altri mezzi per estrarre l&#39;ID utente MVPD.

## Tipi di richieste {#req-type}

L’autenticazione di Adobe Pass supporta le richieste di accesso ed eliminazione.

### Accesso {#access-req}

Per una richiesta di accesso:

Verrà fornito un file JSON contenente un riepilogo del numero totale di richieste di autenticazione e autorizzazione create per l’interessato.
tutti questi eventi vengono filtrati per Cliente.


**Esempio di richiesta**

Devi caricare un JSON con gli identificatori di autenticazione di Adobe Pass per i quali stai inviando la richiesta di accesso ai dati. Per visualizzare l’aspetto di un JSON ben formato, consulta questo esempio:

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["access"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Esempio di risposta**

```JSON
{
    "jobId": "d9a6b417-f619-4420-82a3-09f61fa8eff3",
    "requestId": "15765127177927284RX-739",
    "userKey": "John Dow",
    "action": "access",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/16/2019 04:11 PM GMT",
    "lastModifiedDate": "12/16/2019 04:15 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/16/2019 04:15 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-16T16:15:23.4Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "6",
                        "numberOfAuthorizationDecisions": "11"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/d9a6b417-f619-4420-82a3-09f61fa8eff3/d9a6b417-f619-4420-82a3-09f61fa8eff3.zip",
    "regulation": "ccpa"
}
```

### Elimina {#delete-req}

Devi caricare un JSON con gli identificatori di autenticazione di Adobe Pass per i quali stai inviando la richiesta di eliminazione dei dati. Per visualizzare l’aspetto di un JSON ben formato, consulta questo esempio:

**Esempio di richiesta**

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["delete"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Esempio di risposta**

Per una richiesta di eliminazione:

* condividiamo solo una conferma che i dati sono stati rimossi, non un file aggregato con tutti i dati rimossi.
* la ricevuta inclusa nella risposta contiene un riepilogo del numero totale di token di autenticazione e autorizzazione trovati per l’interessato.

```JSON
{
    "jobId": "aab380d1-a0cd-4a0d-ba95-2649ee90c063",
    "requestId": "15759883098453100RX-074",
    "userKey": "John Dow",
    "action": "delete",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/10/2019 02:31 PM GMT",
    "lastModifiedDate": "12/10/2019 02:34 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/10/2019 02:34 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-10T14:34:55.274Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "2",
                        "numberOfAuthorizationDecisions": "3"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/aab380d1-a0cd-4a0d-ba95-2649ee90c063/aab380d1-a0cd-4a0d-ba95-2649ee90c063.zip",
    "regulation": "ccpa"
}
```

## Come attivare una richiesta {#trigger-req}

Ci sono 2 opzioni per i clienti per inviare richieste di privacy ad Adobe:

* **manualmente** - utilizzando [Interfaccia utente Privacy Service](#privacy-service-ui)
* **automaticamente** - utilizzando [API Privacy Service](#privacy-service-api)

### Utilizzando l’interfaccia utente di Privacy Service {#privacy-service-ui}

Un [tutorial completo](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=it#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) su come accedere e utilizzare l&#39;interfaccia utente di Privacy Service è disponibile online tramite i servizi Adobe I/O. Inoltre, i clienti possono utilizzare questo collegamento per accedere alla libreria di video e articoli sulle normative sulla privacy. Fai clic sul menu Adobe Experience Cloud e RGPD. Verranno aperti diversi video - La guida introduttiva all’interfaccia utente RGPD spiega come utilizzarla.

Nell’interfaccia utente, i clienti devono caricare il proprio IMSOrgID e un JSON contenente i dettagli delle richieste RGPD per ciascun prodotto.

### Utilizzando l’API Privacy Service {#privacy-service-api}

Adobe Experience Platform Privacy Service fornisce un’agevolazione comune e centralizzata delle richieste di accesso/cancellazione e di rinuncia alla vendita di dati privati.

La documentazione dell&#39;**API Privacy Service** descrive in modo approfondito come un cliente Adobe può integrarsi con l&#39;API Adobe.

**Visualizza chiamate API con Postman (un software gratuito di terze parti):**

* [Raccolta Postman API Privacy Service su GitHub](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Guida video per la creazione dell&#39;ambiente Postman](https://video.tv.adobe.com/v/36258?captions=ita)
* [Passaggi per importare ambienti e raccolte in Postman](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**Percorsi API:**

* URL del gateway della piattaforma: `https://platform.adobe.io/`
* Percorso base per questa API: `/data/core/privacy/jobs`
* Esempio di percorso completo: `https://platform.adobe.io/data/core/privacy/jobs/ping`


**Intestazioni richieste:**

* Tutte le chiamate richiedono le intestazioni `Authorization`, `x-gw-ims-org-id` e `x-api-key`. Per ulteriori informazioni su come ottenere questi valori, consulta l&#39;**esercitazione sull&#39;autenticazione**.
* Tutte le richieste con un payload nel corpo della richiesta (ad esempio chiamate POST, PUT e PATCH) devono includere l&#39;intestazione `Content-Type` con valore `application/json`.

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=it#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
