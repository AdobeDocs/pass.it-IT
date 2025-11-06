---
title: Domande frequenti sulle procedure di supporto
description: Domande frequenti sulle procedure di supporto
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 0ab1fc212752dd4a4d6e12a4ab1287ef74e4a282
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# Domande frequenti sulle procedure di supporto {#support-procedures-faqs}

>[!IMPORTANT]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Questo documento illustra le domande frequenti (FAQ) sulle procedure di supporto per incidenti gravi (livello SEVERITY 1) che interessano l’autenticazione di Adobe Pass e dei suoi partner.

## Domande frequenti {#faqs}

### Che cos&#39;è un incidente di livello SEVERITY 1? {#support-procedures-faqs-1}

Un incidente di livello SEVERITY 1 è una situazione live nell’ambiente di produzione che impedisce il completamento dei flussi di autenticazione o autorizzazione per un canale e un MVPD, con un impatto su un numero elevato di abbonati.

Esempi di incidenti con GRAVITÀ 1

* Durante il processo di autenticazione, l’utente non viene reindirizzato alla pagina di accesso dopo aver selezionato il MVPD in qualsiasi browser supportato.

* Durante il processo di autenticazione, l’utente si blocca su una pagina di errore di Adobe senza la possibilità di riavviare il flusso di autenticazione.

* Il partner riceve numerosi rapporti che gli utenti non possono autenticare o autorizzare con un MVPD specifico.

* L’Access Enabler di produzione ospitato in https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js non è disponibile.

### Che cos&#39;è un incidente di livello non GRAVITY 1?

Adobe supporterà le indagini per questi problemi, ma non sono considerati incidenti di livello SEVERITY 1:

* Uno o più abbonati non possono autenticarsi e rimanere nella pagina di accesso di MVPD.

* Uno o più abbonati sono autenticati ma non possono riprodurre video.

* Alcuni o tutti gli abbonati rilevano un errore JavaScript sul sito Programmatore.

### Come vengono gestiti gli incidenti di livello SEVERITY 1?

Un caso di livello SEVERITY 1 può essere avviato da Adobe o da un partner di autenticazione Adobe Pass. Di seguito sono descritti i passaggi per ciascuno di essi.

**Flusso avviato dal partner**

1. Il partner identifica un incidente di livello 1 che richiede l&#39;attenzione immediata di Adobe.

1. Il partner invia un&#39;e-mail a **tve-support@adobe.com** includendo **URGENT - INCIDENT** nella riga dell&#39;oggetto e aggiungendo le seguenti informazioni:
   * Titolo
   * Descrizione e passaggi da riprodurre
   * Sistema operativo/browser
   * SDK e versione
   * Dispositivi interessati
   * % utenti interessati
   * Registri HTTP Trace o Device che illustrano il problema
   * (facoltativo) Eventuali schermate o acquisizioni video che dimostrino il problema

1. Se Adobe non risponde al ticket entro un periodo di tempo, il partner può chiamare il seguente numero: **1-657-312-4623**.

>[!IMPORTANT]
>
> Se non includi &quot;URGENT-INCIDENT&quot; nel titolo del ticket, non verrà preso dal nostro sistema di notifica.

**Flusso avviato da Adobe**

Per un problema di autenticazione Adobe Pass:

1. Adobe identifica un problema interno e apre un ticket nel nostro sistema di tracciamento.

1. Adobe invia una notifica al responsabile del programma e al contatto tecnico del partner, specificando il numero del ticket e l’impatto stimato del problema.

1. Adobe si impegna a risolvere il problema e tiene informati tutti i partner interessati.

Per un problema del partner (Programmatore/MVPD):

1. Adobe identifica un problema relativo all’integrazione con un MVPD o in uno dei siti del programmatore.

1. Adobe avvisa il partner interessato seguendo le procedure di supporto in atto con tale partner e apre un ticket presso l’organizzazione di supporto del partner.

1. Se, durante l’analisi di impatto, Adobe rileva che il problema rientra in una delle decisioni concordate in precedenza sugli scenari di incidente, agirà di conseguenza senza attendere il contributo del partner.

1. Adobe attenderà gli aggiornamenti dal partner e una notifica quando il servizio sarà stato ripristinato.

### Quali sono le decisioni prestabilite sugli scenari di incidente?

Alcune situazioni con azioni predefinite che verranno eseguite se si verifica lo scenario:

|    | Scenario | Descrizione | Azioni |
|----|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | Adobe identifica un problema di integrazione di un MVPD durante le normali operazioni di produzione. | Durante le normali operazioni di produzione, Adobe identifica un problema con uno degli MVPD che rende impossibile l’esecuzione dei flussi di autenticazione/autorizzazione (ad esempio certificati scaduti, risposte SAML scadute, porte chiuse, parametri modificati, ecc.) | Adobe invierà una notifica a MVPD e Programmer interessati.  </br></br> Adobe disattiverà questo MVPD per tutti i programmatori interessati. </br></br> Adobe aprirà un ticket con MVPD seguendo la procedura di supporto concordata con tale MVPD |
| S2 | Adobe attiva un nuovo MVPD per un programmatore e il programmatore consente il MVPD prima della data di avvio. | Adobe sta attivando un nuovo MVPD per il sito di un programmatore e il sito sta già visualizzando il nuovo MVPD nel selettore, anche se non era previsto. | Adobe informerà il programmatore del nuovo MVPD visualizzato nel selettore prima della data pianificata. Il programmatore </br></br> eseguirà un&#39;azione per rimuoverlo dal selettore, se necessario. |
| S3 | Adobe attiva un nuovo MVPD per un programmatore anche se il MVPD non è pronto per la produzione | Adobe sta attivando un nuovo MVPD per un programmatore, ma MVPD non ha ancora implementato il supporto per l’integrazione, pertanto non è possibile eseguire i flussi di autenticazione/autorizzazione | Adobe eseguirà la distribuzione solo se richiesto dal programmatore </br></br> Il programmatore sarà responsabile di garantire l&#39;autorizzazione di MVPD una volta eseguiti tutti i test. |
