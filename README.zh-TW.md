[English](README.md) · [简体中文](README.zh-CN.md) · **繁體中文** · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> 本翻譯僅為方便閱讀而提供，內容以英文 README 為準。 *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner 是 **TensorCash** 的 GPU 挖礦程式，支援 **顯示記憶體 10 GB 以上** 的 NVIDIA GeForce RTX 30、40 和 50 系列顯示卡，在礦池 **`tsc-wss.the-new.win`** 上挖礦。礦池位址已內建於程式中：不需要輸入礦池位址，會直接連線。

目前版本：**v0.1.45**

## v0.1.45 新功能

- **礦池拒絕你的錢包或密碼時，會顯示清楚的訊息。** 挖礦程式會以一行訊息說明，然後停止，不再重試；`--status` 會顯示原因。請檢查錢包和密碼，然後以 `-u` 和 `-p` 重新啟動。
- **你的錢包和密碼不再出現在 `ps`** 或其他使用者的處理程序清單中。
- **`./thenewminer --status`** 會顯示是否正在挖礦、從何時開始、最近的算力，以及被接受 / 被拒絕的份額。
- **支援 HiveOS（實驗性）**：提供 HiveOS 自訂挖礦程式（custom miner）安裝包和可匯入的飛行表（flight sheet）（請見下方 HiveOS 一節）。
- **安全升級**：舊版本仍在挖礦時，新版本會拒絕啟動（並提示你先停止舊版本）；執行 `--stop` 之後，新版本會自行重新安裝程式檔案，並保留你的設定和模型。
- **`--background` 在挖礦程式隨即結束時不再顯示「已啟動」**，而是顯示原因。
- **Docker 與較小的共用記憶體**：當記憶體中的資料夾太小（Docker 預設為 64 MiB）或不允許執行程式時，挖礦程式會改用磁碟上的 `thenewminer/.miner/run/`，並顯示提示。
- **礦池密碼現在可以使用任何可見字元**（包括標點符號和非英文字母）：6–128 個字元，不能有空格。
- 支援更多 Linux 系統：安裝程式現在會檢查它所需的條件（glibc 2.34 或更新版本），而不是只接受少數幾個 Ubuntu 版本。

## 下載

| 檔案 | 適用於 | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz) | Linux x86_64 | `6281bf489fae843551114f09ead5498d3e47af33a07e4cfdfbdc0a5d6bdbafbb` |
| [`TheNewMiner-v0.1.45-windows-wsl2-r1.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-windows-wsl2-r1.zip) | Windows 10 / 11（透過 WSL2 執行） | `90732441b0dc67de84809d9bf3bda4441c0bf7cd9a52fefb95d29242ad7b15e7` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/SHA256SUMS) | 所有安裝包的檢查碼 | |

Windows 的 zip 是一體式安裝包：已經包含 Linux 挖礦程式，只需要下載這一個 zip。

## 驗證下載的檔案

將安裝包和 `SHA256SUMS` 放在同一個資料夾，然後執行：

```bash
# Linux（--ignore-missing：只驗證你已下載的安裝包）
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows（命令提示字元）：將輸出結果與上表中的 SHA-256 比對
certutil -hashfile TheNewMiner-v0.1.45-windows-wsl2-r1.zip SHA256
```

每個檔案都必須顯示 `OK`（在 Windows 上則是與上表完全相同的 64 個字元）。如果不一致，請勿使用。

## 一鍵安裝（Linux）

將這行指令末尾的 `<YOUR_WALLET>`、`<YOUR_POOL_PASSWORD>` 和 `<RIG_NAME>`（連同 `<` `>` 一起）替換成你自己的內容，然後貼到終端機中執行。它會下載此版本的 Linux 安裝包和 `SHA256SUMS`（使用 `curl`；或使用 `wget`，適用於沒有 `curl` 的系統），驗證 SHA-256，將挖礦程式解壓縮到 `~/thenewminer`，並在背景啟動：

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.45 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

任何一個步驟失敗，指令都會在該步驟停止：下載出錯或 SHA-256 不一致時，不會啟動任何程式（驗證是為了防範下載損毀；兩個檔案都來自本頁面）。請保留密碼兩側的單引號：在單引號內，任何字元都是安全的（密碼中的 `'` 要寫成 `'\''`）。如果 `tar` 顯示 `xz: Cannot exec`，請安裝 xz（`sudo apt-get install -y xz-utils`），然後重新執行這行指令。

若要升級已安裝在 `~/thenewminer` 中的版本，請先執行 `~/thenewminer/thenewminer --stop`，再執行這行指令（你的設定和模型會保留）。如果舊版本位於其他資料夾，請先在該處停止它，或依照「Linux」一節的說明在原位置升級（否則會重新下載模型）。

## Linux

```bash
tar -xJf TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| 指令 | 用途 |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | 第一次啟動：安裝所需元件，然後在背景挖礦 |
| `./thenewminer --background` | 之後啟動：設定已被記住，不必再輸入 |
| `./thenewminer` | 在前景挖礦（按 Ctrl+C 停止） |
| `./thenewminer --logs` | 查看挖礦程式運作情形（按 Ctrl+C 離開檢視，挖礦會繼續） |
| `./thenewminer --status` | 是否正在挖礦、最近的算力，以及被接受 / 被拒絕的份額 |
| `./thenewminer --stop` | 停止挖礦 |
| `./thenewminer --help` | 查看所有選項 |

- `-u` 是你的 TensorCash 錢包地址，`-p` 是你的礦池密碼，`-w` 是在礦池上顯示的礦工名稱（預設：本機的主機名稱；1–64 個英文字母、數字、`_`、`-` 或 `.`）。
- 礦池密碼：6–128 個字元，可以是任何可見字元，不能有空格。在命令列上請一律用單引號將它括起來，例如 `-p 'my$pass!'`（單引號內的 `'` 要寫成 `'\''`）。
- 設定會被記住，儲存在 `thenewminer/.miner/` 中（存放密碼的檔案只有你本人可以讀取）。錢包和密碼會立即從命令列中移除，因此不會出現在 `ps` 中（但你的 shell 仍可能在歷史紀錄中保留這行指令）。
- 如果礦池拒絕了錢包或密碼（或尚未核准該帳戶），挖礦程式會顯示訊息並停止。請檢查後以 `-u` 和 `-p` 重新啟動。
- 第一次啟動會下載模型（約 8.1 GiB，只下載一次）、安裝適合你顯示卡的 GPU 引擎，並在顯示卡上執行自我檢測，然後開始挖礦。看到 `[ RATE] ... N/s` 這類訊息，就表示正在挖礦。
- 在 Ubuntu 和 Debian 上，第一次啟動可能會用 `apt` 安裝幾個缺少的系統程式庫（需要 sudo 或 root 權限）；在其他系統上，它會列出這些程式庫，由你自行安裝。
- 多卡礦機：支援，每張 GPU 都會挖礦；此版本是在每台機器一張 GPU 的情況下測試的。
- 如果 `tar` 顯示 `xz: Cannot exec`，請先安裝 xz：`sudo apt-get install -y xz-utils`。
- 在 Docker 或其他共用記憶體較小的容器中，挖礦程式會從磁碟上的 `thenewminer/.miner/run/` 執行其程式檔案，並顯示提示。若要選擇其他資料夾，請以 `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` 啟動（該資料夾必須只有你可以寫入：`chmod 700 <folder>`）。
- **升級**：先執行 `./thenewminer --stop`，在同一個位置解壓縮新的安裝包（會解壓縮到 `thenewminer/`，並保留 `thenewminer/.miner/` 中的設定和模型），然後重新啟動。如果舊版本仍在執行，新版本會拒絕啟動，並提示你先停止舊版本。

## Windows（一鍵使用）

1. 將整個 `TheNewMiner-v0.1.45-windows-wsl2-r1.zip` 解壓縮（按右鍵 › 全部解壓縮）到任何位置：任何資料夾、任何磁碟機皆可，路徑含有空格或中文字也沒問題。請勿在 zip 的預覽視窗中直接執行。
2. 按兩下 **`TheNewMiner.bat`**。
   - **第一次**：檢查 Windows、驅動程式和磁碟；必要時開啟 WSL2（會跳出一次系統管理員確認；可能需要重新開機一次，之後安裝會自動繼續）；顯示資料的存放位置（按 Enter 確認）；建立專用的 Linux（WSL）環境，安裝內附的挖礦程式；**只詢問一次** 你的錢包地址（以 `tc1` 開頭）、礦池密碼（輸入兩次；6–128 個字元，可以是任何可見字元，不能有空格）和礦工名稱（直接按 Enter = 電腦名稱）；下載模型，然後開始挖礦。
   - **之後每一次**：按兩下即會以已儲存的設定立即開始挖礦。桌面上也會建立一個「TheNewMiner」捷徑。如果已經在挖礦時再按兩下，會告訴你正在挖礦並顯示日誌。

日常指令（在解壓縮後的資料夾中開啟命令提示字元）：

| 指令 | 用途 |
|---|---|
| `TheNewMiner.bat` | 必要時先安裝，然後在此視窗中挖礦（關閉視窗即停止挖礦） |
| `TheNewMiner.bat background` | 不開視窗挖礦 |
| `TheNewMiner.bat logs` | 查看挖礦程式運作情形（按 Ctrl+C 離開檢視，挖礦會繼續） |
| `TheNewMiner.bat status` | 是否正在執行，以及最近的算力 |
| `TheNewMiner.bat stop` | 停止挖礦 |
| `TheNewMiner.bat setup` | 重新輸入錢包 / 密碼 / 礦工名稱 |
| `TheNewMiner.bat uninstall` | 移除挖礦程式、其資料資料夾和捷徑（會先詢問） |

- **資料存放位置**：挖礦程式和模型存放在第一次安裝時選定的資料資料夾中（預設為解壓縮所在磁碟機根目錄下的 `TheNewMinerData`，例如 `D:\TheNewMinerData`），而不是在解壓縮後的資料夾裡。該處需要約 20 GB 的可用空間。
- 如果礦池拒絕了錢包或密碼，視窗會顯示訊息並停止挖礦：請執行 `TheNewMiner.bat setup` 重新輸入。
- **升級**：將新版本解壓縮到任何位置，按兩下其中的 `TheNewMiner.bat`。它會找到已安裝的挖礦程式，保留你的設定和模型，直接安裝新版本，不會再詢問。舊資料夾可以刪除。
- **解除安裝**：`TheNewMiner.bat uninstall`。
- 介面語言會依照 Windows 的語言（English / 简体中文 / 繁體中文）。若要自行指定，在任何指令後面加上 `-Lang en`（或 `zh-Hans`、`zh-Hant`），例如 `TheNewMiner.bat -Lang en`；僅對該次執行有效。

## HiveOS（實驗性）

HiveOS 支援是 v0.1.45 新增的功能，目前為 **實驗性**。它使用下方的 HiveOS 自訂挖礦程式（custom miner）安裝包。

| 檔案 | SHA-256 |
|---|---|
| [`thenewminer-0.1.45.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz) | `90576efd388ceeca4e456b62b568f267465320913a07c9f9736f42363b1701dc` |

快速設定（匯入 flight sheet）：

1. **Wallets**（錢包）：新增一個錢包，幣種（coin）選 `TSC`，並填入你的 TensorCash 地址（`tc1...`）。此步驟只需做一次：HiveOS 需要選取一個 `TSC` 錢包，而挖礦程式使用的是你在 flight sheet 中填寫的地址。
2. 開啟 [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json)，複製其內容。將 `<YOUR_WALLET>` 替換為你的錢包地址（保留其後的 `.%WORKER_NAME%`），將 `<YOUR_POOL_PASSWORD>` 替換為你的礦池密碼（在 JSON 中，密碼裡的 `"` 要寫成 `\"`，`\` 要寫成 `\\`）。在 HiveOS 中選擇 Flight Sheets › **Import from clipboard**（從剪貼簿匯入），貼上內容，選擇你的 `TSC` 錢包，然後儲存。
3. 將 flight sheet 套用到你的礦機。礦機會下載安裝包並開始挖礦。

也可以改為手動設定：建立一個 flight sheet，Coin 選 `TSC`，Wallet 選你的 `TSC` 錢包，Pool 選「Configure in miner」，Miner 選「Custom」，按一下 **Setup Miner Config**，然後填寫：

| 欄位 | 值 |
|---|---|
| Miner name | `thenewminer`（依 Installation URL 自動填入） |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz` |
| Hash algorithm | 留空 |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win`（會被忽略：礦池已內建） |
| Pass | 你的礦池密碼 |
| Extra config arguments | 留空，或填入 `--proxy <YOUR_PROXY>`（請見「代理伺服器」一節） |

- 礦池上顯示的礦工名稱就是該礦機在 HiveOS 中的名稱：只能使用英文字母、數字、`_`、`-` 或 `.`（不能有空格），最多 64 個字元。如需使用其他礦工名稱，請在 Extra config arguments 中加入 `-w <RIG_NAME>`。
- HiveOS 磁碟上需要約 12 GB 的可用空間，用於存放模型和挖礦程式（可用 `df -h /hive/miners` 查看）。
- 模型（約 8.1 GiB）只會下載一次，儲存在 `/hive/miners/custom/.thenewminer-models`，升級時會保留。
- 錢包和密碼透過環境變數傳給挖礦程式，而不是透過命令列。
- 如果礦池拒絕了錢包或密碼，挖礦程式的日誌會顯示訊息，挖礦程式會等待 10 分鐘後才由 HiveOS 重新啟動：請修正 flight sheet 並套用（這會立即重新啟動挖礦程式）。
- 日誌：HiveOS 中的挖礦程式畫面（在礦機的 shell 中執行 `miner`）或 `/var/log/miner/custom/custom.log`。
- **升級**：修改 Installation URL 中的兩處版本號碼（例如 `.../raw/v0.1.46/thenewminer-0.1.46.tar.gz`），然後套用 flight sheet。只有 URL 中的檔案名稱改變時，HiveOS 才會安裝。
- **首次啟動需要一些時間**：模型下載完成後，在 RTX 30 系列顯示卡上，一次性的 GPU 檢查在磁碟較慢的礦機上可能逾時，隨後 HiveOS 會自動重新啟動挖礦程式，重啟兩三次（約 8 分鐘）。不必理會，之後就會開始挖礦。
- **已經有模型了？** 在首次啟動前，將 `Qwen3-8B-Q8_0-full-v2.gguf`（約 8.1 GiB，檔案名稱必須完全相同）複製到礦機上的 `/home/user/`：挖礦程式會找到它，驗證其 SHA-256，並直接使用它，而不會重新下載。

**如果礦機無法連線到 GitHub**：在電腦上下載 `thenewminer-0.1.45.tar.gz`（在上方表格中核對其 SHA-256），複製到礦機上，並解壓縮到 HiveOS 存放自訂挖礦程式的位置。然後照常套用 flight sheet：HiveOS 會顯示「Already installed」，不會再下載。檔案名稱必須維持為 `thenewminer-0.1.45.tar.gz`（瀏覽器可能會加上 " (1)"），也不要修改 flight sheet 中的 Installation URL：HiveOS 比對的正是這個檔案名稱。

```bash
# 在電腦上
scp thenewminer-0.1.45.tar.gz user@<RIG_IP>:/tmp/
# 在礦機上，以 root 身分（Hive Shell，或 SSH 登入後執行 sudo -i）
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.45.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.45.tar.gz
```

## 更換錢包、礦池密碼或礦工名稱

礦池會將每個收款地址永久綁定到它自己的礦池帳戶：礦池頁面 [tsc.the-new.win](https://tsc.the-new.win) 將 TSC 收款地址標示為「永久绑定 · 不可修改」（永久綁定，不可修改），並說明若要更換，請直接在挖礦程式中使用新地址。因此，改用另一個錢包需要它自己的礦池帳戶和礦池密碼（請先在礦池頁面上設定好）。如果你在礦池頁面上變更了礦池密碼，請在每個挖礦程式中都更新它。礦池無法找回忘記的密碼。

**Linux**：先停止挖礦程式，然後用新設定啟動一次；新設定會取代已記住的設定：

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

`-u` 和 `-p` 必須一起提供：只提供其中一個不會變更已記住的錢包和密碼。礦工名稱可以單獨變更：`./thenewminer -w <RIG_NAME> --background`（在 `--stop` 之後）。

**Windows**：執行 `TheNewMiner.bat setup`。它會停止正在執行的挖礦程式，並重新詢問錢包、礦池密碼和礦工名稱。然後照常啟動（按兩下 `TheNewMiner.bat`）。

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

## 參考算力

以下是我們在自己的礦機上、於真實礦池實測到的最佳算力（2026-10-14 至 2026-10-23 期間）。這些是 **最高可達** 的數值，並非保證：同型號的顯示卡會因晶片個體差異、散熱、時脈和功耗上限而相差約 5–8%；如果顯示卡同時連接螢幕，或有其他程式在使用 GPU，算力也會下降。

| 顯示卡 | 算力 |
|---|---|
| RTX 5090 | 最高約 99 N/s |
| RTX 4090 | 最高約 70 N/s |
| RTX 3080 10 GB | 最高約 22 N/s |

## 測試結果

在我們的測試機上於真實礦池進行的測試（每台機器一張顯示卡）。A = 被接受的份額，R = 被拒絕的份額。「穩定挖礦」是指兩次任務切換之間的算力中位數。

| 日期 | 顯示卡 | 作業系統 | 安裝包 | 算力 | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | v0.1.45 正式發行建置版本 | 65.42 N/s（穩定挖礦，16 分鐘；含任務切換為 62.35） | 13 / 0（0 個無效） |
| 2026-10-23 | RTX 5090 | Linux | v0.1.45 正式發行建置版本 | 94.18 N/s（穩定挖礦；含任務切換為 92.24） | 17 / 0（0 個無效） |
| 2026-10-23 | RTX 4090 | Linux | v0.1.44 sealed-r4 | 65.58 N/s（中位數，16 分鐘） | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | v0.1.44 sealed-r4 | 93.45 N/s（穩定挖礦，44 分鐘；含任務切換為 90.39） | 31 / 0（停止時另有 1 個過期份額） |
| 2026-10-23 | RTX 3080 10 GB | Windows（WSL2） | v0.1.44 windows-wsl2-r5 | 約 22 N/s（營運者自行測試） | 不適用 |
| 2026-10-23 | RTX 3080 10 GB | Linux（Ubuntu 26.04） | v0.1.44 sealed-r4 | 21.04 N/s（挖礦 23 分鐘中的最後 10 分鐘） | 3 / 0 |

Linux 上的 RTX 3080（v0.1.44）：安裝 6/6 個步驟全部通過，沒有無效視窗，正常停止。日誌摘錄：[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt)。

## 螢幕截圖

來自我們的測試機，使用的是較早的版本：RTX 4090 上為 v0.1.41，RTX 5090 上為早期建置版本。皆為即時讀數；你的實際數值取決於顯示卡。

**RTX 4090，v0.1.41**：單次讀數 69–75 N/s，大多約 70（10 分鐘平均約 68），9 個份額被接受、0 個被拒絕，約 445 W。

![TheNewMiner v0.1.41 在 RTX 4090 上挖礦](screenshots/rtx4090.png)

**RTX 5090，早期建置版本**：100.83 N/s（已完成視窗的即時平均值），約 575 W。

![早期建置版本的 TheNewMiner 在 RTX 5090 上挖礦](screenshots/rtx5090.png)

## 常見問題

**Windows 顯示「Windows 已保護您的電腦」/「未知的發行者」。**
本安裝包沒有程式碼簽章。請按一下 **其他資訊**，再按一下 **仍要執行**。

**防毒軟體將挖礦程式標記為威脅或刪除。**
Microsoft Defender 和其他防毒軟體經常將加密貨幣挖礦程式標記為「可能不需要的應用程式」，即使是你自行選擇執行的合法程式也一樣，因為惡意軟體常會暗藏挖礦程式。如果檔案被隔離：開啟「Windows 安全性 › 病毒與威脅防護 › 保護歷程記錄」，選取 TheNewMiner 項目並選擇 **允許**（或將解壓縮後的資料夾加入「排除項目」）。請只對從本頁面下載、且 SHA-256 相符的安裝包這樣做。

**第一次啟動要很久。**
第一次啟動會下載約 8.1 GiB 的模型，並在你的顯示卡上執行 GPU 自我檢測；在 Windows 上可能還要設定 WSL2。這只會發生一次。之後啟動會快得多，很快就開始挖礦。

**挖礦程式顯示礦池拒絕了錢包 / 密碼。**
請檢查錢包地址和礦池密碼（以及礦池是否已核准你的帳戶），然後重新啟動：Linux 使用 `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`，Windows 使用 `TheNewMiner.bat setup`。憑證錯誤時，挖礦程式不會自行重試。

**是否正在挖礦？日誌在哪裡？**
Linux：`./thenewminer --status`、`./thenewminer --logs`，或檔案 `thenewminer/.miner/thenewminer.log`。
Windows：`TheNewMiner.bat status` 和 `TheNewMiner.bat logs`。

## 安全性說明

安裝包中的挖礦程式和 GPU 引擎以加密形式（ChaCha20-Poly1305）發佈，挖礦程式啟動時會在你的機器上將其解密。這提高了複製的成本，但 **並非無法破解**：任何擁有執行挖礦程式之機器系統管理員權限的人，只要投入足夠心力，都能取得金鑰或解密後的程式碼。你的錢包地址和礦池密碼儲存在你自己的機器上，並且只會傳送給礦池。
