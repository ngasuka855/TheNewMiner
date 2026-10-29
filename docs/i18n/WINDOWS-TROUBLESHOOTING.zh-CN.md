[English](../WINDOWS-TROUBLESHOOTING.md) · **简体中文** · [繁體中文](WINDOWS-TROUBLESHOOTING.zh-TW.md) · [Русский](WINDOWS-TROUBLESHOOTING.ru.md) · [Español](WINDOWS-TROUBLESHOOTING.es.md) · [Português (Brasil)](WINDOWS-TROUBLESHOOTING.pt-BR.md) · [Français](WINDOWS-TROUBLESHOOTING.fr.md) · [Deutsch](WINDOWS-TROUBLESHOOTING.de.md) · [Türkçe](WINDOWS-TROUBLESHOOTING.tr.md) · [Tiếng Việt](WINDOWS-TROUBLESHOOTING.vi.md) · [한국어](WINDOWS-TROUBLESHOOTING.ko.md) · [日本語](WINDOWS-TROUBLESHOOTING.ja.md)

> 本译文仅为方便阅读而提供，内容以英文 README 为准。 *(This translation is provided for convenience; the English README is authoritative.)*

# Windows 故障排除

在 Windows 上，TheNewMiner 在 WSL2 中运行 Linux 矿工程序。安装中途停止时，窗口会说明停在哪一步以及该怎么做，`TheNewMiner.bat` 也会再次显示这些信息（同时保存在 `%LOCALAPPDATA%\TheNewMiner\last-setup-failure.txt` 中）。本页说明常见的情况。安装和日常命令见[指南](GUIDE.zh-CN.md)。

## WSL 内的 DNS 无法工作

安装程序会在第 4 步检查 WSL 能否解析矿池和模型下载地址。如果不能，安装程序会在该步骤停止，不会自行更改任何设置。用记事本打开 `%UserProfile%\.wslconfig`（如果文件不存在就新建一个），加入下面两行（在 Windows 11 22H2 或更新版本上，使用 `networkingMode=mirrored`，放在 `[wsl2]` 下，也可以）：

```ini
[wsl2]
dnsTunneling=true
```

然后在命令提示符中运行 `wsl --shutdown`（它会停止所有 WSL 发行版，包括 Docker Desktop 的），再次双击 `TheNewMiner.bat`。

阻止 WSL 的 VPN 或防火墙也会导致同样的提示：请放行 WSL，或用 `TheNewMiner.bat setup -Proxy ...` 设置代理（见[指南](GUIDE.zh-CN.md)中的“代理”一节）。

## 无法下载 Ubuntu，或虚拟化未开启

- **网络**：安装程序用 `wsl --install` 安装 Ubuntu 24.04。在该下载无法连接的地方（例如无法访问 GitHub 的地区），安装程序会改为从中国的公共镜像站下载 Ubuntu 官方的 WSL 镜像，并且只有在其 SHA-256 一致时才会使用。如果所有镜像站也都没有响应，请检查网络连接，稍后再试。
- **虚拟化**：出现有关虚拟化的提示（例如 `0x80370102`）表示虚拟化未开启。请在 BIOS 中开启 Intel VT-x / AMD SVM，然后再次双击 `TheNewMiner.bat`。

## 安装包位于 Windows 磁盘上

如果你自己解压了 Linux 安装包，并在 WSL 中启动 `./thenewminer`，而它位于 Windows 磁盘上（例如 `/mnt/d/...`），矿工会拒绝启动：Windows 磁盘无法保存它的私有设置文件。它会显示一行可直接粘贴的 `mkdir -p ~/thenewminer && cp -a ...` 命令，把文件夹复制到你的 Linux 主目录并在那里启动。也可以改用 Windows zip：`TheNewMiner.bat` 会自动把一切放到正确的位置。

## 10 GB 显卡速度慢（浏览器、Docker Desktop）

只有在开始挖矿时有约 9.3 GB（9472 MiB）可用显存，10 GB 显卡才能使用快速布局。在 Windows 上，其他程序会共用显卡：开启硬件加速的浏览器或 Docker Desktop 可能占用足够多的显存，使矿工退回到较慢的布局。在 RTX 3080 10 GB 上，这时约为 9 N/s，而不是 21–22 N/s。

1. 关闭浏览器，或关闭其硬件加速（Chrome 和 Edge：设置 › 系统 › “使用图形加速功能（如果可用）”；Firefox：设置 › 常规 › 性能），并退出 Docker Desktop。
2. 在命令提示符中检查可用显存：`nvidia-smi --query-gpu=memory.used,memory.total --format=csv`（总量减去已用量应至少为 9472 MiB）。
3. 重新开始挖矿：`TheNewMiner.bat stop`，然后双击 `TheNewMiner.bat`。

12 GB 显卡同样需要 9472 MiB 可用显存；显存 16 GB 或以上的显卡不受影响。

## 时钟错误

如果矿工的时钟偏差约 30 秒，矿池会停止该矿工。WSL2 的时钟可能会漂移，例如在电脑睡眠之后；时钟有偏差时，安装程序会发出警告。如果矿工报告时钟错误：

1. 打开 Windows 设置 › 时间和语言 › 日期和时间，开启“自动设置时区”和“自动设置时间”，然后点击 **立即同步**。
2. 在命令提示符中运行 `wsl --shutdown`。
3. 重新开始挖矿。

## Windows 安全提示

SmartScreen（“Windows 已保护你的电脑”）以及将矿工标记为威胁的杀毒软件：见[指南](GUIDE.zh-CN.md)中的常见问题。

## 仍然无法解决？

在 [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) 上新建 issue，附上 `TheNewMiner.bat status` 的输出、失败的步骤以及 `last-setup-failure.txt` 的内容。切勿贴出你的矿池密码或钱包私钥。
