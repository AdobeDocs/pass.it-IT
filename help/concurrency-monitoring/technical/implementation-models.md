---
title: Modelli di implementazione
description: Modelli di implementazione
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---

# Modelli di implementazione {#imp-models}

## Criteri lato server {#ss-policies}

Questo modello utilizzerà il CM come punto decisionale politico, delegando in tal modo la decisione di accesso al servizio.

Poiché il client non deve fare alcuna supposizione riguardo ai criteri applicati, l’implementazione deve verificare la decisione sull’inizializzazione della sessione e a intervalli regolari, durante la riproduzione dalla risposta heartbeat.
