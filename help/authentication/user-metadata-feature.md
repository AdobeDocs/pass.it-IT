---
title: Funzione metadati utente
description: Funzione metadati utente
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1649'
ht-degree: 0%

---

# Metadati utente {#user-metadata}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>
</br>

## Introduzione {#intro}

La funzione User Metadata consente ai programmatori di accedere a diversi tipi di dati specifici dell&#39;utente gestiti da MVPD.  I tipi di metadati utente includono codici postali, valutazioni dei genitori, ID utente e altro ancora.  *Utente* i metadati sono un’estensione del file precedentemente disponibile *statico* metadati (TTL del token di autenticazione, TTL del token di autorizzazione e ID dispositivo).


Punti chiave metadati utente:

- Trasmesso all’applicazione del programmatore durante i flussi di autenticazione e autorizzazione
- I valori vengono salvati nel token
- I valori possono essere normalizzati se diversi MVPD forniscono dati in formati diversi
- Alcuni parametri possono essere crittografati utilizzando la chiave del programmatore (ad esempio il codice postale)
- Valori specifici sono resi disponibili da Adobe tramite una modifica alla configurazione

## Ottenimento dei metadati utente {#obtaining}

I metadati utente sono disponibili per i programmatori tramite AccessEnabler `getMetadata()` e tramite il `/usermetadata` nell’API senza client.  Per informazioni dettagliate sull’utilizzo di, consulta la documentazione dell’API della piattaforma. `getMetadata()` e il callback corrispondente `setMetadataStatus()` (o per gli endpoint e i parametri utilizzati nell’API senza client).

I programmatori ottengono i metadati fornendo una chiave per il tipo di metadati che desiderano ottenere: `getMetadata(key)`.

I metadati vengono restituiti come segue: `setMetadataStatus(key, encrypted, data)`:

| Parametro | Tipo | Descrizione |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | Stringa | Specifica il tipo di metadati richiesto |
| `encrypted` | Booleano | Flag booleano che indica se il &quot;valore&quot; è crittografato o meno. Se questo è &quot;true&quot;, &quot;value&quot; sarà in realtà una rappresentazione JSON Web Encrypted del valore effettivo. |
| `data` | Oggetto | Oggetto JSON contenente la rappresentazione dei metadati |



La struttura del parametro dati, i valori variano tra i tipi:

| Chiave | Tipo di valore | Esempio | Descrizione |
| --- | --- | --- | --- |
| `zip` | Array JSON | \[&quot;77754&quot;, &quot;12345&quot;\] | Codice postale |
| `householdID` | Stringa JSON | &quot;1o7241p&quot; | Identificatore della famiglia. Se MVPD non supporta i conti secondari, questo sarà identico a `userID` |
| `maxRating` | Oggetto JSON | { MPAA: &quot;NR&quot;, <br>  VCHIP: &quot;X&quot;,  <br>  URL: &quot;http://manage.my/parental&quot; } | Classificazione genitoriale massima per l’utente |
| `userID` | Stringa JSON | &quot;1o7241p&quot; | L’identificatore utente. Nel caso in cui un MVPD supporti conti secondari e l&#39;utente non sia il conto principale, `userID` sarà diverso da `householdID`. |
| `channelID` | Array JSON | \[&quot;channel-1&quot;, &quot;channel-2&quot; \] | Elenco di canali che un utente può visualizzare |
| `is_hoh` | Stringa JSON | &quot;1&quot; | Un flag che identifica se un utente è a capo della famiglia |
| `encryptedZip` | Stringa JSON | &quot;&quot; | Comcast espone il codice postale crittografato |
| `typeID` | Stringa JSON | Principale | Un flag che identifica se l’account utente è primario/secondario |
| `primaryOID` | Stringa JSON | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Identificatore della famiglia. Se `typeID` è Primario, conterrà il valore del `userID` |
| hba_status | Booleano | &quot;true&quot; &quot;false&quot; | Flag booleano che indica se l’autenticazione basata sulla home page è abilitata per una particolare integrazione |
| allowMirroring | Booleano | &quot;true&quot; &quot;false&quot; | Indica se il mirroring dello schermo è consentito o meno per questo dispositivo |

>[!NOTE]
>
> **Nota:** Se il parametro di dati è crittografato, come in genere avviene per **tasto zip**, quindi la rappresentazione della chiave dei metadati sarà una stringa JSON invece di un array o un oggetto.


**Importante:** I metadati utente effettivi disponibili per un programmatore dipendono da ciò che viene reso disponibile da un MVPD.  Prima di rendere disponibili nell&#39;ambiente di produzione i metadati sensibili degli utenti (come zipcode), è necessario firmare accordi legali con gli MVPD.

</br>


| Nome | Dettagli | Richiede crittografia | Commenti |
| --- | --- | --- | --- |
| ID utente | Come previsto dall&#39;MVPD | No | Si tratta del valore con hash eseguito dall&#39;Adobe ed esposto nel token multimediale e nel callback sendTrackingData().<br><br>L&#39;hashing in questo caso è stato fatto per ragioni storiche<br><br>Questo ID potrebbe essere un ID famiglia o un ID account secondario. Generalmente non è specificato, ma è solo l’ID associato all’accesso utilizzato in quel momento (che può essere un account principale o secondario) |
| ID utente upstream | Previsto dall&#39;MVPD da utilizzare solo per i flussi di monitoraggio della concorrenza | No | Questo valore viene utilizzato quando si applicano limiti di concorrenza tra siti e app MVPD e Programmatori. <br><br>L’ID può contenere anche criteri di monitoraggio della concorrenza<br><br>Per la maggior parte degli MVPD questo valore è uguale all’ID utente |
| ID utente domestico | Fornito dall&#39;MVPD per essere utilizzato principalmente per i flussi di controllo parentale | No | ID che consente ai programmatori di comprendere l’utilizzo degli utenti domestici rispetto a quello degli account secondari.<br><br>A volte viene utilizzato come sostituto del Controllo genitori se le valutazioni vere non sono disponibili (se l’utente ha effettuato l’accesso con l’account domestico che può guardare, altrimenti il contenuto classificato non verrebbe visualizzato)<br><br>Il modo in cui viene rappresentato dagli MVPD varia notevolmente: ID utente della famiglia, ID del responsabile della famiglia, flag del responsabile della famiglia, ecc. |
| Capo famiglia | Segnalazione di flag se il conto corrente è a capo della famiglia o no | No | vedi sopra |
| ID tipo/ID primario | Identificatori dei conti delle famiglie | No | Indicatori specifici AT&amp;T per capo famiglia.<br><br>ID tipo = Flag che identifica se l’account utente è primario/secondario<br><br>OID primario = identificatore della famiglia. Se TypeID è Primary, conterrà il valore di userID |
| Valutazione massima | Valutazione massima consentita per l’account corrente | No | Consente ai programmatori di filtrare i contenuti non adatti all&#39;account.<br><br>Ha valutazioni MPAA o VCHIP |
| Line-up dei canali | Elenco dei canali disponibili nel pacchetto dell’utente | No | Utilizzato per consentire/rimuovere rapidamente vari canali dai portali che aggregano più reti</br></br> *L&#39;autorizzazione di verifica preliminare consente una maggiore flessibilità per questo caso d&#39;uso e deve essere utilizzata in alternativa* <br><br>La specifica OLCA lo consente come AttributeStatement nella risposta AuthN |
| Stato HBA | Indica se si è verificata l&#39;autenticazione tramite HBA | No |     |
| Codice postale | CAP di fatturazione dell&#39;utente | Sì | Utilizzato per la trasmissione o lo sport<br><br>Può essere fornito anche con la risposta AuthZ per aggiornamenti rapidi<br><br>Dati sensibili, richiede accordi legali MVPD |
| Codice postale crittografato | Codice postale di fatturazione dell’utente (Comcast) | Sì | Come sopra ma crittografato da Comcast |
| Lingua | Impostazioni lingua utente | No | Utilizzato per visualizzare i messaggi in base alle preferenze dell’utente |
| Consenti mirroring | Indica se il mirroring dello schermo è consentito o meno per questo dispositivo | No |     |



## Metadati disponibili {#available_metadata}

La tabella seguente illustra lo stato corrente dei metadati dell’utente nell’ecosistema di autenticazione di Adobe Pass:


|     | **Note legali **<br><br>**Contratto **<br><br>**Firmato (solo zip)** | **ID utente **<br><br>**su AuthN** | **Codice postale **<br><br>**su AuthN/Z** | **Valutazione **<br><br>**su AuthN/Z** | **Famiglia **<br><br>**ID su AuthN/Z** | **ID canale su AuthN** | **Capo famiglia su AuthN** | **ID tipo su AuthN** | **OID primario su AuthN** | Lingua | UserID upstream **su AuthN** | Stato HBA | OnNet | inHome | Consenti mirroring in AuthZ | **Note** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Nome formale** | n/d | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | lingua | upstreamUserID | hba_status | onNet | inHome | allowMirroring | 1. Per AuthN: è necessario modificare OiosamlMetadataParser in modo che il nuovo attributo sia abilitato per tutti i parser <br>2.  Per AuthZ: non esiste una modalità generica, perché l’implementazione dell’autorizzazione è specifica per MVPD |
| **Crittografia richiesta** | n/d | **No** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** |     |
| **Sensibile** | n/d | **No** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** |     |     |
| **IdP Adobe** | **Sì** | **Sì** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **Sì** | **Sì** | **Sì** | **Sì** | **No** | **Sì** | **No** | **No** | **No** | **No** | Non è necessario alcun accordo legale, è possibile abilitarlo. |
| **Synacor** | **Sì** | **Sì** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **Sì** | **Sì** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** | **Accordo legale che non copre tutti gli MVPD proxy.**   <br>Si tratta di un supporto generico per Synacor, che probabilmente non include tutti i MVPD. |
| Piatto | **No** | **Sì** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **Sì** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** | Condivide lo stesso elenco di tutti gli MVPD Synacor più upstreamUserID. |
| Comcast | **No** | **Sì** | **No** | **Sì (solo AuthZ)** | **Sì (solo AuthZ)** | **No** | **No** | **No** | **No** | **No** | **Sì** | **Sì** | **No** | **No** | **No** |     |
| **AT&amp;T** | **Sì** | **Sì** | **Sì (solo autenticazione)** | **No** | **Sì (solo autenticazione)** | **No** | **No** | **Sì** | **Sì** | **No** | **Sì** | **No** | **No** | **No** | **No** | Accordo legale firmato. |
| **Cablevision** | **Sì** | **Sì** | **Sì (solo autenticazione)** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** | Accordo legale firmato. |
| **HTC** | **No** | **Sì** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** |     |
| **Massilon proxy** | **Sì** | **Sì** | **Sì (solo autenticazione)** | **No** | **Sì (solo autenticazione)** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** | Accordo legale firmato. |
| **Clearleap proxy** | **Sì** | **Sì** | **Sì (solo autenticazione)** | **Sì (solo AuthZ)** | **No** | **No** | **No** | **No** | **No** | **Sì** | **Sì** | **No** | **No** | **No** | **No** | Accordo legale firmato. |
| Rogers | **No** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** |     |
| RCN | **Sì** | **Sì** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** |     |
| Carta | **Sì** | **Sì** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** |     |
| Verizon | **No** | **Sì** | **Sì (solo autenticazione)** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì** | **Sì** | **No** | **No** | **No** |     |
| Eastlink | **No** | **Sì** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **Sì** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** |     |
| GLD proxy | **No** | **Sì** | **Sì (solo autenticazione)** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** |     |
| DTV | **Sì** | **Sì** | **Sì (solo autenticazione)** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** |     |
| COX | **No** | **Sì** | **Sì (solo autenticazione)** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** |     |
| Cogeco | **No** | **Sì** | **Sì (solo autenticazione)** | **No** | **Sì (solo autenticazione)** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** |     |
| Videotron | **No** | **Sì** | **Sì (solo autenticazione)** | **No** | **Sì*** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** | Espone familyID con lo stesso valore di userID |
| Spettro | **Sì** | **Sì** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **Sì (solo autenticazione)** | **No** | **No** | **No** | **No** | **No** | **Sì** | **Sì** | **No** | **No** | **Sì** |     |
| **Tutti gli altri **<br><br>**MVPD** | **No** | **Sì** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sì** | **No** | **No** | **No** | **No** | **Ancora nessun accordo legale, metadati sensibili non disponibili per la produzione.**  <br>Per tutti gli MVPD, l&#39;ID utente è disponibile senza alcun lavoro aggiuntivo. |


L’elenco dei tipi di metadati utente verrà espanso man mano che nuovi tipi saranno disponibili e aggiunti al sistema di autenticazione di Adobe Pass.

## Esempi di codice {#code_samples}

- [Esempio di codice 1](#code_sample1)
- [Esempio di codice 2 (Mock getMetadata)](#code_sample2)


### Esempio di codice 1 {#code_sample1}

```
    // Assuming a reference to the AccessEnabler has been previously obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if(status ==1) {
        // User is authenticated, request metadata
        ae.getMetadata("zip");
        ae.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if(encrypted) {
        // The metadata value is encrypted
        // Needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```


### Esempio di codice 2 (Mock getMetadata) {#code_sample2}

```
    // Assuming a reference to the AccessEnabler has been
    //   previously obtained and stored in the "ae" variable
     
    // Mock the getMetadata() method
    var aeMock = {
        getMetadata: function(key) {
          var data = null;
          // Set mock data based on the received key,
          // according to the format in the spec
          switch(key) {
            case 'zip': 
              data = [ "1235", "23456" ];
              break;
            case 'maxRating': 
              data = { "MPAA": "PG-13", "VCHIP": "TV-14" };
              break;
            default:
              break;
          }
          // Call the metadata status just like AccessEnabler does
          setMetadataStatus(key, false, data);
        }
     }
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if (status == 1) {
        // User is authenticated, request metadata using mock object
        aeMock.getMetadata("zip");
        aeMock.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the  setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if (encrypted) {
        // The metadata value is encrypted, so it
        //   needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```

<!---

For details on your particular platform, or to gain some insight into how User Metadata is processed on the MVPD side, see the appropriate link in Related Information below.  

## Related Information {#related}

- ActionScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- JavaScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- iOS - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaStatus)
- Android - [getMetadata()](#getMetadata), [setMetadataStatus()](#setMetadaStatus)
- Clientless - [AuthN Metadata](#authn_metadata)
- [MVPD Integration Guide: User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
-->
