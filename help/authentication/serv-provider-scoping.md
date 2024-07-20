---
title: Ambito provider di servizi
description: Ambito provider di servizi
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Ambito provider di servizi {#service-provoider-scoping}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

L&#39;implementazione predefinita di un&#39;integrazione di autenticazione Adobe Pass con un MVPD si basa sulla **specifica OLCA**. Nella sezione Requisiti di autenticazione della specifica OLCA (6.5, Subject Identifier) è indicato che è possibile indicare l&#39;ambito del provider di servizi (SP) per l&#39;identificativo del soggetto. L&#39;identificatore del soggetto è l&#39;ID utente offuscato che MVPD restituisce all&#39;SP.  In un’integrazione di autenticazione Adobe Pass, gli MVPD devono abilitare l’ambito delle richieste di autenticazione SP.

Con l’autenticazione di Adobe Pass che assume il ruolo di SP per il programmatore, è necessario implementare una personalizzazione che consenta l’ambito SP della richiesta di autenticazione.  Questa operazione deve essere eseguita in modo che MVPD possa identificare il brand di rete passato nell’asserzione SAML al provider di identità (IdP) di MVPD.  L&#39;ambito può essere implementato in uno dei due modi descritti nella sezione successiva.

## Ambito provider di servizi {#service-provider-scoping}

L’autenticazione Adobe Pass supporta i due modi seguenti per abilitare l’ambito SP delle richieste di autenticazione:

* **Approccio emittente SAML.** In questo approccio, &quot;ID richiedente&quot; viene aggiunto alla stringa emittente SAML nella richiesta di autenticazione SAML.

* **Approccio Proprietà Di Ambito Personalizzato.** In questo approccio, &quot;ID richiedente&quot; viene incluso esplicitamente come proprietà personalizzata &quot;Scoping&quot; nella richiesta di autenticazione SAML.

>[!NOTE]
>
>L’&quot;ID richiedente&quot; è il modo in cui l’autenticazione Adobe Pass si riferisce al marchio della rete del programmatore (ad esempio: &quot;CNN&quot; è uno dei marchi della rete Turner).

### Approccio emittente SAML {#saml-issuer-approach}

Questo approccio utilizza l&#39;elemento SAML `<Issuer>` nella richiesta di autenticazione SAML, come mostrato nel seguente snippet:

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### Approccio proprietà ambito personalizzato {#custom-scoping-property-approach}

Questo approccio utilizza una proprietà personalizzata denominata &quot;Scoping&quot;, come mostrato in questo snippet di una richiesta di autenticazione SAML:

```xml
...
<samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">requestorID</samlp:RequesterID>
</samlp:Scoping>
...
```

<!--
>[!RELATEDINFORMATION]
>* [MVPD Authentication](/help/authentication/authn-usecase.md)
>* **OLCA Specification**
-->
