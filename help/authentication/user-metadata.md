---
title: Metadati utente
description: Metadati utente
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---

# Metadati utente {#user-metadata}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

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


| Endpoint | Chiamato  </br>Da | Input   </br>Parametri | HTTP  </br>Metodo | Risposta | HTTP  </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | App di streaming</br></br>o</br></br>Servizio programmatore | 1. richiedente</br>2.  deviceId (obbligatorio)</br>3.  device_info/X-Device-Info (Obbligatorio)</br>4.  deviceType</br>5.  deviceUser (obsoleto)</br>6.  appId (obsoleto) | GET | XML o JSON contenente i metadati dell’utente o i dettagli dell’errore in caso di esito negativo. | 200 - Operazione completata<p>404 - Metadati non trovati<p>412 - Token AuthN non valido (ad esempio, token scaduto) |


| Parametro di input | Descrizione |
| --- | --- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| deviceId | Byte ID dispositivo. |
| device_info/<p>X-Device-Info | Informazioni sul dispositivo di streaming.</br></br> **Nota:** Questo PUÒ essere trasmesso device_info come parametro URL, ma a causa delle dimensioni potenziali di questo parametro e delle limitazioni alla lunghezza di un URL GET, DEVE essere trasmesso come X-Device-Info nell’intestazione http. </br></br> Vedi tutti i dettagli in [Trasmissione delle informazioni sul dispositivo e sulla connessione](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Il tipo di dispositivo (ad esempio, Roku, PC).</br></br> Se questo parametro è impostato correttamente, ESM offre metriche che sono [suddiviso per tipo di dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#progr-filter-metrics) quando utilizzi Clientless, in modo che possano essere eseguiti diversi tipi di analisi per, ad esempio, Roku, AppleTV, Xbox, ecc.</br></br> Consulta [Vantaggi dell’utilizzo del parametro del tipo di dispositivo senza client nelle metriche di passaggio](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **Nota:** Il `device_info` sostituisce questo parametro. |
| _deviceUser_ | L’identificatore utente del dispositivo.</br></br> **Nota:** Se utilizzato, `deviceUser` deve avere gli stessi valori di [Crea codice di registrazione](/help/authentication/registration-code-request.md) richiesta. |
| _appId_ | ID/nome dell’applicazione. </br></br> **Nota:** Il `device_info` sostituisce questo parametro. Se utilizzato, `appId` deve avere gli stessi valori di [Crea codice di registrazione](/help/authentication/registration-code-request.md) richiesta. |

>[!NOTE]
> 
>Le informazioni sui metadati dell’utente devono essere disponibili al termine del flusso di autenticazione, ma possono essere aggiornate in base al flusso di autorizzazione, a seconda dell’MVPD e del tipo di metadati.




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

* *aggiornato*: specifica una marca temporale UNIX che rappresenta l’ultimo aggiornamento dei metadati. Questa proprietà verrà impostata inizialmente dal server durante la generazione dei metadati durante la fase di autenticazione. Le chiamate successive (dopo l’aggiornamento dei metadati) genereranno un timestamp incrementato.
* *dati*: contiene i valori effettivi dei metadati.
* *crittografato*: array in cui sono elencate le proprietà crittografate. Per decrittografare un valore di metadati specifico, il programmatore deve eseguire una decodifica Base64 sui metadati e quindi applicare una decrittografia RSA sul valore risultante, utilizzando la propria chiave privata (ad Adobe, crittografa i metadati sul server utilizzando il certificato pubblico del programmatore).

In caso di errore, il server restituirà un oggetto XML o JSON che specifica un messaggio di errore dettagliato.

Per ulteriori informazioni, consulta [Metadati utente](/help/authentication/user-metadata-feature.md).

[Torna a Riferimento API REST](/help/authentication/rest-api-reference.md)
