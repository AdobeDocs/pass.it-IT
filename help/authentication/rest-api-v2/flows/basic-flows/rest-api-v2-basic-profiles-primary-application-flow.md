---
title: Profili di base - Applicazione principale - Flusso
description: REST API V2 - Profili di base - Applicazione principale - Flusso
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 0%

---


# Flusso dei profili di base eseguito all’interno dell’applicazione principale {#basic-profiles-flow-primary-application}

>[!NOTE]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Il flusso **Profili** all&#39;interno del diritto Autenticazione di Adobe Pass consente all&#39;applicazione di streaming di accedere alle informazioni sugli accessi utente attivi.

Il flusso dei profili di base consente di eseguire query per i seguenti scenari:

* [Recuperare i profili](#retrieve-profiles)
* [Recupera profilo per mvpd specifico](#retrieve-profile-for-specific-mvpd)
* [Recupera profilo per codice specifico](#retrieve-profile-for-specific-code)

## Recuperare i profili {#retrieve-profiles}

### Prerequisiti {#prerequisites-retrieve-profiles}

Prima di recuperare i profili, verifica che siano soddisfatti i seguenti prerequisiti:

* L’applicazione di streaming desidera recuperare tutti i profili regolari.

### Flusso di lavoro {#workflow-retrieve-profiles}

Segui i passaggi forniti per implementare il flusso di recupero dei profili di base eseguito all’interno di un’applicazione principale, come illustrato nel diagramma seguente.

![Recupera profili](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-profiles-within-primary-application.png)

*Recupera profili*

1. **Recupera profili:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare tutte le informazioni sui profili inviando una richiesta all&#39;endpoint Profili.

   Per informazioni dettagliate su [Recuperare i profili](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md), consulta la documentazione API di:
   * Tutti i parametri _required_, come `serviceProvider`
   * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

1. **Trova profili regolari:** Il server Adobe Pass identifica tutti i profili validi in base ai parametri e alle intestazioni ricevuti.

1. **Restituisci informazioni sui profili regolari:** La risposta dell&#39;endpoint Profiles contiene informazioni sui profili trovati associati ai parametri e alle intestazioni ricevuti.

   Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione dell&#39;API [Recupera profili](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md).

   >[!NOTE]
   >
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Scegli un profilo e procedi con i flussi di decisioni:** Se la risposta dell&#39;endpoint Profili contiene profili, l&#39;applicazione di streaming utilizza la propria logica interna (eventualmente interagendo con l&#39;utente finale) per scegliere uno dei profili disponibili per continuare con i flussi di decisioni successivi.

1. **Indicare un nuovo flusso di autenticazione di base:** Se la risposta dell&#39;endpoint Profiles non contiene un profilo, l&#39;applicazione di streaming indica all&#39;utente di avviare un nuovo flusso di autenticazione di base.

## Recupera profilo per mvpd specifico {#retrieve-profile-for-specific-mvpd}

### Prerequisiti {#prerequisites-retrieve-profile-for-specific-mvpd}

Prima di recuperare il profilo per un MVPD specifico, accertati che siano soddisfatti i seguenti prerequisiti:

* L&#39;applicazione di streaming, che ha un identificatore `mvpd` selezionato o memorizzato nella cache, desidera recuperare il profilo regolare per un MVPD specifico.

### Flusso di lavoro {#workflow-retrieve-profile-for-specific-mvpd}

Segui i passaggi forniti per implementare il flusso di recupero del profilo di base per uno specifico MVPD eseguito all’interno di un’applicazione primaria, come illustrato nel diagramma seguente.

![Recupera profilo per mvpd](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-mvpd.png) specifico

*Recupera profilo per mvpd* specifico

1. **Recupera profilo per mvpd specifico:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare le informazioni sul profilo per MVPD specifico inviando una richiesta all&#39;endpoint Profili.

   Per informazioni dettagliate su [Recupera profilo per documentazione API mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) specifica, consulta:
   * Tutti i parametri _required_, come `serviceProvider` e `mvpd`
   * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

1. **Trova profilo regolare:** Il server Adobe Pass identifica un profilo valido in base ai parametri e alle intestazioni ricevuti.

1. **Restituisci informazioni sul profilo regolare:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo trovato associato ai parametri e alle intestazioni ricevuti.

   Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione API [Recupera profilo per mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) specifico.

   >[!NOTE]
   >
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Procedi con i flussi di decisioni:** Se la risposta dell&#39;endpoint Profiles contiene un profilo, l&#39;applicazione di streaming utilizza le informazioni sul profilo per continuare con i flussi di decisioni successivi.

1. **Indicare un nuovo flusso di autenticazione di base:** Se la risposta dell&#39;endpoint Profiles non contiene un profilo, l&#39;applicazione di streaming indica all&#39;utente di avviare un nuovo flusso di autenticazione di base.

## Recupera profilo per codice specifico {#retrieve-profile-for-specific-code}

### Prerequisiti {#prerequisites-retrieve-profile-for-specific-code}

Prima di recuperare il profilo per un codice di autenticazione specifico, verifica che siano soddisfatti i seguenti prerequisiti:

* L&#39;applicazione di streaming, che dispone di un `code` utilizzato per eseguire l&#39;autenticazione interattiva con MVPD, desidera recuperare il profilo per un codice di autenticazione specifico.

### Flusso di lavoro {#workflow-retrieve-profile-for-specific-code}

Segui i passaggi forniti per implementare il flusso di recupero del profilo di base per un codice di autenticazione specifico eseguito all’interno di un’applicazione primaria, come illustrato nel diagramma seguente.

![Recupera profilo per codice specifico](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-code.png)

*Recupera profilo per codice specifico*

1. **Recupera profilo per codice specifico:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare le informazioni sul profilo per il codice di autenticazione specifico inviando una richiesta all&#39;endpoint Profili.

   Per informazioni dettagliate su [Recupera profilo per codice specifico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md), consulta la documentazione API per:
   * Tutti i parametri _required_, come `serviceProvider` e `code`
   * Tutte le intestazioni _required_, come `Authorization`
   * Tutti i parametri e le intestazioni _optional_

1. **Trova profilo regolare:** Il server Adobe Pass identifica un profilo valido in base ai parametri e alle intestazioni ricevuti.

1. **Restituisci informazioni sul profilo regolare:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo trovato associato ai parametri e alle intestazioni ricevuti.

   Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione API [Recupera profilo per codice specifico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md).

   >[!NOTE]
   >
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Procedi con i flussi di decisioni:** Se la risposta dell&#39;endpoint Profiles contiene un profilo, l&#39;applicazione di streaming utilizza le informazioni sul profilo per continuare con i flussi di decisioni successivi.

1. **Indicare un nuovo flusso di autenticazione di base:** Se la risposta dell&#39;endpoint Profiles non contiene un profilo, l&#39;applicazione primaria indica all&#39;utente di avviare un nuovo flusso di autenticazione di base.
