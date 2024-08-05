---
title: Intestazione - Adobe-Subject-Token
description: REST API V2 - Intestazione - Adobe-Subject-Token
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 1%

---


# Intestazione - Adobe-Subject-Token {#header-adobe-subject-token}

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>Adobe-Subject-Token</b> contiene l&#39;identificatore di piattaforma univoco `JWS` o `JWE` ottenuto da un servizio o una libreria di identità in esecuzione al di fuori dei sistemi di autenticazione di Adobe Pass.

Questa intestazione è progettata per l’utilizzo in flussi abilitati per il Single Sign-On (SSO) utilizzando il metodo Platform Identity.

Per ulteriori dettagli sui flussi abilitati per il Single Sign-On (SSO) che sfruttano il metodo Platform Identity, fare riferimento alla documentazione [Single Sign-On utilizzando i flussi di identità della piattaforma](../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Sintassi {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Token-Soggetto-Adobe</b>: &lt;unique_platform_identifier&gt;</td>
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

* [Amazon](../../../amazon-fireos-sso-using-clientless-api-cookbook.md)

## Esempi {#examples}

Consulta gli esempi come descritto per le seguenti piattaforme:

* [Amazon](../../../amazon-fireos-sso-using-clientless-api-cookbook.md)
