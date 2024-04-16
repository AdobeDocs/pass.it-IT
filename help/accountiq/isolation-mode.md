---
title: Modalità di isolamento MVPD
description: Informazioni sulla modalità di isolamento MVPD per i programmatori TV Everywhere
source-git-commit: 5639319ce8915f0c33d927ca9554c405b3b2e87d
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 0%

---


# Modalità isolamento MVPD per programmatori TV Everywhere {#isolation-mode-tve}

>[!IMPORTANT]
>
> La limitazione MVPD per la modalità isolamento è applicabile solo ai programmatori TV Everywhere.

In modalità isolamento, i MVPD (come Xfinity) identificano in modo coerente gli abbonati tra i dispositivi in base alle loro interazioni con programmatori specifici. Nella modalità standard, gli MVPD identificano in modo coerente gli abbonati tra i dispositivi indipendentemente dai programmatori coinvolti.

Ecco un esempio:

![](assets/isolation-diff-new.png)

*Modalità isolamento MVPD identifica quattro abbonati diversi anziché due*

* Se un abbonato B di un MVPD in modalità isolamento (come Xfinity) accede al contenuto offerto da due diversi programmatori che utilizzano lo stesso dispositivo, il MVPD assocerà identificatori diversi ai due diversi tentativi di accesso. Sembra che ci siano due diversi abbonati che accedono al contenuto per i programmatori (L e M nella figura).

* Per gli MVPD standard, se l’abbonato B accede a contenuti offerti da due programmatori diversi, l’MVPD assocerà un singolo identificatore di accesso per entrambi i tentativi di accesso.

* Gli MVPD (come Xfinity) in modalità isolamento non identificano in modo coerente un abbonato, anche se questo utilizza lo stesso dispositivo tra programmatori diversi.

Per evitare la distorsione dei dati causata dal conteggio di un singolo abbonato come più abbonati a causa dell&#39;accesso a diversi programmatori, la modalità isolamento limita l&#39;attività riportata su un programmatore alle sole loro applicazioni.

Ad esempio, il programmatore L può visualizzare i dati solo in base all&#39;attività delle identità W e Y, ignorando le identità X e Z nell&#39;immagine precedente.

>[!IMPORTANT]
>
> L&#39;inconveniente è che il programmatore L è privato della condivisione di informazioni raccolte sugli abbonati A e B a causa di attività con qualsiasi programmatore diverso da L.

In modalità isolamento, i punteggi condivisi e le metriche associate vengono calcolati esclusivamente dall’attività di streaming dei dispositivi dalle applicazioni del programmatore e del canale selezionati. I punteggi e le probabilità di condivisione vengono calcolati dall’avvio del flusso sui canali attualmente selezionati.

Il sistema funziona automaticamente in modalità isolamento quando il segmento selezionato contiene un MVPD in modalità isolamento che identifica i singoli abbonati come più abbonati durante lo streaming da programmatori diversi. Tutti i grafici per questi segmenti rifletteranno i risultati di questo comportamento modificato.

>[!IMPORTANT]
>
> Il comportamento in modalità isolamento non è compatibile con la modalità standard, la modalità isolamento MVPD non può essere combinata con altri MVPD e viceversa.

Per creare un segmento analizzato in modalità isolamento, trascinare MVPD modalità isolamento, ad esempio **Xfinity**, nella sezione MVPDs della definizione del segmento.

>[!NOTE]
>
> Poiché gli MVPD in modalità isolamento non possono essere combinati con altri MVPD, la sezione MVPD della definizione del segmento non consente il trascinamento di un altro MVPD.

![](assets/xfinity-in-segment.png)

*Selezione di Xfinity in modalità isolamento*

>[!IMPORTANT]
>
> La condivisione dell’account è più rilevante se misurata per lo streaming tra tutte le applicazioni del programmatore. Aspettati un valore inferiore **Condivisione dei punteggi** e alcune variazioni nelle metriche in modalità isolamento.

![](assets/aggregate-sharing-isolation.png)

*Condivisione dei misuratori di probabilità in modalità isolamento*

I contatori di cui sopra mostrano che solo il 9% di tutti gli account è condiviso e tra questi solo l&#39;11% del contenuto è consumato. A causa dei punteggi naturalmente più bassi, i risultati in modalità isolamento devono essere interpretati in modo diverso dai risultati in modalità standard.
