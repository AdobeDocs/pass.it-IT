---
title: Utilizzo dell’ID Experience Cloud nell’autenticazione di Adobe Pass
description: Utilizzo dell’ID Experience Cloud nell’autenticazione di Adobe Pass
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# Utilizzo dell’ID Experience Cloud nell’autenticazione di Adobe Pass

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Cos’è l’ID Experience Cloud e come ottenerlo? {#what-exp-cloud-id-obtain}

L’ID Experience Cloud (ECID) è un ID univoco generato da Adobe Experience Cloud per ogni singolo utente nell’applicazione/sito web. ECID è ampiamente utilizzato in tutti i rapporti di Experience Cloud utilizzati per collegare le informazioni su un utente specifico in più applicazioni/siti web.

Se disponi già di un sistema che fornisce un ID visitatore, utilizza lo stesso ID per l’ambito di questo documento.

Un modo per ottenere l’ECID è utilizzare il servizio ID Experience Cloud. Puoi utilizzare il tipo di implementazione preferito, basato su TDM, libreria JS, lato server, integrazione diretta o librerie native per piattaforme mobili. Per una visualizzazione completa dei servizi, delle librerie, degli SDK e delle guide all&#39;implementazione disponibili, vedere: <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html?lang=it>

## Quali vantaggi offre l’utilizzo dell’ID Experience Cloud nell’autenticazione di Adobe Pass? {#benefit-ex-cloud-id}

Se configuri gli SDK e l’API REST senza client per utilizzare il tuo ECID, in seguito potrai collegare i dati raccolti tramite l’autenticazione di Adobe Pass alle soluzioni Experience Cloud esistenti. Questo ti consentirà di comprendere meglio il percorso e l’esperienza dei clienti in tutte le soluzioni fornite da Adobe.

## Come si utilizza l’ID Experience Cloud nell’autenticazione di Adobe Pass? {#how-to-ex-cloud-id-authn}

Dopo aver ottenuto l’ECID (descritto in precedenza), devi trasmettere queste informazioni ai nostri SDK e alla nostra API REST senza client. Queste informazioni verranno successivamente trasmesse ai nostri server per ogni chiamata di rete effettuata dall’SDK. Il processo di configurazione è diverso per ogni SDK, come segue:

### SDK JS {#js-sdk}

Per JavaScript è necessario passare l’ECID in una mappa come terzo parametro alla chiamata setRequestor.

**Esempio di utilizzo:**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### SDK per iOS/tvOS {#ios-sdk}

Per l’SDK iOS/tvOS è disponibile un metodo dedicato denominato setOptions.

**Esempio di utilizzo:**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### SDK Android/fireTV {#android-sdk}

Per l’SDK di Android/fireTV, il meccanismo è simile a iOS. Solo il nome del parametro è diverso. La documentazione dell’API è disponibile qui.

**Esempio di utilizzo:**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### API senza client {#clientless-api}

Quando si utilizza Adobe Pass tramite la relativa API REST, il valore **ECID** deve essere inviato **su tutte le API** come parametro denominato **&#39;ap_vi&#39;**.

**Esempio di utilizzo:**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`
