---
title: Vantaggi dell’utilizzo del parametro deviceType senza client nelle metriche di autenticazione di Adobe Pass
description: Vantaggi dell’utilizzo del parametro deviceType senza client nelle metriche di autenticazione di Adobe Pass
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 0%

---

# Vantaggi dell’utilizzo del parametro deviceType senza client nelle metriche di autenticazione di Adobe Pass {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>

## Contesto

Anche se facoltativo, il parametro `deviceType` dall’API senza client, se presente, viene utilizzato nelle metriche di autenticazione di Adobe Pass che vengono esposte tramite [Monitoraggio del servizio di adesione](/help/authentication/entitlement-service-monitoring-overview.md).

Considerando che la connessione tra `deviceType` parametro e relativi **vantaggi** Nelle metriche di autenticazione di Adobe Pass che inizialmente non erano state dichiarate, lo scopo di questa nota tecnica è quello di aggiungere ulteriori informazioni su di esse.

## Spiegazione

Il `deviceType` Il parametro era presente nell’API senza client dalla prima versione, ma le sue implicazioni sulle metriche di autenticazione di Adobe Pass sono state aggiunte in una versione più recente.



>[!IMPORTANT]
>
>Se il parametro `deviceType` è impostato correttamente, presenta le seguenti caratteristiche **benefit** in Monitoraggio del servizio di adesione: offre metriche che sono [suddiviso per tipo di dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) quando utilizzi Clientless, in modo che possano essere eseguiti diversi tipi di analisi per, ad esempio, Roku, AppleTV, Xbox, ecc.


Per ulteriori informazioni sull’API di monitoraggio di Entitlement Service, consulta [albero di espansione,](/help/authentication/entitlement-service-monitoring-api.md#drill-down_tree) che illustra [dimensioni](/help/authentication/entitlement-service-monitoring-overview.md#esm_dimensions) (risorse) disponibili nel MES 2.0.

>[!NOTE]
>
>Il contenuto di questa nota tecnica è stato aggiunto anche al [API senza client](#clientless_device_type).




## Implementazione

Per beneficiare appieno delle metriche di autenticazione di Adobe Pass, sono disponibili 2 tipi di [API senza client](#web_srvs_summary) che sono attualmente in uso e che devono avere il corretto `deviceType` imposta:

1. API che hanno `regcode` come parametro obbligatorio e utilizzerà il `deviceType` il parametro impostato durante la creazione del `regcode`, con la seguente chiamata API:
   - [\&lt;reggie _fqdn=&quot;&quot;>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. API che hanno `deviceType` come parametro facoltativo:
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/authorize](#init_authz)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/token/authz](#retrieve_authz_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/media](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/mediatoken](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/logout](#init_logout)

Si consiglia di utilizzare il `deviceType` e passa il tipo di dispositivo senza client corretto per tutte le API.
