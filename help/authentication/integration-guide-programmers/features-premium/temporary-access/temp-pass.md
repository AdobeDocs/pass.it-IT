---
title: Passaggio temporaneo
description: Passaggio temporaneo
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '2243'
ht-degree: 0%

---

# Passaggio temporaneo {#temp-pass}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Riepilogo delle funzioni {#tempass-featur-summary}

Temp Pass consente ai programmatori di offrire accesso temporaneo ai loro contenuti protetti, per gli utenti che non hanno credenziali dell&#39;account con un MVPD.  Il passaggio temporaneo include le seguenti funzionalità:

* Temp Pass può essere configurato per fornire accesso temporaneo per coprire una varietà di scenari, tra cui i seguenti:
   * Un programmatore può offrire una versione giornaliera, breve (ad esempio, un’anteprima di 10 minuti) di uno dei suoi siti.
   * Un programmatore può offrire una singola, lunga presentazione (ad esempio, quattro ore) di un importante evento sportivo come le Olimpiadi, o NCAA March Madness.
   * Un programmatore può fornire una combinazione dei due scenari precedenti; ad esempio, un periodo di visualizzazione iniziale più lungo di un giorno, seguito da una serie di periodi brevi che si ripetono ogni giorno per un certo numero di giorni successivi.
* I programmatori specificano la durata (Time-To-Live, TTL) della loro Temp Pass.
* Il passaggio temporaneo funziona per richiedente.  Ad esempio, la NBC potrebbe impostare una Temp Pass di 4 ore per il richiedente &quot;NBCOlympics&quot;.
* I programmatori possono reimpostare tutti i token concessi a un particolare richiedente.  Il &quot;MVPD temporaneo&quot; utilizzato per implementare il passaggio temporaneo deve essere configurato con &quot;Autenticazione per richiedente&quot; abilitato.
* **Accesso Passaggio temporaneo concesso a singoli utenti su dispositivi specifici**. Dopo la scadenza dell’accesso Passaggio temporaneo per un utente, quest’ultimo non potrà ottenere l’accesso temporaneo sullo stesso dispositivo fino a quando il token di autorizzazione scaduto dell’utente non verrà cancellato dal server di autenticazione di Adobe Pass.


>[!NOTE]
>
>Passaggio temporaneo fa parte del pacchetto flusso di lavoro Premium. Se sei interessato a utilizzare questa funzionalità, contatta il rappresentante commerciale Adobe Pass.

## Dettagli delle funzioni {#tempass-featur-details}

* **Modalità di calcolo del tempo di visualizzazione** - Il tempo di validità di un passaggio temporaneo non è correlato al tempo di visualizzazione dei contenuti dell&#39;applicazione del programmatore trascorso da un utente.  Al momento della richiesta iniziale di autorizzazione da parte dell’utente tramite Passaggio temporale, viene calcolato un tempo di scadenza aggiungendo il tempo della richiesta corrente iniziale al TTL specificato dal programmatore. Questa data di scadenza è associata all’ID dispositivo dell’utente e all’ID richiedente del programmatore ed è memorizzata nel database di autenticazione di Adobe Pass. Ogni volta che l’utente tenta di accedere al contenuto utilizzando Temp Pass dallo stesso dispositivo, l’autenticazione di Adobe Pass confronta il tempo di richiesta del server con il tempo di scadenza associato all’ID dispositivo dell’utente e all’ID richiedente del programmatore. Se il tempo di richiesta del server è inferiore al tempo di scadenza, l&#39;autorizzazione verrà concessa; in caso contrario, l&#39;autorizzazione verrà negata.
* **Parametri di configurazione** - Un programmatore può specificare i seguenti parametri di Passaggio temporaneo per creare una regola di Passaggio temporaneo:
   * **TTL token** - Il tempo che un utente può osservare senza accedere a un MVPD. L’ora è basata sull’orologio e scade nel caso in cui l’utente stia guardando il contenuto o meno.
  >[!NOTE]
  >A un ID richiedente non può essere associata più di una regola di passaggio temporaneo.
* **Autenticazione/Autorizzazione** - Nel flusso Passaggio temporaneo, si specifica il MVPD come &quot;Passaggio temporaneo&quot;.  L’autenticazione di Adobe Pass non comunica con un MVPD effettivo nel flusso Passaggio temporaneo, pertanto il MVPD &quot;Passaggio temporaneo&quot; autorizza qualsiasi risorsa. I programmatori possono specificare una risorsa accessibile utilizzando Temp Pass, come fanno per le altre risorse sul loro sito. La libreria Media Verifier può essere utilizzata come di consueto per verificare il token multimediale corto Temp Pass e applicare il controllo delle risorse prima della riproduzione.
* **Tracciamento dei dati nel flusso di passaggio temporaneo** - Due punti relativi al tracciamento dei dati durante un flusso di adesione al passaggio temporaneo:
   * L&#39;ID di tracciamento passato dall&#39;autenticazione Adobe Pass al callback **sendTrackingData()** è un hash dell&#39;ID dispositivo.
   * Poiché l&#39;ID MVPD utilizzato nel flusso di Passaggio temporaneo è &quot;Passaggio temporaneo&quot;, lo stesso ID MVPD viene restituito a **sendTrackingData()**. La maggior parte dei programmatori probabilmente vorrà trattare le metriche di Temp Pass in modo diverso rispetto alle metriche effettive di MVPD. Questo richiede un ulteriore lavoro nell’implementazione di Analytics.

L&#39;illustrazione seguente mostra il flusso Passata temporanea:

![Flusso passaggio temporaneo](../../../assets/temp-pass-flow.png)

*Figura: Flusso passaggio temporaneo*

## Implementare Temp Pass {#implement-tempass}

Sul lato Autenticazione di Adobe Pass, Temp Pass è implementato con l&#39;aggiunta di uno pseudo-MVPD denominato &quot;TempPass&quot; alla configurazione del server del programmatore partecipante.  Questo pseudo-MVPD agisce come un MVPD effettivo che concede temporaneamente l&#39;accesso al contenuto protetto del programmatore.

Sul lato programmatore, Temp Pass viene implementato come segue per i due scenari utilizzati dagli MVPD per l&#39;autenticazione:

* **iFrame sulla pagina del programmatore**. Il passaggio temporaneo funziona indipendentemente dal tipo di autenticazione di un MVPD, ma per lo scenario iFrame sono necessari ulteriori passaggi per annullare il flusso di autenticazione corrente e autenticare con il passaggio temporaneo. Questi passaggi sono illustrati nel [accesso iFrame](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) di seguito.
* **Reindirizza alla pagina di accesso di MVPD**. Nel caso più tradizionale in cui l’interfaccia utente per l’attivazione del passaggio Temp viene presentata prima di avviare l’autenticazione con un MVPD, non è necessario eseguire alcuna procedura speciale. Temp Pass deve essere trattato come un normale MVPD.

I punti seguenti si applicano a entrambi gli scenari di implementazione:

* Il &quot;Passaggio temporaneo&quot; deve essere visualizzato nel selettore MVPD solo per gli utenti che non hanno ancora richiesto un&#39;autorizzazione Passaggio temporaneo. È possibile bloccare la visualizzazione per le richieste successive mantenendo un flag sui cookie. Questo funziona se l’utente non cancella la cache del browser. Se gli utenti cancellano le cache del browser, appare nuovamente &quot;Passaggio temporaneo&quot; nel selettore, e l&#39;utente potrà richiederlo nuovamente. L’accesso viene concesso solo se il tempo &quot;Passaggio temporaneo&quot; non è ancora scaduto.
* Quando un utente richiede l’accesso tramite Temp Pass, il server di autenticazione di Adobe Pass non eseguirà la consueta richiesta SAML (Security Assertion Markup Language) durante il processo di autenticazione. L’endpoint di autenticazione restituirà invece esito positivo ogni volta che viene richiamato mentre i token sono validi per il dispositivo.
* Alla scadenza di un passaggio temporaneo, l’utente non verrà più autenticato, perché nel flusso del passaggio temporaneo il token di autenticazione e il token di autorizzazione hanno la stessa data di scadenza. Per spiegare agli utenti che il loro Passaggio Temp è scaduto, i Programmatori devono recuperare il MVPD selezionato subito dopo aver chiamato `setRequestor()`, quindi chiamare `checkAuthentication()` come di consueto. Nel callback `setAuthenticationStatus()` è possibile verificare se lo stato di autenticazione è 0, in modo che se il MVPD selezionato è &quot;TempPass&quot;, sia possibile inviare agli utenti un messaggio che informi che la sessione di Temp Pass è scaduta.
* Se un utente elimina il token Passaggio temporaneo prima della scadenza, i successivi controlli di adesione genereranno un token con un valore TTL uguale al tempo rimanente.
* Se l’utente elimina il token Passaggio temporaneo dopo la scadenza, nei successivi controlli di adesione verrà restituito &quot;utente non autorizzato&quot;.

Vedi gli esempi in [Codice di esempio](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#tempass-sample-code) di seguito per esempi su come codificare i dettagli di implementazione descritti in questa sezione.

## Codice di esempio {#tempass-sample-code}

Le sezioni seguenti mostrano come chiamare l’API di autenticazione di Adobe Pass per implementare il flusso Temp Pass:

* [Esempio di accesso per iFrame](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#iframe-login-sample)
* [Esempio di accesso automatico](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#auto-login-sample)

### Esempio di accesso per iFrame {#iframe-login-sample}

Questo esempio mostra come implementare Temp Pass per il caso in cui i MVPD supportano l’integrazione iFrame:

```HTML
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript" src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var ae, ifrm, providersMenu, previousSelectedProvider;
        var tempassSelected = false;
 
        $(document).ready(function() {
            ifrm = $('#ifrm');
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {wmode: "transparent", allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded() {
            ae = $('#accessEnabler')[0];
            ae.setProviderDialogURL("none");
            ae.setRequestor("sample_requestor_Id");
            previousSelectedProvider = ae.getSelectedProvider(); 
            ae.checkAuthentication();
        }
 
        function createIFrame() {
            providersMenu.hide();
 
            // If the user already used TempPass once, hide the button
            if ($.cookie("TPSelected") == "1"){
                $('#tempassBtn').hide();
            }
            ifrm.show();
        }
 
        function displayProviderDialog(providers) {
            if (tempassSelected) {
                // Remember in a cookie that the user selected temp pass
                $.cookie("TPSelected", "1", {expires: 366, path: '/'});
 
                // Authenticate with temp pass
                ae.setSelectedProvider("TempPass");
            } else {
                $('#loginBtn').hide();
                providersMenu = $('<select></select>');
 
                providersMenu.change(function(event){
                    ae.setSelectedProvider(event.target.value);
                });
 
                $.each(providers, function(k, v) {
                    // Add the MVPDs to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if(v.ID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:v.ID}).text(v.displayName));                       
                    }
                });
                $('body').append(providersMenu);
            }
        }
 
        function setAuthenticationStatus(status, code) {
            loginBtn = $('#loginBtn');
            logoutBtn = $('#logoutBtn');
            console.log(previousSelectedProvider);
 
            if (status == 1) {
                $('#selectedProvider').text("Authenticated with " + ae.getSelectedProvider().MVPD + "   ");
                loginBtn.hide();
                logoutBtn.show();
 
                // Get authorization
                ae.getAuthorization("sample_requestor_Id");
            } else {
                // If selected provider is TempPass but the user is not authenticated,
                //   infer that the TempPass period has expired, so reset the MVPD selection
                if (previousSelectedProvider && previousSelectedProvider.MVPD == "TempPass") {
                    previousSelectedProvider = null;
                    ae.setSelectedProvider(null);
                    alert("Your Temp Pass has expired, please login with your regular cable provider!");
                }
                loginBtn.show();
                logoutBtn.hide();
            }
        }
 
        function selectTempPass() {
            ifrm.hide();
 
            // Signal the fact that the user selected temp pass
            tempassSelected = true;
 
            // Cancel the current authentication flow
            ae.setSelectedProvider(null);
 
            // Retry authentication
            ae.getAuthentication();
 
        }
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="ae.getAuthentication();">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
           style="display: none" onclick="ae.logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px; width: 400px; height: 400px; border: 2px solid red;">
        <button id="tempassBtn"
           onclick="selectTempPass();"
             style="float:left">Don't know your credentials? Click here to get a Temp Pass.
        </button>
        <button onclick="window.location.reload()" style="float:right">X</button>
        <br />
        <hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="90%" height="90%" frameborder="0"></iframe>
    </div>
    <br/>
    <div id="ae" style="display: none">
        <p>Loading Access Enabler...</p>
    </div>
</body>
</html>
```

#### Casi di utilizzo per l’accesso iFrame {#iframe-login-use-cases}

**Per richiedere un passaggio temporaneo per la prima volta:**

1. Un utente accede alla pagina del programmatore e fa clic sul collegamento di accesso.
1. Viene aperto il selettore MVPD e l’utente sceglie un MVPD dall’elenco.
1. Viene visualizzato l’iFrame di autenticazione. Questo iFrame contiene un collegamento &quot;Passaggio temporaneo&quot;.
1. L’utente fa clic su &quot;Passaggio temporaneo&quot;, quindi il programmatore aggiunge un flag a un cookie per impedire che l’utente visualizzi il collegamento &quot;Passaggio temporaneo&quot; nelle visite successive alla pagina.
1. La richiesta di autenticazione tramite Passaggio temporaneo raggiunge i server di autenticazione Adobe Pass e questi ultimi generano un token di autenticazione. Il TTL è uguale al periodo di tempo impostato dal programmatore per il passaggio della temp.
1. La richiesta di autorizzazione Passaggio temporaneo raggiunge i server di autenticazione di Adobe Pass.
1. I server di autenticazione di Adobe Pass estraggono gli ID del dispositivo e del richiedente dalla richiesta e li memorizzano nel database insieme all’ora di scadenza. Il tempo di scadenza viene calcolato come: tempo di richiesta Temp Pass iniziale più il TTL (specificato dal Programmatore).
1. I server di autenticazione di Adobe Pass generano un token di autorizzazione.
1. L’utente accede al contenuto protetto.

**Per richiedere nuovamente un passaggio temporaneo dopo che un utente che ritorna ha eliminato i cookie del browser:**

1. L’utente accede alla pagina del programmatore e fa clic sul collegamento di accesso.
1. Viene aperto il selettore MVPD e l’utente sceglie un MVPD dall’elenco.
1. Viene visualizzato l’iFrame di autenticazione. Questo iFrame contiene un collegamento &quot;Passaggio temporaneo&quot; (l&#39;utente ha eliminato il cookie originale, quindi il programmatore non sa se l&#39;utente ha già fatto clic sul collegamento &quot;Passaggio temporaneo&quot;).
1. L’utente fa di nuovo clic su &quot;Passaggio temporaneo&quot;, quindi il programmatore aggiunge nuovamente un flag a un cookie, per impedire che l’utente visualizzi il collegamento &quot;Passaggio temporaneo&quot; nelle visite successive alla pagina.
1. La richiesta di autenticazione tramite Passaggio temporaneo raggiunge i server di autenticazione di Adobe Pass, che generano un token di autenticazione. Il TTL è ora il tempo rimanente per il passaggio temporaneo (la differenza tra l’ora corrente e l’ora di scadenza associata all’ID dispositivo).
1. La richiesta di autorizzazione Passaggio temporaneo raggiunge i server di autenticazione di Adobe Pass.
1. I server di autenticazione di Adobe Pass estraggono gli ID del dispositivo e del richiedente dalla richiesta e li utilizzano per recuperare la scadenza dal database di autenticazione di Adobe Pass. L’ora corrente viene confrontata con l’ora di scadenza.
1. Se il valore Temp Pass dell’utente non è scaduto, i server di autenticazione di Adobe Pass generano un token di autorizzazione.
1. Se il Passaggio Temp dell’utente non è scaduto, l’utente sarà in grado di accedere al contenuto protetto.

### Esempio di accesso automatico {#auto-login-sample}

L’esempio seguente illustra un caso in cui un utente accede automaticamente con TempPass quando visita un sito. L&#39;utente può scegliere di accedere con un normale MVPD in qualsiasi momento e viene avvisato se TempPass è scaduto:

```HTML
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript"
             src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript"
             src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript"
             src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var REQUESTOR = "REF";
        var RESOURCE = "sample_requestor_Id";
        var selectedProvider = null;
        var mvpds;
        var hasTempPassMVPD = false;
 
        // Used to cache the mvpd picker
        var picker;
 
        $(document).ready(function() {
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded(){
            console.log("AccessEnabler loaded");
            ae = $('#accessEnabler')[0];
 
            // Make sure the default picker is disabled
            ae.setProviderDialogURL("none");
 
            ae.setRequestor(REQUESTOR);
            ae.checkAuthentication();
        }
 
        /**
         * Callback received as a result of setRequestor()
         *
         * @param xml object holding the configuration for the current REQUESTOR
         * including the MVPD list
         */
        function setConfig(config) {
            // Save the mvpd list
            var mvpdList = $.parseXML(config);
            mvpds = $(mvpdList).find('mvpd');
 
            // Create the picker only once and cache it
            if(!picker) {
                picker = $('<div id="mvpdPicker"/>');
 
                var providersMenu = $('<select id="mvpdList" multiple></select>');
 
                $.each(mvpds, function(k, v) {
                    var mvpdID = $(v).find("id").text();
                    var mvpdName = $(v).find("displayName").text();
 
                    // Add the mvpd's to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if (mvpdID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:mvpdID}).text(mvpdName));
                    } else {
                        hasTempPassMVPD = true;
                    }
                });
                picker.append(providersMenu);
                picker.append($('<br/>'));
                picker.append($('<input type="button" onclick="login()" value="login" />'));
                picker.append($('<input type="button" onclick="cancelPicker()" value="cancel" />'));                  
            }
 
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");             
            }
        }
 
        /**
         * Callback triggered for iFramed MVPD's
         */
        function createIFrame() {
            $('#mvpdPicker').remove();
            $('#ifrm').show();
        }
 
        /**
         * Hides the MVPD picker
         * when the user clicks "Cancel"
         */
        function cancelPicker() {
            $('#video').show();
            $('#mvpdPicker').remove();
            $('#loginBtn').show();
        }
 
        /**
         * Pops up the MVPD picker
         */
        function showPicker() {
            $('#video').hide();
            $('#loginBtn').hide();
            $('body').append(picker);
        }
 
        function logout() {
            $.removeCookie('tempPassUsed');
            ae.logout();
        }
 
        /**
         * Performs login with the selected MVPD
         */
        function login() {
            selectedProvider = $('#mvpdList').val()[0];
 
            // Make sure we clear out previously
            // selected. This is a must if we want to force
            // login with a real MVPD while still logged in with
            // TempPass, without doing an ae.logout()
            ae.setSelectedProvider(null);
            ae.getAuthentication();
        }

        /**
         * Callback triggered by AccessEnabler. This is usually
         * triggered in order to display the MVPD picker, but
         * since we already constructed, cached, and displayed the
         * picker, and the user already picked the MVPD, we don't need
         * to do anything here but state management
         */
        function displayProviderDialog() {
            // If the selected MVPD is TempPass
            // store this fact in a cookie,
            // otherwise clear it
            if (selectedProvider != 'TempPass') {
                $.removeCookie('tempPassUsed');
            } else {
                $.cookie("tempPassUsed", 1);
            }
 
            // Since the picker was already shown
            // and the user picked an MVPD,
            // just proceed to login
            ae.setSelectedProvider(selectedProvider);
        }
 
        function setAuthenticationStatus(status, code) {
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");
            } else if(status == 1) {
                selectedProvider = ae.getSelectedProvider().MVPD;
                $('#selectedProvider').text("Authenticated with " + selectedProvider + "   ");
 
                // If authenticated with TempPass
                // allow the user to login with
                // a real MVPD
                if (selectedProvider == "TempPass") {
                    $('#loginBtn').show();
                    $('#logoutBtn').hide();
                } else {
                    $('#loginBtn').hide();
                    $('#logoutBtn').show();
                }
 
                // Get authorization
                // Note: This is mandatory in order to "start" the temp pass countdown
                ae.checkAuthorization(RESOURCE);
            } else if(code != "Provider not Selected Error") {
                // Auto-authenticate with TempPass only if we infer
                // that TempPass has not expired, otherwise we
                // inform the user that TempPass has expired
                if ($.cookie('tempPassUsed') == 1) {
                   $('#selectedProvider').text("Your Temp Pass has expired, please log in with your cable provider!");
                   $('#logoutBtn').show();
                   showPicker();
                } else {
                    selectedProvider = 'TempPass';
                    ae.getAuthentication();
                }
            }
        }
 
        /**
         * Displays the picker as a result
         * of user action
         */
        function loginClicked() {
            $('#loginBtn').hide();
            showPicker();
        }
 
        /**
         * Callback triggered in case of authorization success
         */
        function setToken(token) {
            console.log(token);
            $('#video').html('<img src=">');
        }
 
        /**
         * Callback triggered in case of authz failure
         */
        function tokenRequestFailed(resource, status, message) {
            console.log(resource);
            $('#video').html('<p style="color: red">' + status + ': ' + message + '</p>');
        }
 
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="loginClicked()">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
        style="display: none" onclick="logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px;
         width: 400px; height: 400px; border: 2px solid red;">
        <button onclick="window.location.reload()" style="float:right">Close this window</button>
        <br /><hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="80%" height="80%" frameborder="0"></iframe>  
    </div>
    <br/>
 
    <div id="video"></div>
    <div id="ae" style="display: none"><p>Loading Access Enabler...</p></div>
</body>
</html>
```

## Utilizzare più passate temporanee {#use-mult-tempass}

Alcuni eventi richiedono un accesso graduale e gratuito ai contenuti, ad esempio un intervallo iniziale di accesso gratuito (ad esempio, 4 ore) seguito da accessi giornalieri gratuiti (ad esempio, 10 minuti per ogni giorno successivo).  Affinché un programmatore possa implementare questo scenario, è necessario organizzarlo con il proprio contatto di Adobe per configurare due MVPD temporanei per il programmatore.

Per questo scenario di esempio (una sessione iniziale gratuita di 4 ore, seguita da sessioni gratuite giornaliere di 10 minuti), l’Adobe configura un MVPD denominato TempPass1 con un valore Time-To-Live (TTL) di 4 ore e un TempPass2 con un valore TTL di 10 minuti per il periodo successivo.  Entrambi sono associati all&#39;ID richiedente del programmatore.

### Implementazione del programmatore {#mult-tempass-prog-impl}

Adobe Una volta configurate le due istanze di TempPass, le due MVPD aggiuntive (TempPass1 e TempPass2) verranno visualizzate nell’elenco MVPD del programmatore.  Per implementare più passaggi temporanei, il programmatore deve effettuare le seguenti operazioni:

1. Al momento della visita iniziale dell’utente al sito, effettua automaticamente l’accesso con TempPass1. Come punto di partenza per questa attività è possibile utilizzare l&#39;esempio di accesso automatico riportato sopra.
1. Quando rilevi che TempPass1 è scaduto, archivia il fatto (in un cookie/archivio locale) e presenta all’utente il selettore standard di MVPD. **Assicurarsi di escludere TempPass1 e TempPass2 dall&#39;elenco**.
1. Se la proprietà TempPass1 è scaduta, effettuare l&#39;accesso automatico a ogni giorno successivo con TempPass2.
1. Quando TempPass2 è scaduto, memorizza il fatto (in un cookie/archivio locale) per la giornata e presenta all’utente il selettore standard di MVPD. Di nuovo, assicurati di filtrare TempPass1 e TempPass2 da tale elenco.
1. Ogni nuovo giorno, alle 00:00, reimpostare tutti i passaggi temporanei per TempPass2, utilizzando [Reimposta API Web TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#reset-all-tempass).

>[!NOTE]
>**Nota di programmazione:** l&#39;autenticazione Adobe Pass non dispone di un meccanismo incorporato per arrestare il flusso libero dopo che sono trascorsi 10 minuti.  Spetta ai programmatori limitare l’accesso alla scadenza di TempPass2. A tal fine, i programmatori possono implementare nei loro siti/app una chiamata &quot;checkAuthorization&quot; ogni X minuti, dove X è il periodo di tempo che il programmatore determina ha senso per le loro app.

## Reimposta tutte le passate temporanee {#reset-all-tempass}

Alcune regole aziendali richiedono l’eliminazione regolare di Temp Pass o una reimpostazione ad hoc di tutte le Temp Pass rilasciate per un determinato ID richiedente e MVPD ID. Questa funzione supporta casi d’uso quali i seguenti:

* Un passaggio temporaneo giornaliero di 10 minuti (il passaggio temporaneo deve essere reimpostato all&#39;inizio della giornata)
* Un Temp Pass disponibile per tutti gli utenti durante le ultime notizie. (Il passaggio Temp deve essere reimpostato per tutti i dispositivi non appena iniziano le ultime notizie).
* Lo scenario Passaggio temporaneo multiplo che fornisce una combinazione di un periodo di visualizzazione iniziale di una certa durata, seguito da periodi giornalieri successivi di un&#39;altra durata.

Per reimpostare tutte le passate temporanee, l&#39;autenticazione Adobe Pass fornisce ai programmatori un&#39;API Web *public*:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset
```

>[!NOTE]
>L’URL precedente sostituisce l’API di ripristino precedente. La vecchia API di ripristino (v1) non è più supportata.

* **Protocollo:** HTTPS
* **Host:**
   * Versione - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Percorso:** /reset-tempass/v2/reset
* **Parametri query:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **Intestazioni:** ApiKey - 1232293681726481
* **Risposta:**
   * Operazione completata - HTTP 204
   * Errore:
      * HTTP 400 per una richiesta non corretta
      * HTTP 401 se ApiKey non è stato specificato
      * HTTP 403 se ApiKey non è valido

Ad esempio:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

## Client supportati {#supp-clients}

Supporto di Temp Pass e Reset Tool per piattaforma:

| Client di autenticazione Adobe Pass | Passaggio temporaneo | Strumento Ripristina |
|:--------------------------------------:|:---------:|:----------:|
| JS AccessEnabler | SÌ | SÌ |
| IOS client nativo | SÌ | SÌ |
| TvOS client nativo | SÌ | SÌ |
| Android client nativo | SÌ | SÌ |
| FireTV client nativo | SÌ | SÌ |
| API senza client | SÌ | SÌ |

## Limitazioni e problemi noti {#limitations}

Questa sezione descrive le limitazioni che si applicano all’implementazione corrente di Temp Pass.

**JavaScript SDK**: supporta la funzionalità di reimpostazione passaggio temporaneo dalla versione **3.X e successive**.

<!--For Customers migrating from the 2.X JavaScript AccessEnabler to the 3.X JavaScript AccessEnabler, see [AccessEnabler JS 2.x to JS 3.x migration guide](https://tve.helpdocsonline.com/accessenabler-js-to-js-migration-guide).-->
