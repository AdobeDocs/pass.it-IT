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

[!UICONTROL Account IQ] consente di esportare i dettagli di condivisione dell’account per i primi 1000 account abbonati in base ai [condivisione delle probabilità](/help/accountiq/product-concepts.md#account-sharing-probability-def). Puoi esportare le informazioni sulla condivisione dell’account per il [segmento](/help/accountiq/product-concepts.md#segment-def) e [intervallo di tempo specificato](/help/accountiq/product-concepts.md#time-interval-def) il [Rapporti sugli account condivisi](/help/accountiq/shared-acc-reports.md) pagina.

Segui i passaggi per esportare le informazioni sulla condivisione dell’account degli account abbonato per un segmento specifico.

1. Accedi con le tue credenziali.
1. Accedi a **Account condivisi** scheda in **Rapporti** sezione.
1. Seleziona il segmento e l’intervallo di tempo richiesti dal pannello segmento e intervallo di tempo. Scopri [come selezionare un segmento e un intervallo di tempo](segments-timeinterval.md).

   Se necessario, fare riferimento alle istruzioni per [creazione di un segmento](work-with-segments.md#create-new-segment) o [modifica di un segmento](work-with-segments.md#edit-segment).

1. Seleziona **[!UICONTROL Export top 1000 accounts]** si trova nell’angolo superiore destro del pannello segmento e intervallo di tempo.

   ![Esporta i primi 1000 account](assets/export-top-1000-accounts.png)

   *Seleziona l’opzione Esporta i primi 1000 account*

Il file verrà scaricato automaticamente nel computer locale come file .csv.

Questo file contiene i dati per i primi 1000 account in base alle probabilità di condivisione degli account sottoscrittori nel segmento corrente in ordine decrescente.

Di seguito è riportato un esempio del file .csv esportato.

![dati esportati in un file .csv](assets/exported-csv.png)

*Dati esportati in un file .csv*

## Colonne nel report esportato {#columns-in-export}

**Settimana/Mese**

La settimana o il mese selezionato entro il **[!UICONTROL Granularity and Time Interval]** nel selettore dei segmenti.

**MVPD**

Se sei un programmatore, la colonna mostra il distributore con cui l’account è abbonato.

>[!NOTE]
>
> Il **MVPD** è disponibile solo per le versioni di TV Everywhere.

**ID abbonato**

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

Numero di elementi distinti [cluster](/help/accountiq/product-concepts.md#cluster-def) dove ha avuto luogo lo streaming.

**[!UICONTROL Geographic span (miles)]**

La distanza massima tra le posizioni di streaming associate all’account.

**[!UICONTROL # AuthN OK]**

Il numero di accessi effettuati dagli utenti durante il periodo specificato utilizzando tale account.

>[!NOTE]
>
> Alcuni servizi D2C potrebbero non visualizzare **[!UICONTROL # AuthN OK]** dati in quanto potrebbero non essere inclusi nei dati della loro azienda.

**[!UICONTROL # AuthZ OK]**

Il numero di volte in cui un MVPD ha autorizzato un flusso o concesso l&#39;accesso al contenuto per tale account.

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]** non è disponibile per i servizi D2C.

>[!NOTE]
>
>Per la TV Ovunque, **[!UICONTROL # AuthZ OK]** è correlato al numero di **[N. richieste Play](/help/accountiq/product-concepts.md##play-requests-def)**. Sarà sempre minore di **[!UICONTROL # Play Requests]** poiché Adobe in genere memorizza nella cache le autorizzazioni degli MVPD per circa 24 ore.


**[!UICONTROL # Play Requests]**

Il numero effettivo di flussi che si sono verificati durante un periodo di tempo specificato.

>[!NOTE]
>
>Il [N. richieste Play](/help/accountiq/product-concepts.md##play-requests-def) non è disponibile nella versione MVPD di TV Everywhere.

**[!UICONTROL # Channels]**

Il numero complessivo di canali controllati dall’account in un determinato periodo.

>[!NOTE]
>
> Per i servizi D2C **[!UICONTROL # Channels]** è equivalente al numero di **[!UICONTROL # Video categories]**.

>[!NOTE]
>
>Per TV Everywhere, includono i canali che potrebbero non appartenere al programmatore connesso. Questo numero per l’account include il tuo canale e altri canali a cui si accede durante il periodo specificato.


**Pattern di utilizzo**

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

*Identificatori del pattern di utilizzo nella mappatura .csv esportata con pattern di utilizzo*

**Probabilità di condivisione**

La probabilità che un account specifico condivida le sue credenziali.

>[!NOTE]
>
> La media della probabilità di condivisione di tutti i conti nel segmento selezionato viene utilizzata per calcolare [livello di condivisione](/help/accountiq/data-panels.md#sharing-level) del [punteggio medio di condivisione](/help/accountiq/data-panels.md#aggregated-sharing).
