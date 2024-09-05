---
title: Panoramica di Registrazione client dinamici
description: Panoramica di Registrazione client dinamici
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 0%

---


# Panoramica di Registrazione client dinamici {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

La registrazione client dinamica rappresenta un meccanismo di autorizzazione definito da [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) e si basa sul framework di autorizzazione OAuth 2.0 descritto da [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

Adobe Pass fornisce un servizio di registrazione client dinamico che consente di accedere alle seguenti API protette:

* API di gestione autenticazione di Adobe Pass:
   * [Ripristina API passaggio temporaneo](../reset-temp-pass.md)
   * [API di degradazione](../degradation-api-overview.md)
   * [API MVPD proxy](../proxy-mvpd-webserv.md)
   * [API di monitoraggio del servizio di adesione](../entitlement-service-monitoring-api.md)
* API REST di autenticazione Adobe Pass:
   * [API REST V1](../rest-api-reference.md)
   * [API REST V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
* SDK di autenticazione Adobe Pass:
   * [SDK per JavaScript](../javascript-sdk-api-reference.md)
   * [SDK per iOS/tvOS](../iostvos-sdk-api-reference.md)
   * [SDK di Android](../android-sdk-api-reference.md)
   * [SDK di FireOS](../amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> Il meccanismo di autorizzazione della registrazione client dinamica sostituisce le precedenti soluzioni di autenticazione Adobe Pass soggette a interruzione:
>
> * Meccanismo ID richiedente firmato.
> * Il meccanismo di elenco dei domini.
> * Il meccanismo chiave API.

Con l’adozione della registrazione dinamica dei clienti i vantaggi principali sono:

* Maggiore sicurezza.
* Modello unificato su più piattaforme.
* Controllo dettagliato del ciclo di vita dell&#39;applicazione.

Per ulteriori informazioni su come gestire e utilizzare la registrazione client dinamica, consulta le sezioni seguenti.

## Dynamic Client Registration Management {#dynamic-client-registration-management}

Il processo di gestione della registrazione client dinamica consente alle applicazioni client in esecuzione su piattaforme specifiche e che necessitano dell&#39;accesso a API di autenticazione Adobe Pass specifiche di registrarsi tramite il [dashboard TVE di Adobe Pass](https://console.auth.adobe.com/).

Adobe Pass TVE Dashboard è uno strumento che consente ai clienti di autenticazione Adobe Pass (programmatori) di gestire la configurazione e i dati. Questa dashboard self-service abilita una serie di funzionalità descritte nella [Guida utente di Adobe Pass TVE Dashboard](../tve-dashboard-user-guide.md).

Se hai accesso a [Adobe Pass TVE Dashboard](https://console.auth.adobe.com/), segui i passaggi descritti nelle sezioni seguenti per creare un&#39;applicazione registrata e scaricare l&#39;istruzione software.

### Gestire le applicazioni registrate {#manage-registered-applications}

>[!IMPORTANT]
>
> Se non hai accesso a Adobe Pass TVE Dashboard, crea un ticket tramite il nostro [Zendesk](https://adobeprimetime.zendesk.com) e chiedi al tuo Technical Account Manager (TAM) di creare un&#39;applicazione registrata e condividere con te l&#39;istruzione software.

È possibile creare un&#39;applicazione registrata in due modi:

* **Livello programmatore**

  Il processo di registrazione a livello di programmatore consente di creare un&#39;applicazione registrata collegata a tutti i canali disponibili o a un sottoinsieme selezionato di canali. Per ulteriori dettagli, consulta la sezione [Creare un&#39;applicazione registrata a livello di programmatore](../tve-dashboard-user-guide.md#create-registered-application-programmer-level) dalla [Guida utente di TVE Dashboard](../tve-dashboard-user-guide.md).


* **Livello canale**

  Il processo di registrazione a livello di canale consente di creare un&#39;applicazione registrata collegata solo al canale corrente selezionato. Per ulteriori dettagli, consulta la sezione [Creare un&#39;applicazione registrata a livello di canale](../tve-dashboard-user-guide.md#create-registered-application-channel-level) dalla [Guida utente di TVE Dashboard](../tve-dashboard-user-guide.md).

>[!IMPORTANT]
>
> Si consiglia di creare applicazioni registrate con autorizzazioni più specifiche e limitate per migliorare la sicurezza e impedire l’accesso non autorizzato. Pertanto, durante la creazione di applicazioni registrate, è consigliabile utilizzare opzioni più ristrette per `channels`, `platforms` e `scopes` assegnati.
>
> Si consiglia di creare una nuova applicazione registrata per ogni aggiornamento principale dell&#39;applicazione client per gestirne il ciclo di vita e l&#39;utilizzo. Se necessario, crea un ticket tramite [Zendesk](https://adobeprimetime.zendesk.com) e chiedi al tuo Technical Account Manager (TAM) di revocare un&#39;applicazione registrata per bloccare la funzionalità di una versione specifica dell&#39;applicazione client.

### Gestire le istruzioni software {#manage-software-statements}

>[!IMPORTANT]
>
> Se non hai accesso a Adobe Pass TVE Dashboard, crea un ticket tramite il nostro [Zendesk](https://adobeprimetime.zendesk.com) e chiedi al tuo Technical Account Manager (TAM) di creare un&#39;applicazione registrata e condividere con te l&#39;istruzione software.

Prima di scaricare un&#39;istruzione software, verificare di aver creato un&#39;applicazione registrata come descritto nella sezione [Gestione delle applicazioni registrate](#manage-registered-applications) che soddisfi i requisiti dell&#39;applicazione client.

Sono disponibili due modi per scaricare un&#39;istruzione software in base al livello di creazione dell&#39;applicazione registrata:

* **Livello programmatore**

  Per ulteriori dettagli, fare riferimento alla sezione [Scaricare un&#39;istruzione software a livello di programmatore](../tve-dashboard-user-guide.md#download-software-statement-programmer-level) dalla [Guida utente di TVE Dashboard](../tve-dashboard-user-guide.md).

* **Livello canale**

  Per ulteriori dettagli, consulta la sezione [Scaricare un&#39;istruzione software a livello di canale](../tve-dashboard-user-guide.md#download-software-statement-channel-level) dalla [Guida utente di TVE Dashboard](../tve-dashboard-user-guide.md).

L&#39;istruzione software è un token Web JSON (`JWT`) che contiene informazioni sul software dell&#39;applicazione client come bundle. Quando viene presentata all&#39;API [Recupera credenziali client](./apis/dynamic-client-registration-apis-retrieve-client-credentials.md), l&#39;istruzione software è firmata digitalmente utilizzando la firma Web JSON (`JWS`).

Per informazioni più dettagliate sulle istruzioni software e sul loro funzionamento, consultare la documentazione di [RFC 7591](https://tools.ietf.org/html/rfc7591).

## Flusso di registrazione client dinamici  {#dynamic-client-registration-flow}

In sintesi, il meccanismo dinamico di autorizzazione della registrazione dei clienti prevede diversi passaggi:

**Gestione**

* Un rappresentante del client deve creare un&#39;applicazione registrata come descritto nella sezione [Gestione applicazioni registrate](#manage-registered-applications).
* Un rappresentante del client deve scaricare e incorporare un&#39;istruzione software come descritto nella sezione [Gestione istruzioni software](#manage-software-statements).

**Flusso**

* L&#39;applicazione client deve ottenere le credenziali client come descritto nella documentazione API [Recupera credenziali client](./apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
* L&#39;applicazione client deve ottenere il token di accesso come descritto nella documentazione API [Recupera token di accesso](./apis/dynamic-client-registration-apis-retrieve-access-token.md).

Consulta la documentazione [Flusso di registrazione client dinamico](./flows/dynamic-client-registration-flow.md) per informazioni su come accedere alle API protette di Adobe Pass. Inoltre, puoi anche guardare questa registrazione del [webinar](https://my.adobeconnect.com/pzkp8ujrigg1/), che fornisce più contesto e include una demo.