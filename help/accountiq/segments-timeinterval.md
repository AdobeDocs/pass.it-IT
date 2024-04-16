---
title: Segmenti e intervallo di tempo
description: Definisci le coorti o seleziona i segmenti di abbonati per misurare le possibilità e i pattern di condivisione dell’account da parte dei tuoi visualizzatori di canale, per utilizzare strumenti grafici e rapporti in Account IQ.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# Segmenti e intervallo di tempo {#segment-timeinterval}

Quando accedi a Account IQ, il pannello segmento e intervallo di tempo situato sopra la dashboard ti consente di definire l’abbonato [segmento](product-concepts.md#segmet-def). Questo pannello consente di filtrare i risultati e visualizzare i rapporti sul comportamento e i modelli di condivisione degli abbonati. Un segmento denominato **TUTTI GLI ACCOUNT NELLE PROPRIETÀ** è attualmente selezionato per impostazione predefinita, dove è possibile visualizzare le seguenti opzioni:

![](assets/new-segment-selector-collapsed.png){align="left"}

*Pannello Segmento e intervallo di tempo con riepilogo del segmento compresso*

**R.** Nome del segmento attualmente selezionato **B.** Apri elenco segmenti **C.** Modifica segmento **D.** Crea nuovo segmento **E.** Selettore granularità e intervallo di tempo **F.** Icona per espandere il riepilogo dei segmenti **G.** Riepilogo segmenti compressi **H.** Numero di conti nel segmento per l’intervallo selezionato

>[!NOTE]
>
> Il riepilogo dei segmenti compressi mostra [Categorie video](product-concepts.md#video-category-def) utilizzato nella versione TV Everywhere di Account IQ. Se hai effettuato l’accesso come servizio D2C, queste etichette visualizzano le categorie video specifiche della tua azienda.

Ulteriori informazioni su [come creare](work-with-segments.md#create-new-segment) e [gestire i segmenti](work-with-segments.md#manage-segment) dal **Segmenti** nel pannello sinistro.

## Selezione del segmento {#segment-selection}

Per selezionare un segmento specifico, effettua le seguenti operazioni:

1. Accedi a **[!UICONTROL Open segment]** nel pannello segmento e intervallo di tempo.
1. Seleziona la **Nome segmento** per il quale desideri visualizzare i rapporti di condivisione dell’account.

   ![](assets/open-segment.png){align="left"}

   *Seleziona nome segmento*

   >[!NOTE]
   >
   > Le categorie video mostrate nell’immagine precedente, ad esempio **MVPD**, **Programmatori**, e **Canali** rappresentano le etichette utilizzate nella versione TV Everywhere di Account IQ. Se hai effettuato l’accesso come servizio D2C, queste etichette visualizzano le categorie video specifiche della tua azienda.

1. Seleziona **[!UICONTROL Open segment]**.


## Selezione granularità e intervallo di tempo {#granularity-timeinterval}

Il **Granularità e intervallo di tempo** Il selettore ti consente di specificare le date e la durata aggregate settimanalmente/mensilmente per osservare il comportamento di condivisione degli abbonati. La selezione predefinita è la settimana corrente.

![Granularità e intervallo di tempo](assets/granularity-timeinterval-weekwise.png){align="left"}

*Finestra di dialogo Granularità e intervallo di tempo*

**R.** Selettore granularità e intervallo di tempo **B.** Freccia destra per passare al mese/settimana successivo **C.** Opzione per scegliere la granularità per settimana/mese **D.** Intervallo di tempo attualmente selezionato **E.** Freccia sinistra per passare al mese/settimana precedente

Puoi modificare la durata come segue:

1. Seleziona la **[!UICONTROL Granularity and Time Interval]** dal selettore data.

1. Seleziona una **[!UICONTROL Week]** o **[!UICONTROL Month]** da **[!UICONTROL Aggregate By]** per impostare la granularità per la valutazione.

1. Dopo aver selezionato la granularità, puoi utilizzare le frecce avanti o indietro per spostarti nell’intervallo di tempo.

1. Seleziona un periodo di tempo specifico per la valutazione.

1. Seleziona **[!UICONTROL Apply]** per assicurarsi che la selezione abbia effetto.

Questo consente di definire la dichiarazione del problema come &quot;Abbonati di MVPD A che hanno guardato i canali X, Y e Z durante la settimana scelta di dicembre&quot;.

## Riepilogo segmenti {#segment-summary}

Il Riepilogo segmenti è simile per i servizi D2C e TV Everywhere. Le categorie video saranno diverse per ogni rispettiva versione di Account IQ.

Seleziona <img alt= "espandi riepilogo segmenti" src="./assets/expand-segment-summary.svg" width="25"> per visualizzare il riepilogo dettagliato dei segmenti. Presenta inoltre informazioni sul numero di account abbonati e sulle loro richieste di riproduzione entro il periodo di tempo scelto.

+++ Servizi D2C

![](assets/segment-panel-d2c.png){align="left"}

*Riepilogo segmenti per servizi D2C*

>[!NOTE]
>
>Il [categorie video](product-concepts.md#video-category-def) mostrato nell’immagine precedente, ad esempio **area geografica** e **tipi di contenuto** nel segmento sono solo degli esempi. Quando accedi a Account IQ, queste etichette visualizzano le categorie video specifiche della tua azienda.

Il **Riepilogo segmenti** include le seguenti condizioni che definiscono un segmento:

**[Aree geografiche e tipi di contenuto](product-concepts.md#video-category-def) nel segmento** fai riferimento alle etichette di metadati associate ai flussi video guardati dagli account condivisi rappresentati nei rapporti di condivisione degli account.

**[Metriche](product-concepts.md#metric) nel segmento** fai riferimento agli attributi o ai criteri che i sottoscrittori devono soddisfare per essere identificati nei rapporti di condivisione degli account.

+++

+++ TV Ovunque

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*Riepilogo segmenti per programmatori/MVPD*

Il **Riepilogo segmenti** include le seguenti condizioni che definiscono un segmento:

**[Programmatori](product-concepts.md#programmer-def) nel segmento**  fai riferimento ai provider di contenuti i cui flussi video sono stati guardati da account condivisi rappresentati nei rapporti di condivisione account.

**[Canali](product-concepts.md#channel-def) nel segmento** fai riferimento ai canali i cui flussi video sono stati guardati da account condivisi rappresentati nei rapporti di condivisione account.

**[MVPD](product-concepts.md#mvpd-def) nel segmento** per essere identificati nei rapporti di condivisione dell’account, consulta distributori di programmi video ai quali sono associati gli abbonati.

**[Metriche](product-concepts.md#metric) nel segmento** fai riferimento agli attributi o ai criteri che i sottoscrittori devono soddisfare per essere identificati nei rapporti di condivisione degli account.

+++
