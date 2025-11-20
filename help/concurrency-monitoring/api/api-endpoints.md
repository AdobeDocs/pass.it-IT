---
title: Endpoint API
description: Elenco completo delle API di monitoraggio della concorrenza
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
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
