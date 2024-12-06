---
title: Integrazione dei dati lato server di autenticazione di Adobe Pass in Adobe Analytics
description: Integrazione dei dati lato server di autenticazione di Adobe Pass in Adobe Analytics
exl-id: c1f1f2a3-c98c-4aed-92ad-1f9bfd80b82b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# Integrazione dei dati lato server di autenticazione di Adobe Pass in Adobe Analytics

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

I clienti di Adobe Pass Authentication desiderano visualizzare i dati lato server di Adobe Pass Authentication (Adobe Pass) nel dashboard di Adobe Analytics per semplificarne l’utilizzo.

I dati serviranno a tenere traccia di metriche TVE importanti, come i tassi di conversione di autenticazione per MVPD, gli utenti univoci in base all’ID utente MVPD e altro ancora.

Non intende sostituire un’implementazione lato client se ne esiste già una, in quanto l’attività utente non può essere tracciata oltre gli eventi specifici riportati di seguito in assenza di un ID visitatore. Se i clienti forniscono un ID visitatore nelle chiamate Pass, possiamo sbloccare un altro tipo di integrazione Analytics (in tempo reale) che può unire tutti gli eventi Pass ai dati esistenti del cliente. Ulteriori dettagli su questo nuovo tipo di possibile integrazione qui: &quot;[Utilizzo dell&#39;ID Experience Cloud nell&#39;autenticazione di Adobe Pass](/help/authentication/integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)&quot;

## Metriche incluse {#metrics-included-int-authn-analyt}

| Evento | Descrizione |
|----------------------------|----------------------------------------------------------------------------------------------------------------------|
| AuthN richiesta | Numero di flussi di autenticazione avviati |
| AuthN in sospeso | Numero di token di autenticazione generati correttamente (a prescindere dal fatto che il client lo abbia effettivamente ottenuto o meno) |
| AuthN OK | Numero di token di autenticazione ottenuti correttamente dagli utenti |
| AuthZ richiesta | Numero di tentativi di autorizzazione |
| AuthZ OK | Numero di autorizzazioni riuscite |
| AuthZ non riuscito | Numero di autorizzazioni negate dagli MVPD a livello di applicazione |
| Riproduci richiesta | Numero di token multimediali brevi generati (assimilati al numero di richieste di riproduzione) |
| Disconnessione richiesta | Numero di flussi di disconnessione avviati |
| Disconnessione completata | Numero di flussi di logout completati con successo |
| Disconnessione non riuscita | Numero di flussi di disconnessione non riusciti |
| Preautorizzazione richiesta | Numero di flussi di pre-autorizzazione avviati |
| Preautorizzazione OK | Numero di eventi di preautorizzazione riusciti con le risorse preautorizzate |
| Preautorizzazione negata | Numero di eventi di preautorizzazione con le risorse a cui è stata negata la preautorizzazione |
| Preautorizzazione non riuscita | Numero di eventi di preautorizzazione non riusciti |

| Nome Adobe Analytics | Descrizione |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Canale | ID richiedente utilizzato per eseguire la richiesta di adesione |
| MVPD | MVPD responsabile della concessione dell&#39;autorizzazione all&#39;utente |
| Proxy | Il proxy MVPD (che sarà &quot;Direct&quot; per le integrazioni dirette) |
| Tipo di SDK | L’SDK client utilizzato (Flash, HTML5, Android nativo, iOS, Clientless, ecc.) |
| Versione SDK | Versione dell’SDK del client di autenticazione di Adobe Pass |
| ID risorsa | Titolo effettivo della risorsa coinvolto nella richiesta di autorizzazione (estratto dal payload MRSS come articolo/titolo se fornito) |
| Tipo di errore AuthZ | Motivo degli errori, come segnalato dall&#39;autenticazione Adobe Pass <br/> Di seguito sono riportati i valori più comuni <br/> **noAuthZ** = MVPD ha risposto che l&#39;utente non ha il canale nel pacchetto<br/> **rete** = impossibile raggiungere MVPD (MVPD ha un problema al momento della chiamata e non ha risposto)<br/> **norefreshtoken** = strettamente per le implementazioni OAuth e ciò potrebbe verificarsi se l&#39;utente ha modificato la propria password o se l&#39;MVPD ha negato per qualche motivo. Generalmente genera una nuova autenticazione<br/> **mancata corrispondenza** = se la richiesta viene effettuata da un dispositivo diverso da quello che aveva il token di autenticazione. Può verificarsi se gli utenti tentano di ingannare il sistema, ma la maggior parte di queste si verifica nel contesto del vecchio SDK JavaScript, in cui l’ID dispositivo utilizzava l’indirizzo IP come parte del calcolo. Se un utente guardava TVE a casa e poi al lavoro, questo errore veniva attivato e doveva autenticarsi di nuovo<br/> **non valido** = richiesta non valida, parametri mancanti o non validi<br/>  **authzNone** = I programmatori possono negare le autorizzazioni per una specifica combinazione channelxMVPD. Attivato da un&#39;API di back-end a cui i programmatori hanno accesso<br/> **frode** = è un meccanismo di protezione dalla nostra parte. Se l’utente non riesce a ottenere l’autorizzazione e successivamente la richiede nuovamente un numero di volte in un breve intervallo (secondi), la chiamata viene negata direttamente. In genere si verifica quando un programmatore presenta un bug nell’implementazione che richiede costantemente l’autorizzazione in caso di errore. |
| Tipo di token | Quando i token vengono creati a causa di AuthZ All e AuthN All, dobbiamo sapere cosa viene fatto a causa di una misura di degradazione.<br/> Sono:<br/> &quot;normal&quot; = Caso normale<br/> &quot;authnall&quot; = Quando AuthN All è abilitato<br/> &quot;authzall&quot; = Quando AuthZ All è abilitato<br/> &quot;hba&quot; = Quando HBA è abilitato |
| Tipo di dispositivo senza client | La piattaforma del dispositivo (alternativa), attualmente utilizzata per Clientless.<br/> I valori possono essere:<br/> N/D - l&#39;evento non ha avuto origine da un SDK senza client<br/> Sconosciuto - Poiché il parametro deviceType di un&#39;API **senza client** è facoltativo, esistono chiamate che non contengono alcun valore.<br/> Qualsiasi altro valore inviato tramite **API senza client**. Ad esempio, xbox, appletv e roku. |
| ID utente MVPD | Sostituisce l&#39;ID visitatore basato su cookie |


## Dettagli {#details-int-authn-analyt}

* Le metriche verranno inserite, evento per evento, nella suite di rapporti specifica tramite l’API di inserimento dati di Adobe Analytics
* L’inserimento verrà inserito in batch e inviato ogni 30 minuti. Per questo motivo, il rapporto deve essere contrassegnato con la data e l’ora
* Ogni cliente avrà una o più suite di rapporti. Un ID richiedente (canale) verrà mappato solo su una suite di rapporti. Più ID richiedente possono essere mappati su una sola suite di rapporti.
* È possibile fornire dati storici, ma sarà necessario prestare particolare attenzione a causa di problemi di traffico/prestazioni.
* La variabile visitatore univoco è impostata sull’ID utente MVPD
* La mappatura di eventi ed eVar non è configurabile.


## SLA {#sla-int-authn-serv-anal}

Poiché non si tratta di un componente fondamentale, SLA non garantisce l&#39;integrazione del sistema.

## Configurazione suite di rapporti {#report-suite-config}

Il rapporto deve avere una marca temporale in quanto gli eventi verranno inviati in batch.

### Eventi {#report-suite-config-events}


>[!NOTE]
>Tutti devono essere impostati con:
>
>* Contatore (nessuna relazione secondaria)

| Evento | Evento Adobe Analytics |
|---------------------------------------|-----------------------|
| AuthN richiesta | event1 |
| AuthN in sospeso | event2 |
| AuthN OK | event3 |
| AuthZ richiesta | event4 |
| AuthZ OK | event5 |
| AuthZ non riuscito | event6 |
| Riproduci richiesta | event7 |
| AuthN non riuscito | event8 |
| Richiesta token di autenticazione senza client OK | event9 |
| Richiesta token di autenticazione senza client non riuscita | event10 |
| Richiesta di riproduzione non riuscita | event11 |
| Richiesta di disconnessione | event12 |
| Disconnessione completata | event13 |
| Disconnessione non riuscita | event14 |
| Richiesta di verifica preliminare | event15 |
| Verifica preliminare non riuscita | event16 |
| Verifica preliminare concessa | event17 |
| Verifica preliminare negata | event18 |


### eVar {#evars}


>[!NOTE]
>Tutti devono essere impostati con:
>
>* Allocazione: più recente (ultimo)
>* Scadenza dopo: hit
>* Tipo: Stringa di testo

| Proprietà | eVar |
|-----------------------------------|--------------------------------|
| Canale | EVAR 1 |
| MVPD | EVAR 2 |
| Proxy | EVAR 3 |
| Tipo di SDK | EVAR 4 |
| Versione SDK | EVAR 5 |
| ID risorsa | EVAR 6 |
| Tipo di errore AuthZ | EVAR 7 |
| Tipo di token | EVAR 8 |
| Tipo di dispositivo senza client | EVAR 9 |
| ID utente MVPD | visitorID (completato automaticamente) |
| ID utente MVPD | eVar 10 |
| Tipo di dispositivo | eVar 11 |
| Sistema operativo | eVar 12 |
| Tipo di hardware principale | eVar 13 |
| TTL | eVar 14 |
| Tipo di autenticazione | eVar 15 |
| Versione dell&#39;architettura del server | eVar 16 |
| Provider di autenticazione esterno | eVar 17 |
| Latenza | eVar 18 |
| ID visitatore | eVar 19 |
| Meccanismo SSO | eVar 20 |
| Modello dispositivo | eVar 21 |
| Versione dispositivo | eVar 22 |
| Modello hardware del dispositivo | eVar 23 |
| Fornitore hardware dispositivo | eVar 24 |
| Produttore hardware del dispositivo | eVar 25 |
| Versione hardware del dispositivo | eVar 26 |
| Nome SO dispositivo | eVar 27 |
| Famiglia SO dispositivo | eVar 28 |
| Fornitore SO dispositivo | eVar 29 |
| Versione SO dispositivo | eVar 30 |
| Nome browser dispositivi | eVar 31 |
| Fornitore browser dispositivi | eVar 32 |
| Versione browser dispositivi | eVar 33 |
| Tipo di dispositivo senza client normalizzato | eVar 34 |

## Prezzi {#pricing}

Contatta il tuo TAM per ulteriori informazioni.
