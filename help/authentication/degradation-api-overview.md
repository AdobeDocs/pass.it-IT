---
title: Panoramica dell’API di degradazione
description: Panoramica dell’API di degradazione
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---


# Panoramica dell’API di degradazione {#degradation-api-overview}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Prima di utilizzare l’API di degradazione, assicurati di soddisfare i seguenti prerequisiti:
>
> * Ottenere le credenziali del client come descritto nella [Documentazione API per il recupero delle credenziali del client](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Ottenere il token di accesso come descritto nella documentazione API [Recuperare il token di accesso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Per ulteriori informazioni su come creare un&#39;applicazione registrata e scaricare l&#39;istruzione software, consultare la documentazione [Panoramica registrazione client dinamica](./dcr-api/dynamic-client-registration-overview.md).

## Informazioni generali {#general_info}

>[!NOTE]
>
>Questa API non è generalmente disponibile. Per aggiornamenti sulla disponibilità, contatta il rappresentante di Adobe.

Questa funzione fornisce a una delle tre parti principali di un’integrazione (Programmatori, MVPD e Adobi) la possibilità di bypassare temporaneamente endpoint specifici di autenticazione e autorizzazione MVPD. Di solito è il programmatore che avvia tale azione, ma indipendentemente da chi attiva un evento di degrado, l&#39;azione dipende da accordi precedentemente concordati con gli MVPD interessati.

Il caso d’uso principale di questa funzione si verifica durante sport live o grandi eventi. In scenari di traffico così elevato è possibile che il carico su un endpoint MVPD specifico diventi troppo elevato, con conseguenti tempi di risposta molto lunghi per gli utenti. Al fine di preservare una buona esperienza utente durante tale scenario, il programmatore può decidere di attivare una regola di degradazione che può temporaneamente autenticare/autorizzare automaticamente gli utenti, o disabilitare un MVPD rimuovendolo dall&#39;elenco MVPD disponibile.

Una regola di degradazione viene applicata solo per un periodo di tempo fisso. Anche se i principali clienti di questa funzione sono i canali sportivi e i canali di notizie in diretta, qualsiasi programmatore potrebbe desiderare di avere accesso a questa funzione, in quanto i servizi MVPD si riducono di tanto in tanto.

Note sulla degradazione:

: questa funzione è progettata per essere utilizzata insieme all’API di monitoraggio dell’utilizzo, che fornisce informazioni in tempo reale sul numero di autenticazioni e autorizzazioni per MVPD, latenza media delle autorizzazioni e altre metriche necessarie per una panoramica completa del servizio.
- Questa funzione non consente di bypassare il servizio di autenticazione Adobe Primetim. Se l’autenticazione Adobe Pass non è attiva, all’interno del servizio non è disponibile alcun meccanismo che possa essere utilizzato per consentire agli utenti di visualizzare i contenuti. I siti o le app potrebbero, tuttavia, aggirare l’autenticazione di Adobe Pass da soli.
- L&#39;Adobe non innescherà direttamente il degrado - la decisione deve sempre risiedere con un programmatore specifico che ha accettato tali condizioni con MVPDs. In futuro, l’autenticazione di Adobe Pass potrebbe essere proattiva nell’attivazione delle regole di degradazione se è possibile raggiungere accordi (protezione SLA) con gli MVPD.

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
