[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · **Tiếng Việt** · [한국어](README.ko.md) · [日本語](README.ja.md)

> Bản dịch này được cung cấp để tiện theo dõi; README tiếng Anh là bản chính thức. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner là phần mềm đào bằng GPU cho **TensorCash**. Phần mềm chạy trên card NVIDIA GeForce RTX dòng 30, 40 và 50 có
**từ 10 GB VRAM trở lên** và đào trên pool **`tsc-wss.the-new.win`**, vốn đã được tích hợp sẵn: bạn không cần nhập địa
chỉ pool, và nó kết nối trực tiếp.

Phiên bản hiện tại: **v0.1.45**

## Có gì mới trong v0.1.45

- **Thông báo rõ ràng khi pool từ chối ví hoặc mật khẩu của bạn.** Miner báo điều đó trong một dòng và dừng lại thay vì
  thử lại; `--status` cho biết lý do. Hãy kiểm tra ví và mật khẩu, sau đó khởi động lại với `-u` và `-p`.
- **Ví và mật khẩu của bạn không còn hiển thị trong `ps`** hay trong danh sách tiến trình của người dùng khác.
- **`./thenewminer --status`** cho biết miner có đang đào không, đào từ khi nào, hashrate gần nhất và số share được chấp
  nhận / bị từ chối.
- **Hỗ trợ HiveOS (thử nghiệm):** một gói custom miner cho HiveOS và một flight sheet có thể nhập vào (xem mục HiveOS bên dưới).
- **Nâng cấp an toàn:** phiên bản mới từ chối khởi động khi phiên bản cũ vẫn đang đào (nó sẽ nhắc bạn dừng phiên bản cũ
  trước); sau `--stop` nó tự cài lại các tệp chương trình của mình và giữ nguyên cài đặt cùng model của bạn.
- **`--background` không còn báo "đã khởi động" khi miner dừng ngay lập tức**; thay vào đó nó hiển thị lý do.
- **Docker và bộ nhớ dùng chung nhỏ:** khi thư mục trong bộ nhớ quá nhỏ (mặc định của Docker là 64 MiB) hoặc không cho
  phép chạy chương trình, miner dùng `thenewminer/.miner/run/` trên ổ đĩa thay thế và thông báo điều đó.
- **Mật khẩu pool giờ có thể dùng mọi ký tự hiển thị được** (kể cả dấu câu và chữ cái không phải tiếng Anh): 6–128 ký
  tự, không có dấu cách.
- Hỗ trợ thêm nhiều hệ thống Linux: trình cài đặt giờ kiểm tra những gì nó cần (glibc 2.34 trở lên) thay vì chỉ chấp
  nhận một vài phiên bản Ubuntu.

## Tải về

| Tệp | Dành cho | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz) | Linux x86_64 | `6281bf489fae843551114f09ead5498d3e47af33a07e4cfdfbdc0a5d6bdbafbb` |
| [`TheNewMiner-v0.1.45-windows-wsl2-r1.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-windows-wsl2-r1.zip) | Windows 10 / 11 (chạy thông qua WSL2) | `90732441b0dc67de84809d9bf3bda4441c0bf7cd9a52fefb95d29242ad7b15e7` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/SHA256SUMS) | mã kiểm tra (checksum) của tất cả các gói | |

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
certutil -hashfile TheNewMiner-v0.1.45-windows-wsl2-r1.zip SHA256
```

Mỗi tệp phải báo `OK` (trên Windows: đúng 64 ký tự giống như trong bảng). Nếu không khớp, đừng sử dụng tệp đó.

## Cài đặt một cú nhấp (Linux)

Thay `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` và `<RIG_NAME>` (bao gồm cả dấu `<` `>`) ở cuối lệnh này, rồi dán lệnh vào
terminal. Lệnh sẽ tải gói Linux và `SHA256SUMS` của phiên bản này (bằng `curl`, hoặc `wget` nếu không có `curl`), kiểm
tra SHA-256, giải nén miner vào `~/thenewminer` và khởi động nó ở chế độ nền:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.45 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
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
tar -xJf TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz
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
- Nếu pool từ chối ví hoặc mật khẩu (hoặc chưa duyệt tài khoản), miner sẽ báo điều đó và dừng lại. Hãy kiểm tra chúng và
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

1. Giải nén toàn bộ `TheNewMiner-v0.1.45-windows-wsl2-r1.zip` (nhấp chuột phải › Trích xuất Tất cả) vào bất kỳ đâu:
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

Hỗ trợ HiveOS là tính năng mới trong v0.1.45 và đang ở giai đoạn **thử nghiệm**. Nó dùng gói custom miner cho HiveOS
dưới đây.

| Tệp | SHA-256 |
|---|---|
| [`thenewminer-0.1.45.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz) | `90576efd388ceeca4e456b62b568f267465320913a07c9f9736f42363b1701dc` |

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
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz` |
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
- **Nâng cấp:** đổi cả hai số phiên bản trong installation URL (ví dụ `.../raw/v0.1.46/thenewminer-0.1.46.tar.gz`) và áp
  dụng flight sheet. HiveOS chỉ cài đặt khi tên tệp trong URL thay đổi.
- **Lần khởi động đầu tiên mất một lúc:** sau khi tải model về, trên các card dòng RTX 30, bước kiểm tra GPU một lần có
  thể bị quá thời gian chờ trên ổ đĩa chậm của rig, và khi đó HiveOS tự khởi động lại miner hai hoặc ba lần (khoảng 8
  phút). Cứ để yên; sau đó nó sẽ đào.
- **Đã có sẵn model?** Sao chép `Qwen3-8B-Q8_0-full-v2.gguf` (khoảng 8.1 GiB, đúng tên tệp này) vào `/home/user/` trên
  rig trước lần khởi động đầu tiên: miner sẽ tìm thấy tệp, kiểm tra SHA-256 của nó và dùng nó thay vì tải lại.

**Nếu rig không truy cập được GitHub:** tải `thenewminer-0.1.45.tar.gz` về một máy PC (kiểm tra SHA-256 của nó trong
bảng ở trên), sao chép sang rig và giải nén vào nơi HiveOS lưu các miner tùy chỉnh. Sau đó áp dụng flight sheet như bình
thường: HiveOS báo "Already installed" và không tải lại. Giữ chính xác tên tệp `thenewminer-0.1.45.tar.gz` (trình duyệt
có thể thêm " (1)") và không đổi installation URL trong flight sheet: HiveOS so sánh tên đó.

```bash
# trên PC
scp thenewminer-0.1.45.tar.gz user@<RIG_IP>:/tmp/
# trên rig, với quyền root (Hive Shell, hoặc SSH rồi sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.45.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.45.tar.gz
```

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
Hãy kiểm tra địa chỉ ví và mật khẩu pool (và pool đã duyệt tài khoản của bạn chưa), sau đó khởi động lại:
Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. Miner không tự thử
lại với thông tin đăng nhập sai.

**Miner có đang đào không? Log ở đâu?**
Linux: `./thenewminer --status`, `./thenewminer --logs`, hoặc tệp `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat status` và `TheNewMiner.bat logs`.

## Lưu ý bảo mật

Miner và GPU engine được đóng gói ở dạng mã hóa (ChaCha20-Poly1305) bên trong gói; miner giải mã chúng trên máy của bạn
khi khởi động. Điều này làm tăng chi phí sao chép chúng, nhưng **không phải là không thể phá vỡ**: bất kỳ ai có quyền
quản trị viên trên một máy đang chạy miner, nếu đủ công sức, đều có thể lấy được khóa hoặc mã đã giải mã.
Địa chỉ ví và mật khẩu pool của bạn được lưu trên chính máy của bạn và chỉ được gửi tới pool.
