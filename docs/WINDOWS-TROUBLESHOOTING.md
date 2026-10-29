**English** · [简体中文](i18n/WINDOWS-TROUBLESHOOTING.zh-CN.md) · [繁體中文](i18n/WINDOWS-TROUBLESHOOTING.zh-TW.md) · [Русский](i18n/WINDOWS-TROUBLESHOOTING.ru.md) · [Español](i18n/WINDOWS-TROUBLESHOOTING.es.md) · [Português (Brasil)](i18n/WINDOWS-TROUBLESHOOTING.pt-BR.md) · [Français](i18n/WINDOWS-TROUBLESHOOTING.fr.md) · [Deutsch](i18n/WINDOWS-TROUBLESHOOTING.de.md) · [Türkçe](i18n/WINDOWS-TROUBLESHOOTING.tr.md) · [Tiếng Việt](i18n/WINDOWS-TROUBLESHOOTING.vi.md) · [한국어](i18n/WINDOWS-TROUBLESHOOTING.ko.md) · [日本語](i18n/WINDOWS-TROUBLESHOOTING.ja.md)

# Windows troubleshooting

TheNewMiner on Windows runs the Linux miner inside WSL2. When setup stops, the window says at which step and what to do,
and `TheNewMiner.bat` shows it again (it is also kept in `%LOCALAPPDATA%\TheNewMiner\last-setup-failure.txt`). This
page explains the common cases. Installation and daily commands are in the [guide](GUIDE.md).

## DNS does not work inside WSL

Setup checks in step 4 that WSL can resolve the pool and the model download. If it cannot, setup stops there and changes
nothing by itself. Open `%UserProfile%\.wslconfig` in Notepad (create the file if it does not exist) and add these two
lines (on Windows 11 22H2 or newer, `networkingMode=mirrored` under `[wsl2]` also works):

```ini
[wsl2]
dnsTunneling=true
```

Then run `wsl --shutdown` in a Command Prompt (it stops every WSL distribution, Docker Desktop's too) and double-click
`TheNewMiner.bat` again.

A VPN or a firewall that blocks WSL can cause the same message: allow WSL, or set a proxy with
`TheNewMiner.bat setup -Proxy ...` (see "Proxy" in the [guide](GUIDE.md)).

## Ubuntu cannot be downloaded, or virtualization is off

- **Network:** setup installs Ubuntu 24.04 with `wsl --install`. Where that download cannot connect (for example where
  GitHub is unreachable), setup downloads Ubuntu's own WSL image from a public mirror in China instead and uses it only
  if its SHA-256 matches. If no mirror answers either, check the internet connection and try again later.
- **Virtualization:** a message about virtualization (for example `0x80370102`) means it is off. Turn on Intel VT-x /
  AMD SVM in the BIOS, then double-click `TheNewMiner.bat` again.

## The package is on a Windows drive

If you unpacked the Linux package yourself and started `./thenewminer` inside WSL from a Windows drive (such as
`/mnt/d/...`), the miner refuses to start: a Windows drive cannot keep its private settings file. It prints a
ready-to-paste `mkdir -p ~/thenewminer && cp -a ...` line that copies the folder to your Linux home folder and starts it
there. Or use the Windows zip instead: `TheNewMiner.bat` puts everything in the right place by itself.

## Slow on a 10 GB card (browsers, Docker Desktop)

A 10 GB card gets the fast layout only when about 9.3 GB (9472 MiB) of VRAM is free when mining starts. On Windows,
other programs share the card: a browser with hardware acceleration or Docker Desktop can take enough VRAM that the
miner falls back to a slower layout. On an RTX 3080 10 GB this was about 9 N/s instead of 21–22 N/s.

1. Close your browsers, or turn off their hardware acceleration (Chrome and Edge: Settings › System › "Use graphics
   acceleration when available"; Firefox: Settings › General › Performance), and quit Docker Desktop.
2. Check the free VRAM in a Command Prompt: `nvidia-smi --query-gpu=memory.used,memory.total --format=csv`
   (total minus used should be at least 9472 MiB).
3. Restart mining: `TheNewMiner.bat stop`, then double-click `TheNewMiner.bat`.

12 GB cards need the same 9472 MiB free; cards with 16 GB or more are not affected.

## Clock errors

The pool stops a miner whose clock is about 30 seconds off. WSL2's clock can drift, for example after the PC sleeps,
and setup warns when it is off. If the miner reports a clock error:

1. Open Windows Settings › Time & language › Date & time, turn on "Set time zone automatically" and "Set the time
   automatically", then click **Sync now**.
2. In a Command Prompt run `wsl --shutdown`.
3. Start mining again.

## Windows security prompts

SmartScreen ("Windows protected your PC") and antivirus programs that flag the miner: see the FAQ in the
[guide](GUIDE.md).

## Still stuck?

Open an issue on [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) with the output of
`TheNewMiner.bat status`, the step that failed and the content of `last-setup-failure.txt`. Never post your pool password
or your wallet's private key.
