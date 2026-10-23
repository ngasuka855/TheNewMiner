[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · **Türkçe** · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Bu çeviri kolaylık sağlamak amacıyla sunulmuştur; esas alınacak metin İngilizce README'dir. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner, **TensorCash** için bir GPU madencisidir. **10 GB veya daha fazla VRAM**'e sahip NVIDIA GeForce RTX 30, 40
ve 50 serisi kartlarda çalışır ve yerleşik olarak gelen **`tsc-wss.the-new.win`** havuzunda kazım yapar: havuz adresi
girmeniz gerekmez, doğrudan bağlanır.

Güncel sürüm: **v0.1.44**

## İndirme

| Dosya | Hedef | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64 (Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11 (WSL2 üzerinden çalışır) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | her iki paketin sağlama toplamları | |

Windows zip'i hepsi bir arada bir pakettir: Linux madencisini zaten içerir, bu yüzden yalnızca zip'e ihtiyacınız vardır.

## İndirilen dosyayı doğrulama

Paket(ler)i ve `SHA256SUMS` dosyasını aynı klasöre koyun, ardından:

```bash
# Linux (yalnızca bir paket indirdiyseniz --ignore-missing ekleyin)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows (Komut İstemi): çıktıyı yukarıdaki tablodaki SHA-256 ile karşılaştırın
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

Her dosya için `OK` görünmelidir (Windows'ta: tablodaki ile aynı 64 karakter). Eşleşmiyorsa dosyayı kullanmayın.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background
```

| Komut | Ne yapar |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background` | İlk başlatma: gerekenleri kurar, ardından arka planda kazım yapar |
| `./thenewminer --background` | Sonraki başlatmalar: ayarlarınız hatırlanır |
| `./thenewminer` | Ön planda kazım yapar |
| `./thenewminer --logs` | Madenciyi izler (Ctrl+C görünümden çıkar; kazım devam eder) |
| `./thenewminer --stop` | Kazımı durdurur |

- `-u` TensorCash cüzdan adresiniz, `-p` havuz parolanız, `-w` havuzda görünen worker adıdır
  (varsayılan: makinenin ana bilgisayar adı).
- Ayarlar `thenewminer/.miner/` içinde hatırlanır (parolayı içeren dosyayı yalnızca siz okuyabilirsiniz).
- İlk başlatmada model indirilir (yaklaşık 8.1 GiB, yalnızca bir kez), kartınız için GPU motoru kurulur ve kart üzerinde
  bir öz denetim çalıştırılır, ardından kazım başlar. `[ RATE] ... N/s` gibi satırlar kazım yapıldığı anlamına gelir.
- İlk başlatma `apt` ile birkaç Ubuntu paketi kurabilir (sudo veya root gerekir).
- Bu sürümde makine başına tam olarak bir NVIDIA GPU desteklenir (çoklu GPU rig'leri henüz desteklenmiyor).
- `tar` `xz: Cannot exec` hatası verirse önce xz'yi kurun: `sudo apt-get install -y xz-utils`.
- Paketin içindeki `README.md` eski bir dahili nottur; bu sayfayı izleyin (`--proxy` gerekmez).
- **Güncelleme:** `./thenewminer --stop` çalıştırın, yeni paketi aynı yere açın (`thenewminer/` içine açılır ve
  ayarlarınızı ve modeli içeren `thenewminer/.miner/` klasörünü korur), ardından yeniden başlatın.

## Windows (tek tıkla)

1. `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` dosyasının tamamını (sağ tık › Tümünü Ayıkla) istediğiniz yere çıkarın:
   herhangi bir klasör, herhangi bir sürücü; boşluk veya Çince karakter içeren yollar sorun değildir. Zip görünümünün
   içinden çalıştırmayın.
2. **`TheNewMiner.bat`** dosyasına çift tıklayın.
   - **İlk seferde:** Windows'u, sürücüyü ve diski denetler, gerekirse WSL2'yi açar (bir kez yönetici onayı istenir;
     bir kez yeniden başlatma gerekebilir, kurulum ardından kendiliğinden devam eder), verilerin nereye kaydedileceğini
     gösterir (Enter kabul eder), kendi Linux (WSL) ortamını oluşturur, paketle gelen madenciyi kurar, cüzdan adresinizi
     (`tc1` ile başlar), havuz parolanızı (iki kez yazılır; 6–128 harf, rakam, `_` veya `-`) ve worker adını
     (Enter = bilgisayar adı) **bir kez** sorar, modeli indirir, ardından kazıma başlar.
   - **Sonraki her seferde:** çift tıklayın, kaydedilmiş ayarlarınızla hemen kazıma başlar. Masaüstünde
     "TheNewMiner" kısayolu da oluşturulur.

Günlük komutlar (çıkarılan klasörde bir Komut İstemi açın):

| Komut | Ne yapar |
|---|---|
| `TheNewMiner.bat` | Gerekirse kurar, ardından bu pencerede kazım yapar (pencereyi kapatmak kazımı durdurur) |
| `TheNewMiner.bat background` | Pencere olmadan kazım yapar |
| `TheNewMiner.bat logs` | Madenciyi izler (Ctrl+C görünümden çıkar; kazım devam eder) |
| `TheNewMiner.bat status` | Çalışıp çalışmadığını ve son hashrate değerlerini gösterir |
| `TheNewMiner.bat stop` | Kazımı durdurur |
| `TheNewMiner.bat setup` | Cüzdan / parola / worker adını yeniden girer |
| `TheNewMiner.bat uninstall` | Madenciyi, veri klasörünü ve kısayolu kaldırır (önce sorar) |

- **Veriler nereye gider:** madenci ve model, ilk kurulumda seçilen bir veri klasöründe bulunur (varsayılan olarak
  zip'i çıkardığınız sürücünün kökündeki `TheNewMinerData`, ör. `D:\TheNewMinerData`), çıkarılan klasörde değil.
  Orada yaklaşık 20 GB boş alan gerekir.
- **Güncelleme:** yeni sürümü herhangi bir yere çıkarın ve onun `TheNewMiner.bat` dosyasına çift tıklayın. Kurulu
  madenciyi bulur, ayarlarınızı ve modeli korur ve yeni sürümü soru sormadan kurar. Eski klasör silinebilir.
- **Kaldırma:** `TheNewMiner.bat uninstall`.
- Ekrandaki dil Windows'u izler (English / 简体中文 / 繁體中文). Dili seçmek için herhangi bir komuta `-Lang en` (veya
  `zh-Hans`, `zh-Hant`) ekleyin, ör. `TheNewMiner.bat -Lang en`; bu yalnızca o çalıştırma için geçerlidir.

## Proxy (isteğe bağlı)

Madenci havuza doğrudan bağlanır. Ağınız bir proxy gerektiriyorsa:

```bash
# Linux (değiştirilene kadar hatırlanır)
./thenewminer --proxy <IP>[:<PORT>]            # TCP aktarıcı (belirtilmezse port 443)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5, ayrıca socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # HTTP CONNECT proxy
./thenewminer --proxy none                     # doğrudan bağlantıya geri dön
```

Bu komutlar madenciyi de başlatır: önce madenciyi durdurun ve arka planda çalışması için `--background` ekleyin.

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

TLS havuzla her zaman uçtan uca çalışır, bu nedenle proxy bağlantıyı okuyamaz. Windows'ta aynı bilgisayarda çalışan
bir proxy uygulamasına WSL'den (varsayılan WSL ağ yapılandırmasında) genellikle `127.0.0.1` üzerinden ulaşılamaz:
bilgisayarın yerel ağ (LAN) IP'sini kullanın ve uygulamada LAN bağlantılarına izin verin.

## Gereksinimler

| Öğe | Gereksinim |
|---|---|
| GPU | Tam olarak bir adet NVIDIA GeForce RTX 30 / 40 / 50 serisi kart, 10 GB veya daha fazla VRAM |
| Sürücü | NVIDIA **580 veya daha yeni** (`nvidia-smi` çalışmalıdır). Windows'ta sürücüyü Windows'a kurun, asla WSL içine kurmayın |
| Linux | x86_64 Ubuntu 22.04, 24.04 veya 26.04 (diğer dağıtımlar henüz desteklenmiyor) |
| Windows | Windows 10 21H2 veya daha yenisi ya da Windows 11, 64-bit; BIOS'ta sanallaştırma (Intel VT-x / AMD SVM) açık |
| Disk | yaklaşık 20 GB boş alan |
| RAM | 4 GB veya daha fazla |
| Model | yaklaşık 8.1 GiB, ilk başlatmada bir kez indirilir (paralel, çoklu yansı) ve sonrasında yeniden kullanılır |

## Referans hashrate değerleri

Kendi rig'lerimizde gerçek havuzda ölçülen en iyi değerler (2026-10-14 ile 2026-10-23 arasında). Bunlar **en fazla**
değerlerdir, bir vaat değildir: aynı modeldeki kartlar; çipin kendisine, soğutmaya, saat hızlarına ve güç limitine göre
yaklaşık 5–8% farklılık gösterir ve kart aynı zamanda bir ekranı sürüyorsa veya GPU'yu başka programlar kullanıyorsa
hashrate düşer.

| GPU | Hashrate |
|---|---|
| RTX 5090 | en fazla ~99 N/s |
| RTX 4090 | en fazla ~70 N/s |
| RTX 3080 10 GB | en fazla ~22 N/s |

## Test sonuçları

v0.1.44'ün 2026-10-23 tarihinde test rig'lerimizde gerçek havuzda yapılan testleri (makine başına bir GPU).
A = kabul edilen share'ler, R = reddedilen share'ler.

| Tarih | Kart | İşletim sistemi | Paket | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | sealed-r4 | 65.58 N/s (medyan, 16 dk) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | sealed-r4 | 93.45 N/s (kararlı kazım, 44 dk; iş değişimleri dahil 90.39) | 31 / 0 (+1 durdurmada stale) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | windows-wsl2-r5 | ~22 N/s (operatörün kendi çalıştırması) | yok |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | sealed-r4 | 21.04 N/s (23 dk kazımın son 10 dk'sı) | 3 / 0 |

Linux'ta RTX 3080: kurulum 6/6 adım OK, geçersiz pencere yok, temiz durdurma. Log alıntısı:
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt).

## Ekran görüntüleri

Test rig'lerimizden, önceki sürümlerle: RTX 4090'da v0.1.41 ve RTX 5090'da erken bir derleme. Bunlar canlı
okumalardır; sizin değerleriniz karta bağlıdır.

**RTX 4090, v0.1.41:** okuma başına 69–75 N/s, çoğunlukla ~70 (10 dakikalık ortalama ~68), 9 share kabul edildi,
0 reddedildi, ~445 W.

![RTX 4090 üzerinde kazım yapan TheNewMiner v0.1.41](screenshots/rtx4090.png)

**RTX 5090, erken derleme:** 100.83 N/s (tamamlanan pencereler üzerinden canlı ortalama), ~575 W.

![RTX 5090 üzerinde kazım yapan erken bir TheNewMiner derlemesi](screenshots/rtx5090.png)

## SSS

**Windows "Windows bilgisayarınızı korudu" / "Bilinmeyen yayımcı" diyor.**
Paket kod imzalı değildir. **Ek bilgi**'ye, ardından **Yine de çalıştır**'a tıklayın.

**Antivirüsüm madenciyi işaretliyor veya siliyor.**
Microsoft Defender ve diğer antivirüs programları, kötü amaçlı yazılımlar sıklıkla madenci gizlediği için kripto para
madencilerini, kendi isteğinizle çalıştırdığınız meşru bir madenci olsa bile, çoğu zaman "istenmeyebilecek uygulamalar"
olarak işaretler. Bir dosya karantinaya alınırsa Windows Güvenliği › Virüs ve tehdit koruması › Koruma geçmişi'ni açın,
TheNewMiner öğesini seçin ve **İzin ver**'i seçin (veya çıkarılan klasörü Dışlamalar'a ekleyin). Bunu yalnızca bu
sayfadan indirilmiş ve SHA-256'sı eşleşen bir paket için yapın.

**İlk başlatma uzun sürüyor.**
İlk başlatma ~8.1 GiB'lık modeli indirir ve kartınızda bir GPU öz denetimi çalıştırır; Windows'ta ayrıca WSL2'yi de
kurabilir. Bu yalnızca bir kez olur. Sonraki başlatmalarda kazım çok daha erken başlar.

**Loglar nerede?**
Linux: `./thenewminer --logs` veya `thenewminer/.miner/thenewminer.log` dosyası.
Windows: `TheNewMiner.bat logs` (son hashrate değerleri için `TheNewMiner.bat status`).

**HiveOS?**
Bir HiveOS paketi **yakında geliyor**; hâlâ test aşamasında.

## Güvenlik notu

Madenci ve GPU motoru, paketin içinde şifrelenmiş olarak (ChaCha20-Poly1305) gelir; madenci başlarken bunları sizin
makinenizde çözer. Bu, onları kopyalamanın maliyetini artırır, ancak **kırılamaz değildir**: madencinin çalıştığı bir
makinede yönetici erişimine sahip herkes, yeterli çabayla anahtarı veya şifresi çözülmüş kodu elde edebilir.
Cüzdan adresiniz ve havuz parolanız kendi makinenizde saklanır ve yalnızca havuza gönderilir.
