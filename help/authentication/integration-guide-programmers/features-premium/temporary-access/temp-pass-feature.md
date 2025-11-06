---
title: Funzione TempPass
description: Funzione TempPass
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '2203'
ht-degree: 0%

---

# Funzione TempPass {#temp-pass-feature}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

TempPass è una funzione versatile che consente ai programmatori di offrire accesso temporaneo ai loro contenuti protetti per gli utenti senza credenziali di account MVPD valide. Funge da strumento efficace per coinvolgere gli spettatori, sia attraverso scenari di accesso di base che attraverso campagne promozionali mirate.

TempPass è una soluzione potente che consente ai programmatori di:

* **Coinvolgi i visualizzatori:** offri un assaggio di contenuti premium per attrarre nuovi abbonati.
* **Promozioni per le unità:** esegui campagne mirate per aumentare l&#39;esposizione dei contenuti e fidelizzare il brand.
* **Mantieni controllo:** Gestisci i periodi di accesso, applica limiti e reimposta l&#39;accesso in base alle esigenze per allinearlo agli obiettivi aziendali.

La funzione TempPass viene fornita introducendo uno pseudo-MVPD (denominato ulteriormente &quot;Temp Pass&quot;) nella configurazione del server di autenticazione di Adobe Pass come integrazione con il programmatore partecipante. La funzione TempPass è disponibile in due configurazioni:

* [TempPass di base](#basic-temp-pass) per l&#39;accesso basato sul tempo.
* [Passaggio temporaneo promozionale](#promotional-temp-pass) per un accesso flessibile basato su campagne.

>[!IMPORTANT]
>
> La funzione TempPass è una funzione premium e richiede una licenza corrente da Adobe.

Nella tabella seguente viene fornito un breve confronto tra le funzioni TempPass di base e promozionale:

| **Funzionalità** | **Passaggio temporaneo di base** | **Passaggio temporaneo promozionale** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **Accesso al contenuto** | <ul><li>Basato su tempo</li></ul> | <ul><li>Basato su tempo</li><li>Limitato al numero massimo di risorse</li></ul> |
| **Accesso alla sicurezza basato su** | <ul><li>ID dispositivo</li></ul> | <ul><li>ID dispositivo</li><li>Hash delle informazioni sull’identificatore utente fornite (ad esempio, e-mail)</li></ul> |
| **Codici di errore migliorati** | Disponibile | Disponibile |
| **Funzionalità di ripristino TempPass** | Disponibile | Disponibile |

>[!IMPORTANT]
> 
> L’autenticazione di Adobe Pass non include un meccanismo integrato per arrestare automaticamente il flusso in corso una volta trascorso il tempo assegnato (X minuti). È responsabilità dei programmatori applicare le restrizioni di accesso una volta che il TempPass scade durante un flusso in corso.

TempPass ti offre gli strumenti per espandere il pubblico e al contempo mantenere il controllo sull’accesso, sia che tu stia fornendo una panoramica della tua libreria di contenuti o promuovendo un evento di perimetro di selezione.

## TempPass di base {#basic-temp-pass}

La funzione TempPass di base consente ai programmatori di fornire un accesso limitato nel tempo ai contenuti, in base a vari scenari:

* **Anteprime brevi:** anteprime brevi delle offerte, ad esempio un periodo di accesso giornaliero di 10 minuti, per attirare potenziali abbonati.
* **Accesso basato su eventi:** Abilita l&#39;accesso più lungo per gli eventi principali, ad esempio una sessione di 4 ore.
* **Accesso combinato:** combinare e abbinare le durate, ad esempio un periodo di visualizzazione esteso iniziale seguito da anteprime giornaliere più brevi nell&#39;arco di diversi giorni.

Alcuni eventi possono richiedere un accesso graduale gratuito ai contenuti, ad esempio un periodo iniziale di libero accesso esteso (ad esempio, 4 ore), seguito da intervalli giornalieri più brevi di libero accesso (ad esempio, 10 minuti al giorno). Per implementare questo scenario, i programmatori devono coordinarsi con il proprio rappresentante Adobe per configurare due MVPD TempPass su misura per le loro esigenze.

Ad esempio, per fornire una sessione gratuita iniziale di 4 ore seguita da sessioni gratuite giornaliere di 10 minuti, Adobe può configurare per il programmatore:

* **TempPass1**: configurato con un valore TTL (Time-To-Live) di 4 ore per coprire il periodo di accesso libero iniziale.
* **TempPass2**: configurato con un valore TTL (Time-To-Live) di 10 minuti per i successivi intervalli giornalieri di accesso libero.

Per garantire la funzionalità corretta per l&#39;accesso giornaliero, è necessario reimpostare TempPass2 per tutti i dispositivi alle 00:00 ore ogni giorno.

### Dettagli delle funzioni {#basic-temp-pass-feature-details}

**Parametri di configurazione:**

* **TTL (Time-To-Live):** I programmatori possono specificare la durata dell&#39;accesso. Questo TTL basato su orologio scade indipendentemente dall’ora di visualizzazione effettiva.

**Identificazione utente:**

La funzione TempPass di base utilizza l&#39;identificatore del dispositivo come parametro di identificazione utente.

La tabella seguente consente di comprendere in che modo i parametri di identificazione dell’utente influenzano l’esperienza di prova dell’utente:

| Identificatore dispositivo | Risultato |
|-------------------|----------------|
| Nuovo | Nuova versione di prova |
| Esistente | Versione di prova esistente |

**Calcolo ora di visualizzazione:**

Il TTL rappresenta la durata dal tempo della richiesta di autorizzazione iniziale al tempo di scadenza, indipendentemente dal tempo effettivo impiegato per visualizzare il contenuto. Ogni richiesta futura controlla l’ora del server corrente rispetto all’ora di scadenza memorizzata per autorizzare l’accesso.

**Autenticazione:**

L&#39;autenticazione non è necessaria per Basic TempPass, consentendo di procedere direttamente al passaggio di autorizzazione.

**Autorizzazione:**

Poiché non vi è alcuna interazione con un MVPD effettivo, il MVPD &quot;Temp Pass&quot; di base autorizzerà qualsiasi risorsa dato che il TempPass è valido. In caso di autorizzazione riuscita, la libreria Media Token Verifier rimane applicabile per la verifica del token multimediale e per garantire la convalida delle risorse prima di avviare la riproduzione del contenuto.

La decisione di autorizzazione si basa sui parametri di identificazione dell’utente e sul TTL configurato. Per ottenere un’autorizzazione corretta per una risorsa, una richiesta valida deve soddisfare le seguenti condizioni:

* **Durata non utilizzata:** Il tempo di scadenza viene calcolato aggiungendo il tempo della richiesta di autorizzazione iniziale (salvato nei nostri database) al TTL configurato. L&#39;ora corrente del server viene confrontata con l&#39;ora di scadenza per determinare se TempPass è ancora valido.

Nel caso in cui un utente superi il TTL configurato, non potrà più visualizzare il contenuto sullo stesso dispositivo a meno che il suo TempPass non venga reimpostato.

**Preautorizzazione:**

Quando viene effettuata una richiesta di preautorizzazione per un MVPD &quot;Temp Pass&quot; di base, la risposta restituisce l’intero elenco di risorse dalla richiesta come preautorizzato correttamente. Questo comportamento riflette la logica di autorizzazione, dato che le condizioni di autorizzazione si basano su limiti di tempo, anziché su risorse specifiche. Finché il vincolo di tempo è valido, le risorse richieste verranno autorizzate.

**Disconnessione:**

La disconnessione non è richiesta per Basic TempPass, consentendo di passare direttamente al passaggio di autenticazione utilizzando un utente effettivo MVPD.

**Dati di tracciamento e analisi:**

Durante il flusso TempPass di base, i dati di tracciamento utilizzano una versione con hash dell&#39;identificatore del dispositivo, con l&#39;identificatore MVPD impostato su &quot;Temp Pass&quot;. I programmatori devono distinguere le metriche TempPass dalle metriche MVPD standard nelle loro implementazioni di analisi.

## TempPass promozionale {#promotional-temp-pass}

La funzione TempPass promozionale estende le funzionalità del TempPass di base, progettato appositamente per l&#39;esecuzione di campagne promozionali. Questa funzione consente ai programmatori di coinvolgere gli utenti consentendo l’accesso a un numero predefinito di titoli VOD per un periodo di tempo specificato dopo la raccolta di un’identificazione utente valida, ad esempio un indirizzo e-mail.

Il TempPass promozionale include tutte le funzionalità del TempPass di base, con una maggiore flessibilità per:

* Definizione del numero massimo di titoli VOD accessibili durante il periodo promozionale.
* Impostazione del periodo di tempo durante il quale l&#39;accesso promozionale è valido.

Quando un utente supera i limiti di accesso predefiniti (numero di titoli VOD o durata), non può più visualizzare contenuti sullo stesso dispositivo o con lo stesso identificatore utente, a meno che il suo TempPass non venga reimpostato.

### Dettagli delle funzioni {#promotional-temp-pass-feature-details}

**Parametri di configurazione:**

* **Chiave informazioni utente:** Chiave utilizzata per comunicare l&#39;identificatore fornito dall&#39;utente, ad esempio un indirizzo di posta elettronica (ad esempio, chiave è posta elettronica).
* **Numero di risorse:** definisce il numero di titoli VOD a cui un utente può accedere.
* **TTL (Time-To-Live):** la durata durante la quale l&#39;utente può utilizzare le risorse consentite.

**Identificazione utente:**

La funzione Promotional TempPass utilizza l’hash dell’identificatore fornito dall’utente sopra l’identificatore del dispositivo come parametri di identificazione dell’utente.

>[!IMPORTANT]
>
> La convalida e l’hashing dell’identificatore fornito dall’utente sono gestiti dal programmatore, non da Adobe. Adobe non memorizza dati personali (PII, Personally Identifiable Information). Di conseguenza, il programmatore è responsabile della generazione e dell’invio di un hash dell’identificatore univoco fornito dall’utente durante l’interazione con le API di autenticazione di Adobe Pass.

Adobe consiglia di utilizzare la famiglia **SHA-2** o le funzioni specifiche **SHA-256**, **SHA-512** sui dati prima di inviarli ad Adobe. Ad esempio, **SHA-256** su **&quot;user@domain.com&quot;** è **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**.

La tabella seguente consente di comprendere in che modo i parametri di identificazione dell’utente influenzano l’esperienza di prova dell’utente:

| Hash dell&#39;identificatore fornito dall&#39;utente | Identificatore dispositivo | Risultato |
|-------------------------------|-------------------|---------------------------------------------------------|
| Nuovo | Nuovo | Nuova versione di prova |
| Esistente | Nuovo | Versione di prova esistente (basata sull’hash dell’identificatore fornito dall’utente) |
| Nuovo | Esistente | Versione di prova esistente (basata sull’identificatore del dispositivo) |
| Esistente | Esistente | Versione di prova esistente |

**Calcolo ora di visualizzazione:**

Il TTL rappresenta la durata dal tempo della richiesta di autorizzazione iniziale al tempo di scadenza, indipendentemente dal tempo effettivo impiegato per visualizzare il contenuto. Ogni richiesta futura controlla l’ora del server corrente rispetto all’ora di scadenza memorizzata per autorizzare l’accesso.

**Autenticazione:**

L&#39;autenticazione non è necessaria per il passaggio TempPass promozionale, che consente di procedere direttamente al passaggio di autorizzazione.

Per supportare l&#39;implementazione dell&#39;applicazione del programmatore, il Promotional TempPass espone le seguenti informazioni sui metadati dell&#39;utente, accessibili attraverso le chiavi corrispondenti:

* **`remaining_resources`**: indica il numero di risorse che l&#39;utente ha ancora il diritto di utilizzare.
* **`used_assets`**: fornisce un elenco delle risorse già utilizzate dall&#39;utente.
* **`expiration_date`**: visualizza la data di scadenza del passaggio temporaneo promozionale dell&#39;utente.

**Autorizzazione:**

Poiché non vi è alcuna interazione con un MVPD effettivo, il MVPD Promozionale &quot;Temp Pass&quot; autorizzerà qualsiasi risorsa dato che il TempPass è valido. In caso di autorizzazione riuscita, la libreria Media Token Verifier rimane applicabile per la verifica del token multimediale e per garantire la convalida delle risorse prima di avviare la riproduzione del contenuto.

La decisione di autorizzazione si basa sui parametri di identificazione dell’utente e sul numero configurato di risorse e TTL. Per ottenere un’autorizzazione corretta per una risorsa, una richiesta valida deve soddisfare le seguenti condizioni:

* **Durata non utilizzata:** Il tempo di scadenza viene calcolato aggiungendo il tempo della richiesta di autorizzazione iniziale (salvato nei nostri database) al TTL configurato. L&#39;ora corrente del server viene confrontata con l&#39;ora di scadenza per determinare se TempPass è ancora valido.
* **Risorse non utilizzate:** Il numero di risorse utilizzate viene registrato (salvato nei database). Il numero di risorse utilizzate viene confrontato con il numero di risorse configurato per determinare se TempPass è ancora valido.

Se un utente supera il TTL configurato o il numero di risorse, non potrà più visualizzare il contenuto sullo stesso dispositivo o con lo stesso identificatore fornito dall’utente, a meno che il suo TempPass non venga reimpostato.

**Preautorizzazione:**

Quando viene effettuata una richiesta di preautorizzazione per un MVPD &quot;Temp Pass&quot; promozionale, la risposta restituisce l’intero elenco di risorse dalla richiesta come preautorizzato correttamente. Questo comportamento riflette la logica di autorizzazione, dato che le condizioni di autorizzazione si basano sui limiti di tempo e sul numero totale di risorse a cui si accede, anziché su risorse specifiche. Se il vincolo di tempo è valido e il limite di risorse non è stato superato, le risorse richieste verranno autorizzate.

**Disconnessione:**

La disconnessione non è richiesta per il passaggio al passaggio di autenticazione Promozionale, che consente di passare direttamente al passaggio di autenticazione utilizzando un utente effettivo MVPD.

**Dati di tracciamento e analisi:**

Durante il flusso TempPass promozionale, i dati di tracciamento utilizzano una versione con hash dell&#39;identificatore del dispositivo, con l&#39;identificatore MVPD impostato su &quot;Temp Pass&quot;. I programmatori devono distinguere le metriche TempPass dalle metriche MVPD standard nelle loro implementazioni di analisi.

## Ripristina accesso API TempPass {#reset-tempass-api-access}

Prima di accedere all’API Reset TempPass, devi completare i passaggi richiesti nel processo di registrazione client dinamica (DCR). Questo processo obbligatorio garantisce che tu disponga del token di accesso necessario per interagire con l’API Reset TempPass.

Per istruzioni complete, consulta la documentazione [Panoramica registrazione client dinamico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Reimposta API TempPass - DELETE /reset-tempass/v3/reset {#reset-tempass-v3-reset}

Per reimpostare un TempPass specifico per un dispositivo o per tutti i dispositivi, l&#39;autenticazione Adobe Pass fornisce ai programmatori un&#39;API che funziona sia per il TempPass di base che per quello promozionale.

### Richiesta {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/reset-tempass/v3/reset</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">metodo</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parametri di query</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>Identificatore univoco interno associato al provider di servizi durante il processo di onboarding.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>L’identificatore univoco interno associato a TempPass durante il processo di onboarding.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            L'identificatore del dispositivo per il quale è valida l'operazione di ripristino.
            <br/><br/>
            Se non viene fornito alcun valore, l'operazione di ripristino viene applicata a tutti i dispositivi.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorizzazione</td>
      <td>La generazione del payload del token Bearer è descritta nella documentazione di <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Recupera token di accesso</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

### Risposta {#reset-tempass-v3-reset-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Codice</th>
      <th style="background-color: #EFF2F7;">Testo</th>
      <th style="background-color: #EFF2F7;">Descrizione</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Nessun contenuto</td>
      <td>
        Ripristino riuscito.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Richiesta non valida</td>
      <td>
        Richiesta non valida. Il client deve correggere la richiesta e riprovare.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non autorizzato</td>
      <td>
        Il token di accesso non è valido, il client deve ottenere un nuovo token di accesso e riprovare. Per ulteriori dettagli, consulta la documentazione <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Panoramica registrazione client dinamico</a>.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Non consentito</td>
      <td>
        Il token di accesso non è valido, il client deve ottenere nuove credenziali client e un nuovo token di accesso e riprovare. Per ulteriori dettagli, consulta la documentazione <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Panoramica registrazione client dinamico</a>.
      </td>
   </tr>
</table>

### Esempi {#reset-tempass-v3-reset-samples}

#### Ripristina TempPass per un dispositivo specifico {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### Reimposta TempPass per tutti i dispositivi {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## Ripristina API TempPass - DELETE /reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

Per reimpostare un TempPass specifico per una chiave generica (hash dell&#39;identificatore fornito dall&#39;utente) o per tutte le chiavi, l&#39;autenticazione Adobe Pass fornisce ai programmatori un&#39;API che funziona per il TempPass promozionale.

### Richiesta {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">percorso</td>
      <td>/reset-tempass/v3/reset/generic</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">metodo</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parametri di query</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>Identificatore univoco interno associato al provider di servizi durante il processo di onboarding.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>L’identificatore univoco interno associato a TempPass durante il processo di onboarding.</td>
      <td><i>obbligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chiave</td>
      <td>
            Hash dell'identificatore fornito dall'utente per il quale è valida l'operazione di ripristino.
            <br/><br/>
            Se non viene fornito alcun valore, l’operazione di ripristino viene applicata a tutti gli utenti.
      </td>
      <td>facoltativo</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Intestazioni</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorizzazione</td>
      <td>La generazione del payload del token Bearer è descritta nella documentazione di <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Recupera token di accesso</a>.</td>
      <td><i>obbligatorio</i></td>
   </tr>
</table>

### Risposta {#reset-tempass-v3-reset-generic-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Codice</th>
      <th style="background-color: #EFF2F7;">Testo</th>
      <th style="background-color: #EFF2F7;">Descrizione</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Nessun contenuto</td>
      <td>
        Ripristino riuscito.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Richiesta non valida</td>
      <td>
        Richiesta non valida. Il client deve correggere la richiesta e riprovare.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non autorizzato</td>
      <td>
        Il token di accesso non è valido, il client deve ottenere un nuovo token di accesso e riprovare. Per ulteriori dettagli, consulta la documentazione <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Panoramica registrazione client dinamico</a>.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Non consentito</td>
      <td>
        Il token di accesso non è valido, il client deve ottenere nuove credenziali client e un nuovo token di accesso e riprovare. Per ulteriori dettagli, consulta la documentazione <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Panoramica registrazione client dinamico</a>.
      </td>
   </tr>
</table>

### Esempi {#reset-tempass-v3-reset-generic-samples}

#### Reimposta TempPass per una chiave specifica {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### Reimposta TempPass per tutte le chiavi {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## API REST V2 {#rest-api-v2}

Per utilizzare la funzione TempPass è necessario implementare aggiornamenti del codice per modificare l&#39;interazione dell&#39;applicazione TV Everywhere (TVE) con l&#39;autenticazione Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

Per una guida completa su questi aggiornamenti e sui flussi di lavoro associati, consulta la documentazione [Flussi di accesso temporanei](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).
