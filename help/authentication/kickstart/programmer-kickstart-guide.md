---
title: Guida introduttiva per programmatori
description: Guida introduttiva per programmatori
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Guida introduttiva per programmatori {#programmer-kickstart-guide}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questa guida Kick-Start è destinata ai provider di contenuti (programmatori) che intendono integrare Adobe® Pass Authentication nei propri siti Web o nelle proprie applicazioni.

Il presente documento illustra i primi passi fondamentali per garantire un avvio fluido ed efficiente del processo di integrazione. Mira a chiarire le aspettative e a fornire indicazioni su come collaboreremo con i partner per ottenere integrazioni di successo.

Adobe fornisce una serie di risorse per aiutarti a integrare l’autenticazione Adobe Pass nel tuo sito web o applicazione. Fare riferimento alle **&quot;Verranno fornite&quot;** e **&quot;Adobe fornirà&quot;** menzioni da ogni sezione seguente.

## Processo di configurazione {#setup-process}

Il processo di configurazione prevede, tra l&#39;altro, i seguenti passaggi:

![Processo di integrazione dell&#39;autenticazione pass Adobe®](../assets/progr-flow-int-lifecycle.png)

*Processo di integrazione dell&#39;autenticazione pass Adobe®*

**Fornirai** durante la fase di avvio:

* **Provider di servizi (identificatore del richiedente)**

  Si tratta di una stringa che identificherà in modo univoco il brand del sito web o dell’applicazione che effettua le richieste di autenticazione di Adobe Pass. La stringa stessa è arbitraria, ma deve essere concordata tra Adobe e il programmatore

* **Informazioni canale**

  Set di stringhe utilizzato per identificare i canali di contenuto richiesti dal provider di servizi. In molti casi, il canale e il provider di servizi sono gli stessi. Tuttavia, un singolo identificatore può rappresentare più canali di contenuto. Queste stringhe di nome del canale devono essere allineate ai corrispondenti canali TV via cavo. Tieni presente che alcuni MVPD possono convalidare questo valore durante il processo di autenticazione e/o autorizzazione.

* **Nomi di dominio**

  Questo elenco include i nomi di dominio effettivi elencati in Adobe per rappresentare il provider di servizi. In questo modo solo i domini autorizzati possono accedere all’autenticazione Adobe Pass utilizzando i metadati. Assicurati di fornire e identificare chiaramente i nomi di dominio per gli ambienti di produzione e di staging (test), in quanto possono differire.

**Fornirai** tramite MVPD:

* **Set di credenziali**

  Si tratta di credenziali utilizzate per autenticare e autorizzare o autenticare esclusivamente l’utente con MVPD. In genere, queste credenziali sono costituite da un nome utente e una password, che MVPD ti fornirà per entrambi i profili (staging e produzione).

* **Identificatori risorsa**

  Si tratta di identificatori univoci per i canali di contenuto, gli spettacoli, gli episodi o le risorse che il provider di servizi desidera proteggere. Questi identificatori vengono utilizzati per richiedere decisioni di autorizzazione e devono essere concordati con il MVPD.

>[!IMPORTANT]
>
> Il programmatore è responsabile del coordinamento con MVPD per finalizzare eventuali accordi commerciali necessari. Nel frattempo, l’autenticazione di Adobe Pass collaborerà con MVPD per garantire che l’integrazione tecnica sia stabilita correttamente:
>
> * **Nuovo MVPD**
>
>     Se MVPD non è integrato con Adobe, il codice personalizzato deve essere sviluppato in base ai requisiti specifici di MVPD. Finché questo sviluppo non sarà completato, il MVPD non sarà disponibile e non sarà possibile procedere con il test del prodotto con tale MVPD.
>
> * **MVPD esistente**
>
>     Se MVPD è già integrato con Adobe, il processo di connettività è notevolmente semplificato. Nella maggior parte dei casi, la connettività può essere stabilita rapidamente tramite regolazioni di configurazione anziché tramite uno sviluppo esteso.
>
> Tutte le integrazioni richiedono attività congiunte di controllo qualità (QA, Quality Assurance), incluso il test da parte di MVPD, in quanto l&#39;utente finale è in ultima analisi un cliente di MVPD. Il coordinamento dei cicli di test dipende spesso dalla disponibilità delle risorse di MVPD, che può introdurre potenziali ritardi.

## Accesso all’assistenza clienti {#access-customer-support}

**Adobe fornirà** l&#39;accesso al nostro sistema di assistenza clienti tramite [Zendesk](https://tve.zendesk.com/home). Per accedere a Zendesk, è necessario registrarsi e creare un account su https://tve.zendesk.com/home. Non esiste alcun limite al numero di utenti che è possibile registrare. Una volta effettuata la registrazione, puoi visualizzare e condividere i commenti su qualsiasi ticket inviato.

Il team di autenticazione di Adobe Pass è a tua disposizione per aiutarti a risolvere eventuali domande o problemi tecnici riscontrati durante il processo di integrazione. Contattaci all&#39;indirizzo [tve-support@adobe.com](mailto:tve-support@adobe.com).

## Accesso alla documentazione {#access-documentation}

**Adobe fornirà** accesso alla nostra documentazione pubblica tramite [Adobe Experience League](https://experienceleague.adobe.com/it/docs/pass/authentication/home).

Il team di autenticazione di Adobe Pass fornisce una documentazione completa sulle funzioni e API disponibili nella sezione [Guida all&#39;integrazione per programmatori](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md). Fare riferimento al sommario di questa sezione per collegamenti a informazioni dettagliate su ciascun argomento.

## Accesso allo strumento di test {#access-testing-tool}

**Adobe fornirà** l&#39;accesso al nostro strumento di esplorazione API tramite il sito Web [Adobe Developer](https://developer.adobe.com/adobe-pass/).

## Accesso allo strumento di gestione della configurazione {#access-configuration-management-tool}

**Adobe fornirà** l&#39;accesso a uno strumento self-service per la gestione della configurazione e dei dati tramite [Adobe Pass TVE Dashboard](https://experience.adobe.com/pass/authentication).

Il team di autenticazione di Adobe Pass fornisce una documentazione completa per l&#39;utilizzo del dashboard TVE nella sezione [Guida utente per dashboard TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md). Fare riferimento al sommario di questa sezione per collegamenti a informazioni dettagliate su ciascun argomento.
