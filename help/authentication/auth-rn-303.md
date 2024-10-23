---
title: Note sulla versione di Adobe Pass Authentication 3.0.3
description: Note sulla versione di Adobe Pass Authentication 3.0.3
source-git-commit: 2f5e511f774e1a2d8b8b60084844edfe27be6c76
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 3.0.3 {#pt-authn-303-rn}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-303}

* [Numero build](#build-number-303)
* [Panoramica sulla versione](#release-overview-303)

### Numero build {#build-number-2651}

Autenticazione Adobe Pass: adobe-pass-**3.0.3**
Data di rilascio: **10/29/2024 - 10/31/2024**

### Nuove funzioni {#new-features-303}

#### API REST v2 {#rest-apis}

##### Codice

* Miglioramenti REST API V2 (poiché la versione principale di Adobe Pass 3.0 ha fornito [REST API V2](./rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* Sono stati aggiunti `notBefore` e `notAfter` campi in `/sessions/{code}` per restituire informazioni sulla validità del codice di autenticazione.
* Identificazione della piattaforma migliorata.

##### Documentazione

* Per iniziare con la nuova API REST v2, consulta il documento [Panoramica API REST v2](./rest-api-v2/rest-api-v2-overview.md).

##### Strumenti

* Per provare la nuova API REST v2, fare riferimento alla nuova pagina di autenticazione di Adobe Pass dal sito Web [Adobe Developer](https://developer.adobe.com/adobe-pass).
