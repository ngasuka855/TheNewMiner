# TheNewMiner v0.1.47

**English** · [简体中文](#简体中文)

TheNewMiner v0.1.47 is out for Linux, Windows and HiveOS (experimental). This release makes installing simple: your
pool account is checked before the big download, Windows asks for it once, and an earlier install is upgraded where it
is. Mining speed is unchanged. Everyone on v0.1.46 or older should update.

## What's new

- **Easier install:** on a single-GPU machine the miner checks your wallet and pool password with the pool **before**
  the 8.1 GiB model download, so a typo is found in seconds instead of after the download.
- **The wallet check:** a wallet address with a typo (its checksum does not match), a cut-off paste or the example
  address is refused, with what is wrong and where it came from.
- **Windows: credentials asked once.** One window asks for the wallet, the pool password and the worker name at the
  start, before anything long; after that nothing else is asked. `TheNewMiner.bat setup` opens it again.
- **Windows: upgrade in place.** An install made by an earlier version, also in another WSL distribution (for example
  `Ubuntu-24.04`), is found and upgraded where it is: no second install and no second model download.
- **Windows: Ubuntu from a mirror.** If Ubuntu cannot be downloaded through `wsl --install` (for example where GitHub is
  unreachable), setup downloads Ubuntu's own WSL image from a public mirror in China (Tsinghua TUNA, USTC, Aliyun) and
  uses it only if its SHA-256 matches.
- **No apt on a stock Ubuntu:** Ubuntu 22.04 / 24.04 (WSL's included) already has everything the miner needs, so nothing
  is installed. Where something is missing, the first start lists it before installing.
- **Refused on Windows drives:** a Linux package started from a Windows drive (such as `/mnt/d` in WSL) is refused at
  once, with a ready-to-paste line that copies it to your Linux home folder and starts it there.
- **DNS and network messages:** when DNS inside WSL does not work, setup stops at that step with the fix (`dnsTunneling`);
  a miner that cannot reach the internet or the pool says what to check (DNS, firewall, proxy).
- **Every Windows failure names its step** and what to do, and `TheNewMiner.bat` shows it again.
- **The model is reused:** an upgrade into a new folder finds the model of an earlier install (same size and SHA-256)
  and does not download it again.
- **`--status` reasons:** after a stop, `./thenewminer --status` says why and how to fix it, also after a
  `--background` start.
- **HiveOS backoff:** every failure now waits and retries with growing pauses (one to ten minutes) and shows its reason
  in the log and on the dashboard.
- **The README is shorter:** downloads first; the full guide, Windows troubleshooting, test results and a development
  record are under `docs/`.

## Measured on our test rigs

- RTX 4090: 69.28 N/s (10 min), 10 accepted / 0 rejected; v0.1.46 69.45 N/s in the same session.
- RTX 5090: 93.57 N/s (10 min), 7 accepted / 0 rejected; v0.1.46 93.10 N/s in the same session.
- RTX 3080 10 GB on Windows: 21–22 N/s (operator's own run).

Live rates depend on the card, its cooling and power limit; these are measurements, not a promise.

## Known issues

- **10 GB cards on Windows need about 9.3 GB of free VRAM for full speed.** With less, the miner uses a slower layout
  (an RTX 3080 10 GB dropped to about 9 N/s). Close browsers (or turn off their hardware acceleration) and Docker
  Desktop, then start mining again.
- **Windows clock:** if the miner reports a clock error, turn on "Set time zone automatically" and "Set the time
  automatically" in Windows Settings › Time & language › Date & time, click **Sync now**, then run `wsl --shutdown` and start again.
- HiveOS support is experimental: the changes since v0.1.46 have not run on a real HiveOS rig yet. Please report
  problems.

## New here? Create your pool account first

1. Open https://tsc.the-new.win/#/home and go to **Start mining** (开始挖矿; the site has an English version).
2. Under **1. Bind Wallet**, enter your TSC payout address (`tc1...`, check it carefully), choose a pool password
   (6–128 characters) and confirm it, then click **Apply to join pool**. The account works at once.
3. Start the miner with the same wallet and password. About 3–5 minutes after it starts mining, your rig appears under **My
   Miners**.
   The pool cannot recover a forgotten password.

## How to update

Your settings and the downloaded model are kept.

**Linux** (for an install in `~/thenewminer`): run `~/thenewminer/thenewminer --stop`, then the one-click install
command on the release page with `V=v0.1.47`.

**Windows:** download `TheNewMiner-v0.1.47-windows-wsl2-r3.zip`, unzip it anywhere and double-click `TheNewMiner.bat`.
It finds the installed miner and upgrades it without questions.

**HiveOS:** change both version numbers in the flight sheet's installation URL to
`https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` and apply it. New users: import
`https://github.com/ngasuka855/TheNewMiner/blob/v0.1.47/hiveos/flightsheet-thenewminer.json` (replace `<YOUR_WALLET>`
and `<YOUR_POOL_PASSWORD>`).

## Downloads

- Release page and full instructions: https://github.com/ngasuka855/TheNewMiner
- Linux: https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
- Windows 10 / 11 (WSL2): https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip
- HiveOS package: https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz
- Checksums: https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS
- Report a problem: https://github.com/ngasuka855/TheNewMiner/issues/new/choose

Verify before use (`sha256sum -c --ignore-missing SHA256SUMS`); if it does not match, do not use the file. NVIDIA
driver 580 or newer is required.

---

# 简体中文

[English](#thenewminer-v0147) · **简体中文**

TheNewMiner v0.1.47 已发布，支持 Linux、Windows 和 HiveOS（实验性）。这个版本让安装变得简单：在下载大文件之前先核对矿池账户，Windows 只问一次账户信息，旧的安装会在原位置升级。挖矿速度不变。所有使用 v0.1.46 或更早版本的用户都应更新。

## 新功能

- **安装更简单：** 单卡机器上，挖矿程序会在下载 8.1 GiB 模型**之前**先向矿池核对钱包地址和矿池密码，输错了几秒内就能发现，不用等下载完。
- **钱包地址校验：** 有错字（校验和不对）、粘贴不完整或使用示例地址时会被拒绝，并说明哪里不对、来自哪里。
- **Windows：账户只问一次。** 开始时用一个窗口询问钱包地址、矿池密码和矿工名，在任何耗时步骤之前；之后不再询问。`TheNewMiner.bat setup` 可重新打开该窗口。
- **Windows：原位升级。** 旧版本做的安装，包括在其他 WSL 发行版（例如 `Ubuntu-24.04`）中的安装，会被找到并在原位置升级：不会再装一次，也不会再下载一次模型。
- **Windows：从镜像获取 Ubuntu。** 如果无法通过 `wsl --install` 下载 Ubuntu（例如无法访问 GitHub 的网络），安装程序会从国内公共镜像（清华 TUNA、中科大 USTC、阿里云）下载 Ubuntu 官方的 WSL 镜像，并且只在其 SHA-256 一致时才使用。
- **标准 Ubuntu 无需 apt：** Ubuntu 22.04 / 24.04（包括 WSL 的）已有挖矿程序需要的全部组件，不会安装任何东西。缺少组件时，首次启动会先列出来再安装。
- **拒绝在 Windows 磁盘上运行：** 从 Windows 磁盘（例如 WSL 里的 `/mnt/d`）启动 Linux 安装包会被立即拒绝，并给出一行可直接粘贴的命令，把它复制到 Linux 主目录并在那里启动。
- **DNS 和网络提示：** WSL 内 DNS 不工作时，安装会停在那一步并给出解决方法（`dnsTunneling`）；挖矿程序连不上网络或矿池时，会说明要检查什么（DNS、防火墙、代理）。
- **Windows 每次失败都会指出是哪一步**以及怎么做，`TheNewMiner.bat` 会再显示一次。
- **模型可重用：** 升级到新文件夹时，会找到旧安装的模型（大小和 SHA-256 都相同），不会再下载。
- **`--status` 说明原因：** 停止后，`./thenewminer --status` 会说明原因和解决方法，用 `--background` 启动后也一样。
- **HiveOS 退避重试：** 每次失败都会以逐渐加长的间隔（一到十分钟）重试，并在日志和面板上显示原因。
- **README 更简短：** 下载放在最前面；完整指南、Windows 故障排除、测试结果和开发记录放在 `docs/` 下。

## 测试矿机实测

- RTX 4090：69.28 N/s（10 分钟），接受 10 / 拒绝 0；同一时段 v0.1.46 为 69.45 N/s。
- RTX 5090：93.57 N/s（10 分钟），接受 7 / 拒绝 0；同一时段 v0.1.46 为 93.10 N/s。
- RTX 3080 10 GB（Windows）：21–22 N/s（运营者自己的运行）。

实际算力取决于显卡、散热和功耗限制；以上为实测值，并非承诺。

## 已知问题

- **10 GB 显卡在 Windows 上需要约 9.3 GB 空闲显存才能全速运行。** 不足时会改用较慢的布局（RTX 3080 10 GB 降到约 9 N/s）。请关闭浏览器（或关闭其硬件加速）和 Docker Desktop，然后重新开始挖矿。
- **Windows 时钟：** 如果挖矿程序报告时钟错误，在 Windows 设置 › 时间和语言 › 日期和时间 中打开“自动设置时区”和“自动设置时间”，点击**立即同步**，然后运行 `wsl --shutdown` 再重新启动。
- HiveOS 支持仍为实验性：v0.1.46 以来的改动尚未在真实 HiveOS 矿机上运行过。如有问题请反馈。

## 新用户？请先创建矿池账户

1. 打开 https://tsc.the-new.win/#/home，进入 **开始挖矿**（Start mining）。
2. 在 **1. 绑定钱包** 中填写你的 TSC 收款地址（`tc1...`，请仔细核对），设置矿池密码（6–128 个字符）并确认，然后点击 **申请加入矿池**。账户立即可用。
3. 用同一个钱包和密码启动挖矿程序。开始挖矿约 3–5 分钟后，你的矿机会出现在 **我的矿工** 页面。矿池不提供密码找回服务。

## 如何更新

你的设置和已下载的模型都会保留。

**Linux**（适用于安装在 `~/thenewminer` 的情况）：先运行 `~/thenewminer/thenewminer --stop`，然后运行发布页上的一键安装命令（`V=v0.1.47`）。

**Windows：** 下载 `TheNewMiner-v0.1.47-windows-wsl2-r3.zip`，解压到任意位置，双击 `TheNewMiner.bat`。它会找到已安装的挖矿程序并直接升级，无需回答任何问题。

**HiveOS：** 把 flight sheet 安装 URL 中的两处版本号改为 `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` 并应用。新用户：导入 `https://github.com/ngasuka855/TheNewMiner/blob/v0.1.47/hiveos/flightsheet-thenewminer.json`（替换 `<YOUR_WALLET>` 和 `<YOUR_POOL_PASSWORD>`）。

## 下载

- 发布页和完整说明：https://github.com/ngasuka855/TheNewMiner
- Linux：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
- Windows 10 / 11（WSL2）：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip
- HiveOS 安装包：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz
- 校验文件：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS
- 报告问题：https://github.com/ngasuka855/TheNewMiner/issues/new/choose

使用前请校验（`sha256sum -c --ignore-missing SHA256SUMS`）；如果不一致，请不要使用该文件。需要 NVIDIA 驱动 580 或更新。
