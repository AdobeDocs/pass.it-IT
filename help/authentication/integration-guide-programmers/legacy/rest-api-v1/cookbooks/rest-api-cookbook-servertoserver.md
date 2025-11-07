---
title: Manuale dell’API REST (server-to-server)
description: Rest API cookbook server to server.
exl-id: 36ad4a64-dde8-4a5f-b0fe-64b6c0ddcbee
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1856'
ht-degree: 0%

---

# (Legacy) Manuale REST API (server-to-server) {#rest-api-cookbook-server-to-server}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Panoramica {#overview}

Lo scopo di questo documento di manuale è quello di descrivere nel dettaglio le best practice per l’implementazione dell’autenticazione Adobe Pass utilizzando le architetture server-to-server.  Fornisce requisiti di base, implementazione di flusso dettagliata e considerazioni generali sugli ambienti di produzione e sul funzionamento.

### Meccanismo di limitazione

L&#39;API REST per l&#39;autenticazione di Adobe Pass è gestita da un [meccanismo di limitazione](/help/authentication/integration-guide-programmers/throttling-mechanism.md).


## Componenti {#components}

In una soluzione server-to-server funzionante sono coinvolti i seguenti componenti:


| Tipo | Componente | Descrizione |
| --- | --- | --- |
| Dispositivo di streaming | App di streaming | L&#39;applicazione Programmer che risiede sul dispositivo di streaming dell&#39;utente e riproduce video autenticati. |
| | \[Facoltativo\] Modulo AuthN | Se il dispositivo di streaming dispone di un agente utente (ad esempio, un browser web), il modulo AuthN è responsabile dell’autenticazione dell’utente sul MVPD IdP. |
| \[Facoltativo\] Dispositivo AuthN | App autenticazione | Se il dispositivo di streaming non dispone di un agente utente (ad esempio, un browser web), l’applicazione AuthN è un’applicazione web Programmer a cui si accede dal dispositivo di un utente separato utilizzando un browser web. |
| Infrastruttura del programmatore | Servizio programmatore | Servizio che collega il dispositivo di streaming al servizio Adobe Pass per ottenere le decisioni di autenticazione e autorizzazione. |
| Infrastruttura Adobe | Servizio Adobe Pass | Servizio che si integra con il servizio MVPD IdP e AuthZ e fornisce decisioni di autenticazione e autorizzazione. |
| Infrastruttura MVPD | IdP MVPD | Endpoint MVPD che fornisce un servizio di autenticazione basato su credenziali per convalidare l&#39;identità dell&#39;utente. |
| | Servizio MVPD AuthZ | Un endpoint MVPD che fornisce decisioni di autorizzazione in base agli abbonamenti dell’utente, al controllo genitori, ecc. |

## Flussi {#flows}

### Registrazione Dynamic Client (DCR)


Adobe Pass utilizza il DCR per proteggere le comunicazioni client tra un’applicazione o un server di programmazione e i servizi Adobe Pass. Il flusso DCR è separato e descritto nella documentazione [Panoramica registrazione client dinamica](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


### Autenticazione (authN)

Il flusso di autenticazione consente a un utente di identificarsi
al proprio MVPD per determinare se l’utente dispone di un account valido.

1. L’utente avvia l’app Streaming Device e tenta di accedere o visualizzare contenuti protetti.
2. L’app Streaming Device richiede al servizio Programmatori di determinare se il dispositivo è già autenticato.
3. Il servizio Programmatore registra l’app utilizzando DCR.
4. Il servizio Programmatore controlla lo stato di autenticazione del dispositivo di streaming chiamando l&#39;API **checkauthn** del servizio Adobe Pass.
5. Nel caso in cui la chiamata **checkauthn** restituisca lo stato di autenticazione del dispositivo utente, l&#39;app può procedere al flusso di autorizzazione.
6. Nel caso in cui la chiamata **checkauthn** restituisca lo stato che indica che il dispositivo utente NON è autenticato, l&#39;app deve attendere la richiesta di accesso di un utente.
7. Quando l’utente richiede di accedere direttamente (ad esempio, seleziona il pulsante di accesso) o indirettamente (ad esempio, seleziona contenuti protetti quando non è già autenticato), l’app Streaming Device invia una richiesta al servizio Programmatore per avviare l’autenticazione dell’utente. Il servizio programmatore richiede e riceve un codice di registrazione univoco (regcode) chiamando l&#39;API **regcode** del servizio Adobe Pass.
8. Il servizio programmatore recupera inoltre l&#39;elenco degli MVPD e degli attributi correnti chiamando l&#39;API **config** del servizio Adobe Pass. Nota: questa API può anche essere chiamata in una fase precedente del flusso e memorizzata nella cache.
9. Il servizio Programmatore restituisce il codice regcode all&#39;app Streaming Device e all&#39;elenco MVPD elaborato richiesto nel passaggio \#7. Nota: il formato di elenco MVPD elaborato è specificato dal programmatore e può essere filtrato per consentire o bloccare in modo esplicito MVPD specifici (ovvero elenchi consentiti o bloccati).
10. Se è diverso dal dispositivo AuthN (ovvero &quot;seconda schermata&quot;), per scelta o necessità (ovvero il dispositivo di streaming non supporta un agente utente), il dispositivo di streaming deve visualizzare il codice regcode e un URI affinché l’utente possa accedere all’applicazione AuthN. L&#39;utente digita l&#39;URI nell&#39;agente utente sul dispositivo AuthN per avviare l&#39;applicazione AuthN, quindi digita il codice regcode in tale applicazione. Se il dispositivo di streaming è lo stesso del dispositivo AuthN, il codice regcode può essere passato in modo programmatico al modulo AuthN.
11. Il modulo AuthN avvia l’autenticazione utente con MVPD visualizzando un selettore MVPD. Dopo che l&#39;utente ha selezionato MVPD, il modulo AuthN chiama **authenticate** con il codice regcode, che reindirizza l&#39;agente utente all&#39;IdP di MVPD. Quando l’utente si autentica correttamente con MVPD, l’agente utente viene reindirizzato indietro tramite il servizio Adobe Pass, dove l’autenticazione corretta viene registrata con il codice regcode e quindi reindirizzato al modulo AuthN.
12. Se il dispositivo di streaming è diverso dal dispositivo AuthN, quest’ultimo deve visualizzare un messaggio di autenticazione all’utente e i passaggi per continuare (esempio: &quot;Operazione riuscita\! Ora puoi tornare alla console di gioco per continuare \[...\]&quot;). Se il dispositivo di streaming è lo stesso del dispositivo di autenticazione, il dispositivo di streaming può rilevare a livello di programmazione il completamento dell&#39;autenticazione.



Il diagramma seguente illustra il flusso di autenticazione:

![](../../../../assets/authn-flow.png)

### Autorizzazione (authZ)

Il flusso di autorizzazione viene utilizzato per determinare se un utente ha il diritto di accedere al contenuto richiesto.

1. Ogni volta che l’utente tenta di visualizzare contenuto protetto nell’app Streaming Device, l’app Streaming Device chiama il servizio Programmatore per identificare il contenuto e richiedere le autorizzazioni e le informazioni necessarie per avviare il flusso.
1. Il servizio Programmer chiama l&#39;API Adobe Pass **authorize** trasmettendo l&#39;ID risorsa insieme ad altri parametri richiesti. Il servizio Adobe chiama il servizio MVPD AuthZ con l’ID risorsa e riceve la decisione di autorizzazione che viene quindi passata al servizio programmatore. Questa decisione di autorizzazione verrà memorizzata nella cache dal servizio Adobe Pass per un periodo configurabile. Nelle successive chiamate **authorize** dal servizio Programmer al servizio Adobe Pass, il valore memorizzato in cache verrà restituito purché sia valido.
1. Se l&#39;autorizzazione viene concessa, il servizio Programmer deve chiamare l&#39;API Adobe Pass **/tokens/media**, che restituirà un token multimediale firmato. Il servizio Programmatore deve convalidare il token multimediale utilizzando la libreria Media Token Verifier (JAR). Se valido, il servizio Programmatore deve restituire l&#39;autorizzazione e il necessario per avviare il flusso (ad esempio, l&#39;URL del flusso) richiesto nel passaggio \#1.
1. Se l&#39;autorizzazione viene negata, la chiamata **authorize** restituirà un codice di errore e una descrizione al servizio Programmer. Il servizio Programmatore deve restituire il codice di errore e la descrizione (o un messaggio modificato dal programmatore) alla richiesta nel passaggio \#1.

Il diagramma seguente illustra il flusso di autorizzazione:

![](../../../../assets/authz-flow.png)

### Disconnetti

Il flusso di disconnessione consente a un utente di rimuovere l&#39;identità
associato all’applicazione.

1. Quando l’utente richiede la disconnessione (ovvero la rimozione dal dispositivo dell’account MVPD corrente associato all’applicazione), l’app Streaming Device chiama il servizio Programmatore per informarlo della disconnessione del dispositivo.
1. Il servizio Programmatore deve chiamare l&#39;API Adobe Pass **logout**.

Il diagramma seguente illustra il flusso di logout:

![](../../../../assets/logout-flow.png)

### \[Facoltativo\] Preautorizzazione (ovvero Pre-volo)

La pre-autorizzazione può essere utilizzata per determinare rapidamente da un set di risorse quelle a cui un utente potrebbe avere accesso.  Il risultato di questa chiamata viene in genere utilizzato per personalizzare l’interfaccia utente di un singolo utente.

1. Una volta autenticato l&#39;utente, il dispositivo di streaming può chiamare il servizio programmatore per richiedere il contenuto a cui l&#39;utente ha diritto di streaming.

1. Il servizio Programmatore deve chiamare l&#39;API Adobe Pass **preauthorize** con un elenco di ID di risorse, che sono una stringa semplice che in genere rappresenta un canale che un utente potrebbe avere diritto a inviare in streaming. *Nota: attualmente, la chiamata* ***preauthorize*** *è configurata per limitare l&#39;elenco a cinque (5) ID risorsa. Quando sono necessarie più di cinque risorse, è possibile effettuare più* ***preauthorize*** *chiamate oppure configurare la chiamata per accettare più di cinque risorse con un accordo degli MVPD. Gli implementatori devono tenere presente il costo di una* ***preauthorize*** *chiamata alle risorse MVPD, nonché il tempo di risposta al programmatore e strutturare il loro utilizzo della chiamata in modo giudizioso.*

1. La chiamata **preauthorize** risponderà al servizio Programmer con un oggetto JSON contenente un valore TRUE o FALSE per ogni ID risorsa nella richiesta che indica se l&#39;utente ha diritto o meno al canale associato. *Nota: se un MVPD non fornisce una risposta per un determinato ID risorsa (ad esempio a causa di errori di rete o timeout), il valore predefinito sarà FALSE.*

1. Il Servizio programmatore deve utilizzare la risposta alla chiamata **preauthorize** per creare una risposta personalizzata definita dal programmatore al dispositivo di streaming, in genere per personalizzare la presentazione per l&#39;utente in base ai diritti.

Il diagramma seguente illustra il flusso di preautorizzazione:

![](../../../../assets/preauthz-flow.png)


### \[Facoltativo\] Metadati

I metadati possono essere utilizzati per recuperare le informazioni utente condivise da MVPD.
Alcuni esempi includono ID utente, codice postale, ecc.

1. Una volta autenticato l&#39;utente, il servizio Programmer può chiamare l&#39;API Adobe Pass **usermetadata** per richiedere informazioni sull&#39;utente autenticato.

1. La risposta includerà tutti i metadati disponibili per l’utente specificato. I campi specifici vengono configurati separatamente per ogni integrazione Programmatore/MVPD.

Il diagramma seguente illustra il flusso di preautorizzazione:



![](../../../../assets/user-metadata-api-preauthz.png)



## Ambienti e requisiti funzionali{#environments}



Un programmatore deve creare almeno due ambienti: uno per la produzione e uno o più per la gestione temporanea.


### Produzione

L’ambiente di produzione deve essere altamente disponibile e scalabile in modo appropriato per picchi di grandi dimensioni o imprevisti (ad esempio sport live, rottura
notizie).



Il servizio Adobe Pass viene eseguito su più data center geograficamente distribuiti negli Stati Uniti.  Per ottenere il tempo di risposta migliore (ovvero la latenza più bassa) dal servizio Adobe Pass, il programmatore deve anche creare un servizio simile distribuito geograficamente
infrastrutture.


Il servizio Programmer deve limitare la cache DNS a un massimo di 30 secondi nel caso in cui Adobe debba reindirizzare il traffico. Ciò può verificarsi se un centro dati non è più disponibile.


Il programmatore deve fornire l’intervallo IP pubblico dell’ambiente di produzione. Questi verranno inseriti in un elenco di IP consentiti nell’infrastruttura Adobe Pass per l’accesso e gestiti dai criteri di utilizzo delle API equi di Adobe.

### Staging

L’ambiente di staging può essere minimo, ma deve includere tutti i componenti di sistema e la logica di business. Dovrebbe funzionare in modo simile alla produzione e consentire di testare le versioni al di fuori della produzione. Idealmente, l’ambiente di staging può essere connesso agli ambienti di test di Adobe Pass per l’utilizzo da parte del programmatore e, se necessario, di Adobe in modo che possiamo contribuire ai test e alla risoluzione dei problemi.

### Requisiti funzionali

Il servizio Programmatore deve trasmettere informazioni accurate sull’identificazione del dispositivo per il quale sta eseguendo i flussi. Inoltre, il servizio Programmer deve trasmettere l’IP del dispositivo per il quale sta eseguendo i flussi (in un’intestazione x-forwarded-for) insieme alla porta di origine della connessione (nel campo device info (informazioni sul dispositivo)):

    **X-Forwarded-For: \&lt;client\_ip\>**
    
    dove \&lt;client\_ip\> è l&#39;indirizzo IP pubblico del client
    
    
    
    L&#39;intestazione deve essere aggiunta nelle chiamate **regcode** e **authorize**
    
    Esempi:
    
    POST /reggie/v1/{req\_id}/regcode HTTP/1.1
    
    X-Forwarded-For:203.45.101.20
    
    
    
    GET /api/v1/authorize HTTP/1.1
    
    X-Forwarded-For:203.45.101.20



Il servizio Programmer deve inviare i dati e la formattazione richiesti dai singoli MVPD o dalle app integrate (ad esempio, IP dispositivo, porta di origine, informazioni dispositivo, MRSS, dati opzionali come ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.


Il servizio Programmer deve rispettare i TTL authN e authZ durante il caching e annullare la validità delle sessioni authN o authZ quando riceve la notifica.

Il programmatore deve mantenere i certificati condivisi con Adobe.
