---
title: Endpoint API
description: Elenco completo delle API di monitoraggio della concorrenza
exl-id: e8a9dfd2-cd16-4971-b9bc-9646987dd3ce
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 3%
---
# Endpoint API

## Gestione delle sessioni core

| Endpoint | Metodo | Descrizione |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | POST | Crea una nuova sessione di streaming |
| `/sessions/{idp}/{subject}/{session}` | POST | Invia heartbeat per mantenere attiva la sessione |
| `/sessions/{idp}/{subject}/{session}` | DELETE | Termina una sessione |
| `/runningStreams/{idp}/{subject}` | GET | Ottieni tutte le sessioni attive per un oggetto |

## Gestione metadati

| Endpoint | Metodo | Descrizione |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | Ottieni i campi di metadati richiesti per l’applicazione |
