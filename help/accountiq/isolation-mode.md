---
title: Visualizzare i rapporti in modalità isolamento
description: Visualizza i rapporti in modalità isolamento per Xfinity.
exl-id: e7cf24c5-9bfa-48f6-b5c8-20443a976891
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Visualizzare i rapporti di condivisione in modalità isolamento {#report-isolation-mode}

In modalità isolamento, i MVPD (come Xfinity) identificano in modo coerente gli abbonati tra i dispositivi, ma identificano i loro abbonati in modo diverso, in base ai programmatori con cui interagiscono. Mentre nella modalità standard, gli MVPD identificano costantemente gli abbonati tra i dispositivi, indipendentemente dai programmatori.

Ad esempio, nell&#39;immagine seguente, se un Abbonato B di un MVPD in modalità isolamento (come Xfinity) accede al contenuto offerto da due diversi programmatori utilizzando lo stesso dispositivo, l&#39;MVPD assocerà identificatori diversi ai due diversi tentativi di accesso. Quindi, a quei programmatori (L e M nella figura) e a Account IQ, sembra che ci siano due diversi abbonati che accedono al contenuto. Tuttavia, per MVPD Standard, se l’abbonato B accede a contenuti offerti da due programmatori diversi, MVPD assocerà un singolo identificatore di accesso per entrambi i tentativi di accesso. Gli MVPD (come Xfinity) in modalità isolamento non identificano in modo coerente un abbonato anche se questo utilizza lo stesso dispositivo tra programmatori diversi.

![](assets/isolation-diff-new.png)

*Figura: Modalità isolamento MVPD identifica quattro abbonati diversi anziché due*

Per gestire la distorsione dei dati (dovuta all&#39;identificazione dello stesso abbonato come diverso in base all&#39;accesso a diversi programmatori), la Modalità isolamento limita l&#39;attività segnalata su un programmatore all&#39;attività solo sulle applicazioni di quel programmatore. Ad esempio, per la Modalità isolamento nell&#39;immagine precedente, il programmatore L visualizza i dati in base all&#39;attività delle identità W e Y, ignorando le identità X e Z.

>[!IMPORTANT]
>
> L&#39;inconveniente è che il programmatore L è privato della condivisione di informazioni raccolte sugli abbonati A e B a causa di attività con qualsiasi programmatore diverso da L.

In modalità Isolamento tutti i calcoli effettuati per ottenere i punteggi di condivisione e tutte le metriche associate vengono eseguiti utilizzando solo l’attività dei dispositivi in streaming dalle applicazioni appartenenti al programmatore e ai canali selezionati.
I punteggi e le probabilità di condivisione vengono calcolati solo utilizzando il flusso che inizia dai canali attualmente selezionati.

Per visualizzare le metriche in modalità isolamento:

1. Seleziona **[!UICONTROL isolation mode]** dal **[!UICONTROL MVPDs in segment]** e selezionare **[!UICONTROL Apply Selection]**.

   ![](assets/xfinity-in-segment.gif)

   *Figura: selezione MVPD in modalità isolamento*

1. Seleziona i canali desiderati dalla **[!UICONTROL Channels in segment]** e selezionare **[!UICONTROL Apply Selection]**.

   Inoltre, seleziona un [intervallo temporale](/help/accountiq/product-concepts.md#granularity-def).

   >[!IMPORTANT]
   >
   >Poiché la condivisione dell’account è più rilevante se misurata per lo streaming tra le applicazioni di tutti i programmatori, vedrai punteggi più bassi e alcune variazioni nelle metriche quando sei in modalità isolamento.

   ![](assets/aggregate-sharing-isolation.png)

   *Figura: Condivisione dei misuratori di probabilità in modalità isolamento*

   I contatori di cui sopra mostrano che solo il 6% di tutti gli account è condiviso; e solo l&#39;8% del contenuto è consumato da questi 8%. In questo modo i canali possono confrontare i loro punteggi in modalità Isolamento con quelli degli altri MVPD. Pertanto, le informazioni ottenute utilizzando la modalità di isolamento dovrebbero essere interpretate in modo diverso rispetto agli altri dati.
