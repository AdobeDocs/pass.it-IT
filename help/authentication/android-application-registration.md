---
title: Registrazione applicazione Android
description: Registrazione applicazione Android
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 7107d4a915113fb237602143aafc350b776c55d6
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Registrazione applicazione Android {#android-application-registration}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#intro}

A partire dalla versione 3.0 dell’SDK di Android AccessEnabler, stiamo modificando il meccanismo di autenticazione con i server di Adobe. Invece di utilizzare una chiave pubblica e un sistema segreto per firmare l’ID richiedente, introduciamo il concetto di stringa di informativa software che può essere utilizzata per ottenere un token di accesso che viene successivamente utilizzato per tutte le chiamate dell’SDK ai nostri server. Oltre a una dichiarazione software, è necessario creare un collegamento profondo per l&#39;applicazione.

Per ulteriori informazioni, vedere [Panoramica sulla registrazione client dinamica](./dcr-api/dynamic-client-registration-overview.md).

## Che cos&#39;è una dichiarazione software? {#what}

Un rendiconto software è un token JWT che contiene informazioni sull’applicazione. Ogni applicazione deve disporre di una dichiarazione software univoca utilizzata dai nostri server per identificare l&#39;applicazione nel sistema Adobe.

L&#39;istruzione software deve essere passata quando si inizializza l&#39;SDK `AccessEnabler`. Viene utilizzato per registrare l’applicazione con Adobe. Al momento della registrazione, l’SDK riceve un ID client e un segreto client, utilizzati per ottenere un token di accesso. Qualsiasi chiamata effettuata dall&#39;SDK ai server Adobe richiede un token di accesso valido. L’SDK è responsabile della registrazione dell’applicazione, del recupero e dell’aggiornamento del token di accesso.

>[!NOTE]
>
>Le istruzioni software sono specifiche dell&#39;app e non possono essere utilizzate per più di un&#39;applicazione. Si noti che le istruzioni software a livello di programmatore hanno lo stesso vincolo e possono essere utilizzate solo per una singola applicazione, sia a canale singolo che a più canali.

## Come ottenere una dichiarazione software {#how-to-get-ss}

Di seguito sono riportati alcuni modi per ottenere una dichiarazione software.

### Se hai accesso al dashboard TVE di Adobe

1. Apri il browser e passa a [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication).

1. Passa alla sezione **[!UICONTROL Channels]**, quindi seleziona il tuo canale.

1. Passare alla scheda **[!UICONTROL Registered Applications]**.

1. Fare clic su **[!UICONTROL Add new application]**.

1. Assegna un nome all’applicazione e specifica una versione.

1. Seleziona le piattaforme su cui sarà disponibile l’applicazione (Android in questo caso).

1. Fornisci un **[!UICONTROL Domain Name]** scegliendo da un elenco di domini già configurati per il programmatore.

1. Invia le modifiche al server, quindi torna alla scheda **[!UICONTROL Registered Applications]** del canale.

   Dovresti visualizzare un elenco con tutte le applicazioni registrate. Selezionare **[!UICONTROL Download]** nell&#39;applicazione creata. Potrebbe essere necessario attendere alcuni minuti prima che il Software Statement sia pronto per il download.

   Viene scaricato un file di testo. Utilizzarne il contenuto come informativa software.

Per ulteriori informazioni, vedere [Dynamic Client Registration Management](./dcr-api/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Se non hai accesso alla dashboard TVE di Adobe

Invia un ticket a `tve-support@adobe.com`. Includi le informazioni necessarie come canale, nome dell’applicazione, versione e piattaforme. Qualcuno del nostro team di supporto creerà un rendiconto software per te.

## Come utilizzare l&#39;Informativa sul software {#how-to-use-ss}

Dopo aver ottenuto l&#39;Informativa software, è necessario trasmetterla come parametro nel costruttore di Access Enabler. Si consiglia di ospitare l&#39;Informativa sul software in una posizione remota. In questo modo, è possibile revocare e modificare facilmente l&#39;Informativa software senza rilasciare una nuova versione dell&#39;applicazione.

## Creare e utilizzare un collegamento profondo per l&#39;applicazione {#create}

In Android, utilizza come valore di collegamento profondo l’inverso del nome di dominio selezionato al momento della creazione dell’istruzione software

I collegamenti profondi creati devono avere un valore univoco sul dispositivo Android. Quando più applicazioni utilizzano lo stesso valore di collegamento profondo, i flussi di autenticazione e disconnessione interferiscono.

## Come utilizzare l&#39;Informativa sul software e il collegamento profondo {#use-both}

Nel file di risorse dell&#39;applicazione `strings.xml` aggiungere il codice seguente:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
