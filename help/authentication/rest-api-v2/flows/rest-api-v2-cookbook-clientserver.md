---
title: REST API V2 - Manuale - Passaggi di implementazione client/server
description: REST API V2 - Manuale - Passaggi di implementazione client/server
source-git-commit: 5ba538bdb13d121ba27005df82d4ae604f912241
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# REST API V2 - Manuale - Passaggi di implementazione client/server {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> L&#39;implementazione REST API V2 è limitata dalla documentazione del [meccanismo di limitazione](/help/authentication/throttling-mechanism.md).

## Passaggi per implementare REST API V2 nelle applicazioni lato client {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

Per implementare Adobe Pass REST API V2, devi seguire i passaggi riportati di seguito raggruppati in fasi.

## A. Fase di registrazione {#registration-phase}

### Passaggio 1: registrare l&#39;applicazione {#step-1-register-your-application}

Per poter chiamare l’API REST di Adobe Pass V2, l’applicazione necessita di un token di accesso richiesto dal livello di sicurezza API.
Per ottenere il token di accesso, l’applicazione deve seguire i passaggi descritti:
[Registrazione Client Dinamico](./dynamic-client-registration.md)

## B. Fase di autenticazione {#authentication-phase}

### Passaggio 2: verificare la presenza di profili autenticati esistenti {#step-2-check-for-existing-authenticated-profiles}

L&#39;applicazione di streaming verifica i profili autenticati esistenti: <b>/api/v2/{serviceProvider}/profiles</b><br>
([Recupera profili autenticati](./apis/profiles-apis/rest-api-v2-retrieve-authenticated-profiles.md))

* se non viene trovato alcun profilo e l&#39;applicazione Streaming implementa un flusso TempPass
   * segui la documentazione su come implementare [Flussi di accesso temporanei](./temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Se non viene trovato alcun profilo, l&#39;applicazione Streaming implementa un flusso di autenticazione
   * L&#39;applicazione di streaming recupera l&#39;elenco di MVPD disponibili per serviceProvider: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Recupera elenco di MVPD disponibili](./apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * L&#39;applicazione di streaming può implementare il filtro nell&#39;elenco degli MVPD e visualizzare solo gli MVPD destinati a nasconderne altri (TempPass, MVPD di prova, MVPD in fase di sviluppo, ecc.)
   * Selettore di visualizzazione delle applicazioni in streaming, l&#39;utente seleziona il MVPD
   * Creazione di una sessione da parte dell&#39;applicazione di streaming: <b>/api/v2/{serviceProvider}/session</b><br>
([Crea sessione di autenticazione](./apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * Vengono restituiti un CODICE e un URL da utilizzare per l’autenticazione
      * Se viene trovato un profilo, l&#39;applicazione di streaming può passare a <a href="#preauthorization-phase">C. Fase di pre-autorizzazione</a> o <a href="#authorization-phase">D. Fase di autorizzazione</a>

### Passaggio 3: autenticare l’utente {#step-3-authenticate-the-user}

Utilizzo di un browser o di un&#39;applicazione basata sul Web Second Screen:

* Opzione 1. L’applicazione di streaming può aprire un browser o una visualizzazione web, caricare l’URL per l’autenticazione e l’utente arriva alla pagina di accesso di MVPD in cui è necessario inviare le credenziali
   * utente immetti login/password, reindirizzamento finale mostra una pagina di successo
* Opzione 2. L&#39;applicazione di streaming non può aprire un browser e semplicemente visualizzare il CODICE. <b>È necessario sviluppare un&#39;applicazione Web separata</b> per richiedere all&#39;utente di immettere CODE, build e aprire l&#39;URL: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * utente immetti login/password, reindirizzamento finale mostra una pagina di successo

### Passaggio 4: verificare la presenza di profili autenticati {#step-4-check-for-authenticated-profiles}

L&#39;applicazione in streaming verifica l&#39;autenticazione con MVPD da completare nel browser o nella seconda schermata

* polling ogni 15 secondi consigliato il <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>
([Recupera profili autenticati per MVPD](.apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) specifici)
   * se la selezione MVPD non viene effettuata nell&#39;applicazione Streaming quando il selettore MVPD viene presentato nell&#39;applicazione Second Screen, il polling deve essere eseguito con CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>
([Recupera profili autenticati per codice specifico](./apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* il polling non deve superare i 30 minuti, nel caso in cui siano raggiunti 30 minuti e l’applicazione di streaming sia ancora attiva, è necessario avviare una nuova sessione e verrà restituito un nuovo CODICE e un nuovo URL
* al termine dell’autenticazione, viene restituito 200 con profilo autenticato
* L&#39;applicazione Streaming può passare a <a href="#preauthorization-phase">C. Fase di pre-autorizzazione</a> o <a href="#authorization-phase">D. Fase di autorizzazione</a>

## C. Fase di preautorizzazione {#preauthorization-phase}

### Passaggio 5: verificare la presenza di risorse preautorizzate {#step-5-check-for-preauthorized-resources}

L’applicazione di streaming si prepara a visualizzare i video disponibili per l’utente autenticato e ha la possibilità di controllare
accedere a queste risorse.

* il passaggio è facoltativo ed eseguito se l’applicazione desidera filtrare le risorse non disponibili nel pacchetto utente autenticato
* chiamata a <b>/api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}</b><br>
([Recupera la decisione di pre-autorizzazione utilizzando MVPD](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) specifico)


## D. Fase di autorizzazione {#authorization-phase}

### Passaggio 6: verificare la disponibilità di risorse autorizzate {#step-6-check-for-authorized-resources}

L’applicazione in streaming si prepara a riprodurre un video, una risorsa o una risorsa selezionati dall’utente.

* il passaggio è necessario per ogni avvio di riproduzione
* chiama <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
([Recupera decisione di autorizzazione utilizzando MVPD](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) specifico)
   * decisione = &#39;Permit&#39; , il dispositivo di streaming avvia lo streaming
   * decisione = &#39;Nega&#39;, il dispositivo di streaming informa l&#39;utente che non ha accesso a quel video

## E. Fase di disconnessione {#logout-phase}

### Passaggio 7: disconnessione {#step-7-logout}

Dispositivo di streaming: l’utente desidera disconnettersi da MVPD

* chiama <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Avvia disconnessione per MVPD](.apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) specifico)
* se è presente actionType=&#39;interactive&#39; e url, apri l&#39;url in un browser/seconda schermata per completare la disconnessione con MVPD

