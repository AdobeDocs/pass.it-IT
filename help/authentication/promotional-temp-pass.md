---
title: Passaggio temporaneo promozionale
description: Passaggio temporaneo promozionale
exl-id: 705c1ba9-0430-4e3b-add1-d9e4da3f82d1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 0%

---

# Passaggio temporaneo promozionale {#promotional-temp-pass}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Riepilogo delle funzioni {#feature-summary}

Promotional Temp Pass consente ai programmatori di offrire accesso temporaneo ai loro contenuti protetti per gli utenti che non hanno credenziali dell&#39;account con un MVPD.

Passaggio temporaneo promozionale è progettato per essere utilizzato per l&#39;esecuzione di campagne promozionali in cui un utente, dopo aver fornito informazioni di identificazione valide (ad esempio l&#39;indirizzo e-mail) al programmatore, sarà in grado di utilizzare un **numero predefinito di titoli VOD diversi per un periodo di tempo predefinito**.

>[!IMPORTANT]
>
>Adobe non memorizza informazioni personali (PII, Personally Identifiable Information). Pertanto, il programmatore deve impostare un hash sulle informazioni fornite dall’utente univoco sulle API di autenticazione di Adobe Pass.

Il Passaggio Temporaneo Promozionale è basato sul [Passaggio temporaneo](/help/authentication/temp-pass.md) , ovvero include tutte le funzionalità Passaggio temporaneo.

Una volta superato il numero massimo predefinito di titoli VOD o il periodo di tempo predefinito, l’utente non potrà accedere al contenuto sullo stesso dispositivo o utilizzando le stesse informazioni sull’identificatore utente (ad esempio, l’indirizzo e-mail) fino a quando i token di autorizzazione non vengono cancellati dal server di autenticazione di Adobe Pass.

>[!NOTE]
>
>Passaggio temporaneo fa parte del pacchetto flusso di lavoro Premium. Se sei interessato a utilizzare questa funzionalità, contatta il rappresentante commerciale Adobe Pass.

## Confronto tra Passa temporanea e Passa temporanea promozionale {#tp-ptp-comparison}

| Passaggio temporaneo | Passaggio temporaneo promozionale |
|----------------------------------|----------------------------------------------------------------------------------------|
| Accesso al contenuto <ul><li>basato sul tempo</li></ul> | Accesso al contenuto <ul><li>basato sul tempo</li><li>in base al numero di risorse</li></ul> |
| Accesso alla sicurezza basato su <ul><li>ID dispositivo</li></ul> | Sicurezza basata su <ul><li>ID dispositivo</li><li>hash over sulle informazioni identificative dell&#39;utente fornite (ad esempio, e-mail)</li></ul> |
| API di errore client disponibile | API di errore client disponibile |
| Reimpostazione/rimozione disponibile | Reimpostazione/rimozione disponibile |

## Dettagli delle funzioni {#feature-details}

Questa funzione consente agli utenti di accedere ai contenuti promozionali da un dispositivo specifico (telefono e tablet) dopo aver fornito informazioni univoche, come l&#39;indirizzo e-mail, nell&#39;applicazione del programmatore.

Il programmatore fornirà un hash sui dati PII dell’utente nelle API di autenticazione e autorizzazione. Questo hash verrà utilizzato insieme all’ID del dispositivo per generare una chiave univoca per identificare l’utente e il dispositivo.

In base all’ID dispositivo e alle informazioni fornite dall’utente, e seguendo la logica riportata di seguito, l’autenticazione di Adobe Pass determinerà se l’utente si trova in una nuova versione di prova o in una esistente:

* nuovo hash sulle informazioni fornite dall&#39;utente (ad esempio, e-mail), nuovo ID dispositivo => nuova versione di prova
* hash esistente sulle informazioni fornite dall&#39;utente (ad esempio, e-mail), ID nuovo dispositivo => versione di valutazione esistente (con hash esistente sulle informazioni fornite dall&#39;utente (ad esempio, e-mail))
* nuovo hash sulle informazioni fornite dall&#39;utente (ad esempio, e-mail), ID dispositivo esistente => versione di prova esistente (con ID dispositivo esistente)
* hash esistente sulle informazioni fornite dall&#39;utente (ad esempio, e-mail), ID dispositivo esistente => versione di prova esistente

>[!NOTE]
>La convalida e l’hashing delle informazioni fornite dall’utente vengono gestiti dal programmatore, non da Adobi.

**La funzione Passaggio temporaneo promozionale può essere configurata in base alle seguenti proprietà:**

* Chiave delle informazioni fornite dall&#39;utente (ad esempio, e-mail)
* Numero di risorse che l’utente ha il diritto di utilizzare
* TTL: l’intervallo di tempo in cui l’utente ha il diritto di utilizzare il numero configurato di risorse

### Metadati utente {#user-metadata}

Per facilitare l&#39;attuazione dell&#39;applicazione del programmatore, è necessario: **le informazioni sui metadati dell’utente sono esposte** sul Passaggio temporaneo promozionale, con i tasti corrispondenti (per attivare i tasti, contattare tve-support@adobe.com):

* **risorse_rimanenti**: il numero di risorse rimanenti che l’utente corrente può utilizzare
* **used_assets**: elenco di risorse già utilizzate dall&#39;utente corrente
* **data_scadenza**: la data di scadenza dell’utente corrente

### Come viene calcolato il tempo di visualizzazione? {#compute-viewing-time}

Il tempo di validità di un passaggio temporaneo non è correlato al tempo di visualizzazione del contenuto nell&#39;applicazione del programmatore. Al momento della richiesta iniziale di autorizzazione da parte dell’utente tramite Passaggio temporaneo promozionale, viene calcolato un tempo di scadenza aggiungendo il tempo della richiesta corrente iniziale al TTL (intervallo di tempo della durata) specificato dal programmatore.

### Autenticazione e autorizzazione {#authn-authz}

Per i flussi Promozionali Temp Pass, l&#39;autenticazione e l&#39;autorizzazione non comunicano con un MVPD effettivo, **pertanto tutte le richieste di autorizzazione avranno esito positivo** purché siano soddisfatte tutte le seguenti condizioni:

* i token di autorizzazione sono validi per le risorse specificate
* numero di **used_assets** è inferiore al limite impostato dal programmatore
* **data_scadenza** il valore è successivo alla data corrente.

### Comportamento verifica preliminare {#preflight-beh}

Quando viene effettuata una richiesta di verifica preliminare o di autorizzazione preliminare per un MVPD Promozionale, la risposta di verifica preliminare corrispondente restituita conterrà l&#39;intero elenco di risorse della Richiesta di verifica preliminare come esito positivo della verifica preliminare.

La logica che sta dietro a questo è: le condizioni di autorizzazione Passaggio temporaneo promozionale si basano sul tempo e sul numero di risorsa, anziché su risorse specifiche. In particolare, finché viene rispettato il vincolo di tempo e finché il limite di risorse non viene superato, le risorse chiamate verranno autorizzate.

### SSO {#sso}

SSO non abilitato per le istanze di Passaggio temporaneo promozionale, configurato con &quot;Autenticazione per richiedente&quot; abilitato. Ciò significa che quando l’utente passa dall’applicazione A a un’altra applicazione B che sono entrambe integrate con lo stesso Passaggio temporaneo promozionale, l’accesso non verrà eseguito automaticamente.

### Disconnetti {#logout}

Tutti i token di un dispositivo vengono eliminati alla disconnessione. Pertanto, il passaggio dal Pass Temp Promozionale a un MVPD regolare selezionato dall&#39;utente non deve fare affidamento su questa implementazione. Si consiglia di utilizzare il `setSelectedProvider(null)` per cancellare lo stato dell’applicazione e riavviare il flusso di autenticazione, che offre un’esperienza utente migliore.

### Diagramma del flusso di Passata Temp promozionale {#promo-tempass-flowdia}

![Diagramma del flusso di Passata Temp promozionale](assets/promo-temp-pass-flow.png)

*Figura: Flusso passaggio temporaneo promozionale*

## Implementare il passaggio temporaneo promozionale {#impl-promo-tempass}

Il passaggio temporaneo promozionale richiede le seguenti funzionalità lato client:

* **Informazioni sull&#39;identificatore utente, ad esempio propagazione degli indirizzi di posta elettronica** (invio dell’indirizzo e-mail dell’utente sui flussi di autenticazione e autorizzazione). L’e-mail è richiesta da Adobe Pass Authentication per associare i token di autenticazione e autorizzazione (come nel caso del `device_ID`, obbligatorio per tutte le chiamate).
* **Forza autenticazione** : consente al programmatore di forzare un flusso di autenticazione quando l’utente è già autenticato. Questa funzionalità è necessaria per forzare l’aggiornamento dei metadati dell’utente (la chiave dei metadati dell’utente) **used_assets** contiene il numero di risorse disponibili) a ogni avvio dell&#39;app. Poiché l’utente può accedere a più dispositivi, i metadati dell’utente presenti sul dispositivo durante l’avvio dell’app non sono affidabili e dobbiamo aggiornarli per riflettere lo stato corrente per tale utente specifico (identificato dall’indirizzo e-mail).


>[!IMPORTANT]
>L’autenticazione forzata è possibile solo su iOS e Android.
>L’autenticazione Adobe Pass non dispone di un meccanismo integrato per arrestare lo streaming gratuito una volta trascorsi gli X minuti. L’autenticazione Adobe Pass cesserà di essere rilasciata **autorizzazione** e **file multimediali brevi** token una volta che l’utente utilizza le risorse libere Y. Spetta ai programmatori limitare l’accesso una volta scaduto il Passaggio Temporale Promozionale.

## Sicurezza {#security}

>[!IMPORTANT]
>Adobe non memorizza informazioni personali (PII, Personally Identifiable Information). Pertanto, il programmatore deve impostare un hash sulle informazioni fornite dall’utente univoco sulle API di autenticazione di Adobe Pass.

**Hashing delle informazioni sull’identificatore utente**

L’Adobe consiglia di utilizzare **SHA-2** famiglia o suo specifico **SHA-256**, **SHA-512** funziona sui dati prima che vengano inviati ad Adobe.

Ad esempio: **SHA-256** oltre **user@domain.com** è **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**.

## Reimposta o rimuovi passaggio temporaneo promozionale {#reset-promo-tempass}

Alcune regole aziendali richiedono l&#39;eliminazione periodica del pass temporaneo promozionale. Per farlo, Adobe Pass Authentication fornisce ai programmatori un *pubblico* API web, descritta di seguito:

| `DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset` |
|----|
| <ul><li>Protocollo: **https**</li><li>Host:<ul><li>Versione: **mgmt.auth.adobe.com**</li><li>Prequal: **mgmt-prequal.auth.adobe.com**</li></ul></li><li>Percorso: **/reset-tempass/v2/reset**</li><li>Parametri query: **device_id=all&amp;requestor_id=THE_REQUESTOR_ID&amp;mvpd_id=THE_TEMPASS_MVPD_ID**</li><li>intestazioni: ApiKey: **1232293681726481**</li> <li>Risposta:<ul><li>Operazione completata: **HTTP 204**</li><li>Errore: **HTTP 400** in caso di richieste non corrette, **HTTP 401** se ApiKey non è specificato, **HTTP 403** se ApiKey non è valido</li></ul></li></ul> |

Oltre ai requisiti per l&#39;eliminazione del passaggio temporaneo, il passaggio temporaneo promozionale utilizza l&#39;hash sulle informazioni dell&#39;identificatore utente inviate come **generic_data** relativa all’autenticazione e all’autorizzazione per la rimozione.

Verrà inviato l’hash, non l’intero JSON:

```cURL
$ curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=FlexibleTempPass"
```

### Client supportati {#supported-clients}

| Client di autenticazione Adobe Pass | Passaggio temporaneo promozionale | Strumento Ripristina | Supporto di codice di risposta dedicato/errore client |
|:--------------------------------------:|:---------------------:|:----------:|:-----------------------------------------------:|
| JS Access Enabler | SÌ | SÌ | SÌ (a partire dalla versione v 3.0.0) |
| IOS client nativo | SÌ | SÌ | SÌ (a partire dalla versione 1.10) |
| Android client nativo | SÌ | SÌ | SÌ |
| API senza client | SÌ | SÌ | NO |


## Limitazioni {#limitations}

Questa sezione descrive le limitazioni che si applicano all&#39;implementazione corrente di Promotional Temp Pass.

### Clientless {#lim-clientless}

**Dispositivi avanzati senza un ID dispositivo univoco**

Non tutte le app per Smart Device sono in grado di fornire un ID dispositivo univoco. In assenza di un identificatore, l’autenticazione Adobe Pass può utilizzare l’UUID generato dal servizio Codice di registrazione Adobe come ID dispositivo univoco. Ciò significa che quando l’utente si disconnette, i token di autenticazione e autorizzazione verranno eliminati. Una volta che l’utente tenterà nuovamente di eseguire l’autenticazione, questa volta con informazioni utente diverse (ad esempio, e-mail) l’utente sarà nuovamente in grado di autorizzare. L’Adobe consiglia di aggiungere un flusso di interfaccia utente che non consenta a un utente di &quot;ingannare&quot; il sistema e di aggiungere una logica per determinare se si tratta di un nuovo utente che richiede una versione di prova o di una versione di prova esistente.

**Reimpostazione/rimozione passaggio temporaneo**

Reset Temp Pass for Clientless non è disponibile nei casi specifici di Xbox360 e Xbox One, perché queste piattaforme richiedono un&#39;analisi aggiuntiva degli ID dispositivo, non è possibile con lo strumento Reset Temp Pass.

<!--
>[!RELATEDINFORMATION]
>
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
-->
