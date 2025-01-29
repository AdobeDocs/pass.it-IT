---
title: Glossario REST API V2
description: Glossario REST API V2
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '1744'
ht-degree: 0%

---

# Glossario REST API V2 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questo documento fornisce le definizioni dei termini utilizzati durante l’integrazione dell’API REST di autenticazione di Adobe Pass V2.

>[!MORELIKETHIS]
>
> * [Glossario DCR (Dynamic Client Registration)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## Termini del glossario {#glossary-terms}

### A {#a}

#### Autenticazione {#authentication}

L&#39;autenticazione è un processo che consente a un utente di provare la propria identità a un [Programmatore](#programmer), di accedere al contenuto protetto ([risorsa](#resource)), dopo aver convalidato l&#39;abbonamento utente con [MVPD](#mvpd).

#### Codice di autenticazione {#code}

Il codice di autenticazione è un concetto di autenticazione Adobe Pass che memorizza un valore univoco generato quando un utente avvia il processo di [autenticazione](#authentication) e identifica in modo univoco la [sessione di autenticazione](#session) di un utente fino al completamento del processo di autenticazione.

Il codice di autenticazione può essere utilizzato da un&#39;applicazione [primaria (programmatore)](#primary-application) o da un&#39;applicazione [secondaria (programmatore)](#secondary-application) per completare il processo [authentication](#authentication), recuperare informazioni sulla [sessione di autenticazione](#session) o accedere all&#39;utente [profile](#profile).

Sinonimo di codice di registrazione utilizzato in precedenza.

#### Sessione di autenticazione {#session}

La sessione di autenticazione è un concetto di autenticazione Adobe Pass che memorizza informazioni sul processo di autenticazione dell&#39;utente avviato (o continuato) da un&#39;applicazione [Programmer](#programmer) ed è identificato in modo univoco da un [codice di autenticazione](#code).

La sessione di autenticazione può inoltre indicare all&#39;applicazione [Programmer](#programmer) di procedere con il processo [authorization](#authorization) come fase successiva del flusso [entitlement](#entitlement) nel caso in cui l&#39;utente sia già autenticato.

#### Autorizzazione {#authorization}

L&#39;autorizzazione è un processo che consente a un utente di accedere al contenuto protetto ([resource](#resource)) da un catalogo [Programmer](#programmer) basato sulla sottoscrizione di proprietà [MVPD](#mvpd), dopo aver convalidato i diritti utente con [MVPD](#mvpd).

### C {#c}

#### Configurazione {#configuration}

La configurazione è un concetto di autenticazione Adobe Pass che memorizza informazioni sulle impostazioni di integrazione [Programmer](#programmer) e [MVPD](#mvpd) e può essere utilizzata durante il processo di [autenticazione](#authentication) quando si richiede all&#39;utente di selezionare il proprio [Provider TV](#tv-provider) da un elenco di integrazioni attive.

### D {#d}

#### Decisione {#decision}

La decisione è un concetto di autenticazione Adobe Pass che memorizza informazioni sull&#39;interrogazione del processo [MVPD](#mvpd) [authorization](#authorization) o [preauthorization](#preauthorization) per consentire o negare l&#39;accesso dell&#39;utente a un contenuto protetto da [Programmer](#programmer).

#### Degradazione {#degradation}

La degradazione è una funzionalità di autenticazione di Adobe Pass che consente a un utente di accedere a contenuto protetto anche quando il relativo [MVPD](#mvpd) subisce un&#39;interruzione del servizio.

Per ulteriori informazioni, consulta la documentazione [Panoramica API di degradazione](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md).

#### ID dispositivo {#device-id}

L&#39;ID dispositivo è un identificatore univoco associato al dispositivo dell&#39;utente e deve essere fornito dall&#39;applicazione [Programmer](#programmer) in tutte le fasi del flusso [entitlement](#entitlement).

### E {#e}

#### Diritto {#entitlement}

Il diritto è un concetto di autenticazione di Adobe Pass che incorpora i flussi e le funzionalità disponibili che consentono a un utente di passare attraverso fasi diverse per accedere a contenuti protetti, che vanno dalla [autenticazione](#authentication), alla [preautorizzazione](#preauthorization), alla [autorizzazione](#authorization) e infine alla [disconnessione](#logout).

#### Codice di errore avanzato {#enhanced-error-code}

Il codice di errore avanzato è un concetto di autenticazione Adobe Pass che fornisce informazioni aggiuntive sull’errore che si è verificato durante l’elaborazione di una richiesta.

Per ulteriori informazioni, consulta la documentazione sui [codici di errore avanzati](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

### H {#h}

#### HBA {#hba}

L&#39;HBA (Home-Based Authentication) è il processo mediante il quale a un consumatore viene concesso automaticamente l&#39;accesso al contenuto di [TV Everywhere (TVE)](#tve) su alcuni dispositivi connessi alla rete domestica, che fa parte della posizione all&#39;interno del contratto di abbonamento.

### I {#i}

#### Provider di identità {#identity-provider}

Il provider di identità è un&#39;azienda che fornisce servizi di identità ai consumatori tramite servizi via cavo, satellite o Internet nel contesto di [TV Everywhere (TVE)](#tve).

Sinonimo di [MVPD](#mvpd) e [Provider TV](#tv-provider).

### L {#l}

#### Disconnetti {#logout}

La disconnessione è un processo che consente a un utente di terminare il proprio [profilo](#profile) autenticato all&#39;interno di Adobe Pass Authentication e aggiornare l&#39;applicazione [Programmer](#programmer) in modo da riflettere lo stato dell&#39;utente.

### M {#m}

#### Token multimediale {#media-token}

Il token multimediale è un token generato dall&#39;autenticazione di Adobe Pass a seguito di una [decisione](#decision) di autorizzazione destinata a fornire accesso al contenuto protetto.

Il token multimediale viene passato al [Programmatore](#programmer), che lo convalida per garantire la sicurezza dell&#39;accesso per tale [risorsa](#resource).

Sinonimo del precedente termine utilizzato token di autorizzazione breve.

#### Media Token Verifier {#media-token-verifier}

Il Media Token Verifier è una libreria distribuita da Adobe Pass Authentication responsabile della verifica dell&#39;autenticità di un [token multimediale](#media-token).

Per ulteriori informazioni, consulta la documentazione di [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

#### MVPD {#mvpd}

MVPD (Multichannel Video Programming Distribor) è un&#39;azienda che fornisce servizi televisivi ai consumatori via cavo, satellite o su Internet.

MVPD è identificato da un valore univoco definito durante il processo di onboarding tra MVPD e Adobe.

Sinonimo di [Provider TV](#tv-provider) e [Provider identità](#identity-provider).

### P {#p}

#### Partner {#partner}

Il partner è un&#39;azienda che fornisce un servizio o un framework a un [programmatore](#programmer) per abilitare un&#39;esperienza utente single sign-on.

Il partner è identificato da un valore univoco (ad esempio, &quot;apple&quot;) definito durante il processo di onboarding tra il partner e Adobe.

#### Preautorizzazione {#preauthorization}

La preautorizzazione è un processo che consente a un utente di visualizzare in anteprima un sottoinsieme di [risorse](#resource) da un catalogo [Programmer](#programmer) a cui avrebbe diritto dopo aver convalidato i diritti utente con [MVPD](#mvpd).

Sinonimo di [Verifica preliminare](#preflight).

#### Verifica preliminare {#preflight}

La verifica preliminare è un processo che consente a un utente di visualizzare in anteprima un sottoinsieme di [risorse](#resource) da un catalogo [Programmatore](#programmer) a cui avrebbe diritto dopo aver convalidato i diritti utente con [MVPD](#mvpd).

Sinonimo di [Preautorizzazione](#preauthorization).

#### Applicazione principale (programmatore) {#primary-application}

L&#39;applicazione primaria fa riferimento a un&#39;applicazione [Programmer](#programmer) che avvia [autenticazione](#authentication), ma che potrebbe non essere in grado di completare il processo utilizzando un [agente utente](#user-agent) per passare alla pagina di accesso [MVPD](#mvpd).

#### Profilo {#profile}

Il profilo è un concetto di autenticazione di Adobe Pass che memorizza informazioni sulla data di inizio e di fine dell&#39;autenticazione dell&#39;utente, i metadati dell&#39;utente [](#user-metadata) insieme ad altri campi che indicano il metodo per ottenere l&#39;autenticazione (ad esempio, &quot;regolare&quot;, &quot;degradato&quot;, &quot;temporaneo&quot;, &quot;single sign-on&quot;, ecc.).

Sinonimo del precedente termine utilizzato come token di autenticazione.

#### Programmatore {#programmer}

Il programmatore è un’azienda che fornisce contenuti ai consumatori attraverso canali di proprietà (brand) su varie piattaforme.

Il programmatore raggruppa più canali di proprietà (brand) come [provider di servizi](#service-provider) nell&#39;integrazione con l&#39;autenticazione Adobe Pass.

#### MVPD proxy {#proxy-mvpd}

Il proxy MVPD è una società che fornisce servizi di identità per altri MVPD e si integra direttamente con l’autenticazione di Adobe Pass.

#### MVPD proxy {#proxied-mvpd}

Il proxy MVPD è una società che non dispone di un&#39;integrazione diretta con l&#39;autenticazione di Adobe Pass, ma viene integrato tramite un [proxy MVPD](#proxy-mvpd).

#### Identità piattaforma {#platform-identity}

L&#39;identità della piattaforma è un payload univoco dell&#39;identificatore della piattaforma generato da un servizio o da un framework (libreria) associato al dispositivo dell&#39;utente e fornito al [programmatore](#programmer) per abilitare un&#39;esperienza utente Single Sign-On.

Per ulteriori informazioni, consulta la documentazione [Single Sign-on using Platform identity flows](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

### R {#r}

#### Risorsa {#resource}

La risorsa è un contenuto protetto a cui un utente sta tentando di accedere da un catalogo [Programmer](#programmer).

La risorsa è identificata da un valore univoco concordato tra il programmatore e gli MVPD.

Per ulteriori informazioni, consulta la documentazione di [Risorse protette](/help/authentication/integration-guide-programmers/features-standard/entitlements/protected-resources.md#identifiers).

### S {#s}

#### SAML {#saml}

Il linguaggio SAML (Security Assertion Markup Language) è uno standard aperto per lo scambio di dati di autenticazione e autorizzazione tra parti, in particolare tra un [provider di identità](#identity-provider) e un [provider di servizi](#sp).

#### Applicazione secondaria (programmatore) {#secondary-application}

L&#39;applicazione secondaria fa riferimento a un&#39;applicazione [Programmer](#programmer) in grado di completare il processo [authentication](#authentication) utilizzando un [agente utente](#user-agent) per passare alla pagina di accesso [MVPD](#mvpd).

L’applicazione secondaria può essere eseguita sullo stesso dispositivo dell’applicazione primaria o su un dispositivo (secondario) diverso, nel qual caso l’esperienza di accesso è definita &quot;esperienza utente di autenticazione alla seconda schermata&quot;.

#### Token servizio {#service-token}

Il token di servizio è un identificatore utente univoco generato da un servizio o da un framework (libreria) associato all&#39;utente e viene fornito al [programmatore](#programmer) per abilitare un&#39;esperienza utente Single Sign-On.

Per ulteriori informazioni, consulta la documentazione [Single Sign-on using Service Token Flow](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

#### Provider di servizi {#service-provider}

Il provider di servizi è un canale (marchio) di proprietà di un [programmatore](#programmer).

Il fornitore di servizi è identificato da un valore univoco definito durante il processo di onboarding tra il programmatore e Adobe.

Sinonimo del precedente termine ID richiedente utilizzato.

#### SLO {#slo}

Il Single Logout (SLO) è un processo che consente a un utente di disconnettersi da tutte le applicazioni che facevano parte del [Single Sign-On (SSO)](#sso).

#### SP {#sp}

Il provider di servizi (SP) fa riferimento al ruolo svolto dall&#39;autenticazione Adobe Pass per conto di un [programmatore](#programmer) in un&#39;integrazione con un [MVPD](#mvpd).

#### SSO {#sso}

Il Single Sign-On (SSO) è un processo che consente a un utente di eseguire l&#39;autenticazione una sola volta e di accedere a più applicazioni [Programmer](#programmer) senza dover eseguire l&#39;autenticazione per ciascuna di esse.

### T {#t}

#### TempPass Basic {#temp-pass-basic}

TempPass di base è una funzionalità di autenticazione di Adobe Pass che consente a un utente di accedere a contenuti protetti per un periodo di tempo limitato senza dover eseguire l&#39;autenticazione con un [MVPD](#mvpd).

Per ulteriori informazioni, consulta la documentazione di [Passaggio temporaneo](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md).

#### TempPass promozionale {#temp-pass-promotional}

Il TempPass promozionale è una funzionalità di autenticazione di Adobe Pass che consente a un utente di accedere a contenuti protetti per un numero massimo di risorse e un tempo limitato senza dover eseguire l&#39;autenticazione con un [MVPD](#mvpd).

Per ulteriori informazioni, consulta la documentazione di [Passaggio temporaneo promozionale](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md).

#### TTL {#ttl}

Il valore TTL (time to live) indica il periodo di tempo per il quale un’entità sottostante è valida.

Il TTL può essere menzionato per un [token di accesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token), un [profilo](#profile), un&#39;autorizzazione [decisione](#decision) o un [token multimediale](#media-token).

#### TVE {#tve}

TV Everywhere (TVE) è una nicchia di settore che consente ai consumatori di accedere ai propri programmi TV preferiti, film e altri contenuti su più dispositivi, come smartphone, tablet, laptop e molto altro ancora.

#### Dashboard TVE {#tve-dashboard}

Il dashboard TV Everywhere (TVE) è uno strumento di autenticazione Adobe Pass fornito a [Programmatori](#programmer) per gestire la configurazione e i dati.

Per ulteriori informazioni, consulta la [Guida utente di TVE Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md).

#### Provider TV {#tv-provider}

Il fornitore di servizi televisivi è una società che fornisce servizi televisivi ai consumatori via cavo, via satellite o via Internet.

Il fornitore del televisore è identificato da un valore univoco definito durante la procedura di onboarding tra il fornitore del televisore e l’Adobe.

Sinonimo di [MVPD](#mvpd) e [Identity Provider](#identity-provider).

### U {#u}

#### Agente utente {#user-agent}

L&#39;agente utente fa riferimento a un browser o a un componente simile (specifico per piattaforma) in grado di navigare nel Web e di eseguire il rendering della pagina di accesso [MVPD](#mvpd).

#### ID utente {#user-id}

L&#39;ID utente è un identificatore univoco associato all&#39;utente e proviene dal processo di autenticazione [MVPD](#mvpd).

#### Metadati utente {#user-metadata}

I metadati dell&#39;utente fanno riferimento ad attributi specifici dell&#39;utente (ad esempio, codici postali, valutazioni dei genitori, ID utente, ecc.) gestiti da [MVPD](#mvpd) e forniti dall&#39;autenticazione Adobe Pass come parte di un [profilo](#profile).

Per ulteriori informazioni, consulta la documentazione di [Metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

### V {#v}

#### VSA {#vsa}

L&#39;account VSA (Video Subscriber Account) è un framework sviluppato da Apple fornito al [programmatore](#programmer) per abilitare un&#39;esperienza utente single sign-on.

Per ulteriori informazioni, consulta la documentazione [Framework account sottoscrittore video](https://developer.apple.com/documentation/videosubscriberaccount) e [Single Sign-On tramite flussi partner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).
