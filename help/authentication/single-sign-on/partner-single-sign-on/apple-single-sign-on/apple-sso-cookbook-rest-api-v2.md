---
title: Manuale Apple SSO (REST API V2)
description: Manuale Apple SSO (REST API V2)
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: dbf68d75962e3e34f0c569c409f8c98ae6b9e036
workflow-type: tm+mt
source-wordcount: '3442'
ht-degree: 0%

---

# Manuale Apple SSO (REST API V2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

L’API REST per l’autenticazione di Adobe Pass V2 supporta l’SSO (Single Sign-On) per i partner per gli utenti finali delle applicazioni client in esecuzione su iOS, iPadOS o tvOS.

Questo documento funge da estensione della [Panoramica API REST V2](/help/authentication/rest-api-v2/rest-api-v2-overview.md) esistente che fornisce una visualizzazione di alto livello e il documento che descrive come implementare [Single Sign-On utilizzando i flussi dei partner](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).

## Single Sign-On di Apple tramite i flussi dei partner {#cookbook}

### Prerequisiti {#prerequisites}

Prima di procedere con il Single Sign-On Apple utilizzando i flussi di partner, verifica che siano soddisfatti i seguenti prerequisiti:

* L&#39;applicazione di streaming deve raccogliere tutti i dati necessari richiesti dalle intestazioni `X-Device-Info` e/o `User-Agent` in modo che il backend di autenticazione di Adobe Pass possa identificare la piattaforma del dispositivo e le relative funzionalità. Per ulteriori dettagli sull&#39;intestazione `X-Device-Info`, consulta la documentazione [X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md).

* L’applicazione di streaming deve richiedere l’accesso alle informazioni di abbonamento dell’utente salvate a livello di dispositivo, per le quali l’utente deve concedere l’autorizzazione all’applicazione per procedere, in modo analogo a fornire l’accesso alla fotocamera o al microfono del dispositivo. Questa autorizzazione deve essere richiesta per applicazione utilizzando il framework dell&#39;account dell&#39;utente con sottoscrizione video [di Apple](https://developer.apple.com/documentation/videosubscriberaccount) e il dispositivo salverà la selezione dell&#39;utente.

  È consigliabile incentivare gli utenti che rifiutano di concedere l&#39;autorizzazione per l&#39;accesso alle informazioni sull&#39;abbonamento illustrando i vantaggi dell&#39;esperienza utente Single Sign-On di Apple, ma è bene tenere presente che l&#39;utente può modificare la propria decisione accedendo alle impostazioni dell&#39;applicazione (autorizzazione di accesso del provider TV) o a *`Settings -> TV Provider`* su iOS e iPadOS o a *`Settings -> Accounts -> TV Provider`* su tvOS.

  L&#39;applicazione di streaming può richiedere l&#39;autorizzazione dell&#39;utente quando l&#39;applicazione entra in primo piano, perché l&#39;applicazione può controllare [l&#39;autorizzazione per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di abbonamento dell&#39;utente in qualsiasi momento prima di richiedere l&#39;autenticazione dell&#39;utente.

>[!IMPORTANT]
>
> Presupposti
>
> <br/>
>
> * L&#39;applicazione di streaming ha completato i [prerequisiti per l&#39;onboarding](/help/authentication/single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-overview.md#apple-sso-prerequisites-programmer) applicabili a un programmatore e necessari per abilitare l&#39;esperienza utente Single Sign-On di Apple.

### Flusso di lavoro {#workflow}

Segui i passaggi forniti per implementare il Single Sign-On Apple utilizzando i flussi dei partner, come illustrato nel diagramma seguente.

![Single Sign-On Apple tramite flussi di partner](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*Single Sign-On Apple tramite flussi di partner*

+++A. Fase di registrazione

1. **Recupera credenziali client:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare le credenziali client chiamando l&#39;endpoint Registro client.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recuperare le credenziali del client](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request), consulta la documentazione API di:
   >
   > * Tutti i parametri _required_, come `software_statement`
   > * Tutte le intestazioni _required_, come `Content-Type`, `X-Device-Info`
   > * Tutti i parametri e le intestazioni _optional_

1. **Restituisci credenziali client:** La risposta dell&#39;endpoint Registro client contiene informazioni sulle credenziali client associate ai parametri e alle intestazioni ricevuti.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta alle credenziali del client, consultare la documentazione API [Recupera credenziali client](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success).
   >
   > <br/>
   >
   > Il registro client convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione API [Retrieve client credentials](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error).

   >[!TIP]
   >
   > Suggerimento: le credenziali del client devono essere memorizzate nella cache e possono essere utilizzate a tempo indefinito.

1. **Recupera token di accesso:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare il token di accesso chiamando l&#39;endpoint del token client.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recupera token di accesso](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#request), consulta la documentazione API di:
   >
   > * Tutti i parametri _required_, come `client_id`, `client_secret` e `grant_type`
   > * Tutte le intestazioni _required_, come `Content-Type`, `X-Device-Info`
   > * Tutti i parametri e le intestazioni _optional_

1. **Token di accesso restituito:** La risposta dell&#39;endpoint del token client contiene informazioni sul token di accesso associato ai parametri e alle intestazioni ricevuti.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta del token di accesso, consulta la documentazione API [Recupera token di accesso](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#success).
   >
   > <br/>
   >
   > Il token client convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione API [Recupera token di accesso](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#error).

   >[!TIP]
   >
   > Suggerimento: il token di accesso deve essere memorizzato in cache e utilizzato solo entro la durata specificata (ad esempio, time-to-live di 24 ore). Dopo la scadenza, l’applicazione di streaming deve richiedere un nuovo token di accesso.

+++

+++B. Verifica fase di autenticazione

1. **Recupera stato framework partner:** L&#39;applicazione di streaming chiama il [Framework account sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount) sviluppato da Apple per ottenere le autorizzazioni utente e le informazioni sul provider.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount), consulta:
   >
   > <br/>
   >
   > * L&#39;applicazione di streaming deve verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente e procedere solo se l&#39;utente lo ha consentito.
   > * L&#39;applicazione di streaming deve fornire un [delegato](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) per `VSAccountManager`.
   > * L&#39;applicazione di streaming deve inviare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per le informazioni sull&#39;account del sottoscrittore.
   > * L&#39;applicazione di streaming deve attendere ed elaborare le informazioni di [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L&#39;applicazione di streaming deve verificare di specificare un valore booleano uguale a `false` per la proprietà [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) nell&#39;oggetto `VSAccountMetadataRequest`, per indicare che l&#39;utente non può essere interrotto in questa fase.

1. **Restituisci informazioni sullo stato del framework partner:** L&#39;applicazione di streaming convalida i dati di risposta per verificare che siano soddisfatte le condizioni di base:
   * Lo stato di accesso dell’autorizzazione utente è concesso.
   * Identificatore di mapping del provider utente presente e valido.
   * La data di scadenza del profilo del provider utente (se disponibile) è valida.

1. **Recupera profili:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare tutte le informazioni sui profili inviando una richiesta all&#39;endpoint Profili.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recuperare i profili](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request), consulta la documentazione API di:
   >
   > * Tutti i parametri _required_, come `serviceProvider`
   > * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier` e `AP-Partner-Framework-Status`
   > * Tutti i parametri e le intestazioni _optional_
   >
   > <br/>
   >
   > L’applicazione di streaming deve garantire di includere un valore valido per lo stato del framework del partner in modo che la risposta recuperata possa includere un profilo di tipo &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Per ulteriori dettagli sull&#39;intestazione `AP-Partner-Framework-Status`, consulta la documentazione [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Informazioni restituite sui profili trovati:** La risposta dell&#39;endpoint Profiles contiene informazioni sui profili trovati associati ai parametri e alle intestazioni ricevuti.

1. **Scegli un profilo e procedi con i flussi di decisioni:** Se la risposta dell&#39;endpoint Profili contiene profili, l&#39;applicazione di streaming utilizza la propria logica interna (eventualmente interagendo con l&#39;utente finale) per scegliere uno dei profili disponibili per continuare con i flussi di decisioni successivi.

1. **Procedi con il flusso di autenticazione partner:** Se la risposta dell&#39;endpoint Profiles non contiene un profilo, l&#39;applicazione di streaming continua con il flusso di autenticazione partner.

+++

+++C. Fase di autenticazione partner

1. **Recupera configurazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per recuperare l&#39;elenco degli MVPD con un&#39;integrazione attiva inviando una richiesta all&#39;endpoint di configurazione.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recuperare la configurazione per un provider di servizi specifico](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request), fare riferimento alla documentazione API di Recuperare la configurazione per:
   >
   > * Tutti i parametri _required_, come `serviceProvider`
   > * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier` e `X-Device-Info`
   > * Tutti i parametri e le intestazioni _optional_

1. **Configurazione restituita:** La risposta dell&#39;endpoint Configuration contiene informazioni sugli MVPD con un&#39;integrazione attiva con il provider di servizi.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta alla configurazione, fare riferimento alla documentazione API [Recupera configurazione per un provider di servizi specifico](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response).
   >
   > <br/>
   >
   > L’endpoint Configuration convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi ai [codici di errore avanzati](/help/authentication/enhanced-error-codes.md)

   >[!IMPORTANT]
   >
   > L’applicazione di streaming deve garantire che, nel procedere, elabori i seguenti dettagli forniti per ogni MVPD:
   >
   > * `enablePlatformServices`: indica se MVPD supporta attualmente il Single Sign-On di Apple.
   > * `displayInPlatformPicker`: indica se MVPD può essere visualizzato nel selettore Apple.
   > * `boardingStatus`: indica se MVPD è integrato nel Single Sign-On di Apple.

1. **Recupera stato framework partner:** L&#39;applicazione di streaming chiama il [Framework account sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount) sviluppato da Apple per ottenere le autorizzazioni utente e le informazioni sul provider.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount), consulta:
   >
   > <br/>
   >
   > * L&#39;applicazione di streaming deve verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente e procedere solo se l&#39;utente lo ha consentito.
   > * L&#39;applicazione di streaming deve fornire un [delegato](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) per `VSAccountManager`.
   > * L&#39;applicazione di streaming deve inviare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per le informazioni sull&#39;account del sottoscrittore.
   > * L&#39;applicazione di streaming deve attendere ed elaborare le informazioni di [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L&#39;applicazione di streaming deve verificare di specificare un valore booleano uguale a `true` per la proprietà [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) nell&#39;oggetto `VSAccountMetadataRequest`, per indicare che l&#39;utente può essere interrotto per selezionare il provider TV in questa fase.

1. **Restituisci informazioni sullo stato del framework partner:** L&#39;applicazione di streaming convalida i dati di risposta per verificare che siano soddisfatte le condizioni di base:
   * Lo stato di accesso dell’autorizzazione utente è concesso.
   * Identificatore di mapping del provider utente presente e valido.
   * La data di scadenza del profilo del provider utente (se disponibile) è valida.

1. **Recupera richiesta di autenticazione partner:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per avviare una sessione di autenticazione chiamando l&#39;endpoint Sessions Partner.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recuperare la richiesta di autenticazione partner](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request), consulta la documentazione API di:
   >
   > * Tutti i parametri _required_, come `serviceProvider` e `partner`
   > * Tutte le intestazioni _required_ come `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` e `AP-Partner-Framework-Status`
   > * Tutte le intestazioni e i parametri _optional_
   >
   > <br/>
   >
   > L’applicazione di streaming deve garantire di includere un valore valido per lo stato del framework del partner in modo che la risposta recuperata possa includere una richiesta di autenticazione del partner (richiesta SAML).
   >
   > <br/>
   >
   > Per ulteriori dettagli sull&#39;intestazione `AP-Partner-Framework-Status`, consulta la documentazione [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Indicare l&#39;azione successiva:** La risposta dell&#39;endpoint Sessions Partner contiene i dati necessari per guidare l&#39;applicazione di streaming per quanto riguarda l&#39;azione successiva.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta della sessione, consultare la documentazione API [Recupera richiesta di autenticazione partner](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response).
   >
   > <br/>
   >
   > L’endpoint Sessions Partner convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   >
   > Se la convalida di base non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](/help/authentication/enhanced-error-codes.md).
   >
   > <br/>
   >
   > L’endpoint Sessions Partner convalida i dati della richiesta per garantire che vengano soddisfatte le condizioni di single sign-on del partner:
   >
   >  * La configurazione Single Sign-On del partner nel server Adobe Pass deve essere valida e abilitata.
   >  * Il payload dello stato del framework partner ricevuto tramite l&#39;intestazione [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) deve essere valido.
   >
   > <br/>
   >
   > Se la convalida Single Sign-On del partner non riesce, la risposta viene impostata come flusso di autenticazione di base per impostazione predefinita.

1. **Procedi con i flussi di decisioni:** La risposta dell&#39;endpoint del partner sessioni contiene i dati seguenti:
   * L&#39;attributo `actionName` è impostato su &quot;authorize&quot;.
   * L&#39;attributo `actionType` è impostato su &quot;direct&quot;.

   Se il backend di Adobe Pass identifica un profilo valido, non è necessario che l’applicazione di streaming autentichi nuovamente con l’MVPD selezionato, in quanto esiste già un profilo che può essere utilizzato per i flussi decisionali successivi.

1. **Procedi con il flusso di autenticazione di base:** La risposta dell&#39;endpoint Sessions Partner contiene i dati seguenti:
   * L&#39;attributo `actionName` è impostato su &quot;authenticate&quot; o &quot;resume&quot;.
   * L&#39;attributo `actionType` è impostato su &quot;interactive&quot; o &quot;direct&quot;.

   Se il backend di Adobe Pass non identifica un profilo valido e la convalida single sign-on del partner non riesce, il server Adobe Pass torna al flusso di autenticazione di base.

   Per ulteriori dettagli sul flusso di autenticazione di base, consulta i seguenti documenti:
   * [Eseguire l&#39;autenticazione nell&#39;applicazione principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Eseguire l&#39;autenticazione nell&#39;applicazione secondaria con mvpd preselezionato](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Eseguire l&#39;autenticazione nell&#39;applicazione secondaria senza mvpd preselezionato](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Procedere con il recupero del profilo utilizzando il flusso di risposta di autenticazione partner:** La risposta dell&#39;endpoint del partner sessioni contiene i dati seguenti:
   * L&#39;attributo `actionName` è impostato su &quot;partner_profile&quot;.
   * L&#39;attributo `actionType` è impostato su &quot;direct&quot;.
   * L&#39;attributo `authenticationRequest - type` include il protocollo di sicurezza utilizzato dal framework partner per l&#39;accesso MVPD (attualmente impostato solo su SAML).
   * L&#39;attributo `authenticationRequest - request` include la richiesta SAML passata al framework partner.
   * L&#39;attributo `authenticationRequest - attributesNames` include gli attributi SAML passati al framework partner.

   Se il backend di Adobe Pass non identifica un profilo valido e il partner passa la convalida single sign-on, l’applicazione di streaming riceve una risposta con azioni e dati da passare al framework partner per avviare il flusso di autenticazione con MVPD.

1. **Completare l&#39;autenticazione MVPD con il framework partner:** Inoltrare la richiesta di autenticazione partner (richiesta SAML) ottenuta nel passaggio precedente al [Framework account sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount). Se il flusso di autenticazione ha esito positivo, l&#39;interazione [Framework account sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount) con MVPD genera una risposta di autenticazione partner (risposta SAML) restituita insieme alle informazioni sullo stato del framework partner.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount), consulta:
   >
   > <br/>
   >
   > * L&#39;applicazione di streaming deve verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente e procedere solo se l&#39;utente lo ha consentito.
   > * L&#39;applicazione di streaming deve fornire un [delegato](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) per `VSAccountManager`.
   > * L&#39;applicazione di streaming deve inviare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per le informazioni sull&#39;account del sottoscrittore e deve includere la richiesta di autenticazione partner (richiesta SAML) ottenuta nel passaggio precedente.
   > * L&#39;applicazione di streaming deve attendere ed elaborare le informazioni di [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L&#39;applicazione di streaming deve verificare di specificare un valore booleano uguale a `true` per la proprietà [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) nell&#39;oggetto `VSAccountMetadataRequest`, per indicare che l&#39;utente può essere interrotto per l&#39;autenticazione con il provider TV selezionato in questa fase.

1. **Risposta di autenticazione partner di ritorno:** L&#39;applicazione di streaming convalida i dati di risposta per verificare che siano soddisfatte le condizioni di base:
   * Lo stato di accesso dell’autorizzazione utente è concesso.
   * Identificatore di mapping del provider utente presente e valido.
   * La data di scadenza del profilo del provider utente (se disponibile) è valida.
   * La risposta di autenticazione del partner (risposta SAML) è presente e valida.

1. **Recupera profilo utilizzando la risposta di autenticazione partner:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per creare e recuperare un profilo chiamando l&#39;endpoint partner Profili.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Recuperare il profilo utilizzando la risposta di autenticazione partner](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request), consultare la documentazione API:
   >
   > * Tutti i parametri _required_, come `serviceProvider`, `partner` e `SAMLResponse`
   > * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` e `AP-Partner-Framework-Status`
   > * Tutte le intestazioni e i parametri _optional_
   >
   > <br/>
   >
   > L’applicazione di streaming deve garantire di includere valori validi per lo stato del framework del partner e la risposta di autenticazione del partner (risposta SAML) in modo che la risposta recuperata possa includere un profilo di tipo &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Per ulteriori dettagli sull&#39;intestazione `AP-Partner-Framework-Status`, consulta la documentazione [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Restituire informazioni sul profilo partner:** La risposta dell&#39;endpoint Profiles contiene informazioni sul profilo partner, incluso l&#39;attributo `type` impostato su &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta del profilo, consulta la documentazione API [Recupera profilo tramite risposta di autenticazione partner](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response).
   >
   > <br/>
   >
   > L’endpoint Partner Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](/help/authentication/enhanced-error-codes.md).
   >
   > <br/>
   >
   > L’endpoint Partner Profili convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di single sign-on del partner:
   >
   >  * La configurazione Single Sign-On del partner nel server Adobe Pass deve essere valida e abilitata.
   >  * Il payload dello stato del framework partner ricevuto tramite l&#39;intestazione [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) deve essere valido.
   >
   > <br/>
   >
   > Se la convalida Single Sign-On del partner non riesce, la risposta viene impostata come predefinita sul flusso di recupero dei profili di base.

1. **Procedi con i flussi di decisioni:** L&#39;applicazione di streaming può continuare con i flussi di decisioni successivi.

+++

+++ D. Fase decisionale

1. **Recupera stato framework partner:** L&#39;applicazione di streaming chiama il [Framework account sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount) sviluppato da Apple per ottenere le autorizzazioni utente e le informazioni sul provider.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount), consulta:
   >
   > <br/>
   >
   > * L&#39;applicazione di streaming deve verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente e procedere solo se l&#39;utente lo ha consentito.
   > * L&#39;applicazione di streaming deve fornire un [delegato](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) per `VSAccountManager`.
   > * L&#39;applicazione di streaming deve inviare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per le informazioni sull&#39;account del sottoscrittore.
   > * L&#39;applicazione di streaming deve attendere ed elaborare le informazioni di [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L&#39;applicazione di streaming deve verificare di specificare un valore booleano uguale a `false` per la proprietà [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) nell&#39;oggetto `VSAccountMetadataRequest`, per indicare che l&#39;utente non può essere interrotto in questa fase.

   >[!TIP]
   > 
   > Suggerimento: l’applicazione di streaming può utilizzare un valore memorizzato nella cache per le informazioni sullo stato del framework del partner, che consigliamo di aggiornare quando l’applicazione passa dallo stato in background a quello in primo piano.

1. **Restituisci informazioni sullo stato del framework partner:** L&#39;applicazione di streaming convalida i dati di risposta per verificare che siano soddisfatte le condizioni di base:
   * Lo stato di accesso dell’autorizzazione utente è concesso.
   * Identificatore di mapping del provider utente presente e valido.
   * La data di scadenza del profilo del provider utente (se disponibile) è valida.

1. **Recupera decisioni di preautorizzazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per ottenere le decisioni di preautorizzazione per un elenco di risorse chiamando l&#39;endpoint di preautorizzazione delle decisioni.

   >[!IMPORTANT]
   >
   > Consulta la [Documentazione API mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) specifica per il recupero delle decisioni di preautorizzazione, per informazioni dettagliate su:
   >
   > * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   > * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_
   >
   > <br/>
   >
   > L’applicazione di streaming deve garantire di includere un valore valido per lo stato del framework del partner prima di effettuare un’ulteriore richiesta, quando il profilo scelto è di tipo &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Per ulteriori dettagli sull&#39;intestazione `AP-Partner-Framework-Status`, consulta la documentazione [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Decisioni di pre-autorizzazione restituite:** La risposta dell&#39;endpoint di preautorizzazione delle decisioni contiene una decisione `Permit` o `Deny` per ogni risorsa:
   * Una decisione `Permit` indica che la risorsa è riproducibile. La risposta non include un token multimediale, poiché il flusso di preautorizzazione non deve essere utilizzato per riprodurre le risorse.
   * Una decisione `Deny` indica che la risorsa non è riproducibile. La risposta include un payload di errore conforme alla documentazione di [Codici di errore avanzati](/help/authentication/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta alla decisione, consulta la [Documentazione API mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) specifica per il recupero delle decisioni di preautorizzazione.
   >
   > <br/>
   >
   > L’endpoint di preautorizzazione delle decisioni convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](/help/authentication/enhanced-error-codes.md).

1. **Recupera stato framework partner:** L&#39;applicazione di streaming chiama il [Framework account sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount) sviluppato da Apple per ottenere le autorizzazioni utente e le informazioni sul provider.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount), consulta:
   >
   > <br/>
   >
   > * L&#39;applicazione di streaming deve verificare la presenza di [autorizzazioni per accedere](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) alle informazioni di sottoscrizione dell&#39;utente e procedere solo se l&#39;utente lo ha consentito.
   > * L&#39;applicazione di streaming deve fornire un [delegato](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) per `VSAccountManager`.
   > * L&#39;applicazione di streaming deve inviare una [richiesta](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) per le informazioni sull&#39;account del sottoscrittore.
   > * L&#39;applicazione di streaming deve attendere ed elaborare le informazioni di [metadati](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L&#39;applicazione di streaming deve verificare di specificare un valore booleano uguale a `false` per la proprietà [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) nell&#39;oggetto `VSAccountMetadataRequest`, per indicare che l&#39;utente non può essere interrotto in questa fase.

   >[!TIP]
   >
   > Suggerimento: l’applicazione di streaming può utilizzare un valore memorizzato nella cache per le informazioni sullo stato del framework del partner, che consigliamo di aggiornare quando l’applicazione passa dallo stato in background a quello in primo piano.

1. **Restituisci informazioni sullo stato del framework partner:** L&#39;applicazione di streaming convalida i dati di risposta per verificare che siano soddisfatte le condizioni di base:
   * Lo stato di accesso dell’autorizzazione utente è concesso.
   * Identificatore di mapping del provider utente presente e valido.
   * La data di scadenza del profilo del provider utente (se disponibile) è valida.

1. **Recupera decisione di autorizzazione:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per ottenere una decisione di autorizzazione per una risorsa specifica chiamando l&#39;endpoint Decisions Authorize.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su: [Recuperare le decisioni di autorizzazione utilizzando la documentazione API mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) specifica:
   >
   > * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `resources`
   > * Tutte le intestazioni _required_, come `Authorization` e `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_
   >
   > <br/>
   >
   > L’applicazione di streaming deve garantire di includere un valore valido per lo stato del framework del partner prima di effettuare un’ulteriore richiesta, quando il profilo scelto è di tipo &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Per ulteriori dettagli sull&#39;intestazione `AP-Partner-Framework-Status`, consulta la documentazione [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Decisione di autorizzazione di ritorno:** La risposta dell&#39;endpoint Authorize Decisions contiene una decisione `Permit` o `Deny` per la risorsa specifica:
   * Una decisione `Permit` indica che la risorsa è riproducibile. La risposta include un token multimediale.
   * Una decisione `Deny` indica che la risorsa non è riproducibile. La risposta include un payload di errore conforme alla documentazione di [Codici di errore avanzati](/help/authentication/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta alla decisione, consulta la [documentazione API MVPD](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) specifica per il recupero delle decisioni di autorizzazione.
   >
   > <br/>
   >
   > L’endpoint Decisions Authorize convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](/help/authentication/enhanced-error-codes.md).

+++

+++ D. Fase di disconnessione

1. **Avvia disconnessione di Adobe Pass:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per avviare il flusso di disconnessione chiamando l&#39;endpoint di disconnessione di Adobe Pass.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate su: [Avvia disconnessione per la documentazione API specifica di mvpd](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request):
   >
   > * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `redirectUrl`
   > * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   > * Tutti i parametri e le intestazioni _optional_

1. **Indicare l&#39;azione successiva:** La risposta dell&#39;endpoint di disconnessione di Adobe Pass contiene i dati necessari per guidare l&#39;applicazione di streaming per quanto riguarda l&#39;azione successiva:
   * Attributo `url` mancante perché l&#39;utente deve interagire con il livello del partner (sistema) per completare il flusso di disconnessione.
   * L&#39;attributo `actionName` è impostato su &quot;partner_logout&quot;.
   * L&#39;attributo `actionType` è impostato su &quot;partner_interactive&quot;.

   >[!IMPORTANT]
   >
   > Per informazioni dettagliate sulle informazioni fornite in una risposta di disconnessione, consultare la [Iniziare la disconnessione per la documentazione API mvpd](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) specifica.
   >
   > <br/>
   >
   > L’endpoint di disconnessione di Adobe Pass convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   >
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](/help/authentication/enhanced-error-codes.md).

   >[!IMPORTANT]
   > 
   > L’applicazione di streaming deve garantire che indichi all’utente di continuare a disconnettersi ulteriormente dal livello partner.

+++
