---
title: Attributi metadati standard
description: Attributi metadati standard
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 0%

---

# Attributi metadati standard {#std-metadata-attributes}

Questa pagina si propone di fornire un elenco completo degli attributi di metadati che il servizio di monitoraggio della concorrenza può elaborare e che possono essere utilizzati come base per i criteri che possono essere implementati. Gli attributi di metadati standard possono essere classificati come segue:

* Attributi inclusi per progettazione (inviati in ogni chiamata di inizializzazione della sessione, in quanto sono richiesti nel percorso URL). Senza questi valori non è possibile eseguire chiamate valide.
* Attributi dei metadati: valori che devono essere trasmessi come dati del modulo durante la chiamata di inizializzazione della sessione (nel caso in cui i criteri di back-end richiedano i relativi valori).

## Attributi richiesti per progettazione {#attr-req-by-design}

L&#39;API di monitoraggio della concorrenza forza i client a inviare i seguenti valori come parte di qualsiasi chiamata di inizializzazione valida: [chiamate di avvio sessione](/help/concurrency-monitoring/technical/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| Nome campo | Esempio di valore | Dove utilizzarlo | Ottenuto da |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | Intestazione di autorizzazione | Ticket Zendesk all&#39;integrazione |
| mvpdName | Sample_MVPD | Percorso URI | Autenticazione di Adobe Pass dall’endpoint di configurazione quando l’utente seleziona il MVPD |
| accountId | 12345 | Percorso URI | Metadati upstreamUserID di autenticazione Adobe Pass dopo l&#39;accesso utente [Metadati utente upstreamUserID - Autenticazione Adobe Pass](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) |


## Attributi metadati {#metadata-attr}

I campi nella tabella seguente possono essere utilizzati dai programmatori e dagli MVPD per creare criteri che verranno implementati nel monitoraggio della concorrenza.

Con [API v2.0](https://streams-stage.adobeprimetime.com/swagger-ui/index.html), se uno di questi attributi è richiesto dai criteri definiti, un tentativo di inizio sessione senza tale attributo genererà una richiesta 400 non valida.


| Entità | Nome attributo | Tipo di dati | Descrizione | Riferimento esterno (EIDR, OATC) | Esempio di valore | Regole di convalida |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Media Company | programmerName | stringa | Nome del programmatore |                                                   | ProgrammatoreX |                                                                                   |
| Risorsa | channel | stringa | Canale TV |                                                   | ChannelY |                                                                                   |
|                 | assetId | stringa | Il titolo &quot;descrittivo&quot; o leggibile da leggere per questo contenuto | [Riferimento campi dati EIDR 2.0](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | Ben-Hur |                                                                                   |
|                 | tipo | enumerazione | Valore che descrive il tipo generale di contenuto rappresentato da TveItem. I valori enumerati includono: trasmissione di filmEpisodio nonBroadcastEpisodio musicaVideo awardsMostra clip concerto conferenza newsEvento sportivotrailer evento | [Esercitazione consigliata feed metadati OATC](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | Il campo deve corrispondere a uno degli elementi nell’enumerazione |
|                 | contentType | stringa | Questo campo determina se il contenuto richiesto è live o VOD | N/D | live, vod | live o vod |
|                 | genere | stringa | Genere del contenuto riprodotto in streaming. Descrive il tipo di programmazione generale | [Feed metadati OATC consigliato](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} Esercitazione | Commedia | Tipo di genere valido |
|                 | durata | numero | Durata dell&#39;elemento multimediale in secondi | [Esercitazione consigliata feed metadati OATC](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | sequenza numerica |
| Dispositivo/browser | deviceId | stringa | L’identificatore univoco del dispositivo. | [Proprietà di Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | stringa | Il nome descrittivo del dispositivo. |                                                   | Joe’s iPad |                                                                                   |
|                 | marketingName | stringa | Il nome marketing (o il nome descrittivo del cliente) di un dispositivo | [Proprietà di Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | nome marketing valido |
|                 | mobileDevice | booleano | True se il dispositivo è destinato all&#39;uso in movimento | [Proprietà di Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | true, false | true, false |
|                 | deviceModel | stringa | Il nome del modello del dispositivo, del browser o di un altro componente | [Proprietà di Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | tablet, telefono, xbox. set-top box | nome del modello di dispositivo valido |
|                 | osName | stringa | Sistema operativo in esecuzione sul dispositivo | [Atlante dispositivo - Valori proprietà predefiniti del sistema operativo](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android, Windows 10, OS X, Linux, Altro Nota: per visualizzare i valori delle proprietà è necessario aver effettuato l’accesso con un nome utente e una password in Device Atlas | il valore previsto è uno dei valori delle proprietà predefinite di Device Atlas |
|                 | browserName | stringa | Il nome o il tipo di browser sul dispositivo | [Atlante dispositivo - Valori proprietà predefiniti browser](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | Nome o tipo del browser sul dispositivo.  Nota: per visualizzare i valori delle proprietà, devi aver effettuato l’accesso con un nome utente e una password in Device Atlas | il valore previsto è uno dei valori delle proprietà predefinite di Device Atlas |
|                 | browserVersion | stringa | Versione del browser sul dispositivo | [Proprietà di Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | Versione del browser sul dispositivo |                                                                                   |
| Applicazione | applicationName | stringa | Nome dell’applicazione leggibile dal consumatore o di facile utilizzo | N/D | Applicazione_Esempio |                                                                                   |
|                 | applicationId | stringa | L’ID applicazione che identifica in modo univoco un’applicazione client. | N/D | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | stringa | Piattaforma nativa dell’applicazione | N/D | ios, android |                                                                                   |
|                 | applicationVersion | stringa | Questo valore può essere utilizzato a scopo di analisi | N/D | 1,0, 2,0 |                                                                                   |
| Oggetto | accountId | stringa | ID account dell&#39;oggetto di monitoraggio della concorrenza (nell&#39;ambito di MVPD) | N/D | account test |                                                                                   |
|                 | contractType | stringa | premium, base. I clienti possono aggiungere questo elemento come metadati personalizzati e utilizzarlo nei propri realm. | N/D | premium, base |                                                                                   |
| Utente | name | stringa | Alcuni MVPD forniscono informazioni relative all’utente specifico che riproduce il contenuto. | N/D |                                                                                                                                                         |                                                                                   |
|                 | hba | booleano | Identifica se l’utente tenta di avviare il flusso dalla propria posizione principale | N/D | true, false | true o false |
| Posizione | continente | stringa | Il continente da cui proviene l’ID dispositivo che invia la richiesta di riproduzione | N/D | Nord America | nome continente valido |
|                 | paese | stringa | Il paese da cui proviene l’ID dispositivo che invia la richiesta di riproduzione | N/D | Stati Uniti | nome paese valido |
|                 | stato | stringa | Stato da cui proviene il deviceID che invia la richiesta di riproduzione | N/D | CA | nome stato valido |
|                 | città | stringa | Città da cui proviene il deviceID che invia la richiesta di riproduzione | N/D | Cupertino | nome città valido |
|                 | zipcode | numero | Il codice zip da cui proviene il deviceID che invia la richiesta di riproduzione | N/D | 95014 | zipcode valido |
| Flusso | streamId | stringa | Generato dal servizio CM, non sotto il controllo del cliente. Viene utilizzato in modo implicito quando vengono definite regole di tipo maxstream. | N/D | N/D | N/D |
|                 | streamCDN | stringa | indica la rete CDN da cui è stato recuperato il flusso | N/D | N/D | N/D |

## Esempi di utilizzo degli attributi di metadati per la creazione di criteri {#examples-metadata-attr}

I campi di metadati standard possono essere utilizzati per definire criteri lato server in base ai loro valori di campo:

* È possibile configurare un criterio per applicarlo solo a valori di campo specifici (ad esempio, un criterio iOS dedicato: dove `osType` è `iOS`)
* È possibile limitare il numero di valori distinti per un determinato campo. Alcuni esempi sono i seguenti:
   * non più di X dispositivi distinti: `HAVING DISTINCT COUNT(deviceId) <= 2`
   * non più di X codici zip distinti: `HAVING DISTINCT COUNT(zipcode) <= 3`
* Puoi limitare il numero di flussi attivi per valore di campo. Alcuni esempi sono i seguenti:
   * non più di X flussi attivi per un singolo tipo di dispositivo: `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * non più di X flussi attivi per flussi di contenuto live: `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

Contatta il team di monitoraggio della concorrenza [creando un ticket in Zendesk](mailto:tve-support@adobe.com) e indica i criteri che desideri implementare.

Di seguito sono riportati ulteriori esempi di criteri e manuali di integrazione:

* [Punto decisionale criterio](/help/concurrency-monitoring/technical/cm-policy-decision-point.md)
* [Console API - Monitoraggio concorrenza Adobe](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)
