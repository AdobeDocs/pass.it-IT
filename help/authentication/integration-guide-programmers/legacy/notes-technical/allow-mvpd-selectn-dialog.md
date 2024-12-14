---
title: Consenti MVPD nella finestra di dialogo di selezione
description: Consenti MVPD nella finestra di dialogo di selezione
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# (Legacy) Consenti MVPD nella finestra di dialogo per selezione {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Problema {#issue}

Il programmatore potrebbe voler testare o verificare l’esperienza utente delle nuove integrazioni MVPD prima di passare agli utenti finali.

## Soluzione {#solution}

Nel callback `displayProviderDialog()`, l&#39;autenticazione Adobe Pass restituisce tutti gli MVPD integrati con il programmatore selezionato (ID richiedente). Ma il Programmatore può applicare un filtro all&#39;array di ritorno di MVPD e visualizzare solo quelli che sono in entrambi gli elenchi.

## Esempio {#example}

In questo esempio viene illustrato come visualizzare solo CableCompany_1 e CableCompany_2 all&#39;interno della finestra di dialogo del selettore MVPD e come non visualizzare CableCompany_NewIntegration.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if ( isAllowListed(currentMvpd.ID) ) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isAllowListed(mvpdID) {
    // Implement allowlisting on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
}
```

<!--
**Related Information**
* [Prevent MVPDs from appearing in the Selection Dialog](/help/authentication/prevent-mvpd-selectn-dialog.md)
* **Code Samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
