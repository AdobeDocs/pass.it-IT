---
title: Disconnessione singola - Flusso
description: REST API V2 - Disconnessione singola - Flusso
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 0%

---


# Flusso disconnessione singola {#single-logout-flow}

>[!NOTE]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Avvia disconnessione singola per mvpd specifico {#initiate-single-logout-for-specific-mvpd}

### Prerequisiti {#prerequisites-initiate-single-logout-for-specific-mvpd}

Prima di avviare il logout singolo per un MVPD specifico, verificare che siano soddisfatti i seguenti prerequisiti:

* La seconda applicazione di streaming deve disporre di un profilo Single Sign-On valido creato correttamente per MVPD utilizzando uno dei flussi di autenticazione Single Sign-On:
   * [Eseguire l’autenticazione tramite single sign-on utilizzando l’identità della piattaforma](./rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [Eseguire l&#39;autenticazione tramite Single Sign-On utilizzando il token di servizio](./rest-api-v2-single-sign-on-service-token-flows.md)
* La seconda applicazione di streaming deve avviare il flusso di logout singolo quando deve uscire da MVPD.

>[!IMPORTANT]
> 
> Presupposti
>
> <br/>
> 
> * La prima e la seconda applicazione streaming ottengono lo stesso payload univoco dell&#39;identificatore della piattaforma di `JWS` o `JWE` oppure lo stesso payload univoco dell&#39;identificatore utente di `JWS`.

### Flusso di lavoro {#workflow-initiate-single-logout-for-specific-mvpd}

Eseguire i passaggi forniti per implementare il flusso di logout singolo per un MVPD specifico, come illustrato nel diagramma seguente.

![Avvia disconnessione singola per mvpd](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png) specifico

*Avvia disconnessione singola per mvpd* specifico

1. **Avvia disconnessione di Adobe Pass:** L&#39;applicazione di streaming raccoglie tutti i dati necessari per avviare il flusso di disconnessione chiamando l&#39;endpoint di disconnessione di Adobe Pass.

   Per informazioni dettagliate su: [Avvia disconnessione per la documentazione API specifica di mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md):
   * Tutti i parametri _required_, come `serviceProvider`, `mvpd` e `redirectUrl`
   * Tutte le intestazioni _required_, come `Authorization`, `AP-Device-Identifier`
   * Tutti i parametri e le intestazioni _optional_

   >[!IMPORTANT]
   > 
   > Prima di effettuare una richiesta, l’applicazione di streaming deve garantire di includere un valore valido per l’identificatore univoco della piattaforma o per l’identificatore univoco dell’utente.
   >
   > <br/>
   > 
   > Per ulteriori dettagli sull&#39;intestazione `Adobe-Subject-Token`, consulta la documentazione [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).
   > 
   > <br/>
   > 
   > Per ulteriori dettagli sull&#39;intestazione `AD-Service-Token`, consulta la documentazione [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Trova profili Single Sign-On e regolari:** Il server Adobe Pass identifica sia i profili regolari che quelli validi Single Sign-On in base ai parametri e alle intestazioni ricevuti.

1. **Elimina profili Single Sign-On e regolari:** Il server Adobe Pass elimina i profili Single Sign-On e Standard identificati dal backend di Adobe Pass.

1. **Indicare l&#39;azione successiva:** La risposta dell&#39;endpoint di disconnessione di Adobe Pass contiene i dati necessari per guidare l&#39;applicazione di streaming per quanto riguarda l&#39;azione successiva.

   Per informazioni dettagliate sulle informazioni fornite in una risposta di disconnessione, consultare la [Iniziare la disconnessione per la documentazione API mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) specifica.

   >[!IMPORTANT]
   >
   > L’endpoint di disconnessione di Adobe Pass convalida i dati della richiesta per garantire che siano soddisfatte le condizioni di base:
   >
   > * I parametri e le intestazioni _required_ devono essere validi.
   > * L&#39;integrazione tra `serviceProvider` e `mvpd` specificati deve essere attiva.
   >
   > <br/>
   > 
   > Se la convalida non riesce, verrà generata una risposta di errore che fornirà informazioni aggiuntive conformi alla documentazione di [Codici di errore avanzati](../../../enhanced-error-codes.md).

1. **Indicare disconnessione completata:** Se MVPD non supporta il flusso di disconnessione, l&#39;applicazione di streaming elabora la risposta e può utilizzarla per visualizzare facoltativamente un messaggio specifico nell&#39;interfaccia utente.

1. **Avvia disconnessione MVPD:** Se MVPD non supporta il flusso di disconnessione, l&#39;applicazione di streaming elabora la risposta e utilizza un agente utente per avviare il flusso di disconnessione con MVPD. Il flusso può includere diversi reindirizzamenti ai sistemi MVPD. Tuttavia, il risultato è che MVPD esegue la pulizia interna e invia la conferma di disconnessione finale al backend di Adobe Pass.

1. **Indicare disconnessione completata:** L&#39;applicazione di streaming può attendere che l&#39;agente utente raggiunga il `redirectUrl` specificato e può utilizzarlo come segnale per visualizzare facoltativamente un messaggio specifico nell&#39;interfaccia utente.

>[!NOTE]
>
> I passaggi per il singolo flusso di logout sono gli stessi di cui sopra, se avviati dalla prima applicazione di streaming.
