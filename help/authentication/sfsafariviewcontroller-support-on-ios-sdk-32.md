---
title: Supporto di SFSafariViewController sull'SDK iOS 3.2+
description: Supporto di SFSafariViewController sull'SDK iOS 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# Supporto di SFSafariViewController sull&#39;SDK iOS 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>


**A causa dei requisiti di sicurezza, alcune pagine di accesso di MVPD DEVONO essere presentate in un SFSafariViewController, invece che in visualizzazioni web.**

Alcuni MVPD richiedono che le loro pagine di accesso siano presentate in un controllo browser sicuro come SFSafariViewController. Stanno bloccando attivamente le visualizzazioni web, quindi per potersi autenticare con loro dobbiamo usare SVC.

## Compatibilità {#compatiblity}

A partire dalla versione 3.1 dell&#39;SDK per iOS, l&#39;SDK di AccessEnabler visualizza automaticamente la pagina di accesso per MVPD specifici in un SFSafariViewController, in base alla configurazione del server.

La versione 3.1 dell&#39;SDK presenta automaticamente SFSafariViewController dal controller della visualizzazione radice dell&#39;applicazione. Questo semplifica la gestione delle pagine di accesso per gli implementatori, ma in alcuni casi non è possibile presentare SFSafariViewController dal controller della vista radice a causa dell’implementazione specifica dell’app (come se fosse già visibile un controller modale).

In questi casi, la versione 3.2 introduce la possibilità per il programmatore di gestire manualmente l&#39;SVC.

## Gestione manuale della SVC {#manual-svc-management}

Per gestire manualmente SVC, l&#39;implementatore deve effettuare le seguenti operazioni:


1. chiamare **setOptions([&quot;handleSVC&quot;:true])** dopo l&#39;inizializzazione di AccessEnabler (assicurarsi che la chiamata venga eseguita prima dell&#39;inizio dell&#39;autenticazione). Questo attiverà la gestione &quot;manuale&quot; di SVC; l&#39;SDK non presenterà automaticamente SVC, ma, quando necessario, chiamerà **navigate(toUrl:*{url}* useSVC:true)**.

1. implementare il callback facoltativo **navigateToUrl:useSVC:** all’interno dell’implementazione è necessario creare un’istanza svc utilizzando l’istanza SFSafariViewController con l’URL fornito e presentarla sullo schermo:

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Note:***

   - *SFSafariViewController può essere personalizzato in qualsiasi modo. Ad esempio, in iOS 11+ puoi modificare l’etichetta &quot;Fine&quot; in &quot;Annulla&quot;.*
   - *per poter ignorare l&#39;svc, è necessario un riferimento ad esso. non crearlo nell&#39;ambito di **navigateToUrl:useSVC***
   - *utilizzare il proprio controller di visualizzazione per &quot;myController&quot;*


1. Nell’implementazione delegata dell’applicazione di **application(\_app: UIApplication, url aperto: URL, opzioni: \[UIApplicationOpenURLOptionsKey: Any\]) -\> Bool**, aggiungi il codice per chiudere il svc. Dovresti avere già un codice che chiama **accessEnabler.handleExternalURL()**. Appena sotto aggiungi:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   Anche in questo caso, svc è un riferimento a SFSafariViewController creato al passaggio 2.


1. Implementare **safariViewControllerDidFinish(\_ controller: SFSafariViewController)** da **SFSafariViewControllerDelegate** per rilevare quando l’utente ha annullato il svc utilizzando il pulsante &quot;Done&quot; (Fine). Con questa funzione, per informare l’SDK che l’autenticazione è stata annullata devi chiamare:

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
