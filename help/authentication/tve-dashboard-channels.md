---
title: Canali
description: Scopri i canali e le loro varie configurazioni all’interno della dashboard TVE.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 0%

---


# Canali {#channels}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Il **Canali** della Dashboard TVE consente di visualizzare e gestire le impostazioni per i canali associati a un programmatore specifico. È inoltre possibile [aggiungi un nuovo canale](#add-new-channel) in base alle tue esigenze.

Il **Canali** nel pannello a sinistra viene visualizzato un elenco di canali collegati con i seguenti dettagli:

* **Nome visualizzato**: nome del marchio del canale utilizzato a fini commerciali.
* **ID canale**: identificatore univoco, noto anche come ID richiedente.
* **Integrazioni**: numero di connessioni stabilite con [MVPD](/help/authentication/glossary.md#mvpd).

![Elenco dei canali esistenti](assets/channels-list.png)

*Elenco dei canali esistenti*

Digita il nome del canale nella **Ricerca** sopra l’elenco per ulteriori informazioni sul canale.

## Gestire le configurazioni dei canali {#manage-channel-conf}

Segui i passaggi per gestire varie impostazioni di un canale specifico.

1. Seleziona la **Canali** nel pannello sinistro.
1. Seleziona il canale dall’elenco disponibile.
1. Selezionare una delle schede seguenti per visualizzare e modificare le impostazioni corrispondenti del canale selezionato:

   * [Impostazioni generali](#general-settings)
   * [Integrazioni](#integrations)
   * [Certificati](#certificates)
   * [Domini](#domains)
   * [Applicazioni registrate](#registered-applications)
   * [Schemi personalizzati](#custom-schemes)

   ![Impostazioni canale](assets/channel-settings.png)

   *Impostazioni canale*

>[!IMPORTANT]
>
> Visualizza [Revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md) per ulteriori informazioni sull&#39;attivazione delle modifiche alla configurazione.

### Impostazioni generali {#general-settings}

Questa scheda presenta **Informazioni canale** e **Configurazione analisi**.

#### Informazioni canale {#channel-information}

In questa sezione puoi modificare i seguenti dettagli:

* **Nome visualizzato**: nome del marchio del canale utilizzato a fini commerciali.

* **URL di reindirizzamento predefinito**: URL di reindirizzamento del backup per l’autenticazione e la disconnessione.

* **Segnalazione di errori**: alla selezione **Sì**, gli SDK di Adobe Pass inviano rapporti sugli errori al backend di Adobe Pass per Analytics.

![Modifica informazioni canale](assets/channel-information.png)

*Modifica informazioni canale*

#### Configurazione di Analytics {#analytics-configuration}

Questa sezione ti consente di configurare l’inoltro di eventi di autenticazione Adobe Pass ad Adobe Analytics.

Per abilitare **Configurazione analisi**, contatta il tuo Technical Account Manager (TAM) per ulteriori informazioni sulla configurazione dell’ID suite di rapporti (RSID).

![Abilita configurazioni Analytics](assets/channel-analytical-conf.png)

*Abilita configurazioni Analytics*

Seleziona **Aggiungi nuova configurazione di analisi** per aggiungere più configurazioni.

È stata creata una nuova modifica alla configurazione ed è pronto per l’aggiornamento del server. Per utilizzare la nuova configurazione di Analytics dal **Configurazione analisi** , procedere con [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md) flusso.

### Integrazioni {#integrations}

In questa scheda viene visualizzato un elenco delle integrazioni disponibili tra il canale attualmente selezionato e gli MVPD. L’elenco presenta ogni integrazione con il relativo stato, indicando se è abilitata o meno. Seleziona un’integrazione specifica da questo elenco per accedere alle informazioni dettagliate nella sezione [Integrazioni](tve-dashboard-integrations.md) sezione.

![Elenco delle integrazioni disponibili](assets/channel-integrations.png)

*Elenco delle integrazioni disponibili*

### Certificati {#certificates}

Questa scheda visualizza un elenco di [certificati disponibili](#available-certificates) e [certificati disponibili ereditati](#inherited-avail-certificates) utilizzato nei flussi di crittografia dei metadati utente. Vengono visualizzati i dettagli di ogni certificato, tra cui:

* Lo stato (se abilitato per **crittografia metadati utente** (utilizzato o meno)
* Numero di serie
* Nome dell&#39;organizzazione emittente
* Nome dell&#39;organizzazione soggetto
* Data di emissione
* Data di scadenza
* Menu a discesa per crittografare i metadati utente (se si seleziona **Sì**, il certificato crittografa le informazioni utente riservate, ad esempio i valori del codice postale).

#### Certificati disponibili {#available-certificates}

Questi certificati fungono da chiavi private o pubbliche e vengono utilizzati per la crittografia dei metadati utente.
Puoi apportare le seguenti modifiche nella sezione certificati disponibili:

* [Aggiungi nuovo certificato](#add-new-certificate)
* [Elimina certificato](#delete-certificate)

##### Aggiungi nuovo certificato {#add-new-certificate}

Per aggiungere un nuovo certificato, effettua le seguenti operazioni:

1. Seleziona **Aggiungi nuovo certificato** nella parte superiore della **Certificati disponibili** sezione.

   ![Aggiungi un nuovo certificato](assets/add-new-certificate.png)

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

1. Passa il mouse sul certificato da eliminare dall’elenco di **Certificati disponibili**.
1. Seleziona **Rimuovi**.

   ![Rimuovi il certificato selezionato](assets/channel-delete-certificate.png)

   *Rimuovi il certificato selezionato*

1. Seleziona **Elimina** dal **Elimina certificato attivo** .

È stata creata una nuova modifica alla configurazione ed è pronto per l’aggiornamento del server. Il certificato verrà eliminato dal **Certificati disponibili** sezione solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).

#### Certificati disponibili ereditati {#inherited-avail-certificates}

Le società di media definiscono questi certificati al proprio livello. Tutti i canali associati alla stessa società di media possono utilizzare questi certificati.

![Certificati disponibili ereditati](assets/inherited-available-certificates.png)

*Certificati disponibili ereditati*

### Domini {#domains}

In questa scheda viene visualizzato un elenco dei domini disponibili attraverso i quali il rispettivo canale comunica con l’autenticazione di Adobe Pass.

Puoi apportare le seguenti modifiche ai domini:

* [Aggiungi un nuovo dominio](#add-domains)
* [Elimina dominio](#delete-domain)

>[!TIP]
>
> Evita di aggiungere un nuovo sottodominio se nell’elenco è presente un dominio più generale.

#### Aggiungi nuovo dominio {#add-domains}

Per aggiungere un dominio, segui la procedura riportata di seguito.

1. Seleziona **Aggiungi nuovo dominio** nell&#39;angolo superiore destro del **Domini disponibili** sezione.

   ![Aggiungi un nuovo dominio](assets/add-new-domain.png)

   *Aggiungi un nuovo dominio*

1. Digita il nome del dominio in **Nuovo dominio** .

1. Seleziona **Aggiungi dominio** per aggiungere un nuovo dominio per il canale selezionato.

È stata creata una nuova modifica alla configurazione ed è pronto per l’aggiornamento del server. Per utilizzare il nuovo dominio elencato in **Domini disponibili** , procedere con [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md) flusso.

#### Elimina dominio {#delete-domain}

Per eliminare un dominio, segui la procedura riportata di seguito.

1. Passa il mouse sul dominio da eliminare dall’elenco di **Domini disponibili**.
1. Seleziona **Rimuovi**.

   ![Rimuovi il dominio selezionato](assets/remove-domain.png)

   *Rimuovi il dominio selezionato*

1. Seleziona **Elimina** il **Elimina dominio** .

È stata creata una nuova modifica alla configurazione ed è pronto per l’aggiornamento del server. Il dominio verrà eliminato da **Domini disponibili** sezione solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).

Il dominio selezionato non è più disponibile. Di conseguenza, l’applicazione associata a questo dominio perde l’accesso ai servizi di autenticazione di Adobe Pass.

### Applicazioni registrate {#registered-applications}

Questa scheda fornisce un elenco delle registrazioni delle applicazioni. Visualizza [Gestione registrazione client dinamici](/help/authentication/dynamic-client-registration-management.md) per ulteriori informazioni.

### Schemi personalizzati {#custom-schemes}

In questa scheda viene visualizzato un elenco di schemi personalizzati. Visualizza [Registrazione applicazione iOS/tvOS](/help/authentication/iostvos-application-registration.md) e [Gestione registrazione client dinamici](/help/authentication/dynamic-client-registration-management.md) per ulteriori informazioni.

## Aggiungi nuovo canale {#add-new-channel}

Segui questi passaggi per aggiungere un nuovo canale.

1. Seleziona la **Canali** nel pannello sinistro.
1. Seleziona **Aggiungi nuovo canale** nell&#39;angolo superiore destro del **Canali** sezione.

   ![Aggiungi un nuovo canale](assets/add-new-channel.png)

   *Aggiungi un nuovo canale*

1. Seleziona **ID programmatore** dal menu a discesa nella **Nuovo canale** .

1. Digita un identificatore univoco in **ID canale**.
1. Digita il nome del marchio del canale utilizzato per scopi commerciali nel **Nome visualizzato**.
1. Seleziona **Aggiungi canale**.

È stata creata una nuova modifica alla configurazione ed è pronto per l’aggiornamento del server. Per utilizzare il nuovo canale elencato nella **Canali** , procedere con [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md) flusso.

