[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · **Deutsch** · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Diese Übersetzung dient nur der Bequemlichkeit; maßgeblich ist das englische README. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner ist ein GPU-Miner für **TensorCash**. Er läuft auf NVIDIA GeForce RTX Karten der Serien 30, 40 und 50 mit
**10 GB VRAM oder mehr** und mint auf dem Pool **`tsc-wss.the-new.win`**, der fest eingebaut ist: Sie müssen keine
Pool-Adresse eingeben, und die Verbindung erfolgt direkt.

Aktuelle Version: **v0.1.45**

## Neu in v0.1.45

- **Eine klare Meldung, wenn der Pool Ihre Wallet oder Ihr Passwort ablehnt.** Der Miner sagt das in einer Zeile und
  beendet sich, statt es erneut zu versuchen; `--status` zeigt den Grund. Prüfen Sie Wallet und Passwort und starten Sie
  dann erneut mit `-u` und `-p`.
- **Ihre Wallet und Ihr Passwort sind nicht mehr in `ps`** oder in den Prozesslisten anderer Benutzer sichtbar.
- **`./thenewminer --status`** zeigt, ob er mint, seit wann, die letzte Rate und die akzeptierten / abgelehnten Shares.
- **HiveOS-Unterstützung (experimentell):** ein HiveOS-Custom-Miner-Paket und ein importierbares Flight Sheet (siehe den Abschnitt HiveOS unten).
- **Sichere Upgrades:** Die neue Version startet nicht, solange die alte noch mint (sie fordert Sie auf, diese zuerst zu
  beenden); nach `--stop` installiert sie ihre Programmdateien selbst neu und behält Ihre Einstellungen und das Modell.
- **`--background` meldet nicht mehr „gestartet“, wenn der Miner sofort wieder endet**; es zeigt stattdessen den Grund.
- **Docker und kleiner gemeinsamer Speicher:** Wenn der In-Memory-Ordner zu klein ist (Dockers Standard ist 64 MiB)
  oder das Ausführen von Programmen nicht erlaubt, verwendet der Miner stattdessen `thenewminer/.miner/run/` auf der
  Festplatte und weist darauf hin.
- **Pool-Passwörter dürfen jetzt jedes sichtbare Zeichen enthalten** (auch Satzzeichen und nicht-englische Buchstaben):
  6–128 Zeichen, keine Leerzeichen.
- Mehr Linux-Systeme: Das Installationsprogramm prüft jetzt, was es braucht (glibc 2.34 oder neuer), statt nur einige
  wenige Ubuntu-Versionen zu akzeptieren.

## Download

| Datei | Für | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz) | Linux x86_64 | `6281bf489fae843551114f09ead5498d3e47af33a07e4cfdfbdc0a5d6bdbafbb` |
| [`TheNewMiner-v0.1.45-windows-wsl2-r1.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-windows-wsl2-r1.zip) | Windows 10 / 11 (läuft über WSL2) | `90732441b0dc67de84809d9bf3bda4441c0bf7cd9a52fefb95d29242ad7b15e7` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/SHA256SUMS) | Prüfsummen aller Pakete | |

Das Windows-Zip ist ein Komplettpaket: Es enthält bereits den Linux-Miner, Sie brauchen also nur das Zip.

## Download prüfen

Legen Sie das Paket bzw. die Pakete und `SHA256SUMS` in denselben Ordner und führen Sie dann aus:

```bash
# Linux (--ignore-missing: nur die heruntergeladenen Pakete werden geprüft)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Eingabeaufforderung): Ausgabe mit dem SHA-256 in der Tabelle oben vergleichen
certutil -hashfile TheNewMiner-v0.1.45-windows-wsl2-r1.zip SHA256
```

Jede Datei muss `OK` melden (unter Windows: dieselben 64 Zeichen wie in der Tabelle). Wenn sie nicht übereinstimmt, verwenden Sie sie nicht.

## Installation mit einem Klick (Linux)

Ersetzen Sie `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` und `<RIG_NAME>` (einschließlich der `<` `>`) am Ende dieses Befehls
und fügen Sie ihn dann in ein Terminal ein. Er lädt das Linux-Paket und die `SHA256SUMS` dieser Version herunter (mit
`curl`, oder mit `wget`, wenn kein `curl` vorhanden ist), prüft den SHA-256, entpackt den Miner nach `~/thenewminer` und
startet ihn im Hintergrund:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.45 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Er bricht beim ersten fehlgeschlagenen Schritt ab: Ein Downloadfehler oder ein nicht übereinstimmender SHA-256 startet
niemals etwas (die Prüfung schützt vor einem beschädigten Download; beide Dateien stammen von dieser Seite). Behalten Sie
die einfachen Anführungszeichen um das Passwort bei: Darin ist jedes Zeichen sicher (ein `'` im Passwort wird als `'\''`
geschrieben). Wenn `tar` die Meldung `xz: Cannot exec` ausgibt, installieren Sie xz (`sudo apt-get install -y xz-utils`)
und führen Sie den Befehl erneut aus.

Um eine bereits in `~/thenewminer` vorhandene Version zu aktualisieren, führen Sie zuerst `~/thenewminer/thenewminer --stop`
aus, dann diesen Befehl (Ihre Einstellungen und das Modell bleiben erhalten). Liegt Ihre frühere Version in einem anderen
Ordner, beenden Sie sie zuerst dort oder aktualisieren Sie sie dort wie unter Linux beschrieben (sonst wird das Modell
erneut heruntergeladen).

## Linux

```bash
tar -xJf TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Befehl | Funktion |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | Erster Start: installiert das Nötige und mint dann im Hintergrund |
| `./thenewminer --background` | Spätere Starts: Ihre Einstellungen bleiben gespeichert |
| `./thenewminer` | Im Vordergrund minen (Strg+C beendet) |
| `./thenewminer --logs` | Den Miner beobachten (Strg+C verlässt die Ansicht; das Mining läuft weiter) |
| `./thenewminer --status` | Ob er mint, seine letzte Rate und die akzeptierten / abgelehnten Shares |
| `./thenewminer --stop` | Mining beenden |
| `./thenewminer --help` | Alle Optionen |

- `-u` ist Ihre TensorCash-Wallet-Adresse, `-p` Ihr Pool-Passwort, `-w` der auf dem Pool angezeigte Worker-Name
  (Standard: der Hostname des Rechners; 1–64 Buchstaben, Ziffern, `_`, `-` oder `.`).
- Das Pool-Passwort: 6–128 Zeichen, jedes sichtbare Zeichen, keine Leerzeichen. Setzen Sie es auf der Befehlszeile immer
  in einfache Anführungszeichen, z. B. `-p 'my$pass!'` (ein `'` innerhalb einfacher Anführungszeichen wird als `'\''`
  geschrieben).
- Die Einstellungen werden in `thenewminer/.miner/` gespeichert (die Datei mit dem Passwort ist nur für Sie lesbar). Wallet
  und Passwort werden sofort von der Befehlszeile entfernt, sodass sie in `ps` nicht erscheinen (Ihre Shell kann den
  Befehl dennoch in ihrem Verlauf speichern).
- Wenn der Pool die Wallet oder das Passwort ablehnt (oder das Konto noch nicht freigegeben hat), meldet der Miner das und
  beendet sich. Prüfen Sie beides und starten Sie erneut mit `-u` und `-p`.
- Der erste Start lädt das Modell herunter (etwa 8.1 GiB, einmalig), installiert die GPU-Engine für Ihre Karte, führt
  einen Selbsttest auf der Karte aus und beginnt dann zu minen. Zeilen wie `[ RATE] ... N/s` bedeuten, dass gemint wird.
- Unter Ubuntu und Debian installiert der erste Start eventuell einige fehlende Systembibliotheken mit `apt` (benötigt
  sudo oder root); auf anderen Systemen nennt er sie, damit Sie sie selbst installieren.
- Multi-GPU-Rigs: unterstützt, jede GPU mint; diese Version wurde mit einer GPU pro Rechner getestet.
- Wenn `tar` die Meldung `xz: Cannot exec` ausgibt, installieren Sie zuerst xz: `sudo apt-get install -y xz-utils`.
- In Docker oder anderen Containern mit wenig gemeinsamem Speicher führt der Miner seine Programmdateien aus
  `thenewminer/.miner/run/` auf der Festplatte aus und weist darauf hin. Um einen anderen Ordner zu wählen, starten Sie ihn
  mit `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (ein Ordner, in den nur Sie schreiben können: `chmod 700 <folder>`).
- **Upgrade:** `./thenewminer --stop`, das neue Paket am selben Ort entpacken (es wird nach `thenewminer/` entpackt und
  behält `thenewminer/.miner/` mit Ihren Einstellungen und dem Modell), dann wieder starten. Läuft die alte Version noch,
  startet die neue nicht und fordert Sie auf, die alte zuerst zu beenden.

## Windows (ein Klick)

1. Entpacken Sie die gesamte `TheNewMiner-v0.1.45-windows-wsl2-r1.zip` (Rechtsklick › Alle extrahieren...) an einen
   beliebigen Ort: beliebiger Ordner, beliebiges Laufwerk; Pfade mit Leerzeichen oder chinesischen Zeichen sind kein
   Problem. Nicht aus der Zip-Ansicht heraus starten.
2. Doppelklicken Sie auf **`TheNewMiner.bat`**.
   - **Beim ersten Mal:** Es prüft Windows, den Treiber und die Festplatte, aktiviert bei Bedarf WSL2 (eine
     Administrator-Abfrage; eventuell ist einmal ein Neustart nötig, danach läuft die Einrichtung von selbst weiter),
     zeigt an, wo die Daten abgelegt werden (Enter bestätigt), legt eine eigene Linux-Umgebung (WSL) an, installiert den
     mitgelieferten Miner, fragt **einmalig** nach Ihrer Wallet-Adresse (beginnt mit `tc1`), dem Pool-Passwort (zweimal
     eingeben; 6–128 Zeichen, jedes sichtbare Zeichen, keine Leerzeichen) und dem Worker-Namen (Enter = der
     Computername), lädt das Modell herunter und beginnt dann zu minen.
   - **Jedes weitere Mal:** Doppelklicken, und es mint sofort mit Ihren gespeicherten Einstellungen. Außerdem wird eine
     Desktopverknüpfung „TheNewMiner“ erstellt. Ein erneuter Doppelklick, während bereits gemint wird, weist darauf hin
     und zeigt das Log.

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
- Wenn der Pool die Wallet oder das Passwort ablehnt, meldet das Fenster dies und das Mining stoppt: Führen Sie
  `TheNewMiner.bat setup` aus, um beides neu einzugeben.
- **Upgrade:** Die neue Version an einen beliebigen Ort entpacken und deren `TheNewMiner.bat` doppelklicken. Sie findet den
  installierten Miner, behält Ihre Einstellungen und das Modell und installiert die neue Version ohne Rückfragen. Der alte
  Ordner kann gelöscht werden.
- **Deinstallation:** `TheNewMiner.bat uninstall`.
- Die Bildschirmsprache folgt Windows (English / 简体中文 / 繁體中文). Zum Auswählen `-Lang en` (oder `zh-Hans`,
  `zh-Hant`) an einen beliebigen Befehl anhängen, z. B. `TheNewMiner.bat -Lang en`; das gilt für diesen Lauf.

## HiveOS (experimentell)

Die HiveOS-Unterstützung ist neu in v0.1.45 und **experimentell**. Sie verwendet das unten stehende
HiveOS-Custom-Miner-Paket.

| Datei | SHA-256 |
|---|---|
| [`thenewminer-0.1.45.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz) | `90576efd388ceeca4e456b62b568f267465320913a07c9f9736f42363b1701dc` |

Schnelleinrichtung (Flight Sheet importieren):

1. **Wallets:** Fügen Sie eine Wallet mit dem Coin `TSC` und Ihrer TensorCash-Adresse (`tc1...`) hinzu. Dies ist nur
   einmal nötig: HiveOS verlangt eine ausgewählte `TSC`-Wallet, und der Miner verwendet die Adresse, die Sie im Flight
   Sheet eintragen.
2. Öffnen Sie [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json) und kopieren Sie den Inhalt.
   Ersetzen Sie `<YOUR_WALLET>` durch Ihre Wallet-Adresse (behalten Sie `.%WORKER_NAME%` dahinter bei) und
   `<YOUR_POOL_PASSWORD>` durch Ihr Pool-Passwort (im JSON wird ein `"` im Passwort als `\"` und ein `\` als `\\`
   geschrieben). Wählen Sie in HiveOS Flight Sheets › **Import from clipboard**, fügen Sie den Inhalt ein, wählen Sie
   Ihre `TSC`-Wallet aus und speichern Sie.
3. Wenden Sie das Flight Sheet auf Ihr Rig an. Das Rig lädt das Paket herunter und beginnt mit dem Mining.

Stattdessen manuell einrichten: Erstellen Sie ein Flight Sheet mit Coin `TSC`, Wallet: Ihre `TSC`-Wallet, Pool:
„Configure in miner“, Miner: „Custom“, klicken Sie auf **Setup Miner Config** und füllen Sie aus:

| Feld | Wert |
|---|---|
| Miner name | `thenewminer` (wird aus der Installations-URL übernommen) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz` |
| Hash algorithm | leer lassen |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (wird ignoriert: der Pool ist fest eingebaut) |
| Pass | Ihr Pool-Passwort |
| Extra config arguments | leer, oder `--proxy <YOUR_PROXY>` (siehe Abschnitt Proxy) |

- Der Worker-Name auf dem Pool ist der Name des Rigs in HiveOS: Verwenden Sie nur Buchstaben, Ziffern, `_`, `-` oder `.`
  (keine Leerzeichen), bis zu 64 Zeichen. Um einen anderen Worker-Namen zu verwenden, fügen Sie `-w <RIG_NAME>` zu den
  Extra config arguments hinzu.
- Auf dem HiveOS-Laufwerk müssen etwa 12 GB für das Modell und den Miner frei sein (prüfen mit `df -h /hive/miners`).
- Das Modell (etwa 8.1 GiB) wird einmalig nach `/hive/miners/custom/.thenewminer-models` heruntergeladen und bei Upgrades
  behalten.
- Wallet und Passwort erreichen den Miner über seine Umgebungsvariablen, nicht über seine Befehlszeile.
- Wenn der Pool die Wallet oder das Passwort ablehnt, meldet das Miner-Log dies, und der Miner wartet 10 Minuten, bevor
  HiveOS ihn erneut startet: Korrigieren Sie das Flight Sheet und wenden Sie es an (das startet den Miner sofort neu).
- Logs: der Miner-Bildschirm in HiveOS (`miner` in der Shell des Rigs) oder `/var/log/miner/custom/custom.log`.
- **Upgrade:** Ändern Sie beide Versionsnummern in der Installations-URL (zum Beispiel
  `.../raw/v0.1.46/thenewminer-0.1.46.tar.gz`) und wenden Sie das Flight Sheet an. HiveOS installiert nur, wenn sich der
  Dateiname in der URL ändert.
- **Der erste Start dauert eine Weile:** Nach dem Download des Modells kann auf Karten der RTX-30-Serie eine einmalige
  GPU-Prüfung auf einer langsamen Rig-Festplatte in ein Timeout laufen; HiveOS startet den Miner dann von selbst neu,
  zwei- oder dreimal (etwa 8 Minuten). Lassen Sie ihn in Ruhe; danach mint er.
- **Haben Sie das Modell bereits?** Kopieren Sie `Qwen3-8B-Q8_0-full-v2.gguf` (etwa 8.1 GiB, genau mit diesem
  Dateinamen) vor dem ersten Start nach `/home/user/` auf dem Rig: Der Miner findet die Datei, prüft ihren SHA-256 und
  verwendet sie, statt sie erneut herunterzuladen.

**Wenn das Rig GitHub nicht erreichen kann:** Laden Sie `thenewminer-0.1.45.tar.gz` auf einem PC herunter (prüfen Sie
den SHA-256 in der Tabelle oben), kopieren Sie die Datei auf das Rig und entpacken Sie sie dort, wo HiveOS
benutzerdefinierte Miner ablegt. Wenden Sie dann das Flight Sheet wie gewohnt an: HiveOS meldet „Already installed“ und
lädt das Paket nicht herunter. Behalten Sie den Dateinamen exakt als `thenewminer-0.1.45.tar.gz` bei (ein Browser hängt
eventuell „ (1)“ an) und lassen Sie die Installations-URL im Flight Sheet unverändert: HiveOS vergleicht diesen Namen.

```bash
# auf dem PC
scp thenewminer-0.1.45.tar.gz user@<RIG_IP>:/tmp/
# auf dem Rig, als root (Hive Shell oder SSH und dann sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.45.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.45.tar.gz
```

## Wallet, Pool-Passwort oder Worker-Namen ändern

Der Pool bindet jede Auszahlungsadresse dauerhaft an ihr eigenes Pool-Konto: Die Pool-Seite
[tsc.the-new.win](https://tsc.the-new.win) bezeichnet die TSC-Auszahlungsadresse als „永久绑定 · 不可修改“ (dauerhaft gebunden,
nicht änderbar) und sagt, dass man zum Wechseln die neue Adresse direkt im Miner verwenden soll. Eine andere Wallet
braucht daher ihr eigenes Pool-Konto und ihr eigenes Pool-Passwort (legen Sie diese zuerst auf der Pool-Seite an). Wenn
Sie das Pool-Passwort auf der Pool-Seite ändern, aktualisieren Sie es auf jedem Miner. Der Pool kann ein vergessenes
Passwort nicht wiederherstellen.

**Linux:** Stoppen Sie den Miner und starten Sie ihn dann einmal mit den neuen Einstellungen; sie ersetzen die
gespeicherten:

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

Geben Sie `-u` und `-p` zusammen an: Eine der beiden Optionen allein ändert die gespeicherte Wallet und das gespeicherte
Passwort nicht. Der Worker-Name kann allein geändert werden: `./thenewminer -w <RIG_NAME> --background` (nach `--stop`).

**Windows:** Führen Sie `TheNewMiner.bat setup` aus. Es stoppt den laufenden Miner und fragt erneut nach Wallet,
Pool-Passwort und Worker-Namen. Starten Sie ihn danach wie gewohnt (Doppelklick auf `TheNewMiner.bat`).

**HiveOS:** Bearbeiten Sie das Flight Sheet: Tragen Sie die neue Wallet-Adresse in „Wallet and worker template“
(`<NEW_WALLET>.%WORKER_NAME%`) und das neue Pool-Passwort in Pass ein, speichern Sie es dann und wenden Sie es an. Der
Worker-Name ist der Name des Rigs in HiveOS (oder `-w <RIG_NAME>` in den Extra config arguments).

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
| GPU | NVIDIA GeForce RTX Serie 30 / 40 / 50, 10 GB VRAM oder mehr. Multi-GPU-Rigs: unterstützt, jede GPU mint; in dieser Version mit einer GPU pro Rechner getestet |
| Treiber | NVIDIA **580 oder neuer** (`nvidia-smi` muss funktionieren). Unter Windows in Windows installieren, niemals in WSL |
| Linux | x86_64 mit **glibc 2.34 oder neuer**, zum Beispiel Ubuntu 22.04 oder neuer oder Debian 12 oder neuer. Das Installationsprogramm prüft, was es braucht, und nennt alles, was fehlt |
| Windows | Windows 10 21H2 oder neuer, oder Windows 11, 64-Bit; Virtualisierung (Intel VT-x / AMD SVM) im BIOS aktiviert |
| Festplatte | etwa 20 GB frei |
| RAM | 4 GB oder mehr |
| Modell | etwa 8.1 GiB, einmalig beim ersten Start heruntergeladen (parallel, mehrere Mirrors) und danach wiederverwendet |

## NVIDIA-Treiber aktualisieren (580 oder neuer)

Der Miner braucht den NVIDIA-Treiber **580 oder neuer**. Prüfen Sie, welchen Sie haben:

```bash
nvidia-smi    # die Kopfzeile zeigt "Driver Version: ..."; sie muss 580 oder neuer sein
```

- **Ubuntu:** `sudo ubuntu-drivers install` installiert den empfohlenen Treiber (prüfen Sie danach, dass er 580 oder neuer ist).
  Um die Version selbst zu wählen:
  `sudo apt install nvidia-driver-580` (oder neuer; Karten der Serie RTX 50 brauchen die Open-Variante, z. B.
  `nvidia-driver-580-open`). Starten Sie danach neu. Verwenden Sie entweder die Treiberpakete von Ubuntu oder den
  `.run`-Installer von NVIDIA, niemals beide.
- **Debian:** Die eigenen Treiberpakete von Debian sind älter als 580 (535 in Debian 12, 550 in Debian 13). Installieren Sie
  einen neueren Treiber aus dem
  NVIDIA-Repository, wie im [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)
  (Installationsanleitung für NVIDIA-Treiber) beschrieben, und starten Sie danach neu.
- **Windows:** Aktualisieren Sie mit der [NVIDIA App](https://www.nvidia.com/en-us/software/nvidia-app/) (früher GeForce
  Experience) oder laden Sie den Treiber für Ihre Karte von [nvidia.com](https://www.nvidia.com/en-us/drivers/) herunter
  (580 oder neuer). WSL2 nutzt diesen Windows-Treiber: Installieren Sie niemals einen Linux-NVIDIA-Treiber in WSL.
- **HiveOS:** Führen Sie als root (Hive Shell, oder per SSH und dann `sudo -i`) `nvidia-driver-update --list` aus, um
  die Versionen zu sehen, dann zum Beispiel `nvidia-driver-update 595` (jede Version 580 oder neuer), dann `sreboot`.

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

Tests auf einem echten Pool mit unseren Test-Rigs (eine GPU pro Rechner). A = akzeptierte Shares, R = abgelehnte Shares.
„Stabiles Mining“ ist die Median-Rate zwischen Job-Wechseln.

| Datum | Karte | Betriebssystem | Paket | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | v0.1.45 Release-Build | 65.42 N/s (stabiles Mining, 16 min; 62.35 inklusive Job-Wechsel) | 13 / 0 (0 ungültig) |
| 2026-10-23 | RTX 5090 | Linux | v0.1.45 Release-Build | 94.18 N/s (stabiles Mining; 92.24 inklusive Job-Wechsel) | 17 / 0 (0 ungültig) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.44 sealed-r4 | 65.58 N/s (Median, 16 min) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | v0.1.44 sealed-r4 | 93.45 N/s (stabiles Mining, 44 min; 90.39 inklusive Job-Wechsel) | 31 / 0 (+1 stale beim Beenden) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | v0.1.44 windows-wsl2-r5 | ~22 N/s (eigener Lauf des Betreibers) | k. A. |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | v0.1.44 sealed-r4 | 21.04 N/s (letzte 10 min von 23 min Mining) | 3 / 0 |

RTX 3080 unter Linux (v0.1.44): Installation 6/6 Schritte OK, keine ungültigen Fenster, sauberes Beenden. Log-Auszug:
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

**Der Miner meldet, dass der Pool die Wallet / das Passwort abgelehnt hat.**
Prüfen Sie die Wallet-Adresse und das Pool-Passwort (und ob der Pool Ihr Konto freigegeben hat) und starten Sie dann
erneut: Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. Bei falschen
Zugangsdaten versucht es der Miner nicht von selbst erneut.

**Mint er? Wo sind die Logs?**
Linux: `./thenewminer --status`, `./thenewminer --logs` oder die Datei `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat status` und `TheNewMiner.bat logs`.

## Sicherheitshinweis

Der Miner und die GPU-Engine werden verschlüsselt (ChaCha20-Poly1305) im Paket ausgeliefert; der Miner entschlüsselt sie
beim Start auf Ihrem Rechner. Das erhöht den Aufwand, sie zu kopieren, ist aber **nicht unknackbar**: Jeder mit
Administratorzugriff auf einen Rechner, auf dem der Miner läuft, kann mit genügend Aufwand den Schlüssel oder den
entschlüsselten Code gewinnen.
Ihre Wallet-Adresse und Ihr Pool-Passwort werden auf Ihrem eigenen Rechner gespeichert und nur an den Pool gesendet.
