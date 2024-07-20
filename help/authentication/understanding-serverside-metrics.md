---
title: Informazioni sulle metriche lato server
description: Informazioni sulle metriche lato server
exl-id: 516884e9-6b0b-451a-b84a-6514f571aa44
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2232'
ht-degree: 0%

---

# Informazioni sulle metriche lato server {#understanding-server-side-metrics}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.


## Introduzione {#intro}

Questo documento descrive le metriche lato server di autenticazione di Adobe Pass generate dal servizio ESM (Entitlement Service Monitoring). Non descrive gli stessi eventi visti dal punto di vista del lato client (ciò che i programmatori vedrebbero se implementassero un servizio di misurazione come Adobe Analytics sulla loro pagina/applicazione).

## Riepilogo eventi {#events_summary}

Dal punto di vista del lato server di autenticazione di Adobe Pass vengono generati i seguenti eventi:

* **Eventi generati nel flusso di autenticazione**(un accesso effettivo con MVPD)

   * Notifica del tentativo di autenticazione: viene generato quando l’utente viene inviato al sito di accesso MVPD.
   * Notifica di autenticazione in sospeso: se l’utente riesce ad accedere con il proprio MVPD, questo viene generato quando l’utente è        è stato reindirizzato all’autenticazione Adobe Pass.
   * Notifica di autenticazione concessa: viene generata quando l’utente torna sul sito del programmatore e ha recuperato correttamente il token di autenticazione dall’autenticazione di Adobe Pass.
* **Flusso di autorizzazione** (solo un controllo per l&#39;autorizzazione con un
MVPD)\
  *Prerequisito:* un token AuthN valido
   * Notifica del tentativo di autenticazione
   * Notifica di concessione AuthZ
* **Riproduzione della richiesta completata**\
  *Prerequisito:* token AuthN e AuthZ validi
   * Notifica di un controllo con autenticazione Adobe Pass
   * Una richiesta Play richiede sia un’autenticazione concessa sia un’autorizzazione concessa


Il numero di utenti univoci è descritto in dettaglio nella sezione [Utenti univoci](#unique-users) seguente. In generale, poiché le risposte di autenticazione e autorizzazione concesse sono solitamente memorizzate nella cache, si applicano le seguenti formule:

* Numero di tentativi di autenticazione \> Numero di tentativi di autenticazione concessi
* Numero di tentativi AuthZ \> Numero di tentativi AuthZ concessi
* Numero di tentativi AuthZ \> Numero di tentativi AuthN concessi (in genere)
* Numero di richieste di riproduzione riuscite \> Numero di AuthZ concesse


### Esempio {#example}

L’esempio seguente mostra le metriche lato server per un mese per
un marchio:

| Metrica | MVPD 1 | MVPD 2 | ... | MVPD n | Totale |
| -------------------------- | ------ | ------ | - | ------ | ---------------------------------------------- |
| Autenticazioni riuscite | 1125 | 2892 |   | 2203 | SUM(MVP1+...MVPD n) |
| Autorizzazioni riuscite | 2527 | 5603 |   | 5904 | SUM(MVP1+...MVPD n) |
| Richieste riproduzione riuscite | 4201 | 10518 |   | 10737 | SUM(MVP1+...MVPD n) |
| Utenti univoci | 1375 | 2400 |   | 2890 | SOMMA di tutti gli utenti per tutti gli MVPD deduplicati\* |
| Tentativi di autenticazione | 2147 | 3887 |   | 3108 | SUM(MVP1+...MVPD n) |
| Tentativi di autorizzazione | 2889 | 6139 |   | 6039 | SUM(MVP1+...MVPD n) |

</br>

In questo caso, la deduplicazione non dovrebbe avere alcun effetto, in quanto utenti MVPD diversi non dovrebbero ricevere lo stesso ID utente. Quando si fa una somma per due marchi diversi ma lo stesso MVPD, l’effetto di deduplicazione dovrebbe essere molto maggiore.

## Attivatori evento {#event_triggers}

### Nuovo utente - Flusso completo {#new-user-full-flow}

Il grafico seguente descrive gli eventi e i passaggi per un utente senza token di autenticazione (un nuovo utente o un nuovo utente il cui token di autenticazione è scaduto):



![](assets/ae-flow-with-events.png)



Il flusso prevede round trip a MVPD sia per l&#39;autenticazione (#5 a \#7) che per l&#39;autorizzazione (\#11).



Al termine del flusso, i token di autenticazione e autorizzazione vengono memorizzati nella cache del dispositivo dell’utente. I valori TTL (Time-to-Live) dei token di autenticazione sono compresi tra 6 ore e 90 giorni. La scadenza di un token AuthN forza automaticamente la scadenza di un token AuthZ. Il valore TTL per il token di autorizzazione è in genere di 24 ore.

| Eventi lato server attivati | <ul><li>Tentativo di autenticazione, autenticazione in sospeso, autenticazione concessa</li><li>Tentativo di autorizzazione, autorizzazione concessa</li><li>Richiesta riproduzione riuscita</li></ul> |
|---|---|


### Utente di ritorno - Token AuthZ e AuthN memorizzati nella cache

Per gli utenti che dispongono di token AuthZ e AuthN validi memorizzati nella cache, quanto segue
si verificano dei passaggi:


![](assets/ae-flow-tokens-cached-web.png)



Viene attivato automaticamente quando si chiama `getAuthorization()` e prevede solo controlli con l&#39;autenticazione Adobe Pass. Il MVPD non è coinvolto in questo flusso.


| Eventi lato server attivati | * Richiesta di riproduzione riuscita |
|---|---|


### Utente di ritorno: token AuthN memorizzati nella cache, token AuthZ scaduto

Per gli utenti che dispongono ancora di token di autenticazione validi, si verificano i seguenti passaggi:

![](assets/ae-flow-authn-token-cached.png)


Questo flusso comporta un viaggio di andata e ritorno verso l&#39;MVPD.


| Eventi lato server attivati | <ul><li>Tentativo di autorizzazione, autorizzazione OK</li><li>Richiesta riproduzione riuscita</li> |
|---|---|

## Eventi di autenticazione {#authn_events}

### Tentativo di autenticazione {#authentication-attempt}

Come illustrato nel diagramma precedente, gli eventi di autenticazione vengono attivati solo quando l’utente effettua un round trip a MVPD; gli eventi di autenticazione non includono le autenticazioni dei token memorizzati nella cache.

L’evento di tentativo di autenticazione viene attivato dopo che l’utente ha fatto clic su un MVPD specifico dal selettore.

* Il primo evento sul lato MVPD che si avvicina a questo è il caricamento della pagina
* L’autenticazione di Adobe Pass non conta i tentativi ripetuti dall’utente di accedere alla pagina MVPD (password errata, riprova)
* più tentativi conteggiati come un tentativo
* Alcuni MVPD eseguono anche l’autorizzazione nel passaggio Autenticazione e l’utente non viene reindirizzato se l’autorizzazione non riesce.

### Autenticazione in sospeso {#authentication-pending}

Questo evento si verifica all’avvio del processo di reindirizzamento all’autenticazione di Adobe Pass.

### Autenticazione concessa {#authentication-granted}

L&#39;utente è un abbonato noto di MVPD, in genere con un abbonamento a Pay TV, ma a volte con solo accesso a Internet. Un’autenticazione corretta può verificarsi perché l’utente ha immesso esplicitamente credenziali valide con il proprio MVPD o perché aveva precedentemente inserito credenziali valide e aveva selezionato &quot;ricordami&quot; (e la sessione precedente non era scaduta).

MVPD invia quindi all&#39;autenticazione Adobe Pass una risposta positiva alla richiesta di autenticazione e l&#39;autenticazione Adobe Pass crea un token *AuthN*.

* L’autenticazione viene generalmente memorizzata nella cache per un lungo periodo di tempo (un mese o più). Per questo motivo, gli eventi di autenticazione non saranno più presenti fino alla scadenza del token e al riavvio del flusso.
* L’accesso da un altro sito o app tramite Single Sign-On non attiva eventi di autenticazione.



### Autenticazione Comcast {#comcast-authentication}

Comcast ha un flusso AuthN diverso rispetto agli altri MVPD.

Le seguenti funzioni descrivono le differenze:

* **Comportamento cookie di sessione**: questo comporta la rimozione completa di tutti i token di autenticazione dopo che l&#39;utente ha chiuso il browser. Questa funzione è disponibile solo sul Web. Lo scopo principale è garantire che la sessione Comcast non venga mantenuta in computer non sicuri o condivisi. L’impatto è che ci saranno più tentativi di autenticazione / flussi concessi rispetto al resto degli MVPD.

* **AuthN per requestorID**: Comcast non consente la memorizzazione nella cache dello stato AuthN da un ID richiedente a un altro. Per questo motivo, ogni sito/app deve passare a Comcast per ottenere un token di autenticazione. Oltre alle considerazioni sull’esperienza utente, l’impatto, come sopra, è che verranno generati più tentativi di autenticazione/eventi concessi.

* **Autenticazione passiva**: per migliorare l&#39;esperienza utente, ma
mantenendo comunque la funzionalità AuthN per requestorID, si verifica un flusso di autenticazione passivo in un iFrame nascosto. L’utente non vedrà nulla, ma gli eventi verranno comunque attivati come prima.

Se l’utente fa clic su &quot;ricorda me&quot; nella pagina di accesso di Comcast, le visite successive a questa pagina (in un periodo di 2 settimane) saranno solo un rapido reindirizzamento verso la pagina precedente. In caso contrario, gli utenti dovranno eseguire l’autenticazione sulla pagina.

### Autenticazione non riuscita {#unsuccessful-authentication}

Un’autenticazione senza successo non è un evento di per sé nell’autenticazione di Adobe Pass, ma può essere calcolata come differenza tra tentativi e successi.

Nella versione di maggio 2013, Adobe Pass Authentication aggiungerà codici di errore per le autenticazioni non riuscite dovute a errori di sistema o di rete, inclusi errori DRM (associazione token non riuscita) ed errori LSO (nessuno spazio per scrivere il token, ecc.).

### Tasso di conversione autenticazione {#authenitication-conversion-rate}

Una metrica interessante che i programmatori possono tracciare è il tasso di conversione dell’autenticazione, calcolato come (richieste AuthN / AuthN concesse)%.

Alcune note sulle metriche:

* Poiché si tratta di una metrica basata su eventi, non riflette realmente il tasso di conversione utente univoco, se un utente tenta otto volte e riesce a eseguire la nona operazione, questo si rifletterà molto male nel tasso di conversione indicato sopra.
* Nell’autenticazione di Adobe Pass (sul lato server) non è ancora possibile calcolare una conversione di autenticazione basata su elementi univoci.
* Se nel sito/app sono presenti nuovi tentativi di autenticazione automatica, anche la metrica precedente risulterà distorta.

## Eventi di autorizzazione {#authorization_events}

### Tentativo di autorizzazione {#authorization_attempt}

Oltre a ottenere un token di autenticazione, gli utenti devono ottenere anche un token di autorizzazione prima di riprodurre il contenuto. Ciò si verifica in genere dopo l’autenticazione o se la scadenza del token di autorizzazione. Poiché questo controllo viene eseguito lato server (dai server di autenticazione di Adobe Pass ai server MVPD), l’utente non è tenuto a eseguire alcuna operazione.

### Autorizzazione concessa {#authorization-granted}

Una &quot;autorizzazione concessa&quot; segnala che l’abbonamento dell’utente autenticato include la programmazione richiesta.

Non tutti gli MVPD supportano un passaggio di autorizzazione separato; per alcune autenticazioni, l&#39;autorizzazione è equiparata a. MVPD invia all’autenticazione di Adobe Pass una risposta corretta alla richiesta AuthZ del backchannel e l’autenticazione di Adobe Pass crea un token AuthZ.

* Il token AuthZ viene memorizzato nella cache per un periodo di tempo, in genere 24 ore Durante questo periodo non verrà attivato alcun evento AuthZ.
* Alcuni MVPD funzionano con le autorizzazioni a livello di asset, altri con le autorizzazioni a livello di canale; - a seconda di quale viene utilizzato, vengono attivati più o meno eventi AuthZ. Anche per l’autorizzazione a livello di canale, è attiva la memorizzazione in cache, quindi se la stessa risorsa viene richiesta in meno di 24 ore, non verrà attivato alcun evento.

### Autorizzazione negata {#authorization-denied}

Se un&#39;autorizzazione viene negata, l&#39;utente autenticato non dispone di una sottoscrizione confermata alla programmazione richiesta. La causa più probabile è che il canale non fa parte del pacchetto di abbonamento dell&#39;utente, ma questo può anche riflettere un utente che ha solo accesso a Internet dal MVPD.

Per alcuni MVPD, gli utenti vengono autenticati correttamente anche se dispongono solo di un abbonamento a Internet da MVPD (nessun abbonamento a pay TV). In questo caso, anche se il canale per il quale l’utente richiede l’autorizzazione si trova nel pacchetto di base, l’autorizzazione verrà negata.

Alcuni MVPD offrono messaggi di errore personalizzati per i rifiuti AuthZ che possono includere offerte per aggiornare il proprio pacchetto.


### Tasso di conversione autorizzazione {#authorization-conversion-rate}

Il tasso di conversione dell’autenticazione può essere calcolato come (richieste AuthZ / AuthZ concesse)%.

### Richiesta di riproduzione riuscita {#successful-play-request}

Gli utenti autenticati e autorizzati possono visualizzare contenuti protetti.

In caso di esito positivo di una richiesta di riproduzione, l’autenticazione di Adobe Pass genera un token multimediale di breve durata in cui si asserisce che l’utente ha il diritto di visualizzare il video richiesto. Il programmatore utilizza questo Media Token per un’ulteriore convalida del visualizzatore potenziale. I token multimediali vengono tracciati come richieste di riproduzione riuscite.

* L&#39;autenticazione di Adobe Pass *not* tiene traccia dell&#39;effettivo avvio della riproduzione video dopo la generazione del token multimediale. Ad esempio, in caso di una restrizione geografica sul contenuto, la transazione continua a essere considerata una richiesta di riproduzione corretta, anche se il flusso non viene mai avviato.
* Poiché i token AuthN e AuthZ memorizzano nella cache la risposta MVPD per un periodo di tempo, l’evento di richiesta di riproduzione di successo è l’evento più frequente nelle metriche.

## Utenti univoci {#unique-users}

### Definizione {#definition}

Dopo aver eseguito correttamente l’autenticazione, Adobe Pass Authentication tiene traccia dell’esistenza di un utente univoco, in base al valore di ID utente MVPD restituito.  Questo valore si basa sulle informazioni di accesso dell’utente, ma non contiene informazioni che consentono l’identificazione personale.

Questo valore viene passato anche al sito/app nel callback sendTrackingData.

Questo valore può essere persistente tra i dispositivi (MVPD produce lo stesso valore per un dato utente, indipendentemente da dove si verifica l’accesso) o transitorio (per ogni accesso, viene generato un nuovo valore, che MVPD mappa nel suo back-end. In genere i valori forniti da MVPD all’autenticazione di Adobe Pass sono persistenti tra sessioni e dispositivi, ma, come indicato, la persistenza non è né garantita né convalidata.

Questo valore viene utilizzato come metodo per calcolare gli utenti univoci. Il valore segnalato (per ID richiedente/intervallo/MVPD) viene deduplicato per l’intervallo specifico. Pertanto, la somma degli utenti univoci al giorno è solitamente diversa dal valore mensile, con il valore mensile che ha il valore inferiore.

Questo numero include tutti gli eventi da Autenticazione Adobe Pass, meno i tentativi di autenticazione (che non hanno un ID utente), ma include le autorizzazioni tentate (e potenzialmente non riuscite).

### Esempi {#examples}

#### Giorno 1 {#day1}

L&#39;utente XYZ accede al sito per guardare un video.

Eventi attivati:

* Tentativo AuthN (ancora nessun utente univoco)
* AuthN concessa
   * a questo punto, identifichiamo l’utente in modo univoco in base a ciò che restituisce MVPD, pertanto il conteggio univoco giornaliero degli utenti viene aumentato di 1
   * il token AuthN viene memorizzato nella cache per 30 giorni
* Tentativo AuthZ / evento concesso
   * Token AuthZ memorizzato nella cache per 1 giorno
* Evento di richiesta di riproduzione riuscito

#### Giorno 1 (più tardi) {#day1-later-on}

L&#39;utente XYZ guarda un altro video.

Eventi attivati:

* Evento di richiesta di riproduzione riuscito (gli altri sono memorizzati in cache)
* Nessun aumento degli univoci giornalieri o mensili

#### Giorno 3 {#day3}

L&#39;utente XYZ guarda un altro video.

Eventi attivati:

* Tentativo AuthZ / evento concesso
   * Dal giorno 1 in cui è scaduto il caching
* Evento di richiesta di riproduzione riuscito (gli altri sono memorizzati in cache)
* Utenti univoci giornalieri aumentati di 1 - gli univoci mensili sono ancora 1

#### Giorno 31 {#day31}

L&#39;utente XYZ guarda un altro video.

Come nel giorno 1, dalla scadenza del caching AuthN.

In caso di mancata autorizzazione dello stesso utente, il conteggio mensile degli utenti univoci viene comunque aumentato di 1 in quanto esistono due eventi che contengono l’ID utente: l’autenticazione concessa e il tentativo di autorizzazione.

### Single Sign-On (SSO) {#single-sign-on-sso}

In alcuni casi, il numero di utenti univoci può essere maggiore del numero di autenticazioni riuscite. Questo accade in genere quando molti utenti arrivano tramite SSO da altri siti/app e devono ottenere l’autorizzazione solo sul sito/app corrente.

### Confronto tra utenti univoci lato client e lato server {#comparing-client-side-and-server-side-unique-users}

Se il valore ID utente da `sendTrackingData()` viene utilizzato sul lato client per contare gli utenti univoci, i numeri lato client e lato server devono corrispondere.

Se le differenze sono importanti, i seguenti motivi sono in genere
differenza:

* Univoci riproduzione video e univoci di tutti gli eventi. Come accennato, l’autenticazione Adobe Pass conta gli utenti univoci per tutti gli eventi ad eccezione dei tentativi AuthN. Ciò significa che se l’utente si autentica solo (sulla pagina) ma non visualizza un video, viene comunque attivato un aumento del numero di utenti univoci.

* Conteggio degli utenti che non hanno ottenuto l’autorizzazione: l’autenticazione Adobe Pass conta questi utenti anche nel numero riportato.

<!--
## Related Information {#related-information}

- [Entitlement Service Monitoring API](/help/authentication/entitlement-service-monitoring-api.md)

-->
