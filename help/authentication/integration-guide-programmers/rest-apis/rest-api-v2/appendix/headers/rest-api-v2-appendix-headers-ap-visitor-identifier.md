---
title: Intestazione - AP-Visitor-Identifier
description: REST API V2 - Intestazione - AP-Visitor-Identifier
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
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
