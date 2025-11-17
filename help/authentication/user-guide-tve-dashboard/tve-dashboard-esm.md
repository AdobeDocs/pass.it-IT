---
title: Dashboard ESM
description: Scopri come utilizzare la dashboard ESM per monitorare i dati di adesione ed eventi tra i partner MVPD.
source-git-commit: 53ebbd82fc160f68fccdddb18cf98e249ad6ecce
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%

---


# Dashboard ESM {#esm-dashboard}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Il dashboard ESM fornisce una visualizzazione unificata dei dati di adesione ed eventi per aiutarti a monitorare le prestazioni, identificare le anomalie e comprendere i modelli di accesso degli utenti tra i partner MVPD. Questa guida spiega come utilizzare i filtri della dashboard, interpretare i rapporti e comprendere le metriche chiave negli intervalli di tempo configurabili.

![Visualizzazione dashboard ESM](../assets/tve-dashboard/new-tve-dashboard/esm/esm-full-page.png)

## Casi d’uso {#use-cases}

- Visualizzare le tendenze per piattaforma o MVPD
- Confrontare le prestazioni di MVPD
- Comprendere l’utilizzo dei clienti per applicazione

Ulteriori dettagli sui dati e sugli eventi ESM sono disponibili all&#39;indirizzo [Panoramica sul monitoraggio dei servizi di adesione](https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview).

## Rapporti {#reports}

Sono disponibili i seguenti rapporti:

### Riproduci richieste {#play-requests}

Mostra il numero di richieste di riproduzione per l’intervallo di tempo selezionato.

**Stile grafico** - linea.

### Conversione dell’autenticazione {#authentication-conversion}

Mostra il rapporto tra gli eventi di autenticazione riusciti e il numero totale di tentativi di autenticazione.

**Stile grafico** - barra orizzontale

### Conversione autorizzazione {#authorization-conversion}

Mostra il rapporto tra gli eventi di autenticazione riusciti e il numero totale di tentativi di autenticazione.

**Stile grafico** - barra orizzontale

### Latenza di autorizzazione {#authorization-latency}

Mostra la latenza media (in millisecondi) per le risposte MVPD.

**Stile grafico** - linea.

### Utilizzo MVPD {#mvpd-usage}

Mostra un confronto del numero di utenti univoci per MVPD.

**Stile grafico** - area sovrapposta.

### Autenticazioni riuscite {#successful-authentications}

Mostra il numero totale di autenticazioni riuscite per l&#39;intervallo di tempo selezionato.

**Stile grafico** - linea.

### Autorizzazioni riuscite {#successful-authorizations}

Mostra il numero totale di autorizzazioni riuscite (risposte &quot;Permit&quot; di MVPD) per l’intervallo di tempo selezionato.

**Stile grafico** - linea.

## Azioni {#actions}

### Visualizza per {#view-by}

Per ciascun grafico, è disponibile un menu a discesa &quot;Visualizza per&quot; che consente di selezionare esattamente quali dati visualizzare.

- **Aggregato** - visualizza i dati complessivi
- **MVPDs / Canali / Piattaforme** - delinea i filtri specifici selezionati

### Scarica {#download}

Puoi scaricare i dati non elaborati:

- **Scarica i dati del grafico come CSV** - scarica i dati per un grafico specifico
- **Scarica tutti i dati come CSV**. Tutti i dati ESM vengono scaricati in tutti i grafici

## Filtri {#filters}

Utilizza i filtri per restringere il set di dati e focalizzare l’analisi. Sono disponibili i seguenti filtri:

- **Canale**: include tutti i canali (brand) disponibili
- **MVPD**: concentrarsi su uno o più provider
- **Piattaforma**: Web, dispositivi mobili, TV connessa o famiglia di dispositivi

Per aggiungere un nuovo filtro, selezionare il pulsante &quot;Aggiungi filtri&quot;.

Nella pagina &quot;Filtri per set di dati&quot;, puoi trascinare i filtri necessari.

![Filtri dashboard ESM](../assets/tve-dashboard/new-tve-dashboard/esm/filters-modal.png)

Per ogni sezione è possibile rimuovere i filtri singolarmente o cancellare l&#39;intera selezione.

### Suggerimenti per il filtro {#filter-tips}

- Combina più filtri per isolare una coorte (ad esempio, un MVPD su una piattaforma mobile per un canale).
- Non aggiungere filtri per eseguire lo zoom indietro e stabilire una linea di base prima di eseguire il drilling in.

## Intervalli di tempo {#time-intervals}

Controlla la finestra di analisi e la granularità.

![Intervalli di tempo del dashboard ESM](../assets/tve-dashboard/new-tve-dashboard/esm/date-picker.png)

### Intervallo date {#date-range}

**Predefiniti**: oggi, settimana corrente, ultimi 7 giorni, mese corrente, ultimi 30 giorni, ultimi 3 mesi, ultimi 6 mesi, ultimi 12 mesi

**Personalizzato**: seleziona l&#39;intervallo di tempo desiderato

### Granularità {#granularity}

Giornaliero/Mensile
