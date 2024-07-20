---
title: Funzioni di integrazione MVPD
description: Funzioni di integrazione MVPD
exl-id: fcd65940-9a86-49b2-9d52-9031fb763338
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1733'
ht-degree: 2%

---

# Funzioni di integrazione MVPD

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#mvpd-int-features-overview}

Adobe Pass Authentication supporta gli standard emergenti per TV Everywhere. È conforme alla specifica **CableLabs OLCA (Online Content Access)**, che fornisce requisiti tecnici e architettura per la distribuzione di video a un cliente Pay TV da origini online. Adobe ha partecipato al progetto congiunto di test di interoperabilità CableLabs nel giugno 2011 e ha superato il processo di test per un&#39;implementazione di Service Provider.  L&#39;Adobe è anche membro attivo del **consorzio tecnico per l&#39;autenticazione aperta** e partecipa a diversi progetti di definizione delle specifiche dei sottocomitati come parte di tale organismo.

Dopo aver descritto la conformità OLCA per l’autenticazione di Adobe Pass e la partecipazione di Adobe a OATC, è importante notare che l’autenticazione di Adobe Pass è in realtà &quot;indipendente dal protocollo&quot;.  Ma in questa fase dell’era TVE, l’autenticazione Adobe Pass è decisamente orientata verso gli standard OLCA.  Gli standard delineano i modi attualmente concordati per i diversi attori TVE (programmatori, MVPD e fornitori di servizi) per implementare le funzionalità TVE. Molte di queste funzioni sono elencate nelle tabelle seguenti, con collegamenti a pagine correlate che forniscono dettagli ed esempi su come implementarle.

Le informazioni contenute nelle tabelle sono destinate a indirizzare il processo di integrazione dell’autenticazione di Adobe Pass verso una funzionalità coerente per tutte le integrazioni MVPD. La colonna di priorità classifica le feature in A, B e C:

* R - Si tratta di funzioni &quot;indispensabili&quot; per l’implementazione iniziale di un’integrazione.
* B - Si tratta di miglioramenti importanti all’integrazione iniziale, da aggiungere dopo il rollout iniziale.
* C - Questi sono ulteriori miglioramenti all’integrazione che possono essere implementati dopo i requisiti &quot;B&quot;.


## 1. Funzioni principali {#core-func-features}


| No. | Funzionalità | Descrizione | Priorità | Note |
|------|----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1,1 | [Accesso avviato dal programmatore](/help/authentication/authn-oauth2-protocol.md) | Il visualizzatore seleziona l’MVPD e avvia il flusso di autenticazione (AuthN) dal sito web o dall’applicazione del brand Programmer. | A+ |                                                                                                                                                         |
| 1,2 | [Autorizzazione basata sul canale](/help/authentication/authz-usecase.md) | Una volta autenticata, l&#39;autorizzazione (AuthZ) può avvenire in background, con il passaggio solo di un identificatore di canale di rete, e di un identificatore utente, all&#39;MVPD. | A+ |                                                                                                                                                         |
| 1,3 | Persistenza UserID | MVPD fornisce un UserID offuscato e persistente. | A |                                                                                                                                                         |
| 1,4 | [Supporto Single Sign-On](/help/authentication/sso-support.md) | Il visualizzatore accede con MVPD sul sito di un marchio e può passare a un altro marchio senza che gli venga richiesto di effettuare nuovamente l’accesso. La sessione AuthN è condivisa tra i brand. Il supporto SSO è sia per i siti web che per le applicazioni mobili/dispositivi.  Per gli MVPD, è necessario restituire un ID utente o un altro token utente che possa essere utilizzato per AuthZ tra i brand. | A |                                                                                                                                                         |
| 1,5 | Esperienza utente di accesso (UX) ottimizzato per il dispositivo | MVPD supporta il ridimensionamento della schermata di accesso in modo da adattarla alle dimensioni del dispositivo utilizzato per la visualizzazione. | A |                                                                                                                                                         |
| 1,6 | Logo selettore MVPD predefinito | MVPD fornisce un URL a un logo predefinito di dimensioni appropriate (112x33 pixel). | A | Il logo deve essere ospitato da MVPD e memorizzato nella cache da CDN. |
| 1,7 | [Ambito provider di servizi](/help/authentication/serv-provider-scoping.md) | MVPD supporta il passaggio dell’identificatore del brand (valore Requestor) nella richiesta AuthN. | A- | In questo modo è possibile eseguire l&#39;accesso specifico per il provider di servizi. |
| 1,8 | [Autorizzazione per più canali](/help/authentication/mvpd-preflight-authz.md#preflight-multich-authz) | MVPD supporta un programmatore che invia più canali in una singola richiesta di autorizzazione. | B+ |                                                                                                                                                         |
| 1,9 | Autenticazione basata su iFrame o &quot;JS pop-up&quot; | Il programmatore è in grado di integrare il flusso di accesso in un’esperienza iFrame o pop-up invece di un reindirizzamento HTTP. | B |                                                                                                                                                         |
| 1,10 | [Disconnessione avviata dal programmatore](/help/authentication/usecase-mvpd-logout.md) | Il visualizzatore ha una sessione autenticata sia sul sito o sull&#39;app del programmatore che con l&#39;MVPD. Il visualizzatore può avviare una disconnessione federata dal sito del programmatore e la disconnessione cancella anche la sessione sul portale MVPD. | B | Garantisce che i computer condivisi siano maggiormente protetti dall&#39;uso improprio dal punto di vista del programmatore. |
| 1,11 | [Messaggi Di Errore Di Autorizzazione Personalizzata](/help/authentication/error-reporting.md) | MVPD trasmette la propria stringa di errore personalizzata, appropriata per la visualizzazione all&#39;utente del sito federato o dell&#39;app del programmatore. | B | Abilita scenari di up-selling |
| 1,12 | **Metadati utente nella risposta di autenticazione** | La risposta Autenticazione MVPD può includere metadati utente che fungono da suggerimento per la personalizzazione dell’esperienza dell’utente durante il flusso di adesione. Questo requisito consente di fornire suggerimenti per il controllo parentale dall&#39;MVPD al programmatore. | B- |                                                                                                                                                         |
| 1,13 | Autenticazione avviata da MVPD | Il visualizzatore completa una sessione AuthN sul portale MVPD, quindi passa al sito TVE del programmatore. All&#39;utente non viene richiesto di specificare il selettore MVPD e viene automaticamente autenticato. | B- |                                                                                                                                                         |
| 1,14 | Ambito UserID | L&#39;ID utente MVPD deve essere presente in due forme: una con ambito Programmatori e l&#39;altra con ambito Adobe per la frode.  Questo consente ad Adobe di condividere l&#39;ID utente MVPD con ambito di programmazione senza ulteriore crittografia/offuscamento. | C |                                                                                                                                                         |
| 1,15 | Autorizzazione basata su risorse | Una volta completata la funzione AuthN, AuthZ può essere eseguito in background, trasmettendo dati strutturati che possono includere rete, spettacolo, risorsa, valutazione del controllo genitori e altro ancora in base alle esigenze. Questo consente il controllo genitori su ogni chiamata AuthZ dal programmatore al MVPD. | C |                                                                                                                                                         |
| 1,16 | Disconnessione avviata da MVPD | Il visualizzatore ha una sessione autenticata sia sul sito o sull&#39;app del programmatore che con l&#39;MVPD. Il visualizzatore può avviare una disconnessione federata dal sito MVPD che cancella anche la sessione su tutti i siti Programmatori federati. | C |                                                                                                                                                         |
| 1,17 | Obblighi di autorizzazione | L&#39;MVPD fornisce condizioni aggiuntive nella risposta AuthZ, ad esempio la registrazione, o una classificazione OLCA (Maximum Parental Control) aggiornata. | C |                                                                                                                                                         |
| 1,18 | Contesto indirizzo IP | MVPD richiede di trasmettere esplicitamente l’indirizzo IP. Per AuthZ, dove le chiamate sono lato server, questo fornisce al MVPD informazioni di tracciamento delle frodi su dove l’utente proviene dalla chiamata AuthZ. | C |                                                                                                                                                         |
| 1,19 | Valore TTL (Time-to-live) della persistenza dei token definito in modo dinamico | MVPD è in grado di impostare dinamicamente il TTL del token di autenticazione di Adobe Pass tramite le proprietà nella risposta, in modo che l’Adobe sia escluso dal ciclo per le modifiche TTL. | C- |                                                                                                                                                         |
| 1,20 | Tipo di dispositivo | MVPD supporta il passaggio del tipo di dispositivo nella richiesta AuthN o AuthZ. Questa proprietà informa l’MVPD sulla natura del dispositivo in cui verrà utilizzato il contenuto, in modo che possa regolare il TTL del token AuthN o AuthZ in base alle proprie considerazioni di sicurezza per il dispositivo. | C- | Questa funzione è utile per la piattaforma senza client, in cui potrebbe essere difficile esporre ogni tipo di dispositivo come impostazione di configurazione sul lato Autenticazione di Adobe Pass. |



## 2. Caratteristiche operative {#operational-features}

| No | Funzionalità | Descrizione | Priorità | Note |
|-----|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------|
| 2,1 | Tempo di attività 24 ore su 24, 7 giorni su 7 | Un accordo MVPD per lottare per un tempo di attività 24 ore su 24, 7 giorni su 7 e misurarlo nel tempo. | A |       |
| 2,2 | Credenziali Di Prova Per Ogni Programmatore | MVPD fornisce conti di test separati per ciascun programmatore. | A |       |
| 2,3 | Piano di scadenza e rinnovo del certificato | Un piano documentato MVPD con una pianificazione per garantire che i certificati SAML siano aggiornati. | A- |       |
| 2,4 | Latenza Media Inferiore A 250 Ms / Risposta | Un accordo MVPD per lottare per una bassa latenza e per misurarla nel tempo. | A- |       |
| 2,5 | Hosting del proprio logo di selezione MVPD | L’MVPD deve ospitare il proprio logo e deve essere protetto tramite caching CDN. | B |       |
| 2,6 | Piano di riassegnazione del supporto | Un piano documentato MVPD per l&#39;escalation, aggiornato e rivisto almeno trimestralmente. | A |       |
| 2,7 | Piano di protezione dalle interruzioni | Un co-piano MVPD con Adobi sulle misure di degradazione che possono essere utilizzate in generale in base alle necessità. | B |       |
| 2,8 | Gestisci endpoint di staging e produzione separati | Un test di integrazione MVPD che non richiede lo spoofing dei file host per testare l’integrazione di staging. | B |       |
| 2,9 | Endpoint di controllo qualità aggiuntivo | MVPD mantiene un&#39;integrazione QA IdP aggiuntiva per lo sviluppo congiunto di nuove funzioni.  Il supporto di questa opzione renderebbe meno probabile la necessità di richieste speciali UAT per il test dei certificati dell’app store. | C |       |





## 3. Funzioni dell’esperienza di autenticazione {#authn-exp-features}


| No | Funzionalità | Descrizione | Priorità | Note |
|-----|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 3,1 | Conversione dell’autenticazione rispetto all’aspettativa minima | L&#39;MVPD garantisce un tasso di conversione minimo per dimostrarsi funzionale (5%) e ragionevole (30%). | A |                                                                                                                                                           |
| 3,2 | Recupero password in linea | MVPD fornisce un mezzo per recuperare le password in linea con il flusso AuthN federato. | A |                                                                                                                                                           |
| 3,3 | Registrazione account in linea | MVPD consente di creare un nuovo account in linea con il flusso AuthN federato. | A |                                                                                                                                                           |
| 3,4 | Guida e supporto in linea | MVPD fornisce un mezzo per fornire assistenza durante il flusso AuthN federato. | A |                                                                                                                                                           |
| 3,5 | Autenticazione interna basata su modem | MVPD autentica automaticamente un dispositivo quando si trova sulla rete locale di un modello registrato (solo ISP MVPD). | B | Questa è una priorità più bassa perché è un&#39;ottimizzazione che molti non possono ancora supportare e introduce alcune sfide per la mitigazione delle frodi e il controllo dei genitori |
Ora puoi importare il codice della tabella Markdown direttamente utilizzando la finestra di dialogo File/Incolla dati tabella...



## 4. Funzioni di Analytics {#analytics-features}


| No | Funzionalità | Descrizione | Priorità |
|-----|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 4,1 | Allineamento funnel di conversione autenticazione | MVPD ha una metrica per le richieste AuthN che inizia con la richiesta AuthN SAML, per allinearsi alle metriche di richiesta AuthN di Adobe Pass Authentication e Programmer. | A |
| 4,2 | Utenti univoci | Utenti che sono stati autenticati correttamente e deduplicati in un rollup mensile tra giorni. | A |
| 4,3 | Contabilità fuso orario | I rapporti includono il fuso orario per il passaggio del giorno. | A |





## 5. Caratteristiche di mitigazione delle frodi {#fraud-mitgn-features}


| No | Funzionalità | Descrizione | Priorità | Note |
|-----|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------|
| 5,1 | Convalida del sottoscrittore video all’autenticazione | MVPD garantisce che l’utente sia un abbonato video valido durante il flusso AuthN. | A |                                                                                                |
| 5,2 | Limitazione di velocità per autenticazione/autorizzazione | MVPD supporta la limitazione sui servizi web per limitare l’utilizzo da un account utente specifico quando appropriato. | B |                                                                                                |
| 5,3 | ID utente persistente con ambito globale da Adobe | MVPD garantisce che Adobe disponga di un UserID che può essere tracciato tra i programmatori per individuare eventuali frodi. Questo non deve essere fornito direttamente al cliente. | B | Specifiche del protocollo OMAP (Online Multimedia Authorization Protocol) e di Real User Monitoring (RUM). |
| 5,4 | Convalida utilizzo simultaneo | MVPD dispone di un mezzo per monitorare e limitare l’utilizzo simultaneo dell’account dell’abbonato oltre una soglia aziendale. | B |                                                                                                |

## P1. Funzioni specifiche dei proxy {#proxy-sp-func-features}

| No | Descrizione | Priorità | Note |
|-------|--------------------------------------------------------------------------------|----------|---------------------------------------------------|
| P 1.1 | Proxy responsabile della precisione dell&#39;elenco aggiornato di subMVPDs | A |                                                   |
| P 1.2 | Il proxy fornisce un logo di dimensioni appropriate per ogni subMVPD | A | Alcuni subMVPD non hanno la dimensione del logo corretta |
| P 1,3 | Il proxy fornisce una pagina di accesso con il marchio appropriato per ogni subMVPD | A |                                                   |
| P 1,4 | Proxy responsabile del targeting di specifici marchi di fornitori di servizi con elenco subMVPD | B |                                                   |
| P 1,5 | Proxy specifica correttamente tutte le proprietà subMVPD (dimensioni iFrame, TTL, logo, ecc.) | A |                                                   |
| P 1,6 | Proxy specifica un entityID separato | B |                                                   |

## P2. Funzioni operative dei proxy {#proxy-op-features}

| No | Descrizione | Priorità |
|-------|----------------------------------------------------------------------------------------|----------|
| P 2.1 | La chiave API è protetta | A |
| P 2.2 | Credenziali di test per il primo subMVPD utilizzato nell’integrazione live per ogni nuovo richiedente | A |
| P 2.3 | gli elenchi subMVPD sono accurati e completi per richiedente | A |
