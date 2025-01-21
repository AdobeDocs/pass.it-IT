---
title: Configurazione dell’ambiente e test in Pre-Qual
description: Configurazione dell’ambiente e test in Pre-Qual
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# Configurazione dell&#39;ambiente e test in Pre-Qual{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>Il contenuto presente in questa pagina è fornito solo a scopo informativo. L&#39;utilizzo di questa API richiede una licenza valida da Adobe Systems. Non è consentito alcun uso non autorizzato.

Lo scopo di questa nota tecnica è aiutare i nostri partner a configurare il loro ambiente e iniziare a testare un nuovo versione distribuito nell&#39;ambiente di prequalificazione Adobe Systems.

Poiché ci sono due versione versioni: ***produzione*** e ***staging***, in questo documento concentrarsi sulla configurazione della produzione con la menzione che tutti i passaggi sono gli stessi per la messa in scena, solo gli URL sono diversi.

I passaggi 1 e 2 sono la configurazione dell&#39;ambiente di test su una delle macchine di test, i passaggi 3 sono una verifica del flusso di base e i passaggi 4 e 5 presentano alcune linee guida di test.

>[!IMPORTANT]
>
> È molto importante eseguire i passaggi 1 e 2 ogni volta che si desidera modificare l&#39;ambiente di test (passando dal profilo di staging a quello di produzione o viceversa)


## PASSAGGIO 1: Risoluzione del passaggio del dominio a un IP {#resolving-pass-domain-to-an-ip}

* Per trovare un IP del load balancer che possa essere utilizzato per lo spoofing, eseguire il comando seguente:

* **Su Windows**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

```cmd
C:\>nslookup entitlement-prequal.auth.adobe.com 
...
Addresses:  52.26.79.43
            54.190.212.171
```

```Choose any IP from **addresses** section (e.g. `54.190.212.171)```


* **Su Linux/Mac**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

```sh
    $ dig entitlement-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.26.79.43
    ............ 60 IN A      54.190.212.171
```

```Choose any IP from **A records (**e.g `54.190.212.171)```

>[!NOTE]
>
>Domini esclusi dalla risposta in quanto non pertinenti e potrebbero differire da utente a utente.

>[!IMPORTANT]
>
> Questi indirizzi IP potrebbero cambiare in futuro e potrebbero non essere gli stessi per gli utenti in aree geografiche diverse.


## PASSO 2.  Spoofing dell&#39;ambiente di prequalificazione da produrre {#spoofing-the-prequalification-environment}

* Modifica il file c:\\windows\\System32\\drivers\\etc\\hosts *(in Windows) o*/*etc/hosts* (su Macintosh/Linux/Android) e aggiungere quanto segue:

* Profilo di produzione spoof
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * entitlement.auth.adobe.com.

**Spoofing su Android:** Per eseguire lo spoofing su Android, è necessario utilizzare un emulatore Android.

* Una volta attivato lo spoofing, puoi semplicemente utilizzare gli URL regolari per i profili di produzione e staging: (ovvero, `http://sp.auth-staging.adobe.com` e `http://entitlement.auth-staging.adobe.com` e raggiungerai effettivamente *ambiente di pre-qualificazione/produzione* della* nuova build.


## PASSAGGIO 3:  Verifica di puntare all&#39;ambiente giusto {#Verify-you-are-pointing-to-the-right-environment}

**Questo è un passaggio semplice:**

* Load [entitlement prequal environment](https://entitlement-prequal.auth.adobe.com/environment.html) and [entitlement](https://entitlement.auth.adobe.com/environment.html). Dovrebbero restituire la stessa risposta.


## PASSO 4.  Eseguire un semplice flusso di autenticazione/autorizzazione utilizzando il sito Web del programmatore {#peform-a-simple-auth-flow}

* Questo passaggio richiede l&#39;indirizzo del sito Web del programmatore e alcune credenziali MVPD valide (una utente che sia autenticato e autorizzato).

## PASSO 5.  Eseguire il test dello scenario utilizzando i siti Web del programmatore {#perform-scenario-testing-using-programmer-website}

* Dopo aver completato la configurazione dell’ambiente e aver verificato il funzionamento del flusso di autenticazione-autorizzazione di base, puoi procedere con il test di scenari più complessi.


## PASSAGGIO 6:  Eseguire test utilizzando il sito di test API {#perform-testing-using-api-testing-site}

* Se si desidera approfondire la verifica dell&#39;autenticazione di Adobe Pass, si consiglia di utilizzare il [sito di test API](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

Per ulteriori informazioni sul sito di test delle API, vedere [Come testare i flussi di Authentication e autorizzazione utilizzando il sito](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md) di test delle API di Adobe Systems.
