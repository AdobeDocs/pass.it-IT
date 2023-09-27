---
title: Criteri di conservazione dei dati
description: Criteri di conservazione dei dati
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# Criteri di conservazione dei dati {#data-retention-policy}

>[!WARNING]
>
>**Avviso:** Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.


## Introduzione {#introduction}

Adobe, in qualità di titolare del trattamento dei dati, deve adottare misure adeguate per assistere la propria clientela nel rispondere alle richieste di accesso, cancellazione e di altro genere da parte di singoli individui. L’applicazione di criteri di eliminazione appropriati, sicuri e tempestivi è una parte importante del rispetto di tale obbligo.

## Definizioni {#definitions}

Un criterio di conservazione dei dati determina per quanto tempo Adobe memorizza i dati del cliente. Il criterio di conservazione dei dati predefinito per il monitoraggio della concorrenza è **25 mesi**.

| Periodo di conservazione dei dati | Il periodo di conservazione dei dati è il periodo predefinito di conservazione dei dati (25 mesi). |
|---|---|
| **Finestra di conservazione dei dati** | La finestra di conservazione dei dati definisce i parametri per i quali è possibile visualizzare e segnalare dati completi. La finestra di conservazione dei dati viene determinata come segue:<br/> *Data di inizio* = data corrente - periodo di conservazione dei dati <br/>*Data di fine* = data corrente |

## Raccolta dati {#data-collection}

*Dati di click-stream* rappresenta i dati condivisi dai clienti sugli heartbeat della sessione (ad esempio, subjectID, mvpdName e metadati). Tutti i campi di metadati personalizzati sono indicati in [Attributi metadati standard](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Tipi di clienti {#customer-types}

### Clienti attuali {#current-customers}

A meno che il cliente non acquisti estensioni di conservazione dei dati, il monitoraggio della concorrenza sarà conforme ai seguenti requisiti di conservazione dei dati del cliente:

* *Dati di click-stream* raccolte da Monitoraggio concorrenza devono essere eliminate entro **25 mesi** dalla data di raccolta.

### Clienti terminati {#terminated-customers}

Un cliente cessato è un cliente che ha terminato la relazione con Adobe e non utilizza più il monitoraggio della concorrenza.

* *Dati di click-stream* raccolte da Concurrency Monitoring devono essere eliminate entro **6 mesi** dalla data di cessazione del contratto del cliente.

## Eliminazione dei dati {#data-deletion}

Adobe si riserva il diritto di eliminare i dati per le date successive al periodo di conservazione dei dati senza possibilità di recupero. I dati per i clienti attuali dovrebbero essere cancellati su base mensile continua.

