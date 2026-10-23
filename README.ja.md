[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · **日本語**

> この翻訳は便宜のために提供されています。正式な内容は英語版 README です。 *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner は **TensorCash** 用の GPU マイナーです。**VRAM 10 GB 以上**の NVIDIA GeForce RTX 30、40、50 シリーズのカードで動作し、プール **`tsc-wss.the-new.win`** でマイニングします。このプールは組み込み済みのため、プールアドレスを入力する必要はなく、直接接続します。

現在のバージョン: **v0.1.45**

## v0.1.45 の新機能

- **プールがウォレットまたはパスワードを拒否したときに、明確なメッセージを表示します。** マイナーはそのことを一行で伝え、再試行せずに停止します。`--status` で理由を確認できます。ウォレットとパスワードを確認してから、`-u` と `-p` を付けて再度起動してください。
- **ウォレットとパスワードが `ps` に表示されなくなりました**。他のユーザーのプロセス一覧にも表示されません。
- **`./thenewminer --status`** で、マイニング中かどうか、いつから動作しているか、直近のハッシュレート、承認 / 拒否されたシェアを確認できます。
- **HiveOS 対応 (試験的):** HiveOS 用のカスタムマイナー (custom miner) パッケージと、インポート可能なフライトシート (flight sheet) (下の HiveOS セクションを参照)。
- **安全なアップグレード:** 古いバージョンがまだマイニング中の場合、新しいバージョンは起動を拒否します (先に停止するよう案内します)。`--stop` の後は、プログラムファイルを自動で再インストールし、設定とモデルはそのまま残します。
- **`--background` は、マイナーがすぐに終了した場合に「起動しました」と表示しなくなりました。** 代わりに理由を表示します。
- **Docker と小さな共有メモリ:** メモリ上のフォルダーが小さすぎる場合 (Docker の既定値は 64 MiB)、またはプログラムの実行が許可されていない場合、マイナーは代わりにディスク上の `thenewminer/.miner/run/` を使用し、その旨を表示します。
- **プールのパスワードに、表示可能な任意の文字を使えるようになりました** (記号や英語以外の文字も可): 6–128 文字、スペース不可。
- より多くの Linux システムに対応: インストーラーは一部の Ubuntu バージョンだけを受け付けるのではなく、必要なもの (glibc 2.34 以降) があるかどうかを確認するようになりました。

## ダウンロード

| ファイル | 対象 | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz) | Linux x86_64 | `6281bf489fae843551114f09ead5498d3e47af33a07e4cfdfbdc0a5d6bdbafbb` |
| [`TheNewMiner-v0.1.45-windows-wsl2-r1.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-windows-wsl2-r1.zip) | Windows 10 / 11 (WSL2 経由で動作) | `90732441b0dc67de84809d9bf3bda4441c0bf7cd9a52fefb95d29242ad7b15e7` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/SHA256SUMS) | 全パッケージのチェックサム | |

Windows 用 zip はオールインワンです。Linux 版マイナーがすでに含まれているので、必要なのは zip だけです。

## ダウンロードの検証

パッケージと `SHA256SUMS` を同じフォルダーに置いてから、次を実行します:

```bash
# Linux (--ignore-missing: ダウンロードしたパッケージだけを検査します)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (コマンド プロンプト): 出力を上の表の SHA-256 と比較してください
certutil -hashfile TheNewMiner-v0.1.45-windows-wsl2-r1.zip SHA256
```

各ファイルで `OK` と表示される必要があります (Windows では、表と同じ 64 文字であること)。一致しない場合は、そのファイルを使用しないでください。

## ワンクリックインストール (Linux)

このコマンドの末尾にある `<YOUR_WALLET>`、`<YOUR_POOL_PASSWORD>`、`<RIG_NAME>` を (`<` `>` も含めて) 置き換えてから、ターミナルに貼り付けてください。このバージョンの Linux パッケージと `SHA256SUMS` をダウンロードし (`curl` を使用し、`wget` は `curl` がない場合に使用します)、SHA-256 を確認して、マイナーを `~/thenewminer` に展開し、バックグラウンドで起動します:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.45 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

失敗した最初のステップで止まります。ダウンロードエラーや SHA-256 の不一致があれば、何も起動しません (この確認は破損したダウンロードを防ぐためのもので、どちらのファイルもこのページから取得します)。パスワードを囲むシングルクォートは残してください。その中ではどの文字も安全です (パスワード中の `'` は `'\''` と書きます)。`tar` が `xz: Cannot exec` と表示した場合は、xz をインストールして (`sudo apt-get install -y xz-utils`)、コマンドをもう一度実行してください。

すでに `~/thenewminer` にあるバージョンをアップグレードするには、まず `~/thenewminer/thenewminer --stop` を実行し、次にこのコマンドを実行します (設定とモデルは残ります)。以前のバージョンが別のフォルダーにある場合は、先にそちらで停止するか、Linux セクションの説明どおりにそちらでアップグレードしてください (そうしないとモデルが再ダウンロードされます)。

## Linux

```bash
tar -xJf TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| コマンド | 動作 |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | 初回起動: 必要なものをインストールし、バックグラウンドでマイニングします |
| `./thenewminer --background` | 二回目以降の起動: 設定は記憶されています |
| `./thenewminer` | フォアグラウンドでマイニング (Ctrl+C で停止) |
| `./thenewminer --logs` | マイナーの状態を表示 (Ctrl+C で表示を終了。マイニングは継続します) |
| `./thenewminer --status` | マイニング中かどうか、直近のハッシュレート、承認 / 拒否されたシェア |
| `./thenewminer --stop` | マイニングを停止 |
| `./thenewminer --help` | すべてのオプション |

- `-u` はあなたの TensorCash ウォレットアドレス、`-p` はあなたのプールのパスワード、`-w` はプール上に表示されるワーカー名です (既定値: マシンのホスト名。英字、数字、`_`、`-` または `.` からなる 1–64 文字)。
- プールのパスワード: 6–128 文字、表示可能な任意の文字、スペース不可。コマンドラインでは必ずシングルクォートで囲んでください。例: `-p 'my$pass!'` (シングルクォート内の `'` は `'\''` と書きます)。
- 設定は `thenewminer/.miner/` に記憶されます (パスワードを含むファイルはあなただけが読み取れます)。ウォレットとパスワードはすぐにコマンドラインから取り除かれるため、`ps` には表示されません (ただし、シェルの履歴にはコマンドが残る場合があります)。
- プールがウォレットまたはパスワードを拒否した場合 (またはまだアカウントを承認していない場合)、マイナーはその旨を表示して停止します。確認してから、`-u` と `-p` を付けて再度起動してください。
- 初回起動ではモデル (約 8.1 GiB、一度だけ) をダウンロードし、カードに合った GPU エンジンをインストールして、カード上でセルフチェックを行ってから、マイニングを開始します。`[ RATE] ... N/s` のような行が表示されていれば、マイニング中です。
- Ubuntu と Debian では、初回起動時に不足しているシステムライブラリをいくつか `apt` でインストールする場合があります (sudo または root が必要)。その他のシステムでは、インストールすべきものの名前を表示します。
- マルチ GPU リグ: 対応しており、各 GPU がマイニングします。このリリースはマシン一台につき GPU 一枚でテストしました。
- `tar` が `xz: Cannot exec` と表示する場合は、先に xz をインストールしてください: `sudo apt-get install -y xz-utils`。
- 共有メモリが少ない Docker などのコンテナーでは、マイナーはディスク上の `thenewminer/.miner/run/` からプログラムファイルを実行し、その旨を表示します。別のフォルダーを選ぶには、`TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` で起動してください (あなただけが書き込めるフォルダー: `chmod 700 <folder>`)。
- **アップグレード:** `./thenewminer --stop` を実行し、新しいパッケージを同じ場所に展開します (展開先は `thenewminer/` で、設定とモデルが入った `thenewminer/.miner/` はそのまま残ります)。その後、もう一度起動してください。古いバージョンがまだ動作中の場合、新しいバージョンは起動を拒否し、先に古いほうを停止するよう求めます。

## Windows (ワンクリック)

1. `TheNewMiner-v0.1.45-windows-wsl2-r1.zip` 全体を展開します (右クリック › すべて展開)。展開先はどのフォルダー、どのドライブでもかまいません。スペースや中国語の文字を含むパスでも問題ありません。zip の中身を表示した画面から直接実行しないでください。
2. **`TheNewMiner.bat`** をダブルクリックします。
   - **初回:** Windows、ドライバー、ディスクを確認し、必要であれば WSL2 を有効にします (管理者の確認画面が一度表示されます。再起動が一度必要になる場合がありますが、その後セットアップは自動で続行されます)。データの保存先を表示し (Enter で確定)、専用の Linux (WSL) 環境を作成し、同梱のマイナーをインストールして、ウォレットアドレス (`tc1` で始まる)、プールのパスワード (二回入力。6–128 文字、表示可能な任意の文字、スペース不可)、ワーカー名 (Enter = コンピューター名) を**一度だけ**尋ねます。その後モデルをダウンロードし、マイニングを開始します。
   - **二回目以降:** ダブルクリックするだけで、保存された設定ですぐにマイニングを開始します。デスクトップにショートカット「TheNewMiner」も作成されます。すでにマイニング中にもう一度ダブルクリックすると、その旨を伝えてログを表示します。

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
- プールがウォレットまたはパスワードを拒否した場合、ウィンドウにその旨が表示され、マイニングは停止します。`TheNewMiner.bat setup` を実行して入力し直してください。
- **アップグレード:** 新しいバージョンを任意の場所に展開し、その `TheNewMiner.bat` をダブルクリックします。インストール済みのマイナーを見つけ、設定とモデルを残したまま、何も尋ねずに新しいバージョンをインストールします。古いフォルダーは削除してかまいません。
- **アンインストール:** `TheNewMiner.bat uninstall`。
- 画面の表示言語は Windows の言語に従います (English / 简体中文 / 繁體中文)。言語を選ぶには、任意のコマンドに `-Lang en` (または `zh-Hans`、`zh-Hant`) を追加します。例: `TheNewMiner.bat -Lang en`。その実行にのみ適用されます。

## HiveOS (試験的)

HiveOS 対応は v0.1.45 の新機能で、**試験的**なものです。下の HiveOS 用カスタムマイナー (custom miner) パッケージを使用します。

| ファイル | SHA-256 |
|---|---|
| [`thenewminer-0.1.45.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz) | `90576efd388ceeca4e456b62b568f267465320913a07c9f9736f42363b1701dc` |

クイックセットアップ (フライトシートをインポート):

1. **Wallets** (ウォレット): コイン `TSC` と、あなたの TensorCash アドレス (`tc1...`) でウォレットを追加します。これは一度だけ必要です。HiveOS では `TSC` ウォレットが選択されている必要があり、マイナーはフライトシートに入力したアドレスを使用します。
2. [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json) を開いて内容をコピーします。`<YOUR_WALLET>` をあなたのウォレットアドレスに (後ろの `.%WORKER_NAME%` はそのまま残します)、`<YOUR_POOL_PASSWORD>` をあなたのプールのパスワードに置き換えます (JSON の中では、パスワード中の `"` は `\"`、`\` は `\\` と書きます)。HiveOS で Flight Sheets › **Import from clipboard** を選び、貼り付けて、`TSC` ウォレットを選択し、保存します。
3. フライトシートをリグに適用します。リグがパッケージをダウンロードし、マイニングを開始します。

代わりに手動で設定する場合: Coin `TSC`、Wallet: あなたの `TSC` ウォレット、Pool: "Configure in miner"、Miner: "Custom" でフライトシートを作成し、**Setup Miner Config** をクリックして、次のとおり入力します:

| フィールド | 値 |
|---|---|
| Miner name | `thenewminer` (Installation URL から自動入力) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz` |
| Hash algorithm | 空欄のまま |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (無視されます: プールは組み込み済み) |
| Pass | あなたのプールのパスワード |
| Extra config arguments | 空欄、または `--proxy <YOUR_PROXY>` (プロキシのセクションを参照) |

- プール上のワーカー名は、HiveOS でのリグ名です。英字、数字、`_`、`-` または `.` のみを使用してください (スペース不可)。最大 64 文字です。別のワーカー名を使うには、Extra config arguments に `-w <RIG_NAME>` を追加してください。
- モデルとマイナーのために、HiveOS のドライブに約 12 GB の空き容量が必要です (`df -h /hive/miners` で確認できます)。
- モデル (約 8.1 GiB) は `/hive/miners/custom/.thenewminer-models` に一度だけダウンロードされ、アップグレード時も残ります。
- ウォレットとパスワードは、コマンドラインではなく環境変数を通じてマイナーに渡されます。
- プールがウォレットまたはパスワードを拒否した場合、マイナーのログにその旨が表示され、マイナーは 10 分間待機し、その後 HiveOS がマイナーを再起動します。フライトシートを修正して適用してください (適用するとマイナーはすぐに再起動します)。
- ログ: HiveOS のマイナー画面 (リグのシェルで `miner`)、または `/var/log/miner/custom/custom.log`。
- **アップグレード:** Installation URL 中の両方のバージョン番号を変更し (例: `.../raw/v0.1.46/thenewminer-0.1.46.tar.gz`)、フライトシートを適用します。HiveOS は URL のファイル名が変わったときにだけインストールします。
- **初回起動には時間がかかります:** モデルのダウンロード後、RTX 30 シリーズのカードでは、一度だけ行われる GPU チェックがリグの遅いディスクではタイムアウトすることがあり、その場合 HiveOS がマイナーを自動的に二、三回再起動します (約 8 分)。そのまま待ってください。その後マイニングを開始します。
- **モデルをすでにお持ちですか?** 初回起動の前に `Qwen3-8B-Q8_0-full-v2.gguf` (約 8.1 GiB、ファイル名は正確にこのとおり) をリグの `/home/user/` にコピーしてください。マイナーがそれを見つけて SHA-256 を検査し、再ダウンロードせずにそれを使用します。

**リグから GitHub に接続できない場合:** PC で `thenewminer-0.1.45.tar.gz` をダウンロードし (上の表で SHA-256 を確認してください)、リグにコピーして、HiveOS がカスタムマイナーを置く場所に展開します。その後、いつもどおりフライトシートを適用します。HiveOS は "Already installed" と表示し、ダウンロードは行いません。ファイル名は正確に `thenewminer-0.1.45.tar.gz` のままにし (ブラウザーが " (1)" を付けることがあります)、フライトシートの Installation URL も変更しないでください。HiveOS はこの名前を比較します。

```bash
# PC で
scp thenewminer-0.1.45.tar.gz user@<RIG_IP>:/tmp/
# リグで root として (Hive Shell、または SSH 接続後に sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.45.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.45.tar.gz
```

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
| GPU | NVIDIA GeForce RTX 30 / 40 / 50 シリーズ、VRAM 10 GB 以上。マルチ GPU リグ: 対応しており、各 GPU がマイニングします。このリリースではマシン一台につき GPU 一枚でテストしました |
| ドライバー | NVIDIA **580 以降** (`nvidia-smi` が動作すること)。Windows では Windows 側にインストールし、WSL の中には決してインストールしないでください |
| Linux | **glibc 2.34 以降**の x86_64。例: Ubuntu 22.04 以降、または Debian 12 以降。インストーラーが必要なものを確認し、不足しているものの名前を表示します |
| Windows | Windows 10 21H2 以降、または Windows 11 (64 ビット)。BIOS で仮想化 (Intel VT-x / AMD SVM) を有効にすること |
| ディスク | 約 20 GB の空き容量 |
| メモリ | 4 GB 以上 |
| モデル | 約 8.1 GiB。初回起動時に一度だけダウンロードし (並列、複数ミラー)、以降は再利用します |

## NVIDIA ドライバーの更新 (580 以降)

マイナーには NVIDIA ドライバー **580 以降**が必要です。現在のバージョンを確認してください:

```bash
nvidia-smi    # ヘッダーに "Driver Version: ..." と表示されます。580 以降である必要があります
```

- **Ubuntu:** `sudo ubuntu-drivers install` で推奨ドライバーがインストールされます (インストール後、580 以降であることを確認してください)。バージョンを自分で選ぶ場合は `sudo apt install nvidia-driver-580` (またはそれ以降。RTX 50 シリーズのカードには open 版が必要です。例: `nvidia-driver-580-open`) を実行します。その後、再起動してください。Ubuntu のドライバーパッケージと NVIDIA の `.run` インストーラーは、どちらか一方だけを使い、決して併用しないでください。
- **Debian:** Debian に標準で含まれるドライバーパッケージは 580 より古いバージョンです (Debian 12 では 535、Debian 13 では 550)。[NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html) (NVIDIA ドライバーインストールガイド) の説明に従って NVIDIA のリポジトリから新しいドライバーをインストールし、再起動してください。
- **Windows:** [NVIDIA アプリ](https://www.nvidia.com/en-us/software/nvidia-app/) (旧 GeForce Experience) で更新するか、[nvidia.com](https://www.nvidia.com/en-us/drivers/) からお使いのカード用のドライバー (580 以降) をダウンロードしてください。WSL2 はこの Windows ドライバーを使用します。WSL の中に Linux 用 NVIDIA ドライバーを決してインストールしないでください。
- **HiveOS:** root として (Hive Shell、または SSH で接続してから `sudo -i`)、`nvidia-driver-update --list` を実行してバージョンを確認し、次に例えば `nvidia-driver-update 595` (580 以降であればどれでも可) を実行し、最後に `sreboot` を実行します。

## 参考ハッシュレート

自社のリグで測定した、実際のプールでの最高ハッシュレートです (2026-10-14 から 2026-10-23 の間)。これらは**最大**値であり、保証ではありません。同じモデルのカードでも、個々のチップ、冷却、クロック、電力制限によって約 5–8% の差があり、カードでディスプレイも出力している場合や、他のプログラムが GPU を使用している場合はハッシュレートが下がります。

| GPU | ハッシュレート |
|---|---|
| RTX 5090 | 最大 ~99 N/s |
| RTX 4090 | 最大 ~70 N/s |
| RTX 3080 10 GB | 最大 ~22 N/s |

## テスト結果

自社のテストリグで行った実プールテストです (マシン一台につき GPU 一枚)。A = 承認されたシェア、R = 拒否されたシェア。「安定マイニング時」は、ジョブ切り替えの間のハッシュレートの中央値です。

| 日付 | カード | OS | パッケージ | ハッシュレート | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | v0.1.45 リリースビルド | 65.42 N/s (安定マイニング時、16 分。ジョブ切り替えを含めると 62.35) | 13 / 0 (無効 0) |
| 2026-10-23 | RTX 5090 | Linux | v0.1.45 リリースビルド | 94.18 N/s (安定マイニング時。ジョブ切り替えを含めると 92.24) | 17 / 0 (無効 0) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.44 sealed-r4 | 65.58 N/s (中央値、16 分) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | v0.1.44 sealed-r4 | 93.45 N/s (安定マイニング時、44 分。ジョブ切り替えを含めると 90.39) | 31 / 0 (停止時に stale が +1) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | v0.1.44 windows-wsl2-r5 | ~22 N/s (運営者自身による実行) | 記録なし |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | v0.1.44 sealed-r4 | 21.04 N/s (23 分間のマイニングのうち最後の 10 分) | 3 / 0 |

Linux 上の RTX 3080 (v0.1.44): インストールは 6/6 ステップ成功、無効なウィンドウなし、正常に停止。ログの抜粋:
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

**プールがウォレット / パスワードを拒否したとマイナーが表示する。**
ウォレットアドレスとプールのパスワード (およびプールがアカウントを承認済みかどうか) を確認してから、再度起動してください:
Linux は `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`、Windows は `TheNewMiner.bat setup`。マイナーは誤った認証情報で自動的に再試行することはありません。

**マイニングしていますか? ログはどこにありますか?**
Linux: `./thenewminer --status`、`./thenewminer --logs`、またはファイル `thenewminer/.miner/thenewminer.log`。
Windows: `TheNewMiner.bat status` と `TheNewMiner.bat logs`。

## セキュリティに関する注意

マイナーと GPU エンジンは、パッケージ内で暗号化された状態 (ChaCha20-Poly1305) で配布されます。マイナーは起動時に、あなたのマシン上でそれらを復号します。これによりコピーのコストは上がりますが、**破られないわけではありません**: マイナーを実行しているマシンの管理者権限を持つ人なら誰でも、十分な労力をかければ、鍵または復号されたコードを取り出すことができます。ウォレットアドレスとプールのパスワードはあなた自身のマシンに保存され、プールにのみ送信されます。
