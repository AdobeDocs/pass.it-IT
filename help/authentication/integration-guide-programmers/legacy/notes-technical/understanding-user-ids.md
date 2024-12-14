---
title: Informazioni sugli ID utente
description: Informazioni sugli ID utente
exl-id: 813a8501-db72-4850-a387-c8db6120db80
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 0%

---

# (Legacy) Informazioni sugli ID utente {#understanding-user-ids}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Concettualmente, ogni utente che avvia un flusso di adesione è associato a un singolo ID utente univoco. Tuttavia, attraverso il corso di un flusso di adesione, tale ID utente può essere presentato in modi diversi, a seconda dell’API da cui ottieni l’ID.

`sessionGUID` nel token multimediale breve è la forma sicura dell&#39;ID utente, disponibile tramite la chiamata `sendTrackingData()`. In tutte le integrazioni correnti, si tratta di un GUID persistente per l’utente nel tempo e nei dispositivi. L&#39;origine del GUID inizia con l&#39;ID utente della risposta di autenticazione proveniente da MVPD. Tuttavia, alcuni MVPD potrebbero cambiare idea in futuro e iniziare a inviare un GUID transitorio. Se un programmatore vuole assicurarsi che l’ID utente sorgente di MVPD nella risposta AuthN sia persistente, deve farlo nei suoi accordi con gli MVPD.

Di seguito sono riportati i diversi modi in cui l’ID utente viene rappresentato nelle API di autenticazione di Adobe Pass:

| Proprietà | Finalità | Hash | Firma digitale | Descrizione |
| --- | --- | --- | --- | --- |
| Proprietà GUID sendTrackingData() | Tracciamento/analisi | Sì | No | : ID utente di MVPD con hash eseguito da Adobe. L&#39;ID utente non può essere ricondotto all&#39;origine al MVPD. </br> </br> - Questo modulo dell&#39;ID non dispone di firma digitale, pertanto non è protetto per la prevenzione delle frodi. Tuttavia, è sufficiente per l’analisi.  </br> </br> - Questo modulo dell&#39;ID utente viene fornito lato client su tutti gli eventi generati dall&#39;autenticazione Adobe Pass nel flusso AuthN/AuthZ. |
| Proprietà sessionGUID del token multimediale breve | Tracciamento delle frodi relative all’utilizzo simultaneo | Sì | Sì | : corrisponde all’ID utente tramite sendTrackingData(), tuttavia è dotato di firma digitale per proteggerne l’integrità e può essere utilizzato per il monitoraggio delle frodi. </br> </br> - Deve essere elaborato sul lato server dopo l&#39;utilizzo della libreria di convalida e può essere analizzato per individuare i pattern di frode prima di rilasciare il flusso video al client.  È compito del programmatore eseguire una qualsiasi di queste operazioni. |
| getMetadata(), proprietà userID | Collegamento dell&#39;account, indagine sulle frodi con MVPD | No | No | : questa proprietà consente ad Adobe di esporre l’ID utente MVPD sorgente effettivo al programmatore. </br> </br> - Nella configurazione di Adobe può essere impostato come crittografato o meno (a seconda della preferenza MVPD). Se è crittografato, verrà crittografato con la chiave pubblica del certificato del programmatore fornito ad Adobe, in modo che non venga esposto in modo chiaro al client. </br> </br> - In questo modo il programmatore riceve l&#39;ID utente effettivo da MVPD, che può quindi essere utilizzato per il collegamento dell&#39;account o per indagini sulle frodi direttamente con MVPD. |


**In conclusione**

* In generale, MVPD fornisce un ID univoco persistente <u> e lo trasmette ad Adobe in caso di autenticazione riuscita</u>. Generalmente è coerente su tutte le reti. L’eccezione è Comcast MVPD, che fornisce un ID utente diverso per ciascun canale.

* L’ID utente di MVPD non contiene dati PII e NON è un numero di account. Non è necessario esporlo in forma crittografata poiché abbiamo verificato con tutti gli MVPD che non viene inviato alcun PII.

La modalità di utilizzo dell’ID utente dipende dal caso d’uso:

* Se ne hai bisogno per il tracciamento/analisi, il modo più pratico è ottenerlo da `sendTrackingData()`.
* Se ne hai bisogno sul lato server per la versione di flusso, la frode o i dati operativi, puoi ottenerli dalla convalida Media Token.
* Se ne hai bisogno per il collegamento dell’account e per frodi più profonde, verifica la disponibilità con il tuo contatto di Adobe.
