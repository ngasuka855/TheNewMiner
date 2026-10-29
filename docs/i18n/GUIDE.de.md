[English](../GUIDE.md) · [简体中文](GUIDE.zh-CN.md) · [繁體中文](GUIDE.zh-TW.md) · [Русский](GUIDE.ru.md) · [Español](GUIDE.es.md) · [Português (Brasil)](GUIDE.pt-BR.md) · [Français](GUIDE.fr.md) · **Deutsch** · [Türkçe](GUIDE.tr.md) · [Tiếng Việt](GUIDE.vi.md) · [한국어](GUIDE.ko.md) · [日本語](GUIDE.ja.md)

> Diese Übersetzung dient nur der Bequemlichkeit; maßgeblich ist das englische README. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner-Anleitung

Alles über den ersten Start hinaus: den Download prüfen, tägliche Nutzung unter Linux, Windows und HiveOS, Upgrades,
was zu tun ist, wenn er nicht mint, und die FAQ. Downloads und der Schnellstart stehen auf der
[Hauptseite](README.de.md); Windows-Probleme haben eine eigene Seite: [Windows-Fehlerbehebung](WINDOWS-TROUBLESHOOTING.de.md).

## Download prüfen

| Datei | SHA-256 |
|---|---|
| [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | `86c215dc9401209b8d2a2346252ed5cc1901527b6a51520372e3128583c63656` |
| [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | `45a459646713bef9f8285848486c0a4bd66dee65e8c63090c8d3345c998b2659` |
| [`thenewminer-0.1.47.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz) (HiveOS) | `6b2515c5bf23f60daa236ff4a8b165ce5e55265d7eecac9dd408896e1e31b784` |

Das Windows-Zip ist ein Komplettpaket: Es enthält bereits den Linux-Miner, Sie brauchen also nur das Zip. Legen Sie das
Paket bzw. die Pakete und [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS) in denselben
Ordner und führen Sie dann aus:

```bash
# Linux (--ignore-missing: nur die heruntergeladenen Pakete werden geprüft)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Eingabeaufforderung): Ausgabe mit dem SHA-256 in der Tabelle oben vergleichen
certutil -hashfile TheNewMiner-v0.1.47-windows-wsl2-r3.zip SHA256
```

Jede Datei muss `OK` melden (unter Windows: dieselben 64 Zeichen wie in der Tabelle). Wenn sie nicht übereinstimmt,
verwenden Sie sie nicht.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Befehl | Funktion |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | Erster Start: installiert das Nötige und mint dann im Hintergrund |
| `./thenewminer --background` | Spätere Starts: Ihre Einstellungen bleiben gespeichert |
| `./thenewminer` | Im Vordergrund minen (Strg+C beendet) |
| `./thenewminer --logs` | Den Miner beobachten (Strg+C verlässt die Ansicht; das Mining läuft weiter) |
| `./thenewminer --status` | Ob er mint, seine letzte Rate und die akzeptierten / abgelehnten Shares; nach einem Stopp, warum er gestoppt hat und wie man es behebt |
| `./thenewminer --stop` | Mining beenden |
| `./thenewminer --help` | Alle Optionen |

- `-u` ist Ihre TensorCash-Wallet-Adresse, `-p` Ihr Pool-Passwort, `-w` der auf dem Pool angezeigte Worker-Name
  (Standard: der Hostname des Rechners; 1–64 Buchstaben, Ziffern, `_`, `-` oder `.`).
- Die Wallet-Adresse wird geprüft, bevor irgendetwas startet: Eine Adresse mit einem Tippfehler (ihre Prüfsumme stimmt
  nicht) oder die Beispieladresse wird abgelehnt, mit einer Angabe, was falsch ist.
- Das Pool-Passwort: 6–128 Zeichen, jedes sichtbare Zeichen, keine Leerzeichen. Setzen Sie es auf der Befehlszeile immer
  in einfache Anführungszeichen, z. B. `-p 'my$pass!'` (ein `'` innerhalb einfacher Anführungszeichen wird als `'\''`
  geschrieben).
- Die Einstellungen werden in `thenewminer/.miner/` gespeichert (die Datei mit dem Passwort ist nur für Sie lesbar).
  Wallet und Passwort werden sofort von der Befehlszeile entfernt, sodass sie in `ps` nicht erscheinen (Ihre Shell kann
  den Befehl dennoch in ihrem Verlauf speichern).
- Beim ersten Start auf einem Rechner mit einer einzigen GPU prüft der Miner Wallet und Passwort beim Pool **vor** dem
  Download des Modells, sodass ein Tippfehler in Sekunden auffällt (ein Multi-GPU-Rig bemerkt ihn, wenn das Mining
  beginnt). Lehnt der Pool sie ab, meldet der Miner das und beendet sich: Prüfen Sie beides und starten Sie erneut mit
  `-u` und `-p`.
- Der erste Start lädt das Modell herunter (etwa 8.1 GiB, einmalig), installiert die GPU-Engine für Ihre Karte, führt
  einen Selbsttest auf der Karte aus und beginnt dann zu minen. Zeilen wie `[ RATE] ... N/s` bedeuten, dass gemint wird.
  Das Log ist `thenewminer/.miner/thenewminer.log`.
- Ein Standard-Ubuntu 22.04 / 24.04 (auch das von WSL) hat bereits alle Bibliotheken, die der Miner braucht, also wird
  nichts installiert. Wo eine fehlt (Docker-Images, minimale Systeme), listet der erste Start unter Ubuntu, Debian und
  HiveOS sie auf und installiert sie mit `apt` (benötigt sudo oder root); auf anderen Systemen nennt er sie, damit Sie
  sie selbst installieren.
- Multi-GPU-Rigs: unterstützt, jede GPU mint; diese Version wurde mit einer GPU pro Rechner getestet.
- In Docker oder anderen Containern mit wenig gemeinsamem Speicher führt der Miner seine Programmdateien aus
  `thenewminer/.miner/run/` auf der Festplatte aus und weist darauf hin. Um einen anderen Ordner zu wählen, starten Sie
  ihn mit `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (ein Ordner, in den nur Sie schreiben können: `chmod 700 <folder>`).
- **Upgrade:** `./thenewminer --stop`, das neue Paket am selben Ort entpacken (es wird nach `thenewminer/` entpackt und
  behält `thenewminer/.miner/` mit Ihren Einstellungen und dem Modell), dann wieder starten. Läuft die alte Version noch,
  startet die neue nicht und fordert Sie auf, die alte zuerst zu beenden. Entpacken Sie es stattdessen in einen neuen
  Ordner, findet es das Modell der alten Version (gleiche Größe und gleicher SHA-256) und verwendet es wieder: kein
  zweiter Download von 8.1 GiB. Geben Sie dort `-u -p -w` erneut an.

### Wenn er nicht mint

`./thenewminer --status` sagt, warum der Miner gestoppt hat und wie man es behebt. Die häufigsten Meldungen:

| Sie sehen | Was zu tun ist |
|---|---|
| `bash: ./thenewminer: Permission denied` | Der Ordner erlaubt keine Programme (ein `noexec`-Mount wie manche `/tmp`, oder ein USB-Stick). Entpacken Sie es in Ihrem Home-Ordner: `cd ~ && tar -xJf <the package>.tar.xz && cd thenewminer` |
| `this package is on a FAT (vfat) drive` (oder exFAT / NTFS, oder `Windows (DrvFs, 9p)`: ein Windows-Laufwerk wie `/mnt/d` in WSL) | Ein solches Laufwerk kann die privaten Einstellungen des Miners nicht aufbewahren (Modus 600). Fügen Sie die ausgegebene Zeile `mkdir -p ~/thenewminer && cp -a ...` ein: Sie kopiert den Ordner in Ihren Linux-Home-Ordner und startet ihn dort |
| `this system has no xz` | `sudo apt-get install -y xz-utils` (als root: ohne `sudo`), dann erneut starten |
| `This system is missing what TheNewMiner needs` | Führen Sie die ausgegebene Zeile `sudo apt-get install -y ...` aus und starten Sie dann erneut |
| `No NVIDIA GPU found on this machine` | Es wird keine NVIDIA-Karte erkannt. Prüfen Sie, ob die Karte richtig sitzt und mit Strom versorgt ist; wählen Sie bei einem Cloud-Rechner einen mit NVIDIA-GPU |
| `NVIDIA driver ... is too old` | Installieren Sie den genannten Treiber (580 oder neuer), starten Sie neu und starten Sie den Miner erneut (siehe „NVIDIA-Treiber aktualisieren“ unten) |
| `the pool rejected the wallet / password` | Prüfen Sie `-u` und `-p` und ob das Konto auf der Pool-Website existiert, dann `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' --background` |
| `no download source answered` | Der Modell-Download erreicht das Internet nicht. Hinter einem Proxy: `https_proxy=http://<HOST>:<PORT> ./thenewminer --background`; oder kopieren Sie das Modell (siehe „Haben Sie das Modell bereits?“ unten) |
| `cannot reach the pool` / `pool disconnected` | Prüfen Sie die Internetverbindung, das DNS und ob die Firewall ausgehendes TCP 443 erlaubt; wenn Ihr Netzwerk den Pool blockiert, verwenden Sie `--proxy` (siehe „Proxy“ unten). Er verbindet sich von selbst neu |

### Start nach einem Neustart (optional)

Der Miner startet nach einem Neustart nicht von selbst (HiveOS erledigt das für Sie). Unter Linux fügen Sie entweder
eine `crontab`-Zeile hinzu:

```bash
crontab -e      # dann diese Zeile hinzufügen (mit Ihrem Ordner):
@reboot cd /home/<YOU>/thenewminer && ./thenewminer --background
```

oder einen systemd-Benutzerdienst (einmalig `loginctl enable-linger <YOU>`, damit er ohne Anmeldung startet):

```ini
# ~/.config/systemd/user/thenewminer.service, dann: systemctl --user enable --now thenewminer
[Unit]
Description=TheNewMiner
After=network-online.target
[Service]
WorkingDirectory=%h/thenewminer
ExecStart=%h/thenewminer/thenewminer
ExecStop=%h/thenewminer/thenewminer --stop
Restart=on-failure
RestartSec=60
RestartPreventExitStatus=77
[Install]
WantedBy=default.target
```

Exit-Status 77 bedeutet, dass der Pool die Wallet / das Passwort abgelehnt hat: Ein Neustart kann nicht helfen, daher
hört der Dienst dort auf.

## Windows

1. Entpacken Sie die gesamte `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` (Rechtsklick › Alle extrahieren...) an einen
   beliebigen Ort: beliebiger Ordner, beliebiges Laufwerk; Pfade mit Leerzeichen oder chinesischen Zeichen sind kein
   Problem. Nicht aus der Zip-Ansicht heraus starten.
2. Doppelklicken Sie auf **`TheNewMiner.bat`**.
   - **Beim ersten Mal:** EIN Fenster fragt zuerst nach Ihrem Pool-Konto: der Wallet-Adresse (beginnt mit `tc1`; ihre
     Prüfsumme wird geprüft, und das angezeigte Beispiel ist keine echte Wallet), dem Pool-Passwort (6–128 Zeichen,
     jedes sichtbare Zeichen, keine Leerzeichen) und dem Worker-Namen (der Computername ist vorausgefüllt). OK bleibt
     grau, bis alle drei Angaben stimmen. Nach OK und „Ja“ wird nichts mehr gefragt: Es prüft Windows, den Treiber und
     die Festplatte, aktiviert bei Bedarf WSL2 (eine Administrator-Abfrage; eventuell ist einmal ein Neustart nötig,
     danach läuft die Einrichtung von selbst weiter), zeigt an, wo die Daten abgelegt werden, legt eine eigene
     Linux-Umgebung (WSL) an, installiert den mitgelieferten Miner, lädt das Modell herunter und beginnt dann zu minen.
     Ohne Desktop (über SSH) kommen dieselben Fragen in der Konsole.
   - **Jedes weitere Mal:** Doppelklicken, und es mint sofort mit Ihren gespeicherten Einstellungen. Außerdem wird eine
     Desktopverknüpfung „TheNewMiner“ erstellt. Ein erneuter Doppelklick, während bereits gemint wird, weist darauf hin
     und zeigt das Log.
   - **Wenn die Einrichtung stoppt,** sagt das Fenster, bei welchem Schritt und was zu tun ist, und `TheNewMiner.bat`
     zeigt es erneut an. Siehe [Windows-Fehlerbehebung](WINDOWS-TROUBLESHOOTING.de.md).

Tägliche Befehle (öffnen Sie eine Eingabeaufforderung im entpackten Ordner):

| Befehl | Funktion |
|---|---|
| `TheNewMiner.bat` | Bei Bedarf installieren, dann in diesem Fenster minen (Schließen des Fensters beendet das Mining) |
| `TheNewMiner.bat background` | Ohne Fenster minen |
| `TheNewMiner.bat logs` | Den Miner beobachten (Strg+C verlässt die Ansicht; das Mining läuft weiter) |
| `TheNewMiner.bat status` | Ob er läuft, und die letzten Hashrates |
| `TheNewMiner.bat stop` | Mining beenden |
| `TheNewMiner.bat setup` | Wallet / Passwort / Worker-Namen neu eingeben (Cancel behält sie bei) |
| `TheNewMiner.bat uninstall` | Miner, seinen Datenordner und die Verknüpfung entfernen (fragt vorher nach) |

- **Wo die Daten liegen:** Der Miner und das Modell liegen in einem Datenordner, der bei der ersten Installation gewählt
  wird (standardmäßig `TheNewMinerData` im Stammverzeichnis des Laufwerks, auf das Sie entpackt haben, z. B.
  `D:\TheNewMinerData`), nicht im entpackten Ordner. Dort werden etwa 20 GB freier Speicher benötigt. Um einen anderen
  Ordner zu wählen: `TheNewMiner.bat setup -DataDir <folder>`.
- **Ubuntu:** Die Einrichtung installiert Ubuntu 24.04 mit `wsl --install`. Bekommt dieser Download keine Verbindung,
  lädt sie Ubuntus eigenes WSL-Image `ubuntu-24.04.5-wsl-amd64.wsl` von einem öffentlichen Mirror in China herunter
  (Tsinghua TUNA, dann USTC, dann Aliyun) und verwendet es nur, wenn sein SHA-256 genau
  `bb415d824822c4b878125729af451a5d18fb13d1cf5cbed9a7393ad64ac6039e` ist.
- Wenn der Pool die Wallet oder das Passwort ablehnt, meldet das Fenster dies und das Mining stoppt: Führen Sie
  `TheNewMiner.bat setup` aus, um beides neu einzugeben.
- **Upgrade:** Die neue Version an einen beliebigen Ort entpacken und deren `TheNewMiner.bat` doppelklicken. Sie findet
  den installierten Miner, behält Ihre Einstellungen und das Modell und installiert die neue Version ohne Rückfragen.
  Eine Installation, die von einer früheren Version oder von Hand in einer anderen WSL-Distribution (zum Beispiel
  `Ubuntu-24.04`) angelegt wurde, wird gefunden und dort aktualisiert, wo sie ist: keine zweite Installation, kein neues
  Ubuntu, kein zweiter Modell-Download. Kann sie dort nicht aktualisiert werden, stoppt die Einrichtung und nennt den
  Grund. Der alte Ordner kann gelöscht werden.
- **Deinstallation:** `TheNewMiner.bat uninstall`. Eine eigene WSL-Distribution von Ihnen wird dabei nie entfernt.
- Das Windows-Kit ist auf Englisch.

## HiveOS (experimentell)

Die HiveOS-Unterstützung ist **experimentell** (seit v0.1.45, die auf einem echten HiveOS-Rig lief; die Änderungen seit
v0.1.46 liefen noch auf keinem). Sie verwendet das HiveOS-Custom-Miner-Paket `thenewminer-0.1.47.tar.gz` (sein SHA-256
steht in der Tabelle unter „Download prüfen“).

Schnelleinrichtung (Flight Sheet importieren):

1. **Wallets:** Fügen Sie eine Wallet mit dem Coin `TSC` und Ihrer TensorCash-Adresse (`tc1...`) hinzu. Dies ist nur
   einmal nötig: HiveOS verlangt eine ausgewählte `TSC`-Wallet, und der Miner verwendet die Adresse, die Sie im Flight
   Sheet eintragen.
2. Öffnen Sie [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) und kopieren Sie den
   Inhalt. Ersetzen Sie `<YOUR_WALLET>` durch Ihre Wallet-Adresse (behalten Sie `.%WORKER_NAME%` dahinter bei) und
   `<YOUR_POOL_PASSWORD>` durch Ihr Pool-Passwort (im JSON wird ein `"` im Passwort als `\"` und ein `\` als `\\`
   geschrieben). Wählen Sie in HiveOS Flight Sheets › **Import from clipboard**, fügen Sie den Inhalt ein, wählen Sie
   Ihre `TSC`-Wallet aus und speichern Sie.
3. Wenden Sie das Flight Sheet auf Ihr Rig an. Das Rig lädt das Paket herunter und beginnt mit dem Mining.

Stattdessen manuell einrichten: Erstellen Sie ein Flight Sheet mit Coin `TSC`, Wallet: Ihre `TSC`-Wallet, Pool:
„Configure in miner“, Miner: „Custom“, klicken Sie auf **Setup Miner Config** und füllen Sie aus:

| Feld | Wert |
|---|---|
| Miner name | `thenewminer` (wird aus der Installations-URL übernommen) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` |
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
- **Wenn etwas fehlschlägt** (zum Beispiel lehnt der Pool die Wallet oder das Passwort ab), zeigen das Miner-Log und das
  HiveOS-Dashboard den Grund, und das Rig versucht es mit wachsenden Pausen (eine bis zehn Minuten) erneut. Korrigieren
  Sie das Flight Sheet und wenden Sie es an: Das startet den Miner sofort neu.
- Logs: der Miner-Bildschirm in HiveOS (`miner` in der Shell des Rigs) oder `/var/log/miner/custom/custom.log`.
- **Upgrade:** Ändern Sie beide Versionsnummern in der Installations-URL (zum Beispiel
  `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`) und wenden Sie das Flight Sheet an. HiveOS installiert nur, wenn sich der
  Dateiname in der URL ändert.
- **Der erste Start** lädt das Modell herunter und installiert die GPU-Engine, was einige Minuten dauert.
- **Haben Sie das Modell bereits?** Kopieren Sie es vor dem ersten Start nach `/home/user/` auf dem Rig (siehe den
  nächsten Abschnitt).

**Wenn das Rig GitHub nicht erreichen kann:** Laden Sie `thenewminer-0.1.47.tar.gz` auf einem PC herunter (prüfen Sie
seinen SHA-256), kopieren Sie die Datei auf das Rig und entpacken Sie sie dort, wo HiveOS benutzerdefinierte Miner
ablegt. Wenden Sie dann das Flight Sheet wie gewohnt an: HiveOS meldet „Already installed“ und lädt das Paket nicht
herunter. Behalten Sie den Dateinamen exakt als `thenewminer-0.1.47.tar.gz` bei (ein Browser hängt eventuell „ (1)“ an)
und lassen Sie die Installations-URL im Flight Sheet unverändert: HiveOS vergleicht diesen Namen.

```bash
# auf dem PC
scp thenewminer-0.1.47.tar.gz user@<RIG_IP>:/tmp/
# auf dem Rig, als root (Hive Shell oder SSH und dann sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.47.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.47.tar.gz
```

## Haben Sie das Modell bereits? Kopieren Sie es

Der Miner braucht eine einzige 8.1 GiB große Modelldatei: `Qwen3-8B-Q8_0-full-v2.gguf` (`8709518112` Bytes, SHA-256
`408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`). Liegt sie bereits auf diesem Rechner oder auf einem
anderen Computer in Ihrem Netzwerk, kopieren Sie sie, statt sie erneut herunterzuladen.

Bevor er irgendetwas herunterlädt, sucht der Miner die Datei anhand ihres genauen Namens und ihrer genauen Größe: in
seinem eigenen Modellordner, in früheren TheNewMiner-Ordnern, in Ihrem Home-Ordner (und ein oder zwei Ordnerebenen
darunter oder in einem `models`-Ordner dort), in den Home-Ordnern unter `/home`, in `/data`, `/mnt/*`,
`/media/*`, `/opt`, `/srv`, `/workspace`, `/var/lib` sowie in den Caches von Hugging Face / ModelScope. Findet er dort
nichts, durchsucht er bis zu 20 Sekunden lang diese Ordner auf derselben Festplatte (andere eingehängte Festplatten
werden nur nach dem Namen geprüft) nach einer beliebigen `.gguf`-Datei mit genau dieser Größe. Eine gefundene Datei wird
**nur verwendet, wenn ihr SHA-256 übereinstimmt**; dann verlinkt er sie (gleiche Festplatte) oder kopiert sie in seinen
eigenen Modellordner, sodass jede spätere Version sie sofort findet. Sie können die Datei auch direkt angeben:
`./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`.

**Linux:** Kopieren Sie die Datei in Ihren Home-Ordner und starten Sie den Miner dann wie gewohnt. Von einem Mac oder PC
im selben Netzwerk aus (Windows 10 und 11 haben `scp` in PowerShell) ersetzen Sie `user` und `<MINER_IP>` durch den
Login und die Adresse Ihres Miners:

```bash
# auf dem Miner selbst
cp /path/to/Qwen3-8B-Q8_0-full-v2.gguf ~/
# oder von einem anderen Computer
scp Qwen3-8B-Q8_0-full-v2.gguf user@<MINER_IP>:~/
```

**HiveOS:** Kopieren Sie die Datei nach `/home/user/` auf dem Rig (der SSH-Login von HiveOS ist `user`) und wenden Sie
dann das Flight Sheet an. Der Miner findet sie dort, prüft sie und legt sie neben dem Paket ab
(`/hive/miners/custom/.thenewminer-models`), sodass eine neue Version sie nie wieder herunterlädt. Die Festplatte von
HiveOS ist klein: Führen Sie zuerst `disk-expand` aus, wenn `df -h /hive/miners` weniger als 12 GB freien Speicher
anzeigt.

```bash
scp Qwen3-8B-Q8_0-full-v2.gguf user@<RIG_IP>:/home/user/
```

**Windows:** Nachdem das erste `setup` die WSL-Distribution von TheNewMiner angelegt hat, kopieren Sie die Datei aus
einer Eingabeaufforderung hinein (ändern Sie `D:\Models` in den Ort Ihrer Kopie). Hatte der Download bereits begonnen,
schließen Sie das Fenster, führen Sie diese Befehle aus und starten Sie TheNewMiner erneut: Er findet die Kopie und
überspringt den Download. Das Kopieren dauert einige Minuten und zeigt keinen Fortschritt an.

```bat
wsl -d TheNewMiner -u root -- mkdir -p ~/thenewminer/models
wsl -d TheNewMiner -u root -- cp /mnt/d/Models/Qwen3-8B-Q8_0-full-v2.gguf ~/thenewminer/models/
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

**Windows:** Führen Sie `TheNewMiner.bat setup` aus. Es öffnet das Kontofenster mit Ihrem bereits eingetragenen
Worker-Namen; geben Sie die Wallet und das Pool-Passwort ein und starten Sie ihn dann wie gewohnt (Doppelklick auf
`TheNewMiner.bat`).

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

Diese Befehle starten auch den Miner: Beenden Sie den Miner zuerst und hängen Sie `--background` an, damit er im
Hintergrund bleibt.

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

- **Ubuntu:** `sudo ubuntu-drivers install` installiert den empfohlenen Treiber (prüfen Sie danach, dass er 580 oder
  neuer ist). Um die Version selbst zu wählen:
  `sudo apt install nvidia-driver-580` (oder neuer; Karten der Serie RTX 50 brauchen die Open-Variante, z. B.
  `nvidia-driver-580-open`). Starten Sie danach neu. Verwenden Sie entweder die Treiberpakete von Ubuntu oder den
  `.run`-Installer von NVIDIA, niemals beide.
- **Debian:** Die eigenen Treiberpakete von Debian sind älter als 580 (535 in Debian 12, 550 in Debian 13). Installieren
  Sie einen neueren Treiber aus dem NVIDIA-Repository, wie im
  [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)
  (Installationsanleitung für NVIDIA-Treiber) beschrieben, und starten Sie danach neu.
- **Windows:** Aktualisieren Sie mit der [NVIDIA App](https://www.nvidia.com/en-us/software/nvidia-app/) (früher GeForce
  Experience) oder laden Sie den Treiber für Ihre Karte von [nvidia.com](https://www.nvidia.com/en-us/drivers/) herunter
  (580 oder neuer). WSL2 nutzt diesen Windows-Treiber: Installieren Sie niemals einen Linux-NVIDIA-Treiber in WSL.
- **HiveOS:** Führen Sie als root (Hive Shell, oder per SSH und dann `sudo -i`) `nvidia-driver-update --list` aus, um
  die Versionen zu sehen, dann zum Beispiel `nvidia-driver-update 595` (jede Version 580 oder neuer), dann `sreboot`.

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
Prüfen Sie die Wallet-Adresse und das Pool-Passwort (das Konto muss auf der Website des Pools existieren) und starten Sie
dann erneut: Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. Bei
falschen Zugangsdaten versucht es der Miner nicht von selbst erneut.

**Mint er? Wo sind die Logs?**
Linux: `./thenewminer --status`, `./thenewminer --logs` oder die Datei `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat status` und `TheNewMiner.bat logs`.

**Meine RTX 3080 (10 GB) mint unter Windows viel langsamer als unter Linux.**
Unter Windows teilen sich andere Programme den VRAM. Siehe [Windows-Fehlerbehebung](WINDOWS-TROUBLESHOOTING.de.md):
Schließen Sie Browser mit Hardwarebeschleunigung und Docker Desktop.
