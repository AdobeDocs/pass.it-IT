---
title: Integrazione di Media Token Verifier
description: Integrazione di Media Token Verifier
exl-id: 1688889a-2e30-4d66-96ff-1ddf4b287f68
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 0%

---

# Integrazione di Media Token Verifier

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Informazioni su Media Token Verifier {#about-media-token-verifier}

Quando l’autorizzazione ha esito positivo, Adobe Pass Authentication crea un token di autorizzazione a lunga durata (AuthZ).  Il token AuthZ viene passato al lato client o memorizzato sul lato server, a seconda della piattaforma del client.  (vedere [Informazioni sui token](/help/authentication/programmer-overview.md#understanding-tokens) per informazioni su come i token vengono memorizzati su sistemi client diversi, insieme ad altri dettagli.)


Un token AuthZ autorizza l’utente del sito a visualizzare una determinata risorsa.  Ha un TTL (time-to-live) tipico di 6-24 ore, dopo la scadenza del token. **Per l’accesso effettivo alla visualizzazione, l’autenticazione Adobe Pass utilizza il token AuthZ per generare un token multimediale di breve durata che ottieni e trasmetti al server multimediale**. Questi token multimediali di breve durata hanno un TTL molto breve (in genere, pochi minuti).


Nelle integrazioni AccessEnabler, puoi ottenere il token multimediale di breve durata tramite `setToken()` callback. Per le integrazioni API senza client, ottieni il token Media di breve durata con `<SP_FQDN>/api/v1/tokens/media` Chiamata API. Il token è una stringa inviata in testo non crittografato, firmata da un Adobe, che utilizza la protezione del token basata su PKI (Public Key Infrastructure). Con questa protezione basata su PKI, il token viene firmato utilizzando una chiave asimmetrica, rilasciata all&#39;Adobe da un&#39;autorità di certificazione.


Poiché non esiste alcuna convalida sul lato client per il token, un utente malintenzionato potrebbe utilizzare gli strumenti per inserire false `setToken()` chiamate. Quindi tu **non può** si basa semplicemente sul fatto che `setToken()` è stato attivato quando si valuta se un utente è autorizzato o meno. Devi verificare che il token di breve durata sia legittimo. Lo strumento per eseguire la convalida è la libreria Media Token Verifier.


>[!TIP]
>
>Devi passare l’intera lunghezza della stringa di token restituita al Media Token Verifier per la convalida.

## Convalida dei token di breve durata con Media Token Verifier {#validate-short-livedttokens}

È consigliabile che i programmatori inviino il token a un servizio web che utilizza la Libreria Media Token Verifier, per convalidarlo prima di avviare effettivamente il flusso video. Il TTL molto breve dei token multimediali di breve durata è definito per essere sufficientemente lungo da consentire problemi di sincronizzazione dell’orologio tra il server che genera il token e il server che lo convalida, ma non più.



Il [Libreria di verificatori token multimediali](https://adobeprimetime.zendesk.com/auth/v2/login/signin?return_to=https%3A%2F%2Ftve.zendesk.com%2Fhc%2Fen-us%2Farticles%2F204963159-Media-Token-Verifier-library&amp;theme=hc&amp;locale=en-us&amp;brand_id=343429&amp;auth_origin=343429%2Cfalse%2Ctrue){target=_blank} è disponibile per i partner di autenticazione di Adobe Pass.



La libreria Media Token Verifier è contenuta nell’archivio Java `mediatoken-verifier-VERSION.jar`. La libreria definisce:

* Un&#39;API per la verifica del token (`ITokenVerifier` ), con documentazione JavaDoc
* Chiave pubblica di Adobe utilizzata per verificare che il token provenga effettivamente da Adobe
* Un&#39;implementazione di riferimento (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) che mostra come utilizzare l’API del verificatore e come utilizzare la chiave pubblica di Adobe contenuta nella libreria per verificarne l’origine


L’archivio contiene tutte le dipendenze e i keystore di certificati. La password predefinita per il registro chiavi del certificato incluso è &quot;123456&quot;.

* La libreria di verifica richiede JDK versione 1.5 o successiva.
* Utilizza il provider JCE preferito per l’algoritmo di firma &quot;SHA256WithRSA&quot;.


**La libreria del verificatore deve essere l’unico mezzo utilizzato per analizzare il contenuto del token. I programmatori non devono analizzare il token ed estrarre i dati stessi, perché il formato del token non è garantito ed è soggetto a modifiche future.** Solo l’API del verificatore funziona correttamente. L’analisi diretta della stringa potrebbe funzionare temporaneamente, ma causare problemi in futuro, quando il formato potrebbe cambiare. L’API del verificatore recupera informazioni dal token, ad esempio:

* Il token è valido (il `isValid()` metodo)?
* L’ID risorsa associato al token (il `getResourceID()` , che può essere confrontato con (e deve corrispondere) l&#39;altro parametro del `setToken()` callback di funzione. Se non corrisponde, potrebbe indicare un comportamento fraudolento.
* Ora di emissione del token (`getTimeIssued()` metodo).
* TTL (`getTimeToLive()` metodo).
* GUID di autenticazione anonima ricevuto da MVPD (`getUserSessionGUID()` metodo).
* ID del distributore che ha autenticato l’utente e, se è il caso, proxy-MVPD che ha fornito l’autenticazione per il distributore.

## Utilizzo dell’API di verifica {#using-verifier-api}

Il `ITokenVerifier` la classe definisce i metodi utilizzati per convalidare l’autenticità del token per una determinata risorsa. Utilizza il `ITokenVerifier` metodi per analizzare un token ricevuto in risposta a un `setToken()` richiesta.


Il `isValid()` Il metodo è il metodo principale per convalidare un token. Richiede un argomento, un ID risorsa. Se trasmetti un ID risorsa nullo, il metodo convalida solo l’autenticità del token e il periodo di validità.


Il `isValid()` il metodo restituisce uno dei seguenti valori di stato:



| VALID_TOKEN | Tutte le convalide completate |
|--------------------|-----------------------------------------|
| INVALID_TOKEN_FORMAT | Formato del token non valido |
| INVALID_SIGNATURE | Impossibile convalidare l’autenticità del token |
| TOKEN_EXPIRED | TTL del token non valido |
| INVALID_RESOURCE_ID | Token non valido per la risorsa specificata |
| ERRORE_SCONOSCIUTO | Il token non è ancora stato convalidato |

I metodi aggiuntivi forniscono un accesso specifico all’ID risorsa, al tempo di emissione e al time-to-live di un determinato token.

* Utilizzare `getResourceID()` per recuperare l’ID risorsa associato al token e confrontarlo con l’ID restituito dalla richiesta setToken().
* Utilizzare `getTimeIssued()` per recuperare l’ora di emissione del token.
* Utilizzare `getTimeToLive()` per recuperare il TTL.
* Utilizzare `getUserSessionGUID()` per recuperare un GUID anonimo impostato da MVPD.
* Utilizzare `getMvpdId()` per recuperare l&#39;ID del MVPD che ha autenticato l&#39;utente.
* Utilizzare `getProxyMvpdId()` per recuperare l&#39;ID del Proxy MVPD che ha autenticato l&#39;utente.

## Codice di esempio {#sample-code}

L’archivio Media Token Verifier contiene un’implementazione di riferimento (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) e un esempio di chiamata dell’API con la classe di test. Questo esempio (`com.adobe.entitlement.text.EntitlementVerifierTest.java`) illustra l’integrazione della libreria di verifica dei token in un server multimediale.


```Java
package com.adobe.entitlement.test; 
import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.tve.crypto.SignatureVerificationCredential; 
import java.io.InputStream; 

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
        System.out.println("RequestorID: " + requestorID);
        System.out.println("token: " + tokenToVerify);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        System.out.println("keystore passwd: " + keystorePasswd);
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
            try {
                // load the keystore file 
                ClassLoader loader = EntitlementVerifierTest.class.getClassLoader();
                InputStream certInputStream =  loader.getResourceAsStream(keystoreFile);
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null) {
            System.out.println("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```
