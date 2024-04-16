---
title: Guida utente di Primetime TVE Dashboard
description: Guida utente di Primetime TVE Dashboard
exl-id: 6f7f7901-db3a-4c68-ac6a-27082db9240a
source-git-commit: c6afb9b080ffe36344d7a3d658450e9be767be61
workflow-type: tm+mt
source-wordcount: '4377'
ht-degree: 0%

---

# Guida utente di Primetime TVE Dashboard {#tve-db-user-guide}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#tve-db-intro}

[[!DNL Adobe] Dashboard TVE (Dashboard TVE)](https://console.auth.adobe.com/) è un dashboard self-service destinato agli utenti che lavorano per aziende del settore media (Programmatori) che hanno una relazione commerciale con il team di prodotto Adobe Pass Authentication.

Contatta il tuo Technical Account Manager (TAM) per ottenere l’accesso. Per ottenere l’accesso, dovrai configurare due nuovi gruppi di utenti nell’organizzazione Adobe Marketing Cloud:

* Lettura/scrittura dashboard TVE: i membri di questo gruppo dispongono dei diritti completi su tutte le sezioni modificabili del dashboard
* TVE Dashboard di sola lettura: i membri di questo gruppo dispongono solo dei diritti di visualizzazione per l&#39;intero dashboard


Prima di approfondire questa guida utente, ti consigliamo di esaminare le seguenti risorse per comprendere i flussi e le funzioni forniti dal team di prodotto per l’autenticazione di Adobe Pass e acquisire familiarità con i termini utilizzati nel presente documento:

* [Documento tecnico TVE](/help/authentication/technical-paper.md)
* [Guida di Kick-Start per programmatori](/help/authentication/programmer-kickstart-guide.md)
* [Flusso diritto](/help/authentication/entitlement-flow.md)
* [Glossario](/help/authentication/glossary.md)


Passando alle sezioni successive di questa guida utente, scoprirai come amministrare diverse impostazioni per i canali, i programmatori o le integrazioni tra canali e MVPD (Multichannel Video Program Distributors) della tua azienda.

>[!IMPORTANT]
>Dashboard TVE offre la possibilità di passare da un’area di lavoro di base a un’area di lavoro avanzata. Per farlo, attiva l’icona in alto a destra. Advanced Workspace è destinato agli utenti con conoscenze tecniche sostanziali e conoscenze avanzate delle funzioni offerte dal team di prodotto per l’autenticazione di Adobe Pass.

![Aree di lavoro del dashboard TVE](assets/tve-basic-advanced-workspace.png)

*Figura 1: Elenco a discesa Dashboard Adobe Primetime TVE &quot;Basic / Advanced Workspace&quot;*

## Ambienti {#authn-environments}

A seconda delle attività che un utente potrebbe dover svolgere, potrebbe dover passare da un ambiente di autenticazione Adobe Pass all’altro. Per informazioni dettagliate sugli ambienti di autenticazione di Adobe Pass, consulta il seguente documento: [Informazioni sugli ambienti di autenticazione di Adobe Pass](/help/authentication/understanding-the-adobe-environments.md).

TVE Dashboard fornisce due ambienti denominati Prequal (Prequalifica) e Release, ciascuno con due profili denominati Staging e Produzione, come illustrato di seguito:

* [Staging Prequal](https://console-prequal.auth-staging.adobe.com/)
* [Prequal Production](https://console-prequal.auth.adobe.com/)
* [Staging rilascio](https://console.auth-staging.adobe.com/)
* [Rilascia produzione](https://console.auth.adobe.com/)

Per passare da un ambiente all’altro, l’utente può fare clic sull’ambiente desiderato rappresentato dalla voce presente nell’elemento a discesa illustrato di seguito:

![Menu a discesa degli ambienti del dashboard TVE](assets/tve-dashboard-env.png)

*Figura 2: menu a discesa degli ambienti della dashboard di Adobe Pass TVE*

>[!IMPORTANT]
>
>È molto importante notare che quando si apportano modifiche amministrative alla configurazione di autenticazione di Adobe Pass tramite la dashboard TVE, si consiglia vivamente di seguire la sequenza seguente per garantire la funzionalità corretta.

Per apportare modifiche amministrative alla configurazione dell’autenticazione Adobe Pass tramite il dashboard TVE:

* Eseguire le modifiche in [Rilasciare staging e convalidarli](http://sp.auth-staging.adobe.com/apitest/api.html).
* Eseguire le modifiche in [Produzione Prequal e convalida](http://sp.auth-staging.adobe.com/apitest/api.html).
* Eseguire le modifiche in [Rilasciare la produzione e convalidarla](http://sp.auth-staging.adobe.com/apitest/api.html).

>[!IMPORTANT]
>
>Affinché le modifiche amministrative diventino effettive, gli utenti devono passare alla sezione &quot;Revisione e push delle modifiche&quot; selezionando il pulsante, che verrà visualizzato nella parte inferiore sinistra della barra laterale, per rivedere le modifiche, aggiungere una descrizione per le modifiche appena create e confermare l’aggiornamento della configurazione selezionando &quot;Configurazione push&quot;.

![Revisione di una notifica push tramite Tve Dashboard](assets/tve-review-push-notifications.png)

*Figura 3: Revisione del dashboard di Adobe Primetime TVE e notifica delle modifiche push*

## Sezioni {#sections}

Gli utenti che lavorano per le società di media (programmatori) possono accedere alle seguenti sezioni della dashboard di TVE dalla barra laterale:

* **Canali** - Contiene le impostazioni relative ai provider di contenuti
* **Programmatori** : contiene le impostazioni relative all’organizzazione principale che aggrega una o più **Canali**
* **Integrazioni** : contiene le impostazioni relative all’integrazione tra **Canali** e **MVPD**
* **MVPD** : contiene le impostazioni relative al **MVPD**
* **Rapporti** - Contiene dati aggregati per tre tipi di rapporti: AuthN TTL, AuthZ TTL, SSO
* **Registro modifiche** - Contiene le modifiche più recenti applicate alla configurazione del dashboard TVE

![Sezioni del dashboard TVE](assets/tve-dashboard-sections.png)

*Figura 4: Sezioni del dashboard di Adobe Primetime TVE*

### Canali {#tve-db-channels-section}

Questa sezione consente di visualizzare e modificare le impostazioni per i canali disponibili o di crearne uno nuovo. Facendo clic su uno dei canali disponibili viene visualizzata una schermata con le seguenti schede:

* **Dati canale**
   * **ID canale** : l’ID univoco del canale utilizzato nel nostro sistema, detto anche &quot;ID richiedente&quot;.
   * **Nome visualizzato** - Il nome commerciale del canale.
* **Impostazioni generali**
   * **Configurazione analisi** : configura gli eventi di autenticazione di Adobe Pass da inoltrare ad Adobe Analytics. Contatta l’Adobe per ulteriori dettagli su come configurare l’ID suite di rapporti (RSID) prima di abilitare questa funzione.
* **Certificati**

  Contiene l’elenco dei certificati utilizzati nel flusso di autenticazione insieme all’organizzazione di emissione, alla data di rilascio e alla data di scadenza. Questi certificati fungono da chiavi pubbliche/private e vengono utilizzati a scopo di convalida.
* **Domini**

  Contiene l’elenco dei domini da cui il rispettivo Canale comunicherà con l’autenticazione di Adobe Pass.
* **Integrazioni**

  Contiene l’elenco delle integrazioni con gli MVPD disponibili, insieme allo stato di ogni integrazione che potrebbe essere abilitata o meno. Per accedere alla pagina Integrazione, fai clic su una voce specifica.
* **Applicazioni registrate**

  Contiene l’elenco delle registrazioni delle applicazioni. Per ulteriori dettagli, consulta il documento [Gestione registrazione client dinamici](/help/authentication/dynamic-client-registration-management.md).

* **Schemi personalizzati**

  Contiene l’elenco degli schemi personalizzati. Per ulteriori dettagli, consulta [Registrazione applicazione iOS/tvOS](/help/authentication/iostvos-application-registration.md) e [Gestione registrazione client dinamici](/help/authentication/dynamic-client-registration-management.md)


#### Aggiungi/Elimina domini {#add-delete-domains}

Per avviare il processo di aggiunta di un nuovo dominio per il canale selezionato, fai clic sul pulsante &quot;Aggiungi nuovo dominio&quot; sotto l’elenco Domini. Verrà creata una nuova voce di dominio in cui è possibile specificare il nome di dominio. Se nell’elenco dei domini è già presente un dominio più generico, non è consigliabile aggiungere un nuovo sottodominio.

![Aggiungere un nuovo dominio a una sezione di canale selezionata](assets/add-domain-to-channel-sec.png)

*Figura: Scheda Domini nei canali*

### Programmatori {#tve-db-programmers-section}

Questa sezione consente di visualizzare e modificare le impostazioni per i programmatori disponibili o di crearne una nuova. Facendo clic su uno dei programmatori disponibili viene visualizzata una schermata con le seguenti schede:

* **Dati del programmatore**
   * **ID programmatore** - L’ID univoco del programmatore utilizzato nel nostro sistema.
   * **Nome visualizzato** - Il nome commerciale del programmatore.
   * **URL logo** - Il logo commerciale del programmatore Uniform Resource Locator (URL).
   * **Anteprima logo** - Anteprima del logo commerciale del programmatore scaricandola dall&#39;URL (Uniform Resource Locator) di cui sopra.

* **Certificati**

  Contiene l’elenco dei certificati utilizzati nel flusso di autenticazione insieme all’organizzazione di emissione, alla data di rilascio e alla data di scadenza. Questi certificati fungono da chiavi pubbliche/private e vengono utilizzati a scopo di convalida.

* **Canali**

  Contiene l’elenco dei canali appartenenti a questo programmatore specifico. Per passare alla sezione Canali, fai clic su una voce specifica.

* **Applicazioni registrate**

  Contiene l’elenco delle registrazioni delle applicazioni. Per ulteriori dettagli, consulta [Gestione registrazione client dinamici](/help/authentication/dynamic-client-registration-management.md).

* **Schemi personalizzati**

  Contiene l’elenco degli schemi personalizzati. Per ulteriori dettagli, consulta [Registrazione applicazione iOS/tvOS](/help/authentication/iostvos-application-registration.md) e [Gestione registrazione client dinamici](/help/authentication/dynamic-client-registration-management.md).


### Integrazioni {#tve-db-integrations-sec}

Questa sezione consente di visualizzare e modificare le impostazioni per le integrazioni tra canali e MVPD disponibili o di crearne una nuova. Facendo clic su una delle integrazioni disponibili, viene restituita una singola pagina quando si utilizza l’area di lavoro di base o una schermata con le seguenti schede quando si utilizza l’area di lavoro avanzata:

* **Dati di integrazione**
   * **ID integrazione**- Il risultato dell&#39;aggiunta dell&#39;ID univoco degli MVPD all&#39;ID univoco del canale separato dal carattere &quot;_&quot;.
   * **Nome visualizzato canale** - Il nome commerciale del canale.
   * **ID canale** : l’ID univoco del canale utilizzato nel nostro sistema, detto anche &quot;ID richiedente&quot;.
   * **Nome visualizzato MVPD** - il nome commerciale dell&#39;MVPD.
   * **ID MVPD** - L&#39;ID univoco di MVPD utilizzato nel nostro sistema.
* **Impostazioni generali**
   * **Chiavi metadati utente** : configura le chiavi di metadati disponibili per l’integrazione specifica.
   * **Impostazioni specifiche della piattaforma** : configura impostazioni diverse per una piattaforma specifica (ad esempio, TTL, SSO e IFrames).

* **Impostazioni di autenticazione**
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
* a seconda delle impostazioni MVPD può apparire una finestra a comparsa e richiedere proprietà aggiuntive, che avrebbero dovuto essere fornite in precedenza da MVPD, altrimenti si verificherà un reindirizzamento alla pagina di integrazione appena creata

![](assets/new-integration-window.png)



*Figura 5. Finestra Nuova integrazione del dashboard di Adobe Primetime TVE*


#### Integrazione di aggiornamento {#update-integration}

Per aggiornare un’integrazione esistente, fai clic sulla voce della tabella per tale integrazione specifica dalla sezione Integrazioni o dalla sezione Canali, che contiene una scheda Integrazioni.

Quando si utilizza la modalità Workspace di base, questa sezione consente di visualizzare e modificare le impostazioni più comunemente aggiornate, ad esempio i TTL (time-to-live) dei token di autenticazione e autorizzazione e le impostazioni iFrame. Tieni presente che le impostazioni TTL possono mancare per le integrazioni con MVPD che supportano il TTL di persistenza del token definito in modo dinamico (vedi la voce 1.19 da [Requisiti di integrazione MVPD](/help/authentication/mvpd-integr-features.md)).



Quando si utilizza la modalità Area di lavoro avanzata, questa sezione consente di visualizzare e modificare le impostazioni meno comuni.



Nel caso delle modalità Workspace di base e avanzata, queste impostazioni possono essere modificate a livello di piattaforma (ad esempio, seleziona un valore personalizzato per il token TTL di autorizzazione su Android, impostazione predefinita su ogni altra piattaforma).



>[!IMPORTANT]
>È importante comprendere la catena di ereditarietà delle impostazioni: MVPD -> Endpoint MVPD -> Integrazione -> Piattaforma, dove Platform ha il valore più specifico e MVPD il valore predefinito più generico.

![](assets/inheritance-chain-component.png)


*Figura 6. Componente catena di ereditarietà della proprietà Adobe Primetime TVE Dashboard*


#### Impostazioni specifiche della piattaforma {#platform-sp-settings}

Questa sottosezione può essere utilizzata per ignorare le impostazioni per piattaforme specifiche. Le piattaforme disponibili sono:

* **Tutte le piattaforme** : imposta i valori che verranno applicati a tutte le piattaforme indipendentemente dalle implementazioni del programmatore, nel caso in cui non siano impostati altri valori per una piattaforma specifica.
* **Android** : imposta i valori che verranno applicati alle implementazioni del programmatore tramite l’SDK Android per l’autenticazione di Adobe Pass.
* **API REST senza client** : imposta i valori che verranno applicati alle implementazioni del programmatore tramite l’API REST di autenticazione di Adobe Pass.
* **Fire TV** : imposta i valori che verranno applicati alle implementazioni del programmatore tramite l’SDK FireTV di autenticazione Adobe Pass.
* **SDK per Flash** - Piattaforma obsoleta. **obsoleto**
* **SDK JavaScript** : imposta i valori che verranno applicati alle implementazioni del programmatore tramite l’SDK JavaScript di autenticazione di Adobe Pass.
* **Roku** : imposta i valori che verranno applicati alle implementazioni del programmatore tramite l’API REST di autenticazione di Adobe Pass e che inviano &quot;Roku&quot; come tipo di dispositivo. Nel caso di dispositivi Roku, questo ha la precedenza sui valori impostati per la piattaforma API REST senza client.
* **SDK nativo per Xbox** - Piattaforma obsoleta. **obsoleto**
* **API REST Xbox 360** : imposta i valori che verranno applicati alle implementazioni del programmatore tramite l’API REST di autenticazione di Adobe Pass e che inviano &quot;xbox&quot; come tipo di dispositivo. Questo ha la precedenza sui valori impostati per la piattaforma API REST senza client nel caso di dispositivi Xbox 360.
* **API REST di Xbox One** : imposta i valori che verranno applicati alle implementazioni del programmatore tramite l’API REST di autenticazione di Adobe Pass e che inviano &quot;xboxOne&quot; come tipo di dispositivo. Questo ha la precedenza sui valori impostati per la piattaforma API REST senza client nel caso di dispositivi Xbox One.
* **iOS** : imposta i valori che verranno applicati alle implementazioni del programmatore tramite l’SDK di Adobe Pass Authentication per iOS.
* **tvOS** : imposta i valori che verranno applicati alle implementazioni del programmatore tramite SDK tvOS per l’autenticazione di Adobe Pass.


![](assets/platform-sp-settings.png)

*Figura 7. Impostazioni specifiche della piattaforma Adobe Primetime TVE Dashboard*


#### Abilita Single Sign-On di Platform {#enable-platform-sso}

Per abilitare/disabilitare l&#39;accesso Single Sign On per un&#39;integrazione e una piattaforma specifiche, eseguire la procedura seguente:

* assicurati di utilizzare la modalità Advanced Workspace
* passa all’integrazione desiderata
* passa a **Impostazioni generali** scheda
* selezionare la piattaforma desiderata su cui si desidera attivare o disattivare Single Sign-On
* attiva/disattiva **Abilita Single Sign-On** contrassegno al valore desiderato (Sì / No)

  >[!IMPORTANT]
  >È importante notare che il **Abilita Single Sign-On** Il flag è disponibile solo per le piattaforme iOS, tvOS, Roku e FireTV e solo per le integrazioni con MVPD che supportano il Single Sign On per tali piattaforme.

* attiva/disattiva **Applica autorizzazione piattaforma** contrassegno al valore desiderato (Sì / No)

  >[!IMPORTANT]
  >È importante notare che il **Applica autorizzazione piattaforma** Il flag controlla se la decisione dell’utente di consentire o negare l’accesso alla piattaforma al proprio abbonamento al provider TV verrà applicata o meno. Considerare lo scenario quando **Abilita Single Sign-On** il flag è impostato su &quot;Sì&quot;, **Applica autorizzazione piattaforma** Inoltre, il flag è impostato su &quot;Sì&quot; e l’utente sceglie di negare l’accesso della piattaforma al proprio abbonamento al provider TV, in modo che la rispettiva applicazione (canale) non sia in grado di utilizzare il token di autenticazione di Adobe Pass ottenuto da un’altra applicazione (canale).


#### Abilita autenticazione basata sulla home page {#enable-hba}

Attenersi alla procedura seguente per abilitare/disabilitare l&#39;autenticazione di base per **OAuth2** MVPD basati su:

* assicurati di utilizzare la modalità Advanced Workspace
* passa all’integrazione desiderata
* passa a **Impostazioni di autenticazione** scheda
* passa a **Regole dinamiche AuthN** scheda secondaria
* attiva/disattiva **Tentativo HBA** contrassegno al valore desiderato (Sì / No)


>[!IMPORTANT]
>Tenere presente che il valore &quot;HBA AuthN TTL&quot; non deve mai essere sostituito, altrimenti il flusso di autorizzazione potrebbe non riuscire in modo imprevisto.

Contatta in **tve-support@adobe.com** per informazioni sull&#39;abilitazione dell&#39;autenticazione Home-Base per MVPD basati su SAML.

### MVPD {#tve-db-mvpds-sec}

Questa sezione consente di visualizzare le impostazioni per gli MVPD disponibili. Facendo clic su uno degli MVPD disponibili viene visualizzata una schermata con le seguenti schede:

* **Dati MVPD**
   * **ID MVPD** - L&#39;ID univoco di MVPD utilizzato nel nostro sistema.
   * **Nome visualizzato** - Il nome commerciale dell&#39;MVPD che potrebbe essere utilizzato nel selettore dell&#39;utente.
   * **URL logo** - L&#39;URL (Uniform Resource Locator) del logo commerciale del MVPD.
   * **Anteprima logo** : anteprima del logo commerciale MVPD scaricandola dall’URL (Uniform Resource Locator) indicato sopra.
* **Impostazioni generali**
   * **Chiavi metadati utente**
      * Chiavi di metadati disponibili per l’MVPD specifico.
   * **Proprietà dati client**
      * **Auth / Aggregator** - Se è impostato su &quot;Sì&quot;, è necessario un nuovo token di autenticazione per ogni nuovo canale a cui l’utente sta tentando di accedere.
      * **Autenticazione passiva abilitata** - Se il flag Auth / Aggregator è impostato su &quot;Sì&quot; e se AuthN passivo abilitato è impostato su &quot;Sì&quot;, il processo di autenticazione con un altro canale si verifica in background senza la necessità di un reindirizzamento completo del browser e viene visualizzato il selettore.
      * **Sessione di autenticazione/browser** - Se è impostato su &quot;Sì&quot;, l’utente verrà disconnesso dopo aver chiuso il browser. Se è impostato su &quot;No&quot;, l’utente può riavviare il browser e rimanere connesso.
      * **IFrame obbligatorio** - Se impostato su &quot;Yes&quot;, indica che la finestra di accesso MVPD richiede un iFrame. I campi &quot;Larghezza iFrame&quot; e &quot;Altezza iFrame&quot; rappresentano le dimensioni necessarie per l’iFrame che carica la pagina di accesso MVPD.
* **Impostazioni di autenticazione**
   * **Seleziona endpoint**
      * Questo campo indica gli endpoint di autenticazione esposti da MVPD. L’endpoint può variare a seconda del protocollo di autenticazione utilizzato.
   * **Impostazioni generali autenticazione**
      * In questa scheda secondaria viene visualizzato il protocollo di autenticazione utilizzato dall&#39;MVPD e le informazioni relative al protocollo.
   * **Certificati autenticazione**
      * In questa scheda secondaria vengono visualizzati i certificati utilizzati da MVPD nel flusso di autenticazione insieme all&#39;organizzazione emittente, alla data di emissione e alla data di scadenza. Questi certificati fungono da chiavi pubbliche/private e vengono utilizzati a scopo di convalida.
   * **Regole dinamiche AuthN**
      * Questa scheda secondaria mostra le regole applicabili al processo di autenticazione. Premendo il pulsante Request / Response / Token (Richiesta/Risposta/Token) del diagramma, puoi visualizzare come evidenziato i parametri applicati a quella parte del flusso di autenticazione.
* **Impostazioni autorizzazione**
   * **Seleziona endpoint**
      * Questo campo indica l’endpoint di autorizzazione esposto dall’MVPD. L’endpoint può variare a seconda del protocollo di autorizzazione utilizzato. I protocolli di autorizzazione disponibili sono SOAP, REST (per dispositivi senza client), SAML, XACML e OAUTH.
   * **Impostazioni generali AuthZ**
      * In questa scheda secondaria viene visualizzato il protocollo di autorizzazione utilizzato dall&#39;MVPD e le informazioni relative al protocollo.
      * **Configurazione verifica preliminare**
         * Descrive il numero di risorse che possono essere preautorizzate da un MVPD in una singola chiamata, il modello PreFlight utilizzato, nonché la soglia di timeout. A volte, il numero di risorse può essere diverso per una determinata integrazione. Questa operazione può essere gestita modificando il file &quot;**Numero massimo di risorse di verifica preliminare**&quot;, disponibile nella scheda Impostazioni generali. Questa proprietà è disponibile solo per una determinata integrazione e, se impostata, verrà utilizzata al posto del valore definito in Impostazioni autorizzazione -> Configurazione pre-volo -> Risorse pre-volo max.
      * **Protezione DOS**
         * Descrive la protezione Denial of Service sull&#39;endpoint di autorizzazione MVPD. Per una descrizione esatta di ciascun campo, visualizzare le descrizioni comandi passando con il mouse sopra i campi di protezione DOS.
      * Se il MVPD è un **TempPass**, quindi **Impostazioni generali AuthZ** contiene anche informazioni relative alla durata di TempPass.
      * Se il MVPD è un **FlexibleTempPass**, quindi **Impostazioni generali AuthZ** contiene anche informazioni relative alla durata TempPass, al numero massimo di risorse e al campo di identificazione (vedi l&#39;immagine seguente).
   * **Certificati AuthZ**
      * In questa scheda secondaria vengono visualizzati i certificati utilizzati da MVPD nel flusso di autorizzazione insieme all&#39;organizzazione emittente, alla data di emissione e alla data di scadenza. Questi certificati fungono da chiavi pubbliche/private e vengono utilizzati a scopo di convalida.
   * **Regole dinamiche AuthZ**
      * In questa scheda secondaria vengono visualizzate le regole applicabili al processo di autorizzazione. Premendo il tasto **Richiesta/Risposta/Token**, è possibile visualizzare come evidenziato i parametri applicati a quella parte del flusso di autorizzazione.
* **Impostazioni disconnessione**
   * **Seleziona endpoint**
      * Questo campo indica l&#39;endpoint di disconnessione esposto da MVPD. I protocolli forniti possono essere SAML o OAuth2.
      * **Impostazioni generali disconnessione**
         * In questa scheda secondaria viene visualizzato il protocollo di disconnessione utilizzato da MVPD e le informazioni relative al protocollo.
         * **Richiedi risposta disconnessione firmata** - Se è impostato su &quot;Sì&quot;, la risposta deve essere firmata da un certificato attendibile.
      * **Certificati di disconnessione**
         * In questa scheda secondaria vengono visualizzati i certificati utilizzati da MVPD nel flusso di disconnessione insieme all&#39;organizzazione emittente, alla data di emissione e alla data di scadenza. Questi certificati fungono da chiavi pubbliche/private e vengono utilizzati a scopo di convalida.
      * **Regole dinamiche di disconnessione**
         * In questa scheda secondaria vengono visualizzate le regole applicabili al processo di disconnessione. Premendo il tasto **Richiesta/Risposta/Token**, è possibile visualizzare come evidenziati i parametri applicati alla parte del flusso di logout.

### Rapporti {#tve-db-reports-sec}

Per passare a questa sezione, fai clic su &quot;Rapporti&quot; in &quot;[Sezioni del dashboard](#sections)&quot;. Si passa a una schermata con 3 schede, che vengono presentate in dettaglio nelle seguenti sottosezioni: [Rapporti TTL AuthN](#authn-ttl-reports), [Rapporti TTL AuthZ](#authz-ttl-reports), [Rapporti SSO](#sso-reports).

Questa sezione consente di visualizzare ed esportare dati aggregati per diversi tipi di rapporti per le integrazioni Canale/i con vari MVPD su tutte le piattaforme.

#### Piattaforme {#report-platforms}

Tutti i valori aggregati dei rapporti nelle seguenti piattaforme:

**BROWSER**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite l’SDK JavaScript di autenticazione di Adobe Pass.

**MOBILE: IOS**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite l’SDK iOS per l’autenticazione di Adobe Pass.

**MOBILE: ANDROID**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite l’SDK Android per l’autenticazione di Adobe Pass.

**DISPOSITIVI MOBILI: ALTRI**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite l’API REST di autenticazione di Adobe Pass, sviluppata per i dispositivi mobili.

**TVCD: ROKU**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite l’API REST di autenticazione di Adobe Pass e che inviano &quot;Roku&quot; come tipo di dispositivo.

**TVCD: FIRETV**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite l’SDK FireTV per l’autenticazione di Adobe Pass.

**TVCD: APPLETV**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite l’SDK tvOS per l’autenticazione di Adobe Pass.

**TVCD: ALTRO**
Visualizza i valori che verranno applicati alle implementazioni del programmatore tramite l’API REST di autenticazione di Adobe Pass, sviluppata per i dispositivi connessi alla TV.

**PIATTAFORMA: SCONOSCIUTA**
Visualizza i valori che verranno applicati alle implementazioni del programmatore per le quali i servizi di autenticazione di Adobe Pass rilevano un tipo di dispositivo sconosciuto.

Rivedi il meccanismo di [trasmissione delle informazioni client](/help/authentication/passing-client-information-device-connection-and-application.md) alle API REST o agli SDK di autenticazione di Adobe Pass per maggiori dettagli su come inviare il tipo di dispositivo desiderato (ad esempio, &quot;Roku&quot;).

Tutti i rapporti aggregano i valori calcolati in base alla configurazione specifica per ogni ambiente di autenticazione Adobe Pass. Pertanto, puoi aspettarti dati di reporting diversi quando passi da un ambiente all’altro del dashboard TVE.

Rivedi il [Ambienti](#authn-environments) per ulteriori dettagli relativi agli ambienti disponibili per l’autenticazione di Adobe Pass.


##### Selezione di canali/MVPD specifici {#selecting-specific-channels-mvpds}

Tutti i rapporti consentono l’utilizzo di filtri selezionando canali specifici o selezionando MVPD specifici da includere nei rapporti risultanti.

Per selezionare uno o più canali, utilizzare il **elenco a discesa** inserito dopo l’etichetta &quot;Canali selezionati per il rapporto&quot;. Vedere la Figura 8./9./10. immagini dal basso.

Per selezionare uno o più MVPD/s, utilizzare il **elenco a discesa** inserito dopo l’etichetta &quot;MVPD selezionati per il rapporto&quot;. Vedere la Figura 8./9./10. immagini dal basso.

Per impostazione predefinita, i dati vengono aggregati in tutti i canali della tua azienda (&quot;Tutti i canali&quot;) e negli MVPD con cui sono integrati (&quot;Tutti gli MVPD&quot;).

Se scegli di deselezionare &quot;Tutti i canali&quot; o &quot;Tutti gli MVPD&quot; senza scegliere opzioni specifiche, nell’interfaccia utente viene visualizzato il segnaposto &quot;Nessun dato disponibile&quot;.


##### Esporta report {#export-report}

Tutti i rapporti consentono l’esportazione di dati in un file in formato CSV.

Per esportare i dati, utilizza il pulsante &quot;Export Report&quot; (Esporta rapporto) posizionato nell’angolo in alto a destra della finestra. Vedere la Figura 8./9./10. immagini dal basso.

Un file denominato **Report.csv** verrà scaricato automaticamente nel computer. Assicurati pertanto che le impostazioni del browser consentano il download dei file.

L’icona di caricamento &quot;Esportazione dati&quot; sarà presente sullo schermo durante il calcolo del file Report.csv, che può **tra un paio di minuti** a seconda delle dimensioni dei dati da esportare.

#### Rapporti TTL AuthN (#authn-ttl-reports)

Questo rapporto visualizza il valore TTL (Time-To-Live) del token di autenticazione configurato per le integrazioni canale/i con vari MVPD su tutte le piattaforme.

Time-To-Live del token di autenticazione, noto anche come **TTL AuthN**, viene visualizzato in valori leggibili dall’utente come: **giorni, ore, minuti, secondi**.

In termini di esperienza utente, i rapporti TTL di AuthN consentono di controllare visivamente la quantità di tempo in cui un utente verrà autenticato considerando una specifica MVPD e una piattaforma specifica.

Per passare a questo tipo di report, fai clic sulla scheda &quot;AuthN TTL Reports&quot; (Rapporti TTL di autenticazione) nella sezione &quot;Reports&quot; (Rapporti).

![Rapporti TTL AuthN](assets/authn-ttl-reports.png)

*Figura 8: scheda Rapporto TTL autenticazione dashboard di Adobe Primetime TVE*

La tabella Rapporti TTL AuthN contiene pagine ed è scorrevole in orizzontale e verticale a seconda delle dimensioni dello schermo.

Se prendi in considerazione di apportare una modifica a un valore TTL AuthN, controlla i [Integrazioni](#tve-db-integrations-sec) sezione.

>[!IMPORTANT]
>La &quot;**Impostato da MVPD** Il segnaposto &quot; viene utilizzato nei casi in cui MVPD sarà quello che applica il valore TTL AuthN e non la configurazione di autenticazione Adobe Pass.


#### Rapporti TTL AuthZ {#authz-ttl-reports}

Questo rapporto visualizza il valore TTL (Time-To-Live) del token di autorizzazione configurato per le integrazioni canale/i con vari MVPD su tutte le piattaforme.

Time-To-Live del token di autorizzazione, noto anche come **TTL AuthZ**, viene visualizzato in valori leggibili dall’utente come: **giorni, ore, minuti, secondi**.

In termini di esperienza utente, i rapporti TTL di AuthZ consentono di controllare visivamente la quantità di tempo in cui un utente verrà autorizzato considerando una specifica MVPD e una piattaforma specifica.

Per passare a questo tipo di rapporto, fai clic sulla scheda &quot;Rapporti TTL AuthZ&quot; nella sezione &quot;Rapporti&quot;.

![Rapporti TTL AuthZ](assets/authz-ttl-reports.png)

*Figura 9. Scheda Rapporto TTL AuthZ della dashboard di Adobe Primetime TVE*

La tabella Rapporti TTL AuthZ contiene pagine ed è scorrevole in orizzontale e verticale a seconda delle dimensioni dello schermo.

Se si considera di apportare una modifica a un valore TTL AuthZ, vedere [Integrazioni](#tve-db-integrations-sec) sezione.

>[!IMPORTANT]
>La &quot;**Impostato da MVPD** Il segnaposto &quot; viene utilizzato nei casi in cui MVPD sarà quello che applica il valore TTL AuthZ e non la configurazione di autenticazione Adobe Pass.


#### Rapporti SSO {#sso-reports}

Questo rapporto visualizza lo stato Single Sign-On (SSO) configurato per le integrazioni canale/i con vari MVPD su tutte le piattaforme.

Lo stato Single Sign-On, indicato anche come **Stato SSO**, viene visualizzato come tristato con i seguenti valori possibili: **SSO disabilitato, SSO abilitato, SSO incerto**.

In termini di esperienza utente, i rapporti SSO consentono di esaminare visivamente l’esperienza SSO di autenticazione utente prevista considerando un MVPD specifico e una piattaforma specifica.

Per passare a questo tipo di rapporto, fai clic sul pulsante &quot;**Rapporti SSO**&quot; dalla scheda &quot;**Rapporti**&quot;.


![Scheda Rapporti SSO della dashboard di TVE](assets/sso-reports.png)


*Figura 10: scheda Rapporti SSO del dashboard di Adobe Primetime TVE*

La tabella Rapporti SSO contiene pagine ed è scorrevole in orizzontale e verticale a seconda delle dimensioni dello schermo.

Nel caso in cui si prenda in considerazione di apportare una modifica allo stato SSO, rivedere il [Integrazioni](#tve-db-integrations-sec) sezione.

>[!IMPORTANT]
>&quot;**SSO incerto** Il segnaposto &quot; viene utilizzato nei casi in cui l’SSO è abilitato e possibile, ma le impostazioni della piattaforma utente/le decisioni dell’utente (ad esempio, l’opzione del browser utente per bloccare i cookie di terze parti, l’utente che sceglie di negare l’accesso alla piattaforma al proprio abbonamento al provider TV) o le impostazioni MVPD (ad esempio, MVPD che richiede l’autenticazione per ogni canale) potrebbero impedire l’esecuzione dell’SSO.

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

![modulo di feedback del dashboard di tve](assets/tve-dashboard-feedback.png)

*Figura 11: sezione Feedback sul dashboard di Adobe Primetime TVE*

Per istruzioni su come acquisire le schermate, consulta i collegamenti seguenti:

* [Acquisire screenshot su Windows](https://support.microsoft.com/en-us/windows/use-snipping-tool-to-capture-screenshots-00246869-1843-655f-f220-97299b865f6b#1TC=windows-7)

* [Come catturare screenshot su Mac](https://support.apple.com/en-us/HT201361)

## Risoluzione dei problemi {#tve-db-troubleshoot}

### Modalità di manutenzione {#maintenance-mode}

![App TVE in modalità manutenzione](assets/tveapp-maintenance-mode.png)


*Figura: App TVE in modalità manutenzione*


Se TVE Dashboard è in &quot;modalità di manutenzione&quot;, gli utenti non potranno visualizzare o apportare nuove modifiche.

In questo caso, dovrai aspettare che il team di progettazione dell’autenticazione di Adobe Pass finisca il lavoro di manutenzione sul dashboard TVE.

### Stato degradato {#degraded-state}

![App TVE in stato degradato](assets/tve-degraded-state.png)


*Figura: app TVE in stato degradato*

Se il dashboard TVE è in &quot;stato degradato&quot;, gli utenti non disporranno delle funzionalità di ricerca e ordinamento, ma potranno visualizzare o apportare nuove modifiche.

In questo caso, dovrai aspettare che il team di progettazione dell’autenticazione di Adobe Pass finisca il lavoro di manutenzione sul dashboard TVE.
