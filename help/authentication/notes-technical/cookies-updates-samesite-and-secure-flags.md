---
title: Aggiornamenti dei cookie - Flag SameSite e Secure
description: Aggiornamenti dei cookie - Flag SameSite e Secure
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# Aggiornamenti dei cookie - Flag SameSite e Secure {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

</br>


## Aggiornamenti {#Updates}

Questa sezione evidenzia le modifiche introdotte dal browser Chrome e dall’autenticazione Adobe Pass per la gestione dei cookie di terze parti.



### Aggiornamenti di Chrome 80 {#Chrome}

A partire da Chrome versione 80 (tranne la versione 82), i cookie che non specificano un attributo *SameSite* verranno trattati come se fossero *SameSite=Lax*. Pertanto, i cookie che devono essere consegnati in un contesto intersito devono specificare in modo esplicito *SameSite=None* e devono essere contrassegnati con l&#39;attributo *Secure* e consegnati tramite *HTTPS*. Ulteriori dettagli su questi aggiornamenti sono disponibili nella pagina ufficiale sul cromo: <https://www.chromium.org/updates/same-site> e anche da <https://web.dev/samesite-cookies-explained/>.


### Aggiornamenti dell’autenticazione di Adobe Pass {#Pass-Updates}

Il servizio di autenticazione di Adobe Pass si basa attualmente su un paio di cookie, considerati cookie di terze parti dal punto di vista del browser, incluso Chrome, per funzionare in combinazione con alcune piattaforme e versioni degli SDK di autenticazione di Adobe Pass. Pertanto, per rispettare le modifiche imminenti e continuare a distribuire questi cookie in un contesto intersito da questi SDK precedenti, il servizio di autenticazione di Adobe Pass implementa le modifiche necessarie nella versione *adobe-pass-2.55.1*.

Le modifiche apportate alla versione *adobe-pass-2.55.1* comportano l&#39;aggiunta degli attributi *Secure* e *SameSite=None* per tutti i cookie restituiti a tutti gli SDK di autenticazione di Adobe Pass quando si utilizzano browser Chrome a partire dalla versione 80 e successive (ad eccezione della versione 82).

La sezione successiva presenta alcuni potenziali problemi per un elenco di piattaforme e versioni degli SDK di autenticazione di Adobe Pass nel caso in cui un utente utilizzi Chrome browser 80 e versioni successive (eccetto la versione 82).

## Risoluzione dei problemi {#Troubleshooting}

Durante la navigazione in questa sezione, ricorda che per tutti i cookie del servizio di autenticazione di Adobe Pass deve essere impostato l&#39;attributo *Secure* nella versione *adobe-pass-2.55.1* per tutti i browser, mentre l&#39;attributo *SameSite=None* deve essere impostato solo per i browser Chrome versione 80 e successive (ad eccezione della versione 82).


### Risoluzione dei problemi generali {#General}

1. È importante notare che alcuni agenti utente sono incompatibili con l&#39;attributo *SameSite=None*.

   - Versioni di Chrome da Chrome 51 a Chrome 66 (incluse su entrambe le estremità). Queste versioni di Chrome rifiuteranno un cookie con *SameSite=None*. Questo problema riguarda anche le versioni precedenti dei browser derivati da Chromium e Android WebView. Questo comportamento era corretto in base alla versione della specifica del cookie in quel momento, ma con l’aggiunta del nuovo valore &quot;None&quot; alla specifica, questo comportamento è stato aggiornato in Chrome 67 e versioni successive. Prima di Chrome 51, l&#39;attributo SameSite veniva completamente ignorato e tutti i cookie venivano trattati come se fossero *SameSite=None*.
   - Versioni di UC Browser su Android precedenti alla versione 12.13.2. Le versioni precedenti rifiuteranno un cookie con *SameSite=None*. Questo comportamento era corretto in base alla versione della specifica del cookie in quel momento, ma con l’aggiunta del nuovo valore &quot;None&quot; alla specifica, questo comportamento è stato aggiornato nelle versioni più recenti del browser UC.
   - Versioni di Safari e browser incorporati su MacOS 10.14 e tutti i browser su iOS 12. In queste versioni i cookie contrassegnati con *SameSite=None* verranno erroneamente considerati come contrassegnati come *SameSite=Strict*. Questo bug è stato corretto nelle versioni più recenti di iOS e MacOS.


1. È importante notare che i cookie con attributo *Secure* devono essere inviati tramite *HTTPS*, altrimenti il cookie non raggiungerà il servizio di autenticazione di Adobe Pass.

   - SDK di AccessEnabler per JavaScript:
      - È obbligatorio che la comunicazione con *sp.auth.adobe.com* utilizzi *HTTPS* per le versioni *2.35* e *3.5.0*, prima di introdurre la registrazione client dinamica.
   - SDK di AccessEnabler iOS/tvOS:
      - Obbligatorio che la comunicazione con *sp.auth.adobe.com* utilizzi *HTTPS* per le versioni precedenti a *3.0.0*, prima di introdurre la registrazione client dinamica.
   - SDK di AccessEnabler per Android:
      - Obbligatorio che la comunicazione con *sp.auth.adobe.com* utilizzi *HTTPS* per le versioni precedenti a *3.0.0*, prima di introdurre la registrazione client dinamica.
   - SDK di AccessEnabler FireOS:
      - È obbligatorio che la comunicazione con *sp.auth.adobe.com* utilizzi *HTTPS* per la versione *2.0.4*.

</br>

### AccessEnabler JavaScript SDK versione 2.35 Risoluzione dei problemi {#235-Troubleshooting}

Il flusso di autenticazione dell’utente potrebbe essere interessato in Chrome 80 e versioni successive (ad eccezione della versione 82). Per evitare problemi di autenticazione dell’utente a causa degli aggiornamenti di cui sopra, è possibile:

- Verificare che il cookie *JSESSIONID* sia impostato nel browser e che siano impostati gli attributi *SameSite=None* e *Secure*.
- Verificare che il cookie *JSESSIONID* della richiesta di rete *https://sp.auth.adobe.com/authenticate/saml* corrisponda al cookie *JSESSIONID* della richiesta di rete *https://sp.auth.adobe.com/session*.


### Risoluzione dei problemi di AccessEnabler JavaScript SDK versione 3.5.0 {#350-Troubleshooting}

Il flusso di autenticazione dell’utente potrebbe essere interessato in Chrome 80 e versioni successive (ad eccezione della versione 82). Per evitare problemi di autenticazione dell’utente a causa degli aggiornamenti di cui sopra, è possibile:

- Verificare che il cookie *JSESSIONID* sia impostato nel browser e che siano impostati gli attributi *SameSite=None* e *Secure*.
- Verificare che il cookie *JSESSIONID* della richiesta di rete *https://sp.auth.adobe.com/authenticate/saml* corrisponda al cookie *JSESSIONID* della richiesta di rete *https://sp.auth.adobe.com/session*.
- Verifica che il cookie *pass\_sfp* sia impostato nel browser e che siano impostati gli attributi *SameSite=None* e *Secure*.
- Verificare che il cookie *pass\_sfp* sia impostato nella richiesta di rete *https://sp.auth.adobe.com/session*.


Il flusso di autorizzazione dell’utente potrebbe essere interessato in Chrome 80 e versioni successive (ad eccezione della versione 82). Per evitare problemi all’utente nella visualizzazione di una risorsa protetta, dopo l’autenticazione corretta è possibile effettuare le seguenti operazioni:

- Verifica che il cookie *pass\_sfp* sia impostato nel browser e che siano impostati gli attributi *SameSite=None* e *Secure*.
- Verificare che il cookie *pass\_sfp* sia impostato nella richiesta di rete *https://sp.auth.adobe.com/adobe-services/authorize*.
- Verificare che il cookie *pass\_sfp* sia impostato nella richiesta di rete *https://sp.auth.adobe.com/adobe-services/shortAuthorize*.
