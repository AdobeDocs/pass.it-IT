---
title: Intestazione - X-Device-Info
description: REST API V2 - Intestazione - X-Device-Info
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---


# Intestazione - X-Device-Info {#header-x-device-info}

## Panoramica {#overview}

L&#39;intestazione della richiesta <b>X-Device-Info</b> contiene le informazioni client (dispositivo, connessione e applicazione) relative al dispositivo di streaming effettivo.

## Sintassi {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;informazioni_dispositivo&gt;</td>
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

<b>&lt;informazioni_dispositivo></b>

Il valore `Base64-encoded` dell&#39;elemento JSON contenente almeno gli attributi contrassegnati come richiesto dalla tabella seguente.

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Chiave</th>
        <th style="background-color: #EFF2F7;">Descrizione</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Presenza</th>
        <th style="background-color: #EFF2F7;">Valori possibili</th>
    </tr>
    <tr>
        <td>primaryHardwareType</td>
        <td>Il tipo di hardware principale del dispositivo.</td>
        <td></td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Fotocamera</li>
                <li>DataCollectionTerminal</li>
                <li>Desktop</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>Console giochi</li>
                <li>GeolocationTracker</li>
                <li>Occhiali</li>
                <li>MediaPlayer</li>
                <li>Cellulare</li>
                <li>PaymentTerminal</li>
                <li>PluginModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>Tablet</li>
                <li>Punto attivo wireless</li>
                <li>Orologio</li>
                <li>Sconosciuto</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>modello</td>
        <td>Il nome del modello del dispositivo.</td>
        <td><i>obbligatorio</i></td>
        <td>ad es. iPhone, SM-G930V, AppleTV, ecc.</td>
    </tr>
    <tr>
        <td>version</td>
        <td>Versione del dispositivo.</td>
        <td></td>
        <td>ad esempio 2.0.1, ecc.</td>
    </tr>
    <tr>
        <td>produttore</td>
        <td>La società/organizzazione di produzione del dispositivo.</td>
        <td></td>
        <td>ad es. Samsung, LG, ZTE, Huawei, Motorola, Apple, ecc.</td>
    </tr>
    <tr>
        <td>fornitore</td>
        <td>La società/organizzazione di vendita del dispositivo.</td>
        <td></td>
        <td>ad es. Apple, Samsung, LG, Google, ecc.</td>
    </tr>
    <tr>
        <td>osName</td>
        <td>Il nome del sistema operativo del dispositivo.</td>
        <td><i>obbligatorio</i></td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Android</li>
                <li>CHROME OS</li>
                <li>Linux</li>
                <li>MAC OS</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Sistema operativo Roku</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osFamily</td>
        <td>Il nome del gruppo del sistema operativo del dispositivo.</td>
        <td></td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>Sistema operativo PlayStation</li>
                <li>Sistema operativo Roku</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVendor</td>
        <td>Il fornitore del sistema operativo del dispositivo.</td>
        <td></td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Progetto Tizen</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVersion</td>
        <td>Versione del sistema operativo del dispositivo.</td>
        <td></td>
        <td>ad esempio 10.2, 9.0.1, ecc.</td>
    </tr>
    <tr>
        <td>browserName</td>
        <td>Nome del browser.</td>
        <td></td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Browser Android</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonkey</li>
                <li>Browser Symbian</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVendor</td>
        <td>La società/organizzazione di costruzione del browser.</td>
        <td></td>
        <td>
            I valori sono limitati:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>Motorola</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVersion</td>
        <td>Versione del browser del dispositivo.</td>
        <td></td>
        <td>esempio: 60.0.3112</td>
    </tr>
    <tr>
        <td>userAgent</td>
        <td>L’agente utente del dispositivo.</td>
        <td></td>
        <td>ad esempio Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, like Gecko) versione/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td>displayWidth</td>
        <td>Larghezza fisica dello schermo del dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayHeight</td>
        <td>L’altezza fisica dello schermo del dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayPpi</td>
        <td>Densità fisica dei pixel dello schermo del dispositivo.</td>
        <td></td>
        <td>Esempio: 294</td>
    </tr>
    <tr>
        <td>diagonalScreenSize</td>
        <td>La dimensione diagonale dello schermo fisico del dispositivo, espressa in pollici.</td>
        <td></td>
        <td>ad es. 5.5, 10.1</td>
    </tr>
    <tr>
        <td>connectionIp</td>
        <td>L’IP del dispositivo utilizzato per inviare richieste HTTP.</td>
        <td></td>
        <td>es. 8.8.4.4</td>
    </tr>
    <tr>
        <td>connectionPort</td>
        <td>Porta del dispositivo utilizzata per l’invio di richieste HTTP.</td>
        <td></td>
        <td>ad es. 53124</td>
    </tr>
    <tr>
        <td>connectionType</td>
        <td>Tipo di connessione di rete.</td>
        <td></td>
        <td>ad esempio WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td>connectionSecure</td>
        <td>Stato di protezione della connessione di rete.</td>
        <td></td>
        <td>
            I valori sono limitati:
            <ul>
                <li>true - nel caso di una rete sicura</li>
                <li>false - nel caso di un hotspot pubblico</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>applicationId</td>
        <td>L’identificatore univoco dell’applicazione.</td>
        <td></td>
        <td>es. CNN</td>
    </tr>
</table>


## Esempi {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```
