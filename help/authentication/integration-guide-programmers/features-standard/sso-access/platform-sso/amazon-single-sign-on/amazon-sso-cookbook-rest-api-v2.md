---
title: Manuale Amazon SSO (REST API V2)
description: Manuale Amazon SSO (REST API V2)
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Manuale Amazon SSO (REST API V2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

L’API REST per l’autenticazione di Adobe Pass V2 supporta l’SSO (Single Sign-On) per piattaforma per gli utenti finali delle applicazioni client in esecuzione su FireOS.

Questo documento funge da estensione della [Panoramica REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) esistente che fornisce una visualizzazione di alto livello e il documento che descrive come implementare [Single Sign-On utilizzando i flussi di identità della piattaforma](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Single sign-on Amazon tramite i flussi di identità della piattaforma {#cookbook}

Adobe Pass Authentication collabora con Amazon per migliorare l’esperienza di accesso degli utenti e facilitare il Single Sign-On (SSO) tra le applicazioni TV Everywhere per gli abbonati TV.

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

Il payload del token SSO di Amazon (identità della piattaforma) deve essere presente in tutte le richieste HTTP effettuate sugli endpoint REST API V2 di autenticazione di Adobe Pass:

```
/api/v2/*
```

L’API REST V2 per l’autenticazione di Adobe Pass supporta i seguenti metodi per ricevere il payload del token SSO (identità della piattaforma), che è un identificatore con ambito dispositivo o piattaforma:

* Come intestazione con nome: `Adobe-Subject-Token`

>[!IMPORTANT]
> 
> Per ulteriori dettagli sull&#39;intestazione `Adobe-Subject-Token`, consulta la documentazione [Adobe-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

#### Esempi

**Invio come intestazione**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> Nel caso in cui il valore dell&#39;intestazione `Adobe-Subject-Token` sia mancante o non valido, l&#39;autenticazione Adobe Pass gestirà le richieste senza tenere conto del Single Sign-On.
