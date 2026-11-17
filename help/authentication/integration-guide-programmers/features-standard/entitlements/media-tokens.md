---
title: Token multimediali
description: Token multimediali
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 6fdd77e300d01269357d7917702f31aef35d6751
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%
---
# Token multimediali {#media-tokens}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Il token multimediale è un token generato dall&#39;autenticazione Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) dopo una decisione di autorizzazione destinata a fornire accesso di visualizzazione al contenuto protetto (risorsa).

Il token multimediale è valido per un periodo di tempo limitato e breve (predefinito 7 minuti) specificato al momento del rilascio, che indica il limite di tempo prima che debba essere verificato e utilizzato dall’applicazione client. Il token multimediale è limitato all’uso una tantum e non deve mai essere memorizzato nella cache.

Il token multimediale è costituito da una stringa firmata basata su Infrastruttura a chiave pubblica (PKI) inviata in testo non crittografato. Con la protezione basata su PKI, il token viene firmato utilizzando una chiave asimmetrica rilasciata ad Adobe da un’Autorità di certificazione (CA).

Il token multimediale viene passato al programmatore, che lo convalida utilizzando il Media Token Verifier prima di avviare il flusso video per garantire la sicurezza dell’accesso a tale risorsa.

Il Media Token Verifier è una libreria distribuita da Adobe Pass Authentication che verifica l’autenticità di un token multimediale.

## Media Token Verifier {#media-token-verifier}

Per garantire un accesso sicuro prima di avviare il flusso video, l’autenticazione di Adobe Pass consiglia ai programmatori di inviare il token multimediale al proprio servizio back-end che integra la libreria Media Token Verifier. Il valore TTL (time-to-live) del token multimediale è progettato per tenere conto di potenziali problemi di sincronizzazione dell’orologio tra il server che genera il token e il server di convalida.

L’autenticazione di Adobe Pass consiglia vivamente di non analizzare il token multimediale e di estrarne direttamente i dati, in quanto il formato del token non è garantito e potrebbe cambiare in futuro. La libreria Media Token Verifier deve essere l’unico strumento utilizzato per analizzare il contenuto del token.

Scarica la libreria Media Token Verifier dal seguente collegamento:

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

La libreria Media Token Verifier richiede Java versione 8 o successiva e supporta l&#39;utilizzo di un provider JCE (Java Cryptography Extension) preferito per l&#39;algoritmo della firma (`SHA256WithRSA`).

La distribuzione Media Token Verifier include:

* Chiave pubblica Adobe.
* API di verifica token (`ITokenVerifier.java`).
* Implementazione di riferimento (`com.adobe.entitlement.test.EntitlementVerifierTest.java`).
* Archivio chiavi certificato (`media_token_keystore.jks`).
* Dipendenze runtime in `mediatoken-verifier-VERSION-application-with-dependencies.jar`. Il thin `mediatoken-verifier-VERSION.jar` richiede queste dipendenze separatamente.

>[!IMPORTANT]
> 
> La password predefinita per l&#39;archivio chiavi del certificato incluso è `123456`.

>[!IMPORTANT]
>
> `serializedToken` restituito dall&#39;API REST V1 e V2 ha codifica Base64. Decodificarlo come UTF-8 prima di passarlo a `getInstance(...)`; in caso contrario, la verifica restituisce `INVALID_SIGNATURE`. L’esempio seguente accetta il valore codificato ed esegue questa decodifica.

### Metodi {#methods}

L&#39;interfaccia `ITokenVerifier` definisce i seguenti metodi:

* Metodo `isValid()` utilizzato per convalidare il token multimediale. Accetta un singolo argomento, l&#39;identificatore [della risorsa](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier). Se l&#39;identificatore di risorsa fornito è `null`, il metodo convalida solo l&#39;autenticità e il periodo di validità del token multimediale.

  Il metodo `isValid()` restituisce uno dei seguenti valori di stato:

  | Stato | Significato |
  |----------------------|-------------------------------------------|
  | VALID_TOKEN | Convalide dei token completate |
  | INVALID_TOKEN_FORMAT | Formato del token non valido |
  | INVALID_SIGNATURE | Impossibile convalidare l’autenticità del token |
  | TOKEN_EXPIRED | TTL del token non valido |
  | INVALID_RESOURCE_ID | Token non valido per la risorsa specificata |
  | ERRORE_SCONOSCIUTO | Il token non è ancora stato convalidato |

* Metodo `getResourceID()` utilizzato per recuperare l&#39;identificatore di risorsa associato al token multimediale e confrontarlo con l&#39;identificatore restituito dalla risposta alla decisione di autorizzazione.

* Metodo `getTimeIssued()` utilizzato per recuperare l&#39;ora di emissione del token multimediale.

* Metodo `getTimeToLive()` utilizzato per recuperare il TTL del token multimediale in millisecondi.

* Metodo `getUserSessionGUID()` utilizzato per recuperare un GUID anonimo impostato da MVPD.

* Il metodo `getMvpdId()` utilizzato per recuperare l&#39;identificatore del MVPD che ha autenticato l&#39;utente.

* Il metodo `getProxyMvpdId()` utilizzato per recuperare l&#39;identificatore del MVPD proxy che ha autenticato l&#39;utente.

### Esempio {#sample}

L&#39;archivio Media Token Verifier contiene un&#39;implementazione di riferimento (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) e un esempio di chiamata dell&#39;API con la classe di test. In questo esempio (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) viene illustrata l&#39;integrazione della libreria Media Token Verifier in un server multimediale.

```JAVA
package com.adobe.entitlement.test;

import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.entitlement.verifier.SignatureVerificationCredential;
import java.io.InputStream; 
import java.nio.charset.StandardCharsets;
import java.util.Base64;

public class EntitlementVerifierTest { 
    String mRequestorID = null;
    String mTokenToVerify = null;
    String mPathToCertificate = null;
    String mKeystoreType = null;
    String mKeystorePasswd = null;
    String mResourceID = null;

    public static void main(String[] args) { 
        if (args == null || args.length < 2 ) {
            System.out.println("Incorrect args: Usage: EntitlementVerifierTest requestorID tokenToVerify [resourceID]");
            return;
        } 
        String requestorID = args[0];
        String tokenToVerify = args[1];
        String pathToCertificate = "media_token_keystore.jks"; // the default keystore provided in the entitlement jar 
        String keystoreType = "jks";
        String keystorePasswd = "123456"; // password for the default keystore 
        if (requestorID == null || tokenToVerify == null) {
            System.out.println("One or more arguments is null");
            return;
        } 
        tokenToVerify = new String(Base64.getDecoder().decode(tokenToVerify),
                StandardCharsets.UTF_8);
        System.out.println("RequestorID: " + requestorID);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        String resourceID = null;
        if (args.length > 2) {
            resourceID = args[2];
        }
        System.out.println("Resource ID: " + resourceID);
        EntitlementVerifierTest verifier = new EntitlementVerifierTest(requestorID,
            tokenToVerify, pathToCertificate, keystoreType, keystorePasswd, resourceID);
        verifier.verifyToken();
    } 

    protected EntitlementVerifierTest(String inRequestorID,
                                      String inTokenToVerify,
                                      String inPathToCertificate,
                                      String inKeystoreType,
                                      String inKeystorePasswd, String inResourceID) {
        mRequestorID = inRequestorID;
        mTokenToVerify = inTokenToVerify;
        mPathToCertificate = inPathToCertificate;
        mKeystoreType = inKeystoreType;
        mKeystorePasswd = inKeystorePasswd;
        mResourceID = inResourceID;
    } 

    protected void verifyToken() {
        // It is expected that the SignatureVerificationCredential and 
        // CryptoDataHolder could be created at Init time in a web application 
        // and be reused for all token verifications. 
        CryptoDataHolder cryptoData = createCryptoDataHolder(mPathToCertificate, mKeystoreType, mKeystorePasswd);
        ITokenVerifierFactory tokenVerifierFactory = new SimpleTokenPKISignatureVerifierFactory();
        ITokenVerifier tokenVerifier = tokenVerifierFactory.getInstance(mRequestorID, mTokenToVerify, cryptoData);
        ITokenVerifier.eReturnValue status = tokenVerifier.isValid(mResourceID);
        System.out.println("Is token Valid? : " + status.toString());
        System.out.println("Token User ID: " + tokenVerifier.getUserSessionGUID());
        System.out.println("Token was generated at: " + tokenVerifier.getTimeIssued());

        System.out.println("Token Mvpd ID: " + tokenVerifier.getMvpdId());
        System.out.println("Token Proxy Mvpd ID: " + tokenVerifier.getProxyMvpdId());
    } 
    
    protected CryptoDataHolder createCryptoDataHolder(String pathToCertificate,
                                                      String keystoreType, String keystorePasswd) {
        SignatureVerificationCredential verificationCredential =
            readShortTokenVerificationCredential(pathToCertificate, keystoreType, keystorePasswd);
        CryptoDataHolder cryptoData = new CryptoDataHolder();
        cryptoData.setCertificateInfo(verificationCredential);
        return cryptoData;
    } 
    
    protected SignatureVerificationCredential readShortTokenVerificationCredential(String keystoreFile,
                                                                                   String keystoreType,
                                                                                   String keystorePasswd) {
        SignatureVerificationCredential cred = null; 
        if (keystoreFile != null){
            try (InputStream certInputStream =
                    EntitlementVerifierTest.class.getClassLoader().getResourceAsStream(keystoreFile)) {
                // load the keystore file 
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null || !cred.isValid()) {
            throw new IllegalStateException("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```

## API REST V2 {#rest-api-v2}

Il token multimediale può essere recuperato utilizzando la seguente API:

* [Recuperare le decisioni di autorizzazione utilizzando mvpd specifico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Consulta le sezioni **Risposta** e **Esempi** dell&#39;API precedente per comprendere la struttura delle decisioni di autorizzazione e dei token multimediali.

>[!IMPORTANT]
>
> L&#39;applicazione client non deve eseguire una query su un endpoint separato per recuperare i [token multimediali](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md), in quanto sono già inclusi nelle decisioni di autorizzazione che consentono l&#39;accesso utente.

Per ulteriori dettagli su come e quando integrare la precedente API, consulta il seguente documento:

* [Flusso di autorizzazione di base eseguito nell&#39;applicazione principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!MORELIKETHIS]
>
> [Domande frequenti sulla fase di autorizzazione](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)
