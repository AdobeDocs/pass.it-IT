---
title: Manuale dell’API REST V2 (server-to-server)
description: Manuale dell’API REST V2 (server-to-server)
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1578'
ht-degree: 0%

---

# Manuale dell’API REST V2 (server-to-server) {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione REST API V2 è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

Lo scopo di questo documento del manuale è quello di descrivere nel dettaglio le best practice per l’implementazione dell’autenticazione Adobe Pass utilizzando l’API REST V2 in un’architettura server-to-server. Fornisce requisiti di base, implementazione di flusso dettagliata e considerazioni generali sugli ambienti di produzione e sul funzionamento.

## Componenti {#components}

In una soluzione server-to-server funzionante, sono coinvolti i seguenti componenti:

| Tipo | Componente | Descrizione |
|---------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dispositivo di streaming | App di streaming | L&#39;applicazione Programmer che risiede sul dispositivo di streaming dell&#39;utente e riproduce video autenticati. |
|                           | \[Facoltativo\] Modulo AuthN | Se il dispositivo di streaming dispone di un agente utente (ovvero un browser web), il modulo AuthN è responsabile dell’autenticazione dell’utente sull’IdP MVPD. |
| \[Facoltativo\] Dispositivo AuthN | App autenticazione | Se il dispositivo di streaming non dispone di un agente utente (ad esempio, un browser Web), l&#39;applicazione AuthN è un&#39;applicazione Web Programmatore a cui si accede dal dispositivo di un altro utente mediante un browser Web. |
| Infrastruttura del programmatore | Servizio programmatore | Servizio che collega il dispositivo di streaming al servizio Adobe Pass per ottenere le decisioni di autenticazione e autorizzazione. |
| Infrastruttura Adobe | Servizio Adobe Pass | Servizio che si integra con MVPD IdP e AuthZ Service e fornisce decisioni di autenticazione e autorizzazione. |
| Infrastruttura MVPD | IdP MVPD | Endpoint MVPD che fornisce un servizio di autenticazione basato su credenziali per convalidare l&#39;identità dell&#39;utente. |
|                           | Servizio AuthZ MVPD | Endpoint MVPD che fornisce decisioni di autorizzazione basate su abbonamenti dell&#39;utente, controllo genitori, ecc. |

Ulteriori termini utilizzati nel flusso sono definiti nel [Glossario](/help/authentication/kickstart/glossary.md).

Il diagramma seguente illustra l’intero flusso:

![Manuale dell&#39;API REST V2 (server-to-server)](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

### Passaggi per implementare l’API REST V2 nell’architettura server-to-server {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Per implementare Adobe Pass REST API V2, devi seguire i passaggi riportati di seguito raggruppati in fasi.

## 0. Prerequisiti {#prerequisites}

Nelle implementazioni server-to-server, Streaming App e Programmer Service devono stabilire un protocollo che consenta a Programmer Service di:

* Identifica in modo univoco l’app di streaming sul dispositivo.
* Agisci per conto dell’app di streaming e comunica con il servizio Adobe Pass.
* Raccogli e archivia informazioni sull’app di streaming e sul dispositivo, come indirizzo IP, porta di origine e informazioni sul dispositivo, per passarle ad Adobe Pass.
* Restituisci decisioni e istruzioni all’app di streaming.

Parametri come ID dispositivo, ID client e segreto client (definiti di seguito) possono essere memorizzati su Streaming App o Programmer Service.

## A. Fase di registrazione {#registration-phase}

### Passaggio 1: registrare l&#39;applicazione {#step-1-register-your-application}

Per poter chiamare l’API REST di Adobe Pass V2, l’applicazione necessita di un token di accesso richiesto dal livello di sicurezza API.

Per le implementazioni server-to-server, il servizio Programmatore può registrare per conto di un&#39;istanza dell&#39;applicazione, ma è necessario ottenere le credenziali client (ID client e segreto client) per ogni dispositivo di streaming.

Per ottenere il token di accesso, il servizio programmatore può agire per conto di un&#39;app di streaming e deve seguire i passaggi descritti nella documentazione di [Registrazione client dinamica](../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

## B. Fase di autenticazione {#authentication-phase}

### Passaggio 2: verificare la presenza di profili autenticati esistenti {#step-2-check-for-existing-authenticated-profiles}

Il servizio programmatore verifica per conto dell&#39;app di streaming i profili autenticati esistenti: `/api/v2/{serviceProvider}/profiles` ([Recupera profili autenticati](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)).

* Se non viene trovato alcun profilo e l&#39;applicazione Streaming implementa un flusso TempPass
   * Segui la documentazione su come implementare [Flussi di accesso temporanei](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Se non viene trovato alcun profilo, l&#39;applicazione Streaming implementa un flusso di autenticazione
   * <b>Passaggio 2.a:</b> il servizio Programmatore recupera l&#39;elenco di MVPD disponibili per serviceProvider: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Recupera elenco di MVPD disponibili](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * Il servizio programmatore può implementare il filtraggio nell’elenco degli MVPD e visualizzare solo gli MVPD destinati a nasconderne altri (TempPass, MVPD di prova, MVPD in fase di sviluppo, ecc.)
   * Il servizio programmatore deve restituire un elenco MVPD filtrato per l’app di streaming per visualizzare il selettore; l’utente seleziona l’MVPD
   * Con MVPD selezionato da Streaming App, il servizio Programmatore crea una sessione: <b>/api/v2/{serviceProvider}/session</b><br>
([Crea sessione di autenticazione](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * Vengono restituiti il CODICE e l&#39;URL da utilizzare per l&#39;autenticazione
      * Se viene trovato un profilo, il servizio Programmatore potrebbe passare a <a href="#preauthorization-phase">C. Fase di pre-autorizzazione</a>
   * Il servizio Programmatore deve restituire il CODICE e l&#39;URL all&#39;app di streaming

### Passaggio 3: autenticare l’utente {#step-3-authenticate-the-user}

Utilizzo di un browser o di un&#39;applicazione basata sul Web Second Screen:

* Opzione 1. L’app di streaming può aprire un browser o una visualizzazione web, caricare l’URL da autenticare e l’utente accede alla pagina di accesso di MVPD in cui è necessario inviare le credenziali
   * L’utente immette il login/password, il reindirizzamento finale mostra una pagina di successo
* Opzione 2. L&#39;app di streaming non può aprire un browser e semplicemente visualizzare il CODICE. <b>È necessario sviluppare un&#39;applicazione Web separata, AuthN_APP</b>, per richiedere all&#39;utente di immettere CODE, generare e aprire l&#39;URL: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * L’utente immette il login/password, il reindirizzamento finale mostra una pagina di successo

### Passaggio 4: verificare la presenza di profili autenticati {#step-4-check-for-authenticated-profiles}

Il servizio programmatore verifica l’autenticazione con MVPD da completare nel browser o nella seconda schermata

* Si consiglia di eseguire il polling ogni 15 secondi il <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>
([Recupera profili autenticati per MVPD](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) specifici)
   * Se la selezione MVPD non viene effettuata nell&#39;applicazione Streaming quando il selettore MVPD viene presentato nell&#39;applicazione Second Screen, il polling deve essere eseguito con CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>
([Recupera profili autenticati per codice specifico](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* Il polling non deve superare i 30 minuti, nel caso in cui siano raggiunti 30 minuti e l’applicazione di streaming sia ancora attiva, è necessario avviare una nuova sessione e restituire un nuovo CODICE e un nuovo URL
* Al termine dell’autenticazione, viene restituito 200 con profilo autenticato
* Il servizio Programmatore può passare a <a href="#preauthorization-phase">C. Fase di pre-autorizzazione</a>

## C. Fase di preautorizzazione {#preauthorization-phase}

### Passaggio 5: verificare la presenza di risorse preautorizzate {#step-5-check-for-preauthorized-resources}

Con un profilo di autenticazione valido per un utente, il servizio Programmatore ha la possibilità di controllare l’accesso ai video disponibili e passare l’elenco all’applicazione di streaming per visualizzarlo.

* Il passaggio è facoltativo ed eseguito se l’applicazione desidera filtrare le risorse non disponibili nel pacchetto utente autenticato
* Chiamata a <b>/api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}</b><br>
([Recupera la decisione di pre-autorizzazione utilizzando MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) specifico)

## D. Fase di autorizzazione {#authorization-phase}

### Passaggio 6: verificare la disponibilità di risorse autorizzate {#step-6-check-for-authorized-resources}

L’app in streaming si prepara a riprodurre un video, una risorsa o una risorsa selezionati dall’utente.

* Il passaggio è necessario per ogni inizio della riproduzione
* L&#39;app di streaming trasmette queste informazioni al servizio Programmatore
* Il servizio Programmatore per conto dell&#39;app di streaming, chiama <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
([Recupera decisione di autorizzazione utilizzando MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifico)
   * decision = &#39;Permit&#39;, il servizio programmatore indica all&#39;app di streaming di avviare lo streaming
   * decision = &#39;Nega&#39;, il servizio programmatore indica allo Streaming App di informare l&#39;utente che non ha accesso a quel video
   * durante il processo, il servizio Programmatore può valutare altre regole di business e restituire la decisione appropriata allo Streaming App

## E. Fase di disconnessione {#logout-phase}

### Passaggio 7: disconnessione {#step-7-logout}

App di streaming: l’utente vuole disconnettersi da MVPD

* L&#39;app di streaming informa il servizio programmatore che deve disconnettersi dall&#39;MVPD per questa app specifica.
* Il servizio Programmatore può eliminare le informazioni memorizzate sull&#39;utente autenticato
* Chiamata del servizio programmatore <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Avvia disconnessione per MVPD](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) specifico)
* Se l&#39;URL e il tipo di azione di risposta sono presenti, il servizio Programmatore restituirà all&#39;app di streaming l&#39;URL
* In base alle funzionalità esistenti, l’app di streaming può aprire l’URL nel browser (in genere lo stesso utilizzato per l’autenticazione)
* Se l’app di streaming non dispone di un browser o se si tratta di un’istanza diversa da quella in fase di autenticazione, il flusso può essere interrotto in quanto la sessione MVPD non è stata resa persistente nella cache del browser.

## Ambienti e requisiti funzionali{#environments}

Un programmatore deve creare almeno due ambienti: uno per la produzione e uno o più per la gestione temporanea.

### Produzione

L’ambiente di produzione deve essere altamente disponibile e scalabile in modo appropriato per picchi di grandi dimensioni o imprevisti (ad esempio, sport live, ultime notizie).

Il servizio Adobe Pass viene eseguito su più data center geograficamente distribuiti negli Stati Uniti. Per ottenere il miglior tempo di risposta (ovvero la latenza più bassa) dal servizio Adobe Pass, il programmatore deve anche creare un’infrastruttura di servizio simile, geograficamente distribuita.

Il servizio Programmer deve limitare la cache DNS a un massimo di 30 secondi nel caso in cui Adobe debba reindirizzare il traffico. Ciò può verificarsi se un centro dati non è più disponibile.

Il programmatore deve fornire l’intervallo IP pubblico dell’ambiente di produzione. Questi verranno inseriti in un elenco di IP consentiti nell’infrastruttura Adobe Pass per l’accesso e gestiti dai criteri di utilizzo delle API equi di Adobe.

### Staging

L’ambiente di staging può essere minimo, ma deve includere tutti i componenti di sistema e la logica di business. Dovrebbe funzionare in modo simile alla produzione e consentire di testare le versioni al di fuori della produzione. Idealmente, l’ambiente di staging può essere connesso agli ambienti di test di Adobe Pass per l’utilizzo da parte del programmatore e, se necessario, di Adobe, in modo da poter contribuire ai test e alla risoluzione dei problemi.

### Requisiti funzionali

Il servizio programmatore deve trasmettere informazioni accurate sull’identificazione del dispositivo per il quale sta eseguendo i flussi. Inoltre, il servizio Programmer deve trasmettere l’IP del dispositivo per il quale sta eseguendo i flussi (in un’intestazione x-forwarded-for) insieme alla porta di origine della connessione (nel campo device info (informazioni sul dispositivo)):

Il servizio Programmatore deve inviare i dati e la formattazione richiesti dai singoli MVPD o dalle app integrate (ad esempio, IP dispositivo, porta di origine, informazioni dispositivo, MRSS, dati facoltativi come ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.

Il servizio programmatore deve rispettare il profilo di autenticazione e la validità delle decisioni durante la memorizzazione nella cache e invalidare le autenticazioni o le decisioni una volta ricevute le notifiche.

Il servizio Programmatore deve mantenere i certificati condivisi con Adobe (per i metadati utente crittografati).

## Informazioni correlate {#related}

* [Riferimento REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
