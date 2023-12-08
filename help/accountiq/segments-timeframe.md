---
title: Segmenti di Abbonamento e intervallo di tempo
description: Definisci le coorti o seleziona i segmenti di abbonati per misurare le possibilità e i pattern di condivisione dell’account da parte dei tuoi visualizzatori di canale, per utilizzare strumenti grafici e rapporti in Account IQ.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: 6b790728f3d6a8eed5dfc0f8b3d0dad283af6418
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---


# Segmenti di Abbonamento e intervallo di tempo {#cohorts-segments}


Quando accedi a Account IQ, il pannello di avvio del segmento nella parte superiore ti consente di specificare l’abbonato [segmento](/help/accountiq/product-concepts.md#segment-segmet-def). Questo aiuta a filtrare i risultati quando si visualizzano i rapporti sul comportamento e i modelli di condivisione degli abbonati. Un segmento predefinito denominato - Tutti gli account nelle proprietà è già selezionato e nel modulo di avvio dei segmenti sono visualizzate le seguenti opzioni:

![](assets/new-segment-selector-collapsed.png){width="800" align="left"}

*Figura: Modulo di avvio segmenti con riepilogo dei segmenti compresso*

**A** Nome del segmento attualmente selezionato<br/>
**B** Intervallo di tempo e selettore di granularità<br/>
**C** Riepilogo segmenti compresso<br/>
**D** Opzione per espandere il riepilogo dei segmenti<br/>
**E** Dati del segmento (in termini di numero di account abbonato nel segmento per una durata)<br/>
**F** Opzione Apri elenco segmenti<br/>
**G** Opzione Modifica segmento<br/>
**H** Opzione Crea nuovo segmento<br/>

## Selezione del segmento {#segment-selection}

Per gli utenti programmatori o MVPD, passare alla **Apri segmento** opzione. Scegli un segmento dall’elenco e seleziona **Apri segmento** per visualizzare i rapporti sulla condivisione dell’account.

Utilizza il **Occhio** per visualizzare il riepilogo dettagliato dei segmenti, presentando le informazioni sul numero di account abbonato e le richieste di riproduzione da parte loro entro l’intervallo di tempo scelto.

+++Pannello di selezione dei segmenti per programmatori/MVPD

![](assets/segment-panel-programmers-mvpds.png) {width="800" align="left"}

*Figura: Pannello dei segmenti per programmatori/MVPD*

+++

Il riepilogo dei segmenti viene utilizzato per definire i seguenti parametri:

**[!UICONTROL Programmers in segment]**

**[!UICONTROL Channels in segment]**

**[!UICONTROL MVPD in segment]**

**[!UICONTROL Metrics in segment]**

<!-- The definitions of these parameters will be defined in the glossary article-->

## [!UICONTROL Granularity and time interval] {#granularity-timeinterval}

Il **[!UICONTROL Granularity and time interval]** Il selettore ti consente di specificare le date e la durata aggregate settimanalmente/mensilmente per osservare il comportamento di condivisione dell’account degli abbonati. La selezione predefinita dell&#39;intervallo di tempo è la settimana corrente, ma è possibile modificare la durata utilizzando le opzioni visualizzate nell&#39;immagine.

![[!UICONTROL Granularity and timeinterval]](assets/granularity-timeinterval-weekwise.png){width="350" align="left"}

*Figura: finestra di dialogo Granularità e intervallo di tempo*

**A** Scegli una data dal selettore data<br/>
**B** Selezionare la freccia sinistra per spostarsi all&#39;indietro<br/>
**C** Selezionare la freccia destra per spostarsi in avanti<br/>
**D** Seleziona la granularità per settimana/mese<br/>
**E** Intervallo di tempo selezionato<br/>

Applicando questi controlli puoi definire la tua problematica come &quot;abbonati del MVPD A che hanno guardato i canali X, Y e Z nel mese di ottobre&quot;.

