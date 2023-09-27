---
title: Procedure di inoltro per il monitoraggio della concorrenza
description: Procedure di inoltro per il monitoraggio della concorrenza
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '823'
ht-degree: 0%

---


# Procedure di inoltro per il monitoraggio della concorrenza {#esc-procedures}

>[!NOTE]
>
>Chiama la hotline: +1-205-693-9813 e invia un&#39;e-mail a `tve-support@adobe.com` l&#39;inclusione di &quot;URGENT - INCIDENT&quot; (INCIDENTE URGENTE) nell&#39;oggetto.


## Introduzione {#cm-escalation-intro}

Questo documento descrive le procedure di supporto per gli incidenti gravi (**GRAVITÀ 1** Adobe Pass, Adobe Pass Concurrency Monitoring e i relativi partner.

## Definizione del livello di gravità della riassegnazione 1 {#defn-escl-sevrityone-level}

A **GRAVITÀ 1** livello incidente è un **LIVE** situazione, **nell&#39;ambiente di produzione**, che non consente il completamento dei flussi di autenticazione e/o autorizzazione per un canale e un MVPD, interessando un gran numero di abbonati dell&#39;MVPD che esegue il flusso.

## Esempi di incidenti con gravità 1 {#exampl-sevone-incident}

* L’Access Enabler di produzione ospitato in <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> non è disponibile.

* Per un MVPD specifico, Adobe non reindirizza più / visualizza la pagina di accesso, dopo che l’utente seleziona l’MVPD (in nessuno dei browser supportati).

* Il partner riceve un numero elevato di rapporti che gli utenti non possono autenticare/autorizzare con un MVPD specifico.

* Durante il processo di autenticazione, l’utente si blocca su una pagina di errore di Adobe senza la possibilità di riavviare il flusso di autenticazione/autorizzazione.


## Esempi di *NOT* un caso di gravità 1 {#exampl-not-sev1}

*Per problemi di questo tipo, Adobe fornirà supporto per le indagini, ma non si tratta di incidenti di gravità 1:*

* Uno o più abbonati non sono in grado di eseguire il flusso a causa di un problema di versione del Flash (Flash mancante, blocchi del Flash, versione del Flash errata).
* Uno o più abbonati non sono in grado di eseguire l&#39;autenticazione e rimangono nella pagina di accesso MVPD.
* Uno o più abbonati sono autenticati ma non sono in grado di riprodurre video.
* Uno o pochi abbonati/tutti riscontrano un errore JavaScript sul sito Programmer.

## Flussi di escalation gravità 1 {#sevone-escalation-flows}

Gli incidenti di gravità 1 possono essere avviati da Adobe o da un partner di autenticazione Adobe Pass. Di seguito sono riportati i passaggi per ciascuno di essi.

### Flusso avviato dal partner {#partner-initiated-flow}

1. Il partner identifica un incidente di Gravità 1 (come descritto sopra) che richiede l’attenzione immediata di Adobe.

1. Il partner invia un&#39;e-mail a tve-support@adobe.com includendo &quot;URGENT - INCIDENT&quot; (URGENTE - INCIDENTE) nella riga dell&#39;oggetto e aggiungendo le seguenti informazioni:

   * Titolo
   * Descrizione e passaggi da riprodurre
   * SO
   * Browser
   * Versione Flash
   * (facoltativo) Eventuali schermate o acquisizioni video che dimostrino il problema

1. Se l’Adobe non risponde al ticket entro 30 minuti, il partner chiama il numero seguente:

   * **1-205-693-9813**


**Se non includi &quot;URGENT-INCIDENT&quot; nel titolo del ticket, non verrà ritirato dal nostro sistema di notifica.**

### Flusso avviato da Adobe {#adobe-initiated-flow}

**...per un problema di autenticazione Adobe Pass**

1. L’Adobe identifica un problema interno e apre un ticket con il nostro sistema di tracciamento.

1. L&#39;Adobe avvisa il responsabile del programma e il contatto tecnico del partner, specificando il numero del ticket e l&#39;impatto stimato del problema.

1. Adobe si impegna a risolvere l&#39;incidente e tiene informati tutti i partner interessati.


**...per un problema del partner (Programmatore/MVPD)**

1. L&#39;Adobe identifica un problema relativo all&#39;integrazione con un MVPD o su uno dei siti del programmatore.

1. Adobe di notifica al partner interessato **seguire le procedure di assistenza in atto con tale partner** e apre un ticket con l’organizzazione di supporto del partner.

1. Se, durante l’analisi dell’impatto, l’Adobe rileva che la questione appartiene a una delle decisioni concordate in precedenza sugli scenari di incidente (vedi la sezione &quot;Decisioni concordate in precedenza sugli scenari di incidente&quot; di seguito), agirà di conseguenza senza attendere il partner1. dell&#39;input.

1. Adobe attende gli aggiornamenti dal partner e una notifica dal partner quando il servizio è stato ripristinato.

### Decisioni concordate in anticipo sugli scenari di incidente {#pre-agreed-decisions}

Esistono alcune situazioni in cui verrà eseguita un’azione predefinita nel caso dell’occorrenza di quello scenario:

|    | Scenario | Descrizione | Azioni |
|:---:|:---|:---|:---|
| S1 | L&#39;Adobe identifica un problema di integrazione di un MVPD durante le normali operazioni di produzione. | Durante le normali operazioni di produzione, Adobe identifica un problema con uno degli MVPD che rende impossibile l’esecuzione dei flussi di autenticazione/autorizzazione (ad esempio certificati scaduti, risposte SAML scadute, porte chiuse, parametri modificati, ecc.) | Adobe invierà una notifica all’MVPD e ai programmatori interessati. Adobe disattiverà questo MVPD per tutti i programmatori interessati. L’Adobe aprirà un ticket con l’MVPD seguendo la procedura di assistenza concordata con tale MVPD |
| S2 | Adobe attiva un nuovo MVPD per un programmatore e il programmatore inserisce il MVPD nella whitelist prima della data di lancio. | Adobe sta attivando un nuovo MVPD per un sito di Programmatori, e il sito sta già visualizzando il nuovo MVPD nel selettore, anche se non era previsto. | L’Adobe invierà una notifica al programmatore relativamente al nuovo MVPD visualizzato nel selettore prima della data pianificata. Il programmatore eseguirà un&#39;azione per rimuoverlo dal selettore, se necessario. |
| S3 | L&#39;Adobe attiva un nuovo MVPD per un programmatore anche se il MVPD non è pronto per la produzione | L’Adobe sta attivando un nuovo MVPD per un programmatore, ma MVPD non ha ancora implementato il supporto per l’integrazione, pertanto non è possibile eseguire i flussi di autenticazione/autorizzazione | Adobe farà la distribuzione solo se richiesto dal programmatore Il programmatore sarà responsabile di garantire la whitelist del MVPD una volta che tutti i test sono stati eseguiti. |

### Aspettative Di Risposta Per Gli Incidenti Di Gravità 1 {#response-expectations}

* Risposta iniziale: 30 minuti (24/7)
* Piano d&#39;azione: 1 ora (24/7)
* Risoluzione: ASAP (24/7)
