---
title: Aggiornamenti dei cookie - Flag SameSite e Secure
description: Aggiornamenti dei cookie - Flag SameSite e Secure
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 0%

---

# Aggiornamenti dei cookie - Flag SameSite e Secure {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>


## Aggiornamenti {#Updates}

Questa sezione evidenzia le modifiche introdotte dal browser Chrome e dall’autenticazione Adobe Pass per la gestione dei cookie di terze parti.



### Aggiornamenti di Chrome 80 {#Chrome}

A partire da Chrome versione 80 (eccetto la versione 82), i cookie che non specificano un *SameSite* verrà trattato come se fosse *SameSite=Lax*. Pertanto, i cookie che devono essere consegnati in un contesto cross-site devono specificare esplicitamente *SameSite=Nessuno*, e devono anche essere contrassegnati con *Protetto* e consegnati tramite *HTTPS*. Maggiori dettagli su questi aggiornamenti possono essere letti dalla pagina ufficiale chromium: <https://www.chromium.org/updates/same-site> e anche da <https://web.dev/samesite-cookies-explained/>.


### Aggiornamenti dell’autenticazione di Adobe Pass {#Pass-Updates}

Il servizio di autenticazione di Adobe Pass si basa attualmente su un paio di cookie, considerati cookie di terze parti dal punto di vista del browser, incluso Chrome, per funzionare in combinazione con alcune piattaforme e versioni degli SDK di autenticazione di Adobe Pass. Pertanto, al fine di rispettare le modifiche imminenti e continuare a fornire questi cookie in un contesto cross-site da questi SDK precedenti, il servizio di autenticazione di Adobe Pass implementa le modifiche necessarie nel *adobe-pass-2.55.1* versione.

Queste modifiche apportate dal *adobe-pass-2.55.1* versione comporta l&#39;aggiunta di *Protetto* e *SameSite=Nessuno* Attributi di tutti i cookie restituiti a tutti gli SDK di autenticazione di Adobe Pass quando si utilizzano i browser Chrome a partire dalla versione 80 e successive (eccetto la versione 82).

La sezione successiva presenta alcuni potenziali problemi per un elenco di piattaforme e versioni degli SDK di autenticazione di Adobe Pass nel caso in cui un utente utilizzi il browser Chrome 80 e versioni successive (eccetto la versione 82).

## Risoluzione dei problemi {#Troubleshooting}

Durante la navigazione in questa sezione tieni presente che tutti i cookie del servizio di autenticazione di Adobe Pass devono avere *Protetto* attributo impostato in *adobe-pass-2.55.1* versione per tutti i browser, mentre il *SameSite=Nessuno* L’attributo deve essere impostato solo per i browser Chrome versione 80 e successive (eccetto la versione 82).


### Risoluzione dei problemi generali {#General}

1. È importante notare che alcuni agenti utente sono noti per essere incompatibili con *SameSite=Nessuno* attributo.

   - Versioni di Chrome da Chrome 51 a Chrome 66 (incluse su entrambe le estremità). Queste versioni di Chrome rifiuteranno un cookie con *SameSite=Nessuno*. Questo problema riguarda anche le versioni precedenti dei browser derivati da Chromium e Android WebView. Questo comportamento era corretto in base alla versione della specifica del cookie in quel momento, ma con l’aggiunta del nuovo valore &quot;None&quot; alla specifica, questo comportamento è stato aggiornato in Chrome 67 e versioni successive. (Prima di Chrome 51, l’attributo SameSite veniva completamente ignorato e tutti i cookie venivano trattati come se fossero *SameSite=Nessuno*.)
   - Versioni di UC Browser su Android precedenti alla versione 12.13.2. Le versioni precedenti rifiuteranno un cookie con *SameSite=Nessuno*. Questo comportamento era corretto in base alla versione della specifica del cookie in quel momento, ma con l’aggiunta del nuovo valore &quot;None&quot; alla specifica, questo comportamento è stato aggiornato nelle versioni più recenti del browser UC.
   - Versioni di Safari e browser incorporati su MacOS 10.14 e tutti i browser su iOS 12. Queste versioni tratteranno erroneamente i cookie contrassegnati con *SameSite=Nessuno* come se fossero contrassegnati *SameSite=Strict*. Questo bug è stato corretto nelle versioni più recenti di iOS e MacOS.


1. Importante notare che i cookie con *Protetto* l&#39;attributo deve essere inviato *HTTPS*, altrimenti il cookie non raggiungerà il servizio di autenticazione di Adobe Pass.

   - SDK JavaScript per AccessEnabler:
      - Obbligatorio che la comunicazione con *sp.auth.adobe.com* utilizza *HTTPS* per le versioni *2,35* e *3,5,0*, prima di introdurre Dynamic Client Registration.
   - SDK di AccessEnabler iOS/tvOS:
      - Obbligatorio che la comunicazione con *sp.auth.adobe.com* utilizza *HTTPS* per le versioni precedenti a *3,0,0*, prima di introdurre Dynamic Client Registration.
   - SDK di AccessEnabler per Android:
      - Obbligatorio che la comunicazione con *sp.auth.adobe.com* utilizza *HTTPS* per le versioni precedenti a *3,0,0*, prima di introdurre Dynamic Client Registration.
   - SDK di AccessEnabler FireOS:
      - Obbligatorio che la comunicazione con *sp.auth.adobe.com* utilizza *HTTPS* per versione *2.0.4.*.

</br>

### AccessEnabler JavaScript SDK versione 2.35 Risoluzione dei problemi {#235-Troubleshooting}

Il flusso di autenticazione dell’utente potrebbe essere interessato in Chrome 80 e versioni successive (ad eccezione della versione 82). Per evitare problemi di autenticazione dell’utente a causa degli aggiornamenti di cui sopra, è possibile:

- Verifica che la *JSESSIONID* il cookie è impostato nel browser e presenta *SameSite=Nessuno* e *Protetto* attributi impostati.
- Verifica che la *JSESSIONID* cookie da *https://sp.auth.adobe.com/authenticate/saml* la richiesta di rete corrisponde al *JSESSIONID* cookie da *https://sp.auth.adobe.com/session* richiesta di rete.


### Risoluzione dei problemi relativi all’SDK JavaScript per AccessEnabler versione 3.5.0 {#350-Troubleshooting}

Il flusso di autenticazione dell’utente potrebbe essere interessato in Chrome 80 e versioni successive (ad eccezione della versione 82). Per evitare problemi di autenticazione dell’utente a causa degli aggiornamenti di cui sopra, è possibile:

- Verifica che la *JSESSIONID* il cookie è impostato nel browser e presenta *SameSite=Nessuno* e *Protetto* attributi impostati.
- Verifica che la *JSESSIONID* cookie da *https://sp.auth.adobe.com/authenticate/saml* la richiesta di rete corrisponde al *JSESSIONID* cookie da *https://sp.auth.adobe.com/session* richiesta di rete.
- Verifica che la *pass\_sfp* il cookie è impostato nel browser e presenta *SameSite=Nessuno* e *Protetto* attributi impostati.
- Verifica che la *pass\_sfp* cookie è impostato in *https://sp.auth.adobe.com/session* richiesta di rete.


Il flusso di autorizzazione dell’utente potrebbe essere interessato in Chrome 80 e versioni successive (ad eccezione della versione 82). Per evitare problemi all’utente nella visualizzazione di una risorsa protetta, dopo l’autenticazione corretta è possibile effettuare le seguenti operazioni:

- Verifica che la *pass\_sfp* il cookie è impostato nel browser e presenta *SameSite=Nessuno* e *Protetto* attributi impostati.
- Verifica che la *pass\_sfp* cookie è impostato in *https://sp.auth.adobe.com/adobe-services/authorize* richiesta di rete.
- Verifica che la *pass\_sfp* cookie è impostato in *https://sp.auth.adobe.com/adobe-services/shortAuthorize* richiesta di rete.
