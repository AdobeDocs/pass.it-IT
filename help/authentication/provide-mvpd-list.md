---
title: Fornisci elenco MVPD
description: Fornisci elenco MVPD
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Fornisci elenco MVPD {#provide-mvpd-list}

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

Restituisce l&#39;elenco di MVPD configurati per il richiedente.

| Endpoint | Chiamato  </br>Da | Input   </br>Parametri | HTTP  </br>Metodo | Risposta | HTTP  </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/config/{requestorId}</br></br>Ad esempio:</br></br>&lt;sp_fqdn>/api/v1/config/sampleRequestorId | Autenticazione Adobe Pass | 1. Richiedente</br>    (componente Percorso)</br>_2.  deviceType (obsoleto)_ | GET | XML o JSON contenente l’elenco degli MVPD. | 200 |

{style="table-layout:auto"}


| Parametro di input | Descrizione |
| --------------- | ------------------------------------------------------------- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| *deviceType* | Tipo di dispositivo. |

{style="table-layout:auto"}

### Risposta di esempio {#sample-response}

Come risposta XML MVPD esistente al servlet /config

Nota: tutti gli MVPD configurati per utilizzare l’SSO di Platform avranno le seguenti proprietà aggiuntive all’interno del nodo corrispondente (JSON/XML):

* **enablePlatformServices (booleano):** Flag che indica se questo MVPD è integrato tramite SSO piattaforma
* **boardingStatus (stringa):** Flag che indica se MVPD supporta completamente Platform SSO (SUPPORTED) o se MVPD appare solo nel selettore della piattaforma (PICKER)
* **displayInPlatformPicker (booleano):** indica se questo MVPD deve essere visualizzato nel selettore piattaforma
* **platformMappingId (stringa):** l’identificatore di questo MVPD noto alla piattaforma
* **requiredMetadataFields (matrice di stringhe):** i campi di metadati utente che si prevede saranno disponibili al momento dell’accesso
