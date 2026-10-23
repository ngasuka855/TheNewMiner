# TheNewMiner v0.1.45

**English** · [简体中文](#简体中文)

TheNewMiner v0.1.45 is out for Linux, Windows and, new, HiveOS (experimental).
This release makes installing, upgrading and troubleshooting easier.
The GPU engine is the same as v0.1.44, so the hashrate does not change.

## What's new

- **Clear message when the pool rejects your wallet or password:** the miner says so in one line and stops instead of
  retrying. Check them and start again with `-u` and `-p` (Windows: `TheNewMiner.bat setup`).
- **Wallet and password hidden:** they no longer show in `ps` or in other users' process lists.
- **`./thenewminer --status`:** shows whether it is mining, since when, the last rate and the accepted / rejected shares.
- **Safe upgrades:** the new version refuses to start while the old one is still mining; after `--stop` it re-installs
  itself and keeps your settings and the model.
- **`--background` no longer says "started" when the miner ends at once**; it shows the reason.
- **Docker / small shared memory:** the miner falls back to a folder on disk and says so.
- **Pool password:** 6–128 characters, any visible character (punctuation and non-English letters too), no spaces.
- **More Linux systems:** any x86_64 Linux with glibc 2.34 or newer (Ubuntu 22.04+, Debian 12+); the installer names
  anything missing.
- **HiveOS support (experimental):** a HiveOS custom-miner package and a flight sheet you import from the clipboard.

## Who should update

Everyone on v0.1.44 or older, on Linux and Windows. It is especially useful if you run in Docker, use punctuation in
your pool password, or share the machine with other users.

## How to update

Your settings (wallet, password, worker name) and the downloaded model are kept.

**Linux, one command** (for an install in `~/thenewminer`). Stop the old version first:

```bash
~/thenewminer/thenewminer --stop
```

Then replace `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` and `<RIG_NAME>` (including the `<` `>`) and run:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.45 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

It downloads the package and `SHA256SUMS`, checks the SHA-256, unpacks and starts the miner; it stops at the first
error and never starts an unverified file. Keep the single quotes around the password. If your miner is in another
folder: `./thenewminer --stop` there, unpack the new package in the same place, and start it again.

**Windows:** download `TheNewMiner-v0.1.45-windows-wsl2-r1.zip`, unzip it anywhere and double-click
`TheNewMiner.bat`. It finds the installed miner and upgrades it without questions.

## HiveOS (experimental)

1. **Wallets:** add a wallet with the coin `TSC` and your `tc1...` address (once).
2. Copy the flight sheet from https://github.com/ngasuka855/TheNewMiner/blob/v0.1.45/hiveos/flightsheet-thenewminer.json, replace `<YOUR_WALLET>` (keep
   `.%WORKER_NAME%` after it) and `<YOUR_POOL_PASSWORD>`, then Flight Sheets › **Import from clipboard**, select your
   `TSC` wallet and save.
3. Apply it to the rig. The first start downloads the model (about 8.1 GiB). On RTX 30 series cards a one-time GPU
   check may then time out and HiveOS restarts the miner by itself two or three times (about 8 minutes more). Leave it
   alone; it then mines.

- Already have the model? Copy `Qwen3-8B-Q8_0-full-v2.gguf` (exactly this name) to `/home/user/` on the rig first: the miner checks it and
  reuses it.
- Rig cannot reach GitHub? Download `thenewminer-0.1.45.tar.gz` on a PC, copy it to the rig and unpack it in
  `/hive/miners/custom/` (steps on the release page); HiveOS then reports "Already installed".
- Driver (as root): `nvidia-driver-update --list`, then for example `nvidia-driver-update 595`, then `sreboot`.


## Change your wallet, pool password or worker name

See "Change your wallet, pool password or worker name" on https://github.com/ngasuka855/TheNewMiner (Linux: `--stop`,
then start once with the new `-u`, `-p` and `-w`; Windows: `TheNewMiner.bat setup`; HiveOS: edit the flight sheet). The
payout address is permanently bound to its pool account: a different wallet needs its own pool account and password.

## NVIDIA driver 580 or newer

The miner needs driver **580 or newer**. Check with `nvidia-smi` ("Driver Version" in the header).

- **Ubuntu:** `sudo ubuntu-drivers install`, or `sudo apt install nvidia-driver-580` (RTX 50 series:
  `nvidia-driver-580-open`), then reboot and check that `nvidia-smi` shows 580 or newer. Do not also use NVIDIA's
  `.run` installer.
- **Debian:** Debian's own packages are older than 580 (535 in Debian 12, 550 in Debian 13). Use NVIDIA's repository,
  see the [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html),
  then reboot.
- **Windows:** the NVIDIA app (formerly GeForce Experience) or [nvidia.com/drivers](https://www.nvidia.com/en-us/drivers/).
  WSL2 uses the Windows driver: never install a Linux NVIDIA driver inside WSL.
- **HiveOS:** `nvidia-driver-update --list`, then for example `nvidia-driver-update 595`, then `sreboot`.

## Downloads

- Release page and full instructions: https://github.com/ngasuka855/TheNewMiner
- Linux: https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz
- Windows 10 / 11 (WSL2): https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-windows-wsl2-r1.zip
- Checksums: https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/SHA256SUMS
- HiveOS package: https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz
- HiveOS flight sheet: https://github.com/ngasuka855/TheNewMiner/blob/v0.1.45/hiveos/flightsheet-thenewminer.json

Verify before use (`sha256sum -c --ignore-missing SHA256SUMS`); if it does not match, do not use the file.

## Known issues

- **RTX 30 series, first start on a slow disk:** on the first start (or the first start after a driver update), a
  one-time GPU check can time out and the miner stops with "could not prepare the configuration for this GPU". The card
  is fine: start it again (Linux: `./thenewminer --background`; Windows: double-click `TheNewMiner.bat`). It may take
  two or three tries. On Linux, `--background` may already have said "started", so check with `./thenewminer --status`.
  RTX 40 and 50 series are not affected. A fix is coming in v0.1.46.
- **HiveOS is experimental:** please report problems. On RTX 30 series cards the first start may need two or three
  automatic restarts (see above). To upgrade later, change both version numbers in the installation URL.

---

# 简体中文

TheNewMiner v0.1.45 已发布，支持 Linux、Windows，并新增 HiveOS（实验性）。
这个版本让安装、升级和排查问题更简单。GPU 引擎与 v0.1.44 相同，算力不变。

## 新功能

- **矿池拒绝钱包或密码时给出清楚提示：** 挖矿程序会用一行信息说明原因并停止，不再反复重试。检查后用 `-u` 和 `-p` 重新启动（Windows：`TheNewMiner.bat setup`）。
- **隐藏钱包和密码：** 它们不再出现在 `ps` 或其他用户的进程列表中。
- **`./thenewminer --status`：** 显示是否在挖矿、运行了多久、最近的算力以及接受 / 拒绝的份额。
- **安全升级：** 旧版本仍在挖矿时，新版本会拒绝启动；执行 `--stop` 后它会自动重新安装，并保留你的设置和模型。
- **`--background`：** 如果挖矿程序一启动就结束，不再显示“已启动”，而是显示原因。
- **Docker / 共享内存太小：** 挖矿程序会改用硬盘上的文件夹，并给出说明。
- **矿池密码：** 6–128 个字符，任何可见字符都可以（包括标点和非英文字母），不能有空格。
- **支持更多 Linux 系统：** 任何 glibc 2.34 或更新的 x86_64 Linux（Ubuntu 22.04+、Debian 12+）；安装程序会指出缺少的组件。
- **支持 HiveOS（实验性）：** 提供 HiveOS 自定义矿工（custom miner）安装包，以及可从剪贴板导入的 flight sheet。

## 谁应该更新

所有使用 v0.1.44 或更早版本的用户（Linux 和 Windows）。如果你在 Docker 中运行、矿池密码含有标点，或与他人共用机器，更新尤其有用。

## 如何更新

你的设置（钱包、密码、矿工名）和已下载的模型都会保留。

**Linux，一条命令**（适用于安装在 `~/thenewminer` 的情况）。先停止旧版本：

```bash
~/thenewminer/thenewminer --stop
```

然后替换 `<YOUR_WALLET>`、`<YOUR_POOL_PASSWORD>` 和 `<RIG_NAME>`（连同 `<` `>` 一起替换）并运行：

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.45 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

它会下载安装包和 `SHA256SUMS`，校验 SHA-256，解压并启动挖矿程序；任何一步出错都会停止，绝不会启动未经校验的文件。密码两边的单引号请保留。如果挖矿程序装在其他文件夹：先在那里执行 `./thenewminer --stop`，把新安装包解压到同一位置，再重新启动。

**Windows：** 下载 `TheNewMiner-v0.1.45-windows-wsl2-r1.zip`，解压到任意位置，双击 `TheNewMiner.bat`。它会找到已安装的挖矿程序并直接升级，无需回答任何问题。

## HiveOS（实验性）

1. **Wallets：** 添加一个币种为 `TSC`、地址为你的 `tc1...` 的钱包（只需一次）。
2. 从 https://github.com/ngasuka855/TheNewMiner/blob/v0.1.45/hiveos/flightsheet-thenewminer.json 复制 flight sheet，替换 `<YOUR_WALLET>`（保留后面的 `.%WORKER_NAME%`）和 `<YOUR_POOL_PASSWORD>`，然后在 Flight Sheets › **Import from clipboard** 导入，选择你的 `TSC` 钱包并保存。
3. 应用到矿机。首次启动会先下载模型（约 8.1 GiB）。在 RTX 30 系列显卡上，之后一次性的 GPU 检查可能超时，HiveOS 会自动重启挖矿程序两三次（再需约 8 分钟）。不用理会，之后就会开始挖矿。

- 已经有模型？先把 `Qwen3-8B-Q8_0-full-v2.gguf`（文件名必须完全一致）复制到矿机的 `/home/user/`：挖矿程序会校验并直接使用它。
- 矿机无法访问 GitHub？在电脑上下载 `thenewminer-0.1.45.tar.gz`，复制到矿机并解压到 `/hive/miners/custom/`（步骤见发布页）；之后 HiveOS 会显示 "Already installed"。
- 驱动（以 root 运行）：`nvidia-driver-update --list`，然后例如 `nvidia-driver-update 595`，再执行 `sreboot`。


## 更换钱包、矿池密码或矿工名

参见 https://github.com/ngasuka855/TheNewMiner 中的 "Change your wallet, pool password or worker name"（Linux：先 `--stop`，再用新的 `-u`、`-p` 和 `-w` 启动一次；Windows：`TheNewMiner.bat setup`；HiveOS：修改 flight sheet）。收款地址与其矿池账户永久绑定：换用其他钱包需要该钱包自己的矿池账户和密码。

## NVIDIA 驱动 580 或更新

挖矿程序需要 **580 或更新**的驱动。用 `nvidia-smi` 检查（输出顶部的 “Driver Version”）。

- **Ubuntu：** `sudo ubuntu-drivers install`，或 `sudo apt install nvidia-driver-580`（RTX 50 系列：`nvidia-driver-580-open`），然后重启，并用 `nvidia-smi` 确认版本为 580 或更新。不要同时使用 NVIDIA 的 `.run` 安装程序。
- **Debian：** Debian 自带的驱动都早于 580（Debian 12 为 535，Debian 13 为 550）。请使用 NVIDIA 的软件源，参见 [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)，然后重启。
- **Windows：** 使用 NVIDIA App（原 GeForce Experience）或 [nvidia.com/drivers](https://www.nvidia.com/en-us/drivers/)。WSL2 使用 Windows 驱动：切勿在 WSL 内安装 Linux 版 NVIDIA 驱动。
- **HiveOS：** `nvidia-driver-update --list`，然后例如 `nvidia-driver-update 595`，再执行 `sreboot`。

## 下载

- 发布页和完整说明：https://github.com/ngasuka855/TheNewMiner
- Linux：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz
- Windows 10 / 11（WSL2）：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-windows-wsl2-r1.zip
- 校验文件：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/SHA256SUMS
- HiveOS 安装包：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz
- HiveOS flight sheet：https://github.com/ngasuka855/TheNewMiner/blob/v0.1.45/hiveos/flightsheet-thenewminer.json

使用前请校验（`sha256sum -c --ignore-missing SHA256SUMS`）；如果不一致，请不要使用该文件。

## 已知问题

- **RTX 30 系列，在较慢的硬盘上首次启动：** 首次启动（或更新驱动后的首次启动）时，一次性的 GPU 检查可能超时，挖矿程序会停止并显示 “could not prepare the configuration for this GPU”。显卡没有问题，重新启动即可（Linux：`./thenewminer --background`；Windows：双击 `TheNewMiner.bat`），可能需要重试两三次。在 Linux 上 `--background` 可能已经显示 “started”，请用 `./thenewminer --status` 确认。RTX 40 和 50 系列不受影响。v0.1.46 将修复。
- **HiveOS 为实验性支持：** 遇到问题请反馈。RTX 30 系列显卡首次启动可能需要自动重启两三次（见上文）。以后升级时，请同时修改安装 URL 中的两处版本号。
