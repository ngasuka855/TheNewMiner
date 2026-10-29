# TheNewMiner

TheNewMiner là phần mềm đào bằng GPU cho **TensorCash** trên card NVIDIA GeForce RTX dòng 30, 40 và 50 có từ 10 GB VRAM trở lên; pool **`tsc-wss.the-new.win`** đã được tích hợp sẵn.

[English](../../README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · **Tiếng Việt** · [한국어](README.ko.md) · [日本語](README.ja.md)

> Bản dịch này được cung cấp để tiện theo dõi; README tiếng Anh là bản chính thức. *(This translation is provided for convenience; the English README is authoritative.)*

## Tải về

Phiên bản hiện tại: **v0.1.47** · Trang web pool: [tsc.the-new.win](https://tsc.the-new.win/#/home)

| Hệ thống | Tải về | Sau đó |
|---|---|---|
| Linux x86_64 | [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | hoặc dùng cách cài đặt một cú nhấp bên dưới |
| Windows 10 / 11 | [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | giải nén, rồi nhấp đúp vào `TheNewMiner.bat` |
| HiveOS (thử nghiệm) | [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) | nhập flight sheet (xem mục HiveOS bên dưới) |

Mã kiểm tra (checksum): [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS). Cách kiểm tra
có trong [hướng dẫn](GUIDE.vi.md). Nếu một tệp không khớp, đừng sử dụng tệp đó.

## Trước khi bắt đầu: tạo tài khoản pool

Để đào, bạn cần một tài khoản pool: địa chỉ nhận thanh toán TSC (ví) của bạn và một mật khẩu pool do bạn tự chọn trên
trang web pool. Trang web có phiên bản tiếng Anh (dùng menu ngôn ngữ của trang).

1. Mở [tsc.the-new.win](https://tsc.the-new.win/#/home) và vào **Start mining** (开始挖矿).
2. Trong **1. Bind Wallet**, nhập **TSC payout address** của bạn (`tc1...`; hãy kiểm tra kỹ: thu nhập đào được gửi tới
   một địa chỉ sai sẽ không thể lấy lại), chọn một **Pool Password** (6–128 ký tự) và nhập lại ở **Confirm Password**.
3. Nhấp **Apply to join pool**. Trang sẽ xác nhận ví đã được liên kết; tài khoản dùng được ngay.
4. Dùng cùng ví và mật khẩu đó trong miner: `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'` trên Linux, cửa sổ tài khoản
   của `TheNewMiner.bat` trên Windows, hoặc flight sheet trên HiveOS. Khoảng 3–5 phút sau khi bắt đầu đào, rig của bạn
   sẽ xuất hiện trong **My Miners** trên trang web pool.

Pool không thể khôi phục mật khẩu pool bị quên, và địa chỉ nhận thanh toán không thể thay đổi về sau. Mật khẩu pool
không phải là khóa riêng (private key) của ví: tuyệt đối không đưa khóa riêng cho bất kỳ ai.

## Cài đặt một cú nhấp (Linux)

Thay `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` và `<RIG_NAME>` (bao gồm cả dấu `<` `>`) ở cuối lệnh này bằng ví của bạn,
mật khẩu pool của bạn và một tên cho rig này, rồi dán lệnh vào terminal. Lệnh sẽ tải gói Linux và `SHA256SUMS`, kiểm tra
SHA-256, giải nén miner vào `~/thenewminer` và khởi động nó ở chế độ nền:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.47 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Lỗi tải về hoặc SHA-256 không khớp sẽ không bao giờ khởi động thứ gì. Hãy giữ dấu nháy đơn quanh mật khẩu (dấu `'` trong
mật khẩu được viết là `'\''`). Trên máy có một GPU, miner kiểm tra ví và mật khẩu với pool trước khi tải model (khoảng
8.1 GiB, chỉ một lần). Sau đó `./thenewminer --logs` cho thấy miner đang đào và `./thenewminer --status` cho biết miner có
đang chạy không, hoặc vì sao nó đã dừng. Để nâng cấp, trước tiên chạy `~/thenewminer/thenewminer --stop`, sau đó chạy lại
cùng lệnh này: cài đặt và model của bạn được giữ lại. Mọi thứ khác (các lệnh hằng ngày, nâng cấp, tự khởi động, phải làm
gì khi miner không đào) đều có trong [hướng dẫn](GUIDE.vi.md).

## Windows

1. Giải nén toàn bộ `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` (nhấp chuột phải › Trích xuất Tất cả) vào bất kỳ đâu.
2. Nhấp đúp vào **`TheNewMiner.bat`**. Ở lần đầu tiên, MỘT cửa sổ hỏi tài khoản pool của bạn (ví, mật khẩu pool, tên
   worker); sau đó không hỏi gì thêm: nó thiết lập WSL2 và Ubuntu, cài miner đi kèm, tải model và bắt đầu đào. Các lần
   nhấp đúp sau sẽ bắt đầu đào ngay. Trước tiên Windows có thể báo "Windows đã bảo vệ PC của bạn" (bộ cài không được ký
   mã): nhấp **Thêm thông tin**, sau đó nhấp **Vẫn chạy**. Việc bật WSL sẽ yêu cầu quyền quản trị viên một lần và có thể
   cần khởi động lại một lần; sau đó quá trình thiết lập sẽ tự tiếp tục.
3. Phiên bản cũ hơn sẽ được tìm thấy và nâng cấp ngay tại chỗ, giữ nguyên cài đặt và model của bạn.

Nếu miner dừng hoặc đào chậm, hãy xem [Khắc phục sự cố trên Windows](WINDOWS-TROUBLESHOOTING.vi.md) (DNS, ổ đĩa Windows,
VRAM bị trình duyệt và Docker Desktop sử dụng, đồng hồ hệ thống).

## HiveOS (thử nghiệm)

1. **Wallets** (ví): thêm một ví với coin `TSC` và địa chỉ TensorCash của bạn (`tc1...`).
2. Mở [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json), sao chép nội dung của nó và thay
   `<YOUR_WALLET>` (giữ `.%WORKER_NAME%` ở phía sau) và `<YOUR_POOL_PASSWORD>`. Trong HiveOS chọn Flight Sheets ›
   **Import from clipboard**, dán nội dung vào, chọn ví `TSC` của bạn rồi lưu.
3. Áp dụng flight sheet cho rig của bạn. Nó sẽ cài `thenewminer-0.1.47.tar.gz` và bắt đầu đào.

Thiết lập thủ công, nâng cấp và rig không truy cập được GitHub: xem [hướng dẫn](GUIDE.vi.md).

## Có gì mới trong v0.1.47

- **Cài đặt dễ hơn:** trên máy có một GPU, miner kiểm tra ví và mật khẩu pool của bạn với pool **trước khi** tải model
  8.1 GiB, nên lỗi gõ nhầm được phát hiện trong vài giây. Địa chỉ ví bị gõ nhầm (checksum không khớp) hoặc địa chỉ ví dụ
  sẽ bị từ chối, kèm theo thông báo cho biết sai ở đâu.
- **Windows: thông tin đăng nhập chỉ được hỏi một lần,** trong một cửa sổ ngay lúc bắt đầu, trước mọi bước mất nhiều thời
  gian. Bản cài đặt do phiên bản cũ hơn tạo ra, kể cả trong một bản phân phối WSL khác, sẽ được tìm thấy và **nâng cấp ngay
  tại chỗ** (giữ nguyên cài đặt và model). Nếu không tải được Ubuntu từ Microsoft, quá trình cài đặt sẽ lấy image WSL
  chính thức của Ubuntu từ một mirror ở Trung Quốc và chỉ dùng nó nếu SHA-256 của nó khớp.
- **Không cần apt trên Ubuntu nguyên bản:** Ubuntu 22.04 / 24.04 (kể cả bản trong WSL) đã có đủ mọi thứ miner cần, nên
  không cài thêm gì. Nếu thiếu thứ gì, lần khởi động đầu tiên sẽ liệt kê chúng trước khi cài.
- **Gói nằm trên ổ đĩa Windows** (chẳng hạn `/mnt/d` trong WSL) sẽ bị từ chối ngay, kèm theo một dòng lệnh sẵn để dán
  giúp sao chép gói vào thư mục home Linux của bạn và khởi động nó ở đó.
- **Thông báo mạng rõ ràng:** khi DNS bên trong WSL không hoạt động, quá trình cài đặt dừng ở bước đó kèm cách khắc phục;
  miner không truy cập được internet hoặc pool sẽ cho biết cần kiểm tra gì.
- **Model được dùng lại:** khi nâng cấp vào một thư mục mới, miner tìm thấy model của bản cài đặt trước (cùng kích thước
  và SHA-256) và không tải lại.
- **`--status` cho biết lý do** miner đã dừng và cách khắc phục, kể cả sau khi khởi động bằng `--background`.
- **HiveOS:** mọi lỗi giờ đều được chờ và thử lại với khoảng nghỉ tăng dần (từ một đến mười phút), và lý do được hiển thị
  trong log và trên dashboard.

Ghi chú đầy đủ: [`docs/releases/RELEASE-NOTES-v0.1.47.md`](../releases/RELEASE-NOTES-v0.1.47.md).

## Vấn đề đã biết

- **Card 10 GB trên Windows cần khoảng 9.3 GB VRAM trống để đạt tốc độ tối đa.** Nếu ít hơn, miner dùng một bố cục chậm
  hơn (một card RTX 3080 10 GB giảm từ 21–22 xuống khoảng 9 N/s). Trình duyệt bật tăng tốc phần cứng hoặc Docker Desktop
  có thể chiếm phần bộ nhớ đó: hãy đóng chúng (hoặc tắt tăng tốc phần cứng của trình duyệt), rồi bắt đầu đào lại.
- **Đồng hồ Windows:** nếu miner báo lỗi đồng hồ, hãy mở Cài đặt Windows › Thời gian & ngôn ngữ › Ngày & giờ, bật
  "Đặt múi giờ tự động" và "Đặt thời gian tự động", nhấp **Đồng bộ ngay**, sau đó chạy `wsl --shutdown` và khởi động
  lại.

## Hashrate

Hashrate tốt nhất đo được trên pool thật với các rig của chính chúng tôi. Đây là các con số **tối đa**, không phải lời cam
kết: các card cùng model chênh lệch nhau khoảng 5–8%, và hashrate sẽ giảm nếu card đồng thời xuất hình ra màn hình hoặc
có chương trình khác sử dụng GPU.

| GPU | Hashrate |
|---|---|
| RTX 5090 | tối đa ~99 N/s |
| RTX 4090 | tối đa ~70 N/s |
| RTX 3080 10 GB | tối đa ~22 N/s |

Các số đo có ghi ngày theo từng phiên bản: [kết quả thử nghiệm](../TEST-RESULTS.md). Các cột mốc của dự án:
[nhật ký phát triển](../DEVELOPMENT-RECORD.md).

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
