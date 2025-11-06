---
title: Glossario di Dynamic Client Registration (DCR)
description: Glossario di Dynamic Client Registration (DCR)
exl-id: 4ce67fa5-b0e5-4967-b83d-c682426d9329
source-git-commit: ae02f53afc58b7d31f57bcc1e4dd1328f12abc3e
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Glossario di Dynamic Client Registration (DCR) {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questo documento fornisce le definizioni dei termini utilizzati durante l’integrazione di DCR (Dynamic Client Registration) di Adobe Pass Authentication.

>[!MORELIKETHIS]
> 
> * [Glossario REST API v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## Termini del glossario {#glossary-terms}

### A {#a}

#### Token di accesso {#access-token}

Il token di accesso è un token generato dall&#39;autenticazione di Adobe Pass come risultato del processo [Dynamic Client Registration (DCR)](#dcr) per garantire l&#39;accesso alle API protette.

### C {#c}

#### Credenziali client {#client-credentials}

Le credenziali client sono un insieme di valori univoci generati durante il processo [Dynamic Client Registration (DCR)](#dcr) e devono essere utilizzati per ottenere un [token di accesso](#access-token).

#### Schema personalizzato {#custom-scheme}

Lo schema personalizzato è un valore univoco che fa riferimento a un&#39;applicazione [Programmer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) che può essere generata e scaricata da Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) e deve essere utilizzata come reindirizzamento finale nelle applicazioni in esecuzione su dispositivi iOS.

### D {#d}

#### DCR {#dcr}

Dynamic Client Registration (DCR) è un meccanismo di autorizzazione definito da [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) e si basa sul framework di autorizzazione OAuth 2.0 descritto da [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

Il DCR viene consegnato a un [programmatore](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) come servizio di autenticazione di Adobe Pass che può abilitare ulteriormente l&#39;accesso alle API protette.

Per ulteriori informazioni, consulta la documentazione [Panoramica registrazione client dinamico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

### R {#r}

#### Applicazione registrata {#registered-application}

L&#39;applicazione registrata è un concetto di autenticazione Adobe Pass che memorizza informazioni sull&#39;applicazione [Programmer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) che richiede di procedere con il processo [DCR (Dynamic Client Registration)](#dcr).

### S {#s}

#### Dichiarazione software {#software-statement}

L&#39;istruzione software è un token Web JSON (JWT) che può essere scaricato da Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) e deve essere utilizzato come parte del processo [Dynamic Client Registration (DCR)](#dcr).
