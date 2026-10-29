[English](../WINDOWS-TROUBLESHOOTING.md) · [简体中文](WINDOWS-TROUBLESHOOTING.zh-CN.md) · **繁體中文** · [Русский](WINDOWS-TROUBLESHOOTING.ru.md) · [Español](WINDOWS-TROUBLESHOOTING.es.md) · [Português (Brasil)](WINDOWS-TROUBLESHOOTING.pt-BR.md) · [Français](WINDOWS-TROUBLESHOOTING.fr.md) · [Deutsch](WINDOWS-TROUBLESHOOTING.de.md) · [Türkçe](WINDOWS-TROUBLESHOOTING.tr.md) · [Tiếng Việt](WINDOWS-TROUBLESHOOTING.vi.md) · [한국어](WINDOWS-TROUBLESHOOTING.ko.md) · [日本語](WINDOWS-TROUBLESHOOTING.ja.md)

> 本翻譯僅為方便閱讀而提供，內容以英文 README 為準。 *(This translation is provided for convenience; the English README is authoritative.)*

# Windows 疑難排解

在 Windows 上，TheNewMiner 會在 WSL2 中執行 Linux 挖礦程式。安裝中途停止時，視窗會說明停在哪個步驟以及該怎麼做，`TheNewMiner.bat` 也會再次顯示這些訊息（同時保存在 `%LOCALAPPDATA%\TheNewMiner\last-setup-failure.txt` 中）。本頁說明常見的情況。安裝和日常指令請見[指南](GUIDE.zh-TW.md)。

## WSL 內的 DNS 無法運作

安裝程式會在第 4 步檢查 WSL 能否解析礦池和模型下載的位址。如果無法解析，安裝程式會在該步驟停止，不會自行變更任何設定。用記事本開啟 `%UserProfile%\.wslconfig`（如果檔案不存在就建立一個），加入下面兩行（在 Windows 11 22H2 或更新版本上，使用 `networkingMode=mirrored`，放在 `[wsl2]` 下，也可以）：

```ini
[wsl2]
dnsTunneling=true
```

然後在命令提示字元中執行 `wsl --shutdown`（它會停止所有 WSL 發行版本，包括 Docker Desktop 的），再按兩下 `TheNewMiner.bat`。

封鎖 WSL 的 VPN 或防火牆也會造成相同的訊息：請允許 WSL，或以 `TheNewMiner.bat setup -Proxy ...` 設定代理伺服器（請見[指南](GUIDE.zh-TW.md)中的「代理伺服器」一節）。

## 無法下載 Ubuntu，或虛擬化未啟用

- **網路**：安裝程式以 `wsl --install` 安裝 Ubuntu 24.04。在該下載無法連線的地方（例如無法連線到 GitHub 的地區），安裝程式會改為從中國的公共鏡像站下載 Ubuntu 官方的 WSL 映像檔，並且只有在其 SHA-256 相符時才會使用。如果所有鏡像站也都沒有回應，請檢查網路連線，稍後再試。
- **虛擬化**：出現與虛擬化有關的訊息（例如 `0x80370102`）表示虛擬化未啟用。請在 BIOS 中啟用 Intel VT-x / AMD SVM，然後再按兩下 `TheNewMiner.bat`。

## 安裝包位於 Windows 磁碟機上

如果你自行解壓縮了 Linux 安裝包，並在 WSL 中啟動 `./thenewminer`，而它位於 Windows 磁碟機上（例如 `/mnt/d/...`），挖礦程式會拒絕啟動：Windows 磁碟機無法保存它的私人設定檔。它會顯示一行可直接貼上的 `mkdir -p ~/thenewminer && cp -a ...` 指令，將資料夾複製到你的 Linux 家目錄並在那裡啟動。也可以改用 Windows zip：`TheNewMiner.bat` 會自動將一切放到正確的位置。

## 10 GB 顯示卡速度慢（瀏覽器、Docker Desktop）

只有在開始挖礦時有約 9.3 GB（9472 MiB）可用顯示記憶體，10 GB 顯示卡才能使用快速配置。在 Windows 上，其他程式會共用顯示卡：開啟硬體加速的瀏覽器或 Docker Desktop 可能佔用足夠多的顯示記憶體，使挖礦程式退回較慢的配置。在 RTX 3080 10 GB 上，這時約為 9 N/s，而不是 21–22 N/s。

1. 關閉瀏覽器，或關閉其硬體加速（Chrome 和 Edge：設定 › 系統 › 「使用圖形加速功能 (如果可用)」；Firefox：設定 › 一般 › 效能），並結束 Docker Desktop。
2. 在命令提示字元中檢查可用的顯示記憶體：`nvidia-smi --query-gpu=memory.used,memory.total --format=csv`（總量減去已使用量應至少為 9472 MiB）。
3. 重新開始挖礦：`TheNewMiner.bat stop`，然後按兩下 `TheNewMiner.bat`。

12 GB 顯示卡同樣需要 9472 MiB 可用顯示記憶體；顯示記憶體 16 GB 以上的顯示卡不受影響。

## 時鐘錯誤

如果挖礦程式的時鐘偏差約 30 秒，礦池會停止該挖礦程式。WSL2 的時鐘可能會漂移，例如在電腦睡眠之後；時鐘有偏差時，安裝程式會發出警告。如果挖礦程式回報時鐘錯誤：

1. 開啟 Windows 設定 › 時間與語言 › 日期和時間，開啟「自動設定時區」和「自動設定時間」，然後按一下 **立即同步**。
2. 在命令提示字元中執行 `wsl --shutdown`。
3. 重新開始挖礦。

## Windows 安全性提示

SmartScreen（「Windows 已保護您的電腦」）以及將挖礦程式標記為威脅的防毒軟體：請見[指南](GUIDE.zh-TW.md)中的常見問題。

## 仍然無法解決？

在 [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) 上開一個 issue，附上 `TheNewMiner.bat status` 的輸出、失敗的步驟，以及 `last-setup-failure.txt` 的內容。切勿張貼你的礦池密碼或錢包私鑰。
