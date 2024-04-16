---
title: Utilizzare i segmenti
description: Informazioni e utilizzo dei segmenti. Scopri come creare e gestire un segmento.
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1970'
ht-degree: 0%

---

# Utilizzare i segmenti {#work-with-segments}

[Segmenti](product-concepts.md#segmet-def) sono una raccolta di account sottoscrittore che ti consentono di analizzare la condivisione delle credenziali in condizioni definite dall&#39;utente. È possibile utilizzare i segmenti per esaminare diversi set di account sottoscrittore e generare report di dati corrispondenti in tabelle e grafici. Esistono due tipi di segmenti in Account IQ:

1. **Segmento predefinito**: **Tutti gli account nelle proprietà** è un segmento predefinito nel sistema che include tutti gli account abbonato attivi senza condizioni specifiche applicate.

   >[!NOTE]
   >
   >L’utilizzo del segmento predefinito potrebbe impedire la visualizzazione di determinate tabelle come [Categorie video nel segmento](data-panels.md#video-categories-segment), [Condivisione del punteggio per canali e MVPD](data-panels.md#sharin-score-by-channels-and-mvpds), e [Distribuzione del pattern di utilizzo per categorie video](usage-patterns.md#usage-pattern-dis-video-categories). Queste tabelle possono contenere e visualizzare solo dati per un massimo di 20 righe alla volta. Le tabelle, i grafici e i rapporti rimanenti sono gli stessi per i segmenti predefiniti e personalizzati.

1. **Segmenti personalizzati**: si tratta di segmenti personalizzati che consentono di raggruppare gli account degli abbonati da categorie specifiche, ad esempio tipi di contenuto D2C, programmatori, canali e MVPD per l’analisi della condivisione delle credenziali in condizioni definite dall’utente. Ulteriori informazioni su come [creare un segmento personalizzato](#create-new-segment).

   >[!IMPORTANT]
   >
   >Tutte le procedure descritte in questa guida si basano su segmenti personalizzati. Tuttavia, i concetti restano gli stessi per i segmenti predefiniti e personalizzati.

Quando si passa al **Azioni** e seleziona la **[!UICONTROL Segments]** nel pannello a sinistra, viene visualizzato un elenco dei segmenti disponibili nel sistema. La pagina dei segmenti consente di valutare rapidamente i dettagli chiave di ciascun segmento in formato tabulare. I dettagli includono il nome del segmento, il numero di [categorie video](product-concepts.md#video-category-def), metriche, [operazioni](product-concepts.md#operation-def) utilizzando il segmento corrente, la data e l’ora dell’ultima modifica, nonché il nome dell’autore del segmento.

Con i segmenti puoi eseguire le seguenti funzioni:

* [Crea un nuovo segmento](#create-new-segment)
* [Gestire i segmenti](#manage-segments)


## Crea un nuovo segmento {#create-new-segment}

Il processo di creazione di un nuovo segmento è simile per i servizi D2C e TV Everywhere. Le categorie video saranno diverse per ogni rispettiva versione di Account IQ.

Servizi +++D2C

Per creare un segmento e analizzare il comportamento di condivisione del sottoscrittore, seleziona **[!UICONTROL Create new segment]** in alto a destra.

![Seleziona Crea nuovo segmento](assets/d2c-create-new-segment.png)

*Seleziona Crea nuovo segmento*

>[!NOTE]
>
>Le categorie video mostrate nell’immagine precedente, ad esempio **Aree geografiche** e **Tipi di contenuto** sono solo esempi. Quando accedi a Account IQ, queste etichette visualizzano le categorie video specifiche della tua azienda.

Apre un **Nuovo segmento** , che include i seguenti elementi:

![Pagina Nuovo segmento](assets/d2c-new-segment-dialog.png)

*Pagina Nuovo segmento*

**R.** Componenti del segmento **B.** Definizione del segmento **C.** Riepilogo segmenti

* **Componenti del segmento**: inventario di [categorie video](product-concepts.md##video-category-def) e le metriche calcolate utilizzate per definire un segmento.

  >[!NOTE]
  >
  >Utilizzare **[!UICONTROL Show all]** per espandere l’elenco dei componenti del segmento. Per trovare rapidamente un componente, cercarne il nome in **cerca componenti segmento** anziché scorrere l&#39;intero elenco.

* **Definizione del segmento**: area di lavoro in cui puoi trascinare e rilasciare vari componenti del segmento per creare un segmento.

* **Riepilogo segmenti**: riepilogo che valuta i conti qualificati in base ai componenti nella definizione del segmento e fornisce una breve panoramica del segmento durante il periodo di valutazione.

Per creare un segmento, effettua le seguenti operazioni:

1. Digita il nome del segmento in **Nome segmento** che saranno visibili nell’elenco dei segmenti e durante la selezione dei segmenti.
1. Digita una descrizione dettagliata del segmento in **Descrizione segmento**.
1. Ad esempio, trascina **Aree geografiche e tipi di contenuto** dai componenti del segmento nel pannello a sinistra e rilasciali nel **Aree geografiche/Tipi di contenuto** sezione all&#39;interno del **Definizione del segmento**.

   >[!NOTE]
   >
   >Puoi creare un segmento in base alle aree geografiche o ai tipi di contenuto. Visualizza i tipi di contenuto associati di un&#39;area da un menu a discesa.

   Se inizi aggiungendo una **tipo di contenuto** nel **Aree geografiche/Tipi di contenuto** , è possibile aggiungere tipi di contenuto solo come componenti successivi.

   Se inizi aggiungendo una **Regione** nel **Aree geografiche/Tipi di contenuto** viene visualizzata una finestra di dialogo di decisione.

   ![Aggiungere un componente segmento come area o relativi tipi di contenuto ](assets/d2c-segment-basis-selector.png){width="550" align="left"}

   *Aggiungi componente segmento come area geografica o finestra di dialogo dei relativi tipi di contenuto*

   Decidi se confrontare aree specifiche o un segmento in base ai tipi di contenuto associati a un’area.

   Seleziona **[!UICONTROL As a region]** per aggiungere aree al **Aree geografiche/Tipi di contenuto** sezione.

   Seleziona **[!UICONTROL As its content types]** per aggiungere tipi di contenuto di un&#39;area.

1. Trascina **Metriche** dai componenti del segmento nel pannello a sinistra e rilasciali nel **Metriche** sezione all&#39;interno del **Definizione del segmento**.

   ![Seleziona un operatore e imposta un valore per la metrica aggiunta](assets/component-metrics.png)

   *Seleziona un operatore e assegna un valore per la metrica aggiunta*

   Dopo aver aggiunto le metriche nella definizione del segmento, scegli un operatore da **[!UICONTROL Select an operator]** menu a discesa e assegna un valore tramite **[!UICONTROL Select an option]**.

   Regola i valori per alcune metriche utilizzando la freccia verso l&#39;alto per aumentare e la freccia verso il basso per diminuire.

1. Trascina **Metriche calcolate** dai componenti del segmento nel pannello a sinistra e rilasciali nel **Metriche calcolate** sezione all&#39;interno del **Definizione del segmento**.

   ![Seleziona un operatore e imposta un valore per la metrica calcolata aggiunta](assets/component-calculated-metrics.png)

   *Seleziona un operatore e assegna un valore per la metrica calcolata aggiunta*

   Dopo l’aggiunta delle metriche calcolate nella definizione del segmento, **[!UICONTROL Select an operator]** dal menu a discesa e assegna un valore utilizzando **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Tutte le metriche e le metriche calcolate rilasciate nella definizione del segmento sono accompagnate da operatori appropriati per assegnare valori alle rispettive metriche e metriche calcolate.

1. Rivedi i dettagli del segmento in **Riepilogo segmenti** per decidere le modifiche da implementare nel segmento.
1. Seleziona **[!UICONTROL Last week]** o **[!UICONTROL Last month]** dal **Periodo di valutazione** menu a discesa per stimare i valori di riepilogo per la settimana o il mese trascorso.
1. Seleziona **[!UICONTROL Update estimation]** per calcolare il numero di conti qualificati stimati nel segmento corrente in base al periodo di valutazione selezionato.
1. Seleziona **[!UICONTROL Save segment]**.

Il segmento creato è ora disponibile nell’elenco dei segmenti.

+++

+++TV Ovunque

Per creare un segmento e analizzare il comportamento di condivisione del sottoscrittore, seleziona **[!UICONTROL Create new segment]** in alto a destra.

![Seleziona Crea nuovo segmento](assets/create-new-segment.png)

*Seleziona Crea nuovo segmento*

Apre un **Nuovo segmento** , che include i seguenti elementi:

![Pagina Nuovo segmento](assets/new-segment-dialog.png)

*Pagina Nuovo segmento*

**R.** Componenti del segmento **B.** Definizione del segmento **C.** Riepilogo segmenti

* **Componenti del segmento**: inventario di programmatori e canali, MVPD, metriche e metriche calcolate utilizzati per definire un segmento.

  >[!NOTE]
  >
  >Utilizzare **[!UICONTROL Show all]** per espandere l’elenco dei componenti del segmento. Per trovare rapidamente un componente, cercarne il nome in **cerca componenti segmento** anziché scorrere l&#39;intero elenco.

* **Definizione del segmento**: area di lavoro in cui puoi trascinare e rilasciare vari componenti del segmento per creare un segmento.

* **Riepilogo segmenti**: riepilogo che valuta i conti qualificati in base ai componenti nella definizione del segmento e fornisce una breve panoramica del segmento durante il periodo di valutazione.

Per creare un segmento, effettua le seguenti operazioni:

1. Digita il nome del segmento in **Nome segmento** che saranno visibili nell’elenco dei segmenti e durante la selezione dei segmenti.
1. Digita una descrizione dettagliata del segmento in **Descrizione segmento**.
1. Trascina **Programmatori e canali** dai componenti del segmento nel pannello a sinistra e rilasciali nel **Programmatori/Canali** sezione all&#39;interno del **Definizione del segmento**.

   >[!NOTE]
   >
   >Puoi creare un segmento basato su programmatori o canali. Visualizza i canali associati con un programmatore da un menu a discesa.

   Se inizi aggiungendo una **Canale** nel **Programmatori/Canali** , è possibile aggiungere canali solo come componenti successivi.

   Se inizi aggiungendo una **Programmatore** nel **Programmatori/Canali** viene visualizzata una finestra di dialogo di decisione.

   ![Aggiungere un componente segmento come programmatore o relativi canali ](assets/segment-basis-selector.png){width="550" align="left"}


   *Aggiungere un componente segmento come programmatore o finestra di dialogo dei suoi canali*

   Decidi se confrontare programmatori specifici o un segmento in base ai canali associati a un programmatore.

   Seleziona **[!UICONTROL As a programmer]** per aggiungere programmatori al **Programmatori/Canali** sezione.

   Seleziona **[!UICONTROL As its channels]** per aggiungere tutti i canali di un programmatore.

1. Trascina **MVPD** dai componenti del segmento nel pannello a sinistra e rilasciali nel **MVPD** sezione all&#39;interno del **Definizione del segmento**.

   >[!NOTE]
   >
   >Quando accedi come programmatore, un MVPD denominato **xfinity** viene visualizzato come opzione autonoma nella finestra di dialogo **MVPD** sezione. Non è possibile combinarlo con altri MVPD.

1. Trascina **Metriche** dai componenti del segmento nel pannello a sinistra e rilasciali nel **Metriche** sezione all&#39;interno del **Definizione del segmento**.

   ![Seleziona un operatore e imposta un valore per la metrica aggiunta](assets/component-metrics.png)

   *Seleziona un operatore e assegna un valore per la metrica aggiunta*

   Dopo aver aggiunto le metriche nella definizione del segmento, scegli un operatore da **[!UICONTROL Select an operator]** menu a discesa e assegna un valore tramite **[!UICONTROL Select an option]**.

   Regola i valori per alcune metriche utilizzando la freccia verso l&#39;alto per aumentare e la freccia verso il basso per diminuire.

1. Trascina **Metriche calcolate** dai componenti del segmento nel pannello a sinistra e rilasciali nel **Metriche calcolate** sezione all&#39;interno del **Definizione del segmento**.

   ![Seleziona un operatore e imposta un valore per la metrica calcolata aggiunta](assets/component-calculated-metrics.png)

   *Seleziona un operatore e assegna un valore per la metrica calcolata aggiunta*

   Dopo l’aggiunta delle metriche calcolate nella definizione del segmento, **[!UICONTROL Select an operator]** dal menu a discesa e assegna un valore utilizzando **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Tutte le metriche e le metriche calcolate rilasciate nella definizione del segmento sono accompagnate da operatori appropriati per assegnare valori alle rispettive metriche e metriche calcolate.

1. Rivedi i dettagli del segmento in **Riepilogo segmenti** per decidere le modifiche da implementare nel segmento.
1. Seleziona **[!UICONTROL Last week]** o **[!UICONTROL Last month]** dal **Periodo di valutazione** menu a discesa per stimare i valori di riepilogo per la settimana o il mese trascorso.
1. Seleziona **[!UICONTROL Update estimation]** per calcolare il numero di conti qualificati stimati nel segmento corrente in base al periodo di valutazione selezionato.
1. Seleziona **[!UICONTROL Save segment]**.

Il segmento creato è ora disponibile nell’elenco dei segmenti.
+++

## Gestire i segmenti {#manage-segments}

Puoi selezionare un segmento dall’elenco dei segmenti ed eseguire le azioni seguenti:

* [Modificare un segmento](#edit-segment)
* [Duplicare un segmento](#duplicate-segment)
* [Eliminare un segmento](#delete-segment)

![Modificare, duplicare o eliminare un segmento](assets/manage-segments-list.png)

*Seleziona un segmento da modificare, duplicare o eliminare*

**R.** [Categorie video](product-concepts.md#video-category-def) **B.** [Segmento predefinito](#work-with-segments)

>[!NOTE]
>
>Le categorie video mostrate in questa sezione, ad esempio **MVPD**, **Programmatori**, e **Canali** rappresentano le etichette utilizzate nella versione TV Everywhere di Account IQ. Se hai effettuato l’accesso come servizio D2C, queste etichette visualizzano le categorie video specifiche della tua azienda.

Non è possibile modificare, duplicare o eliminare il segmento predefinito denominato **Tutti gli account nelle proprietà**.

### Modificare un segmento {#edit-segment}

1. Accedi a **[!UICONTROL Segments]** scheda in **Azioni** nel pannello a sinistra per visualizzare un elenco di segmenti.
1. Seleziona un segmento da modificare.
1. Seleziona **[!UICONTROL Edit]**.
1. Modifica i dettagli del segmento, come il nome del segmento, la descrizione o i componenti in **Definizione del segmento**.

   >[!TIP]
   >
   >Utilizzare **[!UICONTROL Clear all]** per rimuovere contemporaneamente tutti i componenti del segmento all’interno di ogni sezione nella definizione del segmento. In alternativa, selezionate il pulsante croce per rimuovere singoli elementi.

   ![Cancella tutti i componenti del segmento in ogni sezione nella definizione del segmento ](assets/clear-all-components.png)

   *Seleziona Cancella tutto per rimuovere tutti i componenti di segmento contemporaneamente*

1. Seleziona una **[!UICONTROL Update segment]** per aggiornare il segmento esistente o **[!UICONTROL Save as new segment]** per creare un nuovo segmento con le modifiche.

   >[!NOTE]
   >
   >Non è consentito aggiornare segmenti attualmente in fase di operazioni. Il salvataggio delle modifiche come nuovo segmento è l’unica opzione per i segmenti con operazioni in corso.

### Duplicare un segmento {#duplicate-segment}

1. Accedi a **[!UICONTROL Segments]** scheda in **Azioni** nel pannello a sinistra per visualizzare un elenco di segmenti.
1. Seleziona un segmento da duplicare.
1. Seleziona **[!UICONTROL Duplicate]**.

Una copia del segmento selezionato viene generata e posizionata alla fine dell’elenco dei segmenti. Puoi modificare i dettagli necessari nel segmento duplicato e quindi aggiornare il segmento duplicato o salvarlo come nuovo segmento.

### Eliminare un segmento {#delete-segment}

1. Accedi a **[!UICONTROL Segments]** scheda in **Azioni** nel pannello a sinistra per visualizzare un elenco di segmenti.
1. Seleziona un segmento da rimuovere.

   Seleziona più segmenti per eliminarli in una singola operazione. È inoltre possibile selezionare una casella di controllo a sinistra della **Nome segmento** per eliminare tutti i segmenti contemporaneamente.

   >[!NOTE]
   >
   > Puoi eliminare più di un segmento o tutti i segmenti solo se nessuno di essi è utilizzato dalle operazioni. Inoltre, l’eliminazione del segmento predefinito denominato **Tutti gli account nelle proprietà** non è consentito. Quando tenti di eliminare tutti i segmenti contemporaneamente, l’opzione non viene selezionata.

   ![Eliminare più di un segmento](assets/delete-more-than-one-segment.png)

   *Seleziona più segmenti per eliminare più segmenti*

1. Seleziona **[!UICONTROL Delete]**.
1. Conferma a **[!UICONTROL Delete]** nella finestra di dialogo per rimuovere definitivamente il segmento.

   >[!NOTE]
   >
   >Il segmento viene eliminato definitivamente dal sistema e non puoi annullare questa azione.


