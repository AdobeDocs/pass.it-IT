---
title: Consenti MVPD nella finestra di dialogo di selezione
description: Consenti MVPD nella finestra di dialogo di selezione
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 0%

---

# Consenti MVPD nella finestra di dialogo di selezione {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Problema {#issue}

Il programmatore potrebbe voler testare o verificare l&#39;esperienza utente delle nuove integrazioni MVPD prima di passare agli utenti finali.

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
