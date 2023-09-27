---
title: Note sulla versione di Adobe Pass Concurrency Monitoring 2.6.0
description: Note sulla versione di Adobe Pass Concurrency Monitoring 2.6.0
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Note sulla versione di Adobe Pass Concurrency Monitoring 2.6.0 {#cm-260}


Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:



## Data di rilascio: 11/10/2016



## Nuove funzioni

Questa versione aggiunge la possibilità di terminare i flussi esistenti per consentire l’avvio del flusso corrente (ovvero il flusso di terminazione).



**Terminazione remota**

* In una risposta di conflitto 409, ogni sessione elencata nel campo &quot;conflitti&quot; del consiglio conterrà un attributo terminationCode.
* All’utente può essere richiesto l’elenco delle sessioni in conflitto e gli può essere consentito di scegliere quelle da terminare
* Le sessioni remote possono essere terminate solo passando un’intestazione di richiesta X-Terminate (con i codici di terminazione selezionati come valori) all’interno di un tentativo di sessione iniziale.
* È stato definito un nuovo tipo di &quot;consiglio&quot; per la risposta 410 Gone per indicare la sessione che ha ucciso quella corrente.


Per ulteriori informazioni, consulta la documentazione aggiornata.



>[!NOTE]
>
>La definizione della sessione utilizzata per l’elenco delle sessioni attive è stata aggiornata per includere il nome dell’applicazione e del dispositivo (anziché l’ID dell’applicazione).




## Correzioni di bug {#bug-fixes}

Sono state rimosse le intestazioni duplicate nella risposta del server (la correzione coinvolge sia le intestazioni CORS che la Data 1).




## Problemi noti {#known-issues}

N/D
