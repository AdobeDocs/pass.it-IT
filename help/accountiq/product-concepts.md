---
title: Glossario di Account IQ
description: Un glossario di terminologie di prodotto.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# Concetti e glossario dei prodotti {#glossary}

## Terminologie comuni per D2C e TV Everywhere

Le seguenti terminologie di prodotto e le relative definizioni sono comuni a tutte le [versioni di Account IQ](versions-aiq.md).

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

Pannello della dashboard con grafici che divide i punteggi di condivisione del segmento corrente in categorie di condivisione Molto basso, Basso, Moderato, Alto e Molto alto.

### [!UICONTROL Action] {#action-def}

Un evento diretto o indiretto associato a un&#39;operazione [Operation](#operation-def) che influisce sulle caratteristiche (ad esempio, la condivisione di un punteggio o del numero di dispositivi in uso) di un segmento di operazione correlato.

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

Un pannello di dashboard con grafici che dividono i punteggi di condivisione del segmento corrente in categorie di condivisione molto basse, basse, moderate, alte e molto alte, insieme a ciascuna categoria percentuale della quantità totale di streaming per il segmento.

### [!UICONTROL AuthN] {#authn-def}

Numero di tentativi di autenticazione. Un tentativo di autenticazione è il processo con cui un utente tenta di accedere con il servizio D2C o MVPD. Per gli utenti di TV Everywhere, l&#39;utente viene reindirizzato al MVPD prescelto, dove si identifica nel MVPD - in genere con un nome utente e una password.

### [!UICONTROL AuthN OK] {#authn-ok-def}

Numero di autenticazioni riuscite. Un&#39;autenticazione corretta si verifica quando l&#39;identificazione di un utente viene confermata da un servizio D2C o MVPD. Per gli utenti di TV Everywhere, questo comporta il reindirizzamento dell’utente all’app o al sito del programmatore.

### [!UICONTROL Cluster] {#cluster-def}

Un cluster è una raccolta di posizioni e dispositivi. I cluster vengono creati individuando posizioni comuni tra i dispositivi. I dispositivi visualizzati in una posizione comune verranno considerati appartenenti allo stesso cluster. Due dispositivi possono trovarsi nello stesso cluster anche se non hanno posizioni comuni, ma possono essere collegati tramite le posizioni di altri dispositivi.

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

Un cluster privo di dispositivi statici.

#### [!UICONTROL Static cluster] {#static-cluster-def}

Cluster con almeno un dispositivo statico.

### [!UICONTROL Concurrency] {#consurrency-def}

La simultanea è definita da due (o più) flussi riprodotti contemporaneamente o molto vicini nel tempo in modo che l&#39;intervallo tra di essi non possa essere giustificato viaggiando a una velocità normale.
L’utilizzo simultaneo viene calcolato utilizzando la velocità massima (miglia/ora) tra 2 cluster diversi. Si considera che un utente sia simultaneamente utilizzato se ha una velocità superiore a 124 m/h su una distanza inferiore a 124 miglia o se ha una velocità superiore a 400 m/h su una distanza superiore a 124 miglia. La distanza viene calcolata tra le posizioni di cluster diversi. Utilizzo simultaneo consentito nello stesso cluster.

### [!UICONTROL Device] {#device-def}

Prodotto hardware video digitale in grado di riprodurre contenuti in streaming. Ad esempio, smartphone, notebook e computer desktop, console giochi e televisori intelligenti.

### [!UICONTROL Evaluation period] {#evaluation-period-def}

Il periodo di valutazione è il tempo che intercorre tra l&#39;inizio dell&#39;azione associata all&#39;operazione e la fine dell&#39;azione o della sua misurazione.

### [!UICONTROL Geographical Span] {#geographical-span-def}

La distanza tra i punti più lontani in un insieme di posizioni.

### [!UICONTROL Granularity] {#granularity-def}

In riferimento all&#39;intervallo di tempo, la dimensione del periodo, ad esempio una settimana o un mese.

### [!UICONTROL IP] {#ip-def}

Indirizzo IP assegnato a un dispositivo da un provider di servizi Internet. Ad esempio, provider di servizi via cavo e provider di servizi di celle.

### [!UICONTROL Location] {#location-def}

Un punto unico sulla terra. È anche indicata come geolocalizzazione per una specifica richiesta di gioco con una precisione di 1000m x 1000m (un km quadrato).

### [!UICONTROL Media Company] {#media-company-def}

Media Company è un&#39;azienda che possiede un gruppo di reti di media.

### [!UICONTROL Metric] {#metric}

La metrica è un attributo dell’account dell’abbonato (ad esempio, il suo MVPD, i programmatori e i canali dei contenuti che streaming e il numero di dispositivi che utilizzano).

### [!UICONTROL Mobile device] {#mobile-device-def}

Un dispositivo ad alta mobilità. Ad esempio, telefono cellulare e tablet.

### Operazione {#operation-def}

L&#39;operazione è un record creato per tenere traccia dell&#39;effetto di una particolare [azione](#action-def) su un segmento associato. Un esempio di azione può essere un limite posto al numero di flussi simultanei consentiti per i conti identificati dal segmento.

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

Valore che aiuta gli utenti a comprendere l’entità della condivisione di password e fornisce loro un senso di urgenza per agire di conseguenza.

### [!UICONTROL Play Request] {#play-requests-def}

Equivalente all&#39;inizio di un flusso. Questo evento segna l’inizio di un contenuto in streaming per l’utente.

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

Anche noto come Utilizzo da account condivisi, è un valore calcolato in base al numero di richieste di riproduzione effettuate da ciascun account ponderato per la probabilità di condivisione di ciascun account. È anche noto come Utilizzo da parte dell’indice di rischio degli account condivisi.

### [!UICONTROL Segment] {#segmet-def}

Il segmento è un set di account che soddisfano le condizioni definite dall’utente specificate dalle metriche selezionate. Ad esempio, &quot;utenti nelle aree A, B, C, D o E che hanno più di tre dispositivi&quot;.

### [!UICONTROL Sharing level] {#sharing-level-def}

Noto anche come Indice di rischio - Conti o Indice di rischio conti condivisi, è un valore calcolato sulla base di una media della probabilità di condivisione calcolata per ogni conto nel segmento corrente che è stato inviato in streaming almeno una volta durante l’intervallo di tempo selezionato.

### [!UICONTROL Static device] {#static-device-def}

Un dispositivo a mobilità molto ridotta. Ad esempio, console di gioco, set top box e televisore.

### [!UICONTROL Time interval] {#time-interval-def}

Anche nota come periodo, è la finestra di tempo che contiene l’attività di richiesta di riproduzione rappresentata nell’interfaccia utente e nelle tabelle dall’inizio alla fine.

### [!UICONTROL Trend] {#trend-def}

Differenza percentuale nella metrica associata (ad esempio, percentuale delle richieste di riproduzione totali) tra il periodo corrente e quello precedente.

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

Il numero di account univoci inviati in streaming almeno una volta durante un determinato periodo.

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

Più di 37 richieste di riproduzione al mese.

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

Meno di 9 richieste di riproduzione al mese.

#### [!UICONTROL Regular user] {#regular-user-def}

Da 9 a 37 richieste di riproduzione al mese.

### [!UICONTROL Usage Pattern] {#usage-patern-def}

Uno di un set finito di etichette di categoria applicate a un account che caratterizza meglio gli utenti dell’account in termini di gruppi sociali o comportamenti (ad esempio, una piccola famiglia, un viaggiatore o un pendolare, condivisione social, ecc.).

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

Una categoria di video è un’etichetta specifica che qualifica la natura del video. Ad esempio, area dell’origine, tipi di contenuto, come VOD o live, evento o etichette specifiche come team.

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

Una categoria video è definita dalla combinazione di Programmatore, canale e MVPD associata al flusso.

### [!UICONTROL Zip Code] {#zip-code-def}

Il codice postale degli Stati Uniti associato alle ubicazioni all&#39;interno degli Stati Uniti
<!--calculated metrics-->


## Terminologie specifiche per TV Everywhere

### [!UICONTROL AuthZ] {#authz-def}

Autorizzazione o il numero di richieste di autorizzazione. Una richiesta di autorizzazione è la procedura con cui un programmatore richiede l&#39;autorizzazione da un MVPD attraverso un Adobe per iniziare a trasmettere in streaming il contenuto richiesto da un utente. In genere MVPD concede la richiesta in base ai diritti sul contenuto associati all’abbonamento MVPD dell’utente (ad esempio, se il canale associato al contenuto si trova nell’abbonamento dell’utente). Alcune risposte alle richieste di autorizzazione sono memorizzate nella cache da Adobe, che consente ad Adobe di rispondere immediatamente senza passare la richiesta all’MVPD.

### [!UICONTROL AuthZ OK] {#authz-ok-def}

Numero di autorizzazioni completate.

### [!UICONTROL Channel] {#channel-def}

Il canale, noto anche come Proprietà, è una fonte tematicamente correlata di contenuti video. Tradizionalmente rappresenta un feed video continuo distinto e indirizzabile numericamente da un MVPD. Il canale è direttamente mappato su un canale accessibile di contenuti disponibili agli abbonati tramite il loro set top box (STB).

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

Valore calcolato per ogni indice di rischio (Account, Utilizzo, Complessivo) in tutti i Programmatori e MVPD durante l&#39;intervallo di tempo selezionato.

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

Un tipo di analisi di condivisione in cui la valutazione di un account è limitata agli eventi che si sono verificati direttamente sui programmatori nel segmento selezionato.  Normalmente, vengono valutati tutti gli eventi dell’account, il che fornisce una stima molto più accurata della condivisione.  Alcuni dati MVPD sono strutturati in modo da consentire solo l&#39;analisi in modalità isolamento.

### [!UICONTROL MVPD] {#mvpd-def}

MVPD, noto anche come Distributore, è un aggregatore, rivenditore e distributore di contenuti video di Media Company.

### [!UICONTROL Programmer] {#programmer-def}

Programmer, noto anche come Network, è una società controllata da una società più grande (corporation) che possiede e gestisce uno o più canali.

### [!UICONTROL requestorID] {#requestorid-def}

L’ID utilizzato da una società di media per identificarsi o essere una controllata di un MVPD.  A seconda dell’implementazione del programmatore, questa potrebbe essere mappata a una Media Company, un programmatore o un canale. In genere, questo viene mappato su un canale.  Con la creazione di pseudo-canali come MML (March Madness Live) e le mosse tecnicamente guidate per risolvere le limitazioni dei dati guidati da MVPD, requestorID sta iniziando a diventare più associato alla Media Company.

### [!UICONTROL resourceID] {#resource-id-def}

Il contenuto richiesto dall’utente finale. In genere, questo identificava il canale associato al contenuto richiesto dall’utente.  I miglioramenti di sistema consentono a tale ID di rappresentare programmi specifici (ad esempio con valutazioni specifiche), l’ID continua a identificare il canale associato.


