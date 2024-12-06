---
title: Intestazione - AP-Device-Identifier
description: REST API V2 - Intestazione - AP-Device-Identifier
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# Intestazione - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>AP-Device-Identifier</b> contiene l&#39;identificatore del dispositivo di streaming creato dall&#39;applicazione client.

## Sintassi {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;tipo&gt; &lt;identificatore&gt;</td>
   </tr>
   <tr>
      <td>Tipo di intestazione</td>
      <td>Intestazione richiesta</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>No</td>
   </tr>
</table>

## Direttive {#directives}

<b>&lt;tipo></b>

Tipo di identificatore del dispositivo.

È disponibile un solo tipo supportato, come illustrato di seguito.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Tipo</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>impronta digitale</td>
      <td>
            L’identificatore del dispositivo è costituito da un identificatore stabile e univoco creato e gestito dall’applicazione client.
            <br/>
            L'applicazione client deve impedire le modifiche del valore causate da azioni dell'utente quali la disinstallazione, la reinstallazione o gli aggiornamenti dell'applicazione.
      </td>
   </tr>
</table>


<b>&lt;identificatore></b>

Il valore `Base64-encoded` dell&#39;identificatore del dispositivo.

## Esempio {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```

## Cookbook {#cookbooks}

>[!IMPORTANT]
>
> Le risorse di documentazione sono fornite a scopo di riferimento.
>
> Le risorse della documentazione non sono esaustive e potrebbero richiedere ulteriori modifiche per lavorare al progetto.
> 
> Indipendentemente dall&#39;implementazione effettiva, l&#39;intestazione `AP-Device-Identifier` deve contenere un valore formattato come descritto nella sezione [Direttive](#directives).

### Browser {#browsers}

Per generare l&#39;intestazione `AP-Device-Identifier` per i dispositivi in esecuzione in un browser, l&#39;applicazione client richiede di calcolare un identificatore stabile e univoco in base ai dati disponibili, ad esempio dati specifici del browser, del dispositivo o dell&#39;utente.

_(*) Si consiglia di integrare una libreria o un servizio che fornisca un meccanismo di rilevamento delle impronte digitali del browser o del dispositivo._

### Dispositivi mobili {#mobile-devices}

#### iOS e iPadOS {#ios-ipados}

Per generare l&#39;intestazione `AP-Device-Identifier` per i dispositivi che eseguono [iOS o iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes), è possibile fare riferimento ai seguenti documenti:

* Documentazione per gli sviluppatori di Apple per [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Si consiglia di applicare una funzione hash SHA-256 al valore del sistema operativo specificato._

#### Android {#android}

Per generare l&#39;intestazione `AP-Device-Identifier` per i dispositivi che eseguono [Android](https://developer.android.com/about/versions), è possibile fare riferimento ai seguenti documenti:

* Documentazione per gli sviluppatori di Android per [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Si consiglia di applicare una funzione hash SHA-256 al valore del sistema operativo specificato._

### Dispositivi collegati al televisore {#tv-connected-devices}

#### tvOS {#tvos}

Per generare l&#39;intestazione `AP-Device-Identifier` per i dispositivi che eseguono [tvOS](https://developer.apple.com/documentation/tvos-release-notes), è possibile fare riferimento ai seguenti documenti:

* Documentazione per gli sviluppatori di Apple per [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Si consiglia di applicare una funzione hash SHA-256 al valore del sistema operativo specificato._

#### Fire OS {#fireos}

Per generare l&#39;intestazione `AP-Device-Identifier` per i dispositivi che eseguono [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html), è possibile fare riferimento ai seguenti documenti:

* Documentazione per gli sviluppatori di Android per [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Si consiglia di applicare una funzione hash SHA-256 al valore del sistema operativo specificato._

#### Sistema operativo Roku {#rokuos}

Per generare l&#39;intestazione `AP-Device-Identifier` per i dispositivi che eseguono [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md), è possibile fare riferimento ai seguenti documenti:

* Documentazione per gli sviluppatori Roku per [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string).

_(*) Si consiglia di applicare una funzione hash SHA-256 al valore del sistema operativo specificato._
