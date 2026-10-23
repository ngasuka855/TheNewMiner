[English](README.md) · [简体中文](README.zh-CN.md) · **繁體中文** · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> 本翻譯僅為方便閱讀而提供,內容以英文 README 為準。 *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner 是 **TensorCash** 的 GPU 挖礦程式,支援 **顯示記憶體 10 GB 以上** 的 NVIDIA GeForce RTX 30、40 和 50 系列顯示卡,
在礦池 **`tsc-wss.the-new.win`** 上挖礦。礦池位址已內建於程式中:不需要輸入礦池位址,會直接連線。

目前版本:**v0.1.44**

## 下載

| 檔案 | 適用於 | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64(Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11(透過 WSL2 執行) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | 兩個安裝包的檢查碼 | |

Windows 的 zip 是一體式安裝包:已經包含 Linux 挖礦程式,只需要下載這一個 zip。

## 驗證下載的檔案

將安裝包和 `SHA256SUMS` 放在同一個資料夾,然後執行:

```bash
# Linux(如果只下載了其中一個安裝包,請加上 --ignore-missing)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows(命令提示字元):將輸出結果與上表中的 SHA-256 比對
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

每個檔案都必須顯示 `OK`(在 Windows 上則是與上表完全相同的 64 個字元)。如果不一致,請勿使用。

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background
```

| 指令 | 用途 |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background` | 第一次啟動:安裝所需元件,然後在背景挖礦 |
| `./thenewminer --background` | 之後啟動:設定已被記住,不必再輸入 |
| `./thenewminer` | 在前景挖礦 |
| `./thenewminer --logs` | 查看挖礦程式運作情形(按 Ctrl+C 離開檢視,挖礦會繼續) |
| `./thenewminer --stop` | 停止挖礦 |

- `-u` 是你的 TensorCash 錢包地址,`-p` 是你的礦池密碼,`-w` 是在礦池上顯示的礦工名稱
  (預設:本機的主機名稱)。
- 設定會被記住,儲存在 `thenewminer/.miner/` 中(存放密碼的檔案只有你本人可以讀取)。
- 第一次啟動會下載模型(約 8.1 GiB,只下載一次)、安裝適合你顯示卡的 GPU 引擎,並在顯示卡上執行自我檢測,然後開始挖礦。看到 `[ RATE] ... N/s` 這類訊息,就表示正在挖礦。
- 第一次啟動可能會用 `apt` 安裝幾個 Ubuntu 套件(需要 sudo 或 root 權限)。
- 此版本每台機器只支援恰好一張 NVIDIA 顯示卡(尚不支援多卡礦機)。
- 如果 `tar` 顯示 `xz: Cannot exec`,請先安裝 xz:`sudo apt-get install -y xz-utils`。
- 安裝包內的 `README.md` 是較舊的內部說明,請以本頁為準(不需要 `--proxy`)。
- **升級:** 先執行 `./thenewminer --stop`,在同一個位置解壓縮新的安裝包(會解壓縮到 `thenewminer/`,並保留
  `thenewminer/.miner/` 中的設定和模型),然後重新啟動。

## Windows(一鍵使用)

1. 將整個 `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` 解壓縮(按右鍵 › 全部解壓縮)到任何位置:任何資料夾、任何磁碟機皆可,
   路徑含有空格或中文字也沒問題。請勿在 zip 的預覽視窗中直接執行。
2. 按兩下 **`TheNewMiner.bat`**。
   - **第一次:** 檢查 Windows、驅動程式和磁碟;必要時開啟 WSL2(會跳出一次系統管理員確認;可能需要重新開機一次,之後安裝會自動繼續);
     顯示資料的存放位置(按 Enter 確認);建立專用的 Linux(WSL)環境,安裝內附的挖礦程式;**只詢問一次** 你的錢包地址
     (以 `tc1` 開頭)、礦池密碼(輸入兩次;6–128 個英文字母、數字、`_` 或 `-`)和礦工名稱(直接按 Enter = 電腦名稱);
     下載模型,然後開始挖礦。
   - **之後每一次:** 按兩下即會以已儲存的設定立即開始挖礦。桌面上也會建立一個「TheNewMiner」捷徑。

日常指令(在解壓縮後的資料夾中開啟命令提示字元):

| 指令 | 用途 |
|---|---|
| `TheNewMiner.bat` | 必要時先安裝,然後在此視窗中挖礦(關閉視窗即停止挖礦) |
| `TheNewMiner.bat background` | 不開視窗挖礦 |
| `TheNewMiner.bat logs` | 查看挖礦程式運作情形(按 Ctrl+C 離開檢視,挖礦會繼續) |
| `TheNewMiner.bat status` | 是否正在執行,以及最近的算力 |
| `TheNewMiner.bat stop` | 停止挖礦 |
| `TheNewMiner.bat setup` | 重新輸入錢包 / 密碼 / 礦工名稱 |
| `TheNewMiner.bat uninstall` | 移除挖礦程式、其資料資料夾和捷徑(會先詢問) |

- **資料存放位置:** 挖礦程式和模型存放在第一次安裝時選定的資料資料夾中(預設為解壓縮所在磁碟機根目錄下的 `TheNewMinerData`,
  例如 `D:\TheNewMinerData`),而不是在解壓縮後的資料夾裡。該處需要約 20 GB 的可用空間。
- **升級:** 將新版本解壓縮到任何位置,按兩下其中的 `TheNewMiner.bat`。它會找到已安裝的挖礦程式,保留你的設定和模型,
  直接安裝新版本,不會再詢問。舊資料夾可以刪除。
- **解除安裝:** `TheNewMiner.bat uninstall`。
- 介面語言會依照 Windows 的語言(English / 简体中文 / 繁體中文)。若要自行指定,在任何指令後面加上 `-Lang en`(或 `zh-Hans`、
  `zh-Hant`),例如 `TheNewMiner.bat -Lang en`;僅對該次執行有效。

## 代理伺服器(選用)

挖礦程式會直接連線到礦池。如果你的網路需要使用代理伺服器:

```bash
# Linux(會一直記住,直到你變更為止)
./thenewminer --proxy <IP>[:<PORT>]            # TCP 中繼(省略連接埠時為 443)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5,也支援 socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # HTTP CONNECT 代理伺服器
./thenewminer --proxy none                     # 恢復直接連線
```

這些指令同時也會啟動挖礦程式:請先停止挖礦程式;加上 `--background` 可讓它在背景執行。

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

TLS 一律與礦池進行端對端加密,因此代理伺服器無法讀取連線內容。在 Windows 上,於同一台電腦執行的代理程式,通常無法從 WSL 以
`127.0.0.1` 連線(WSL 預設網路模式):請改用電腦的區域網路 IP,並在代理程式中允許區域網路連線。

## 系統需求

| 項目 | 需求 |
|---|---|
| 顯示卡 | 恰好一張 NVIDIA GeForce RTX 30 / 40 / 50 系列顯示卡,顯示記憶體 10 GB 以上 |
| 驅動程式 | NVIDIA **580 或更新版本**(`nvidia-smi` 必須能正常執行)。在 Windows 上請安裝於 Windows 中,切勿安裝在 WSL 內 |
| Linux | x86_64 Ubuntu 22.04、24.04 或 26.04(尚不支援其他發行版) |
| Windows | Windows 10 21H2 或更新版本,或 Windows 11，64 位元;已在 BIOS 中啟用虛擬化(Intel VT-x / AMD SVM) |
| 磁碟 | 約 20 GB 可用空間 |
| 記憶體 | 4 GB 以上 |
| 模型 | 約 8.1 GiB,第一次啟動時下載一次(多鏡像平行下載),之後重複使用 |

## 參考算力

以下是我們在自己的礦機上、於真實礦池實測到的最佳算力(2026-10-14 至 2026-10-23 期間)。這些是 **最高可達** 的數值,
並非保證:同型號的顯示卡會因晶片個體差異、散熱、時脈和功耗上限而相差約 5–8%;如果顯示卡同時連接螢幕,或有其他程式在使用 GPU,
算力也會下降。

| 顯示卡 | 算力 |
|---|---|
| RTX 5090 | 最高約 99 N/s |
| RTX 4090 | 最高約 70 N/s |
| RTX 3080 10 GB | 最高約 22 N/s |

## 測試結果

2026-10-23 在我們的測試機上以 v0.1.44 於真實礦池進行的測試(每台機器一張顯示卡)。A = 被接受的份額,R = 被拒絕的份額。

| 日期 | 顯示卡 | 作業系統 | 安裝包 | 算力 | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | sealed-r4 | 65.58 N/s(中位數,16 分鐘) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | sealed-r4 | 93.45 N/s(穩定挖礦,44 分鐘;含任務切換為 90.39) | 31 / 0(停止時另有 1 個過期份額) |
| 2026-10-23 | RTX 3080 10 GB | Windows(WSL2) | windows-wsl2-r5 | 約 22 N/s(營運者自行測試) | 不適用 |
| 2026-10-23 | RTX 3080 10 GB | Linux(Ubuntu 26.04) | sealed-r4 | 21.04 N/s(挖礦 23 分鐘中的最後 10 分鐘) | 3 / 0 |

RTX 3080 Linux:安裝 6/6 個步驟全部通過,沒有無效視窗,正常停止。日誌摘錄:
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt)。

## 螢幕截圖

來自我們的測試機,使用的是較早的版本:RTX 4090 上為 v0.1.41,RTX 5090 上為早期建置版本。皆為即時讀數;你的實際數值取決於顯示卡。

**RTX 4090,v0.1.41:** 單次讀數 69–75 N/s,大多約 70(10 分鐘平均約 68),9 個份額被接受、0 個被拒絕,約 445 W。

![TheNewMiner v0.1.41 在 RTX 4090 上挖礦](screenshots/rtx4090.png)

**RTX 5090,早期建置版本:** 100.83 N/s(已完成視窗的即時平均值),約 575 W。

![早期建置版本的 TheNewMiner 在 RTX 5090 上挖礦](screenshots/rtx5090.png)

## 常見問題

**Windows 顯示「Windows 已保護您的電腦」/「未知的發行者」。**
本安裝包沒有程式碼簽章。請按一下 **其他資訊**,再按一下 **仍要執行**。

**防毒軟體將挖礦程式標記為威脅或刪除。**
Microsoft Defender 和其他防毒軟體經常將加密貨幣挖礦程式標記為「可能不需要的應用程式」,即使是你自行選擇執行的合法程式也一樣,
因為惡意軟體常會暗藏挖礦程式。如果檔案被隔離:開啟「Windows 安全性 › 病毒與威脅防護 › 保護歷程記錄」,選取 TheNewMiner
項目並選擇 **允許**(或將解壓縮後的資料夾加入「排除項目」)。請只對從本頁面下載、且 SHA-256 相符的安裝包這樣做。

**第一次啟動要很久。**
第一次啟動會下載約 8.1 GiB 的模型,並在你的顯示卡上執行 GPU 自我檢測;在 Windows 上可能還要設定 WSL2。這只會發生一次。之後啟動會快得多,很快就開始挖礦。

**日誌在哪裡?**
Linux:`./thenewminer --logs`,或檔案 `thenewminer/.miner/thenewminer.log`。
Windows:`TheNewMiner.bat logs`(`TheNewMiner.bat status` 可查看最近的算力)。

**HiveOS?**
HiveOS 安裝包 **即將推出**,目前仍在測試中。

## 安全性說明

安裝包中的挖礦程式和 GPU 引擎以加密形式(ChaCha20-Poly1305)發佈,挖礦程式啟動時會在你的機器上將其解密。這提高了複製的成本,
但 **並非無法破解**:任何擁有執行挖礦程式之機器系統管理員權限的人,只要投入足夠心力,都能取得金鑰或解密後的程式碼。你的錢包地址和礦池密碼儲存在你自己的機器上,並且只會傳送給礦池。
