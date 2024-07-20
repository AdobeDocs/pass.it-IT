---
title: Rapporti sugli account condivisi
description: Rapporti sugli account condivisi
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---

# Rapporti sugli account condivisi {#shared-accounts-reports}

I rapporti Conti condivisi forniscono un altro gruppo di grafici e grafici che riflettono il comportamento di condivisione e il consumo per il segmento corrente. Ad esempio, **[!UICONTROL Over Moderate Probability]** e **[!UICONTROL Over Low Probability]** per il segmento corrente.

## Probabilità di condivisione account {#accounts-sharing-probability}

Questi grafici ad anello e a barre mostrano le percentuali (e i numeri assoluti) degli account abbonato che rientrano in intervalli specifici di probabilità di condivisione. Questi intervalli sono definiti come:

* Molto alta (80%-100%)
* Alta (60%-80%)
* Moderato (40%-60%)
* Basso (20%-40%)
* Molto bassa (0%-20%)

La linea rossa contrassegna l&#39;intervallo di soglia selezionato nel pannello [Account oltre la soglia nel segmento corrente](#threshold-selector) e l&#39;area rossa chiara contiene il totale di tutti gli account al di sopra di tale soglia.

![](assets/accounts-sharing-probability-pie.png)

Il grafico a barre rappresenta il numero di conti che rientrano in ciascun intervallo sull’asse y per ciascuno degli intervalli (tracciati sull’asse x).

![](assets/accounts-sharing-probability-bar.png)

Anche in questo caso, la linea rossa indica la soglia corrente e l&#39;area rossa chiaro contiene il totale di tutti gli account al di sopra di tale soglia.

>[!NOTE]
>
> L’asse y del grafico a barre è logaritmico.

### Conti oltre la soglia nel segmento corrente{#threshold-selector}

Questo pannello consente di selezionare l’intervallo di soglia per i grafici ad anello e a barre qui sopra. Le quattro opzioni sono:

* Gli account **su molto bassi** condividono **probabilità**

* Account **oltre il minimo** che condividono **probabilità**

* Account **superiori a moderati** che condividono **probabilità**

* Account **oltre l&#39;alto** che condividono **probabilità**

![](assets/threshold-selector-shared-accounts.png)

Dopo aver selezionato la soglia, il pannello mostra la percentuale (e il numero) di account di tutti gli account abbonato nel segmento selezionato.

## Richieste di riproduzione dei segmenti sul totale {#play-request-out-total}

Il grafico ad anello mostra la percentuale (e il numero) di richieste di riproduzione effettuate dagli abbonati nel segmento consente di confrontare le richieste di riproduzione effettuate dagli abbonati non nel segmento definito.

![](assets/play-req-outof-total.png)

Quando si sposta il cursore sul grafico ad anello, vengono visualizzati anche le percentuali e i numeri degli abbonati da vari intervalli di probabilità.

<!--![](assets/play-request-total.gif)-->

## Numero medio di dispositivi per account per segmento{#avg-devices-account}

Il grafico a barre mostra il numero medio di dispositivi di ciascun tipo attualmente in uso dagli abbonati nel segmento corrente e di quelli non nel segmento corrente.

![](assets/avg-devices-per-acc.png)

## Codici postali di segmento per periodo per account {#zip-codes-period-account}

Questo grafico ti informa sul numero di abbonati nel segmento corrente che utilizzano contenuto da posizioni diverse (come misurato dal codice postale) per l’intervallo di tempo specificato.

![](assets/zip-period-account.png)

>[!NOTE]
>
>Per ingrandire le barre che rappresentano più set di codici postali, rappresentati da un segno più (**+**), fare doppio clic su di esse.


## Estensione geografica del segmento per periodo per conto {#geo-span-period-account}

Questo grafico a barre rappresenta il numero di account degli abbonati che utilizzano contenuto da posizioni che rientrano in intervalli geografici diversi in miglia. L&#39;intervallo si basa sulla distanza massima tra le posizioni da cui un abbonato ha effettuato lo streaming durante l&#39;intervallo di tempo.

![](assets/geogr-span-account.png)

>[!NOTE]
>
> È possibile ingrandire le barre che rappresentano più di un insieme di distanze geografiche, rappresentate da un segno più (**+**), ad esempio 1000+, facendo doppio clic su di esse.

>[!MORELIKETHIS]
>
>* Scopri come esportare i rapporti per i primi 1000 abbonati nel segmento selezionato utilizzando i filtri nei rapporti Account condivisi utilizzando l’opzione [Esporta i primi 1000 account](/help/accountiq/export-acc-information.md).
