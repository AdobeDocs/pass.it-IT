---
title: Panoramica di REST API V2
description: Panoramica di REST API V2
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: a02ba4ca1b6579781e40ecd0d12dbfdd23ea7398
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# Panoramica di REST API V2 {#rest-api-v2-overview}

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

Si desidera migliorare l&#39;efficienza dei costi delle applicazioni TVE?

Si desidera ridurre i tempi di sviluppo e le risorse necessarie per supportare applicazioni TVE su più piattaforme?

Desideri garantire un’esperienza utente coerente su tutte le piattaforme?

Vuoi ridurre l’impegno di manutenzione e semplificare la fornitura di aggiornamenti, correzioni di bug o miglioramenti?

## Introduzione all’API REST V2 {#rest-api-v2-introduction}

Adobe Pass Authentication è entusiasta di annunciare il lancio dell’API REST V2, progettata per migliorare l’esperienza utente e semplificare l’integrazione con i servizi Pass.

Siamo entusiasti delle possibilità della nostra nuova API REST, che rappresenta un importante passo avanti per la nostra piattaforma e apre le porte a nuove funzioni e flussi di applicazioni.

## Novità? {#rest-api-v2-whats-new}

Implementazione univoca su tutte le piattaforme

Le applicazioni dei clienti possono ora utilizzare la stessa implementazione su più piattaforme, semplificando l’avvio di nuove funzioni o la gestione di app live.

### SSO tra dispositivi {#rest-api-v2-cross-device-sso}

REST API V2 consente di passare in modo sicuro una sessione di autenticazione tra dispositivi diversi. Passando semplicemente la sessione tra i dispositivi, gli utenti possono autenticarsi sul proprio dispositivo mobile e riprodurre video in streaming su un dispositivo connesso alla TV senza ripetere l’autenticazione.

### Più sessioni di autenticazione attive {#rest-api-v2-multiple-active-authentication-sessions}

Sono ora possibili diverse sessioni attive di MVPD e i clienti possono scegliere di passare da TempPass alla normale integrazione MVPD quando necessario.

### Meccanismo di sicurezza avanzato {#rest-api-v2-enhanced-security-mechanism}

Dynamic Client Registration è il meccanismo di sicurezza utilizzato per tutti i flussi e le funzionalità. Consente un controllo più sicuro e granulare delle applicazioni dei clienti e può registrare le applicazioni su tutte le piattaforme.

### Prestazioni migliorate per tempi di risposta più rapidi {#rest-api-v2-improved-performance}

I meccanismi di caching migliorati consentono di ridurre il traffico verso gli MVPD, migliorando i tempi di risposta e riducendo la latenza. Nel complesso, il numero di chiamate API viene ridotto fino all’avvio del video.

### Codici di errore migliorati su tutti i flussi {#rest-api-v2-enhanced-error-codes}

I codici di errore avanzati sono ora disponibili su tutti i flussi di accesso nello stesso formato, con informazioni aggiuntive che guidano le applicazioni per migliorare l’esperienza utente complessiva.

### Controllo migliorato su tutte le sessioni di autenticazione {#rest-api-v2-improved-control}

La nuova API REST V2 consente azioni su più sessioni autenticate contemporaneamente.

### Riduzione dei costi di manutenzione {#rest-api-v2-reduce-maintenance-costs}

Tutte le risposte e le informazioni sugli errori sono ora normalizzate.

## Quali sono le prossime novità? {#rest-api-v2-whats-next}

Tutti i clienti che attualmente utilizzano le nostre API tramite SDK o chiamate REST possono continuare a farlo, poiché prevediamo di continuare a fornire supporto fino alla fine del 2025.

Tuttavia, tutti gli sviluppi futuri saranno basati sull’API REST V2. Consigliamo vivamente di avviare il processo di migrazione per sfruttare le funzionalità Adobe Pass più recenti.

## Vuoi saperne di più? {#rest-api-v2-want-to-learn-more}

Per iniziare, consulta la nostra documentazione pubblica:

- [Webinar](#rest-api-v2-webinar)
- [Glossario](rest-api-v2-glossary.md)
- [Elenco di controllo](rest-api-v2-checklist.md)
- [Domande frequenti](rest-api-v2-faqs.md)
- [API](apis/rest-api-v2-apis-overview.md)
- [Flussi](flows/rest-api-v2-flows-overview.md)
- Cookbook
- Appendice
- [Requisiti minimi di sistema](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

Il nostro team dedicato di supporto è disponibile anche per aiutarti con qualsiasi domanda o assistenza tecnica di cui potresti aver bisogno.

### Webinar {#rest-api-v2-webinar}

Guarda il webinar sulla nuova API REST V2, dove abbiamo fornito una panoramica delle nuove funzioni e funzionalità, nonché una demo live dell’API REST V2 in azione.

>[!VIDEO](https://video.tv.adobe.com/v/3457461/?quality=12&learn=on)

## Vuoi provare l’API REST V2? {#rest-api-v2-want-to-try}

Ora puoi esplorare l&#39;API REST V2 tramite la nostra pagina dedicata al prodotto dal sito Web [Adobe Developer](https://developer.adobe.com/adobe-pass/).
