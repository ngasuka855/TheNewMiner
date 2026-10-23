[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · **Türkçe** · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Bu çeviri kolaylık sağlamak amacıyla sunulmuştur; esas alınacak metin İngilizce README'dir. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner, **TensorCash** için bir GPU madencisidir. **10 GB veya daha fazla VRAM**'e sahip NVIDIA GeForce RTX 30, 40
ve 50 serisi kartlarda çalışır ve yerleşik olarak gelen **`tsc-wss.the-new.win`** havuzunda kazım yapar: havuz adresi
girmeniz gerekmez, doğrudan bağlanır.

Güncel sürüm: **v0.1.45**

## v0.1.45'teki yenilikler

- **Havuz cüzdanınızı veya parolanızı reddettiğinde açık bir mesaj.** Madenci bunu tek satırla bildirir ve yeniden
  denemek yerine durur; `--status` nedenini gösterir. Cüzdanı ve parolayı kontrol edin, ardından `-u` ve `-p` ile
  yeniden başlatın.
- **Cüzdanınız ve parolanız artık `ps` çıktısında** veya diğer kullanıcıların işlem listelerinde görünmez.
- **`./thenewminer --status`** kazım yapılıp yapılmadığını, ne zamandan beri yapıldığını, son hashrate değerini ve kabul edilen / reddedilen share'leri gösterir.
- **HiveOS desteği (deneysel):** bir HiveOS custom miner paketi ve içe aktarılabilir bir flight sheet (aşağıdaki HiveOS bölümüne bakın).
- **Güvenli güncellemeler:** eski sürüm hâlâ kazım yaparken yeni sürüm başlamayı reddeder (önce eskisini durdurmanızı
  söyler); `--stop` sonrasında program dosyalarını kendiliğinden yeniden kurar, ayarlarınızı ve modeli korur.
- **`--background`, madenci hemen sona erdiğinde artık "başladı" demez**; bunun yerine nedenini gösterir.
- **Docker ve küçük paylaşılan bellek:** bellek içi klasör çok küçükse (Docker'ın varsayılanı 64 MiB) veya program
  çalıştırmaya izin vermiyorsa, madenci bunun yerine diskteki `thenewminer/.miner/run/` klasörünü kullanır ve bunu bildirir.
- **Havuz parolaları artık herhangi bir görünür karakteri içerebilir** (noktalama işaretleri ve İngilizce dışı harfler
  de dahil): 6–128 karakter, boşluksuz.
- Daha fazla Linux sistemi: kurulum programı artık yalnızca birkaç Ubuntu sürümünü kabul etmek yerine ihtiyaç duyduğu
  şeyi (glibc 2.34 veya daha yeni) denetler.

## İndirme

| Dosya | Hedef | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz) | Linux x86_64 | `6281bf489fae843551114f09ead5498d3e47af33a07e4cfdfbdc0a5d6bdbafbb` |
| [`TheNewMiner-v0.1.45-windows-wsl2-r1.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-windows-wsl2-r1.zip) | Windows 10 / 11 (WSL2 üzerinden çalışır) | `90732441b0dc67de84809d9bf3bda4441c0bf7cd9a52fefb95d29242ad7b15e7` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/SHA256SUMS) | tüm paketlerin sağlama toplamları | |

Windows zip'i hepsi bir arada bir pakettir: Linux madencisini zaten içerir, bu yüzden yalnızca zip'e ihtiyacınız vardır.

## İndirilen dosyayı doğrulama

Paket(ler)i ve `SHA256SUMS` dosyasını aynı klasöre koyun, ardından:

```bash
# Linux (--ignore-missing: yalnızca indirdiğiniz paketler denetlenir)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Komut İstemi): çıktıyı yukarıdaki tablodaki SHA-256 ile karşılaştırın
certutil -hashfile TheNewMiner-v0.1.45-windows-wsl2-r1.zip SHA256
```

Her dosya için `OK` görünmelidir (Windows'ta: tablodaki ile aynı 64 karakter). Eşleşmiyorsa dosyayı kullanmayın.

## Tek tıkla kurulum (Linux)

Bu komutun sonundaki `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` ve `<RIG_NAME>` değerlerini (`<` `>` işaretleri dahil)
değiştirin, ardından komutu bir terminale yapıştırın. Komut, bu sürümün Linux paketini ve `SHA256SUMS` dosyasını
indirir (`curl` ile ya da `wget` ile; ikincisi yalnızca `curl` yoksa kullanılır), SHA-256'yı denetler, madenciyi
`~/thenewminer` içine açar ve arka planda başlatır:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.45 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Komut, başarısız olan ilk adımda durur: bir indirme hatası veya SHA-256 uyuşmazlığı durumunda hiçbir şey başlatılmaz
(denetim bozuk bir indirmeye karşı korur; her iki dosya da bu sayfadan gelir). Parolanın etrafındaki tek tırnakları
koruyun: bunların içinde her karakter güvenlidir (parolada geçen bir `'` karakteri `'\''` olarak yazılır). `tar`
`xz: Cannot exec` hatası verirse xz'yi kurun (`sudo apt-get install -y xz-utils`) ve komutu yeniden çalıştırın.

`~/thenewminer` içinde zaten kurulu olan bir sürümü güncellemek için önce `~/thenewminer/thenewminer --stop`
komutunu, ardından bu komutu çalıştırın (ayarlarınız ve model korunur). Önceki sürümünüz başka bir klasördeyse, önce
onu orada durdurun ya da Linux bölümünde anlatıldığı gibi orada güncelleyin (aksi halde model yeniden indirilir).

## Linux

```bash
tar -xJf TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Komut | Ne yapar |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | İlk başlatma: gerekenleri kurar, ardından arka planda kazım yapar |
| `./thenewminer --background` | Sonraki başlatmalar: ayarlarınız hatırlanır |
| `./thenewminer` | Ön planda kazım yapar (Ctrl+C durdurur) |
| `./thenewminer --logs` | Madenciyi izler (Ctrl+C görünümden çıkar; kazım devam eder) |
| `./thenewminer --status` | Kazım yapılıp yapılmadığını, son hashrate değerini ve kabul edilen / reddedilen share'leri gösterir |
| `./thenewminer --stop` | Kazımı durdurur |
| `./thenewminer --help` | Tüm seçenekler |

- `-u` TensorCash cüzdan adresiniz, `-p` havuz parolanız, `-w` havuzda görünen worker adıdır
  (varsayılan: makinenin ana bilgisayar adı; 1–64 karakter: İngilizce harfler, rakamlar, `_`, `-` veya `.`).
- Havuz parolası: 6–128 karakter, herhangi bir görünür karakter, boşluksuz. Komut satırında her zaman tek tırnak
  içine alın, ör. `-p 'my$pass!'` (tek tırnak içindeki bir `'` karakteri `'\''` olarak yazılır).
- Ayarlar `thenewminer/.miner/` içinde hatırlanır (parolayı içeren dosyayı yalnızca siz okuyabilirsiniz). Cüzdan ve
  parola komut satırından hemen kaldırılır, bu yüzden `ps` çıktısında görünmezler (kabuğunuz komutu yine de geçmişinde
  tutabilir).
- Havuz cüzdanı veya parolayı reddederse (ya da hesabı henüz onaylamadıysa), madenci bunu bildirir ve durur.
  Bunları kontrol edin ve `-u` ve `-p` ile yeniden başlatın.
- İlk başlatmada model indirilir (yaklaşık 8.1 GiB, yalnızca bir kez), kartınız için GPU motoru kurulur ve kart üzerinde
  bir öz denetim çalıştırılır, ardından kazım başlar. `[ RATE] ... N/s` gibi satırlar kazım yapıldığı anlamına gelir.
- Ubuntu ve Debian'da ilk başlatma, eksik birkaç sistem kütüphanesini `apt` ile kurabilir (sudo veya root gerekir);
  diğer sistemlerde bunları sizin kurmanız için adlarını belirtir.
- Çoklu GPU rig'leri: desteklenir, her GPU kazım yapar; bu sürüm makine başına bir GPU ile test edilmiştir.
- `tar` `xz: Cannot exec` hatası verirse önce xz'yi kurun: `sudo apt-get install -y xz-utils`.
- Docker'da veya paylaşılan belleği az olan diğer konteynerlerde madenci, program dosyalarını diskteki
  `thenewminer/.miner/run/` klasöründen çalıştırır ve bunu bildirir. Başka bir klasör seçmek için madenciyi
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` ile başlatın (yalnızca sizin yazabildiğiniz bir klasör: `chmod 700 <folder>`).
- **Güncelleme:** `./thenewminer --stop` çalıştırın, yeni paketi aynı yere açın (`thenewminer/` içine açılır ve
  ayarlarınızı ve modeli içeren `thenewminer/.miner/` klasörünü korur), ardından yeniden başlatın. Eski sürüm hâlâ
  çalışıyorsa yeni sürüm başlamayı reddeder ve önce eskisini durdurmanızı ister.

## Windows (tek tıkla)

1. `TheNewMiner-v0.1.45-windows-wsl2-r1.zip` dosyasının tamamını (sağ tık › Tümünü Ayıkla) istediğiniz yere çıkarın:
   herhangi bir klasör, herhangi bir sürücü; boşluk veya Çince karakter içeren yollar sorun değildir. Zip görünümünün
   içinden çalıştırmayın.
2. **`TheNewMiner.bat`** dosyasına çift tıklayın.
   - **İlk seferde:** Windows'u, sürücüyü ve diski denetler, gerekirse WSL2'yi açar (bir kez yönetici onayı istenir;
     bir kez yeniden başlatma gerekebilir, kurulum ardından kendiliğinden devam eder), verilerin nereye kaydedileceğini
     gösterir (Enter kabul eder), kendi Linux (WSL) ortamını oluşturur, paketle gelen madenciyi kurar, cüzdan adresinizi
     (`tc1` ile başlar), havuz parolanızı (iki kez yazılır; 6–128 karakter, herhangi bir görünür karakter, boşluksuz) ve
     worker adını (Enter = bilgisayar adı) **bir kez** sorar, modeli indirir, ardından kazıma başlar.
   - **Sonraki her seferde:** çift tıklayın, kaydedilmiş ayarlarınızla hemen kazıma başlar. Masaüstünde
     "TheNewMiner" kısayolu da oluşturulur. Zaten kazım yaparken yeniden çift tıklarsanız bunu size söyler ve logu gösterir.

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
- Havuz cüzdanı veya parolayı reddederse pencere bunu bildirir ve kazım durur: bunları yeniden girmek için
  `TheNewMiner.bat setup` çalıştırın.
- **Güncelleme:** yeni sürümü herhangi bir yere çıkarın ve onun `TheNewMiner.bat` dosyasına çift tıklayın. Kurulu
  madenciyi bulur, ayarlarınızı ve modeli korur ve yeni sürümü soru sormadan kurar. Eski klasör silinebilir.
- **Kaldırma:** `TheNewMiner.bat uninstall`.
- Ekrandaki dil Windows'u izler (English / 简体中文 / 繁體中文). Dili seçmek için herhangi bir komuta `-Lang en` (veya
  `zh-Hans`, `zh-Hant`) ekleyin, ör. `TheNewMiner.bat -Lang en`; bu yalnızca o çalıştırma için geçerlidir.

## HiveOS (deneysel)

HiveOS desteği v0.1.45 ile gelen yeni bir özelliktir ve **deneyseldir**. Aşağıdaki HiveOS custom miner paketini kullanır.

| Dosya | SHA-256 |
|---|---|
| [`thenewminer-0.1.45.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz) | `90576efd388ceeca4e456b62b568f267465320913a07c9f9736f42363b1701dc` |

Hızlı kurulum (flight sheet'i içe aktarın):

1. **Wallets** (cüzdanlar): `TSC` coin'i ve TensorCash adresinizle (`tc1...`) bir cüzdan ekleyin. Bu yalnızca bir kez
   gereklidir: HiveOS'ta bir `TSC` cüzdanının seçili olması gerekir ve madenci, flight sheet'e yazdığınız adresi
   kullanır.
2. [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json) dosyasını açın ve içeriğini kopyalayın.
   `<YOUR_WALLET>` değerini cüzdan adresinizle (arkasındaki `.%WORKER_NAME%` kısmını koruyun) ve `<YOUR_POOL_PASSWORD>`
   değerini havuz parolanızla değiştirin (JSON içinde, paroladaki bir `"` karakteri `\"` olarak, bir `\` karakteri ise
   `\\` olarak yazılır). HiveOS'ta Flight Sheets › **Import from clipboard** seçeneğini seçin, içeriği yapıştırın, `TSC`
   cüzdanınızı seçin ve kaydedin.
3. Flight sheet'i rig'inize uygulayın. Rig paketi indirir ve madenciliğe başlar.

Bunun yerine elle kurulum: Coin `TSC`, Wallet: `TSC` cüzdanınız, Pool: "Configure in miner", Miner: "Custom" olan bir
flight sheet oluşturun, **Setup Miner Config**'e tıklayın ve şunları doldurun:

| Alan | Değer |
|---|---|
| Miner name | `thenewminer` (Installation URL'den otomatik doldurulur) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz` |
| Hash algorithm | boş bırakın |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (yok sayılır: havuz yerleşiktir) |
| Pass | havuz parolanız |
| Extra config arguments | boş veya `--proxy <YOUR_PROXY>` (Proxy bölümüne bakın) |

- Havuzdaki worker adı, rig'in HiveOS'taki adıdır: yalnızca İngilizce harfler, rakamlar, `_`, `-` veya `.` kullanın
  (boşluk olmadan), en fazla 64 karakter. Başka bir worker adı kullanmak için Extra config arguments alanına
  `-w <RIG_NAME>` ekleyin.
- Model ve madenci için HiveOS sürücüsünde yaklaşık 12 GB boş alan olmalıdır (`df -h /hive/miners` ile kontrol edin).
- Model (yaklaşık 8.1 GiB) bir kez `/hive/miners/custom/.thenewminer-models` konumuna indirilir ve güncelleme
  yaptığınızda korunur.
- Cüzdan ve parola madenciye komut satırı üzerinden değil, ortam değişkenleri üzerinden iletilir.
- Havuz cüzdanı veya parolayı reddederse madenci logunda bu belirtilir ve madenci, HiveOS onu yeniden başlatmadan önce
  10 dakika bekler: flight sheet'i düzeltin ve uygulayın (bu, madenciyi hemen yeniden başlatır).
- Loglar: HiveOS'taki madenci ekranı (rig'in kabuğunda `miner`) veya `/var/log/miner/custom/custom.log`.
- **Güncelleme:** Installation URL'deki iki sürüm numarasını da değiştirin (örneğin
  `.../raw/v0.1.46/thenewminer-0.1.46.tar.gz`) ve flight sheet'i uygulayın. HiveOS yalnızca URL'deki dosya adı
  değiştiğinde kurulum yapar.
- **İlk başlatma biraz sürer:** model indirildikten sonra, RTX 30 serisi kartlarda yavaş bir rig diskinde tek seferlik
  bir GPU denetimi zaman aşımına uğrayabilir ve HiveOS madenciyi kendiliğinden iki ya da üç kez yeniden başlatır
  (yaklaşık 8 dakika). Müdahale etmeyin; ardından madencilik başlar.
- **Model zaten sizde mi?** İlk başlatmadan önce `Qwen3-8B-Q8_0-full-v2.gguf` dosyasını (yaklaşık 8.1 GiB, tam olarak bu
  dosya adıyla) rig'deki `/home/user/` dizinine kopyalayın: madenci dosyayı bulur, SHA-256 değerini denetler ve yeniden
  indirmek yerine onu kullanır.

**Rig GitHub'a erişemiyorsa:** `thenewminer-0.1.45.tar.gz` dosyasını bir PC'ye indirin (SHA-256 değerini yukarıdaki
tabloda kontrol edin), rig'e kopyalayın ve HiveOS'un özel madencileri tuttuğu yere açın. Ardından flight sheet'i her
zamanki gibi uygulayın: HiveOS "Already installed" bildirir ve paketi indirmez. Dosya adını tam olarak
`thenewminer-0.1.45.tar.gz` olarak koruyun (tarayıcı " (1)" ekleyebilir) ve flight sheet'teki Installation URL'yi
değiştirmeyin: HiveOS bu adı karşılaştırır.

```bash
# PC'de
scp thenewminer-0.1.45.tar.gz user@<RIG_IP>:/tmp/
# rig'de, root olarak (Hive Shell ya da SSH ve ardından sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.45.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.45.tar.gz
```

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
| GPU | NVIDIA GeForce RTX 30 / 40 / 50 serisi, 10 GB veya daha fazla VRAM. Çoklu GPU rig'leri: desteklenir, her GPU kazım yapar; bu sürümde makine başına bir GPU ile test edilmiştir |
| Sürücü | NVIDIA **580 veya daha yeni** (`nvidia-smi` çalışmalıdır). Windows'ta sürücüyü Windows'a kurun, asla WSL içine kurmayın |
| Linux | **glibc 2.34 veya daha yeni** bir x86_64 sistem, örneğin Ubuntu 22.04 veya daha yeni ya da Debian 12 veya daha yeni. Kurulum programı ihtiyaç duyduklarını denetler ve eksik olanları adıyla belirtir |
| Windows | Windows 10 21H2 veya daha yenisi ya da Windows 11, 64-bit; BIOS'ta sanallaştırma (Intel VT-x / AMD SVM) açık |
| Disk | yaklaşık 20 GB boş alan |
| RAM | 4 GB veya daha fazla |
| Model | yaklaşık 8.1 GiB, ilk başlatmada bir kez indirilir (paralel, çoklu yansı) ve sonrasında yeniden kullanılır |

## NVIDIA sürücünüzü güncelleyin (580 veya daha yeni)

Madencinin çalışması için NVIDIA sürücüsü **580 veya daha yeni** olmalıdır. Hangi sürümün kurulu olduğunu kontrol edin:

```bash
nvidia-smi    # başlıkta "Driver Version: ..." görünür; 580 veya daha yeni olmalıdır
```

- **Ubuntu:** `sudo ubuntu-drivers install` önerilen sürücüyü kurar (ardından sürümün 580 veya daha yeni olduğunu kontrol edin).
  Sürümü kendiniz seçmek için:
  `sudo apt install nvidia-driver-580` (veya daha yenisi; RTX 50 serisi kartlar open varyantını gerektirir, örneğin
  `nvidia-driver-580-open`). Ardından yeniden başlatın. Ya Ubuntu'nun sürücü paketlerini ya da NVIDIA'nın `.run` kurulum
  programını kullanın, asla ikisini birlikte kullanmayın.
- **Debian:** Debian'ın kendi sürücü paketleri 580'den eskidir (Debian 12'de 535, Debian 13'te 550). Daha yeni bir
  sürücüyü NVIDIA'nın deposundan
  [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html) (NVIDIA sürücü
  kurulum kılavuzu) içinde anlatıldığı gibi kurun, ardından yeniden başlatın.
- **Windows:** [NVIDIA app](https://www.nvidia.com/en-us/software/nvidia-app/) (eski adıyla GeForce Experience) ile
  güncelleyin ya da kartınızın sürücüsünü [nvidia.com](https://www.nvidia.com/en-us/drivers/) adresinden indirin (580 veya
  daha yeni). WSL2 bu Windows sürücüsünü kullanır: WSL içine asla bir Linux NVIDIA sürücüsü kurmayın.
- **HiveOS:** root olarak (Hive Shell veya SSH ile bağlanıp ardından `sudo -i`) sürümleri görmek için
  `nvidia-driver-update --list` komutunu çalıştırın, ardından örneğin `nvidia-driver-update 595` (580 veya daha yeni
  herhangi bir sürüm) ve son olarak `sreboot` komutunu çalıştırın.

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

Test rig'lerimizde gerçek havuzda yapılan testler (makine başına bir GPU). A = kabul edilen share'ler, R = reddedilen
share'ler. "Kararlı kazım", iş değişimleri arasındaki medyan hashrate'tir.

| Tarih | Kart | İşletim sistemi | Paket | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | v0.1.45 yayın derlemesi | 65.42 N/s (kararlı kazım, 16 dk; iş değişimleri dahil 62.35) | 13 / 0 (0 geçersiz) |
| 2026-10-23 | RTX 5090 | Linux | v0.1.45 yayın derlemesi | 94.18 N/s (kararlı kazım; iş değişimleri dahil 92.24) | 17 / 0 (0 geçersiz) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.44 sealed-r4 | 65.58 N/s (medyan, 16 dk) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | v0.1.44 sealed-r4 | 93.45 N/s (kararlı kazım, 44 dk; iş değişimleri dahil 90.39) | 31 / 0 (+1 durdurmada stale) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | v0.1.44 windows-wsl2-r5 | ~22 N/s (operatörün kendi çalıştırması) | yok |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | v0.1.44 sealed-r4 | 21.04 N/s (23 dk kazımın son 10 dk'sı) | 3 / 0 |

Linux'ta RTX 3080 (v0.1.44): kurulum 6/6 adım OK, geçersiz pencere yok, temiz durdurma. Log alıntısı:
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

**Madenci, havuzun cüzdanı / parolayı reddettiğini söylüyor.**
Cüzdan adresini ve havuz parolasını (ve havuzun hesabınızı onayladığını) kontrol edin, ardından yeniden başlatın:
Linux'ta `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows'ta `TheNewMiner.bat setup`. Madenci
hatalı kimlik bilgileriyle kendiliğinden yeniden denemez.

**Kazım yapıyor mu? Loglar nerede?**
Linux: `./thenewminer --status`, `./thenewminer --logs` veya `thenewminer/.miner/thenewminer.log` dosyası.
Windows: `TheNewMiner.bat status` ve `TheNewMiner.bat logs`.

## Güvenlik notu

Madenci ve GPU motoru, paketin içinde şifrelenmiş olarak (ChaCha20-Poly1305) gelir; madenci başlarken bunları sizin
makinenizde çözer. Bu, onları kopyalamanın maliyetini artırır, ancak **kırılamaz değildir**: madencinin çalıştığı bir
makinede yönetici erişimine sahip herkes, yeterli çabayla anahtarı veya şifresi çözülmüş kodu elde edebilir.
Cüzdan adresiniz ve havuz parolanız kendi makinenizde saklanır ve yalnızca havuza gönderilir.
