[English](../GUIDE.md) · [简体中文](GUIDE.zh-CN.md) · [繁體中文](GUIDE.zh-TW.md) · [Русский](GUIDE.ru.md) · [Español](GUIDE.es.md) · [Português (Brasil)](GUIDE.pt-BR.md) · [Français](GUIDE.fr.md) · [Deutsch](GUIDE.de.md) · [Türkçe](GUIDE.tr.md) · **Tiếng Việt** · [한국어](GUIDE.ko.md) · [日本語](GUIDE.ja.md)

> Bản dịch này được cung cấp để tiện theo dõi; README tiếng Anh là bản chính thức. *(This translation is provided for convenience; the English README is authoritative.)*

# Hướng dẫn TheNewMiner

Mọi thứ sau lần khởi động đầu tiên: kiểm tra tệp tải về, sử dụng hằng ngày trên Linux, Windows và HiveOS, nâng cấp, phải
làm gì khi miner không đào, và các câu hỏi thường gặp. Tải về và hướng dẫn bắt đầu nhanh có trên [trang chính](README.vi.md);
các sự cố trên Windows có trang riêng: [Khắc phục sự cố trên Windows](WINDOWS-TROUBLESHOOTING.vi.md).

## Xác minh tệp đã tải

| Tệp | SHA-256 |
|---|---|
| [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | `86c215dc9401209b8d2a2346252ed5cc1901527b6a51520372e3128583c63656` |
| [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | `45a459646713bef9f8285848486c0a4bd66dee65e8c63090c8d3345c998b2659` |
| [`thenewminer-0.1.47.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz) (HiveOS) | `6b2515c5bf23f60daa236ff4a8b165ce5e55265d7eecac9dd408896e1e31b784` |

Tệp zip cho Windows là gói tất cả trong một: nó đã chứa sẵn miner Linux, nên bạn chỉ cần tệp zip. Đặt (các) gói và
[`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS) vào cùng một thư mục, sau đó:

```bash
# Linux (--ignore-missing: chỉ kiểm tra các gói bạn đã tải về)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Dấu nhắc Lệnh): so sánh kết quả với SHA-256 trong bảng ở trên
certutil -hashfile TheNewMiner-v0.1.47-windows-wsl2-r3.zip SHA256
```

Mỗi tệp phải báo `OK` (trên Windows: đúng 64 ký tự giống như trong bảng). Nếu không khớp, đừng sử dụng tệp đó.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Lệnh | Chức năng |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | Lần khởi động đầu tiên: cài đặt những gì cần thiết, sau đó đào ở chế độ nền |
| `./thenewminer --background` | Các lần khởi động sau: cài đặt của bạn đã được ghi nhớ |
| `./thenewminer` | Đào ở chế độ hiển thị (foreground) (Ctrl+C để dừng) |
| `./thenewminer --logs` | Theo dõi miner (Ctrl+C thoát khỏi màn hình theo dõi; việc đào vẫn tiếp tục) |
| `./thenewminer --status` | Miner có đang đào không, hashrate gần nhất và số share được chấp nhận / bị từ chối; sau khi dừng, cho biết vì sao miner dừng và cách khắc phục |
| `./thenewminer --stop` | Dừng đào |
| `./thenewminer --help` | Tất cả các tùy chọn |

- `-u` là địa chỉ ví TensorCash của bạn, `-p` là mật khẩu pool của bạn, `-w` là tên worker hiển thị trên pool
  (mặc định: tên máy (host name) của máy tính; 1–64 ký tự gồm chữ cái, chữ số, `_`, `-` hoặc `.`).
- Địa chỉ ví được kiểm tra trước khi bất cứ thứ gì khởi động: địa chỉ bị gõ nhầm (checksum không khớp) hoặc địa chỉ ví
  dụ sẽ bị từ chối, kèm theo thông báo cho biết sai ở đâu.
- Mật khẩu pool: 6–128 ký tự, mọi ký tự hiển thị được, không có dấu cách. Luôn đặt nó trong dấu nháy đơn trên dòng
  lệnh, ví dụ `-p 'my$pass!'` (dấu `'` bên trong dấu nháy đơn được viết là `'\''`).
- Cài đặt được ghi nhớ trong `thenewminer/.miner/` (tệp chứa mật khẩu chỉ bạn mới đọc được). Ví và mật khẩu được gỡ khỏi
  dòng lệnh ngay lập tức, nên chúng không hiển thị trong `ps` (shell của bạn vẫn có thể lưu lệnh trong lịch sử).
- Ở lần khởi động đầu tiên trên máy có một GPU, miner kiểm tra ví và mật khẩu với pool **trước khi** tải model, nên lỗi
  gõ nhầm được phát hiện trong vài giây (rig nhiều GPU sẽ phát hiện khi bắt đầu đào). Nếu pool từ chối chúng, miner sẽ
  báo điều đó và dừng lại: hãy kiểm tra chúng và khởi động lại với `-u` và `-p`.
- Lần khởi động đầu tiên sẽ tải model (khoảng 8.1 GiB, chỉ một lần), cài đặt GPU engine cho card của bạn và chạy tự
  kiểm tra trên card, sau đó bắt đầu đào. Các dòng như `[ RATE] ... N/s` nghĩa là miner đang đào. Log nằm ở
  `thenewminer/.miner/thenewminer.log`.
- Ubuntu 22.04 / 24.04 nguyên bản (kể cả bản trong WSL) đã có mọi thư viện miner cần, nên không cài thêm gì. Nếu thiếu
  thư viện nào (image Docker, hệ thống tối giản), lần khởi động đầu tiên trên Ubuntu, Debian và HiveOS sẽ liệt kê nó và
  cài bằng `apt` (cần sudo hoặc root); trên các hệ thống khác, miner sẽ nêu tên để bạn tự cài.
- Rig nhiều GPU: được hỗ trợ, mỗi GPU đều đào; bản phát hành này được thử nghiệm với một GPU trên mỗi máy.
- Trong Docker hoặc các container khác có ít bộ nhớ dùng chung, miner chạy các tệp chương trình của nó từ
  `thenewminer/.miner/run/` trên ổ đĩa và thông báo điều đó. Để chọn thư mục khác, hãy khởi động bằng
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (một thư mục chỉ bạn mới ghi được: `chmod 700 <folder>`).
- **Nâng cấp:** `./thenewminer --stop`, giải nén gói mới vào cùng vị trí (gói sẽ giải nén vào `thenewminer/` và
  giữ lại `thenewminer/.miner/` cùng cài đặt và model của bạn), sau đó khởi động lại. Nếu phiên bản cũ vẫn đang
  chạy, phiên bản mới sẽ từ chối khởi động và yêu cầu bạn dừng phiên bản cũ trước. Nếu giải nén vào một thư mục mới,
  nó sẽ tìm thấy model của bản cũ (cùng kích thước và SHA-256) và dùng lại: không phải tải 8.1 GiB lần thứ hai. Khi đó
  hãy đưa lại `-u -p -w` ở thư mục mới.

### Khi miner không đào

`./thenewminer --status` cho biết vì sao miner đã dừng và cách khắc phục. Các thông báo thường gặp nhất:

| Bạn thấy | Cần làm gì |
|---|---|
| `bash: ./thenewminer: Permission denied` | Thư mục không cho phép chạy chương trình (ổ được gắn với `noexec`, chẳng hạn một số `/tmp`, hoặc USB). Hãy giải nén trong thư mục home của bạn: `cd ~ && tar -xJf <the package>.tar.xz && cd thenewminer` |
| `this package is on a FAT (vfat) drive` (hoặc exFAT / NTFS, hoặc `Windows (DrvFs, 9p)`: một ổ đĩa Windows, chẳng hạn `/mnt/d` trong WSL) | Ổ đĩa như vậy không thể giữ các cài đặt riêng tư của miner (quyền 600). Hãy dán dòng `mkdir -p ~/thenewminer && cp -a ...` mà miner in ra: nó sao chép thư mục vào thư mục home Linux của bạn và khởi động ở đó |
| `this system has no xz` | `sudo apt-get install -y xz-utils` (nếu là root: bỏ `sudo`), sau đó khởi động lại |
| `This system is missing what TheNewMiner needs` | Chạy dòng `sudo apt-get install -y ...` mà miner in ra, sau đó khởi động lại |
| `No NVIDIA GPU found on this machine` | Không thấy card NVIDIA nào. Hãy kiểm tra card đã được cắm chắc và cấp nguồn; trên máy cloud, hãy chọn máy có GPU NVIDIA |
| `NVIDIA driver ... is too old` | Cài driver được nêu tên (580 trở lên), khởi động lại máy, rồi khởi động lại miner (xem "Cập nhật driver NVIDIA" bên dưới) |
| `the pool rejected the wallet / password` | Kiểm tra `-u` và `-p` và tài khoản đã tồn tại trên trang web pool, sau đó chạy `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' --background` |
| `no download source answered` | Việc tải model không kết nối được internet. Nếu dùng proxy: `https_proxy=http://<HOST>:<PORT> ./thenewminer --background`; hoặc sao chép model (xem "Đã có sẵn model?" bên dưới) |
| `cannot reach the pool` / `pool disconnected` | Kiểm tra kết nối internet, DNS và tường lửa có cho phép kết nối TCP 443 đi ra không; nếu mạng của bạn chặn pool, hãy dùng `--proxy` (xem "Proxy" bên dưới). Miner tự kết nối lại |

### Khởi động sau khi khởi động lại máy (tùy chọn)

Miner không tự khởi động sau khi khởi động lại máy (HiveOS tự làm việc này cho bạn). Trên Linux, hãy thêm một dòng
`crontab`:

```bash
crontab -e      # sau đó thêm dòng này (thư mục của bạn):
@reboot cd /home/<YOU>/thenewminer && ./thenewminer --background
```

hoặc một dịch vụ systemd cho người dùng (chạy `loginctl enable-linger <YOU>` một lần để nó khởi động mà không cần đăng nhập):

```ini
# ~/.config/systemd/user/thenewminer.service, sau đó: systemctl --user enable --now thenewminer
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

Mã thoát 77 nghĩa là pool đã từ chối ví / mật khẩu: khởi động lại không giúp được gì, nên dịch vụ dừng ở đó.

## Windows

1. Giải nén toàn bộ `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` (nhấp chuột phải › Trích xuất Tất cả) vào bất kỳ đâu:
   thư mục nào, ổ đĩa nào cũng được; đường dẫn có dấu cách hoặc ký tự tiếng Trung đều không sao. Đừng chạy nó từ bên
   trong cửa sổ xem tệp zip.
2. Nhấp đúp vào **`TheNewMiner.bat`**.
   - **Lần đầu tiên:** trước hết, MỘT cửa sổ hỏi tài khoản pool của bạn: địa chỉ ví (bắt đầu bằng `tc1`; checksum của
     nó được kiểm tra, và địa chỉ ví dụ hiển thị không phải là ví thật), mật khẩu pool (6–128 ký tự, mọi ký tự hiển thị
     được, không có dấu cách) và tên worker (tên máy tính được điền sẵn). Nút OK bị mờ cho đến khi cả ba đều hợp lệ. Sau
     khi nhấp OK và "Có", không có gì được hỏi thêm: nó kiểm tra Windows, driver và ổ đĩa, bật WSL2 nếu cần (một lần hỏi
     quyền quản trị viên; có thể cần khởi động lại máy một lần, sau đó quá trình cài đặt tự tiếp tục), cho biết dữ liệu
     sẽ được lưu ở đâu, tạo môi trường Linux (WSL) riêng, cài miner đi kèm, tải model, rồi bắt đầu đào. Nếu không có
     giao diện desktop (qua SSH), các câu hỏi tương tự sẽ hiện trong console.
   - **Mỗi lần sau đó:** nhấp đúp là bắt đầu đào ngay với cài đặt đã lưu. Một lối tắt "TheNewMiner" trên màn hình
     nền cũng được tạo. Nếu nhấp đúp lần nữa khi miner đang đào, nó sẽ báo cho bạn biết và hiển thị log.
   - **Nếu quá trình cài đặt dừng lại,** cửa sổ sẽ cho biết dừng ở bước nào và cần làm gì, và `TheNewMiner.bat` sẽ hiển
     thị lại thông tin đó. Xem [Khắc phục sự cố trên Windows](WINDOWS-TROUBLESHOOTING.vi.md).

Các lệnh dùng hằng ngày (mở Dấu nhắc Lệnh trong thư mục đã giải nén):

| Lệnh | Chức năng |
|---|---|
| `TheNewMiner.bat` | Cài đặt nếu cần, sau đó đào trong cửa sổ này (đóng cửa sổ sẽ dừng đào) |
| `TheNewMiner.bat background` | Đào mà không có cửa sổ |
| `TheNewMiner.bat logs` | Theo dõi miner (Ctrl+C thoát khỏi màn hình theo dõi; việc đào vẫn tiếp tục) |
| `TheNewMiner.bat status` | Miner có đang chạy không, và hashrate gần nhất |
| `TheNewMiner.bat stop` | Dừng đào |
| `TheNewMiner.bat setup` | Nhập lại ví / mật khẩu / tên worker (Cancel để giữ nguyên) |
| `TheNewMiner.bat uninstall` | Gỡ miner, thư mục dữ liệu của nó và lối tắt (có hỏi trước) |

- **Dữ liệu được lưu ở đâu:** miner và model nằm trong một thư mục dữ liệu được chọn ở lần cài đặt đầu tiên (mặc định
  là `TheNewMinerData` ở thư mục gốc của ổ đĩa mà bạn đã giải nén vào, ví dụ `D:\TheNewMinerData`), không nằm trong
  thư mục đã giải nén. Cần khoảng 20 GB dung lượng trống ở đó. Để chọn thư mục khác: `TheNewMiner.bat setup -DataDir <folder>`.
- **Ubuntu:** quá trình cài đặt cài Ubuntu 24.04 bằng `wsl --install`. Khi lượt tải đó không kết nối được, nó sẽ tải
  image WSL chính thức của Ubuntu `ubuntu-24.04.5-wsl-amd64.wsl` từ một mirror công khai ở Trung Quốc (Tsinghua TUNA,
  sau đó USTC, sau đó Aliyun) và chỉ dùng nó nếu SHA-256 của nó đúng bằng
  `bb415d824822c4b878125729af451a5d18fb13d1cf5cbed9a7393ad64ac6039e`.
- Nếu pool từ chối ví hoặc mật khẩu, cửa sổ sẽ báo điều đó và việc đào dừng lại: hãy chạy `TheNewMiner.bat setup`
  để nhập lại chúng.
- **Nâng cấp:** giải nén phiên bản mới vào bất kỳ đâu và nhấp đúp vào `TheNewMiner.bat` của nó. Nó sẽ tìm miner đã cài,
  giữ lại cài đặt và model của bạn, và cài phiên bản mới mà không hỏi gì. Bản cài đặt do phiên bản cũ hơn tạo ra hoặc
  được cài thủ công bên trong một bản phân phối WSL khác (ví dụ `Ubuntu-24.04`) sẽ được tìm thấy và nâng cấp ngay tại
  đó: không cài lần thứ hai, không tạo Ubuntu mới, không tải model lần thứ hai. Nếu không thể nâng cấp tại đó, quá trình
  cài đặt sẽ dừng và cho biết lý do. Có thể xóa thư mục cũ.
- **Gỡ cài đặt:** `TheNewMiner.bat uninstall`. Lệnh này không bao giờ xóa bản phân phối WSL do chính bạn tạo.
- Bộ cài cho Windows hiển thị bằng tiếng Anh.

## HiveOS (thử nghiệm)

Hỗ trợ HiveOS đang ở giai đoạn **thử nghiệm** (từ v0.1.45, phiên bản đã chạy trên một rig HiveOS thật; các thay đổi kể
từ v0.1.46 chưa được chạy trên rig HiveOS nào). Nó dùng gói custom miner cho HiveOS `thenewminer-0.1.47.tar.gz`
(SHA-256 của gói nằm trong bảng ở mục "Xác minh tệp đã tải").

Thiết lập nhanh (nhập flight sheet):

1. **Wallets** (ví): thêm một ví với coin `TSC` và địa chỉ TensorCash của bạn (`tc1...`). Việc này chỉ cần làm một lần:
   HiveOS cần một ví `TSC` được chọn, còn miner dùng địa chỉ bạn điền trong flight sheet.
2. Mở [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) và sao chép nội dung của nó. Thay
   `<YOUR_WALLET>` bằng địa chỉ ví của bạn (giữ `.%WORKER_NAME%` ở phía sau) và `<YOUR_POOL_PASSWORD>` bằng mật khẩu
   pool của bạn (trong JSON, ký tự `"` trong mật khẩu được viết là `\"` và ký tự `\` được viết là `\\`). Trong HiveOS
   chọn Flight Sheets › **Import from clipboard**, dán nội dung vào, chọn ví `TSC` của bạn rồi lưu.
3. Áp dụng flight sheet cho rig của bạn. Rig sẽ tải gói về và bắt đầu đào.

Hoặc thiết lập thủ công: tạo một flight sheet với Coin `TSC`, Wallet: ví `TSC` của bạn, Pool: "Configure in miner",
Miner: "Custom", nhấp **Setup Miner Config** và điền:

| Trường | Giá trị |
|---|---|
| Miner name | `thenewminer` (tự điền từ installation URL) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` |
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
- **Khi có lỗi** (ví dụ pool từ chối ví hoặc mật khẩu), log của miner và dashboard HiveOS sẽ hiển thị lý do, và rig thử
  lại với khoảng nghỉ tăng dần (từ một đến mười phút). Hãy sửa flight sheet và áp dụng lại: việc này khởi động lại miner
  ngay lập tức.
- Log: màn hình miner trong HiveOS (`miner` trong shell của rig) hoặc `/var/log/miner/custom/custom.log`.
- **Nâng cấp:** đổi cả hai số phiên bản trong installation URL (ví dụ
  `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`) và áp dụng flight sheet. HiveOS chỉ cài đặt khi tên tệp trong URL thay đổi.
- **Lần khởi động đầu tiên** tải model về và cài engine GPU, việc này mất vài phút.
- **Đã có sẵn model?** Sao chép nó vào `/home/user/` trên rig trước lần khởi động đầu tiên (xem mục tiếp theo).

**Nếu rig không truy cập được GitHub:** tải `thenewminer-0.1.47.tar.gz` về một máy PC (kiểm tra SHA-256 của nó), sao
chép sang rig và giải nén vào nơi HiveOS lưu các miner tùy chỉnh. Sau đó áp dụng flight sheet như bình thường: HiveOS
báo "Already installed" và không tải lại. Giữ chính xác tên tệp `thenewminer-0.1.47.tar.gz` (trình duyệt có thể thêm
" (1)") và không đổi installation URL trong flight sheet: HiveOS so sánh tên đó.

```bash
# trên PC
scp thenewminer-0.1.47.tar.gz user@<RIG_IP>:/tmp/
# trên rig, với quyền root (Hive Shell, hoặc SSH rồi sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.47.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.47.tar.gz
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

**Windows:** chạy `TheNewMiner.bat setup`. Lệnh này mở cửa sổ tài khoản với tên worker của bạn được điền sẵn; hãy nhập
ví và mật khẩu pool, sau đó khởi động như thường lệ (nhấp đúp vào `TheNewMiner.bat`).

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

- **Ubuntu:** `sudo ubuntu-drivers install` sẽ cài driver được khuyến nghị (sau đó hãy kiểm tra rằng đó là bản 580 trở
  lên). Để tự chọn phiên bản:
  `sudo apt install nvidia-driver-580` (hoặc mới hơn; card dòng RTX 50 cần bản open, ví dụ
  `nvidia-driver-580-open`). Sau đó khởi động lại máy. Chỉ dùng gói driver của Ubuntu hoặc trình cài đặt `.run` của
  NVIDIA, tuyệt đối không dùng cả hai.
- **Debian:** các gói driver có sẵn của Debian cũ hơn 580 (535 trong Debian 12, 550 trong Debian 13). Hãy cài driver mới
  hơn từ kho của NVIDIA theo mô tả trong
  [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)
  (hướng dẫn cài đặt driver NVIDIA), sau đó khởi động lại máy.
- **Windows:** cập nhật bằng [NVIDIA app](https://www.nvidia.com/en-us/software/nvidia-app/) (trước đây là GeForce
  Experience), hoặc tải driver cho card của bạn từ [nvidia.com](https://www.nvidia.com/en-us/drivers/) (580 trở lên).
  WSL2 dùng chính driver Windows này: tuyệt đối không cài driver NVIDIA cho Linux bên trong WSL.
- **HiveOS:** với quyền root (Hive Shell, hoặc SSH rồi `sudo -i`), chạy `nvidia-driver-update --list` để xem các phiên
  bản, sau đó chạy ví dụ `nvidia-driver-update 595` (bất kỳ bản nào từ 580 trở lên), rồi chạy `sreboot`.

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

**Card RTX 3080 (10 GB) của tôi đào trên Windows chậm hơn nhiều so với trên Linux.**
Trên Windows, các chương trình khác dùng chung VRAM. Xem [Khắc phục sự cố trên Windows](WINDOWS-TROUBLESHOOTING.vi.md):
đóng các trình duyệt đang bật tăng tốc phần cứng và Docker Desktop.
