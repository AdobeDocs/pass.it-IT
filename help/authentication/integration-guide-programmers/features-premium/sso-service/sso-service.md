---
title: Servizio Adobe Single Sign-On
description: Scopri il servizio SSO di Adobe Pass che consente l’autenticazione senza soluzione di continuità tra più dispositivi e applicazioni.
exl-id: a1ff85d4-f7d2-4dea-b82f-d29730d9012f
source-git-commit: 151c64276377be5ef21bca4c0d3eaa04ac3da495
workflow-type: tm+mt
source-wordcount: '3615'
ht-degree: 2%

---


# Servizio Adobe Single Sign-On {#sso-service}

Questo documento descrive casi d’uso, endpoint e API per il servizio Adobe Single Sign-On.

**Revisione corrente - 1.0.0**

## Ambito {#scope}

Il servizio Adobe Pass SSO consente l&#39;autenticazione senza soluzione di continuità tra più dispositivi e applicazioni, fornendo un&#39;esperienza utente unificata e mantenendo al contempo gli standard di sicurezza e conformità. Questo servizio risponde alla crescente esigenza di autenticazione tra piattaforme nell’attuale scenario di streaming multi-dispositivo.

## Panoramica {#overview}

### Che cos’è {#what-it-is}

Una soluzione Single Sign-On completa che consente agli utenti di eseguire l&#39;autenticazione una sola volta e di gestire il trasferimento dell&#39;autenticazione in modo informato ad altri dispositivi

### Problemi attuali per l’autenticazione {#current-challenges}

* L’utente deve effettuare l’autenticazione con ogni servizio di streaming a cui è abbonato
* L&#39;utente deve eseguire l&#39;autenticazione separatamente su ogni dispositivo o applicazione
* A causa delle difficoltà di immissione di una password nel flusso di autenticazione su alcune piattaforme, potrebbero verificarsi tassi di abbandono più elevati

### Opportunità per un servizio Adobe Pass SSO {#opportunity}

* Aumento del numero di servizi di streaming che richiedono la propria autenticazione
* Domanda crescente di esperienze multi-dispositivo senza soluzione di continuità
* Aumento del numero di dispositivi di streaming per famiglia
* Necessità di autenticazione unificata per famiglia

### Vantaggi per il business {#business-benefits}

#### Provider di contenuti {#content-providers}

* **Maggiore coinvolgimento degli utenti** - La perfetta esperienza porta a tempi di sessione più lunghi
* **Limitazione ridotta** - Le barriere di autenticazione inferiori aumentano il consumo di contenuti
* **Conservazione migliorata** - Una migliore esperienza utente riduce l&#39;abbandono
* **Riduzione costi** - Meno ticket di supporto relativi a problemi di autenticazione

#### Utenti finali {#end-users}

* **Esperienza perfetta** - Esegui l&#39;autenticazione una sola volta e accedi ovunque
* **Risparmio di tempo** - Nessun processo di accesso ripetuto
* **Flessibilità dispositivo** - Passaggio tra dispositivi senza interruzione
* **Esperienza coerente** - Autenticazione uniforme su tutte le piattaforme

#### IdP (MVPD, Telcos, ecc.) {#idps}

* Gli MVPD possono essere informati di dispositivi aggiuntivi utilizzando un SSO autenticato
* **Maggiore soddisfazione dell&#39;utente** - Migliore esperienza di autenticazione
* **Carico di supporto ridotto** - Meno chiamate di supporto relative all&#39;autenticazione
* **Vantaggio competitivo** - Migliore esperienza utente rispetto alla concorrenza

## Casi d’uso {#use-cases}

### Mappatura identità {#identity-mapping}

Il servizio crea la possibilità di collegare un account D2C e un account TVE nella stessa app.

Un numero maggiore di servizi di streaming sta creando pacchetti da vendere a terzi (MVPD/MVPD virtuale/Telco, ecc.). Gli utenti potrebbero dover gestire più account nella stessa app. Per creare un’esperienza di autenticazione fluida, un servizio deve collegare questi account per semplificare l’esperienza di accesso.

L’utente effettuerà l’autenticazione con il proprio account D2C, quindi l’autenticazione UNA VOLTA con un altro account (ad esempio MVPD). Con il nostro servizio, questi account saranno collegati, il che significa che in futuro, le successive autenticazioni su dispositivi diversi potranno essere eseguite solo con l’account D2C.

### SSO tra dispositivi {#cross-device-sso}

L&#39;autenticazione su un dispositivo connesso alla TV può essere più complicata rispetto a un telefono cellulare. Un&#39;esperienza utente ottimale consiste nell&#39;eseguire l&#39;autenticazione al telefono e quindi passare l&#39;autenticazione alla smart TV.

## Componenti chiave {#key-components}

* **API del token di servizio**: componente chiave che gestisce in modo sicuro il meccanismo Single Sign-On
* **Elenco API** - L&#39;applicazione può aiutare l&#39;utente a comprendere l&#39;elenco dei dispositivi nel suo ecosistema
* **Collega API** - L&#39;applicazione consente all&#39;utente di aggiungere altri dispositivi nel proprio ecosistema
* **Scollega API** - L&#39;applicazione consente all&#39;utente di rimuovere i dispositivi nel suo ecosistema

## Casi d’uso dettagliati {#use-cases-detailed}

### SSO D2C-TVE {#d2c-tve-sso}

Questo caso d’uso consente il collegamento tra le credenziali D2C e TVE(MVPD) su un dispositivo e lo sfruttamento del profilo collegato su altri dispositivi all’interno della stessa applicazione.

![Flusso SSO D2C-TVE](../../../assets/sso_service_d2c_1.png)

![Flusso SSO tra dispositivi](../../../assets/sso_service_d2c_2.png)

### SSO tra dispositivi {#cross-device-sso-detailed}

Questo caso d’uso consente agli utenti che hanno già eseguito l’autenticazione su un dispositivo di riutilizzare il profilo autenticato sulla stessa applicazione D2C o TVE installata su altri dispositivi (applicazione necessaria per implementare Adobe Pass REST V2 per l’autenticazione e l’autorizzazione).

## Integrare il servizio Adobe SSO in un’applicazione D2C-TVE {#integration}

### Passaggio 1 - Ottenere un identificatore comune {#step-1}

Per integrare il servizio SSO di Adobe, un&#39;implementazione dell&#39;applicazione deve stabilire un ID univoco e persistente da utilizzare come attributo SSO dell&#39;identificatore comune in X-SSO-ID. È possibile ottenere questo risultato autenticando un utente con un servizio D2C e conservando un attributo relativo a questa autenticazione.

### Passaggio 2: ottenere un token di servizio {#step-2}

Utilizzando l’identificatore comune in X-SSO-ID nell’endpoint POST /serviceToken, verrà recuperato un JWT firmato con il seguente payload:

```json
{
  "iss": "ssoservicetoken",
  "sub": "unique_common_identifier",
  "nbf": 1758093558,
  "exp": 1758097158,
  "iat": 1758093558
}
```

Il token di servizio ha &quot;iat&quot; - emesso alle e &quot;exp&quot; - ora di scadenza in cui l’intervallo del token di servizio è valido. In caso di scadenza, è possibile ottenere un nuovo JWT utilizzando l’endpoint GET /serviceToken con il token di servizio scaduto.

### Passaggio 3: effettuare l’autenticazione utilizzando l’API REST di Adobe Pass V2 con un MVPD TVE {#step-3}

L&#39;autenticazione con Adobe Pass deve essere implementata utilizzando il token di servizio: [REST API V2 - Flussi token di servizio Single Sign-On](https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-flows/rest-api-v2-single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows)

### Passaggio 4: collegare un altro dispositivo {#step-4}

Dall’applicazione autenticata, è possibile creare un collegamento da utilizzare su un dispositivo diverso utilizzando l’API /link

```json
{
    "status": "CREATED",
    "code": "228128",
    "notBefore": 1758094617220,
    "notAfter": 1758098217220
}
```

Il &quot;codice&quot; è un codice di collegamento di breve durata in una sequenza di 6 cifre che l’utente introdurrà in un’applicazione non autenticata su un dispositivo secondario

### Passaggio 5: recupero dell&#39;autenticazione Single Sign-On {#step-5}

Su un dispositivo diverso, una volta che l’utente ha introdotto il codice, l’applicazione può:

* recuperare identità dal servizio D2C
* recuperare il profilo MVPD da Adobe Pass con API REST V2

Il profilo MVPD sarà valido tramite SSO per il periodo in cui è stata ottenuta l’autenticazione iniziale.

Nel caso in cui MVPD invalidi il profilo o l’utente scelga di disconnettersi da MVPD, l’applicazione che utilizza Adobe Pass REST API V2 non avrà più un record del profilo e dovrà richiedere all’utente di eseguire nuovamente l’autenticazione con MVPD.

### Passaggio 6: gestire il Single Sign-On su altri dispositivi {#step-6}

Un’applicazione può ottenere informazioni su tutti gli altri dispositivi collegati con lo stesso identificatore comune utilizzando l’API /list.

È possibile rimuovere un dispositivo dall&#39;accesso Single Sign-On in qualsiasi momento scollegandolo dall&#39;API /unlink.

## API {#apis}

### API del token di servizio {#service-token-api}

#### Descrizione {#service-token-description}

L’API del token di servizio può essere utilizzata per richiedere e gestire i token di servizio che abilitano la funzionalità Single Sign-On (SSO) tra più applicazioni o dispositivi. Questi token di servizio identificano i profili autenticati (profili SSO) e sono essenziali per stabilire e mantenere connessioni SSO.

>[!WARNING]
>
>I token di servizio contengono informazioni di autenticazione riservate. Le applicazioni devono gestire questi token in modo sicuro e non esporli mai a parti non attendibili.

L’API del token di servizio fornisce due endpoint primari:

* **POST /api/{serviceProvider}/serviceToken** - Ottieni un token di servizio JWS appena creato
* **GET /api/{serviceProvider}/serviceToken** - Aggiorna un token di servizio JWS esistente

Nel caso in cui la richiesta API del token di servizio non possa essere gestita a causa di un errore dei servizi di autenticazione di Adobe Pass, nella risposta API verranno incluse informazioni di errore aggiuntive.

#### POST - serviceToken {#post-service-token}

##### Richiesta {#post-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">metodo</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parametri percorso</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identificatore del provider di servizi per cui viene richiesto il token.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorizzazione</td>
      <td>La generazione del payload del token Bearer è descritta nella documentazione dell'intestazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Id</td>
      <td>
         La generazione del payload dell'identificatore del dispositivo è descritta nella documentazione dell'intestazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.
         <br/><br/>
         Questo identificatore viene utilizzato come identificatore SSO predefinito quando X-SSO-ID non viene fornito.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         Le informazioni sul dispositivo specificate nella documentazione dell'intestazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-x-device-info">X-Device-Info</a>.
         <br/><br/>
         <b>Si consiglia vivamente</b> di utilizzare quando la piattaforma del dispositivo dell'applicazione consente di fornire valori validi in modo esplicito.
         <br/><br/>
         Il backend di autenticazione di Adobe Pass unirà i valori impostati in modo esplicito con i valori estratti in modo implicito. Se non viene fornito, verranno utilizzati i valori estratti predefiniti.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-LINK</td>
      <td>
         Il codice di collegamento che associa questa richiesta a un profilo autenticato esistente. Se fornito, la risposta includerà un token di servizio per SSO con il profilo che ha generato il codice di collegamento.
         <br/><br/>
         Questo viene in genere utilizzato quando un’applicazione o un dispositivo secondario desidera connettersi a un profilo autenticato da un’applicazione o da un dispositivo principale.
      </td>
      <td>obbligatorio se x-sso-id non viene fornito</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-ID</td>
      <td>
         Identificatore comune su cui l'applicazione richiede di basare l'SSO.
         <br/><br/>
         Se fornito, questo identificatore verrà utilizzato per stabilire un profilo SSO comune tra dispositivi e/o applicazioni.
      </td>
      <td>obbligatorio se x-sso-link non viene fornito</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accetta</td>
      <td>
         Tipo di supporto accettato dall'applicazione client.
         <br/><br/>
         Se specificato, deve essere application/json.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>Agente utente dell’applicazione client.</td>
      <td>facoltativo</td>
   </tr>
</table>

##### Risposta {#post-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Codice</th>
      <th style="background-color: #EFF2F7;">Testo</th>
      <th style="background-color: #EFF2F7;">Descrizione</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Creato</td>
      <td>
        Il token di servizio è stato generato e restituito nel corpo della risposta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Richiesta non valida</td>
      <td>
        Richiesta non valida. Il client deve correggere la richiesta e riprovare. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non autorizzato</td>
      <td>
        Il token di accesso non è valido, il client deve ottenere un nuovo token di accesso e riprovare. Per ulteriori dettagli, consulta la documentazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Panoramica registrazione client dinamico</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Errore interno del server</td>
      <td>
        Si è verificato un problema sul lato server. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.
      </td>
   </tr>
</table>

###### Completato {#success-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>201</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">stato</td>
      <td>Stato HTTP (ad esempio, "CREATED")</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Firma Web JSON (JWS) con codifica Base64 contenente il token di servizio. Questo token può essere utilizzato nelle chiamate API successive per identificare il profilo autenticato e abilitare la funzionalità SSO.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoca ms o 0 in caso di errore</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoca ms o 0 in caso di errore</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

###### Errore {#error-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>400, 401, 500</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Il corpo della risposta può fornire informazioni di errore aggiuntive conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

## Esempi {#samples-post-service-token}

### &#x200B;1. Richiedi un nuovo token di servizio (con ID SSO)

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-ID: <sso_id>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    Accept: application/json
```

>[!TAB Risposta]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### &#x200B;2. Richiedere un nuovo token di servizio (con collegamento SSO)

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-LINK: <link_code>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    User-Agent: <user_agent>
    Accept: application/json
```

>[!TAB Risposta]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

#### GET - serviceToken {#get-service-token}

##### Richiesta {#get-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">metodo</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parametri percorso</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identificatore del provider di servizi per cui viene richiesto il token.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorizzazione</td>
      <td>La generazione del payload del token Bearer è descritta nella documentazione dell'intestazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         Un token di servizio ottenuto in precedenza che deve essere aggiornato.
         <br/><br/>
         Questo token deve essere valido o è scaduto di recente per essere idoneo all’aggiornamento.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accetta</td>
      <td>
         Tipo di supporto accettato dall'applicazione client.
         <br/><br/>
         Se specificato, deve essere application/json.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>Agente utente dell’applicazione client.</td>
      <td>facoltativo</td>
   </tr>
</table>

##### Risposta {#get-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Codice</th>
      <th style="background-color: #EFF2F7;">Testo</th>
      <th style="background-color: #EFF2F7;">Descrizione</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Il token di servizio è stato aggiornato e restituito nel corpo della risposta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Richiesta non valida</td>
      <td>
        Richiesta non valida. Il client deve correggere la richiesta e riprovare. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non autorizzato</td>
      <td>
        Il token di accesso o il token di servizio non è valido. Il client deve ottenere un nuovo token di accesso o di servizio e riprovare. Per ulteriori dettagli, consulta la documentazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Panoramica registrazione client dinamico</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Errore interno del server</td>
      <td>
        Si è verificato un problema sul lato server. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.
      </td>
   </tr>
</table>

###### Completato {#success-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>200</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">stato</td>
      <td>Stato HTTP (ad esempio, "OK")</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Firma Web JSON (JWS) con codifica Base64 contenente il token di servizio aggiornato. Questo token può essere utilizzato nelle chiamate API successive per identificare il profilo autenticato e abilitare la funzionalità SSO.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoca ms o 0 in caso di errore</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoca ms o 0 in caso di errore</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

###### Errore {#error-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>400, 401, 500</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Il corpo della risposta può fornire informazioni di errore aggiuntive conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

## Esempi {#samples-get-service-token}

### &#x200B;1. Richiesta di aggiornamento di un token di servizio

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
GET /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Risposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### Collega API {#link-api}

#### Descrizione {#link-description}

L’API di collegamento può essere utilizzata per richiedere un codice di collegamento (o codice QR) che può abilitare il Single Sign-On (SSO) tra più applicazioni o dispositivi. Questo codice di collegamento consente agli utenti di collegare una nuova applicazione o un nuovo dispositivo a un profilo autenticato esistente (profilo SSO), fornendo un’esperienza SSO fluida tra applicazioni o dispositivi.

L&#39;API di collegamento richiede che venga fornito un token di servizio valido nell&#39;intestazione AD-Service-Token.

Il codice di collegamento generato viene in genere visualizzato agli utenti su un&#39;applicazione o un dispositivo principale e immesso su un&#39;applicazione o un dispositivo secondario per stabilire la connessione SSO. Il codice di collegamento ha un periodo di validità limitato (in genere 5-30 minuti) ed è destinato all’uso una tantum.

Nel caso in cui la richiesta API di collegamento non possa essere gestita a causa di un errore dei servizi di autenticazione di Adobe Pass, verranno incluse informazioni di errore aggiuntive come parte del risultato della risposta dell’API di collegamento.

#### POST - collegamento {#post-link}

##### Richiesta {#post-link-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/api/{serviceProvider}/link</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">metodo</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parametri percorso</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identificatore del provider di servizi per cui viene richiesto il token.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorizzazione</td>
      <td>La generazione del payload del token Bearer è descritta nella documentazione dell'intestazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Id</td>
      <td>La generazione del payload dell'identificatore del dispositivo è descritta nella documentazione dell'intestazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         La generazione del token di servizio è descritta nella documentazione dell’API del token di servizio.
         <br/><br/>
         Questo token di servizio identifica il profilo autenticato per il quale verrà generato il codice di collegamento.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accetta</td>
      <td>
         Tipo di supporto accettato dall'applicazione client.
         <br/><br/>
         Se specificato, deve essere application/json.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>Agente utente dell’applicazione client.</td>
      <td>facoltativo</td>
   </tr>
</table>

##### Risposta {#post-link-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Codice</th>
      <th style="background-color: #EFF2F7;">Testo</th>
      <th style="background-color: #EFF2F7;">Descrizione</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Creato</td>
      <td>
        Il codice di collegamento è stato generato e restituito nel corpo della risposta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Richiesta non valida</td>
      <td>
        Richiesta non valida. Il client deve correggere la richiesta e riprovare. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non autorizzato</td>
      <td>
        Il token di accesso non è valido, il client deve ottenere un nuovo token di accesso e riprovare. Per ulteriori dettagli, consulta la documentazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Panoramica registrazione client dinamico</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Errore interno del server</td>
      <td>
        Si è verificato un problema sul lato server. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.
      </td>
   </tr>
</table>

###### Completato {#success-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>201</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">stato</td>
      <td>Stato HTTP (ad esempio, "CREATED")</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">link</td>
      <td>Codice numerico o alfanumerico breve che può essere utilizzato su un'applicazione o un dispositivo secondario per stabilire una connessione SSO.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Il timestamp (in millisecondi dall’epoca) di validità del codice di collegamento.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>La marca temporale (in millisecondi dall’epoca) di scadenza del codice di collegamento.</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

###### Errore {#error-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>400, 401, 500</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Il corpo della risposta può fornire informazioni di errore aggiuntive conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

## Esempi {#samples-post-link}

### &#x200B;1. Richiedi un codice di collegamento per un profilo autenticato esistente

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
POST /api/{serviceProvider}/link HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Risposta]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{            
   "status": "CREATED",
   "code": "123456",
   "notBefore": 1623840000000,
   "notAfter": 1623842700000
}
```

>[!ENDTABS]

### Scollega API {#unlink-api}

#### Descrizione {#unlink-description}

L’API Unlink può essere utilizzata per richiedere la rimozione di uno o più dispositivi da un profilo autenticato (profilo SSO). Questa API consente agli utenti di disconnettere i dispositivi dalla configurazione SSO, fornendo il controllo su quali dispositivi hanno accesso al loro profilo autenticato.

>[!WARNING]
>
>L&#39;API Unlink richiede che venga fornito un token di servizio valido nell&#39;intestazione AD-Service-Token.

Nel caso in cui la richiesta Unlink API non possa essere gestita a causa di un errore dei servizi di autenticazione di Adobe Pass, verranno incluse informazioni di errore aggiuntive come parte del risultato della risposta Unlink API.

#### POST - scollega {#post-unlink}

##### Richiesta {#post-unlink-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/api/{serviceProvider}/unlink</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">metodo</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parametri percorso</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identificatore del provider di servizi.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parametri corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">dispositivi</td>
      <td>
         Array di identificatori di dispositivo da scollegare.
         <br/><br/>
         Esempio:<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorizzazione</td>
      <td>La generazione del payload del token Bearer è descritta nella documentazione dell'intestazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         Tipo di file multimediale accettato per le risorse inviate.
         <br/><br/>
         Deve essere application/json.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Id</td>
      <td>La generazione del payload dell'identificatore del dispositivo è descritta nella documentazione dell'intestazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         La generazione del token di servizio è descritta nella documentazione dell’API del token di servizio.
         <br/><br/>
         Questo token di servizio identifica il profilo autenticato per il quale verranno scollegati i dispositivi.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accetta</td>
      <td>
         Tipo di supporto accettato dall'applicazione client.
         <br/><br/>
         Se specificato, deve essere application/json.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>Agente utente dell’applicazione client.</td>
      <td>facoltativo</td>
   </tr>
</table>

##### Risposta {#post-unlink-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Codice</th>
      <th style="background-color: #EFF2F7;">Testo</th>
      <th style="background-color: #EFF2F7;">Descrizione</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Scollegamento dei dispositivi richiesti dalla configurazione SSO completato.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Richiesta non valida</td>
      <td>
        Richiesta non valida. Il client deve correggere la richiesta e riprovare. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non autorizzato</td>
      <td>
        Il token di accesso non è valido, il client deve ottenere un nuovo token di accesso e riprovare. Per ulteriori dettagli, consulta la documentazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Panoramica registrazione client dinamico</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Metodo non consentito</td>
      <td>
        Metodo HTTP non valido. Il client deve utilizzare un metodo HTTP consentito per la risorsa richiesta e riprovare.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Errore interno del server</td>
      <td>
        Si è verificato un problema sul lato server. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.
      </td>
   </tr>
</table>

###### Completato {#success-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>200</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">stato</td>
      <td>Informazioni sul risultato operativo: "OK"</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">unlinkedDevices</td>
      <td>
         Elenco dei dispositivi scollegati correttamente.
         <br/><br/>
         Esempio:<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

###### Errore {#error-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>400, 401, 405, 500</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Il corpo della risposta può fornire informazioni di errore aggiuntive conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

## Esempi {#samples-post-unlink}

### &#x200B;1. Richiesta di scollegare dispositivi da un profilo SSO

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!TAB Risposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### &#x200B;2. Richiesta di scollegare i dispositivi con successo parziale

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "unknowndevice",
    "deviceid3"
  ]
}
```

>[!TAB Risposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### Elenco API {#list-api}

#### Descrizione {#list-description}

L’API elenco può essere utilizzata per ottenere un elenco dei dispositivi attualmente connessi a un profilo autenticato (profilo SSO). Questa API consente agli utenti e alle applicazioni di visualizzare quali dispositivi fanno parte della configurazione SSO, fornendo visibilità e funzionalità di gestione per la loro esperienza autenticata su più dispositivi.

>[!WARNING]
>
>L&#39;API elenco richiede che venga fornito un token di servizio valido nell&#39;intestazione AD-Service-Token.

L’API Elenco restituisce i dettagli di ciascun dispositivo nel profilo autenticato (profilo SSO), inclusi gli identificatori del dispositivo e i metadati che possono aiutare gli utenti a riconoscere i propri dispositivi. Queste informazioni possono essere utilizzate per aiutare gli utenti a prendere decisioni informate sui dispositivi da mantenere nella configurazione SSO.

Nel caso in cui la richiesta List API non possa essere gestita a causa di un errore di Adobe Pass Authentication Services, verranno incluse informazioni di errore aggiuntive come parte del risultato della risposta List API.

#### GET - elenco {#get-list}

##### Richiesta {#get-list-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/api/{serviceProvider}/list</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">metodo</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parametri percorso</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identificatore del provider di servizi.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorizzazione</td>
      <td>La generazione del payload del token Bearer è descritta nella documentazione dell'intestazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Id</td>
      <td>La generazione del payload dell'identificatore del dispositivo è descritta nella documentazione dell'intestazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         La generazione del token di servizio è descritta nella documentazione dell’API del token di servizio.
         <br/><br/>
         Questo token di servizio identifica il profilo autenticato per il quale verrà recuperato l’elenco dei dispositivi.
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accetta</td>
      <td>
         Tipo di supporto accettato dall'applicazione client.
         <br/><br/>
         Se specificato, deve essere application/json.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>Agente utente dell’applicazione client.</td>
      <td>facoltativo</td>
   </tr>
</table>

##### Risposta {#get-list-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Codice</th>
      <th style="background-color: #EFF2F7;">Testo</th>
      <th style="background-color: #EFF2F7;">Descrizione</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        L'elenco dei dispositivi nella configurazione SSO è stato recuperato e restituito nel corpo della risposta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Richiesta non valida</td>
      <td>
        Richiesta non valida. Il client deve correggere la richiesta e riprovare. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non autorizzato</td>
      <td>
        Il token di accesso non è valido, il client deve ottenere un nuovo token di accesso e riprovare. Per ulteriori dettagli, consulta la documentazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Panoramica registrazione client dinamico</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Metodo non consentito</td>
      <td>
        Metodo HTTP non valido. Il client deve utilizzare un metodo HTTP consentito per la risorsa richiesta e riprovare.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Errore interno del server</td>
      <td>
        Si è verificato un problema sul lato server. Il corpo della risposta può contenere informazioni di errore conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.
      </td>
   </tr>
</table>

###### Completato {#success-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>200</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">dispositivi</td>
      <td>
         JSON contenente una mappa di coppie chiave-valore.
         <br/><br/>
         <b>Chiave:</b> deviceId - Payload dell'identificatore del dispositivo come descritto nella documentazione dell'intestazione <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>
         <br/><br/>
         <b>Valore:</b> attributi - JSON contenente una mappa di attributi di metadati del dispositivo, tra cui:
         <ul>
            <li>tipo di dispositivo</li>
            <li>piattaforma</li>
            <li>agente utente</li>
            <li>altri metadati rilevanti che consentono di identificare il dispositivo</li>
         </ul>
         I valori per gli attributi possono essere semplici (stringa, numero intero, booleano, ecc.)
      </td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

###### Errore {#error-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Stato</td>
      <td>400, 401, 405, 500</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Il corpo della risposta può fornire informazioni di errore aggiuntive conformi alla documentazione di <a href="https://experienceleague.adobe.com/it/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codici di errore avanzati</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

## Esempi {#samples-get-list}

### &#x200B;1. Richiesta di elencare i dispositivi in un profilo SSO

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Risposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {
    "deviceid1": {
      "deviceType": "smartTV",
      "model": "Samsung",
      "os": "Tizen",
      "osVersion": "5.0",
      "lastSeen": 1623840000000,
      "type": "regular"
    },
    "deviceid2": {
      "deviceType": "mobile",
      "model": "iPhone",
      "os": "iOS",
      "osVersion": "14.5",
      "lastSeen": 1623830000000,
      "type": "sso"
    },
    "deviceid3": {
      "deviceType": "tablet",
      "model": "iPad",
      "os": "iPadOS",
      "osVersion": "14.5",
      "lastSeen": 1623820000000,
      "type": "sso"
    }
  }
}
```

>[!ENDTABS]

### &#x200B;2. Richiesta di elencare i dispositivi senza dispositivi collegati

>[!BEGINTABS]

>[!TAB Richiesta]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Risposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {}
}
```

>[!ENDTABS]

## Codici errore {#error-codes}

### Struttura delle risposte di errore {#error-structure}

Tutte le risposte di errore includono i campi seguenti:

| Campo | Tipo | Descrizione |
|:---|:---|:---|
| stato | Intero | Codice di stato HTTP (400, 401, 500, ecc.) |
| codice | Stringa | Codice di errore leggibile al computer |
| messaggio | Stringa | Descrizione leggibile |
| azione | Stringa | Azione suggerita per il client |
| helpUrl | Stringa | Collegamento alla documentazione |
| traccia | Stringa | ID richiesta univoco (UUID) per la correlazione |

**Esempio di risposta all&#39;errore**

```json
{
  "status": "BAD_REQUEST",
  "error": {
    "status": 400,
    "code": "header_missing",
    "message": "Required header is missing",
    "action": "check_headers",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=it",
    "trace": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  }
}
```

**Esempio di risposta riuscita**

```json
{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIs...",
  "notBefore": 1697500800000,
  "notAfter": 1697587200000
}
```

>[!NOTE]
>
>Le risposte di successo NON includono un campo di errore. Le risposte di errore NON includono campi di operazione riuscita.

### Catalogo codici errore {#error-catalog}

#### Errori generali

| codice | stato | messaggio | azione |
|:---|:---|:---|:---|
| token_invalid | 400 | Il token fornito non è valido | get_new_token |
| token_scaduto | 401 | Il token è scaduto | get_new_token |
| header_missing | 400 | Manca un’intestazione richiesta | check_headers |
| non autorizzato | 401 | Accesso non autorizzato | nessuno |
| internal_error | 500 | Errore interno | nessuno |

#### Errori di convalida

| codice | stato | messaggio | azione |
|:---|:---|:---|:---|
| request_null | 400 | L’oggetto della richiesta non può essere nullo | nessuno |

#### Errori POST ServiceToken

| codice | stato | messaggio | azione |
|:---|:---|:---|:---|
| header_missing | 400 | Per le richieste POST è necessaria l&#39;intestazione x-sso-id o x-sso-link | check_headers |
| header_missing | 400 | L’intestazione AP-Device-Identifier è necessaria per le richieste POST | check_headers |

#### Errori ServiceToken di GET

| codice | stato | messaggio | azione |
|:---|:---|:---|:---|
| header_missing | 400 | L’intestazione AD-Service-Token è richiesta per le richieste GET | check_headers |
| header_invalid | 401 | Firma JWT non valida nel token AD-Service | get_new_token |
| header_invalid | 401 | Errore durante la convalida della firma JWT | get_new_token |
| header_invalid | 401 | Oggetto JWT (sub) mancante o vuoto in AD-Service-Token | get_new_token |
| header_invalid | 401 | Errore durante l’estrazione dell’oggetto JWT | get_new_token |

#### Errori di convalida dei collegamenti

| codice | stato | messaggio | azione |
|:---|:---|:---|:---|
| header_missing | 401 | L’intestazione AD-Service-Token è richiesta per le richieste di collegamento | check_headers |
| header_invalid | 401 | Firma JWT non valida nel token AD-Service | get_new_token |
| header_invalid | 401 | Errore durante la convalida della firma JWT | get_new_token |

#### Scollega errori di convalida

| codice | stato | messaggio | azione |
|:---|:---|:---|:---|
| header_missing | 401 | L’intestazione AD-Service-Token è richiesta per le richieste di scollegamento | check_headers |
| header_invalid | 401 | Firma JWT non valida nel token AD-Service | get_new_token |
| header_invalid | 401 | Errore durante la convalida della firma JWT | get_new_token |
| header_invalid | 401 | Oggetto JWT (sub) mancante o vuoto in AD-Service-Token | get_new_token |
| request_invalid | 400 | L&#39;elenco dei dispositivi non può essere nullo o vuoto | check_request_body |

#### Elenca errori di convalida

| codice | stato | messaggio | azione |
|:---|:---|:---|:---|
| header_missing | 401 | L’intestazione AD-Service-Token è richiesta per le richieste di elenco | check_headers |
| header_invalid | 401 | Firma JWT non valida nel token AD-Service | get_new_token |
| header_invalid | 401 | Errore durante la convalida della firma JWT | get_new_token |
| header_invalid | 401 | Oggetto JWT (sub) mancante o vuoto in AD-Service-Token | get_new_token |
| header_invalid | 401 | Errore durante l’estrazione dell’oggetto JWT | get_new_token |

