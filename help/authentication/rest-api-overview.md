---
title: Panoramica API REST
description: Panoramica delle API REST
exl-id: 5533d852-f644-417e-bf80-6f7aa1edd6b2
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '1612'
ht-degree: 0%

---

# Panoramica API REST {#rest-api-overview}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.


## Panoramica {#over}

L’API REST per l’autenticazione di Adobe Pass fornisce accesso diretto ai servizi di autenticazione e autorizzazione di TV Everywhere (TVE). Questa API supporta due architetture primarie: server-to-server o dispositivi collegati (ad esempio console di giochi, Smart TV, set-top box e così via) applicazioni che non dispongono di funzionalità di esplorazione Web.

### Meccanismo di limitazione

L’API REST di autenticazione di Adobe Pass è governata da un [Meccanismo di limitazione](/help/authentication/throttling-mechanism.md).


### Server-to-Server

Le soluzioni server-to-server coinvolgono applicazioni client Programmer che si integrano con i servizi Programmer che si connettono con i servizi di autenticazione di Adobe Pass per i flussi TVE. Questo approccio sposta la maggior parte dell&#39;implementazione TVE dal client al server, dove è possibile creare e gestire un singolo modulo di autorizzazione unificato. La responsabilità principale rimanente delle applicazioni client è la gestione di una visualizzazione web per l’autenticazione degli utenti.



### Dispositivi collegati

Le app Dispositivi connessi comunicano direttamente con l’autenticazione di Adobe Pass tramite le API REST per eseguire la configurazione, la registrazione, i controlli dello stato di autenticazione e i flussi di autorizzazione, mentre per il flusso di autenticazione è necessaria una seconda app schermata (browser). Di conseguenza, gli SDK nativi non vengono utilizzati.



### Altre architetture

Oltre alle due architetture principali basate su API REST, Server-to-Server e Direct client per dispositivi intelligenti, esistono altre architetture.  Tra questi, è primaria l’architettura SDK, che utilizza un componente client denominato Access Enabler che l’autenticazione di Adobe Pass fornisce ai programmatori.  L&#39;app utilizza le API Access Enabler per gestire l&#39;avvio, l&#39;autenticazione, l&#39;autorizzazione e la disconnessione.  Tutte le comunicazioni tra l’app del programmatore e i server di autenticazione di Adobe Pass avvengono tramite Access Enabler.  Access Enabler è disponibile in una versione diversa per le seguenti piattaforme: JavaScript, iOS, tvOS, Android e FireTV.

Anche se è possibile utilizzare l’API REST direttamente sulle piattaforme client che supportano SDK nativi al di fuori di una soluzione server-to-server, questo approccio non è consigliato.



## Pro e contro API REST {#ProsAndCons}

L’API REST di autenticazione di Adobe Pass è stata creata per fornire una soluzione TV Everywhere (TVE) per i dispositivi che non hanno funzionalità di navigazione web o archiviazione persistente. L’API REST supporta tutti i flussi di autenticazione e autorizzazione, ma perché non dispone di un componente SDK nativo. Gli SDK forniti e mantenuti tramite l’autenticazione di Adobe Pass dispongono di funzionalità predefinite che implementano regole di business che, nel caso dell’API REST, devono essere implementate e gestite dai programmatori. Nella tabella Responsabilità del programmatore riportata di seguito vengono descritte le limitazioni dell’API REST corrente che devono essere risolte dai programmatori.



### Pro e contro basati su server e client

Un&#39;architettura server-to-server consente di consolidare la maggior parte della logica di autenticazione e autorizzazione in un&#39;unica unità logica o implementazione.  Questo approccio ha pro e contro.  I vantaggi includono:

* Implementazione singola per la logica di business di autenticazione e autorizzazione.
* Evita la necessità di implementare tale logica su ogni piattaforma supportata utilizzando gli strumenti nativi di tale piattaforma.
* Possibilità di aggiornare le funzionalità senza dover aggiornare i client con tutti i requisiti associati (ad esempio, aggiornamenti degli app store).
* **Più facilmente** estendere e personalizzare le funzionalità authN e authZ (ad esempio, aggiungere D2C).
* Gestione diretta del traffico associato per un maggiore controllo, qualità e monitoraggio.



Anche in questo caso, i contro sono elencati nelle responsabilità del programmatore, ma includono quanto segue:

* L’SSO deve essere implementato per ogni client per le piattaforme senza SSO di Platform.
* Se necessario, i programmatori devono implementare una logica specifica per MVPD.
* Tutte le piattaforme che utilizzano l’API REST condividono una singola configurazione che regola proprietà come i TTL di autenticazione.



### Dispositivi collegati

Per la maggior parte dei dispositivi connessi, è necessario utilizzare l’API REST in un modo o nell’altro, poiché un SDK non è disponibile. Il dispositivo connesso utilizzerà direttamente l’API REST oppure si integrerà con una soluzione server-to-server che utilizza l’API REST.

## Responsabilità del programmatore {#programmer-responsibilities}

Di seguito sono riportate le applicazioni Server-to-Server e Connected Device.

| **Funzionalità** | **Responsabile della gestione della funzionalità** | **Descrizione delle limitazioni dell’API client corrente e delle differenze rispetto agli SDK nativi** |
| --- | --- | --- |
| Impostazioni di configurazione applicate per piattaforma | Adobe | Uno **limitazione importante** Quando si utilizza l’API REST in tutte le piattaforme (inclusi i dispositivi mobili come iOS e Android), le impostazioni di configurazione corrispondenti all’API REST nello strumento di configurazione TVE Dashboard vengono applicate a tutti i dispositivi (anche se è presente un dispositivo iOS che esegue un’applicazione nativa implementata sopra l’API REST). Questa limitazione **può interrompere** i TTL concordati e le impostazioni della piattaforma concordate con gli MVPD, se differiscono per ogni piattaforma. [1](#1) |
| Single Sign-On | Programmatori | Utilizzando l’API REST, SSO è disponibile solo sulle piattaforme che supportano l’SSO con Platform (ad esempio Apple, Roku, Amazon), mentre SSO non può essere garantito per altre piattaforme quando si utilizza l’API REST. Gli SDK memorizzano i dati nella cache in modalità cross-site/app. Ciò significa che l’utente accede una volta a un sito/app e ha già effettuato l’accesso ai siti partecipanti, senza che sia necessaria alcuna interazione da parte dell’utente. [2](#2) |
| Disconnessione singola | Programmatori | In uno scenario SSO SDK nativo, la disconnessione da un’applicazione partecipante comporta la disconnessione dell’utente da qualsiasi posizione. SLO non è supportato nell’API REST corrente. La disconnessione da un’applicazione comporta la disconnessione dell’utente solo per quella particolare applicazione. |
| Memorizzazione in cache | Programmatori | Le implementazioni REST API dovranno implementare un proprio meccanismo di caching per gli elementi dati aziendali concordati. Gli SDK memorizzano automaticamente nella cache vari elementi di dati, tenendo in considerazione diverse regole aziendali. Ad esempio, i metadati dell’utente vengono memorizzati nella cache con lo stesso TTL del token di autenticazione, mentre alcuni elementi possono essere esclusi a livello di programmazione dal caching (verifica preliminare). |
| Meccanismo dettagliato di segnalazione errori | Programmatori | L’API REST si basa principalmente sui codici di errore HTTP per segnalare gli errori dell’applicazione, mentre gli SDK dispongono di un meccanismo di segnalazione degli errori dettagliato che aiuta gli sviluppatori dell’applicazione a comprendere meglio cosa sta accadendo. |
| Ripristino degli errori dell’applicazione (nuovo tentativo in caso di errore, rilevamento del ciclo, ecc.) | Programmatori | Le implementazioni REST API devono creare i propri sistemi di ripristino delle applicazioni, mentre le implementazioni sopra gli SDK beneficiano del sistema di recupero degli errori SDK: ripristino da errori di rete transitori, ripetendo determinate chiamate di rete con logica in loco per evitare &quot;loop&quot;. |
| Autenticazione per richiedente | Programmatori | Alcuni MVPD richiedono l&#39;autenticazione per ogni sito/app, per motivi aziendali o a causa di problemi tecnici. Gli SDK applicano automaticamente questo in base alla configurazione del dashboard TVE. I implementatori REST API devono implementare essi stessi questo, al fine di non violare gli accordi commerciali, o per essere in grado di completare l&#39;autorizzazione per quei MVPD che eseguono l&#39;ambito dei loro dati di autenticazione per applicazione. |
| Autenticazione passiva | Programmatori | Alcuni MVPD supportano l&#39;autenticazione &quot;passiva&quot;, in cui l&#39;utente non è tenuto a immettere le credenziali e viene effettuato automaticamente un tentativo di autenticazione dell&#39;utente. Ciò è particolarmente utile per gli MVPD che hanno come requisito anche &quot;Autenticazione per richiedente&quot;. In questo caso, l’interfaccia utente abilitata dagli SDK è diretta, in cui l’utente si autentica una sola volta in un’applicazione e l’SDK esegue l’autenticazione &quot;passiva&quot; per altre applicazioni nell’ecosistema. L’utente non vede le chiamate passive aggiuntive, semplicemente sarà già autenticato, mentre l’MVPD raggiunge l’obiettivo di avere sessioni di autenticazione separate per ogni applicazione. |
| Rilevamento di dispositivi implicito e uniforme | Programmatori | Gli SDK rilevano automaticamente il tipo di dispositivo e inviano tali informazioni in modo standard. Le implementazioni REST API sono soggette all’invio di informazioni diverse, a seconda dell’implementatore, distorcendo/violando le regole e le statistiche aziendali tra i siti. **I programmatori devono assicurarsi che ci abbiano inviato informazioni corrette sul dispositivo** su ciascuna delle loro app. Per le implementazioni server-to-server, l’API REST non identifica correttamente l’indirizzo IP dell’utente finale, a meno che non vengano eseguiti passaggi aggiuntivi. L&#39;indirizzo IP è importante in alcuni casi di utilizzo, ad esempio per la prevenzione delle frodi o l&#39;HBA. |
| TempPass anti-manomissione | Programmatori | L’applicazione di TempPass si basa su un ID dispositivo stabile. Gli SDK usano tecniche di impronte digitali/informazioni hardware per identificare il dispositivo e questo meccanismo non è pubblico, quindi più sicuro e privo di collisioni. Per le implementazioni REST API, i programmatori devono implementare i propri algoritmi per l’identificazione dei dispositivi e il fingerprinting. |
| Associazione dispositivo | Programmatori | I token generati su un dispositivo con un’applicazione tramite SDK non sono portatili, rendendo difficile per un utente malintenzionato condividere i propri token e fornire l’accesso ad altri utenti. Si basa sullo stesso meccanismo di identificazione del dispositivo del &quot;TempPass a prova di manomissione&quot;. Per l’API REST, i token rimangono nel cloud, il che significa che due dispositivi con gli stessi deviceID che eseguono le stesse chiamate otterranno le stesse risposte/accesso. I programmatori devono accertarsi di disporre di un meccanismo solido, sicuro e privo di collisioni per la generazione/allocazione degli ID dispositivo. |
| Set di funzioni prevedibili e certificate | Programmatori | Il set di funzioni esistente in ogni SDK è coerente, prevedibile, certificato e testato completamente. Alcuni requisiti aziendali vengono applicati tramite gli SDK, rendendo rischioso per il programmatore violare i contratti mentre utilizza l’API REST. Anche se l’API REST potrebbe essere più flessibile, Adobe garantisce che le implementazioni SDK applichino le decisioni aziendali e le impostazioni da TVE Dashboard. Nel caso di Clientless, ogni programmatore implementa il proprio sottoinsieme di regole di business che suite le sue applicazioni in un dato momento. |


1. Come parte della nuova iniziativa One API, prevediamo di correggere questo limite e di poter applicare regole per piattaforma in base all’identificazione del dispositivo.

2. Adobe continua a lavorare con tutte le principali piattaforme per implementare Platform SSO che può essere utilizzato con la nostra API REST. La nostra iniziativa One API offrirà il supporto SSO tra le app implementate utilizzando SDK nativi e le app implementate utilizzando API REST.

## Requisiti minimi del dispositivo {#min_reqs}

Per utilizzare l’API REST di autenticazione di Adobe Pass, i dispositivi devono soddisfare o superare i requisiti tecnici minimi elencati nella sezione REST API della [Documento sui requisiti di piattaforma/dispositivo/strumenti di autenticazione di Adobe Pass](#general_clientless_reqs).
