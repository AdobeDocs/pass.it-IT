---
title: Segmenti e intervallo di tempo
description: Definisci le coorti o seleziona i segmenti di abbonati per misurare le possibilità di condivisione dell’account e i modelli dei tuoi visualizzatori di canale per utilizzare strumenti grafici e rapporti in Account IQ.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# Segmenti e intervallo di tempo {#segment-timeinterval}

Quando accedi ad Account IQ, il pannello del segmento e dell&#39;intervallo di tempo situato sopra la dashboard ti consente di definire il [segmento](product-concepts.md#segmet-def) del destinatario predefinito. Questo pannello consente di filtrare i risultati e visualizzare i rapporti sul comportamento e i modelli di condivisione degli abbonati. Un segmento denominato **TUTTI GLI ACCOUNT NELLE PROPRIETÀ** è attualmente selezionato per impostazione predefinita, dove è possibile visualizzare le opzioni seguenti:

![](assets/new-segment-selector-collapsed.png){align="left"}

*Pannello Segmento e intervallo di tempo con riepilogo segmento compresso*

**A.** Nome segmento attualmente selezionato **B.** Apri elenco segmenti **C.** Modifica segmento **D.** Crea nuovo segmento **E.** Granularità e selettore intervallo di tempo **F.** Icona per espandere riepilogo segmenti **G.** Riepilogo segmenti compresso **H.** Numero di account nel segmento per l&#39;intervallo selezionato

>[!NOTE]
>
> Il riepilogo dei segmenti compressi mostra le [categorie video](product-concepts.md#video-category-def) utilizzate nella versione TV Everywhere di Account IQ. Se hai effettuato l’accesso come servizio D2C, queste etichette visualizzano le categorie video specifiche della tua azienda.

Ulteriori informazioni su [come creare](work-with-segments.md#create-new-segment) e [gestire i segmenti](work-with-segments.md#manage-segment) dalla scheda **Segmenti** nel pannello a sinistra.

## Selezione del segmento {#segment-selection}

Per selezionare un segmento specifico, effettua le seguenti operazioni:

1. Passare all&#39;opzione **[!UICONTROL Open segment]** nel pannello Segmento e Intervallo di tempo.
1. Selezionare il **Nome segmento** per il quale si desidera visualizzare i report di condivisione account.

   ![](assets/open-segment.png){align="left"}

   *Seleziona Nome Segmento*

   >[!NOTE]
   >
   > Le categorie video visualizzate nell&#39;immagine precedente, ad esempio **MVPDs**, **Programmer** e **Canali**, rappresentano le etichette utilizzate nella versione TV Everywhere di Account IQ. Se hai effettuato l’accesso come servizio D2C, queste etichette visualizzano le categorie video specifiche della tua azienda.

1. Selezionare **[!UICONTROL Open segment]**.


## Selezione granularità e intervallo di tempo {#granularity-timeinterval}

Il selettore **Granularità e intervallo di tempo** ti consente di specificare le date e la durata aggregate settimanalmente/mensilmente per osservare il comportamento di condivisione degli abbonati. La selezione predefinita è la settimana corrente.

![Granularità e intervallo di tempo](assets/granularity-timeinterval-weekwise.png){align="left"}

*Finestra di dialogo Granularità e intervallo di tempo*

**A.** Granularità e selettore intervallo di tempo **B.** Freccia destra per passare al mese/settimana successivo **C.** Opzione per scegliere la granularità per settimana/mese **D.** Intervallo di tempo attualmente selezionato **E.** Freccia sinistra per passare al mese/settimana precedente

Puoi modificare la durata come segue:

1. Seleziona **[!UICONTROL Granularity and Time Interval]** dal selettore data.

1. Selezionare **[!UICONTROL Week]** o **[!UICONTROL Month]** dall&#39;opzione **[!UICONTROL Aggregate By]** per impostare la granularità per la valutazione.

1. Dopo aver selezionato la granularità, puoi utilizzare le frecce avanti o indietro per spostarti nell’intervallo di tempo.

1. Seleziona un periodo di tempo specifico per la valutazione.

1. Seleziona **[!UICONTROL Apply]** per assicurarti che la selezione abbia effetto.

Questo consente di definire la dichiarazione del problema come &quot;Abbonati di MVPD A che hanno guardato i canali X, Y e Z durante la settimana scelta di dicembre&quot;.

## Riepilogo segmenti {#segment-summary}

Il Riepilogo segmenti è simile per i servizi D2C e TV Everywhere. Le categorie video saranno diverse per ogni rispettiva versione di Account IQ.

Seleziona Icona <img alt= "espandi riepilogo segmenti" src="./assets/expand-segment-summary.svg" width="25"> per visualizzare il riepilogo dettagliato dei segmenti. Presenta inoltre informazioni sul numero di account abbonati e sulle loro richieste di riproduzione entro il periodo di tempo scelto.

+++ Servizi D2C

![](assets/segment-panel-d2c.png){align="left"}

*Riepilogo segmenti per servizi D2C*

>[!NOTE]
>
>Le [categorie video](product-concepts.md#video-category-def) mostrate nell&#39;immagine precedente, ad esempio **area** e **tipi di contenuto** nel segmento sono solo esempi. Quando accedi ad Account IQ, queste etichette visualizzano le categorie video specifiche della tua azienda.

Il **Riepilogo segmenti** include le seguenti condizioni che definiscono un segmento:

**[Le aree geografiche e i tipi di contenuto](product-concepts.md#video-category-def) nel segmento** fanno riferimento alle etichette di metadati associate ai flussi video controllati dagli account condivisi rappresentati nei report di condivisione account.

**[Le metriche](product-concepts.md#metric) nel segmento** fanno riferimento agli attributi o ai criteri che i sottoscrittori devono soddisfare per essere identificati nei report di condivisione account.

+++

+++ TV Ovunque

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*Riepilogo segmenti per programmatori/MVPD*

Il **Riepilogo segmenti** include le seguenti condizioni che definiscono un segmento:

**[I programmatori](product-concepts.md#programmer-def) nel segmento** fanno riferimento a provider di contenuto i cui flussi video sono stati guardati da account condivisi rappresentati nei report di condivisione account.

**[Canali](product-concepts.md#channel-def) nel segmento** si riferiscono a canali i cui flussi video sono stati guardati da account condivisi rappresentati nei report di condivisione account.

**[MVPDs](product-concepts.md#mvpd-def) nel segmento** fanno riferimento a distributori di programmazione video multipli a cui sono associati gli abbonati per essere identificati nei rapporti di condivisione account.

**[Le metriche](product-concepts.md#metric) nel segmento** fanno riferimento agli attributi o ai criteri che i sottoscrittori devono soddisfare per essere identificati nei report di condivisione account.

+++
