---
title: Impedisci la visualizzazione di MVPD nella finestra di dialogo per selezione
description: Impedisci la visualizzazione di MVPD nella finestra di dialogo per selezione
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 0%
---
# (Legacy) Impedisci la visualizzazione degli MVPD nella finestra di dialogo per selezione

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Problema {#issue-prevent-mvpd-sel-dialog}

È necessario impedire che MVPD specifici (&quot;elenco Bloccati&quot;) vengano visualizzati nel selettore MVPD.


## Soluzione {#solution-prevent-mvpd-sel-dialog}

La soluzione consiste nell&#39;eseguire l&#39;inserimento in un elenco Bloccati quando viene chiamato `displayProviderDialog()`.

Ad esempio, se si desidera che CableCompany_1 e CableCompany_2 non vengano visualizzati all&#39;interno del selettore MVPD, è necessario eseguire una procedura simile a quella illustrata nell&#39;esempio seguente.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if (!isBlocklisted(currentMvpd.ID)) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isBlocklisted(mvpdID) {
    // Implement block-listing on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
} 
```
