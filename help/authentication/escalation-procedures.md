---
title: Procedure di riassegnazione
description: Procedure di riassegnazione
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---

# Procedure di riassegnazione {#escalation-procedures}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
> 
>Chiamare la hotline: **+1-205-693-9813** e inviare un&#39;e-mail a **tve-support@adobe.com** includendo **URGENT - INCIDENT** nell&#39;oggetto.

## Introduzione {#introduction}

Questo documento descrive le procedure di supporto per gli incidenti gravi (**GRAVITÀ 1** livello) che interessano l&#39;autenticazione di Adobe Pass, il monitoraggio della concorrenza di Adobe Pass e i relativi partner.


## Definizione di incidente di livello SEVERITY 1 {#definition-of-a-severity-1-level-incident}

Un evento imprevisto di livello **SEVERITY 1** è una situazione **LIVE**, **che si verifica nell&#39;ambiente di produzione**, che non consente il completamento dei flussi di autenticazione e/o autorizzazione per un canale e un MVPD, e interessa un numero elevato di sottoscrittori dell&#39;MVPD che eseguono il flusso.


## Esempi di incidenti con GRAVITÀ 1 {#examples-of-severity-1-incidentcs}

* L&#39;attivatore dell&#39;accesso alla produzione ospitato in `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js` (o `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`) non è disponibile.

* Per un MVPD specifico, Adobe non reindirizza più / visualizza la pagina di accesso, dopo che l’utente seleziona l’MVPD (in nessuno dei browser supportati).

* Il partner riceve un numero elevato di rapporti che gli utenti non possono autenticare/autorizzare con un MVPD specifico.

* Durante il processo di autenticazione, l’utente si blocca su una pagina di errore di Adobe senza la possibilità di riavviare il flusso di autenticazione/autorizzazione.


| Esempi di cosa è **NOT** un problema di gravità 1 |
|---|
| Per problemi di questo tipo, Adobe fornirà supporto per le indagini, ma non si tratta di incidenti di gravità 1:<ul><li>Uno o più abbonati non sono in grado di eseguire il flusso a causa di un problema di versione del Flash (Flash mancante, blocchi del Flash, versione del Flash errata).</li><li>Uno o più abbonati non sono in grado di eseguire l&#39;autenticazione e rimangono nella pagina di accesso MVPD.</li><li>Uno o più abbonati sono autenticati ma non sono in grado di riprodurre video.</li><li>Uno/pochi/tutti gli abbonati incontrano un errore JavaScript sul sito Programmatore</li></ul> |

## Flussi di escalation gravità 1 {#severity-1-escalation-flows}

Gli incidenti di gravità 1 possono essere avviati da Adobe o da un partner di autenticazione Adobe Pass. Di seguito sono riportati i passaggi per ciascuno di essi.

### Flusso avviato dal partner {#partner-initiated-flow}

1. Il partner identifica un incidente di Gravità 1 (come descritto sopra) che richiede l’attenzione immediata di Adobe.
1. Il partner invia un&#39;e-mail a **tve-support@adobe.com** includendo **URGENT - INCIDENT** nella riga dell&#39;oggetto e aggiungendo le seguenti informazioni:
   * Titolo
   * Descrizione e passaggi da riprodurre
   * Sistema operativo/browser
   * SDK e versione
   * Dispositivi interessati
   * % utenti interessati
   * Registri HTTP Trace o Device che illustrano il problema
   * (facoltativo) Eventuali schermate o acquisizioni video che dimostrino il problema
1. Se l’Adobe non risponde al ticket entro 30 minuti, il partner chiama il seguente numero:
   **1-205-693-9813**
   >[!IMPORTANT]
   >Se non includi &quot;URGENT-INCIDENT&quot; nel titolo del ticket, non verrà ritirato dal nostro sistema di notifica**.

### Flusso avviato da Adobe {#adobe-initiated-flow}

#### ...per un problema di autenticazione Adobe Pass {#adobe-initiated-flow-authn-issue}

1. L’Adobe identifica un problema interno e apre un ticket con il nostro sistema di tracciamento.

1. L&#39;Adobe avvisa il responsabile del programma e il contatto tecnico del partner, specificando il numero del ticket e l&#39;impatto stimato del problema.

1. Adobe si impegna a risolvere l&#39;incidente e tiene informati tutti i partner interessati.

#### ...per un problema del partner (Programmatore/MVPD) {#adobe-initiated-flow-partner-issue}

1. L&#39;Adobe identifica un problema relativo all&#39;integrazione con un MVPD o su uno dei siti del programmatore.

1. Adobe invia una notifica al partner interessato <u> seguendo le procedure di supporto in vigore con tale partner</u> e apre un ticket con l&#39;organizzazione di supporto del partner.

1. Se, durante l&#39;analisi dell&#39;impatto, l&#39;Adobe identifica che il problema appartiene a una delle decisioni concordate in precedenza sugli scenari di incidente, vedere **Decisioni concordate in precedenza sugli scenari di incidente**, l&#39;azione verrà eseguita di conseguenza senza attendere l&#39;input del partner.

1. Adobe attende gli aggiornamenti dal partner e una notifica dal partner quando il servizio è stato ripristinato.

## Decisioni concordate in anticipo sugli scenari di incidente {#pre-agreed-descn}

Esistono alcune situazioni in cui verrà eseguita un’azione predefinita nel caso dell’occorrenza di quello scenario:

|   | Scenario | Descrizione | Azioni |
|---|---|---|---|
| S1 | L&#39;Adobe identifica un problema di integrazione di un MVPD durante le normali operazioni di produzione. | Durante le normali operazioni di produzione, Adobe identifica un problema con uno degli MVPD che rende impossibile l’esecuzione dei flussi di autenticazione/autorizzazione (ad esempio certificati scaduti, risposte SAML scadute, porte chiuse, parametri modificati, ecc.) | - Adobe notificherà l&#39;MVPD e i programmatori interessati.  </br> </br> - L&#39;Adobe disattiverà questo MVPD per tutti i programmatori interessati. </br> </br> - L&#39;Adobe aprirà un ticket con MVPD seguendo la procedura di supporto concordata con tale MVPD |
| S2 | Adobe attiva un nuovo MVPD per un programmatore e il programmatore consente al MVPD prima della data di lancio. | Adobe sta attivando un nuovo MVPD per un sito di Programmatori, e il sito sta già visualizzando il nuovo MVPD nel selettore, anche se non era previsto. | - L&#39;Adobe comunicherà al programmatore il nuovo MVPD che appare nel selettore prima della data pianificata. </br> </br> - Il programmatore eseguirà un&#39;azione per rimuoverlo dal selettore, se necessario. |
| S3 | L&#39;Adobe attiva un nuovo MVPD per un programmatore anche se il MVPD non è pronto per la produzione | L’Adobe sta attivando un nuovo MVPD per un programmatore, ma MVPD non ha ancora implementato il supporto per l’integrazione, pertanto non è possibile eseguire i flussi di autenticazione/autorizzazione | - L&#39;Adobe eseguirà la distribuzione solo se richiesto dal programmatore </br> </br> - Il programmatore sarà responsabile di garantire l&#39;autorizzazione dell&#39;MVPD una volta eseguiti tutti i test. |

## Aspettative Di Risposta Per Gli Incidenti Di Gravità 1 {#response-expectations-for-severity-one-incidents}

* Risposta iniziale: 30 minuti (24/7)
* Piano d&#39;azione: 1 ora (24/7)
* Risoluzione: ASAP (24/7)
