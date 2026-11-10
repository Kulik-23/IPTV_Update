# StreamBox

Player nativo .NET MAUI per Android, Android TV e Windows. Supporta playlist M3U/M3U8 e Xtream Codes, raggruppa i contenuti per categoria e permette ricerca, preferiti, cronologia e gestione delle playlist.

## Versione

Release corrente Android: **1.3.5**. Le informazioni della release sono in `update.json`; aggiornalo a ogni nuova release insieme a `ApplicationDisplayVersion`, `ApplicationVersion` (numero build Android), `Version`, `FileVersion` e `InformationalVersion` nel progetto MAUI. Ricostruisci il pacchetto della piattaforma interessata; usa il numero di release nei nomi dei file. Su Android l'app controlla automaticamente il manifest GitHub all'avvio, verifica l'hash SHA-256 e scarica le release più recenti. L'installazione richiede la conferma del programma d'installazione Android e l'autorizzazione per installare app da questa sorgente. Per aggiornare, installa la nuova APK sopra la versione precedente senza disinstallarla; poi carica APK e `update.json` nella radice di `Kulik-23/IPTV_Update`.

## Funzioni

- Importazione di playlist M3U/M3U8 da file o URL.
- Accesso Xtream Codes con categorie separate per TV, film e serie; selezione di stagione ed episodio.
- Aggiornamento e rimozione delle playlist, ricerca, preferiti e cronologia.
- Interfaccia utilizzabile con touch, tastiera e telecomando Android TV.
- Password Xtream conservate tramite SecureStorage della piattaforma.
- Librerie estese caricate nella schermata a pagine; archivio dati su file per evitare i limiti delle Preferenze.
- Errori imprevisti registrati localmente e mostrati all'avvio successivo, con dettagli tecnici per la diagnosi.
- Riproduzione Android tramite Media3/ExoPlayer 1.11.1, con supporto ai flussi HLS, fallback del decoder e controlli audio. Android 6.0 (API 23) o successivo è richiesto dalle librerie AndroidX correnti.
- Il player si apre in verticale; il pulsante **Schermo intero** ruota il video in orizzontale e nasconde le barre di sistema.
- L'aggiornamento Android controlla `update.json` all'avvio; il manifest deve contenere `app`, `platform`, `packageId`, `version`, `build`, `apk`, `sha256` e `releaseNotes`. Pubblica l'APK all'URL indicato, con la stessa firma della versione installata.
- Le sezioni Xtream per TV, film e serie vengono caricate separatamente; un errore in una categoria non nasconde le altre.

## Prerequisiti

.NET 10 SDK, i workload `maui-android` e `maui-windows`, JDK 17 e Android SDK. Android SDK e JDK sono installati in `%LOCALAPPDATA%` e configurati tramite `ANDROID_HOME`, `ANDROID_SDK_ROOT` e `JAVA_HOME`.

## Compilazione

Da PowerShell, nella cartella del progetto:

```powershell
dotnet build native\StreamBox.Native\StreamBox.Native.csproj --framework net10.0-windows10.0.19041.0
dotnet publish native\StreamBox.Native\StreamBox.Native.csproj --framework net10.0-android --configuration Release -p:AndroidPackageFormats=apk
```

Per creare il pacchetto Windows eseguibile:

```powershell
dotnet publish native\StreamBox.Native\StreamBox.Native.csproj --framework net10.0-windows10.0.19041.0 --configuration Release -p:PublishReadyToRun=false
```

Il pacchetto Android include il launcher e il banner Android TV, oltre al supporto ai telecomandi. Il pacchetto Windows è un'app MAUI non MSIX e richiede il runtime WebView2.

## Installazione su Amazon Fire TV / Firestick

La Fire TV e il PC devono essere sulla stessa rete. Attiva **Opzioni sviluppatore > Debug ADB** sulla Fire TV e individua il suo indirizzo IP. Da Windows, con PowerShell e Android Platform Tools installati, scarica/ospita prima l'APK firmato su un indirizzo HTTPS raggiungibile dal dispositivo. Quindi esegui dalla cartella del progetto:

```powershell
.\scripts\install-firestick.ps1 -ApkUrl "https://TUO-SITO/StreamBox-Android-1.2.0.apk" -Device "192.168.1.50:5555" -Launch
```

Lo script scarica l'APK sul PC, si collega alla Fire TV con ADB, installa o aggiorna StreamBox e, con `-Launch`, apre l'app. Sostituisci URL e indirizzo IP con i tuoi valori. Alla prima connessione conferma la richiesta di debug sulla TV. Sulle Fire TV con **Debug wireless** che richiedono l'associazione, completa prima l'associazione ADB mostrata nelle impostazioni della TV; questo script usa il dispositivo già raggiungibile via ADB. Non è possibile fornire un URL di download pubblico finché l'APK non viene caricato su un sito o server accessibile.

## Note

StreamBox non fornisce contenuti: aggiungi soltanto playlist e servizi che sei autorizzato a utilizzare. La riproduzione dipende dal server e dal formato; HLS/M3U8 è supportato, mentre i flussi MPEG-TS grezzi non sono riproducibili dal player web integrato. Le password Xtream sono conservate tramite SecureStorage; gli URL delle playlist restano nella libreria locale del dispositivo.
