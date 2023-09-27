---
title: Modelli di implementazione
description: Modelli di implementazione
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---


# Modelli di implementazione {#imp-models}

## Criteri lato server {#ss-policies}

Questo modello utilizzerà il CM come punto decisionale politico, delegando in tal modo la decisione di accesso al servizio.

Poiché il client non deve fare alcuna supposizione riguardo ai criteri applicati, l’implementazione deve verificare la decisione sull’inizializzazione della sessione e a intervalli regolari, durante la riproduzione dalla risposta heartbeat.
