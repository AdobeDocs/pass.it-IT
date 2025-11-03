---
title: Autenticazione basata sulla home page (HBA)
description: Autenticazione basata sulla home page (HBA)
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 0%

---

# Autenticazione basata sulla home page (HBA) {#home-based-authentication}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

L&#39;HBA (Home-Based Authentication) è una funzionalità di TV Everywhere che consente agli abbonati alla TV a pagamento di accedere ai contenuti TV online senza immettere le credenziali MVPD quando sono connessi alla rete domestica, migliorando notevolmente l&#39;esperienza di autenticazione.

Secondo il Comitato per la tecnologia di autenticazione aperta (Open Authentication Technology Committee, OATC):

> &quot;L&#39;autenticazione automatica in-home è il processo mediante il quale un MVPD/OVD utilizza le caratteristiche della rete domestica (o identificatori accessibili automaticamente tra i dispositivi della rete domestica) per autenticare l&#39;account dell&#39;utente abbonato associato a quella rete domestica, eliminando la necessità per gli utenti di immettere manualmente le credenziali quando si avvia una sessione TVE per accedere al contenuto protetto.&quot;

Per ulteriori informazioni sull&#39;HBA (Home-Based Authentication) e sugli standard di settore pertinenti, fare riferimento alle risorse seguenti:

>[!MORELIKETHIS]
>
> * [Accesso immediato (HBA) da parte di CTAM](http://www.ctamtve.com/instantaccess)
> * [Casi di utilizzo e requisiti di autenticazione basata su Home da parte di OATC](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf)
> * [Infografica dell&#39;autenticazione basata su Home da Adobe](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640)
> * [Autenticazione con MVPD abilitati per OAuth2](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
> * [Autenticazione con MVPD abilitati per SAML](/help/authentication/integration-guide-mvpds/authn-usecase.md)
> * [Metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

## Vantaggi dell&#39;HBA {#hba-benefits}

L&#39;autenticazione basata sulla home page (HBA) è una funzionalità chiave che rimuove la barriera di accesso per i visualizzatori a casa con un abbonamento via cavo attivo. Questa barriera rappresenta una sfida importante per i servizi di TV Everywhere, con quasi la metà dei tentativi di accesso che hanno avuto esito negativo.

L&#39;HBA è in grado di migliorare notevolmente il coinvolgimento dei visualizzatori, fornendo un&#39;esperienza utente semplice e superiore per accedere ai contenuti TV Everywhere.

## Supporto HBA {#hba-support}

>[!IMPORTANT]
>
> Se si desidera sfruttare le funzionalità HBA, contattare il rappresentante commerciale di Adobe Pass per ulteriori informazioni, in quanto alcuni flussi HBA sono inclusi nel pacchetto Flusso di lavoro Premium.

L&#39;HBA è supportato da una serie di MVPD integrati con l&#39;autenticazione Adobe Pass, ma per beneficiare dell&#39;HBA potrebbe essere necessario eseguire alcuni passaggi aggiuntivi.

**MVPD SAML**

Per gli MVPD SAML, l&#39;HBA è attivato solo sul lato MVPD.

**MVPD OAuth2**

Per gli MVPD OAuth2, l&#39;HBA può essere attivato o disattivato tramite la [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) seguendo i passaggi della [Guida utente integrazioni dashboard TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

### MVPD {#hba-support-mvpds}

**MVPD SAML**

Nella tabella seguente viene fornita una panoramica degli MVPD abilitati per SAML che supportano gli HBA:

| MVPD | Funzionalità di base disponibili? | Invia un flag alla risposta di autenticazione? |
|--------------|--------------------------------|----------------------------------------|
| DirecTV | Sì | No |
| Piatto | Sì | No |
| Spettro | Sì | Sì |
| Cox | Sì | No |
| AT&amp;T | Sì | No |
| Verizon | Sì | Sì |
| Cablevision | Sì | No |
| Mediacom | Sì | No |
| Midcontinente | Sì | No |
| Massilon | Sì | No |
| AlticeOne | Sì | Sì |

**MVPD OAuth2**

Nella tabella seguente viene fornita una panoramica degli MVPD abilitati per OAuth2 che supportano l&#39;HBA:

| MVPD | Funzionalità di base disponibili? | Invia un flag alla risposta di autenticazione? |
|---------|--------------------------------|----------------------------------------|
| Comcast | Sì | Sì |

### Autenticazione Adobe Pass {#hba-support-adobe-pass-authentication}

Questa sezione descrive l&#39;esperienza abilitata per gli HBA e descrive il supporto fornito dall&#39;autenticazione Adobe Pass, evidenziando le funzioni chiave come:

* **Identificazione HBA:** capacità di indicare ai programmatori se l&#39;autenticazione è HBA o non HBA (richiede il supporto di MVPD).
* **TTL di autenticazione configurabili:** possibilità di impostare valori TTL (Time-To-Live) di autenticazione diversi per le autenticazioni HBA rispetto a quelle non HBA (richiede il supporto di MVPD).

Nella tabella seguente viene fornita una panoramica di alto livello dell&#39;esperienza utente in un HBA e del flusso di autenticazione regolare (non HBA):

| Tipo di autenticazione | Esperienza utente |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HBA | Gli utenti selezionano il proprio MVPD e vengono automaticamente autenticati quando si connettono alla rete domestica. Una volta scaduta l’autenticazione, gli utenti devono riselezionare il proprio MVPD, dopodiché verranno automaticamente autenticati di nuovo. |
| Regolare (non HBA) | Gli utenti selezionano il proprio MVPD e viene richiesto di immettere le proprie credenziali, indipendentemente dalla connessione a una rete domestica. Una volta scaduta l’autenticazione, gli utenti devono riselezionare il MVPD e immettere nuovamente le credenziali. |

**MVPD SAML**

Nella tabella seguente viene fornita una panoramica dell&#39;implementazione dell&#39;HBA nel caso di MVPD abilitati per SAML:

| Azioni utente | Azioni di sistema |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| L’utente tenta di riprodurre un video.<br/><br/>Viene visualizzato il selettore MVPD.<br/><br/>L&#39;utente seleziona il proprio MVPD e continua ad accedere. | Quando MVPD applica le regole per l’identificazione degli utenti, viene eseguito un controllo dei precedenti personali. Ad esempio, potrebbe essere necessario mappare l&#39;indirizzo IP dell&#39;utente all&#39;indirizzo MAC dei modem forniti dal distributore o dei set-top box connessi a banda larga. |
| Viene visualizzata una schermata che persiste per circa 3 secondi.<br/><br/>Una pagina interstiziale potrebbe informare l&#39;utente che l&#39;accesso verrà eseguito automaticamente con il proprio account MVPD. | L’applicazione apre l’URL di autenticazione in un agente utente, avviando una richiesta HTTP all’endpoint di autenticazione di Adobe Pass.<br/><br/>L&#39;endpoint di autenticazione Adobe Pass inoltra la richiesta all&#39;endpoint di autenticazione di MVPD tramite un reindirizzamento dell&#39;agente utente.<br/><br/>È previsto che MVPD invii una decisione di autenticazione sotto forma di risposta SAML che include il flag HBA (`hba_status`) con valore &quot;true&quot; o &quot;false&quot;.Il backend di autenticazione di <br/><br/>Adobe Pass invia una richiesta all&#39;endpoint del profilo utente di MVPD per esporre il flag `hba_status` come parte dei [metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md). |
| L’utente è autenticato e ora può sfogliare contenuti TV Everywhere autorizzati. | L’applicazione recupera il profilo utente e può procedere con il flusso delle decisioni per riprodurre il contenuto. |

**MVPD OAuth2**

La tabella seguente fornisce una panoramica dell&#39;implementazione dell&#39;HBA nel caso di MVPD abilitati per OAuth2:

| Azioni utente | Azioni di sistema |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| L’utente tenta di riprodurre un video.<br/><br/>Viene visualizzato il selettore MVPD.<br/><br/>L&#39;utente seleziona il proprio MVPD e continua ad accedere. | Quando MVPD applica le regole per l’identificazione degli utenti, viene eseguito un controllo dei precedenti personali. Ad esempio, potrebbe essere necessario mappare l&#39;indirizzo IP dell&#39;utente all&#39;indirizzo MAC dei modem forniti dal distributore o dei set-top box connessi a banda larga. |
| Viene visualizzata una schermata che persiste per circa 3 secondi.<br/><br/>Una pagina interstiziale potrebbe informare l&#39;utente che l&#39;accesso verrà eseguito automaticamente con il proprio account MVPD. | L’applicazione apre l’URL di autenticazione in un agente utente, avviando una richiesta HTTP all’endpoint di autenticazione di Adobe Pass.<br/><br/>L&#39;endpoint di autenticazione Adobe Pass inoltra la richiesta all&#39;endpoint di autenticazione di MVPD tramite un reindirizzamento dell&#39;agente utente.<br/><br/>L&#39;endpoint di autenticazione MVPD invia un codice di autorizzazione all&#39;endpoint di autenticazione Adobe Pass.<br/><br/>L&#39;autenticazione di Adobe Pass utilizza il codice di autorizzazione per richiedere un token di aggiornamento e un token di accesso dall&#39;endpoint del token di MVPD.<br/><br/>MVPD deve inviare una decisione di autenticazione che includa il flag HBA (`hba_status`) con un valore &quot;true&quot; o &quot;false&quot; come parte di `id_token`.Il backend di autenticazione di <br/><br/>Adobe Pass invia una richiesta all&#39;endpoint del profilo utente di MVPD per esporre il flag `hba_status` come parte dei [metadati utente](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).<br/><br/>MVPD imposta il TTL del token di aggiornamento su un valore concordato da MVPD e Adobe imposta il TTL di autenticazione su un valore minore o uguale al valore del token di aggiornamento. |
| L’utente è autenticato e ora può sfogliare contenuti TV Everywhere autorizzati. | L’applicazione recupera il profilo utente e può procedere con il flusso delle decisioni per riprodurre il contenuto. |

>[!IMPORTANT]
>
> Nel flusso HBA per MVPD che utilizzano il protocollo di autenticazione OAuth 2.0, MVPD emette un token di aggiornamento con un TTL definito dai requisiti aziendali, mentre Adobe emette un token di autenticazione HBA con un TTL che non deve superare il TTL del token di aggiornamento.

## Domande frequenti {#faqs}

1. Perché esiste una distinzione tra implementazione HBA per i protocolli SAML e OAuth2?

   La separazione tra l&#39;HBA (Home-Based Authentication) per i protocolli SAML e OAuth2 esiste perché questi protocolli operano in modo diverso in termini di meccanismi di autenticazione, configurazione e flessibilità di implementazione. Per gli MVPD SAML non è richiesta alcuna azione da parte del programmatore per abilitare l&#39;HBA, mentre per gli MVPD OAuth2 è possibile attivare o disattivare l&#39;HBA tramite [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication).

1. Quando l&#39;HBA è attivato, gli utenti devono comunque immettere il nome utente e la password durante l&#39;autenticazione iniziale?

   No, nome utente e password non sono obbligatori.

1. Come si può applicare il controllo genitori?

   L&#39;autenticazione Adobe Pass può disabilitare l&#39;HBA per le integrazioni con canali che richiedono l&#39;approvazione del controllo genitori. Inoltre, stiamo lavorando con OATC su un documento UX che consiglia come impostare l&#39;esperienza HBA con il controllo genitori.

1. I provider che supportano gli HBA utilizzano finestre TTL più brevi rispetto all&#39;autenticazione regolare?

   L’impostazione TTL è configurabile. È consigliabile impostare un TTL più breve per le integrazioni MVPD con HBA per evitare errori di gestione.
