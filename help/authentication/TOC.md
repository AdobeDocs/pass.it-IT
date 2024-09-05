---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Autenticazione Adobe Pass
user-guide-description: L’autenticazione Adobe Pass è una soluzione di gestione dei diritti per TV Everywhere, che fornisce un framework modulare per determinare se chi richiede l’accesso a una risorsa ne abbia diritto.
source-git-commit: 82fd63a0e208a90753235b81fa52757283c9d314
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 2%

---


# Guida all’autenticazione di Adobe Pass {#authentication}

+ [Panoramica sull’autenticazione di Adobe Pass](home.md)
+ Concetti di autenticazione Adobe Pass {#authentication-concepts}
   + [Documento tecnico](technical-paper.md)
   + [Panoramica sui programmatori](programmer-overview.md)
   + [Panoramica di MVPD](mvpd-overview.md)
+ Guide rapide {#kickstart-guides}
   + [Guida introduttiva per programmatori](programmer-kickstart-guide.md)
   + [Guida introduttiva MVPD](mvpd-kickstart-guide.md)
+ Guida all&#39;integrazione dei programmatori {#programmer-integration-guide}
   + [Panoramica della guida all’integrazione dei programmatori](programmer-integration-guide-overview.md)
   + [Flusso adesione programmatore](entitlement-flow.md)
   + [Casi di utilizzo dei programmatori](programmer-use-cases.md)
   + [Trasmissione delle informazioni del client (dispositivo, connessione e applicazione)](passing-client-information-device-connection-and-application.md)
   + [Meccanismo di limitazione](throttling-mechanism.md)
   + API REST V1 {#rest-api-v1}
      + [Panoramica API REST](rest-api-overview.md)
      + [Manuale dell’API REST (server-to-server)](rest-api-cookbook-servertoserver.md)
      + [Manuale dell’API REST (da client a server)](rest-api-cookbook-clienttoserver.md)
      + Riferimento API REST {#rest-api-reference}
         + [Riferimento API REST](rest-api-reference.md)
         + [Richiesta codice di registrazione](registration-code-request.md)
         + [Record di registrazione di ritorno](return-registration-record.md)
         + [Elimina record di registrazione](delete-registration-record.md)
         + [Fornisci elenco MVPD](provide-mvpd-list.md)
         + [Avvia autenticazione](initiate-authentication.md)
         + [Controlla token di autenticazione](check-authentication-token.md)
         + [Recupera token di autenticazione](retrieve-authentication-token.md)
         + [Avvia autorizzazione](initiate-authorization.md)
         + [Recupera token di autorizzazione](retrieve-authorization-token.md)
         + [Ottieni token multimediale breve](obtain-short-media-token.md)
         + [Controlla il flusso di autenticazione tramite l’app web Second Screen](check-authentication-flow-by-second-screen-web-app.md)
         + [Recupera elenco risorse autorizzate](retrieve-list-of-preauthorized-resources.md)
         + [Recupera elenco di risorse preautorizzate tramite l’app web Second Screen](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [Avvia disconnessione](initiate-logout.md)
         + [Metadati utente](user-metadata.md)
         + [Recupera richiesta-profilo](retrieve-profilerequest.md)
         + [Scambio di token](token-exchange.md)
         + [Anteprima gratuita per Passaggio temporaneo e Passaggio temporaneo promozionale](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + API REST V2 {#rest-api-v2}
      + API {#rest-api-v2-apis}
         + [REST API V2 - API - Panoramica](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
         + Configurazione {#rest-api-v2-configuration-apis}
            + [Recupera la configurazione per un provider di servizi specifico](./rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
         + Sessioni {#rest-api-v2-sessions-apis}
            + [Crea sessione di autenticazione](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
            + [Riprendi sessione di autenticazione](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
            + [Recupera sessione di autenticazione](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
            + [Eseguire l’autenticazione nell’agente utente](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
         + Profili {#rest-api-v2-profiles-apis}
            + [Recuperare i profili](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
            + [Recupera profilo per mvpd specifico](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
            + [Recupera profilo per codice specifico](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
         + Decisioni {#rest-api-v2-decisions-apis}
            + [Recuperare le decisioni di autorizzazione utilizzando mvpd specifico](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
            + [Recuperare le decisioni di pre-autorizzazione utilizzando mvpd specifico](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
         + Disconnetti {#rest-api-v2-logout-apis}
            + [Avvia disconnessione per mvpd specifico](./rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
         + Single Sign-On Partner {#rest-api-v2-partner-single-sign-on-apis}
            + [Recupera richiesta di autenticazione partner](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
            + [Recupera il profilo utilizzando la risposta di autenticazione partner](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
      + Flussi {#rest-api-v2-flows}
         + [REST API V2 - Flussi - Panoramica](./rest-api-v2/flows/rest-api-v2-flows-overview.md)
         + Flussi di accesso di base {#rest-api-v2-basic-access-flows}
            + [Flusso dei profili di base eseguito all’interno dell’applicazione principale](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
            + [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
            + [Flusso di autenticazione di base eseguito nell&#39;applicazione principale](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
            + [Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
            + [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
            + [Flusso di pre-autorizzazione di base eseguito nell’applicazione principale](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
            + [Flusso di logout di base eseguito nell&#39;applicazione principale](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
         + Flussi di accesso danneggiati {#rest-api-v2-degraded-access-flows}
            + [Flussi di accesso degradati](rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
         + Flussi di accesso temporanei {#rest-api-v2-temporary-access-flows}
            + [Flussi di accesso temporanei](rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
         + Flussi di accesso Single Sign-On {#rest-api-v2-single-sign-on-access-flows}
            + [Single sign-on con flussi di partner](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
            + [Single sign-on con flussi di identità della piattaforma](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
            + [Single sign-on con flussi di token di servizio](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
            + [Flusso disconnessione singola](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
      + Appendice {#rest-api-v2-appendix}
         + Intestazioni {#rest-api-v2-appendix-headers}
            + [Intestazione - Authorization](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
            + [Intestazione - AP-Device-Identifier](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
            + [Intestazione - X-Device-Info](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
            + [Intestazione - AD-Service-Token](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
            + [Intestazione - Adobe-Subject-Token](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
            + [Intestazione - AP-Partner-Framework-Status](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
            + [Intestazione - AP-TempPass-Identity](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + SDK AccessEnabler {#accessenabler-sdk}
      + SDK JavaScript {#javascriptsdk}
         + [Panoramica dell’SDK per JavaScript](javascript-sdk-overview.md)
         + [Manuale dell’SDK di JavaScript](javascript-sdk-cookbook.md)
         + [Riferimento API di JavaScript SDK](javascript-sdk-api-reference.md)
         + Linee guida {#js-sdk-guidelines}
            + [Accesso e disconnessione senza aggiornamento](refreshless-login-and-logout.md)
         + API JavaScript {#js-api}
            + [Autorizza in anticipo](js-preauthorize.md)
      + SDK iOS/tvOS {#ios-sdk}
         + [Panoramica dell’SDK iOS/tvOS](iostvos-sdk-overview.md)
         + [Manuale dell’SDK iOS/tvOS](iostvos-sdk-cookbook.md)
         + [Riferimento API per iOS/tvOS SDK](iostvos-sdk-api-reference.md)
         + Linee guida {#ios-tvos-sdk-guidelines}
            + [Registrazione applicazione iOS/tvOS](iostvos-application-registration.md)
            + Linee guida per la migrazione {#migration-guidelines}
               + [Guida alla migrazione ad iOS/tvOS v3.x](iostvos-v3x-migration-guide.md)
            + [Controlli di integrità archiviazione iOS/tvOS](iostvos-sdk-storage-integrity-checks.md)
         + API iOS/tvOS {#ios-tvos-api}
            + [Autorizza in anticipo](preauthorize.md)
      + SDK di Android {#androidsdk}
         + [Panoramica dell’SDK di Android](android-sdk-overview.md)
         + [Manuale dell’SDK di Android](android-sdk-cookbook.md)
         + [Riferimento API dell&#39;SDK per Android](android-sdk-api-reference.md)
         + Linee guida {#androidguidelines}
            + [Registrazione applicazione Android](android-application-registration.md)
            + [SDK per Android con registrazione client dinamica](android-sdk-with-dynamic-client-registration.md)
         + API Android{#androidapi}
            + [Autorizza in anticipo](preauthorize-android.md)
      + SDK di Amazon FireOS {#fireossdk}
         + [Amazon FireOS SSO - Guida introduttiva per il programmatore](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [SSO di Amazon FireOS utilizzando il manuale API senza client](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Panoramica tecnica di Amazon FireOS](amazon-fireos-technical-overview.md)
         + [Manuale dell’integrazione di Amazon FireOS](amazon-fireos-integration-cookbook.md)
         + [Riferimento API di Amazon FireOS](amazon-fireos-native-client-api-reference.md)
         + [Registrazione applicazione Amazon FireOS](amazon-fireos-application-registration.md)
         + [SDK FireOS con registrazione client dinamica](fireos-sdk-with-dynamic-client-registration.md)
   + SSO piattaforma {#platform-sso}
      + SSO Apple {#apple-sso}
         + [Panoramica di Apple SSO](apple-sso-overview.md)
         + [Manuale Apple SSO (REST API)](apple-sso-cookbook-rest-api.md)
         + [Manuale Apple SSO (iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + SSO Roku {#roku-sso}
         + [SSO Roku](roku-sso-overview.md)
   + Metadati contenuto {#content-metadata}
      + [Identificazione di una risorsa protetta](identify-protected-resources.md)
   + Integrazione del server dei contenuti {#content-serv-int}
      + [Integrare Media Token Verifier](media-token-verifier-int.md)
   + Appendici {#appendices}
      + [Suggerimenti per il debug](appendix-b-debugging-tips.md)
+ Guida all&#39;integrazione di MVPD {#mvpd-int-guide}
   + [Funzioni di integrazione](mvpd-integr-features.md)
   + [Autenticazione](authn-usecase.md)
   + [Autenticazione tramite il protocollo OAuth 2.0](authn-oauth2-protocol.md)
   + [Autorizzazione](authz-usecase.md)
   + [Autorizzazione di verifica preliminare](mvpd-preflight-authz.md)
   + [Disconnessione MVPD](usecase-mvpd-logout.md)
   + [Scambio di metadati del contenuto](mvpd-content-metadata-exchange.md)
   + [Scambio di metadati utente](mvpd-user-metadata-exchng.md)
   + [Servizio Web MVPD proxy](proxy-mvpd-webserv.md)
   + [Integrazione SAML MVPD proxy](proxy-mvpd-saml-int.md)
   + [Ambito provider di servizi](serv-provider-scoping.md)
   + [Indirizzi IP consentiti MVPD](mvpd-listing-ip-addres.md)
+ Funzioni di autenticazione di Adobe Pass {#auth-features}
   + Integrazione Adobe Analytics {#analytics-int}
      + [Integrazione dei dati lato server di autenticazione di Adobe Pass in Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [Utilizzo dell’ID Experience Cloud nell’autenticazione di Adobe Pass](exp-cloud-id-authn.md)
   + Monitoraggio del servizio spettanti {#entitlement-service-monitoring}
      + [Panoramica sul monitoraggio del servizio di adesione](entitlement-service-monitoring-overview.md)
      + [API di monitoraggio del servizio di adesione](entitlement-service-monitoring-api.md)
      + [Metriche lato server](understanding-serverside-metrics.md)
   + Passaggio temporaneo {#temp-pass}
      + [Passaggio temporaneo](temp-pass.md)
      + [Passaggio temporaneo promozionale](promotional-temp-pass.md)
      + [Ripristina passaggio temporaneo](reset-temp-pass.md)
   + Single Sign-On {#sso}
      + [Supporto del Single Sign-On](sso-support.md)
      + [SSO tramite autenticazione passiva](sso-passive-authn.md)
   + Autenticazione basata su Home {#home-based-auth}
      + [Autenticazione basata su Home per TV Everywhere](home-based-authn-tve.md)
      + [Stato HBA per MVPD](hba-status-mvpds.md)
   + Metadati utente {#user-metadat}
      + [Metadati utente](user-metadata-feature.md)
   + [Autorizzazione di verifica preliminare](preflight-authz.md)
   + Segnalazione errori {#error-reportn}
      + [Segnalazione di errori](error-reporting.md)
      + [Codici di errore migliorati](enhanced-error-codes.md)
   + Registrazione client {#dcr-api}
      + [Panoramica sulla registrazione client dinamica](./dcr-api/dynamic-client-registration-overview.md)
      + API {#dcr-api-apis}
         + [Recupera credenziali client](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
         + [Recupera token di accesso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
      + Flussi {#dcr-api-flows}
         + [Flusso di registrazione client dinamico](./dcr-api/flows/dynamic-client-registration-flow.md)
   + Servizio di degradazione {#degrn-service}
      + [Panoramica dell’API di degradazione](degradation-api-overview.md)
   + Conformità privacy {#privacy-readiness}
      + [Panoramica sul supporto della privacy](privacy-supp-overview.md)
      + [Come effettuare una richiesta di accesso a dati personali](make-privacy-req.md)
+ Suggerimenti e risoluzione dei problemi {#tips-troubleshoot}
   + [Consenti MVPD nella finestra di dialogo di selezione](allow-mvpd-selectn-dialog.md)
   + [Impedisci a MVPDs di visualizzare la finestra di dialogo di selezione](prevent-mvpd-selectn-dialog.md)
+ Supporto {#support}
   + [Procedure di riassegnazione](escalation-procedures.md)
   + [Monitoraggio di Adobe Pass Adobe PayTV Pass](monitoring-adobe-pay-tv-pass.md)
   + [Requisiti minimi di sistema](minimum-system-requirements.md)
+ Note sulla versione {#release-notes}
   + [Note sulla versione di Adobe Pass Authentication 3.0](auth-rn-300.md)
   + [Note sulla versione di Adobe Pass Authentication 2.70](auth-rn-270.md)
   + [Note sulla versione di Adobe Pass Authentication 2.69](auth-rn-269.md)
   + [Note sulla versione di Adobe Pass Authentication 2.68](auth-rn-268.md)
   + [Note sulla versione di Adobe Pass Authentication 2.67](auth-rn-267.md)
   + [Note sulla versione di Adobe Pass Authentication 2.66](auth-rn-266.md)
   + [Note sulla versione di Adobe Pass Authentication 2.65.1](auth-rn-2651.md)
   + [Note sulla versione di Adobe Pass Authentication 2.65](auth-rn-265.md)
   + [Note sulla versione di Adobe Pass Authentication 2.64.1](auth-rn-2641.md)
   + [Note sulla versione di Adobe Pass Authentication 2.64](auth-rn-264.md)
   + [Note sulla versione di Adobe Pass Authentication 2.63](auth-rn-263.md)
   + [Note sulla versione di Adobe Pass Authentication 2.62.1](auth-rn-2621.md)
   + Note sulla versione di JavaScript SDK {#release-notes-javascript}
      + [Note sulla versione di Adobe Pass Authentication JavaScript 4.7.0](authn-rn-javascript-470.md)
      + [Note sulla versione di Adobe Pass Authentication JavaScript 4.6.0](authn-rn-javascript-460.md)
      + [Note sulla versione di Adobe Pass Authentication JavaScript 4.4.0](authn-rn-javascript-440.md)
      + [Note sulla versione di Adobe Pass Authentication JavaScript 4.2.0](authn-rn-javascript-420.md)
      + [Note sulla versione di Adobe Pass Authentication JavaScript 4.1.1](authn-rn-javascript-411.md)
      + [Note sulla versione di Adobe Pass Authentication JavaScript 4.1.0](authn-rn-javascript-410.md)
      + [Note sulla versione di Adobe Pass Authentication JavaScript 4.0.0](authn-rn-javascript-400.md)
      + [Note sulla versione di Adobe Pass Authentication JavaScript 3.5.0](authn-rn-javascript-350.md)
   + Note sulla versione dell&#39;SDK iOS/tvOS {#release-notes-ios}
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.9.2](authn-rn-ios-tvos-392.md)
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.8.4](authn-rn-ios-tvos-384.md)
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.8.3](authn-rn-ios-tvos-383.md)
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.8.2](authn-rn-ios-tvos-382.md)
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.8.1](authn-rn-ios-tvos-381.md)
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.7.0](authn-rn-ios-tvos-370.md)
   + Note sulla versione di Android SDK {#release-notes-android}
      + [Note sulla versione di Adobe Pass Authentication Android 3.7.3](authn-rn-android-373.md)
+ Note tecniche {#tech-notes}
   + SDK di autenticazione Adobe Pass {#primetime-authentication-sdks}
      + [Domande e risposte sui certificati](certificates-qa.md)
      + SDK JavaScript {#javascript}
         + [Valutazione della prevenzione del tracciamento - Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [Valutazione della prevenzione del tracciamento - Google Chrome](tracking-prevention-assessment-google-chrome.md)
         + [Aggiornamenti dei cookie - Flag SameSite e Secure](cookies-updates-samesite-and-secure-flags.md)
      + SDK di Android {#android}
         + [Accesso Single Sign-On (SSO) SDK di Android Enabler sulle app Android 10](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Autenticazione di Adobe Pass e il nuovo modello di autorizzazioni &quot;Marshmallow&quot; di Android 6](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + SDK iOS/tvOS {#iostvos}
         + [Supporto WKWebView sull&#39;SDK iOS 3.1+](wkwebview-support-on-ios-sdk-31.md)
         + [Supporto di SFSafariViewController sull&#39;SDK iOS 3.2+](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO su iOS quando si utilizza Adobe Pass Authentication Access Enabler](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [Errore di autenticazione iOS - Impossibile trovare adobepass.ios.app](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Ripristina passaggio temporaneo su iOS](reset-temp-pass-on-ios.md)
         + [Debug dell’SDK iOS/tvOS di AccessEnabler tramite i registri dell’app della console](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [Percorso di aggiornamento ad AccessEnabler iOS/tvOS 3.7.0](accessenabler-iostvos-370-upgrade-path.md)
   + Ambienti di autenticazione pass {#primetime-authentication-environments}
      + [Informazioni sugli ambienti Adobe](understanding-the-adobe-environments.md)
      + [Configurazione dell’ambiente e test in Pre-Qual](setting-up-your-environment-and-testing-in-prequal.md)
      + [Verificare i flussi di autenticazione e autorizzazione utilizzando il sito di test API Adobe](test-authn-authz-flows-using-adobes-api-test-site.md)
   + API senza client {#clientless-api}
      + [Implementazione API senza client - Codici di errore/messaggi con motivo/causa probabile](clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [Flusso API senza client in assenza di ID dispositivo](clientless-api-flow-in-the-absence-of-device-id.md)
      + [Clientless: evita di utilizzare &#39;&amp;&#39;reg_code nella richiesta /authenticate](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Abilitazione dei servizi di adesione Adobe Pass per un programmatore su Xbox 360 e Xbox One Clientless](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Tipo di dispositivo e metriche senza client](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + Esperienza utente {#user-exp}
      + [Come migrare la pagina di accesso MVPD da iFrame a popup](migr-mvpd-login-iframe-popup.md)
      + [Funzione di verifica preliminare: come abilitare, risolvere o determinare il problema](preflight-feature.md)
   + Strumenti e utilità {#tools-and-utilities}
      + [Utilizzo di Charles Proxy](using-charles-proxy.md)
   + Concetti {#concepts}
      + [Informazioni sugli ID utente](understanding-user-ids.md)
+ [Guida utente di TVE Dashboard](tve-dashboard-user-guide.md)
+ Nuova guida utente di TVE Dashboard {#user-guide}
   + [Panoramica del dashboard di TVE](/help/authentication/tve-dashboard-overview.md)
   + [Ambienti](/help/authentication/tve-dashboard-environments.md)
   + [Revisione e invio di modifiche](/help/authentication/tve-dashboard-review-push-changes.md)
   + [Dashboard](/help/authentication/tve-dashboard-home.md)
   + [Canali](/help/authentication/tve-dashboard-channels.md)
   + [Programmatori](/help/authentication/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/tve-dashboard-mvpds.md)
   + [Integrazioni](/help/authentication/tve-dashboard-integrations.md)
   + [Rapporti](/help/authentication/tve-dashboard-reports.md)
   + [Registro delle modifiche](/help/authentication/tve-dashboard-changes-log.md)
+ [Glossario](glossary.md)
