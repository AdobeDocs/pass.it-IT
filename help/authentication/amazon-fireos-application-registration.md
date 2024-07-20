---
title: Registrazione applicazione Amazon FireOS
description: Registrazione applicazione Amazon FireOS
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 0%

---

# Registrazione applicazione Amazon FireOS {#amazon-fireos-application-registration}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>

## Introduzione {#intro}

A partire dalla versione 3.0 dell’SDK di FireOS AccessEnabler, stiamo modificando il meccanismo di autenticazione con i server di Adobe. Invece di utilizzare una chiave pubblica e un sistema segreto per firmare l’ID richiedente, introduciamo il concetto di stringa di informativa software che può essere utilizzata per ottenere un token di accesso che viene successivamente utilizzato per tutte le chiamate dell’SDK ai nostri server. Oltre a una dichiarazione software, è necessario creare un collegamento profondo per l&#39;applicazione.

Per ulteriori informazioni, vedere [Registrazione client dinamica](/help/authentication/dynamic-client-registration.md)

## Che cos&#39;è una dichiarazione software? {#what}

Un rendiconto software è un token JWT che contiene informazioni sull’applicazione. Ogni applicazione deve disporre di una dichiarazione software univoca utilizzata dai nostri server per identificare l&#39;applicazione nel sistema Adobe. L&#39;istruzione software deve essere passata quando si inizializza l&#39;SDK di AccessEnabler e verrà utilizzata per registrare l&#39;applicazione con Adobe. Al momento della registrazione, l’SDK riceverà un ID client e un segreto client che verranno utilizzati per ottenere un token di accesso. Qualsiasi chiamata effettuata dall&#39;SDK ai nostri server richiederà un token di accesso valido. L’SDK è responsabile della registrazione dell’applicazione, del recupero e dell’aggiornamento del token di accesso.

**Nota:** le istruzioni software sono specifiche per l&#39;app e non è possibile utilizzare una singola istruzione software per più applicazioni. Tieni presente che questo vale anche per le applicazioni che offrono accesso a più canali.

## Come si ottiene una dichiarazione software? {#how-to}

### Se hai accesso al dashboard TVE di Adobe:

1. Apri il browser e passa a `https://console.auth.adobe.com`.

1. Passa alla sezione **[!UICONTROL Channels]**, quindi seleziona il tuo canale.

1. Passare alla scheda **[!UICONTROL Registered Applications]**.

1. Fare clic su **[!UICONTROL Add new application]**.

1. Specifica un nome e una versione per l’applicazione e seleziona le piattaforme su cui sarà disponibile (ad esempio Android).

1. Fornisci un **[!UICONTROL Domain Name]** scegliendo da un elenco di domini già configurati per il programmatore.

1. Invia le modifiche al server, quindi torna alla scheda **[!UICONTROL Registered Applications]** del canale.

   Dovresti visualizzare un elenco con tutte le applicazioni registrate.

1. Fai clic su **[!UICONTROL Download]** nell&#39;applicazione appena creata.

   Potrebbe essere necessario attendere alcuni minuti prima che l&#39;informativa software sia pronta per il download.

   Viene scaricato un file di testo. Utilizzarne il contenuto come informativa software.

Per ulteriori informazioni, vedere [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)

### Se non hai accesso al dashboard TVE di Adobe:

Invia un ticket a [tve-support@adobe.com](mailto:tve-support@adobe.com). Includere tutte le informazioni necessarie, inclusi il canale, il nome dell&#39;applicazione, la versione e le piattaforme. Verrà creata una dichiarazione software per l&#39;utente da parte del team di supporto.

## Come utilizzare l&#39;Informativa sul software {#use}

Dopo aver ottenuto l&#39;Informativa software, è necessario trasmetterla come parametro nel costruttore di Access Enabler. L&#39;Adobe consiglia di ospitare l&#39;Informativa sul software in una posizione remota. In questo modo, è possibile revocare e modificare facilmente l&#39;Informativa software senza rilasciare una nuova versione dell&#39;applicazione.

## Come utilizzare l&#39;Informativa sul software {#use-both}

Nel file di risorse dell&#39;applicazione `strings.xml` aggiungere il codice seguente:

```XML
<string name="software_statement">softwarestatement value</string>
```
