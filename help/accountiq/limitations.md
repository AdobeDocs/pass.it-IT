---
title: Limitazioni
description: Scopri le limitazioni e la modalità di isolamento MVPD per i programmatori in Account IQ.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Limitazioni {#limitations}

Le versioni D2C e TV Everywhere di Account IQ forniscono ai provider di streaming le analisi relative all’utilizzo e alla condivisione degli abbonamenti. Tuttavia, la versione corrente 1.3 presenta alcune limitazioni che verranno affrontate nelle versioni future.

* Il [Punteggio di condivisione complessivo](/help/accountiq/data-panels.md#overall-sharing-score) include attualmente [Livello di condivisione](/help/accountiq/data-panels.md#sharing-level) e [Utilizzo da account condivisi](/help/accountiq/data-panels.md#usage-from-shared-accounts). Le versioni future includeranno ulteriori metriche.

* Quando si definiscono i segmenti nel dashboard o i pattern di utilizzo, le [categorie video nel segmento](/help/accountiq/data-panels.md#video-categories-segment), [Condivisione del punteggio per canali e MVPD](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds) e [Distribuzione dei pattern di utilizzo per le categorie video](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) i report possono visualizzare solo i dati per un massimo di 20 [categorie video](product-concepts.md#video-category-def). I segmenti contenenti più di 20 categorie video non visualizzeranno i dati in questi rapporti.

* Attualmente, l&#39;opzione per esportare le statistiche dei conti è limitata all&#39;esportazione di 1000 conti.

* Quando si definiscono le operazioni, l&#39;opzione per selezionare il tipo di segmento [&#128279;](/help/accountiq/operations.md#segment) è limitata a **numero fisso di account**. L&#39;opzione **Numero variabile di account** sarà disponibile in una versione futura.

* Le sezioni **Benchmarking**, **Modelli di rilevamento**, **Azioni** e **Impostazioni** nella barra di navigazione a sinistra sono attualmente disabilitate e saranno disponibili in una versione futura.

* Durante la creazione delle operazioni, è possibile applicare solo due tipi di [azioni](/help/accountiq/operations.md#action): regole di controllo della concorrenza e azioni esterne.

* Al momento, è possibile [creare](/help/accountiq/operations.md#create-new-operation) e [pianificare](/help/accountiq/operations.md#schedule) operazioni. Le versioni future ti consentiranno di metterle in pausa, riprenderle e gestirle completamente.

* Quando selezioni Granularità e Intervallo di tempo, puoi analizzare i dati solo per una settimana o un mese alla volta.

* Non è possibile aggiungere MVPD in modalità isolamento alla definizione del segmento con altri MVPD. Alcuni MVPD non identificano in modo univoco gli abbonati tra più canali di programmazione. Pertanto, questi MVPD sono trattati separatamente per i programmatori TV Everywhere.



