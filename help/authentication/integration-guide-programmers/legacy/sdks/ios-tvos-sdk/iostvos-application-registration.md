---
title: Registrazione applicazione iOS/tvOS
description: Registrazione applicazione iOS/tvOS
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 0%

---


# (Legacy) Registrazione applicazione iOS/tvOS {#iostvos-application-registration}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#Intro}

A partire dalla versione 3.0 di iOS/tvOS AccessEnabler SDK, stiamo modificando il meccanismo di autenticazione con i server di Adobe. Invece di utilizzare una chiave pubblica e un sistema segreto per firmare l’ID richiedente, introduciamo il concetto di una stringa di istruzione software che può essere utilizzata per ottenere un token di accesso che viene successivamente utilizzato per tutte le chiamate effettuate da SDK ai nostri server. Oltre a un&#39;informativa sul software, è necessario anche uno schema URL personalizzato per l&#39;applicazione.

Per ulteriori informazioni, vedere [Panoramica sulla registrazione client dinamica](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Che cos&#39;è una dichiarazione software? {#Soft_state}

Un rendiconto software è un token JWT che contiene informazioni sull’applicazione. Ogni applicazione deve disporre di una dichiarazione software univoca utilizzata dai nostri server per identificare l’applicazione nel sistema Adobe. L&#39;istruzione software deve essere passata quando si inizializza AccessEnabler SDK e verrà utilizzata per registrare l&#39;applicazione con Adobe. Al momento della registrazione, SDK riceverà un ID client e un segreto client che verranno utilizzati per ottenere un token di accesso. Qualsiasi chiamata effettuata da SDK ai nostri server richiederà un token di accesso valido. SDK è responsabile della registrazione dell’applicazione, del recupero e dell’aggiornamento del token di accesso.

**Nota:** un&#39;istruzione software è specifica dell&#39;app e non può essere utilizzata in più applicazioni. Si noti che anche le istruzioni software a livello di programmatore seguono la stessa procedura, ovvero possono essere utilizzate solo per una singola applicazione, sia che si tratti di un singolo canale che di più canali. Questa limitazione si applica anche allo schema personalizzato.

## Come si ottiene una dichiarazione software? {#obtain}

### Se hai accesso a Adobe TVE Dashboard:

- Apri il browser e passa a <https://experience.adobe.com/#/pass/authentication>
- Passa alla sezione `Channels` e seleziona il tuo canale.
- Passa alla scheda `Registered Applications`.
- Fai clic su `Add new application`.
- Specifica un nome e una versione per l’applicazione e seleziona la   piattaforme su cui sarà disponibile. iOS/tvOS nel nostro caso.
- Invia le modifiche al server e quindi torna alla scheda Applicazioni registrate del tuo canale.
- Dovresti visualizzare un elenco con tutte le applicazioni registrate. Fai clic su   Pulsante `Download` dell&#39;applicazione appena creata. Potrebbe essere necessario attendere alcuni minuti prima che il Software Statement sia pronto per il download.
- Verrà scaricato un file di testo. Utilizza il contenuto come informativa sul software.

Per ulteriori informazioni, vedere [Dynamic Client Registration Management](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Se non hai accesso a Adobe TVE Dashboard:

Invia un ticket a <tve-support@adobe.com>. Includi tutte le informazioni necessarie come il canale, il nome dell’applicazione, la versione e le piattaforme e qualcuno del nostro team di supporto creerà un rendiconto software per te.

## Come utilizzare la dichiarazione software? {#use}

Dopo aver ottenuto l&#39;Informativa sul software, è necessario trasmetterla come parametro nel costruttore di Access Enabler. Si consiglia di ospitare l&#39;Informativa sul software in una posizione remota. In questo modo, è possibile revocare e modificare facilmente l&#39;informativa sul software senza rilasciare una nuova versione dell&#39;applicazione.

## Generazione di uno schema URL personalizzato per l’applicazione {#generating}

### Se hai accesso a Adobe TVE Dashboard:

- Apri il browser e passa a <https://experience.adobe.com/#/pass/authentication>
- Passa alla sezione `Channels` e seleziona il tuo canale.
- Passa alla scheda `Custom Schemes`.
- Fai clic su `Generate a new custom scheme`.
- Verrà generato un nuovo schema personalizzato per l&#39;applicazione. Esempio: `adbe.1JqxQsYhQOCIrwPjaooY8w://`
- Invia le modifiche al server.

### Se non hai accesso a Adobe TVE Dashboard:

Invia un ticket a <tve-support@adobe.com>. Includi l’ID canale: verrà creato uno schema personalizzato dal nostro team di supporto.

## Come utilizzare lo schema personalizzato {#use_custom}

Nel file `info.plist` dell&#39;applicazione aggiungere il codice seguente:

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>adbe.u-XFXJeTSDuJiIQs0HVRAg</string> // replace this with your custom scheme
            </array>
        </dict>
    </array>
```
