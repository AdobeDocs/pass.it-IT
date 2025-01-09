---
title: Panoramica del dashboard di TVE
description: Conoscere TVE Dashboard e le risorse.
exl-id: 91baeb34-a32a-4dc3-94d8-f6cfca59dc4e
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# Panoramica del dashboard di TVE {#tve-db-overview}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Il [[!DNL Adobe] Pass TVE Dashboard](https://experience.adobe.com/pass/authentication) è uno strumento che consente ai clienti di Adobe Pass Authentication (programmatori) di gestire la configurazione e i dati. Questo dashboard self-service abilita una serie di funzionalità, ad esempio:

* **Gestione dell&#39;integrazione**: aggiungi nuove integrazioni tra i rispettivi marchi (canali) di un programmatore e di MVPD (Multichannel Video Programming Distributors) nell&#39;ecosistema di autenticazione di Adobe Pass.

* **Configurazione delle proprietà**: configurare più proprietà per ogni integrazione per implementare regole business granulari personalizzate in base alle esigenze specifiche della piattaforma.

* **Generazione report**: accedere ed esportare report dettagliati sulla configurazione in MVPDs. Tali rapporti includono:
   * Categorie di piattaforme come *Dispositivi connessi a desktop, dispositivi mobili e TV*
   * Piattaforme come *iOS, Android™, tvOS, Roku e FireTV*

  I rapporti forniscono informazioni sul supporto del Single Sign-On (SSO) e sulla durata delle sessioni di autenticazione o autorizzazione degli abbonati a livello di MVPD e piattaforma.

* **Visualizzazione traffico**: visualizza i dati relativi al traffico di autenticazione e autorizzazione di alto livello delle proprietà del programmatore.

Contatta il tuo Technical Account Manager (TAM) per accedere alla dashboard di TVE. Questo accesso richiede la configurazione di due nuovi gruppi di utenti all’interno dell’organizzazione Adobe Marketing Cloud:

* **Lettura/scrittura dashboard TVE**: i membri di questo gruppo dispongono di diritti di modifica completi in tutte le sezioni del dashboard.
* **Dashboard TVE di sola lettura**: ai membri di questo gruppo viene concesso l&#39;accesso in sola visualizzazione all&#39;intero dashboard.

L’autenticazione di Adobe Pass fornisce le seguenti sezioni nel dashboard TVE:

* [Ambienti](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
* [Revisione e invio di modifiche](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
* [Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
* [Canali](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
* [Programmatori](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
* [MVPD](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
* [Integrazioni](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
* [Rapporti](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
* [Registro delle modifiche](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)

## Risorse {#resources-tve-dashboard}

Adobe consiglia di utilizzare le risorse seguenti per comprendere in modo approfondito i flussi e le funzioni, per acquisire familiarità con la terminologia utilizzata in questa guida:

* [Documento tecnico TVE](/help/authentication/kickstart/technical-paper.md)
* [Guida di Kick-Start per programmatori](/help/authentication/kickstart/programmer-kickstart-guide.md)
* [Flusso diritto](/help/authentication/integration-guide-programmers/entitlement-flow.md)
* [Glossario di Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)
* [Glossario REST API v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
