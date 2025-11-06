---
title: Glossario
description: Glossario dei termini nel monitoraggio della concorrenza
exl-id: 3b3b36fe-9f04-4de9-bd84-9f8d766bbc71
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Glossario {#glossary}

## ID account {#accid-defn}

* Account MVPD di un abbonato, in genere corrispondente all’account di fatturazione effettivo. Questo account deve essere identificabile da MVPD nel proprio sistema.

## Azione {#action-defn}

* Tipo di accesso richiesto dall&#39;oggetto. I valori possibili per CM sono ***begin*** o ***continue*** una sessione di streaming.

## Flusso attivo {#active-stream-defn}

* Un flusso che ha ricevuto almeno 1 evento (heartbeat) negli ultimi 90 secondi.

* ***Nota:*** se l&#39;ultimo evento nel flusso è di tipo stop (`?event=stop`), non verrà conteggiato. Si tratta di un’ottimizzazione che consente a un lettore di chiudere esplicitamente un flusso in modo che non venga più considerato &quot;attivo&quot;.

## Applicazione {#application-defn}

* Sviluppato dal tenant per l’accesso ai contenuti video
* Prende e applica decisioni sull&#39;accesso ai contenuti in base alle informazioni fornite dal servizio di monitoraggio della concorrenza (valido nel caso [Punto informazioni criteri](/help/concurrency-monitoring/policy-info-pt-versionone.md))
* Avrà un **ID applicazione** univoco fornito da Adobe.

## Servizio di monitoraggio della concorrenza {#cm-service-defn}

* Funge da sistema di monitoraggio per gli abbonati, sostenendo gli MVPD e i programmatori nei loro requisiti di applicazione incrociata dei criteri.
* Riceve heartbeat che indicano l’attività del flusso.
* Agisce come _punto decisionale dei criteri_ valutando le richieste di autorizzazione in base all&#39;attività dell&#39;utente e fornendo una risposta di tipo consenti/nega.
* Agisce come _Punto informazioni criteri_ segnalando il numero di flussi attivi (e metadati di flusso aggiuntivi) per un sottoscrittore.

## Ambiente {#env-defn}

* Informazioni aggiuntive rilevanti per la richiesta, come impostazioni di configurazione o ora di sistema.

## MVPD {#mvpd-defn}

* Distributore di programmi video multicanale
* Agisce come provider di autenticazione e autorizzazione, ma può anche essere provider di servizi o di contenuti.

## Policy {#policy-defn}

* Il concetto di controllo degli accessi di base in CM definito come destinazione e una o più regole raggruppate sotto un nome univoco.

## Punto di amministrazione criteri (PAP) {#policy-admin-pt-defn}

* Punto che gestisce i criteri di autorizzazione di accesso. Questo non sarà documentato qui, ma alla fine forniremo una console self-service per i clienti per gestire i loro criteri di accesso.

## Punto decisionale criterio (PDP) {#policy-decn-pt-defn}

* Punto che valuta le richieste di accesso in base ai criteri di autorizzazione prima di emettere decisioni di accesso.

## Punto di applicazione dei criteri (PEP) {#policy-ef-pt-defn}

* Punto che intercetta la richiesta di accesso dell’utente a una risorsa, invia una richiesta di decisione al PDP e applica tale decisione alla richiesta. Questa è l&#39;applicazione client e non è previsto il trasferimento di questo ruolo al controllo della concorrenza.

## Punto informazioni criterio (PIP) {#policy-info-pt-defn}

* Origine dei valori di attributo. Il monitoraggio della concorrenza funge da punto di informazione fornendo:
   * metadati del flusso pass-through.
   * metriche delle attività relative ai flussi simultanei.

## Programmatore {#programmer-defn}

* Funge da provider di servizi e contenuti.
* Si basa sull&#39;applicazione client distribuita che si integra con il servizio di monitoraggio della concorrenza per applicare i criteri di sicurezza definiti in base ai dati del servizio sopra menzionati.
* Deve supportare MVPD nella raccolta delle attività degli abbonati e nell’applicazione delle regole di limitazione quando presente sulle loro proprietà.
* Potrebbe anche essere interessato a limitare l’accesso simultaneo ai propri contenuti su tutti i portali di destinazione, come regola separata.

  *Q: perché il programmatore e non l&#39;ID richiedente come nel resto dell&#39;autenticazione Adobe Pass?*

  *A: il motivo è consentire ai programmatori di utilizzare questo parametro in modo flessibile per passare o isolare i dati tra le proprietà a seconda dei casi d&#39;uso.*

## Risorsa {#resource-defn}

* Contenuto effettivo che un soggetto desidera utilizzare. La risorsa di solito include attributi relativi al proprietario (editore) e può anche fornire informazioni aggiuntive come genere o altro.

## Regola {#rule-defn}

* Funzione booleana valutata rispetto a un particolare flusso e all’attività utente rilevante per determinare se l’accesso a tale flusso deve essere consentito o negato.

## Sessione di streaming {#streaming-session-defn}

* Sessione video in streaming avviata da un soggetto per l’utilizzo di una risorsa specifica.

## Oggetto {#subj-defn}

* Il consumatore del contenuto (video) su Internet. Stiamo deliberatamente evitando il termine _**utente**_, in quanto il monitoraggio della concorrenza di solito tratta gli ID account di MVPD (che coinvolgono diversi utenti effettivi che condividono lo stesso contratto, ad esempio i membri della famiglia per una famiglia).

* Per ogni flusso, l’oggetto può essere migliorato con gli attributi relativi alla persona che utilizza effettivamente il servizio, al dispositivo connesso alla rete e così via.

## Abbonato {#subscriber-defn}

* Cliente pagante di un MVPD o persona che condivide le credenziali di un cliente pagante
* Il servizio di monitoraggio della concorrenza può interrompere la visualizzazione dei contenuti da parte dell’applicazione client che utilizza il servizio sopra indicato.
* Nel migliore dei casi, non nota mai l’esistenza del servizio di monitoraggio della concorrenza

## Target {#target-defn}

* Predicato di flusso che restituirà se la regola è applicabile a un dato flusso. Il target implicito in CM sarà qualsiasi flusso creato da un&#39;applicazione che fa riferimento al criterio in questione. Inoltre, è possibile aggiungere condizioni per il valore dell’attributo al fine di perfezionare il filtro dell’attività prima di applicare le regole.

## Tenant {#tenant-defn}

* Un cliente che esegue il monitoraggio della concorrenza nell&#39;organizzazione.
