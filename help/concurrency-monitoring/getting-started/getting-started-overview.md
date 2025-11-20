---
title: Introduzione al monitoraggio della concorrenza
description: Scopri le nozioni di base sul monitoraggio della concorrenza e come iniziare a utilizzare l’integrazione
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Introduzione al monitoraggio della concorrenza {#getting-started-overview}

Monitoraggio della concorrenza Questa guida ti aiuterà a comprendere le nozioni di base e a rendere operativa rapidamente la tua integrazione.

## Il monitoraggio della concorrenza dei problemi risolve {#problem-solved}

Nell’attuale scenario di streaming, gli abbonati si aspettano di accedere ai contenuti su più dispositivi e applicazioni. Tuttavia, questo crea problemi ai fornitori di contenuti:

- **I contratti di licenza per i contenuti** spesso limitano il numero di flussi simultanei
- **I costi della larghezza di banda** aumentano con un utilizzo simultaneo eccessivo
- **Esperienza utente** si verifica quando troppi flussi peggiorano le prestazioni
- **La protezione dei ricavi** richiede di impedire gli abusi nella condivisione degli account

Il monitoraggio della concorrenza fornisce una soluzione centralizzata a queste sfide.


## Funzionamento del monitoraggio della concorrenza

### Funzionalità di base

Il monitoraggio della concorrenza funziona come un **servizio di gestione delle sessioni** che:

1. **Tiene traccia delle sessioni di streaming attive** in tempo reale in tutte le applicazioni
2. **Valuta i criteri aziendali** in base ai criteri di utilizzo correnti
3. **Applica limiti** quando i criteri vengono violati
4. **Fornisce opzioni di risoluzione** quando si verificano conflitti

## Vantaggi per le diverse parti interessate

### Provider di contenuti (programmatori)

- **Proteggi i diritti sui contenuti** e rispetta i contratti di licenza
- **Ottimizzare i costi della larghezza di banda** impedendo un utilizzo eccessivo
- **Migliora l&#39;esperienza utente** con un feedback chiaro sui limiti
- **Ottieni informazioni sull&#39;utilizzo** tramite la generazione di rapporti dettagliati

### Provider di identità (MVPD)

- **Applica contratti sottoscrittori** per tutti i partner di contenuto
- **Gestione centralizzata dei criteri** per più applicazioni
- **Monitoraggio in tempo reale** dei modelli di utilizzo
- **Architettura scalabile** che gestisce milioni di sessioni

### Utenti finali

- **Cancella comprensione** dei loro limiti di utilizzo
- **Esperienza coerente** in tutte le applicazioni
- **Opzioni per risolvere i conflitti** al raggiungimento dei limiti
- **Feedback trasparente** sul motivo per cui l&#39;accesso è limitato


## Percorso introduttivo {#quick-start-path}

1. **Rivedi concetti chiave** - Scopri di più su [sessioni, criteri e metadati](key-concepts.md)
2. **Scegli la tua strategia** - Rivedi le [strategie LIFO e FIFO](../use-cases/lifo-fifo-strategies.md)
3. **Inizia a implementare** - Segui il [Riferimento API](../api/api-reference-overview.md)

## Intendete integrare? {#ready-to-integrate}

- [Riferimento API](../api/api-reference-overview.md)
- [Casi d’uso](../use-cases/cm-use-cases.md)


## Registrazione servizio {#service-registration}

Per iniziare a utilizzare il monitoraggio della concorrenza, contatta il nostro [team di supporto](mailto:tve-support@adobe.com) con le seguenti informazioni:

1. **Nome società** e dettagli di contatto
2. **Applicazioni** da integrare con il monitoraggio concorrenza. Per ciascuna domanda, fornire:
   1. Nome applicazione
   2. Piattaforme applicazione
3. **Partner di integrazione** (nel caso in cui si sottoscriva al monitoraggio della concorrenza su richiesta di un&#39;altra parte, programmatore o MVPD)


## Hai bisogno di assistenza? {#need-help}

- **API Explorer** - Test interattivo delle API in [Interfaccia Swagger](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)
- **Termini chiave e definizioni** - [Glossario](../cm-glossary.md)
- **Come ottenere assistenza?** - [Procedure di supporto](../support/cm-escalation-procedures.md)
- **Supporto** - Contattare [tve-support@adobe.com](mailto:tve-support@adobe.com)
