---
title: Metadati utente
description: Metadati utente
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# Metadati utente (legacy) {#user-metadata}

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

`<REGGIE_FQDN>`:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* Produzione - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrizione {#description}

Recupera i metadati condivisi da MVPD sull’utente autenticato.


| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | Servizio programmatore </br></br>o</br></br>app in streaming | &#x200B;1. richiedente</br>2.  deviceId (obbligatorio)</br>3.  device_info/X-Device-Info (obbligatorio)</br>4.  deviceType</br>5.  deviceUser (obsoleto)</br>6.  appId (obsoleto) | GET | XML o JSON contenente i metadati dell’utente o i dettagli dell’errore in caso di esito negativo. | 200 - Operazione completata<p>404 - Metadati non trovati<p>412 - Token AuthN non valido (ad esempio, token scaduto) |


| Parametro di input | Descrizione |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| device_info/<p>X-Device-Info | Informazioni dispositivo di streaming.</br></br> **Nota:** È possibile che questo parametro venga passato come parametro URL device_info, ma a causa delle dimensioni potenziali del parametro e delle limitazioni alla lunghezza di un URL GET, DEVE essere passato come X-Device-Info nell&#39;intestazione http. </br></br> Vedere i dettagli completi in [Trasmissione delle informazioni sul dispositivo e sulla connessione](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _tipoDispositivo_ | Il tipo di dispositivo (ad esempio, Roku, PC).</br></br> Se questo parametro è impostato correttamente, ESM offre metriche [suddivise per tipo di dispositivo](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#progr-filter-metrics) quando si utilizza Clientless, in modo che possano essere eseguiti diversi tipi di analisi per esempio Roku, AppleTV, Xbox ecc.</br></br> Vedere [Vantaggi dell&#39;utilizzo del parametro del tipo di dispositivo senza client nelle metriche Pass](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **Nota:** `device_info` sostituisce questo parametro. |
| _utenteDispositivo_ | Identificatore utente del dispositivo.</br></br> **Nota:** se utilizzato, `deviceUser` deve avere gli stessi valori della richiesta [Crea codice di registrazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | ID/nome dell’applicazione. </br></br> **Nota:** `device_info` sostituisce questo parametro. Se utilizzato, `appId` deve avere gli stessi valori della richiesta [Crea codice di registrazione](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |

>[!NOTE]
> 
>Le informazioni sui metadati utente devono essere disponibili al termine del flusso di autenticazione, ma possono essere aggiornate in base al flusso di autorizzazione, a seconda del MVPD e del tipo di metadati.




## Risposta di esempio {#sample-response}

Dopo una chiamata corretta, il server risponderà con un oggetto XML (predefinito) o JSON con una struttura simile a quella presentata di seguito:


```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
              zip: ["12345", "34567"],
              maxRating: { 
                  "MPAA": "PG-13",
                  "VCHIP": "TV-Y", 
                  "URL": "http://exam.pl/e/manage/ratings"
                         },
              householdID: "3456",
              userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
              channelID: ["channel-1", "channel-2"]
              }
    }
```

Nella radice dell&#39;oggetto ci saranno tre nodi:

* *updated*: specifica un timestamp UNIX che rappresenta l&#39;ultimo aggiornamento dei metadati. Questa proprietà verrà impostata inizialmente dal server durante la generazione dei metadati durante la fase di autenticazione. Le chiamate successive (dopo l’aggiornamento dei metadati) genereranno un timestamp incrementato.
* *dati*: contiene i valori metadati effettivi.
* *encrypted*: matrice che elenca le proprietà crittografate. Per decrittografare un valore di metadati specifico, il programmatore deve eseguire una decodifica Base64 sui metadati e quindi applicare una decrittografia RSA sul valore risultante, utilizzando la propria chiave privata (Adobe crittografa i metadati sul server utilizzando il certificato pubblico del programmatore).

In caso di errore, il server restituirà un oggetto XML o JSON che specifica un messaggio di errore dettagliato.

Per ulteriori informazioni, vedere [Metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

[Torna a Riferimento API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
