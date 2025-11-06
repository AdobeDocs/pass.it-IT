---
title: Appendice B "Suggerimenti per il debug"
description: Appendice B "Suggerimenti per il debug"
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---

# (Legacy) Appendice B: suggerimenti di debug {#appendix-b-debugging-tips}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Cancellazione dei dati temporanei {#clearing-temporary-data}

L’autenticazione di Adobe Pass memorizza dati temporanei come la cache del browser, la cache degli LSO e i cookie. La cancellazione dei dati temporanei è importante, per essere certi di ottenere un’area di lavoro pulita durante il test.

- [Cancellazione della cache e dei cookie del browser](#clearing-the-browser-cache-and-cookies)
- [Cancellazione cache LSO](#clearing-lsos-cache)


## Cancellazione della cache e dei cookie del browser {#clearing-the-browser-cache-and-cookies}

È affidabile dal browser, ma in Firefox: &quot;Tools&quot; -\> &quot;Clear Recent History...&quot; -\> On &quot;Time range to clear:&quot; seleziona &quot;Everything&quot;; and on &quot;Details&quot;: check the &quot;Cookies&quot; and &quot;Cache&quot; -\> Fai clic su &quot;Clear Now&quot; (Cancella ora).


## Cancellazione cache LSO {#clearing-lsos-cache}

Accedere alla [Guida di Flash Player](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html).

Selezionare ```entitlement.\*``` (a seconda di ciò che è stato testato) e fare clic su &quot;Elimina sito Web&quot;.


## Strumenti di debug {#tools}

I tecnici dell’autenticazione di Adobe Pass utilizzano i seguenti strumenti di debug:

- Firebug - <http://www.getfirebug.com/>
- Flashbug (funziona con la versione di debug di Flash Player)
- Filtro - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>
