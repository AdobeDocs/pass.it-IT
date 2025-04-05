---
title: Elenco di controllo REST API V2
description: Elenco di controllo REST API V2
source-git-commit: f0001d86f595040f4be74f357c95bd2919dadf15
workflow-type: tm+mt
source-wordcount: '2535'
ht-degree: 0%

---

# Elenco di controllo REST API V2 {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questo documento aggrega in un&#39;unica posizione i requisiti obbligatori e le procedure consigliate per i programmatori che implementano applicazioni client che utilizzano l&#39;autenticazione Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

Il seguente documento deve essere considerato parte dei criteri di accettazione durante l’implementazione dell’API REST V2 e deve essere utilizzato come elenco di controllo per garantire che siano stati eseguiti tutti i passaggi necessari per ottenere una corretta integrazione.

## Requisiti obbligatori {#mandatory-requirements}

### 1. Fase di registrazione {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisiti</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Ambito applicazione registrato</i></td>
      <td>Utilizza un’applicazione registrata con ambito REST API v2.</td>
      <td>Rischi che scatenano risposte di errore HTTP 401 "Non autorizzato", sovraccarico delle risorse di sistema e aumento della latenza.</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>Memorizzazione in cache delle credenziali client</i></td>
      <td>Memorizza le credenziali del client nell’archivio permanente e riutilizzale per ogni richiesta di token di accesso.</td>
      <td>Rischia la perdita dell'autenticazione quando vengono rigenerate le credenziali del client.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Memorizzazione in cache dei token di accesso</i></td>
      <td>Memorizza i token di accesso nell’archiviazione persistente e riutilizzali fino alla scadenza; non richiedere un nuovo token per ogni chiamata API REST v2.</td>
      <td>Rischi di sovraccarico delle risorse di sistema, aumento della latenza e potenziale attivazione delle risposte di errore HTTP 429 "Troppe richieste".</td>
   </tr>
</table>

### 2. Fase di configurazione {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisiti</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Recupero configurazione</i></td>
      <td>Recupera la risposta di configurazione solo quando viene richiesto di richiedere all’utente di selezionare il proprio MVPD (provider TV) prima della fase di autenticazione.<br/><br/>Non è necessario recuperare la risposta di configurazione quando:<ul><li>Utente già autenticato.</li><li>All’utente viene offerto l’accesso temporaneo.</li><li>L’autenticazione dell’utente è scaduta, ma è possibile richiedere all’utente di confermare che è ancora un abbonato del MVPD selezionato in precedenza.</li></ul></td>
      <td>Rischi di sovraccarico delle risorse di sistema e aumento della latenza.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Memorizzazione in cache della selezione del provider TV</i></td>
      <td>Memorizza la selezione del provider di Pay TV (MVPD) dell'utente in un archivio permanente per utilizzarla in tutte le fasi successive:<ul><li>Dall’archivio di risposta della configurazione, l’utente ha selezionato "id" MVPD.</li><li>Dall’archivio di risposta della configurazione, l’utente ha selezionato MVPD "displayName".</li><li>Dall’archivio di risposta della configurazione, l’utente ha selezionato "logoUrl" MVPD.</li></ul></td>
      <td>Rischi di sovraccarico delle risorse di sistema e aumento della latenza.</td>
   </tr>
</table>

### 3. Fase di autenticazione {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisiti</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Avvio meccanismo di polling</i></td>
      <td>Avvia il meccanismo di polling nelle seguenti condizioni:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Autenticazione eseguita nell'applicazione primaria (schermata)</a></b><ul><li>L’applicazione primaria (streaming) deve avviare il polling quando l’utente raggiunge la pagina di destinazione finale, dopo che il componente browser carica l’URL specificato per il parametro "redirectUrl" nella richiesta dell’endpoint "Sessions".</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Autenticazione eseguita all'interno di un'applicazione secondaria (schermo)</a></b><ul><li>L’applicazione principale (streaming) deve avviare il polling non appena l’utente avvia il processo di autenticazione, subito dopo aver ricevuto la risposta dell’endpoint Sessions e aver visualizzato il codice di autenticazione per l’utente.</li></ul></td>
      <td>Rischi di sovraccarico delle risorse di sistema, aumento della latenza e potenziale attivazione delle risposte di errore HTTP 429 "Troppe richieste".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Arresto del meccanismo di polling</i></td>
      <td>Arresta il meccanismo di polling nelle seguenti condizioni:<br/><br/><b>Autenticazione riuscita</b><ul><li>Le informazioni del profilo dell’utente vengono recuperate correttamente, confermando il loro stato di autenticazione, pertanto il polling non è più necessario.</li></ul><br/><b>Sessione di autenticazione e scadenza del codice</b><ul><li>La sessione di autenticazione e il codice scadono, l’utente deve riavviare il processo di autenticazione e il polling che utilizza il codice di autenticazione precedente deve essere interrotto immediatamente.</li></ul><br/><b>Nuovo codice di autenticazione generato</b><ul><li>Se l’utente richiede un nuovo codice di autenticazione, la sessione esistente viene invalidata e il polling che utilizza il codice di autenticazione precedente deve essere interrotto immediatamente.</li></ul></td>
      <td>Rischi di sovraccarico delle risorse di sistema, aumento della latenza e potenziale attivazione delle risposte di errore HTTP 429 "Troppe richieste".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Configurazione meccanismo di polling</i></td>
      <td>Configura la frequenza del meccanismo di polling nelle seguenti condizioni:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Autenticazione eseguita nell'applicazione primaria (schermata)</a></b><ul><li>L’applicazione principale (streaming) deve eseguire il polling ogni 3-5 secondi.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Autenticazione eseguita all'interno di un'applicazione secondaria (schermo)</a></b><ul><li>L’applicazione principale (streaming) deve eseguire il polling ogni 3-5 secondi.</li></ul></td>
      <td>Rischi di sovraccarico delle risorse di sistema, aumento della latenza e potenziale attivazione delle risposte di errore HTTP 429 "Troppe richieste".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Memorizzazione in cache dei profili</i></td>
      <td>Memorizza parti delle informazioni del profilo dell’utente nell’archiviazione persistente per migliorare le prestazioni e ridurre al minimo le chiamate REST API v2 non necessarie.<br/><br/>La memorizzazione nella cache deve concentrarsi sui seguenti campi di risposta dei profili:<br/><br/><b>mvpd</b><ul><li>L'applicazione client può utilizzarlo per tenere traccia del provider TV selezionato dall'utente e continuare a utilizzarlo ulteriormente durante le fasi di pre-autorizzazione o autorizzazione.</li><li>Alla scadenza del profilo utente corrente, l’applicazione client può utilizzare la selezione di MVPD memorizzata e chiedere semplicemente all’utente di confermare.</li></ul><br/><b>attributi</b><ul><li>Utilizzato per personalizzare l’esperienza utente in base a diverse chiavi di metadati utente (ad esempio, zip, maxRating, ecc.).</li><li>I metadati dell’utente diventano disponibili al termine del flusso di autenticazione, pertanto l’applicazione client non deve eseguire query su un endpoint separato per recuperare le informazioni sui metadati dell’utente, in quanto sono già incluse nelle informazioni del profilo.</li><li>Alcuni attributi di metadati possono essere aggiornati durante la fase di autorizzazione, a seconda di MVPD (ad esempio, Charter) e dell’attributo di metadati specifico (ad esempio, familyID). Di conseguenza, l’applicazione client potrebbe dover eseguire nuovamente la query sulle API dei profili dopo l’autorizzazione per recuperare i metadati dell’utente più recenti.</li></ul></td>
      <td>Rischi di sovraccarico delle risorse di sistema, aumento della latenza e potenziale attivazione delle risposte di errore HTTP 429 "Troppe richieste".</td>
   </tr>
</table>

### 4. (Facoltativo) Fase di pre-autorizzazione {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisiti</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Recupero decisioni di pre-autorizzazione</i></td>
      <td>Utilizza le decisioni di preautorizzazione per il filtro dei contenuti e mai per le decisioni di riproduzione.</td>
      <td>Rischi che violano gli accordi contrattuali tra programmatore, MVPD e Adobe.<br/><br/>Rischio di aggirare i sistemi di monitoraggio e di avviso.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Recupero decisioni di pre-autorizzazione - Nuovo tentativo</i></td>
      <td>Gestisci i <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">codici di errore avanzati</a> in modo appropriato e utilizza il campo Azione per determinare i passaggi di correzione necessari.<br/><br/>Solo un numero limitato di codici di errore avanzati richiede un nuovo tentativo, mentre la maggior parte richiede risoluzioni alternative come specificato nel campo Azione.<br/><br/>Assicurarsi che qualsiasi meccanismo di ripetizione dei tentativi implementato per recuperare le decisioni di pre-autorizzazione non generi un loop infinito e che limiti i tentativi a un numero ragionevole (ad esempio, 2-3).</td>
      <td>Rischi di sovraccarico delle risorse di sistema, aumento della latenza e potenziale attivazione delle risposte di errore HTTP 429 "Troppe richieste".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Decisioni di pre-autorizzazione nella cache</i></td>
      <td>Memorizza nella cache le decisioni di autorizzazione riuscite per migliorare le prestazioni e ridurre al minimo le chiamate REST API v2 non necessarie, in quanto gli aggiornamenti degli abbonamenti durante l’esecuzione dell’applicazione non sono frequenti.</td>
      <td>Rischi di sovraccarico delle risorse di sistema, aumento della latenza e potenziale attivazione delle risposte di errore HTTP 429 "Troppe richieste".</td>
   </tr>
</table>

### 5. Fase di autorizzazione {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisiti</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Recupero decisioni di autorizzazione</i></td>
      <td>Ottenere decisioni di autorizzazione prima della riproduzione, indipendentemente dall'esistenza di una decisione di preautorizzazione.<br/><br/>Consenti ai flussi di continuare senza interruzioni anche se il token multimediale scade durante la riproduzione e richiedi una nuova decisione di autorizzazione contenente un token multimediale (nuovo) quando l'utente effettua la richiesta di riproduzione successiva, indipendentemente dal fatto che si tratti della stessa risorsa o di una risorsa diversa.<br/><br/>I flussi attivi in esecuzione per periodi di tempo prolungati possono scegliere di richiedere una nuova decisione di autorizzazione in seguito a operazioni video quali la sospensione del contenuto, l'avvio di interruzioni commerciali o la modifica di configurazioni a livello di risorsa quando il sistema MRSS subisce modifiche.</td>
      <td>Rischi che violano gli accordi contrattuali tra programmatore, MVPD e Adobe.<br/><br/>Rischio di aggirare i sistemi di monitoraggio e di avviso.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Nuovo tentativo di recupero decisioni di autorizzazione</i></td>
      <td>Gestisci i <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">codici di errore avanzati</a> in modo appropriato e utilizza il campo Azione per determinare i passaggi di correzione necessari.<br/><br/>Solo un numero limitato di codici di errore avanzati richiede un nuovo tentativo, mentre la maggior parte richiede risoluzioni alternative come specificato nel campo Azione.<br/><br/>Assicurarsi che qualsiasi meccanismo di esecuzione di nuovi tentativi implementato per recuperare le decisioni di autorizzazione non generi un loop infinito e che limiti i nuovi tentativi a un numero ragionevole (ad esempio, 2-3).</td>
      <td>Rischi di sovraccarico delle risorse di sistema, aumento della latenza e potenziale attivazione delle risposte di errore HTTP 429 "Troppe richieste".</td>
   </tr>
</table>

### 6. Fase di disconnessione {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisiti</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Supporto disconnessione</i></td>
      <td>Implementa l’API di logout per consentire agli utenti di disconnettersi manualmente, terminando il loro profilo autenticato e seguire il nome dell’azione REST API v2 specificato per ogni profilo rimosso:<ul><li>Per gli MVPD che supportano un endpoint di logout, l’applicazione client deve passare all’"url" fornito in un user-agent.</li><li>Per i profili di tipo "appleSSO", l’applicazione client richiede di guidare l’utente anche alla disconnessione dal livello partner (impostazioni di sistema di Apple).</li></ul></td>
      <td>Rischia il malfunzionamento dell'applicazione client a causa del supporto mancante sul lato dell'applicazione client.</td>
   </tr>
</table>

### 7. Parametri e intestazioni {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisiti</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Intestazione di autorizzazione Invio</i></td>
      <td>Invia l'intestazione <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a> per ogni richiesta REST API v2.</td>
      <td>Rischi che scatenano risposte di errore HTTP 401 "Non autorizzato", sovraccarico delle risorse di sistema e aumento della latenza.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Invia intestazione AP-Device-Identifier</i></td>
      <td>Invia l'intestazione <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> per ogni richiesta REST API v2.<br/><br/>Anche quando la richiesta proviene da un server per conto di un dispositivo, il valore dell'intestazione AP-Device-Identifier deve riflettere l'identificatore effettivo del dispositivo di streaming.</td>
      <td>Rischi che attivano le risposte di errore HTTP 400 "Richiesta non valida", sovraccarico delle risorse di sistema e aumento della latenza.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Intestazione Send X-Device-Info</i></td>
      <td>Invia l'intestazione <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> per ogni richiesta REST API v2.<br/><br/>Anche quando la richiesta proviene da un server per conto di un dispositivo, il valore dell'intestazione X-Device-Info deve riflettere le informazioni effettive sul dispositivo di streaming.</td>
      <td>Rischi classificati come provenienti da una piattaforma sconosciuta e trattati come insicuri, soggetti a norme più restrittive, come TTL di autenticazione più brevi.<br/><br/>Inoltre, alcuni campi, come connectionIp e connectionPort del dispositivo di streaming, sono obbligatori per funzionalità quali l'autenticazione di base di Spectrum.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Identificatore dispositivo stabile</i></td>
      <td>Calcola e archivia un identificatore di dispositivo stabile che non cambia in seguito a aggiornamenti o riavvii per l'intestazione <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.<br/><br/>Per le piattaforme senza un identificatore hardware, generare un identificatore univoco dagli attributi dell'applicazione e mantenerlo.</td>
      <td>Rischia di perdere l’autenticazione quando l’identificatore del dispositivo cambia.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Segui i riferimenti delle API</i></td>
      <td>Assicurati di inviare solo i parametri e le intestazioni previsti dell’API REST v2.</td>
      <td>Rischi che attivano le risposte di errore HTTP 400 "Richiesta non valida", sovraccarico delle risorse di sistema e aumento della latenza.</td>
   </tr>
</table>

### 8. Gestione degli errori {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisiti</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Supporto migliorato per la gestione dei codici di errore</i></td>
      <td>Gestisci i <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">codici di errore avanzati</a> in modo appropriato e utilizza il campo Azione per determinare i passaggi di correzione necessari.<br/><br/>Solo un numero limitato di codici di errore avanzati richiede un nuovo tentativo, mentre la maggior parte richiede risoluzioni alternative come specificato nel campo Azione.<br/><br/>La maggior parte dei codici di errore avanzati elencati nella documentazione di <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2">Codici di errore avanzati - REST API V2</a> può essere impedita completamente se gestita correttamente durante la fase di sviluppo prima di avviare l'applicazione.</td>
      <td>Rischi di sovraccarico delle risorse di sistema, aumento della latenza e potenziale attivazione delle risposte di errore HTTP 429 "Troppe richieste".<br/><br/>Rischia il malfunzionamento dell'applicazione client a causa di una gestione mancante dei codici di errore avanzati, causando messaggi di errore non chiari, istruzioni utente non corrette o comportamenti di fallback non corretti.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Supporto per la gestione degli errori HTTP</i></td>
      <td>Differenziare tra la gestione delle risposte di errore HTTP (ad es. 400, 401, 403, 404, 405, 500) e delle risposte di successo (ad es. 200, 201) che contengono payload di codici di errore avanzati, come descritto in precedenza.<br/><br/>Solo un numero limitato di codici di errore HTTP richiede un nuovo tentativo, mentre la maggior parte richiede risoluzioni alternative.<br/><br/>La maggior parte delle risposte di errore HTTP può essere evitata completamente se gestita correttamente durante la fase di sviluppo prima di avviare l'applicazione.</td>
      <td>Rischi di sovraccarico delle risorse di sistema, aumento della latenza e potenziale attivazione delle risposte di errore HTTP 429 "Troppe richieste".<br/><br/>Rischia il malfunzionamento dell'applicazione client a causa di una gestione mancante dei codici di errore avanzati, causando messaggi di errore non chiari, istruzioni utente non corrette o comportamenti di fallback non corretti.</td>
   </tr>
</table>

### 9. Prove {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisiti</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Test del ciclo di vita</i></td>
      <td>Sviluppa e testa l’applicazione utilizzando gli ambienti ufficiali non di produzione per l’autenticazione di Adobe Pass:<ul><li>Prequal-Produzione</li><li>Release-Staging</li></ul><br/>Esegui un controllo qualità completo in questi ambienti prima di lanciare la versione di produzione.<br/><br/>Le applicazioni client non devono passare alla fase Release-Production senza aver prima completato la convalida end-to-end in ambienti non di produzione.</td>
      <td>Rischi di lancio con difetti critici e gravi.<br/><br/>La mancanza di un percorso di debug breve ed efficiente può impedire al supporto e al team tecnico di Adobe di intervenire rapidamente.</td>
   </tr>
</table>

## Procedure consigliate {#recommended-practices}

### 1. Fase di registrazione {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiche</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Convalida dei token di accesso</i></td>
      <td>Verifica in modo proattivo la validità del token di accesso e aggiornalo se scaduto.<br/><br/>Prima di ritentare la richiesta originale, verificare che il token di accesso venga aggiornato in base a eventuali tentativi per la gestione di errori HTTP 401 "Non autorizzati".</td>
      <td>Rischi che scatenano risposte di errore HTTP 401 "Non autorizzato", sovraccarico delle risorse di sistema e aumento della latenza.</td>
   </tr>
</table>

### 2. Fase di configurazione {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiche</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Memorizzazione in cache della configurazione</i></td>
      <td>Memorizza la risposta di configurazione in memoria o nell’archiviazione persistente per un breve periodo di tempo (ad esempio, 3-5 minuti) per migliorare le prestazioni e ridurre al minimo le chiamate REST API v2 non necessarie.</td>
      <td>Rischi di sovraccarico delle risorse di sistema e aumento della latenza.</td>
   </tr>
</table>

### 3. Fase di autenticazione {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiche</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Convalida del codice di autenticazione (seconda schermata)</i></td>
      <td>Convalida il codice di autenticazione inviato tramite l'input dell'utente nell'applicazione secondaria (seconda) (schermata) prima di chiamare l'API /api/v2/authenticate nelle seguenti condizioni:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd">Autenticazione eseguita nell'applicazione secondaria (schermata) con mvpd preselezionato</a></b><ul><li>Utilizza <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md">Riprendi sessione di autenticazione</a> - POST /api/v2/{serviceProvider}/session/{code}</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd">Autenticazione eseguita nell'applicazione secondaria (schermo) senza mvpd preselezionato</a></b><ul><li>Utilizza <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md">Recupera sessione di autenticazione</a> - GET /api/v2/{serviceProvider}/session/{code}</li></ul><br/>L’applicazione client riceverebbe un errore se il codice di autenticazione fornito fosse stato digitato in modo errato o nel caso in cui la sessione di autenticazione fosse scaduta.</td>
      <td>Rischia varie risposte di errore e problemi del flusso di lavoro durante l’autenticazione.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Supporto di più profili</i></td>
      <td>Assicurati che l’applicazione client possa gestire più profili chiedendo all’utente di selezionare un profilo o applicando una logica personalizzata, ad esempio selezionando automaticamente il profilo con il periodo di validità più lungo.</td>
      <td>Rischia il malfunzionamento dell'applicazione client a causa del supporto mancante sul lato dell'applicazione client.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>(Facoltativo) Supporto di flussi non di base</i></td>
      <td>Supporta i flussi non di base nel caso in cui l’attività dell’applicazione client richieda:<ul><li>Flussi di accesso degradati (funzionalità premium)</li><li>Flussi di accesso temporanei (funzionalità Premium)</li><li>Flussi di accesso Single Sign-On (funzionalità standard)</li></ul></td>
      <td>Rischi che creano un’esperienza utente non ideale.</td>
   </tr>
</table>

### 4. (Facoltativo) Fase di pre-autorizzazione {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiche</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Esperienza utente</i></td>
      <td>Visualizza un feedback chiaro degli utenti se viene negata una decisione di preautorizzazione, utilizzando i messaggi forniti da MVPD o Adobe tramite codici di errore avanzati.</td>
      <td>Rischi che creano un’esperienza utente non ideale.</td>
   </tr>
</table>

### 5. Fase di autorizzazione {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiche</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Convalida dei token multimediali</i></td>
      <td>Convalidare i token multimediali utilizzando la libreria <a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier">Media Token Verifier</a>.</td>
      <td>Mettono a rischio i sistemi di frode, come ad esempio lo strappo a flusso.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Esperienza utente</i></td>
      <td>Visualizza un feedback chiaro degli utenti se una decisione di autorizzazione viene negata, utilizzando i messaggi forniti da MVPD o Adobe tramite codici di errore avanzati.</td>
      <td>Rischi che creano un’esperienza utente non ideale.</td>
   </tr>
</table>

### 6. Fase di disconnessione {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiche</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Esperienza utente</i></td>
      <td>Evitare di chiamare automaticamente (a livello di programmazione) l'API di disconnessione in scenari quali la preautorizzazione o l'autorizzazione negata, in quanto l'API di disconnessione deve essere richiamata solo in risposta a una richiesta diretta dell'utente.</td>
      <td>Rischia di confondere l’utente sul fatto che l’autenticazione non riesca.</td>
   </tr>
</table>

### 7. Parametri e intestazioni {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiche</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Riutilizza codice</i></td>
      <td>Riutilizza il codice dell’API REST v1 per calcolare l’identificatore del dispositivo e le informazioni sul dispositivo con modifiche minori, ma assicurati di inviare solo i parametri e le intestazioni previsti per l’API REST v2.<br/><br/>Riutilizzare il codice da REST API v1 per richiamare l'API DCR per recuperare un token di accesso.</td>
      <td>-</td>
   </tr>
</table>

### 8. Prove {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiche</th>
      <th style="background-color: #EFF2F7;">Rischi</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Copertura del test</i></td>
      <td>Verifica che i seguenti flussi di base siano testati tra dispositivi e piattaforme:<br/><br/><b>Flussi di autenticazione</b><ul><li>Scenario di autenticazione applicazione primaria (schermata)</li><li>Scenario di autenticazione applicazione secondaria (schermata)</li></ul><br/><b>(Facoltativo) Flussi di preautorizzazione</b><ul><li>Scenario delle decisioni relative alle autorizzazioni di prova</li><li>Verifica scenario decisioni di rifiuto</li></ul><br/><b>Flussi di autorizzazione</b><ul><li>Scenario delle decisioni relative alle autorizzazioni di prova</li><li>Verifica scenario decisioni di rifiuto</li></ul><br/><b>Flussi di disconnessione</b><br/><br/>Verificare inoltre altri flussi di accesso, se applicabili:<br/><br/><ul><li>Flussi di accesso degradati (funzionalità premium)</li><li>Flussi di accesso temporanei (funzionalità Premium)</li><li>Flussi di accesso Single Sign-On (funzionalità standard)</li></ul><br/>Integrazioni MVPD di livello superiore (per i provider più utilizzati).</td>
      <td>Rischio di guasti imprevisti nella produzione, in particolare nelle piattaforme testate con minore frequenza o flussi rari come scenari negativi.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Strumenti di prova</i></td>
      <td>Utilizza il sito Web <a href="https://developer.adobe.com/adobe-pass/">Adobe Developer</a>.</td>
      <td>-</td>
   </tr>
</table>

## Riepilogo {#summary}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Fase</th>
      <th style="background-color: #EFF2F7;">Obbligatorio</th>
      <th style="background-color: #EFF2F7;">(fortemente) consigliato</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Registrazione</i></td>
      <td>Memorizza credenziali client nella cache<br/><br/>Token di accesso alla cache</td>
      <td>Convalidare e aggiornare i token di accesso</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Configurazione</i></td>
      <td>Riduci al minimo i recuperi delle risposte di configurazione</td>
      <td>Risposta alla configurazione della cache</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autenticazione</i></td>
      <td>Ottimizzazione del meccanismo di polling<br/><br/>Memorizza nella cache parti dei profili</td>
      <td>Supporta più profili<br/><br/>Supporta la funzionalità di degrado (se richiesta dall'azienda)<br/><br/>Supporta la funzionalità TempPass (se richiesta dall'azienda)<br/><br/>Supporta la funzionalità Single Sign-On (se richiesta dall'azienda)</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Preautorizzazione</i></td>
      <td>Decisioni di pre-autorizzazione per la cache<br/><br/>Ripeti ottimizzazione del meccanismo</td>
      <td>Migliorare l’esperienza utente utilizzando codici di errore per le decisioni di preautorizzazione negate</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autorizzazione</i></td>
      <td>Recupera la decisione di autorizzazione quando l'utente richiede la riproduzione<br/><br/>Ripeti ottimizzazione del meccanismo</td>
      <td>Migliora l'esperienza utente utilizzando codici di errore per le decisioni di autorizzazione negate<br/><br/>Convalida del token multimediale</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Disconnetti</i></td>
      <td>Implementare l'API di disconnessione per consentire agli utenti di disconnettersi manualmente</td>
      <td>Evita di chiamare automaticamente l'API di disconnessione</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Obbligatorio</th>
      <th style="background-color: #EFF2F7;">(fortemente) consigliato</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Parametri e intestazioni</i></td>
      <td>Segui le specifiche delle intestazioni obbligatorie</td>
      <td>Riutilizzare il codice dall’API REST v1</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Gestione degli errori</i></td>
      <td>Implementa gestione avanzata degli errori<br/><br/>Implementa gestione degli errori HTTP</td>
      <td>-</td>
   </tr>
</table>
