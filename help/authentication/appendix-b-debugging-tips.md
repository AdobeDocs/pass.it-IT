---
title: Appendice B "Suggerimenti per il debug"
description: Appendice B "Suggerimenti per il debug"
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Appendice B: suggerimenti per il debug {#appendix-b-debugging-tips}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.


## Cancellazione dei dati temporanei {#clearing-temporary-data}

L’autenticazione di Adobe Pass memorizza dati temporanei come la cache del browser, la cache degli LSO e i cookie. La cancellazione dei dati temporanei è importante, per essere certi di ottenere un’area di lavoro pulita durante il test.

- [Cancellazione della cache e dei cookie del browser](#clearing-the-browser-cache-and-cookies)
- [Cancellazione cache LSO](#clearing-lsos-cache)


## Cancellazione della cache e dei cookie del browser {#clearing-the-browser-cache-and-cookies}

È affidabile dal browser, ma in Firefox: &quot;Tools&quot; -\> &quot;Clear Recent History...&quot; -\> On &quot;Time range to clear:&quot; seleziona &quot;Everything&quot;; and on &quot;Details&quot;: check the &quot;Cookies&quot; and &quot;Cache&quot; -\> Fai clic su &quot;Clear Now&quot; (Cancella ora).


## Cancellazione cache LSO {#clearing-lsos-cache}

Accedere a [Guida del Flash Player](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html).

Seleziona la ```entitlement.\*``` (a seconda di ciò che viene testato) e fare clic su &quot;Elimina sito Web&quot;.


## Strumenti di debug {#tools}

I tecnici dell’autenticazione di Adobe Pass utilizzano i seguenti strumenti di debug:

- Firebug - <http://www.getfirebug.com/>
- Flashbug (funziona con la versione di debug di Flash Player)
- Filtro - <http://www.fiddler2.com/fiddler2/>
- Charles <http://www.charlesproxy.com/>
- Wireshark <http://www.wireshark.org/>


<!--
## Related Information

- [Programmer Integration Guide](/help/authentication/programmer-integration-guide-overview.md)

- [Using Charles Proxy (Tech Note)](https://tve.zendesk.com/hc/en-us/articles/204962849-Using-Charles-Proxy)
-->
