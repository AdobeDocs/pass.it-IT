---
title: Panoramica di JavaScript SDK
description: Panoramica di JavaScript SDK
exl-id: 8756c804-a4c1-4ee3-b2b9-be45f38bdf94
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---

# (Legacy) Panoramica di JavaScript SDK {#javascript-sdk-overview}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Introduzione

Adobe consiglia vivamente di migrare alla versione più recente di JS v4.x della libreria AccessEnabler.

L’integrazione Adobe Pass Authentication JavaScript offre ai programmatori una soluzione TV-Everywhere nel familiare ambiente di sviluppo delle applicazioni web JS. I componenti principali dell’integrazione sono l’applicazione di &quot;alto livello&quot; (interazione dell’utente, presentazione video) e la libreria AccessEnabler di &quot;basso livello&quot; fornita dall’Adobe, che fornisce l’accesso ai flussi di adesione e gestisce le comunicazioni con i server di autenticazione di Adobe Pass.

Nelle sezioni seguenti vengono fornite descrizioni ed esempi specifici dell&#39;integrazione di JavaScript AccessEnabler.

>[!IMPORTANT]
>
>Questo documento descrive l’implementazione per una soluzione web desktop. La libreria JavaScript non è supportata sulle piattaforme mobili (ad esempio, Safari su iOS e Chrome su Android). Utilizza i nostri SDK nativi per eseguire il targeting di una piattaforma mobile (iOS, Android, Windows).

## Creazione della finestra di dialogo Selezione MVPD {#creating-the-mvpd-selection-dialog}

Affinché un utente possa accedere al proprio MVPD e autenticarsi, la pagina o il lettore deve fornire all’utente un modo per identificare il proprio MVPD. Per lo sviluppo viene fornita una versione predefinita della finestra di dialogo di selezione di MVPD. Per l’utilizzo in produzione, devi implementare un selettore MVPD personalizzato.

Se sai già chi è il provider del cliente, puoi [impostare MVPD a livello di programmazione](/help/authentication/home.md), senza l&#39;interazione dell&#39;utente. La tecnica è la stessa, ma evita di richiamare la finestra di dialogo Selettore provider e di chiedere al cliente di selezionare il proprio MVPD.

## Visualizzazione del provider di servizi {#displaying-the-service-provider}

Nell&#39;esempio di codice riportato di seguito viene illustrato come individuare e visualizzare il provider di servizi per il cliente corrente:

**HTML** - Questa pagina aggiunge una sezione alla pagina in cui viene visualizzato il provider scelto dal cliente, se questi ha già effettuato l&#39;accesso:

```HTML
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
            "http://www.w3.org/TR/html4/loose.dtd"> 
    <html>
    <head>
        <title>Get the distributor that is used for the authentication</title>
        <script type="text/javascript" src="libs/jquery-1.4.4.min.js"></script>
        <script type="text/javascript" src="libs/swfobject.js"></script>
        <script type="text/javascript" src="scripts/sample6.js"></script>
    </head>
    <body>
        <div id="alternative">
        <a href="http://www.adobe.com/go/getflashplayer"> 
            <img src="http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif" 
                 alt="Get Adobe Flash player"/> </a>
        </div> 
        <div id="user_authenticated">Please wait while we verify your authentication status</div> 
        <div id="control">
        <button id="sign_in" style="display:none;">Sign in</button>
        <button id="sign_out" style="display:none;">Sign out</button>
        </div> 
        <div id="distributor"></div> 
        <div id="provider_selector" style="display:none;">
        <select id="provider_list"></select>
        <button id="login">Login</button>
        <button id="cancel">Cancel</button>
        </div> 
        <div id="video_player">This is a video player showing an image as a preview.  You are not yet authorized to see this content.  Make sure that you first sign in.
        </div> 
        <div id="get_authorization">
        <button id="request_access" style="display:none;">Request access</button>
        </div> 
    </body>
    </html>
```


**JavaScript** Se l&#39;utente ha già eseguito l&#39;accesso, questo file JavaScript esegue una query sull&#39;attivatore di accesso per il provider corrente e visualizza il risultato nella sezione della pagina riservata a tale utente. Implementa anche una finestra di dialogo del selettore MVPD:

```JS
    $(function() {
        embedAE();
    }); 
    function embedAE() {
        var flashvars = {};
        var params = {
            bgcolor: "#869ca7",
            quality: "high",
            allowscriptaccess: "always"
        };
        var attributes = {
            id: "ae_swf",
            align: "middle",
            name: "ae_swf"
        };
        swfobject.embedSWF(
                "https://entitlement.auth-staging.adobe.com/entitlement/AccessEnabler.swf",      
                "alternative", "1", "1", "10.1.53", "", flashvars, params, attributes);
    } 
    function getAE() {
        return document.getElementById("ae_swf");
    } 
    function swfLoaded() {
        var swf = getAE();
        initBindings();
        // don't load the default provider-selection dialog 
        swf.setProviderDialogURL("none");
        // identify yourself 
        swf.setRequestor("sample_requestor_Id");
        swf.checkAuthentication();
    } 
    // Define the button actions for the provider dialog
    function initBindings() {
        var provider_selector = $('#provider_selector');
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var login = $('#login');
        var cancel = $('#cancel');
        var request_access = $('#request_access'); 
        sign_out.click(function() {
            getAE().logout();
        });
        sign_in.click(function() {
            getAE().getAuthentication();
        }); 
        login.click(function() {
            var selected_mvpd_id = $("#provider_list option:selected").val();
            getAE().setSelectedProvider(selected_mvpd_id);
        }); 
        cancel.click(function() {
            getAE().setSelectedProvider(null);
            provider_selector.hide();
        }); 
        request_access.click(function(){
            getAE().getAuthorization("sample_requestor_Id");
        });
    }
    // Called if user is already authenticated; queries AE to find the current user's MVPD, 
    // Adds the result to the UI 
    function setDistributor() {
        var distributor = $("#distributor");
        var selectedProvider = getAE().getSelectedProvider();
        distributor.replaceWith('<div id="distributor">' +
                                'Courtesy of ' +
                                selectedProvider.MVPD +
                                '</div>');
    } 
    // Adjust the contents of the provider dialog, based on authentication status 
    // Adds the call to check and display the current distributor, if the user is already
    // logged in. 
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        var user_authenticated = $("#user_authenticated");
        var video_player = $("#video_player");
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var request_access = $('#request_access'); 
        if (isAuthenticated == 1) {
            setDistributor();
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are authenticated - if you wish you can sign out
                                           </div>');
            sign_out.show();
            sign_in.hide();
            video_player.replaceWith('<div id="video_player">Now you can ask for access.</div>');
            request_access.show();
        } else {
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are not authenticated please sign in
                                           </div>');
            sign_in.show();
            sign_out.hide();
        }
    } 
    // Allow user to choose a provider 
    function displayProviderDialog(providers) {
        var provider_list = $("#provider_list");
        var options = provider_list.attr('options');
        for (var index in providers) {
            options[index] = new Option(providers[index].displayName,
                                        providers[index].ID);
        }
        //select the first one by default 
        if (!$("#provider_list option:selected").length)
            $("#provider_list option[0]").attr('selected', 'selected'); 
        $('#provider_selector').show();
    } 
    // Handle the authorization response by sending token to video player 
    function setToken(requested_resource_id, token) {
        var video_player = $("#video_player");
        var request_access = $("#request_access");
        video_player.replaceWith('<div id="video_player">Now you are authorized to ' +
                                 requested_resource_id +
                                 ' content with ' + token + ' . </div>');
    }
```

## Disconnessione {#logout}

Chiamare `logout()` per avviare il processo di disconnessione. Questo metodo non accetta argomenti. Consente di disconnettere l&#39;utente corrente, cancellando tutte le informazioni di autenticazione e autorizzazione per tale utente ed eliminando tutti i token AuthN e AuthZ dal sistema locale.

In alcuni casi, il lettore non è responsabile della gestione dei loghi utente:



- **Quando la disconnessione viene avviata da un sito non integrato con l&#39;autenticazione di Adobe Pass.** In questo caso MVPD può richiamare il servizio di disconnessione singola dell&#39;autenticazione di Adobe Pass tramite un reindirizzamento del browser. La chiamata SLO tramite una chiamata backchannel non è attualmente supportata.

>[!NOTE]
>
>Se l&#39;utente lascia il computer inattivo per un tempo sufficiente a far scadere i token, può comunque tornare alla sessione e avviare correttamente la disconnessione. L’autenticazione di Adobe Pass assicura che tutti i token vengano eliminati e notifica al MVPD di eliminare anche la loro sessione.

Il seguente codice JavaScript illustra la disconnessione (deautenticazione) di un utente attualmente autenticato:

```JS
    [...]
    /*
     * @param isAuthenticated authentication status: 1 - Authenticated, 0 - not authenticated 
     * @param errorCode Any error that occurred when determining the authentication status.
     * An empty string if no error occurred.
     */
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        if (isAuthenticated == 1 ) {
            /* User is authenticated – we can logout / deauthenticate */
            accessEnablerObject.logout();
            /* Logs out the current user, clearing all authentication
             * and authorization information for that user. Deletes
             * all authN and authZ tokens from the user's system.
             */
        } else {
            /*
             * User is NOT authenticated – we do not need to logout;
             * Show the log-in image/button/message
             */
        }
    }
```

<!--
**Related Information**

- [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
- [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
- [JavaScript Code Samples](#javascript-code-samples)
- [Understanding Tokens](#understanding_tokens)
-->
