[English](README.md) · **简体中文** · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> 本译文仅为方便阅读而提供,内容以英文 README 为准。 *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner 是 **TensorCash** 的 GPU 矿工,支持 **显存 10 GB 或以上** 的 NVIDIA GeForce RTX 30、40 和 50 系列显卡,
在矿池 **`tsc-wss.the-new.win`** 上挖矿。矿池地址已内置在程序中:无需填写矿池地址,直接连接。

当前版本:**v0.1.44**

## 下载

| 文件 | 适用 | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64(Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11(通过 WSL2 运行) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | 两个安装包的校验值 | |

Windows 的 zip 是一体化安装包:已经包含 Linux 矿工程序,只需下载这一个 zip。

## 校验下载文件

把安装包和 `SHA256SUMS` 放在同一个文件夹,然后运行:

```bash
# Linux(如果只下载了其中一个安装包,请加上 --ignore-missing)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows(命令提示符):把输出与上表中的 SHA-256 对比
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

每个文件都必须显示 `OK`(Windows 上则是与上表完全相同的 64 个字符)。如果不一致,请不要使用。

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background
```

| 命令 | 作用 |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background` | 第一次启动:自动安装所需组件,然后在后台挖矿 |
| `./thenewminer --background` | 之后启动:设置已记住,无需再输入 |
| `./thenewminer` | 在前台挖矿 |
| `./thenewminer --logs` | 查看矿工运行情况(Ctrl+C 退出查看,挖矿不会中断) |
| `./thenewminer --stop` | 停止挖矿 |

- `-u` 是你的 TensorCash 钱包地址,`-p` 是你的矿池密码,`-w` 是在矿池上显示的矿工名
  (默认:本机的主机名)。
- 设置会记住并保存在 `thenewminer/.miner/` 中(保存密码的文件只有你本人可读)。
- 第一次启动会下载模型(约 8.1 GiB,只下载一次)、安装适合你显卡的 GPU 引擎,并在显卡上运行自检,然后开始挖矿。看到 `[ RATE] ... N/s` 这样的行,就表示正在挖矿。
- 第一次启动可能会用 `apt` 安装几个 Ubuntu 软件包(需要 sudo 或 root)。
- 本版本每台机器只支持恰好一块 NVIDIA 显卡(暂不支持多卡矿机)。
- 如果 `tar` 提示 `xz: Cannot exec`,请先安装 xz:`sudo apt-get install -y xz-utils`。
- 安装包内的 `README.md` 是较旧的内部说明,请以本页为准(无需 `--proxy`)。
- **升级:** 先 `./thenewminer --stop`,在同一位置解压新安装包(会解压到 `thenewminer/`,并保留
  `thenewminer/.miner/` 中的设置和模型),然后重新启动。

## Windows(一键使用)

1. 把 `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` 整个解压(右键 › 全部解压缩)到任意位置:任意文件夹、任意磁盘,
   路径带空格或中文都没问题。不要在 zip 预览窗口里直接运行。
2. 双击 **`TheNewMiner.bat`**。
   - **第一次:** 检查 Windows、驱动和硬盘;如有需要会开启 WSL2(弹出一次管理员确认;可能需要重启一次,之后安装会自动继续);
     显示数据存放位置(按 Enter 确认);创建自己的 Linux(WSL)环境,安装内置的矿工程序;**只询问一次** 你的钱包地址
     (以 `tc1` 开头)、矿池密码(输入两次;6–128 个字母、数字、`_` 或 `-`)和矿工名(直接按 Enter = 电脑名);
     下载模型,然后开始挖矿。
   - **之后每次:** 双击即可用已保存的设置立即开始挖矿。桌面上还会创建一个“TheNewMiner”快捷方式。

日常命令(在解压后的文件夹中打开命令提示符):

| 命令 | 作用 |
|---|---|
| `TheNewMiner.bat` | 需要时先安装,然后在本窗口挖矿(关闭窗口即停止挖矿) |
| `TheNewMiner.bat background` | 无窗口挖矿 |
| `TheNewMiner.bat logs` | 查看矿工运行情况(Ctrl+C 退出查看,挖矿不会中断) |
| `TheNewMiner.bat status` | 是否在运行,以及最近的算力 |
| `TheNewMiner.bat stop` | 停止挖矿 |
| `TheNewMiner.bat setup` | 重新输入钱包 / 密码 / 矿工名 |
| `TheNewMiner.bat uninstall` | 卸载矿工程序、数据文件夹和快捷方式(会先询问) |

- **数据位置:** 矿工程序和模型放在第一次安装时选定的数据文件夹中(默认是解压所在磁盘根目录下的 `TheNewMinerData`,
  例如 `D:\TheNewMinerData`),不在解压后的文件夹里。那里需要约 20 GB 可用空间。
- **升级:** 把新版本解压到任意位置,双击其中的 `TheNewMiner.bat`。它会找到已安装的矿工程序,保留你的设置和模型,
  直接安装新版本,不再询问。旧文件夹可以删除。
- **卸载:** `TheNewMiner.bat uninstall`。
- 界面语言跟随 Windows(English / 简体中文 / 繁體中文)。如需指定,在任何命令后加 `-Lang en`(或 `zh-Hans`、
  `zh-Hant`),例如 `TheNewMiner.bat -Lang en`;仅对本次运行有效。

## 代理(可选)

矿工直接连接矿池。如果你的网络需要代理:

```bash
# Linux(会一直记住,直到你更改)
./thenewminer --proxy <IP>[:<PORT>]            # TCP 中转(不写端口则为 443)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5,也支持 socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # HTTP CONNECT 代理
./thenewminer --proxy none                     # 恢复直接连接
```

这些命令同时会启动矿工:请先停止矿工;加上 `--background` 可让它在后台运行。

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

TLS 始终与矿池端到端加密,因此代理无法读取连接内容。在 Windows 上,同一台电脑上运行的代理软件在 WSL 中通常无法通过
`127.0.0.1` 访问(WSL 默认网络模式):请使用电脑的局域网 IP,并在代理软件中允许局域网连接。

## 系统要求

| 项目 | 要求 |
|---|---|
| 显卡 | 恰好一块 NVIDIA GeForce RTX 30 / 40 / 50 系列显卡,显存 10 GB 或以上 |
| 驱动 | NVIDIA **580 或更新**(`nvidia-smi` 必须能正常运行)。Windows 上请在 Windows 中安装,切勿在 WSL 里安装 |
| Linux | x86_64 Ubuntu 22.04、24.04 或 26.04(暂不支持其他发行版) |
| Windows | Windows 10 21H2 或更新,或 Windows 11，64 位;BIOS 中已开启虚拟化(Intel VT-x / AMD SVM) |
| 硬盘 | 约 20 GB 可用空间 |
| 内存 | 4 GB 或以上 |
| 模型 | 约 8.1 GiB,第一次启动时下载一次(多镜像并行下载),之后重复使用 |

## 参考算力

以下是我们在自己的矿机上、于真实矿池实测到的最佳算力(2026-10-14 至 2026-10-23 期间)。这些是 **最高可达** 的数值,
并非保证:同型号显卡因芯片个体、散热、频率和功耗上限不同,算力会相差约 5–8%;如果显卡同时连接显示器,或有其他程序在使用 GPU,
算力也会下降。

| 显卡 | 算力 |
|---|---|
| RTX 5090 | 最高约 99 N/s |
| RTX 4090 | 最高约 70 N/s |
| RTX 3080 10 GB | 最高约 22 N/s |

## 测试结果

2026-10-23 在我们的测试机上用 v0.1.44 于真实矿池进行的测试(每台机器一块显卡)。A = 被接受的份额,R = 被拒绝的份额。

| 日期 | 显卡 | 系统 | 安装包 | 算力 | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | sealed-r4 | 65.58 N/s(中位数,16 分钟) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | sealed-r4 | 93.45 N/s(稳定挖矿,44 分钟;含任务切换为 90.39) | 31 / 0(停止时另有 1 个过期份额) |
| 2026-10-23 | RTX 3080 10 GB | Windows(WSL2) | windows-wsl2-r5 | 约 22 N/s(运营者自行测试) | 无 |
| 2026-10-23 | RTX 3080 10 GB | Linux(Ubuntu 26.04) | sealed-r4 | 21.04 N/s(挖矿 23 分钟中的最后 10 分钟) | 3 / 0 |

RTX 3080 Linux:安装 6/6 步全部通过,没有无效窗口,正常停止。日志摘录:
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt)。

## 截图

来自我们的测试机,使用的是较早版本:RTX 4090 上为 v0.1.41,RTX 5090 上为早期构建版本。均为实时读数;你的实际数值取决于显卡。

**RTX 4090,v0.1.41:** 单次读数 69–75 N/s,多数约 70(10 分钟平均约 68),9 个份额被接受、0 个被拒绝,约 445 W。

![TheNewMiner v0.1.41 在 RTX 4090 上挖矿](screenshots/rtx4090.png)

**RTX 5090,早期构建版本:** 100.83 N/s(已完成窗口的实时平均值),约 575 W。

![早期构建版本的 TheNewMiner 在 RTX 5090 上挖矿](screenshots/rtx5090.png)

## 常见问题

**Windows 提示“Windows 已保护你的电脑”/“未知发布者”。**
本安装包没有代码签名。点击 **更多信息**,再点击 **仍要运行**。

**杀毒软件报毒或删除了矿工程序。**
Microsoft Defender 等杀毒软件常常把加密货币矿工标记为“可能不需要的应用”,即使是你自己选择运行的正当程序也一样,
因为恶意软件经常暗藏挖矿程序。如果文件被隔离:打开“Windows 安全中心 › 病毒和威胁防护 › 保护历史记录”,选择 TheNewMiner
这一项并选择 **允许**(或把解压后的文件夹加入“排除项”)。只对从本页面下载、且 SHA-256 校验一致的安装包这样做。

**第一次启动很慢。**
第一次启动要下载约 8.1 GiB 的模型,并在你的显卡上运行 GPU 自检;Windows 上可能还要设置 WSL2。这只发生一次。之后启动会快得多,很快就开始挖矿。

**日志在哪里?**
Linux:`./thenewminer --logs`,或文件 `thenewminer/.miner/thenewminer.log`。
Windows:`TheNewMiner.bat logs`(`TheNewMiner.bat status` 可查看最近的算力)。

**HiveOS?**
HiveOS 安装包 **即将推出**,目前仍在测试中。

## 安全说明

安装包中的矿工程序和 GPU 引擎以加密形式(ChaCha20-Poly1305)发布,矿工启动时在你的机器上将其解密。这提高了复制的成本,
但 **并非无法破解**:任何拥有运行矿工的机器管理员权限的人,只要投入足够精力,都能取得密钥或解密后的代码。你的钱包地址和矿池密码保存在你自己的机器上,并且只发送给矿池。
