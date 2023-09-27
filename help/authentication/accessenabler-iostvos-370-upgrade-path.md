---
title: Percorso di aggiornamento ad AccessEnabler iOS/tvOS 3.7.0
description: Percorso di aggiornamento ad AccessEnabler iOS/tvOS 3.7.0
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---

# Percorso di aggiornamento ad AccessEnabler iOS/tvOS 3.7.0 {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>Il contenuto di questa pagina viene fornito solo a scopo informativo. L’utilizzo di questa API richiede una licenza corrente di Adobe. Non è consentito alcun uso non autorizzato.

</br>

La memorizzazione dei portachiavi cambia da [nuova versione di AccessEnabler 3.7.0](/help/authentication/authn-rn-ios-tvos-370.md) non sono compatibili con l’implementazione dell’archiviazione Keychain dalla versione di AccessEnabler precedente alla 3.7.0.

Il percorso di aggiornamento di un&#39;applicazione che adotta la nuova versione di AccessEnabler 3.7.0 eseguirà la migrazione di tutti i token dalle versioni precedenti dell&#39;archiviazione Keychain. Pertanto, gli utenti finali **non deve verificarsi la perdita di sessioni di autenticazione/autorizzazione** durante il processo di aggiornamento del framework AccessEnabler.

## Limitazioni note

Alcune limitazioni, descritte di seguito, potrebbero essere riscontrate dagli implementatori.


1. L&#39;SSO regolare (di Adobe) non funziona tra un&#39;applicazione che utilizza AccessEnabler versione 3.7.0 e un&#39;applicazione che utilizza AccessEnabler versione/i inferiore/i alla 3.7.0, anche per le applicazioni sviluppate dallo stesso fornitore.

   - **Importante:**
      - L’SSO a livello di sistema (Apple) non subirà modifiche.
      - L&#39;SSO regolare (di Adobe) continuerà a funzionare se entrambe le applicazioni sono sviluppate dallo stesso fornitore e utilizzano una o più versioni di AccessEnabler inferiori alla 3.7.0.
      - L&#39;SSO regolare (di Adobe) funziona se entrambe le applicazioni sono sviluppate dallo stesso fornitore e utilizzano AccessEnabler versione 3.7.0.

1. Se si esegue il downgrade di un&#39;applicazione che utilizza AccessEnabler versione 3.7.0 a una versione inferiore di AccessEnabler, la migrazione dei nuovi token generati non verrà eseguita. Di conseguenza, gli utenti finali potrebbero andare incontro alla perdita di sessioni di autenticazione/autorizzazione, senza aspettarselo.

   - **Importante:**
      - Gli utenti finali autenticati tramite l’SSO a livello di sistema (Apple) non subiranno modifiche.
      - Gli utenti finali già autenticati prima dell&#39;aggiornamento alla nuova applicazione tramite AccessEnabler versione 3.7.0 non subiranno modifiche.

1. Se si esegue il downgrade di un&#39;applicazione che utilizza AccessEnabler versione 3.7.0 a una versione inferiore di AccessEnabler, i token eliminati non verranno riconosciuti. Pertanto, gli utenti finali potrebbero rilevare la presenza di sessioni di autenticazione/autorizzazione, senza che ciò sia previsto.
