# TheNewMiner

**English** · [简体中文](#简体中文)

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

---

# 简体中文

[English](#thenewminer) · **简体中文**

TheNewMiner 是 **TensorCash** 的 GPU 矿工,支持 **显存 10 GB 或以上** 的 NVIDIA GeForce RTX 30 / 40 / 50 系列显卡,
连接矿池 **`tsc-wss.the-new.win`**。矿池地址已内置在程序中,无需填写,直接连接。

当前版本:**v0.1.44**

## 下载

| 文件 | 适用 | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64(Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11(通过 WSL2 运行) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | 两个安装包的校验值 | |

Windows 的 zip 是一体化安装包,已经包含 Linux 矿工程序,只需下载这一个 zip。

## 校验下载文件

把安装包和 `SHA256SUMS` 放在同一个文件夹,然后运行:

```bash
# Linux(如果只下载了其中一个安装包,加上 --ignore-missing)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows(命令提示符):把输出与上表中的 SHA-256 对比
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

每个文件都必须显示 `OK`(Windows 上则是与上表完全相同的 64 个字符)。不一致就不要使用。

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <你的钱包地址> -p <你的矿池密码> -w rig-01 --background
```

| 命令 | 作用 |
|---|---|
| `./thenewminer -u <你的钱包地址> -p <你的矿池密码> -w rig-01 --background` | 第一次启动:自动安装所需组件,然后在后台挖矿 |
| `./thenewminer --background` | 之后启动:设置已记住,无需再输入 |
| `./thenewminer` | 在前台挖矿 |
| `./thenewminer --logs` | 查看矿工运行情况(Ctrl+C 退出查看,挖矿不会中断) |
| `./thenewminer --stop` | 停止挖矿 |

- `-u` 是你的 TensorCash 钱包地址,`-p` 是矿池密码,`-w` 是在矿池上显示的矿工名(默认:本机主机名)。
- 设置保存在 `thenewminer/.miner/` 中(保存密码的文件只有你本人可读)。
- 第一次启动会下载模型(约 8.1 GiB,只下载一次)、安装适合你显卡的 GPU 引擎,并在你的显卡上自检,然后开始挖矿。
  看到 `[ RATE] ... N/s` 这样的行,就表示正在挖矿。
- 第一次启动可能会用 `apt` 安装几个系统组件(需要 sudo 或 root)。
- 本版本每台机器只支持一块 NVIDIA 显卡(暂不支持多卡矿机)。
- 如果 `tar` 提示 `xz: Cannot exec`,请先安装 xz:`sudo apt-get install -y xz-utils`。
- 安装包内的 `README.md` 是旧的内部说明,请以本页为准(无需 `--proxy`)。
- **升级:** 先 `./thenewminer --stop`,在同一位置解压新版本(会解压到 `thenewminer/`,并保留
  `thenewminer/.miner/` 中的设置和模型),然后重新启动。

## Windows(一键使用)

1. 把 `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` 整个解压(右键 › 全部解压缩)到任意位置:任意文件夹、任意磁盘,
   路径带空格或中文都没问题。不要在 zip 预览窗口里直接运行。
2. 双击 **`TheNewMiner.bat`**。
   - **第一次:** 检查 Windows、驱动和硬盘;如有需要会开启 WSL2(弹出一次管理员确认;可能需要重启一次,之后会自动继续);
     显示数据存放位置(按 Enter 确认);创建自己的 Linux(WSL)环境,安装内置的矿工程序;**只问一次** 你的钱包地址
     (以 `tc1` 开头)、矿池密码(输入两次;6–128 个字母、数字、`_` 或 `-`)和矿工名(直接按 Enter = 电脑名);
     下载模型,然后开始挖矿。
   - **之后每次:** 双击即用已保存的设置直接开始挖矿。安装后桌面上还会有一个“TheNewMiner”快捷方式。

日常命令(在解压后的文件夹中打开命令提示符):

| 命令 | 作用 |
|---|---|
| `TheNewMiner.bat` | 需要时先安装,然后在本窗口挖矿(关闭窗口即停止) |
| `TheNewMiner.bat background` | 后台挖矿(无窗口) |
| `TheNewMiner.bat logs` | 查看矿工运行情况(Ctrl+C 退出查看,挖矿不会中断) |
| `TheNewMiner.bat status` | 是否在运行,以及最近的算力 |
| `TheNewMiner.bat stop` | 停止挖矿 |
| `TheNewMiner.bat setup` | 重新输入钱包 / 密码 / 矿工名 |
| `TheNewMiner.bat uninstall` | 卸载矿工程序、数据文件夹和快捷方式(会先询问) |

- **数据位置:** 矿工程序和模型放在第一次安装时选定的数据文件夹中(默认是解压所在磁盘根目录的 `TheNewMinerData`,
  例如 `D:\TheNewMinerData`),不在解压后的文件夹里。那里需要约 20 GB 可用空间。
- **升级:** 把新版本解压到任意位置,双击其中的 `TheNewMiner.bat`。它会自动找到已安装的矿工程序,保留设置和模型,
  直接安装新版本,不再询问。旧文件夹可以删除。
- **卸载:** `TheNewMiner.bat uninstall`。
- 界面语言跟随 Windows 显示语言(English / 简体中文 / 繁體中文)。如需指定,在任何命令后加 `-Lang zh-Hans`
  (或 `zh-Hant`、`en`),例如 `TheNewMiner.bat -Lang zh-Hans`,仅对本次运行有效。

## 代理(可选)

矿工默认直接连接矿池。如果你的网络需要代理:

```bash
# Linux(会记住,直到你更改)
./thenewminer --proxy <IP>[:<端口>]             # TCP 中转(不写端口则为 443)
./thenewminer --proxy socks5://<主机>:<端口>    # SOCKS5,也支持 socks5://<用户名>:<密码>@<主机>:<端口>
./thenewminer --proxy http://<主机>:<端口>      # HTTP CONNECT 代理
./thenewminer --proxy none                      # 恢复直接连接
```

这些命令同时会启动矿工:请先停止矿工;加 `--background` 可在后台运行。

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<端口>]
TheNewMiner.bat setup -Proxy socks5://<主机>:<端口>
TheNewMiner.bat setup -Proxy http://<主机>:<端口>
TheNewMiner.bat setup -Proxy none
```

TLS 始终与矿池端到端加密,代理无法读取连接内容。Windows 上,同一台电脑上运行的代理软件在 WSL 中通常不能用
`127.0.0.1` 访问(WSL 默认网络模式):请使用电脑的局域网 IP,并在代理软件中允许局域网连接。

## 系统要求

| 项目 | 要求 |
|---|---|
| 显卡 | 一块 NVIDIA GeForce RTX 30 / 40 / 50 系列显卡,显存 10 GB 或以上 |
| 驱动 | NVIDIA **580 或更新**(`nvidia-smi` 能正常运行)。Windows 上请装在 Windows 中,不要在 WSL 里装驱动 |
| Linux | x86_64 Ubuntu 22.04、24.04 或 26.04(暂不支持其他发行版) |
| Windows | Windows 10 21H2 或更新,或 Windows 11,64 位;BIOS 中已开启虚拟化(Intel VT-x / AMD SVM) |
| 硬盘 | 约 20 GB 可用空间 |
| 内存 | 4 GB 或以上 |
| 模型 | 约 8.1 GiB,第一次启动时下载一次(多镜像并行下载),之后重复使用 |

## 参考算力

以下是我们在自己的测试机上、于真实矿池实测到的最佳算力(2026-10-14 至 2026-10-23),表示 **最高可达**,不作保证:
同型号显卡因芯片个体、散热、频率和功耗上限不同,算力会相差约 5–8%;显卡同时接显示器或有其他程序运行时也会降低。

| 显卡 | 算力 |
|---|---|
| RTX 5090 | 最高约 99 N/s |
| RTX 4090 | 最高约 70 N/s |
| RTX 3080 10 GB | 最高约 22 N/s |

## 常见问题

**Windows 提示“Windows 已保护你的电脑”/“未知发布者”。**
这个安装包没有代码签名。点击 **更多信息**,再点击 **仍要运行**。

**杀毒软件报毒或删除了矿工。**
Microsoft Defender 等杀毒软件常常会把加密货币矿工标记为“可能不需要的应用”,即使是你自己选择运行的正当程序也一样,
因为恶意软件经常偷偷植入矿工。如果文件被隔离:打开“Windows 安全中心 › 病毒和威胁防护 › 保护历史记录”,选择 TheNewMiner
这一项并点击 **允许**(或把解压后的文件夹加入“排除项”)。只对从本页面下载、且 SHA-256 校验一致的安装包这样做。

**第一次启动很慢。**
第一次启动要下载约 8.1 GiB 的模型,并在你的显卡上做 GPU 自检;Windows 上可能还要开启 WSL2。这些只做一次,之后启动会快得多。

**日志在哪里?**
Linux:`./thenewminer --logs`,或文件 `thenewminer/.miner/thenewminer.log`。
Windows:`TheNewMiner.bat logs`(`TheNewMiner.bat status` 可看最近的算力)。

**HiveOS?**
HiveOS 安装包 **即将推出**,目前仍在测试中。

## 安全说明

安装包中的矿工程序和 GPU 引擎以加密形式(ChaCha20-Poly1305)发布,在你的机器上启动时解密。这能提高复制的成本,
但 **并非无法破解**:任何拥有运行机器管理员权限的人,只要投入足够精力,都能取得密钥或解密后的代码。
你的钱包地址和矿池密码只保存在你自己的机器上,并且只发送给矿池。
