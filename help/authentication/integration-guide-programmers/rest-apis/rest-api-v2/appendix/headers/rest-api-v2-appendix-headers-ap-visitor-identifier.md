---
title: Intestazione - AP-Visitor-Identifier
description: REST API V2 - Intestazione - AP-Visitor-Identifier
exl-id: b4f8e2a1-9c7d-4e3a-8f5b-2d1c6e9a4b7f
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# Intestazione - AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>AP-Visitor-Identifier</b> contiene il `ECID` richiesto dall&#39;applicazione client per identificare in modo univoco un visitatore nelle soluzioni Adobe Experience Cloud.

Per ulteriori dettagli sull&#39;utilizzo di ECID nell&#39;autenticazione di Adobe Pass, consulta [Utilizzo di Experience Cloud ID nella documentazione di Adobe Pass Authentication](../../../../features-premium/analytics/exp-cloud-id-authn.md).

## Sintassi {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Visitor-Identifier</b>: &lt;identificatore_visitatore&gt;</td>
   </tr>
   <tr>
      <td>Tipo di intestazione</td>
      <td>Intestazione richiesta</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>No</td>
   </tr>
</table>

## Esempi {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
