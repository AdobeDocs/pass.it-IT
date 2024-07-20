---
title: Rapporti di utilizzo generali
description: Scopri i rapporti sull’utilizzo generale
exl-id: 1272073a-61fe-47ec-aced-2e8055b6b11e
source-git-commit: 4a8a73d6c67508e88ba3ffbb9033b7e339f4fe8f
workflow-type: tm+mt
source-wordcount: '1055'
ht-degree: 0%

---

# [!UICONTROL General usage] report {#general-usage-reports}

I report [!UICONTROL Account IQ] sono strumenti analitici di base che consentono di analizzare i dati per isolare [coorti](/help/accountiq/product-concepts.md#segmet-def), identificare le anomalie e comprendere le caratteristiche dell&#39;account.

La pagina dei report [!UICONTROL General usage] fornisce gli strumenti per ricavare le metriche dei sottogruppi in base al numero di dispositivi dell&#39;account in uso, agli IP rilevati e ai rispettivi codici postali.

I report sono tutti basati sul segmento corrente selezionato dal pannello [Segmenti e intervallo di tempo](/help/accountiq/segments-timeinterval.md). Puoi ottimizzare la selezione e limitarla ulteriormente specificando (numero di dispositivi, numero di IP e numero di codici postali) soglie nel pannello [Panoramica snapshot-Account superiori alle soglie](#snapshot-overview).

## Riproduci richieste e abbonati univoci {#playreq-uniquesubs}

I grafici a linee forniscono una visualizzazione delle modifiche nel tempo dei valori, ad esempio Richieste Play e Abbonati univoci in un intervallo di tempo selezionato per il segmento definito.

+++ Servizi D2C: Richieste Play/Abbonati Univoci

![](assets/d2c-line-graph-gu.png)


*Richieste Play/Sottoscrittori univoci per servizi D2C*

+++

+++Programmatori: Riproduci richieste/Abbonati univoci

![](assets/progr-line-graph-gu.png)


*Riproduci richieste/Sottoscrittori univoci per programmatori*

+++

+++MVPDs: abbonati univoci

![](assets/mvpd-line-graph-gu.png)

*Sottoscrittori univoci per MVPDs*

+++

<br/>

L’asse x rappresenta il tempo in base all’intervallo corrente e l’asse y rappresenta le metriche dell’attività di base del sottoscrittore durante tale periodo. I grafici a linee consentono di visualizzare e confrontare l’attività degli abbonati nel segmento corrente. A seconda della versione di Account IQ, le metriche includono:

* **AuthN OK**: numero di autenticazioni riuscite. Ulteriori informazioni su [AuthN OK](/help/accountiq/product-concepts.md#authn-ok-def).

* **AuthZ OK**: numero di autorizzazioni completate. Ulteriori informazioni su [AuthZ OK](/help/accountiq/product-concepts.md#authz-ok-def).

* **Richieste Play**: numero di richieste Play. Ulteriori informazioni sulle [richieste Play](/help/accountiq/product-concepts.md#play-requests-def).

* **Sottoscrittori univoci**: numero di sottoscrittori univoci riusciti. Ulteriori informazioni su [Sottoscrittori univoci](/help/accountiq/product-concepts.md#unique-subscriber-def).

>[!NOTE]
>
>La disponibilità delle metriche dipende dalla versione di Account IQ.

## Panoramica snapshot - Conti superiori alle soglie {#snapshot-overview}

Ottimizza le analisi e i rapporti utilizzando questo filtro aggiuntivo per impostare varie soglie di utilizzo. Dopo aver selezionato un segmento, puoi anche utilizzare i seguenti filtri per analizzare ulteriormente il comportamento dell’abbonato:

* Soglia numero di dispositivi

* Soglia numero di IP

* Soglia numero di codici postali

Quando aggiorni i valori di soglia nel pannello [Segmento account in base alle soglie selezionate](#account-segments-basedon-segments), visualizzerai l&#39;effetto in:

* [Dispositivi per settimana (o mese) per account](#devices-week-account)

* [Posizioni per settimana (o mese) per account](#locations-week-account)

* [IP per settimana (o mese) per account](#ip-week-account)

* [Visualizzazione cronologica del segmento dei conti](#account-segment-historical-view)

>[!NOTE]
>
>Ogni soglia è impostata su un valore predefinito pari a 4. In altre parole, la pagina Uso generale mostra l&#39;analisi per gli abbonati che utilizzano più di quattro dispositivi, che utilizzano contenuti provenienti da più di quattro indirizzi IP diversi, *e* più di quattro codici postali diversi.

### Conti basati su segmenti in base alle soglie selezionate {#account-segments-basedon-segments}

Il pannello **Account basati sul segmento in base alle soglie selezionate** consente di impostare soglie (comprese tra 1 e 10) per il numero di dispositivi, il numero di IP e il numero di codici postali.

Il grafico mostra:

* Numero assoluto di account sottoscrittori.

* Percentuale degli account abbonato totali nel segmento che utilizzano il numero di dispositivi, dal numero di IP, nel numero di codici postali come specificato dalle soglie.

![](assets/select-thresholds.png)

## Dispositivi per settimana (o mese) per account {#devices-week-account}

Questo grafico a barre fornisce informazioni sul comportamento di utilizzo in termini di utilizzo dei dispositivi da parte degli abbonati per accedere al contenuto.

L&#39;asse x rappresenta il numero di account e l&#39;asse y il numero di dispositivi. In base alla soglia impostata per il numero di dispositivi per account, indica il numero assoluto di account di abbonati che utilizzano contenuti da un numero specifico di dispositivi nella durata di una settimana.

![](assets/bar-gr-devices-w-acc.png)

Quando si passa il mouse su una barra (specifica per il numero di dispositivi), viene visualizzata un’etichetta che fornisce informazioni sul numero di account dell’abbonato (e sulla percentuale del totale di account dell’abbonato nel segmento) che stanno trasmettendo contenuti del canale utilizzando questi numerosi dispositivi in una settimana.

Il grafico contrassegna inoltre quanto segue:

* Una linea rossa per contrassegnare la soglia impostata.

* Una linea verde per indicare la media del numero di dispositivi diversi utilizzati da un account abbonato alla settimana (o al mese).

L&#39;anello fornisce una vista alternativa dei dispositivi in uso dagli account nel segmento corrente al di sopra della soglia impostata.

![](assets/donut-devices-w-acc.png)

## Posizioni per settimana (o mese) per account {#locations-week-account}

Analogamente alla metrica per [Dispositivi alla settimana (o al mese) per account](#devices-week-account), la metrica Posizioni alla settimana (o al mese) per account consente di analizzare l&#39;utilizzo dell&#39;account dell&#39;abbonato da posizioni diverse. L&#39;asse x rappresenta il numero di conti e l&#39;asse y il numero di posizioni.

![](assets/graph-loc-week-acc.png)

Dopo aver impostato la soglia per il numero di posizioni, puoi utilizzare il grafico per identificare quanto segue:

* Numero (e percentuale) di abbonati che utilizzano contenuti da (uno specifico) x numero di posizioni in una settimana.

* Percentuale di account sottoscrittori totali che visualizzano il contenuto da più posizioni rispetto alla soglia.

* Confronta la media settimanale (numero di posizioni diverse per un account) con la soglia.

## Ips per settimana (o mese) per account {#ip-week-account}

Simile alla metrica per **Numero di posizioni per settimana per account**, la metrica per **Numero di IP per settimana per account** consente di valutare la quantità di modifica all&#39;origine dello streaming per il segmento corrente.

L’asse x rappresenta il numero di account e l’asse y il numero di IP.

![](assets/graph-ip-week-acc.png)

Dopo aver definito un segmento e impostato la soglia per il numero di IP, puoi utilizzare il grafico per identificare quanto segue:

* Numero (e percentuale) di abbonati che utilizzano contenuti da un numero specifico di IP in una settimana.

* Percentuale di account sottoscrittori totali che visualizzano il contenuto da più indirizzi IP della soglia.

* Confronta la media settimanale (numero di IP diversi per un account) con la soglia.

## Visualizzazione cronologica dei segmenti degli account {#account-segment-historical-view}

Il grafico a barre Visualizzazione cronologica consente di confrontare le metriche di utilizzo tra intervalli di tempo diversi. Inoltre, traccia collettivamente le varie metriche di utilizzo, ad esempio [Dispositivi per settimana (o mese) per account](#devices-week-account), [Posizioni per settimana (o mese) per account](#locations-week-account) e [IP per settimana (o mese) per account](#ip-week-account).

* L’asse x rappresenta l’intervallo di tempo e l’asse y rappresenta il numero di account abbonato, dispositivi, posizioni e IP.

* Le barre arancioni colorate indicano i segmenti in vari intervalli di tempo.

* Il grafico a linee traccia le modifiche in [Dispositivi per settimana (o mese) per account](#devices-week-account), [Posizioni per settimana (o mese) per account](#locations-week-account) e [IP per settimana (o mese) per account](#ip-week-account) valori nell&#39;intervallo di tempo in base alla soglia.

![](assets/historical-view.png)

* Le barre blu indicano il numero totale di abbonati attivi nel settore per un intervallo di tempo.

* Potete selezionare legende specifiche che vi aiuteranno a scalare il grafico.

![](assets/historical-view-total.png)
