---
title: Informazioni sull’autenticazione di Adobe Pass
description: Informazioni sull’autenticazione di Adobe Pass
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---

# Informazioni sull’autenticazione pass di Adobe® {#about-adobe-pass-authentication}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Informazioni sulla TV Ovunque {#about-tv-everywhere}

I telespettatori di oggi si aspettano un accesso senza soluzione di continuità ai contenuti della Pay TV in qualsiasi momento e ovunque. Con l’aumento dei dispositivi connessi a Internet, il pubblico sta consumando contenuti attraverso una gamma sempre più ampia di piattaforme, tra cui:

* Notebook
* Compresse
* Smartphone
* Siti Web
* App federate
* Console di gioco
* Set-top box
* Smart TV

TV Everywhere è un&#39;iniziativa del settore che garantisce agli abbonati a PayTV di poter accedere ai contenuti già pagati su più dispositivi, sia all&#39;interno che all&#39;esterno delle proprie abitazioni.

Mentre la TV tradizionale (lineare) rimane forte, i segmenti in più rapida crescita del consumo video sono i contenuti spostati nel tempo, lo streaming online e gli schermi alternativi. Questo cambiamento ha sconvolto il mercato della distribuzione video, rendendo TV Everywhere una soluzione cruciale che allinea gli interessi di **programmatori, fornitori di TV a pagamento e abbonati.**

### Goals of TV Everywhere {#goals-tv-everywhere}

**Obiettivo tecnico**

* Consentire ai clienti di PayTV di accedere ai contenuti a cui sono abbonati in modo semplice su tutti i dispositivi e le piattaforme.

**Obiettivi aziendali**

* Mantenere e rafforzare le relazioni con i clienti esistenti e offrire nuove opportunità.
* I programmatori e i proprietari dei contenuti possono raggiungere un pubblico più ampio e massimizzare il valore dei contenuti premium.
* Estendi i brand attraverso il coinvolgimento diretto online con i visualizzatori.

### La sfida della TV ovunque {#challenges-tv-everywhere}

Con le opportunità di TV Everywhere, il diritto al premio è la più importante. Prima che un visualizzatore possa accedere al contenuto dell’abbonamento, un sistema deve verificarne l’adesione.

Le domande chiave includono:

* L&#39;utente dispone di un abbonamento attivo con un provider di Pay TV?
* La sottoscrizione include il contenuto richiesto?

Determinare i diritti è particolarmente difficile per i programmatori e i proprietari dei contenuti, perché gli operatori di televisione a pagamento controllano i dati dei clienti e i privilegi di accesso.

Oltre all’adesione, sorgono diverse sfide tecniche e di integrazione, tra cui:

* Sviluppare una strategia multi-dispositivo che garantisca un accesso senza soluzione di continuità.
* Gestione di relazioni complesse tra programmatori e fornitori di servizi di televisione a pagamento.
* Prevenire l&#39;accesso fraudolento e far rispettare i termini di servizio.
* Offrire un’esperienza di autenticazione coerente e intuitiva per tutti i siti web e le app.
* Mantenere un rapido time-to-market per allinearsi agli accordi di affiliazione.
* Controllo dei costi associati a più integrazioni.

Queste problematiche rendono le integrazioni dirette tra i programmatori e i sistemi di autenticazione di più Pay TV estremamente dispendiose in termini di risorse e richiedono sia tempo che competenze tecniche.

Per superare questi ostacoli, **Adobe® Pass Authentication** semplifica e semplifica la verifica delle adesioni, consentendo un accesso semplice e sicuro ai contenuti TV Everywhere.

## Introduzione all’autenticazione di Adobe Pass {#introduction-adobe-pass-authentication}

L’autenticazione Adobe Pass consente di mediare in modo sicuro le transazioni di adesione tra i programmatori e i provider di servizi di televisione a pagamento, garantendo ai clienti giusti di poter accedere facilmente ai contenuti corretti.

![](../assets/programmers-connect-authn.png)

*Alcuni programmatori e provider di servizi di televisione a pagamento che si connettono tramite l&#39;autenticazione Adobe Pass*

**Utenti che beneficiano dell&#39;autenticazione Adobe Pass**

* **Programmatori**

  che desiderano integrarsi facilmente con i provider di servizi di televisione a pagamento, noti anche come MVPD (Multichannel Video Programming Distributors), per raggiungere il pubblico più ampio e ottimizzare i ricavi.

* **Provider di servizi di televisione a pagamento (MVPD)**

  Che cercano di entrare in contatto con più proprietari di contenuti (programmatori) tramite un’unica integrazione, migliorando la soddisfazione dei clienti facilitando l’accesso ai contenuti in abbonamento online.

* **Clienti PayTV**

  che desiderano guardare i contenuti per i quali pagano già in qualsiasi momento, ovunque e su qualsiasi dispositivo.

**Programmatori**

I programmatori che desiderano massimizzare la portata del pubblico e i ricavi possono:

* Integrazione semplice con i principali provider di Pay TV senza dover gestire più connessioni dirette.
* Espandi il pubblico garantendo l’autenticazione in tutti i principali provider e piattaforme.
* Protezione dei contenuti premium con autenticazione sicura, limitando l&#39;accesso a utenti e dispositivi autorizzati.
* Abilita il Single Sign-On (SSO) per migliorare l’esperienza utente in app e siti web.

**Provider di servizi di televisione a pagamento (MVPD)**

I provider di servizi di televisione a pagamento possono migliorare la customer experience e semplificare le operazioni:

* Connessione con più proprietari di contenuti tramite un&#39;unica integrazione.
* Offre un’esperienza di visualizzazione senza soluzione di continuità e con marchio su dispositivi e piattaforme diversi.
* Garantire un&#39;autenticazione sicura per impedire l&#39;accesso non autorizzato e gestire flussi simultanei per famiglia.

**Clienti PayTV**

Gli abbonati beneficiano di TV Everywhere, che permette loro di guardare i contenuti che già pagano in qualsiasi momento, ovunque, su qualsiasi dispositivo.

### Integrazione con l’autenticazione di Adobe Pass {#integrating-adobe-pass-authentication}

Che tu sia un provider di Pay TV o un programmatore, l’integrazione con Adobe Pass Authentication richiede la partecipazione attiva. Di seguito è riportata una panoramica generale del processo per entrambi i ruoli.

#### Processo di integrazione dei programmatori {#programmer-integration-process}

Una volta avviata formalmente l’integrazione, sono disponibili ulteriori indicazioni, ma in genere il processo prevede i seguenti passaggi:

**Prerequisiti**

* Un sistema di gestione dei contenuti (CMS).
* Un meccanismo di distribuzione dei contenuti, che può includere una rete CDN (Content Delivery Network) di terze parti.
* Piattaforma video online esistente con un lettore multimediale incorporato in un sito web o in un’applicazione autonoma.

**Attività di integrazione**

* Integrare l&#39;autenticazione Adobe Pass [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).
* Integrare l&#39;autenticazione Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md).
* Integra il [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) di autenticazione Adobe Pass.
* Sviluppa un’interfaccia utente per il flusso di lavoro di autenticazione, autorizzazione e disconnessione.

Per ulteriori dettagli sul processo di integrazione del programmatore, consultare i documenti [Guida rapida per programmatori](/help/authentication/kickstart/programmer-kickstart-guide.md) e [Guida all&#39;integrazione dei programmatori](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md).

#### Processo di integrazione del provider di televisione a pagamento {#pay-tv-provider-integration-process}

Una volta avviata formalmente l’integrazione, sono disponibili ulteriori indicazioni, ma in genere il processo prevede i seguenti passaggi:

**Prerequisiti**

* Firma il Contratto di non divulgazione dell’autenticazione di Adobe Pass (NDA).
* Fornisci al team di progettazione dell’autenticazione di Adobe Pass le specifiche per il sistema di autenticazione e autorizzazione. Per un’integrazione più semplice, si consigliano un provider di identità basato su SAML (IdP) per l’autenticazione e un sistema di autorizzazione basato su SOAP.

**Attività di integrazione**

* Stabilisci la connettività con i server di autenticazione Adobe Pass.
* Completa la versione di staging e assicurati il controllo qualità.
* Rilascio completo della produzione e garanzia di qualità.

L’integrazione standard è gratuita per i provider di servizi di televisione a pagamento, inclusa la documentazione e il supporto di base per le e-mail. Tuttavia, i fornitori che necessitano di assistenza estesa o di tempistiche accelerate possono incorrere in una commissione di supporto o scegliere di lavorare con un partner di terze parti esperto, come Synacor.

L’autenticazione di Adobe Pass può supportare in modo efficiente la logica di business specifica del provider di servizi Pay TV in due modi chiave:

* Per la logica di business indipendente e applicata dal provider di Pay TV quando viene ricevuta una richiesta di autorizzazione, Adobe fornisce i dati necessari (ad esempio, ID dispositivo univoco, indirizzo IP) per supportare l’applicazione.
* Per la logica di business che richiede l’intervento dell’utente o una gestione specifica da parte di Adobe, è possibile mantenere proprietà personalizzate per ogni provider di servizi di televisione a pagamento. Queste configurazioni possono includere flussi di lavoro predefiniti attivati in punti specifici del processo di autenticazione.

Per ulteriori dettagli sul processo di integrazione del provider di servizi Pay TV, consulta i documenti [MVPD kickstart guide](/help/authentication/kickstart/mvpd-kickstart-guide.md) e [MVPD integration guide](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md).

### Flusso diritto {#entitlement-flow}

L’autenticazione Adobe Pass funge da proxy e facilita il flusso dei diritti tra programmatori e MVPD offrendo interfacce sicure e coerenti per entrambe le parti.

Per i programmatori, l&#39;autenticazione Adobe Pass fornisce API come parte di un livello **Standard** o **Premium**:

* API di autenticazione standard di Adobe Pass:
   * [DCR REST API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* API di autenticazione Premium Adobe Pass:
   * [Ripristina API passaggio temporaneo](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [Funzione TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [API di degradazione](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [Funzione di degradazione](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [API di monitoraggio del servizio di adesione](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

Per ulteriori dettagli sul flusso di adesione, consulta la [Documentazione di Programmer Integration Guide](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md#entitlement-flow).

#### Informazioni sulle adesioni {#understanding-entitlements}

La soluzione di autenticazione Adobe Pass si basa sulla creazione di diritti, ossia parti specifiche di dati generati al completamento dei flussi di lavoro di autenticazione e autorizzazione. Questi diritti consentono l’accesso a contenuti protetti, ma hanno una durata limitata. Una volta scaduto, il diritto deve essere rinnovato avviando nuovamente i processi di autenticazione o autorizzazione.

Per ulteriori informazioni sulle adesioni, consulta i seguenti documenti:

* **Profili**

  Dopo l’autenticazione corretta, Adobe Pass Authentication crea un profilo autenticato (&quot;di lunga durata&quot;) associato all’identificatore dell’applicazione, del dispositivo e del provider di servizi richiedente (identificatore del richiedente).

* **[Metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Dopo l’autenticazione corretta (e in alcuni casi anche dopo l’autorizzazione), Adobe Pass Authentication riceve metadati utente da MVPD che possono esporli all’applicazione richiedente.

* **[Decisioni](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  In caso di esito positivo dell’autorizzazione, Adobe Pass Authentication crea una decisione di autorizzazione (&quot;di lunga durata&quot;) associata all’applicazione, al dispositivo, all’identificatore del provider di servizi (identificatore del richiedente) e a una risorsa protetta specifica (identificatore della risorsa).

* **[Token multimediali](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Dopo aver ottenuto l’autorizzazione, Adobe Pass Authentication crea un token multimediale (&quot;di breve durata&quot;) associato a una richiesta di riproduzione corretta e fornisce supporto per le best practice del settore per mitigare le frodi (ad esempio, la copia in streaming).

I valori TTL (time-to-live) per profili e decisioni sono impostati in base ad accordi tra programmatori e fornitori di servizi di televisione a pagamento, che concordano su un valore che meglio si adatta a tutti gli utenti coinvolti.

#### Creazione dell&#39;interfaccia utente {#building-user-interface}

I programmatori sono responsabili della progettazione e dell’implementazione dell’interfaccia utente (UI) per il flusso di lavoro di adesione all’interno del sito web o dell’app, mentre alcuni elementi, come il processo di accesso, sono gestiti dal provider di Pay TV.

I programmatori devono almeno:

* **Implementare un&#39;interfaccia di selezione del provider**
   * Consenti ai nuovi utenti di identificare il provider di Pay TV e di accedere per la prima volta.
   * Alcuni provider di Pay TV reindirizzano gli utenti a una pagina di accesso esterna, mentre altri richiedono l’accesso all’interno di un iframe. I programmatori devono implementare una funzione di callback per generare l’iframe quando necessario.

* **Gestione di un elenco di provider di Pay TV supportati**
   * Assicurati che gli utenti possano accedere ai contenuti solo tramite provider approvati.

* **Indicare lo stato di autenticazione**
   * Mostra quando un utente è autenticato all’interno dell’app o del sito web.

* **Identificare le risorse protette**
   * Indica chiaramente quale contenuto richiede l’autorizzazione prima della visualizzazione.
   * Aggiorna l’interfaccia utente in modo che rifletta l’autorizzazione di accesso concessa.

## Domande frequenti {#faqs}

**Che cos&#39;è TV Everywhere?**

TV Everywhere è un&#39;iniziativa del settore che consente ai clienti di PayTV di accedere ai contenuti premium a cui sono già abbonati su vari dispositivi connessi a Internet. Ciò include personal computer, tablet, smartphone, console giochi, set-top box e Smart TV. La sfida principale consiste nel garantire un processo di autenticazione semplice e intuitivo, che consenta ai clienti di accedere ai contenuti dell’abbonamento senza più accessi o barriere tecniche.

**Che cos&#39;è l&#39;autenticazione di Adobe Pass e come supporta TV Everywhere?**

L’autenticazione di Adobe Pass dà vita a TV Everywhere verificando in modo sicuro il diritto di un utente ai contenuti in modo semplice ed efficiente. Si tratta di un servizio in hosting che facilita la rapida integrazione back-end basata sulle regole aziendali impostate sia dai programmatori che dai provider di servizi di televisione a pagamento. Ciò consente di accelerare il time-to-market per tutte le parti interessate, un ambiente più sicuro che riduca al minimo le frodi e una migliore esperienza utente, con un maggior numero di contenuti TV accessibili su più piattaforme.

**Come viene consegnata l&#39;autenticazione Adobe Pass?**

L’autenticazione Adobe Pass viene fornita come soluzione SaaS (Software as a Service). Questo approccio garantisce una comunicazione sicura tra utenti finali, programmatori e fornitori di servizi di televisione a pagamento per la convalida del diritto ai contenuti.

**Cosa rende l&#39;autenticazione Adobe Pass diversa dalle altre soluzioni TV Everywhere?**

L’autenticazione Adobe Pass offre diversi vantaggi rispetto alle soluzioni alternative:

* **Single Sign-On (SSO) senza soluzione di continuità** - A differenza delle integrazioni dirette con i singoli provider, l&#39;autenticazione di Adobe Pass consente un&#39;esperienza di accesso permanente quando gli utenti si spostano tra siti Web e app diversi.
* **Ampia penetrazione del mercato** - Una volta che un programmatore si integra con l&#39;autenticazione di Adobe Pass, ottiene immediatamente l&#39;accesso agli operatori di Pay TV che coprono oltre il 90% delle famiglie degli Stati Uniti.
* **Integrazione con l&#39;ecosistema di Adobe**: funziona perfettamente con altre soluzioni Adobe per la distribuzione, la protezione e la monetizzazione dei contenuti, incluso Adobe Analytics.

**Quanto è sicura l&#39;autenticazione di Adobe Pass?**

La sicurezza è una priorità assoluta. L’autenticazione di Adobe Pass garantisce che solo gli utenti autorizzati possano accedere ai contenuti premium associando l’accesso al dispositivo dell’utente. Fornisce inoltre opzioni per limitare il numero di flussi, sessioni o dispositivi simultanei per famiglia.

**Quali dispositivi sono supportati dall&#39;autenticazione di Adobe Pass?**

L’autenticazione Adobe Pass è progettata per funzionare su un’ampia gamma di dispositivi, ad esempio dispositivi basati su web, dispositivi mobili o dispositivi connessi alla TV.

**L&#39;autenticazione Adobe Pass costa qualcosa agli utenti finali?**

No. L’autenticazione Adobe Pass è gratuita per gli utenti finali.
