---
title: Valutazione della prevenzione del tracciamento Google Chrome
description: Valutazione della prevenzione del tracciamento Google Chrome
source-git-commit: 579ce868b6ee94e1854bbc51145fc7840268db26
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 0%

---

# Valutazione della prevenzione del tracciamento - Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica

Questo documento aggrega risorse utili e valuta le imminenti modifiche pianificate da Google Chrome nell’ambito della loro iniziativa per eliminare gradualmente i cookie di terze parti.

La valutazione viene eseguita per le applicazioni TV Everywhere (TVE) in esecuzione sul browser Google Chrome che utilizzano l’SDK JavaScript v4 di Adobe Pass Access Enabler per l’integrazione con i servizi back-end di autenticazione di Adobe Pass.

## Risorse pubbliche

Di seguito è riportato un elenco di risorse aggregate dal sito web Google dedicato agli sviluppatori e dal blog ufficiale che consigliamo ai nostri clienti di consultare:

* [Passaggio successivo verso la graduale eliminazione dei cookie di terze parti in Chrome](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [Documentazione per gli sviluppatori su Privacy Sandbox](https://developers.google.com/privacy-sandbox)
* [Prepararsi per le restrizioni dei cookie di terze parti](https://developers.google.com/privacy-sandbox/3pcd)
* [Prepararsi per la chiusura graduale dei cookie di terze parti](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [Preparazione alla fine dei cookie di terze parti](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Cookie di terze parti limitati per impostazione predefinita per l’1% degli utenti di Chrome](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## Timeline

Come breve riepilogo, Google Chrome ha iniziato il test [Protezione del tracciamento](https://privacysandbox.com/), una nuova funzione che limita il tracciamento tra siti che influisce su tutti i cookie di terze parti.

Inizialmente, tale modifica è iniziata all’inizio del 2024 e interessa circa l’1% dei suoi utenti e il suo piano (provvisorio) è di estenderla fino al 100% degli utenti a partire dal terzo trimestre del 2024.

## Valutazione

Google ha pubblicato un documento che aggrega il playbook consigliato per prepararsi per i cookie di terze parti ed è disponibile al seguente collegamento: https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout.

Abbiamo aderito a questo playbook per la valutazione delle applicazioni TV Everywhere (TVE) in esecuzione su Google Chrome e che utilizzano Adobe Pass Access Enabler JavaScript SDK v4 per l’integrazione con i servizi back-end di autenticazione di Adobe Pass.

### Conclusioni

In base ai nostri test, simulando i prossimi aggiornamenti di Google Chrome, i flussi di business principali di TVE **continuerà a funzionare come previsto**.

Tuttavia, è fondamentale riconoscere la strategia più ampia di Google, che comporta non solo l’interruzione dei cookie di terze parti, ma anche il partizionamento dello storage di terze parti.

Di conseguenza, gli utenti di Chrome subiranno interruzioni con le funzioni Single Sign-On (SSO), Single Logout (SLO) e autenticazione passiva, che richiedono azioni di accesso/disconnessione separate per ogni applicazione TVE utilizzata (in linea con l’esperienza corrente su Safari).

## Invito all&#39;autovalutazione

Invitiamo i nostri clienti a condurre valutazioni simili in modo proattivo, per individuare potenziali problemi con largo anticipo e acquisire familiarità con l’esperienza utente rivista di Google Chrome.

Questa valutazione deve includere sia servizi di prime parti che servizi di terze parti, soprattutto per quanto riguarda l’integrazione dell’SDK JavaScript v4 per Adobe Pass Access Enabler.

In caso di difficoltà relative ai flussi aziendali di TVE, tra cui autenticazione, pre-autorizzazione, autorizzazione, metadati utente o disconnessione, ti invitiamo a segnalare un rapporto tramite un ticket Zendesk con il nostro team di assistenza clienti.

Per assistenza nello sviluppo del piano di autovalutazione, fare riferimento alle sezioni seguenti.

### Controllo dell’utilizzo dei cookie

A partire da Chrome 118, il [Problemi relativi a DevTools](https://developer.chrome.com/docs/devtools/issues/) La scheda evidenzia i cookie potenzialmente interessati con il seguente messaggio: `Cookie sent in cross-site context will be blocked in future Chrome versions`.

I cookie contrassegnati per l’utilizzo da parte di terze parti possono essere identificati dal rispettivo `SameSite=None` valore attributo.

Per ulteriori informazioni, visitare il sito https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### Prova di rottura

Per testare la rottura avviare Chrome utilizzando `--test-third-party-cookie-phaseout` flag della riga di comando o da Chrome 118 abilita `#test-third-party-cookie-phaseout` in `chrome://flags/`.

Questo imposterà Google Chrome per bloccare i cookie di terze parti e garantire che la funzionalità futura sia attiva al fine di simulare al meglio lo stato dopo il ritiro graduale.

Vale la pena approfondire le specifiche tecniche per i seguenti flag Chrome:

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

Per ulteriori informazioni, visitare il sito https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## Altri browser

### Firefox

Firefox ha implementato un meccanismo chiamato: `Enhanced Tracking Protection` un paio di anni fa.

Consulta di seguito alcune utili risorse da Firefox:

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari ha implementato il suo meccanismo chiamato: `Intelligent Tracking Prevention` un paio di anni fa.

Di seguito trovi alcune utili risorse di Safari:

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Di seguito trovi alcune utili risorse da Adobe Pass:

* [Valutazione della prevenzione del tracciamento - Apple Safari](tracking-prevention-assessment-apple-safari.md)
