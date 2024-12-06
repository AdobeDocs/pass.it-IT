---
title: Ambienti dashboard TVE
description: Comprendi l’utilizzo e il funzionamento dei diversi ambienti nel dashboard TVE.
exl-id: 591becb8-2f6c-46e0-b108-c64e6df69f89
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Ambienti {#environments}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Il dashboard TVE fornisce diversi ambienti personalizzati per soddisfare scopi specifici all’interno dell’autenticazione di Adobe Pass. Esistono due ambienti principali:

* **Prequal**: l&#39;ambiente di pre-qualificazione funge da test per la preparazione e il test di nuove build prima della distribuzione in produzione.

* **Versione**: l&#39;ambiente di rilascio ospita le build finalizzate e testate per la produzione.

All’interno di ogni ambiente, sono disponibili due profili diversi:

* **Gestione temporanea**: il profilo di gestione temporanea si connette al server di gestione temporanea di MVPD per il test e la convalida delle integrazioni prima della pubblicazione.

* **Produzione**: il profilo di produzione si connette al profilo di produzione MVPD per le attività di produzione effettive.

## Casi d’uso

Gli ambienti nel dashboard TVE vengono utilizzati per vari casi d’uso durante l’intero ciclo di vita dell’applicazione. Questi ambienti consentono di:

### Staging Prequal

* Convalida le nuove funzioni non rilasciate del server di autenticazione di Adobe Pass utilizzando gli endpoint di staging di MVPD.
* Utilizzato principalmente dal team di prodotto per l’autenticazione di Adobe Pass per aggiungere e convalidare nuove integrazioni MVPD.

### Prequal Production

* Convalida nuove funzioni o configurazioni non rilasciate del server di autenticazione di Adobe Pass utilizzando gli endpoint di produzione di MVPD.
* Convalida le nuove versioni dell’applicazione per ogni canale utilizzando gli endpoint di produzione di MVPD.
* Convalida ogni modifica alla configurazione prima di inviarla alla produzione.

### Staging rilascio

* Convalida le nuove versioni dell’applicazione per ogni canale utilizzando gli endpoint di staging di MVPD.
* Eseguire test delle prestazioni o della capacità in questo ambiente.

### Rilascia produzione

* Rappresenta l’ambiente live con la versione più recente di Adobe Pass, generalmente disponibile per tutti gli utenti finali.
* Mantiene la stabilità del codice e della configurazione e riflette immediatamente le modifiche di configurazione nell’applicazione dell’utente finale.

## Cambiare ambienti {#switch-environments}

Segui i passaggi per passare da un ambiente dashboard TVE di autenticazione Adobe Pass all’altro.

1. Accedi con le credenziali del programmatore.

1. Seleziona l&#39;ambiente di staging o produzione richiesto dal menu a discesa **Ambiente** nella parte superiore del pannello a sinistra.

   ![Menu a discesa degli ambienti del dashboard TVE](../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

   *Menu a discesa dell&#39;ambiente del dashboard TVE di autenticazione di Adobe Pass*

>[!NOTE]
>
> Le configurazioni possono variare in ogni ambiente in base alle impostazioni.
