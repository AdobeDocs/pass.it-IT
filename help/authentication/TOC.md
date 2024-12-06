---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Autenticazione Adobe Pass
user-guide-description: L’autenticazione Adobe Pass è una soluzione di gestione dei diritti per TV Everywhere, che fornisce un framework modulare per determinare se chi richiede l’accesso a una risorsa ne abbia diritto.
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1154'
ht-degree: 3%

---


# Guida all’autenticazione di Adobe Pass {#authentication}

+ [Autenticazione Adobe Pass](home.md)
+ Kick-Start {#kickstart}
   + [Documento tecnico](kickstart/technical-paper.md)
   + [Panoramica sui programmatori](kickstart/programmer-overview.md)
   + [Panoramica di MVPD](kickstart/mvpd-overview.md)
   + [Guida introduttiva per programmatori](kickstart/programmer-kickstart-guide.md)
   + [Guida introduttiva MVPD](kickstart/mvpd-kickstart-guide.md)
   + [Procedure di riassegnazione](notes-technical/escalation-procedures.md)
   + [Glossario](kickstart/glossary.md)
+ Guida All&#39;Integrazione Per I Programmatori {#integration-guide-programmers}
   + API REST {#rest-apis}
      + DCR REST API {#rest-api-dcr}
         + [Panoramica sulla registrazione client dinamica](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + API {#rest-api-dcr-apis}
            + [Recupera credenziali client](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [Recupera token di accesso](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + Flussi {#rest-api-dcr-flows}
            + [Flusso di registrazione client dinamico](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + API REST V2 {#rest-api-v2}
         + [Panoramica di REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [Glossario REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [Domande frequenti su REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + API {#rest-api-v2-apis}
            + [Panoramica delle API REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + Configurazione {#rest-api-v2-configuration-apis}
               + [Recupera la configurazione per un provider di servizi specifico](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + Sessioni {#rest-api-v2-sessions-apis}
               + [Crea sessione di autenticazione](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [Riprendi sessione di autenticazione](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [Recupera sessione di autenticazione](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [Eseguire l’autenticazione nell’agente utente](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + Profili {#rest-api-v2-profiles-apis}
               + [Recuperare i profili](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [Recupera profilo per mvpd specifico](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [Recupera profilo per codice specifico](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + Decisioni {#rest-api-v2-decisions-apis}
               + [Recuperare le decisioni di autorizzazione utilizzando mvpd specifico](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [Recuperare le decisioni di pre-autorizzazione utilizzando mvpd specifico](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + Disconnetti {#rest-api-v2-logout-apis}
               + [Avvia disconnessione per mvpd specifico](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + Single Sign-On Partner {#rest-api-v2-partner-single-sign-on-apis}
               + [Recupera richiesta di autenticazione partner](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [Recupera il profilo utilizzando la risposta di autenticazione partner](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + Flussi {#rest-api-v2-flows}
            + [Panoramica dei flussi REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + Flussi di accesso di base {#rest-api-v2-basic-access-flows}
               + [Flusso dei profili di base eseguito all’interno dell’applicazione principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [Flusso di autenticazione di base eseguito nell&#39;applicazione principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [Flusso di autenticazione di base eseguito nell&#39;applicazione secondaria](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [Flusso di pre-autorizzazione di base eseguito nell’applicazione principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [Flusso di logout di base eseguito nell&#39;applicazione principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + Flussi di accesso danneggiati {#rest-api-v2-degraded-access-flows}
               + [Flussi di accesso degradati](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + Flussi di accesso temporanei {#rest-api-v2-temporary-access-flows}
               + [Flussi di accesso temporanei](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + Flussi di accesso Single Sign-On {#rest-api-v2-single-sign-on-access-flows}
               + [Single sign-on con flussi di partner](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [Single sign-on con flussi di identità della piattaforma](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [Single sign-on con flussi di token di servizio](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [Flusso disconnessione singola](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + Manuali di ricerca {#rest-api-v2-cookbooks}
            + [Manuale dell’API REST V2 (da client a server)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [Manuale dell’API REST V2 (server-to-server)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + Appendice {#rest-api-v2-appendix}
            + Intestazioni {#rest-api-v2-appendix-headers}
               + [Intestazione - Authorization](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [Intestazione - AP-Device-Identifier](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [Intestazione - X-Device-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [Intestazione - AD-Service-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [Intestazione - Adobe-Subject-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [Intestazione - AP-Partner-Framework-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [Intestazione - AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + Funzioni standard {#standard-features}
      + Diritti {#entitlements}
         + [Identificazione di una risorsa protetta](integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md)
         + [Autorizzazione di verifica preliminare](integration-guide-programmers/features-standard/entitlements/preflight-authz.md)
         + [Integrare Media Token Verifier](integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md)
         + [Metadati utente](integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md)
         + [Certificato metadati utente per la crittografia](integration-guide-programmers/features-standard/entitlements/user-metadata-certificate.md)
      + Segnalazione errori {#error-reporting}
         + [Codici di errore migliorati](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
         + [Segnalazione di errori](integration-guide-programmers/features-standard/error-reporting/error-reporting.md)
      + Accesso Single Sign-On {#sso-access}
         + Single Sign-On Partner {#partner-sso}
            + Apple Single Sign-On {#apple-sso}
               + [Panoramica di Apple SSO](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Manuale Apple SSO (REST API V2)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
               + [Manuale Apple SSO (REST API V1)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v1.md)
               + [Manuale Apple SSO (iOS/tvOS SDK)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-iostvos-sdk.md)
         + Single Sign-On piattaforma {#platform-sso}
            + Amazon Single Sign-On {#amazon-sso}
               + [Manuale Amazon SSO (REST API V2)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
               + [Manuale Amazon SSO (REST API V1)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v1.md)
            + Single Sign-On Roku {#roku-sso}
               + [Panoramica di Roku SSO](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
         + [Supporto del Single Sign-On](integration-guide-programmers/features-standard/sso-access/sso-support.md)
         + [SSO tramite autenticazione passiva](integration-guide-programmers/features-standard/sso-access/sso-passive-authn.md)
      + Accesso all&#39;autenticazione basata sulla home page {#hba-access}
         + [Autenticazione basata su Home per TV Everywhere](integration-guide-programmers/features-standard/hba-access/home-based-authn-tve.md)
         + [Stato HBA per MVPD](integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md)
      + Supporto per la privacy {#privacy-support}
         + [Panoramica sul supporto della privacy](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [Come effettuare una richiesta di accesso a dati personali](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + Funzionalità Premium {#features-premium}
      + Accesso temporaneo {#temporary-access}
         + [Passaggio temporaneo](integration-guide-programmers/features-premium/temporary-access/temp-pass.md)
         + [Passaggio temporaneo promozionale](integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)
         + [Ripristina passaggio temporaneo](integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md)
      + Accesso Danneggiato {#degraded-access}
         + [Panoramica API di degradazione](integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md)
      + ESM {#esm}
         + [Panoramica sul monitoraggio del servizio di adesione](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [API di monitoraggio del servizio di adesione](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [Metriche lato server](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + Analytics {#analytics}
         + [Integrazione dei dati lato server di autenticazione di Adobe Pass in Adobe Analytics](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [Utilizzo dell’ID Experience Cloud nell’autenticazione di Adobe Pass](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + Legacy {#legacy}
      + (Legacy) API REST V1 {#rest-api-v1}
         + [Panoramica di REST API V1](integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md)
         + [Riferimento REST API V1](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + (Legacy) API {#rest-api-v1-apis}
            + [Richiesta codice di registrazione](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [Record di registrazione di ritorno](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [Elimina record di registrazione](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [Fornisci elenco MVPD](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [Avvia autenticazione](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [Controlla token di autenticazione](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [Recupera token di autenticazione](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [Avvia autorizzazione](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [Recupera token di autorizzazione](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [Ottieni token multimediale breve](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [Controlla il flusso di autenticazione tramite l’app web Second Screen](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [Recupera elenco risorse autorizzate](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [Recupera elenco di risorse preautorizzate tramite l’app web Second Screen](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [Avvia disconnessione](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [Metadati utente](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [Recupera richiesta-profilo](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [Scambio di token](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [Anteprima gratuita per Passaggio temporaneo e Passaggio temporaneo promozionale](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + (Legacy) Cookbook {#rest-api-v1-cookbooks}
            + [Manuale dell’API REST V1 (da client a server)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [Manuale dell’API REST V1 (server-to-server)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + (Legacy) SDK {#sdks}
         + (Legacy) JavaScript SDK {#javascript-sdk}
            + [Panoramica dell’SDK per JavaScript](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [Manuale dell’SDK di JavaScript](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [Riferimento API di JavaScript SDK](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [Preautorizzazione API SDK di JavaScript](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + Linee guida (legacy) {#javascript-sdk-guidelines}
               + [Accesso e disconnessione senza aggiornamento](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + (Legacy) SDK iOS/tvOS {#ios-tvos-sdk}
            + [Panoramica dell’SDK iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [Manuale dell’SDK iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [Riferimento API per iOS/tvOS SDK](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [Preautorizzazione API SDK iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + Linee guida (legacy) {#ios-tvos-sdk-guidelines}
               + [Registrazione applicazione iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [Guida alla migrazione ad iOS/tvOS v3.x](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [Controlli di integrità archiviazione iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + (Legacy) Android SDK {#android-sdk}
            + [Panoramica dell’SDK di Android](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [Manuale dell’SDK di Android](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [Riferimento API dell&#39;SDK per Android](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [Preautorizzazione API SDK di Android](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + Linee guida (legacy) {#android-sdk-guidelines}
               + [Registrazione applicazione Android](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [SDK per Android con registrazione client dinamica](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + (Legacy) SDK FireOS {#fireos-sdk}
            + [Panoramica tecnica di Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [Manuale dell’integrazione di Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [Riferimento API di Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [Registrazione applicazione Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [SDK FireOS con registrazione client dinamica](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [Amazon FireOS SSO - Guida introduttiva per il programmatore](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
   + [Panoramica della guida all’integrazione dei programmatori](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [Meccanismo di limitazione](integration-guide-programmers/throttling-mechanism.md)
   + [Requisiti minimi di sistema](integration-guide-programmers/minimum-system-requirements.md)
   + [Flusso adesione programmatore](integration-guide-programmers/entitlement-flow.md)
   + [Casi di utilizzo dei programmatori](integration-guide-programmers/programmer-use-cases.md)
   + [Trasmissione delle informazioni del client (dispositivo, connessione e applicazione)](integration-guide-programmers/passing-client-information-device-connection-and-application.md)
+ Guida All&#39;Integrazione Per MVPD {#integration-guide-mvpds}
   + [Funzioni di integrazione](integration-guide-mvpds/mvpd-integr-features.md)
   + [Autenticazione](integration-guide-mvpds/authn-usecase.md)
   + [Autenticazione tramite il protocollo OAuth 2.0](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [Autorizzazione](integration-guide-mvpds/authz-usecase.md)
   + [Autorizzazione di verifica preliminare](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [Disconnessione MVPD](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [Scambio di metadati del contenuto](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [Scambio di metadati utente](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [Servizio Web MVPD proxy](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [Integrazione SAML MVPD proxy](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [Ambito provider di servizi](integration-guide-mvpds/serv-provider-scoping.md)
   + [Indirizzi IP consentiti MVPD](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ Guida Utente Per Il Dashboard TVE {#user-guide-tve-dashboard}
   + [Panoramica del dashboard di TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [Ambienti](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [Revisione e invio di modifiche](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [Canali](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [Programmatori](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [Integrazioni](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [Rapporti](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [Registro delle modifiche](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
   + [Guida utente di TVE Dashboard](user-guide-tve-dashboard/tve-dashboard-user-guide.md)
+ Note sulla versione {#release-notes}
   + 2024 {#release-notes-2024}
      + [Note sulla versione di Adobe Pass Authentication 3.0.3](notes-releases/auth-rn-303.md)
      + [Note sulla versione di Adobe Pass Authentication 3.0](notes-releases/auth-rn-300.md)
      + [Note sulla versione di Adobe Pass Authentication 2.70](notes-releases/auth-rn-270.md)
      + [Note sulla versione di Adobe Pass Authentication 2.69](notes-releases/auth-rn-269.md)
      + [Note sulla versione di Adobe Pass Authentication JavaScript 4.7.0](notes-releases/authn-rn-javascript-470.md)
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.9.2](notes-releases/authn-rn-ios-tvos-392.md)
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.8.4](notes-releases/authn-rn-ios-tvos-384.md)
   + 2023 {#release-notes-2023}
      + [Note sulla versione di Adobe Pass Authentication 2.68](notes-releases/auth-rn-268.md)
      + [Note sulla versione di Adobe Pass Authentication 2.67](notes-releases/auth-rn-267.md)
      + [Note sulla versione di Adobe Pass Authentication 2.66](notes-releases/auth-rn-266.md)
      + [Note sulla versione di Adobe Pass Authentication 2.65.1](notes-releases/auth-rn-2651.md)
      + [Note sulla versione di Adobe Pass Authentication 2.65](notes-releases/auth-rn-265.md)
      + [Note sulla versione di Adobe Pass Authentication 2.64.1](notes-releases/auth-rn-2641.md)
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.8.3](notes-releases/authn-rn-ios-tvos-383.md)
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.8.2](notes-releases/authn-rn-ios-tvos-382.md)
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.8.1](notes-releases/authn-rn-ios-tvos-381.md)
      + [Note sulla versione di Adobe Pass Authentication Android 3.7.3](notes-releases/authn-rn-android-373.md)
   + 2022 {#release-notes-2022}
      + [Note sulla versione di Adobe Pass Authentication 2.64](notes-releases/auth-rn-264.md)
      + [Note sulla versione di Adobe Pass Authentication 2.63](notes-releases/auth-rn-263.md)
      + [Note sulla versione di Adobe Pass Authentication 2.62.1](notes-releases/auth-rn-2621.md)
      + [Note sulla versione di Adobe Pass Authentication JavaScript 4.6.0](notes-releases/authn-rn-javascript-460.md)
   + 2021 {#release-notes-2021}
      + [Note sulla versione di Adobe Pass Authentication JavaScript 4.4.0](notes-releases/authn-rn-javascript-440.md)
      + [Note sulla versione di Adobe Pass Authentication iOS/tvOS 3.7.0](notes-releases/authn-rn-ios-tvos-370.md)
+ Note tecniche {#tech-notes}
   + Ambienti {#tech-notes-environments}
      + [Informazioni sugli ambienti Adobe](notes-technical/understanding-the-adobe-environments.md)
      + [Configurazione dell’ambiente e test in Pre-Qual](notes-technical/setting-up-your-environment-and-testing-in-prequal.md)
      + [Verificare i flussi di autenticazione e autorizzazione utilizzando il sito di test di Adobe API](notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
   + Esperienza utente {#tech-notes-user-experience}
      + [Come migrare la pagina di accesso MVPD da iFrame a popup](notes-technical/migr-mvpd-login-iframe-popup.md)
      + [Funzione di verifica preliminare: come abilitare, risolvere o determinare il problema](notes-technical/preflight-feature.md)
      + [Consenti MVPD nella finestra di dialogo di selezione](notes-technical/allow-mvpd-selectn-dialog.md)
      + [Impedisci a MVPDs di visualizzare la finestra di dialogo di selezione](notes-technical/prevent-mvpd-selectn-dialog.md)
   + API REST V1 {#tech-notes-rest-api-v1}
      + [Implementazione API senza client - Codici di errore/messaggi con motivo/causa probabile](notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [Flusso API senza client in assenza di ID dispositivo](notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
      + [Clientless: evita di utilizzare &#39;&amp;&#39;reg_code nella richiesta /authenticate](notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Abilitazione dei servizi di adesione Adobe Pass per un programmatore su Xbox 360 e Xbox One Clientless](notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Tipo di dispositivo e metriche senza client](notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + SDK {#tech-notes-sdks}
      + [Domande e risposte sui certificati](notes-technical/certificates-qa.md)
      + [Informazioni sugli ID utente](notes-technical/understanding-user-ids.md)
      + SDK JavaScript {#tech-notes-javascript-sdk}
         + [Valutazione della prevenzione del tracciamento - Apple Safari](notes-technical/tracking-prevention-assessment-apple-safari.md)
         + [Valutazione della prevenzione del tracciamento - Google Chrome](notes-technical/tracking-prevention-assessment-google-chrome.md)
         + [Aggiornamenti dei cookie - Flag SameSite e Secure](notes-technical/cookies-updates-samesite-and-secure-flags.md)
         + [Suggerimenti per il debug](notes-technical/appendix-b-debugging-tips.md)
      + SDK di Android {#tech-notes-android-sdk}
         + [Accesso Single Sign-On (SSO) SDK di Android Enabler sulle app Android 10](notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Autenticazione di Adobe Pass e il nuovo modello di autorizzazioni &quot;Marshmallow&quot; di Android 6](notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + SDK iOS/tvOS {#tech-notes-ios-tvos-sdk}
         + [Supporto WKWebView sull&#39;SDK iOS 3.1+](notes-technical/wkwebview-support-on-ios-sdk-31.md)
         + [Supporto di SFSafariViewController sull&#39;SDK iOS 3.2+](notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO su iOS quando si utilizza Adobe Pass Authentication Access Enabler](notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [Errore di autenticazione iOS - Impossibile trovare adobepass.ios.app](notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Ripristina passaggio temporaneo su iOS](notes-technical/reset-temp-pass-on-ios.md)
         + [Debug dell’SDK iOS/tvOS di AccessEnabler tramite i registri dell’app della console](notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [Percorso di aggiornamento ad AccessEnabler iOS/tvOS 3.7.0](notes-technical/accessenabler-iostvos-370-upgrade-path.md)
   + Risoluzione dei problemi di {#tech-notes-troubleshooting}
      + [Utilizzo di Charles Proxy](notes-technical/using-charles-proxy.md)
      + [Monitoraggio di Adobe Pass Adobe PayTV Pass](notes-technical/monitoring-adobe-pay-tv-pass.md)
