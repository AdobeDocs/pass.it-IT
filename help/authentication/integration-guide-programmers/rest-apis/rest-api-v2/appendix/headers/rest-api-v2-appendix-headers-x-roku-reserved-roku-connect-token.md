---
title: Intestazione - X-Roku-Reserved-Roku-Connect-Token
description: REST API V2 - Intestazione - X-Roku-Reserved-Roku-Connect-Token
exl-id: 21016d5b-4d10-4018-a82c-f2797b2d9fb9
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# Intestazione - X-Roku-Reserved-Roku-Connect-Token {#header-x-roku-reserved-roku-connect-token}

>[!NOTE]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>X-Roku-Reserved-Roku-Connect-Token</b> contiene l&#39;identificatore univoco della piattaforma come `JWS` o `JWE` ottenuto da un servizio o una libreria di identità in esecuzione al di fuori dei sistemi di autenticazione di Adobe Pass.

Questa intestazione è progettata per l’utilizzo in flussi abilitati per il Single Sign-On (SSO) utilizzando il metodo Platform Identity.

Per ulteriori dettagli sui flussi abilitati per il Single Sign-On (SSO) che sfruttano il metodo Platform Identity, fare riferimento alla documentazione [Single Sign-On utilizzando i flussi di identità della piattaforma](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Sintassi {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Roku-Reserved-Roku-Connect-Token</b>: &lt;identificatore_piattaforma_univoco&gt;</td>
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

* [Manuale Roku SSO (REST API V2)](../../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)
