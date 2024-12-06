---
title: Migrazione della pagina di accesso MVPD da iFrame a Popup
description: Migrazione della pagina di accesso MVPD da iFrame a Popup
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '686'
ht-degree: 0%

---

# Migrazione della pagina di accesso MVPD da iFrame a Popup {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Popup e iFrame {#popup-vs-iframe}

Alcuni utenti hanno riscontrato problemi con i cookie di terze parti nell’implementazione iFrame di una pagina di accesso MVPD.
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

Il team di autenticazione di Adobe Pass **consiglia di implementare la pagina di accesso a comparsa o la nuova pagina di accesso alla finestra**, anziché la versione iFrame su Firefox e Safari.  Tuttavia, se implementi una pagina di accesso per Internet Explorer, potrebbero verificarsi problemi con l’implementazione della finestra a comparsa. I problemi di Internet Explorer sono causati dal fatto che, dopo che l’utente si autentica con il proprio MVPD nella finestra popup, l’autenticazione di Adobe Pass forza un reindirizzamento della pagina padre, che viene visto come un blocco popup da Internet Explorer. Il team di autenticazione di Adobe Pass **consiglia di implementare l&#39;accesso iFrame per Internet Explorer**.

Il codice di esempio presentato in questa nota tecnica utilizza un’implementazione ibrida sia di iFrame che di un elemento a comparsa, aprendo un iFrame su Internet Explorer e un elemento a comparsa sugli altri browser.

Considerando che esiste già un’implementazione iFrame, la prima parte della nota tecnica presenta il codice per l’implementazione iFrame e la seconda presenta le modifiche per adattarsi all’implementazione popup come impostazione predefinita.


## Selettore MVPD con pagina di accesso in un iFrame {#mvpd-pickr-iframe}

Gli esempi di codice precedenti mostravano una pagina HTML contenente il tag &lt;div> in cui deve essere creato l’iFrame insieme al pulsante chiudi iFrame:

```HTML
<body> 
    <div id="mvpddiv">
        <div style="background: red">
            <input type="button" id="btnCloseIframe" value="X" onclick="closeIframeAction();">
        </div>
        <br/>
        <!-- We use the "about:blank" value so that when the iFrame loads
             we do not see the the parent page. -->
        <iframe id="mvpdframe" name="mvpdframe" src="about:blank"></iframe>
    </div> 
</body>
```

Ecco il codice **JavaScript** associato:

```JavaScript
/*
 * Callback indicating that the AccessEnabler swf has initialized
 */
function swfLoaded() {
    // AccessEnabler is loaded so we can use the API function it provides
    accessEnablerObject.setRequestor(requestorID); 
    accessEnablerObject.checkAuthentication();
} 
/*
* The code the correctly closes the opened IFrame and does not leave the
* AccessEnabler in an undefined state.
*/
function closeIframeAction () {
    accessEnablerObject.setSelectedProvider(null);
    /* We use the "about:blank" value so that when the iFrame loads
     * we do not see the the previous MVPD.
     */
    document.getElementById('mvpdframe').src="about:blank";
    document.getElementById('mvpddiv').style.visibility="hidden";
    document.getElementById('mvpddiv').style.display="none";
}
/*
* Some of the supported MVPDs require their login pages to be displayed within an iFrame.
* In your HTML page, you must include the following <div> tag: "<div id="mvpddiv"></div>".
* Called when the selected MVPD requires an iFrame in which to display its authentication UI.
*/
function createIFrame(inWidth, inHeight) {
     // Create the iFrame to the specified width and height for the auth page
    ifrm = document.createElement("IFRAME");
    ifrm.name = "mvpdframe";
    ...
    document.getElementById('mvpddiv').appendChild(ifrm);
    ...
    // Force the name into the DOM since it is still not refreshed, for IE
    window.frames["mvpdframe"].name = "mvpdframe";
}
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
    /* This is an example how previously the MVPD picker was build and will be replaced. */
}
/* 
* Sending the user selected MVPD of the custom dialog is achieved
* by calling the API function setSelectedProvider(providerID:String).
*/
function setSelectedProvider(providerID) {
    accessEnablerObject.setSelectedProvider(providerID);
}
```


## Selettore MVPD con pagina di accesso in una finestra popup {#mvpd-pickr-popup}

Poiché non verrà più utilizzato un **iFrame**, il codice HTML non conterrà l&#39;iFrame o il pulsante per chiudere l&#39;iFrame. Il div che in precedenza conteneva l&#39;iFrame - **mvpddiv** - verrà mantenuto e utilizzato per i seguenti elementi:

* per notificare all&#39;utente che la pagina di accesso MVPD è già aperta se lo stato attivo della finestra a comparsa viene perso
* per fornire un collegamento che consenta di riattivare la finestra a comparsa

```HTML
<body onload="javascript:loadAccessEnabler();"> 
   <div id="aeHolder">
       <div name="contentAccessEnabler" id="contentAccessEnabler"></div>
   </div>
   <div id="mvpddiv">
      <p>
      <strong>No login window?</strong>
      <br/>
      <a href="javascript:mvpdWindow.focus();">Click here to open it.</a>
      <br/>
      Still not working? Check popup blockers!
      </p>
   </div> 
 
   <div id="picker" style="display:none">
      Choose MVPD: <br/>
      <select id="mvpdList" multiple></select>
      <br/>
         <a id="authenticateButton" onclick="javascript:authenticate();">Authenticate</a>
   </div>
</body>
```

L&#39;elenco di MVPD verrà visualizzato nel div denominato **selettore** come **-mvpdList** selezionato.

Verrà utilizzato un nuovo callback API - **setConfig(configXML)**. Il callback viene attivato dopo la chiamata della funzione setRequestor(requestorID). Questo callback restituisce l&#39;elenco di MVPD integrati con requestorID precedentemente impostato. Nel metodo di callback verrà analizzato il codice XML in ingresso e verrà memorizzato nella cache l&#39;elenco di MVPD. Viene creato anche il selettore MVPD, ma non visualizzato.

```JavaScript
var mvpdList;  // The list of cached MVPDs
var mvpdWindow;  // The reference to the popup with the MVPD login page
var cancelTimer = 0;
/* Callback indicating that the AccessEnabler swf has initialized */
function swfLoaded() {
   accessEnablerObject = $('#AccessEnabler').get(0);
   // Using a custom implementation of the MVPD picker
   accessEnablerObject.setProviderDialogURL('none');
   accessEnablerObject.setRequestor(requestorID); 
}

function setConfig(configXML) {
   mvpdList = [];
   $.each($($.parseXML(configXML)).find('mvpd'), function (idx, item) {
      var mvpdId = $(item).find('id').text();
      mvpdList[mvpdId] = {
         displayName: $(item).find('displayName').text(),
         logo: $(item).find('logoUrl').text(),
         popup: $(item).find('iFrameRequired').text() == "true",
         width: $(item).find('iFrameWidth').text(),
         height: $(item).find('iFrameHeight').text()
      };

      $('#mvpdList').append($('<option value="' + mvpdId + '" title="' + mvpdId + '">' + mvpdList[mvpdId].displayName + '</option>'));
   });
   accessEnablerObject.getAuthentication();
}
```

Dopo aver chiamato la funzione getAuthentication() o getAuthorization(), viene attivato il callback displayProviderDialog(). Normalmente, all’interno di questo callback, l’elenco MVPD sarebbe stato generato e visualizzato. Poiché il selettore MVPD è già stato creato, l’unica cosa che rimane da fare è visualizzarlo all’utente.

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

Dopo che l’utente ha selezionato un MVPD dal selettore, è necessario creare la finestra a comparsa. Alcuni browser possono bloccare la finestra a comparsa se viene creata con about:blank o con una pagina presente in un altro dominio, pertanto si consiglia di aprirla con il nome host da cui è caricato AccessEnabler.

Nell’implementazione di iFrame, il ripristino del flusso di autenticazione veniva eseguito dal pulsante btnCloseIframe e dalla funzione di JavaScript closeIframeAction(), ma ora la decorazione dell’iFrame non è più possibile. Quindi, lo stesso comportamento si ottiene osservando quando la finestra a comparsa viene chiusa (dall’utente o completando il flusso di autenticazione). È stato aggiunto uno snippet di codice che aiuta anche nel caso in cui l’utente perda lo stato attivo della finestra a comparsa:

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

Nel callback createIFrame() verrà visualizzato il div **mvpddiv**.

```JavaScript
function createIFrame(width, height) {
   $('#mvpddiv').show();
   if (useIframeLoginStyle) {
      ...
   }
}

/* Function called when user has selected the MVPD from the picker */
function authenticate() {
   var selectedMvpd = $('#mvpdList').val()[0];
   if (mvpdList[selectedMvpd].popup) {
      mvpdWindow = window.open("http://entitlement.auth-staging.adobe.com", "mvpdframe", "width=" + mvpdList[selectedMvpd].width + ",height=" + mvpdList[selectedMvpd].height, true);
      if (!mvpdWindow) {
         // Message to user that popup blocker is not allowing the authentication process to start
      }
      //watch the mvpd popup for close
      clearInterval(cancelTimer);
      cancelTimer = setInterval(checkClosed, 200);
   }
   accessEnablerObject.setSelectedProvider(selectedMvpd);
}

function checkClosed() {
   try {
      if (mvpdWindow && mvpdWindow["closed"]) {
         clearInterval(cancelTimer);
         accessEnablerObject.setSelectedProvider(null);
         accessEnablerObject.getAuthentication();
         $('#mvpddiv').hide();
      }
   } catch (error) {
      console.log(error);
   }
}
```

>[!IMPORTANT]
>
>* Il codice di esempio contiene una variabile hardcoded per il requestorID - &#39;REF&#39; utilizzato che deve essere sostituito da un vero ID richiedente programmatore.
>* Il codice di esempio verrà eseguito correttamente solo da un dominio inserito nella whitelist associato all’ID richiedente utilizzato.
>* Poiché l’intero codice è disponibile per il download, il codice presentato in questa nota tecnica è stato troncato. Per un esempio completo, vedere **Esempio di iFrame JS vs. esempio popup**.
>* Le librerie JavaScript esterne sono state collegate da [Google Hosted Services](https://developers.google.com/speed/libraries/).
