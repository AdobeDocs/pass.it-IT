---
title: Evita di usare '&'reg_code nella richiesta /authenticate
description: Evita di usare '&'reg_code nella richiesta /authenticate
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Evita di usare &#39;&amp;&#39;reg_code nella richiesta /authenticate {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>



## Problema

Il browser IE 9 interpreta &#39;\&amp;reg&#39; come comando speciale e lo converte in ®.

## Spiegazione

Se la richiesta `/authenticate` è composta come segue...


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


...verrà interpretato dal browser IE come di seguito e inviato all&#39;Adobe in questo formato:


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


Il richiedente\_id verrà interpretato come univision®\_code=EKAFMFI, poiché non esiste &#39;&amp;&#39; e l&#39;Adobe non troverà un parametro `regCode` a cui associare il token.  È possibile che il token AuthN non venga creato, nel qual caso `/checkauthn` chiamate non riusciranno a trovare alcun token.



## Soluzione

Per risolvere il problema, scegliere una delle seguenti opzioni:

1. Evitare di utilizzare il parametro `&reg_code` tra gli altri parametri della stringa di query.  Spostalo invece sul primo parametro della stringa di query nell’URL della richiesta, creando l’URL della richiesta come segue:


       &lt;FQDN>autentica?reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   In questo modo, il parametro `&reg` non verrà interpretato in modo errato.

1. Normalizza `&reg_code` come utente di `&amp;reg_code`.

1. Un Adobe potrebbe introdurre una nuova funzione per inviare un codice di errore alla seconda schermata in risposta a una chiamata di autenticazione, se la creazione del token AuthN non è riuscita.
