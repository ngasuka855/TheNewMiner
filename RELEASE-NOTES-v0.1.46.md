# TheNewMiner v0.1.46

**English** · [简体中文](#简体中文)

TheNewMiner v0.1.46 is out for Linux, Windows and HiveOS (experimental). It makes the first start on RTX 30
series cards and HiveOS more robust and makes the waves a little smoother. Everyone on v0.1.45 or older should update.

## What's new

- **Fewer first-start failures on RTX 30 series cards and HiveOS:** the one-time GPU check now uses GPU code
  precompiled for every supported card family and already runs once during the installation; the GPU compile cache is
  kept between starts and upgrades. If the check still times out on an RTX 30 series card, the miner does that step on
  the CPU instead and keeps mining. If the card's configuration cannot be prepared, the miner says why in one `Reason:`
  line (exit code 78).
- **HiveOS:** when the miner cannot prepare its configuration for the GPU on a first start (exit code 78), the rig now
  waits and retries with growing pauses (one to ten minutes) instead of restarting every few seconds.
- **Smoother waves (root-prefetch tick):** when the pool's checkpoint changes in the middle of a wave, the next wave's
  work is prepared again in the background. On an RTX 5090 this removed the waits at that moment (+0.3% sustained
  rate, identical results).
- **Nothing left in memory after a rejected password:** `--status` and `--logs` after a stop no longer leave the
  unpacked program files (about 1.5 GB) in memory.
- **Already have the model? Copy it:** the README explains how to copy `Qwen3-8B-Q8_0-full-v2.gguf` from another
  machine instead of downloading it again (Linux, HiveOS and Windows). The miner checks its SHA-256 before using it.

## Measured on our test rigs

- RTX 5090: 95.41 N/s (steady mining, 12 min; 91.70 including job changes), 10 accepted / 0 rejected (+1 stale), 0 invalid.
- RTX 4090: 66.23 N/s (median, 10 min), 2 accepted / 0 rejected.

Live rates depend on the card, its cooling and power limit; these are measurements, not a promise.

## New here? Create your pool account first

1. Open https://tsc.the-new.win/#/home and go to **Start mining** (开始挖矿; the site has an English version).
2. Under **1. Bind Wallet**, enter your TSC payout address (`tc1...`, check it carefully), choose a pool password
   (6–128 characters) and confirm it, then click **Apply to join pool**. The account works at once.
3. Start the miner with the same wallet and password (`-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`). After about
   3–5 minutes your rig appears under **My Miners**. The pool cannot recover a forgotten password.

## How to update

Your settings and the downloaded model are kept.

**Linux, one command** (for an install in `~/thenewminer`). Stop the old version first:

```bash
~/thenewminer/thenewminer --stop
```

Then replace `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` and `<RIG_NAME>` (including the `<` `>`) and run:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.46 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

**Windows:** download `TheNewMiner-v0.1.46-windows-wsl2-r1.zip`, unzip it anywhere and double-click `TheNewMiner.bat`.
It finds the installed miner and upgrades it without questions.

**HiveOS:** change both version numbers in the flight sheet's installation URL to
`https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/thenewminer-0.1.46.tar.gz` and apply it. New users: import
`https://github.com/ngasuka855/TheNewMiner/blob/v0.1.46/hiveos/flightsheet-thenewminer.json` (replace `<YOUR_WALLET>` and `<YOUR_POOL_PASSWORD>`).

## Downloads

- Release page and full instructions: https://github.com/ngasuka855/TheNewMiner
- Linux: https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/TheNewMiner-v0.1.46-linux-x86_64-sealed.tar.xz
- Windows 10 / 11 (WSL2): https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/TheNewMiner-v0.1.46-windows-wsl2-r1.zip
- HiveOS package: https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/thenewminer-0.1.46.tar.gz
- Checksums: https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/SHA256SUMS

Verify before use (`sha256sum -c --ignore-missing SHA256SUMS`); if it does not match, do not use the file. NVIDIA
driver 580 or newer is required (see the README).

## Known issues

- The first-start changes for RTX 30 series cards and HiveOS were tested on RTX 4090 and RTX 5090 machines, not yet on
  a real RTX 30 series card or HiveOS rig: please report how the first start goes.
- On RTX 40 and 50 series cards a GPU check that times out still stops the start (exit code 78 with a `Reason:` line);
  starting again retries it (HiveOS does this by itself).
- HiveOS support is experimental: please report problems.

---

# 简体中文

TheNewMiner v0.1.46 已发布，支持 Linux、Windows 和 HiveOS（实验性）。这个版本让 RTX 30 系列显卡和 HiveOS 的首次启动更稳定，并让 wave 的衔接更顺畅。所有使用 v0.1.45 或更早版本的用户都应更新。

## 新功能

- **RTX 30 系列和 HiveOS 首次启动更稳定：** 一次性 GPU 检查所用的 GPU 代码现在已为每个支持的显卡架构预先编译，并在安装时先运行一次；GPU 编译缓存会在多次启动和升级之间保留。如果在 RTX 30 系列显卡上检查仍然超时，挖矿程序会改在 CPU 上完成这一步并继续挖矿。如果无法为显卡准备好配置，挖矿程序会用一行 `Reason:` 说明原因（退出码 78）。
- **HiveOS：** 首次启动时如果无法为显卡准备好配置（退出码 78），矿机会等待并以逐渐加长的间隔（一到十分钟）重试，而不是每隔几秒就重启。
- **wave 更顺畅（root-prefetch tick）：** 当矿池的 checkpoint 在 wave 中途变化时，下一个 wave 的工作会在后台重新准备。在 RTX 5090 上，这消除了此时的等待（持续算力 +0.3%，结果完全一致）。
- **密码被拒后不再在内存中留下文件：** 停止后运行 `--status` 和 `--logs`，不再在内存中留下已解压的程序文件（约 1.5 GB）。
- **已经有模型？直接复制：** README 说明了如何从其他机器复制 `Qwen3-8B-Q8_0-full-v2.gguf`，无需重新下载（Linux、HiveOS 和 Windows）。挖矿程序会先校验其 SHA-256 再使用。

## 测试矿机实测

- RTX 5090：95.41 N/s（稳定挖矿，12 分钟；含任务切换为 91.70），接受 10 / 拒绝 0（另有 1 个过期份额），无效 0。
- RTX 4090：66.23 N/s（中位数，10 分钟），接受 2 / 拒绝 0。

实际算力取决于显卡、散热和功耗限制；以上为实测值，并非承诺。

## 新用户？请先创建矿池账户

1. 打开 https://tsc.the-new.win/#/home，进入 **开始挖矿**（Start mining）。
2. 在 **1. 绑定钱包** 中填写你的 TSC 收款地址（`tc1...`，请仔细核对），设置矿池密码（6–128 个字符）并确认，然后点击 **申请加入矿池**。账户立即可用。
3. 用同一个钱包和密码启动挖矿程序（`-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`）。约 3–5 分钟后，你的矿机会出现在 **我的矿工** 页面。矿池不提供密码找回服务。

## 如何更新

你的设置和已下载的模型都会保留。

**Linux，一条命令**（适用于安装在 `~/thenewminer` 的情况）。先停止旧版本：

```bash
~/thenewminer/thenewminer --stop
```

然后替换 `<YOUR_WALLET>`、`<YOUR_POOL_PASSWORD>` 和 `<RIG_NAME>`（连同 `<` `>` 一起替换）并运行：

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.46 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

**Windows：** 下载 `TheNewMiner-v0.1.46-windows-wsl2-r1.zip`，解压到任意位置，双击 `TheNewMiner.bat`。它会找到已安装的挖矿程序并直接升级，无需回答任何问题。

**HiveOS：** 把 flight sheet 安装 URL 中的两处版本号改为 `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/thenewminer-0.1.46.tar.gz` 并应用。新用户：导入 `https://github.com/ngasuka855/TheNewMiner/blob/v0.1.46/hiveos/flightsheet-thenewminer.json`（替换 `<YOUR_WALLET>` 和 `<YOUR_POOL_PASSWORD>`）。

## 下载

- 发布页和完整说明：https://github.com/ngasuka855/TheNewMiner
- Linux：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/TheNewMiner-v0.1.46-linux-x86_64-sealed.tar.xz
- Windows 10 / 11（WSL2）：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/TheNewMiner-v0.1.46-windows-wsl2-r1.zip
- HiveOS 安装包：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/thenewminer-0.1.46.tar.gz
- 校验文件：https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/SHA256SUMS

使用前请校验（`sha256sum -c --ignore-missing SHA256SUMS`）；如果不一致，请不要使用该文件。需要 NVIDIA 驱动 580 或更新（见 README）。

## 已知问题

- RTX 30 系列和 HiveOS 的首次启动改动已在 RTX 4090 和 RTX 5090 上测试，尚未在真实的 RTX 30 系列显卡或 HiveOS 矿机上测试：请反馈首次启动的情况。
- 在 RTX 40 和 50 系列显卡上，GPU 检查超时仍会中止启动（退出码 78，并有一行 `Reason:`）；再次启动会重试（HiveOS 会自动重试）。
- HiveOS 为实验性支持：遇到问题请反馈。
