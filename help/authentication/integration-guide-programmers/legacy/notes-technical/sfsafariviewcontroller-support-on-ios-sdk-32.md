---
title: Supporto di SFSafariViewController in iOS SDK 3.2+
description: Supporto di SFSafariViewController in iOS SDK 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# (Legacy) Supporto di SFSafariViewController in iOS SDK 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

</br>


**A causa dei requisiti di sicurezza, alcune pagine di accesso di MVPD DEVONO essere presentate in un SFSafariViewController, anziché in visualizzazioni Web.**

Alcuni MVPD richiedono che le loro pagine di accesso siano presentate in un controllo browser sicuro come SFSafariViewController. Stanno bloccando attivamente le visualizzazioni web, quindi per potersi autenticare con loro dobbiamo usare SVC.

## Compatibilità {#compatiblity}

A partire da iOS SDK versione 3.1, AccessEnabler SDK visualizza automaticamente la pagina di accesso per specifici MVPD in un SFSafariViewController, in base alla configurazione del server.

La versione 3.1 di SDK presenta automaticamente SFSafariViewController dal controller della visualizzazione radice dell&#39;applicazione. Questo semplifica la gestione delle pagine di accesso per gli implementatori, ma in alcuni casi non è possibile presentare SFSafariViewController dal controller della vista radice a causa dell’implementazione specifica dell’app (come se fosse già visibile un controller modale).

In questi casi, la versione 3.2 introduce la possibilità per il programmatore di gestire manualmente l&#39;SVC.

## Gestione manuale della SVC {#manual-svc-management}

Per gestire manualmente SVC, l&#39;implementatore deve effettuare le seguenti operazioni:


1. chiamare **setOptions([&quot;handleSVC&quot;:true])** dopo l&#39;inizializzazione di AccessEnabler (assicurarsi che la chiamata venga eseguita prima dell&#39;inizio dell&#39;autenticazione). In questo modo, si abiliterà la gestione &quot;manuale&quot; della SVC; il SDK non presenterà automaticamente la SVC, ma, quando necessario     chiama **navigate(toUrl:*{url}* useSVC:true)**.

1. implementare il callback facoltativo **`navigateToUrl:useSVC:`** all&#39;interno dell&#39;implementazione è necessario creare un&#39;istanza svc utilizzando l&#39;istanza SFSafariViewController utilizzando l&#39;URL specificato e presentarla sullo schermo:

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Note:***

   - *È possibile personalizzare SFSafariViewController in qualsiasi modo. In iOS 11+, ad esempio, è possibile modificare l&#39;etichetta &quot;Fine&quot; in &quot;Annulla&quot;.*
   - *per poter ignorare l&#39;svc, è necessario un riferimento ad esso. Non crearlo nell&#39;ambito di **navigateToUrl:useSVC***
   - *usa il tuo controller di visualizzazione per &quot;myController&quot;*


1. Nell&#39;implementazione delegata dell&#39;applicazione di **application(\_app: UIApplication, open url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]) -\> Bool**, aggiungi il codice per chiudere svc. Dovresti avere già del codice che chiama **accessEnabler.handleExternalURL()**. Appena sotto aggiungi:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   Anche in questo caso, svc è un riferimento a SFSafariViewController creato al passaggio 2.


1. Implementare **safariViewControllerDidFinish(\_ controller: SFSafariViewController)** da **SFSafariViewControllerDelegate** per rilevare quando l&#39;utente ha annullato il svc utilizzando il pulsante &quot;Fine&quot;. In questa funzione, per informare SDK che l’autenticazione è stata annullata devi chiamare:

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
