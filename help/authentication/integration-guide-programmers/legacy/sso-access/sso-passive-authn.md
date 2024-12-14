---
title: SSO tramite autenticazione passiva
description: SSO tramite autenticazione passiva
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 0%

---

# (Legacy) SSO tramite autenticazione passiva

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

>[!IMPORTANT]
>
> Assicurati di essere sempre informato sugli ultimi annunci di prodotto per l&#39;autenticazione di Adobe Pass e sulle timeline di disattivazione aggregate nella pagina [Annunci di prodotto](/help/authentication/product-announcements.md).

## Introduzione

Scopo del presente documento è descrivere l’implementazione del flusso di autenticazione passiva e il suo funzionamento con l’approccio standard del Single Sign-On.

## Casi d&#39;uso

L’autenticazione Adobe Pass abilita il Single Sign-On (SSO) tra app/siti. Dopo che un utente effettua l’accesso con le proprie credenziali MVPD, l’autenticazione di Adobe Pass genera un token protetto che rappresenta la sessione di autenticazione di MVPD e associa tale token al dispositivo dell’utente utilizzando un ID dispositivo. L’autenticazione Adobe Pass memorizza il token o l’ID dispositivo su un server o sul dispositivo.

Se il token è ancora valido, gli utenti verranno visualizzati direttamente come autenticati. In questo modo gli utenti possono immettere le credenziali con minore frequenza, mantenendo al contempo sicure le transazioni.



Il caso di utilizzo aziendale descritto di seguito è un requisito molto specifico: che l’utente DEVE essere autenticato almeno una volta per ogni sito visitato. Questo consente a MVPD di applicare regole business associate alla sessione di autenticazione che possono variare a seconda della rete. È in conflitto con la promessa TVE corrente che un utente deve effettuare l’accesso una sola volta e sarà autenticato su tutti i siti che fanno parte dell’ecosistema di autenticazione di Adobe Pass.



Al fine di mantenere la regola di business ma anche una buona esperienza utente, MVPD NON richiede a un utente di fornire manualmente le informazioni sulle credenziali. Possiamo beneficiare del cookie di sessione impostato in precedenza e tentare di effettuare una riautenticazione automatica utilizzando il flusso passivo; dal punto di vista dell’utente, apparirà come connesso automaticamente.



Per risolvere questi problemi, abbiamo implementato 2 funzioni distinte: autenticazione per rete e supporto dell’autenticazione passiva. Gli MVPD supporteranno l&#39;autenticazione passiva SAML in cui si limitano a autenticare nuovamente un utente se nell&#39;IdP esiste una sessione di autenticazione, indipendentemente dal sito in cui è stata creata.



## Autenticazione per rete

Questa funzione assicura che MVPD riceva una richiesta di autenticazione una volta per ogni sito visitato. Questa funzionalità indica che un token di autenticazione Adobe Pass sarà limitato a requestorID, ed è quindi valido solo per la rete che lo ha richiesto. Di conseguenza, una volta che l’utente si autentica sul sito &quot;A&quot; e successivamente visita il sito &quot;B&quot;, sarà necessario eseguire l’autenticazione.



Tieni presente che a causa del fatto che su MVPD IdP l’utente è già autenticato, non gli verrà richiesto di fornire le informazioni di accesso, ma il browser verrà semplicemente reindirizzato dal sito &quot;B&quot; a MVPD IdP e quindi nuovamente. Lo stesso utente sarà comunque autenticato sul sito &quot;A&quot; se ritorna.



Il seguente flusso illustra la funzione Autenticazione di base per rete:





## Autenticazione passiva

L’obiettivo è fare in modo che il processo di riautenticazione avvenga in background senza la necessità di un reindirizzamento completo del browser e che venga visualizzato il selettore. Di conseguenza, un utente che si sposta dal sito A al sito B sarà automaticamente autenticato.



Dal punto di vista dell’esperienza utente, non vi sarà alcuna differenza tra questo flusso e un flusso eseguito con un normale MVPD. Ciò che l’utente vede è che dopo aver inserito le credenziali in seguito alla visita del sito A, verrà automaticamente autenticato sul sito B.



Per rendere fattibile questo flusso, MVPD dovrà supportare l’autenticazione passiva in modo che l’iframe nascosto non si blocchi sulla pagina di accesso nel caso in cui MVPD non abbia più una sessione. Questa operazione viene eseguita tramite l’attributo standard &quot;isPassive&quot;.



Il diagramma seguente illustra il flusso migliorato e l’autenticazione passiva &quot;dietro le quinte&quot;:





Esempio di richiesta SAML
Di seguito è riportato un esempio di richiesta SAML per il flusso di autenticazione passivo:


```xml
<saml2p:AuthnRequest xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                     AssertionConsumerServiceURL="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                     Destination="https://mvpd_idp_url"
                     ForceAuthn="false"
                     ID="_15056686-399c-4528-b21a-4a9542cfc8ec"
                     IsPassive="true"
                     IssueInstant="2014-11-03T14:18:12.394Z"
                     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                     Version="2.0"
                     >
    <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth.adobe.com </saml2:Issuer>
    <saml2p:Extensions>
        <thrpty:RespondTo xmlns:thrpty="urn:oasis:names:tc:SAML:protocol:ext:third-party">https://saml.sp.auth.adobe.com</thrpty:RespondTo>
    </saml2p:Extensions>
    <saml2p:NameIDPolicy AllowCreate="true"
                         Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient"
                         SPNameQualifier="https://saml.sp.auth.adobe.com"
                         />
</saml2p:AuthnRequest>
```

## Regole aziendali

Gli MVPD hanno restrizioni di dominio con ambito SSO specifiche. Ad esempio, alcuni MVPD potrebbero consentire solo alcuni domini (SSO per la stessa società di media ma non tra società).
Alcuni MVPD potrebbero richiedere l&#39;applicazione di regole di autenticazione diverse. Ad esempio, gli MVPD potrebbero avere TTL di autenticazione diversi per reti diverse. Inoltre, gli MVPD potrebbero abilitare l&#39;autenticazione basata su home per alcune reti ma non per altre (qui sono fortemente rappresentati i casi di utilizzo del controllo genitori).


Questi requisiti aziendali devono tenere presente che il caso d’uso principale è che l’utente non deve essere tenuto ad accedere nuovamente dopo aver effettuato correttamente l’accesso con il proprio MVPD.

Questa operazione può essere eseguita utilizzando l’autenticazione per rete con il flag di autenticazione passiva.



## Limitazioni note

iOS: a causa della natura dell’archiviazione locale di iOS, i flussi SSO non funzionano su iOS per le applicazioni sviluppate da fornitori diversi. Per ulteriori informazioni sull’SSO in iOS 8 e versioni successive, consulta questa nota tecnica.


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
