---
title: Introduzione al monitoraggio della concorrenza
description: Introduzione al monitoraggio della concorrenza
exl-id: 725cc64b-6b03-46e3-a038-41e9b1341c6b
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Introduzione al monitoraggio della concorrenza {#intro}

Concurrency Monitoring è un servizio che consente ai provider di contenuti e ai provider di identità (MVPD e Programmatori) di definire e applicare limiti allo streaming video simultaneo su più applicazioni, dispositivi e piattaforme. Sia che si tratti di un programmatore che cerca di controllare il numero di flussi che un abbonato può guardare simultaneamente, o di un MVPD che desidera applicare criteri di utilizzo tra i partner di contenuti, il monitoraggio della concorrenza fornisce gli strumenti necessari.

## Cos&#39;è il monitoraggio della concorrenza? {#what-is-cm}

Concurrency Monitoring è un servizio centralizzato che tiene traccia e gestisce sessioni di streaming video attive in tempo reale. Consente di:

- **Limita flussi simultanei per sottoscrittore** - Controlla quanti flussi video simultanei un utente può accedere
- **Applica restrizioni dispositivo** - Limita lo streaming a tipi o quantità di dispositivi specifici
- **Implementare criteri basati sulla posizione** - Limitare lo streaming in base alla posizione geografica
- **Crea regole specifiche per il contenuto** - Applica limiti diversi per contenuti live e VOD
- **Monitorare i pattern di utilizzo** - Ottieni informazioni approfondite sul modo in cui i contenuti vengono utilizzati

## Come funziona {#how-it-works}

Il monitoraggio della concorrenza funziona tramite un’API semplice ma potente:

1. **Inizializzazione sessione** - Quando un utente inizia a guardare il contenuto, l&#39;applicazione crea una sessione
2. **Valutazione dei criteri** - Il servizio valuta i criteri definiti in base all&#39;utilizzo corrente
3. **Monitoraggio in tempo reale** - Le chiamate Heartbeat mantengono le sessioni attive e monitorano la conformità

## Sei nuovo al monitoraggio della concorrenza? {#new-to-cm}

Inizia con la [Guida introduttiva](getting-started/getting-started-overview.md) per comprendere le nozioni di base e configurare la tua prima integrazione.

