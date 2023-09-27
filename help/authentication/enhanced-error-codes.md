---
title: Codici di errore migliorati
description: Codici di errore migliorati
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 40aeba47c293f2cc4569f01a6fb1ca4bfbcd0de6
workflow-type: tm+mt
source-wordcount: '2299'
ht-degree: 2%

---

# Codici di errore migliorati {#enhanced-error-codes}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

Questo documento descrive l’elenco dei codici di errore API e le informazioni di errore aggiuntive restituite alle applicazioni.

Per utilizzare Codici di errore avanzati nell&#39;applicazione Programmatori, è necessario richiedere al team di supporto di apportare una modifica alla configurazione.

## Gestione degli errori di risposta {#response-error-handling}

Nella maggior parte degli scenari, l’API di autenticazione di Adobe Pass include nel corpo della risposta informazioni di errore aggiuntive che consentono di fornire **contesto significativo** il motivo per cui si è verificato un determinato errore e/o possibili soluzioni per risolvere automaticamente il problema.  *Tuttavia, in alcuni casi specifici che riguardano i flussi di autenticazione o disconnessione, i servizi di autenticazione di Adobe Pass potrebbero restituire una risposta HTML o un corpo vuoto. Per ulteriori informazioni, consulta la documentazione dell’API.*

Anche se alcuni tipi di errori possono essere gestiti automaticamente (ad esempio, riprovare una richiesta di autorizzazione in caso di timeout della rete o richiedere la nuova autenticazione dell’utente se la sessione è scaduta), altri tipi potrebbero richiedere modifiche alla configurazione o l’interazione con il team di assistenza clienti. È importante che i programmatori raccolgano e forniscano informazioni complete sugli errori in tali casi.

L’API di autenticazione di Adobe Pass restituisce codici di stato HTTP compresi tra 400 e 500 per indicare errori o errori. Ogni codice di stato HTTP ha alcune implicazioni:

- I codici di errore 4xx implicano che l’errore sia generato dal client e che quest’ultimo debba eseguire ulteriori operazioni per correggerlo (ad esempio, ottenere un token di accesso prima di richiamare API protette o fornire qualsiasi parametro richiesto)

- I codici di errore 5xx indicano che l’errore è generato dal server e che quest’ultimo deve eseguire ulteriori operazioni per correggerlo.

Le informazioni aggiuntive sull’errore sono incluse nel campo &quot;error&quot; (Errore) all’interno del corpo della risposta.

<table>
<thead>
  <tr>
    <th>Nome</th>
    <th>Tipo</th>
    <th>Esempio</th>
    <th>Descrizione</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>errore</td>
    <td><i>oggetto</i></td>
    <td><strong>JSON</strong>
    <br>
    <code>{<br>&nbsp;&nbsp;&nbsp;&nbsp;"status" : 403,<br>&nbsp;&nbsp;&nbsp;&nbsp;"code" : "network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"message" : "Unable to contact your TV provider<br>&nbsp;&nbsp;&nbsp;&nbsp;services",<br>&nbsp;&nbsp;&nbsp;&nbsp;"helpUrl" : "https://tve.helpdocsonline.com/errors<br>&nbsp;&nbsp;&nbsp;&nbsp;/network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"trace" : "12f6fef9-d2e0-422b-a9d7-60d799abe353",<br>&nbsp;&nbsp;&nbsp;&nbsp;"action" : "retry"<br>}
    </code>
    <p>
    <p>
    <strong>XML</strong>
    <br>
    <code>&lt;error&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;status&gt;403&lt;/status&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;code&gt;network_connection_failure&lt;/code&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;message&gt;Unable to contact your TV provider services&lt;/message&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;helpUrl&gt;https://tve.helpdocsonline.com/errors/network_connection_failure&lt;/helpUrl&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;trace>12f6fef9-d2e0-422b-a9d7-60d799abe353&lt;/trace&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;action>retry&lt;/action&gt;<br>&lt;/error&gt;
    </code>
    </td>
    <td>Si riferisce a una raccolta o a oggetti di errore raccolti durante il tentativo di completare la richiesta.</td>
  </tr>
</tbody>
</table>

</br>

Le API di Adobe Pass che gestiscono più elementi (API di pre-autorizzazione, ecc.) possono indicare se l’elaborazione non è riuscita per un particolare elemento ed è riuscita per altri elementi utilizzando le informazioni di errore a livello di elemento. In questo caso, il ***&quot;error&quot;*** l&#39;oggetto si trova a livello di elemento e il corpo della risposta potrebbe contenere più ***&quot;errors&quot;*** oggetti: consulta la documentazione dell’API.

</br>

**Esempio con successo parziale ed errore a livello di elemento**

```json
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream2",
            "authorized" : false,
            "error" : {
 
               "status" : 403,
               "code" : "network_connection_failure",
               "message" : "Unable to contact your TV provider services",
               "details" : "",
               "helpUrl" : "https://tve.helpdocsonline.com/errors/network_connection_failure",
               "trace" : "8bcb17f9-b172-47d2-86d9-3eb146eba85e",
               "action" : "retry"
            }
 
        }
    ]
}
```

</br>

Ogni oggetto errore ha i seguenti parametri:

| Nome | Tipo | Esempio | Limitato | Descrizione |
|---|---|----|:---:|---|
| *stato* | *numero intero* | *403* | &amp;verifica; | Il codice di stato HTTP della risposta come documentato nella RFC 7231 (<https://tools.ietf.org/html/rfc7231#section-6>) <ul><li>400 - Richiesta non valida</li><li>401 Non autorizzato</li><li>403 - Non consentito</li><li>404 Non trovato</li><li>405 Metodo non consentito</li><li>Conflitto 409</li><li>410 Non più disponibile</li><li>Precondizione 412 non riuscita</li><li>429 Troppe richieste</li><li>Errore 500 Interval Server</li><li>Servizio 503 non disponibile</li></ul> |
| *codice* | *stringa* | *errore_connessione_di_rete* | &amp;verifica; | Codice di errore standard di autenticazione di Adobe Pass. L’elenco completo dei codici di errore è riportato di seguito. |
| *messaggio* | *stringa* | *Impossibile contattare il provider TV* | | Messaggio leggibile che può essere visualizzato all’utente finale. |
| *dettagli* | *stringa* | *Il pacchetto di abbonamento non include il canale &quot;Live&quot;* | | In alcuni casi un messaggio dettagliato viene fornito dagli endpoint di autorizzazione MVPD o dal programmatore attraverso regole di degradazione. <p> Si noti che se non è stato ricevuto alcun messaggio personalizzato dai servizi partner, questo campo potrebbe non essere presente nei campi di errore. |
| *helpUrl* | *url* | &quot;`http://`&quot; | | Un URL che collega a ulteriori informazioni sul motivo di questo errore e sulle possibili soluzioni. <p>L’URI rappresenta un URL assoluto e non deve essere dedotto dal codice di errore. A seconda del contesto dell’errore, è possibile fornire un URL diverso. Ad esempio, lo stesso codice di errore bad_request produrrà URL diversi per i servizi di autenticazione e autorizzazione. |
| *traccia* | *stringa* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* | | Identificatore univoco di questa risposta che può essere utilizzato quando si contatta il supporto per identificare problemi specifici in scenari più complessi. |
| *azione* | *stringa* | *riprova* | &amp;verifica; | Azione raccomandata per porre rimedio alla situazione: <ul><li> *nessuno* - Sfortunatamente non esiste un’azione predefinita per risolvere questo problema. Ciò potrebbe indicare una chiamata non corretta dell’API pubblica</li><li>*configurazione* - È necessaria una modifica della configurazione tramite il dashboard TVE o contattando il supporto. </li><li>*registrazione dell&#39;applicazione* - La domanda deve essere nuovamente registrata. </li><li>*autenticazione* : l’utente deve autenticare o autenticare di nuovo. </li><li>*autorizzazione* - L’utente deve ottenere l’autorizzazione per la risorsa specifica. </li><li>*degradazione* - Deve essere applicata una qualche forma di degradazione. </li><li>*riprova* - Un nuovo tentativo di richiesta potrebbe risolvere il problema</li><li>*riprova dopo* - Un nuovo tentativo di richiesta dopo il periodo di tempo indicato potrebbe risolvere il problema.</li></ul> |

</br>

**Note:**

- ***Limitato*** colonna *indica se il rispettivo valore di campo rappresenta un set finito* (ad esempio, codici di stato HTTP esistenti per &quot;*stato*&quot;). I futuri aggiornamenti di questa specifica potrebbero aggiungere valori all&#39;elenco limitato, ma non rimuoveranno o modificheranno quelli esistenti. I campi senza restrizioni in genere possono contenere qualsiasi dato, ma potrebbero esserci limitazioni per garantire una dimensione ragionevole.

- Ogni risposta di Adobe conterrà un &quot;Adobe-Request-Id&quot; che identifica la richiesta del client in tutti i servizi HTTP. La &quot;**traccia**&quot;completano questo campo e devono essere segnalati insieme.

## Codici di stato HTTP e codici di errore {#http-status-codes-and-error-codes}

Le incoerenze tra i vari codici di errore e i codici di stato HTTP associati sono dovute ai requisiti di compatibilità con le versioni precedenti di sdk e applicazioni meno recenti (ad esempio *unknown\_application* restituisce 400 richiesta non valida mentre *unknown\_software\_statement* 401 Non autorizzato). La soluzione di tali incoerenze sarà mirata nelle iterazioni future.

## Azioni e codici di errore {#actions-and-error-codes}

Per la maggior parte dei codici di errore, più azioni potrebbero essere idonee come percorsi per la risoluzione del problema in questione oppure potrebbero essere necessarie più azioni per correggerle automaticamente. Abbiamo scelto di indicare quello con la probabilità più elevata di correggere l’errore. Il **azioni** può essere suddiviso in tre categorie:

1. quelli che tentano di correggere il contesto della richiesta (riprova, riprova dopo)
1. quelli che tentano di correggere il contesto utente all’interno dell’applicazione (registrazione dell’applicazione, autenticazione, autorizzazione)
1. che tentano di correggere il contesto di integrazione tra un’applicazione e un provider di identità (configurazione, degrado)

Per la prima categoria (riprova e riprova dopo), potrebbe essere sufficiente ripetere la stessa richiesta per risolvere il problema. Nel caso di API che gestiscono più elementi, l’applicazione deve ripetere la richiesta e includere solo gli elementi con l’azione &quot;riprova&quot; o &quot;riprova dopo&quot;. Per &quot;*riprova dopo*&quot; azione, a &quot;<u>Riprova dopo</u>L’intestazione &quot; indica quanti secondi l’applicazione deve attendere prima di ripetere la richiesta.

Per la seconda e la terza categoria, l’implementazione effettiva dell’azione dipende fortemente dalle caratteristiche dell’applicazione. Ad esempio, &quot;*degradazione*&quot; può essere implementato sia come &quot;passaggio a 15 minuti passaggi temporanei per consentire agli utenti la riproduzione del contenuto&quot; o come &quot;strumento automatico per applicare la degradazione AUTHN-ALL o AUTHZ-ALL per la sua integrazione con il MVPD specificato&quot;. Simile a &quot;*autenticazione*&quot; può attivare un’autenticazione passiva (autenticazione back-channel) su un tablet e un flusso di autenticazione a schermo intero di secondo livello sui televisori connessi. Ecco perché abbiamo deciso di fornire URL completi con schema e tutti i parametri.

## Codici errore {#error-codes}

La tabella di errori riportata di seguito elenca i possibili codici di errore, i messaggi associati e le azioni possibili.

| Azione | Codice di errore | Codice di stato HTTP | Descrizione |
|---|---|---|---|
| **nessuno** | *authorized_rejected_by_mvpd* | 403 | L&#39;MVPD ha restituito una decisione &quot;Nega&quot; quando ha richiesto l&#39;autorizzazione per la risorsa specificata. |
|  | *authorized_rejected_by_parental_controls* | 403 | L&#39;MVPD ha restituito una decisione &quot;Nega&quot; a causa delle impostazioni di Controllo genitori per la risorsa specificata. |
|  | *authorized_rejected_by_programmer* | 403 | La regola di degradazione applicata dal programmatore applica una decisione di &quot;negazione&quot; per l’utente corrente. |
|  | *bad_request* | 400 | La richiesta API non è valida o non è formata correttamente. Consulta la documentazione API per determinare i requisiti della richiesta. |
|  | *individualization_service_unavailable* | 503 | Richiesta non riuscita a causa della non disponibilità del servizio di individuazione. |
|  | *internal_error* | 500 | Richiesta non riuscita a causa di un errore interno del server. |
|  | *invalid_client_time* | 400 | Data/ora/fuso orario del computer client non impostato correttamente. Questo potrebbe causare errori di autenticazione/autorizzazione. |
|  | *invalid_custom_scheme* | 400 | Lo schema personalizzato specificato utilizzato nella registrazione dell&#39;applicazione non è riconosciuto. Controlla la configurazione del dashboard TVE per verificare i valori dello schema personalizzato. |
|  | *invalid_domain* | 400 | Il richiedente utilizza un dominio non valido. Tutti i domini utilizzati da un particolare ID richiedente devono essere inseriti nella whitelist per Adobe. |
|  | *invalid_header* | 400 | Richiesta non riuscita perché conteneva un’intestazione non valida. Consulta la documentazione API per determinare quali intestazioni sono valide per la richiesta e se esistono limitazioni per il loro valore. |
|  | *invalid_http_method* | 405 | Il metodo HTTP associato alla richiesta non è supportato. Consulta la documentazione API per determinare i metodi HTTP supportati per la richiesta. |
|  | *invalid_parameter_value* | 400 | Richiesta non riuscita perché conteneva un valore di parametro o parametro non valido. Consulta la documentazione API per determinare quali parametri sono validi per la richiesta e se esistono limitazioni per il loro valore. |
|  | *invalid_resource_value* | 400 | Richiesta non riuscita. È stata utilizzata una risorsa non valida o non valida. Consulta la documentazione API per determinare quanto devono essere codificate le risorse complesse per la richiesta e se esistono limitazioni per il loro valore e/o le loro dimensioni. |
|  | *invalid_registration_code* | 404 | Il codice di registrazione specificato non è più valido o è scaduto. |
|  | *invalid_service_configuration* | 500 | Richiesta non riuscita a causa di una configurazione del servizio errata. |
|  | *missing_authentication_header* | 400 | Richiesta non riuscita perché non contiene l’intestazione di autenticazione richiesta per l’API specifica. |
|  | *missing_resource_mapping* | 400 | Non esiste alcuna mappatura corrispondente per la risorsa specificata. Contatta il supporto per correggere la mappatura richiesta. |
|  | *preauthorization_rejected_by_mvpd* | 403 | L&#39;MVPD ha restituito una decisione di rifiuto quando ha richiesto la pre-autorizzazione per la risorsa specificata. |
|  | *preauthorization_rejected_by_programmer* | 403 | Le regole di degradazione applicate dal programmatore impongono una decisione di &quot;negazione&quot; per l’utente corrente. |
|  | *registration_code_service_unavailable* | 503 | Richiesta non riuscita perché il servizio del codice di registrazione non è disponibile. |
|  | *service_unavailable* | 503 | La richiesta non è riuscita perché il servizio di autenticazione o autorizzazione non è disponibile. |
|  | *access_token_unavailable* | 400 | Richiesta non riuscita a causa di un errore imprevisto durante il recupero del token di accesso. Controlla la configurazione del dashboard TVE per le istruzioni software disponibili e gli schemi personalizzati registrati. |
|  | *unsupported_client_version* | 400 | Questa versione di Adobe Pass Authentication SDK è troppo vecchia e non è più supportata. Consulta la documentazione dell’API per conoscere i passaggi necessari per effettuare l’aggiornamento alla versione più recente. |
| **configurazione** | *network_required_ssl* | 403 | Problema di connessione SSL per il servizio partner di destinazione. Contatta il team di supporto. |
|  | *troppe_risorse* | 403 | Richiesta di autorizzazione o preautorizzazione non riuscita perché sono state eseguite query su troppe risorse. Contatta il team di supporto per configurare correttamente le limitazioni di autorizzazione e preautorizzazione. |
|  | *unknown_programmer* | 400 | Il programmatore o il provider di servizi non è riconosciuto. Utilizzate la dashboard TVE per registrare il programmatore specificato. |
|  | *unknown_application* | 400 | Applicazione non riconosciuta. Utilizzare il dashboard TVE per registrare l&#39;applicazione specificata. |
|  | *unknown_integration* | 400 | L&#39;integrazione tra il programmatore specificato e il provider di identità non esiste. Utilizzate il dashboard TVE per creare l&#39;integrazione richiesta. |
|  | *unknown_software_statement* | 401 | L&#39;istruzione software associata al token di accesso non è riconosciuta. Contattare il team di supporto per chiarire lo stato del rendiconto software. |
| **registrazione dell&#39;applicazione** | *access_token_scaduto* | 401 | Il token di accesso è scaduto. L’applicazione deve aggiornare il token di accesso come indicato nella documentazione API. |
|  | *invalid_access_token_signature* | 401 | La firma del token di accesso non è più valida. L’applicazione deve aggiornare il token di accesso come indicato nella documentazione API. |
|  | *invalid_client_id* | 401 | Identificatore client associato non riconosciuto. La domanda deve seguire il processo di registrazione indicato nella documentazione dell’API. |
| **autenticazione** | *authentication_session_expiry* | 410 | La sessione di autenticazione corrente è scaduta. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|  | *authentication_session_missing* | 401 | Impossibile recuperare la sessione di autenticazione associata a questa richiesta. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|  | *authentication_session_invalidated* | 401 | Sessione di autenticazione invalidata dal provider di identità. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|  | *authentication_session_issuer_mismatch* | 400 | Richiesta di autorizzazione non riuscita a causa del fatto che l’MVPD indicato per il flusso di autorizzazione è diverso da quello che ha emesso la sessione di autenticazione. Per continuare, l’utente deve autenticare di nuovo con il MVPD desiderato. |
|  | *authorized_rejected_by_hba_policies* | 403 | MVPD ha restituito una decisione di rifiuto a causa di criteri di autenticazione basati su home. L&#39;autenticazione corrente è stata ottenuta utilizzando un flusso di autenticazione basato sulla home, ma il dispositivo non è più a casa quando si richiede l&#39;autorizzazione per la risorsa specificata. Per continuare, l’utente deve ripetere l’autenticazione con un MVPD supportato. |
|  | *identity_not_recognition_by_mvpd* | 403 | Richiesta di autorizzazione non riuscita a causa del mancato riconoscimento dell&#39;identità utente da parte di MVPD. |
| **autorizzazione** | *autorizzazione_scaduta* | 410 | L&#39;autorizzazione precedente per la risorsa specificata è scaduta. L’utente deve ottenere una nuova autorizzazione per continuare. |
|  | *authorized_not_found* | 404 | Non è stata trovata alcuna autorizzazione per la risorsa specificata. L’utente deve ottenere una nuova autorizzazione per continuare. |
|  | *device_identifier_mismatch* | 403 | L&#39;identificatore di dispositivo specificato non corrisponde all&#39;identificazione del dispositivo di autorizzazione. L’utente deve ottenere una nuova autorizzazione per continuare. |
| **riprova** | *errore_connessione_di_rete* | 403 | Errore di connessione con il servizio partner associato. Un nuovo tentativo di richiesta potrebbe risolvere il problema. |
|  | *network_connection_timeout* | 403 | Timeout di connessione con il servizio partner associato. Un nuovo tentativo di richiesta potrebbe risolvere il problema. |
|  | *network_received_error* | 403 | Errore di lettura durante il recupero della risposta dal servizio partner associato. Un nuovo tentativo di richiesta potrebbe risolvere il problema. |
|  | *maximum_execution_time_exceeded* | 403 | La richiesta non è stata completata entro il tempo massimo consentito. Un nuovo tentativo di richiesta potrebbe risolvere il problema. |
| **riprova dopo** | *troppe_richieste* | 429 | Troppe richieste inviate entro un determinato intervallo. L’applicazione può ritentare la richiesta dopo il periodo di tempo suggerito. |
|  | *user_rate_limit_exceeded* | 429 | Un utente specifico ha emesso troppe richieste in un determinato intervallo. L’applicazione può ritentare la richiesta dopo il periodo di tempo suggerito. |
