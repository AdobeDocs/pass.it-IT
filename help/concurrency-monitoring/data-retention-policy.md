---
title: Criteri di conservazione dei dati
description: Criteri di conservazione dei dati
exl-id: aa7d2d5e-9a8b-404b-874c-9e5923417784
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 1%

---

# Criteri di conservazione dei dati {#data-retention-policy}

>[!WARNING]
>
>**Avviso:** il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.


## Introduzione {#introduction}

Adobe, in qualità di titolare del trattamento dei dati, deve adottare misure idonee ad assistere la propria clientela nel rispondere alle richieste di accesso, cancellazione e di altro genere da parte di singoli individui. L’applicazione di criteri di eliminazione appropriati, sicuri e tempestivi è una parte importante del rispetto di tale obbligo.

## Definizioni {#definitions}

I criteri di conservazione dei dati determinano per quanto tempo Adobe memorizza i dati dei clienti. Il criterio di conservazione dei dati predefinito per il monitoraggio della concorrenza è **25 mesi**.

| Periodo di conservazione dei dati | Il periodo di conservazione dei dati è il periodo predefinito di conservazione dei dati (25 mesi). |
|---|---|
| **Finestra di conservazione dei dati** | La finestra di conservazione dei dati definisce i parametri per i quali è possibile visualizzare e segnalare dati completi. L&#39;intervallo di conservazione dei dati è determinato come segue:<br/> *Data inizio* = data corrente - periodo di conservazione dei dati <br/>*Data fine* = data corrente |

## Raccolta dati {#data-collection}

*I dati di click-stream* rappresentano i dati condivisi dai clienti negli heartbeat della sessione (ad esempio, subjectID, mvpdName e metadati). Tutti i campi di metadati personalizzati sono indicati negli [attributi di metadati standard](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Tipi di clienti {#customer-types}

### Clienti attuali {#current-customers}

A meno che il cliente non acquisti estensioni di conservazione dei dati, il monitoraggio della concorrenza sarà conforme ai seguenti requisiti di conservazione dei dati del cliente:

* *I dati clickstream* raccolti dal monitoraggio della concorrenza devono essere eliminati entro **25 mesi** dalla data di raccolta.

### Clienti terminati {#terminated-customers}

Un cliente cessato è un cliente che ha terminato la relazione con Adobe e non utilizza più il monitoraggio della concorrenza.

* *I dati di clickstream* raccolti dal monitoraggio della concorrenza devono essere eliminati entro **6 mesi** dalla data di cessazione del contratto del cliente.

## Eliminazione dei dati {#data-deletion}

Adobe si riserva il diritto di eliminare i dati per le date successive al periodo di conservazione senza possibilità di ripristino. I dati per i clienti attuali dovrebbero essere cancellati su base mensile continua.
