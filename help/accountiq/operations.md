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

Dopo aver analizzato i pattern di utilizzo dell’abbonato e identificato le istanze di condivisione delle password per un segmento selezionato utilizzando [!DNL Account IQ] analytics, puoi intraprendere azioni mirate tramite procedure mirate denominate operazioni in [!DNL Account IQ].

**Operazioni** consente di monitorare e gestire in modo efficace la condivisione delle credenziali verso un gruppo di account per mitigare la condivisione delle password e migliorare l’esperienza per gli abbonati più importanti.

È possibile applicare azioni a un [segmento](/help/accountiq/product-concepts.md#segment-def) per gestire la condivisione di password all&#39;interno di un [intervallo di tempo](/help/accountiq/product-concepts.md#time-interval-def) e pianificare l&#39;esecuzione dell&#39;operazione in una data futura. Queste azioni includono restrizioni per ridurre al minimo la condivisione di password o semplificare i vincoli sugli account non condivisi.

Utilizzando le operazioni, non solo puoi specificare le azioni e il relativo ambito, ma anche misurarne i risultati.

Valutando i risultati, puoi perfezionare la tua strategia per ottimizzare gli effetti, convertendo i mutuatari, mitigando la condivisione delle credenziali o riducendo l’abbandono.

È possibile eseguire varie funzioni con le operazioni seguenti:

* [Visualizzare i rapporti sulle operazioni](#operation-reports)
* [Crea una nuova operazione](#create-new-operation)
* [Interrompi operazione](#stop-operation)

## Visualizzare i rapporti sulle operazioni {#operation-reports}

È possibile esaminare gli effetti di un&#39;operazione tramite i rapporti sulle operazioni. Per visualizzare il rapporto delle operazioni, selezionare **Operazioni** scheda in **Azioni** nel pannello a sinistra dell’applicazione Account IQ. Viene visualizzato un elenco delle operazioni disponibili nel sistema. Puoi accedere ai dettagli chiave di ciascuna operazione in formato tabulare. I dettagli includono:

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

*Elenco e dettagli delle operazioni esistenti nell’Account IQ*

Seleziona il **Nome operazione** dall’elenco delle operazioni. Vengono visualizzati i seguenti rapporti:

### Prestazioni operative {#operation-performance}

Le prestazioni dell&#39;operazione forniscono una lettura della riga superiore che riepiloga il numero di conti interessati, l&#39;avanzamento dell&#39;operazione e il punteggio di condivisione complessivo dei conti nel segmento durante l&#39;operazione [periodo di valutazione](/help/accountiq/product-concepts.md#evaluation-period-def).

![Rapporto prestazioni operazione](assets/operation-performance.png)

*Rapporto prestazioni operazione*

**R.** Account interessati **B.** Avanzamento operazione **C.** Punteggio di condivisione complessivo

#### Account interessati {#impacted-accounts}

Questo numero indica il numero di account sottoscrittore interessati dall&#39;azione eseguita durante il periodo di valutazione dell&#39;operazione.

#### Avanzamento operazione {#operation-progress}

Questo indicatore mostra il numero di giorni e la percentuale di completamento dell&#39;operazione rispetto al programma pianificato.

#### Punteggio di condivisione complessivo {#overall-sharing-score}

Questo grafico a linee rappresenta [punteggio di condivisione complessivo](/help/accountiq/data-panels.md#overall-sharing-score), che include il livello di condivisione e l&#39;utilizzo da account condivisi in ogni settimana durante il periodo di valutazione dell&#39;operazione.

### Impatto operazione: conti nel segmento {#impact-accounts}

Questo rapporto viene visualizzato come un grafico a colonne sovrapposte che illustra l’impatto di un’operazione nel tempo.

![Impatto delle operazioni sui conti nel grafico dei segmenti](assets/accounts-in-segment.png)

*Impatto delle operazioni sui conti nel grafico dei segmenti*

L’asse x rappresenta il [periodo di valutazione](/help/accountiq/product-concepts.md#evaluation-period-def), mentre l&#39;asse y indica lo stato dei conti nel segmento dell&#39;operazione. Ogni barra del grafico è divisa in tre colori:

* Il rosa rappresenta il numero di account che soddisfano le condizioni del segmento utilizzate in questa operazione.

* Blu rappresenta il numero di account attivi che erano originariamente nel segmento ma che non soddisfacevano le condizioni del segmento durante ogni settimana o mese nel [periodo di valutazione](/help/accountiq/product-concepts.md#evaluation-period-def).

* Grigio rappresenta gli account inattivi durante il periodo di valutazione.

>[!NOTE]
>
>La prima barra rosa rappresenta il numero di conti che soddisfano le condizioni del segmento di operazione all&#39;inizio del periodo di valutazione.

Nel tempo, il grafico illustra i cambiamenti nel comportamento dell’account rispetto ai criteri originali (ad esempio, se la probabilità di condivisione è superiore a 90 e se si utilizzano più di 5 dispositivi, questi sono diventati inattivi).

### Impatto operazione: metriche account condivisi {#impact-shared-accounts}

Le metriche degli account condivisi forniscono una panoramica delle richieste di livello di condivisione e riproduzione da parte degli account sottoscrittori nel segmento dell&#39;operazione durante il [periodo di valutazione](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Livello di condivisione {#share-level}

Questo grafico a linee rappresenta [livello di condivisione](/help/accountiq/data-panels.md#sharing-level) ogni settimana nel periodo di valutazione dell&#39;operazione.

![Grafico a linee a livello di condivisione](assets/share-level.png){width="550" align="left"}

*Grafico a linee a livello di condivisione*

#### Numero di richieste di riproduzione {#play-requests}

Questo grafico a linee rappresenta [riproduci richieste](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) ogni settimana del periodo di valutazione dell&#39;operazione.

![Grafico a linee del numero di richieste di riproduzione](assets/number-play-requests.png){width="550" align="left"}

*Grafico a linee del numero di richieste di riproduzione*

### Impatto sulle operazioni: metriche di utilizzo generali {#impact-general-usage}

Le metriche di utilizzo generali forniscono una panoramica del numero medio di dispositivi, IP e posizioni nel segmento dell’operazione durante il [periodo di valutazione](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Numero di dispositivi {#devices}

Questo grafico a linee rappresenta la media [numero di dispositivi](/help/accountiq/general-usage-reports.md#devices-week-account) ogni settimana del periodo di valutazione dell&#39;operazione.

![Grafico a linee del numero di dispositivi](assets/number-devices.png){width="550" align="left"}

*Grafico a linee del numero di dispositivi*

#### Numero di IP e posizioni {#IPs-locations}

Questo grafico a linee rappresenta la media [numero di IP](/help/accountiq/general-usage-reports.md#ip-week-account) e [posizioni](/help/accountiq/general-usage-reports.md#locations-week-account) ogni settimana del periodo di valutazione dell&#39;operazione.

![Grafico a linee Numero di IP e posizioni](assets/number-ips-locations.png){width="550" align="left"}

*Grafico a linee Numero di IP e posizioni*

Per chiudere il report e tornare al **Operazioni** pagina, seleziona **Operazioni** scheda in **Azioni** nel pannello a sinistra.

## Crea nuova operazione {#create-new-operation}

Quando si passa al **Operazioni** scheda in **Azioni** nel pannello a sinistra, seleziona **Crea nuova operazione** nella parte superiore della **Operazioni** pagina.

Per creare una nuova operazione, attieniti alle istruzioni riportate nelle sezioni seguenti:

* [Dettagli operazione](#operation-details)
* [Segmento](#segment)
* [Azione](#action)
* [Pianificazione](#schedule)

### Dettagli operazione {#operation-details}

In questa sezione digitare il nome dell&#39;operazione in **Nome operazione**.

>[!TIP]
>
>Descrivere lo scopo dell&#39;operazione o la natura dell&#39;azione **nome operazione** per una rapida identificazione. Opzione per **Aggiungi descrizione e tag** sarà disponibile nelle versioni future.

![Aggiungi nome operazione nei dettagli operazione](assets/operation-details.png)

*Aggiungi nome operazione*

### Segmento {#segment}

In questa sezione, fai clic su **Seleziona segmento** e scegliere un segmento per il quale si desidera utilizzare questa operazione. Scopri [come selezionare un segmento](/help/accountiq/segments-timeinterval.md#segment-selection).

Dopo aver selezionato un segmento, utilizza <img alt= "espandi riepilogo segmenti" src="./assets/expand-segment-summary.svg" width="25"> per visualizzare il riepilogo dettagliato dei segmenti. Ulteriori informazioni su [riepilogo segmenti](segments-timeinterval.md#segment-summary).

![Seleziona segmento e intervallo di tempo](assets/select-segment-timeinterval.png)

*Seleziona segmento e intervallo di tempo*

>[!NOTE]
>
>Il [categorie video](product-concepts.md#video-category-def) mostrato nell’immagine precedente, ad esempio **MVPD**, **Programmatori**, e **Canali** rappresentano le etichette utilizzate nella versione TV Everywhere di Account IQ. Se hai effettuato l’accesso come servizio D2C, queste etichette visualizzano le categorie video specifiche della tua azienda.

Se necessario, utilizza <img alt= "modifica segmento" src="./assets/edit-segment.svg" width="25"> per modificare il segmento selezionato o  <img alt= "crea nuovo segmento" src="./assets/create-new-segment.svg" width="25"> per creare un nuovo segmento. Per ulteriori informazioni, consulta le istruzioni per [creazione di un nuovo segmento](work-with-segments.md#create-new-segment) o [modifica di un segmento](work-with-segments.md#edit-segment).

>[!IMPORTANT]
>
>**Tipo di segmento** denominato **[!UICONTROL Fixed number of accounts]** è attualmente selezionato per impostazione predefinita. Opzione da selezionare **[!UICONTROL Variable number of accounts]** sarà disponibile nelle prossime versioni.

Seleziona **Granularità e intervallo di tempo** per monitorare l&#39;operazione durante un periodo specifico. Ulteriori informazioni su [come selezionare granularità e intervallo di tempo](/help/accountiq/segments-timeinterval.md#granularity-timeinterval).

### Azione {#action}

In questa sezione, scegli un **Azione** desideri eseguire sul segmento selezionato dal menu a discesa.

![Seleziona il tipo di azione](assets/apply-actions.png)

*Seleziona il tipo di azione*

Sono disponibili due opzioni:

* Seleziona **Criterio CM** per il sistema di monitoraggio della concorrenza integrato con Account IQ.

* Seleziona **Azioni esterne** per creare ed elaborare flussi di lavoro esterni all’Account IQ e non integrati con il sistema Account IQ.

>[!NOTE]
>
>Le azioni esterne non sempre sono direttamente correlate alla condivisione delle password, ma possono comunque avere un impatto su di essa, ad esempio il lancio di una nuova stagione.

### Pianificazione {#schedule}

In questa sezione, seleziona la **Data di inizio** e **Data di fine** dal selettore data per impostare l’attivazione per l’operazione.

>[!IMPORTANT]
>
>Attualmente, l&#39;attivazione predefinita **Data di inizio** e **Data di fine** sono impostati su **In data**. Opzione da selezionare **Quando viene soddisfatta una condizione** e **Manualmente** sarà disponibile nelle prossime versioni.

>[!NOTE]
>
>Assicurati che la data di inizio e la data di fine siano allineate alla granularità selezionata per la valutazione in **Passaggio 4**.

* Se hai optato per la granularità aggregata per settimane, seleziona le date di inizio e fine in settimane (ad esempio, Settimana 10).
* Se hai optato per la granularità aggregata per mesi, seleziona le date di inizio e fine in mesi.

![Seleziona data di inizio e data di fine dal selettore data](assets/add-schedule.png)

*Seleziona Data di inizio e Data di fine dal selettore data*

**R.** Selettore data di inizio **B.** Selettore data di fine

>[!NOTE]
>
>Il **Data di inizio** deve essere successivo sia al periodo di valutazione che alla data corrente, mentre **Data di fine** deve essere successiva alla data di inizio e alla data corrente per pianificare ed eseguire operazioni nel periodo futuro.

Seleziona **Operazione Salva** nella parte superiore della **Operazioni** per elaborare una nuova operazione.

## Interrompi operazione {#stop-operation}

È possibile interrompere solo le operazioni attualmente in corso **In esecuzione** stato. Per interrompere un&#39;operazione esistente, eseguire la procedura seguente:

1. Accedi a **Operazioni** scheda in **Azioni** nella navigazione a sinistra dell’applicazione Account IQ.
1. Seleziona **Opzioni** dell&#39;operazione che si desidera interrompere.

   ![Seleziona il menu delle opzioni per interrompere l’operazione](assets/stop-operation.png)

   *Selezionare il menu Opzioni per interrompere l&#39;operazione*

1. Seleziona **Interrompi**.



