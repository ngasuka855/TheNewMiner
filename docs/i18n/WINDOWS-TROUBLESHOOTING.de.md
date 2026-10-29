[English](../WINDOWS-TROUBLESHOOTING.md) · [简体中文](WINDOWS-TROUBLESHOOTING.zh-CN.md) · [繁體中文](WINDOWS-TROUBLESHOOTING.zh-TW.md) · [Русский](WINDOWS-TROUBLESHOOTING.ru.md) · [Español](WINDOWS-TROUBLESHOOTING.es.md) · [Português (Brasil)](WINDOWS-TROUBLESHOOTING.pt-BR.md) · [Français](WINDOWS-TROUBLESHOOTING.fr.md) · **Deutsch** · [Türkçe](WINDOWS-TROUBLESHOOTING.tr.md) · [Tiếng Việt](WINDOWS-TROUBLESHOOTING.vi.md) · [한국어](WINDOWS-TROUBLESHOOTING.ko.md) · [日本語](WINDOWS-TROUBLESHOOTING.ja.md)

> Diese Übersetzung dient nur der Bequemlichkeit; maßgeblich ist das englische README. *(This translation is provided for convenience; the English README is authoritative.)*

# Windows-Fehlerbehebung

TheNewMiner führt unter Windows den Linux-Miner in WSL2 aus. Wenn die Einrichtung stoppt, sagt das Fenster, bei welchem
Schritt und was zu tun ist, und `TheNewMiner.bat` zeigt es erneut an (es wird auch in
`%LOCALAPPDATA%\TheNewMiner\last-setup-failure.txt` gespeichert). Diese Seite erklärt die häufigen Fälle. Installation
und tägliche Befehle stehen in der [Anleitung](GUIDE.de.md).

## DNS funktioniert in WSL nicht

Die Einrichtung prüft in Schritt 4, ob WSL die Namen des Pools und des Modell-Downloads auflösen kann. Wenn nicht, stoppt
die Einrichtung dort und ändert nichts von selbst. Öffnen Sie `%UserProfile%\.wslconfig` im Editor (legen Sie die Datei
an, falls sie nicht existiert) und fügen Sie diese zwei Zeilen hinzu (unter Windows 11 22H2 oder neuer funktioniert
auch `networkingMode=mirrored` unter `[wsl2]`):

```ini
[wsl2]
dnsTunneling=true
```

Führen Sie dann in einer Eingabeaufforderung `wsl --shutdown` aus (das beendet jede WSL-Distribution, auch die von
Docker Desktop) und doppelklicken Sie erneut auf `TheNewMiner.bat`.

Ein VPN oder eine Firewall, die WSL blockiert, kann dieselbe Meldung verursachen: Lassen Sie WSL zu oder legen Sie mit
`TheNewMiner.bat setup -Proxy ...` einen Proxy fest (siehe „Proxy“ in der [Anleitung](GUIDE.de.md)).

## Ubuntu lässt sich nicht herunterladen, oder die Virtualisierung ist aus

- **Netzwerk:** Die Einrichtung installiert Ubuntu 24.04 mit `wsl --install`. Wo dieser Download keine Verbindung
  bekommt (zum Beispiel dort, wo GitHub nicht erreichbar ist), lädt die Einrichtung stattdessen Ubuntus eigenes
  WSL-Image von einem öffentlichen Mirror in China herunter und verwendet es nur, wenn sein SHA-256 übereinstimmt.
  Antwortet auch kein Mirror, prüfen Sie die Internetverbindung und versuchen Sie es später erneut.
- **Virtualisierung:** Eine Meldung zur Virtualisierung (zum Beispiel `0x80370102`) bedeutet, dass sie ausgeschaltet
  ist. Schalten Sie Intel VT-x / AMD SVM im BIOS ein und doppelklicken Sie dann erneut auf `TheNewMiner.bat`.

## Das Paket liegt auf einem Windows-Laufwerk

Wenn Sie das Linux-Paket selbst entpackt und `./thenewminer` in WSL von einem Windows-Laufwerk aus gestartet haben (etwa
`/mnt/d/...`), verweigert der Miner den Start: Ein Windows-Laufwerk kann seine private Einstellungsdatei nicht
aufbewahren. Er gibt eine fertige Zeile `mkdir -p ~/thenewminer && cp -a ...` zum Einfügen aus, die den Ordner in Ihren
Linux-Home-Ordner kopiert und ihn dort startet. Oder verwenden Sie stattdessen das Windows-Zip: `TheNewMiner.bat` legt
alles von selbst an die richtige Stelle.

## Langsam auf einer 10-GB-Karte (Browser, Docker Desktop)

Eine 10-GB-Karte bekommt das schnelle Layout nur, wenn beim Start des Minings etwa 9.3 GB (9472 MiB) VRAM frei sind.
Unter Windows teilen sich andere Programme die Karte: Ein Browser mit Hardwarebeschleunigung oder Docker Desktop kann so
viel VRAM belegen, dass der Miner auf ein langsameres Layout zurückfällt. Auf einer RTX 3080 10 GB waren das etwa 9 N/s
statt 21–22 N/s.

1. Schließen Sie Ihre Browser oder schalten Sie ihre Hardwarebeschleunigung aus (Chrome und Edge: Einstellungen ›
   System › „Grafikbeschleunigung verwenden, falls verfügbar“; Firefox: Einstellungen › Allgemein › Leistung), und
   beenden Sie Docker Desktop.
2. Prüfen Sie den freien VRAM in einer Eingabeaufforderung:
   `nvidia-smi --query-gpu=memory.used,memory.total --format=csv` (total minus used sollte mindestens 9472 MiB
   betragen).
3. Starten Sie das Mining neu: `TheNewMiner.bat stop`, dann `TheNewMiner.bat` doppelklicken.

Karten mit 12 GB brauchen dieselben 9472 MiB freien Speicher; Karten mit 16 GB oder mehr sind nicht betroffen.

## Uhrzeitfehler

Der Pool stoppt einen Miner, dessen Uhr um etwa 30 Sekunden abweicht. Die Uhr von WSL2 kann abdriften, zum Beispiel
nachdem der PC im Energiesparmodus war, und die Einrichtung warnt, wenn sie falsch geht. Wenn der Miner einen
Uhrzeitfehler meldet:

1. Öffnen Sie Windows-Einstellungen › Zeit und Sprache › Datum und Uhrzeit, schalten Sie „Zeitzone automatisch
   festlegen“ und „Uhrzeit automatisch festlegen“ ein und klicken Sie dann auf **Jetzt synchronisieren**.
2. Führen Sie in einer Eingabeaufforderung `wsl --shutdown` aus.
3. Starten Sie das Mining erneut.

## Windows-Sicherheitsabfragen

SmartScreen („Der Computer wurde durch Windows geschützt“) und Antivirenprogramme, die den Miner melden: siehe die FAQ in
der [Anleitung](GUIDE.de.md).

## Kommen Sie nicht weiter?

Eröffnen Sie ein Issue unter [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) mit der Ausgabe
von `TheNewMiner.bat status`, dem fehlgeschlagenen Schritt und dem Inhalt von `last-setup-failure.txt`. Posten Sie
niemals Ihr Pool-Passwort oder den privaten Schlüssel Ihrer Wallet.
