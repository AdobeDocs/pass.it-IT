---
title: Verifica preliminare, come abilitare, risolvere o determinare il problema
description: Verifica preliminare, come abilitare, risolvere o determinare il problema
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# Funzione Verifica preliminare: come abilitare, risolvere i problemi o determinare il problema {#preflight-feature}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Si è verificata una modifica nel modo in cui l’autenticazione Adobe Pass calcola le risorse preAuthorizeResources. L’API di pre-autorizzazione presenta una nuova implementazione. Questa implementazione sostituisce la vecchia soluzione, che prevede di effettuare solo più chiamate di autorizzazione.
L’interfaccia esterna per l’API di pre-autorizzazione non è stata modificata e non sono richiesti aggiornamenti nell’applicazione del programmatore.

Esistono tre modi per calcolare le risorse Verifica preliminare:

* **Fork e metodo join a MVPD**: questo comporta che un Adobe effettui più chiamate di autorizzazione all’MVPD (il cliente dovrà comunque effettuare una chiamata di verifica preliminare).
* **Line-up dei canali**: MVPD espone la linea di canali per l’utente connesso nella risposta di autenticazione SAML e l’Adobe restituisce le risorse autorizzate in base a tale risposta. La risposta SAML authN nel tracciatore SAML deve esporre tale elenco.
* **Autorizzazione multicanale**: l’autenticazione client e Adobe effettua una singola chiamata a MVPD per un set di risorse.

Indipendentemente da MVPD, l’applicazione client effettuerà una singola chiamata all’endpoint di verifica preliminare (checkPreauthorizedResources API), trasmettendo un set di resourceID. In base a uno dei modi precedenti supportati da MVPD, Adobe restituirà quindi gli ID risorsa preautorizzati.

Se la verifica preliminare è basata sul metodo fork &amp; join, il backend di autenticazione Adobe Pass controlla un valore impostato per il numero massimo di chiamate di preautorizzazione nella relativa configurazione. È configurato dall’Adobe.

Il valore predefinito per la configurazione &quot;chiamate di preautorizzazione massime&quot; è &quot;5&quot;, il che significa che al massimo è possibile inviare solo 5 risorse in verifica preliminare per gli MVPD di tipo fork e join. Il passaggio di più di 5 risorse genera un’eccezione e viene restituito un elenco nullo. Questo è il comportamento previsto. Possiamo configurarlo su qualsiasi valore se MVPD non supporta la derivazione dei canali o l’autorizzazione multicanale, ma solo dopo averli consultati come chiamate di autorizzazione multiple fork &amp; join aumenteranno i loro tempi di caricamento.

Di conseguenza, quando si abilita/si risolve il problema della verifica preliminare per un MVPD, è necessario eseguire le operazioni seguenti:

* Il metodo supportato da MVPD (fork &amp; join, line-up di canali o multicanale).
* Se è supportato solo il fork e il join, è necessario chiedere al programmatore quanti ID risorsa invierà nella chiamata di verifica preliminare.
* L’MVPD deve essere consultato e deve conoscere l’impatto di un numero &quot;n&quot; di chiamate di autorizzazione fork &amp; join. In seguito, il valore deve essere configurato nella configurazione se è maggiore di 5.

**Limitazione**

Tieni presente che non si ottiene alcun resourceID dalla chiamata di verifica preliminare per alcuni MVPD come AT&amp;T &amp; TWC se uno qualsiasi dei resourceID è un ID falso o un ID non riconosciuto nell’elenco dei resourceID inviati nella chiamata di verifica preliminare anche se abbiamo risorse valide e autorizzate anche in tale elenco.
