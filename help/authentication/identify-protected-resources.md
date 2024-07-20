---
title: Identificazione delle risorse protette
description: Identificazione delle risorse protette
exl-id: e96aea02-54b2-491d-ba91-253c0d0e681c
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# Identificazione delle risorse protette {#identifying-protected-resources}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

Ogni richiesta di autorizzazione (o richiesta di verifica dell’autorizzazione) deve contenere un identificatore univoco per la risorsa protetta di cui l’utente richiede l’accesso. Una risorsa protetta può essere qualsiasi livello di contenuto autorizzato, come concordato tra un MVPD e i programmatori partecipanti. Le potenziali risorse protette devono adattarsi a questa struttura ad albero di granularità sempre più specifica:

- Rete
   - Canale
      - Spettacolo
         - Episodio
            - Risorsa

</br>

## Formato Media RSS {#media_rss}

Le risorse possono essere identificate da una stringa semplice (un identificatore univoco per un canale) o possono essere rappresentate in formato Media RSS (MRSS), come concordato tra Adobe (o un partner autorizzato di autenticazione Adobe Pass) e gli MVPD e i programmatori partecipanti. La stringa RSS utilizzata come identificatore di risorsa può includere informazioni aggiuntive, ad esempio valutazioni e metadati per il controllo genitori.


Se utilizzi un identificatore di risorsa semplice, ad esempio &quot;TNT&quot;, si presume che rappresenti un canale e viene convertito in questo identificatore di risorsa RSS:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>TNT</title>
        </channel>
    </rss>
```


Un identificatore più complesso potrebbe includere, ad esempio, informazioni di rating aggiuntive. È possibile passare l&#39;intera stringa RSS alle funzioni di Access Enabler che richiedono un ID risorsa, ad esempio [`getAuthorization()`](/help/authentication/rest-api-reference.md):

```rss
    var resource = 
        '<rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
             <channel>
                 <title>TNT</title>
                 <media:rating scheme="urn:mpaa">pg</media:rating>
             </channel>
         </rss>'; 
    getAuthorization(resource);
```

Gli identificatori di risorse sono opachi all’autenticazione di Adobe Pass e vengono semplicemente trasmessi al MVPD. Se MVPD non riconosce o non è in grado di analizzare l&#39;identificatore di risorsa, restituisce un errore all&#39;autenticazione Adobe Pass, che restituisce l&#39;errore al callback `tokenRequestFailed()`.

<!--
## Related Information {#related}

-  User Metadata
-  Preflight Authorization
-->
