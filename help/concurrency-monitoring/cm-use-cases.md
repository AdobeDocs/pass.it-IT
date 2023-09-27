---
title: Casi d’uso
description: Casi d’uso nel monitoraggio della concorrenza.
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---


# Casi d’uso {#use-cases}

Il caso d’uso principale del servizio di conteggio dei flussi sta conteggiando il numero di flussi video simultanei guardati da un utente e fornisce una decisione relativa al suo utilizzo simultaneo per lo stesso ID account.

Per monitorare l&#39;utilizzo da parte dell&#39;abbonato, è necessario un servizio centralizzato che possa aggregare l&#39;attività dell&#39;utente indipendentemente dal fatto che avvenga sul sito web o sull&#39;applicazione del programmatore, sul portale dei contenuti MVPD o su una proprietà sindacata.

I principali casi d’uso supportati da questo servizio centralizzato dovrebbero essere:

1. Non appena un abbonato inizia a guardare un video, l’applicazione può **inizializzare una sessione di streaming** e inizia **attività di reporting** dati.
1. Nello stesso servizio centrale, un’altra istanza riceverà ***Decisioni CM*** - nel caso in cui l’applicazione disponga di uno o più criteri registrati nel servizio CM, il servizio risponderà con una decisione di accesso basata sull’attività corrente.


## Creazione di una sessione {#create-session}

Questa chiamata API consente al client di creare una nuova sessione CM quando l’utente preme il pulsante &quot;play&quot; per guardare alcuni contenuti. La risposta del server conterrà il nuovo URL del flusso (contenente l’ID del flusso) per mantenerlo attivo e l’ora in cui il flusso si interromperà. L’applicazione client deve segnalare l’attività tramite heartbeat. La chiamata di inizializzazione della sessione deve includere metadati sotto forma di coppie chiave/valore inviate come dati del modulo (o parametri della stringa di query). Inoltre, la risposta includerà un flag per indicare se la riproduzione è &quot;conforme ai criteri&quot;. In caso contrario, la riproduzione non è consentita.

## Attività di reporting {#reporting-activity}

Una volta creata una sessione, l’applicazione deve inviare heartbeat regolarmente affinché il flusso rimanga attivo. Inoltre, si consiglia che l’app client interrompa il flusso una volta che l’utente interrompe la riproduzione, in modo che il flusso non venga conteggiato come attivo fino al timeout.

La risposta della chiamata heartbeat può consentire all’applicazione client di continuare la riproduzione del video (quando è conforme ai criteri) o di interromperla. Se il flusso video non è conforme, l’applicazione client deve arrestarlo. La risposta fornisce informazioni affinché l’applicazione client visualizzi un messaggio di errore e/o le azioni disponibili per consentire all’utente di continuare la riproduzione.
