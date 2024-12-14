---
title: SSO su iOS quando si utilizza Adobe Pass Authentication Access Enabler
description: SSO su iOS quando si utilizza Adobe Pass Authentication Access Enabler
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1144'
ht-degree: 0%

---

# (Legacy) SSO su iOS quando si utilizza Adobe Pass Authentication Access Enabler {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

</br>

## Panoramica

Il Single Sign-On (SSO) tra le app basate sull’autenticazione di Adobe Pass funziona in modi diversi a seconda del sistema operativo sottostante.

Questo documento si rivolge a **SSO su iOS** quando si utilizza l&#39;enabler di accesso **Adobe Pass Authentication**.

**Access Enabler** **1.10** è la versione più recente del SDK nativo di Adobe Pass Authentication iOS. Adobe consiglia vivamente di passare a questa versione, anziché utilizzare una versione precedente. Se utilizzi una versione precedente di Access Enabler, puoi scaricare la versione più recente [qui](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

L’SSO su iOS è determinato dalle seguenti condizioni:

- Le app devono utilizzare lo stesso **archivio token** (sotto forma di una bacheca personalizzata creata dall&#39;Access Enabler).
- Le app devono generare lo stesso **ID dispositivo** (Access Enabler calcola l&#39;ID dispositivo in base all&#39;indirizzo MAC o all&#39;IDFV, a seconda della versione del sistema operativo).

## Comportamento

Il comportamento SSO è il seguente:

- **iOS 6 e versioni precedenti**: SSO funziona automaticamente tra app sviluppate dallo stesso team o da team diversi. L’ID dispositivo viene calcolato in base all’indirizzo MAC (lo stesso valore viene prodotto in tutte le app) e l’area di archiviazione è comune a tutte le app (il tavolo di montaggio personalizzato è condivisibile tra le app in iOS 6 e versioni successive).
   - **Importante:** tieni presente che la versione di iOS SDK 1.9.4 ha [aumentato la destinazione minima della distribuzione iOS a iOS 7.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 e versioni successive**: SSO funzionerà nelle seguenti condizioni:

1. Le app vengono pubblicate utilizzando lo stesso profilo di distribuzione Apple o profili che appartengono allo stesso team. Questo è l&#39;unico modo in cui le app possono condividere bacheche personalizzate su iOS 7 e versioni successive. In tutti gli altri scenari, il tavolo di montaggio è in modalità sandbox per applicazione. Da [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html): \+\[`UIPasteboard pasteboardWithName:create:\`] e +\[`UIPasteboard pasteboardWithUniqueName`\] ora specifica il nome specificato per consentire solo alle app dello stesso gruppo di applicazioni di accedere alla tavola di montaggio. Se lo sviluppatore tenta di creare un tavolo di montaggio con un nome che esiste già e non fa parte della stessa suite di app, otterrà il proprio tavolo di montaggio univoco e privato. Tieni presente che questo non influisce sul sistema di pastboard fornito, generale e trova.

1. Le app hanno lo stesso prefisso dell’ID bundle (tutti i componenti tranne l’ultimo). Solo le applicazioni che condividono lo stesso prefisso Bundle ID calcoleranno lo stesso valore IDFV. Da [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor): In IOS 7, tutti i componenti del bundle ad eccezione dell&#39;ultimo componente vengono utilizzati per generare l&#39;ID fornitore. Se l’ID bundle ha un solo componente, viene utilizzato l’intero ID bundle.

Concentriamoci ora sullo scenario **&#39;iOS 7 e versioni successive&#39;**, in quanto è il più frequente per gli utenti reali:

Entrambe le condizioni (condivisione di un profilo dallo stesso team di sviluppo e presenza di un prefisso di identificatore bundle comune) sono condizioni obbligatorie per l’SSO.

Di seguito sono elencate le possibili combinazioni e i risultati ottenuti:

- **Profili dello stesso team e prefisso dello stesso ID bundle**: le app condivideranno lo stesso spazio di archiviazione e avranno lo stesso ID dispositivo (IDFV). Un utente dovrà effettuare l’autenticazione una sola volta (nella prima app utilizzata) e lo stato di autenticazione verrà condiviso tra tutte le altre app. Flusso di esempio:

1. L&#39;utente apre l&#39;app A (con ID bundle *com.x.y.AppA*) e non è autenticato
1. L’utente esegue l’autenticazione nell’app A
1. L&#39;utente apre l&#39;app B (con ID bundle *com.x.y.AppB*) e viene autenticato automaticamente condividendo i dati di adesione dall&#39;app
A (dal passaggio 2)
1. L’utente apre l’app A ed è ancora autenticato (dal passaggio 2)



- **Profili dello stesso team ma con prefissi di ID bundle diversi**: le app condivideranno lo stesso spazio di archiviazione del tavolo di montaggio ma avranno ID dispositivo (IDFV) diversi. Un utente dovrà effettuare l’autenticazione una volta per ogni app. Flusso di esempio:

1. L&#39;utente apre l&#39;app A (con ID bundle *com.x.y.AppA*) e non è autenticato
1. L’utente esegue l’autenticazione nell’app A
1. L&#39;utente apre l&#39;app B (con ID bundle *com.z.AppB*) e Access Enabler rileva il token ottenuto dalla prima app (perché lo storage è condiviso) ma non tenta di utilizzarlo tramite SSO a causa di ID dispositivo diversi
1. L’utente esegue l’autenticazione nell’app B
1. L’utente apre l’app A ed è ancora autenticato (dal passaggio 2)



- **Profili di team diversi (l&#39;ID bundle è irrilevante in questo scenario)**: le app avranno archivi diversi sulla bacheca e l&#39;SSO verrà disabilitato tra di loro. Un utente dovrà effettuare l’autenticazione una volta per ogni app e le sessioni di autenticazione rimarranno permanenti quando si passa da un’app all’altra. Flusso di esempio:


1. L’utente apre l’app A e non è autenticato
1. L’utente esegue l’autenticazione nell’app A
1. L’utente apre l’app B e non è autenticato
1. L’utente esegue l’autenticazione nell’app B
1. L’utente apre l’app A ed è autenticato (dal passaggio 2)
1. L’utente apre l’app B ed è autenticato (dal passaggio 4)

**Nota:** Tieni presente che le condizioni di cui sopra per l&#39;SSO sono applicabili quando si installano applicazioni tramite **Apple App Store**. Se le app sono distribuite su un simulatore (in cui non è applicabile la firma dell’app), installate con Xcode o distribuite tramite un profilo Ad Hoc, puoi ottenere risultati diversi.

**Importante:** Nota (**relativa ad AccessEnabler v1.8**): il secondo scenario descritto in precedenza (profili dello stesso team ma con prefissi di ID bundle diversi) creerà un&#39;esperienza utente molto sgradevole per gli utenti di **AccessEnabler v1.8** nelle applicazioni sviluppate dallo stesso team (media company). L’utente verrà disconnesso automaticamente durante la transizione tra le app della stessa media company, pertanto gli sviluppatori di applicazioni devono prestare attenzione quando decidono l’ID bundle e il profilo di distribuzione. Lo scenario esatto in questo caso è presentato di seguito:

Le app condivideranno lo stesso archivio in bacheca, ma avranno ID dispositivo diversi (IDFV, Device ID). Un utente dovrà eseguire l&#39;autenticazione una volta per ogni app, **ma le sessioni di autenticazione verranno cancellate quando si passa da un&#39;app all&#39;altra**. Flusso di esempio:

1. L&#39;utente apre l&#39;app A (con ID bundle *com.x.y.AppA*) e non è autenticato
1. L’utente esegue l’autenticazione nell’app A
1. L’utente apre l’app B (con ID bundle *com.z.AppB*) e i dati di adesione creati dall’app A vengono automaticamente cancellati dall’Access Enabler (meccanismo di sicurezza che rileva un conflitto tra l’ID dispositivo attualmente calcolato nell’app B e quello memorizzato nei token di adesione, creato dall’app A)
1. L’utente esegue l’autenticazione nell’app B
1. L’utente apre l’app A e i dati di adesione creati dall’app B vengono automaticamente cancellati dall’Access Enabler (meccanismo di sicurezza che rileva un conflitto tra l’ID dispositivo attualmente calcolato nell’app A e quello memorizzato nei token di adesione, creato dall’app B)
