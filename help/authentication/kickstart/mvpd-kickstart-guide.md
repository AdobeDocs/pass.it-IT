---
title: Guida di Kick-Start per MVPD
description: Guida di Kick-Start per MVPD
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# Guida di Kick-Start per MVPD {#mvpd-kickstart-guide}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questa guida introduttiva è destinata ai distributori di programmi video multicanale (MVPD) che intendono integrarsi con l’autenticazione di Adobe® Pass.

Il presente documento illustra i primi passi fondamentali per garantire un avvio fluido ed efficiente del processo di integrazione. Mira a chiarire le aspettative e a fornire indicazioni su come collaboreremo con i partner per ottenere integrazioni di successo.

Adobe fornisce una serie di risorse per aiutarti a integrarti con l’autenticazione di Adobe Pass. Fare riferimento alle **&quot;Verranno fornite&quot;** e **&quot;Adobe fornirà&quot;** menzioni da ogni sezione seguente.

>[!CAUTION]
>
> Ogni volta che un utente avvia un flusso di adesione, gli viene assegnato un singolo ID utente opaco e univoco. Questo ID, proveniente da MVPD, viene utilizzato per identificare l’utente all’interno dell’app di un programmatore.
>
> <br/>
>
> L’ID utente non deve contenere dati o informazioni personali (PII, Personally Identifiable Information) che potrebbero essere utilizzati da soli o in combinazione con altri dettagli per identificare, contattare o individuare l’utente.

## Processo di configurazione {#setup-process}

Il processo di configurazione prevede, tra l&#39;altro, i seguenti passaggi:

![Processo di integrazione dell&#39;autenticazione pass Adobe®](../assets/mvpd-int-lifecycle.png)

*Processo di integrazione dell&#39;autenticazione pass Adobe®*

### Avvio {#kickoff}

**Fornirai** durante la fase di avvio:

* **Nome visualizzato**

  Si tratta di una stringa visualizzata sui siti Web o sulle applicazioni dei programmatori quando viene richiesto agli utenti di selezionare il provider di Pay TV.

* **URL logo**

  Si tratta di un file, che misura 112 x 33 pixel, contenente il logo visualizzato sui siti Web o sulle applicazioni dei programmatori quando si richiede agli utenti di selezionare il proprio provider di Pay TV.

* **Durata (TTL)**

  Il TTL è un valore in genere impostato da MVPD come parte dei processi di autenticazione o autorizzazione. Tuttavia, Adobe può ignorare tali valori TTL e fornire valori diversi a seconda di quanto concordato sia dal programmatore che dal MVPD.

* **Set di credenziali**

  Si tratta di credenziali utilizzate per autenticare e autorizzare o autenticare esclusivamente l’utente con MVPD. In genere, queste credenziali sono costituite da un nome utente e una password, che devono essere forniti per entrambi i profili (staging e produzione).

### Scambio di metadati (SAML) {#metadata-exchange-saml}

**Adobe fornirà** durante la fase di scambio dei metadati:

* **Metadati dell&#39;ambiente di staging**

  I metadati SP di Adobe possono essere recuperati da https://sp.auth-staging.adobe.com/sp/metadata.

* **Metadati dell&#39;ambiente di produzione**

  I metadati SP di Adobe possono essere recuperati da https://sp.auth.adobe.com/sp/metadata.

**Fornirai** durante la fase di scambio dei metadati:

* **Metadati gestione temporanea**

  I metadati di MVPD per l’ambiente di staging.

* **Metadati di produzione**

  I metadata di MVPD per l&#39;ambiente di produzione.

### Connettività {#connectivity}

**L&#39;utente fornirà** un modo per inserire nell&#39;elenco Consentiti gli IP da Adobe, poiché l&#39;autenticazione Adobe Pass richiede firewall per consentire il traffico attraverso le porte 80 e 443 per abilitare l&#39;accesso alle risorse limitate durante i processi di autenticazione e autorizzazione.

**Fornirai** una distribuzione nel profilo di staging per testare la connettività.

### Sviluppo {#development}

**Adobe fornirà** tempo di progettazione per lavorare a stretto contatto con MVPD per garantire che l&#39;integrazione tecnica sia stabilita correttamente. Questo processo comporta lo sviluppo di un codice personalizzato adatto ai requisiti specifici di MVPD.

### Implementazione nell’area di gestione temporanea {#deployment-staging}

**Adobe fornirà** una build con gli aggiornamenti di codice richiesti che verranno prima distribuiti nell&#39;ambiente di staging PRE-QUAL. Durante questa fase verranno implementate anche le modifiche di configurazione necessarie per integrare MVPD con il provider di servizi `TestDistributors` a scopo di test.

**L&#39;utente e Adobe forniranno** tempo di controllo qualità per garantire che l&#39;integrazione sia testata correttamente nell&#39;ambiente di staging PRE-QUAL. Dopo questa fase, il MVPD verrà spostato nell’ambiente di staging RELEASE per ulteriori test con un programmatore effettivo.

### Implementazione in produzione {#deployment-production}

**Fornirai** una distribuzione nel profilo di produzione per testare la connettività.

**Adobe fornirà** una build con gli aggiornamenti di codice richiesti che verranno distribuiti nell&#39;ambiente di produzione PRE-QUAL.

**L&#39;utente e Adobe forniranno** tempo per il controllo qualità (QA) al fine di garantire che l&#39;integrazione sia stata testata correttamente utilizzando il profilo di produzione. Se a questo punto tutto è a posto, Adobe può spostare l’integrazione nell’ambiente di produzione RELEASE (&quot;live&quot;), che è disponibile per tutti gli utenti.

>[!IMPORTANT]
>
> Una volta che l’integrazione è attiva nell’ambiente di produzione RELEASE, è fondamentale mantenere un’esperienza del cliente ottimale. Per affrontare in modo efficace gli scenari di server-down, gli MVPD devono fornire ad Adobe la documentazione dettagliata della procedura di riassegnazione per la gestione di tali problemi.
>
> In cambio, Adobe garantisce che gli MVPD ricevano la versione più recente del processo di escalation dell’autenticazione di Adobe Pass per una risoluzione semplificata dei problemi.

## Accesso agli ambienti {#access-environments}

**Adobe fornirà** l&#39;accesso agli ambienti per le diverse fasi del processo di sviluppo:

* **Prequalificazione (PRE-QUAL)**

  L’ambiente PRE-QUAL ospita la versione successiva candidata e funge da piattaforma di integrazione iniziale per i nuovi partner. Prima di passare all’ambiente RELEASE, ai partner viene dato il tempo di testare la loro integrazione in PRE-QUAL.

* **Versione (Release)**

  L’ambiente RELEASE ospita la build di produzione corrente (stabile).

Per ulteriori informazioni su come utilizzare questi ambienti, consulta la [Documentazione sugli ambienti Adobe](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md).

>[!IMPORTANT]
> 
> Le modifiche alla configurazione di questi ambienti devono essere esplicitamente richieste tramite il rappresentante Adobe, seguendo il processo di richiesta di modifica stabilito.

## Accesso all’assistenza clienti {#access-customer-support}

**Adobe fornirà** l&#39;accesso al nostro sistema di assistenza clienti tramite [Zendesk](https://tve.zendesk.com/home). Per accedere a Zendesk, è necessario registrarsi e creare un account su https://tve.zendesk.com/home.

Il team di autenticazione di Adobe Pass è disponibile per rispondere a qualsiasi domanda o problema tecnico riscontrato durante il processo di integrazione. Contattaci all&#39;indirizzo [tve-support@adobe.com](mailto:tve-support@adobe.com).

## Accesso alla documentazione {#access-documentation}

**Adobe fornirà** accesso alla nostra documentazione pubblica tramite [Adobe Experience League](https://experienceleague.adobe.com/it/docs/pass/authentication/home).

Il team di autenticazione di Adobe Pass fornisce una documentazione completa sulle funzioni e i flussi di lavoro disponibili nella sezione [Guida all&#39;integrazione per MVPDs](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md). Fare riferimento al sommario di questa sezione per collegamenti a informazioni dettagliate su ciascun argomento.

## Accesso allo strumento di test {#access-testing-tool}

**Adobe fornirà** l&#39;accesso al nostro strumento di esplorazione API tramite il sito Web [Adobe Developer](https://developer.adobe.com/adobe-pass/).
