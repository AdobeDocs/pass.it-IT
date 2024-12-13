---
title: Ripristina passaggio temporaneo su iOS
description: Ripristina passaggio temporaneo su iOS
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 0%

---

# (Legacy) Ripristina passaggio temporaneo su iOS {#reset-temp-pass-on-ios}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

</br>

L’app di dimostrazione iOS include una schermata dedicata per il ripristino del TTL di Temp Pass. Per l&#39;operazione di ripristino sono necessarie le seguenti informazioni:

- Adobe **Ambiente:** specifica l&#39;endpoint del server pass di PayTV che riceverà la chiamata di rete Reset Temp Pass. Valori possibili: **Prequal** (*mgmt-prequal.auth-staging.adobe.com*), **Release** (*mgmt.auth.adobe.com*) o **Custom** (riservato per test interni di Adobe).
- **Token Bearer OAuth2:** il token OAuth2 è necessario per autorizzare il programmatore per l&#39;autenticazione Adobe della Pay-TV. Tale token può essere ottenuto dall&#39;endpoint OAuth2 dedicato di autenticazione della Pay-TV (ad esempio *curl -u &quot;\&lt;consumer\_key\>:\&lt;consumer\_secret\_key\>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials&quot;*).
- **ID richiedente:** l&#39;ID univoco per il programmatore corrente. Questo valore viene letto dalla schermata principale dell’app demo (il campo richiedente).
- **ID passaggio temporaneo:** l&#39;ID univoco per il MVPD passaggio temporaneo.
- **ID dispositivo:** ID dispositivo con hash calcolato dall&#39;app demo.
- **Chiave generica:** alcuni file MVPD di Passaggio temporaneo (ovvero la funzionalità Passaggio temporaneo estensibile successiva) supportano una chiave generica per la reimpostazione del Passaggio temporaneo (insieme all&#39;ID dispositivo).

Tutti i parametri di cui sopra (ad eccezione della *chiave generica*) sono obbligatori. Ecco un esempio di parametri e la chiamata di rete associata che verrà eseguita dall’app demo (l’esempio è sotto forma di un comando *curl *):

- **Ambiente:** Versione (*mgmt.auth.adobe.com*)
- **Token Bearer OAuth2:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **ID programmatore:** REF
- **ID passaggio temporaneo:** TempPassREF
- **ID dispositivo:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **Chiave generica:** null (nessun valore specificato)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

verrà effettuata una richiesta HTTP DELETE all&#39;endpoint **/reset**, passando il *Token Bearer OAuth2* nell&#39;intestazione Autorizzazione e il *ID dispositivo*, *ID richiedente* e *ID passaggio temporaneo (ID MVPD)* come parametri.

Se il programmatore fornisce un valore per la *chiave generica*, verrà eseguita un&#39;altra chiamata HTTP (questa volta all&#39;endpoint **/reset/generic**), passando la *chiave generica* nel parametro della richiesta *chiave*.

Ad esempio, impostando la *chiave generica* su un hash di indirizzo di posta elettronica (per
I file MVPD con passaggio temporaneo che supportano questo tipo di funzionalità) produrranno
seguente chiamata HTTP (l&#39;e-mail è `user@domain.com` il relativo SHA-256
hash `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

È importante sottolineare che il ripristino di Passata temporanea nell’app demo potrebbe non avere lo stesso effetto per un’app Programmatore sullo stesso dispositivo. Ciò è dovuto al fatto che l’ID dispositivo (calcolato dall’app demo e da AccessEnabler) potrebbe non essere lo stesso per tutte le applicazioni sul dispositivo:

- iOS 6 e versioni precedenti: l’ID dispositivo viene calcolato utilizzando l’indirizzo MAC (che è univoco per tutte le app), pertanto il ripristino di Passaggio temporaneo nell’app demo lo reimposterà in tutte le altre app Programmatore sul dispositivo.

- iOS 7 e versione superiore: l’ID dispositivo viene calcolato in base al valore IDFV (ID per fornitore), che è univoco per tutte le applicazioni che hanno lo stesso prefisso dell’ID bundle (ovvero tutti i componenti tranne l’ultimo). Poiché l’app demo e un’app programmatore hanno ID bundle diversi, il ripristino di Passaggio temporaneo nell’app demo non avrà alcun effetto su un’app programmatore.

L’ultimo caso d’uso (iOS 7 e versioni successive) è il più comune, quindi vediamo come i programmatori possono reimpostare il passaggio temporaneo per le loro app in questa situazione. Sono disponibili diverse opzioni:

1. Porta il codice dall’app demo all’app programmatore. Le classi *TempPassResetViewController* e *DeviceIdDemoApp* contengono la logica di base per la reimpostazione di Temp Pass e possono essere facilmente modificate e incluse nell&#39;app Programmatore.

1. Esegui la richiesta HTTP per reimpostare il passaggio temporaneo con *curl*. Il parametro device\_Id può essere ottenuto calcolando l&#39;IDFV dell&#39;app Programmer e applicando un hash SHA-256 (codice di esempio nella classe *DeviceIdDemoApp*).

1. È sufficiente eseguire il ripristino dall&#39;app demo specificando l&#39;IDFV con hash dell&#39;app del programmatore come *chiave generica*. Ciò comporterà due chiamate di rete: una per la reimpostazione del passaggio temporaneo per l&#39;app demo (irrilevante per il programmatore) e una per la reimpostazione del passaggio temporaneo per l&#39;app programmatore.

Tutte le opzioni di cui sopra sono simili, spetta al programmatore sceglierne una a seconda della facilità di implementazione.
