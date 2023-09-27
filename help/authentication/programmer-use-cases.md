---
title: Casi di utilizzo dei programmatori
description: Casi di utilizzo dei programmatori
exl-id: 51ca7e4f-b0d8-4e35-8398-2efb4879de2a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1643'
ht-degree: 0%

---

# Casi di utilizzo dei programmatori {#programmer-use-cases}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

Questo documento riepiloga i casi di utilizzo dell’integrazione del programmatore supportati dall’autenticazione di Adobe Pass. Controlla questa pagina prima di iniziare un progetto di integrazione per vedere quali funzioni sono attualmente supportate.

## Casi d’uso {#use-cases}


### Integrazione di base: autenticazione e autorizzazione federate per una rete a canale singolo {#basic-integration}

**Priorità** - Alta

**Raggruppamento** - App TVE a marchio di singolo programmatore con 1 canale di rete ospitato all’interno dell’esperienza

Questo consente ai programmatori di offrire contenuti premium, nella propria app TVE con marchio*, con un controllo federato di adesione all’MVPD. RequestorID deve essere allineato in modo che corrisponda al brand dell’applicazione che fornisce il contenuto al visualizzatore. In questo scenario, esiste una relazione 1 a 1 tra l’ID richiedente autenticazione di Adobe Pass e l’ID risorsa che viene verificato per l’adesione.

>[!NOTE]
>
>In questo documento viene utilizzata l’app TVE per fare riferimento collettivamente ai diversi tipi di applicazioni (applicazioni web, app mobili, ecc.) supportato dall’autenticazione di Adobe Pass. La colonna Piattaforme riportata di seguito può contenere dettagli sulle piattaforme supportate per casi d’uso specifici.

#### Casi d’uso specifici (comuni alla maggior parte delle integrazioni) {#sp-use-cases-basic-int}

| Priorità | Caso d’uso | Descrizione | Piattaforme | Note MVPD |
|:--------:|:-----------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------:|:-----------------------------------------:|
| Alta | Individuazione MVPD dall&#39;app TVE del programmatore | L&#39;utente inizia con l&#39;app TVE del programmatore e gli viene richiesto di selezionare il proprio provider MVPD. | API Web (SWF/JS) Mobile (iOS/Android) senza client (per il secondo schermo) |                                           |
| Alta | Autenticazione federata dall’app TVE del programmatore | L&#39;utente inizia con l&#39;app TVE con marchio del programmatore e dopo aver selezionato il proprio provider MVPD, passa alla pagina di accesso di MVPD per immettere le proprie credenziali. | Web (SWF/JS) Mobile (iOS/Android) |                                           |
| Alta | Autorizzazione Dal Programmatore TVE App | Dopo che l&#39;utente è autenticato, l&#39;app TVE del programmatore è in grado di effettuare richieste di autorizzazione back-channel all&#39;MVPD per verificare l&#39;adesione dell&#39;utente. Di solito si tratta solo di verificare se la rete del canale è nel pacchetto di abbonamento MVPD degli utenti.                                  In questo caso, l’ID richiedente e l’ID risorsa corrispondono a 1:1. | Tutte le piattaforme |                                           |
| Medio | Disconnetti dall&#39;app TVE del programmatore | Consente all’utente di disconnettersi e cancellare i token AuthN/AuthZ di autenticazione Adobe Pass. In molti casi, questo comporta anche la disconnessione dell’utente dal MVPD. Ma i MVPD variano nel caso in cui questo sia supportato. Cancella sempre la sessione di autenticazione e i token di Adobe Pass. | Tutte le piattaforme tranne XBox nativo | Diversi MVPD non supportano questa funzione. |
| Alta | Single Sign-On tra siti e app | Consente all&#39;utente di condividere la sessione di accesso tra siti e app senza dover effettuare di nuovo l&#39;accesso. | Tutte le piattaforme tranne API client | Richiede almeno SDK 1.7 per alcuni MVPD. |

### App TVE singola che ospita più reti di canali {#single-app-multi-channel}

**Priorità**- Alta

Consente al programmatore di aggregare diverse reti di canali di contenuti sulla stessa destinazione del marchio per i propri visualizzatori.

#### Casi d’uso specifici {#sp-use-cases-singl-tve-app}

| Priorità | Caso d’uso | Descrizione | Piattaforme | Note MVPD |
|--------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alta | Autorizzazione del canale distinta | L’utente è in grado di guardare contenuti provenienti da diverse reti di canali all’interno della stessa app TVE. Il Programmatore è in grado di effettuare chiamate di autorizzazione specifiche per ogni rete di canali per confermare il diritto degli utenti. | Tutte le piattaforme | Tutti gli MVPD ora lo supportano in qualche forma. |
| Basso | Query di autorizzazione verifica preliminare | Questo consente al programmatore di controllare quali canali ha l’utente nel suo pacchetto in una singola chiamata API. Questa operazione viene eseguita prima delle chiamate AuthZ effettive per filtrare il contenuto dall’interfaccia utente a cui l’utente non ha accesso. |               | La maggior parte degli MVPD non espone ancora questi dati come Attributi utente, quindi Adobe effettua effettivamente chiamate AuthZ per ottenerli. Inoltre, la maggior parte degli MVPD è limitata a 5 alla volta, perché non supportano più canali in una singola chiamata.                             È molto importante verificare il numero di canali necessari al programmatore per la verifica preliminare. Indipendentemente dal numero, dovremo verificare che sia corretto con gli MVPD. La maggior parte degli MVPD non supporta attualmente (3° trimestre 2013) più di 5 canali. |

### Autorizzazione a livello di risorsa {#asset-level-authz}

**Priorità** - Bassa

**Raggruppamento** - Trasmettere Un Identificatore Di Risorsa Alla Richiesta Di Autorizzazione

**Piattaforme** - Tutte le piattaforme

#### Casi d’uso specifici {#sp-use-cases-asset-lvl-authz}

Consente a MVPD di ottenere analisi a livello di risorsa per ogni chiamata AuthZ. Questo ha lo svantaggio di negare la cache AuthZ di autenticazione di Adobe Pass.

| Priorità | Caso d’uso | Descrizione | Piattaforme | Note MVPD |
|--------|-------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|-------------|--------------------------------------|
| Basso | Trasmettere un identificatore di risorsa alla richiesta di autorizzazione | Consente a MVPD di ottenere analisi a livello di risorsa per ogni chiamata AuthZ.  Ha lo svantaggio di negare la cache AuthZ di autenticazione di Adobe Pass. | Tutte le piattaforme | Al momento questo è supportato da un solo MVPD. |




### Controllo genitori {#parental-controls}

**Priorità** - Bassa

Consente di applicare restrizioni all&#39;account utente MVPD nell&#39;app TVE del programmatore.

| Priorità | Caso d’uso | Descrizione | Piattaforme | Note MVPD |
|--------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|-----------------------------------|
| Basso | Filtrare i contenuti in base agli attributi utente | Consente al programmatore di controllare la valutazione massima consentita per un utente prima di eseguire il rendering dell’elenco dei contenuti disponibili per l’utente. | Web (Flash/JS) Mobile (iOS/Android) | Attualmente funziona solo con un MVPD. |
| Basso | Trasmettere le valutazioni dei contenuti nella richiesta AuthZ | Consente al programmatore di trasmettere la valutazione specifica del contenuto che l’utente desidera guardare come parte della richiesta AuthZ all’MVPD correlata alla #3, poiché le valutazioni si riferiscono in genere al livello della risorsa. | Tutte le piattaforme | Attualmente funziona solo con un MVPD. |

#### Personalizzazione dell’integrazione MVPD per marchio del programmatore {#mvpd-int-cust-prog-brand}

**Priorità** - Media

Abilita l’esperienza personalizzata durante AuthN o per messaggi di errore AuthZ.

| Priorità | Caso d’uso | Descrizione | Piattaforme | Note MVPD |
|--------|------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|-----------------------------------------|
| Medio | Passa l’identificatore del provider di servizi nella richiesta AuthN. | Abilita il branding specifico nella pagina di accesso MVPD specifica per il provider di servizi. Abilita anche la selezione automatica del valore predefinito in modo che corrisponda al pubblico, ad esempio spagnolo per Univision. | Tutte le piattaforme | Varia in base a MVPD. Alcuni non supportano questa impostazione. |
| Medio | Messaggi di errore personalizzati nella risposta AuthZ | Consente ai programmatori o ai brand di inviare messaggi di errore specifici da MVPD che possono includere messaggi specifici per la vendita in upselling con un collegamento che aggiorna il pacchetto. | Web, Android, iOS | Varia in base a MVPD. Alcuni non supportano questa impostazione. |


### Casi di utilizzo dei dispositivi collegati {#connected-devices}

| Priorità | Caso d’uso | Descrizione | Piattaforme | Note MVPD |
|--------|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Medio | XBox LiveID SSO tra app e console | Consente all’utente di condividere una sessione AuthN tra app e tra console di gioco diverse, in base all’account LiveID. | SDK nativo XBox | Questo non piace alla maggior parte degli MVPD perché il modello tipico è quello di associare il token al dispositivo, non all’utente.                             Se possibile, non consigliamo più questo approccio. |
| Alta | Dispositivo connesso con token associato all&#39;appID sul dispositivo | Consente al programmatore di associare il diritto MVPD nel token all&#39;appID sul dispositivo a cui è stato rilasciato. | API senza client | In questo modo il dispositivo collegato viene allineato più strettamente all&#39;implementazione standard di Pass per i token.                             È ancora necessario migliorare per essere un ID a livello di dispositivo. |

### Lunghezza TTL AuthN specifica del dispositivo {#authn-ttl-length}

Abilita l’adesione TVE per eventi speciali che potrebbero non essere risorse presenti nel database dei diritti MVPD come i canali normali.

| Priorità | Caso d’uso | Descrizione | Piattaforme | Note MVPD |
|--------|------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|--------------------------------------------------------------------------------------------------------------------------|
| Alta | Imposta valori TTL diversi per piattaforma | Consente al programmatore di stabilire una lunghezza TTL diversa per dispositivi web, mobili e connessi. Attualmente, l’autenticazione Adobe Pass supporta la possibilità di disporre di 3 valori TTL separati: Web (Flash) Mobile/HTML5 Clientless - Dispositivi connessi |           | Alcuni MVPD impostano il TTL in modo dinamico. Se necessario, Adobe può ignorare queste impostazioni dinamiche utilizzando le impostazioni di configurazione. |

### Applicazioni speciali basate su eventi {#special-event}

**Priorità** - Bassa

Abilita l’adesione TVE per eventi speciali che potrebbero non essere risorse presenti nel database dei diritti MVPD come i canali normali.

| Priorità | Caso d’uso | Descrizione | Piattaforme | Note MVPD |
|--------|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|------------------------------------------|
| Basso | Più canali come proxy per un evento | Questo è stato fatto per le Olimpiadi, dove gli abbonati dovevano avere due canali diversi nel loro pacchetto per avere accesso. In questo caso, l’autenticazione Adobe Pass ha creato un nuovo resourceID e ha richiesto a tutti gli MVPD di eseguire la mappatura sui canali specifici della propria estremità.  Ha funzionato bene con un preavviso sufficiente. Questo era importante perché la maggior parte degli MVPD non supporta più chiamate di risorse. | Tutte le piattaforme | Supportato da tutti gli MVPD con adeguato preavviso. |
| Basso | Nuova applicazione evento speciale, utilizzando le risorse di canale esistenti | Questo è stato fatto per la pazzia di marzo. Il provider di contenuti ha creato una nuova app con un nuovo requestorID. Tutti gli MVPD dovevano aggiungere il supporto per il nuovo requestorID nel proprio sistema. I resourceID erano canali normali.  Alcuni MVPD dovevano anche mappare i canali come validi sotto il nuovo richiedente, quindi in quei casi era necessario più tempo. | Tutte le piattaforme | Supportato da tutti gli MVPD con adeguato preavviso. |
| Basso | ID richiedente esistente, ID risorsa | Questo è stato fatto per il torneo di golf di Masters. Era solo un piccolo evento per un paio di giorni, e i Maestri avevano la loro app mobile che era in grado di visualizzare il contenuto. Il programmatore ha pianificato di pagare per il traffico di autenticazione di Adobe Pass e di utilizzare solo i propri requestorID e resourceID standard. L’unico trucco è stato quello di far condividere al programmatore un certificato mobile per la firma requestorID con i master, e averlo aggiunto alla loro configurazione come certificato di backup per quel fine settimana. | Tutte le piattaforme | Nessun impatto per gli MVPD |

### Integrazione del server dei contenuti {#content-server-integration}

**Priorità**- Media

Abilitazione della convalida del token multimediale prima di rilasciare il flusso video al lettore client.
| Priorità | Caso d’uso | Descrizione | Piattaforme | Note MVPD | ---------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|----------| | Alto | Programmatore Federated Player - Con autorizzazione a livello di pagina | Le API di autenticazione di Adobe Pass vengono eseguite in JavaScript nella pagina e il token viene passato al lettore. Il token può essere passato al servizio di convalida in due modi: Ottieni parametro sul parametro URL del servizio di convalida trasmesso nella stringa query dell’URL di flusso API Interfaccia esterna FlashVars | | | | Medio | Programmatore Federated Player - Con Autorizzazione Interna Del Lettore | Le API di autenticazione di Adobe Pass vengono eseguite in ActionScript nel SWF del lettore, in modo che il token sia disponibile per il lettore dal callback.                                                                                                                                                                                         | | | | Alto | Lettore sindacato - Ospitato sul portale MVPD con autorizzazione a livello di pagina utilizzando un iFrame per avvolgere il lettore | Simile al lettore con autorizzazione a livello di pagina, ma con il wrapper della pagina del lettore iFramed nel portale MVPD. L&#39;autenticazione deve avvenire separatamente nel portale MVPD.                                                                                                                                                    |           |                        |


<!--
>[!RELATEDINFORMATION]
>
>* MVPD Integration Features
>* Entitlement Flow
>* Platform / Device Requirements
-->
