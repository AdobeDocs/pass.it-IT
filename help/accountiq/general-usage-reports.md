---
title: Rapporti di utilizzo generali
description: Rapporti di utilizzo generali
exl-id: 1272073a-61fe-47ec-aced-2e8055b6b11e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 0%

---

# [!UICONTROL General Usage] rapporti {#general-usage-reports}

[!UICONTROL Account IQ] i report sono strumenti analitici di base e report che consentono di analizzare in profondità i dati per isolarli [coorti](/help/accountiq/product-concepts.md#segmet-def), identifica le anomalie e acquisisci una maggiore comprensione delle caratteristiche del tuo account.

[!UICONTROL General Usage] La pagina dei rapporti fornisce gli strumenti per suddividere le metriche dei sottogruppi in base al numero di dispositivi dell’account in uso, IP rilevati e rispettivi codici postali.

<!--Divide the content in cohorts.

Content filters
device filters

segment and definition replicate to cohorts. Number of people and number of account that ......
content consumption.....-->

I rapporti si basano tutti sul segmento corrente selezionato utilizzando [Segmenti e intervallo temporale](/help/accountiq/howto-select-segment-timeframe.md) pannello. Puoi ottimizzare la selezione e limitarla ulteriormente specificando (numero di dispositivi, numero di IP e numero di codici postali) le soglie in [Panoramica snapshot - Conti superiori alle soglie](#snapshot-overview) pannello.

<!--To view General Usage Reports:

1. Select the desired MVPDs from the **MVPDs in Segment** option.

2. Select the desired programmer channels from the **Channels in Segment** Option.

3. Select an appropriate time frame from the **Granularity and time frame** option.

   Using the above options you have defined segments for your analysis. Based on your segment selection, following graphs and reports are displayed.

4. You can fine tune your selection and further narrow it down by specifying (number of devices, number of IPs, and number of zip codes) thresholds in [Snapshot Overview - Accounts above thresholds](#snapshot-overview) widget/panel.-->

## AuthN OK / AuthZ OK / Riproduci richieste / Abbonati univoci {#authn-authz-playreq-uniquesubs}

I grafici a linee forniscono una visualizzazione delle modifiche nel tempo dei valori di AuthN OK, AuthZ OK, Play Requests e Unique Subscribers in un intervallo di tempo selezionato per il segmento definito.

+++Programmatore- **AuthN OK / AuthZ OK / Riproduci richieste / Abbonati univoci**

![](assets/progr-line-graph-gu.png)


*Figura: AuthN OK / AuthZ OK / Play Requests / Abbonati univoci per l’utente del programmatore*


+++


+++MVPD- **AuthN OK / AuthZ OK / Abbonati univoci**

![](assets/mvpd-line-graph-gu.png)


*Figura: AuthN OK / AuthZ OK / Abbonati univoci per l’utente MVPD*


+++

L’asse x presenta le unità entro l’intervallo di tempo corrente e l’asse y rappresenta le metriche di base dell’attività del sottoscrittore durante tale periodo. I grafici a linee ti consentono di confrontare i seguenti valori per gli abbonati a MVPD e ai canali selezionati nel pannello di selezione dei segmenti:

* **AuthN OK**

  AuthN OK è il numero di autenticazioni riuscite. Per ulteriori informazioni e definizioni consulta [Concetti del prodotto: AuthN OK](/help/accountiq/product-concepts.md#authn-ok-def).

* **AuthZ OK**

  AuthZ OK indica il numero di autorizzazioni completate. Per ulteriori informazioni e definizioni consulta [Concetti del prodotto: AuthZ OK](/help/accountiq/product-concepts.md#authz-ok-def).

* **Riproduci richieste**

  Richieste Play indica il numero di richieste Play. Per ulteriori informazioni e definizioni consulta [Concetti di prodotto: Riproduci richieste](/help/accountiq/product-concepts.md#play-requests-def)

  >[!NOTE]
  >
  >Il grafico a linee delle richieste di riproduzione non è disponibile per gli utenti MVPD.


* **Abbonati univoci**

  Abbonati univoci indica il numero di abbonati univoci riusciti. Per ulteriori informazioni e definizioni consulta [Concetti di prodotto: abbonati univoci](/help/accountiq/product-concepts.md#unique-subscriber-def)

  >[!NOTE]
  >
  >Il numero totale di abbonati univoci include anche il numero di dispositivi univoci se l’utilizzo da parte di un programmatore di TempPass Adobe (ovvero anteprima gratuita) fa parte del segmento.

## Panoramica snapshot - Conti superiori alle soglie {#snapshot-overview}

Ottimizza le analisi e i rapporti utilizzando questo filtro aggiuntivo per impostare varie soglie di utilizzo. Dopo aver definito il segmento (o la coorte) per l’analisi selezionando gli MVPD e i canali desiderati, puoi utilizzare i seguenti filtri per per analizzare il comportamento degli abbonati:

* Soglia numero di dispositivi

* Soglia numero di IP

* Soglia numero di codici postali

Quando si aggiornano i valori di soglia in [Segmento conti - in base alle soglie selezionate](#account-segments-basedon-segments) dell&#39;effetto in:

* [Dispositivi per settimana (o mese) per account](#devices-week-account)

* [Posizioni per settimana (o mese) per account](#locations-week-account)

* [IP per settimana (o mese) per account](#ip-week-account)

* [Visualizzazione cronologica del segmento dei conti](#account-segment-historical-view)

>[!NOTE]
>
>Il valore predefinito per ciascuna soglia è 4. Ciò significa che la pagina Uso generale mostra l’analisi per MVPD con gli abbonati che utilizzano quattro (e più di quattro) dispositivi, utilizzando contenuti provenienti da quattro (e più) diverse posizioni geografiche e quattro (e più) diversi codici postali.

### Segmento conti - in base alle soglie selezionate {#account-segments-basedon-segments}

Il **Segmento conti - in base alle soglie selezionate** Il pannello consente di impostare soglie (tra 1 e 10) per il numero di dispositivi, il numero di IP e il numero di codici postali.

Il grafico mostra:

* numero assoluto di account abbonati e

* percentuale del totale dei conti degli abbonati in quel segmento,

  che utilizzano il numero X di dispositivi, il numero Y di IP e il numero Z di codici postali per utilizzare contenuti dal canale per gli MVPD (segmenti definiti di), per un intervallo di tempo.

![](assets/select-thresholds.png)

## Dispositivi per settimana (o mese) per account {#devices-week-account}

Il **grafico a barre** fornisce informazioni sul comportamento di utilizzo in termini di utilizzo dei dispositivi da parte degli abbonati per accedere ai contenuti.

L&#39;asse x rappresenta il numero di account e l&#39;asse y il numero di dispositivi. In base alla soglia impostata per il numero di dispositivi per account, indica il numero assoluto di account di abbonati che utilizzano contenuti da un numero specifico di dispositivi nella durata di una settimana.

![](assets/bar-gr-devices-w-acc.png)

Quando si passa il mouse su una barra (specifica per il numero di dispositivi), viene visualizzata un’etichetta che fornisce informazioni sul numero di account dell’abbonato (e sulla percentuale del totale di account dell’abbonato nel segmento) che stanno trasmettendo contenuti del canale utilizzando questi numerosi dispositivi in una settimana.

Il grafico contrassegna inoltre quanto segue:

* Una linea rossa per contrassegnare la soglia impostata.

* Una linea verde per indicare la media del numero di dispositivi diversi utilizzati da un account abbonato alla settimana (o al mese).

Puoi confrontare il livello di soglia con la media settimanale del numero di diversi dispositivi utilizzati da un account, per giudicare il livello di condivisione.

Il grafico fornisce anche un’idea della percentuale di account degli abbonati che utilizzano un numero di dispositivi maggiore della soglia impostata.

Il grafico ad anello consente di valutare rapidamente l’entità degli account degli abbonati che consumano contenuto del canale utilizzando dispositivi superiori alla soglia impostata (in un arco temporale).

![](assets/donut-devices-w-acc.png)

## Posizioni per settimana (o mese) per account {#locations-week-account}

Mi piace [Dispositivi per settimana (o mese) per account](#devices-week-account), la metrica Posizioni per settimana (o mese) per account consente di analizzare l’utilizzo dell’account abbonato da posizioni diverse, per identificare più da vicino la condivisione delle password. L&#39;asse x rappresenta il numero di conti e l&#39;asse y il numero di posizioni.

Risultati da questa metrica combinati con il numero di [Dispositivi per settimana (o mese) per account](#devices-week-account) e numero di [IP per settimana (o mese) per account](#ip-week-account) consente di valutare in modo più accurato le istanze di condivisione delle password, in modo che gli utenti autentici non vengano conteggiati in.

![](assets/graph-loc-week-acc.png)

Dopo aver definito un segmento e impostato la soglia per il numero di posizioni, puoi identificare dal grafico:

* Numero (e percentuale) di abbonati che utilizzano contenuti da (uno specifico) x numero di posizioni in una settimana.

* Percentuale di account sottoscrittori totali che visualizzano il contenuto da più posizioni rispetto alla soglia.

* Confronta la media settimanale (numero di posizioni diverse per un account) con Soglia.

## IP per settimana (o mese) per account {#ip-week-account}

Simile a [Dispositivi per settimana (o mese) per account](#devices-week-account) e [Posizioni per settimana (o mese) per account](#locations-week-account), il **Numero di IP alla settimana per account** La metrica consente di analizzare la condivisione delle password in modo più preciso e con maggiore granularità.

L’asse x rappresenta il numero di account e l’asse y il numero di IP.

![](assets/graph-ip-week-acc.png)

Dopo aver definito un segmento (selezionando MVPD e canali) e impostato la soglia per il numero di IP, puoi identificare dal grafico:

* Numero (e percentuale) di abbonati che utilizzano contenuti da (uno specifico) x numero di IP in una settimana.

* Percentuale di account sottoscrittori totali che visualizzano il contenuto da più indirizzi IP della soglia.

* Confronta la media settimanale (numero di IP diversi per un account) con la soglia.

## Segmento conti - Visualizzazione cronologica {#account-segment-historical-view}

Il grafico a barre Visualizzazione cronologica consente di confrontare le metriche di utilizzo tra diversi intervalli di tempo. Inoltre, traccia collettivamente le varie metriche di utilizzo, come [Dispositivi per settimana (o mese) per account](#devices-week-account), [Posizioni per settimana (o mese) per account](#locations-week-account), e [IP per settimana (o mese) per account](#ip-week-account).

* L’asse x rappresenta l’intervallo di tempo e l’asse y rappresenta il numero di account abbonato, dispositivi, posizioni e IP.

* Le barre arancioni colorate indicano i segmenti in vari intervalli di tempo.

* Il grafico a linee traccia le modifiche in [Dispositivi per settimana (o mese) per account](#devices-week-account), [Posizioni per settimana (o mese) per account](#locations-week-account), e [IP per settimana (o mese) per account](#ip-week-account) valori nell’intervallo di tempo in base alla soglia.

![](assets/historical-view.png)

* Le barre blu indicano il numero totale di abbonati attivi nel settore per un intervallo di tempo.

* Potete selezionare legende specifiche che vi aiuteranno a scalare il grafico.

![](assets/historical-view-total.png)

>[!MORELIKETHIS]
>
>* Scopri come esportare rapporti per i primi 1000 abbonati nel segmento selezionato utilizzando i filtri in Rapporto di utilizzo generale utilizzando [Esporta i primi 1000 account](/help/accountiq/export-acc-information.md) opzione.
