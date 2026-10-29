[English](../GUIDE.md) · [简体中文](GUIDE.zh-CN.md) · [繁體中文](GUIDE.zh-TW.md) · [Русский](GUIDE.ru.md) · [Español](GUIDE.es.md) · [Português (Brasil)](GUIDE.pt-BR.md) · [Français](GUIDE.fr.md) · [Deutsch](GUIDE.de.md) · [Türkçe](GUIDE.tr.md) · [Tiếng Việt](GUIDE.vi.md) · [한국어](GUIDE.ko.md) · **日本語**

> この翻訳は便宜のために提供されています。正式な内容は英語版 README です。 *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner ガイド

初回起動以降のすべて: ダウンロードの検証、Linux・Windows・HiveOS での日常的な使い方、アップグレード、マイニングしないときの対処、よくある質問。ダウンロードとクイックスタートは[メインページ](README.ja.md)にあります。Windows の問題には専用のページがあります: [Windows のトラブルシューティング](WINDOWS-TROUBLESHOOTING.ja.md)。

## ダウンロードの検証

| ファイル | SHA-256 |
|---|---|
| [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | `86c215dc9401209b8d2a2346252ed5cc1901527b6a51520372e3128583c63656` |
| [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | `45a459646713bef9f8285848486c0a4bd66dee65e8c63090c8d3345c998b2659` |
| [`thenewminer-0.1.47.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz) (HiveOS) | `6b2515c5bf23f60daa236ff4a8b165ce5e55265d7eecac9dd408896e1e31b784` |

Windows 用 zip はオールインワンです。Linux 版マイナーがすでに含まれているので、必要なのは zip だけです。パッケージと [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS) を同じフォルダーに置いてから、次を実行します:

```bash
# Linux (--ignore-missing: ダウンロードしたパッケージだけを検査します)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (コマンド プロンプト): 出力を上の表の SHA-256 と比較してください
certutil -hashfile TheNewMiner-v0.1.47-windows-wsl2-r3.zip SHA256
```

各ファイルで `OK` と表示される必要があります (Windows では、表と同じ 64 文字であること)。一致しない場合は、そのファイルを使用しないでください。

## Linux

```bash
tar -xJf TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| コマンド | 動作 |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | 初回起動: 必要なものをインストールし、バックグラウンドでマイニングします |
| `./thenewminer --background` | 二回目以降の起動: 設定は記憶されています |
| `./thenewminer` | フォアグラウンドでマイニング (Ctrl+C で停止) |
| `./thenewminer --logs` | マイナーの状態を表示 (Ctrl+C で表示を終了。マイニングは継続します) |
| `./thenewminer --status` | マイニング中かどうか、直近のハッシュレート、承認 / 拒否されたシェア。停止後は、停止した理由と対処方法 |
| `./thenewminer --stop` | マイニングを停止 |
| `./thenewminer --help` | すべてのオプション |

- `-u` はあなたの TensorCash ウォレットアドレス、`-p` はあなたのプールパスワード、`-w` はプール上に表示されるワーカー名です (既定値: マシンのホスト名。英字、数字、`_`、`-` または `.` からなる 1–64 文字)。
- ウォレットアドレスは、何かを始める前に確認されます。入力ミスのあるアドレス (チェックサムが一致しない) や例として示されたアドレスは、何が誤っているかを示したうえで拒否されます。
- プールパスワード: 6–128 文字、表示可能な任意の文字、スペース不可。コマンドラインでは必ずシングルクォートで囲んでください。例: `-p 'my$pass!'` (シングルクォート内の `'` は `'\''` と書きます)。
- 設定は `thenewminer/.miner/` に記憶されます (パスワードを含むファイルはあなただけが読み取れます)。ウォレットとパスワードはすぐにコマンドラインから取り除かれるため、`ps` には表示されません (ただし、シェルの履歴にはコマンドが残る場合があります)。
- GPU が一枚のマシンの初回起動では、マイナーはモデルをダウンロードする**前に**ウォレットとパスワードをプールで確認するため、入力ミスは数秒で見つかります (マルチ GPU リグではマイニング開始時に見つかります)。プールがそれらを拒否した場合、マイナーはその旨を表示して停止します。確認してから、`-u` と `-p` を付けて再度起動してください。
- 初回起動ではモデル (約 8.1 GiB、一度だけ) をダウンロードし、カードに合った GPU エンジンをインストールして、カード上でセルフチェックを行ってから、マイニングを開始します。`[ RATE] ... N/s` のような行が表示されていれば、マイニング中です。ログは `thenewminer/.miner/thenewminer.log` です。
- 標準の Ubuntu 22.04 / 24.04 (WSL のものを含む) には、マイナーに必要なライブラリがすべてそろっているため、何もインストールしません。不足しているもの (Docker イメージ、最小構成のシステム) がある場合、Ubuntu、Debian、HiveOS では初回起動時にその一覧を表示し、`apt` でインストールします (sudo または root が必要)。その他のシステムでは、インストールすべきものの名前を表示します。
- マルチ GPU リグ: 対応しており、各 GPU がマイニングします。このリリースはマシン一台につき GPU 一枚でテストしました。
- 共有メモリが少ない Docker などのコンテナーでは、マイナーはディスク上の `thenewminer/.miner/run/` からプログラムファイルを実行し、その旨を表示します。別のフォルダーを選ぶには、`TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` で起動してください (あなただけが書き込めるフォルダー: `chmod 700 <folder>`)。
- **アップグレード:** `./thenewminer --stop` を実行し、新しいパッケージを同じ場所に展開します (展開先は `thenewminer/` で、設定とモデルが入った `thenewminer/.miner/` はそのまま残ります)。その後、もう一度起動してください。古いバージョンがまだ動作中の場合、新しいバージョンは起動を拒否し、先に古いほうを停止するよう求めます。代わりに新しいフォルダーに展開した場合は、古いほうのモデル (同じサイズと SHA-256) を見つけて再利用するため、8.1 GiB を再ダウンロードすることはありません。その場合は、そこで `-u -p -w` をもう一度指定してください。

### マイニングしないとき

`./thenewminer --status` で、マイナーが停止した理由と対処方法がわかります。よくあるメッセージ:

| 表示 | 対処 |
|---|---|
| `bash: ./thenewminer: Permission denied` | そのフォルダーではプログラムを実行できません (`noexec` でマウントされた場所 (一部の `/tmp` など)、または USB メモリ)。ホームフォルダーに展開してください: `cd ~ && tar -xJf <the package>.tar.xz && cd thenewminer` |
| `this package is on a FAT (vfat) drive` (または exFAT / NTFS、あるいは `Windows (DrvFs, 9p)`: WSL での `/mnt/d` などの Windows のドライブ) | このようなドライブではマイナーの非公開設定 (モード 600) を保持できません。表示される `mkdir -p ~/thenewminer && cp -a ...` の行を貼り付けてください。フォルダーを Linux のホームフォルダーにコピーし、そこで起動します |
| `this system has no xz` | `sudo apt-get install -y xz-utils` を実行し (root の場合は `sudo` なし)、もう一度起動してください |
| `This system is missing what TheNewMiner needs` | 表示される `sudo apt-get install -y ...` の行を実行し、もう一度起動してください |
| `No NVIDIA GPU found on this machine` | NVIDIA のカードが認識されていません。カードが正しく装着され、電源が供給されているか確認してください。クラウドのマシンでは、NVIDIA GPU を搭載したものを選んでください |
| `NVIDIA driver ... is too old` | 表示されたドライバー (580 以降) をインストールし、再起動してから、もう一度起動してください (下の「NVIDIA ドライバーの更新」を参照) |
| `the pool rejected the wallet / password` | `-u` と `-p`、およびプールのウェブサイトにアカウントが存在することを確認してから、`./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' --background` を実行してください |
| `no download source answered` | モデルのダウンロードがインターネットに接続できません。プロキシ経由の場合: `https_proxy=http://<HOST>:<PORT> ./thenewminer --background`。またはモデルをコピーしてください (下の「モデルをすでにお持ちですか?」を参照) |
| `cannot reach the pool` / `pool disconnected` | インターネット接続、DNS、ファイアウォールが送信方向の TCP 443 を許可しているかを確認してください。ネットワークがプールをブロックしている場合は `--proxy` を使います (下の「プロキシ」を参照)。再接続は自動で行われます |

### 再起動後に起動する (任意)

マイナーは再起動後に自動では起動しません (HiveOS では自動で起動します)。Linux では、`crontab` に一行追加するか:

```bash
crontab -e      # 次の行を追加します (ご自分のフォルダー):
@reboot cd /home/<YOU>/thenewminer && ./thenewminer --background
```

systemd のユーザーサービスを使います (ログインなしで起動するよう、一度だけ `loginctl enable-linger <YOU>` を実行します):

```ini
# ~/.config/systemd/user/thenewminer.service、その後: systemctl --user enable --now thenewminer
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

終了ステータス 77 は、プールがウォレット / パスワードを拒否したことを意味します。再起動しても解決しないため、サービスはそこで停止します。

## Windows

1. `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` 全体を展開します (右クリック › すべて展開)。展開先はどのフォルダー、どのドライブでもかまいません。スペースや中国語の文字を含むパスでも問題ありません。zip の中身を表示した画面から直接実行しないでください。
2. **`TheNewMiner.bat`** をダブルクリックします。
   - **初回:** まず一つのウィンドウでプールアカウントを尋ねます。ウォレットアドレス (`tc1` で始まる。チェックサムが確認されます。表示される例は本物のウォレットではありません)、プールパスワード (6–128 文字、表示可能な任意の文字、スペース不可)、ワーカー名 (コンピューター名が入力済み) です。三つすべてが正しくなるまで OK はグレー表示のままです。OK と「はい」を押した後は何も尋ねられません。Windows、ドライバー、ディスクを確認し、必要であれば WSL2 を有効にし (管理者の確認画面が一度表示されます。再起動が一度必要になる場合がありますが、その後セットアップは自動で続行されます)、データの保存先を表示し、専用の Linux (WSL) 環境を作成し、同梱のマイナーをインストールし、モデルをダウンロードしてから、マイニングを開始します。デスクトップがない場合 (SSH 経由) は、同じ質問がコンソールに表示されます。
   - **二回目以降:** ダブルクリックするだけで、保存された設定ですぐにマイニングを開始します。デスクトップにショートカット「TheNewMiner」も作成されます。すでにマイニング中にもう一度ダブルクリックすると、その旨を伝えてログを表示します。
   - **セットアップが止まった場合**、どの段階で止まったか、何をすべきかがウィンドウに表示され、`TheNewMiner.bat` を実行すると再び表示されます。[Windows のトラブルシューティング](WINDOWS-TROUBLESHOOTING.ja.md)を参照してください。

日常的に使うコマンド (展開したフォルダーでコマンド プロンプトを開きます):

| コマンド | 動作 |
|---|---|
| `TheNewMiner.bat` | 必要ならインストールし、このウィンドウでマイニング (ウィンドウを閉じるとマイニングが停止します) |
| `TheNewMiner.bat background` | ウィンドウなしでマイニング |
| `TheNewMiner.bat logs` | マイナーの状態を表示 (Ctrl+C で表示を終了。マイニングは継続します) |
| `TheNewMiner.bat status` | 動作中かどうかと、直近のハッシュレート |
| `TheNewMiner.bat stop` | マイニングを停止 |
| `TheNewMiner.bat setup` | ウォレット / パスワード / ワーカー名を入力し直す (キャンセルすると現在の設定を維持) |
| `TheNewMiner.bat uninstall` | マイナー、そのデータフォルダー、ショートカットを削除 (事前に確認します) |

- **データの保存先:** マイナーとモデルは、初回インストール時に選んだデータフォルダーに置かれます (既定では、展開先ドライブのルートにある `TheNewMinerData`。例: `D:\TheNewMinerData`)。展開したフォルダーには置かれません。そこに約 20 GB の空き容量が必要です。別のフォルダーを選ぶには: `TheNewMiner.bat setup -DataDir <folder>`。
- **Ubuntu:** セットアップは `wsl --install` で Ubuntu 24.04 をインストールします。そのダウンロードで接続できない場合は、中国の公開ミラー (清華大学 TUNA、次に USTC、次に Aliyun) から Ubuntu 公式の WSL イメージ `ubuntu-24.04.5-wsl-amd64.wsl` をダウンロードし、その SHA-256 が `bb415d824822c4b878125729af451a5d18fb13d1cf5cbed9a7393ad64ac6039e` と完全に一致した場合にのみ使用します。
- プールがウォレットまたはパスワードを拒否した場合、ウィンドウにその旨が表示され、マイニングは停止します。`TheNewMiner.bat setup` を実行して入力し直してください。
- **アップグレード:** 新しいバージョンを任意の場所に展開し、その `TheNewMiner.bat` をダブルクリックします。インストール済みのマイナーを見つけ、設定とモデルを残したまま、何も尋ねずに新しいバージョンをインストールします。以前のバージョンで行ったインストールや、別の WSL ディストリビューション (例: `Ubuntu-24.04`) 内に手動で行ったインストールも検出され、その場所でアップグレードされます。二重のインストールも、新しい Ubuntu も、モデルの再ダウンロードもありません。その場所でアップグレードできない場合、セットアップは停止し、その理由を表示します。古いフォルダーは削除してかまいません。
- **アンインストール:** `TheNewMiner.bat uninstall`。ご自身で作成した WSL ディストリビューションを削除することはありません。
- Windows 用キットの表示は英語です。

## HiveOS (試験的)

HiveOS 対応は**試験的**なものです (v0.1.45 から提供。v0.1.45 は実際の HiveOS リグで動作しましたが、v0.1.46 以降の変更はまだ HiveOS リグで動かしていません)。HiveOS 用カスタムマイナー (custom miner) パッケージ `thenewminer-0.1.47.tar.gz` を使用します (SHA-256 は「ダウンロードの検証」の表にあります)。

クイックセットアップ (フライトシートをインポート):

1. **Wallets** (ウォレット): コイン `TSC` と、あなたの TensorCash アドレス (`tc1...`) でウォレットを追加します。これは一度だけ必要です。HiveOS では `TSC` ウォレットが選択されている必要があり、マイナーはフライトシートに入力したアドレスを使用します。
2. [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) を開いて内容をコピーします。`<YOUR_WALLET>` をあなたのウォレットアドレスに (後ろの `.%WORKER_NAME%` はそのまま残します)、`<YOUR_POOL_PASSWORD>` をあなたのプールパスワードに置き換えます (JSON の中では、パスワード中の `"` は `\"`、`\` は `\\` と書きます)。HiveOS で Flight Sheets › **Import from clipboard** を選び、貼り付けて、`TSC` ウォレットを選択し、保存します。
3. フライトシートをリグに適用します。リグがパッケージをダウンロードし、マイニングを開始します。

代わりに手動で設定する場合: Coin `TSC`、Wallet: あなたの `TSC` ウォレット、Pool: "Configure in miner"、Miner: "Custom" でフライトシートを作成し、**Setup Miner Config** をクリックして、次のとおり入力します:

| フィールド | 値 |
|---|---|
| Miner name | `thenewminer` (Installation URL から自動入力) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` |
| Hash algorithm | 空欄のまま |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (無視されます: プールは組み込み済み) |
| Pass | あなたのプールパスワード |
| Extra config arguments | 空欄、または `--proxy <YOUR_PROXY>` (プロキシのセクションを参照) |

- プール上のワーカー名は、HiveOS でのリグ名です。英字、数字、`_`、`-` または `.` のみを使用してください (スペース不可)。最大 64 文字です。別のワーカー名を使うには、Extra config arguments に `-w <RIG_NAME>` を追加してください。
- モデルとマイナーのために、HiveOS のドライブに約 12 GB の空き容量が必要です (`df -h /hive/miners` で確認できます)。
- モデル (約 8.1 GiB) は `/hive/miners/custom/.thenewminer-models` に一度だけダウンロードされ、アップグレード時も残ります。
- ウォレットとパスワードは、コマンドラインではなく環境変数を通じてマイナーに渡されます。
- **何かが失敗した場合** (例えばプールがウォレットまたはパスワードを拒否した場合)、マイナーのログと HiveOS のダッシュボードにその理由が表示され、リグは待ち時間を徐々に延ばしながら (一分から十分まで) 再試行します。フライトシートを修正して適用してください。適用するとマイナーはすぐに再起動します。
- ログ: HiveOS のマイナー画面 (リグのシェルで `miner`)、または `/var/log/miner/custom/custom.log`。
- **アップグレード:** Installation URL 中の両方のバージョン番号を変更し (例: `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`)、フライトシートを適用します。HiveOS は URL のファイル名が変わったときにだけインストールします。
- **初回起動**ではモデルをダウンロードして GPU エンジンをインストールするため、数分かかります。
- **モデルをすでにお持ちですか?** 初回起動の前に、リグの `/home/user/` にコピーしてください (次のセクションを参照)。

**リグから GitHub に接続できない場合:** PC で `thenewminer-0.1.47.tar.gz` をダウンロードし (SHA-256 を確認してください)、リグにコピーして、HiveOS がカスタムマイナーを置く場所に展開します。その後、いつもどおりフライトシートを適用します。HiveOS は "Already installed" と表示し、ダウンロードは行いません。ファイル名は正確に `thenewminer-0.1.47.tar.gz` のままにし (ブラウザーが " (1)" を付けることがあります)、フライトシートの Installation URL も変更しないでください。HiveOS はこの名前を比較します。

```bash
# PC で
scp thenewminer-0.1.47.tar.gz user@<RIG_IP>:/tmp/
# リグで root として (Hive Shell、または SSH 接続後に sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.47.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.47.tar.gz
```

## モデルをすでにお持ちですか? コピーしてください

マイナーには 8.1 GiB のモデルファイルが一つ必要です: `Qwen3-8B-Q8_0-full-v2.gguf` (`8709518112` バイト、SHA-256 `408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`)。このマシン、またはネットワーク上の別のコンピューターにすでにある場合は、もう一度ダウンロードせずにコピーしてください。

何かをダウンロードする前に、マイナーはファイルを正確な名前とサイズで探します。探す場所は、マイナー自身のモデルフォルダー、以前の TheNewMiner フォルダー、ホームフォルダー (およびその一、二階層下のフォルダー、またはそこにある `models` フォルダー)、`/home` の下の各ホームフォルダー、`/data`、`/mnt/*`、`/media/*`、`/opt`、`/srv`、`/workspace`、`/var/lib`、そして Hugging Face / ModelScope のキャッシュです。そこで見つからない場合は、同じディスク上のそれらのフォルダーで、サイズが完全に一致する `.gguf` を最大 20 秒間探します (ほかにマウントされているディスクは名前でのみ確認します)。見つかったファイルは **SHA-256 が一致した場合にのみ** 使用されます。その後マイナーは、そのファイルを自分のモデルフォルダーにリンク (同じディスクの場合) またはコピーするので、以降のどのバージョンもすぐに見つけられます。直接指定することもできます: `./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`。

**Linux:** ホームフォルダーにコピーしてから、いつもどおりマイナーを起動します。同じネットワーク上の Mac や PC からコピーする場合は (Windows 10 と 11 では PowerShell で `scp` を使えます)、`user` と `<MINER_IP>` をマイナーのログイン名とアドレスに置き換えてください:

```bash
# マイナー本体のマシン上で
cp /path/to/Qwen3-8B-Q8_0-full-v2.gguf ~/
# または別のコンピューターから
scp Qwen3-8B-Q8_0-full-v2.gguf user@<MINER_IP>:~/
```

**HiveOS:** リグの `/home/user/` にコピーし (HiveOS の SSH ログイン名は `user` です)、フライトシートを適用します。マイナーはそこでファイルを見つけて検査し、パッケージの隣 (`/hive/miners/custom/.thenewminer-models`) に保存するので、新しいバージョンが再びダウンロードすることはありません。HiveOS のディスクは小さいため、先に `disk-expand` を実行してください (`df -h /hive/miners` で空き容量が 12 GB 未満と表示される場合)。

```bash
scp Qwen3-8B-Q8_0-full-v2.gguf user@<RIG_IP>:/home/user/
```

**Windows:** 最初の `setup` で TheNewMiner の WSL ディストリビューションが作成された後、コマンド プロンプトからファイルをその中にコピーします (`D:\Models` はお手元のコピーがある場所に変更してください)。すでにダウンロードが始まっていた場合は、ウィンドウを閉じてこれらのコマンドを実行し、もう一度 TheNewMiner を起動してください。コピーを見つけてダウンロードを省略します。コピーには数分かかり、進行状況は表示されません。

```bat
wsl -d TheNewMiner -u root -- mkdir -p ~/thenewminer/models
wsl -d TheNewMiner -u root -- cp /mnt/d/Models/Qwen3-8B-Q8_0-full-v2.gguf ~/thenewminer/models/
```

## ウォレット、プールパスワード、ワーカー名の変更

プールは各支払いアドレスを、そのアドレス専用のプールアカウントに恒久的に紐付けます。プールのページ [tsc.the-new.win](https://tsc.the-new.win) は TSC の支払いアドレスを「永久绑定 · 不可修改」(永久に紐付け済み、変更不可) と記載し、切り替えるにはマイナーで新しいアドレスを直接使うよう案内しています。したがって、別のウォレットにはそのウォレット専用のプールアカウントとプールパスワードが必要です (先にプールのページで作成してください)。プールのページでプールパスワードを変更した場合は、すべてのマイナーでパスワードを更新してください。プールは忘れたパスワードを復元できません。

**Linux:** マイナーを停止してから、新しい設定で一度起動します。新しい設定が、記憶されている設定に置き換わります:

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

`-u` と `-p` は必ず一緒に指定してください。どちらか一方だけでは、記憶されているウォレットとパスワードは変更されません。ワーカー名は単独で変更できます: `./thenewminer -w <RIG_NAME> --background` (`--stop` の後で)。

**Windows:** `TheNewMiner.bat setup` を実行します。ワーカー名が入力済みの状態でアカウント入力ウィンドウが開きます。ウォレットとプールパスワードを入力してから、いつもどおり起動します (`TheNewMiner.bat` をダブルクリック)。

**HiveOS:** フライトシートを編集します。"Wallet and worker template" に新しいウォレットアドレス (`<NEW_WALLET>.%WORKER_NAME%`) を、Pass に新しいプールパスワードを入力し、保存して適用します。ワーカー名は HiveOS でのリグ名です (または Extra config arguments の `-w <RIG_NAME>`)。

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

## よくある質問

**Windows で「Windows によって PC が保護されました」/「不明な発行元」と表示される。**
このキットはコード署名されていません。**詳細情報**をクリックしてから、**実行**をクリックしてください。

**ウイルス対策ソフトがマイナーを検出する、または削除する。**
Microsoft Defender などのウイルス対策ソフトは、マルウェアがマイナーを隠し持っていることが多いため、自分で実行を選んだ正規のものであっても、暗号通貨マイナーを「望ましくない可能性のあるアプリケーション」として検出することがよくあります。ファイルが検疫された場合は、Windows セキュリティ › ウイルスと脅威の防止 › 保護の履歴 を開き、TheNewMiner の項目を選んで **許可** を選択してください (または、展開したフォルダーを除外に追加してください)。これは、このページからダウンロードし、SHA-256 が一致したパッケージに対してのみ行ってください。

**初回起動に時間がかかる。**
初回起動では約 8.1 GiB のモデルをダウンロードし、カード上で GPU セルフチェックを行います。Windows では WSL2 のセットアップも行う場合があります。これは一度だけです。二回目以降は、はるかに早くマイニングが始まります。

**プールがウォレット / パスワードを拒否したとマイナーが表示する。**
ウォレットアドレスとプールパスワード (アカウントがプールのウェブサイトに存在している必要があります) を確認してから、再度起動してください: Linux は `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`、Windows は `TheNewMiner.bat setup`。マイナーは誤った認証情報で自動的に再試行することはありません。

**マイニングしていますか? ログはどこにありますか?**
Linux: `./thenewminer --status`、`./thenewminer --logs`、またはファイル `thenewminer/.miner/thenewminer.log`。
Windows: `TheNewMiner.bat status` と `TheNewMiner.bat logs`。

**RTX 3080 (10 GB) が Windows では Linux よりずっと遅い。**
Windows では、ほかのプログラムが VRAM を共有しています。[Windows のトラブルシューティング](WINDOWS-TROUBLESHOOTING.ja.md)を参照し、ハードウェアアクセラレーションを有効にしたブラウザーと Docker Desktop を閉じてください。
