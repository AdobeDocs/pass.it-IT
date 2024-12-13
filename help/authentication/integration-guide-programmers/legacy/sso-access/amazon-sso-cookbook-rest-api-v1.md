---
title: Manuale Amazon SSO (REST API V1)
description: Manuale Amazon SSO (REST API V1)
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 0%

---

# (Legacy) Manuale di Amazon SSO (REST API V1) {#amazon-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

L’API REST per l’autenticazione di Adobe Pass V1 supporta l’SSO (Single Sign-On) per piattaforma per gli utenti finali delle applicazioni client in esecuzione su FireOS.

Questo documento funge da estensione della [Panoramica API REST V1](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md) esistente che fornisce una visualizzazione di alto livello.

## Single sign-on Amazon tramite i flussi di identità della piattaforma {#cookbook}

### Prerequisiti {#prerequisites}

Prima di procedere con il Single Sign-On Amazon utilizzando i flussi di identità della piattaforma, verifica che siano soddisfatti i seguenti prerequisiti.

#### Integrare Amazon SSO SDK {#integrate-amazon-sso-sdk}

L&#39;applicazione di streaming deve integrare nella propria build la libreria [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) per Single Sign-On (SSO).

* Scaricare e copiare la libreria SDK SSO di Amazon più recente in una cartella `/SSOEnabler` parallela alla directory dell&#39;applicazione.

* Aggiorna i file manifest e Gradle per utilizzare la libreria SDK SSO di Amazon.

  **Manifesto:**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Livello:**

  In archivi:

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  In dipendenze:

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Utilizzare Amazon SSO SDK {#use-amazon-sso-sdk}

L’applicazione di streaming deve utilizzare Amazon SSO SDK per ottenere il payload del token SSO (identità della piattaforma).

Amazon SSO SDK fornisce API sia sincrone che asincrone per ottenere il payload del token SSO (identità della piattaforma).

L’applicazione di streaming può scegliere una delle due opzioni in base alla propria architettura.

##### API asincrone

* Ottieni l&#39;istanza `SSOEnabler` e imposta `SSOEnablerCallback`:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```

  Questa operazione può essere eseguita durante l’inizializzazione dell’applicazione di streaming.

  ```JAVA
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

  Il bundle di risposta di successo del token SSO conterrà:
   * Un token SSO come `string` con chiave &quot;SSOToken&quot;.

  <br/>

  Il bundle di risposta in caso di errore del token SSO conterrà:
   * Codice di errore come `int` con chiave &quot;ErrorCode&quot;.
   * Descrizione di errore come `string` con chiave &quot;ErrorDescription&quot;.

  <br/>

* Ottieni il token SSO:

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  Questa API fornirà la risposta tramite callback impostati durante l’inizializzazione.

##### API sincrone

* Ottieni l&#39;istanza `SSOEnabler`:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* Ottieni il token SSO:

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  Questa API blocca il thread chiamante e risponde con il bundle dei risultati. Poiché si tratta di una chiamata sincrona, non utilizzarla nel thread principale.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  Questa API imposterà il valore di timeout per la chiamata sincrona. Il valore di timeout predefinito è 1 minuto.

#### Fallback per Amazon SSO {#fallback-amazon-sso}

L’applicazione di streaming deve gestire gli scenari di fallback dal flusso SSO di Amazon al flusso di autenticazione regolare.

Assicurati che l’applicazione di streaming gestisca:

* L’assenza dell’applicazione ausiliaria Amazon che deve essere in esecuzione sul dispositivo Amazon.
   * L&#39;applicazione di streaming potrebbe incontrare un `ClassNotFoundException` in fase di runtime nella seguente classe `com.amazon.ottssotokenlib.SSOEnabler`.

* L’assenza del payload del token SSO (identità della piattaforma) che deve essere restituito dalle API precedenti.
   * L’applicazione di streaming può contattare i rappresentanti di Amazon e Adobe per effettuare un’indagine.

### Flusso di lavoro {#workflow}

Il payload del token SSO di Amazon (identità della piattaforma) deve essere presente in tutte le richieste HTTP effettuate sugli endpoint di autenticazione di Adobe Pass:

```
/adobe-services/*
/reggie/*
/api/*
```

>[!IMPORTANT]
> 
> L&#39;applicazione di streaming potrebbe ignorare l&#39;invio del payload del token SSO di Amazon (identità della piattaforma) nella chiamata `/authenticate`, come specificato nella chiamata `/regcode`.

L’autenticazione Adobe Pass supporta i seguenti metodi per ricevere il payload del token SSO (identità della piattaforma), che è un identificatore con ambito dispositivo o piattaforma:

* Come intestazione con nome: `Adobe-Subject-Token`
* Come parametro di query denominato: `ast`
* Come parametro post denominato: `ast`

>[!IMPORTANT]
>
> Se inviato come parametro di query, l’intero URL potrebbe diventare molto lungo e rifiutato.
>
> Se inviato come parametro query/post, deve essere incluso durante la generazione della firma della richiesta.

#### Esempi

**Invio come intestazione**

```HTTPS
GET /api/v1/config/{requestorId} HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**Invio come parametro di query**

```HTTPS
GET /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com
```

**Invio come parametro post**

```HTTPS
POST /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com 
Content-Type: multipart/form-data;
```

>[!IMPORTANT]
>
> Nel caso in cui l&#39;intestazione `Adobe-Subject-Token` o il valore del parametro `ast` sia mancante o non valido, Adobe Pass Authentication gestirà le richieste senza tenere conto del Single Sign-On.
