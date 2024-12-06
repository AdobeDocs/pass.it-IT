---
title: Profili di base - Applicazione secondaria - Flusso
description: REST API V2 - Profili di base - Applicazione secondaria - Flusso
exl-id: 1fcefcfa-7534-4b85-b3b5-df513685d66b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 0%

---

# Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria {#basic-profiles-flow-secondary-application}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione REST API V2 è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

Il flusso **Profili** all&#39;interno del diritto di autenticazione di Adobe Pass consente all&#39;applicazione secondaria di accedere alle informazioni sugli accessi utente attivi.

Il flusso dei profili di base consente di eseguire query per i seguenti scenari:

* [Recupera profilo per codice specifico](#retrieve-profile-for-specific-code)

## Recupera profilo per codice specifico {#retrieve-profile-for-specific-code}

### Prerequisiti {#prerequisites-retrieve-profile-for-specific-code}

Prima di recuperare il profilo per un codice di autenticazione specifico, verifica che siano soddisfatti i seguenti prerequisiti:

* L&#39;applicazione secondaria, che ha un `code` utilizzato per eseguire l&#39;autenticazione interattiva con MVPD, desidera recuperare il profilo per un codice di autenticazione specifico.

### Flusso di lavoro {#workflow-retrieve-profile-for-specific-code}

Segui i passaggi forniti per implementare il flusso di recupero del profilo di base per un codice di autenticazione specifico eseguito all’interno di un’applicazione secondaria, come illustrato nel diagramma seguente.

![Recupera profilo per codice specifico](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*Recupera profilo per codice specifico*

1. **Recupera profilo per codice specifico:** L&#39;applicazione secondaria raccoglie tutti i dati necessari per recuperare le informazioni del profilo per il codice di autenticazione specifico inviando una richiesta all&#39;endpoint Profili.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recupera profilo per codice specifico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md), consulta la documentazione API per:
   >
   > * Tutti i parametri _required_, come `serviceProvider` e `code`
   > * Tutte le intestazioni _required_, come `Authorization`
   > * Tutti i parametri e le intestazioni _optional_

1. **Trova profilo regolare:** Il server Adobe Pass identifica un profilo valido in base ai parametri e alle intestazioni ricevuti.

1. **Restituisci informazioni sul profilo regolare:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo trovato associato ai parametri e alle intestazioni ricevuti.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta al profilo, consulta la documentazione API [Recupera profilo per codice specifico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).
   > 
   > <br/>
   > 
   > L’endpoint Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Indicare il completamento del flusso di autenticazione:** Se la risposta dell&#39;endpoint Profiles contiene un profilo, l&#39;applicazione secondaria elabora la risposta e può utilizzarla per visualizzare facoltativamente un messaggio specifico nell&#39;interfaccia utente.

1. **Indica che il flusso di autenticazione ha rilevato un problema:** Se la risposta dell&#39;endpoint Profiles non contiene un profilo, l&#39;applicazione secondaria elabora la risposta e può utilizzarla per visualizzare facoltativamente un messaggio specifico nell&#39;interfaccia utente.
