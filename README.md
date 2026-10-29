# TheNewMiner

TheNewMiner is a GPU miner for **TensorCash** on NVIDIA GeForce RTX 30, 40 and 50 series cards with 10 GB of VRAM or more; the pool **`tsc-wss.the-new.win`** is built in.

**English** · [简体中文](docs/i18n/README.zh-CN.md) · [繁體中文](docs/i18n/README.zh-TW.md) · [Русский](docs/i18n/README.ru.md) · [Español](docs/i18n/README.es.md) · [Português (Brasil)](docs/i18n/README.pt-BR.md) · [Français](docs/i18n/README.fr.md) · [Deutsch](docs/i18n/README.de.md) · [Türkçe](docs/i18n/README.tr.md) · [Tiếng Việt](docs/i18n/README.vi.md) · [한국어](docs/i18n/README.ko.md) · [日本語](docs/i18n/README.ja.md)

## Download

Current version: **v0.1.47** · Pool website: [tsc.the-new.win](https://tsc.the-new.win/#/home)

| System | Download | Then |
|---|---|---|
| Linux x86_64 | [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | or use the one-click install below |
| Windows 10 / 11 | [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | unzip it, double-click `TheNewMiner.bat` |
| HiveOS (experimental) | [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json) | import the flight sheet (see HiveOS below) |

Checksums: [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS). How to check them is in the
[guide](docs/GUIDE.md). If a file does not match, do not use it.

## Before you start: create your pool account

Mining needs a pool account: your TSC payout address (wallet) and a pool password that you choose on the pool website.
The site has an English version (use its language menu).

1. Open [tsc.the-new.win](https://tsc.the-new.win/#/home) and go to **Start mining** (开始挖矿).
2. Under **1. Bind Wallet**, enter your **TSC payout address** (`tc1...`; check it carefully: mining income sent to a
   wrong address cannot be recovered), choose a **Pool Password** (6–128 characters) and enter it again under
   **Confirm Password**.
3. Click **Apply to join pool**. The page confirms that the wallet is bound; the account works at once.
4. Use the same wallet and password in the miner: `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'` on Linux, the account
   window of `TheNewMiner.bat` on Windows, or the flight sheet on HiveOS. About 3–5 minutes after it starts mining,
   your rig appears under **My Miners** on the pool website.

The pool cannot recover a forgotten pool password, and the payout address cannot be changed later. The pool password is
not your wallet's private key: never give your private key to anyone.

## One-click install (Linux)

Replace `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` and `<RIG_NAME>` (including the `<` `>`) at the end of this command with
your wallet, your pool password and a name for this rig, and paste it into a terminal. It downloads the Linux package
and `SHA256SUMS`, checks the SHA-256, unpacks the miner into `~/thenewminer` and starts it in the background:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.47 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

A download error or a SHA-256 mismatch never starts anything. Keep the single quotes around the password (a `'` in the
password is written `'\''`). On a single-GPU machine the miner checks the wallet and the password with the pool before it
downloads the model (about 8.1 GiB, once). Then `./thenewminer --logs` shows it mining and `./thenewminer --status` says whether it runs, or
why it stopped. To upgrade, run `~/thenewminer/thenewminer --stop` first, then the same command: your settings and the
model are kept. Everything else (daily commands, upgrades, autostart, what to do when it does not mine) is in the
[guide](docs/GUIDE.md).

## Windows

1. Unzip the whole `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` (right-click › Extract All) anywhere.
2. Double-click **`TheNewMiner.bat`**. The first time, ONE window asks for your pool account (wallet, pool password,
   worker name); after that nothing else is asked: it sets up WSL2 and Ubuntu, installs the bundled miner, downloads the
   model and starts mining. Later double-clicks start mining at once. Windows may first say "Windows protected your PC"
   (the kit is not code-signed): click **More info**, then **Run anyway**. Turning on WSL asks for administrator rights
   once and may need one restart; setup then continues by itself.
3. An earlier version is found and upgraded in place, with your settings and the model.

If it stops or mines slowly, see [Windows troubleshooting](docs/WINDOWS-TROUBLESHOOTING.md) (DNS, Windows drives, the
VRAM that browsers and Docker Desktop use, the clock).

## HiveOS (experimental)

1. **Wallets:** add a wallet with the coin `TSC` and your TensorCash address (`tc1...`).
2. Open [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json), copy its content and replace
   `<YOUR_WALLET>` (keep `.%WORKER_NAME%` after it) and `<YOUR_POOL_PASSWORD>`. In HiveOS choose Flight Sheets ›
   **Import from clipboard**, paste it, select your `TSC` wallet and save.
3. Apply the flight sheet to your rig. It installs `thenewminer-0.1.47.tar.gz` and starts mining.

Manual setup, upgrades and a rig that cannot reach GitHub: see the [guide](docs/GUIDE.md).

## What's new in v0.1.47

- **Easier install:** on a single-GPU machine the miner checks your wallet and pool password with the pool **before**
  the 8.1 GiB model download, so a typo is found in seconds. A wallet address with a typo (its checksum does not match) or the example
  address is refused with what is wrong.
- **Windows: credentials asked once,** in one window at the start, before anything long. An install made by an earlier
  version, also in another WSL distribution, is found and **upgraded in place** (settings and model kept). If Ubuntu
  cannot be downloaded from Microsoft, setup takes Ubuntu's own WSL image from a mirror in China and uses it only if its
  SHA-256 matches.
- **No apt on a stock Ubuntu:** Ubuntu 22.04 / 24.04 (WSL's included) already has everything the miner needs, so nothing
  is installed. Where something is missing, the first start lists it before installing.
- **A package on a Windows drive** (such as `/mnt/d` in WSL) is refused at once, with a ready-to-paste line that copies
  it to your Linux home folder and starts it there.
- **Clear network messages:** when DNS inside WSL does not work, setup stops at that step with the fix; a miner that
  cannot reach the internet or the pool says what to check.
- **The model is reused:** an upgrade into a new folder finds the model of an earlier install (same size and SHA-256)
  and does not download it again.
- **`--status` says why** the miner stopped and how to fix it, also after a `--background` start.
- **HiveOS:** every failure now waits and retries with growing pauses (one to ten minutes) and shows its reason in the
  log and on the dashboard.

Full notes: [`docs/releases/RELEASE-NOTES-v0.1.47.md`](docs/releases/RELEASE-NOTES-v0.1.47.md).

## Known issues

- **10 GB cards on Windows need about 9.3 GB of free VRAM for full speed.** With less, the miner uses a slower layout
  (an RTX 3080 10 GB dropped from 21–22 to about 9 N/s). A browser with hardware acceleration or Docker Desktop can take
  that memory: close them (or turn off the browser's hardware acceleration), then start mining again.
- **Windows clock:** if the miner reports a clock error, open Windows Settings › Time & language › Date & time, turn on
  "Set time zone automatically" and "Set the time automatically", click **Sync now**, then run `wsl --shutdown` and start again.

## Hashrates

The best real-pool rates measured on our own rigs. They are **up to** figures, not a promise: cards of the same model
differ by about 5–8%, and the rate drops if the card also drives a display or other programs use the GPU.

| GPU | Hashrate |
|---|---|
| RTX 5090 | up to ~99 N/s |
| RTX 4090 | up to ~70 N/s |
| RTX 3080 10 GB | up to ~22 N/s |

Dated measurements per version: [test results](docs/TEST-RESULTS.md). The project's milestones:
[development record](docs/DEVELOPMENT-RECORD.md).

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
