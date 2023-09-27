---
title: Registrazione applicazione Android
description: Registrazione applicazione Android
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Registrazione applicazione Android {#android-application-registration}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Introduzione {#intro}

A partire dalla versione 3.0 dell’SDK per Android AccessEnabler, stiamo modificando il meccanismo di autenticazione con i server di Adobe. Invece di utilizzare una chiave pubblica e un sistema segreto per firmare l’ID richiedente, introduciamo il concetto di una stringa di istruzione Software che può essere utilizzata per ottenere un token di accesso che viene successivamente utilizzato per tutte le chiamate dell’SDK ai nostri server. Oltre a un&#39;informativa sul software, è necessario creare un collegamento profondo per l&#39;applicazione.

Per ulteriori informazioni, consulta [Registrazione client dinamici](/help/authentication/dynamic-client-registration.md)

## Che cos&#39;è una dichiarazione software? {#what}

Un rendiconto software è un token JWT che contiene informazioni sull’applicazione. Ogni applicazione deve disporre di una dichiarazione software univoca utilizzata dai nostri server per identificare l&#39;applicazione nel sistema Adobe. L&#39;istruzione software deve essere passata quando si inizializza l&#39;SDK di AccessEnabler e verrà utilizzata per registrare l&#39;applicazione con Adobe. Al momento della registrazione, l’SDK riceverà un ID client e un segreto client che verranno utilizzati per ottenere un token di accesso. Qualsiasi chiamata effettuata dall&#39;SDK ai nostri server richiederà un token di accesso valido. L’SDK è responsabile della registrazione dell’applicazione, del recupero e dell’aggiornamento del token di accesso.

>[!NOTE]
>
>Le istruzioni software sono specifiche dell&#39;app e non possono essere utilizzate per più di un&#39;applicazione. Si noti che le istruzioni software a livello di programmatore hanno lo stesso vincolo e possono essere utilizzate solo per una singola applicazione, sia a canale singolo che a più canali.

## Come si ottiene una dichiarazione software? {#how-to-get-ss}

### Se hai accesso al dashboard TVE di Adobe:

* Apri il browser e passa a [Dashboard TVE di Adobe Pass](https://console.auth.adobe.com).
* Accedi a `Channels` e selezionare il canale.
* Accedi a `Registered Applications` Tab.
* Fai clic su `Add new application`.
* Specifica un nome e una versione per l’applicazione e seleziona le piattaforme su cui sarà disponibile. Android nel nostro caso.
* Specifica un nome dominio scegliendo un dominio da un elenco già configurato per il programmatore.
* Invia le modifiche al server e quindi torna alla scheda Applicazioni registrate del canale.
* Dovresti visualizzare un elenco con tutte le applicazioni registrate. Seleziona la **Scarica** sull&#39;applicazione appena creata. Potrebbe essere necessario attendere alcuni minuti prima che il Software Statement sia pronto per il download.
* Verrà scaricato un file di testo. Utilizzarne il contenuto come informativa software.

Per ulteriori informazioni, consulta [Gestione registrazione client dinamici](/help/authentication/dynamic-client-registration-management.md)

### Se non hai accesso al dashboard TVE di Adobe:

Invia un ticket a `tve-support@adobe.com`. Includi tutte le informazioni necessarie come il canale, il nome dell’applicazione, la versione e le piattaforme e qualcuno del nostro team di supporto creerà un rendiconto software per te.

## Come utilizzare la dichiarazione software? {#how-to-use-ss}

Dopo aver ottenuto l&#39;Informativa sul software, è necessario trasmetterla come parametro nel costruttore di Access Enabler. Si consiglia di ospitare l&#39;Informativa sul software in una posizione remota. In questo modo, è possibile revocare e modificare facilmente l&#39;informativa sul software senza rilasciare una nuova versione dell&#39;applicazione.

## Creare e utilizzare un collegamento profondo per l&#39;applicazione {#create}

Su Android, utilizza come valore di collegamento profondo l’inverso del nome di dominio selezionato al momento della creazione dell’istruzione software

I collegamenti profondi creati devono avere un valore univoco sul dispositivo Android. Quando più applicazioni utilizzano lo stesso valore di collegamento profondo, i flussi di autenticazione e disconnessione interferiscono.

## Come utilizzare l&#39;Informativa sul software e il collegamento profondo {#use-both}

Nel file di risorse dell’applicazione `strings.xml` aggiungi il seguente codice:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
