---
title: Panoramica dei riferimenti API
description: Riferimento completo per l’API di monitoraggio della concorrenza, inclusi endpoint, autenticazione e formati di risposta
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# Panoramica dei riferimenti API {#api-reference-overview}

L’API di monitoraggio della concorrenza fornisce un’interfaccia RESTful per gestire le sessioni di streaming e applicare i criteri di utilizzo simultanei. Questa guida fornisce una documentazione completa per tutti gli endpoint, i metodi di autenticazione, i formati di richiesta/risposta e la gestione degli errori.

## URL di base API

### Ambiente di produzione

```
https://streams.adobeprimetime.com/v2/
```

### Ambiente di staging

```
https://streams-stage.adobeprimetime.com/v2/
```

**Nota:** utilizza sempre l&#39;ambiente di gestione temporanea per lo sviluppo e il testing. Le credenziali di produzione vengono fornite solo dopo l’integrazione della gestione temporanea.

## Autenticazione

Tutte le chiamate API richiedono l’autenticazione HTTP Basic utilizzando le credenziali dell’applicazione:

- **Nome utente:** ID applicazione (fornito da Adobe)
- **Password:** Stringa vuota

### Esempio di intestazione di autenticazione

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## Standard del formato di risposta

### Risposte di successo

Tutte le risposte di successo seguono questa struttura:

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Risposte di errore

Tutte le risposte di errore seguono questa struttura:

```json
{
  "associatedAdvice": [
    {
      "policyName": "string",
      "ruleName": "string",
      "scope": {},
      "attribute": "string",
      "threshold": 0,
      "conflicts": [
        {}
      ]
    }
  ],
  "obligations": [
    {
      "namespace": "string",
      "action": "string",
      "arguments": [
        "string"
      ]
    }
  ]
}
```

### Formato risultati valutazione

Quando vengono valutati i criteri (in particolare per i conflitti 409), le risposte includono un risultato di valutazione:

```json
{
  "evaluationResult": {
    "decision": "DENY",
    "obligations": [
      {
        "id": "obligation-id",
        "fulfillOn": "DENY",
        "attributes": {
          "attribute1": "value1"
        }
      }
    ],
    "associatedAdvice": [
      {
        "id": "advice-id",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "rule-name",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Codici di stato HTTP comuni

| Codice | Descrizione | Quando Restituito |
|------|----------------------|------------------------------------------------|
| 200 | OK | Richieste GET riuscite |
| 202 | Creato/accettato | Creazione della sessione/heartbeat registrato correttamente |
| 400 | Richiesta non valida | Parametri non validi o campi obbligatori mancanti |
| 401 | Non autorizzato | Autenticazione non valida o mancante |
| 403 | Non consentito | Autorizzazioni insufficienti |
| 404 | ID sessione non trovato | ID sessione non generato dal servizio CM |
| 409 | Conflitto | Violazione dei criteri (limite simultaneo raggiunto) |
| 410 | Non più | Sessione scaduta o terminata |
| 429 | Troppe richieste | Limite di frequenza superato |

## Metodi di passaggio dei parametri

### Parametri percorso

Parametri obbligatori che fanno parte del percorso URL:

1. `{idp}` - Identificatore provider di identità
2. `{subject}` - Identificatore utente (in genere da Adobe Pass)
3. `{sessionId}` - Identificatore sessione (restituito nell&#39;intestazione Posizione)

### Parametri aggiuntivi

I parametri facoltativi vengono passati nell’URL:

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### Dati modulo (POST/PUT)

Metadati e dati della sessione nel corpo della richiesta:

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### Intestazioni

Parametri speciali trasmessi nelle intestazioni HTTP:

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## Best practice per la gestione degli errori

### Gestione dei conflitti 409

Quando ricevi una risposta di conflitto 409:

1. **Analizzare il risultato della valutazione** per comprendere la violazione dei criteri
2. **Estrai informazioni sul conflitto** da `associatedAdvice`
3. **Presentare le opzioni all&#39;utente** in base alla strategia LIFO/FIFO
4. **Utilizzare i codici di terminazione** se si implementa il comportamento LIFO

### 410 Gone Handling

Quando si riceve una risposta 410 Gone:

1. **Verifica se la risposta ha un corpo** - indica la terminazione remota
2. **Analizza il consiglio** per capire perché la sessione è stata terminata
3. **Aggiorna l&#39;interfaccia utente** per riflettere la chiusura della sessione
4. **Gestione corretta** - la sessione potrebbe essere scaduta per timeout naturale
5. **Avvia una nuova sessione**. Se ritenuto appropriato, avviare una nuova sessione

### Limitazione di tariffa

Quando si riceve un 429 Troppe richieste:

1. **Limita la frequenza di chiamata** a un massimo di 200 richieste al minuto, che è il livello massimo accettato da CM
2. **Invia heartbeat all&#39;intervallo richiesto**, ovvero uno al minuto.

## Strumenti di test

### API Explorer interattiva

Utilizza la nostra [interfaccia utente Swagger](https://streams-stage.adobeprimetime.com/swagger-ui/index.html) per i test interattivi:

1. Inserisci l’ID applicazione nell’angolo in alto a destra
2. Fai clic su &quot;Esplora&quot; per impostare l’autenticazione
3. Endpoint di test con parametri reali
4. Visualizza esempi di richieste/risposte
