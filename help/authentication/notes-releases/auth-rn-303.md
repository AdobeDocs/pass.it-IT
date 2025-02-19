---
title: Note sulla versione di Adobe Pass Authentication 3.0.3
description: Note sulla versione di Adobe Pass Authentication 3.0.3
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Note sulla versione di Adobe Pass Authentication 3.0.3 {#authn-303-rn}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Questa pagina descrive nuove funzioni, modifiche e problemi noti relativi a questa versione:

## Client Web e lato server {#server-side-web-clients-303}

* [Numero build](#build-number-303)
* [Panoramica sulla versione](#release-overview-303)

### Numero build {#build-number-303}

Autenticazione Adobe Pass: adobe-pass-**3.0.3**

Data di rilascio: **10/29/2024 - 10/31/2024**

### Panoramica sulla versione {#release-overview-303}

#### API REST v2

##### Codice

* Miglioramenti REST API V2 (poiché la versione principale di Adobe Pass 3.0 ha fornito [REST API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* Sono stati aggiunti `notBefore` e `notAfter` campi in `/sessions/{code}` per restituire informazioni sulla validità del codice di autenticazione.
* Identificazione della piattaforma migliorata.

##### Documentazione

* Per iniziare con la nuova API REST v2, consulta il documento [Panoramica API REST v2](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

##### Strumenti

* Per provare la nuova API REST v2, fare riferimento alla nuova pagina di autenticazione di Adobe Pass dal sito Web [Adobe Developer](https://developer.adobe.com/adobe-pass).
