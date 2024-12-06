---
title: Meccanismo di controllo dell'integrità dello storage iOS/tvOS
description: Meccanismo di controllo dell’integrità di iOS/tvOS
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 0%

---

# Meccanismo di controllo dell’integrità di iOS/tvOS {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#Intro}

A partire dalla versione 3.8.3 dell&#39;SDK iOS/tvOS AccessEnabler, l&#39;opzione di esecuzione dei controlli di integrità dell&#39;archiviazione è disponibile all&#39;inizializzazione di AccessEnabler.

Per utilizzare questo meccanismo, l’API è stata estesa con un metodo di inizializzazione aggiuntivo per la classe AccessEnabler.

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## Controlli di integrità {#Checks}

I controlli di integrità dello storage sono utili quando si sospetta un danneggiamento dello storage AccessEnabler (ad esempio quando si verifica una situazione di tipo &quot;race condition&quot; durante un&#39;operazione di archiviazione in lettura/scrittura).

Sono disponibili i controlli seguenti da eseguire sull&#39;inizializzazione di AccessEnabler:
- Operabilità dello storage: verifica del successo delle operazioni di lettura e scrittura
- Integrità dei valori memorizzati: verifica che tutti i valori siano validi e nel formato previsto

>[!IMPORTANT]
> 
>Se uno dei controlli non riesce, tutti i valori nell’archiviazione vengono cancellati e l’utente viene disconnesso, il che potrebbe causare un’esperienza utente errata. Utilizzare i controlli di integrità dello storage solo quando ritenuto necessario.


## Comportamento predefinito {#Default}

I controlli di integrità dell&#39;archiviazione vengono disattivati per impostazione predefinita quando si inizializza AccessEnabler utilizzando il metodo di inizializzazione predefinito:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

Per specificare in modo esplicito i controlli di integrità dello storage da eseguire all&#39;inizializzazione di AccessEnabler, utilizzare il metodo di inizializzazione seguente:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## TipoControlloIntegrità {#Switcher}

L&#39;enumerazione IntegrityCheckType è esposta all&#39;applicazione client e presenta i valori seguenti:

| Valore | Controlli eseguiti | Archiviazione cancellata | Descrizione | Caso d’uso consigliato |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | Nessuno | Mai | Nessun controllo di integrità eseguito all&#39;inizializzazione dell&#39;archiviazione | Quando i flussi SDK funzionano come previsto |
| INTEGRITY_CHECK_ALL | Operabilità archiviazione <br/> Validità dei valori archiviati | Errore al momento dell’assegno | Tutti i controlli di integrità disponibili vengono eseguiti all&#39;inizializzazione dell&#39;archiviazione | Quando si sospetta un danneggiamento dell’archiviazione SDK. <br/> In caso di errore di uno dei controlli di integrità, l&#39;utente verrà disconnesso |
| INTEGRITY_CHECK_CLEAR | Nessuno | Sempre | L&#39;archiviazione viene cancellata durante l&#39;inizializzazione dell&#39;archiviazione | Quando i flussi SDK non possono essere completati come previsto |
