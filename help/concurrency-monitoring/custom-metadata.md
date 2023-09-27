---
title: Metadati personalizzati
description: Metadati personalizzati
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---



# Metadati personalizzati {#cm}

>[!NOTE]
>
> Questa pagina è obsoleta in quanto si applica alla versione precedente dell’API, che non è più consigliata per le nuove integrazioni

Il servizio consente ai clienti di utilizzare campi standard e personalizzati sia nelle query che nei processi decisionali. I campi standard sono sempre disponibili per qualsiasi flusso (in quanto sono necessari per la creazione del flusso o generati dal server):

* applicazione
* mvpd
* accountId
* streamId
* streamStart
* iniziatore


I campi personalizzati sono qualsiasi coppia chiave/valore passata durante l’inizializzazione del flusso, come parametri di una maschera o di una stringa di query. I campi standard e personalizzati possono quindi essere utilizzati nei seguenti scenari (per i dettagli, consulta la documentazione effettiva per le risorse API coinvolte di seguito):

* Richiesta di campi aggiuntivi (tramite il parametro della stringa di query fields) durante il recupero dell’elenco di flussi per un account (risorsa /streams)
* Suddividere l’attività dell’account specificando le dimensioni da raggruppare (risorsa /activity)
* Definizione dei criteri lato server in base ai valori dei campi o alle cardinalità (gli esempi utilizzano pseudo-SQL solo per maggiore chiarezza):
* Configura un criterio da applicare solo a valori di campo specifici (ad esempio un criterio iOS dedicato: WHERE osType IS &#39;iOS&#39;)
* Limita il numero di valori distinti per un determinato campo (ad esempio, non più di X dispositivi distinti: HAVING DISTINCT COUNT(deviceId) >= 2)
* Limita il numero di flussi attivi per valore di campo (ad esempio, non più di X flussi attivi per un singolo tipo di dispositivo: GROUP BY deviceType HAVING COUNT(streamId) >= 3)


In base a queste chiavi/valori inviati, è possibile stabilire regole diverse. Questo potrebbe essere qualcosa del genere:

1. Il cliente decide di voler inviare il gruppo di parametri, che avrà come valori &quot;SPORTS&quot; e &quot;KIDS&quot;.
1. L’app dovrà quindi eseguire questa operazione:
   * Per i canali sportivi, al momento dell’inizializzazione del flusso l’app invierebbe ***type=SPORTS*** come parametro di query
   * Per i canali con contenuti relativi ai bambini, al momento dell’inizializzazione del flusso l’app inviava ***type=KIDS*** come parametro di query
1. Quindi si potrebbe definire un criterio come questo:
   * `GROUP by type HAVING COUNT(streamID) < 4) IF type=KIDS`
   * `GROUP by type HAVING COUNT(streamID) < 2) IF type=SPORTS`
1. In pratica, questo significa che quando un utente guarda contenuti sportivi, non può farlo su più di 1 dispositivo; tuttavia, quando l’utente guarda contenuti per bambini, la visualizzazione è consentita su un massimo di 3 dispositivi.

