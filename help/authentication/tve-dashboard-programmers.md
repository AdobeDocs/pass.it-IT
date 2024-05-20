---
title: Programmatori
description: Scopri i programmatori e le relative configurazioni nel dashboard di TVE.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Programmatori {#programmers}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Il **Programmatori** della Dashboard TVE consente di visualizzare e gestire le impostazioni per [programmatori](/help/authentication/glossary.md#programmer) collegati ai diritti del tuo account. È inoltre possibile [aggiungi un nuovo programmatore](#add-new-programmer) in base alle tue esigenze.

Il **Programmatori** nel pannello a sinistra viene visualizzato un elenco di programmatori esistenti con i seguenti dettagli:

* **ID programmatore**: identificatore della società di media all’interno del sistema.
* **Canali**: numero di canali associati collegati a un programmatore.

![Elenco dei programmatori esistenti](assets/programmers-list.png)

*Elenco dei programmatori esistenti*

Digita il nome del programmatore in **Ricerca** sopra l&#39;elenco per ulteriori informazioni su un programmatore.

## Gestione configurazioni programmatore {#manage-programmer-conf}

Segui questi passaggi per gestire varie impostazioni di un programmatore specifico.

1. Seleziona la **Programmatori** nel pannello sinistro.
1. Selezionare un programmatore dall&#39;elenco.
1. Selezionare una delle schede seguenti per visualizzare e modificare le impostazioni corrispondenti del programmatore selezionato:

   * [Canali](#channels)
   * [Certificati](#certificates)
   * [Applicazioni registrate](#registered-applications)
   * [Schemi personalizzati](#custom-schemes)

   ![Impostazioni del programmatore](assets/programmer-settings.png)

   *Impostazioni del programmatore*

>[!IMPORTANT]
>
> Visualizza [Revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md) per ulteriori informazioni sull&#39;attivazione delle modifiche alla configurazione.

### Canali {#channels}

Questa scheda visualizza un elenco di canali collegati a un programmatore corrente. Selezionare un canale specifico da questo elenco per accedere alle informazioni dettagliate nella [Canali](/help/authentication/tve-dashboard-channels.md) sezione.

Per aggiungere un nuovo canale per il programmatore selezionato, selezionare **Aggiungi nuovo canale** dall&#39;angolo superiore destro di **Canali disponibili** sezione. Scopri [come aggiungere un nuovo canale](/help/authentication/tve-dashboard-channels.md#add-new-channel).

![Aggiungi un nuovo canale](assets/programmers-channels.png)

*Aggiungi un nuovo canale*

### Certificati {#certificates}

Questa scheda visualizza un elenco di [certificati disponibili](#available-certificates) utilizzato nei flussi di crittografia dei metadati utente. Vengono visualizzati i dettagli di ogni certificato, tra cui:

* Lo stato (se abilitato per **crittografia metadati utente** (utilizzato o meno)
* Numero di serie
* Nome dell&#39;organizzazione emittente
* Nome dell&#39;organizzazione soggetto
* Data di emissione
* Data di scadenza
* Menu a discesa per crittografare i metadati utente (se si seleziona **Sì**, il certificato crittograferà le informazioni utente riservate, ad esempio i valori del codice postale).

#### Certificati disponibili {#available-certificates}

Questi certificati fungono da chiavi private o pubbliche e vengono utilizzati per la crittografia dei metadati utente. Tutti i canali associati alla stessa società di media possono utilizzare questi certificati.

È possibile apportare le seguenti modifiche ai certificati disponibili:

* [Aggiungi nuovo certificato](#add-new-certificate)
* [Elimina certificato](#delete-certificate)

##### Aggiungi nuovo certificato {#add-new-certificate}

Per aggiungere un nuovo certificato, segui la procedura riportata di seguito.

1. Seleziona **Aggiungi nuovo certificato** nell&#39;angolo superiore destro del **Certificati disponibili** sezione.

   ![Aggiungi un nuovo certificato](assets/programmer-add-new-certificate.png)

   *Aggiungi un nuovo certificato*

1. Incolla la chiave pubblica del certificato in **Nuovo certificato** .
1. Seleziona **Aggiungi certificato**.

   È stata creata una nuova modifica alla configurazione ed è pronto per l’aggiornamento del server. Per utilizzare il nuovo certificato elencato in **Certificati disponibili** , procedere con [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md) flusso.

1. Individua il nuovo certificato nell’elenco di **Certificati disponibili**.

   >[!IMPORTANT]
   >
   > Assicurati che i sistemi siano aggiornati e pronti per l’utilizzo del nuovo certificato.

1. Seleziona **Sì** da **Utilizzato per crittografare i metadati dell’utente** per attivare un nuovo certificato.

##### Elimina certificato {#delete-certificate}

Per eliminare un certificato, segui la procedura riportata di seguito.

1. Passa il puntatore del mouse sul certificato da eliminare dall’elenco **Certificati disponibili**.
1. Seleziona **Rimuovi**.

   ![Rimuovi il certificato selezionato](assets/programmer-remove-certificate.png)

   *Rimuovi il certificato selezionato*

1. Seleziona **Elimina** il **Elimina certificato** .

È stata creata una nuova modifica alla configurazione ed è pronto per l’aggiornamento del server. Il certificato verrà eliminato dal **Certificati disponibili** sezione solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).

### Applicazioni registrate {#registered-applications}

Questa scheda fornisce un elenco delle registrazioni delle applicazioni. Visualizza [Gestione registrazione client dinamici](/help/authentication/dynamic-client-registration-management.md), per ulteriori informazioni.

### Schemi personalizzati {#custom-schemes}

In questa scheda viene visualizzato un elenco di schemi personalizzati. Visualizza [Registrazione applicazione iOS/tvOS](/help/authentication/iostvos-application-registration.md) e [Gestione registrazione client dinamici](/help/authentication/dynamic-client-registration-management.md), per ulteriori informazioni.

## Aggiungi nuovo programmatore {#add-new-programmer}

Per aggiungere una nuova entità programmatore, segui la procedura riportata di seguito.

1. Seleziona la **Programmatori** nel pannello sinistro.
1. Seleziona **Aggiungi nuovo programmatore** nell&#39;angolo superiore destro del **Programmatori** sezione.

   ![Aggiungi un nuovo programmatore](assets/add-new-programmer.png)

   *Aggiungi un nuovo programmatore*

1. Digita l’identificatore dell’azienda multimediale in **ID programmatore** nel **Nuovo programmatore** .
1. Digita un nome commerciale da mostrare nella console in **Nome visualizzato**.
1. Seleziona **Aggiungi programma**.

È stata creata una nuova modifica alla configurazione ed è pronto per l’aggiornamento del server. Per utilizzare il nuovo programmatore elencato nella **Programmatori** , procedere con [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md) flusso.

