---
title: Monitoraggio dell’autenticazione di Adobe Pass
description: Monitoraggio dell’autenticazione di Adobe Pass
exl-id: fb000e9d-b5aa-45b1-a914-9e419ec8a4d9
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# (Legacy) Monitoraggio dell’autenticazione di Adobe Pass {#monitoring-adobe-primetime-authentication}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Introduzione {#intro}

I clienti possono utilizzare [Nagios](http://www.nagios.org) o altri strumenti per verificare se l&#39;autenticazione Adobe Pass è attiva o inattiva.

## Endpoint di monitoraggio {#monitoring-endpoints}

### Endpoint che è possibile monitorare {#endpoints-to-monitor}

* L&#39;endpoint di configurazione per tutte le piattaforme: `https://sp.auth.adobe.com/adobe-services/config/[your-config-ID]`- È disponibile tramite HTTP o HTTPS (a seconda della scelta effettuata dallo sviluppatore del provider di contenuti). Se questo endpoint non è presente, significa che il contenuto non sarà disponibile su tutte le piattaforme e su tutti gli MVPD. Per l&#39;API REST senza client è disponibile anche il seguente endpoint: `https://api.auth.adobe.com/adobe-services/config your-config-ID]`.

* I seguenti endpoint fanno parte di Adobe Pass Authentication Web SDK.  Se manca significa che pay-TVpass è inattivo per tutti i programmatori e tutte le proprietà web:

   * `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`
   * `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`


### Endpoint da non monitorare {#endpoints-not-monitor}

* `https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer`

  Riceverai sempre un errore 503, perché questo endpoint necessita di una risposta SAML di MVPD.

* Altri endpoint di adesione - `adobe-services/1.0/authenticate/`, `adobe-services/1.0/deviceShortAuthorize`, `adobe-services/1.0/authorize`

Non è possibile monitorare questi endpoint perché richiedono un payload per una risposta pertinente.
