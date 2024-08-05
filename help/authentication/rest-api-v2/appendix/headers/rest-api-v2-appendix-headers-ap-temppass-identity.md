---
title: Intestazione - AP-TempPass-Identity
description: REST API V2 - Intestazione - AP-TempPass-Identity
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 3%

---


# Intestazione - AP-TempPass-Identity {#header-ap-temppass-identity}

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>AP-TempPass-Identity</b> contiene le informazioni sull&#39;identità utente utilizzate per ottenere un TempPass promozionale.

## Sintassi {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-TempPass-Identity</b>: &lt;user_identity_information&gt;</td>
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

<b>&lt;informazioni_identità_utente></b>

Il valore `Base64-encoded` sulle informazioni di identità utente associate all&#39;utente finale a cui deve essere concesso un accesso temporaneo promozionale.

## Esempi {#examples}

```JSON
// Identity
// {"email": "example@domain.com"}

// Base64-encoded
// eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==

AP-TempPass-Identity: eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
