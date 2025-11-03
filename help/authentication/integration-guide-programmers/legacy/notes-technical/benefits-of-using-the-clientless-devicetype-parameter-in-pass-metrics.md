---
title: Vantaggi dell’utilizzo del parametro deviceType senza client nelle metriche di autenticazione di Adobe Pass
description: Vantaggi dell’utilizzo del parametro deviceType senza client nelle metriche di autenticazione di Adobe Pass
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# (Legacy) Vantaggi dell’utilizzo del parametro deviceType senza client nelle metriche di autenticazione di Adobe Pass {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

</br>

## Contesto

Anche se facoltativo, il parametro `deviceType` dell&#39;API senza client, se presente, viene utilizzato nelle metriche di autenticazione di Adobe Pass esposte tramite il monitoraggio del [Servizio diritti](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md).

Considerando che la connessione tra il parametro `deviceType` e i relativi **vantaggi** sulle metriche di autenticazione di Adobe Pass non è stata inizialmente dichiarata, l&#39;obiettivo di questa nota tecnica è quello di aggiungere ulteriori informazioni su di essi.

## Spiegazione

Il parametro `deviceType` era presente nell&#39;API senza client dalla prima versione, ma le sue implicazioni sulle metriche di autenticazione di Adobe Pass sono state aggiunte in una versione più recente.



>[!IMPORTANT]
>
>Se il parametro `deviceType` è impostato correttamente, avrà il seguente **beneficio** nel monitoraggio del servizio di adesione: offre metriche [suddivise per tipo di dispositivo](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) quando si utilizza Clientless, in modo che diversi tipi di analisi possano essere eseguiti per esempio per Roku, AppleTV, Xbox ecc.


Per ulteriori informazioni sull&#39;API di monitoraggio del servizio di adesione, fare riferimento alla [struttura di espansione,](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#esm_dimensions) (risorse) disponibile in ESM 2.0.

>[!NOTE]
>
>Il contenuto di questa nota tecnica è stato aggiunto anche all&#39;[API senza client](#clientless_device_type).




## Implementazione

Per beneficiare appieno delle metriche di autenticazione di Adobe Pass, esistono 2 tipi di [API senza client](#web_srvs_summary) attualmente in uso e che devono avere impostato `deviceType` corretto:

1. API che hanno `regcode` come parametro obbligatorio e utilizzeranno il parametro `deviceType` impostato durante la creazione di `regcode`, con la seguente chiamata API:
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/](#reg_serv)

1. API che hanno `deviceType` come parametro opzionale:
   - [\&lt;SP\_FQDN\>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorize](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/token/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/token/supporto](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

Si consiglia di utilizzare il parametro `deviceType` e passare il tipo di dispositivo senza client corretto per tutte le API.
