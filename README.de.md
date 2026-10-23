[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · **Deutsch** · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Diese Übersetzung dient nur der Bequemlichkeit; maßgeblich ist das englische README. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner ist ein GPU-Miner für **TensorCash**. Er läuft auf NVIDIA GeForce RTX Karten der Serien 30, 40 und 50 mit
**10 GB VRAM oder mehr** und mint auf dem Pool **`tsc-wss.the-new.win`**, der fest eingebaut ist: Sie müssen keine
Pool-Adresse eingeben, und die Verbindung erfolgt direkt.

Aktuelle Version: **v0.1.44**

## Download

| Datei | Für | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64 (Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11 (läuft über WSL2) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | Prüfsummen beider Pakete | |

Das Windows-Zip ist ein Komplettpaket: Es enthält bereits den Linux-Miner, Sie brauchen also nur das Zip.

## Download prüfen

Legen Sie das Paket bzw. die Pakete und `SHA256SUMS` in denselben Ordner und führen Sie dann aus:

```bash
# Linux (--ignore-missing anhängen, wenn Sie nur ein Paket heruntergeladen haben)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows (Eingabeaufforderung): Ausgabe mit dem SHA-256 in der Tabelle oben vergleichen
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

Jede Datei muss `OK` melden (unter Windows: dieselben 64 Zeichen wie in der Tabelle). Wenn sie nicht übereinstimmt, verwenden Sie sie nicht.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background
```

| Befehl | Funktion |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background` | Erster Start: installiert das Nötige und mint dann im Hintergrund |
| `./thenewminer --background` | Spätere Starts: Ihre Einstellungen werden gespeichert |
| `./thenewminer` | Im Vordergrund minen |
| `./thenewminer --logs` | Den Miner beobachten (Strg+C verlässt die Ansicht; das Mining läuft weiter) |
| `./thenewminer --stop` | Mining beenden |

- `-u` ist Ihre TensorCash-Wallet-Adresse, `-p` Ihr Pool-Passwort, `-w` der auf dem Pool angezeigte Worker-Name
  (Standard: der Hostname des Rechners).
- Die Einstellungen werden in `thenewminer/.miner/` gespeichert (die Datei mit dem Passwort ist nur für Sie lesbar).
- Der erste Start lädt das Modell herunter (etwa 8.1 GiB, einmalig), installiert die GPU-Engine für Ihre Karte, führt
  einen Selbsttest auf der Karte aus und beginnt dann zu minen. Zeilen wie `[ RATE] ... N/s` bedeuten, dass gemint wird.
- Der erste Start installiert eventuell einige Ubuntu-Pakete mit `apt` (benötigt sudo oder root).
- Genau eine NVIDIA-GPU pro Rechner in dieser Version (Multi-GPU-Rigs werden noch nicht unterstützt).
- Wenn `tar` die Meldung `xz: Cannot exec` ausgibt, installieren Sie zuerst xz: `sudo apt-get install -y xz-utils`.
- Die `README.md` im Paket ist eine ältere interne Notiz; folgen Sie dieser Seite (kein `--proxy` nötig).
- **Upgrade:** `./thenewminer --stop`, das neue Paket am selben Ort entpacken (es wird nach `thenewminer/` entpackt und
  behält `thenewminer/.miner/` mit Ihren Einstellungen und dem Modell), dann wieder starten.

## Windows (ein Klick)

1. Entpacken Sie die gesamte `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` (Rechtsklick › Alle extrahieren...) an einen
   beliebigen Ort: beliebiger Ordner, beliebiges Laufwerk; Pfade mit Leerzeichen oder chinesischen Zeichen sind kein
   Problem. Nicht aus der Zip-Ansicht heraus starten.
2. Doppelklicken Sie auf **`TheNewMiner.bat`**.
   - **Beim ersten Mal:** Es prüft Windows, den Treiber und die Festplatte, aktiviert bei Bedarf WSL2 (eine
     Administrator-Abfrage; eventuell ist einmal ein Neustart nötig, danach läuft die Einrichtung von selbst weiter),
     zeigt an, wo die Daten abgelegt werden (Enter bestätigt), legt eine eigene Linux-Umgebung (WSL) an, installiert den
     mitgelieferten Miner, fragt **einmalig** nach Ihrer Wallet-Adresse (beginnt mit `tc1`), dem Pool-Passwort (zweimal
     eingeben; 6–128 Buchstaben, Ziffern, `_` oder `-`) und dem Worker-Namen (Enter = der Computername), lädt das Modell
     herunter und beginnt dann zu minen.
   - **Jedes weitere Mal:** Doppelklicken, und es mint sofort mit Ihren gespeicherten Einstellungen. Außerdem wird eine
     Desktopverknüpfung „TheNewMiner“ erstellt.

Tägliche Befehle (öffnen Sie eine Eingabeaufforderung im entpackten Ordner):

| Befehl | Funktion |
|---|---|
| `TheNewMiner.bat` | Bei Bedarf installieren, dann in diesem Fenster minen (Schließen des Fensters beendet das Mining) |
| `TheNewMiner.bat background` | Ohne Fenster minen |
| `TheNewMiner.bat logs` | Den Miner beobachten (Strg+C verlässt die Ansicht; das Mining läuft weiter) |
| `TheNewMiner.bat status` | Ob er läuft, und die letzten Hashrates |
| `TheNewMiner.bat stop` | Mining beenden |
| `TheNewMiner.bat setup` | Wallet / Passwort / Worker-Namen neu eingeben |
| `TheNewMiner.bat uninstall` | Miner, seinen Datenordner und die Verknüpfung entfernen (fragt vorher nach) |

- **Wo die Daten liegen:** Der Miner und das Modell liegen in einem Datenordner, der bei der ersten Installation gewählt
  wird (standardmäßig `TheNewMinerData` im Stammverzeichnis des Laufwerks, auf das Sie entpackt haben, z. B.
  `D:\TheNewMinerData`), nicht im entpackten Ordner. Dort werden etwa 20 GB freier Speicher benötigt.
- **Upgrade:** Die neue Version an einen beliebigen Ort entpacken und deren `TheNewMiner.bat` doppelklicken. Sie findet den
  installierten Miner, behält Ihre Einstellungen und das Modell und installiert die neue Version ohne Rückfragen. Der alte
  Ordner kann gelöscht werden.
- **Deinstallation:** `TheNewMiner.bat uninstall`.
- Die Bildschirmsprache folgt Windows (English / 简体中文 / 繁體中文). Zum Auswählen `-Lang en` (oder `zh-Hans`,
  `zh-Hant`) an einen beliebigen Befehl anhängen, z. B. `TheNewMiner.bat -Lang en`; das gilt für diesen Lauf.

## Proxy (optional)

Der Miner verbindet sich direkt mit dem Pool. Falls Ihr Netzwerk einen Proxy benötigt:

```bash
# Linux (gespeichert, bis es geändert wird)
./thenewminer --proxy <IP>[:<PORT>]            # TCP-Relay (Port 443, wenn weggelassen)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5, auch socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # HTTP-CONNECT-Proxy
./thenewminer --proxy none                     # zurück zur direkten Verbindung
```

Diese Befehle starten auch den Miner: Beenden Sie den Miner zuerst und hängen Sie `--background` an, damit er im Hintergrund bleibt.

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

TLS läuft immer Ende-zu-Ende mit dem Pool, daher kann der Proxy die Verbindung nicht mitlesen. Unter Windows ist eine
Proxy-App, die auf demselben PC läuft, von WSL aus meist nicht als `127.0.0.1` erreichbar (Standard-WSL-Netzwerk):
Verwenden Sie die LAN-IP des PCs und erlauben Sie in der App LAN-Verbindungen.

## Voraussetzungen

| Komponente | Voraussetzung |
|---|---|
| GPU | Genau eine NVIDIA GeForce RTX Karte der Serie 30 / 40 / 50, 10 GB VRAM oder mehr |
| Treiber | NVIDIA **580 oder neuer** (`nvidia-smi` muss funktionieren). Unter Windows in Windows installieren, niemals in WSL |
| Linux | x86_64 Ubuntu 22.04, 24.04 oder 26.04 (andere Distributionen werden noch nicht unterstützt) |
| Windows | Windows 10 21H2 oder neuer, oder Windows 11, 64-Bit; Virtualisierung (Intel VT-x / AMD SVM) im BIOS aktiviert |
| Festplatte | etwa 20 GB frei |
| RAM | 4 GB oder mehr |
| Modell | etwa 8.1 GiB, einmalig beim ersten Start heruntergeladen (parallel, mehrere Mirrors) und danach wiederverwendet |

## Referenz-Hashrates

Die besten Raten, gemessen auf einem echten Pool mit unseren eigenen Rigs (zwischen 2026-10-14 und 2026-10-23). Es sind
**Bis-zu**-Werte, kein Versprechen: Karten desselben Modells unterscheiden sich um etwa 5–8% je nach Chip, Kühlung,
Taktraten und Power-Limit, und die Rate sinkt, wenn die Karte zusätzlich einen Bildschirm ansteuert oder andere Programme
die GPU nutzen.

| GPU | Hashrate |
|---|---|
| RTX 5090 | bis zu ~99 N/s |
| RTX 4090 | bis zu ~70 N/s |
| RTX 3080 10 GB | bis zu ~22 N/s |

## Testergebnisse

Tests von v0.1.44 auf einem echten Pool mit unseren Test-Rigs am 2026-10-23 (eine GPU pro Rechner). A = akzeptierte Shares,
R = abgelehnte Shares.

| Datum | Karte | Betriebssystem | Paket | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | sealed-r4 | 65.58 N/s (Median, 16 min) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | sealed-r4 | 93.45 N/s (stabiles Mining, 44 min; 90.39 inklusive Job-Wechsel) | 31 / 0 (+1 stale beim Beenden) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | windows-wsl2-r5 | ~22 N/s (eigener Lauf des Betreibers) | k. A. |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | sealed-r4 | 21.04 N/s (letzte 10 min von 23 min Mining) | 3 / 0 |

RTX 3080 unter Linux: Installation 6/6 Schritte OK, keine ungültigen Fenster, sauberes Beenden. Log-Auszug:
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt).

## Screenshots

Von unseren Test-Rigs, mit früheren Versionen: v0.1.41 auf der RTX 4090 und ein früher Build auf der RTX 5090. Dies sind
Live-Werte; Ihre Zahlen hängen von der Karte ab.

**RTX 4090, v0.1.41:** 69–75 N/s pro Ablesung, meist ~70 (10-Minuten-Durchschnitt ~68), 9 Shares akzeptiert, 0 abgelehnt, ~445 W.

![TheNewMiner v0.1.41 beim Mining auf einer RTX 4090](screenshots/rtx4090.png)

**RTX 5090, früher Build:** 100.83 N/s (Live-Durchschnitt über abgeschlossene Fenster), ~575 W.

![Ein früher TheNewMiner-Build beim Mining auf einer RTX 5090](screenshots/rtx5090.png)

## FAQ

**Windows meldet „Der Computer wurde durch Windows geschützt“ / „Unbekannter Herausgeber“.**
Das Kit ist nicht codesigniert. Klicken Sie auf **Weitere Informationen** und dann auf **Trotzdem ausführen**.

**Mein Virenschutz meldet oder löscht den Miner.**
Microsoft Defender und andere Antivirenprogramme melden Kryptowährungs-Miner oft als „potenziell unerwünschte Apps“, auch
einen legitimen, den Sie bewusst ausführen, weil Schadsoftware häufig Miner versteckt. Wenn eine Datei unter Quarantäne
gestellt wurde, öffnen Sie Windows-Sicherheit › Viren- & Bedrohungsschutz › Schutzverlauf, wählen Sie den
TheNewMiner-Eintrag aus und wählen Sie **Zulassen** (oder fügen Sie den entpackten Ordner zu den Ausschlüssen hinzu). Tun
Sie das nur für ein Paket, das von dieser Seite heruntergeladen wurde und dessen SHA-256 übereinstimmt.

**Der erste Start dauert lange.**
Der erste Start lädt das ~8.1 GiB große Modell herunter und führt einen GPU-Selbsttest auf Ihrer Karte aus; unter Windows
richtet er eventuell auch WSL2 ein. Das passiert nur einmal. Spätere Starts beginnen viel schneller mit dem Mining.

**Wo sind die Logs?**
Linux: `./thenewminer --logs` oder die Datei `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat logs` (und `TheNewMiner.bat status` für die letzten Hashrates).

**HiveOS?**
Ein HiveOS-Paket kommt **demnächst**; es ist noch in der Testphase.

## Sicherheitshinweis

Der Miner und die GPU-Engine werden verschlüsselt (ChaCha20-Poly1305) im Paket ausgeliefert; der Miner entschlüsselt sie
beim Start auf Ihrem Rechner. Das erhöht den Aufwand, sie zu kopieren, ist aber **nicht unknackbar**: Jeder mit
Administratorzugriff auf einen Rechner, auf dem der Miner läuft, kann mit genügend Aufwand den Schlüssel oder den
entschlüsselten Code gewinnen.
Ihre Wallet-Adresse und Ihr Pool-Passwort werden auf Ihrem eigenen Rechner gespeichert und nur an den Pool gesendet.
