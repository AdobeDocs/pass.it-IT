---
title: Intestazione - AP-Partner-Framework-Status
description: REST API V2 - Intestazione - AP-Partner-Framework-Status
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---


# Intestazione - AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>AP-Partner-Framework-Status</b> contiene informazioni sullo stato ottenute da un framework partner per ottenere un Single Sign-On (SSO).

## Sintassi {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b>: &lt;partner_framework_status_information&gt;</td>
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

<b>&lt;informazioni_stato_framework_partner></b>

Il valore `Base64-encoded` dell&#39;elemento JSON contenente i seguenti attributi:

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attributo</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         Questo è un attributo obbligatorio.
         <br/><br/>
         Informazioni sullo stato delle autorizzazioni utente restituite dal framework partner ed elaborate dall'applicazione.
         <br/><br/>
         Questo è un elemento JSON con i seguenti attributi:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Attributo</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>accessStatus</td>
               <td>
                  Questo è un attributo obbligatorio.
                  <br/><br/>
                  Questa è un’enumerazione con i seguenti valori possibili:
                  <br/>
                  <ul>
                     <li>concesso: l’utente ha consentito all’applicazione di accedere alle informazioni di abbonamento.</li>
                     <li>negato: l'utente ha negato l'accesso alle informazioni sulla sottoscrizione.</li>
                     <li>in sospeso: l’utente non ha ancora scelto di consentire all’applicazione di accedere alle informazioni di abbonamento.</li>
                     <li>notDetermined - L’applicazione non è autorizzata ad accedere alle informazioni di abbonamento.</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>errore</td>
               <td>
                  Questo è un attributo facoltativo.
                  <br/><br/>
                  Questo può essere utilizzato per trasmettere l’errore del framework partner nel caso in cui ne venga attivato uno durante la query per le informazioni sullo stato delle autorizzazioni utente.
                  <br/><br/>
                  Questo è un elemento JSON con i seguenti attributi:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Attributo</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>codice</td>
                        <td>Stringa che identifica in modo univoco l’errore come definito dal framework del partner.</td>
                     </tr>
                     <tr>
                        <td>messaggio</td>
                        <td>Stringa che contiene la descrizione dell'errore come definito dal framework partner.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
   <tr>
      <td>frameworkProviderInfo</td>
      <td>
         Questo è un attributo obbligatorio.
         <br/><br/>
         Informazioni sullo stato di accesso del provider restituite dal framework del partner ed elaborate dall'applicazione.
         <br/><br/>
         Questo è un elemento JSON con i seguenti attributi:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Attributo</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  Questo è un attributo obbligatorio.
                  <br/><br/>
                  MappingId che identifica l'MVPD utilizzato durante il flusso di autenticazione a livello di framework del partner.
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  Questo è un attributo obbligatorio.
                  <br/><br/>
                  Si tratta della data di scadenza del profilo utente autenticato, nel caso in cui l’utente abbia effettuato correttamente l’accesso utilizzando un MVPD supportato a livello di framework del partner.
               </td>
            </tr>
            <tr>
               <td>errore</td>
               <td>
                  Questo è un attributo facoltativo.
                  <br/><br/>
                  Questo può essere utilizzato per trasmettere l’errore del framework del partner nel caso in cui uno venga attivato durante la query per le informazioni sullo stato di accesso del provider.
                  <br/><br/>
                  Questo è un elemento JSON con i seguenti attributi:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Attributo</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>codice</td>
                        <td>Stringa che identifica in modo univoco l’errore come definito dal framework del partner.</td>
                     </tr>
                     <tr>
                        <td>messaggio</td>
                        <td>Stringa che contiene la descrizione dell'errore come definito dal framework partner.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
</table>

## Esempi {#examples}

```JSON
// Partner framework status information
// {
//    "frameworkPermissionInfo": {
//        "accessStatus": "....",
//        "error": {
//            "code" : "....",
//            "message" : "...."
//        }
//     },
//    "frameworkProviderInfo" : {
//        "id" : "....",
//        "expirationDate" : "....",
//        "error" : {
//            "code" : "...",
//            "message" : "....."
//        }
//     }
// }  
 
// Base64-encoded
// ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAg
// ImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAg
// ICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAg
// ICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIs
// CiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
 
AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAgImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAgICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAgICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
```
