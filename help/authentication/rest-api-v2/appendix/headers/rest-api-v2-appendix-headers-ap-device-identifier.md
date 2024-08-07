---
title: Intestazione - AP-Device-Identifier
description: REST API V2 - Intestazione - AP-Device-Identifier
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---


# Intestazione - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>AP-Device-Identifier</b> contiene l&#39;identificatore del dispositivo di streaming creato dall&#39;applicazione client.

## Sintassi {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;tipo&gt; &lt;identificatore&gt;</td>
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

## Direttive {#directives}

<b>&lt;tipo></b>

Tipo di identificatore del dispositivo.

È disponibile un solo tipo supportato, come illustrato di seguito.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Tipo</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>impronta digitale</td>
      <td>L’identificatore del dispositivo è costituito da un identificatore opaco ed è creato dall’applicazione client.</td>
   </tr>
</table>


<b>&lt;identificatore></b>

Il valore `Base64-encoded` dell&#39;identificatore del dispositivo.

## Esempio {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```
