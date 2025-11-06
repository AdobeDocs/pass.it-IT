---
title: Intestazione - Authorization
description: REST API V2 - Intestazione - Autorizzazione
exl-id: 86917d7e-ffd9-4d34-8f9c-5a50083f85e6
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---


# Intestazione - Authorization {#header-authorization}

>[!NOTE]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>Authorization</b> contiene il token di accesso `Bearer` richiesto dall&#39;applicazione client per accedere alle API protette di Adobe Pass.

Per ulteriori dettagli sul meccanismo di accesso alle API protette di Adobe Pass, consulta la documentazione [Panoramica sulla registrazione dei client dinamici](../../../rest-api-dcr/dynamic-client-registration-overview.md).

## Sintassi {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Autorizzazione</b>: Bearer &lt;token_di_accesso&gt;</td>
   </tr>
   <tr>
      <td>Tipo di intestazione</td>
      <td>Intestazione richiesta</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Sì</td>
   </tr>
</table>

## Direttive {#directives}

<b>&lt;token_di_accesso></b>

Il valore del token di accesso è un valore opaco con durata limitata (ad esempio, 24 ore) che deve essere ottenuto da Adobe Pass come descritto nella documentazione dell&#39;API [Recupera token di accesso](../../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

## Esempi {#examples}

```JSON
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI0NmY0MGZiMy01NmJkLTQyYTktOTExYS02YmZmNmEyZmY0
                      MDciLCJuYmYiOjE3MjM1NjE4ODUsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpO
                      mNsaWVudDp2MiIsImV4cCI6MTcyMzU4MzQ4NSwiaWF0IjoxNzIzNTYxODg1fQ.aZUZqwN2fCqNXgX
                      SdKFG9_HcqHjha66G6HmsfTJYcZc12iuLxMu7TT7MbhWVz3kW1jRqgJv8PHhrFSBL5_dgJ1PRSuDg
                      97ZK1secgMKwk46vKZVdtx7LF5t3jGVzQTwN4RqChqyvkW2o67KxVk5xarwJtwB2fwhX_732CYDcv
                      1gWOTLx4xyH5IVvg-P_aImyveG0D-x65I2nOKXaROVvv-kYE6B9OQv_-JBGj72R_yS2AyJQC0R_im
                      0h5S4YvL-c2UZrYK7pvdZq-xAj0uW1wad7PLZjl8yL5CWUz9vzQk2Cmj8adsydjb0u0P3aFrJ0HE9
                      ISqtRvjf4plR1TGWgw6
```
