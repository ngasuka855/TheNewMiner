[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · **日本語**

> この翻訳は便宜のために提供されています。正式な内容は英語版 README です。 *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner は **TensorCash** 用の GPU マイナーです。**VRAM 10 GB 以上**の NVIDIA GeForce RTX 30、40、50 シリーズのカードで動作し、プール **`tsc-wss.the-new.win`** でマイニングします。このプールは組み込み済みのため、プールアドレスを入力する必要はなく、直接接続します。

現在のバージョン: **v0.1.44**

## ダウンロード

| ファイル | 対象 | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64 (Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11 (WSL2 経由で動作) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | 両パッケージのチェックサム | |

Windows 用 zip はオールインワンです。Linux 版マイナーがすでに含まれているので、必要なのは zip だけです。

## ダウンロードの検証

パッケージと `SHA256SUMS` を同じフォルダーに置いてから、次を実行します:

```bash
# Linux (パッケージを一つだけダウンロードした場合は --ignore-missing を追加)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows (コマンド プロンプト): 出力を上の表の SHA-256 と比較してください
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

各ファイルで `OK` と表示される必要があります (Windows では、表と同じ 64 文字であること)。一致しない場合は、そのファイルを使用しないでください。

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background
```

| コマンド | 動作 |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background` | 初回起動: 必要なものをインストールし、バックグラウンドでマイニングします |
| `./thenewminer --background` | 二回目以降の起動: 設定は記憶されています |
| `./thenewminer` | フォアグラウンドでマイニング |
| `./thenewminer --logs` | マイナーの状態を表示 (Ctrl+C で表示を終了。マイニングは継続します) |
| `./thenewminer --stop` | マイニングを停止 |

- `-u` はあなたの TensorCash ウォレットアドレス、`-p` はあなたのプールのパスワード、`-w` はプール上に表示されるワーカー名です
  (既定値: マシンのホスト名)。
- 設定は `thenewminer/.miner/` に記憶されます (パスワードを含むファイルはあなただけが読み取れます)。
- 初回起動ではモデル (約 8.1 GiB、一度だけ) をダウンロードし、カードに合った GPU エンジンをインストールして、カード上でセルフチェックを行ってから、マイニングを開始します。`[ RATE] ... N/s` のような行が表示されていれば、マイニング中です。
- 初回起動時に `apt` でいくつかの Ubuntu パッケージをインストールする場合があります (sudo または root が必要)。
- このリリースでは、マシン一台につき NVIDIA GPU はちょうど一枚のみです (マルチ GPU リグはまだサポートされていません)。
- `tar` が `xz: Cannot exec` と表示する場合は、先に xz をインストールしてください: `sudo apt-get install -y xz-utils`。
- パッケージ内の `README.md` は古い内部メモです。このページの手順に従ってください (`--proxy` は不要です)。
- **アップグレード:** `./thenewminer --stop` を実行し、新しいパッケージを同じ場所に展開します (展開先は `thenewminer/` で、設定とモデルが入った `thenewminer/.miner/` はそのまま残ります)。その後、もう一度起動してください。

## Windows (ワンクリック)

1. `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` 全体を展開します (右クリック › すべて展開)。展開先はどのフォルダー、どのドライブでもかまいません。スペースや中国語の文字を含むパスでも問題ありません。zip の中身を表示した画面から直接実行しないでください。
2. **`TheNewMiner.bat`** をダブルクリックします。
   - **初回:** Windows、ドライバー、ディスクを確認し、必要であれば WSL2 を有効にします (管理者の確認画面が一度表示されます。再起動が一度必要になる場合がありますが、その後セットアップは自動で続行されます)。データの保存先を表示し (Enter で確定)、専用の Linux (WSL) 環境を作成し、同梱のマイナーをインストールして、ウォレットアドレス (`tc1` で始まる)、プールのパスワード (二回入力。6–128 文字の英字、数字、`_` または `-`)、ワーカー名
     (Enter = コンピューター名) を**一度だけ**尋ねます。その後モデルをダウンロードし、マイニングを開始します。
   - **二回目以降:** ダブルクリックするだけで、保存された設定ですぐにマイニングを開始します。デスクトップにショートカット「TheNewMiner」も作成されます。

日常的に使うコマンド (展開したフォルダーでコマンド プロンプトを開きます):

| コマンド | 動作 |
|---|---|
| `TheNewMiner.bat` | 必要ならインストールし、このウィンドウでマイニング (ウィンドウを閉じるとマイニングが停止します) |
| `TheNewMiner.bat background` | ウィンドウなしでマイニング |
| `TheNewMiner.bat logs` | マイナーの状態を表示 (Ctrl+C で表示を終了。マイニングは継続します) |
| `TheNewMiner.bat status` | 動作中かどうかと、直近のハッシュレート |
| `TheNewMiner.bat stop` | マイニングを停止 |
| `TheNewMiner.bat setup` | ウォレット / パスワード / ワーカー名を入力し直す |
| `TheNewMiner.bat uninstall` | マイナー、そのデータフォルダー、ショートカットを削除 (事前に確認します) |

- **データの保存先:** マイナーとモデルは、初回インストール時に選んだデータフォルダーに置かれます (既定では、展開先ドライブのルートにある `TheNewMinerData`。例: `D:\TheNewMinerData`)。展開したフォルダーには置かれません。そこに約 20 GB の空き容量が必要です。
- **アップグレード:** 新しいバージョンを任意の場所に展開し、その `TheNewMiner.bat` をダブルクリックします。インストール済みのマイナーを見つけ、設定とモデルを残したまま、何も尋ねずに新しいバージョンをインストールします。古いフォルダーは削除してかまいません。
- **アンインストール:** `TheNewMiner.bat uninstall`。
- 画面の表示言語は Windows の言語に従います (English / 简体中文 / 繁體中文)。言語を選ぶには、任意のコマンドに `-Lang en` (または `zh-Hans`、
  `zh-Hant`) を追加します。例: `TheNewMiner.bat -Lang en`。その実行にのみ適用されます。

## プロキシ (任意)

マイナーはプールに直接接続します。ネットワークでプロキシが必要な場合:

```bash
# Linux (変更するまで記憶されます)
./thenewminer --proxy <IP>[:<PORT>]            # TCP リレー (省略時はポート 443)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5、socks5://<USER>:<PASS>@<HOST>:<PORT> も可
./thenewminer --proxy http://<HOST>:<PORT>     # HTTP CONNECT プロキシ
./thenewminer --proxy none                     # 直接接続に戻す
```

これらのコマンドはマイナーの起動も行います。先にマイナーを停止し、バックグラウンドで動かし続けるには `--background` を追加してください。

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

TLS は常にプールとの間でエンドツーエンドで動作するため、プロキシが通信内容を読み取ることはできません。Windows では、同じ PC で動作しているプロキシアプリには、通常 WSL から `127.0.0.1` として到達できません (WSL の既定のネットワーク設定の場合)。PC の LAN IP を使い、アプリで LAN からの接続を許可してください。

## 動作要件

| 項目 | 要件 |
|---|---|
| GPU | NVIDIA GeForce RTX 30 / 40 / 50 シリーズのカードちょうど一枚、VRAM 10 GB 以上 |
| ドライバー | NVIDIA **580 以降** (`nvidia-smi` が動作すること)。Windows では Windows 側にインストールし、WSL の中には決してインストールしないでください |
| Linux | x86_64 Ubuntu 22.04、24.04 または 26.04 (その他のディストリビューションはまだサポートされていません) |
| Windows | Windows 10 21H2 以降、または Windows 11 (64 ビット)。BIOS で仮想化 (Intel VT-x / AMD SVM) を有効にすること |
| ディスク | 約 20 GB の空き容量 |
| メモリ | 4 GB 以上 |
| モデル | 約 8.1 GiB。初回起動時に一度だけダウンロードし (並列、複数ミラー)、以降は再利用します |

## 参考ハッシュレート

自社のリグで測定した、実際のプールでの最高ハッシュレートです (2026-10-14 から 2026-10-23 の間)。これらは**最大**値であり、保証ではありません。同じモデルのカードでも、個々のチップ、冷却、クロック、電力制限によって約 5–8% の差があり、カードでディスプレイも出力している場合や、他のプログラムが GPU を使用している場合はハッシュレートが下がります。

| GPU | ハッシュレート |
|---|---|
| RTX 5090 | 最大 ~99 N/s |
| RTX 4090 | 最大 ~70 N/s |
| RTX 3080 10 GB | 最大 ~22 N/s |

## テスト結果

2026-10-23 に自社のテストリグで行った v0.1.44 の実プールテストです (マシン一台につき GPU 一枚)。A = 承認されたシェア、R = 拒否されたシェア。

| 日付 | カード | OS | パッケージ | ハッシュレート | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | sealed-r4 | 65.58 N/s (中央値、16 分) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | sealed-r4 | 93.45 N/s (安定マイニング時、44 分。ジョブ切り替えを含めると 90.39) | 31 / 0 (停止時に stale が +1) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | windows-wsl2-r5 | ~22 N/s (運営者自身による実行) | なし |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | sealed-r4 | 21.04 N/s (23 分間のマイニングのうち最後の 10 分) | 3 / 0 |

Linux 上の RTX 3080: インストールは 6/6 ステップ成功、無効なウィンドウなし、正常に停止。ログの抜粋:
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt)。

## スクリーンショット

自社のテストリグで、以前のバージョンを使って撮影したものです: RTX 4090 では v0.1.41、RTX 5090 では初期ビルド。これらはリアルタイムの表示値です。実際の数値はカードによって異なります。

**RTX 4090、v0.1.41:** 表示値ごとに 69–75 N/s、ほとんどが ~70 (10 分平均 ~68)、シェア 9 件承認、0 件拒否、~445 W。

![RTX 4090 でマイニング中の TheNewMiner v0.1.41](screenshots/rtx4090.png)

**RTX 5090、初期ビルド:** 100.83 N/s (完了したウィンドウのリアルタイム平均)、~575 W。

![RTX 5090 でマイニング中の TheNewMiner 初期ビルド](screenshots/rtx5090.png)

## よくある質問

**Windows で「Windows によって PC が保護されました」/「不明な発行元」と表示される。**
このキットはコード署名されていません。**詳細情報**をクリックしてから、**実行**をクリックしてください。

**ウイルス対策ソフトがマイナーを検出する、または削除する。**
Microsoft Defender などのウイルス対策ソフトは、マルウェアがマイナーを隠し持っていることが多いため、自分で実行を選んだ正規のものであっても、暗号通貨マイナーを「望ましくない可能性のあるアプリケーション」として検出することがよくあります。ファイルが検疫された場合は、
Windows セキュリティ › ウイルスと脅威の防止 › 保護の履歴 を開き、TheNewMiner の項目を選んで
**許可**を選択してください (または、展開したフォルダーを除外に追加してください)。これは、このページからダウンロードし、
SHA-256 が一致したパッケージに対してのみ行ってください。

**初回起動に時間がかかる。**
初回起動では約 8.1 GiB のモデルをダウンロードし、カード上で GPU セルフチェックを行います。Windows では WSL2 のセットアップも行う場合があります。これは一度だけです。二回目以降は、はるかに早くマイニングが始まります。

**ログはどこにありますか?**
Linux: `./thenewminer --logs`、またはファイル `thenewminer/.miner/thenewminer.log`。
Windows: `TheNewMiner.bat logs` (直近のハッシュレートは `TheNewMiner.bat status`)。

**HiveOS は?**
HiveOS 用パッケージは**近日公開予定**です。現在はまだテスト中です。

## セキュリティに関する注意

マイナーと GPU エンジンは、パッケージ内で暗号化された状態 (ChaCha20-Poly1305) で配布されます。マイナーは起動時に、あなたのマシン上でそれらを復号します。これによりコピーのコストは上がりますが、**破られないわけではありません**: マイナーを実行しているマシンの管理者権限を持つ人なら誰でも、十分な労力をかければ、鍵または復号されたコードを取り出すことができます。ウォレットアドレスとプールのパスワードはあなた自身のマシンに保存され、プールにのみ送信されます。
