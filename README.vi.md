[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · **Tiếng Việt** · [한국어](README.ko.md) · [日本語](README.ja.md)

> Bản dịch này được cung cấp để tiện theo dõi; README tiếng Anh là bản chính thức. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner là phần mềm đào bằng GPU cho **TensorCash**. Phần mềm chạy trên card NVIDIA GeForce RTX dòng 30, 40 và 50 có
**từ 10 GB VRAM trở lên** và đào trên pool **`tsc-wss.the-new.win`**, vốn đã được tích hợp sẵn: bạn không cần nhập địa
chỉ pool, và nó kết nối trực tiếp.

Phiên bản hiện tại: **v0.1.44**

## Tải về

| Tệp | Dành cho | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64 (Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11 (chạy thông qua WSL2) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | mã kiểm tra (checksum) của cả hai gói | |

Tệp zip cho Windows là gói tất cả trong một: nó đã chứa sẵn miner Linux, nên bạn chỉ cần tệp zip.

## Xác minh tệp đã tải

Đặt (các) gói và `SHA256SUMS` vào cùng một thư mục, sau đó:

```bash
# Linux (thêm --ignore-missing nếu bạn chỉ tải một gói)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows (Dấu nhắc Lệnh): so sánh kết quả với SHA-256 trong bảng ở trên
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

Mỗi tệp phải báo `OK` (trên Windows: đúng 64 ký tự giống như trong bảng). Nếu không khớp, đừng sử dụng tệp đó.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background
```

| Lệnh | Chức năng |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background` | Lần khởi động đầu tiên: cài đặt những gì cần thiết, sau đó đào ở chế độ nền |
| `./thenewminer --background` | Các lần khởi động sau: cài đặt của bạn đã được ghi nhớ |
| `./thenewminer` | Đào ở chế độ hiển thị (foreground) |
| `./thenewminer --logs` | Theo dõi miner (Ctrl+C thoát khỏi màn hình theo dõi; việc đào vẫn tiếp tục) |
| `./thenewminer --stop` | Dừng đào |

- `-u` là địa chỉ ví TensorCash của bạn, `-p` là mật khẩu pool của bạn, `-w` là tên worker hiển thị trên pool
  (mặc định: tên máy (host name) của máy tính).
- Cài đặt được ghi nhớ trong `thenewminer/.miner/` (tệp chứa mật khẩu chỉ bạn mới đọc được).
- Lần khởi động đầu tiên sẽ tải model (khoảng 8.1 GiB, chỉ một lần), cài đặt GPU engine cho card của bạn và chạy tự
  kiểm tra trên card, sau đó bắt đầu đào. Các dòng như `[ RATE] ... N/s` nghĩa là miner đang đào.
- Lần khởi động đầu tiên có thể cài một vài gói Ubuntu bằng `apt` (cần sudo hoặc root).
- Phiên bản này hỗ trợ đúng một GPU NVIDIA trên mỗi máy (rig nhiều GPU chưa được hỗ trợ).
- Nếu `tar` báo `xz: Cannot exec`, hãy cài xz trước: `sudo apt-get install -y xz-utils`.
- Tệp `README.md` bên trong gói là một ghi chú nội bộ cũ; hãy làm theo trang này (không cần `--proxy`).
- **Nâng cấp:** `./thenewminer --stop`, giải nén gói mới vào cùng vị trí (gói sẽ giải nén vào `thenewminer/` và
  giữ lại `thenewminer/.miner/` cùng cài đặt và model của bạn), sau đó khởi động lại.

## Windows (một cú nhấp)

1. Giải nén toàn bộ `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` (nhấp chuột phải › Trích xuất Tất cả) vào bất kỳ đâu:
   thư mục nào, ổ đĩa nào cũng được; đường dẫn có dấu cách hoặc ký tự tiếng Trung đều không sao. Đừng chạy nó từ bên
   trong cửa sổ xem tệp zip.
2. Nhấp đúp vào **`TheNewMiner.bat`**.
   - **Lần đầu tiên:** nó kiểm tra Windows, driver và ổ đĩa, bật WSL2 nếu cần (một lần hỏi quyền quản trị viên;
     có thể cần khởi động lại máy một lần, sau đó quá trình cài đặt tự tiếp tục), cho biết dữ liệu sẽ được lưu ở đâu
     (Enter để chấp nhận), tạo môi trường Linux (WSL) riêng, cài miner đi kèm, hỏi **một lần** địa chỉ ví của bạn
     (bắt đầu bằng `tc1`), mật khẩu pool (nhập hai lần; 6–128 ký tự gồm chữ cái, chữ số, `_` hoặc `-`) và tên worker
     (Enter = tên máy tính), tải model, sau đó bắt đầu đào.
   - **Mỗi lần sau đó:** nhấp đúp là bắt đầu đào ngay với cài đặt đã lưu. Một lối tắt "TheNewMiner" trên màn hình
     nền cũng được tạo.

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
- **Nâng cấp:** giải nén phiên bản mới vào bất kỳ đâu và nhấp đúp vào `TheNewMiner.bat` của nó. Nó sẽ tìm miner đã cài,
  giữ lại cài đặt và model của bạn, và cài phiên bản mới mà không hỏi gì. Có thể xóa thư mục cũ.
- **Gỡ cài đặt:** `TheNewMiner.bat uninstall`.
- Ngôn ngữ hiển thị trên màn hình theo ngôn ngữ của Windows (English / 简体中文 / 繁體中文). Để tự chọn, thêm `-Lang en`
  (hoặc `zh-Hans`, `zh-Hant`) vào bất kỳ lệnh nào, ví dụ `TheNewMiner.bat -Lang en`; tùy chọn này áp dụng cho lần chạy đó.

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
| GPU | Đúng một card NVIDIA GeForce RTX dòng 30 / 40 / 50, từ 10 GB VRAM trở lên |
| Driver | NVIDIA **580 trở lên** (`nvidia-smi` phải hoạt động). Trên Windows, cài driver trên Windows, tuyệt đối không cài trong WSL |
| Linux | x86_64 Ubuntu 22.04, 24.04 hoặc 26.04 (các bản phân phối khác chưa được hỗ trợ) |
| Windows | Windows 10 21H2 trở lên, hoặc Windows 11, 64-bit; bật ảo hóa (Intel VT-x / AMD SVM) trong BIOS |
| Ổ đĩa | khoảng 20 GB trống |
| RAM | từ 4 GB trở lên |
| Model | khoảng 8.1 GiB, tải một lần ở lần khởi động đầu tiên (song song, nhiều mirror) và dùng lại về sau |

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

Thử nghiệm v0.1.44 trên pool thật với các rig thử nghiệm của chúng tôi vào ngày 2026-10-23 (một GPU mỗi máy).
A = share được chấp nhận, R = share bị từ chối.

| Ngày | Card | Hệ điều hành | Gói | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | sealed-r4 | 65.58 N/s (trung vị, 16 phút) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | sealed-r4 | 93.45 N/s (đào ổn định, 44 phút; 90.39 khi tính cả các lần đổi job) | 31 / 0 (+1 stale lúc dừng) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | windows-wsl2-r5 | ~22 N/s (lần chạy riêng của nhà vận hành) | không có |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | sealed-r4 | 21.04 N/s (10 phút cuối của 23 phút đào) | 3 / 0 |

RTX 3080 trên Linux: cài đặt 6/6 bước OK, không có cửa sổ (window) không hợp lệ nào, dừng sạch. Trích đoạn log:
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

**Log ở đâu?**
Linux: `./thenewminer --logs`, hoặc tệp `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat logs` (và `TheNewMiner.bat status` để xem hashrate gần nhất).

**HiveOS?**
Gói cho HiveOS **sắp ra mắt**; hiện vẫn đang trong giai đoạn thử nghiệm.

## Lưu ý bảo mật

Miner và GPU engine được đóng gói ở dạng mã hóa (ChaCha20-Poly1305) bên trong gói; miner giải mã chúng trên máy của bạn
khi khởi động. Điều này làm tăng chi phí sao chép chúng, nhưng **không phải là không thể phá vỡ**: bất kỳ ai có quyền
quản trị viên trên một máy đang chạy miner, nếu đủ công sức, đều có thể lấy được khóa hoặc mã đã giải mã.
Địa chỉ ví và mật khẩu pool của bạn được lưu trên chính máy của bạn và chỉ được gửi tới pool.
