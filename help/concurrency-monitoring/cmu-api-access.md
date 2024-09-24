---
title: Accesso API CMU
description: Accesso API CMU
exl-id: 8d216703-aabc-489e-93fe-d4d105616b1d
source-git-commit: 7107d4a915113fb237602143aafc350b776c55d6
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 0%

---

# Accesso all’API di utilizzo del monitoraggio della concorrenza {#cmu-api-usage-access}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato. Per domande sulla disponibilità, contatta il rappresentante di Adobe.

## Panoramica sulla procedura di accesso {#api-access-procedure-overview}

Abbiamo aggiornato l’accesso ai rapporti CMU per renderli compatibili con il protocollo di registrazione client dinamico OAuth 2.0. Un server di autorizzazione OAuth 2.0 personalizzato viene distribuito per soddisfare le esigenze dell&#39;applicazione di monitoraggio della concorrenza. \
Affinché le applicazioni client possano utilizzare l&#39;autorizzazione OAuth 2.0, il server deve registrarsi in modo dinamico per ottenere informazioni specifiche (credenziali client) per poter interagire con essa. Come parte del processo di registrazione, il client deve presentare un set di metadati incorporati all’endpoint di registrazione del client.
Questi metadati vengono comunicati come un&#39;istruzione software, che contiene un &quot;software_id&quot; per consentire al nostro server di autorizzazione di correlare diverse istanze di un&#39;applicazione utilizzando la stessa istruzione software.
Un’istruzione software è un JSON Web Token (JWT) che asserisce i valori dei metadati del software client come bundle. Quando viene presentata al server di autorizzazione come parte di una richiesta di registrazione client, l&#39;istruzione software deve essere firmata digitalmente o MACed utilizzando la firma Web JSON (JWS). \
Per ulteriori informazioni sulle istruzioni software e sul loro funzionamento, vedere la documentazione ufficiale <a href="https://datatracker.ietf.org/doc/html/rfc7591" target="_blank">[RFC7591]</a>.
Per ottenere l’accesso, segui i passaggi descritti nelle sezioni seguenti.

## Passaggi della procedura di accesso {#access-procedure-steps}

1. Avere un&#39;applicazione registrata nel server Adobe Pass DCR. Per questo passaggio, contatta il nostro [team di supporto](mailto:tve-support@adobe.com).

2. Ottieni il rendiconto del software
   1. Vai a [Dashboard TVE di Adobe Pass](https://experience.adobe.com/#/pass/authentication)
   2. Seleziona programmatore
   3. Vai alla scheda *Applicazioni registrate*
   4. Seleziona applicazione
   5. Fare clic su Scarica nella riga dell&#39;applicazione registrata per la quale si desidera ottenere un&#39;istruzione software e salvarla come file nel computer locale
      <figure>
          <img src="assets/programmer-download-software-statement-button.png"
               alt="Scarica la dichiarazione del software">
      </figure>

      <figure>
          <img src="assets/software_statement_2.png"
               alt="Esempio di istruzioni software">
      </figure>

3. Ottieni token di accesso
   1. Ottieni le credenziali del client utilizzando l’istruzione software ottenuta in precedenza ed eseguendo la chiamata di seguito. In questo modo verrà ottenuta una coppia client_id - client_secret, che può essere utilizzata per ottenere il token di accesso.
      *Questo passaggio non deve essere eseguito ogni volta. Questa operazione deve essere ripetuta solo alla scadenza delle credenziali.*
      <figure>
          <img src="assets/dcr_request_1_get_client_credentials.png"
               alt="Ottieni credenziali client">
       </figure>

   2. Ottieni il token di accesso utilizzando la chiamata di seguito. Utilizza questo token di accesso per chiamare qualsiasi API CMU fino alla scadenza del token.
      *Questo passaggio deve essere eseguito solo se l&#39;ultimo token generato è scaduto.*
      <figure>
          <img src="assets/dcr_get_access_token_call.png"
               alt="Ottieni token di accesso">
       </figure>

4. Chiama API CMU - consulta le relative informazioni di seguito.
   <figure>
          <img src="assets/call_cmu_reports_sample.png"
               alt="Chiama API CMU">
       </figure>

## Informazioni correlate {#related-information}

* [Panoramica CMU](/help/concurrency-monitoring/cm-usage-reports.md)
* [API CMU](/help/concurrency-monitoring/cmu-api.md)
