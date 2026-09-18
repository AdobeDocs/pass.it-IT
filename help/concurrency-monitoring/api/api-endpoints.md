---
title: Endpoint API
description: Elenco completo delle API di monitoraggio della concorrenza
exl-id: e8a9dfd2-cd16-4971-b9bc-9646987dd3ce
source-git-commit: 39384d753e7808fa433f30d8dafabd531dbf3acf
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
