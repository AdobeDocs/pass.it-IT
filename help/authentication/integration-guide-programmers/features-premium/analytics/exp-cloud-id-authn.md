---
title: Utilizzo dell’ID di Experience Cloud nell’autenticazione di Adobe Pass
description: Utilizzo dell’ID di Experience Cloud nell’autenticazione di Adobe Pass
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---

# Utilizzo dell’ID di Experience Cloud nell’autenticazione di Adobe Pass

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Cos’è Experience Cloud ID e come ottenerlo? {#what-exp-cloud-id-obtain}

L’Experience Cloud ID (ECID in breve) è un ID univoco generato da Adobe Experience Cloud per ogni singolo utente nell’applicazione/sito web. ECID è ampiamente utilizzato in tutti i rapporti di Experience Cloud utilizzati per collegare informazioni su un utente specifico in più applicazioni/siti web.

Se disponi già di un sistema che fornisce un ID visitatore, utilizza lo stesso ID per l’ambito di questo documento.

Un modo per ottenere l’ECID è utilizzare il servizio Experience Cloud ID. Puoi utilizzare il tipo di implementazione preferito, basato su TDM, libreria JS, lato server, integrazione diretta o librerie native per piattaforme mobili. Per una visualizzazione completa dei servizi, delle librerie, delle guide di SDK e di implementazione disponibili, vedere: <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html?lang=it>

## Quali vantaggi offre l’utilizzo dell’Experience Cloud ID nell’autenticazione di Adobe Pass? {#benefit-ex-cloud-id}

Se configuri gli SDK e l’API REST senza client per utilizzare il tuo ECID, in seguito potrai collegare i dati raccolti tramite l’autenticazione di Adobe Pass alle soluzioni Experience Cloud esistenti. Questo ti consentirà di comprendere meglio il percorso e l’esperienza dei clienti in tutte le soluzioni fornite da Adobe.

## Come si utilizza l’Experience Cloud ID nell’autenticazione di Adobe Pass? {#how-to-ex-cloud-id-authn}

Dopo aver ottenuto l’ECID (descritto in precedenza), devi trasmettere queste informazioni ai nostri SDK e alla nostra API REST senza client. Queste informazioni saranno successivamente trasmesse ai nostri server per ogni chiamata di rete effettuata da SDK. Il processo di configurazione è diverso per ogni SDK, come segue:

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

### iOS/tvOS SDK {#ios-sdk}

Per iOS/tvOS SDK è disponibile un metodo dedicato denominato setOptions.

**Esempio di utilizzo:**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### SDK Android/fireTV {#android-sdk}

Per Android/fireTV SDK il meccanismo è simile a iOS. Solo il nome del parametro è diverso. La documentazione dell’API è disponibile qui.

**Esempio di utilizzo:**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### API senza client {#clientless-api}

Quando si utilizza Adobe Pass tramite l&#39;API REST v1, il valore **ECID** deve essere inviato **su tutte le API** come parametro denominato **&#39;ap_vi&#39;**.

**Esempio di utilizzo:**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`

### API REST V2 {#rest-api-v2}

Quando si utilizza Adobe Pass tramite l&#39;API REST v2, il valore **ECID** deve essere inviato **su tutte le API** come intestazione denominata **&#39;AP-Visitor-Identifier&#39;**.

**Esempio di utilizzo:**

`POST: https://api.auth.adobe.com/api/v2/${serviceProvider}/sessions/`\
Intestazioni:\
`AP-Visitor-Identifier: THE_ECID_VALUE`

