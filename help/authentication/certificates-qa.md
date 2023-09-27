---
title: Domande e risposte sui certificati
description: Domande e risposte sui certificati
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Domande e risposte sui certificati {#certificates-q}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>

**Q1:** È possibile registrare i certificati in iOS e Android?

**R:** Il certificato per iOS e Android è lo stesso nella configurazione corrente. Il certificato nativo viene utilizzato per entrambe le piattaforme.

</br>

**D2:** È possibile utilizzare gli stessi certificati iOS come certificati primari e di backup negli ambienti di produzione e di staging? Se non è consigliato, puoi fornire una spiegazione?

**R:** Non ha senso configurare lo stesso certificato come certificato primario e di backup. Il concetto di certificati primari e di backup consente di configurare più certificati per un programmatore in caso di scadenza o revoca del certificato primario. Disporre di un certificato di backup darà ai programmatori il tempo di modificare quello principale senza influire sull’ambiente di rilascio. È tuttavia possibile utilizzare lo stesso set di certificati primari e di backup per i profili di produzione e di gestione temporanea.

</br>

**D3:** È necessario un nuovo certificato per le pagine web che utilizzeranno il nuovo TempPass flessibile?

**R:** Il certificato (e qualsiasi certificato) è configurato a livello di Media Company &amp; Programmer. FlexibleTempPass è un MVPD, non è necessario configurare alcun certificato per esso, quindi se si sta integrando un programmatore esistente con TempPass flessibile, verrà utilizzato il certificato già configurato a livello di Programmatore / Media Company.
