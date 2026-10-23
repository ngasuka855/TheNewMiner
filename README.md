**English** · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

# TheNewMiner

TheNewMiner is a GPU miner for **TensorCash**. It runs on NVIDIA GeForce RTX 30, 40 and 50 series cards with
**10 GB or more of VRAM** and mines on the pool **`tsc-wss.the-new.win`**, which is built in: you do not enter a pool
address, and it connects directly.

Current version: **v0.1.44**

## Download

| File | For | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64 (Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11 (runs through WSL2) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | checksums of both packages | |

The Windows zip is all-in-one: it already contains the Linux miner, so you only need the zip.

## Verify the download

Put the package(s) and `SHA256SUMS` in the same folder, then:

```bash
# Linux (add --ignore-missing if you downloaded only one package)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows (Command Prompt): compare the output with the SHA-256 in the table above
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

Each file must report `OK` (on Windows: the same 64 characters as in the table). If it does not match, do not use it.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background
```

| Command | What it does |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background` | First start: installs what is needed, then mines in the background |
| `./thenewminer --background` | Later starts: your settings are remembered |
| `./thenewminer` | Mine in the foreground |
| `./thenewminer --logs` | Watch the miner (Ctrl+C leaves the view; mining continues) |
| `./thenewminer --stop` | Stop mining |

- `-u` is your TensorCash wallet address, `-p` your pool password, `-w` the worker name shown on the pool
  (default: the machine's host name).
- Settings are remembered in `thenewminer/.miner/` (the file with the password is readable only by you).
- The first start downloads the model (about 8.1 GiB, once), installs the GPU engine for your card and runs a
  self-check on the card, then mines. Lines like `[ RATE] ... N/s` mean it is mining.
- The first start may install a few Ubuntu packages with `apt` (needs sudo or root).
- Exactly one NVIDIA GPU per machine in this release (multi-GPU rigs are not supported yet).
- If `tar` says `xz: Cannot exec`, install xz first: `sudo apt-get install -y xz-utils`.
- The `README.md` inside the package is an older internal note; follow this page (no `--proxy` is needed).
- **Upgrade:** `./thenewminer --stop`, unpack the new package in the same place (it unpacks into `thenewminer/` and
  keeps `thenewminer/.miner/` with your settings and the model), then start it again.

## Windows (one click)

1. Unzip the whole `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` (right-click › Extract All) anywhere: any folder, any
   drive; paths with spaces or Chinese characters are fine. Do not run it from inside the zip view.
2. Double-click **`TheNewMiner.bat`**.
   - **First time:** it checks Windows, the driver and the disk, turns on WSL2 if needed (one administrator prompt;
     a restart may be needed once, and setup then continues by itself), shows where the data will go (Enter
     accepts), creates its own Linux (WSL) environment, installs the bundled miner, asks **once** for your wallet
     address (starts with `tc1`), pool password (typed twice; 6–128 letters, digits, `_` or `-`) and worker name
     (Enter = the computer name), downloads the model, then starts mining.
   - **Every time after:** double-click and it starts mining at once with your saved settings. A desktop shortcut
     "TheNewMiner" is also created.

Daily commands (open a Command Prompt in the unzipped folder):

| Command | What it does |
|---|---|
| `TheNewMiner.bat` | Install if needed, then mine in this window (closing the window stops mining) |
| `TheNewMiner.bat background` | Mine without a window |
| `TheNewMiner.bat logs` | Watch the miner (Ctrl+C leaves the view; mining continues) |
| `TheNewMiner.bat status` | Whether it runs, and the last rates |
| `TheNewMiner.bat stop` | Stop mining |
| `TheNewMiner.bat setup` | Enter the wallet / password / worker name again |
| `TheNewMiner.bat uninstall` | Remove the miner, its data folder and the shortcut (asks first) |

- **Where the data goes:** the miner and the model live in a data folder chosen at the first install (by default
  `TheNewMinerData` at the root of the drive you unzipped to, e.g. `D:\TheNewMinerData`), not in the unzipped
  folder. About 20 GB free is needed there.
- **Upgrade:** unzip the new version anywhere and double-click its `TheNewMiner.bat`. It finds the installed miner,
  keeps your settings and the model, and installs the new version without questions. The old folder can be deleted.
- **Uninstall:** `TheNewMiner.bat uninstall`.
- The on-screen language follows Windows (English / 简体中文 / 繁體中文). To choose it, add `-Lang en` (or `zh-Hans`,
  `zh-Hant`) to any command, e.g. `TheNewMiner.bat -Lang en`; it applies to that run.

## Proxy (optional)

The miner connects to the pool directly. If your network needs a proxy:

```bash
# Linux (remembered until changed)
./thenewminer --proxy <IP>[:<PORT>]            # TCP relay (port 443 if omitted)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5, also socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # HTTP CONNECT proxy
./thenewminer --proxy none                     # back to a direct connection
```

These commands also start the miner: stop the miner first, and add `--background` to keep it in the background.

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

TLS always runs end to end with the pool, so the proxy cannot read the connection. On Windows, a proxy app running
on the same PC is usually not reachable as `127.0.0.1` from WSL (default WSL networking): use the PC's LAN IP and
allow LAN connections in the app.

## Requirements

| Item | Requirement |
|---|---|
| GPU | Exactly one NVIDIA GeForce RTX 30 / 40 / 50 series card, 10 GB VRAM or more |
| Driver | NVIDIA **580 or newer** (`nvidia-smi` must work). On Windows, install it on Windows, never inside WSL |
| Linux | x86_64 Ubuntu 22.04, 24.04 or 26.04 (other distributions are not supported yet) |
| Windows | Windows 10 21H2 or newer, or Windows 11, 64-bit; virtualization (Intel VT-x / AMD SVM) on in the BIOS |
| Disk | about 20 GB free |
| RAM | 4 GB or more |
| Model | about 8.1 GiB, downloaded once on the first start (parallel, multi-mirror) and reused afterwards |

## Reference hashrates

The best real-pool rates measured on our own rigs (between 2026-10-14 and 2026-10-23). They are **up to** figures,
not a promise: cards of the same model differ by about 5–8% with the individual chip, cooling, clocks and power limit,
and the rate drops if the card also drives a display or other programs use the GPU.

| GPU | Hashrate |
|---|---|
| RTX 5090 | up to ~99 N/s |
| RTX 4090 | up to ~70 N/s |
| RTX 3080 10 GB | up to ~22 N/s |

## Test results

Real-pool tests of v0.1.44 on our test rigs on 2026-10-23 (one GPU per machine). A = accepted shares, R = rejected
shares.

| Date | Card | OS | Package | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | sealed-r4 | 65.58 N/s (median, 16 min) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | sealed-r4 | 93.45 N/s (steady mining, 44 min; 90.39 including job changes) | 31 / 0 (+1 stale at stop) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | windows-wsl2-r5 | ~22 N/s (operator's own run) | n/a |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | sealed-r4 | 21.04 N/s (last 10 min of 23 min mining) | 3 / 0 |

RTX 3080 on Linux: install 6/6 steps OK, no invalid windows, clean stop. Log excerpt:
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt).

## Screenshots

From our test rigs, with earlier versions: v0.1.41 on the RTX 4090 and an early build on the RTX 5090. These are live
readings; your numbers depend on the card.

**RTX 4090, v0.1.41:** 69–75 N/s per reading, mostly ~70 (10-minute average ~68), 9 shares accepted, 0 rejected, ~445 W.

![TheNewMiner v0.1.41 mining on an RTX 4090](screenshots/rtx4090.png)

**RTX 5090, early build:** 100.83 N/s (live average over completed windows), ~575 W.

![An early TheNewMiner build mining on an RTX 5090](screenshots/rtx5090.png)

## FAQ

**Windows says "Windows protected your PC" / "unknown publisher".**
The kit is not code-signed. Click **More info**, then **Run anyway**.

**My antivirus flags or deletes the miner.**
Microsoft Defender and other antivirus programs often flag cryptocurrency miners as "potentially unwanted
applications", even a legitimate one you chose to run, because malware frequently hides miners. If a file is quarantined,
open Windows Security › Virus & threat protection › Protection history, select the TheNewMiner item and choose
**Allow** (or add the unzipped folder to Exclusions). Only do this for a package downloaded from this page whose
SHA-256 matches.

**The first start takes a long time.**
The first start downloads the ~8.1 GiB model and runs a GPU self-check on your card; on Windows it may also set up
WSL2. This happens once. Later starts begin mining much sooner.

**Where are the logs?**
Linux: `./thenewminer --logs`, or the file `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat logs` (and `TheNewMiner.bat status` for the last rates).

**HiveOS?**
A HiveOS package is **coming soon**; it is still in testing.

## Security note

The miner and the GPU engine are shipped encrypted (ChaCha20-Poly1305) inside the package; the miner decrypts them on
your machine when it starts. This raises the cost of copying them, but it is **not unbreakable**: anyone with
administrator access to a machine running the miner can, with enough effort, recover the key or the decrypted code.
Your wallet address and pool password are stored on your own machine and sent only to the pool.
