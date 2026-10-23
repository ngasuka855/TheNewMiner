**English** · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

# TheNewMiner

TheNewMiner is a GPU miner for **TensorCash**. It runs on NVIDIA GeForce RTX 30, 40 and 50 series cards with
**10 GB or more of VRAM** and mines on the pool **`tsc-wss.the-new.win`**, which is built in: you do not enter a pool
address, and it connects directly.

Current version: **v0.1.46** · Pool website: [tsc.the-new.win](https://tsc.the-new.win/#/home)

## Before you start: create your pool account

Mining needs a pool account: your TSC payout address (wallet) and a pool password that you choose on the pool website.
The site has an English version (use its language menu).

1. Open [tsc.the-new.win](https://tsc.the-new.win/#/home) and go to **Start mining** (开始挖矿).
2. Under **1. Bind Wallet**, enter your **TSC payout address** (`tc1...`; check it carefully: mining income sent to a
   wrong address cannot be recovered), choose a **Pool Password** (6–128 characters) and enter it again under
   **Confirm Password**.
3. Click **Apply to join pool**. The page confirms that the wallet is bound; the account works at once.
4. Use the same wallet and password in the miner: `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'` on Linux, the questions
   of `TheNewMiner.bat` on Windows, or the flight sheet on HiveOS. About 3–5 minutes after the start, your rig appears
   under **My Miners** on the pool website.

The pool cannot recover a forgotten pool password, and the payout address cannot be changed later. The pool password is
not your wallet's private key: never give your private key to anyone.

## What's new in v0.1.46

- **Fewer first-start failures on RTX 30 series cards and HiveOS.** The one-time GPU check now uses GPU code
  precompiled for every supported card family and already runs once during the installation; the GPU compile cache is
  kept between starts and upgrades. If the check still times out on an RTX 30 series card, the miner does that step on
  the CPU instead and keeps mining. If the card's configuration cannot be prepared, the miner says why in one `Reason:`
  line (exit code 78).
- **HiveOS:** when the miner cannot prepare its configuration for the GPU on a first start (exit code 78), the rig now
  waits and retries with growing pauses (one to ten minutes) instead of restarting every few seconds.
- **Smoother waves:** when the pool's checkpoint changes in the middle of a wave, the next wave's work is now prepared
  again in the background. On an RTX 5090 this removed the waits at that moment (+0.3% sustained rate, identical
  results).
- **Nothing left in memory after a rejected password:** `--status` and `--logs` after a stop no longer leave the
  unpacked program files (about 1.5 GB) in memory.
- **Already have the model?** Copy it instead of downloading it again: see the section below.

The changes of v0.1.45 are in [`RELEASE-NOTES-v0.1.45.md`](RELEASE-NOTES-v0.1.45.md).

## Download

| File | For | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.46-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/TheNewMiner-v0.1.46-linux-x86_64-sealed.tar.xz) | Linux x86_64 | `3585360e80993176475b6e400258888b4ddf8cb130ff97574e74e15cf1e248a8` |
| [`TheNewMiner-v0.1.46-windows-wsl2-r1.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/TheNewMiner-v0.1.46-windows-wsl2-r1.zip) | Windows 10 / 11 (runs through WSL2) | `dd9cb395e4012bfb81a5b9f0a1fdfd196955b781265a4a4e0d67266bd67b6a9e` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/SHA256SUMS) | checksums of all packages | |

The Windows zip is all-in-one: it already contains the Linux miner, so you only need the zip.

## Verify the download

Put the package(s) and `SHA256SUMS` in the same folder, then:

```bash
# Linux (--ignore-missing: only the packages you downloaded are checked)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Command Prompt): compare the output with the SHA-256 in the table above
certutil -hashfile TheNewMiner-v0.1.46-windows-wsl2-r1.zip SHA256
```

Each file must report `OK` (on Windows: the same 64 characters as in the table). If it does not match, do not use it.

## One-click install (Linux)

Create your pool account first (see "Before you start" above). Then replace `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` and
`<RIG_NAME>` (including the `<` `>`) at the end of this command with your wallet, your pool password and a name for this
rig, and paste it into a terminal. It downloads the Linux package and `SHA256SUMS` of this version (with `curl`, or `wget` if there is no
`curl`), checks the SHA-256, unpacks the miner into `~/thenewminer` and starts it in the background:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.46 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

It stops at the first step that fails: a download error or a SHA-256 mismatch never starts anything (the check guards
against a damaged download; both files come from this page). Keep the single quotes around the password: inside them
every character is safe (a `'` in the password is written `'\''`). If `tar` says `xz: Cannot exec`, install xz
(`sudo apt-get install -y xz-utils`) and run the command again.

To upgrade a version already in `~/thenewminer`, first run `~/thenewminer/thenewminer --stop`, then this command (your
settings and the model are kept). If your earlier version is in another folder, stop it there first, or upgrade it
there as described under Linux (otherwise the model is downloaded again).

## Linux

```bash
tar -xJf TheNewMiner-v0.1.46-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Command | What it does |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | First start: installs what is needed, then mines in the background |
| `./thenewminer --background` | Later starts: your settings are remembered |
| `./thenewminer` | Mine in the foreground (Ctrl+C stops) |
| `./thenewminer --logs` | Watch the miner (Ctrl+C leaves the view; mining continues) |
| `./thenewminer --status` | Whether it is mining, its last rate and the accepted / rejected shares |
| `./thenewminer --stop` | Stop mining |
| `./thenewminer --help` | All options |

- `-u` is your TensorCash wallet address, `-p` your pool password, `-w` the worker name shown on the pool
  (default: the machine's host name; 1–64 letters, digits, `_`, `-` or `.`).
- The pool password: 6–128 characters, any visible character, no spaces. Always put it in single quotes on the
  command line, e.g. `-p 'my$pass!'` (a `'` inside single quotes is written `'\''`).
- Settings are remembered in `thenewminer/.miner/` (the file with the password is readable only by you). The wallet
  and the password are taken off the command line at once, so they do not show in `ps` (your shell may still keep the
  command in its history).
- If the pool rejects the wallet or the password, the miner says so and stops.
  Check them and start again with `-u` and `-p`.
- The first start downloads the model (about 8.1 GiB, once), installs the GPU engine for your card and runs a
  self-check on the card, then mines. Lines like `[ RATE] ... N/s` mean it is mining.
- On Ubuntu and Debian the first start may install a few missing system libraries with `apt` (needs sudo or root); on
  other systems it names them for you to install.
- Multi-GPU rigs: supported, each GPU mines; this release was tested with one GPU per machine.
- If `tar` says `xz: Cannot exec`, install xz first: `sudo apt-get install -y xz-utils`.
- In Docker or other containers with little shared memory, the miner runs its program files from
  `thenewminer/.miner/run/` on disk and says so. To choose another folder, start it with
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (a folder only you can write to: `chmod 700 <folder>`).
- **Upgrade:** `./thenewminer --stop`, unpack the new package in the same place (it unpacks into `thenewminer/` and
  keeps `thenewminer/.miner/` with your settings and the model), then start it again. If the old version is still
  running, the new one refuses to start and asks you to stop it first.

## Windows (one click)

1. Unzip the whole `TheNewMiner-v0.1.46-windows-wsl2-r1.zip` (right-click › Extract All) anywhere: any folder, any
   drive; paths with spaces or Chinese characters are fine. Do not run it from inside the zip view.
2. Double-click **`TheNewMiner.bat`**.
   - **First time:** it checks Windows, the driver and the disk, turns on WSL2 if needed (one administrator prompt;
     a restart may be needed once, and setup then continues by itself), shows where the data will go (Enter
     accepts), creates its own Linux (WSL) environment, installs the bundled miner, asks **once** for your wallet
     address (starts with `tc1`), pool password (typed twice; 6–128 characters, any visible character, no spaces) and
     worker name (Enter = the computer name), downloads the model, then starts mining.
   - **Every time after:** double-click and it starts mining at once with your saved settings. A desktop shortcut
     "TheNewMiner" is also created. Double-clicking again while it is already mining tells you so and shows the log.

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
- If the pool rejects the wallet or the password, the window says so and mining stops: run `TheNewMiner.bat setup`
  to enter them again.
- **Upgrade:** unzip the new version anywhere and double-click its `TheNewMiner.bat`. It finds the installed miner,
  keeps your settings and the model, and installs the new version without questions. The old folder can be deleted.
- **Uninstall:** `TheNewMiner.bat uninstall`.
- The on-screen language follows Windows (English / 简体中文 / 繁體中文). To choose it, add `-Lang en` (or `zh-Hans`,
  `zh-Hant`) to any command, e.g. `TheNewMiner.bat -Lang en`; it applies to that run.

## HiveOS (experimental)

HiveOS support is **experimental** (since v0.1.45, which ran on a real HiveOS rig; the v0.1.46 first-start changes have
not run on one yet). It uses the HiveOS custom-miner package below.

| File | SHA-256 |
|---|---|
| [`thenewminer-0.1.46.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/thenewminer-0.1.46.tar.gz) | `2284c258176e6b372bf56947a343116688e773dee2d479a7f5f125ad9f0a1915` |

Quick setup (import the flight sheet):

1. **Wallets:** add a wallet with the coin `TSC` and your TensorCash address (`tc1...`). This is needed once: HiveOS
   needs a `TSC` wallet selected, and the miner uses the address you put in the flight sheet.
2. Open [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json) and copy its content. Replace
   `<YOUR_WALLET>` with your wallet address (keep `.%WORKER_NAME%` after it) and `<YOUR_POOL_PASSWORD>` with your pool
   password (in the JSON, a `"` in the password is written `\"` and a `\` is written `\\`). In HiveOS choose Flight
   Sheets › **Import from clipboard**, paste it, select your `TSC` wallet and save.
3. Apply the flight sheet to your rig. The rig downloads the package and starts mining.

Manual setup instead: create a flight sheet with Coin `TSC`, Wallet: your `TSC` wallet, Pool: "Configure in miner",
Miner: "Custom", click **Setup Miner Config** and fill in:

| Field | Value |
|---|---|
| Miner name | `thenewminer` (filled in from the installation URL) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/thenewminer-0.1.46.tar.gz` |
| Hash algorithm | leave empty |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (ignored: the pool is built in) |
| Pass | your pool password |
| Extra config arguments | empty, or `--proxy <YOUR_PROXY>` (see the Proxy section) |

- The worker name on the pool is the rig's name in HiveOS: use only letters, digits, `_`, `-` or `.` (no spaces), up to
  64 characters. To use another worker name, add `-w <RIG_NAME>` to the extra config arguments.
- About 12 GB must be free on the HiveOS drive for the model and the miner (check with `df -h /hive/miners`).
- The model (about 8.1 GiB) is downloaded once to `/hive/miners/custom/.thenewminer-models` and kept when you upgrade.
- The wallet and the password reach the miner through its environment, not its command line.
- If the pool rejects the wallet or the password, the miner log says so and the miner waits 10 minutes before HiveOS
  starts it again: fix the flight sheet and apply it (that restarts the miner at once).
- Logs: the miner screen in HiveOS (`miner` in the rig's shell) or `/var/log/miner/custom/custom.log`.
- **Upgrade:** change both version numbers in the installation URL (for example
  `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`) and apply the flight sheet. HiveOS installs only when the file name in
  the URL changes.
- **The first start** downloads the model and installs the GPU engine, which takes a few minutes. If the miner then
  cannot prepare its configuration for the GPU, the log says why in a `Reason:` line and HiveOS retries with growing
  pauses (one to ten minutes).
- **Already have the model?** Copy it to `/home/user/` on the rig before the first start (see the next section).

**If the rig cannot reach GitHub:** download `thenewminer-0.1.46.tar.gz` on a PC (check its SHA-256 in the table
above), copy it to the rig and unpack it where HiveOS keeps custom miners. Then apply the flight sheet as usual:
HiveOS reports "Already installed" and does not download it. Keep the file name exactly `thenewminer-0.1.46.tar.gz`
(a browser may add " (1)") and keep the installation URL in the flight sheet unchanged: HiveOS compares that name.

```bash
# on the PC
scp thenewminer-0.1.46.tar.gz user@<RIG_IP>:/tmp/
# on the rig, as root (Hive Shell, or SSH and then sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.46.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.46.tar.gz
```

## Already have the model? Copy it

The miner needs one 8.1 GiB model file: `Qwen3-8B-Q8_0-full-v2.gguf` (`8709518112` bytes, SHA-256
`408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`). If it is already on this machine, or on another
computer on your network, copy it instead of downloading it again.

Before it downloads anything, the miner looks for the file by its exact name and size: in its own model folder, in
earlier TheNewMiner folders, in your home folder (and one or two folders below it, or a `models` folder there), in
the home folders under `/home`, `/data`, `/mnt/*`, `/media/*`, `/opt`, `/srv`, `/workspace`, `/var/lib`, and in the Hugging Face /
ModelScope caches. If nothing is found there, it spends up to 20 seconds searching those folders on the same disk
(other mounted disks are checked by name only) for any `.gguf` of exactly that size. A file it finds is used **only if its SHA-256 matches**; it then links it (same disk) or copies it into its own
model folder, so every later version finds it at once. You can also point at it directly:
`./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`.

**Linux:** copy it into your home folder, then start the miner as usual. From a Mac or a PC on the same network
(Windows 10 and 11 have `scp` in PowerShell), replace `user` and `<MINER_IP>` with your miner's login and address:

```bash
# on the miner itself
cp /path/to/Qwen3-8B-Q8_0-full-v2.gguf ~/
# or from another computer
scp Qwen3-8B-Q8_0-full-v2.gguf user@<MINER_IP>:~/
```

**HiveOS:** copy it to the rig's `/home/user/` (HiveOS's SSH login is `user`), then apply the flight sheet. The miner
finds it there, checks it and keeps it beside the package (`/hive/miners/custom/.thenewminer-models`), so a new version
never downloads it again. HiveOS's disk is small: run `disk-expand` first if `df -h /hive/miners` shows less than 12 GB free.

```bash
scp Qwen3-8B-Q8_0-full-v2.gguf user@<RIG_IP>:/home/user/
```

**Windows:** after the first `setup` has created TheNewMiner's WSL distribution, copy the file into it from a Command
Prompt (change `D:\Models` to where your copy is). If the download had already started, close the window, run these
commands, then start TheNewMiner again: it finds the copy and skips the download. The copy takes a few minutes and
shows no progress.

```bat
wsl -d TheNewMiner -u root -- mkdir -p ~/thenewminer/models
wsl -d TheNewMiner -u root -- cp /mnt/d/Models/Qwen3-8B-Q8_0-full-v2.gguf ~/thenewminer/models/
```

## Change your wallet, pool password or worker name

The pool binds each payout address to its own pool account for good: the pool page
[tsc.the-new.win](https://tsc.the-new.win) calls the TSC payout address "永久绑定 · 不可修改" (permanently bound, cannot be
modified) and says to switch by using the new address directly on the miner. A different wallet therefore needs its
own pool account and pool password (set them up on the pool page first). If you change the pool password on the pool
page, update it on every miner. The pool cannot recover a forgotten password.

**Linux:** stop the miner, then start it once with the new settings; they replace the remembered ones:

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

Give `-u` and `-p` together: one of them alone does not change the remembered wallet and password. The worker name can
be changed on its own: `./thenewminer -w <RIG_NAME> --background` (after `--stop`).

**Windows:** run `TheNewMiner.bat setup`. It stops the running miner and asks again for the wallet, the pool password
and the worker name. Then start it as usual (double-click `TheNewMiner.bat`).

**HiveOS:** edit the flight sheet: put the new wallet address in "Wallet and worker template"
(`<NEW_WALLET>.%WORKER_NAME%`) and the new pool password in Pass, then save and apply it. The worker name is the rig's
name in HiveOS (or `-w <RIG_NAME>` in the extra config arguments).

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
| GPU | NVIDIA GeForce RTX 30 / 40 / 50 series, 10 GB VRAM or more. Multi-GPU rigs: supported, each GPU mines; tested with one GPU per machine in this release |
| Driver | NVIDIA **580 or newer** (`nvidia-smi` must work). On Windows, install it on Windows, never inside WSL |
| Linux | x86_64 with **glibc 2.34 or newer**, for example Ubuntu 22.04 or newer or Debian 12 or newer. The installer checks what it needs and names anything missing |
| Windows | Windows 10 21H2 or newer, or Windows 11, 64-bit; virtualization (Intel VT-x / AMD SVM) on in the BIOS |
| Disk | about 20 GB free |
| RAM | 4 GB or more |
| Model | about 8.1 GiB, downloaded once on the first start (parallel, multi-mirror) and reused afterwards |

## Update your NVIDIA driver (580 or newer)

The miner needs NVIDIA driver **580 or newer**. Check which one you have:

```bash
nvidia-smi    # the header shows "Driver Version: ..."; it must be 580 or newer
```

- **Ubuntu:** `sudo ubuntu-drivers install` installs the recommended driver (check afterwards that it is 580 or
  newer). To choose the version yourself:
  `sudo apt install nvidia-driver-580` (or newer; RTX 50 series cards need the open variant, e.g.
  `nvidia-driver-580-open`). Then reboot. Use Ubuntu's driver packages or NVIDIA's `.run` installer, never both.
- **Debian:** Debian's own driver packages are older than 580 (535 in Debian 12, 550 in Debian 13). Install a newer driver from NVIDIA's repository as
  described in the [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html),
  then reboot.
- **Windows:** update with the [NVIDIA app](https://www.nvidia.com/en-us/software/nvidia-app/) (formerly GeForce
  Experience), or download the driver for your card from [nvidia.com](https://www.nvidia.com/en-us/drivers/) (580 or
  newer). WSL2 uses this Windows driver: never install a Linux NVIDIA driver inside WSL.
- **HiveOS:** as root (Hive Shell, or SSH and then `sudo -i`), run `nvidia-driver-update --list` to see the versions, then for example
  `nvidia-driver-update 595` (any 580 or newer), then `sreboot`.

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

Real-pool tests on our test rigs (one GPU per machine). A = accepted shares, R = rejected shares. "Steady mining" is
the median rate between job changes.

| Date | Card | OS | Package | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 5090 | Linux | v0.1.46 release build | 95.41 N/s (steady mining, 12 min; 91.70 including job changes) | 10 / 0 (+1 stale; 0 invalid) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.46 release build | 66.23 N/s (median, 10 min) | 2 / 0 |
| 2026-10-23 | RTX 4090 | Linux | v0.1.45 release build | 65.42 N/s (steady mining, 16 min; 62.35 including job changes) | 13 / 0 (0 invalid) |
| 2026-10-23 | RTX 5090 | Linux | v0.1.45 release build | 94.18 N/s (steady mining; 92.24 including job changes) | 17 / 0 (0 invalid) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.44 sealed-r4 | 65.58 N/s (median, 16 min) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | v0.1.44 sealed-r4 | 93.45 N/s (steady mining, 44 min; 90.39 including job changes) | 31 / 0 (+1 stale at stop) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | v0.1.44 windows-wsl2-r5 | ~22 N/s (operator's own run) | n/a |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | v0.1.44 sealed-r4 | 21.04 N/s (last 10 min of 23 min mining) | 3 / 0 |

RTX 3080 on Linux (v0.1.44): install 6/6 steps OK, no invalid windows, clean stop. Log excerpt:
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

**The miner says the pool rejected the wallet / password.**
Check the wallet address and the pool password (the account must exist on the pool website), then start again:
Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. The miner does not
retry wrong credentials by itself.

**Is it mining? Where are the logs?**
Linux: `./thenewminer --status`, `./thenewminer --logs`, or the file `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat status` and `TheNewMiner.bat logs`.

## Report a problem

Open an issue on [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) and fill in the bug report
form: the TheNewMiner version, your system, GPU model(s) and driver, the `./thenewminer --status` output and the last log
lines. Issues are public: **never post your pool password or your wallet's private key**, and remove the password (and,
if you prefer, your wallet address) from anything you paste.

## Security note

The miner and the GPU engine are shipped encrypted (ChaCha20-Poly1305) inside the package; the miner decrypts them on
your machine when it starts. This raises the cost of copying them, but it is **not unbreakable**: anyone with
administrator access to a machine running the miner can, with enough effort, recover the key or the decrypted code.
Your wallet address and pool password are stored on your own machine and sent only to the pool.
