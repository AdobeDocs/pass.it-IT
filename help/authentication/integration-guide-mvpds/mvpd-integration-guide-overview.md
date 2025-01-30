---
title: Guida all’integrazione di MVPD
description: Guida all’integrazione di MVPD
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 2b9a8ce374f7a73cd815e9735d672e5c9ba285cc
workflow-type: tm+mt
source-wordcount: '1265'
ht-degree: 0%

---

# Guida all’integrazione di MVPD {#mvpd-integration-guide}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questa guida all’integrazione è destinata ai distributori di programmi video multicanale (MVPD) che intendono effettuare l’integrazione con Adobe® Pass Authentication.

TV Everywhere (TVE) è un&#39;iniziativa trasformativa nel settore della Pay TV, che consente agli abbonati di accedere ai contenuti che già pagano su più dispositivi, sia a casa che in viaggio. Per i fornitori di servizi di televisione a pagamento, TVE offre opportunità significative tra cui il rafforzamento delle relazioni con i clienti esistenti e l’apertura di porte a nuove. Tuttavia, queste opportunità presentano delle sfide.

Nell&#39;ecosistema TVE, **I programmatori** forniscono il contenuto, mentre **MVPDs** (Multichannel Video Programming Distributors) gestiscono i dati dei clienti necessari per verificare se i visualizzatori sono abbonati idonei. Anche se coordinare l&#39;autenticazione e l&#39;autorizzazione con un singolo programmatore può essere gestibile, farlo con decine o anche centinaia di programmatori introduce una notevole complessità.

**Adobe ® Pass Authentication** semplifica il processo. Gli MVPD devono solo implementare un’unica integrazione semplificata con Adobe Pass per accedere all’intero ecosistema TVE. Il framework di integrazione fornito accelera il time-to-market, fornisce un ambiente sicuro per mitigare le frodi e migliora l’esperienza del cliente distribuendo più contenuti TV su più piattaforme.

## Autenticazione Adobe Pass per TV Everywhere {#adobe-pass-authentication-for-tv-everywhere}

Adobe Pass Authentication opera come soluzione SaaS (Software as a Service) progettata per consentire una rapida integrazione back-end (server-to-server), in conformità con le regole di business sia dei MVPD (Multichannel Video Programming Distributors) che dei programmatori.

### Standard e protocolli {#standards-protocols}

L’autenticazione Adobe Pass è completamente allineata con gli standard e i protocolli emergenti per TV Everywhere (TVE), e supporta l’integrazione e la conformità senza soluzione di continuità nell’ecosistema.

* **Specifica OLCA di CableLabs (Online Content Access)**\
  L’autenticazione di Adobe Pass è conforme alle specifiche OLCA di CableLabs, che definiscono i requisiti tecnici e l’architettura per la distribuzione di contenuti video da origini online ai clienti di Pay TV. Adobe ha partecipato attivamente al progetto di test di interoperabilità di CableLabs nel giugno 2011, superando con successo il processo di test per l’implementazione di un provider di servizi.

L’autenticazione Adobe Pass è progettata per supportare più protocolli (ad esempio SAML, OAuth 2.0 e così via) e offre flessibilità per le espansioni future, inclusi i protocolli personalizzati, in base alle esigenze in continua evoluzione.

La maggior parte delle integrazioni utilizza il protocollo SAML (Security Assertion Markup Language), uno standard primario per l’autenticazione. Adobe Pass Authentication funge da provider di servizi proxy nel framework SAML, mantenendo la risposta di autenticazione SAML come token protetto nel dominio comune Adobe.

In conclusione, l’autenticazione di Adobe Pass è indipendente dal protocollo e progettata per essere allineata agli standard OLCA. Questi standard stabiliscono un quadro comune per i programmatori, i MVPD e i fornitori di servizi, garantendo un approccio coesivo all&#39;implementazione delle funzionalità TVE.

### Integrazione e supporto {#integration-support}

Adobe Pass Authentication collabora con i team tecnici di MVPD per configurare integrazioni personalizzate in base ai loro requisiti specifici, come segue:

* **Integrazioni standard**\
  Offerta gratuita, con documentazione e supporto di base via e-mail.

* **Supporto avanzato**\
  Per personalizzazioni significative o sequenze temporali rapide, è possibile applicare una tariffa di supporto.

L’autenticazione di Adobe Pass supporta la gestione efficiente della logica di business di MVPD, come segue:

* **Regola aziendale indipendente**\
  Per la logica di business applicata completamente da MVPD durante le richieste di autorizzazione, Adobe fornisce i dati necessari. Questi dati possono includere, tra l’altro, l’ID dispositivo univoco e l’indirizzo IP del dispositivo dell’utente che effettua la richiesta.

* **Supporto proprietà personalizzata**\
  Per la logica di business che richiede l’intervento dell’utente o la gestione specifica di Adobe, Adobe può gestire configurazioni personalizzate per ogni MVPD. Questi criteri abilitano flussi di lavoro predefiniti che possono essere attivati in punti specifici del flusso di lavoro di adesione. Per informazioni, contatta il rappresentante del tuo Adobe.

## Flusso diritto {#entitlement-flow}

Il flusso di adesione è una serie di passaggi che un’applicazione Programmatore (TVE) deve completare per inviare in streaming contenuti protetti. Il flusso include diverse fasi che comportano interazioni con MVPD:

* [Fase di autenticazione](#authentication-phase)
* (Facoltativo) Fase di pre-autorizzazione
* [Fase di autorizzazione](#authorization-phase)
* Fase disconnessione

Durante la visita iniziale di un utente a un’applicazione Programmatore (TVE), il flusso di adesione segue la sequenza descritta. Tuttavia, nelle visite successive, l’applicazione può ignorare alcuni passaggi in base allo stato dell’autenticazione e ai criteri di visualizzazione applicabili.

>[!NOTE]
>
> L’applicazione Programmatore (TVE) viene utilizzata in questo documento per fare riferimento collettivamente ai tipi di applicazioni in esecuzione su piattaforme diverse (browser, dispositivi mobili, dispositivi collegati alla TV, ecc.) supportate dall’autenticazione di Adobe Pass.

### Fase di autenticazione {#authentication-phase}

I passaggi seguenti delineano i passaggi di alto livello in caso di integrazione SAML:

1. Caricamento applicazione del programmatore **(sito Web)**\
   L&#39;utente passa all&#39;applicazione del programmatore (sito Web), che integra l&#39;autenticazione Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Richiesta contenuto protetto**\
   Quando l’utente tenta di accedere a contenuti protetti, l’applicazione del Programmatore visualizza un elenco di MVPD tra cui l’utente può effettuare la selezione.

1. **Inizializzazione richiesta autenticazione**\
   Dopo aver selezionato MVPD, l’utente viene reindirizzato a un server di autenticazione Adobe Pass. In questo caso, viene generata una richiesta di autenticazione SAML crittografata per il MVPD selezionato, in caso di integrazione SAML. Questa richiesta viene inviata al MVPD per conto del programmatore. A seconda del sistema di MVPD, il browser dell&#39;utente viene reindirizzato alla pagina di accesso di MVPD oppure un iFrame di accesso è incorporato nell&#39;applicazione del programmatore.

1. **Accesso a MVPD**\
   MVPD accetta la richiesta e presenta la sua interfaccia di accesso tramite reindirizzamento o iFrame.

1. **Accesso e convalida utente**\
   L’utente accede con le proprie credenziali MVPD. MVPD convalida lo stato di abbonamento dell’utente e stabilisce la propria sessione HTTP.

1. **Risposta di MVPD all&#39;autenticazione di Adobe Pass**\
   Una volta completata la convalida, MVPD genera una risposta SAML (crittografata) e la invia nuovamente all’autenticazione di Adobe Pass.

1. **Generazione profilo**\
   L’autenticazione di Adobe Pass verifica la risposta SAML, genera un profilo utente che viene memorizzato in cache e reindirizza l’utente all’applicazione del programmatore (sito web).

### Fase di autorizzazione {#authorization-phase}

**Passaggi di alto livello**

Le seguenti fasi delineano le fasi di alto livello:

1. **Gestione dell&#39;identificatore di risorsa**\
   Il contenuto protetto è identificato da un [identificatore di risorsa](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier), che può essere una stringa semplice o una struttura più complessa. Questo identificatore è predefinito e concordato tra il programmatore e MVPD. L&#39;applicazione del programmatore invia l&#39;identificatore di risorsa all&#39;autenticazione Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Controllo autorizzazione MVPD**\
   Il server di autenticazione di Adobe Pass comunica con l’endpoint di autorizzazione di MVPD utilizzando protocolli standardizzati.

1. **Risposta di MVPD all&#39;autenticazione di Adobe Pass**\
   Una volta completata la convalida, MVPD conferma che l’utente dispone o meno dei diritti necessari per accedere al contenuto e invia nuovamente una risposta all’autenticazione di Adobe Pass.

1. **Generazione di token di decisioni e multimediali**\
   L&#39;autenticazione di Adobe Pass verifica la risposta, genera una [decisione](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) che viene memorizzata nella cache e restituisce la decisione contenente un token multimediale all&#39;applicazione del programmatore (sito Web).

1. **Verifica dell&#39;accesso ai contenuti**\
   L&#39;applicazione del programmatore utilizza il [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) per confermare che l&#39;utente corretto sta accedendo al contenuto corretto. Dopo la convalida, l’utente ottiene l’accesso per visualizzare il contenuto protetto.

## Informazioni sulle adesioni {#understanding-entitlements}

La soluzione di autenticazione Adobe Pass si basa sulla creazione di diritti, ossia parti specifiche di dati generati al completamento dei flussi di lavoro di autenticazione e autorizzazione. Questi diritti consentono l’accesso a contenuti protetti, ma hanno una durata limitata. Una volta scaduto, il diritto deve essere rinnovato avviando nuovamente i processi di autenticazione o autorizzazione.

Per ulteriori informazioni sulle adesioni, consulta i seguenti documenti:

* **Profili**

  Dopo l’autenticazione corretta, Adobe Pass Authentication crea un profilo autenticato (&quot;di lunga durata&quot;) associato all’identificatore dell’applicazione, del dispositivo e del provider di servizi richiedente (identificatore del richiedente).

* **[Metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Dopo l’autenticazione corretta (e in alcuni casi anche dopo l’autorizzazione), Adobe Pass Authentication riceve metadati utente da MVPD che possono esporli all’applicazione richiedente.

* **[Decisioni](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  In caso di esito positivo dell’autorizzazione, Adobe Pass Authentication crea una decisione di autorizzazione (&quot;di lunga durata&quot;) associata all’applicazione, al dispositivo, all’identificatore del provider di servizi (identificatore del richiedente) e a una risorsa protetta specifica (identificatore della risorsa).

* **[Token multimediali](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Se l’autorizzazione viene concessa correttamente, Adobe Pass Authentication crea un token multimediale (&quot;di breve durata&quot;) associato a una richiesta di riproduzione riuscita.
