---
title: Informazioni sugli ambienti Adobe
description: Informazioni sugli ambienti Adobe
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: c6afb9b080ffe36344d7a3d658450e9be767be61
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Informazioni sugli ambienti Adobe {#understanding-the-adobe-environments}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

La documentazione ufficiale che descrive gli ambienti Adobe è disponibile [Configurazione dell&#39;ambiente e test in Pre-qual](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md):

L’Adobe Ambienti, riassunto in poche parole:

Adobe con due ambienti: **Pre-qualificazione** e **Versione**.

* Nell’ambiente di pre-qualificazione stiamo preparando la nuova build da rilasciare.

* La build della versione corrente si trova nell’ambiente di rilascio.

Ogni ambiente ha due profili: **staging** e **produzione**.

* Il profilo di staging si connette al server di staging MVPD
* Il profilo di produzione si connette al profilo di produzione MVPD.

Il motivo per cui disponiamo dei due profili è che sul profilo di staging stiamo preparando nuovi partner per la pubblicazione e desiderano testare il sistema con la prossima build (Pre-Qualificazione) o con la versione uno (più stabile).

Se un partner desidera testare la nuova versione, è necessario eseguire alcuni passaggi aggiuntivi. Vedi [Configurazione dell&#39;ambiente e test in Pre-qual](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

Seguendo i passaggi precedenti, si garantisce che la prossima versione verrà testata nell’ambiente di pre-qualifica.
