---
title: Intestazione - AD-Service-Token
description: REST API V2 - Intestazione - AD-Service-Token
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 1%

---


# Intestazione - AD-Service-Token {#header-ad-service-token}

>[!NOTE]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>AD-Service-Token</b> contiene l&#39;identificatore utente univoco `JWS` ottenuto da un servizio Identity in esecuzione al di fuori dei sistemi di autenticazione di Adobe Pass.

Questa intestazione è progettata per l’utilizzo in flussi abilitati per il Single Sign-On (SSO) utilizzando il metodo Service Token.

Per ulteriori dettagli sui flussi abilitati per il Single Sign-On (SSO) che sfruttano il metodo Service Token, fare riferimento alla documentazione [Single Sign-On utilizzando i flussi di token di servizio](../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-service-token-flows.md).

## Sintassi {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Token-servizio-Active Directory</b>: &lt;identificatore_utente_univoco&gt;</td>
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

<b>identificatore_utente_univoco</b>

La firma Web JSON (`JWS`), che è un token Web JSON firmato (`JWT`) contenente informazioni univoche sull&#39;identificatore utente.

`JWT` ha i seguenti attributi:

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attributo</th>
      <th style="background-color: #EFF2F7;">Descrizione</th>
   </tr>
   <tr>
      <td>iss</td>
      <td>L’identificatore univoco associato all’entità che offre all’applicazione un servizio di identità esterna per ottenere il single sign-on (SSO).</td>
   </tr>
   <tr>
      <td>sub</td>
      <td>L’identificatore univoco dell’utente restituito dal servizio identità esterno.</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>Il pubblico, che dovrebbe essere "Adobe".</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>Il rilascio della marca temporale per il JWT attuale.</td>
   </tr>
   <tr>
      <td>exp</td>
      <td>Il timestamp di scadenza del JWT presente.</td>
   </tr>
</table>

`JWT` deve essere firmato utilizzando l&#39;algoritmo `SHA256withRSA`.

`JWT` deve essere firmato con una chiave privata, parte di una coppia di chiave privata RSA - chiave pubblica gestita dal servizio identità esterna.

La chiave pubblica di tale coppia deve essere consegnata all&#39;autenticazione di Adobe Pass per poter riconoscere `JWT` token firmati con la chiave privata di cui sopra.

## Esempi {#examples}

```JSON
// JWT
// Header
// {
//  "alg": "RS256",
//  "kid": "qapEaY0hYNvphytwII3Sae_cAKyLS7GZOqtT_a4ajeo"
// }
// Payload data
// {
//  "sub": "Jane",
//  "name": "Jane Smith",
//  "iat": 1516239022,
//  "iss": "adobe",
//  "exp": 1720152820,
//  "aud": "adobe",
//  "jti": "3b2fb040-30a9-43d7-b647-d00ac495bab"
// }
 
// JWS
// eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA

AD-Service-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA
```
