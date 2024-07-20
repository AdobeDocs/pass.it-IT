---
title: Operazioni nell’account IQ
description: Le operazioni in Account IQ comportano l’esecuzione di azioni per eseguire automazioni e operazioni in blocco sugli account degli abbonati e tenere traccia dei loro effetti.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# Operazioni {#operations-tab-next-steps}

Dopo aver analizzato i pattern di utilizzo dell&#39;utente iscritto e identificato le istanze di condivisione password per un segmento selezionato utilizzando [!DNL Account IQ] Analytics, è possibile eseguire azioni mirate tramite procedure mirate denominate operazioni in [!DNL Account IQ].

**Operazioni** ti consentono di monitorare e gestire in modo efficace la condivisione delle credenziali verso un gruppo di account per mitigare la condivisione delle password e migliorare l&#39;esperienza per gli abbonati valutati.

Puoi applicare azioni a un [segmento](/help/accountiq/product-concepts.md#segment-def) definito per gestire la condivisione password entro un [intervallo di tempo](/help/accountiq/product-concepts.md#time-interval-def) specifico e pianificare l&#39;esecuzione dell&#39;operazione in una data futura. Queste azioni includono restrizioni per ridurre al minimo la condivisione di password o semplificare i vincoli sugli account non condivisi.

Utilizzando le operazioni, non solo puoi specificare le azioni e il relativo ambito, ma anche misurarne i risultati.

Valutando i risultati, puoi perfezionare la tua strategia per ottimizzare gli effetti, convertendo i mutuatari, mitigando la condivisione delle credenziali o riducendo l’abbandono.

È possibile eseguire varie funzioni con le operazioni seguenti:

* [Visualizzare i rapporti sulle operazioni](#operation-reports)
* [Crea una nuova operazione](#create-new-operation)
* [Interrompi operazione](#stop-operation)

## Visualizzare i rapporti sulle operazioni {#operation-reports}

È possibile esaminare gli effetti di un&#39;operazione tramite i rapporti sulle operazioni. Per visualizzare il report delle operazioni, seleziona la scheda **Operazioni** in **Azioni** nel pannello a sinistra dell&#39;applicazione Account IQ. Viene visualizzato un elenco delle operazioni disponibili nel sistema. Puoi accedere ai dettagli chiave di ciascuna operazione in formato tabulare. I dettagli includono:

* Nome dell’operazione
* Stato corrente (ad esempio Pianificato, In esecuzione, Terminato, Errore o Interrotto)
* Percentuale di completamento avanzamento
* Pubblico di destinazione o segmento a cui viene applicata l’operazione
* Tipo di azione selezionata per l&#39;operazione
* Data di inizio dell’operazione
* Data di fine dell’operazione
* Data di creazione dell&#39;operazione
* Data dell’ultima modifica dell’operazione

![](assets/operations-page.png)

*Elenco e dettagli delle operazioni esistenti in Account IQ*

Selezionare **Nome operazione** dall&#39;elenco delle operazioni. Vengono visualizzati i seguenti rapporti:

### Prestazioni operative {#operation-performance}

Le prestazioni dell&#39;operazione forniscono una lettura di prima riga che riepiloga il numero di account interessati, l&#39;avanzamento dell&#39;operazione e il punteggio di condivisione complessivo degli account nel segmento durante il [periodo di valutazione](/help/accountiq/product-concepts.md#evaluation-period-def) dell&#39;operazione.

![Rapporto prestazioni operazione](assets/operation-performance.png)

*Rapporto prestazioni operazione*

**A.** Account interessati **B.** Avanzamento operazione **C.** Punteggio complessivo condivisione

#### Account interessati {#impacted-accounts}

Questo numero indica il numero di account sottoscrittore interessati dall&#39;azione eseguita durante il periodo di valutazione dell&#39;operazione.

#### Avanzamento operazione {#operation-progress}

Questo indicatore mostra il numero di giorni e la percentuale di completamento dell&#39;operazione rispetto al programma pianificato.

#### Punteggio di condivisione complessivo {#overall-sharing-score}

Questo grafico a linee rappresenta il [punteggio di condivisione complessivo](/help/accountiq/data-panels.md#overall-sharing-score), che include il livello di condivisione e l&#39;utilizzo degli account condivisi in ogni settimana durante il periodo di valutazione dell&#39;operazione.

### Impatto operazione: conti nel segmento {#impact-accounts}

Questo rapporto viene visualizzato come un grafico a colonne sovrapposte che illustra l’impatto di un’operazione nel tempo.

![Impatto dell&#39;operazione sugli account nel grafico segmenti](assets/accounts-in-segment.png)

*Impatto dell&#39;operazione sugli account nel grafico segmenti*

L&#39;asse x rappresenta il [periodo di valutazione](/help/accountiq/product-concepts.md#evaluation-period-def) dell&#39;operazione, mentre l&#39;asse y indica lo stato dei conti nel segmento dell&#39;operazione. Ogni barra del grafico è divisa in tre colori:

* Il rosa rappresenta il numero di account che soddisfano le condizioni del segmento utilizzate in questa operazione.

* Blu rappresenta il numero di account attivi che originariamente erano nel segmento ma che non soddisfacevano le condizioni del segmento durante ogni settimana o mese nel [periodo di valutazione](/help/accountiq/product-concepts.md#evaluation-period-def) dell&#39;operazione.

* Grigio rappresenta gli account inattivi durante il periodo di valutazione.

>[!NOTE]
>
>La prima barra rosa rappresenta il numero di conti che soddisfano le condizioni del segmento di operazione all&#39;inizio del periodo di valutazione.

Nel tempo, il grafico illustra i cambiamenti nel comportamento dell’account rispetto ai criteri originali (ad esempio, se la probabilità di condivisione è superiore a 90 e se si utilizzano più di 5 dispositivi, questi sono diventati inattivi).

### Impatto operazione: metriche account condivisi {#impact-shared-accounts}

Le metriche degli account condivisi forniscono una panoramica delle richieste di livello di condivisione e riproduzione da parte degli account sottoscrittori nel segmento dell&#39;operazione durante il [periodo di valutazione](/help/accountiq/product-concepts.md#evaluation-period-def) dell&#39;operazione.

#### Livello di condivisione {#share-level}

Questo grafico a linee rappresenta il [livello di condivisione](/help/accountiq/data-panels.md#sharing-level) ogni settimana nel periodo di valutazione dell&#39;operazione.

![Grafico a linee a livello di condivisione](assets/share-level.png){width="550" align="left"}

*Grafico a linee a livello di condivisione*

#### Numero di richieste di riproduzione {#play-requests}

Questo grafico a linee rappresenta le [richieste di riproduzione](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) ogni settimana nel periodo di valutazione dell&#39;operazione.

![Grafico a linee del numero di richieste di riproduzione](assets/number-play-requests.png){width="550" align="left"}

*Grafico a linee del numero di richieste di riproduzione*

### Impatto sulle operazioni: metriche di utilizzo generali {#impact-general-usage}

Le metriche di utilizzo generali forniscono una panoramica del numero medio di dispositivi, IP e posizioni nel segmento dell&#39;operazione durante il [periodo di valutazione](/help/accountiq/product-concepts.md#evaluation-period-def) dell&#39;operazione.

#### Numero di dispositivi {#devices}

Questo grafico a linee rappresenta il numero medio di [dispositivi](/help/accountiq/general-usage-reports.md#devices-week-account) ogni settimana nel periodo di valutazione dell&#39;operazione.

![Grafico a linee del numero di dispositivi](assets/number-devices.png){width="550" align="left"}

*Grafico a linee del numero di dispositivi*

#### Numero di IP e posizioni {#IPs-locations}

Questo grafico a linee rappresenta la media [del numero di IP](/help/accountiq/general-usage-reports.md#ip-week-account) e [posizioni](/help/accountiq/general-usage-reports.md#locations-week-account) ogni settimana nel periodo di valutazione dell&#39;operazione.

![Grafico a linee Numero di IP e posizioni](assets/number-ips-locations.png){width="550" align="left"}

*Grafico a linee Numero di IP e posizioni*

Per chiudere il report e tornare alla pagina principale **Operazioni**, seleziona la scheda **Operazioni** in **Azioni** nel pannello a sinistra.

## Crea nuova operazione {#create-new-operation}

Quando accedi alla scheda **Operazioni** in **Azioni** nel pannello a sinistra, seleziona **Crea nuova operazione** nella parte superiore della pagina **Operazioni**.

Per creare una nuova operazione, attieniti alle istruzioni riportate nelle sezioni seguenti:

* [Dettagli operazione](#operation-details)
* [Segmento](#segment)
* [Azione](#action)
* [Pianificazione](#schedule)

### Dettagli operazione {#operation-details}

In questa sezione digitare il nome dell&#39;operazione in **Nome operazione**.

>[!TIP]
>
>Descrivi lo scopo dell&#39;operazione o la natura dell&#39;azione in **nome operazione** per una rapida identificazione. L&#39;opzione per **Aggiungere descrizione e tag** sarà disponibile nelle versioni future.

![Aggiungi nome operazione nei dettagli operazione](assets/operation-details.png)

*Aggiungi nome operazione*

### Segmento {#segment}

In questa sezione, fare clic su **Seleziona segmento** e scegliere un segmento in cui si desidera utilizzare questa operazione. Scopri [come selezionare un segmento](/help/accountiq/segments-timeinterval.md#segment-selection).

Dopo aver selezionato un segmento, utilizza Icona <img alt= "espandi riepilogo segmenti" src="./assets/expand-segment-summary.svg" width="25"> per visualizzare il riepilogo dettagliato dei segmenti. Ulteriori informazioni su [riepilogo segmenti](segments-timeinterval.md#segment-summary).

![Seleziona segmento e intervallo di tempo](assets/select-segment-timeinterval.png)

*Seleziona segmento e intervallo di tempo*

>[!NOTE]
>
>Le [categorie video](product-concepts.md#video-category-def) visualizzate nell&#39;immagine precedente, ad esempio **MVPDs**, **Programmer** e **Canali**, rappresentano le etichette utilizzate nella versione TV Everywhere di Account IQ. Se hai effettuato l’accesso come servizio D2C, queste etichette visualizzano le categorie video specifiche della tua azienda.

Se necessario, utilizza Icona <img alt= "modifica segmento" src="./assets/edit-segment.svg" width="25"> per modificare il segmento selezionato o  Icona <img alt= "crea nuovo segmento" src="./assets/create-new-segment.svg" width="25"> per creare un nuovo segmento. Per ulteriori dettagli, consulta le istruzioni per [creare un nuovo segmento](work-with-segments.md#create-new-segment) o [modificare un segmento](work-with-segments.md#edit-segment).

>[!IMPORTANT]
>
>**Il tipo di segmento** denominato **[!UICONTROL Fixed number of accounts]** è attualmente selezionato per impostazione predefinita. L&#39;opzione per selezionare **[!UICONTROL Variable number of accounts]** sarà disponibile nelle prossime versioni.

Seleziona **Granularità e intervallo di tempo** per monitorare l&#39;operazione durante un periodo specifico. Ulteriori informazioni su [come selezionare granularità e intervallo di tempo](/help/accountiq/segments-timeinterval.md#granularity-timeinterval).

### Azione {#action}

In questa sezione, scegli l&#39;**azione** che desideri eseguire sul segmento selezionato dal menu a discesa.

![Selezionare il tipo di azione](assets/apply-actions.png)

*Selezionare il tipo di azione*

Sono disponibili due opzioni:

* Selezionare **Criteri CM** per il sistema di monitoraggio della concorrenza integrato con Account IQ.

* Seleziona **Azioni esterne** per creare ed elaborare flussi di lavoro esterni ad Account IQ e non integrati con Account IQ.

>[!NOTE]
>
>Le azioni esterne non sempre sono direttamente correlate alla condivisione delle password, ma possono comunque avere un impatto su di essa, ad esempio il lancio di una nuova stagione.

### Pianificazione {#schedule}

In questa sezione, seleziona **Data inizio** e **Data fine** dal selettore data per impostare l&#39;attivazione per l&#39;operazione.

>[!IMPORTANT]
>
>Attualmente, la data di attivazione predefinita **Data di inizio** e la **Data di fine** sono impostate su **Data di inizio**. L&#39;opzione per selezionare **Quando una condizione viene soddisfatta** e **Manualmente** sarà disponibile nelle prossime versioni.

>[!NOTE]
>
>Assicurati che la data di inizio e la data di fine siano allineate con la granularità selezionata per la valutazione nel **passaggio 4**.

* Se hai optato per la granularità aggregata per settimane, seleziona le date di inizio e fine in settimane (ad esempio, Settimana 10).
* Se hai optato per la granularità aggregata per mesi, seleziona le date di inizio e fine in mesi.

![Seleziona data di inizio e data di fine dal selettore data](assets/add-schedule.png)

*Seleziona data di inizio e data di fine dal selettore data*

**A.** Selezione data di inizio **B.** Selezione data di fine

>[!NOTE]
>
>La **data di inizio** deve essere successiva sia al periodo di valutazione che alla data corrente, mentre la **data di fine** deve essere successiva alla data di inizio e alla data corrente per pianificare ed eseguire operazioni nel periodo futuro.

Seleziona **Salva operazione** nella parte superiore della pagina **Operazioni** per elaborare una nuova operazione.

## Interrompi operazione {#stop-operation}

È possibile interrompere solo le operazioni attualmente in stato **In esecuzione**. Per interrompere un&#39;operazione esistente, eseguire la procedura seguente:

1. Passa alla scheda **Operazioni** in **Azioni** nell&#39;area di navigazione a sinistra dell&#39;applicazione Account IQ.
1. Selezionare il menu **Opzioni** dell&#39;operazione che si desidera interrompere.

   ![Selezionare il menu Opzioni per interrompere l&#39;operazione](assets/stop-operation.png)

   *Selezionare il menu Opzioni per interrompere l&#39;operazione*

1. Seleziona **Interrompi**.



