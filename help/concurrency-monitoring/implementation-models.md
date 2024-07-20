---
title: Modelli di implementazione
description: Modelli di implementazione
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---

# Modelli di implementazione {#imp-models}

## Criteri lato server {#ss-policies}

Questo modello utilizzerà il CM come punto decisionale politico, delegando in tal modo la decisione di accesso al servizio.

Poiché il client non deve fare alcuna supposizione riguardo ai criteri applicati, l’implementazione deve verificare la decisione sull’inizializzazione della sessione e a intervalli regolari, durante la riproduzione dalla risposta heartbeat.
