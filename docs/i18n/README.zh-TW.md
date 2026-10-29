# TheNewMiner

TheNewMiner 是 **TensorCash** 的 GPU 挖礦程式，支援顯示記憶體 10 GB 以上的 NVIDIA GeForce RTX 30、40 和 50 系列顯示卡；礦池 **`tsc-wss.the-new.win`** 已內建。

[English](../../README.md) · [简体中文](README.zh-CN.md) · **繁體中文** · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> 本翻譯僅為方便閱讀而提供，內容以英文 README 為準。 *(This translation is provided for convenience; the English README is authoritative.)*

## 下載

目前版本：**v0.1.47** · 礦池網站：[tsc.the-new.win](https://tsc.the-new.win/#/home)

| 系統 | 下載 | 接著 |
|---|---|---|
| Linux x86_64 | [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | 或使用下方的一鍵安裝 |
| Windows 10 / 11 | [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | 解壓縮後按兩下 `TheNewMiner.bat` |
| HiveOS（實驗性） | [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) | 匯入 flight sheet（請見下方 HiveOS 一節） |

檢查碼：[`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS)。驗證方法請見[指南](GUIDE.zh-TW.md)。如果檔案不一致，請勿使用。

## 開始之前：建立礦池帳戶

挖礦需要一個礦池帳戶：你的 TSC 收款地址（錢包），以及你在礦池網站上設定的礦池密碼。網站提供英文版（可透過網站的語言選單切換）。

1. 開啟 [tsc.the-new.win](https://tsc.the-new.win/#/home)，進入 **Start mining**（开始挖矿）。
2. 在 **1. Bind Wallet**（绑定钱包）下，輸入你的 **TSC payout address**（TSC 收款地址，即 `tc1...`；請仔細核對：挖礦收入一旦發送到錯誤的地址，將無法追回），設定一組 **Pool Password**（矿池密码，6–128 個字元），並在 **Confirm Password**（确认密码）下再次輸入。
3. 點擊 **Apply to join pool**（申请加入矿池）。頁面會確認錢包已綁定；帳戶立即可用。
4. 在挖礦程式中使用相同的錢包和密碼：Linux 上使用 `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`，Windows 上在 `TheNewMiner.bat` 的帳戶視窗中填寫，HiveOS 上則填在 flight sheet 中。開始挖礦後約 3–5 分鐘，你的礦機會出現在礦池網站的 **My Miners**（我的礦工）中。

礦池無法找回遺忘的礦池密碼，收款地址日後也無法變更。礦池密碼不是你錢包的私鑰：切勿將私鑰交給任何人。

## 一鍵安裝（Linux）

將這行指令末尾的 `<YOUR_WALLET>`、`<YOUR_POOL_PASSWORD>` 和 `<RIG_NAME>`（連同 `<` `>` 一起）分別替換成你的錢包、你的礦池密碼和這台礦機的名稱，再貼到終端機中執行。它會下載 Linux 安裝包和 `SHA256SUMS`，驗證 SHA-256，將挖礦程式解壓縮到 `~/thenewminer`，並在背景啟動：

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.47 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

下載出錯或 SHA-256 不一致時，不會啟動任何程式。請保留密碼兩側的單引號（密碼中的 `'` 要寫成 `'\''`）。在單卡機器上，挖礦程式會在下載模型（約 8.1 GiB，只下載一次）之前，先向礦池核對錢包和密碼。之後 `./thenewminer --logs` 會顯示挖礦情形，`./thenewminer --status` 會說明它是否正在執行，或者為什麼停止。若要升級，請先執行 `~/thenewminer/thenewminer --stop`，再執行同一行指令：你的設定和模型會保留。其他所有內容（日常指令、升級、開機自動啟動、沒有在挖礦時該怎麼辦）都在[指南](GUIDE.zh-TW.md)中。

## Windows

1. 將整個 `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` 解壓縮（按右鍵 › 全部解壓縮）到任何位置。
2. 按兩下 **`TheNewMiner.bat`**。第一次執行時，會有 **一個** 視窗詢問你的礦池帳戶（錢包、礦池密碼、礦工名稱）；之後不會再詢問任何事：它會設定 WSL2 和 Ubuntu，安裝內附的挖礦程式，下載模型，然後開始挖礦。之後再按兩下，就會立即開始挖礦。Windows 可能會先顯示「Windows 已保護您的電腦」（本安裝包沒有程式碼簽章）：請按一下 **其他資訊**，再按一下 **仍要執行**。開啟 WSL 時會要求一次系統管理員權限，並可能需要重新開機一次；之後安裝會自動繼續。
3. 如果找到較早的版本，會在原位置升級，並保留你的設定和模型。

如果它停止了或挖礦很慢，請參閱 [Windows 疑難排解](WINDOWS-TROUBLESHOOTING.zh-TW.md)（DNS、Windows 磁碟機、瀏覽器和 Docker Desktop 佔用的顯示記憶體、時鐘）。

## HiveOS（實驗性）

1. **Wallets**（錢包）：新增一個錢包，幣種（coin）選 `TSC`，並填入你的 TensorCash 地址（`tc1...`）。
2. 開啟 [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json)，複製其內容，替換 `<YOUR_WALLET>`（保留其後的 `.%WORKER_NAME%`）和 `<YOUR_POOL_PASSWORD>`。在 HiveOS 中選擇 Flight Sheets › **Import from clipboard**（從剪貼簿匯入），貼上內容，選擇你的 `TSC` 錢包，然後儲存。
3. 將 flight sheet 套用到你的礦機。它會安裝 `thenewminer-0.1.47.tar.gz` 並開始挖礦。

手動設定、升級，以及無法連線到 GitHub 的礦機：請見[指南](GUIDE.zh-TW.md)。

## v0.1.47 新功能

- **安裝更簡單**：在單卡機器上，挖礦程式會在下載 8.1 GiB 模型 **之前** 先向礦池核對你的錢包和礦池密碼，輸入錯誤幾秒內就能發現。有打字錯誤的錢包地址（檢查碼不符）或範例地址會被拒絕，並說明錯在哪裡。
- **Windows：憑證只詢問一次**，在開始時的一個視窗中詢問，在任何耗時較長的步驟之前。由較早版本完成的安裝（包括位於其他 WSL 發行版本中的安裝）會被找到並 **在原位置升級**（保留設定和模型）。如果無法從 Microsoft 下載 Ubuntu，安裝程式會從中國的鏡像站取得 Ubuntu 官方的 WSL 映像檔，並且只有在其 SHA-256 相符時才會使用。
- **標準 Ubuntu 上不需要 apt**：Ubuntu 22.04 / 24.04（包括 WSL 中的）已具備挖礦程式需要的一切，因此不會安裝任何東西。若有缺少，第一次啟動會先列出缺少的項目，再進行安裝。
- **位於 Windows 磁碟機上的安裝包**（例如 WSL 中的 `/mnt/d`）會立即被拒絕，並提供一行可直接貼上的指令，將它複製到你的 Linux 家目錄並在那裡啟動。
- **更清楚的網路訊息**：當 WSL 內的 DNS 無法運作時，安裝程式會在該步驟停止並提供解決方法；無法連線到網際網路或礦池的挖礦程式會說明需要檢查什麼。
- **模型可重複使用**：升級到新資料夾時，會找到先前安裝的模型（大小和 SHA-256 都相同），不會再次下載。
- **`--status` 會說明原因**：挖礦程式為什麼停止以及如何解決，以 `--background` 啟動後也一樣。
- **HiveOS**：每次失敗後都會等待並重試，間隔逐漸拉長（一到十分鐘），並在日誌和儀表板上顯示原因。

完整說明：[`docs/releases/RELEASE-NOTES-v0.1.47.md`](../releases/RELEASE-NOTES-v0.1.47.md)。

## 已知問題

- **Windows 上的 10 GB 顯示卡需要約 9.3 GB 可用顯示記憶體才能全速運作。** 不足時，挖礦程式會使用較慢的配置（一張 RTX 3080 10 GB 從 21–22 降到約 9 N/s）。開啟硬體加速的瀏覽器或 Docker Desktop 可能會佔用這部分顯示記憶體：請關閉它們（或關閉瀏覽器的硬體加速），然後重新開始挖礦。
- **Windows 時鐘**：如果挖礦程式回報時鐘錯誤，請開啟 Windows 設定 › 時間與語言 › 日期和時間，開啟「自動設定時區」和「自動設定時間」，按一下 **立即同步**，然後執行 `wsl --shutdown` 再重新啟動。

## 算力

以下是我們在自己的礦機上、於真實礦池實測到的最佳算力。這些是 **最高可達** 的數值，並非保證：同型號的顯示卡之間會相差約 5–8%；如果顯示卡同時連接螢幕，或有其他程式在使用 GPU，算力也會下降。

| 顯示卡 | 算力 |
|---|---|
| RTX 5090 | 最高約 99 N/s |
| RTX 4090 | 最高約 70 N/s |
| RTX 3080 10 GB | 最高約 22 N/s |

各版本附日期的實測數據：[測試結果](../TEST-RESULTS.md)。專案的里程碑：[開發紀錄](../DEVELOPMENT-RECORD.md)。

## 回報問題

在 [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) 上開一個 issue，並填寫錯誤回報表單：TheNewMiner 版本、你的系統、GPU 型號和驅動程式、`./thenewminer --status` 的輸出，以及最後幾行日誌。Issue 是公開的：**切勿張貼你的礦池密碼或錢包私鑰**；貼上任何內容前，請刪除其中的密碼（如果你願意，也可以刪除錢包地址）。

## 安全性說明

安裝包中的挖礦程式和 GPU 引擎以加密形式（ChaCha20-Poly1305）發佈，挖礦程式啟動時會在你的機器上將其解密。這提高了複製的成本，但 **並非無法破解**：任何擁有執行挖礦程式之機器系統管理員權限的人，只要投入足夠心力，都能取得金鑰或解密後的程式碼。你的錢包地址和礦池密碼儲存在你自己的機器上，並且只會傳送給礦池。
