---
title: Ambienti dashboard TVE
description: Comprendi l’utilizzo e il funzionamento dei diversi ambienti nel dashboard TVE.
source-git-commit: 06c2e1e54515a2ec47722ba1f360467dadd1f73b
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Ambienti {#environments}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Il dashboard TVE fornisce diversi ambienti personalizzati per soddisfare scopi specifici all’interno dell’autenticazione di Adobe Pass. Esistono due ambienti principali:

* **Prequal**: l’ambiente di pre-qualificazione funge da campo di prova per la preparazione e il test di nuove build prima della distribuzione in produzione.

* **Versione**: l’ambiente di rilascio ospita le build finalizzate e testate per la produzione.

All’interno di ogni ambiente, sono disponibili due profili diversi:

* **Staging**: il profilo di staging si connette al server di staging di MVPD per il test e la convalida delle integrazioni prima della pubblicazione.

* **Produzione**: il profilo di produzione si collega al profilo di produzione dell’MVPD per le attività di produzione effettive.

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
1. Seleziona l’ambiente di staging o produzione richiesto da **Ambiente** menu a discesa nella parte superiore del pannello sinistro.

   ![Menu a discesa degli ambienti del dashboard TVE](assets/tve-dashboard-env.png)

   *Menu a discesa dell’ambiente del dashboard TVE di autenticazione di Adobe Pass*

>[!NOTE]
>
> Le configurazioni possono variare in ogni ambiente in base alle impostazioni.

