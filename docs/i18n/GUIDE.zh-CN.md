[English](../GUIDE.md) · **简体中文** · [繁體中文](GUIDE.zh-TW.md) · [Русский](GUIDE.ru.md) · [Español](GUIDE.es.md) · [Português (Brasil)](GUIDE.pt-BR.md) · [Français](GUIDE.fr.md) · [Deutsch](GUIDE.de.md) · [Türkçe](GUIDE.tr.md) · [Tiếng Việt](GUIDE.vi.md) · [한국어](GUIDE.ko.md) · [日本語](GUIDE.ja.md)

> 本译文仅为方便阅读而提供，内容以英文 README 为准。 *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner 指南

首次启动之外的一切内容：校验下载文件，在 Linux、Windows 和 HiveOS 上的日常使用，升级，不挖矿时怎么办，以及常见问题。下载和快速入门见[主页面](README.zh-CN.md)；Windows 问题有单独的页面：[Windows 故障排除](WINDOWS-TROUBLESHOOTING.zh-CN.md)。

## 校验下载文件

| 文件 | SHA-256 |
|---|---|
| [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | `86c215dc9401209b8d2a2346252ed5cc1901527b6a51520372e3128583c63656` |
| [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | `45a459646713bef9f8285848486c0a4bd66dee65e8c63090c8d3345c998b2659` |
| [`thenewminer-0.1.47.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz)（HiveOS） | `6b2515c5bf23f60daa236ff4a8b165ce5e55265d7eecac9dd408896e1e31b784` |

Windows 的 zip 是一体化安装包：已经包含 Linux 矿工程序，只需下载这一个 zip。把安装包和 [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS) 放在同一个文件夹，然后运行：

```bash
# Linux（--ignore-missing：只校验你已下载的安装包）
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows（命令提示符）：把输出与上表中的 SHA-256 对比
certutil -hashfile TheNewMiner-v0.1.47-windows-wsl2-r3.zip SHA256
```

每个文件都必须显示 `OK`（Windows 上则是与上表完全相同的 64 个字符）。如果不一致，请不要使用。

## Linux

```bash
tar -xJf TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| 命令 | 作用 |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | 第一次启动：自动安装所需组件，然后在后台挖矿 |
| `./thenewminer --background` | 之后启动：设置已记住，无需再输入 |
| `./thenewminer` | 在前台挖矿（按 Ctrl+C 停止） |
| `./thenewminer --logs` | 查看矿工运行情况（Ctrl+C 退出查看，挖矿不会中断） |
| `./thenewminer --status` | 是否正在挖矿、最近的算力，以及被接受 / 被拒绝的份额；停止后会说明停止的原因以及解决方法 |
| `./thenewminer --stop` | 停止挖矿 |
| `./thenewminer --help` | 查看全部选项 |

- `-u` 是你的 TensorCash 钱包地址，`-p` 是你的矿池密码，`-w` 是在矿池上显示的矿工名（默认：本机的主机名；1–64 个字母、数字、`_`、`-` 或 `.`）。
- 钱包地址会在任何程序启动之前接受检查：有输入错误的地址（校验和不匹配）或示例地址会被拒绝，并说明错在哪里。
- 矿池密码：6–128 个字符，可以是任何可见字符，不能有空格。在命令行中请始终用单引号把它括起来，例如 `-p 'my$pass!'`（单引号内的 `'` 要写成 `'\''`）。
- 设置会记住并保存在 `thenewminer/.miner/` 中（保存密码的文件只有你本人可读）。钱包和密码会立即从命令行中移除，因此不会出现在 `ps` 中（但你的 shell 可能仍会在历史记录中保留这条命令）。
- 单卡机器第一次启动时，矿工会在下载模型 **之前** 先向矿池核对钱包和密码，输错了几秒内就能发现（多卡矿机在开始挖矿时才会发现）。如果矿池拒绝了它们，矿工会给出提示并停止：请检查后用 `-u` 和 `-p` 重新启动。
- 第一次启动会下载模型（约 8.1 GiB，只下载一次）、安装适合你显卡的 GPU 引擎，并在显卡上运行自检，然后开始挖矿。看到 `[ RATE] ... N/s` 这样的行，就表示正在挖矿。日志文件是 `thenewminer/.miner/thenewminer.log`。
- 标准的 Ubuntu 22.04 / 24.04（包括 WSL 中的）已经具备矿工需要的全部系统库，因此不会安装任何东西。缺少时（Docker 镜像、精简系统），在 Ubuntu、Debian 和 HiveOS 上第一次启动会列出缺少的库并用 `apt` 安装（需要 sudo 或 root）；在其他系统上，它会指出这些库，由你自行安装。
- 多卡矿机：支持，每块 GPU 都会挖矿；本版本是在每台机器一块 GPU 的情况下测试的。
- 在 Docker 或其他共享内存较小的容器中，矿工会从磁盘上的 `thenewminer/.miner/run/` 运行其程序文件，并给出提示。如需选择其他文件夹，请用 `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` 启动（该文件夹必须只有你可以写入：`chmod 700 <folder>`）。
- **升级**：先 `./thenewminer --stop`，在同一位置解压新安装包（会解压到 `thenewminer/`，并保留 `thenewminer/.miner/` 中的设置和模型），然后重新启动。如果旧版本仍在运行，新版本会拒绝启动，并提示你先停止旧版本。如果改为解压到新的文件夹，它会找到旧版本的模型（大小和 SHA-256 都相同）并直接使用：不会再下载一次 8.1 GiB。在新文件夹里要重新给出 `-u -p -w`。

### 没有在挖矿时

`./thenewminer --status` 会说明矿工停止的原因以及解决方法。最常见的提示：

| 你看到 | 怎么做 |
|---|---|
| `bash: ./thenewminer: Permission denied` | 这个文件夹不允许运行程序（`noexec` 挂载，例如某些 `/tmp`，或 U 盘）。解压到你的主目录：`cd ~ && tar -xJf <the package>.tar.xz && cd thenewminer` |
| `this package is on a FAT (vfat) drive`（或 exFAT / NTFS，或 `Windows (DrvFs, 9p)`：Windows 磁盘，例如 WSL 中的 `/mnt/d`） | 这种磁盘保存不了矿工的私有设置（权限 600）。粘贴它显示的 `mkdir -p ~/thenewminer && cp -a ...` 那一行：它会把文件夹复制到你的 Linux 主目录并在那里启动 |
| `this system has no xz` | `sudo apt-get install -y xz-utils`（root 用户不用 `sudo`），然后重新启动 |
| `This system is missing what TheNewMiner needs` | 运行它显示的 `sudo apt-get install -y ...` 那一行，然后重新启动 |
| `No NVIDIA GPU found on this machine` | 系统看不到 NVIDIA 显卡。检查显卡是否插好、供电是否接好；云服务器请选择带 NVIDIA GPU 的机型 |
| `NVIDIA driver ... is too old` | 安装它指出的驱动（580 或更新），重启后再启动（见下文“更新 NVIDIA 驱动”） |
| `the pool rejected the wallet / password` | 检查 `-u` 和 `-p`，以及矿池网站上是否已有该账户，然后运行 `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' --background` |
| `no download source answered` | 下载模型时连不上互联网。需要通过代理上网时：`https_proxy=http://<HOST>:<PORT> ./thenewminer --background`；或者复制模型（见下文“已经有模型了？”） |
| `cannot reach the pool` / `pool disconnected` | 检查网络连接、DNS，以及防火墙是否允许对外的 TCP 443；如果你的网络屏蔽了矿池，请使用 `--proxy`（见下文“代理”）。它会自动重连 |

### 开机自动启动（可选）

重启后矿工不会自己启动（HiveOS 会帮你启动）。在 Linux 上，可以添加一行 `crontab`：

```bash
crontab -e      # 然后加入这一行（换成你的文件夹）：
@reboot cd /home/<YOU>/thenewminer && ./thenewminer --background
```

或者使用 systemd 用户服务（先运行一次 `loginctl enable-linger <YOU>`，这样不登录也能启动）：

```ini
# ~/.config/systemd/user/thenewminer.service，然后运行：systemctl --user enable --now thenewminer
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

退出码 77 表示矿池拒绝了钱包 / 密码：重启也没用，所以服务会停在那里。

## Windows

1. 把 `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` 整个解压（右键 › 全部解压缩）到任意位置：任意文件夹、任意磁盘，路径带空格或中文都没问题。不要在 zip 预览窗口里直接运行。
2. 双击 **`TheNewMiner.bat`**。
   - **第一次**：会有 **一个** 窗口先询问你的矿池账户：钱包地址（以 `tc1` 开头；会检查其校验和，窗口中显示的示例不是真实钱包）、矿池密码（6–128 个字符，可以是任何可见字符，不能有空格）和矿工名（已填入电脑名）。三项都正确之前，OK 按钮一直是灰色的。点击 OK 和“是”之后，不会再询问任何内容：它会检查 Windows、驱动和硬盘；如有需要会开启 WSL2（弹出一次管理员确认；可能需要重启一次，之后安装会自动继续）；显示数据存放位置；创建自己的 Linux（WSL）环境，安装内置的矿工程序，下载模型，然后开始挖矿。没有桌面时（通过 SSH），同样的问题会在控制台中询问。
   - **之后每次**：双击即可用已保存的设置立即开始挖矿。桌面上还会创建一个“TheNewMiner”快捷方式。如果已经在挖矿时再次双击，会提示正在挖矿并显示日志。
   - **如果安装中途停止**，窗口会说明停在哪一步以及该怎么做，`TheNewMiner.bat` 也会再次显示这些信息。见 [Windows 故障排除](WINDOWS-TROUBLESHOOTING.zh-CN.md)。

日常命令（在解压后的文件夹中打开命令提示符）：

| 命令 | 作用 |
|---|---|
| `TheNewMiner.bat` | 需要时先安装，然后在本窗口挖矿（关闭窗口即停止挖矿） |
| `TheNewMiner.bat background` | 无窗口挖矿 |
| `TheNewMiner.bat logs` | 查看矿工运行情况（Ctrl+C 退出查看，挖矿不会中断） |
| `TheNewMiner.bat status` | 是否在运行，以及最近的算力 |
| `TheNewMiner.bat stop` | 停止挖矿 |
| `TheNewMiner.bat setup` | 重新输入钱包 / 密码 / 矿工名（点击 Cancel 则保留原有设置） |
| `TheNewMiner.bat uninstall` | 卸载矿工程序、数据文件夹和快捷方式（会先询问） |

- **数据位置**：矿工程序和模型放在第一次安装时选定的数据文件夹中（默认是解压所在磁盘根目录下的 `TheNewMinerData`，例如 `D:\TheNewMinerData`），不在解压后的文件夹里。那里需要约 20 GB 可用空间。如需选择其他文件夹：`TheNewMiner.bat setup -DataDir <folder>`。
- **Ubuntu**：安装程序用 `wsl --install` 安装 Ubuntu 24.04。如果该下载无法连接，它会从中国的公共镜像站（依次为清华 TUNA、中科大 USTC、阿里云）下载 Ubuntu 官方的 WSL 镜像 `ubuntu-24.04.5-wsl-amd64.wsl`，并且只有在其 SHA-256 与 `bb415d824822c4b878125729af451a5d18fb13d1cf5cbed9a7393ad64ac6039e` 完全一致时才会使用。
- 如果矿池拒绝了钱包或密码，窗口会给出提示并停止挖矿：请运行 `TheNewMiner.bat setup` 重新输入。
- **升级**：把新版本解压到任意位置，双击其中的 `TheNewMiner.bat`。它会找到已安装的矿工程序，保留你的设置和模型，直接安装新版本，不再询问。由较早版本或手动在其他 WSL 发行版（例如 `Ubuntu-24.04`）中完成的安装，会被找到并在原处升级：不会重复安装，不会新装 Ubuntu，也不会再次下载模型。如果无法在原处升级，安装程序会停止并说明原因。旧文件夹可以删除。
- **卸载**：`TheNewMiner.bat uninstall`。它绝不会删除你自己的 WSL 发行版。
- Windows 安装包的界面语言为英文。

## HiveOS（实验性）

HiveOS 支持目前为 **实验性**（自 v0.1.45 起提供，该版本已在真实的 HiveOS 矿机上运行过；v0.1.46 之后的改动尚未在 HiveOS 矿机上运行过）。它使用 HiveOS 自定义矿工（custom miner）安装包 `thenewminer-0.1.47.tar.gz`（其 SHA-256 见“校验下载文件”下的表格）。

快速设置（导入 flight sheet）：

1. **Wallets**（钱包）：添加一个钱包，币种（coin）选 `TSC`，并填入你的 TensorCash 地址（`tc1...`）。此步骤只需做一次：HiveOS 需要选中一个 `TSC` 钱包，而矿工使用的是你在 flight sheet 中填写的地址。
2. 打开 [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json)，复制其内容。把 `<YOUR_WALLET>` 替换为你的钱包地址（保留其后的 `.%WORKER_NAME%`），把 `<YOUR_POOL_PASSWORD>` 替换为你的矿池密码（在 JSON 中，密码里的 `"` 要写成 `\"`，`\` 要写成 `\\`）。在 HiveOS 中选择 Flight Sheets › **Import from clipboard**（从剪贴板导入），粘贴内容，选择你的 `TSC` 钱包，然后保存。
3. 将 flight sheet 应用到你的矿机。矿机会下载安装包并开始挖矿。

也可以改为手动设置：新建一个 flight sheet，Coin 选 `TSC`，Wallet 选你的 `TSC` 钱包，Pool 选“Configure in miner”，Miner 选“Custom”，点击 **Setup Miner Config**，然后填写：

| 字段 | 值 |
|---|---|
| Miner name | `thenewminer`（根据 Installation URL 自动填入） |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` |
| Hash algorithm | 留空 |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win`（会被忽略：矿池已内置） |
| Pass | 你的矿池密码 |
| Extra config arguments | 留空，或填 `--proxy <YOUR_PROXY>`（见“代理”一节） |

- 矿池上显示的矿工名就是该矿机在 HiveOS 中的名称：只能使用字母、数字、`_`、`-` 或 `.`（不能有空格），最多 64 个字符。如需使用其他矿工名，请在 Extra config arguments 中添加 `-w <RIG_NAME>`。
- HiveOS 磁盘上需要约 12 GB 可用空间，用于存放模型和矿工程序（可用 `df -h /hive/miners` 查看）。
- 模型（约 8.1 GiB）只下载一次，保存在 `/hive/miners/custom/.thenewminer-models`，升级时会保留。
- 钱包和密码通过环境变量传给矿工，而不是通过命令行。
- **出现故障时**（例如矿池拒绝了钱包或密码），矿工日志和 HiveOS 仪表盘会显示原因，矿机会重试，间隔逐渐加长（一到十分钟）。请修正 flight sheet 并应用：这会立即重启矿工。
- 日志：HiveOS 中的矿工画面（在矿机的 shell 中运行 `miner`）或 `/var/log/miner/custom/custom.log`。
- **升级**：修改 Installation URL 中的两处版本号（例如 `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`），然后应用 flight sheet。只有 URL 中的文件名发生变化时，HiveOS 才会安装。
- **首次启动**会下载模型并安装 GPU 引擎，需要几分钟。
- **已经有模型了？** 在首次启动前，把它复制到矿机上的 `/home/user/`（见下一节）。

**如果矿机无法访问 GitHub**：在电脑上下载 `thenewminer-0.1.47.tar.gz`（核对其 SHA-256），复制到矿机上，并解压到 HiveOS 存放自定义矿工的位置。然后照常应用 flight sheet：HiveOS 会显示“Already installed”，不会再下载。文件名必须保持为 `thenewminer-0.1.47.tar.gz`（浏览器可能会加上 " (1)"），也不要修改 flight sheet 中的 Installation URL：HiveOS 比较的正是这个文件名。

```bash
# 在电脑上
scp thenewminer-0.1.47.tar.gz user@<RIG_IP>:/tmp/
# 在矿机上，以 root 身份（Hive Shell，或 SSH 登录后执行 sudo -i）
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.47.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.47.tar.gz
```

## 已经有模型了？直接复制

矿工需要一个 8.1 GiB 的模型文件：`Qwen3-8B-Q8_0-full-v2.gguf`（`8709518112` 字节，SHA-256 `408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`）。如果这台机器上，或你网络中的另一台电脑上已经有这个文件，请直接复制，无需重新下载。

在下载任何内容之前，矿工会按准确的文件名和大小查找该文件：它自己的模型文件夹、以前的 TheNewMiner 文件夹、你的主目录（以及其下一两层的文件夹，或其中的 `models` 文件夹）、`/home` 下的各个主目录、`/data`、`/mnt/*`、`/media/*`、`/opt`、`/srv`、`/workspace`、`/var/lib`，以及 Hugging Face / ModelScope 缓存。如果这些位置都没有找到，它会花最多 20 秒在同一磁盘上的这些文件夹中搜索任何大小完全相同的 `.gguf` 文件（其他已挂载的磁盘只按文件名查找）。找到的文件 **只有在 SHA-256 一致时** 才会被使用；随后矿工会把它链接（同一磁盘时）或复制到自己的模型文件夹中，这样以后每个版本都能立即找到它。你也可以直接指定它：`./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`。

**Linux**：把它复制到你的主目录，然后照常启动矿工。如果从同一网络中的 Mac 或电脑复制（Windows 10 和 11 的 PowerShell 自带 `scp`），请把 `user` 和 `<MINER_IP>` 换成你矿工机器的登录名和地址：

```bash
# 在矿工机器本机上
cp /path/to/Qwen3-8B-Q8_0-full-v2.gguf ~/
# 或者从另一台电脑
scp Qwen3-8B-Q8_0-full-v2.gguf user@<MINER_IP>:~/
```

**HiveOS**：把它复制到矿机的 `/home/user/`（HiveOS 的 SSH 登录名是 `user`），然后应用 flight sheet。矿工会在那里找到它并校验，再把它保存在安装包旁边（`/hive/miners/custom/.thenewminer-models`），因此新版本不会再次下载。HiveOS 的磁盘很小：请先运行 `disk-expand`（如果 `df -h /hive/miners` 显示可用空间不足 12 GB）。

```bash
scp Qwen3-8B-Q8_0-full-v2.gguf user@<RIG_IP>:/home/user/
```

**Windows**：首次 `setup` 创建好 TheNewMiner 的 WSL 发行版之后，在命令提示符中把文件复制进去（把 `D:\Models` 改成你的副本所在的位置）。如果下载已经开始，请关闭窗口，运行下面的命令，然后再次启动 TheNewMiner：它会找到这份副本并跳过下载。复制需要几分钟，期间不会显示进度。

```bat
wsl -d TheNewMiner -u root -- mkdir -p ~/thenewminer/models
wsl -d TheNewMiner -u root -- cp /mnt/d/Models/Qwen3-8B-Q8_0-full-v2.gguf ~/thenewminer/models/
```

## 更换钱包、矿池密码或矿工名

矿池会把每个收款地址永久绑定到它自己的矿池账户：矿池页面 [tsc.the-new.win](https://tsc.the-new.win) 将 TSC 收款地址标注为“永久绑定 · 不可修改”（永久绑定，不可修改），并说明如需更换，请直接在矿工上使用新地址。因此，换用另一个钱包需要它自己的矿池账户和矿池密码（请先在矿池页面上设置好）。如果你在矿池页面上修改了矿池密码，请在每个矿工上都更新它。矿池无法找回忘记的密码。

**Linux**：先停止矿工，然后用新设置启动一次；新设置会替换已记住的设置：

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

`-u` 和 `-p` 必须一起提供：只提供其中一个不会更改已记住的钱包和密码。矿工名可以单独更改：`./thenewminer -w <RIG_NAME> --background`（在 `--stop` 之后）。

**Windows**：运行 `TheNewMiner.bat setup`。它会打开账户窗口，并已填入你的矿工名；输入钱包和矿池密码，然后照常启动（双击 `TheNewMiner.bat`）。

**HiveOS**：编辑 flight sheet：在“Wallet and worker template”中填入新的钱包地址（`<NEW_WALLET>.%WORKER_NAME%`），在 Pass 中填入新的矿池密码，然后保存并应用。矿工名就是该矿机在 HiveOS 中的名称（或者在 Extra config arguments 中使用 `-w <RIG_NAME>`）。

## 代理（可选）

矿工直接连接矿池。如果你的网络需要代理：

```bash
# Linux（会一直记住，直到你更改）
./thenewminer --proxy <IP>[:<PORT>]            # TCP 中转（不写端口则为 443）
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5，也支持 socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # HTTP CONNECT 代理
./thenewminer --proxy none                     # 恢复直接连接
```

这些命令同时会启动矿工：请先停止矿工；加上 `--background` 可让它在后台运行。

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

TLS 始终与矿池端到端加密，因此代理无法读取连接内容。在 Windows 上，同一台电脑上运行的代理软件在 WSL 中通常无法通过 `127.0.0.1` 访问（WSL 默认网络模式）：请使用电脑的局域网 IP，并在代理软件中允许局域网连接。

## 系统要求

| 项目 | 要求 |
|---|---|
| 显卡 | NVIDIA GeForce RTX 30 / 40 / 50 系列，显存 10 GB 或以上。多卡矿机：支持，每块 GPU 都会挖矿；本版本是在每台机器一块 GPU 的情况下测试的 |
| 驱动 | NVIDIA **580 或更新**（`nvidia-smi` 必须能正常运行）。Windows 上请在 Windows 中安装，切勿在 WSL 里安装 |
| Linux | x86_64，需要 **glibc 2.34 或更新版本**，例如 Ubuntu 22.04 或更新版本、Debian 12 或更新版本。安装程序会检查所需条件，并列出缺少的项目 |
| Windows | Windows 10 21H2 或更新，或 Windows 11，64 位；BIOS 中已开启虚拟化（Intel VT-x / AMD SVM） |
| 硬盘 | 约 20 GB 可用空间 |
| 内存 | 4 GB 或以上 |
| 模型 | 约 8.1 GiB，第一次启动时下载一次（多镜像并行下载），之后重复使用 |

## 更新 NVIDIA 驱动（580 或更新）

矿工需要 NVIDIA 驱动 **580 或更新版本**。先查看你当前的驱动版本：

```bash
nvidia-smi    # 表头会显示 "Driver Version: ..."；必须是 580 或更新版本
```

- **Ubuntu**：`sudo ubuntu-drivers install` 会安装推荐的驱动（安装后请确认版本为 580 或更新）。如果想自己选择版本：`sudo apt install nvidia-driver-580`（或更新版本；RTX 50 系列显卡需要 open 版本，例如 `nvidia-driver-580-open`）。然后重启。Ubuntu 的驱动包和 NVIDIA 的 `.run` 安装程序只能二选一，切勿混用。
- **Debian**：Debian 自带的驱动包版本低于 580（Debian 12 为 535，Debian 13 为 550）。请按照 [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)（NVIDIA 驱动安装指南）的说明，从 NVIDIA 的软件源安装较新的驱动，然后重启。
- **Windows**：使用 [NVIDIA App](https://www.nvidia.com/en-us/software/nvidia-app/)（原 GeForce Experience）更新，或从 [nvidia.com](https://www.nvidia.com/en-us/drivers/) 下载适用于你显卡的驱动（580 或更新版本）。WSL2 使用的就是这个 Windows 驱动：切勿在 WSL 里安装 Linux 版 NVIDIA 驱动。
- **HiveOS**：以 root 身份（Hive Shell，或通过 SSH 登录后运行 `sudo -i`）运行 `nvidia-driver-update --list` 查看可用版本，然后运行例如 `nvidia-driver-update 595`（任何 580 或更新版本均可），最后运行 `sreboot`。

## 常见问题

**Windows 提示“Windows 已保护你的电脑”/“未知发布者”。**
本安装包没有代码签名。点击 **更多信息**，再点击 **仍要运行**。

**杀毒软件报毒或删除了矿工程序。**
Microsoft Defender 等杀毒软件常常把加密货币矿工标记为“可能不需要的应用”，即使是你自己选择运行的正当程序也一样，因为恶意软件经常暗藏挖矿程序。如果文件被隔离：打开“Windows 安全中心 › 病毒和威胁防护 › 保护历史记录”，选择 TheNewMiner 这一项并选择 **允许**（或把解压后的文件夹加入“排除项”）。只对从本页面下载、且 SHA-256 校验一致的安装包这样做。

**第一次启动很慢。**
第一次启动要下载约 8.1 GiB 的模型，并在你的显卡上运行 GPU 自检；Windows 上可能还要设置 WSL2。这只发生一次。之后启动会快得多，很快就开始挖矿。

**矿工提示矿池拒绝了钱包 / 密码。**
请检查钱包地址和矿池密码（矿池网站上必须已有该账户），然后重新启动：Linux 用 `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`，Windows 用 `TheNewMiner.bat setup`。凭据错误时，矿工不会自行重试。

**是否在挖矿？日志在哪里？**
Linux：`./thenewminer --status`、`./thenewminer --logs`，或文件 `thenewminer/.miner/thenewminer.log`。
Windows：`TheNewMiner.bat status` 和 `TheNewMiner.bat logs`。

**我的 RTX 3080（10 GB）在 Windows 上挖矿比在 Linux 上慢得多。**
在 Windows 上，其他程序会共用显存。见 [Windows 故障排除](WINDOWS-TROUBLESHOOTING.zh-CN.md)：关闭开启硬件加速的浏览器和 Docker Desktop。
