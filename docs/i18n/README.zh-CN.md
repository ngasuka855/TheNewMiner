# TheNewMiner

TheNewMiner 是 **TensorCash** 的 GPU 矿工，支持显存 10 GB 或以上的 NVIDIA GeForce RTX 30、40 和 50 系列显卡；矿池 **`tsc-wss.the-new.win`** 已内置。

[English](../../README.md) · **简体中文** · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> 本译文仅为方便阅读而提供，内容以英文 README 为准。 *(This translation is provided for convenience; the English README is authoritative.)*

## 下载

当前版本：**v0.1.47** · 矿池网站：[tsc.the-new.win](https://tsc.the-new.win/#/home)

| 系统 | 下载 | 然后 |
|---|---|---|
| Linux x86_64 | [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | 或使用下方的一键安装 |
| Windows 10 / 11 | [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | 解压后双击 `TheNewMiner.bat` |
| HiveOS（实验性） | [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) | 导入 flight sheet（见下方 HiveOS 一节） |

校验值：[`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS)。校验方法见[指南](GUIDE.zh-CN.md)。如果文件不一致，请不要使用。

## 开始之前：创建矿池账户

挖矿需要一个矿池账户：你的 TSC 收款地址（钱包），以及你在矿池网站上设置的矿池密码。网站提供英文版（可通过网站的语言菜单切换）。

1. 打开 [tsc.the-new.win](https://tsc.the-new.win/#/home)，进入 **Start mining**（开始挖矿）。
2. 在 **1. Bind Wallet**（绑定钱包）下，填写你的 **TSC payout address**（TSC 收款地址，即 `tc1...`；请仔细核对：挖矿收入一旦发到错误的地址，将无法追回），设置一个 **Pool Password**（矿池密码，6–128 个字符），并在 **Confirm Password**（确认密码）下再次输入。
3. 点击 **Apply to join pool**（申请加入矿池）。页面会确认钱包已绑定；账户立即可用。
4. 在矿工中使用同一个钱包和密码：Linux 上用 `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`，Windows 上在 `TheNewMiner.bat` 的账户窗口中填写，HiveOS 上填在 flight sheet 中。开始挖矿后约 3–5 分钟，你的矿机会出现在矿池网站的 **My Miners**（我的矿工）中。

矿池无法找回忘记的矿池密码，收款地址以后也无法更改。矿池密码不是你钱包的私钥：切勿把私钥交给任何人。

## 一键安装（Linux）

把这条命令末尾的 `<YOUR_WALLET>`、`<YOUR_POOL_PASSWORD>` 和 `<RIG_NAME>`（连同 `<` `>` 一起）分别替换成你的钱包、你的矿池密码和这台矿机的名称，再粘贴到终端中运行。它会下载 Linux 安装包和 `SHA256SUMS`，校验 SHA-256，把矿工程序解压到 `~/thenewminer`，并在后台启动：

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.47 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

下载出错或 SHA-256 不一致时，不会启动任何程序。请保留密码两边的单引号（密码中的 `'` 要写成 `'\''`）。在单卡机器上，矿工会在下载模型（约 8.1 GiB，只下载一次）之前，先向矿池核对钱包和密码。之后 `./thenewminer --logs` 会显示挖矿情况，`./thenewminer --status` 会说明它是否在运行，或者为什么停止。要升级，请先运行 `~/thenewminer/thenewminer --stop`，再运行同一条命令：你的设置和模型会保留。其他所有内容（日常命令、升级、开机自动启动、不挖矿时怎么办）都在[指南](GUIDE.zh-CN.md)中。

## Windows

1. 把 `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` 整个解压（右键 › 全部解压缩）到任意位置。
2. 双击 **`TheNewMiner.bat`**。第一次运行时，会有 **一个** 窗口询问你的矿池账户（钱包、矿池密码、矿工名）；之后不会再询问任何内容：它会设置 WSL2 和 Ubuntu，安装内置的矿工程序，下载模型，然后开始挖矿。以后再双击，会立即开始挖矿。Windows 可能会先提示“Windows 已保护你的电脑”（本安装包没有代码签名）：点击 **更多信息**，再点击 **仍要运行**。开启 WSL 时会请求一次管理员权限，并可能需要重启一次；之后安装会自动继续。
3. 如果找到较早的版本，会在原位置升级，并保留你的设置和模型。

如果它停止了或挖矿很慢，请参阅 [Windows 故障排除](WINDOWS-TROUBLESHOOTING.zh-CN.md)（DNS、Windows 磁盘、浏览器和 Docker Desktop 占用的显存、时钟）。

## HiveOS（实验性）

1. **Wallets**（钱包）：添加一个钱包，币种（coin）选 `TSC`，并填入你的 TensorCash 地址（`tc1...`）。
2. 打开 [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json)，复制其内容，替换 `<YOUR_WALLET>`（保留其后的 `.%WORKER_NAME%`）和 `<YOUR_POOL_PASSWORD>`。在 HiveOS 中选择 Flight Sheets › **Import from clipboard**（从剪贴板导入），粘贴内容，选择你的 `TSC` 钱包，然后保存。
3. 将 flight sheet 应用到你的矿机。它会安装 `thenewminer-0.1.47.tar.gz` 并开始挖矿。

手动设置、升级以及无法访问 GitHub 的矿机：见[指南](GUIDE.zh-CN.md)。

## v0.1.47 更新内容

- **安装更简单**：在单卡机器上，矿工会在下载 8.1 GiB 模型 **之前** 先向矿池核对你的钱包和矿池密码，输错了几秒内就能发现。有输入错误的钱包地址（校验和不匹配）或示例地址会被拒绝，并说明错在哪里。
- **Windows：凭据只询问一次**，在开始时的一个窗口中询问，在任何耗时较长的步骤之前。由较早版本完成的安装（包括在其他 WSL 发行版中的安装）会被找到并 **在原位置升级**（保留设置和模型）。如果无法从 Microsoft 下载 Ubuntu，安装程序会从中国的镜像站获取 Ubuntu 官方的 WSL 镜像，并且只有在其 SHA-256 一致时才会使用。
- **标准 Ubuntu 上无需 apt**：Ubuntu 22.04 / 24.04（包括 WSL 中的）已经具备矿工需要的一切，因此不会安装任何东西。若有缺失，第一次启动会先列出缺少的部分，再进行安装。
- **位于 Windows 磁盘上的安装包**（例如 WSL 中的 `/mnt/d`）会被立即拒绝，并给出一行可直接粘贴的命令，把它复制到你的 Linux 主目录并在那里启动。
- **网络提示更清楚**：当 WSL 内的 DNS 无法工作时，安装程序会在该步骤停止并给出解决方法；无法连接互联网或矿池的矿工会说明需要检查什么。
- **模型可重复使用**：升级到新文件夹时，会找到之前安装的模型（大小和 SHA-256 都相同），不会再次下载。
- **`--status` 会说明原因**：矿工为什么停止以及如何解决，用 `--background` 启动后也一样。
- **HiveOS**：每次失败后都会等待并重试，间隔逐渐加长（一到十分钟），并在日志和仪表盘上显示原因。

完整说明：[`docs/releases/RELEASE-NOTES-v0.1.47.md`](../releases/RELEASE-NOTES-v0.1.47.md)。

## 已知问题

- **Windows 上的 10 GB 显卡需要约 9.3 GB 可用显存才能全速运行。** 不足时，矿工会使用较慢的布局（一块 RTX 3080 10 GB 从 21–22 降到约 9 N/s）。开启硬件加速的浏览器或 Docker Desktop 可能会占用这部分显存：请关闭它们（或关闭浏览器的硬件加速），然后重新开始挖矿。
- **Windows 时钟**：如果矿工报告时钟错误，请打开 Windows 设置 › 时间和语言 › 日期和时间，开启“自动设置时区”和“自动设置时间”，点击 **立即同步**，然后运行 `wsl --shutdown` 再重新启动。

## 算力

以下是我们在自己的矿机上、于真实矿池实测到的最佳算力。这些是 **最高可达** 的数值，并非保证：同型号显卡之间相差约 5–8%；如果显卡同时连接显示器，或有其他程序在使用 GPU，算力也会下降。

| 显卡 | 算力 |
|---|---|
| RTX 5090 | 最高约 99 N/s |
| RTX 4090 | 最高约 70 N/s |
| RTX 3080 10 GB | 最高约 22 N/s |

各版本带日期的实测数据：[测试结果](../TEST-RESULTS.md)。项目的里程碑：[开发记录](../DEVELOPMENT-RECORD.md)。

## 报告问题

在 [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) 上新建 issue，并填写错误报告表单：TheNewMiner 版本、你的系统、GPU 型号和驱动、`./thenewminer --status` 的输出以及最后几行日志。Issue 是公开的：**切勿贴出你的矿池密码或钱包私钥**；粘贴任何内容前，请删除其中的密码（如果你愿意，也可以删除钱包地址）。

## 安全说明

安装包中的矿工程序和 GPU 引擎以加密形式（ChaCha20-Poly1305）发布，矿工启动时在你的机器上将其解密。这提高了复制的成本，但 **并非无法破解**：任何拥有运行矿工的机器管理员权限的人，只要投入足够精力，都能取得密钥或解密后的代码。你的钱包地址和矿池密码保存在你自己的机器上，并且只发送给矿池。
