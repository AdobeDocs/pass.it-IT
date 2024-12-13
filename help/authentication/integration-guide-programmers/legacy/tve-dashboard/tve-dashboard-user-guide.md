---
title: Guida utente di Primetime TVE Dashboard
description: Guida utente di Primetime TVE Dashboard
exl-id: 6f7f7901-db3a-4c68-ac6a-27082db9240a
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '5505'
ht-degree: 0%

---

# (Legacy) Guida utente di Primetime TVE Dashboard {#tve-db-user-guide}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#tve-db-intro}

[[!DNL Adobe] TVE Dashboard (TVE Dashboard)](https://console.auth.adobe.com/) è un dashboard self-service destinato agli utenti che lavorano per aziende di media (Programmatori) che hanno una relazione commerciale con il team di prodotto per l&#39;autenticazione di Adobe Pass.

Contatta il tuo Technical Account Manager (TAM) per ottenere l’accesso. Per ottenere l’accesso, dovrai configurare due nuovi gruppi di utenti nell’organizzazione Adobe Marketing Cloud:

* Lettura/scrittura dashboard TVE: i membri di questo gruppo dispongono dei diritti completi su tutte le sezioni modificabili del dashboard
* TVE Dashboard di sola lettura: i membri di questo gruppo dispongono solo dei diritti di visualizzazione per l&#39;intero dashboard


Prima di approfondire questa guida utente, ti consigliamo di esaminare le seguenti risorse per comprendere i flussi e le funzioni forniti dal team di prodotto per l’autenticazione di Adobe Pass e acquisire familiarità con i termini utilizzati nel presente documento:

* [Documento tecnico TVE](/help/authentication/kickstart/technical-paper.md)
* [Guida di Kick-Start per programmatori](/help/authentication/kickstart/programmer-kickstart-guide.md)
* [Flusso diritto](/help/authentication/integration-guide-programmers/entitlement-flow.md)
* [Glossario](/help/authentication/kickstart/glossary.md)


Passando alle sezioni successive di questa guida utente, scoprirai come amministrare diverse impostazioni per i canali, i programmatori o le integrazioni tra canali e MVPD (Multichannel Video Program Distributors) della tua azienda.

>[!IMPORTANT]
>TVE Dashboard offre la possibilità di passare da un Workspace di base a uno avanzato. Per farlo, attiva l’icona in alto a destra. Advanced Workspace è destinato agli utenti con conoscenze tecniche sostanziali e conoscenze avanzate delle funzioni offerte dal team di prodotto Adobe Pass Authentication.

![Aree di lavoro del dashboard TVE](../../../assets/tve-dashboard/old-tve-dashboard/tve-basic-advanced-workspace.png)

*Figura 1: elenco a discesa &quot;Basic / Advanced Workspace&quot; del dashboard di Adobe Primetime TVE*

## Ambienti {#authn-environments}

A seconda delle attività che un utente potrebbe dover svolgere, potrebbe dover passare da un ambiente di autenticazione Adobe Pass all’altro. Per informazioni dettagliate sugli ambienti di autenticazione Adobe Pass, consulta il seguente documento: [Informazioni sugli ambienti di autenticazione Adobe Pass](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md).

TVE Dashboard fornisce due ambienti denominati Prequal (Prequalifica) e Release, ciascuno con due profili denominati Staging e Produzione, come illustrato di seguito:

* [Gestione temporanea Prequal](https://console-prequal.auth-staging.adobe.com/)
* [Produzione Prequal](https://console-prequal.auth.adobe.com/)
* [Staging versione](https://console.auth-staging.adobe.com/)
* [Versione produzione](https://console.auth.adobe.com/)

Per passare da un ambiente all’altro, l’utente può fare clic sull’ambiente desiderato rappresentato dalla voce presente nell’elemento a discesa illustrato di seguito:

![Menu a discesa degli ambienti del dashboard TVE](../../../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

*Figura 2: elenco a discesa degli ambienti del dashboard di Adobe Pass TVE*

>[!IMPORTANT]
>
>È molto importante notare che quando si apportano modifiche amministrative alla configurazione di autenticazione di Adobe Pass tramite la dashboard TVE, si consiglia vivamente di seguire la sequenza seguente per garantire la funzionalità corretta.

Per apportare modifiche amministrative alla configurazione dell’autenticazione Adobe Pass tramite il dashboard TVE:

* Eseguire le modifiche in [Gestione temporanea delle versioni e convalidarle](http://sp.auth-staging.adobe.com/apitest/api.html).
* Apportare le modifiche in [Prequal Production e convalidarle](http://sp.auth-staging.adobe.com/apitest/api.html).
* Eseguire le modifiche in [Release Production e convalidarle](http://sp.auth-staging.adobe.com/apitest/api.html).

>[!IMPORTANT]
>
>Affinché le modifiche amministrative diventino effettive, gli utenti devono passare alla sezione &quot;Revisione e push delle modifiche&quot; selezionando il pulsante, che verrà visualizzato nella parte inferiore sinistra della barra laterale, per rivedere le modifiche, aggiungere una descrizione per le modifiche appena create e confermare l’aggiornamento della configurazione selezionando &quot;Configurazione push&quot;.

![Tve Dashboard esamina una notifica push](../../../assets/tve-dashboard/old-tve-dashboard/tve-review-push-notifications.png)

*Figura 3: revisione del dashboard di Adobe Primetime TVE e notifica delle modifiche push*

## Sezioni {#sections}

Gli utenti che lavorano per le società di media (programmatori) possono accedere alle seguenti sezioni della dashboard di TVE dalla barra laterale:

* **Canali** - Contiene le impostazioni relative ai provider di contenuti
* **Programmatori** - Contiene le impostazioni relative all&#39;organizzazione padre che aggregano uno o più **Canali**
* **Integrazioni** - Contiene le impostazioni relative all&#39;integrazione tra **Canali** e **MVPDs**
* **MVPDs** - Contiene le impostazioni relative ai **MVPDs** disponibili
* **Rapporti** - Contiene dati aggregati per tre tipi di rapporti: TTL AuthN, TTL AuthZ, SSO
* **Registro modifiche** - Contiene le modifiche più recenti applicate alla configurazione del dashboard TVE

![Sezioni dashboard TVE](../../../assets/tve-dashboard/old-tve-dashboard/tve-dashboard-sections.png)

*Figura 4: sezioni del dashboard TVE di Adobe Primetime*

### Canali {#tve-db-channels-section}

Questa sezione consente di visualizzare e modificare le impostazioni per i canali disponibili o di crearne uno nuovo. Facendo clic su uno dei canali disponibili viene visualizzata una schermata con le seguenti schede:

* **Dati canale**
   * **ID canale**: l&#39;ID univoco del canale utilizzato nel nostro sistema, detto anche &quot;ID richiedente&quot;.
   * **Nome visualizzato**: il nome commerciale del canale.
* **Impostazioni generali**
   * **Configurazione di Analytics** - Configura eventi di autenticazione Adobe Pass da inoltrare ad Adobe Analytics. Contatta Adobe per ulteriori dettagli sulla configurazione dell’ID suite di rapporti (RSID) prima di abilitare questa funzione.
* **Certificati**

  Contiene l’elenco dei certificati utilizzati nel flusso di autenticazione insieme all’organizzazione di emissione, alla data di rilascio e alla data di scadenza. Questi certificati fungono da chiavi pubbliche/private e vengono utilizzati a scopo di convalida.
* **Domini**

  Contiene l’elenco dei domini da cui il rispettivo Canale comunicherà con l’autenticazione di Adobe Pass.
* **Integrazioni**

  Contiene l’elenco delle integrazioni con gli MVPD disponibili, insieme allo stato di ogni integrazione che potrebbe essere abilitata o meno. Per accedere alla pagina Integrazione, fai clic su una voce specifica.
* **Applicazioni registrate**

  Contiene l’elenco delle registrazioni delle applicazioni. Per ulteriori dettagli, vedere il documento [Dynamic Client Registration Management](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

* **Schemi personalizzati**

  Contiene l’elenco degli schemi personalizzati. Per ulteriori dettagli, vedere [Registrazione dell&#39;applicazione iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md) e [Dynamic Client Registration Management](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management)

#### Aggiungi/Elimina domini {#add-delete-domains}

Per avviare il processo di aggiunta di un nuovo dominio per il canale selezionato, fai clic sul pulsante &quot;Aggiungi nuovo dominio&quot; sotto l’elenco Domini. Verrà creata una nuova voce di dominio in cui è possibile specificare il nome di dominio. Se nell’elenco dei domini è già presente un dominio più generico, non è consigliabile aggiungere un nuovo sottodominio.

![Aggiungi un nuovo dominio a una sezione canale selezionata](../../../assets/tve-dashboard/old-tve-dashboard/add-domain-to-channel-sec.png)

*Figura: scheda Domini nei canali*

#### Creare un&#39;applicazione registrata a livello di canale {#create-registered-application-channel-level}

Per creare un&#39;applicazione registrata a livello di canale, passare al menu &quot;Canali&quot; e scegliere quello per il quale si desidera creare un&#39;applicazione. Quindi, dopo aver selezionato la scheda &quot;Applicazioni registrate&quot;, fare clic sul pulsante &quot;Aggiungi nuova applicazione&quot;.

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-new-app-channel-level.png)

Come mostrato nell’immagine seguente, i campi da compilare sono:

* **Nome applicazione**: il nome dell&#39;applicazione

* **Assegnato al canale** - Come mostrato di seguito, ciò che qui è leggermente diverso, rispetto alla stessa azione eseguita a livello di Programmatore, è il menu a discesa &quot;Canali assegnati&quot; che non è abilitato, quindi non c&#39;è alcuna opzione per associare l&#39;applicazione registrata al di fuori del canale corrente.

* **Versione applicazione** - per impostazione predefinita, è impostato su &quot;1.0.0&quot;, ma si consiglia di modificarlo con la propria versione dell&#39;applicazione. Come best practice, se decidi di modificare la versione dell’applicazione, rifletterla creando una nuova applicazione registrata.

* **Piattaforme applicazione**: le piattaforme con cui collegare l&#39;applicazione. È possibile selezionarli tutti o più valori.

* **Nomi dominio**: i domini con cui collegare l&#39;applicazione. I domini nell’elenco a discesa sono una selezione unificata di tutti i domini da tutti i canali. È possibile selezionare più domini dall&#39;elenco. Il significato dei domini è URL di reindirizzamento [RFC6749](https://tools.ietf.org/html/rfc6749). Nel processo di registrazione del client, l’applicazione client può richiedere di essere autorizzata a utilizzare un URL di reindirizzamento per la finalizzazione del flusso di autenticazione. Quando un’applicazione client richiede un URL di reindirizzamento specifico, questo viene convalidato in base ai domini inseriti nella whitelist di questa applicazione registrata associata all’istruzione software.

![](../../../assets/tve-dashboard/old-tve-dashboard/new-reg-app-channel.png)

Dopo aver riempito i campi con i valori appropriati, fai clic su &quot;Fine&quot; per salvare l’applicazione nella configurazione.

Tieni presente che **non è disponibile alcuna opzione per modificare un&#39;applicazione già creata**. Se si scopre che un elemento creato non soddisfa più i requisiti, sarà necessario creare e utilizzare una nuova applicazione registrata con l&#39;applicazione client di cui soddisfa i requisiti.

##### Scaricare un rendiconto software {#download-software-statement-channel-level}

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-list.png)

Facendo clic sul pulsante &quot;Download&quot; nella voce dell’elenco per la quale è necessaria un’istruzione software, verrà generato un file di testo. Questo file contiene un elemento simile all’output di esempio seguente.

![](../../../assets/download-software-statement.png)

Il nome del file viene identificato in modo univoco tramite il prefisso &quot;software_statement&quot; e l’aggiunta della marca temporale corrente.

Si noti che, per la stessa applicazione registrata, ogni volta che si fa clic sul pulsante di download verranno ricevute istruzioni software diverse, ma ciò non invalida le istruzioni software ottenute in precedenza per l&#39;applicazione. Questo accade perché vengono generate sul posto, per richiesta di azione.

Esiste una **limitazione** relativa all&#39;azione di download. Se un’istruzione software viene richiesta facendo clic sul pulsante &quot;Scarica&quot; poco dopo la creazione dell’applicazione registrata e tale istruzione non è stata ancora salvata e il json di configurazione non è stato sincronizzato, nella parte inferiore della pagina viene visualizzato il seguente messaggio di errore.

![](../../../assets/tve-dashboard/old-tve-dashboard/error-sw-statement-notready.png)

Viene eseguito il wrapping di un codice di errore HTTP 404 Non trovato ricevuto dal core poiché l&#39;ID dell&#39;applicazione registrata non è stato ancora propagato e il core non ne è a conoscenza.

Dopo aver creato l&#39;applicazione registrata, la soluzione consiste nell&#39;attendere al massimo 2 minuti per la sincronizzazione della configurazione. In questo caso, il messaggio di errore non verrà più ricevuto e il file di testo con l&#39;istruzione software sarà disponibile per il download.

### Programmatori {#tve-db-programmers-section}

Questa sezione consente di visualizzare e modificare le impostazioni per i programmatori disponibili o di crearne una nuova. Facendo clic su uno dei programmatori disponibili viene visualizzata una schermata con le seguenti schede:

* **Dati programmatore**
   * **ID programmatore** - ID univoco del programmatore utilizzato nel sistema.
   * **Nome visualizzato**: il nome commerciale del programmatore.
   * **URL logo** - URL (Uniform Resource Locator) del logo commerciale del programmatore.
   * **Anteprima logo** - Anteprima logo commerciale del programmatore scaricata dall&#39;URL (Uniform Resource Locator) indicato sopra.

* **Certificati**

  Contiene l’elenco dei certificati utilizzati nel flusso di autenticazione insieme all’organizzazione di emissione, alla data di rilascio e alla data di scadenza. Questi certificati fungono da chiavi pubbliche/private e vengono utilizzati a scopo di convalida.

* **Canali**

  Contiene l’elenco dei canali appartenenti a questo programmatore specifico. Per passare alla sezione Canali, fai clic su una voce specifica.

* **Applicazioni registrate**

  Contiene l’elenco delle registrazioni delle applicazioni. Per ulteriori dettagli, vedere [Dynamic Client Registration Management](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

* **Schemi personalizzati**

  Contiene l’elenco degli schemi personalizzati. Per ulteriori dettagli, consulta [Registrazione dell&#39;applicazione iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md).

#### Creare un&#39;applicazione registrata a livello di programmatore {#create-registered-application-programmer-level}

Vai alla scheda **Programmatori** > **Applicazioni registrate**.

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-progr-level.png)

Nella scheda Applicazioni registrate fare clic su **Aggiungi nuova applicazione**. Compila i campi obbligatori nella nuova finestra.

Come mostrato nell’immagine seguente, i campi da compilare sono:

* **Nome applicazione**: il nome dell&#39;applicazione

* **Assegnato al canale**: il nome del canale, t</span>o al quale è collegata l&#39;applicazione. L&#39;impostazione predefinita nella maschera a discesa è **Tutti i canali.** L&#39;interfaccia consente di selezionare un canale o tutti i canali.

* **Versione applicazione** - per impostazione predefinita, è impostato su &quot;1.0.0&quot;, ma si consiglia di modificarlo con la propria versione dell&#39;applicazione. Come best practice, se decidi di modificare la versione dell’applicazione, rifletterla creando una nuova applicazione registrata.

* **Piattaforme applicazione**: le piattaforme con cui collegare l&#39;applicazione. È possibile selezionarli tutti o più valori.

* **Nomi dominio**: i domini con cui collegare l&#39;applicazione. I domini nell’elenco a discesa sono una selezione unificata di tutti i domini da tutti i canali. È possibile selezionare più domini dall&#39;elenco. Il significato dei domini è URL di reindirizzamento [RFC6749](https://tools.ietf.org/html/rfc6749). Nel processo di registrazione del client, l’applicazione client può richiedere di essere autorizzata a utilizzare un URL di reindirizzamento per la finalizzazione del flusso di autenticazione. Quando un’applicazione client richiede un URL di reindirizzamento specifico, questo viene convalidato in base ai domini inseriti nella whitelist di questa applicazione registrata associata all’istruzione software.

![](../../../assets/tve-dashboard/old-tve-dashboard/new-reg-app.png)

Dopo aver riempito i campi con i valori appropriati, fai clic su &quot;Fine&quot; per salvare l’applicazione nella configurazione.

Tieni presente che **non è disponibile alcuna opzione per modificare un&#39;applicazione già creata**. Se si scopre che un elemento creato non soddisfa più i requisiti, sarà necessario creare e utilizzare una nuova applicazione registrata con l&#39;applicazione client di cui soddisfa i requisiti.

##### Scaricare un rendiconto software {#download-software-statement-programmer-level}

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-list.png)

Facendo clic sul pulsante &quot;Download&quot; nella voce dell’elenco per la quale è necessaria un’istruzione software, verrà generato un file di testo. Questo file contiene un elemento simile all’output di esempio seguente.

![](../../../assets/download-software-statement.png)

Il nome del file viene identificato in modo univoco tramite il prefisso &quot;software_statement&quot; e l’aggiunta della marca temporale corrente.

Si noti che, per la stessa applicazione registrata, ogni volta che si fa clic sul pulsante di download verranno ricevute istruzioni software diverse, ma ciò non invalida le istruzioni software ottenute in precedenza per l&#39;applicazione. Questo accade perché vengono generate sul posto, per richiesta di azione.

Esiste una **limitazione** relativa all&#39;azione di download. Se un’istruzione software viene richiesta facendo clic sul pulsante &quot;Scarica&quot; poco dopo la creazione dell’applicazione registrata e tale istruzione non è stata ancora salvata e il json di configurazione non è stato sincronizzato, nella parte inferiore della pagina viene visualizzato il seguente messaggio di errore.

![](../../../assets/tve-dashboard/old-tve-dashboard/error-sw-statement-notready.png)

Viene eseguito il wrapping di un codice di errore HTTP 404 Non trovato ricevuto dal core poiché l&#39;ID dell&#39;applicazione registrata non è stato ancora propagato e il core non ne è a conoscenza.

Dopo aver creato l&#39;applicazione registrata, la soluzione consiste nell&#39;attendere al massimo 2 minuti per la sincronizzazione della configurazione. In questo caso, il messaggio di errore non verrà più ricevuto e il file di testo con l&#39;istruzione software sarà disponibile per il download.

### Integrazioni {#tve-db-integrations-sec}

Questa sezione consente di visualizzare e modificare le impostazioni per le integrazioni tra canali e MVPD disponibili o di crearne una nuova. Facendo clic su una delle integrazioni disponibili verrà restituita una singola pagina quando si utilizza il Workspace di base o una schermata con le seguenti schede quando si utilizza il Workspace avanzato:

* **Dati di integrazione**
   * **ID integrazione**- Risultato dell&#39;aggiunta dell&#39;ID univoco MVPDs all&#39;ID univoco del canale separato dal carattere &quot;_&quot;.
   * **Nome visualizzato canale**: il nome commerciale del canale.
   * **ID canale**: l&#39;ID univoco del canale utilizzato nel nostro sistema, detto anche &quot;ID richiedente&quot;.
   * **Nome visualizzato MVPD**: il nome commerciale di MVPD.
   * **ID MVPD**: l&#39;ID univoco di MVPD utilizzato nel sistema.
* **Impostazioni generali**
   * **Chiavi metadati utente** - Configura le chiavi metadati disponibili per l&#39;integrazione specifica.
   * **Impostazioni specifiche della piattaforma** - Configurare impostazioni diverse per una piattaforma specifica (ad esempio, TTL, SSO e IFrames).

* **Impostazioni autenticazione**
   * Contiene le impostazioni relative alla funzione di autenticazione di Adobe Pass.
* **Impostazioni autorizzazione**
   * Contiene le impostazioni relative alla funzione Autenticazione di Adobe Pass.
* **Impostazioni disconnessione**
   * Contiene le impostazioni relative alla funzionalità di disconnessione dell&#39;autenticazione di Adobe Pass.

#### Creare l’integrazione {#create-integration}

Per creare una nuova integrazione, segui i passaggi seguenti:

* fai clic sul pulsante &quot;Aggiungi nuova integrazione&quot;
* cerca e seleziona un canale
* cerca e seleziona un MVPD
* attendi che TVE Dashboard calcoli l’ID integrazione e visualizzi gli endpoint MVPD disponibili
* selezionare gli endpoint di autenticazione, autorizzazione e disconnessione oppure utilizzare i valori predefiniti
* fai clic sul pulsante &quot;Crea integrazione&quot;
* a seconda delle impostazioni di MVPD, è possibile visualizzare una finestra a comparsa e richiedere proprietà aggiuntive, che avrebbero dovuto essere fornite in precedenza da MVPD; in caso contrario verrà effettuato un reindirizzamento alla pagina di integrazione appena creata

![](../../../assets/tve-dashboard/old-tve-dashboard/new-integration-window.png)



*Figura 5. Finestra Nuova integrazione del dashboard di Adobe Primetime TVE*


#### Integrazione di aggiornamento {#update-integration}

Per aggiornare un’integrazione esistente, fai clic sulla voce della tabella per tale integrazione specifica dalla sezione Integrazioni o dalla sezione Canali, che contiene una scheda Integrazioni.

Quando si utilizza la modalità Workspace di base, questa sezione consente di visualizzare e modificare le impostazioni più comunemente aggiornate, ad esempio i TTL (time-to-live) dei token di autenticazione e autorizzazione e le impostazioni iFrame. Tieni presente che le impostazioni TTL possono mancare per le integrazioni con MVPD che supportano il TTL di persistenza dei token definiti in modo dinamico (vedi la voce 1.19 di [Requisiti di integrazione di MVPD](/help/authentication/integration-guide-mvpds/mvpd-integr-features.md)).



Quando si utilizza la modalità Workspace avanzata, questa sezione consente di visualizzare e modificare le impostazioni meno comuni.



Nel caso delle modalità Workspace di base e avanzata, queste impostazioni possono essere modificate a livello di piattaforma (ad esempio, seleziona un valore personalizzato per il token TTL di autorizzazione su Android, impostazione predefinita su ogni altra piattaforma).



>[!IMPORTANT]
>È importante comprendere la catena di ereditarietà delle impostazioni: MVPD -> Endpoint MVPD -> Integrazione -> Piattaforma, dove Platform ha il valore più specifico e MVPD il valore predefinito più generico.

![](../../../assets/tve-dashboard/old-tve-dashboard/inheritance-chain-component.png)


*Figura 6. Componente catena ereditarietà proprietà dashboard TVE di Adobe Primetime*


#### Impostazioni specifiche della piattaforma {#platform-sp-settings}

Questa sottosezione può essere utilizzata per ignorare le impostazioni per piattaforme specifiche. Le piattaforme disponibili sono:

* **Tutte le piattaforme** - Imposta i valori che verranno applicati a tutte le piattaforme indipendentemente dalle implementazioni del programmatore nel caso in cui non siano impostati altri valori per una piattaforma specifica.
* **Android** - Imposta i valori che verranno applicati alle implementazioni del programmatore tramite Adobe Pass Authentication Android SDK.
* **API REST senza client** - Imposta i valori che verranno applicati alle implementazioni del programmatore tramite l&#39;API REST di autenticazione di Adobe Pass.
* **Fire TV** - Imposta i valori che verranno applicati alle implementazioni del programmatore tramite Adobe Pass Authentication FireTV SDK.
* **Flash SDK** - Piattaforma obsoleta. **obsoleto**
* **JavaScript SDK** - Imposta i valori che verranno applicati alle implementazioni del programmatore tramite Adobe Pass Authentication JavaScript SDK.
* **Roku** - Imposta i valori che verranno applicati alle implementazioni del programmatore tramite l&#39;API REST di autenticazione Adobe Pass e che inviano &quot;Roku&quot; come tipo di dispositivo. Nel caso di dispositivi Roku, questo ha la precedenza sui valori impostati per la piattaforma API REST senza client.
* **Xbox native SDK** - Questa piattaforma è obsoleta. **obsoleto**
* **API REST Xbox 360** - Imposta i valori che verranno applicati alle implementazioni del programmatore tramite l&#39;API REST di autenticazione Adobe Pass e che inviano &quot;xbox&quot; come tipo di dispositivo. Questo ha la precedenza sui valori impostati per la piattaforma API REST senza client nel caso di dispositivi Xbox 360.
* **API REST di Xbox One** - Imposta i valori che verranno applicati alle implementazioni del programmatore tramite l&#39;API REST di autenticazione di Adobe Pass e che inviano &quot;xboxOne&quot; come tipo di dispositivo. Questo ha la precedenza sui valori impostati per la piattaforma API REST senza client nel caso di dispositivi Xbox One.
* **iOS** - Imposta i valori che verranno applicati alle implementazioni del programmatore tramite Adobe Pass Authentication iOS SDK.
* **tvOS** - Imposta i valori che verranno applicati alle implementazioni del programmatore tramite Adobe Pass Authentication tvOS SDK.


![](../../../assets/tve-dashboard/old-tve-dashboard/platform-sp-settings.png)

*Figura 7. Impostazioni specifiche della piattaforma Adobe Primetime TVE Dashboard*


#### Abilita Single Sign-On di Platform {#enable-platform-sso}

Per abilitare/disabilitare l&#39;accesso Single Sign On per un&#39;integrazione e una piattaforma specifiche, eseguire la procedura seguente:

* assicurarsi di utilizzare la modalità Workspace avanzata
* passa all’integrazione desiderata
* passa alla scheda **Impostazioni generali**
* selezionare la piattaforma desiderata su cui si desidera attivare o disattivare Single Sign-On
* attiva il flag **Abilita Single Sign On** sul valore desiderato (Sì / No)

  >[!IMPORTANT]
  >È importante notare che il flag **Abilita Single Sign On** è disponibile solo per le piattaforme iOS, tvOS, Roku e FireTV e solo per le integrazioni con MVPD che supportano il Single Sign On per tali piattaforme.

* attiva il flag **Applica autorizzazione piattaforma** sul valore desiderato (Sì / No)

  >[!IMPORTANT]
  >È importante notare che il flag **Applica autorizzazione piattaforma** controlla se la decisione dell&#39;utente di consentire o negare l&#39;accesso alla piattaforma al proprio abbonamento al provider TV verrà applicata o meno. Considerando lo scenario in cui il flag **Abilita Single Sign-On** è impostato su &quot;Sì&quot;, anche il flag **Applica autorizzazione piattaforma** è impostato su &quot;Sì&quot; e l&#39;utente sceglie di negare l&#39;accesso alla piattaforma al proprio abbonamento al provider TV, la rispettiva applicazione (canale) non sarà in grado di utilizzare il token di autenticazione Adobe Pass ottenuto da un&#39;altra applicazione (canale).


#### Abilita autenticazione basata sulla home page {#enable-hba}

Segui i passaggi seguenti per abilitare/disabilitare l&#39;autenticazione di base per gli MVPD basati su **OAuth2**:

* assicurarsi di utilizzare la modalità Workspace avanzata
* passa all’integrazione desiderata
* passa alla scheda **Impostazioni autenticazione**
* passa alla scheda secondaria **Regole dinamiche AuthN**
* attiva il flag **Tentativo HBA** sul valore desiderato (Sì / No)


>[!IMPORTANT]
>Tenere presente che il valore &quot;HBA AuthN TTL&quot; non deve mai essere sostituito, altrimenti il flusso di autorizzazione potrebbe non riuscire in modo imprevisto.

Rivolgersi a **tve-support@adobe.com** per informazioni sull&#39;abilitazione dell&#39;autenticazione Home Base per MVPD basati su SAML.

### MVPD {#tve-db-mvpds-sec}

Questa sezione consente di visualizzare le impostazioni per gli MVPD disponibili. Facendo clic su uno degli MVPD disponibili viene visualizzata una schermata con le seguenti schede:

* **Dati MVPD**
   * **ID MVPD**: l&#39;ID univoco di MVPD utilizzato nel sistema.
   * **Nome visualizzato**: il nome commerciale del MVPD che potrebbe essere utilizzato nel selettore dell&#39;utente.
   * **URL logo** - URL (Uniform Resource Locator) del logo commerciale di MVPD.
   * **Anteprima logo** - Anteprima logo commerciale MVPD scaricata dall&#39;URL (Uniform Resource Locator) indicato sopra.
* **Impostazioni generali**
   * **Chiavi metadati utente**
      * Chiavi metadati disponibili per il MVPD specifico.
   * **Proprietà dati client**
      * **Auth / Aggregator** - Se impostato su &quot;Sì&quot;, è necessario un nuovo token di autenticazione per ogni nuovo canale a cui l&#39;utente sta tentando di accedere.
      * **AuthN passivo abilitato** - Se il flag Auth/Aggregator è impostato su &quot;Sì&quot; e se AuthN passivo abilitato è impostato su &quot;Sì&quot;, il processo di autenticazione con un altro canale verrà eseguito in background senza che sia necessario un reindirizzamento completo del browser e che venga visualizzato il selettore.
      * **Sessione autenticazione/browser** - Se impostato su &quot;Sì&quot;, l&#39;utente verrà disconnesso dopo la chiusura del browser. Se è impostato su &quot;No&quot;, l’utente può riavviare il browser e rimanere connesso.
      * **IFrame Argomento obbligatorio** - Se impostato su &quot;Sì&quot;, indica che la finestra di accesso di MVPD richiede un iFrame. I campi &quot;Larghezza iFrame&quot; e &quot;Altezza iFrame&quot; rappresentano le dimensioni necessarie per l’iFrame che carica la pagina di accesso di MVPD.
* **Impostazioni autenticazione**
   * **Seleziona endpoint**
      * Questo campo indica gli endpoint di autenticazione esposti da MVPD. L’endpoint può variare a seconda del protocollo di autenticazione utilizzato.
   * **Impostazioni generali AuthN**
      * In questa scheda secondaria viene visualizzato il protocollo di autenticazione utilizzato da MVPD e le informazioni relative al protocollo.
   * **Certificati AuthN**
      * Questa scheda secondaria mostra i certificati utilizzati da MVPD nel flusso di autenticazione insieme all’organizzazione dell’emittente, alla data di emissione e alla data di scadenza. Questi certificati fungono da chiavi pubbliche/private e vengono utilizzati a scopo di convalida.
   * **Regole dinamiche AuthN**
      * Questa scheda secondaria mostra le regole applicabili al processo di autenticazione. Premendo il pulsante Request / Response / Token (Richiesta/Risposta/Token) del diagramma, puoi visualizzare come evidenziato i parametri applicati a quella parte del flusso di autenticazione.
* **Impostazioni autorizzazione**
   * **Seleziona endpoint**
      * Questo campo indica l’endpoint di autorizzazione esposto da MVPD. L’endpoint può variare a seconda del protocollo di autorizzazione utilizzato. I protocolli di autorizzazione disponibili sono SOAP, REST (per dispositivi senza client), SAML, XACML e OAUTH.
   * **Impostazioni generali AuthZ**
      * In questa scheda secondaria viene visualizzato il protocollo di autorizzazione utilizzato da MVPD e le informazioni relative al protocollo.
      * **Configurazione verifica preliminare**
         * Descrive il numero di risorse che possono essere preautorizzate da un MVPD in una singola chiamata, il modello PreFlight utilizzato, nonché la soglia di timeout. A volte, il numero di risorse può essere diverso per una determinata integrazione. Questa operazione può essere gestita modificando la proprietà &quot;**Numero massimo di risorse verifica preliminare**&quot;, disponibile nella scheda Impostazioni generali. Questa proprietà è disponibile solo per una determinata integrazione e, se impostata, verrà utilizzata al posto del valore definito in Impostazioni autorizzazione -> Configurazione pre-volo -> Risorse pre-volo max.
      * **Protezione DOS**
         * Descrive la protezione Denial of Service sull’endpoint di autorizzazione MVPD. Per una descrizione esatta di ciascun campo, visualizzare le descrizioni comandi passando con il mouse sopra i campi di protezione DOS.
      * Se il MVPD è un **TempPass**, le **Impostazioni generali AuthZ** contengono anche informazioni relative alla durata di TempPass.
      * Se MVPD è un **FlexibleTempPass**, le **Impostazioni generali AuthZ** contengono anche informazioni relative alla durata TempPass, al numero massimo di risorse e al campo di identificazione (vedi l&#39;immagine seguente).
   * **Certificati AuthZ**
      * Questa scheda secondaria mostra i certificati utilizzati da MVPD nel flusso di autorizzazione insieme all’organizzazione dell’emittente, alla data di emissione e alla data di scadenza. Questi certificati fungono da chiavi pubbliche/private e vengono utilizzati a scopo di convalida.
   * **Regole dinamiche AuthZ**
      * In questa scheda secondaria vengono visualizzate le regole applicabili al processo di autorizzazione. Premendo sul **Richiesta / Risposta / Token** del diagramma, puoi visualizzare come evidenziati i parametri applicati a quella parte del flusso di autorizzazione.
* **Impostazioni disconnessione**
   * **Seleziona endpoint**
      * Questo campo indica l&#39;endpoint di disconnessione esposto da MVPD. I protocolli forniti possono essere SAML o OAuth2.
      * **Impostazioni generali disconnessione**
         * In questa scheda secondaria viene visualizzato il protocollo di disconnessione utilizzato da MVPD e le relative informazioni sul protocollo.
         * **Richiedi risposta disconnessione firmata** - Se impostato su &quot;Sì&quot;, la risposta deve essere firmata da un certificato attendibile.
      * **Certificati di disconnessione**
         * In questa scheda secondaria vengono visualizzati i certificati utilizzati da MVPD nel flusso di disconnessione insieme all&#39;organizzazione emittente, alla data di emissione e alla data di scadenza. Questi certificati fungono da chiavi pubbliche/private e vengono utilizzati a scopo di convalida.
      * **Logout regole dinamiche**
         * In questa scheda secondaria vengono visualizzate le regole applicabili al processo di disconnessione. Premendo sul **Richiesta / Risposta / Token** del diagramma, puoi visualizzare come evidenziati i parametri applicati a quella parte del flusso di disconnessione.

### Rapporti {#tve-db-reports-sec}

Per passare a questa sezione, fai clic su &quot;Rapporti&quot; nel menu &quot;[Sezioni dashboard](#sections)&quot;. Verrà visualizzata una schermata con 3 schede, che verranno presentate in dettaglio nelle seguenti sottosezioni: [Report TTL AuthN](#authn-ttl-reports), [Report TTL AuthZ](#authz-ttl-reports), [Report SSO](#sso-reports).

Questa sezione consente di visualizzare ed esportare dati aggregati per diversi tipi di rapporti per le integrazioni Canale/i con vari MVPD su tutte le piattaforme.

#### Piattaforme {#report-platforms}

Tutti i valori aggregati dei rapporti nelle seguenti piattaforme:

**BROWSER**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite Adobe Pass Authentication JavaScript SDK.

**DISPOSITIVI MOBILI: IOS**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite Adobe Pass Authentication iOS SDK.

**DISPOSITIVI MOBILI: ANDROID**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite Adobe Pass Authentication Android SDK.

**DISPOSITIVI MOBILI: ALTRI**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite l’API REST di autenticazione di Adobe Pass, sviluppata per i dispositivi mobili.

**TVCD: ROKU**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite l’API REST di autenticazione di Adobe Pass e che inviano &quot;Roku&quot; come tipo di dispositivo.

**TVCD: FIRETV**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite Adobe Pass Authentication FireTV SDK.

**TVCD: APPLETV**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite Adobe Pass Authentication tvOS SDK.

**TVCD: OTHER**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite l’API REST di autenticazione di Adobe Pass, sviluppata per i dispositivi connessi alla TV.

**PIATTAFORMA: SCONOSCIUTA**
Visualizza i valori che verranno applicati alle implementazioni del programmatore per le quali i servizi di autenticazione di Adobe Pass rilevano un tipo di dispositivo sconosciuto.

Rivedi il meccanismo di [trasferimento delle informazioni client](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md) alle API REST o agli SDK di Adobe Pass Authentication per ulteriori dettagli su come inviare il tipo di dispositivo desiderato (ad esempio,&quot;Roku&quot;).

Tutti i rapporti aggregano i valori calcolati in base alla configurazione specifica per ogni ambiente di autenticazione Adobe Pass. Pertanto, puoi aspettarti dati di reporting diversi quando passi da un ambiente all’altro del dashboard TVE.

Consulta la sezione [Ambienti](#authn-environments) per ulteriori dettagli relativi agli ambienti disponibili per l&#39;autenticazione Adobe Pass.


##### Selezione di canali/MVPD specifici {#selecting-specific-channels-mvpds}

Tutti i rapporti consentono l’utilizzo di filtri selezionando canali specifici o selezionando MVPD specifici da includere nei rapporti risultanti.

Per selezionare uno o più canali, utilizza l&#39;elenco a discesa **1} inserito dopo l&#39;etichetta &quot;Canali selezionati per il report&quot;.** Vedere la Figura 8./9./10. immagini dal basso.

Per selezionare uno o più MVPD/s, utilizza l&#39;**elenco a discesa** inserito dopo l&#39;etichetta &quot;MVPD selezionati per il report&quot;. Vedere la Figura 8./9./10. immagini dal basso.

Per impostazione predefinita, i dati vengono aggregati in tutti i canali della tua azienda (&quot;Tutti i canali&quot;) e negli MVPD con cui sono integrati (&quot;Tutti gli MVPD&quot;).

Se scegli di deselezionare &quot;Tutti i canali&quot; o &quot;Tutti gli MVPD&quot; senza scegliere opzioni specifiche, nell’interfaccia utente viene visualizzato il segnaposto &quot;Nessun dato disponibile&quot;.


##### Esporta report {#export-report}

Tutti i rapporti consentono l’esportazione di dati in un file in formato CSV.

Per esportare i dati, utilizza il pulsante &quot;Export Report&quot; (Esporta rapporto) posizionato nell’angolo in alto a destra della finestra. Vedere la Figura 8./9./10. immagini dal basso.

Un file denominato **Report.csv** verrà scaricato automaticamente nel computer. Assicurati pertanto che le impostazioni del browser consentano il download dei file.

Durante il calcolo del file Report.csv, l&#39;icona di caricamento &quot;Esportazione dei dati&quot; sarà presente sullo schermo. L&#39;operazione può richiedere da **a un paio di minuti**, a seconda delle dimensioni dei dati da esportare.

#### Rapporti TTL AuthN (#authn-ttl-reports)

Questo rapporto visualizza il valore TTL (Time-To-Live) del token di autenticazione configurato per le integrazioni canale/i con vari MVPD su tutte le piattaforme.

Il valore Time-To-Live del token di autenticazione, indicato anche come **AuthN TTL**, viene visualizzato in valori leggibili dall&#39;utente come: **giorni, ore, minuti, secondi**.

In termini di esperienza utente, i rapporti TTL di AuthN consentono di controllare visivamente la quantità di tempo in cui un utente verrà autenticato considerando una specifica piattaforma e un MVPD specifico.

Per passare a questo tipo di report, fai clic sulla scheda &quot;AuthN TTL Reports&quot; (Rapporti TTL di autenticazione) nella sezione &quot;Reports&quot; (Rapporti).

![Report TTL AuthN](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-authn-ttl-export-button.png)

*Figura 8: scheda Rapporto TTL autenticazione dashboard Adobe Primetime TVE*

La tabella Rapporti TTL AuthN contiene pagine ed è scorrevole in orizzontale e verticale a seconda delle dimensioni dello schermo.

Se prendi in considerazione di apportare una modifica a un valore TTL AuthN, controlla la sezione [Integrazioni](#tve-db-integrations-sec).

>[!IMPORTANT]
>Il segnaposto &quot;**Impostato da MVPD**&quot; viene utilizzato nei casi in cui MVPD sarà quello che applica il valore TTL AuthN e non la configurazione di autenticazione Adobe Pass.


#### Rapporti TTL AuthZ {#authz-ttl-reports}

Questo rapporto visualizza il valore TTL (Time-To-Live) del token di autorizzazione configurato per le integrazioni canale/i con vari MVPD su tutte le piattaforme.

Il valore Time-To-Live del token di autorizzazione, indicato anche come **AuthZ TTL**, viene visualizzato in valori leggibili dall&#39;utente quali: **giorni, ore, minuti, secondi**.

In termini di esperienza utente, i rapporti TTL di AuthZ consentono di controllare visivamente la quantità di tempo in cui un utente verrà autorizzato considerando una specifica piattaforma e un MVPD specifico.

Per passare a questo tipo di rapporto, fai clic sulla scheda &quot;Rapporti TTL AuthZ&quot; nella sezione &quot;Rapporti&quot;.

![Rapporti TTL AuthZ](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-authz-ttl-export-button.png)

*Figura 9. Scheda Report TTL AuthZ dashboard Adobe Primetime TVE*

La tabella Rapporti TTL AuthZ contiene pagine ed è scorrevole in orizzontale e verticale a seconda delle dimensioni dello schermo.

Se pensi di apportare una modifica a un valore TTL AuthZ, consulta la sezione [Integrazioni](#tve-db-integrations-sec).

>[!IMPORTANT]
>Il segnaposto &quot;**Impostato da MVPD**&quot; viene utilizzato nei casi in cui MVPD sarà quello che applica il valore TTL AuthZ e non la configurazione di autenticazione Adobe Pass.


#### Rapporti SSO {#sso-reports}

Questo rapporto visualizza lo stato Single Sign-On (SSO) configurato per le integrazioni canale/i con vari MVPD su tutte le piattaforme.

Lo stato Single Sign-On, indicato anche come **Stato SSO**, viene visualizzato come tristato con i seguenti valori possibili: **SSO disabilitato, SSO abilitato, SSO incerto**.

In termini di esperienza utente, i rapporti SSO consentono di esaminare visivamente l’esperienza SSO di autenticazione utente prevista considerando un MVPD specifico e una piattaforma specifica.

Per passare a questo tipo di report, fare clic sulla scheda &quot;**Report SSO**&quot; dalla sezione &quot;**Report**&quot;.


![Scheda report SSO dashboard TVE](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-sso-export-button.png)


*Figura 10: scheda Rapporti SSO del dashboard di Adobe Primetime TVE*

La tabella Rapporti SSO contiene pagine ed è scorrevole in orizzontale e verticale a seconda delle dimensioni dello schermo.

Se pensi di apportare una modifica allo stato di un SSO, controlla la sezione [Integrazioni](#tve-db-integrations-sec).

>[!IMPORTANT]
>Il segnaposto &quot;**SSO incerto**&quot; viene utilizzato nei casi in cui l&#39;SSO è abilitato e possibile, ma le impostazioni della piattaforma utente e le decisioni dell&#39;utente (ad esempio, l&#39;opzione del browser utente per bloccare i cookie di terze parti, la scelta dell&#39;utente di negare l&#39;accesso alla piattaforma al proprio abbonamento al provider TV) o le impostazioni di MVPD (ad esempio, la richiesta di autenticazione da parte di MVPD per ogni canale) potrebbero impedire l&#39;esecuzione dell&#39;SSO.

### Registro modifiche {#tve-db-changelog-sec}

In questa sezione viene visualizzato un elenco di tutte le modifiche inviate tramite il dashboard TVE all’ambiente e alla configurazione di autenticazione di Adobe Pass.

Sono disponibili colonne che indicano la data push, l’utente che ha eseguito la modifica e lo stato del push.

Questa sezione consente anche il confronto di due voci di tabella per limitare le modifiche specifiche che si desidera verificare e condividere il confronto come un elemento di posta.

### Feedback {#tve-db-feedback-sec}

Questa sezione consente agli utenti di inviare feedback. Segui i passaggi per fornire un feedback al team di prodotto per l’autenticazione di Adobe Pass:

* fai clic sul pulsante &quot;Feedback&quot; sul lato destro dello schermo
* immetti l’oggetto
* immetti il messaggio
* se necessario, carica una schermata nel messaggio facendo clic sul pulsante &quot;Carica schermata&quot;
* fai clic sul pulsante &quot;Invia&quot;

![modulo di feedback del dashboard](../../../assets/tve-dashboard/old-tve-dashboard/tve-dashboard-feedback.png)

*Figura 11: sezione Feedback del dashboard di Adobe Primetime TVE*

Per istruzioni su come acquisire le schermate, consulta i collegamenti seguenti:

* [Acquisire screenshot in Windows](https://support.microsoft.com/en-us/windows/use-snipping-tool-to-capture-screenshots-00246869-1843-655f-f220-97299b865f6b#1TC=windows-7)

* [Acquisire screenshot in Mac](https://support.apple.com/en-us/HT201361)

## Risoluzione dei problemi {#tve-db-troubleshoot}

### Modalità di manutenzione {#maintenance-mode}

![App TVE in modalità manutenzione](../../../assets/tve-dashboard/old-tve-dashboard/tveapp-maintenance-mode.png)


*Figura: app TVE in modalità manutenzione*


Se TVE Dashboard è in &quot;modalità di manutenzione&quot;, gli utenti non potranno visualizzare o apportare nuove modifiche.

In questo caso, dovrai aspettare che il team di progettazione dell’autenticazione di Adobe Pass finisca il lavoro di manutenzione sul dashboard TVE.

### Stato degradato {#degraded-state}

![App TVE in uno stato degradato](../../../assets/tve-dashboard/old-tve-dashboard/tve-degraded-state.png)


*Figura: app TVE in uno stato degradato*

Se il dashboard TVE è in &quot;stato degradato&quot;, gli utenti non disporranno delle funzionalità di ricerca e ordinamento, ma potranno visualizzare o apportare nuove modifiche.

In questo caso, dovrai aspettare che il team di progettazione dell’autenticazione di Adobe Pass finisca il lavoro di manutenzione sul dashboard TVE.
