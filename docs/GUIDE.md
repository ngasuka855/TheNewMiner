**English** · [简体中文](i18n/GUIDE.zh-CN.md) · [繁體中文](i18n/GUIDE.zh-TW.md) · [Русский](i18n/GUIDE.ru.md) · [Español](i18n/GUIDE.es.md) · [Português (Brasil)](i18n/GUIDE.pt-BR.md) · [Français](i18n/GUIDE.fr.md) · [Deutsch](i18n/GUIDE.de.md) · [Türkçe](i18n/GUIDE.tr.md) · [Tiếng Việt](i18n/GUIDE.vi.md) · [한국어](i18n/GUIDE.ko.md) · [日本語](i18n/GUIDE.ja.md)

# TheNewMiner guide

Everything beyond the first start: checking the download, daily use on Linux, Windows and HiveOS, upgrades, what to do
when it does not mine, and the FAQ. Downloads and the quick start are on the [main page](../README.md); Windows problems
have their own page: [Windows troubleshooting](WINDOWS-TROUBLESHOOTING.md).

## Verify the download

| File | SHA-256 |
|---|---|
| [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | `86c215dc9401209b8d2a2346252ed5cc1901527b6a51520372e3128583c63656` |
| [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | `45a459646713bef9f8285848486c0a4bd66dee65e8c63090c8d3345c998b2659` |
| [`thenewminer-0.1.47.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz) (HiveOS) | `6b2515c5bf23f60daa236ff4a8b165ce5e55265d7eecac9dd408896e1e31b784` |

The Windows zip is all-in-one: it already contains the Linux miner, so you only need the zip. Put the package(s) and
[`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS) in the same folder, then:

```bash
# Linux (--ignore-missing: only the packages you downloaded are checked)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Command Prompt): compare the output with the SHA-256 in the table above
certutil -hashfile TheNewMiner-v0.1.47-windows-wsl2-r3.zip SHA256
```

Each file must report `OK` (on Windows: the same 64 characters as in the table). If it does not match, do not use it.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Command | What it does |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | First start: installs what is needed, then mines in the background |
| `./thenewminer --background` | Later starts: your settings are remembered |
| `./thenewminer` | Mine in the foreground (Ctrl+C stops) |
| `./thenewminer --logs` | Watch the miner (Ctrl+C leaves the view; mining continues) |
| `./thenewminer --status` | Whether it is mining, its last rate and the accepted / rejected shares; after a stop, why it stopped and how to fix it |
| `./thenewminer --stop` | Stop mining |
| `./thenewminer --help` | All options |

- `-u` is your TensorCash wallet address, `-p` your pool password, `-w` the worker name shown on the pool
  (default: the machine's host name; 1–64 letters, digits, `_`, `-` or `.`).
- The wallet address is checked before anything starts: an address with a typo (its checksum does not match) or the
  example address is refused, with what is wrong.
- The pool password: 6–128 characters, any visible character, no spaces. Always put it in single quotes on the
  command line, e.g. `-p 'my$pass!'` (a `'` inside single quotes is written `'\''`).
- Settings are remembered in `thenewminer/.miner/` (the file with the password is readable only by you). The wallet
  and the password are taken off the command line at once, so they do not show in `ps` (your shell may still keep the
  command in its history).
- On the first start of a single-GPU machine the miner checks the wallet and the password with the pool **before** the
  model download, so a typo is found in seconds (a multi-GPU rig finds it when mining starts). If the pool rejects them,
  the miner says so and stops: check them and start again with `-u` and `-p`.
- The first start downloads the model (about 8.1 GiB, once), installs the GPU engine for your card and runs a
  self-check on the card, then mines. Lines like `[ RATE] ... N/s` mean it is mining. The log is
  `thenewminer/.miner/thenewminer.log`.
- A stock Ubuntu 22.04 / 24.04 (WSL's included) already has every library the miner needs, so nothing is installed.
  Where one is missing (Docker images, minimal systems), the first start on Ubuntu, Debian and HiveOS lists it and
  installs it with `apt` (needs sudo or root); on other systems it names it for you to install.
- Multi-GPU rigs: supported, each GPU mines; this release was tested with one GPU per machine.
- In Docker or other containers with little shared memory, the miner runs its program files from
  `thenewminer/.miner/run/` on disk and says so. To choose another folder, start it with
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (a folder only you can write to: `chmod 700 <folder>`).
- **Upgrade:** `./thenewminer --stop`, unpack the new package in the same place (it unpacks into `thenewminer/` and
  keeps `thenewminer/.miner/` with your settings and the model), then start it again. If the old version is still
  running, the new one refuses to start and asks you to stop it first. Unpacked into a new folder instead, it finds the
  model of the old one (same size and SHA-256) and reuses it: no second 8.1 GiB download. Give `-u -p -w` again there.

### When it does not mine

`./thenewminer --status` says why the miner stopped and how to fix it. The most common messages:

| You see | What to do |
|---|---|
| `bash: ./thenewminer: Permission denied` | The folder does not allow programs (a `noexec` mount such as some `/tmp`, or a USB stick). Unpack it in your home folder: `cd ~ && tar -xJf <the package>.tar.xz && cd thenewminer` |
| `this package is on a FAT (vfat) drive` (or exFAT / NTFS, or `Windows (DrvFs, 9p)`: a Windows drive such as `/mnt/d` in WSL) | Such a drive cannot keep the miner's private settings (mode 600). Paste the `mkdir -p ~/thenewminer && cp -a ...` line it prints: it copies the folder to your Linux home folder and starts it there |
| `this system has no xz` | `sudo apt-get install -y xz-utils` (as root: without `sudo`), then start again |
| `This system is missing what TheNewMiner needs` | Run the `sudo apt-get install -y ...` line it prints, then start again |
| `No NVIDIA GPU found on this machine` | No NVIDIA card is seen. Check the card is seated and powered; on a cloud machine choose one with an NVIDIA GPU |
| `NVIDIA driver ... is too old` | Install the driver it names (580 or newer), reboot, start again (see "Update your NVIDIA driver" below) |
| `the pool rejected the wallet / password` | Check `-u` and `-p` and that the account exists on the pool website, then `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' --background` |
| `no download source answered` | The model download cannot reach the internet. Behind a proxy: `https_proxy=http://<HOST>:<PORT> ./thenewminer --background`; or copy the model (see "Already have the model?" below) |
| `cannot reach the pool` / `pool disconnected` | Check the internet connection, the DNS and that the firewall allows outbound TCP 443; if your network blocks the pool, use `--proxy` (see "Proxy" below). It reconnects by itself |

### Start after a reboot (optional)

The miner does not start by itself after a reboot (HiveOS does this for you). On Linux, either add a `crontab` line:

```bash
crontab -e      # then add this line (your folder):
@reboot cd /home/<YOU>/thenewminer && ./thenewminer --background
```

or a systemd user service (`loginctl enable-linger <YOU>` once, so it starts without a login):

```ini
# ~/.config/systemd/user/thenewminer.service, then: systemctl --user enable --now thenewminer
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

Exit status 77 means the pool rejected the wallet / password: restarting cannot help, so the service stops there.

## Windows

1. Unzip the whole `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` (right-click › Extract All) anywhere: any folder, any
   drive; paths with spaces or Chinese characters are fine. Do not run it from inside the zip view.
2. Double-click **`TheNewMiner.bat`**.
   - **First time:** ONE window asks for your pool account first: the wallet address (starts with `tc1`; its checksum
     is checked, and the example shown is not a real wallet), the pool password (6–128 characters, any visible
     character, no spaces) and the worker name (the computer name is filled in). OK stays grey until all three are
     right. After OK and Yes nothing else is asked: it checks Windows, the driver and the disk, turns on WSL2 if needed
     (one administrator prompt; a restart may be needed once, and setup then continues by itself), shows where the
     data will go, sets up its own Linux (WSL) environment, installs the bundled miner, downloads the model, then
     starts mining. Without a desktop (over SSH) the same questions come in the console.
   - **Every time after:** double-click and it starts mining at once with your saved settings. A desktop shortcut
     "TheNewMiner" is also created. Double-clicking again while it is already mining tells you so and shows the log.
   - **If setup stops,** the window says at which step and what to do, and `TheNewMiner.bat` shows it again. See
     [Windows troubleshooting](WINDOWS-TROUBLESHOOTING.md).

Daily commands (open a Command Prompt in the unzipped folder):

| Command | What it does |
|---|---|
| `TheNewMiner.bat` | Install if needed, then mine in this window (closing the window stops mining) |
| `TheNewMiner.bat background` | Mine without a window |
| `TheNewMiner.bat logs` | Watch the miner (Ctrl+C leaves the view; mining continues) |
| `TheNewMiner.bat status` | Whether it runs, and the last rates |
| `TheNewMiner.bat stop` | Stop mining |
| `TheNewMiner.bat setup` | Enter the wallet / password / worker name again (Cancel keeps them) |
| `TheNewMiner.bat uninstall` | Remove the miner, its data folder and the shortcut (asks first) |

- **Where the data goes:** the miner and the model live in a data folder chosen at the first install (by default
  `TheNewMinerData` at the root of the drive you unzipped to, e.g. `D:\TheNewMinerData`), not in the unzipped
  folder. About 20 GB free is needed there. To choose another folder: `TheNewMiner.bat setup -DataDir <folder>`.
- **Ubuntu:** setup installs Ubuntu 24.04 with `wsl --install`. When that download cannot connect, it downloads
  Ubuntu's own WSL image `ubuntu-24.04.5-wsl-amd64.wsl` from a public mirror in China (Tsinghua TUNA, then USTC, then
  Aliyun) and uses it only if its SHA-256 is exactly `bb415d824822c4b878125729af451a5d18fb13d1cf5cbed9a7393ad64ac6039e`.
- If the pool rejects the wallet or the password, the window says so and mining stops: run `TheNewMiner.bat setup`
  to enter them again.
- **Upgrade:** unzip the new version anywhere and double-click its `TheNewMiner.bat`. It finds the installed miner,
  keeps your settings and the model, and installs the new version without questions. An install made by an earlier
  version or by hand inside another WSL distribution (for example `Ubuntu-24.04`) is found and upgraded where it is: no
  second install, no new Ubuntu, no second model download. If it cannot be upgraded there, setup stops and says why.
  The old folder can be deleted.
- **Uninstall:** `TheNewMiner.bat uninstall`. It never removes a WSL distribution of your own.
- The Windows kit speaks English.

## HiveOS (experimental)

HiveOS support is **experimental** (since v0.1.45, which ran on a real HiveOS rig; the changes since v0.1.46 have not
run on one yet). It uses the HiveOS custom-miner package `thenewminer-0.1.47.tar.gz` (its SHA-256 is in the table under
"Verify the download").

Quick setup (import the flight sheet):

1. **Wallets:** add a wallet with the coin `TSC` and your TensorCash address (`tc1...`). This is needed once: HiveOS
   needs a `TSC` wallet selected, and the miner uses the address you put in the flight sheet.
2. Open [`hiveos/flightsheet-thenewminer.json`](../hiveos/flightsheet-thenewminer.json) and copy its content. Replace
   `<YOUR_WALLET>` with your wallet address (keep `.%WORKER_NAME%` after it) and `<YOUR_POOL_PASSWORD>` with your pool
   password (in the JSON, a `"` in the password is written `\"` and a `\` is written `\\`). In HiveOS choose Flight
   Sheets › **Import from clipboard**, paste it, select your `TSC` wallet and save.
3. Apply the flight sheet to your rig. The rig downloads the package and starts mining.

Manual setup instead: create a flight sheet with Coin `TSC`, Wallet: your `TSC` wallet, Pool: "Configure in miner",
Miner: "Custom", click **Setup Miner Config** and fill in:

| Field | Value |
|---|---|
| Miner name | `thenewminer` (filled in from the installation URL) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` |
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
- **When something fails** (for example the pool rejects the wallet or the password), the miner log and the HiveOS
  dashboard show the reason, and the rig retries with growing pauses (one to ten minutes). Fix the flight sheet and
  apply it: that restarts the miner at once.
- Logs: the miner screen in HiveOS (`miner` in the rig's shell) or `/var/log/miner/custom/custom.log`.
- **Upgrade:** change both version numbers in the installation URL (for example
  `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`) and apply the flight sheet. HiveOS installs only when the file name in
  the URL changes.
- **The first start** downloads the model and installs the GPU engine, which takes a few minutes.
- **Already have the model?** Copy it to `/home/user/` on the rig before the first start (see the next section).

**If the rig cannot reach GitHub:** download `thenewminer-0.1.47.tar.gz` on a PC (check its SHA-256), copy it to the rig
and unpack it where HiveOS keeps custom miners. Then apply the flight sheet as usual: HiveOS reports "Already
installed" and does not download it. Keep the file name exactly `thenewminer-0.1.47.tar.gz` (a browser may add " (1)")
and keep the installation URL in the flight sheet unchanged: HiveOS compares that name.

```bash
# on the PC
scp thenewminer-0.1.47.tar.gz user@<RIG_IP>:/tmp/
# on the rig, as root (Hive Shell, or SSH and then sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.47.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.47.tar.gz
```

## Already have the model? Copy it

The miner needs one 8.1 GiB model file: `Qwen3-8B-Q8_0-full-v2.gguf` (`8709518112` bytes, SHA-256
`408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`). If it is already on this machine, or on another
computer on your network, copy it instead of downloading it again.

Before it downloads anything, the miner looks for the file by its exact name and size: in its own model folder, in
earlier TheNewMiner folders, in your home folder (and one or two folders below it, or a `models` folder there), in
the home folders under `/home`, `/data`, `/mnt/*`, `/media/*`, `/opt`, `/srv`, `/workspace`, `/var/lib`, and in the
Hugging Face / ModelScope caches. If nothing is found there, it spends up to 20 seconds searching those folders on the
same disk (other mounted disks are checked by name only) for any `.gguf` of exactly that size. A file it finds is used
**only if its SHA-256 matches**; it then links it (same disk) or copies it into its own model folder, so every later
version finds it at once. You can also point at it directly: `./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`.

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
never downloads it again. HiveOS's disk is small: run `disk-expand` first if `df -h /hive/miners` shows less than 12 GB
free.

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

**Windows:** run `TheNewMiner.bat setup`. It opens the account window with your worker name filled in; enter the
wallet and the pool password, then start it as usual (double-click `TheNewMiner.bat`).

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
- **Debian:** Debian's own driver packages are older than 580 (535 in Debian 12, 550 in Debian 13). Install a newer
  driver from NVIDIA's repository as described in the
  [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html),
  then reboot.
- **Windows:** update with the [NVIDIA app](https://www.nvidia.com/en-us/software/nvidia-app/) (formerly GeForce
  Experience), or download the driver for your card from [nvidia.com](https://www.nvidia.com/en-us/drivers/) (580 or
  newer). WSL2 uses this Windows driver: never install a Linux NVIDIA driver inside WSL.
- **HiveOS:** as root (Hive Shell, or SSH and then `sudo -i`), run `nvidia-driver-update --list` to see the versions,
  then for example `nvidia-driver-update 595` (any 580 or newer), then `sreboot`.

## FAQ

**Windows says "Windows protected your PC" / "unknown publisher".**
The kit is not code-signed. Click **More info**, then **Run anyway**.

**My antivirus flags or deletes the miner.**
Microsoft Defender and other antivirus programs often flag cryptocurrency miners as "potentially unwanted
applications", even a legitimate one you chose to run, because malware frequently hides miners. If a file is
quarantined, open Windows Security › Virus & threat protection › Protection history, select the TheNewMiner item and
choose **Allow** (or add the unzipped folder to Exclusions). Only do this for a package downloaded from this page whose
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

**My RTX 3080 (10 GB) mines much slower on Windows than on Linux.**
On Windows other programs share the VRAM. See [Windows troubleshooting](WINDOWS-TROUBLESHOOTING.md): close browsers
with hardware acceleration and Docker Desktop.
