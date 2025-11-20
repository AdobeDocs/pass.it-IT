---
title: Procedure di inoltro per il monitoraggio della concorrenza
description: Procedure di inoltro per il monitoraggio della concorrenza
exl-id: eb110465-3a74-489e-a521-0e17f5aeecb8
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Procedure di inoltro per il monitoraggio della concorrenza {#esc-procedures}

>[!NOTE]
>
>Chiamare la hotline: +1-657-312-4623 e inviare un&#39;e-mail a `tve-support@adobe.com` specificando &quot;URGENT - INCIDENT&quot; nell&#39;oggetto.


## Introduzione {#cm-escalation-intro}

In questo documento vengono descritte le procedure di supporto per gli incidenti gravi (**GRAVITÀ 1** livello) che interessano l&#39;autenticazione di Adobe Pass, il monitoraggio della concorrenza di Adobe Pass e i relativi partner.

## Definizione del livello di gravità della riassegnazione 1 {#defn-escl-sevrityone-level}

Un evento imprevisto di livello **SEVERITY 1** è una situazione **LIVE**, **che si verifica nell&#39;ambiente di produzione**, che non consente il completamento dei flussi di autenticazione e/o autorizzazione per un canale e un MVPD e che interessa un numero elevato di abbonati al MVPD che eseguono il flusso.

## Esempi di incidenti con gravità 1 {#exampl-sevone-incident}

* L&#39;attivatore dell&#39;accesso alla produzione ospitato in <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> non è disponibile.

* Per un MVPD specifico, Adobe non reindirizza più / visualizza la pagina di accesso, dopo che l’utente seleziona il MVPD (in uno dei browser supportati).

* Il partner riceve un numero elevato di rapporti che gli utenti non possono autenticare/autorizzare con un MVPD specifico.

* Durante il processo di autenticazione, l’utente si blocca su una pagina di errore di Adobe senza la possibilità di riavviare il flusso di autenticazione/autorizzazione.


## Esempi di cosa è *NOT* un problema di gravità 1 {#exampl-not-sev1}

*Per problemi di questo tipo, Adobe fornirà supporto per le indagini, ma non si tratta di incidenti di gravità 1:*

* Uno o più abbonati non sono in grado di eseguire il flusso a causa di un problema di versione di Flash (Flash mancante, Flash bloccanti, versione Flash errata).
* Uno o più abbonati non sono in grado di eseguire l’autenticazione e rimangono nella pagina di accesso di MVPD.
* Uno o più abbonati sono autenticati ma non sono in grado di riprodurre video.
* Uno/pochi/tutti gli abbonati incontrano un errore JavaScript sul sito Programmatore.

## Flussi di escalation gravità 1 {#sevone-escalation-flows}

Gli incidenti di gravità 1 possono essere avviati da Adobe o da un partner di autenticazione Adobe Pass. Di seguito sono riportati i passaggi per ciascuno di essi.

### Flusso avviato dal partner {#partner-initiated-flow}

1. Il partner identifica un caso di gravità 1 (come descritto sopra) che richiede l’attenzione immediata di Adobe.

1. Il partner invia un&#39;e-mail a tve-support@adobe.com includendo &quot;URGENT - INCIDENT&quot; (URGENTE - INCIDENTE) nella riga dell&#39;oggetto e aggiungendo le seguenti informazioni:

   * Titolo
   * Descrizione e passaggi da riprodurre
   * SO
   * Browser
   * Versione Flash
   * (facoltativo) Eventuali schermate o acquisizioni video che dimostrino il problema

1. Se Adobe non risponde al ticket entro 30 minuti, il partner chiama il numero seguente:

   * **1-205-693-9813**


**Se non includi &quot;URGENT-INCIDENT&quot; nel titolo del ticket, non verrà ritirato dal nostro sistema di notifica.**

### Flusso avviato da Adobe {#adobe-initiated-flow}

**...per un problema di autenticazione Adobe Pass**

1. Adobe identifica un problema interno e apre un ticket con il nostro sistema di tracciamento.

1. Adobe invia una notifica al responsabile del programma e al contatto tecnico del partner, specificando il numero del ticket e l’impatto stimato del problema.

1. Adobe si impegna a risolvere l’incidente e tiene informati tutti i partner interessati.


**...per un problema del partner (Programmatore/MVPD)**

1. Adobe identifica un problema relativo all’integrazione con un MVPD o in uno dei siti del programmatore.

1. Adobe invia una notifica al partner interessato **seguendo le procedure di supporto in vigore con tale partner** e apre un ticket con l&#39;organizzazione di supporto del partner.

1. Se, durante l’analisi di impatto, Adobe identifica che il problema appartiene a una delle decisioni concordate in precedenza sugli scenari di incidente (vedi la sezione &quot;Decisioni concordate in anticipo sugli scenari di incidente&quot; di seguito), agirà di conseguenza senza attendere il partner1. dell&#39;input.

1. Adobe attenderà gli aggiornamenti dal partner e una notifica dal partner quando il servizio sarà stato ripristinato.

### Decisioni concordate in anticipo sugli scenari di incidente {#pre-agreed-decisions}

Esistono alcune situazioni in cui verrà eseguita un’azione predefinita nel caso dell’occorrenza di quello scenario:

|    | Scenario | Descrizione | Azioni |
|:--:|:------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | Adobe identifica un problema di integrazione di un MVPD durante le normali operazioni di produzione. | Durante le normali operazioni di produzione, Adobe identifica un problema con uno degli MVPD che rende impossibile l’esecuzione dei flussi di autenticazione/autorizzazione (ad esempio certificati scaduti, risposte SAML scadute, porte chiuse, parametri modificati, ecc.) | Adobe invierà una notifica a MVPD e Programmer interessati. Adobe disattiverà questo MVPD per tutti i programmatori interessati. Adobe aprirà un ticket con MVPD seguendo la procedura di supporto concordata con tale MVPD |
| S2 | Adobe attiva un nuovo MVPD per un programmatore e il programmatore inserisce nella whitelist il MVPD prima della data di lancio. | Adobe sta attivando un nuovo MVPD per il sito di un programmatore e il sito sta già visualizzando il nuovo MVPD nel selettore, anche se non era previsto. | Adobe informerà il programmatore del nuovo MVPD visualizzato nel selettore prima della data pianificata. Il programmatore eseguirà un&#39;azione per rimuoverlo dal selettore, se necessario. |
| S3 | Adobe attiva un nuovo MVPD per un programmatore anche se il MVPD non è pronto per la produzione | Adobe sta attivando un nuovo MVPD per un programmatore, ma MVPD non ha ancora implementato il supporto per l’integrazione, pertanto non è possibile eseguire i flussi di autenticazione/autorizzazione | Adobe eseguirà l’implementazione solo se richiesto dal programmatore. Il programmatore sarà responsabile di garantire l’inserimento nella whitelist di MVPD una volta eseguiti tutti i test. |

### Aspettative Di Risposta Per Gli Incidenti Di Gravità 1 {#response-expectations}

* Risposta iniziale: 30 minuti (24/7)
* Piano d&#39;azione: 1 ora (24/7)
* Risoluzione: ASAP (24/7)
