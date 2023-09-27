---
title: Come distinguere tra VOD e contenuti live nel monitoraggio della concorrenza
description: Come distinguere tra VOD e contenuti live nel monitoraggio della concorrenza
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---


# Procedura: Distinguere tra VOD e contenuti live nel monitoraggio della concorrenza {#dist-vod-live}

**D:** Il servizio di monitoraggio della concorrenza è in grado di distinguere tra il tipo di contenuto riprodotto (contenuti live e video on-demand)?



**R:** Il monitoraggio della concorrenza non è in grado di distinguere direttamente tra contenuti live e video on-demand (VOD). Il lettore video deve conoscere il tipo di contenuto che sta riproducendo e inviare queste informazioni durante il [chiamata di inizializzazione della sessione](/help/concurrency-monitoring/cm-api-overview.md#session-initial) (obbligatorio per il monitoraggio della concorrenza). Il flusso di lavoro normale si presenta così:

1. I clienti di Monitoraggio della concorrenza definiscono un set di metadati su cui desiderano implementare le regole (ad esempio content-type=live|vod, device-type=mobile|console|desktop).
1. Il team di monitoraggio della concorrenza implementa i criteri desiderati. Esempio:
   1. se content-type=live, max streams=3, ultime vittorie
   1. se content-type=vod, max streams=1, ultime vittorie

1. Quando l’utente finale riproduce il contenuto, il lettore video deve inviare i valori per i campi di metadati che sono stati stabiliti come parte di un criterio.

1. Il servizio di monitoraggio della concorrenza, basato sul criterio definito e sui valori ricevuti, emetterà una decisione (riproduzione/interruzione).

1. Affinché il sistema funzioni, la decisione deve essere rispettata dal lettore video.



## Informazioni correlate {#related-info-vod-live-dist}

* [Attributi metadati standard per il monitoraggio della concorrenza](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [Panoramica API di monitoraggio della concorrenza](/help/concurrency-monitoring/cm-api-overview.md)
