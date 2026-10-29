[English](../WINDOWS-TROUBLESHOOTING.md) · [简体中文](WINDOWS-TROUBLESHOOTING.zh-CN.md) · [繁體中文](WINDOWS-TROUBLESHOOTING.zh-TW.md) · [Русский](WINDOWS-TROUBLESHOOTING.ru.md) · [Español](WINDOWS-TROUBLESHOOTING.es.md) · [Português (Brasil)](WINDOWS-TROUBLESHOOTING.pt-BR.md) · [Français](WINDOWS-TROUBLESHOOTING.fr.md) · [Deutsch](WINDOWS-TROUBLESHOOTING.de.md) · [Türkçe](WINDOWS-TROUBLESHOOTING.tr.md) · **Tiếng Việt** · [한국어](WINDOWS-TROUBLESHOOTING.ko.md) · [日本語](WINDOWS-TROUBLESHOOTING.ja.md)

> Bản dịch này được cung cấp để tiện theo dõi; README tiếng Anh là bản chính thức. *(This translation is provided for convenience; the English README is authoritative.)*

# Khắc phục sự cố trên Windows

Trên Windows, TheNewMiner chạy miner Linux bên trong WSL2. Khi quá trình cài đặt dừng lại, cửa sổ sẽ cho biết dừng ở
bước nào và cần làm gì, và `TheNewMiner.bat` sẽ hiển thị lại thông tin đó (thông tin này cũng được lưu trong
`%LOCALAPPDATA%\TheNewMiner\last-setup-failure.txt`). Trang này giải thích các trường hợp thường gặp. Cách cài đặt và
các lệnh dùng hằng ngày có trong [hướng dẫn](GUIDE.vi.md).

## DNS không hoạt động bên trong WSL

Ở bước 4, quá trình cài đặt kiểm tra xem WSL có phân giải được tên miền của pool và của nguồn tải model hay không. Nếu
không, quá trình cài đặt dừng ở đó và không tự thay đổi gì. Hãy mở `%UserProfile%\.wslconfig` bằng Notepad (tạo tệp nếu
chưa có) và thêm hai dòng sau (trên Windows 11 22H2 trở lên, đặt `networkingMode=mirrored` dưới `[wsl2]` cũng được):

```ini
[wsl2]
dnsTunneling=true
```

Sau đó chạy `wsl --shutdown` trong Dấu nhắc Lệnh (lệnh này dừng mọi bản phân phối WSL, kể cả của Docker Desktop) và
nhấp đúp lại vào `TheNewMiner.bat`.

VPN hoặc tường lửa chặn WSL cũng có thể gây ra thông báo tương tự: hãy cho phép WSL, hoặc đặt proxy bằng
`TheNewMiner.bat setup -Proxy ...` (xem mục "Proxy" trong [hướng dẫn](GUIDE.vi.md)).

## Không tải được Ubuntu, hoặc ảo hóa đang tắt

- **Mạng:** quá trình cài đặt cài Ubuntu 24.04 bằng `wsl --install`. Khi lượt tải đó không kết nối được (ví dụ ở nơi không
  truy cập được GitHub), quá trình cài đặt sẽ tải image WSL chính thức của Ubuntu từ một mirror công khai ở Trung Quốc
  thay thế, và chỉ dùng nó nếu SHA-256 của nó khớp. Nếu cũng không có mirror nào phản hồi, hãy kiểm tra kết nối internet
  và thử lại sau.
- **Ảo hóa:** thông báo về ảo hóa (ví dụ `0x80370102`) nghĩa là tính năng này đang tắt. Hãy bật Intel VT-x / AMD SVM
  trong BIOS, rồi nhấp đúp lại vào `TheNewMiner.bat`.

## Gói nằm trên ổ đĩa Windows

Nếu bạn tự giải nén gói Linux và khởi động `./thenewminer` bên trong WSL từ một ổ đĩa Windows (chẳng hạn
`/mnt/d/...`), miner sẽ từ chối khởi động: ổ đĩa Windows không thể giữ tệp cài đặt riêng tư của miner. Miner in ra một
dòng `mkdir -p ~/thenewminer && cp -a ...` sẵn để dán, giúp sao chép thư mục vào thư mục home Linux của bạn và khởi động
nó ở đó. Hoặc hãy dùng tệp zip cho Windows: `TheNewMiner.bat` tự đặt mọi thứ vào đúng chỗ.

## Chậm trên card 10 GB (trình duyệt, Docker Desktop)

Card 10 GB chỉ dùng được bố cục nhanh khi còn trống khoảng 9.3 GB (9472 MiB) VRAM lúc bắt đầu đào. Trên Windows, các
chương trình khác dùng chung card: trình duyệt bật tăng tốc phần cứng hoặc Docker Desktop có thể chiếm đủ VRAM để miner
phải chuyển sang một bố cục chậm hơn. Trên RTX 3080 10 GB, hashrate khi đó khoảng 9 N/s thay vì 21–22 N/s.

1. Đóng các trình duyệt, hoặc tắt tăng tốc phần cứng của chúng (Chrome và Edge: Cài đặt › Hệ thống › "Sử dụng tính năng
   tăng tốc đồ họa khi có sẵn"; Firefox: Cài đặt › Tổng quát › Hiệu suất), và thoát Docker Desktop.
2. Kiểm tra VRAM trống trong Dấu nhắc Lệnh: `nvidia-smi --query-gpu=memory.used,memory.total --format=csv`
   (tổng trừ đi phần đã dùng phải ít nhất là 9472 MiB).
3. Khởi động lại việc đào: `TheNewMiner.bat stop`, sau đó nhấp đúp vào `TheNewMiner.bat`.

Card 12 GB cũng cần còn trống 9472 MiB như vậy; card có từ 16 GB trở lên không bị ảnh hưởng.

## Lỗi đồng hồ

Pool sẽ dừng miner có đồng hồ lệch khoảng 30 giây. Đồng hồ của WSL2 có thể bị lệch, ví dụ sau khi PC ở chế độ ngủ, và
quá trình cài đặt sẽ cảnh báo khi đồng hồ bị lệch. Nếu miner báo lỗi đồng hồ:

1. Mở Cài đặt Windows › Thời gian & ngôn ngữ › Ngày & giờ, bật "Đặt múi giờ tự động" và "Đặt thời gian tự động", sau đó
   nhấp **Đồng bộ ngay**.
2. Trong Dấu nhắc Lệnh, chạy `wsl --shutdown`.
3. Bắt đầu đào lại.

## Lời nhắc bảo mật của Windows

SmartScreen ("Windows đã bảo vệ PC của bạn") và các phần mềm diệt virus gắn cờ miner: xem mục Câu hỏi thường gặp trong
[hướng dẫn](GUIDE.vi.md).

## Vẫn chưa giải quyết được?

Mở một issue trên [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) kèm kết quả của
`TheNewMiner.bat status`, bước bị lỗi và nội dung của `last-setup-failure.txt`. Tuyệt đối không đăng mật khẩu pool hoặc
khóa riêng của ví.
