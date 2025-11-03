---
title: Funzione di degradazione
description: Funzione di degradazione
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Funzione di degradazione {#degradation-feature}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Nel dinamico mondo degli sport dal vivo e degli eventi più importanti, garantire un&#39;esperienza di visione fluida è essenziale. Durante questi eventi, il traffico elevato può sopraffare gli endpoint di autenticazione e autorizzazione del server di distribuzione di programmi video multicanale (MVPD), causando ritardi o interruzioni.

L&#39;autenticazione Adobe Pass affronta queste problematiche con la sua **funzionalità di degrado**, una soluzione che consente di ignorare temporaneamente endpoint specifici di autenticazione e autorizzazione MVPD. Questa funzione è particolarmente utile durante gli eventi di traffico di picco, dove i tempi di risposta possono deteriorarsi a causa di un carico elevato sui sistemi MVPD.

La **funzionalità di degradazione** può essere una protezione fondamentale per i programmatori, garantendo la continuità del servizio. Sebbene il pubblico principale comprenda eventi sportivi live e canali di notizie, la sua utilità si estende a tutti i programmatori che cercano di attenuare il rischio di interruzioni causate dagli endpoint MVPD.

>[!IMPORTANT]
>
> L’API di degradazione è una funzione premium e richiede una licenza corrente da Adobe.

Applicando una regola di degradazione, i programmatori possono temporaneamente abilitare l&#39;autenticazione automatica o l&#39;autorizzazione automatica, garantendo un accesso ininterrotto al contenuto per il periodo in cui viene applicata la degradazione. Le azioni di degrado sono sempre avviate da un programmatore sulla base di accordi prestabiliti con gli MVPD. Anche se al momento Adobe non attiva direttamente il degrado, le funzionalità future potrebbero includere la gestione proattiva nel caso in cui vengano stabiliti accordi sui livelli di servizio (SLA).

Questa funzione è progettata per essere utilizzata insieme a un&#39;[API di monitoraggio dell&#39;utilizzo](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md) e in base ai pre-accordi con gli MVPD, l&#39;autenticazione di Adobe Pass offre uno strumento potente per bilanciare l&#39;esperienza utente, l&#39;affidabilità e il controllo operativo durante i momenti critici.

>[!IMPORTANT]
>
> Questa funzione non consente di bypassare il servizio di autenticazione di Adobe Pass. Se Autenticazione Adobe Pass non è disponibile, non è disponibile alcun meccanismo integrato all’interno di questo servizio per facilitare l’accesso degli utenti. In tali casi, siti o applicazioni possono scegliere di implementare proprie soluzioni di routing alternative per mantenere la distribuzione dei contenuti.

## Accesso all’API di degradazione {#degradation-api-access}

Prima di accedere all&#39;[API di degradazione](#degradation-api), è necessario completare i passaggi richiesti nel processo di registrazione del client dinamico (DCR). Questo processo obbligatorio garantisce che tu disponga del token di accesso necessario per interagire con l’API di degradazione.

Per istruzioni complete, consulta la documentazione [Panoramica registrazione client dinamico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## API di degradazione {#degradation-api}

Degradation API è un’API RESTful che consente ai programmatori di gestire le regole di degradazione per MVPD specifici. L’API consente di attivare, rimuovere e recuperare lo stato delle regole di degradazione attive.

Per ulteriori informazioni sull&#39;API Degradation, fare riferimento al seguente documento di Zendesk [Autenticazione Adobe Pass | API di degradazione v3](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3) e cercare il file PDF da scaricare.

## API REST V2 {#rest-api-v2}

Per sfruttare la funzione di degradazione è necessario implementare aggiornamenti del codice per modificare l&#39;interazione dell&#39;applicazione TV Everywhere (TVE) con l&#39;autenticazione Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

Per una guida completa su questi aggiornamenti e sui flussi di lavoro associati, consulta la documentazione [Flussi di accesso danneggiati](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).
