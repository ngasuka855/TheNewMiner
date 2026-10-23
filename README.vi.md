[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · **Tiếng Việt** · [한국어](README.ko.md) · [日本語](README.ja.md)

> Bản dịch này được cung cấp để tiện theo dõi; README tiếng Anh là bản chính thức. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner là phần mềm đào bằng GPU cho **TensorCash**. Phần mềm chạy trên card NVIDIA GeForce RTX dòng 30, 40 và 50 có
**từ 10 GB VRAM trở lên** và đào trên pool **`tsc-wss.the-new.win`**, vốn đã được tích hợp sẵn: bạn không cần nhập địa
chỉ pool, và nó kết nối trực tiếp.

Phiên bản hiện tại: **v0.1.46** · Trang web pool: [tsc.the-new.win](https://tsc.the-new.win/#/home)

## Trước khi bắt đầu: tạo tài khoản pool

Để đào, bạn cần một tài khoản pool: địa chỉ nhận thanh toán TSC (ví) của bạn và một mật khẩu pool do bạn tự chọn trên
trang web pool. Trang web có phiên bản tiếng Anh (dùng menu ngôn ngữ của trang).

1. Mở [tsc.the-new.win](https://tsc.the-new.win/#/home) và vào **Start mining** (开始挖矿).
2. Trong **1. Bind Wallet**, nhập **TSC payout address** của bạn (`tc1...`; hãy kiểm tra kỹ: thu nhập đào được gửi tới
   một địa chỉ sai sẽ không thể lấy lại), chọn một **Pool Password** (6–128 ký tự) và nhập lại ở **Confirm Password**.
3. Nhấp **Apply to join pool**. Trang sẽ xác nhận ví đã được liên kết; tài khoản dùng được ngay.
4. Dùng cùng ví và mật khẩu đó trong miner: `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'` trên Linux, các câu hỏi của
   `TheNewMiner.bat` trên Windows, hoặc flight sheet trên HiveOS. Khoảng 3–5 phút sau khi khởi động, rig của bạn sẽ xuất
   hiện trong **My Miners** trên trang web pool.

Pool không thể khôi phục mật khẩu pool bị quên, và địa chỉ nhận thanh toán không thể thay đổi về sau. Mật khẩu pool
không phải là khóa riêng (private key) của ví: tuyệt đối không đưa khóa riêng cho bất kỳ ai.

## Có gì mới trong v0.1.46

- **Ít lỗi hơn ở lần khởi động đầu tiên trên card dòng RTX 30 và HiveOS.** Bước kiểm tra GPU một lần giờ dùng mã GPU
  được biên dịch sẵn cho mọi dòng card được hỗ trợ và đã chạy một lần trong lúc cài đặt; bộ nhớ đệm biên dịch GPU được
  giữ lại giữa các lần khởi động và nâng cấp. Nếu bước kiểm tra vẫn quá thời gian chờ trên một card dòng RTX 30, miner
  sẽ làm bước đó bằng CPU và tiếp tục đào. Nếu không thể chuẩn bị cấu hình cho card, miner báo lý do trong một dòng
  `Reason:` (mã thoát 78).
- **HiveOS:** khi miner không thể chuẩn bị cấu hình cho GPU ở lần khởi động đầu tiên (mã thoát 78), rig giờ sẽ chờ và
  thử lại với khoảng nghỉ tăng dần (từ một đến mười phút) thay vì khởi động lại sau mỗi vài giây.
- **Các wave mượt hơn:** khi checkpoint của pool thay đổi giữa chừng một wave, công việc của wave tiếp theo giờ được
  chuẩn bị lại ở chế độ nền. Trên RTX 5090, điều này loại bỏ các lần chờ vào thời điểm đó (+0.3% hashrate duy trì, kết
  quả giống hệt).
- **Không còn gì sót lại trong bộ nhớ sau khi mật khẩu bị từ chối:** `--status` và `--logs` sau khi dừng không còn để
  lại các tệp chương trình đã giải nén (khoảng 1.5 GB) trong bộ nhớ.
- **Đã có sẵn model?** Hãy sao chép nó thay vì tải lại: xem mục bên dưới.

Các thay đổi của v0.1.45 nằm trong [`RELEASE-NOTES-v0.1.45.md`](RELEASE-NOTES-v0.1.45.md).

## Tải về

| Tệp | Dành cho | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.46-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/TheNewMiner-v0.1.46-linux-x86_64-sealed.tar.xz) | Linux x86_64 | `3585360e80993176475b6e400258888b4ddf8cb130ff97574e74e15cf1e248a8` |
| [`TheNewMiner-v0.1.46-windows-wsl2-r1.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/TheNewMiner-v0.1.46-windows-wsl2-r1.zip) | Windows 10 / 11 (chạy thông qua WSL2) | `dd9cb395e4012bfb81a5b9f0a1fdfd196955b781265a4a4e0d67266bd67b6a9e` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/SHA256SUMS) | mã kiểm tra (checksum) của tất cả các gói | |

Tệp zip cho Windows là gói tất cả trong một: nó đã chứa sẵn miner Linux, nên bạn chỉ cần tệp zip.

## Xác minh tệp đã tải

Đặt (các) gói và `SHA256SUMS` vào cùng một thư mục, sau đó:

```bash
# Linux (--ignore-missing: chỉ kiểm tra các gói bạn đã tải về)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Dấu nhắc Lệnh): so sánh kết quả với SHA-256 trong bảng ở trên
certutil -hashfile TheNewMiner-v0.1.46-windows-wsl2-r1.zip SHA256
```

Mỗi tệp phải báo `OK` (trên Windows: đúng 64 ký tự giống như trong bảng). Nếu không khớp, đừng sử dụng tệp đó.

## Cài đặt một cú nhấp (Linux)

Hãy tạo tài khoản pool trước (xem "Trước khi bắt đầu" ở trên). Sau đó thay `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` và
`<RIG_NAME>` (bao gồm cả dấu `<` `>`) ở cuối lệnh này bằng ví của bạn, mật khẩu pool của bạn và một tên cho rig này, rồi
dán lệnh vào terminal. Lệnh sẽ tải gói Linux và `SHA256SUMS` của phiên bản này (bằng `curl`, hoặc `wget` nếu không có
`curl`), kiểm tra SHA-256, giải nén miner vào `~/thenewminer` và khởi động nó ở chế độ nền:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.46 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Lệnh dừng lại ở bước đầu tiên bị lỗi: lỗi tải về hoặc SHA-256 không khớp sẽ không bao giờ khởi động thứ gì (việc kiểm
tra này bảo vệ khỏi tệp tải về bị hỏng; cả hai tệp đều đến từ trang này). Hãy giữ dấu nháy đơn quanh mật khẩu: bên trong
chúng mọi ký tự đều an toàn (dấu `'` trong mật khẩu được viết là `'\''`). Nếu `tar` báo `xz: Cannot exec`, hãy cài xz
(`sudo apt-get install -y xz-utils`) và chạy lại lệnh.

Để nâng cấp một phiên bản đã có trong `~/thenewminer`, trước tiên chạy `~/thenewminer/thenewminer --stop`, sau đó chạy
lệnh này (cài đặt và model của bạn được giữ lại). Nếu phiên bản trước của bạn nằm ở thư mục khác, hãy dừng nó ở đó
trước, hoặc nâng cấp nó ngay tại đó như hướng dẫn trong mục Linux (nếu không model sẽ bị tải lại).

## Linux

```bash
tar -xJf TheNewMiner-v0.1.46-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Lệnh | Chức năng |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | Lần khởi động đầu tiên: cài đặt những gì cần thiết, sau đó đào ở chế độ nền |
| `./thenewminer --background` | Các lần khởi động sau: cài đặt của bạn đã được ghi nhớ |
| `./thenewminer` | Đào ở chế độ hiển thị (foreground) (Ctrl+C để dừng) |
| `./thenewminer --logs` | Theo dõi miner (Ctrl+C thoát khỏi màn hình theo dõi; việc đào vẫn tiếp tục) |
| `./thenewminer --status` | Miner có đang đào không, hashrate gần nhất và số share được chấp nhận / bị từ chối |
| `./thenewminer --stop` | Dừng đào |
| `./thenewminer --help` | Tất cả các tùy chọn |

- `-u` là địa chỉ ví TensorCash của bạn, `-p` là mật khẩu pool của bạn, `-w` là tên worker hiển thị trên pool
  (mặc định: tên máy (host name) của máy tính; 1–64 ký tự gồm chữ cái, chữ số, `_`, `-` hoặc `.`).
- Mật khẩu pool: 6–128 ký tự, mọi ký tự hiển thị được, không có dấu cách. Luôn đặt nó trong dấu nháy đơn trên dòng
  lệnh, ví dụ `-p 'my$pass!'` (dấu `'` bên trong dấu nháy đơn được viết là `'\''`).
- Cài đặt được ghi nhớ trong `thenewminer/.miner/` (tệp chứa mật khẩu chỉ bạn mới đọc được). Ví và mật khẩu được gỡ khỏi
  dòng lệnh ngay lập tức, nên chúng không hiển thị trong `ps` (shell của bạn vẫn có thể lưu lệnh trong lịch sử).
- Nếu pool từ chối ví hoặc mật khẩu, miner sẽ báo điều đó và dừng lại. Hãy kiểm tra chúng và
  khởi động lại với `-u` và `-p`.
- Lần khởi động đầu tiên sẽ tải model (khoảng 8.1 GiB, chỉ một lần), cài đặt GPU engine cho card của bạn và chạy tự
  kiểm tra trên card, sau đó bắt đầu đào. Các dòng như `[ RATE] ... N/s` nghĩa là miner đang đào.
- Trên Ubuntu và Debian, lần khởi động đầu tiên có thể cài một vài thư viện hệ thống còn thiếu bằng `apt` (cần sudo hoặc
  root); trên các hệ thống khác, miner sẽ liệt kê tên chúng để bạn tự cài.
- Rig nhiều GPU: được hỗ trợ, mỗi GPU đều đào; bản phát hành này được thử nghiệm với một GPU trên mỗi máy.
- Nếu `tar` báo `xz: Cannot exec`, hãy cài xz trước: `sudo apt-get install -y xz-utils`.
- Trong Docker hoặc các container khác có ít bộ nhớ dùng chung, miner chạy các tệp chương trình của nó từ
  `thenewminer/.miner/run/` trên ổ đĩa và thông báo điều đó. Để chọn thư mục khác, hãy khởi động bằng
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (một thư mục chỉ bạn mới ghi được: `chmod 700 <folder>`).
- **Nâng cấp:** `./thenewminer --stop`, giải nén gói mới vào cùng vị trí (gói sẽ giải nén vào `thenewminer/` và
  giữ lại `thenewminer/.miner/` cùng cài đặt và model của bạn), sau đó khởi động lại. Nếu phiên bản cũ vẫn đang
  chạy, phiên bản mới sẽ từ chối khởi động và yêu cầu bạn dừng phiên bản cũ trước.

## Windows (một cú nhấp)

1. Giải nén toàn bộ `TheNewMiner-v0.1.46-windows-wsl2-r1.zip` (nhấp chuột phải › Trích xuất Tất cả) vào bất kỳ đâu:
   thư mục nào, ổ đĩa nào cũng được; đường dẫn có dấu cách hoặc ký tự tiếng Trung đều không sao. Đừng chạy nó từ bên
   trong cửa sổ xem tệp zip.
2. Nhấp đúp vào **`TheNewMiner.bat`**.
   - **Lần đầu tiên:** nó kiểm tra Windows, driver và ổ đĩa, bật WSL2 nếu cần (một lần hỏi quyền quản trị viên;
     có thể cần khởi động lại máy một lần, sau đó quá trình cài đặt tự tiếp tục), cho biết dữ liệu sẽ được lưu ở đâu
     (Enter để chấp nhận), tạo môi trường Linux (WSL) riêng, cài miner đi kèm, hỏi **một lần** địa chỉ ví của bạn
     (bắt đầu bằng `tc1`), mật khẩu pool (nhập hai lần; 6–128 ký tự, mọi ký tự hiển thị được, không có dấu cách) và
     tên worker (Enter = tên máy tính), tải model, sau đó bắt đầu đào.
   - **Mỗi lần sau đó:** nhấp đúp là bắt đầu đào ngay với cài đặt đã lưu. Một lối tắt "TheNewMiner" trên màn hình
     nền cũng được tạo. Nếu nhấp đúp lần nữa khi miner đang đào, nó sẽ báo cho bạn biết và hiển thị log.

Các lệnh dùng hằng ngày (mở Dấu nhắc Lệnh trong thư mục đã giải nén):

| Lệnh | Chức năng |
|---|---|
| `TheNewMiner.bat` | Cài đặt nếu cần, sau đó đào trong cửa sổ này (đóng cửa sổ sẽ dừng đào) |
| `TheNewMiner.bat background` | Đào mà không có cửa sổ |
| `TheNewMiner.bat logs` | Theo dõi miner (Ctrl+C thoát khỏi màn hình theo dõi; việc đào vẫn tiếp tục) |
| `TheNewMiner.bat status` | Miner có đang chạy không, và hashrate gần nhất |
| `TheNewMiner.bat stop` | Dừng đào |
| `TheNewMiner.bat setup` | Nhập lại ví / mật khẩu / tên worker |
| `TheNewMiner.bat uninstall` | Gỡ miner, thư mục dữ liệu của nó và lối tắt (có hỏi trước) |

- **Dữ liệu được lưu ở đâu:** miner và model nằm trong một thư mục dữ liệu được chọn ở lần cài đặt đầu tiên (mặc định
  là `TheNewMinerData` ở thư mục gốc của ổ đĩa mà bạn đã giải nén vào, ví dụ `D:\TheNewMinerData`), không nằm trong
  thư mục đã giải nén. Cần khoảng 20 GB dung lượng trống ở đó.
- Nếu pool từ chối ví hoặc mật khẩu, cửa sổ sẽ báo điều đó và việc đào dừng lại: hãy chạy `TheNewMiner.bat setup`
  để nhập lại chúng.
- **Nâng cấp:** giải nén phiên bản mới vào bất kỳ đâu và nhấp đúp vào `TheNewMiner.bat` của nó. Nó sẽ tìm miner đã cài,
  giữ lại cài đặt và model của bạn, và cài phiên bản mới mà không hỏi gì. Có thể xóa thư mục cũ.
- **Gỡ cài đặt:** `TheNewMiner.bat uninstall`.
- Ngôn ngữ hiển thị trên màn hình theo ngôn ngữ của Windows (English / 简体中文 / 繁體中文). Để tự chọn, thêm `-Lang en`
  (hoặc `zh-Hans`, `zh-Hant`) vào bất kỳ lệnh nào, ví dụ `TheNewMiner.bat -Lang en`; tùy chọn này áp dụng cho lần chạy đó.

## HiveOS (thử nghiệm)

Hỗ trợ HiveOS đang ở giai đoạn **thử nghiệm** (từ v0.1.45, phiên bản đã chạy trên một rig HiveOS thật; các thay đổi về
lần khởi động đầu tiên của v0.1.46 chưa được chạy trên rig HiveOS nào). Nó dùng gói custom miner cho HiveOS dưới đây.

| Tệp | SHA-256 |
|---|---|
| [`thenewminer-0.1.46.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/thenewminer-0.1.46.tar.gz) | `2284c258176e6b372bf56947a343116688e773dee2d479a7f5f125ad9f0a1915` |

Thiết lập nhanh (nhập flight sheet):

1. **Wallets** (ví): thêm một ví với coin `TSC` và địa chỉ TensorCash của bạn (`tc1...`). Việc này chỉ cần làm một lần:
   HiveOS cần một ví `TSC` được chọn, còn miner dùng địa chỉ bạn điền trong flight sheet.
2. Mở [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json) và sao chép nội dung của nó. Thay
   `<YOUR_WALLET>` bằng địa chỉ ví của bạn (giữ `.%WORKER_NAME%` ở phía sau) và `<YOUR_POOL_PASSWORD>` bằng mật khẩu
   pool của bạn (trong JSON, ký tự `"` trong mật khẩu được viết là `\"` và ký tự `\` được viết là `\\`). Trong HiveOS
   chọn Flight Sheets › **Import from clipboard**, dán nội dung vào, chọn ví `TSC` của bạn rồi lưu.
3. Áp dụng flight sheet cho rig của bạn. Rig sẽ tải gói về và bắt đầu đào.

Hoặc thiết lập thủ công: tạo một flight sheet với Coin `TSC`, Wallet: ví `TSC` của bạn, Pool: "Configure in miner",
Miner: "Custom", nhấp **Setup Miner Config** và điền:

| Trường | Giá trị |
|---|---|
| Miner name | `thenewminer` (tự điền từ installation URL) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/thenewminer-0.1.46.tar.gz` |
| Hash algorithm | để trống |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (bị bỏ qua: pool đã được tích hợp sẵn) |
| Pass | mật khẩu pool của bạn |
| Extra config arguments | để trống, hoặc `--proxy <YOUR_PROXY>` (xem mục Proxy) |

- Tên worker trên pool là tên của rig trong HiveOS: chỉ dùng chữ cái, chữ số, `_`, `-` hoặc `.` (không có dấu cách), tối
  đa 64 ký tự. Để dùng tên worker khác, thêm `-w <RIG_NAME>` vào trường Extra config arguments.
- Cần khoảng 12 GB trống trên ổ đĩa HiveOS cho model và miner (kiểm tra bằng `df -h /hive/miners`).
- Model (khoảng 8.1 GiB) được tải một lần vào `/hive/miners/custom/.thenewminer-models` và được giữ lại khi bạn nâng cấp.
- Ví và mật khẩu được chuyển tới miner qua biến môi trường của nó, không qua dòng lệnh.
- Nếu pool từ chối ví hoặc mật khẩu, log của miner sẽ báo điều đó và miner chờ 10 phút trước khi HiveOS khởi động lại
  nó: hãy sửa flight sheet và áp dụng lại (việc này khởi động lại miner ngay lập tức).
- Log: màn hình miner trong HiveOS (`miner` trong shell của rig) hoặc `/var/log/miner/custom/custom.log`.
- **Nâng cấp:** đổi cả hai số phiên bản trong installation URL (ví dụ `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`) và áp
  dụng flight sheet. HiveOS chỉ cài đặt khi tên tệp trong URL thay đổi.
- **Lần khởi động đầu tiên** tải model về và cài engine GPU, việc này mất vài phút. Nếu sau đó miner không thể chuẩn bị
  cấu hình cho GPU, log sẽ ghi lý do trong một dòng `Reason:` và HiveOS thử lại với khoảng nghỉ tăng dần (từ một đến
  mười phút).
- **Đã có sẵn model?** Sao chép nó vào `/home/user/` trên rig trước lần khởi động đầu tiên (xem mục tiếp theo).

**Nếu rig không truy cập được GitHub:** tải `thenewminer-0.1.46.tar.gz` về một máy PC (kiểm tra SHA-256 của nó trong
bảng ở trên), sao chép sang rig và giải nén vào nơi HiveOS lưu các miner tùy chỉnh. Sau đó áp dụng flight sheet như bình
thường: HiveOS báo "Already installed" và không tải lại. Giữ chính xác tên tệp `thenewminer-0.1.46.tar.gz` (trình duyệt
có thể thêm " (1)") và không đổi installation URL trong flight sheet: HiveOS so sánh tên đó.

```bash
# trên PC
scp thenewminer-0.1.46.tar.gz user@<RIG_IP>:/tmp/
# trên rig, với quyền root (Hive Shell, hoặc SSH rồi sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.46.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.46.tar.gz
```

## Đã có sẵn model? Hãy sao chép nó

Miner cần một tệp model 8.1 GiB: `Qwen3-8B-Q8_0-full-v2.gguf` (`8709518112` byte, SHA-256
`408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`). Nếu tệp này đã có trên máy này, hoặc trên một máy
tính khác trong mạng của bạn, hãy sao chép nó thay vì tải lại.

Trước khi tải bất cứ thứ gì, miner tìm tệp theo đúng tên và kích thước: trong thư mục model của chính nó, trong các thư
mục TheNewMiner trước đây, trong thư mục home của bạn (và một hoặc hai cấp thư mục bên dưới, hoặc một thư mục `models` ở
đó), trong các thư mục home bên dưới `/home`, `/data`, `/mnt/*`, `/media/*`, `/opt`, `/srv`, `/workspace`,
`/var/lib`, và trong bộ nhớ đệm của Hugging Face / ModelScope. Nếu không tìm thấy ở đó, nó dành tối đa 20 giây tìm trong
những thư mục đó trên cùng ổ đĩa (các ổ đĩa khác đang được gắn chỉ được kiểm tra theo tên) bất kỳ tệp `.gguf` nào có
đúng kích thước đó. Tệp tìm được **chỉ được dùng nếu SHA-256 của nó khớp**; sau đó miner tạo liên kết tới tệp (nếu cùng
ổ đĩa) hoặc sao chép nó vào thư mục model của chính mình, để mọi phiên bản sau tìm thấy nó ngay. Bạn cũng có thể chỉ
thẳng tới tệp: `./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`.

**Linux:** sao chép tệp vào thư mục home của bạn, rồi khởi động miner như thường lệ. Từ một máy Mac hoặc PC trong cùng
mạng (Windows 10 và 11 có `scp` trong PowerShell), hãy thay `user` và `<MINER_IP>` bằng tên đăng nhập và địa chỉ của máy
đào:

```bash
# ngay trên chính máy miner
cp /path/to/Qwen3-8B-Q8_0-full-v2.gguf ~/
# hoặc từ một máy tính khác
scp Qwen3-8B-Q8_0-full-v2.gguf user@<MINER_IP>:~/
```

**HiveOS:** sao chép tệp vào `/home/user/` của rig (tên đăng nhập SSH của HiveOS là `user`), rồi áp dụng flight sheet.
Miner tìm thấy tệp ở đó, kiểm tra nó và giữ nó bên cạnh gói (`/hive/miners/custom/.thenewminer-models`), nên phiên bản
mới sẽ không bao giờ tải lại. Ổ đĩa của HiveOS nhỏ: hãy chạy `disk-expand` trước nếu `df -h /hive/miners` cho thấy còn
trống ít hơn 12 GB.

```bash
scp Qwen3-8B-Q8_0-full-v2.gguf user@<RIG_IP>:/home/user/
```

**Windows:** sau khi lần `setup` đầu tiên đã tạo bản phân phối WSL của TheNewMiner, hãy sao chép tệp vào đó từ Dấu nhắc
Lệnh (đổi `D:\Models` thành nơi chứa bản sao của bạn). Nếu việc tải xuống đã bắt đầu, hãy đóng cửa sổ, chạy các lệnh
sau, rồi khởi động lại TheNewMiner: nó sẽ tìm thấy bản sao và bỏ qua bước tải xuống. Việc sao chép mất vài phút và không
hiển thị tiến trình.

```bat
wsl -d TheNewMiner -u root -- mkdir -p ~/thenewminer/models
wsl -d TheNewMiner -u root -- cp /mnt/d/Models/Qwen3-8B-Q8_0-full-v2.gguf ~/thenewminer/models/
```

## Đổi ví, mật khẩu pool hoặc tên worker

Pool gắn vĩnh viễn mỗi địa chỉ nhận thanh toán với tài khoản pool riêng của nó: trang pool
[tsc.the-new.win](https://tsc.the-new.win) ghi địa chỉ nhận thanh toán TSC là "永久绑定 · 不可修改" (gắn vĩnh viễn, không thể
sửa đổi) và hướng dẫn rằng muốn đổi thì dùng trực tiếp địa chỉ mới trên miner. Vì vậy, một ví khác cần có tài khoản pool
và mật khẩu pool riêng (hãy tạo chúng trên trang pool trước). Nếu bạn đổi mật khẩu pool trên trang pool, hãy cập nhật
mật khẩu đó trên mọi miner. Pool không thể khôi phục mật khẩu đã quên.

**Linux:** dừng miner, rồi khởi động nó một lần với cài đặt mới; cài đặt mới sẽ thay thế cài đặt đã được ghi nhớ:

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

Hãy đưa `-u` và `-p` cùng lúc: chỉ đưa một trong hai thì ví và mật khẩu đã được ghi nhớ sẽ không thay đổi. Tên worker có
thể đổi riêng: `./thenewminer -w <RIG_NAME> --background` (sau `--stop`).

**Windows:** chạy `TheNewMiner.bat setup`. Lệnh này dừng miner đang chạy và hỏi lại ví, mật khẩu pool và tên worker. Sau
đó khởi động như thường lệ (nhấp đúp vào `TheNewMiner.bat`).

**HiveOS:** sửa flight sheet: điền địa chỉ ví mới vào "Wallet and worker template" (`<NEW_WALLET>.%WORKER_NAME%`) và mật
khẩu pool mới vào Pass, rồi lưu và áp dụng. Tên worker là tên của rig trong HiveOS (hoặc `-w <RIG_NAME>` trong Extra
config arguments).

## Proxy (tùy chọn)

Miner kết nối trực tiếp tới pool. Nếu mạng của bạn cần proxy:

```bash
# Linux (được ghi nhớ cho đến khi thay đổi)
./thenewminer --proxy <IP>[:<PORT>]            # TCP relay (cổng 443 nếu bỏ trống)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5, cũng có thể dùng socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # proxy HTTP CONNECT
./thenewminer --proxy none                     # quay lại kết nối trực tiếp
```

Các lệnh này cũng khởi động miner: hãy dừng miner trước, và thêm `--background` để miner tiếp tục chạy nền.

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

TLS luôn được mã hóa đầu cuối (end to end) với pool, nên proxy không thể đọc được kết nối. Trên Windows, ứng dụng proxy
chạy trên cùng PC thường không truy cập được qua `127.0.0.1` từ WSL (với chế độ mạng WSL mặc định): hãy dùng IP LAN của
PC và cho phép kết nối LAN trong ứng dụng proxy.

## Yêu cầu

| Hạng mục | Yêu cầu |
|---|---|
| GPU | NVIDIA GeForce RTX dòng 30 / 40 / 50, từ 10 GB VRAM trở lên. Rig nhiều GPU: được hỗ trợ, mỗi GPU đều đào; bản phát hành này được thử nghiệm với một GPU trên mỗi máy |
| Driver | NVIDIA **580 trở lên** (`nvidia-smi` phải hoạt động). Trên Windows, cài driver trên Windows, tuyệt đối không cài trong WSL |
| Linux | x86_64 với **glibc 2.34 trở lên**, ví dụ Ubuntu 22.04 trở lên hoặc Debian 12 trở lên. Trình cài đặt kiểm tra những gì nó cần và liệt kê tên mọi thứ còn thiếu |
| Windows | Windows 10 21H2 trở lên, hoặc Windows 11, 64-bit; bật ảo hóa (Intel VT-x / AMD SVM) trong BIOS |
| Ổ đĩa | khoảng 20 GB trống |
| RAM | từ 4 GB trở lên |
| Model | khoảng 8.1 GiB, tải một lần ở lần khởi động đầu tiên (song song, nhiều mirror) và dùng lại về sau |

## Cập nhật driver NVIDIA (580 trở lên)

Miner cần driver NVIDIA **580 trở lên**. Hãy kiểm tra phiên bản bạn đang dùng:

```bash
nvidia-smi    # dòng tiêu đề hiển thị "Driver Version: ..."; phải là 580 trở lên
```

- **Ubuntu:** `sudo ubuntu-drivers install` sẽ cài driver được khuyến nghị (sau đó hãy kiểm tra rằng đó là bản 580 trở lên). Để tự chọn phiên bản:
  `sudo apt install nvidia-driver-580` (hoặc mới hơn; card dòng RTX 50 cần bản open, ví dụ
  `nvidia-driver-580-open`). Sau đó khởi động lại máy. Chỉ dùng gói driver của Ubuntu hoặc trình cài đặt `.run` của
  NVIDIA, tuyệt đối không dùng cả hai.
- **Debian:** các gói driver có sẵn của Debian cũ hơn 580 (535 trong Debian 12, 550 trong Debian 13). Hãy cài driver mới
  hơn từ kho của NVIDIA theo mô tả trong [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html) (hướng dẫn cài
  đặt driver NVIDIA), sau đó khởi động lại máy.
- **Windows:** cập nhật bằng [NVIDIA app](https://www.nvidia.com/en-us/software/nvidia-app/) (trước đây là GeForce
  Experience), hoặc tải driver cho card của bạn từ [nvidia.com](https://www.nvidia.com/en-us/drivers/) (580 trở lên).
  WSL2 dùng chính driver Windows này: tuyệt đối không cài driver NVIDIA cho Linux bên trong WSL.
- **HiveOS:** với quyền root (Hive Shell, hoặc SSH rồi `sudo -i`), chạy `nvidia-driver-update --list` để xem các phiên
  bản, sau đó chạy ví dụ `nvidia-driver-update 595` (bất kỳ bản nào từ 580 trở lên), rồi chạy `sreboot`.

## Hashrate tham khảo

Hashrate tốt nhất đo được trên pool thật với các rig của chính chúng tôi (từ 2026-10-14 đến 2026-10-23). Đây là các
con số **tối đa**, không phải lời cam kết: các card cùng model chênh lệch nhau khoảng 5–8% tùy theo từng con chip, tản
nhiệt, xung nhịp và giới hạn công suất, và hashrate sẽ giảm nếu card đồng thời xuất hình ra màn hình hoặc có chương
trình khác sử dụng GPU.

| GPU | Hashrate |
|---|---|
| RTX 5090 | tối đa ~99 N/s |
| RTX 4090 | tối đa ~70 N/s |
| RTX 3080 10 GB | tối đa ~22 N/s |

## Kết quả thử nghiệm

Thử nghiệm trên pool thật với các rig thử nghiệm của chúng tôi (một GPU mỗi máy). A = share được chấp nhận, R = share bị
từ chối. "Đào ổn định" là hashrate trung vị giữa các lần đổi job.

| Ngày | Card | Hệ điều hành | Gói | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 5090 | Linux | bản build phát hành v0.1.46 | 95.41 N/s (đào ổn định, 12 phút; 91.70 khi tính cả các lần đổi job) | 10 / 0 (+1 stale; 0 không hợp lệ) |
| 2026-10-23 | RTX 4090 | Linux | bản build phát hành v0.1.46 | 66.23 N/s (trung vị, 10 phút) | 2 / 0 |
| 2026-10-23 | RTX 4090 | Linux | bản build phát hành v0.1.45 | 65.42 N/s (đào ổn định, 16 phút; 62.35 khi tính cả các lần đổi job) | 13 / 0 (0 không hợp lệ) |
| 2026-10-23 | RTX 5090 | Linux | bản build phát hành v0.1.45 | 94.18 N/s (đào ổn định; 92.24 khi tính cả các lần đổi job) | 17 / 0 (0 không hợp lệ) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.44 sealed-r4 | 65.58 N/s (trung vị, 16 phút) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | v0.1.44 sealed-r4 | 93.45 N/s (đào ổn định, 44 phút; 90.39 khi tính cả các lần đổi job) | 31 / 0 (+1 stale lúc dừng) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | v0.1.44 windows-wsl2-r5 | ~22 N/s (lần chạy riêng của nhà vận hành) | không có |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | v0.1.44 sealed-r4 | 21.04 N/s (10 phút cuối của 23 phút đào) | 3 / 0 |

RTX 3080 trên Linux (v0.1.44): cài đặt 6/6 bước OK, không có cửa sổ (window) không hợp lệ nào, dừng sạch. Trích đoạn log:
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt).

## Ảnh chụp màn hình

Từ các rig thử nghiệm của chúng tôi, với các phiên bản trước: v0.1.41 trên RTX 4090 và một bản build sớm trên RTX 5090.
Đây là các số đo trực tiếp; con số của bạn phụ thuộc vào card.

**RTX 4090, v0.1.41:** 69–75 N/s mỗi lần đo, phần lớn ~70 (trung bình 10 phút ~68), 9 share được chấp nhận, 0 bị từ
chối, ~445 W.

![TheNewMiner v0.1.41 đang đào trên RTX 4090](screenshots/rtx4090.png)

**RTX 5090, bản build sớm:** 100.83 N/s (trung bình trực tiếp trên các window đã hoàn tất), ~575 W.

![Một bản build sớm của TheNewMiner đang đào trên RTX 5090](screenshots/rtx5090.png)

## Câu hỏi thường gặp

**Windows báo "Windows đã bảo vệ PC của bạn" / "Nhà phát hành không xác định".**
Bộ cài không được ký mã (code-signed). Nhấp **Thêm thông tin**, sau đó nhấp **Vẫn chạy**.

**Phần mềm diệt virus của tôi gắn cờ hoặc xóa miner.**
Microsoft Defender và các phần mềm diệt virus khác thường gắn cờ phần mềm đào tiền mã hóa là "ứng dụng có thể không
mong muốn", kể cả một miner hợp lệ do chính bạn chọn chạy, vì phần mềm độc hại thường ẩn miner bên trong. Nếu một tệp bị
cách ly, hãy mở Bảo mật Windows › Bảo vệ khỏi vi-rút & mối đe dọa › Lịch sử bảo vệ, chọn mục TheNewMiner và chọn
**Cho phép** (hoặc thêm thư mục đã giải nén vào Loại trừ). Chỉ làm vậy với gói được tải từ trang này và có SHA-256 khớp.

**Lần khởi động đầu tiên mất nhiều thời gian.**
Lần khởi động đầu tiên tải model ~8.1 GiB và chạy tự kiểm tra GPU trên card của bạn; trên Windows nó cũng có thể thiết
lập WSL2. Việc này chỉ xảy ra một lần. Các lần khởi động sau sẽ bắt đầu đào sớm hơn nhiều.

**Miner báo pool đã từ chối ví / mật khẩu.**
Hãy kiểm tra địa chỉ ví và mật khẩu pool (tài khoản phải tồn tại trên trang web của pool), sau đó khởi động lại:
Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. Miner không tự thử
lại với thông tin đăng nhập sai.

**Miner có đang đào không? Log ở đâu?**
Linux: `./thenewminer --status`, `./thenewminer --logs`, hoặc tệp `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat status` và `TheNewMiner.bat logs`.

## Báo cáo sự cố

Mở một issue trên [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) và điền vào biểu mẫu báo
lỗi: phiên bản TheNewMiner, hệ thống của bạn, model GPU và driver, kết quả của `./thenewminer --status` và các dòng log
cuối cùng. Issue là công khai: **tuyệt đối không đăng mật khẩu pool hoặc khóa riêng của ví**, và hãy xóa mật khẩu (và,
nếu muốn, cả địa chỉ ví) khỏi mọi nội dung bạn dán.

## Lưu ý bảo mật

Miner và GPU engine được đóng gói ở dạng mã hóa (ChaCha20-Poly1305) bên trong gói; miner giải mã chúng trên máy của bạn
khi khởi động. Điều này làm tăng chi phí sao chép chúng, nhưng **không phải là không thể phá vỡ**: bất kỳ ai có quyền
quản trị viên trên một máy đang chạy miner, nếu đủ công sức, đều có thể lấy được khóa hoặc mã đã giải mã.
Địa chỉ ví và mật khẩu pool của bạn được lưu trên chính máy của bạn và chỉ được gửi tới pool.
