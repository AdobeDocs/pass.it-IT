---
title: Punto di informazione sui criteri
description: Punto di informazione sui criteri
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---



# Punto di informazione sui criteri {#pip}

>[!NOTE]
>
>Questa pagina è obsoleta in quanto si applica alla versione precedente dell’API, che non è più consigliata per le nuove integrazioni

Il diagramma seguente mostra il flusso nel caso in cui il cliente opti per il **Punto di informazione sui criteri**, nel qual caso CM viene utilizzato semplicemente per eseguire una query sull’attività e tutta la logica di accesso è incorporata nell’applicazione client):

![](assets/pip-workflow.png)



Il diagramma seguente illustra il funzionamento del conteggio del flusso per un utente che guarda contenuti da 2 dispositivi.

![](assets/pip-sequence.png)

In breve, il flusso di messaggi abituale è il seguente:

1. Inizialmente, per un utente che non ha mai utilizzato il servizio, viene caricata una pagina Web / viene aperta un’applicazione, in cui un’applicazione strumentata del servizio di monitoraggio della concorrenza effettua una chiamata di inizializzazione della sessione.
1. Il servizio di monitoraggio della concorrenza restituisce la nuova risorsa di flusso per gli heartbeat, nonché l’attività utente corrente.
1. Durante la riproduzione di un video, l’applicazione instrumentata effettua chiamate heartbeat al servizio di monitoraggio della concorrenza, mostrando che l’utente sta attualmente utilizzando un video.
1. In qualsiasi altro momento, altre applicazioni instrumentate possono effettuare chiamate di query di stato al servizio di monitoraggio della concorrenza, che restituirà l&#39;attività utente corrente.
1. Al termine della riproduzione del video, l’applicazione dotata di strumentazione può effettuare una chiamata heartbeat con &quot;event=stop&quot;, che indica che il video è stato interrotto e che il flusso corrente non deve più essere conteggiato come flusso attivo.

