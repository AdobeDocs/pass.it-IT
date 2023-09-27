---
title: Debug dell’SDK iOS/tvOS di AccessEnabler tramite i registri dell’app della console
description: Debug dell’SDK iOS/tvOS di AccessEnabler tramite i registri dell’app della console
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 0%

---

# Debug dell’SDK iOS/tvOS di AccessEnabler tramite i registri dell’app della console {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.


## Panoramica

Lo scopo di questo documento è acquisire e presentare l’evoluzione del meccanismo di registrazione SDK iOS/tvOS di AccessEnabler, insieme ad alcuni dettagli utili per il debug del framework AccessEnabler tramite i registri dell’app della console.

## Stato meccanismo di registrazione

Lo scopo del meccanismo di registrazione di AccessEnabler iOS/tvOS è quello di generare messaggi utili per la risoluzione dei possibili problemi che un&#39;applicazione che utilizza il framework AccessEnabler potrebbe incontrare a causa di esso.

### AccessEnabler iOS/tvOS 3.5.0 e versioni successive

A partire dalla versione di AccessEnabler iOS/tvOS 3.5.0, il meccanismo di registrazione introduce i seguenti miglioramenti man mano che vengono apportate modifiche:

* Il framework AccessEnabler utilizza i protocolli consigliati di Apple [OSLog](https://developer.apple.com/documentation/os/oslog) implementazione.

* Il framework AccessEnabler introduce la possibilità di filtrare i registri dell’app della console in base al sottosistema: **com.adobe.pass.AccessEnabler**. Tutti i messaggi emessi dall’SDK fanno parte di com.adobe.pass.AccessEnabler.

* Il framework AccessEnabler introduce la possibilità di filtrare i registri dell’app Console in base a Qualsiasi (prefisso): **[AccessEnabler]**. Tutti i messaggi emessi dall&#39;SDK hanno il prefisso [AccessEnabler].

* Il framework AccessEnabler introduce la possibilità di filtrare i registri dell’app della console in base alla categoria: **debug**, **errore** insieme a uno dei due criteri precedenti: Subsystem o Any (prefisso).

## Debug tramite i registri app della console

A seconda dei problemi esaminati, potrebbe essere utile includere o escludere i messaggi di registrazione emessi dal framework AccessEnabler; di seguito sono riportati alcuni dettagli utili che possono essere utili durante le indagini e quando si utilizzano i registri dell’app della console.


### AccessEnabler iOS/tvOS 3.5.0 e versioni successive

#### Inclusione {#including}

Prima di tutto, per poter visualizzare i messaggi di registrazione emessi dal framework AccessEnabler, **deve** seleziona &quot;Include Info Messages&quot; (Includi messaggi di informazioni) e &quot;Include Debug Messages&quot; (Includi messaggi di debug) nella sezione Azione dell’app Console, come illustrato nell’immagine seguente.

![](assets/include-info-debug-msg.png)


Per eseguire il debug della funzionalità dell’SDK di AccessEnabler iOS/tvOS e **vedi** I registri del framework AccessEnabler consentono di effettuare le operazioni riportate di seguito.

* Cerca nell’app della console tramite **Sottosistema** che è uguale al valore com.adobe.pass.AccessEnabler come nell’immagine seguente.

![](assets/subsys-console-app.png)

* Cerca nell’app della console tramite **Qualsiasi** opzione che contiene
  [AccessEnabler] come nell’immagine seguente.

![](assets/any-optn-console-app.png)

Oltre ai due criteri di cui sopra, è possibile utilizzare anche **Categoria** opzione in combinazione con **Sottosistema** o **Qualsiasi (prefisso)** per cercare in modo esplicito **debug** o **errore** messaggi di livello emessi dall’SDK AccessEnabler iOS/tvOS.

#### Esclusione

Per eseguire il debug delle funzionalità di altri componenti e **escludi** I registri del framework AccessEnabler consentono di effettuare le operazioni riportate di seguito.

* Cerca nell’app della console tramite **Sottosistema** che non è uguale al valore com.adobe.pass.AccessEnabler.
* Cerca nell’app della console tramite **Qualsiasi** opzione che non contiene [AccessEnabler] valore.

## Segnalazione di un problema

Quando segnali un problema all’autenticazione di Adobe Pass, prendi in considerazione i seguenti suggerimenti:

* prova a fornire i passaggi di riproduzione.
* provare a fornire la/le versione/i del sistema operativo e il/i modello/i di dispositivo su cui si è verificato il problema.
* prova a fornire la versione dell’SDK AccessEnabler iOS/tvOS che presenta il problema.
* prova a acquisire e allegare tutti i messaggi di registrazione dell’SDK iOS/tvOS di AccessEnabler utilizzando una delle due opzioni presentate nella [Inclusione](#including) sezione.
