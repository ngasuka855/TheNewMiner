# TheNewMiner

TheNewMiner は、VRAM 10 GB 以上の NVIDIA GeForce RTX 30、40、50 シリーズのカード向けの **TensorCash** 用 GPU マイナーです。プール **`tsc-wss.the-new.win`** が組み込まれています。

[English](../../README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · **日本語**

> この翻訳は便宜のために提供されています。正式な内容は英語版 README です。 *(This translation is provided for convenience; the English README is authoritative.)*

## ダウンロード

現在のバージョン: **v0.1.47** · プールのウェブサイト: [tsc.the-new.win](https://tsc.the-new.win/#/home)

| システム | ダウンロード | その後 |
|---|---|---|
| Linux x86_64 | [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | または下のワンクリックインストールを使用 |
| Windows 10 / 11 | [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | 展開して `TheNewMiner.bat` をダブルクリック |
| HiveOS (試験的) | [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) | フライトシートをインポート (下の HiveOS を参照) |

チェックサム: [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS)。確認方法は[ガイド](GUIDE.ja.md)にあります。ファイルが一致しない場合は、そのファイルを使用しないでください。

## 始める前に: プールアカウントを作成する

マイニングにはプールアカウントが必要です。TSC の受取アドレス (ウォレット) と、プールのウェブサイトで自分で決めるプールパスワードです。サイトには英語版があります (サイトの言語メニューで切り替えてください)。

1. [tsc.the-new.win](https://tsc.the-new.win/#/home) を開き、**Start mining** (开始挖矿) に進みます。
2. **1. Bind Wallet** で **TSC payout address** を入力し (`tc1...`。よく確認してください。誤ったアドレスに送られたマイニング収入は取り戻せません)、**Pool Password** (6–128 文字) を決めて、**Confirm Password** にもう一度入力します。
3. **Apply to join pool** をクリックします。ウォレットが紐付けられたことがページに表示され、アカウントはすぐに使えます。
4. マイナーでも同じウォレットとパスワードを使います。Linux では `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`、Windows では `TheNewMiner.bat` のアカウント入力ウィンドウ、HiveOS ではフライトシートで指定します。マイニング開始から約 3–5 分後に、プールのウェブサイトの **My Miners** にリグが表示されます。

忘れたプールパスワードはプール側で復旧できず、受取アドレスも後から変更できません。プールパスワードはウォレットの秘密鍵ではありません。秘密鍵は決して誰にも渡さないでください。

## ワンクリックインストール (Linux)

このコマンドの末尾にある `<YOUR_WALLET>`、`<YOUR_POOL_PASSWORD>`、`<RIG_NAME>` を (`<` `>` も含めて) それぞれご自分のウォレット、プールパスワード、このリグの名前に置き換えてから、ターミナルに貼り付けてください。Linux パッケージと `SHA256SUMS` をダウンロードし、SHA-256 を確認して、マイナーを `~/thenewminer` に展開し、バックグラウンドで起動します:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.47 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

ダウンロードエラーや SHA-256 の不一致があれば、何も起動しません。パスワードを囲むシングルクォートは残してください (パスワード中の `'` は `'\''` と書きます)。GPU が一枚のマシンでは、マイナーはモデル (約 8.1 GiB、一度だけ) をダウンロードする前に、ウォレットとパスワードをプールで確認します。その後、`./thenewminer --logs` でマイニングの様子を確認でき、`./thenewminer --status` で動作中かどうか、または停止した理由がわかります。アップグレードするには、まず `~/thenewminer/thenewminer --stop` を実行し、次に同じコマンドを実行します。設定とモデルは残ります。それ以外のこと (日常的に使うコマンド、アップグレード、自動起動、マイニングしないときの対処) はすべて[ガイド](GUIDE.ja.md)にあります。

## Windows

1. `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` 全体を任意の場所に展開します (右クリック › すべて展開)。
2. **`TheNewMiner.bat`** をダブルクリックします。初回は、プールアカウント (ウォレット、プールパスワード、ワーカー名) を尋ねるウィンドウが一つだけ表示されます。その後は何も尋ねられません。WSL2 と Ubuntu をセットアップし、同梱のマイナーをインストールし、モデルをダウンロードしてマイニングを開始します。二回目以降は、ダブルクリックするとすぐにマイニングが始まります。最初に Windows で「Windows によって PC が保護されました」と表示されることがあります (このキットはコード署名されていません)。**詳細情報**をクリックしてから、**実行**をクリックしてください。WSL を有効にするときに管理者権限を一度求められ、再起動が一度必要になる場合があります。その後、セットアップは自動的に続行されます。
3. 以前のバージョンが見つかった場合は、設定とモデルを残したまま、その場でアップグレードされます。

停止したりマイニングが遅かったりする場合は、[Windows のトラブルシューティング](WINDOWS-TROUBLESHOOTING.ja.md) (DNS、Windows のドライブ、ブラウザーや Docker Desktop が使う VRAM、時計) を参照してください。

## HiveOS (試験的)

1. **Wallets** (ウォレット): コイン `TSC` と、あなたの TensorCash アドレス (`tc1...`) でウォレットを追加します。
2. [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) を開いて内容をコピーし、`<YOUR_WALLET>` (後ろの `.%WORKER_NAME%` はそのまま残します) と `<YOUR_POOL_PASSWORD>` を置き換えます。HiveOS で Flight Sheets › **Import from clipboard** を選び、貼り付けて、`TSC` ウォレットを選択し、保存します。
3. フライトシートをリグに適用します。`thenewminer-0.1.47.tar.gz` がインストールされ、マイニングが始まります。

手動での設定、アップグレード、GitHub に接続できないリグについては、[ガイド](GUIDE.ja.md)を参照してください。

## v0.1.47 の新機能

- **インストールがより簡単に:** GPU が一枚のマシンでは、マイナーは 8.1 GiB のモデルをダウンロードする**前に**、ウォレットとプールパスワードをプールで確認するため、入力ミスは数秒で見つかります。入力ミスのあるウォレットアドレス (チェックサムが一致しない) や例として示されたアドレスは、何が誤っているかを示したうえで拒否されます。
- **Windows: 認証情報を尋ねるのは一度だけ:** 時間のかかる処理の前に、最初に一つのウィンドウで尋ねます。以前のバージョンで行ったインストールは、別の WSL ディストリビューション内のものも含めて検出され、**その場でアップグレード**されます (設定とモデルは残ります)。Microsoft から Ubuntu をダウンロードできない場合、セットアップは中国にあるミラーから Ubuntu 公式の WSL イメージを取得し、その SHA-256 が一致した場合にのみ使用します。
- **標準の Ubuntu では apt 不要:** Ubuntu 22.04 / 24.04 (WSL のものを含む) には、マイナーに必要なものがすべてそろっているため、何もインストールしません。不足しているものがある場合は、初回起動時にインストール前にその一覧を表示します。
- **Windows のドライブ上のパッケージ** (WSL での `/mnt/d` など) はすぐに拒否され、Linux のホームフォルダーにコピーしてそこで起動する、そのまま貼り付けられる一行が表示されます。
- **わかりやすいネットワークメッセージ:** WSL 内で DNS が機能しない場合、セットアップはその段階で止まり、対処方法を示します。インターネットやプールに接続できないマイナーは、何を確認すべきかを表示します。
- **モデルを再利用:** 新しいフォルダーへのアップグレードでは、以前のインストールのモデル (同じサイズと SHA-256) を見つけ、再ダウンロードしません。
- **`--status` が理由を表示:** マイナーが停止した理由と対処方法を、`--background` で起動した後も表示します。
- **HiveOS:** どの失敗でも、待ち時間を徐々に延ばしながら (一分から十分まで) 待機して再試行し、その理由をログとダッシュボードに表示するようになりました。

詳細なリリースノート: [`docs/releases/RELEASE-NOTES-v0.1.47.md`](../releases/RELEASE-NOTES-v0.1.47.md)。

## 既知の問題

- **Windows 上の 10 GB のカードでは、最高速度を出すために約 9.3 GB の空き VRAM が必要です。** それより少ないと、マイナーはより遅いレイアウトを使います (RTX 3080 10 GB では 21–22 から約 9 N/s に低下しました)。ハードウェアアクセラレーションを有効にしたブラウザーや Docker Desktop がそのメモリを使うことがあります。それらを閉じて (またはブラウザーのハードウェアアクセラレーションをオフにして)、マイニングを再開してください。
- **Windows の時計:** マイナーが時計のエラーを報告した場合は、Windows の 設定 › 時刻と言語 › 日付と時刻 を開き、「タイム ゾーンを自動的に設定する」と「時刻を自動的に設定する」をオンにして **今すぐ同期** をクリックし、`wsl --shutdown` を実行してから、もう一度起動してください。

## ハッシュレート

自社のリグで測定した、実際のプールでの最高ハッシュレートです。これらは**最大**値であり、保証ではありません。同じモデルのカードでも約 5–8% の差があり、カードでディスプレイも出力している場合や、他のプログラムが GPU を使用している場合はハッシュレートが下がります。

| GPU | ハッシュレート |
|---|---|
| RTX 5090 | 最大 ~99 N/s |
| RTX 4090 | 最大 ~70 N/s |
| RTX 3080 10 GB | 最大 ~22 N/s |

バージョンごとの日付入りの測定結果: [テスト結果](../TEST-RESULTS.md)。プロジェクトの主な節目: [開発記録](../DEVELOPMENT-RECORD.md)。

## 問題の報告

[GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) で issue を作成し、バグ報告フォームに記入してください。TheNewMiner のバージョン、システム、GPU の型番とドライバー、`./thenewminer --status` の出力、ログの最後の数行を記入します。issue は公開されます。**プールパスワードやウォレットの秘密鍵は決して投稿しないでください**。また、貼り付ける内容からはパスワード (必要であればウォレットアドレスも) を削除してください。

## セキュリティに関する注意

マイナーと GPU エンジンは、パッケージ内で暗号化された状態 (ChaCha20-Poly1305) で配布されます。マイナーは起動時に、あなたのマシン上でそれらを復号します。これによりコピーのコストは上がりますが、**破られないわけではありません**: マイナーを実行しているマシンの管理者権限を持つ人なら誰でも、十分な労力をかければ、鍵または復号されたコードを取り出すことができます。ウォレットアドレスとプールパスワードはあなた自身のマシンに保存され、プールにのみ送信されます。
