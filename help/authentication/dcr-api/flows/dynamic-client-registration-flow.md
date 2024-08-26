---
title: Flusso di registrazione client dinamici
description: Flusso di registrazione client dinamici
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---


# Flusso di registrazione client dinamici {#dynamic-client-registration-flow}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione dell&#39;API Dynamic Client Registration è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/throttling-mechanism.md).

## Accedere alle API protette di Adobe Pass {#access-adobe-pass-protected-apis}

### Prerequisiti {#prerequisites-access-adobe-pass-protected-apis}

Prima di accedere alle API protette di Adobe Pass, assicurati di soddisfare i seguenti prerequisiti:

* Un rappresentante del client deve creare un&#39;applicazione registrata come descritto nella sezione [Gestione applicazioni registrate](../dynamic-client-registration-overview.md#manage-registered-applications).
* Un rappresentante del client deve scaricare e incorporare un&#39;istruzione software come descritto nella sezione [Gestione istruzioni software](../dynamic-client-registration-overview.md#manage-software-statements).

>[!IMPORTANT]
>
> Gli SDK di autenticazione di Adobe Pass sono responsabili dell’ottenimento e dell’aggiornamento delle credenziali del client e del token di accesso per conto dell’applicazione client.
> 
> Per tutte le altre API protette di Adobe Pass, l’applicazione client deve seguire il flusso di lavoro riportato di seguito.

### Flusso di lavoro {#workflow-access-adobe-pass-protected-apis}

Segui i passaggi forniti per accedere alle API protette da Adobe Pass, come illustrato nel diagramma seguente.

![Accesso alle API protette di Adobe Pass](../../assets/dcr-api/dcr-api-access-adobe-pass-protected-apis.png)

*Accesso alle API protette di Adobe Pass*

1. **Recupera credenziali client:** L&#39;applicazione client raccoglie tutti i dati necessari per recuperare le credenziali client chiamando l&#39;endpoint Registro client.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recuperare le credenziali del client](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request), consulta la documentazione API di:
   >
   > * Tutti i parametri _required_, come `software_statement`
   > * Tutte le intestazioni _required_, come `Content-Type`, `X-Device-Info`
   > * Tutti i parametri e le intestazioni _optional_

1. **Restituisci credenziali client:** La risposta dell&#39;endpoint Registro client contiene informazioni sulle credenziali client associate ai parametri e alle intestazioni ricevuti.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta alle credenziali del client, consultare la documentazione API [Recupera credenziali client](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success).
   >
   > <br/>
   >
   > Il registro client convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione API [Retrieve client credentials](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error).

   >[!TIP]
   >
   > Suggerimento: le credenziali del client devono essere memorizzate nella cache e possono essere utilizzate a tempo indefinito.

1. **Recupera token di accesso:** L&#39;applicazione client raccoglie tutti i dati necessari per recuperare il token di accesso chiamando l&#39;endpoint del token client.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recupera token di accesso](../apis/dynamic-client-registration-apis-retrieve-access-token.md#request), consulta la documentazione API di:
   >
   > * Tutti i parametri _required_, come `client_id`, `client_secret` e `grant_type`
   > * Tutte le intestazioni _required_, come `Content-Type`, `X-Device-Info`
   > * Tutti i parametri e le intestazioni _optional_

1. **Token di accesso restituito:** La risposta dell&#39;endpoint del token client contiene informazioni sul token di accesso associato ai parametri e alle intestazioni ricevuti.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta del token di accesso, consulta la documentazione API [Recupera token di accesso](../apis/dynamic-client-registration-apis-retrieve-access-token.md#success).
   >
   > <br/>
   >
   > Il token client convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione API [Recupera token di accesso](../apis/dynamic-client-registration-apis-retrieve-access-token.md#error).

   >[!TIP]
   >
   > Suggerimento: il token di accesso deve essere memorizzato in cache e utilizzato solo entro la durata specificata (ad esempio, time-to-live di 24 ore). Dopo la scadenza, l’applicazione client deve richiedere un nuovo token di accesso.

1. **Procedere con l&#39;accesso alle API protette:** L&#39;applicazione client utilizza il token di accesso per accedere ad altre API protette di Adobe Pass. L&#39;applicazione client deve includere il token di accesso nell&#39;intestazione della richiesta `Authorization` utilizzando lo schema di autenticazione `Bearer` (ovvero `Authorization: Bearer <access_token>`).

   >[!IMPORTANT]
   >
   > Le API protette di Adobe Pass convalidano il token di accesso per garantire che siano soddisfatte le condizioni di base:
   >
   > * _access_token_ deve essere valido.
   > * Il _access_token_ deve essere associato a un _client_id_ e a un _client_secret_ validi.
   > * Il _access_token_ deve essere associato a un _software_statement_ valido.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../enhanced-error-codes.md).
