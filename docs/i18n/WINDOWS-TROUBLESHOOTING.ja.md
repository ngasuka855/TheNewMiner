[English](../WINDOWS-TROUBLESHOOTING.md) · [简体中文](WINDOWS-TROUBLESHOOTING.zh-CN.md) · [繁體中文](WINDOWS-TROUBLESHOOTING.zh-TW.md) · [Русский](WINDOWS-TROUBLESHOOTING.ru.md) · [Español](WINDOWS-TROUBLESHOOTING.es.md) · [Português (Brasil)](WINDOWS-TROUBLESHOOTING.pt-BR.md) · [Français](WINDOWS-TROUBLESHOOTING.fr.md) · [Deutsch](WINDOWS-TROUBLESHOOTING.de.md) · [Türkçe](WINDOWS-TROUBLESHOOTING.tr.md) · [Tiếng Việt](WINDOWS-TROUBLESHOOTING.vi.md) · [한국어](WINDOWS-TROUBLESHOOTING.ko.md) · **日本語**

> この翻訳は便宜のために提供されています。正式な内容は英語版 README です。 *(This translation is provided for convenience; the English README is authoritative.)*

# Windows のトラブルシューティング

Windows 版の TheNewMiner は、WSL2 の中で Linux 版マイナーを動かします。セットアップが止まった場合、どの段階で止まったか、何をすべきかがウィンドウに表示され、`TheNewMiner.bat` を実行すると再び表示されます (`%LOCALAPPDATA%\TheNewMiner\last-setup-failure.txt` にも保存されます)。このページでは、よくあるケースを説明します。インストールと日常的に使うコマンドは[ガイド](GUIDE.ja.md)にあります。

## WSL の中で DNS が機能しない

セットアップはステップ 4 で、WSL がプールとモデルのダウンロード先の名前を解決できるかを確認します。解決できない場合、セットアップはそこで停止し、自分では何も変更しません。`%UserProfile%\.wslconfig` をメモ帳で開き (ファイルがない場合は作成します)、次の二行を追加してください (Windows 11 22H2 以降では、`networkingMode=mirrored` を `[wsl2]` の下に書いても機能します):

```ini
[wsl2]
dnsTunneling=true
```

その後、コマンド プロンプトで `wsl --shutdown` を実行し (すべての WSL ディストリビューションが停止します。Docker Desktop のものも含みます)、もう一度 `TheNewMiner.bat` をダブルクリックしてください。

WSL をブロックする VPN やファイアウォールが原因で、同じメッセージが表示されることもあります。WSL を許可するか、`TheNewMiner.bat setup -Proxy ...` でプロキシを設定してください ([ガイド](GUIDE.ja.md)の「プロキシ」を参照)。

## Ubuntu をダウンロードできない、または仮想化がオフになっている

- **ネットワーク:** セットアップは `wsl --install` で Ubuntu 24.04 をインストールします。そのダウンロードで接続できない場合 (例えば GitHub に接続できない地域)、セットアップは代わりに中国の公開ミラーから Ubuntu 公式の WSL イメージをダウンロードし、その SHA-256 が一致した場合にのみ使用します。どのミラーも応答しない場合は、インターネット接続を確認し、しばらくしてからもう一度お試しください。
- **仮想化:** 仮想化に関するメッセージ (例えば `0x80370102`) は、仮想化がオフになっていることを意味します。BIOS で Intel VT-x / AMD SVM をオンにしてから、もう一度 `TheNewMiner.bat` をダブルクリックしてください。

## パッケージが Windows のドライブ上にある

Linux パッケージを自分で展開し、WSL の中で `./thenewminer` を Windows のドライブ上 (`/mnt/d/...` など) から起動した場合、マイナーは起動を拒否します。Windows のドライブでは、マイナーの非公開の設定ファイルを保持できないためです。マイナーは、フォルダーを Linux のホームフォルダーにコピーしてそこで起動する、そのまま貼り付けられる `mkdir -p ~/thenewminer && cp -a ...` の行を表示します。または、代わりに Windows 用 zip を使ってください。`TheNewMiner.bat` がすべてを自動で正しい場所に置きます。

## 10 GB のカードで遅い (ブラウザー、Docker Desktop)

10 GB のカードが高速なレイアウトを使えるのは、マイニング開始時に約 9.3 GB (9472 MiB) の VRAM が空いている場合だけです。Windows では、ほかのプログラムがカードを共有しています。ハードウェアアクセラレーションを有効にしたブラウザーや Docker Desktop が VRAM を多く使うと、マイナーはより遅いレイアウトに切り替わります。RTX 3080 10 GB では、21–22 N/s ではなく約 9 N/s になりました。

1. ブラウザーを閉じるか、ハードウェアアクセラレーションをオフにし (Chrome と Edge: 設定 › システム › 「グラフィック アクセラレーションが使用可能な場合は使用する」、Firefox: 設定 › 一般 › パフォーマンス)、Docker Desktop を終了します。
2. コマンド プロンプトで空き VRAM を確認します: `nvidia-smi --query-gpu=memory.used,memory.total --format=csv` (total から used を引いた値が 9472 MiB 以上である必要があります)。
3. マイニングを再開します: `TheNewMiner.bat stop` を実行してから、`TheNewMiner.bat` をダブルクリックします。

12 GB のカードにも同じく 9472 MiB の空きが必要です。16 GB 以上のカードは影響を受けません。

## 時計のエラー

時計が約 30 秒ずれているマイナーは、プールによって停止されます。WSL2 の時計は、例えば PC がスリープした後にずれることがあり、ずれているとセットアップが警告します。マイナーが時計のエラーを報告した場合:

1. Windows の 設定 › 時刻と言語 › 日付と時刻 を開き、「タイム ゾーンを自動的に設定する」と「時刻を自動的に設定する」をオンにしてから、**今すぐ同期** をクリックします。
2. コマンド プロンプトで `wsl --shutdown` を実行します。
3. もう一度マイニングを開始します。

## Windows のセキュリティに関する警告

SmartScreen (「Windows によって PC が保護されました」) と、マイナーを検出するウイルス対策ソフトについては、[ガイド](GUIDE.ja.md)のよくある質問を参照してください。

## それでも解決しない場合

[GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) で issue を作成し、`TheNewMiner.bat status` の出力、失敗したステップ、`last-setup-failure.txt` の内容を記入してください。プールパスワードやウォレットの秘密鍵は決して投稿しないでください。
