---
title: Modelli di implementazione
description: Modelli di implementazione
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 0%
---
# Modelli di implementazione {#imp-models}

## Criteri lato server {#ss-policies}

Questo modello utilizzerà il CM come punto decisionale politico, delegando in tal modo la decisione di accesso al servizio.

Poiché il client non deve fare alcuna supposizione riguardo ai criteri applicati, l’implementazione deve verificare la decisione sull’inizializzazione della sessione e a intervalli regolari, durante la riproduzione dalla risposta heartbeat.
