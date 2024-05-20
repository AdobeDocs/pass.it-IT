---
title: Integrazioni dashboard TVE
description: Scopri le integrazioni tra i canali e gli MVPD e come gestire le integrazioni.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '2093'
ht-degree: 0%

---

# Integrazioni

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Il **Integrazioni** della dashboard di TVE consente di visualizzare e gestire le impostazioni per le integrazioni tra i canali e MVPD. È inoltre possibile [creare una nuova integrazione](#create-new-integration) in base alle tue esigenze.

Il **Integrazioni** Nel pannello a sinistra, la scheda mostra un elenco delle integrazioni esistenti con i seguenti dettagli:

* Stato che indica se l’integrazione è attualmente attiva o inattiva
* Integrazione che collega canali specifici con i rispettivi MVPD
* Nome del canale con ID canale
* Nome visualizzato MVPD e ID MVPD

![Elenco delle integrazioni esistenti](assets/integrations-list.png)

*Elenco delle integrazioni esistenti*

Digita il nome del canale o MVPD in **Ricerca** sopra l’elenco per ulteriori informazioni sull’integrazione.

## Gestire le configurazioni di integrazione {#manage-integration-conf}

Per gestire un’integrazione specifica, segui la procedura riportata di seguito.

1. Seleziona la **Integrazioni** nel pannello sinistro.
1. Seleziona un’integrazione dall’elenco fornito per visualizzare e modificare le varie impostazioni nelle sezioni seguenti:

   * [Selezione endpoint](#endpoint-selection)
   * [Impostazioni piattaforma](#platform-settings)
   * [Metadati utente](#user-metadata)

>[!IMPORTANT]
>
> Visualizza [Revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md) per ulteriori informazioni sull&#39;attivazione delle modifiche alla configurazione.

### Selezione endpoint {#endpoint-selection}

Questa sezione consente di scegliere gli endpoint del MVPD utilizzati per i flussi di autenticazione, autorizzazione e disconnessione dai rispettivi menu a discesa.

![Endpoint per flussi di autenticazione, autorizzazione e disconnessione](assets/endpoint-selection.png)

*Endpoint per flussi di autenticazione, autorizzazione e disconnessione*

>[!NOTE]
>
>Gli MVPD possono fornire uno o più endpoint per ciascun flusso. Quando si integra un nuovo canale, l’MVPD deve specificare il proprio endpoint preferito per ogni flusso.

>[!IMPORTANT]
>
>Qualsiasi modifica agli endpoint influisce sul comportamento complessivo di un’integrazione. Queste modifiche devono essere implementate solo dopo aver ricevuto conferma dall’MVPD.

### Impostazioni piattaforma {#platform-settings}

Questa sezione ti consente di visualizzare e modificare le impostazioni di integrazione in tutti [piattaforme](/help/authentication/tve-dashboard-reports.md#platforms). Puoi modificare queste impostazioni in base alle singole piattaforme. Ad esempio, puoi regolare la durata TTL dell’autorizzazione su Android mantenendo un valore predefinito per un’altra piattaforma.

Ogni proprietà nelle impostazioni della piattaforma eredita un valore predefinito impostato da MVPD ma può essere regolato se necessario.

>[!IMPORTANT]
>
>È necessario un accordo con MVPD per determinare i valori impostati per ogni proprietà nelle impostazioni della piattaforma.

>[!IMPORTANT]
>
> L’ereditarietà delle impostazioni segue una catena che inizia dalle impostazioni MVPD (che sono le più generali), quindi dall’endpoint MVPD, dall’integrazione, dalla categoria di piattaforma e dalla piattaforma (che contiene il valore più specifico).

**Impostazioni piattaforma** viene utilizzato per sostituire le impostazioni per ogni livello della catena di ereditarietà. I livelli disponibili nella catena sono raggruppati come segue:

* **Predefinito per tutti**: imposta i valori per le proprietà applicabili universalmente a tutte le piattaforme se non sono definiti valori di piattaforma specifici, indipendentemente dalle implementazioni del programmatore.

* **Dispositivi desktop**: imposta i valori per le proprietà applicabili a tutti i computer desktop e laptop, indipendentemente dal metodo di programmazione (SDK JS o API REST).

* **Dispositivi mobili**: imposta i valori per le proprietà applicabili a tutti i dispositivi mobili, inclusi **iOS**, **Android** e altri, indipendentemente dall’approccio di programmazione (SDK o API REST).

* **Dispositivi collegati al televisore**: imposta i valori per le proprietà applicabili a tutti i dispositivi collegati al televisore, inclusi **tvOS**, **Roku**, **FireTV** e altri, indipendentemente dal metodo di programmazione (SDK o API REST).

* **Dispositivi non identificati**: imposta i valori delle proprietà applicabili a tutti i dispositivi in cui il meccanismo corrente non è in grado di identificare con precisione la piattaforma. In tali casi, applicare le norme più restrittive definite dalla MVPD.

  ![Categoria di piattaforme e relativi dispositivi](assets/platform-settings.png)

  *Categoria di piattaforme e relativi dispositivi*

Seleziona <img alt= "icona catena di ereditarietà" src="./assets/multiple-icon.svg" width="25"> si trova a destra di ogni proprietà per esplorare le proprietà utilizzate per ogni livello di ereditarietà descritto in precedenza.

#### Flussi aziendali più utilizzati {#most-used-flows}

Il **Impostazioni piattaforma** Questa sezione presenta una serie di proprietà utilizzate nei diversi flussi di business. Le proprietà effettive possono variare a seconda degli MVPD selezionati nell’integrazione specifica. Di seguito sono riportati i flussi più utilizzati:

**AuthN TTL e AuthZ TTL su tutte le piattaforme**

>[!IMPORTANT]
>
>I valori TTL di autenticazione (AuthN) e TTL di autorizzazione (AuthZ) devono essere allineati in modo coerente con le impostazioni MVPD.

Per modificare il TTL di autenticazione e autorizzazione in tutte le piattaforme per un’integrazione specifica, segui la procedura riportata di seguito.

1. Seleziona la **Integrazioni** nel pannello sinistro.
1. Seleziona l’integrazione per la quale desideri modificare i valori AuthN TTL e AuthZ TTL.
1. Accedi a **Impostazioni piattaforma** sezione.

1. Seleziona **Predefinito per tutti** scheda in **Impostazioni piattaforma**.

   >[!NOTE]
   >
   >Se desideri modificare la durata di **TTL AuthN** e **TTL AuthZ** per una categoria di piattaforma o una piattaforma specifica, seleziona la piattaforma di conseguenza.

   ![Modifica la durata TTL AuthN TTL AuthZ per tutte le piattaforme](assets/authn-ttl-authz-ttl-for-all-platform.png)

   *Modifica la durata TTL AuthN TTL AuthZ per tutte le piattaforme*

   **R.** AuthN, proprietà TTL **B.** AuthZ, proprietà TTL

1. Selezionare le frecce verso l&#39;alto e verso il basso per regolare la durata per il numero di giorni, ore, minuti e secondi in **TTL AuthN** e **TTL AuthZ** proprietà.

La durata per **TTL AuthN** e **TTL AuthZ** su tutte le piattaforme verrà aggiornato solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).

**Abilita SSO piattaforma**

>[!IMPORTANT]
>
>**Abilita Single Sign-On** la proprietà è supportata esclusivamente su *iOS, tvOS, Roku e FireTV* piattaforme. Si applica solo alle integrazioni con MVPD che supportano il single sign-on per queste piattaforme.

Segui questi passaggi per abilitare o disabilitare l’SSO per un’integrazione e una piattaforma specifiche.

1. Seleziona la **Integrazioni** nel pannello sinistro.
1. Selezionare l&#39;integrazione per la quale si desidera attivare o disattivare l&#39;accesso Single Sign-On.

1. Accedi a **Impostazioni piattaforma** sezione.

1. Selezionare una piattaforma o una categoria specifica di piattaforme per cui si desidera attivare l&#39;accesso single sign-on in **Impostazioni piattaforma**.

   ![Abilita Single Sign-On per una piattaforma specifica](assets/single-sign-on.png)

   *Abilita Single Sign-On per una piattaforma specifica*

   **R.** Single Sign-On, proprietà **B.** Proprietà Enforce Platform Permissions (Applica autorizzazioni piattaforma)

1. Seleziona **Sì** per abilitare o **No** per disattivare da **Abilita Single Sign-On** menu a discesa.

1. Seleziona **Sì** per abilitare o **No** per disattivare da **Applica autorizzazione piattaforma** menu a discesa.

   **Applica autorizzazione piattaforma** controlli di proprietà se la decisione dell&#39;utente di **Consenti** o **Rifiuta** l&#39;accesso alla piattaforma al proprio abbonamento al provider TV è rispettato.

   Ad esempio, se entrambi **Abilita Single Sign-On** e **Applica autorizzazione piattaforma** sono attivate e l’utente nega l’accesso alla piattaforma al proprio abbonamento al provider TV, quindi la rispettiva applicazione (canale) non sarà in grado di utilizzare il token di autenticazione Adobe Pass ottenuto da un’altra applicazione (canale).

Il **Single Sign-On** per una piattaforma selezionata verrà attivata o disattivata solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).

**Abilita autenticazione basata su home**

Segui questi passaggi per abilitare o disabilitare l’autenticazione basata su home per gli MVPD basati su OAuth2.

1. Seleziona la **Integrazioni** nel pannello sinistro.
1. Seleziona l’integrazione per la quale desideri abilitare o disabilitare l’autenticazione basata sulla home.
1. Accedi a **Impostazioni piattaforma** sezione.
1. Seleziona una piattaforma o una categoria di piattaforme specifica per la quale desideri abilitare l’autenticazione basata sulla home in **Impostazioni piattaforma**.

   ![Abilitare l’autenticazione basata su home per una piattaforma specifica](assets/attempt-hba.png)

   *Abilitare l’autenticazione basata su home per una piattaforma specifica*

   **R.** Tentativo proprietà HBA **B.** Proprietà TTL AuthN HBA

1. Seleziona **Sì** per abilitare e **No** per disattivare da **Tentativo HBA** menu a discesa.

>[!IMPORTANT]
>
>Modifica della durata di **TTL AuthN HBA** deve essere evitata. Potrebbero verificarsi errori imprevisti nel processo di autorizzazione.

Il **Tentativo HBA** per un MVPD specifico verrà abilitata o disabilitata solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).

#### Aggiungi altre proprietà {#add-more-properties}

Il **Aggiungi altre proprietà** consente la flessibilità di includere proprietà specifiche aggiuntive per le integrazioni, in particolare per i flussi meno comuni.

Puoi aggiungere le seguenti proprietà:

* Per tutte le piattaforme, seleziona **Predefinito per tutti** a sinistra.
* Per una categoria di piattaforma, seleziona **Dispositivi desktop**, **Dispositivi mobili**, o **Dispositivi collegati al televisore** a sinistra.
* Per un dispositivo specifico, seleziona **iOS**, **Android**, **tvOS**, **Roku**, o **FireTV** a sinistra.

Di seguito sono riportati alcuni esempi di flussi diversi che possono essere abilitati aggiungendo queste proprietà:

**Modifica il numero di risorse pre-autorizzate**

Per impostazione predefinita, la maggior parte degli MVPD supporta una chiamata authZ di verifica preliminare utilizzando fino a 5 ID di risorsa.
Tuttavia, nei casi in cui i MVPD accettano di aumentare questo limite, puoi passare alla **Aggiungi altre proprietà** e seleziona **Verifica preliminare risorse massime** dal menu delle opzioni.

**Verifica preliminare risorse massime** aggiungerà un nuovo attributo per il quale è possibile specificare il limite concordato con l&#39;MVPD.

![Aggiungi proprietà Max Resources verifica preliminare](assets/preflight-max-resources.png)

*Aggiungi proprietà Max Resources verifica preliminare*

Il **Verifica preliminare risorse massime** la proprietà verrà aggiunta solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).

**Modifica nome visualizzato MVPD o URL logo**

Per le applicazioni di programmazione che non desiderano creare il selettore MVPD e che si basano invece sulle configurazioni fornite, puoi passare alla **Aggiungi altre proprietà** e seleziona **Nome visualizzato** o **URL logo** per aggiungere il nome visualizzato o gli URL del logo richiesti per ogni MVPD dal menu delle opzioni.

È possibile utilizzare valori diversi per queste proprietà per lo stesso MVPD a seconda della piattaforma del dispositivo e dell’esperienza utente desiderata.

![Aggiungi nome visualizzato o proprietà URL logo](assets/displayname-logourl.png)

*Aggiungi nome visualizzato o proprietà URL logo*

Il **Nome visualizzato** o **URL logo** la proprietà verrà aggiunta solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).

**Richiedi un nuovo flusso di autenticazione al passaggio dell’app (canale)**

Se desideri forzare una nuova autenticazione quando gli utenti passano da un’app all’altra. In tal caso, è possibile passare alla **Aggiungi altre proprietà**, seleziona la **Autenticazione per aggregatore** proprietà.

Aggiunta **Autenticazione per aggregatore** interrompe effettivamente l&#39;accesso single sign-on per il rispettivo canale.

![Aggiungi proprietà Auth Per Aggregator](assets/auth-per-aggregator.png)

*Aggiungi proprietà Auth Per Aggregator*

Il **Autenticazione per aggregatore** la proprietà verrà aggiunta solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).

Una volta aggiunto, seleziona **Sì** per abilitare **Autenticazione per aggregatore** per un’integrazione selezionata.

#### Elimina proprietà {#delete-properties}

Seleziona <img alt= "pulsante elimina proprietà" src="./assets/delete-icon.svg" width="25"> si trova a destra di ogni proprietà per eliminare le proprietà che non sono più necessarie.

>[!NOTE]
>
>Alcune proprietà non possono essere rimosse in quanto sono requisiti obbligatori per l&#39;MVPD selezionato.

La proprietà verrà eliminata da **Impostazioni piattaforma** sezione solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).

### Metadati utente {#user-metadata}

Questa sezione ti consente di aggiornare le impostazioni per ogni parametro di metadati utente condiviso da MVPD.

>[!NOTE]
>
>Ogni MVPD può condividere parametri diversi. Per ulteriori informazioni sui parametri che uno specifico MVPD può condividere, contatta il tuo rappresentante di Adobe.

La sezione metadati utente visualizza le colonne seguenti:

**Chiave**: rappresenta i parametri effettivi dei metadati dell’utente da utilizzare nell’API per estrarre i valori.

**Descrizione**: fornisce una breve descrizione di ciascun parametro di metadati utente.

**Crittografato**: questa colonna ti consente di abilitare o disabilitare i parametri nell’API selezionando **Sì** o **No** rispettivamente dal menu a discesa. Scelta **Sì** indica che il valore del parametro verrà crittografato nell’API. La crittografia viene eseguita utilizzando un certificato definito da un **Metadati utente** ambito.

>[!TIP]
>
>
> Assicurati sempre che **ZIP** il parametro è crittografato.

Ulteriori informazioni sui certificati disponibili in [Programmatori](/help/authentication/tve-dashboard-programmers.md#available-certificates) e [Canali](/help/authentication/tve-dashboard-channels.md#available-certificates) sezioni.

**Abilitato**: questa colonna ti consente di abilitare o disabilitare i parametri nell’API selezionando **Sì** o **No** rispettivamente dal menu a discesa.

![Parametri disponibili per i metadati utente](assets/user-metadata.png)

*Parametri disponibili per i metadati utente*

## Creare una nuova integrazione {#create-new-integration}

Per creare una nuova integrazione con un nuovo MVPD nella configurazione corrente, effettua le seguenti operazioni:

1. Seleziona la **Integrazioni** nel pannello sinistro.
1. Seleziona **Creare una nuova integrazione** in alto a destra del **Integrazioni** sezione.

   ![Creare una nuova integrazione](assets/create-new-integration.png)

   *Creare una nuova integrazione*

   Vengono visualizzate le seguenti sezioni:

   **Seleziona canale e MVPD**

   Seleziona un **Canale** dal **Seleziona canale** per aggiungere una nuova integrazione. Dopo aver selezionato il canale, seleziona il **MVPD** dal **Seleziona MVPD** da integrare con il canale selezionato.

   ![Seleziona canale e MVPD](assets/select-channel-mvpd.png)

   *Seleziona canale e MVPD*

   **Seleziona endpoint**

   Dopo aver selezionato il MVPD richiesto, **Seleziona endpoint**  La sezione verrà precompilata con gli endpoint predefiniti configurati per quel particolare MVPD.

   >[!IMPORTANT]
   >
   >Non modificare gli endpoint predefiniti in alcun flusso a meno che non sia espressamente indicato dal MVPD.

   ![Seleziona endpoint ](assets/select-endpoints.png)

   *Seleziona endpoint*

   **Informazioni aggiuntive**

   Questa sezione include varie proprietà che devono essere configurate per l&#39;MVPD selezionato nel **Seleziona canale e MVPD** sezione.

   >[!NOTE]
   >
   > Le proprietà effettive possono variare a seconda degli MVPD selezionati nella **Seleziona canale e MVPD** sezione.

   Ad esempio, puoi modificare i **TTL AuthN** o **ID partner** (ID canale) a scopo di co-branding, nella pagina di accesso di MVPD nell’immagine seguente.

   ![Modifica informazioni aggiuntive](assets/additional-information.png)

   *Modifica informazioni aggiuntive*

   Seleziona **Salva integrazione** in alto a destra del **Creare una nuova integrazione** sezione.

Una nuova integrazione verrà creata solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).


## Disabilita integrazione {#disable-integratgion}

Per disabilitare un’integrazione, effettua le seguenti operazioni:

1. Seleziona la **Integrazioni** nel pannello sinistro.
1. Seleziona l’integrazione da disabilitare.
1. Disattiva l’opzione disponibile in alto a destra nell’integrazione selezionata.

   ![Disabilita integrazione](assets/disable-integration.png)

   *Disabilita integrazione*

L’integrazione verrà disabilitata solo dopo [revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md).

Una volta disattivata l&#39;integrazione, gli utenti finali perderanno la possibilità di eseguire l&#39;autenticazione o di autorizzare l&#39;utilizzo di MVPD specifico.


