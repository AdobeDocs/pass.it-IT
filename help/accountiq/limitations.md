---
title: Limitazioni e problemi noti
description: Problemi noti nel prodotto.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 2ced89dc1f77d2c090b599c40e778f3054f1a8dd
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# Problemi noti e limitazioni {#known-issues}

Adobe si impegna a offrire funzionalità affidabili e un’esperienza utente fluida attraverso le proprie offerte. La versione corrente (versione 1.0) di Account IQ fornisce analisi di condivisione dell’utilizzo e dell’abbonamento ai provider di streaming con un elevato grado di affidabilità. Tuttavia, nelle prossime versioni di verranno risolte le seguenti limitazioni.

* Attualmente, quando si definiscono le coorti nelle pagine della dashboard o dei report, non è possibile aggiungere metriche quali **numero di dispositivi** per perfezionare il segmento. Questa funzionalità sarà disponibile in una versione futura.

* Nella stima dei punteggi di condivisione per i singoli account, Account IQ adotta un approccio prudente che consente alle aziende di agire sulla condivisione con grande sicurezza. Tuttavia, questo approccio tende a sottovalutare la quantità totale di condivisione quando aggregata tra molti conti.

* Il [Punteggio di condivisione complessivo](/help/accountiq/dashboard.md#overall-sharing-score) attualmente solo fattori in [Livello di condivisione](/help/accountiq/dashboard.md#sharing-level) e [Utilizzo da account condivisi](/help/accountiq/dashboard.md#usage-from-shared-accounts). Le versioni future includeranno metriche aggiuntive.

* Quando definisci le coorti nelle pagine della dashboard o dei report, i selettori per MVPD e canali non dispongono, per ora, del meccanismo di ricerca.

* Quando definisci le coorti nelle pagine del dashboard o dei report, esiste un limite di selezione per un massimo di 10 MVPD e Programmatori (o singoli canali).

* L&#39;opzione per esportare le statistiche dei conti è limitata ad esportare 1000 conti, al momento.

* Opzione da selezionare [Tipo di segmento](#segment-type) quando la definizione delle operazioni è limitata a **Numero fisso di account**. Il **Numero variabile di account** Questa opzione sarà disponibile in una versione successiva.

* Le sezioni Benchmarking, Detection Models, Segments, Snapshot e Rules (Benchmarking, Modelli di rilevamento, Segmenti, Snapshot e Regole) nella navigazione a sinistra sono attualmente disabilitate e saranno disponibili in una versione futura.

* Durante la creazione [Operazioni](/help/accountiq/operation-affecting-user-segment.md), è possibile identificare solo due tipi di [Azioni](/help/accountiq/operation-affecting-user-segment.md) al momento — Regole di monitoraggio della concorrenza e azioni esterne.

* Attualmente, è possibile creare solo operazioni e [pianificato](/help/accountiq/operation-affecting-user-segment.md#action). Le versioni future ti consentiranno di metterle in pausa, riprenderle e gestirle completamente.

* A causa del set più limitato di dati utilizzati, la modalità di isolamento non riflette realmente la quantità di condivisione. Pertanto, MVPD in modalità isolamento non può essere confrontato con nessun altro MVPD. <!--do we need to separate out this limitation, which is from a different persona i.e. only for Programmer persona?-->

* Quando si definisce una nuova [segmento](/help/accountiq/segments-timeframe.md) per un’operazione puoi aggiungere metriche. Ma se selezioni un segmento salvato, non puoi aggiungere altre metriche per perfezionare il segmento.

* La granularità e il selettore dell’intervallo temporale sono limitati a una settimana o a un mese, il che significa che i dati possono essere valutati solo su una settimana o su un mese.

* Gli intervalli predefiniti sono attualmente disabilitati nel selettore di granularità e arco temporale e saranno disponibili in una versione futura.
