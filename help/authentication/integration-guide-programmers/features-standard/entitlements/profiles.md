---
title: Profili
description: Profili
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Profili {#profiles}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

I profili vengono creati con l&#39;autenticazione Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) quando un utente si autentica correttamente con il proprio provider di Pay TV (MVPD).

Il tipo di profili varia in base al metodo di autenticazione utilizzato:

* **Normale**

  Creato tramite autenticazione di base.

* **SSO Apple**

  Creato tramite Single Sign-On (SSO) utilizzando il framework dell’account Apple Video Subscriber.

* **SSO piattaforma**

  Creato tramite Single Sign-On (SSO) utilizzando un’identità di piattaforma.

* **SSO token di servizio**

  Creato tramite Single Sign-On (SSO) utilizzando un token di servizio.

I profili memorizzano i dati chiave che consentono alle applicazioni client di:

* Determina lo stato di autenticazione dell’utente.
* Identifica il metodo di autenticazione utilizzato.
* Identifica il provider di identità.
* Accedi a [metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

I profili vengono memorizzati in modo sicuro nel backend di Adobe Pass Authentication e sono collegati all’identificatore dell’applicazione, del dispositivo e del provider di servizi richiedente. Rimangono validi per un periodo di tempo limitato, come definito da [Authentication Time-to-Live (TTL)](#authentication-ttl-management).

## Gestione TTL (Authentication Time-to-Live) {#authentication-ttl-management}

Il valore TTL (Authentication Time-to-Live) definisce per quanto tempo un utente rimane autenticato prima di dover ripetere l’autenticazione. Questo periodo di tempo è limitato e deve essere concordato con i rappresentanti di MVPD. I valori TTL possono variare in base a:

* Categoria piattaforma (ad esempio desktop, dispositivi mobili, dispositivi collegati alla TV)
* Piattaforma specifica (ad esempio iOS, Android, tvOS, Roku, FireTV)

Il TTL di autenticazione (authN) può essere visualizzato e modificato tramite il dashboard di Adobe Pass [TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consulta la [Guida utente per l&#39;integrazione del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

## API REST V2 {#rest-api-v2}

I profili possono essere recuperati utilizzando le seguenti API:

* [Recuperare i profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recupera profilo per mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recupera profilo per codice specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Per informazioni sulla struttura dei profili, consulta le sezioni **Risposta** e **Esempi** delle API precedenti.

Per ulteriori dettagli su come e quando integrare le API di cui sopra, consulta i seguenti documenti:

* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [Domande frequenti sulla fase di autenticazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
