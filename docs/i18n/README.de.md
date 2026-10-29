# TheNewMiner

TheNewMiner ist ein GPU-Miner für **TensorCash** auf NVIDIA GeForce RTX Karten der Serien 30, 40 und 50 mit 10 GB VRAM oder mehr; der Pool **`tsc-wss.the-new.win`** ist fest eingebaut.

[English](../../README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · **Deutsch** · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Diese Übersetzung dient nur der Bequemlichkeit; maßgeblich ist das englische README. *(This translation is provided for convenience; the English README is authoritative.)*

## Download

Aktuelle Version: **v0.1.47** · Pool-Website: [tsc.the-new.win](https://tsc.the-new.win/#/home)

| System | Download | Danach |
|---|---|---|
| Linux x86_64 | [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | oder die Installation mit einem Klick unten verwenden |
| Windows 10 / 11 | [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | entpacken, `TheNewMiner.bat` doppelklicken |
| HiveOS (experimentell) | [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) | das Flight Sheet importieren (siehe HiveOS unten) |

Prüfsummen: [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS). Wie Sie sie prüfen, steht in
der [Anleitung](GUIDE.de.md). Wenn eine Datei nicht übereinstimmt, verwenden Sie sie nicht.

## Bevor Sie beginnen: Pool-Konto anlegen

Zum Minen brauchen Sie ein Pool-Konto: Ihre TSC-Auszahlungsadresse (Wallet) und ein Pool-Passwort, das Sie auf der
Pool-Website wählen. Die Website hat eine englische Version (über ihr Sprachmenü).

1. Öffnen Sie [tsc.the-new.win](https://tsc.the-new.win/#/home) und gehen Sie zu **Start mining** (开始挖矿).
2. Geben Sie unter **1. Bind Wallet** Ihre **TSC payout address** ein (`tc1...`; prüfen Sie sie sorgfältig:
   Mining-Einnahmen, die an eine falsche Adresse gehen, lassen sich nicht zurückholen), wählen Sie ein **Pool Password**
   (6–128 Zeichen) und geben Sie es unter **Confirm Password** erneut ein.
3. Klicken Sie auf **Apply to join pool**. Die Seite bestätigt, dass die Wallet gebunden ist; das Konto funktioniert
   sofort.
4. Verwenden Sie dieselbe Wallet und dasselbe Passwort im Miner: `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'` unter
   Linux, das Kontofenster von `TheNewMiner.bat` unter Windows oder das Flight Sheet unter HiveOS. Etwa 3–5 Minuten nach
   dem Start des Minings erscheint Ihr Rig unter **My Miners** auf der Pool-Website.

Der Pool kann ein vergessenes Pool-Passwort nicht wiederherstellen, und die Auszahlungsadresse lässt sich später nicht
ändern. Das Pool-Passwort ist nicht der private Schlüssel Ihrer Wallet: Geben Sie Ihren privaten Schlüssel niemals an
jemanden weiter.

## Installation mit einem Klick (Linux)

Ersetzen Sie `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` und `<RIG_NAME>` (einschließlich der `<` `>`) am Ende dieses
Befehls durch Ihre Wallet, Ihr Pool-Passwort und einen Namen für dieses Rig, und fügen Sie ihn in ein Terminal ein. Er
lädt das Linux-Paket und `SHA256SUMS` herunter, prüft den SHA-256, entpackt den Miner nach `~/thenewminer` und startet
ihn im Hintergrund:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.47 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Ein Downloadfehler oder ein nicht übereinstimmender SHA-256 startet niemals etwas. Behalten Sie die einfachen
Anführungszeichen um das Passwort bei (ein `'` im Passwort wird als `'\''` geschrieben). Auf einem Rechner mit einer
einzigen GPU prüft der Miner Wallet und Passwort beim Pool, bevor er das Modell herunterlädt (etwa 8.1 GiB, einmalig).
Danach zeigt `./thenewminer --logs`, wie er mint, und `./thenewminer --status` sagt, ob er läuft oder warum er gestoppt
hat. Für ein Upgrade führen Sie zuerst `~/thenewminer/thenewminer --stop` aus, dann denselben Befehl: Ihre Einstellungen
und das Modell bleiben erhalten. Alles Weitere (tägliche Befehle, Upgrades, Autostart, was zu tun ist, wenn er nicht
mint) steht in der [Anleitung](GUIDE.de.md).

## Windows

1. Entpacken Sie die gesamte `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` (Rechtsklick › Alle extrahieren...) an einen
   beliebigen Ort.
2. Doppelklicken Sie auf **`TheNewMiner.bat`**. Beim ersten Mal fragt EIN Fenster nach Ihrem Pool-Konto (Wallet,
   Pool-Passwort, Worker-Name); danach wird nichts mehr gefragt: Es richtet WSL2 und Ubuntu ein, installiert den
   mitgelieferten Miner, lädt das Modell herunter und beginnt zu minen. Spätere Doppelklicks starten das Mining sofort.
   Windows meldet möglicherweise zuerst „Der Computer wurde durch Windows geschützt“ (das Kit ist nicht codesigniert):
   Klicken Sie auf **Weitere Informationen** und dann auf **Trotzdem ausführen**. Das Einschalten von WSL fragt einmal
   nach Administratorrechten und kann einen Neustart erfordern; danach läuft die Einrichtung von selbst weiter.
3. Eine frühere Version wird gefunden und an Ort und Stelle aktualisiert, mit Ihren Einstellungen und dem Modell.

Wenn es stoppt oder langsam mint, siehe [Windows-Fehlerbehebung](WINDOWS-TROUBLESHOOTING.de.md) (DNS,
Windows-Laufwerke, der VRAM, den Browser und Docker Desktop belegen, die Uhr).

## HiveOS (experimentell)

1. **Wallets:** Fügen Sie eine Wallet mit dem Coin `TSC` und Ihrer TensorCash-Adresse (`tc1...`) hinzu.
2. Öffnen Sie [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json), kopieren Sie den
   Inhalt und ersetzen Sie `<YOUR_WALLET>` (behalten Sie `.%WORKER_NAME%` dahinter bei) und `<YOUR_POOL_PASSWORD>`.
   Wählen Sie in HiveOS Flight Sheets › **Import from clipboard**, fügen Sie den Inhalt ein, wählen Sie Ihre
   `TSC`-Wallet aus und speichern Sie.
3. Wenden Sie das Flight Sheet auf Ihr Rig an. Es installiert `thenewminer-0.1.47.tar.gz` und beginnt mit dem Mining.

Manuelle Einrichtung, Upgrades und ein Rig, das GitHub nicht erreichen kann: siehe die [Anleitung](GUIDE.de.md).

## Neu in v0.1.47

- **Einfachere Installation:** Auf einem Rechner mit einer einzigen GPU prüft der Miner Ihre Wallet und Ihr
  Pool-Passwort beim Pool **vor** dem Download des 8.1 GiB großen Modells, sodass ein Tippfehler in Sekunden auffällt.
  Eine Wallet-Adresse mit einem Tippfehler (ihre Prüfsumme stimmt nicht) oder die Beispieladresse wird abgelehnt, mit
  einer Angabe, was falsch ist.
- **Windows: Zugangsdaten werden einmal abgefragt,** in einem Fenster gleich zu Beginn, vor allem, was lange dauert.
  Eine Installation einer früheren Version, auch in einer anderen WSL-Distribution, wird gefunden und **an Ort und
  Stelle aktualisiert** (Einstellungen und Modell bleiben erhalten). Wenn Ubuntu nicht von Microsoft heruntergeladen
  werden kann, holt sich die Einrichtung Ubuntus eigenes WSL-Image von einem Mirror in China und verwendet es nur, wenn
  sein SHA-256 übereinstimmt.
- **Kein apt auf einem Standard-Ubuntu:** Ubuntu 22.04 / 24.04 (auch das von WSL) hat bereits alles, was der Miner
  braucht, also wird nichts installiert. Wo etwas fehlt, listet der erste Start es vor der Installation auf.
- **Ein Paket auf einem Windows-Laufwerk** (etwa `/mnt/d` in WSL) wird sofort abgelehnt, mit einer fertigen Zeile zum
  Einfügen, die es in Ihren Linux-Home-Ordner kopiert und dort startet.
- **Klare Netzwerkmeldungen:** Wenn DNS in WSL nicht funktioniert, stoppt die Einrichtung bei diesem Schritt und nennt
  die Lösung; ein Miner, der das Internet oder den Pool nicht erreicht, sagt, was zu prüfen ist.
- **Das Modell wird wiederverwendet:** Ein Upgrade in einen neuen Ordner findet das Modell einer früheren Installation
  (gleiche Größe und gleicher SHA-256) und lädt es nicht erneut herunter.
- **`--status` nennt den Grund,** warum der Miner gestoppt hat und wie man es behebt, auch nach einem Start mit
  `--background`.
- **HiveOS:** Jeder Fehler wartet jetzt und versucht es mit wachsenden Pausen (eine bis zehn Minuten) erneut und zeigt
  seinen Grund im Log und im Dashboard.

Vollständige Hinweise: [`docs/releases/RELEASE-NOTES-v0.1.47.md`](../releases/RELEASE-NOTES-v0.1.47.md).

## Bekannte Probleme

- **10-GB-Karten unter Windows brauchen für volle Geschwindigkeit etwa 9.3 GB freien VRAM.** Mit weniger verwendet der
  Miner ein langsameres Layout (eine RTX 3080 10 GB fiel von 21–22 auf etwa 9 N/s). Ein Browser mit
  Hardwarebeschleunigung oder Docker Desktop kann diesen Speicher belegen: Schließen Sie sie (oder schalten Sie die
  Hardwarebeschleunigung des Browsers aus) und starten Sie das Mining dann erneut.
- **Windows-Uhr:** Wenn der Miner einen Uhrzeitfehler meldet, öffnen Sie Windows-Einstellungen › Zeit und Sprache ›
  Datum und Uhrzeit, schalten Sie „Zeitzone automatisch festlegen“ und „Uhrzeit automatisch festlegen“ ein und klicken
  Sie auf **Jetzt synchronisieren**, führen Sie dann `wsl --shutdown` aus und starten Sie erneut.

## Hashrates

Die besten Raten, gemessen auf einem echten Pool mit unseren eigenen Rigs. Es sind **Bis-zu**-Werte, kein Versprechen:
Karten desselben Modells unterscheiden sich um etwa 5–8%, und die Rate sinkt, wenn die Karte zusätzlich einen Bildschirm
ansteuert oder andere Programme die GPU nutzen.

| GPU | Hashrate |
|---|---|
| RTX 5090 | bis zu ~99 N/s |
| RTX 4090 | bis zu ~70 N/s |
| RTX 3080 10 GB | bis zu ~22 N/s |

Datierte Messungen pro Version: [Testergebnisse](../TEST-RESULTS.md). Die Meilensteine des Projekts:
[Entwicklungsprotokoll](../DEVELOPMENT-RECORD.md).

## Problem melden

Eröffnen Sie ein Issue unter [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) und füllen Sie
das Fehlerberichtsformular aus: die TheNewMiner-Version, Ihr System, GPU-Modell(e) und Treiber, die Ausgabe von
`./thenewminer --status` und die letzten Logzeilen. Issues sind öffentlich: **Posten Sie niemals Ihr Pool-Passwort oder
den privaten Schlüssel Ihrer Wallet**, und entfernen Sie das Passwort (und, wenn Sie möchten, Ihre Wallet-Adresse) aus
allem, was Sie einfügen.

## Sicherheitshinweis

Der Miner und die GPU-Engine werden verschlüsselt (ChaCha20-Poly1305) im Paket ausgeliefert; der Miner entschlüsselt sie
beim Start auf Ihrem Rechner. Das erhöht den Aufwand, sie zu kopieren, ist aber **nicht unknackbar**: Jeder mit
Administratorzugriff auf einen Rechner, auf dem der Miner läuft, kann mit genügend Aufwand den Schlüssel oder den
entschlüsselten Code gewinnen. Ihre Wallet-Adresse und Ihr Pool-Passwort werden auf Ihrem eigenen Rechner gespeichert und
nur an den Pool gesendet.
