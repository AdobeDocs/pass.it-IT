---
title: Manuale Roku SSO (REST API V2)
description: Manuale Roku SSO (REST API V2)
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Manuale Roku SSO (REST API V2) {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

L’API REST per l’autenticazione di Adobe Pass V2 supporta l’SSO (Single Sign-On) per piattaforma per gli utenti finali delle applicazioni client in esecuzione su RokuOS.

Questo documento funge da estensione della [Panoramica REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) esistente che fornisce una visualizzazione di alto livello e il documento che descrive come implementare [Single Sign-On utilizzando i flussi di identità della piattaforma](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Single Sign-On Roku tramite i flussi di identità della piattaforma {#cookbook}

Adobe Pass Authentication collabora con Roku per migliorare l’esperienza di accesso degli utenti e facilitare il Single Sign-On (SSO) tra le applicazioni TV Everywhere per gli abbonati TV.

### Prerequisiti {#prerequisites}

Prima di procedere con il Single Sign-On Roku utilizzando i flussi di identità della piattaforma, accertati che Roku SSO sia abilitato. Roku SSO è abilitato per impostazione predefinita a meno che il programmatore o MVPD non richiedano la disabilitazione dell&#39;SSO.

Ogni programmatore può abilitare o disabilitare il Single Sign-On (SSO) sulla piattaforma Roku per integrazioni specifiche tramite [Adobe Pass TVE Dashboard](https://experience.adobe.com/pass/authentication).

### Flusso di lavoro {#workflow}

**Da client a server**

Per le applicazioni programmatrici che utilizzano un’architettura client-to-server per integrare REST API V2, Roku SSO funziona perfettamente senza alcuna modifica.

RokuOS aggiunge automaticamente due intestazioni HTTP a tutte le richieste inviate agli endpoint di autenticazione di Adobe Pass.

**Server-to-Server**

Per le applicazioni Programmer che utilizzano un’architettura server-to-server per integrare REST API V2, il Programmatore deve coordinarsi con il team Roku per configurare queste intestazioni in modo che vengano incluse in tutti i flussi API diretti al proprio dominio.

Per abilitare l’SSO tra applicazioni e dispositivi, è necessario utilizzare l’ID abbonato fornito da Roku al posto dell’ID dispositivo trasmesso dall’applicazione.

Per ulteriori informazioni, consulta la seguente documentazione:

* [Intestazione - X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [Intestazione - AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

Per informazioni specifiche sul formato delle intestazioni necessarie, contatta il tuo rappresentante Adobe.

### Domande frequenti {#faqs}

* **Funzionamento dell&#39;SSO**

  L’SSO funziona su tutte le applicazioni Programmer basate sull’autenticazione Adobe Pass su tutti i dispositivi Roku associati allo stesso utente Roku. Non tutti gli MVPD consentiranno Roku SSO.


* **Verranno apportate modifiche ai TTL di autenticazione?**

  Il primo token di autenticazione valido verrà utilizzato per l&#39;esecuzione dell&#39;SSO e, in questo caso, tutte le altre applicazioni che verranno autenticate tramite l&#39;SSO utilizzeranno lo stesso TTL fino alla scadenza. Pertanto, quando si passa da un’applicazione all’altra, la seconda applicazione condividerà il TTL della prima applicazione che si autentica.


* **Altre funzionalità di Adobe funzioneranno come prima?**

  Tutte le funzionalità di autenticazione di Adobe Pass funzioneranno come prima.


* **Esiste un processo di consenso/rinuncia del programmatore che trae vantaggio dall&#39;SSO sulla piattaforma Roku?**

  Si tratta di una modifica alla configurazione nel dashboard TVE di Adobe. Ogni programmatore può abilitare o disabilitare SSO sulla piattaforma Roku per integrazioni specifiche.


* **Quali sono alcuni problemi comuni?**

  I programmatori devono verificare che le loro implementazioni correnti basate sull’API REST di Adobe non impediscano l’SSO della piattaforma Roku.

  Di seguito è riportato un elenco di possibili problemi e come risolverli.

| Problema | Possibile causa | Soluzioni possibili |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Nessuna intestazione Roku SSO inviata ad Adobe | Utilizzo di HTTP invece di HTTPS per le chiamate ai domini di autenticazione Adobe Pass | Usa HTTPS |
| Logo MVPD non visualizzato/non aggiornato per i token SSO | L’interfaccia utente si basa sull’archiviazione locale | Le applicazioni devono aggiornare l’interfaccia utente (e l’archiviazione locale, se necessario) dopo aver verificato l’autenticazione |
| Disconnessione attivata su AuthZ | Progettazione dell’applicazione | L&#39;applicazione deve essere aggiornata in modo da non eseguire mai la disconnessione in background |
