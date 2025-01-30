---
title: Guida introduttiva per programmatori
description: Guida introduttiva per programmatori
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '969'
ht-degree: 0%

---

# Guida introduttiva per programmatori {#programmer-kickstart-guide}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#prog-kickstart-guide-intro}

Benvenuti in Autenticazione Adobe Pass per TV Everywhere. Saremo lieti di collaborare con voi.

>[!NOTE]
>
>Questa è la Guida Kick-Start per programmatori (provider di contenuti). Se utilizzi un distributore di programmi video multicanale (MVPD), assicurati di consultare la [guida rapida di MVPD](/help/authentication/kickstart/mvpd-kickstart-guide.md).


Contatti di autenticazione Adobe Pass:

* Supporto: per tutte le domande, i problemi o le richieste di funzionalità, `tve-support@adobe.com`
* Un contatto di abilitazione verrà assegnato al progetto al momento dell’avvio del progetto.

Le seguenti informazioni delineano alcuni importanti primi passi per iniziare in modo solido ed efficiente. L’obiettivo è quello di fornire una spiegazione e aspettative su come lavoreremo con i partner per raggiungere le integrazioni. Si prega di notare le sezioni &quot;si fornirà&quot; / &quot;Adobe fornirà&quot; di seguito per ogni elemento. Questi vengono elencati tramite una checklist o una guida mentre lavoriamo attraverso il progetto.

In questo documento si presuppone che i programmatori siano registrati per lavorare con un partner MVPD scelto.

## Pianificazione della versione {#release-schedule}

Il ciclo di sviluppo di Adobe sprint è pianificato in modo da poter vedere quando vengono pianificate le nostre versioni e come ogni versione viene promossa attraverso il nostro sistema di sviluppo.

Una volta completato, lo sprint è disponibile per il QE o per visualizzare nuove implementazioni sul nostro server UAT.

Ogni 6 settimane circa verrà rilasciata una versione al server di pre-qualificazione di Adobe. (Questo è il server in cui è conservata la prossima versione proposta durante l’esecuzione del QE finale). Queste build includeranno tutto il lavoro completato negli sprint dall’ultima goccia. Al momento è disponibile una finestra di controllo qualità di due settimane che consente ai partner di testare questa versione.

Supponendo che non si siano verificati problemi critici durante le due settimane precedenti del periodo di test, la versione verrà promossa a produzione live. Ciò significa che l’integrazione sarà disponibile nell’ambiente di rilascio di Adobe, ma i partner scelgono quando rendere pubblica la versione.

<!--For the latest release schedule information, see the Release Calendar.-->

## Documentazione di supporto {#supp-doc}

Adobe fornirà:

* Guida alla distribuzione: **`https://tve.zendesk.com/entries/498741-tve-deployment-guide`**
* Accesso al nostro sistema di assistenza clienti Zendesk. Qui puoi anche trovare esempi, informazioni e tutorial video su alcuni dei processi. Per accedere a questo documento su Zendesk, insieme ad altri documenti pubblicati, dovrai registrarti e creare un account su `https://tve.zendesk.com/home`. Non esiste alcun limite alla quantità di utenti che è possibile registrare.  Puoi visualizzare e condividere i commenti su qualsiasi ticket archiviato. Tutte le domande di supporto devono essere indirizzate a `tve-support@adobe.com`.
* Libreria del verificatore di token multimediali: `https://tve.zendesk.com/entries/471323-media-token-validator-library`.

## Configurazione dell’ambiente di test {#test-env-setup}

Adobe esegue prima la configurazione con il sito di test Adobe, in cui Adobe funge da MVPD a scopo di test. Il team può quindi configurare un sito web di prova che chiama l’API Adobe. Utilizza il selettore MVPD predefinito e seleziona &quot;Adobe&quot; come idP.

Fornirai:

1. ID richiedente. Si tratta di una stringa che identificherà in modo univoco il brand del sito web o dell’applicazione che effettua le richieste di autenticazione di Adobe Pass. La stringa stessa è arbitraria, ma deve essere concordata tra Adobe e il programmatore
1. Informazioni sul canale. Questo è un set di stringhe che identifica i canali di contenuto richiesti dall’ID richiedente. In molti casi il canale e l’ID richiedente sono gli stessi. Tuttavia, puoi avere più canali di contenuto che possono essere richiesti dallo stesso ID. Le stringhe del nome del canale devono corrispondere ai canali TV via cavo. Alcuni MVPD convalideranno questo valore sul protocollo AuthN e/o AuthZ.
1. Nomi di dominio (da consentire per l’ID richiedente). Si tratterà di un elenco di nomi di dominio effettivi che verranno elencati per Adobe per accettare l’ID richiedente. In questo modo solo i domini approvati potranno accedere all’autenticazione di Adobe Pass con i tuoi metadati. NOTA: i nomi di dominio validi per la produzione possono essere diversi per test/staging e devono essere forniti e identificati.

Adobe configurerà l’account e Adobe fornirà:

* Accesso e password per accedere al sito di test

## Configurazione con MVPD {#setup-mvpd}

Questa sezione descrive cosa devi fare quando effettui la migrazione dal sito di test di Adobe per lavorare con un MVPD.

Fornirai (tramite MVPD):

* **Due set di credenziali**:
   * AuthN + AuthZ : login/password per un utente autenticato e autorizzato
   * AuthN + Non-AuthZ : login/password per un utente autenticato ma non autorizzato
* **ID risorsa**. Si tratta di un identificatore di contenuto specifico che verrà convalidato con un MVPD tramite il protocollo AuthZ. Può essere a livello di canale, programma, episodio o risorsa; deve essere concordato con il tuo MVPD.

L’autenticazione di Adobe Pass supporta uno schema di metadati basato su MRSS, il che significa che gli ID risorsa possono essere specifici in base alle esigenze e possono includere identificatori che possono essere univoci per uno specifico MVPD.

**NUOVA integrazione di MVPD**: è importante ricordare che il MVPD scelto è parte integrante del completamento di qualsiasi integrazione. Adobe deve scrivere il codice per ogni MVPD in base alle proprie specifiche. Fino a quando questi passaggi non saranno completati, non potrai selezionare quel MVPD dalla finestra di dialogo o completare il test del prodotto. Adobe deve pianificare questo lavoro in anticipo per adattarlo al prossimo sprint disponibile. Per informazioni sulla pianificazione corrente, fare riferimento al Calendario delle versioni.

**Integrazioni MVPD esistenti**: se il MVPD scelto è già configurato con Adobe, i passaggi di connettività dovrebbero essere molto più semplici (più veloci) e spesso è possibile ottenere la connettività tramite modifiche alla configurazione.

>[!NOTE]
>
>Il MVPD dovrà COMUNQUE abilitare il programmatore e firmare eventuali accordi commerciali pertinenti.

**QE con MVPDs**: tutte le integrazioni implicheranno un QE congiunto e, poiché l&#39;utente finale è alla fine un cliente di MVPD, molti hanno impostato cicli di test prima di inviare il messaggio &quot;live&quot;. Poiché si tratta della pianificazione delle risorse di MVPD, è possibile che si verifichino ritardi.

<!--
>[RELATEDINFORMATION]
>[MVPD Kickstart Guide](help\authentication\mvpd-kickstart-guide.md)
-->
