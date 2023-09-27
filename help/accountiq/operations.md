---
title: Operazioni nell’account IQ
description: Le operazioni in Account IQ comportano l’esecuzione di azioni per eseguire automazioni e operazioni in blocco sugli account degli abbonati e tenere traccia dei loro effetti.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 0%

---

# Operazioni {#operations-tab-next-steps}

Dopo aver compreso i pattern di utilizzo degli abbonati e identificato la condivisione delle password per il segmento selezionato (utilizzando [!DNL Analytics] in Account IQ), puoi intraprendere azioni mirate verso un obiettivo per mitigare la condivisione delle password.

La funzionalità Operazioni in Account IQ consente di gestire e gestire in modo efficace la condivisione delle credenziali tramite procedure mirate denominate operazioni. Consente di progettare azioni mirate, mirate e obiettive (basate sull’obiettivo) per gruppi specifici di account di abbonati e di automatizzarne l’esecuzione per una durata futura. Grazie alla funzionalità Operazioni, è possibile non solo creare ed eseguire operazioni, ma anche misurarne l&#39;impatto. Quindi, misurando gli impatti è possibile regolare la strategia per ottimizzare l&#39;effetto, sia che si tratti della conversione dei mutuatari o della mitigazione della condivisione delle credenziali.

Per visualizzare **Operazioni** selezione pagina **Operazioni** opzione in **Azioni** nella navigazione a sinistra dell’applicazione Account IQ. Nella pagina Operazioni sono elencate tutte le operazioni già esistenti nel sistema Account IQ e i relativi dettagli.

![](assets/operations-page.png)

*Figura: Elenco e dettagli delle operazioni esistenti nell’Account IQ*

Nella pagina Operazioni è possibile effettuare le operazioni riportate di seguito.

* Visualizza un elenco di operazioni già esistenti nell’Account IQ

* Visualizzare i dettagli dell&#39;operazione, ad esempio:

   * Stato (Pianificato, In esecuzione, Terminato, Errore o Arrestato)

   * avanzamento (percentuale di completamento)

   * pubblico di destinazione (segmento su cui eseguire l’operazione)

   * programma (data di inizio e fine dell&#39;operazione)

   * data di creazione e di fine dell&#39;operazione

* [Crea nuova operazione](/help/accountiq/operation-affecting-user-segment.md)

* [Visualizzare i rapporti sulle operazioni](#operation-reports)

<!--* Search from the list of operations using Search field

* Stop an operation.

* Create a duplicate operation.

* [Configure columns of Operations details page](#configure-columns)-->

## Visualizzare i rapporti sulle operazioni {#operation-reports}

È possibile analizzare l&#39;impatto di un&#39;operazione visualizzandone il report. Per visualizzare il report di un&#39;operazione:

1. Selezionare il nome dell&#39;operazione nella pagina Operazioni principali.

   Il rapporto viene visualizzato sotto forma di grafico a colonne sovrapposte.

   ![](assets/operation-impact-report.png)

   *Figura: Rapporto Operazioni per visualizzare l&#39;impatto delle operazioni*

   L&#39;asse X rappresenta il periodo di valutazione e l&#39;asse Y rappresenta l&#39;impatto dell&#39;operazione (in termini di numero di conti in un segmento durante il periodo di valutazione). Ogni barra è divisa in tre parti.

   * Una parte rappresenta il numero di conti che soddisfano ancora i criteri del segmento di operazione.

   * Un&#39;altra parte rappresenta il numero di conti attivi per quel periodo che erano originariamente nel segmento, ma che non soddisfano più i criteri del segmento di operazione.

   * La terza parte rappresenta i conti che non erano attivi in quel periodo.

   >[!NOTE]
   >
   >La prima barra rappresenta il numero di conti che soddisfano le condizioni del segmento di operazione all&#39;inizio del periodo di valutazione.

   Nel tempo il grafico mostra l’effetto dell’azione (tramite l’operazione) indicando il numero di account che hanno cambiato il loro comportamento rispetto ai criteri originali (ad esempio, con una probabilità di condivisione superiore a 90 e utilizzando più di 5 dispositivi) o che sono diventati inattivi.

<!--For example, in the above image the variable on the y-axis is number of accounts. Looking at the graph you can compare the number of accounts that are in the operations' segment versus the number of accounts that are outside the operations segment at a particular time (such as week 2nd of the operations evaluation period). Therefore, you can analyze how over the evaluation period do number of accounts vary within the operation segment and outside the segment.

So, if your operation was to send out warning emails to suspecting accounts, and accounts in operations segment were those with sharing probability more than 90 and using more than 5 devices to stream content, then in the beginning of the evaluation period accounts in segment are more than 17 thousand. This number changes over the evaluation period as shown in the graph, thereby indicating the impact of operation. Based on the evaluation, you can take remedial measures on suspecting accounts, or continue with the operation, or adjust your strategy for better outcomes to curb credential sharing.-->

1. Per chiudere il report e tornare alla pagina Operazioni principale, selezionare **Operazioni** opzione in **Azioni** nel menu di navigazione a sinistra.

<!--

![](assets/operations-details.png)

*Figure: Operation details*
## Configure columns {#configure-columns}

You can select the icon to **Configure columns** on the top of the operations table.

![](assets/config-columns.png)

*Figure: Configure columns of Operations details page*-->