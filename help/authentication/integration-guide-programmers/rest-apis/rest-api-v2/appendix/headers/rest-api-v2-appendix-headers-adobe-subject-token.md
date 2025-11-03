---
title: Intestazione - Adobe-Subject-Token
description: REST API V2 - Intestazione - Adobe-Subject-Token
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 1%

---

# Intestazione - Adobe-Subject-Token {#header-adobe-subject-token}

>[!NOTE]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>Adobe-Subject-Token</b> contiene l&#39;identificatore di piattaforma univoco `JWS` o `JWE` ottenuto da un servizio o una libreria di identità in esecuzione al di fuori dei sistemi di autenticazione di Adobe Pass.

Questa intestazione è progettata per l’utilizzo in flussi abilitati per il Single Sign-On (SSO) utilizzando il metodo Platform Identity.

Per ulteriori dettagli sui flussi abilitati per il Single Sign-On (SSO) che sfruttano il metodo Platform Identity, fare riferimento alla documentazione [Single Sign-On utilizzando i flussi di identità della piattaforma](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Sintassi {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe-Subject-Token</b>: &lt;unique_platform_identifier&gt;</td>
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

<b>identificatore_piattaforma_univoco</b>

La firma Web JSON (`JWS`) o la crittografia Web JSON (`JWE`) che è un token Web JSON (`JWT`) firmato o crittografato contenente informazioni univoche sull&#39;identificatore della piattaforma.

Questa funzione è disponibile per le seguenti piattaforme:

* [Manuale Amazon SSO (REST API V2)](/help/premium-workflow/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## Esempi {#examples}

Consulta gli esempi come descritto per le seguenti piattaforme:

* [Manuale Amazon SSO (REST API V2)](/help/premium-workflow/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
