---
title: Accesso e disconnessione senza aggiornamento
description: Accesso e disconnessione senza aggiornamento
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1761'
ht-degree: 0%

---

# Accesso e disconnessione senza aggiornamento {#tefresh-less-login-and-logout}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

Per le applicazioni web, è necessario tenere conto di alcuni possibili scenari diversi per l’autenticazione e la disconnessione degli utenti.  Gli MVPD richiedono che gli utenti accedano alla pagina web di MVPD per autenticarsi, con i seguenti fattori aggiuntivi in gioco:

- Alcuni MVPD richiedono un reindirizzamento completo dal sito alla pagina di accesso
- Alcuni MVPD richiedono di aprire un iFrame sul sito per visualizzare la pagina di accesso di MVPD
- Alcuni browser non gestiscono bene lo scenario iFrame, quindi per questi browser un’alternativa migliore è utilizzare una finestra popup invece dell’iFrame

Prima dell’autenticazione Adobe Pass 2.7, tutti questi scenari per l’autenticazione di un utente richiedevano un aggiornamento dell’intera pagina del programmatore.Per la versione 2.7 e successive, il team di autenticazione di Adobe Pass ha migliorato questi flussi in modo che l’utente non debba aggiornare la pagina dell’app durante l’accesso e la disconnessione.


## Descrizione dettagliata {#detailed_description}

Iniziamo con un riepilogo dei flussi di autenticazione e disconnessione originali, quindi seguiamo questo con i flussi di autenticazione e disconnessione migliorati. Si noti che le prime quattro sezioni riguardano i normali MVPD (non-TempPass), mentre l&#39;ultima descrive l&#39;implementazione speciale che deve essere applicata per TempPass:

- [Flusso di autenticazione originale](#orig_authn)
- [Flusso di disconnessione originale](#orig_logout)
- [Flusso di autenticazione migliorato](#improved_authn)
- [Flusso di disconnessione migliorato](#improved_logout)
- [Flusso TempPass](#improved_temppass)

</br>

## Flussi di autenticazione/disconnessione originali {#orig_authn}

**Autenticazione**

I client web di Adobe Pass Authentication possono autenticarsi in due modi, a seconda dei requisiti degli MVPD:

1. **Reindirizzamento a pagina intera -** dopo che l&#39;utente ha selezionato un provider    (configurato con reindirizzamento a pagina intera) dal selettore MVPD sulla    Il sito Web del programmatore, `setSelectedProvider(<mvpd>)`, viene richiamato in AccessEnabler e l&#39;utente viene reindirizzato alla pagina di accesso di MVPD. Dopo che l&#39;utente ha fornito credenziali valide, viene reindirizzato al sito Web del programmatore. AccessEnabler è inizializzato e il token di autenticazione viene recuperato dall&#39;autenticazione di Adobe Pass durante `setRequestor`.
1. **Finestra iFrame/Popup -** Dopo che l&#39;utente ha selezionato un provider (configurato con iFrame), `setSelectedProvider(<mvpd>)` viene richiamato in AccessEnabler. Questa azione attiverà il callback `createIFrame(width, height)`, avvisando il programmatore di creare un iFrame (o pop-up - a seconda del browser/preferenze) con il nome `"mvpdframe"` e le dimensioni fornite. Dopo aver creato l&#39;iFrame/popup, AccessEnabler carica la pagina di accesso di MVPD nell&#39;iFrame/popup. L’utente fornisce credenziali valide e l’iFrame/popup viene reindirizzato all’autenticazione Adobe Pass, che restituisce uno snippet JS che chiude l’iFrame/popup e ricarica la pagina padre (sito web Programmatore). Analogamente al flusso 1, il token di autenticazione viene recuperato durante `setRequestor`.

Il callback `displayProviderDialog` (attivato da `getAuthentication`/`getAuthorization`) restituisce un elenco di MVPD e le relative impostazioni appropriate. La proprietà `iFrameRequired` di un MVPD consente al programmatore di sapere se deve attivare il flusso 1 o il flusso 2. Si noti che il programmatore è tenuto a intraprendere azioni aggiuntive (creazione di un iFrame/popup) solo per il flusso 2.

**Annulla autenticazione**

Inoltre, l’utente annulla esplicitamente il flusso di autenticazione chiudendo la pagina di accesso. Di seguito sono riportati gli scenari e la soluzione proposta ai programmatori:

1. **Reindirizzamento a pagina intera -** Quando la pagina di accesso viene chiusa, l&#39;utente dovrà tornare al sito Web del programmatore e avviare l&#39;intero flusso dall&#39;inizio. In questo scenario non è richiesta alcuna azione esplicita da parte del programmatore.
1. **iFrame -** Si consiglia al programmatore di ospitare l&#39;iFrame all&#39;interno di un `div` (o componente simile dell&#39;interfaccia utente) a cui è associato un pulsante Chiudi. Quando l&#39;utente preme il pulsante Chiudi, il programmatore distrugge l&#39;iFrame insieme all&#39;interfaccia utente associata ed esegue `setSelectedProvider(null)`. Questa chiamata consente ad AccessEnabler di cancellare il proprio stato interno e consente all&#39;utente di avviare un flusso di autenticazione successivo. `setAuthenticationStatus` e `sendTrackingData(AUTHENTICATION_DETECTION...)` verranno attivati per segnalare un flusso di autenticazione non riuscito (sia su `getAuthentication` che su `getAuthorization`).
1. **Popup -** Alcuni browser non sono in grado di rilevare con precisione l&#39;evento di chiusura della finestra, pertanto è necessario adottare un approccio diverso (a differenza del flusso iFrame riportato sopra). L&#39;Adobe consiglia al programmatore di inizializzare un timer che verifichi periodicamente l&#39;esistenza della finestra popup di accesso. Se la finestra non esiste, il programmatore può essere sicuro che l&#39;utente abbia annullato manualmente il flusso di accesso e che il programmatore possa continuare a chiamare `setSelectedProvider(null)`. I callback attivati sono gli stessi del flusso 2 precedente.

</br>

## Flusso di disconnessione originale {#orig_logout}

L&#39;API di logout di AccessEnabler cancella lo stato locale della libreria e carica l&#39;URL di logout di MVPD nella scheda/finestra corrente. Il browser passa all’endpoint di disconnessione dell’MVPD e, al termine del processo, l’utente viene reindirizzato al sito web del programmatore. L&#39;unica azione richiesta per conto dell&#39;utente è premere il pulsante o il collegamento Logout e avviare il flusso; non è richiesta alcuna interazione dell&#39;utente sull&#39;endpoint logout dell&#39;MVPD.

**Autenticazione Originale / Flusso Di Disconnessione Con Aggiornamento Pagina**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## Autenticazione migliorata (senza aggiornamento) {#improved_authn}

>[!NOTE]
>
>I flussi migliorati di accesso e disconnessione senza aggiornamento richiedono che il browser supporti le moderne tecnologie HTML5, tra cui la messaggistica web.

I flussi di autenticazione (accesso) e disconnessione descritti in precedenza forniscono un’esperienza utente simile ricaricando la pagina principale al termine di ogni flusso.  La funzione corrente mira a migliorare l’esperienza utente fornendo un accesso e una disconnessione senza aggiornamento (in background). Il programmatore può abilitare/disabilitare l&#39;accesso e la disconnessione in background passando due flag booleani (`backgroundLogin` e `backgroundLogout`) al parametro `configInfo` dell&#39;API `setRequestor`. Per impostazione predefinita, l’accesso o la disconnessione in background sono disabilitati (in questo modo si garantisce la compatibilità con l’implementazione precedente).

**Esempio:**

```JSON
    var configInfo = {
        callSetConfig: true,
        backgroundLogin: true,
        backgroundLogout: true
    };
    accessEnabler.setRequestor(REQUESTOR_ID, null, configInfo);
```

**Autenticazione**

I punti seguenti descrivono la transizione tra i flussi di autenticazione originali e i flussi migliorati:

1. Il reindirizzamento a pagina intera viene sostituito da una nuova scheda del browser in cui viene eseguito l’accesso MVPD. Il programmatore deve creare una nuova scheda (tramite `window.open`) denominata `mvpdwindow` quando l&#39;utente seleziona un MVPD (con `iFrameRequired = false`). Il programmatore esegue quindi `setSelectedProvider(<mvpd>)`, consentendo ad AccessEnabler di caricare l&#39;URL di accesso MVPD nella nuova scheda. Dopo che l&#39;utente avrà fornito credenziali valide, Adobe Pass Authentication chiuderà la scheda e invierà un window.postMessage al sito Web del programmatore che segnala ad AccessEnabler il completamento del flusso di autenticazione. Vengono attivati i seguenti callback:

   - Se il flusso è stato avviato da `getAuthentication`: `setAuthenticationStatus` e `sendTrackingData(AUTHENTICATION_DETECTION...)` verranno attivati per segnalare un&#39;autenticazione riuscita/non riuscita.

   - Se il flusso è stato avviato da `getAuthorization`: `setToken/tokenRequestFailed` e `sendTrackingData(AUTHORIZATION_DETECTION...)` verranno attivati per segnalare un&#39;autorizzazione riuscita/non riuscita.

1. Il flusso della finestra iFrame / popup rimane per lo più invariato, con la differenza che dopo che l&#39;utente ha fornito credenziali valide, la pagina padre non verrà ricaricata. L&#39;iFrame o il popup verrà chiuso automaticamente dopo l&#39;accesso e `window.postMessage` verrà inviato alla pagina padre, notificando all&#39;AccessEnabler il completamento del flusso. Vengono attivati gli stessi callback del flusso precedente, **più il nuovo callback seguente:`destroyIFrame`**. Il callback `destroyIFrame` consente al programmatore di rimuovere qualsiasi componente iFrame associato/ausiliario, ad esempio le decorazioni dell&#39;interfaccia utente. L’esistenza di questo callback non era necessaria nel vecchio flusso di autenticazione perché al termine dell’accesso, l’autenticazione Adobe Pass ricaricava la pagina del programmatore, distruggendo tutti i componenti dell’interfaccia utente.

</br>

>[!IMPORTANT]
> 
>È necessario caricare l&#39;iFrame di accesso MVPD o la finestra popup come figlio diretto della pagina che contiene l&#39;istanza di AccessEnabler. Se l&#39;iFrame o la finestra popup di accesso MVPD è nidificata a due o più livelli sotto la pagina contenente l&#39;istanza di AccessEnabler, il flusso potrebbe bloccarsi. Ad esempio, se disponi di un iFrame situato tra la pagina principale e l’iFrame MVPD (Page =\> iFrame =\> MVPD iFrame), il flusso di accesso potrebbe non riuscire.

</br>

**Annulla autenticazione**

Di seguito sono riportati i flussi per annullare l’autenticazione:

1. **Scheda Browser -** Poiché la scheda è fondamentalmente una nuova finestra, l&#39;acquisizione del relativo evento di chiusura presenta le stesse limitazioni illustrate nello scenario 3 dai flussi di autenticazione precedenti. Inoltre, l’approccio basato sul timer non è possibile in questo caso, perché non è possibile distinguere tra una scheda chiusa manualmente dall’utente e una scheda chiusa automaticamente alla fine del flusso di accesso. In questo caso, la soluzione prevede che AccessEnabler rimanga &quot;invisibile all’utente&quot; (non vengono attivati callback) quando l’utente annulla il flusso. Inoltre, il programmatore non è tenuto a intraprendere alcuna azione specifica. L&#39;utente sarà in grado di avviare un altro flusso di autenticazione senza ricevere l&#39;errore &quot;Errore più richieste di autenticazione&quot; (questo errore è stato disabilitato in AccessEnabler per l&#39;accesso in background).

1. **iFrame -** Il programmatore può seguire l&#39;approccio descritto nello scenario 2 dai vecchi flussi di autenticazione (creare un&#39;interfaccia utente wrapper dall&#39;iFrame e il pulsante Close associato che attiva `setSelectedProvider(null)`). Anche se questo approccio non è più un requisito importante (per l’accesso in background sono consentiti più flussi di autenticazione, come discusso nello scenario 1 di cui sopra), è ancora consigliato dall’Adobe.

1. **Popup -** È identico al flusso della scheda Browser qui sopra.

</br>

## Flusso di disconnessione migliorato {#improved_logout}

Il nuovo flusso di logout verrà eseguito in un iFrame nascosto, eliminando in tal modo il reindirizzamento dell’intera pagina.  Ciò è possibile perché l&#39;utente non ha bisogno di intraprendere azioni specifiche sulla pagina di disconnessione di MVPD.

Una volta completato il flusso di logout, l’iFrame verrà reindirizzato a un endpoint di autenticazione Adobe Pass personalizzato. Verrà distribuito un frammento JS che esegue `window.postMessage` all&#39;elemento padre, notificando all&#39;AccessEnabler il completamento della disconnessione. Vengono attivati i seguenti callback: `setAuthenticationStatus()` e `sendTrackingData(AUTHENTICATION_DETECTION ...)`, segnalando che l&#39;utente non è più autenticato.

L’illustrazione seguente mostra il flusso senza aggiornamento che consente a un utente di accedere al proprio MVPD senza aggiornare la pagina principale dell’applicazione:

**Flusso di autenticazione/disconnessione migliorato (senza aggiornamento)**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## Flusso TempPass {#improved_temppas}

L’accesso senza aggiornamento segue un approccio diverso per gli MVPD di tipo TempPass.

Poiché il flusso TempPass richiede che una finestra venga creata automaticamente e chiusa senza esplicita interazione da parte dell’utente, potrebbe rappresentare un problema per alcuni browser (blocchi popup). AccessEnabler implementa quindi la fase di accesso dietro le quinte, senza richiedere un contenitore Web creato dal programmatore.

Di seguito sono riportati gli aspetti di cui il programmatore deve essere a conoscenza quando implementa TempPass per l’accesso e la disconnessione senza aggiornamento:

- Prima di avviare l&#39;autenticazione, è necessario creare l&#39;iFrame o la finestra popup solo per gli MVPD non TempPass. Il programmatore può rilevare se un MVPD è TempPass o meno leggendo la proprietà `tempPass` dell&#39;oggetto MVPD (restituito da `setConfig()` / `displayProviderDialog()`).

- Il callback `createIFrame()` deve contenere un controllo per TempPass ed eseguire la relativa logica solo quando MVPD non è TempPass.

- Il callback `destroyIFrame()` deve contenere un controllo per TempPass ed eseguire la relativa logica solo quando MVPD non è TempPass.

- I callback `setAuthenticationStatus()` e `sendTrackingData()` vengono richiamati al termine dell&#39;autenticazione (esattamente come nel flusso senza aggiornamento per i normali MVPD).

>[!NOTE]
>
>Questo flusso è disponibile solo per TempPass senza aggiornamento. Per il flusso di aggiornamento, TempPass deve essere gestito esplicitamente (quando TempPass richiede iFrame / popup)

</br>

Nell&#39;esempio di codice riportato di seguito viene illustrato come gestire una finestra MVPD sul sito Web di un programmatore (sia per MVPD normali che per TempPass):

```javascript
    var aeHostname = "https://entitlement.auth.adobe.com";
    var mvpdWindow = null;
    var mvpd = <mvpd_object_from_displayProviderDialog>;
    var useIframeLogin = <boolean_depending_on_browser_or_Programmer_preferences>;
    var backgroundLogin = <boolean_depending_on_Programmer_preferences>;
     
    // Do not create any windows for refreshless and temp pass
    if (!(backgroundLogin && mvpd.tempPass)) {
        if (backgroundLogin && !mvpd.popup) {
            mvpdWindow = window.open(aeHostname, "mvpdwindow");
        } else if (mvpd.popup && !useIframeLogin) {
            var width = mvpd.width;
            var height = mvpd.height;
            // Center on screen
            var top = (document.all) ? window.screenTop : window.screenY + 100;
            var left = (document.all) ? window.screenLeft : window.screenX + window.innerWidth / 2 - width / 2;
        
            mvpdWindow = window.open(aeHostname, "mvpdframe",
                           "width=" + width + ",height=" + height + ",top=" + top + ",left=" + left);
            // Monitor the mvpd popup for close
            if (!backgroundLogin) {
                clearInterval(cancelTimer);
                cancelTimer = setInterval(function () {
                    if (mvpdWindow && mvpdWindow.closed) {
                        clearInterval(cancelTimer);
                        $('#mvpddiv').hide();
                        accessEnablerAPI.setSelectedProvider(null);
                    }
                }, 200);
            }
        }
    }
```
