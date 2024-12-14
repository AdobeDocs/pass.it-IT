---
title: Domande e risposte sui certificati
description: Domande e risposte sui certificati
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# (Legacy) Domande e risposte sui certificati {#certificates-q}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

</br>

**Q1:** È possibile registrare certificati in iOS e Android?

**A:** Il certificato per iOS e Android è lo stesso nella configurazione corrente. Il certificato nativo viene utilizzato per entrambe le piattaforme.

</br>

**T2:** è possibile utilizzare gli stessi certificati iOS come certificati primari e di backup negli ambienti di produzione e di staging? Se non è consigliato, puoi fornire una spiegazione?

**A:** Non ha senso configurare lo stesso certificato come certificato primario e di backup. Il concetto di certificati primari e di backup consente di configurare più certificati per un programmatore in caso di scadenza o revoca del certificato primario. Disporre di un certificato di backup darà ai programmatori il tempo di modificare quello principale senza influire sull’ambiente di rilascio. È tuttavia possibile utilizzare lo stesso set di certificati primari e di backup per i profili di produzione e di gestione temporanea.

</br>

**Q3:** è necessario un nuovo certificato per le pagine Web che utilizzeranno il nuovo TempPass flessibile?

**A:** Il certificato (e qualsiasi certificato) è configurato a livello di Media Company &amp; Programmer. FlexibleTempPass è un MVPD, non è necessario configurare alcun certificato per esso, quindi se si sta integrando un programmatore esistente con TempPass flessibile, verrà utilizzato il certificato già configurato a livello di Programmatore / Media Company.
