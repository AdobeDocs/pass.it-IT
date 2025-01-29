---
title: Metadati utente
description: Metadati utente
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1793'
ht-degree: 0%

---

# Metadati utente {#user-metadata}

>[!IMPORTANT]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

I metadati utente fanno riferimento a [attributi](#attributes) specifici dell&#39;utente (ad esempio, codici postali, valutazioni dei genitori, ID utente ecc.) gestiti da MVPD e forniti ai programmatori tramite l&#39;autenticazione Adobe Pass [REST API V2](#apis).

I metadati dell’utente possono essere utilizzati per migliorare la personalizzazione degli utenti, ma possono anche essere utilizzati per l’analisi. Ad esempio, un programmatore può utilizzare il codice postale di un utente per inviare notizie localizzate o aggiornamenti meteo, o per applicare il controllo genitori.

L’autenticazione Adobe Pass normalizza i valori dei metadati dell’utente quando i MVPD forniscono dati in formati diversi. Inoltre, per alcuni attributi (ad esempio, codice postale), i valori possono essere [crittografati](#encryption) utilizzando un certificato del programmatore.

L&#39;autenticazione Adobe Pass consente ai programmatori di esaminare i metadati utente resi disponibili nelle loro integrazioni MVPD e di [gestirli](#management) tramite [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication).

## Attributi metadati utente {#attributes}

Nella tabella seguente sono elencati alcuni degli attributi dei metadati utente resi disponibili ai programmatori:

| Chiave | Tipo | Esempio | Richiede crittografia | Descrizione | Dettagli |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | Stringa | &quot;1o7241p&quot; | No | Identificatore dell’account. | Il valore dell’attributo può essere un identificativo della famiglia o un identificativo del conto secondario. Il valore `userID` sarà diverso da `householdID` se MVPD supporta account secondari e l&#39;utente corrente non è il titolare dell&#39;account principale. |
| `upstreamUserID` | Stringa | &quot;1o7241p&quot; | No | Identificatore dell’account per il monitoraggio della concorrenza. | Il valore dell’attributo può essere utilizzato per applicare limiti di concorrenza tra siti e app MVPD e Programmer. Il valore `upstreamUserID` è uguale al valore `userID` per la maggior parte degli MVPD. |
| `householdID` | Stringa | &quot;1o7241p&quot; | No | Identificatore dell’account per il controllo genitori. | Il valore dell’attributo può essere utilizzato per distinguere tra l’utilizzo della famiglia e quello dei conti secondari. A volte può essere utilizzato come sostituto del controllo genitori se le valutazioni vere non sono disponibili, se l’utente ha effettuato l’accesso con l’account della famiglia può guardare, altrimenti il contenuto classificato non verrebbe visualizzato. Il modo in cui viene rappresentato è molto diverso tra gli MVPD (ad esempio, ID utente di un nucleo familiare, ID del nucleo familiare, flag del capofamiglia, ecc.). Se MVPD non supporta i conti secondari, sarà identico a `userID`. |
| `primaryOID` | Stringa | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | No | Identificatore dell’account. | L&#39;attributo è specifico di AT&amp;T. Il valore `primaryOID` è uguale al valore `userID` quando il valore `typeID` è impostato su &quot;Primary&quot;. |
| `typeID` | Stringa | &quot;Primaria&quot; | No | Attributo che indica se l&#39;utente corrente è un titolare di conto principale o secondario. | L&#39;attributo è specifico di AT&amp;T. Il valore `primaryOID` è uguale al valore `userID` quando il valore `typeID` è impostato su &quot;Primary&quot;. |
| `is_hoh` | Stringa | &quot;1&quot; | No | Attributo che indica se l’utente corrente è o meno a capo della famiglia. | L&#39;attributo è specifico di Synacor. |
| `hba_status` | Booleano | &quot;true&quot; | No | Attributo che indica se l&#39;utente corrente ha eseguito l&#39;autenticazione tramite l&#39;HBA o meno. |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | Booleano | &quot;true&quot; | No | Attributo che indica se il dispositivo corrente può eseguire o meno il mirroring dello schermo. | L&#39;attributo è specifico per Spectrum. |
| `zip` | Array | \[&quot;77754&quot;, &quot;12345&quot;\] | Sì | Codice postale dell’utente. | Il valore dell’attributo può essere utilizzato per fornire notizie localizzate, aggiornamenti meteo o eventi sportivi. Il valore `zip` rappresenta dati sensibili che richiedono accordi legali con MVPD. Se crittografata, la rappresentazione della chiave `zip` sarà `String` anziché `Array`. |
| `encryptedZip` | Stringa | &quot;&quot; | Sì | Codice postale crittografato dell’utente. | L&#39;attributo è specifico di Comcast. |
| `channelID` | Array | \[&quot;channel-1&quot;, &quot;channel-2&quot;\] | No | Elenco dei canali che l’utente ha il diritto di visualizzare. | Il valore dell&#39;attributo può essere utilizzato per filtrare vari canali da portali che aggregano più reti. Si consiglia di utilizzare l&#39;API [Preauthorize](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) invece di questo attributo di metadati per filtrare i canali non disponibili per l&#39;utente. |
| `maxRating` | Oggetto | { MPAA: &quot;NR&quot;, VCHIP: &quot;X&quot;, URL: &quot;http://manage.my/parental&quot; } | No | Valutazione genitori massima per l’utente corrente. | Il valore dell’attributo può essere utilizzato per filtrare contenuti non adatti all’utente corrente in base alle valutazioni &quot;MPAA&quot; o &quot;VCHIP&quot;. |
| `language` | Stringa | &quot;English&quot; | No | Impostazioni della lingua. | Il valore dell’attributo può essere utilizzato per visualizzare i messaggi in base alle preferenze di lingua dell’utente. |

Gli attributi dei metadati utente resi disponibili a un programmatore dipendono da ciò che fornisce un MVPD. Nella tabella seguente sono elencati gli attributi resi disponibili da vari MVPD:

|                         | **Contratto legale firmato (solo zip)** | **ID utente su AuthN** | **ID utente upstream in AuthN** | **ID famiglia su AuthN/Z** | **OID primario in AuthN** | **ID tipo su AuthN** | **Responsabile della famiglia in AuthN** | **Stato HBA** | **Consenti mirroring in AuthZ** | **Codice postale su AuthN/Z** | **ID canale su AuthN** | **Valutazione su AuthN/Z** | **Lingua** | **suNet** | **inHome** | **Note** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome formale** | n/d | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **Richiede crittografia** | n/d | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| **Sensibile** | n/d | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| IdP Adobe | **Sì** | **Sì** | **Sì** | **Sì (solo AuthN)** | **Sì** | **Sì** | **Sì** | **No** | **No** | **Sì (solo AuthN)** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **No** | Non è necessario alcun accordo legale. |
| Synacor | **Sì** | **Sì** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **Sì** | **No** | **No** | **Sì (solo AuthN)** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **No** | Accordo legale che non copre tutti gli MVPD proxy. Si tratta di un supporto generico per Synacor e possibilmente non aggregato a tutti i loro MVPD. |
| Piatto | **No** | **Sì** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **No** | Condivide lo stesso elenco di tutti gli MVPD Synacor, più `upstreamUserID`. |
| Comcast | **No** | **Sì** | **Sì** | **Sì (solo AuthZ)** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **Sì (solo AuthZ)** | **No** | **No** | **No** |                                                                                                                                           |
| AT&amp;T | **Sì** | **Sì** | **Sì** | **Sì (solo AuthN)** | **Sì** | **Sì** | **No** | **No** | **No** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | Accordo legale firmato. |
| DTV | **Sì** | **Sì** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| COX | **No** | **Sì** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Cablevision | **Sì** | **Sì** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **Sì** | **No** | **No** | **No** | **No** | Accordo legale firmato. |
| Spettro | **Sì** | **Sì** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **No** | **Sì** | **Sì** | **Sì (solo AuthN)** | **No** | **Sì (solo AuthN)** | **No** | **No** | **No** |                                                                                                                                           |
| Carta | **Sì** | **Sì** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **No** | **Sì (solo AuthN)** | **No** | **No** | **No** |                                                                                                                                           |
| Verizon | **No** | **Sì** | **Sì** | **No** | **No** | **No** | **No** | **Sì** | **No** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| HTC | **No** | **Sì** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Rogers | **No** | **Sì** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| RCN | **Sì** | **Sì** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **No** | **Sì (solo AuthN)** | **No** | **No** | **No** |                                                                                                                                           |
| Eastlink | **No** | **Sì** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **No** |                                                                                                                                           |
| Cogeco | **No** | **Sì** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Videotron | **No** | **Sì** | **Sì** | **Sì*** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | Espone `householdID` con lo stesso valore di `userID`. |
| Massilon proxy | **Sì** | **Sì** | **Sì** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | Accordo legale firmato. |
| Clearleap proxy | **Sì** | **Sì** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **No** | **Sì (solo AuthZ)** | **Sì** | **No** | **No** | Accordo legale firmato. |
| GLD proxy | **No** | **Sì** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì (solo AuthN)** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Altri MVPD | **No** | **Sì** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | Ancora nessun accordo legale, metadati sensibili non disponibili per la produzione. Per tutti gli MVPD `userID` è disponibile senza lavoro aggiuntivo. |

>[!IMPORTANT]
>
> Gli accordi legali devono essere firmati con i MVPD prima di rendere disponibili i metadati sensibili degli utenti (ad esempio, il codice postale).

## Crittografia metadati utente {#encryption}

Per crittografare e decrittografare gli attributi dei metadati utente, il programmatore deve generare un certificato (coppia di chiavi pubblica/privata) e [configurare autonomamente](#management) il certificato tramite [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) o condividere la chiave pubblica con i rappresentanti di autenticazione di Adobe Pass.

Per garantire che il certificato sia generato e configurato correttamente, segui i passaggi seguenti:

1. Scarica e installa il toolkit OpenSSL (http://www.openssl.org).

1. Generare una richiesta di firma del certificato (CSR, Certificate Signing Request):

   * Genera una coppia di chiavi. Sul terminale di comando eseguire le operazioni seguenti:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Genera la CSR. Sul terminale di comando eseguire le operazioni seguenti:

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     Verrà richiesto di immettere la password per la chiave privata.

   * Crea una copia di backup della chiave privata e della password. CSR di esempio:

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. Invia la CSR a un’autorità di certificazione (CA) (ad esempio, Verisign).

1. La CA ti invierà il certificato in formato .p7b (PKCS#7, Cryptographic Message Syntax Standard).

1. Distribuisci il certificato .p7b. Converti il file PKCS#7 (.p7b) in un file PKCS#12 (file PFX, Personal Information Exchange Syntax Standard) utilizzando la tua chiave privata e genera il file PEM (file contenitore certificato concatenato):

   * Convertire il file PKCS#7 in un file PEM temporaneo. Sulla riga di comando eseguire le operazioni seguenti:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Convertire il file PEM temporaneo in un file PFX. Sulla riga di comando eseguire le operazioni seguenti:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Convertire il file PEM temporaneo in un file PEM finale. Sulla riga di comando eseguire le operazioni seguenti:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Utilizza il file PEM per [configurare](#management) il certificato tramite [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) o inviare il file PEM agli agenti di autenticazione di Adobe Pass.

   * Per ulteriori dettagli su come gestire i certificati tramite [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication), consulta la sezione successiva.

   * L’autenticazione Adobe Pass supporta sia un certificato primario che un certificato di backup. Se il certificato principale viene compromesso, è possibile revocarlo e passare al certificato secondario. In questo modo la transizione tra i certificati risulterà fluida e con un impatto minimo sul cliente.

## Gestione metadati utenti {#management}

>[!IMPORTANT]
>
> Se non hai accesso a Adobe Pass TVE Dashboard, crea un ticket tramite il nostro [Zendesk](https://adobeprimetime.zendesk.com) e chiedi al tuo Technical Account Manager (TAM) di apportare le modifiche appropriate per te.

Adobe Pass TVE Dashboard è uno strumento che consente ai clienti di autenticazione Adobe Pass (programmatori) di gestire la configurazione e i dati. Questa dashboard self-service abilita una serie di funzionalità descritte nella [Guida utente di Adobe Pass TVE Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md).

Per rivedere e gestire gli attributi dei metadati utente resi disponibili da un MVPD, segui i passaggi descritti nella [Guida utente di TVE Dashboard per le integrazioni](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata).

Per esaminare e gestire i certificati utilizzati per crittografare gli attributi dei metadati utente, seguire i passaggi descritti nella [Guida utente di TVE Dashboard per i programmatori](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates) o nella [Guida utente di TVE Dashboard per i canali](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates).

## API REST V2 {#rest-api-v2}

Gli attributi dei metadati utente possono essere recuperati utilizzando le seguenti API:

* [Recuperare i profili](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recupera profilo per mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recupera profilo per codice specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Per informazioni sulla struttura degli attributi dei metadati utente, consulta le sezioni **Response** e **Samples** delle API di cui sopra.

Per ulteriori dettagli su come e quando integrare le API di cui sopra, consulta i seguenti documenti:

* [Flusso dei profili di base eseguito all’interno dell’applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flusso dei profili di base eseguito all&#39;interno dell&#39;applicazione secondaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
