---
title: Punto decisionale del criterio
description: Punto decisionale del criterio
exl-id: 94bc638c-bef8-45ea-b20a-9b7038adecdd
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 0%

---

# Punto decisionale del criterio {#policy-desc-pt}

## Modello di dominio {#domain-model}

Questa pagina è destinata a fungere da riferimento per diversi casi d’uso e implementazioni di criteri. Ti consigliamo inoltre di consultare la sezione [Glossario](/help/concurrency-monitoring/cm-glossary.md) della documentazione per le definizioni dei termini.

Un **tenant** possiede **applicazioni** per le quali desidera applicare **criteri**. **Le applicazioni client** devono essere configurate con **ID applicazione** (fornito dall&#39;Adobe).

Il tenant associa quindi ogni applicazione a uno o più criteri, creati da lui o condivisi da altri. I criteri possono essere collegati tra più tenant.

L&#39;**attività oggetto** è costituita da tutti i flussi (indipendentemente dall&#39;applicazione) segnalati al monitoraggio concorrenza per un determinato oggetto.

Quando un flusso deve essere autorizzato per un determinato soggetto, il sistema controlla innanzitutto tutti i criteri definiti per l&#39;applicazione che ha creato il flusso.

Per ciascuno dei criteri applicabili, è quindi necessario raccogliere tutte le **attività rilevanti** che verranno passate alla regola. L&#39;**attività rilevante** per un criterio P includerà un flusso S solo se soddisfa la seguente condizione:

**Il flusso &quot;S&quot; è avviato da un&#39;applicazione che include il criterio &quot;P&quot; tra i criteri.**

![Il flusso &quot;S&quot; è avviato da un&#39;applicazione che include il criterio &quot;P&quot; tra i criteri.](assets/pdp-domain-model.png)

## Casi di utilizzo di esecuzione di prova {#dry-run-use-cases}

La procedura dettagliata seguente mira a convalidare il modello rispetto ad alcuni casi d’uso. Lo faremo gradualmente, iniziando con una configurazione di base e aggiungendo complessità in vari modi.

### 1. Un locatario. Un&#39;applicazione. Una regola. Un flusso {#onetenant-oneapp-onepolicy-onestream}

Inizieremo con un singolo tenant, con una singola applicazione e un singolo criterio associato. Supponiamo che il criterio stabilisca che possa esserci al massimo un flusso attivo per qualsiasi utente (è consentita la riproduzione del flusso più recente).

Una volta avviato un flusso, l’attività sarà costituita solo da tale flusso e può essere riprodotta.

![Un tenant. Un&#39;applicazione. Una regola. Un flusso](assets/onetenant-app-policy-stream.png)


### 2. Un locatario. Un&#39;applicazione. Una regola. Due ruscelli. {#onetenant-oneapp-onepolicy-twostreams}

Una volta avviato un secondo flusso (dallo stesso soggetto utilizzando la stessa applicazione), l&#39;attività utilizzata per la convalida sarà costituita da **s1** e **s2**.

Il limite è stato superato perché il criterio indica che è consentito riprodurre un solo flusso, pertanto verrà consentito solo il flusso più recente (**s2**).

![Un tenant. Un&#39;applicazione. Una regola. Due flussi.](assets/tenant-app-policy-twostream.png)

>[!NOTE]
>
>I diagrammi rappresentano la vista di sistema sull’attività dell’utente. Per i tentativi di inizializzazione del flusso, la decisione di accesso verrà inclusa nella risposta. Per i flussi attivi, la decisione viene restituita alla risposta heartbeat.

### 3. Due locatari. Due applicazioni. Una regola. Due ruscelli. {#twotenant-twoapp-onepolicy-twostreams}

Supponiamo ora che un nuovo tenant desideri applicare lo stesso criterio nelle proprie applicazioni:

![Due tenant. Due applicazioni. Una regola. Due flussi.](assets/onepolicy-twotenant-app-stream.png)

Poiché i due tenant sono collegati dallo stesso criterio, la situazione descritta nel caso d&#39;uso 2 è applicabile qui e **s3** può essere riprodotto in quanto si tratta del flusso più recente.

### 4. Due locatari. Tre domande. Due criteri. Due ruscelli. {#twotenants-threeapps-twopolicies-twostreams}

Supponiamo ora che il secondo tenant distribuisca una nuova applicazione e desideri definire un nuovo criterio che verrà condiviso tra **app2** e **app3**.

![Due tenant. Tre domande. Due criteri. Due flussi.](assets/twotenant-policies-streams-threeapps.png)

Al momento, i flussi attivi **s3** e **s4** sono entrambi consentiti. Per **s3**, quando viene valutato il criterio **P1**, il sistema conteggerà solo **s3** come **attività rilevante** (**s4** non è in alcun modo correlato al criterio **P1**), quindi non vi è alcuna violazione.

Il criterio **P2** è applicato a entrambi i flussi e includerà **s3** e **s4** come attività rilevante. Poiché questa attività si trova entro i limiti di due flussi, sono consentiti entrambi.

### 5. Due affittuari. Tre domande. Due criteri. Tre ruscelli. {#twotenants-threeapps-twopolicies-threestreams}

Ora si presuppone che venga eseguito un nuovo tentativo di inizializzazione del flusso utilizzando **app2**:

![Due tenant. Tre domande. Due criteri. Tre flussi.](assets/twotenants-policies-threeapps-streams.png)

L&#39;avvio di **s5** è consentito da **P1** (che consente ai flussi più recenti di assumere il controllo) ma è negato da **P2**, pertanto non verrà avviato.

Lo stesso accadrà se si tenta di avviare un flusso con l’app3: lo stesso criterio P2 negherà l’accesso a tale flusso.

![](assets/stream-init-attempted-app3.png)

Ora vediamo cosa accade se l’utente tenta di creare un nuovo flusso utilizzando l’app1:

![](assets/new-stream-with-app1.png)

L&#39;applicazione app1 non è in alcun modo correlata al criterio **P2**, quindi applicherà solo il criterio **P1**: che consente l&#39;avvio del nuovo flusso e nega quello precedente (**s3** in questo caso).
