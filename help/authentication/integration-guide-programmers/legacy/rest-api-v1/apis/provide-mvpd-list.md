---
title: Fornisci elenco MVPD
description: Fornisci elenco MVPD
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 2%

---

# (Legacy) Fornisci elenco MVPD {#provide-mvpd-list}

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

Restituisce l&#39;elenco di MVPD configurati per il richiedente.

| Endpoint | Chiamato </br> da | Input   </br>Parametri | Metodo HTTP </br> | Risposta | HTTP </br>Risposta |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>Esempio:</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Autenticazione Adobe Pass | 1. Richiedente</br>    (componente percorso)</br>_2.  deviceType (obsoleto)_ | GET | XML o JSON contenente l’elenco degli MVPD. | 200 |

{style="table-layout:auto"}


| Parametro di input | Descrizione |
| --------------- | ------------------------------------------------------------- |
| richiedente | ID richiedente del programmatore per il quale è valida questa operazione. |
| *tipoDispositivo* | Tipo di dispositivo. |

{style="table-layout:auto"}

### Risposta di esempio {#sample-response}

Uguale alla risposta XML di MVPD esistente al servlet /config

Nota: tutti gli MVPD configurati per utilizzare l’SSO di Platform avranno le seguenti proprietà aggiuntive all’interno del nodo corrispondente (JSON/XML):

* Flag **enablePlatformServices (booleano):** che indica se il MVPD è integrato tramite l&#39;SSO della piattaforma
* **boardingStatus (stringa):** flag che indica se MVPD supporta completamente Platform SSO (SUPPORTED) o se MVPD viene visualizzato solo nel selettore della piattaforma (PICKER)
* **displayInPlatformPicker (booleano):** se questo MVPD deve essere visualizzato nel selettore della piattaforma
* **platformMappingId (stringa):** l&#39;identificatore di questo MVPD noto alla piattaforma
* **requiredMetadataFields (array di stringhe):** i campi di metadati dell&#39;utente prevedevano di essere disponibili in caso di accesso riuscito
