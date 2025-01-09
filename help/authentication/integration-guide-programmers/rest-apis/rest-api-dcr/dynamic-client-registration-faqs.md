---
title: Domande frequenti su Dynamic Client Registration (DCR)
description: Domande frequenti su Dynamic Client Registration (DCR)
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Domande frequenti su Dynamic Client Registration (DCR) {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questo documento fornisce risposte generali elevate alle domande frequenti sull’adozione di DCR (Dynamic Client Registration) di Adobe Pass.

Per ulteriori informazioni generali su Dynamic Client Registration (DCR), vedere la [Documentazione di Dynamic Client Registration Overview](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

>[!MORELIKETHIS]
>
> * [Domande frequenti su REST API v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)

## Domande frequenti generali {#general-faqs}

Inizia con questa sezione se stai lavorando su un’applicazione che deve integrare Dynamic Client Registration (DCR), sia che si tratti di una nuova applicazione o di un’applicazione esistente che esegue la migrazione da uno dei meccanismi precedenti.

### Domande frequenti sulla fase di registrazione {#registration-phase-faqs-general}

+++Domande frequenti sulla fase di registrazione

#### 1. Qual è lo scopo della fase di registrazione? {#registration-phase-faq1}

Lo scopo della fase di registrazione è registrare l&#39;applicazione client rispetto all&#39;autenticazione Adobe Pass tramite il processo [Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr).

Il processo Dynamic Client Registration (DCR) richiede che l&#39;applicazione client ottenga una coppia di credenziali client e recuperi un token di accesso come obiettivo finale della fase di registrazione.

Per ulteriori informazioni, consulta la documentazione [Panoramica registrazione client dinamico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 2. La fase di registrazione è obbligatoria? {#registration-phase-faq2}

La fase di registrazione è obbligatoria, ma l’applicazione client può saltare questa fase se dispone di una coppia memorizzata nella cache di credenziali client e di un token di accesso ancora valido.

#### 3. Che cos&#39;è un&#39;istruzione software e per quanto tempo è valida? {#registration-phase-faq3}

L&#39;istruzione software è un termine definito nella [documentazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement).

L&#39;istruzione software è costituita da un JSON Web Token (JWT) che può essere generato e scaricato dal dashboard di Adobe Pass [TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

L’istruzione software è valida per un periodo di tempo illimitato, ma puoi scegliere di chiedere a un rappresentante dell’autenticazione di Adobe Pass di revocarla in qualsiasi momento.

L&#39;applicazione client deve memorizzare l&#39;istruzione software e utilizzarla quando è necessario recuperare le credenziali client.

Per ulteriori dettagli, consulta la documentazione [Panoramica registrazione client dinamico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 4. Come generare e scaricare un rendiconto software? {#registration-phase-faq4}

Questa operazione può essere completata tramite Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da uno degli amministratori dell&#39;organizzazione o da un rappresentante di Adobe Pass Authentication che agisce per tuo conto.

Per ulteriori dettagli, consultare la [Guida utente canali dashboard TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) o la [Guida utente programmatori dashboard TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

#### 5. Cosa succede se viene revocata un&#39;istruzione software? {#registration-phase-faq5}

Quando l&#39;istruzione software viene revocata, è importante considerare una delle seguenti conseguenze:

* Le applicazioni client che utilizzano l&#39;istruzione software revocata non saranno più in grado di eseguire i flussi [adesione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement), il che significa che gli utenti non potranno più riprodurre il contenuto.

#### 6. Cosa sono le credenziali del client e per quanto tempo sono valide? {#registration-phase-faq6}

Le credenziali client sono un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials).

Le credenziali client sono costituite da un identificatore client e da una coppia di segreto client che è possibile recuperare dall&#39;endpoint Registro client.

Le credenziali client sono valide per un periodo di tempo illimitato.

L’applicazione client deve memorizzare a tempo indeterminato le credenziali client e utilizzarle quando è necessario recuperare un token di accesso.

Per ulteriori informazioni, consulta la documentazione di [Recupero credenziali client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

#### 7. Come gestire le credenziali del client? {#registration-phase-faq7}

È consigliabile che l’applicazione client gestisca una coppia univoca di credenziali client per ogni istanza dell’applicazione utente in caso di integrazioni client-server e server-to-server con l’autenticazione di Adobe Pass.

L’applicazione client deve memorizzare a tempo indeterminato le credenziali client e utilizzarle quando è necessario recuperare un token di accesso.

#### 8. Cosa succede se le credenziali del client memorizzate nella cache vengono perse? {#registration-phase-faq8}

Quando le credenziali del client memorizzate nella cache vengono perse, è necessario considerare tre conseguenze importanti:

* L&#39;applicazione client deve ottenere una nuova coppia di credenziali client.
* L&#39;applicazione client deve ottenere un nuovo token di accesso utilizzando la nuova coppia di credenziali client.
* L’applicazione client dovrà chiedere all’utente di autenticare nuovamente, in quanto perderà l’accesso ai profili autenticati ottenuti in precedenza.

#### 9. Cos’è un token di accesso e per quanto tempo è valido? {#registration-phase-faq9}

Il token di accesso è un termine definito nella documentazione di [Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token).

Il token di accesso è costituito da un [token bearer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) che può essere recuperato dall&#39;endpoint del token client.

Il token di accesso è valido per un periodo di tempo limitato e breve specificato al momento del rilascio.

L’applicazione client deve memorizzare il token di accesso e utilizzarlo fino alla scadenza quando viene eseguita la destinazione dell’API REST V2.

Per evitare richieste non autorizzate, l&#39;applicazione client deve ottenere un nuovo token di accesso prima della scadenza di quella corrente.

Per ulteriori informazioni, consulta la documentazione [Recuperare il token di accesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

#### 10. Come può l’applicazione client aggiornare un token di accesso? {#registration-phase-faq10}

L’applicazione client deve aggiornare un token di accesso nello stesso modo in cui recupera un nuovo token di accesso, ma utilizzando le credenziali client memorizzate nella cache.

L’applicazione client non deve registrarsi nuovamente per aggiornare un token di accesso, ma deve utilizzare le credenziali client memorizzate; in caso contrario, gli utenti dovranno ripetere l’autenticazione.

Per ulteriori informazioni, consulta la documentazione [Recuperare il token di accesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
