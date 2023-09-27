---
title: Rapporti Account condivisi
description: Rapporti sugli account condivisi
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# Rapporti sugli account condivisi {#shared-accounts-reports}

I rapporti Account condivisi suddividono le metriche, ad esempio il numero di dispositivi e tipi di dispositivi, in base all’intervallo di probabilità di condivisione selezionato, **[!UICONTROL Over Moderate Probability]** e **[!UICONTROL Over Low Probability]** per il segmento corrente.

Questi intervalli possono quindi fungere da soglie definite dall&#39;utente e i grafici vengono aggiornati in base alle soglie selezionate.

Il conto IQ classifica tutti i conti degli abbonati del segmento definito nei conti con le cinque categorie seguenti in base alle loro probabilità di condivisione:

* Molto alta (80%-100%)
* Alta (60%-80%)
* Moderato (40%-60%)
* Basso (20%-40%)
* Molto bassa (0%-20%)

## Probabilità di condivisione account {#accounts-sharing-probability}

Il grafico ad anello qui categorizza e mostra le percentuali (e i numeri assoluti) degli account abbonato da varie categorie di probabilità.

La linea rossa contrassegna l’intervallo di soglia selezionato dagli utenti in [Conti oltre la soglia nel segmento corrente](#threshold-selector) pannello.

![](assets/accounts-sharing-probability-pie.png)

Il grafico a barre rappresenta il numero di conti sull’asse y per varie categorie di probabilità di condivisione (tracciato sull’asse x).

![](assets/accounts-sharing-probability-bar.png)

La linea rossa indica l’intervallo di soglia e può essere regolata nel grafico a barre. La soglia corretta nel grafico a barre si riflette nell’intervallo di soglia nel grafico ad anello.

<!--![](assets/shared-accounts-rep.gif)-->

### Conti oltre la soglia nel segmento corrente{#threshold-selector}

Questo pannello consente di selezionare un intervallo tra i seguenti come soglia per gli account abbonato (in base alle loro probabilità di condivisione):

* Account **su molto basso** condivisione **probabilità**

* Account **oltre il basso** condivisione **probabilità**

* Account **oltre moderato** condivisione **probabilità**

* Account **oltre l&#39;altezza** condivisione **probabilità**

![](assets/threshold-selector-shared-accounts.png)

Dopo aver selezionato la soglia, il pannello mostra la percentuale (e il numero) di account di tutti gli account abbonato nel segmento selezionato.

## Segmento: riproduci richieste sul totale {#play-request-out-total}

Il grafico ad anello mostra la percentuale (e il numero) di richieste di riproduzione effettuate dagli abbonati nel segmento e consente di confrontare le richieste di riproduzione effettuate dagli abbonati non nel segmento definito.

![](assets/play-req-outof-total.png)

Quando si sposta il cursore sul grafico ad anello, vengono visualizzati anche le percentuali e i numeri degli abbonati da vari intervalli di probabilità.

<!--![](assets/play-request-total.gif)-->

## Numero medio di dispositivi per conto del segmento{#avg-devices-account}

Il grafico a barre mostra il numero medio di dispositivi di ciascun tipo utilizzati dagli abbonati nel segmento corrente e dagli abbonati non nel segmento corrente.

![](assets/avg-devices-per-acc.png)

## Segmento - Codici postali per periodo per conto {#zip-codes-period-account}

Questo grafico ti informa sul numero di abbonati che utilizzano contenuti da posizioni diverse in un intervallo di tempo.

![](assets/zip-period-account.png)

Potete ingrandire per ridurre e visualizzare le specifiche di una barra nel grafico che traccia un intervallo di posizioni.

<!--![](assets/zip-code-period.gif)-->

## Segmento - Estensione geografica/Periodo/Account {#geo-span-period-account}

Questo grafico a barre rappresenta il numero di account degli abbonati rispetto a diversi intervalli di intervalli geografici in miglia. L’intervallo si basa sulla distanza massima tra le posizioni da cui un abbonato ha effettuato lo streaming durante l’intervallo di tempo.

<!--Total number of users ...

How many accounts are within 99 miles of each other.....and how many are apart. 

Based on points on the map.-->

![](assets/geogr-span-account.png)

Quando selezioni una barra che rappresenta un intervallo di distanza geografica, questo si espande per mostrare più dettagli.

<!--![](assets/geo-span-period-acc.gif)-->
