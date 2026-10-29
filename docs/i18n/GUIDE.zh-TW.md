[English](../GUIDE.md) · [简体中文](GUIDE.zh-CN.md) · **繁體中文** · [Русский](GUIDE.ru.md) · [Español](GUIDE.es.md) · [Português (Brasil)](GUIDE.pt-BR.md) · [Français](GUIDE.fr.md) · [Deutsch](GUIDE.de.md) · [Türkçe](GUIDE.tr.md) · [Tiếng Việt](GUIDE.vi.md) · [한국어](GUIDE.ko.md) · [日本語](GUIDE.ja.md)

> 本翻譯僅為方便閱讀而提供，內容以英文 README 為準。 *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner 指南

首次啟動以外的所有內容：驗證下載的檔案，在 Linux、Windows 和 HiveOS 上的日常使用，升級，沒有在挖礦時該怎麼辦，以及常見問題。下載和快速入門請見[主頁面](README.zh-TW.md)；Windows 問題另有專頁：[Windows 疑難排解](WINDOWS-TROUBLESHOOTING.zh-TW.md)。

## 驗證下載的檔案

| 檔案 | SHA-256 |
|---|---|
| [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | `86c215dc9401209b8d2a2346252ed5cc1901527b6a51520372e3128583c63656` |
| [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | `45a459646713bef9f8285848486c0a4bd66dee65e8c63090c8d3345c998b2659` |
| [`thenewminer-0.1.47.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz)（HiveOS） | `6b2515c5bf23f60daa236ff4a8b165ce5e55265d7eecac9dd408896e1e31b784` |

Windows 的 zip 是一體式安裝包：已經包含 Linux 挖礦程式，只需要下載這一個 zip。將安裝包和 [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS) 放在同一個資料夾，然後執行：

```bash
# Linux（--ignore-missing：只驗證你已下載的安裝包）
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows（命令提示字元）：將輸出結果與上表中的 SHA-256 比對
certutil -hashfile TheNewMiner-v0.1.47-windows-wsl2-r3.zip SHA256
```

每個檔案都必須顯示 `OK`（在 Windows 上則是與上表完全相同的 64 個字元）。如果不一致，請勿使用。

## Linux

```bash
tar -xJf TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| 指令 | 用途 |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | 第一次啟動：安裝所需元件，然後在背景挖礦 |
| `./thenewminer --background` | 之後啟動：設定已被記住，不必再輸入 |
| `./thenewminer` | 在前景挖礦（按 Ctrl+C 停止） |
| `./thenewminer --logs` | 查看挖礦程式運作情形（按 Ctrl+C 離開檢視，挖礦會繼續） |
| `./thenewminer --status` | 是否正在挖礦、最近的算力，以及被接受 / 被拒絕的份額；停止後會說明停止的原因以及解決方法 |
| `./thenewminer --stop` | 停止挖礦 |
| `./thenewminer --help` | 查看所有選項 |

- `-u` 是你的 TensorCash 錢包地址，`-p` 是你的礦池密碼，`-w` 是在礦池上顯示的礦工名稱（預設：本機的主機名稱；1–64 個英文字母、數字、`_`、`-` 或 `.`）。
- 錢包地址會在任何程式啟動之前接受檢查：有打字錯誤的地址（檢查碼不符）或範例地址會被拒絕，並說明錯在哪裡。
- 礦池密碼：6–128 個字元，可以是任何可見字元，不能有空格。在命令列上請一律用單引號將它括起來，例如 `-p 'my$pass!'`（單引號內的 `'` 要寫成 `'\''`）。
- 設定會被記住，儲存在 `thenewminer/.miner/` 中（存放密碼的檔案只有你本人可以讀取）。錢包和密碼會立即從命令列中移除，因此不會出現在 `ps` 中（但你的 shell 仍可能在歷史紀錄中保留這行指令）。
- 單卡機器第一次啟動時，挖礦程式會在下載模型 **之前** 先向礦池核對錢包和密碼，輸入錯誤幾秒內就能發現（多卡礦機要到開始挖礦時才會發現）。如果礦池拒絕了它們，挖礦程式會顯示訊息並停止：請檢查後以 `-u` 和 `-p` 重新啟動。
- 第一次啟動會下載模型（約 8.1 GiB，只下載一次）、安裝適合你顯示卡的 GPU 引擎，並在顯示卡上執行自我檢測，然後開始挖礦。看到 `[ RATE] ... N/s` 這類訊息，就表示正在挖礦。日誌檔案是 `thenewminer/.miner/thenewminer.log`。
- 標準的 Ubuntu 22.04 / 24.04（包括 WSL 中的）已具備挖礦程式需要的所有程式庫，因此不會安裝任何東西。缺少時（Docker 映像檔、精簡系統），在 Ubuntu、Debian 和 HiveOS 上第一次啟動會列出缺少的程式庫並以 `apt` 安裝（需要 sudo 或 root 權限）；在其他系統上，它會指出這些程式庫，由你自行安裝。
- 多卡礦機：支援，每張 GPU 都會挖礦；此版本是在每台機器一張 GPU 的情況下測試的。
- 在 Docker 或其他共用記憶體較小的容器中，挖礦程式會從磁碟上的 `thenewminer/.miner/run/` 執行其程式檔案，並顯示提示。若要選擇其他資料夾，請以 `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` 啟動（該資料夾必須只有你可以寫入：`chmod 700 <folder>`）。
- **升級**：先執行 `./thenewminer --stop`，在同一個位置解壓縮新的安裝包（會解壓縮到 `thenewminer/`，並保留 `thenewminer/.miner/` 中的設定和模型），然後重新啟動。如果舊版本仍在執行，新版本會拒絕啟動，並提示你先停止舊版本。如果改為解壓縮到新的資料夾，它會找到舊版本的模型（大小和 SHA-256 都相同）並直接使用：不必再下載一次 8.1 GiB。在新資料夾中要重新提供 `-u -p -w`。

### 沒有在挖礦時

`./thenewminer --status` 會說明挖礦程式停止的原因以及解決方法。最常見的訊息：

| 你看到 | 該怎麼做 |
|---|---|
| `bash: ./thenewminer: Permission denied` | 這個資料夾不允許執行程式（`noexec` 掛載，例如某些 `/tmp`，或 USB 隨身碟）。請解壓縮到你的家目錄：`cd ~ && tar -xJf <the package>.tar.xz && cd thenewminer` |
| `this package is on a FAT (vfat) drive`（或 exFAT / NTFS，或 `Windows (DrvFs, 9p)`：Windows 磁碟機，例如 WSL 中的 `/mnt/d`） | 這種磁碟機無法保存挖礦程式的私人設定（權限 600）。貼上它顯示的 `mkdir -p ~/thenewminer && cp -a ...` 那一行：它會將資料夾複製到你的 Linux 家目錄並在那裡啟動 |
| `this system has no xz` | `sudo apt-get install -y xz-utils`（以 root 身分執行時不需要 `sudo`），然後重新啟動 |
| `This system is missing what TheNewMiner needs` | 執行它顯示的 `sudo apt-get install -y ...` 那一行，然後重新啟動 |
| `No NVIDIA GPU found on this machine` | 系統偵測不到 NVIDIA 顯示卡。請檢查顯示卡是否插好、電源是否接好；雲端主機請選擇配備 NVIDIA GPU 的機型 |
| `NVIDIA driver ... is too old` | 安裝它指出的驅動程式（580 或更新版本），重新開機後再啟動（請見下方「更新 NVIDIA 驅動程式」） |
| `the pool rejected the wallet / password` | 檢查 `-u` 和 `-p`，以及礦池網站上是否已有該帳戶，然後執行 `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' --background` |
| `no download source answered` | 下載模型時無法連線到網際網路。需要透過代理伺服器上網時：`https_proxy=http://<HOST>:<PORT> ./thenewminer --background`；或者複製模型（請見下方「已經有模型了？」） |
| `cannot reach the pool` / `pool disconnected` | 檢查網路連線、DNS，以及防火牆是否允許對外的 TCP 443；如果你的網路封鎖了礦池，請使用 `--proxy`（請見下方「代理伺服器」）。它會自動重新連線 |

### 開機後自動啟動（選用）

重新開機後，挖礦程式不會自行啟動（HiveOS 會替你處理）。在 Linux 上，可以加入一行 `crontab`：

```bash
crontab -e      # 然後加入這一行（換成你的資料夾）：
@reboot cd /home/<YOU>/thenewminer && ./thenewminer --background
```

或者使用 systemd 使用者服務（先執行一次 `loginctl enable-linger <YOU>`，這樣不登入也能啟動）：

```ini
# ~/.config/systemd/user/thenewminer.service，然後執行：systemctl --user enable --now thenewminer
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

結束代碼 77 表示礦池拒絕了錢包 / 密碼：重新啟動也沒有用，所以服務會停在那裡。

## Windows

1. 將整個 `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` 解壓縮（按右鍵 › 全部解壓縮）到任何位置：任何資料夾、任何磁碟機皆可，路徑含有空格或中文字也沒問題。請勿在 zip 的預覽視窗中直接執行。
2. 按兩下 **`TheNewMiner.bat`**。
   - **第一次**：會有 **一個** 視窗先詢問你的礦池帳戶：錢包地址（以 `tc1` 開頭；會檢查其檢查碼，視窗中顯示的範例不是真正的錢包）、礦池密碼（6–128 個字元，可以是任何可見字元，不能有空格）和礦工名稱（已填入電腦名稱）。三項都正確之前，OK 按鈕會一直是灰色的。按下 OK 和「是」之後，就不會再詢問任何事：它會檢查 Windows、驅動程式和磁碟；必要時開啟 WSL2（會跳出一次系統管理員確認；可能需要重新開機一次，之後安裝會自動繼續）；顯示資料的存放位置；建立專用的 Linux（WSL）環境，安裝內附的挖礦程式，下載模型，然後開始挖礦。沒有桌面時（透過 SSH），同樣的問題會在主控台中詢問。
   - **之後每一次**：按兩下即會以已儲存的設定立即開始挖礦。桌面上也會建立一個「TheNewMiner」捷徑。如果已經在挖礦時再按兩下，會告訴你正在挖礦並顯示日誌。
   - **如果安裝中途停止**，視窗會說明停在哪個步驟以及該怎麼做，`TheNewMiner.bat` 也會再次顯示這些訊息。請見 [Windows 疑難排解](WINDOWS-TROUBLESHOOTING.zh-TW.md)。

日常指令（在解壓縮後的資料夾中開啟命令提示字元）：

| 指令 | 用途 |
|---|---|
| `TheNewMiner.bat` | 必要時先安裝，然後在此視窗中挖礦（關閉視窗即停止挖礦） |
| `TheNewMiner.bat background` | 不開視窗挖礦 |
| `TheNewMiner.bat logs` | 查看挖礦程式運作情形（按 Ctrl+C 離開檢視，挖礦會繼續） |
| `TheNewMiner.bat status` | 是否正在執行，以及最近的算力 |
| `TheNewMiner.bat stop` | 停止挖礦 |
| `TheNewMiner.bat setup` | 重新輸入錢包 / 密碼 / 礦工名稱（按 Cancel 則保留原有設定） |
| `TheNewMiner.bat uninstall` | 移除挖礦程式、其資料資料夾和捷徑（會先詢問） |

- **資料存放位置**：挖礦程式和模型存放在第一次安裝時選定的資料資料夾中（預設為解壓縮所在磁碟機根目錄下的 `TheNewMinerData`，例如 `D:\TheNewMinerData`），而不是在解壓縮後的資料夾裡。該處需要約 20 GB 的可用空間。若要選擇其他資料夾：`TheNewMiner.bat setup -DataDir <folder>`。
- **Ubuntu**：安裝程式以 `wsl --install` 安裝 Ubuntu 24.04。如果該下載無法連線，它會從中國的公共鏡像站（依序為清華 TUNA、中科大 USTC、阿里雲）下載 Ubuntu 官方的 WSL 映像檔 `ubuntu-24.04.5-wsl-amd64.wsl`，並且只有在其 SHA-256 與 `bb415d824822c4b878125729af451a5d18fb13d1cf5cbed9a7393ad64ac6039e` 完全相符時才會使用。
- 如果礦池拒絕了錢包或密碼，視窗會顯示訊息並停止挖礦：請執行 `TheNewMiner.bat setup` 重新輸入。
- **升級**：將新版本解壓縮到任何位置，按兩下其中的 `TheNewMiner.bat`。它會找到已安裝的挖礦程式，保留你的設定和模型，直接安裝新版本，不會再詢問。由較早版本或手動在其他 WSL 發行版本（例如 `Ubuntu-24.04`）中完成的安裝，會被找到並在原處升級：不會重複安裝、不會新裝 Ubuntu，也不會再次下載模型。如果無法在原處升級，安裝程式會停止並說明原因。舊資料夾可以刪除。
- **解除安裝**：`TheNewMiner.bat uninstall`。它絕不會移除你自己的 WSL 發行版本。
- Windows 安裝包的介面語言為英文。

## HiveOS（實驗性）

HiveOS 支援目前為 **實驗性**（自 v0.1.45 起提供，該版本已在實際的 HiveOS 礦機上執行過；v0.1.46 之後的變更尚未在 HiveOS 礦機上執行過）。它使用 HiveOS 自訂挖礦程式（custom miner）安裝包 `thenewminer-0.1.47.tar.gz`（其 SHA-256 請見「驗證下載的檔案」下的表格）。

快速設定（匯入 flight sheet）：

1. **Wallets**（錢包）：新增一個錢包，幣種（coin）選 `TSC`，並填入你的 TensorCash 地址（`tc1...`）。此步驟只需做一次：HiveOS 需要選取一個 `TSC` 錢包，而挖礦程式使用的是你在 flight sheet 中填寫的地址。
2. 開啟 [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json)，複製其內容。將 `<YOUR_WALLET>` 替換為你的錢包地址（保留其後的 `.%WORKER_NAME%`），將 `<YOUR_POOL_PASSWORD>` 替換為你的礦池密碼（在 JSON 中，密碼裡的 `"` 要寫成 `\"`，`\` 要寫成 `\\`）。在 HiveOS 中選擇 Flight Sheets › **Import from clipboard**（從剪貼簿匯入），貼上內容，選擇你的 `TSC` 錢包，然後儲存。
3. 將 flight sheet 套用到你的礦機。礦機會下載安裝包並開始挖礦。

也可以改為手動設定：建立一個 flight sheet，Coin 選 `TSC`，Wallet 選你的 `TSC` 錢包，Pool 選「Configure in miner」，Miner 選「Custom」，按一下 **Setup Miner Config**，然後填寫：

| 欄位 | 值 |
|---|---|
| Miner name | `thenewminer`（依 Installation URL 自動填入） |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` |
| Hash algorithm | 留空 |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win`（會被忽略：礦池已內建） |
| Pass | 你的礦池密碼 |
| Extra config arguments | 留空，或填入 `--proxy <YOUR_PROXY>`（請見「代理伺服器」一節） |

- 礦池上顯示的礦工名稱就是該礦機在 HiveOS 中的名稱：只能使用英文字母、數字、`_`、`-` 或 `.`（不能有空格），最多 64 個字元。如需使用其他礦工名稱，請在 Extra config arguments 中加入 `-w <RIG_NAME>`。
- HiveOS 磁碟上需要約 12 GB 的可用空間，用於存放模型和挖礦程式（可用 `df -h /hive/miners` 查看）。
- 模型（約 8.1 GiB）只會下載一次，儲存在 `/hive/miners/custom/.thenewminer-models`，升級時會保留。
- 錢包和密碼透過環境變數傳給挖礦程式，而不是透過命令列。
- **發生錯誤時**（例如礦池拒絕了錢包或密碼），挖礦程式的日誌和 HiveOS 儀表板會顯示原因，礦機會重試，間隔逐漸拉長（一到十分鐘）。請修正 flight sheet 並套用：這會立即重新啟動挖礦程式。
- 日誌：HiveOS 中的挖礦程式畫面（在礦機的 shell 中執行 `miner`）或 `/var/log/miner/custom/custom.log`。
- **升級**：修改 Installation URL 中的兩處版本號碼（例如 `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`），然後套用 flight sheet。只有 URL 中的檔案名稱改變時，HiveOS 才會安裝。
- **首次啟動**會下載模型並安裝 GPU 引擎，需要幾分鐘。
- **已經有模型了？** 在首次啟動前，將它複製到礦機上的 `/home/user/`（請見下一節）。

**如果礦機無法連線到 GitHub**：在電腦上下載 `thenewminer-0.1.47.tar.gz`（核對其 SHA-256），複製到礦機上，並解壓縮到 HiveOS 存放自訂挖礦程式的位置。然後照常套用 flight sheet：HiveOS 會顯示「Already installed」，不會再下載。檔案名稱必須維持為 `thenewminer-0.1.47.tar.gz`（瀏覽器可能會加上 " (1)"），也不要修改 flight sheet 中的 Installation URL：HiveOS 比對的正是這個檔案名稱。

```bash
# 在電腦上
scp thenewminer-0.1.47.tar.gz user@<RIG_IP>:/tmp/
# 在礦機上，以 root 身分（Hive Shell，或 SSH 登入後執行 sudo -i）
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.47.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.47.tar.gz
```

## 已經有模型了？直接複製

挖礦程式需要一個 8.1 GiB 的模型檔案：`Qwen3-8B-Q8_0-full-v2.gguf`（`8709518112` 位元組，SHA-256 `408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`）。如果這台電腦上，或你網路中的另一台電腦上已經有這個檔案，請直接複製，不必重新下載。

在下載任何內容之前，挖礦程式會依照確切的檔案名稱和大小尋找該檔案：它自己的模型資料夾、先前的 TheNewMiner 資料夾、你的家目錄（以及其下一兩層的資料夾，或其中的 `models` 資料夾）、`/home` 下的各個家目錄、`/data`、`/mnt/*`、`/media/*`、`/opt`、`/srv`、`/workspace`、`/var/lib`，以及 Hugging Face / ModelScope 快取。如果這些位置都找不到，它會花最多 20 秒在同一磁碟上的這些資料夾中搜尋任何大小完全相同的 `.gguf` 檔案（其他已掛載的磁碟只依檔案名稱尋找）。找到的檔案 **只有在 SHA-256 相符時** 才會被使用；接著挖礦程式會將它連結（位於同一磁碟時）或複製到自己的模型資料夾中，這樣之後的每個版本都能立即找到它。你也可以直接指定它：`./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`。

**Linux**：將它複製到你的家目錄，然後照常啟動挖礦程式。若要從同一網路中的 Mac 或電腦複製（Windows 10 和 11 的 PowerShell 內建 `scp`），請將 `user` 和 `<MINER_IP>` 換成挖礦主機的登入名稱和位址：

```bash
# 在挖礦主機本機上
cp /path/to/Qwen3-8B-Q8_0-full-v2.gguf ~/
# 或從另一台電腦
scp Qwen3-8B-Q8_0-full-v2.gguf user@<MINER_IP>:~/
```

**HiveOS**：將它複製到礦機的 `/home/user/`（HiveOS 的 SSH 登入名稱是 `user`），然後套用 flight sheet。挖礦程式會在那裡找到它並驗證，再將它保存在安裝包旁邊（`/hive/miners/custom/.thenewminer-models`），因此新版本不會再次下載。HiveOS 的磁碟很小：請先執行 `disk-expand`（如果 `df -h /hive/miners` 顯示可用空間不足 12 GB）。

```bash
scp Qwen3-8B-Q8_0-full-v2.gguf user@<RIG_IP>:/home/user/
```

**Windows**：第一次執行 `setup` 建立 TheNewMiner 的 WSL 發行版本之後，從命令提示字元將檔案複製進去（將 `D:\Models` 改成你的副本所在的位置）。如果下載已經開始，請關閉視窗，執行下列指令，然後再次啟動 TheNewMiner：它會找到這份副本並略過下載。複製需要幾分鐘，過程中不會顯示進度。

```bat
wsl -d TheNewMiner -u root -- mkdir -p ~/thenewminer/models
wsl -d TheNewMiner -u root -- cp /mnt/d/Models/Qwen3-8B-Q8_0-full-v2.gguf ~/thenewminer/models/
```

## 更換錢包、礦池密碼或礦工名稱

礦池會將每個收款地址永久綁定到它自己的礦池帳戶：礦池頁面 [tsc.the-new.win](https://tsc.the-new.win) 將 TSC 收款地址標示為「永久绑定 · 不可修改」（永久綁定，不可修改），並說明若要更換，請直接在挖礦程式中使用新地址。因此，改用另一個錢包需要它自己的礦池帳戶和礦池密碼（請先在礦池頁面上設定好）。如果你在礦池頁面上變更了礦池密碼，請在每個挖礦程式中都更新它。礦池無法找回忘記的密碼。

**Linux**：先停止挖礦程式，然後用新設定啟動一次；新設定會取代已記住的設定：

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

`-u` 和 `-p` 必須一起提供：只提供其中一個不會變更已記住的錢包和密碼。礦工名稱可以單獨變更：`./thenewminer -w <RIG_NAME> --background`（在 `--stop` 之後）。

**Windows**：執行 `TheNewMiner.bat setup`。它會開啟帳戶視窗，並已填入你的礦工名稱；輸入錢包和礦池密碼，然後照常啟動（按兩下 `TheNewMiner.bat`）。

**HiveOS**：編輯 flight sheet：在「Wallet and worker template」中填入新的錢包地址（`<NEW_WALLET>.%WORKER_NAME%`），在 Pass 中填入新的礦池密碼，然後儲存並套用。礦工名稱就是該礦機在 HiveOS 中的名稱（或在 Extra config arguments 中使用 `-w <RIG_NAME>`）。

## 代理伺服器（選用）

挖礦程式會直接連線到礦池。如果你的網路需要使用代理伺服器：

```bash
# Linux（會一直記住，直到你變更為止）
./thenewminer --proxy <IP>[:<PORT>]            # TCP 中繼（省略連接埠時為 443）
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5，也支援 socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # HTTP CONNECT 代理伺服器
./thenewminer --proxy none                     # 恢復直接連線
```

這些指令同時也會啟動挖礦程式：請先停止挖礦程式；加上 `--background` 可讓它在背景執行。

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

TLS 一律與礦池進行端對端加密，因此代理伺服器無法讀取連線內容。在 Windows 上，於同一台電腦執行的代理程式，通常無法從 WSL 以 `127.0.0.1` 連線（WSL 預設網路模式）：請改用電腦的區域網路 IP，並在代理程式中允許區域網路連線。

## 系統需求

| 項目 | 需求 |
|---|---|
| 顯示卡 | NVIDIA GeForce RTX 30 / 40 / 50 系列，顯示記憶體 10 GB 以上。多卡礦機：支援，每張 GPU 都會挖礦；此版本是在每台機器一張 GPU 的情況下測試的 |
| 驅動程式 | NVIDIA **580 或更新版本**（`nvidia-smi` 必須能正常執行）。在 Windows 上請安裝於 Windows 中，切勿安裝在 WSL 內 |
| Linux | x86_64，需要 **glibc 2.34 或更新版本**，例如 Ubuntu 22.04 或更新版本、Debian 12 或更新版本。安裝程式會檢查所需條件，並列出缺少的項目 |
| Windows | Windows 10 21H2 或更新版本，或 Windows 11，64 位元；已在 BIOS 中啟用虛擬化（Intel VT-x / AMD SVM） |
| 磁碟 | 約 20 GB 可用空間 |
| 記憶體 | 4 GB 以上 |
| 模型 | 約 8.1 GiB，第一次啟動時下載一次（多鏡像平行下載），之後重複使用 |

## 更新 NVIDIA 驅動程式（580 或更新版本）

挖礦程式需要 NVIDIA 驅動程式 **580 或更新版本**。先查看你目前的驅動程式版本：

```bash
nvidia-smi    # 標頭會顯示 "Driver Version: ..."；必須是 580 或更新版本
```

- **Ubuntu**：`sudo ubuntu-drivers install` 會安裝建議的驅動程式（安裝後請確認版本為 580 或更新）。若想自行選擇版本：`sudo apt install nvidia-driver-580`（或更新版本；RTX 50 系列顯示卡需要 open 版本，例如 `nvidia-driver-580-open`）。然後重新開機。Ubuntu 的驅動程式套件和 NVIDIA 的 `.run` 安裝程式只能擇一使用，切勿混用。
- **Debian**：Debian 內建的驅動程式套件版本低於 580（Debian 12 為 535，Debian 13 為 550）。請依照 [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)（NVIDIA 驅動程式安裝指南）的說明，從 NVIDIA 的套件庫安裝較新的驅動程式，然後重新開機。
- **Windows**：使用 [NVIDIA App](https://www.nvidia.com/en-us/software/nvidia-app/)（前身為 GeForce Experience）更新，或從 [nvidia.com](https://www.nvidia.com/en-us/drivers/) 下載適用於你顯示卡的驅動程式（580 或更新版本）。WSL2 使用的就是這個 Windows 驅動程式：切勿在 WSL 內安裝 Linux 版 NVIDIA 驅動程式。
- **HiveOS**：以 root 身分（Hive Shell，或透過 SSH 登入後執行 `sudo -i`）執行 `nvidia-driver-update --list` 查看可用版本，然後執行例如 `nvidia-driver-update 595`（任何 580 或更新版本皆可），最後執行 `sreboot`。

## 常見問題

**Windows 顯示「Windows 已保護您的電腦」/「未知的發行者」。**
本安裝包沒有程式碼簽章。請按一下 **其他資訊**，再按一下 **仍要執行**。

**防毒軟體將挖礦程式標記為威脅或刪除。**
Microsoft Defender 和其他防毒軟體經常將加密貨幣挖礦程式標記為「可能不需要的應用程式」，即使是你自行選擇執行的合法程式也一樣，因為惡意軟體常會暗藏挖礦程式。如果檔案被隔離：開啟「Windows 安全性 › 病毒與威脅防護 › 保護歷程記錄」，選取 TheNewMiner 項目並選擇 **允許**（或將解壓縮後的資料夾加入「排除項目」）。請只對從本頁面下載、且 SHA-256 相符的安裝包這樣做。

**第一次啟動要很久。**
第一次啟動會下載約 8.1 GiB 的模型，並在你的顯示卡上執行 GPU 自我檢測；在 Windows 上可能還要設定 WSL2。這只會發生一次。之後啟動會快得多，很快就開始挖礦。

**挖礦程式顯示礦池拒絕了錢包 / 密碼。**
請檢查錢包地址和礦池密碼（礦池網站上必須已有該帳戶），然後重新啟動：Linux 使用 `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`，Windows 使用 `TheNewMiner.bat setup`。憑證錯誤時，挖礦程式不會自行重試。

**是否正在挖礦？日誌在哪裡？**
Linux：`./thenewminer --status`、`./thenewminer --logs`，或檔案 `thenewminer/.miner/thenewminer.log`。
Windows：`TheNewMiner.bat status` 和 `TheNewMiner.bat logs`。

**我的 RTX 3080（10 GB）在 Windows 上挖礦比在 Linux 上慢很多。**
在 Windows 上，其他程式會共用顯示記憶體。請見 [Windows 疑難排解](WINDOWS-TROUBLESHOOTING.zh-TW.md)：關閉開啟硬體加速的瀏覽器和 Docker Desktop。
