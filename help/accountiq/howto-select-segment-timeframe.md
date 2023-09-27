---
title: Definire un segmento e un intervallo di tempo
description: Definire un segmento e un intervallo di tempo
exl-id: 86fe010d-3202-4ce2-b803-ff44f5538d7e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---

# Definire un segmento e un intervallo di tempo {#define-segment}

Tutte le attività di analisi o visualizzazione dei rapporti in [!UICONTROL Account IQ] inizia con la definizione del segmento e la selezione dell’intervallo di tempo per la valutazione. [Segmento](/help/accountiq/product-concepts.md#segmet-def) si riferisce a tutti gli abbonati o visualizzatori che soddisfano i tuoi criteri (abbonarsi a un MVPD e visualizzare canali specifici) di valutazione.

![](assets/segment-panel.png)

*Figura: Selezione di segmenti e intervalli temporali*

Nella parte superiore di tutte le pagine dei rapporti in [!UICONTROL Account IQ], è disponibile un pannello per definire i segmenti selezionando MVPD, programmatori di canale, granularità e intervallo di tempo.

## Selezione del segmento {#select-segment}

### Seleziona MVPD nel segmento {#select-segment-mvpds}

Per selezionare MVPD da **[!UICONTROL MVPDs in segment]** opzione:

1. Tocca o fai clic sul pulsante **[!UICONTROL MVPDs in segment]** a discesa.

   >[!NOTE]
   >
   >**Tutti** gli MVPD del settore sono selezionati per impostazione predefinita. Da qui, puoi selezionare una delle seguenti opzioni **Primi 10 MVPD condividendo il punteggio**, **Primi 10 MVPD per utilizzo**, **Primi 10 MVPD per account**, o singoli MVPD. Tuttavia, per selezionare singoli MVPD è necessario deselezionare **Tutti**.

1. Tocca o fai clic sui MVPD desiderati.

   È possibile rimuovere un MVPD dalla selezione deselezionandolo.

1. Tocca o fai clic su **[!UICONTROL Apply selection]** affinché la selezione diventi effettiva. In caso contrario, perderai la selezione effettuata.

   >[!NOTE]
   >
   >Se si seleziona la modalità Isolamento, non è possibile selezionare nessun altro MVPD.

### Seleziona canali nel segmento {#select-segment-channels}

Per selezionare i canali programmatori desiderati dalla **[!UICONTROL Channels in segment]** opzione:

1. Tocca o fai clic sul pulsante **[!UICONTROL Channels in segment]** a discesa.

   >[!NOTE]
   >
   >**Tutti** i canali del programmatore della società sono selezionati per impostazione predefinita. Per selezionare singoli canali o programmatori, è necessario prima deselezionare **Tutti**.

1. Tocca o fai clic sui canali o sui programmatori desiderati.

   Le voci di elenco di primo livello in **[!UICONTROL Channels in segment]** sono [programmatore](/help/accountiq/product-concepts.md#programmer-def) aziende e le voci di elenco sotto i nomi dei programmatori sono i loro [canali](/help/accountiq/product-concepts.md#channel-def). È possibile selezionare singoli canali sotto i programmatori oppure selezionare i programmatori e tutte le attività dei canali sotto quel programmatore sono incluse nei risultati del rapporto e del grafico.

   ![](assets/programmer-channels.png)


   *Figura: Programmatori e canali elencati nel selettore dei canali*

   >[!IMPORTANT]
   >
   >I risultati della selezione dei singoli canali sotto un programmatore non sono gli stessi della selezione del programmatore.
   >
   >
   >Quando selezioni singoli canali, le attività di tali canali vengono suddivise singolarmente in alcuni rapporti. Tuttavia, quando selezioni il programmatore principale di tutti questi canali, tutte le attività di tali canali vengono incluse ma non vengono suddivise singolarmente nei rapporti.

1. Tocca o fai clic su **[!UICONTROL Apply selection]** affinché la selezione diventi effettiva.

>[!NOTE]
>
>Non è possibile selezionare più di 10 elementi nei menu a discesa MVPD o programmer.

### Deseleziona MVPD e canali {#deselect-segment-mvpds-channels}

Oltre a modificare la selezione in **[!UICONTROL MVPDs in segment]** e **[!UICONTROL Channels in segment]** selettori di segmenti, puoi deselezionare gli MVPD e i canali selezionati in precedenza:

* Selezione del **[!UICONTROL Remove]** icona (![icona rimuovi](assets/remove-icon.png)) sui nomi degli MVPD e dei canali selezionati visualizzati sotto il selettore di segmenti.

* Puoi anche utilizzare **[!UICONTROL Clear Selection]** per rimuovere tutti gli MVPD o i canali precedentemente selezionati.

![](assets/segment-panel-selection.png)

*Figura: MVPD e canali selezionati nel segmento e nel pannello dell’arco temporale*

## Granularità e selezione dell’intervallo temporale {#granularity-timeframe}

Per selezionare un periodo di valutazione:

1. Seleziona la **[!UICONTROL Granularity and time frame]** selezione data.

1. Seleziona una **[!UICONTROL Week]** o **[!UICONTROL Month]** da **[!UICONTROL Aggregate By]** per impostare la granularità per la valutazione.

   ![](assets/granularity-timeframe-weekwise.png)


   *Figura: Selettore data per selezionare Granularità e intervallo di tempo*

1. Dopo aver selezionato la granularità, puoi utilizzare le frecce avanti o indietro per spostarti in avanti o indietro nel tempo.

1. Specifica un periodo di tempo trascorso (in mese o settimana in base alla granularità selezionata) per la valutazione.

1. Seleziona **[!UICONTROL Apply Selection]** per assicurarsi che la selezione abbia effetto.
