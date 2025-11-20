---
title: Concetti chiave
description: Scopri i concetti fondamentali del monitoraggio della concorrenza, tra cui sessioni, criteri, metadati e altro ancora
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---


# Concetti chiave {#key-concepts}

Comprendere i concetti fondamentali del monitoraggio della concorrenza è essenziale per una corretta implementazione. Questa guida illustra gli elementi di base e il modo in cui funzionano insieme.

## Concetti di base {#core-concepts}

### Sessioni

Una **sessione** rappresenta un&#39;istanza di streaming video attiva. Consideralo come un &quot;ticket&quot; che consente a un utente di guardare i contenuti.

#### Ciclo di vita della sessione

1. **Creazione** - Quando l&#39;utente inizia a guardare il contenuto
2. **Attivo** - Mentre l&#39;utente sta guardando (gestito da heartbeat)
3. **Terminazione** - Quando l&#39;utente smette di guardare o la sessione scade

#### Proprietà sessione

```json
{
  "sessionId": "unique-session-identifier",
  "idp": "identity-provider",
  "subject": "user-identifier",
  "startTime": "2024-01-15T10:30:00Z",
  "expiresAt": "2024-01-15T10:40:00Z",
  "metadata": {
    "deviceId": "device-123",
    "channel": "Channel1",
    "contentType": "live"
  }
}
```

### Criteri

**I criteri** sono regole aziendali che definiscono limiti e restrizioni per lo streaming simultaneo. Essi determinano quando gli utenti possono avviare nuovi flussi.

#### Componenti dei criteri

- **Regole** - Limiti e condizioni specifici
- **Requisiti metadati** - Quali dati sono necessari
- **Logica di valutazione** - Modalità di applicazione del criterio

#### Esempio di criterio

```json
{
  "name": "basic_stream_limit",
  "description": "Maximum 3 concurrent streams per user",
  "rules": [
    {
      "type": "maxstreams",
      "limit": 3,
      "message": "You have reached your maximum of 3 concurrent streams"
    }
  ]
}
```

### Metadati

**I metadati** forniscono il contesto di ogni sessione. Include informazioni sul dispositivo, sul contenuto, sull’utente e sull’applicazione.

#### Categorie metadati

| Categoria | Esempi | Finalità |
|-----------------|------------------------------------------|---------------------------------|
| **Dispositivo** | `deviceId`, `deviceType`, `osName` | Identificare e classificare i dispositivi |
| **Contenuto** | `channel`, `contentType`, `assetId` | Descrivi cosa viene guardato |
| **Utente** | `subject`, `subscriptionTier` | Informazioni specifiche per l&#39;utente |
| **Applicazione** | `applicationName`, `applicationPlatform` | Dettagli specifici dell’app |
| **Posizione** | `country`, `hba` | Informazioni geografiche e di rete |

#### Metadati obbligatori e facoltativi

- **Metadati richiesti** - Devono essere forniti per la creazione di sessioni
- **Metadati facoltativi** - Possono essere forniti per i criteri avanzati
- **Metadati standard** - Campi predefiniti disponibili per tutte le applicazioni
- **Metadati personalizzati** - Campi specifici dell&#39;applicazione definiti

## Identità e autenticazione {#identity-and-authentication}

### Provider di identità (IdP)

Un **provider di identità** è il servizio che autentica gli utenti e fornisce le relative informazioni di identità. Nell’integrazione di Adobe Pass, in genere si tratta di un MVPD.

#### Esempi di IdP

- Società via cavo (Comcast, Charter, ecc.)
- Fornitori satellitari (DirecTV, Dish)
- Servizi di streaming (Netflix, Hulu)
- Operatori mobili (Verizon, AT&amp;T)

### Oggetto

Un **oggetto** è l&#39;identificatore univoco di un utente all&#39;interno di un provider di identità. In genere si tratta dell’ID account o dell’ID dell’abbonato dell’utente.

## Gestione delle sessioni {#session-management}

### Heartbeats

**Gli heartbeat** sono chiamate API periodiche che mantengono attive le sessioni. Dicono al sistema che &quot;l&#39;utente sta ancora guardando&quot;.

#### Requisiti Heartbeat

- **Frequenza** - Ogni 60 secondi (consigliato)
- **Scopo** - Mantenere attiva la sessione, aggiornare i metadati
- **Terminazione** - La sessione scade se gli heartbeat si arrestano


### Terminazione sessione

Le sessioni possono essere terminate in diversi modi:

1. **L&#39;utente non guarda più** - L&#39;applicazione chiama l&#39;endpoint DELETE
2. **Scadenza sessione** - Nessun heartbeat ricevuto entro il timeout
3. **Violazione dei criteri** - La nuova sessione termina la precedente (FIFO)
4. **Terminazione remota** - Un altro dispositivo termina la sessione

## Valutazione dei criteri {#policy-evaluation}

### Funzionamento dei criteri

1. **Richieste utente nuovo flusso**
2. **Il sistema valuta i criteri** in base all&#39;utilizzo corrente
3. **Decisione effettuata** - Consenti o nega
4. **Risposta inviata** - Informazioni sull&#39;esito positivo o negativo

## Risoluzione dei conflitti {#conflict-resolution}

### Che cos&#39;è un conflitto?

Un **conflitto** si verifica quando un utente tenta di avviare un nuovo flusso ma supera i propri limiti.

### Risposta al conflitto

Quando si verifica un conflitto, il sistema restituisce una risposta di conflitto 409 con informazioni dettagliate:

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
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {...}
            }
          ]
        }
      }
    ]
  }
}
```

### Strategie di risoluzione

#### LIFO (ultimo ingresso, primo uscita)

- **Le sessioni precedenti sono protette**
- **Nuova sessione bloccata**
- **Interfaccia utente più semplice**
- **Migliore per la visualizzazione estesa**


#### FIFO (primo ingresso, primo uscita)

- **La nuova sessione termina una sessione esistente**
- **L&#39;utente sceglie la sessione da interrompere**
- **È richiesta un&#39;interfaccia utente più complessa**
- **Migliore per cambio contenuto**

## Applicazioni e tenant {#applications-and-tenants}

### Applicazione

Un&#39;applicazione **application** è un programma software che si integra con il monitoraggio della concorrenza. Ogni applicazione ha un ID univoco e può avere i propri criteri.

#### Esempi di applicazioni

- App mobile (iOS/Android)
- Applicazione web
- App Smart TV
- Applicazione set-top box

### Tenant

Un **tenant** è un&#39;organizzazione proprietaria di una o più applicazioni. I tenant possono condividere i criteri tra le applicazioni.

#### Struttura tenant

```
Tenant: "Streaming Company"
├── Application: "Mobile App"
├── Application: "Web App"
└── Application: "TV App"
```

## Ambienti {#environments}

### Ambiente di staging

- **Finalità** - Sviluppo e test
- **URL** - `https://streams-stage.adobeprimetime.com/v2/`
- **Credenziali** - Verifica ID applicazione
- **Dati** - Solo dati di prova

### Ambiente di produzione

- **Scopo** - Traffico utente live
- **URL** - `https://streams.adobeprimetime.com/v2/`
- **Credenziali** - ID applicazione di produzione
- **Dati** - Dati utente reali

## Flusso di integrazione {#integration-flow}

### Flusso di base

1. **Autenticazione utente** - Autenticazione con Adobe Pass
2. **Creazione sessione** - Crea sessione CM con metadati utente
3. **Gestione heartbeat** - Invia heartbeat periodici
4. **Terminazione sessione** - Termina quando l&#39;utente smette di guardare

### Gestione degli errori

1. **404 non trovato** - Gestisce le richieste con ID sessione non generato dal servizio CM
2. **409 Conflitti** - Gestisce le violazioni dei criteri
3. **410 non più disponibile** - Gestisce la chiusura della sessione
4. **Errori di rete** - Gestione dei problemi di connettività
5. **Errori di autenticazione** - Gestione dei problemi relativi alle credenziali


## Terminologia comune {#common-terminology}

| Termine | Definizione |
|-----------------|--------------------------------------------|
| **Sessione** | Istanza di streaming video attivo |
| **Criterio** | Regola di business che definisce i limiti di utilizzo |
| **Metadati** | Informazioni contestuali sulle sessioni |
| **IDP** | Provider di identità (servizio di autenticazione) |
| **Oggetto** | Identificatore utente all&#39;interno di un IDP |
| **Heartbeat** | Chiamata periodica per mantenere attiva la sessione |
| **Conflitto** | Violazione dei criteri all’avvio di un nuovo flusso |
| **LIFO** | Risoluzione dei conflitti Last In, First Out |
| **FIFO** | Risoluzione dei conflitti Primo in, Primo in uscita |
| **Tenant** | Applicazioni proprietarie dell’organizzazione |
| **Applicazione** | Programma software con CM |

