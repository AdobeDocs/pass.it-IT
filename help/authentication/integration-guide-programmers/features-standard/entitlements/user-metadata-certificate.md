---
title: Certificato metadati utente per la crittografia
description: Certificato metadati utente per la crittografia
exl-id: 6f5d9a31-945e-418b-a9df-985bdbf29dff
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# Certificato metadati utente per la crittografia

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente da Adobe. Non è consentito alcun uso non autorizzato.

Per l’integrazione dell’autenticazione Adobe Pass con metadati utente crittografati è necessaria una coppia di chiavi pubblica/privata.

Questo documento descrive un processo per la generazione di certificati a chiave pubblica da utilizzare nell’autenticazione di Adobe Pass. Il processo qui descritto utilizza il toolkit OpenSSL.

## Procedura dettagliata per la generazione di certificati (#generation)

1. Scarica e installa il toolkit OpenSSL (http://www.openssl.org).

1. Generare una richiesta di firma del certificato (CSR, Certificate Signing Request):

   * Genera una coppia di chiavi.  Aprire una finestra di comando/terminale ed eseguire il comando seguente:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Genera la CSR. Sulla riga di comando eseguire le operazioni seguenti:

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     Verrà richiesto di immettere la password per la chiave privata.

   * Crea una copia di backup della chiave privata e della password. CSR di esempio:

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. Invia la CSR a un’autorità di certificazione (CA) (ad esempio, Verisign).

1. La CA ti invierà il certificato in formato .p7b (PKCS#7, Cryptographic Message Syntax Standard)

1. Distribuisci il certificato .p7b. Converti il file PKCS#7 (.p7b) in un file PKCS#12 (file PFX, Personal Information Exchange Syntax Standard) utilizzando la tua chiave privata e genera il file PEM (file contenitore certificato concatenato):

   * Convertire il file PKCS#7 in un file PEM temporaneo. Sulla riga di comando eseguire le operazioni seguenti:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Convertire il file PEM temporaneo in un file PFX.  Sulla riga di comando eseguire le operazioni seguenti:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Convertire il file PEM temporaneo in un file PEM finale. Sulla riga di comando eseguire le operazioni seguenti:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Invia il file PEM finale ad Adobe per configurarlo.

   * La persona che deve ricevere il file PEM è l’ingegnere di abilitazione dell’Adobe assegnato alla tua integrazione/convalida. Se non lavori direttamente con quella persona, puoi scoprire a chi inviare il file dal tuo rappresentante Adobe.
   * Adobe supporta sia un certificato primario che un certificato di backup. Se il certificato principale viene compromesso, puoi revocarlo e passare al certificato secondario per firmare l’ID richiedente nell’app. In questo modo la transizione tra i certificati in produzione sarà fluida e avrà un impatto minimo sui clienti.
   * Adobe Una volta ricevuto il file PEM, i tecnici dell’autenticazione lo aggiungeranno alla configurazione sul lato server e confermeranno la ricezione del file.
