---
title: Codici di errore migliorati
description: Codici di errore migliorati
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '2593'
ht-degree: 2%

---

# Codici di errore migliorati {#enhanced-error-codes}

>[!IMPORTANT]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

I codici di errore avanzati rappresentano una funzione di autenticazione di Adobe Pass che fornisce informazioni aggiuntive sugli errori alle applicazioni client integrate con:

* API REST di autenticazione Adobe Pass:
   * [API REST v1](./rest-api-overview.md)
   * [API REST v2](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
* API di preautorizzazione SDK per autenticazione Adobe Pass:
   * [JavaScript SDK (API di preautorizzazione)](./preauthorize-api-javascript-sdk.md)
   * [SDK iOS/tvOS (API di preautorizzazione)](./preauthorize-api-ios-tvos-sdk.md)
   * [SDK di Android (API di preautorizzazione)](./preauthorize-api-android-sdk.md)

  _(*) L&#39;API di preautorizzazione è l&#39;unica API SDK di autenticazione di Adobe Pass che fornisce supporto per codici di errore avanzati._

>[!IMPORTANT]
>
> Le applicazioni che integrano Adobe Pass Authentication REST API v2 beneficeranno di codici di errore avanzati per impostazione predefinita senza richiedere una configurazione aggiuntiva.
>
> <br/>
>
> Le applicazioni che integrano Adobe Pass Authentication REST API v1 o SDK Preauthorize API possono beneficiare di codici di errore avanzati solo se la funzione è abilitata esplicitamente.
>
> <br/>
>
> Per abilitare esplicitamente questa funzione, crea un ticket tramite la nostra [Zendesk](https://adobeprimetime.zendesk.com) e chiedi assistenza al tuo Technical Account Manager (TAM).

## Rappresentazione {#enhanced-error-codes-representation}

I codici di errore avanzati possono essere rappresentati nel formato `JSON` o `XML` a seconda dell&#39;API di autenticazione Adobe Pass integrata e del valore di intestazione &quot;Accept&quot; utilizzato (ad esempio `application/json` o `application/xml`):

| API di autenticazione di Adobe Pass | JSON | XML |
|-------------------------------|---------|---------|
| API REST v1 | &amp;check; | &amp;check; |
| API REST v2 | &amp;check; |         |
| API di pre-autorizzazione SDK | &amp;check; |         |

>[!IMPORTANT]
>
> L’autenticazione Adobe Pass può comunicare codici di errore avanzati alle applicazioni client in due forme:
>
> <br/>
>
> * **Informazioni sull&#39;errore di primo livello**: in questo caso, l&#39;oggetto ***&quot;error&quot;*** si trova di primo livello, pertanto il corpo della risposta può contenere solo l&#39;oggetto ***&quot;error&quot;***.
> * **Informazioni sull&#39;errore a livello di elemento**: in questo caso, l&#39;oggetto ***&quot;error&quot;*** si trova a livello di elemento, pertanto il corpo della risposta può contenere un oggetto ***&quot;error&quot;*** per tutti gli elementi che hanno riscontrato un errore durante la manutenzione.
>
> <br/>
>
> Per determinare le specifiche della rappresentazione dei codici di errore avanzati, consulta la documentazione pubblica di ogni API di autenticazione di Adobe Pass integrata.

Fare riferimento alle seguenti risposte HTTP contenenti esempi di codici di errore avanzati rappresentati come `JSON` o `XML`.

>[!BEGINTABS]

>[!TAB API REST v1 - Informazioni di errore di primo livello (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB API REST v1 - Informazioni di errore di primo livello (XML)]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!TAB API REST v1 - Informazioni sull&#39;errore a livello di elemento (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resources": [
    {
      "id": "TestStream1",
      "authorized": true
    },
    {
      "id": "TestStream2",
      "authorized": false,
      "error": {
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB API REST v2 - Informazioni di errore di primo livello (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!TAB API REST v2 - Informazioni di errore a livello di elemento (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "decisions": [
    {
      "resource": "REF30",
      "serviceProvider": "REF30",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": true,
      "token": {
        "issuedAt": 1697094207324,
        "notBefore": 1697094207324,
        "notAfter": 1697094802367,
        "serializedToken": "PHNpZ25hdHVyZUluZm8..."
      }
    },
    {
      "resource": "REF40",
      "serviceProvider": "REF40",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": false,
      "error" : {
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!ENDTABS]

I codici di errore avanzati includono i seguenti campi `JSON` o attributi `XML`:

| Nome | Tipo | Esempio | Limitato | Descrizione |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *azione* | *stringa* | *riprova* | &amp;check; | L’autenticazione di Adobe Pass ha consigliato un’azione che potrebbe risolvere la situazione come definito in questo documento. <br/><br/> Per ulteriori dettagli, consulta la sezione [Azione](#enhanced-error-codes-action). |
| *stato* | *numero intero* | *403* | &amp;check; | Il codice di stato della risposta HTTP come definito nel documento [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6). <br/><br/> Per ulteriori dettagli, consulta la sezione [Stato](#enhanced-error-codes-status). |
| *codice* | *stringa* | *errore_connessione_rete* | &amp;check; | Il codice identificativo univoco dell’autenticazione Adobe Pass associato all’errore come definito in questo documento. <br/><br/> Per ulteriori dettagli, consulta la sezione [Codice](#enhanced-error-codes-code). |
| *messaggio* | *stringa* | *Impossibile contattare il provider TV* |            | Il messaggio leggibile che in alcuni casi può essere visualizzato all’utente finale. <br/><br/> Per ulteriori dettagli, consulta la sezione [Gestione delle risposte](#enhanced-error-codes-response-handling). |
| *dettagli* | *stringa* | *Il pacchetto di abbonamento non include il canale &quot;Live&quot;* |            | Messaggio dettagliato che potrebbe essere fornito da un partner di servizi in alcuni casi, <br/><br/> Questo campo potrebbe non essere presente nel caso in cui il partner di servizi non fornisca alcun messaggio personalizzato. |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html* |            | L’URL della documentazione pubblica di Autenticazione di Adobe Pass che rimanda a ulteriori informazioni sul motivo di questo errore e sulle possibili soluzioni. <br/><br/> Questo campo contiene un URL assoluto e non deve essere dedotto dal codice di errore, a seconda del contesto di errore è possibile fornire un URL diverso. |
| *traccia* | *stringa* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | Identificatore univoco della risposta che può essere utilizzato quando si contatta il supporto per l’autenticazione di Adobe Pass per la risoluzione di problemi specifici. |

>[!IMPORTANT]
>
> La colonna **Limitato** indica se il rispettivo campo contiene un valore di un set finito, mentre i campi senza restrizioni possono contenere qualsiasi dato.
>
> <br/>
>
> I futuri aggiornamenti di questo documento potrebbero aggiungere valori ai set finiti, ma non rimuoveranno o modificheranno quelli esistenti.

### Azione {#enhanced-error-codes-representation-action}

I codici di errore avanzati includono un campo &quot;action&quot; che fornisce un’azione consigliata che potrebbe risolvere la situazione.

I valori possibili per il campo &quot;azione&quot; includono:

| Azione | Descrizione | Categoria |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| nessuno | Non esiste un’azione predefinita per risolvere questo problema, ma in alcuni casi ciò potrebbe indicare una chiamata impropria dell’API. | Correggi il contesto della richiesta. |
| configurazione | L’applicazione client richiede una modifica della configurazione, la maggior parte del tempo eseguita tramite Adobe Pass TVE Dashboard. | Correggi il contesto di configurazione dell’integrazione. |
| registrazione dell&#39;applicazione | L&#39;applicazione client richiede di registrarsi di nuovo. | Correggere il contesto dell&#39;applicazione client. |
| autenticazione | L&#39;applicazione client richiede l&#39;autenticazione o la riautenticazione dell&#39;utente. | Correggere il contesto dell&#39;applicazione client. |
| autorizzazione | L&#39;applicazione client richiede di ottenere l&#39;autorizzazione per la risorsa specificata. | Correggere il contesto dell&#39;applicazione client. |
| riprova | L&#39;applicazione client richiede di ripetere la richiesta. | Correggi il contesto della richiesta. |

_(*) Per alcuni errori, potrebbero essere possibili più azioni, ma il campo &quot;action&quot; indica quella con la maggiore probabilità di correggere l&#39;errore._

### Stato {#enhanced-error-codes-representation-status}

I codici di errore avanzati includono un campo &quot;status&quot; che indica il codice di stato HTTP associato all’errore.

I valori possibili per il campo &quot;status&quot; includono:

| Codice | Frase-Motivo |
|------|-----------------------|
| 400 | Richiesta non valida |
| 401 | Non autorizzato |
| 403 | Non consentito |
| 404 | Non trovato |
| 405 | Metodo non consentito |
| 410 | Non più |
| 412 | Precondizione non riuscita |
| 500 | Errore interno del server |

Codici di errore migliorati con uno &quot;stato&quot; 4xx vengono in genere visualizzati quando l’errore viene generato dal client e la maggior parte delle volte implica che il client richiede un lavoro aggiuntivo per correggerlo.

Codici di errore migliorati con uno &quot;stato&quot; 5xx vengono in genere visualizzati quando l’errore viene generato dal server e la maggior parte delle volte implica che il server richiede un lavoro aggiuntivo per correggerlo.

>[!IMPORTANT]
>
> In alcuni casi il codice di stato della risposta HTTP è diverso dal campo &quot;stato&quot; del Codice di errore avanzato, in particolare quando si interagisce con un’API di autenticazione di Adobe Pass che comunica i Codici di errore avanzati come informazioni di errore a livello di elemento.

### Codice {#enhanced-error-codes-representation-code}

I codici di errore avanzati includono un campo &quot;code&quot; che fornisce un identificatore univoco di autenticazione Adobe Pass associato all’errore.

I valori possibili per il campo &quot;code&quot; sono aggregati [di seguito](#enhanced-error-codes-list) in due elenchi basati sull&#39;API di autenticazione Adobe Pass integrata.

## Elenchi {#enhanced-error-codes-lists}

### API REST v1 {#enhanced-error-codes-lists-rest-api-v1}

La tabella seguente elenca i possibili codici di errore avanzati che un’applicazione client potrebbe incontrare quando integrata con l’API REST di autenticazione di Adobe Pass v1.

| Azione | Codice | Stato | Messaggio |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nessuno** | *invalid_requestor* | 400 | Parametro del richiedente mancante o non valido. |
|                    | *informazioni_dispositivo_non_valido* | 400 | Informazioni dispositivo mancanti o non valide. |
|                    | *invalid_device_id* | 400 | Identificatore dispositivo mancante o non valido. |
|                    | *risorsa_mancante* | 400, 412 | Parametro di risorsa mancante. |
|                    | *richiesta_authz_non valida* | 400, 412 | La richiesta di autorizzazione è nulla o non valida. |
|                    | *preautorizzazione_negata_da_mvpd* | 403 | L&#39;MVPD ha restituito una decisione di rifiuto quando ha richiesto la pre-autorizzazione per la risorsa specificata. |
|                    | *autorizzazione_negata_da_mvpd* | 403 | L&#39;MVPD ha restituito una decisione &quot;Nega&quot; quando ha richiesto l&#39;autorizzazione per la risorsa specificata. |
|                    | *autorizzazione_negata_da_controlli_genitori* | 403 | L&#39;MVPD ha restituito una decisione &quot;Nega&quot; a causa delle impostazioni di Controllo genitori per la risorsa specificata. |
|                    | *errore_interno* | 400, 405, 500 | Richiesta non riuscita a causa di un errore interno del server. |
| **configurazione** | *integrazione_sconosciuta* | 400, 412 | L&#39;integrazione tra il programmatore specificato e il provider di identità non esiste. Utilizzate il dashboard TVE per creare l&#39;integrazione richiesta. |
|                    | *troppe_risorse* | 403 | Richiesta di autorizzazione o preautorizzazione non riuscita perché sono state eseguite query su troppe risorse. Contatta il team di supporto per configurare correttamente le limitazioni di autorizzazione e preautorizzazione. |
| **autenticazione** | *authentication_session_issuer_mismatch* | 400 | Richiesta di autorizzazione non riuscita a causa del fatto che l’MVPD indicato per il flusso di autorizzazione è diverso da quello che ha emesso la sessione di autenticazione. Per continuare, l’utente deve autenticare di nuovo con il MVPD desiderato. |
|                    | *autorizzazione_negata_da_hba_policies* | 403 | MVPD ha restituito una decisione di rifiuto a causa di criteri di autenticazione basati su home. L&#39;autenticazione corrente è stata ottenuta utilizzando un flusso di autenticazione basato sulla home, ma il dispositivo non è più a casa quando si richiede l&#39;autorizzazione per la risorsa specificata. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|                    | *autorizzazione_negata_da_sessione_invalidata* | 403 | Sessione di autenticazione invalidata dal provider di identità. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|                    | *identità_non_riconosciuta_da_mvpd* | 403 | Richiesta di autorizzazione non riuscita a causa del mancato riconoscimento dell&#39;identità utente da parte di MVPD. |
|                    | *autenticazione_sessione_invalidata* | 403 | Sessione di autenticazione invalidata dal provider di identità. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|                    | *sessione di autenticazione_mancante* | 404, 412 | Impossibile recuperare la sessione di autenticazione associata a questa richiesta. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|                    | *sessione di autenticazione_scaduta* | 404, 412 | La sessione di autenticazione corrente è scaduta. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|                    | *sessione_autenticazione_preautorizzazione_mancante* | 412 | Impossibile recuperare la sessione di autenticazione associata a questa richiesta. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|                    | *sessione_autenticazione_preautorizzazione_scaduta* | 412 | La sessione di autenticazione corrente è scaduta. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
| **autorizzazione** | *autorizzazione_non_trovata* | 403, 404 | Non è stata trovata alcuna autorizzazione per la risorsa specificata. L’utente deve ottenere una nuova autorizzazione per continuare. |
|                    | *autorizzazione_scaduta* | 410 | L&#39;autorizzazione precedente per la risorsa specificata è scaduta. L’utente deve ottenere una nuova autorizzazione per continuare. |
| **riprova** | *errore_ricevuto_di_rete* | 403 | Errore di lettura durante il recupero della risposta dal servizio partner associato. Un nuovo tentativo di richiesta potrebbe risolvere il problema. |
|                    | *connessione di rete_timeout* | 403 | Timeout di connessione con il servizio partner associato. Un nuovo tentativo di richiesta potrebbe risolvere il problema. |
|                    | *tempo_esecuzione_massimo_superato* | 403 | La richiesta non è stata completata entro il tempo massimo consentito. Un nuovo tentativo di richiesta potrebbe risolvere il problema. |

### API di pre-autorizzazione SDK {#enhanced-error-codes-lists-sdks-preauthorize-api}

Consulta la [sezione](#enhanced-error-codes-list-rest-api-v1) precedente per informazioni sui possibili codici di errore avanzati che un&#39;applicazione client potrebbe incontrare quando integrata con l&#39;API di preautorizzazione degli SDK di autenticazione di Adobe Pass.

### API REST v2 {#enhanced-error-codes-lists-rest-api-v2}

La tabella seguente elenca i possibili codici di errore avanzati che un’applicazione client potrebbe incontrare quando integrata con l’API REST di autenticazione di Adobe Pass v2.

| Azione | Codice | Stato | Messaggio |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nessuno** | *invalid_parameter_service_provider* | 400 | Il valore del parametro del provider di servizi è mancante o non valido. |
|                              | *invalid_parameter_mvpd* | 400 | Il valore del parametro mvpd è mancante o non valido. |
|                              | *invalid_parameter_code* | 400 | Valore del parametro di codice mancante o non valido. |
|                              | *invalid_parameter_resources* | 400 | Il valore del parametro delle risorse è mancante o non valido. |
|                              | *invalid_parameter_redirect_url* | 400 | Il valore del parametro URL di reindirizzamento è mancante o non valido. |
|                              | *partner_parametro_non valido* | 400 | Il valore del parametro partner è mancante o non valido. |
|                              | *invalid_parameter_saml_response* | 400 | Il valore del parametro di risposta SAML è mancante o non valido. |
|                              | *informazioni_dispositivo_intestazione_non_valida* | 400 | Valore dell&#39;intestazione delle informazioni sul dispositivo mancante o non valido. |
|                              | *invalid_header_device_identifier* | 400 | Il valore dell&#39;intestazione dell&#39;identificatore del dispositivo è mancante o non valido. |
|                              | *invalid_header_identity_for_temporary_access* | 400 | Identità del valore dell&#39;intestazione di accesso temporaneo mancante o non valida. |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | Il valore dello stato di accesso alle autorizzazioni dall’intestazione dello stato del framework del partner non è presente. |
|                              | *invalid_header_pfs_permission_access_not_determinate* | 400 | Il valore dello stato di accesso alle autorizzazioni dall’intestazione dello stato del framework del partner è indeterminato. |
|                              | *invalid_header_pfs_permission_access_not_allowed* | 400 | Il valore dello stato di accesso alle autorizzazioni dall’intestazione dello stato del framework del partner non è concesso. |
|                              | *intestazione_non_valida_pfs_provider_id_non_determinata* | 400 | Il valore dell&#39;ID provider dall&#39;intestazione dello stato del framework partner non è associato a un mvpd noto. |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | Il valore dell’ID del provider dall’intestazione dello stato del framework del partner non corrisponde al mvpd inviato come parametro. |
|                              | *invalid_integration* | 400 | L&#39;integrazione tra il provider di servizi specificato e mvpd non esiste o è disabilitata. |
|                              | *invalid_authentication_session* | 400 | La sessione di autenticazione associata a questa richiesta è mancante o non valida. |
|                              | *preautorizzazione_negata_da_mvpd* | 403 | L&#39;MVPD ha restituito una decisione di rifiuto quando ha richiesto la pre-autorizzazione per la risorsa specificata. |
|                              | *autorizzazione_negata_da_mvpd* | 403 | L&#39;MVPD ha restituito una decisione &quot;Nega&quot; quando ha richiesto l&#39;autorizzazione per la risorsa specificata. |
|                              | *autorizzazione_negata_da_controlli_genitori* | 403 | L&#39;MVPD ha restituito una decisione &quot;Nega&quot; a causa delle impostazioni di Controllo genitori per la risorsa specificata. |
|                              | *autorizzazione_negata_per_regola_degradazione* | 403 | L&#39;integrazione tra il provider di servizi specificato e mvpd prevede l&#39;applicazione di una regola di degrado che nega l&#39;autorizzazione per le risorse richieste. |
|                              | *errore_server_interno* | 500 | Richiesta non riuscita a causa di un errore interno del server. |
| **configurazione** | *troppe_risorse* | 403 | Richiesta di autorizzazione o preautorizzazione non riuscita perché sono state eseguite query su troppe risorse. Contatta il team di supporto per configurare correttamente le limitazioni di autorizzazione e preautorizzazione. |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | Configurazione del certificato metadati utente mancante o non valida. |
|                              | *invalid_configuration_temporary_access* | 500 | Configurazione di accesso temporaneo non valida. |
|                              | *piattaforma_configurazione_non_valida* | 500 | Configurazione della piattaforma mancante o non valida per l’integrazione. |
|                              | *invalid_configuration_platform_id* | 500 | Configurazione dell’ID piattaforma mancante o non valida. |
|                              | *invalid_configuration_platform_trait* | 500 | Configurazione delle caratteristiche della piattaforma mancante o non valida. |
|                              | *invalid_configuration_platform_category_trait* | 500 | Configurazione della caratteristica della categoria della piattaforma mancante o non valida. |
|                              | *invalid_configuration_platform_services* | 500 | Configurazione dei servizi Platform mancante o non valida per l’integrazione. |
|                              | *piattaforma_mvpd_configuration_invalid* | 500 | Configurazione piattaforma mvpd mancante o non valida per mvpd e piattaforma. |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | Configurazione dello stato di boarding della piattaforma mvpd mancante o non valida per mvpd e piattaforma. |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | Configurazione di scambio del profilo della piattaforma mvpd mancante o non valida per mvpd e piattaforma. |
| **registrazione-applicazione** | *provider_servizio_accesso_non_valido* | 401 | Token di accesso non valido a causa di un provider di servizi non valido. |
|                              | *invalid_access_token_client_application* | 401 | Token di accesso non valido a causa di un&#39;applicazione client non valida. |
| **autenticazione** | *profilo_autenticato_mancante* | 403 | Manca il profilo autenticato associato a questa richiesta. |
|                              | *profilo_autenticato_scaduto* | 403 | Il profilo autenticato associato a questa richiesta è scaduto. |
|                              | *profilo_autenticato_invalidato* | 403 | Il profilo autenticato associato a questa richiesta è stato invalidato. |
|                              | *temporary_access_duration_limit_exceeded* | 403 | È stato superato il limite di durata dell’accesso temporaneo. |
|                              | *temporary_access_resources_limit_exceeded* | 403 | È stato superato il limite delle risorse di accesso temporanee. |
|                              | *autorizzazione_negata_da_hba_policies* | 403 | MVPD ha restituito una decisione di rifiuto a causa di criteri di autenticazione basati su home. L&#39;autenticazione corrente è stata ottenuta tramite un flusso di autenticazione basato sulla Home, ma il dispositivo non è più in-home quando si richiede l&#39;autorizzazione per la risorsa specificata. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|                              | *autorizzazione_negata_da_sessione_invalidata* | 403 | Sessione di autenticazione invalidata dal provider di identità. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|                              | *identità_non_riconosciuta_da_mvpd* | 403 | Richiesta di autorizzazione non riuscita a causa del mancato riconoscimento dell&#39;identità utente da parte di MVPD. |
| **riprova** | *errore_ricevuto_di_rete* | 403 | Errore di lettura durante il recupero della risposta dal servizio partner associato. Un nuovo tentativo di richiesta potrebbe risolvere il problema. |
|                              | *connessione di rete_timeout* | 403 | Timeout di connessione con il servizio partner associato. Un nuovo tentativo di richiesta potrebbe risolvere il problema. |
|                              | *tempo_esecuzione_massimo_superato* | 403 | La richiesta non è stata completata entro il tempo massimo consentito. Un nuovo tentativo di richiesta potrebbe risolvere il problema. |

## Gestione della risposta {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> Sono disponibili codici di errore avanzati che possono essere gestiti automaticamente nel codice dell’applicazione client, ad esempio un nuovo tentativo di richiesta di autorizzazione in caso di timeout della rete o la richiesta di nuova autenticazione da parte dell’utente alla scadenza della sessione, ma altri tipi potrebbero richiedere modifiche alla configurazione o l’interazione con il team di assistenza clienti per l’autenticazione di Adobe Pass.
>
> <br/>
>
> Pertanto, è importante raccogliere e fornire informazioni complete sugli errori durante la creazione di un ticket tramite la nostra [Zendesk](https://adobeprimetime.zendesk.com), per garantire che vengano apportate le modifiche necessarie prima di avviare la nuova applicazione o la nuova funzionalità.

In sintesi, quando gestisci le risposte contenenti codici di errore avanzati, prendi in considerazione quanto segue:

1. **Verifica entrambi i valori di stato**: controlla sempre sia il codice di stato della risposta HTTP che il campo &quot;stato&quot; del codice di errore avanzato. Potrebbero essere diversi ed entrambi forniscono informazioni preziose.

1. **Informazioni di errore di livello principale e di livello elemento**: gestire le informazioni di errore di livello principale e di livello elemento indipendentemente dal modo in cui vengono comunicate, assicurarsi di poter gestire entrambe le forme di trasmissione dei codici di errore avanzati.

1. **Logica tentativi**: per gli errori che richiedono un nuovo tentativo, assicurati che i nuovi tentativi vengano eseguiti con un backoff esponenziale per evitare di sopraffare il server. Inoltre, nel caso di API di autenticazione di Adobe Pass che gestiscono più elementi contemporaneamente (ad esempio, API di preautorizzazione), devi includere nella richiesta ripetuta solo gli elementi contrassegnati con &quot;riprova&quot; e non l’intero elenco.

1. **Modifiche alla configurazione**: per gli errori che richiedono modifiche alla configurazione, verificare che le modifiche necessarie siano state apportate prima di avviare la nuova applicazione o la nuova funzionalità.

1. **Autenticazione e autorizzazione**: per gli errori relativi all&#39;autenticazione e all&#39;autorizzazione, è necessario richiedere all&#39;utente di ripetere l&#39;autenticazione o ottenere una nuova autorizzazione in base alle esigenze.

1. **Feedback utente**: facoltativo, utilizzare i campi &quot;messaggio&quot; e (potenziali) &quot;dettagli&quot; leggibili per informare l&#39;utente del problema. Il messaggio di testo &quot;details&quot; (Dettagli) potrebbe essere trasmesso dagli endpoint di preautorizzazione o autorizzazione MVPD o dal Programmatore quando si applicano regole di degradazione.
