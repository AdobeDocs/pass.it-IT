---
title: Gestione degli errori di conflitto 409
description: Scopri come gestire gli errori di conflitto 409 quando vengono raggiunti i limiti di utilizzo simultanei
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---


# Gestione degli errori di conflitto 409 {#handling-409-errors}

Quando un utente tenta di avviare un nuovo flusso e raggiunge un limite di utilizzo simultaneo, il monitoraggio della concorrenza restituisce una risposta **409 Conflict**. Comprendere come gestire questo errore è fondamentale per fornire una buona esperienza utente.

## Cos&#39;è un conflitto 409? {#what-is-a-409-conflict}

Si verifica un conflitto 409 quando:

1. **L&#39;utente tenta di avviare un nuovo flusso**
2. **La valutazione dei criteri determina** che la richiesta supererebbe i limiti
3. **Il sistema restituisce 409** con informazioni dettagliate sul conflitto
4. **L&#39;applicazione deve decidere** come gestire il conflitto

### Struttura di risposta 409

```json
{
  "status": "error",
  "error": {
    "code": "POLICY_VIOLATION",
    "message": "Concurrent usage limit exceeded"
  },
  "evaluationResult": {
    "decision": "DENY",
    "associatedAdvice": [
      {
        "id": "advice-1",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1",
                "contentType": "live"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Informazioni sulla risposta {#understanding-the-response}

### Campi chiave

- **`decision`**: sempre &quot;NEGA&quot; per conflitti 409
- **`associatedAdvice`**: Array di oggetti di suggerimento che spiegano la violazione
- **`conflicts`**: elenco delle sessioni attive che causano il conflitto
- **`terminationCode`**: codice univoco per terminare una sessione specifica

### Tipi di consigli

#### Avviso di violazione regola

```json
{
  "adviceType": "rule-violation",
  "attributes": {
    "rule": "max_streams",
    "threshold": 3,
    "current": 4,
    "conflicts": [...]
  }
}
```

#### Avviso di terminazione remota

```json
{
  "adviceType": "remote-termination",
  "attributes": {
    "terminatedBy": "session-456",
    "reason": "New session requested with X-Terminate header"
  }
}
```

## Strategie di gestione {#handling-strategies}

- In modalità FIFO, CM consente di avviare la nuova sessione interrompendo una sessione esistente.
- In modalità LIFO, CM blocca la nuova sessione e informa l’utente


## Best practice {#best-practices}

### &#x200B;1. Cancella comunicazione utente

- **Spiega il limite** - Gli utenti dovrebbero capire perché sono bloccati
- **Mostra opzioni disponibili** - Operazioni possibili per risolvere il conflitto
- **Fornisci contesto** - Mostra quali sessioni sono attive

### &#x200B;2. Gestione corretta degli errori

- **Non arrestare** - Gestisci gli errori 409 in modo corretto
- **Fornisci alternative** - Offri modi per risolvere il conflitto
- **Salva stato utente** - Non perdere la selezione del contenuto

### &#x200B;3. Considerazioni sull’esperienza utente

- **Risoluzione rapida** - Semplificare la risoluzione dei conflitti
- **Cancella scelte** - Gli utenti devono comprendere le proprie opzioni
- **Comportamento coerente** - Gestisci i conflitti sempre allo stesso modo

### &#x200B;4. Considerazioni tecniche

- **Analizzare attentamente la risposta** - Estrarre tutte le informazioni rilevanti
- **Gestione dei casi edge** - Cosa succede se non vengono restituiti conflitti?
- **Registra conflitti** - Tieni traccia delle violazioni dei criteri per l&#39;analisi


