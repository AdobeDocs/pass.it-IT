---
title: Esporta informazioni per account con punteggio di condivisione elevato
description: Esporta informazioni per account con punteggio di condivisione elevato.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# Esporta informazioni per account con punteggio di condivisione elevato {#export-account-info-high-score}

[!UICONTROL Account IQ] consente di esportare i dettagli di condivisione dell&#39;account per i primi 1000 account sottoscrittori in base alle loro [probabilità di condivisione](/help/accountiq/product-concepts.md#account-sharing-probability-def). È possibile esportare le informazioni sulla condivisione dell&#39;account per il [segmento](/help/accountiq/product-concepts.md#segment-def) corrente e il [intervallo di tempo specificato](/help/accountiq/product-concepts.md#time-interval-def) nella pagina [Report account condivisi](/help/accountiq/shared-acc-reports.md).

Segui i passaggi per esportare le informazioni sulla condivisione dell’account degli account abbonato per un segmento specifico.

1. Accedi con le tue credenziali.
1. Passa alla scheda **Account condivisi** nella sezione **Rapporti**.
1. Seleziona il segmento e l’intervallo di tempo richiesti dal pannello segmento e intervallo di tempo. Scopri [come selezionare un segmento e un intervallo di tempo](segments-timeinterval.md).

   Se necessario, fare riferimento alle istruzioni per [creare un segmento](work-with-segments.md#create-new-segment) o [modificare un segmento](work-with-segments.md#edit-segment).

1. Seleziona **[!UICONTROL Export top 1000 accounts]** che si trova nell&#39;angolo superiore destro del segmento e del pannello Intervallo di tempo.

   ![Esporta i primi 1000 account](assets/export-top-1000-accounts.png)

   *Selezionare l&#39;opzione Esporta i primi 1000 account*

Il file verrà scaricato automaticamente nel computer locale come file .csv.

Questo file contiene i dati per i primi 1000 account in base alle probabilità di condivisione degli account sottoscrittori nel segmento corrente in ordine decrescente.

Di seguito è riportato un esempio del file .csv esportato.

![dati esportati nel file CSV](assets/exported-csv.png)

*Dati esportati nel file CSV*

## Colonne nel report esportato {#columns-in-export}

**Settimana/Mese**

La settimana o il mese selezionato all&#39;interno dell&#39;opzione **[!UICONTROL Granularity and Time Interval]** nel selettore di segmenti.

**MVPD**

Se sei un programmatore, la colonna mostra il distributore con cui l’account è abbonato.

>[!NOTE]
>
> La colonna **MVPD** è disponibile solo per le versioni di TV Everywhere.

**ID sottoscrittore**

L’identificatore univoco dell’account specifico.

**Numero minimo di dispositivi**

Il numero minimo di dispositivi da cui gli utenti effettuano lo streaming attivo dei contenuti.

>[!NOTE]
>
>Il numero effettivo di dispositivi in streaming è maggiore del numero minimo di dispositivi specificato per un account specifico.

**Numero minimo di persone**

Il numero minimo di singoli utenti che hanno inviato in streaming contenuti utilizzando tali dispositivi.

>[!NOTE]
>
>Il numero effettivo di singoli utenti che trasmettono contenuti in streaming è maggiore del numero minimo di persone assegnate a un account specifico.

**[!UICONTROL # IPs]**

Il numero di indirizzi IP da cui il contenuto viene inviato in streaming.

**[!UICONTROL # Locations]**

Il numero di posizioni (in base al codice postale) da cui il contenuto viene inviato in streaming.

**[!UICONTROL # Cities]**

Il numero di città in cui ha avuto luogo l’attività di streaming.

**[!UICONTROL # States]**

Il numero di stati in cui ha avuto luogo l’attività di streaming.

**[!UICONTROL # Clusters]**

Numero di [cluster](/help/accountiq/product-concepts.md#cluster-def) distinti in cui è stato eseguito lo streaming.

**[!UICONTROL Geographic span (miles)]**

La distanza massima tra le posizioni di streaming associate all’account.

**[!UICONTROL # AuthN OK]**

Il numero di accessi effettuati dagli utenti durante il periodo specificato utilizzando tale account.

>[!NOTE]
>
> Alcuni servizi D2C potrebbero non visualizzare i dati di **[!UICONTROL # AuthN OK]** perché potrebbero non essere inclusi nei dati della loro azienda.

**[!UICONTROL # AuthZ OK]**

Il numero di volte in cui un MVPD ha autorizzato un flusso o concesso l&#39;accesso al contenuto per tale account.

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]** non è disponibile per i servizi D2C.

>[!NOTE]
>
>Per TV Everywhere, **[!UICONTROL # AuthZ OK]** è correlato al numero di **[# richieste Play](/help/accountiq/product-concepts.md##play-requests-def)**. Sarà sempre inferiore a **[!UICONTROL # Play Requests]** perché Adobe in genere memorizza nella cache le autorizzazioni degli MVPD per circa 24 ore.


**[!UICONTROL # Play Requests]**

Il numero effettivo di flussi che si sono verificati durante un periodo di tempo specificato.

>[!NOTE]
>
>La colonna [# richieste Play](/help/accountiq/product-concepts.md##play-requests-def) non è disponibile nella versione MVPD di TV Everywhere.

**[!UICONTROL # Channels]**

Il numero complessivo di canali controllati dall’account in un determinato periodo.

>[!NOTE]
>
> Per i servizi D2C **[!UICONTROL # Channels]** equivale al numero di **[!UICONTROL # Video categories]**.

>[!NOTE]
>
>Per TV Everywhere, includono i canali che potrebbero non appartenere al programmatore connesso. Questo numero per l’account include il tuo canale e altri canali a cui si accede durante il periodo specificato.


**Schema di utilizzo**

I valori all’interno di queste colonne fungono da identificatori corrispondenti a uno dei 14 modelli utilizzati per categorizzare tutti gli account utente.

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">Modelli di utilizzo</th>
      </tr>
      <tr>
        <td>1</td>
        <td>Utente normale</td>
      </tr>
      <tr>
        <td>2</td>
        <td>Viaggiatore o pendolare</td>
      </tr>
      <tr>
        <td>3</td>
        <td>Famiglia numerosa</td>
      </tr>
      <tr>
        <td>4</td>
        <td>Famiglia e amici intimi</td>
      </tr>
      </tr>
         <td>5 e 8</td>
         <td>Condivisione gruppo social network</td>
      </tr>
      </tr>
         <td>6</td>
         <td>Grande gruppo di amici</td>
      </tr>
      </tr>
         <td>7</td>
         <td>Streaming simultaneo</td>
      </tr>
      </tr>
         <td>9</td>
         <td>Condivisione community</td>
      </tr>
      </tr>
         <td>10 e 11</td>
         <td>Comportamento incerto</td>
      </tr>
      </tr>
         <td>12</td>
         <td>Famiglia piccola</td>
      </tr>
      </tr>
         <td>13</td>
         <td>Seconda pagina principale </td>
      </tr>
      </tr>
         <td>14</td>
         <td>Uso anormale</td>
      </tr>
    </tbody>
  </table>

*Identificatori pattern di utilizzo nel mapping .csv esportato con pattern di utilizzo*

**Probabilità di condivisione**

La probabilità che un account specifico condivida le sue credenziali.

>[!NOTE]
>
> La media della probabilità di condivisione di tutti gli account nel segmento selezionato viene utilizzata per calcolare il [livello di condivisione](/help/accountiq/data-panels.md#sharing-level) del [punteggio medio di condivisione](/help/accountiq/data-panels.md#aggregated-sharing).
