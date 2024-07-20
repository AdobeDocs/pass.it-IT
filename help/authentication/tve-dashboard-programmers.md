---
title: Programmatori
description: Scopri i programmatori e le relative configurazioni nel dashboard di TVE.
exl-id: b450d7cc-d5b5-4454-8f95-8047856bfb98
source-git-commit: c2dcea9e4170a3e10654bcd3f8d2f5cdb82c9603
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Programmatori {#programmers}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

La sezione **Programmatori** della dashboard TVE consente di visualizzare e gestire le impostazioni per i [programmatori](/help/authentication/glossary.md#programmer) collegati alle adesioni del tuo account. Puoi anche [aggiungere un nuovo programmatore](#add-new-programmer) in base alle tue esigenze.

La scheda **Programmatori** nel pannello a sinistra visualizza un elenco di programmatori esistenti con i dettagli seguenti:

* **ID programmatore**: un identificatore di società di media all&#39;interno del sistema.
* **Canali**: numero di canali associati collegati a un programmatore.

![Elenco dei programmatori esistenti](assets/programmers-list.png)

*Elenco dei programmatori esistenti*

Digitare il nome del programmatore nella barra di **Ricerca** sopra l&#39;elenco per ulteriori informazioni su un programmatore.

## Gestione configurazioni programmatore {#manage-programmer-conf}

Segui questi passaggi per gestire varie impostazioni di un programmatore specifico.

1. Seleziona la scheda **Programmatori** nel pannello a sinistra.
1. Selezionare un programmatore dall&#39;elenco.
1. Selezionare una delle schede seguenti per visualizzare e modificare le impostazioni corrispondenti del programmatore selezionato:

   * [Canali](#channels)
   * [Certificati](#certificates)
   * [Applicazioni registrate](#registered-applications)
   * [Schemi personalizzati](#custom-schemes)

   ![Impostazioni programmatore](assets/programmer-settings.png)

   *Impostazioni programmatore*

>[!IMPORTANT]
>
> Visualizza [Rivedi e invia modifiche](/help/authentication/tve-dashboard-review-push-changes.md) per ulteriori informazioni sull&#39;attivazione delle modifiche alla configurazione.

### Canali {#channels}

Questa scheda visualizza un elenco di canali collegati a un programmatore corrente. Seleziona un canale specifico da questo elenco per accedere alle informazioni dettagliate nella sezione [Canali](/help/authentication/tve-dashboard-channels.md).

Per aggiungere un nuovo canale per il programmatore selezionato, selezionare **Aggiungi nuovo canale** dall&#39;angolo superiore destro della sezione **Canali disponibili**. Scopri [come aggiungere un nuovo canale](/help/authentication/tve-dashboard-channels.md#add-new-channel).

![Aggiungi un nuovo canale](assets/programmers-channels.png)

*Aggiungi un nuovo canale*

### Certificati {#certificates}

In questa scheda viene visualizzato un elenco di [certificati disponibili](#available-certificates) utilizzati nei flussi di crittografia dei metadati utente. Vengono visualizzati i dettagli di ogni certificato, tra cui:

* Lo stato (se abilitato o meno per la crittografia dei metadati dell&#39;utente ****)
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

1. Seleziona **Aggiungi nuovo certificato** nell&#39;angolo superiore destro della sezione **Certificati disponibili**.

   ![Aggiungi un nuovo certificato](assets/programmer-add-new-certificate.png)

   *Aggiungi un nuovo certificato*

1. Incolla la chiave pubblica del certificato nella finestra di dialogo **Nuovo certificato**.
1. Seleziona **Aggiungi certificato**.

   È stata creata una nuova modifica alla configurazione ed è pronto per l’aggiornamento del server. Per utilizzare il nuovo certificato elencato nella sezione **Certificati disponibili**, procedere con il flusso [revisioni e modifiche push](/help/authentication/tve-dashboard-review-push-changes.md).

1. Individua il nuovo certificato nell&#39;elenco di **Certificati disponibili**.

   >[!IMPORTANT]
   >
   > Assicurati che i sistemi siano aggiornati e pronti per l’utilizzo del nuovo certificato.

1. Seleziona **Sì** da **Utilizzato per crittografare i metadati utente** menu a discesa per attivare un nuovo certificato.

##### Elimina certificato {#delete-certificate}

Per eliminare un certificato, segui la procedura riportata di seguito.

1. Passa il puntatore del mouse sul certificato da eliminare dall&#39;elenco di **certificati disponibili**.
1. Selezionare **Rimuovi**.

   ![Rimuovi il certificato selezionato](assets/programmer-remove-certificate.png)

   *Rimuovi il certificato selezionato*

1. Selezionare **Elimina** nella finestra di dialogo **Elimina certificato**.

È stata creata una nuova modifica alla configurazione ed è pronto per l’aggiornamento del server. Il certificato verrà eliminato dalla sezione **Certificati disponibili** solo dopo [modifiche di revisione e push](/help/authentication/tve-dashboard-review-push-changes.md).

### Applicazioni registrate {#registered-applications}

Questa scheda fornisce un elenco delle registrazioni delle applicazioni. Per ulteriori informazioni, visualizzare [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md).

### Schemi personalizzati {#custom-schemes}

In questa scheda viene visualizzato un elenco di schemi personalizzati. Per ulteriori informazioni, visualizzare [Registrazione applicazione iOS/tvOS](/help/authentication/iostvos-application-registration.md) e [Gestione registrazione client dinamica](/help/authentication/dynamic-client-registration-management.md).

## Aggiungi nuovo programmatore {#add-new-programmer}

Per aggiungere una nuova entità programmatore, segui la procedura riportata di seguito.

1. Seleziona la scheda **Programmatori** nel pannello a sinistra.
1. Seleziona **Aggiungi nuovo programmatore** nell&#39;angolo superiore destro della sezione **Programmatori**.

   ![Aggiungi un nuovo programmatore](assets/add-new-programmer.png)

   *Aggiungi un nuovo programmatore*

1. Digitare l&#39;identificatore della società di supporti in **ID programmatore** nella finestra di dialogo **Nuovo programmatore**.
1. Digitare il nome del marchio commerciale che si desidera visualizzare nella console in **Nome visualizzato**.
1. Seleziona **Aggiungi programmatore**.

È stata creata una nuova modifica alla configurazione ed è pronto per l’aggiornamento del server. Per utilizzare il nuovo programmatore elencato nella sezione **Programmer**, procedere con il flusso [revisione e push modifiche](/help/authentication/tve-dashboard-review-push-changes.md).
