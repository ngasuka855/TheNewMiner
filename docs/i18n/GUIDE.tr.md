[English](../GUIDE.md) · [简体中文](GUIDE.zh-CN.md) · [繁體中文](GUIDE.zh-TW.md) · [Русский](GUIDE.ru.md) · [Español](GUIDE.es.md) · [Português (Brasil)](GUIDE.pt-BR.md) · [Français](GUIDE.fr.md) · [Deutsch](GUIDE.de.md) · **Türkçe** · [Tiếng Việt](GUIDE.vi.md) · [한국어](GUIDE.ko.md) · [日本語](GUIDE.ja.md)

> Bu çeviri kolaylık sağlamak amacıyla sunulmuştur; esas alınacak metin İngilizce README'dir. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner kılavuzu

İlk başlatmanın ötesindeki her şey: indirilen dosyayı doğrulama, Linux, Windows ve HiveOS'ta günlük kullanım,
güncellemeler, kazım yapmadığında ne yapılacağı ve SSS. İndirmeler ve hızlı başlangıç [ana sayfadadır](README.tr.md);
Windows sorunlarının kendi sayfası vardır: [Windows sorun giderme](WINDOWS-TROUBLESHOOTING.tr.md).

## İndirilen dosyayı doğrulama

| Dosya | SHA-256 |
|---|---|
| [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | `86c215dc9401209b8d2a2346252ed5cc1901527b6a51520372e3128583c63656` |
| [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | `45a459646713bef9f8285848486c0a4bd66dee65e8c63090c8d3345c998b2659` |
| [`thenewminer-0.1.47.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz) (HiveOS) | `6b2515c5bf23f60daa236ff4a8b165ce5e55265d7eecac9dd408896e1e31b784` |

Windows zip'i hepsi bir arada bir pakettir: Linux madencisini zaten içerir, bu yüzden yalnızca zip'e ihtiyacınız vardır.
Paket(ler)i ve [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS) dosyasını aynı klasöre
koyun, ardından:

```bash
# Linux (--ignore-missing: yalnızca indirdiğiniz paketler denetlenir)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Komut İstemi): çıktıyı yukarıdaki tablodaki SHA-256 ile karşılaştırın
certutil -hashfile TheNewMiner-v0.1.47-windows-wsl2-r3.zip SHA256
```

Her dosya için `OK` görünmelidir (Windows'ta: tablodaki ile aynı 64 karakter). Eşleşmiyorsa dosyayı kullanmayın.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Komut | Ne yapar |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | İlk başlatma: gerekenleri kurar, ardından arka planda kazım yapar |
| `./thenewminer --background` | Sonraki başlatmalar: ayarlarınız hatırlanır |
| `./thenewminer` | Ön planda kazım yapar (Ctrl+C durdurur) |
| `./thenewminer --logs` | Madenciyi izler (Ctrl+C görünümden çıkar; kazım devam eder) |
| `./thenewminer --status` | Kazım yapılıp yapılmadığını, son hashrate değerini ve kabul edilen / reddedilen share'leri gösterir; durduktan sonra neden durduğunu ve nasıl düzeltileceğini söyler |
| `./thenewminer --stop` | Kazımı durdurur |
| `./thenewminer --help` | Tüm seçenekler |

- `-u` TensorCash cüzdan adresiniz, `-p` havuz parolanız, `-w` havuzda görünen worker adıdır
  (varsayılan: makinenin ana bilgisayar adı; 1–64 karakter: İngilizce harfler, rakamlar, `_`, `-` veya `.`).
- Cüzdan adresi herhangi bir şey başlamadan önce denetlenir: yazım hatası içeren (sağlama toplamı eşleşmeyen) bir adres
  veya örnek adres, neyin yanlış olduğu belirtilerek reddedilir.
- Havuz parolası: 6–128 karakter, herhangi bir görünür karakter, boşluksuz. Komut satırında her zaman tek tırnak
  içine alın, ör. `-p 'my$pass!'` (tek tırnak içindeki bir `'` karakteri `'\''` olarak yazılır).
- Ayarlar `thenewminer/.miner/` içinde hatırlanır (parolayı içeren dosyayı yalnızca siz okuyabilirsiniz). Cüzdan ve
  parola komut satırından hemen kaldırılır, bu yüzden `ps` çıktısında görünmezler (kabuğunuz komutu yine de geçmişinde
  tutabilir).
- Tek GPU'lu bir makinenin ilk başlatmasında madenci, model indirmesinden **önce** cüzdanı ve parolayı havuzla denetler;
  böylece bir yazım hatası saniyeler içinde bulunur (çoklu GPU'lu bir rig bunu kazım başladığında fark eder). Havuz
  bunları reddederse madenci bunu bildirir ve durur: bunları kontrol edin ve `-u` ve `-p` ile yeniden başlatın.
- İlk başlatmada model indirilir (yaklaşık 8.1 GiB, yalnızca bir kez), kartınız için GPU motoru kurulur ve kart üzerinde
  bir öz denetim çalıştırılır, ardından kazım başlar. `[ RATE] ... N/s` gibi satırlar kazım yapıldığı anlamına gelir.
  Log dosyası `thenewminer/.miner/thenewminer.log` dosyasıdır.
- Standart bir Ubuntu 22.04 / 24.04 (WSL'deki dahil) madencinin ihtiyaç duyduğu tüm kütüphanelere zaten sahiptir, bu
  yüzden hiçbir şey kurulmaz. Bir kütüphane eksikse (Docker imajları, minimal sistemler) Ubuntu, Debian ve HiveOS'ta ilk
  başlatma onu listeler ve `apt` ile kurar (sudo veya root gerekir); diğer sistemlerde sizin kurmanız için adını
  belirtir.
- Çoklu GPU rig'leri: desteklenir, her GPU kazım yapar; bu sürüm makine başına bir GPU ile test edilmiştir.
- Docker'da veya paylaşılan belleği az olan diğer konteynerlerde madenci, program dosyalarını diskteki
  `thenewminer/.miner/run/` klasöründen çalıştırır ve bunu bildirir. Başka bir klasör seçmek için madenciyi
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` ile başlatın (yalnızca sizin yazabildiğiniz bir klasör: `chmod 700 <folder>`).
- **Güncelleme:** `./thenewminer --stop` çalıştırın, yeni paketi aynı yere açın (`thenewminer/` içine açılır ve
  ayarlarınızı ve modeli içeren `thenewminer/.miner/` klasörünü korur), ardından yeniden başlatın. Eski sürüm hâlâ
  çalışıyorsa yeni sürüm başlamayı reddeder ve önce eskisini durdurmanızı ister. Bunun yerine yeni bir klasöre
  açılırsa eski sürümün modelini (aynı boyut ve SHA-256) bulur ve yeniden kullanır: 8.1 GiB'lık ikinci bir indirme
  olmaz. Orada `-u -p -w` değerlerini yeniden verin.

### Kazım yapmadığında

`./thenewminer --status` madencinin neden durduğunu ve nasıl düzeltileceğini söyler. En sık görülen mesajlar:

| Gördüğünüz | Ne yapmalı |
|---|---|
| `bash: ./thenewminer: Permission denied` | Klasör program çalıştırmaya izin vermiyor (bir `noexec` bağlama noktası, örneğin bazı `/tmp` dizinleri, veya bir USB bellek). Paketi ev klasörünüze açın: `cd ~ && tar -xJf <the package>.tar.xz && cd thenewminer` |
| `this package is on a FAT (vfat) drive` (veya exFAT / NTFS ya da `Windows (DrvFs, 9p)`: WSL'deki `/mnt/d` gibi bir Windows sürücüsü) | Böyle bir sürücü madencinin özel ayarlarını (600 modu) tutamaz. Madencinin yazdırdığı `mkdir -p ~/thenewminer && cp -a ...` satırını yapıştırın: klasörü Linux ev klasörünüze kopyalar ve orada başlatır |
| `this system has no xz` | `sudo apt-get install -y xz-utils` (root olarak: `sudo` olmadan), ardından yeniden başlatın |
| `This system is missing what TheNewMiner needs` | Madencinin yazdırdığı `sudo apt-get install -y ...` satırını çalıştırın, ardından yeniden başlatın |
| `No NVIDIA GPU found on this machine` | Hiçbir NVIDIA kartı görülmüyor. Kartın yerine oturduğunu ve güç aldığını kontrol edin; bulut makinesinde NVIDIA GPU'lu bir makine seçin |
| `NVIDIA driver ... is too old` | Belirtilen sürücüyü (580 veya daha yeni) kurun, yeniden başlatın ve madenciyi yeniden başlatın (aşağıdaki "NVIDIA sürücünüzü güncelleyin" bölümüne bakın) |
| `the pool rejected the wallet / password` | `-u` ve `-p` değerlerini ve hesabın havuz web sitesinde var olduğunu kontrol edin, ardından `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' --background` |
| `no download source answered` | Model indirmesi internete ulaşamıyor. Bir proxy arkasındaysanız: `https_proxy=http://<HOST>:<PORT> ./thenewminer --background`; ya da modeli kopyalayın (aşağıdaki "Model zaten sizde mi?" bölümüne bakın) |
| `cannot reach the pool` / `pool disconnected` | İnternet bağlantısını, DNS'i ve güvenlik duvarının giden TCP 443 bağlantılarına izin verdiğini kontrol edin; ağınız havuzu engelliyorsa `--proxy` kullanın (aşağıdaki "Proxy" bölümüne bakın). Madenci kendiliğinden yeniden bağlanır |

### Yeniden başlatmadan sonra otomatik başlatma (isteğe bağlı)

Madenci, bilgisayar yeniden başlatıldıktan sonra kendiliğinden başlamaz (HiveOS bunu sizin için yapar). Linux'ta ya bir
`crontab` satırı ekleyin:

```bash
crontab -e      # ardından şu satırı ekleyin (kendi klasörünüzle):
@reboot cd /home/<YOU>/thenewminer && ./thenewminer --background
```

ya da bir systemd kullanıcı hizmeti oluşturun (oturum açmadan başlaması için bir kez `loginctl enable-linger <YOU>`):

```ini
# ~/.config/systemd/user/thenewminer.service, ardından: systemctl --user enable --now thenewminer
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

Çıkış kodu 77, havuzun cüzdanı / parolayı reddettiği anlamına gelir: yeniden başlatmak işe yaramaz, bu yüzden hizmet
orada durur.

## Windows

1. `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` dosyasının tamamını (sağ tık › Tümünü Ayıkla) istediğiniz yere çıkarın:
   herhangi bir klasör, herhangi bir sürücü; boşluk veya Çince karakter içeren yollar sorun değildir. Zip görünümünün
   içinden çalıştırmayın.
2. **`TheNewMiner.bat`** dosyasına çift tıklayın.
   - **İlk seferde:** TEK bir pencere önce havuz hesabınızı sorar: cüzdan adresi (`tc1` ile başlar; sağlama toplamı
     denetlenir ve gösterilen örnek gerçek bir cüzdan değildir), havuz parolası (6–128 karakter, herhangi bir görünür
     karakter, boşluksuz) ve worker adı (bilgisayar adı önceden doldurulur). Üçü de doğru olana kadar OK düğmesi gri
     kalır. OK ve Evet'ten sonra başka hiçbir şey sorulmaz: Windows'u, sürücüyü ve diski denetler, gerekirse WSL2'yi
     açar (bir kez yönetici onayı istenir; bir kez yeniden başlatma gerekebilir, kurulum ardından kendiliğinden devam
     eder), verilerin nereye kaydedileceğini gösterir, kendi Linux (WSL) ortamını oluşturur, paketle gelen madenciyi
     kurar, modeli indirir, ardından kazıma başlar. Masaüstü olmadan (SSH üzerinden) aynı sorular konsolda sorulur.
   - **Sonraki her seferde:** çift tıklayın, kaydedilmiş ayarlarınızla hemen kazıma başlar. Masaüstünde
     "TheNewMiner" kısayolu da oluşturulur. Zaten kazım yaparken yeniden çift tıklarsanız bunu size söyler ve logu
     gösterir.
   - **Kurulum durursa** pencere hangi adımda durduğunu ve ne yapılacağını söyler, `TheNewMiner.bat` de bunu yeniden
     gösterir. [Windows sorun giderme](WINDOWS-TROUBLESHOOTING.tr.md) sayfasına bakın.

Günlük komutlar (çıkarılan klasörde bir Komut İstemi açın):

| Komut | Ne yapar |
|---|---|
| `TheNewMiner.bat` | Gerekirse kurar, ardından bu pencerede kazım yapar (pencereyi kapatmak kazımı durdurur) |
| `TheNewMiner.bat background` | Pencere olmadan kazım yapar |
| `TheNewMiner.bat logs` | Madenciyi izler (Ctrl+C görünümden çıkar; kazım devam eder) |
| `TheNewMiner.bat status` | Çalışıp çalışmadığını ve son hashrate değerlerini gösterir |
| `TheNewMiner.bat stop` | Kazımı durdurur |
| `TheNewMiner.bat setup` | Cüzdan / parola / worker adını yeniden girer (Cancel mevcut değerleri korur) |
| `TheNewMiner.bat uninstall` | Madenciyi, veri klasörünü ve kısayolu kaldırır (önce sorar) |

- **Veriler nereye gider:** madenci ve model, ilk kurulumda seçilen bir veri klasöründe bulunur (varsayılan olarak
  zip'i çıkardığınız sürücünün kökündeki `TheNewMinerData`, ör. `D:\TheNewMinerData`), çıkarılan klasörde değil.
  Orada yaklaşık 20 GB boş alan gerekir. Başka bir klasör seçmek için: `TheNewMiner.bat setup -DataDir <folder>`.
- **Ubuntu:** kurulum Ubuntu 24.04'ü `wsl --install` ile kurar. Bu indirme bağlanamadığında Ubuntu'nun kendi WSL imajı
  `ubuntu-24.04.5-wsl-amd64.wsl` dosyasını Çin'deki herkese açık bir yansıdan (Tsinghua TUNA, ardından USTC, ardından
  Aliyun) indirir ve yalnızca SHA-256 değeri tam olarak
  `bb415d824822c4b878125729af451a5d18fb13d1cf5cbed9a7393ad64ac6039e` ise kullanır.
- Havuz cüzdanı veya parolayı reddederse pencere bunu bildirir ve kazım durur: bunları yeniden girmek için
  `TheNewMiner.bat setup` çalıştırın.
- **Güncelleme:** yeni sürümü herhangi bir yere çıkarın ve onun `TheNewMiner.bat` dosyasına çift tıklayın. Kurulu
  madenciyi bulur, ayarlarınızı ve modeli korur ve yeni sürümü soru sormadan kurar. Önceki bir sürümle ya da elle başka
  bir WSL dağıtımının içine (örneğin `Ubuntu-24.04`) yapılmış bir kurulum bulunur ve bulunduğu yerde güncellenir: ikinci
  bir kurulum, yeni bir Ubuntu veya ikinci bir model indirmesi olmaz. Orada güncellenemezse kurulum durur ve nedenini
  söyler. Eski klasör silinebilir.
- **Kaldırma:** `TheNewMiner.bat uninstall`. Size ait bir WSL dağıtımını asla kaldırmaz.
- Windows paketi İngilizce konuşur.

## HiveOS (deneysel)

HiveOS desteği **deneyseldir** (v0.1.45'ten beri; v0.1.45 gerçek bir HiveOS rig'inde çalıştı, v0.1.46'dan bu yana
yapılan değişiklikler ise henüz bir HiveOS rig'inde çalışmadı). HiveOS custom miner paketi `thenewminer-0.1.47.tar.gz`
dosyasını kullanır (SHA-256 değeri "İndirilen dosyayı doğrulama" bölümündeki tablodadır).

Hızlı kurulum (flight sheet'i içe aktarın):

1. **Wallets** (cüzdanlar): `TSC` coin'i ve TensorCash adresinizle (`tc1...`) bir cüzdan ekleyin. Bu yalnızca bir kez
   gereklidir: HiveOS'ta bir `TSC` cüzdanının seçili olması gerekir ve madenci, flight sheet'e yazdığınız adresi
   kullanır.
2. [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) dosyasını açın ve içeriğini
   kopyalayın. `<YOUR_WALLET>` değerini cüzdan adresinizle (arkasındaki `.%WORKER_NAME%` kısmını koruyun) ve
   `<YOUR_POOL_PASSWORD>` değerini havuz parolanızla değiştirin (JSON içinde, paroladaki bir `"` karakteri `\"` olarak,
   bir `\` karakteri ise `\\` olarak yazılır). HiveOS'ta Flight Sheets › **Import from clipboard** seçeneğini seçin,
   içeriği yapıştırın, `TSC` cüzdanınızı seçin ve kaydedin.
3. Flight sheet'i rig'inize uygulayın. Rig paketi indirir ve madenciliğe başlar.

Bunun yerine elle kurulum: Coin `TSC`, Wallet: `TSC` cüzdanınız, Pool: "Configure in miner", Miner: "Custom" olan bir
flight sheet oluşturun, **Setup Miner Config**'e tıklayın ve şunları doldurun:

| Alan | Değer |
|---|---|
| Miner name | `thenewminer` (Installation URL'den otomatik doldurulur) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` |
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
- **Bir şey başarısız olduğunda** (örneğin havuz cüzdanı veya parolayı reddettiğinde) madenci logu ve HiveOS paneli
  nedenini gösterir ve rig giderek uzayan aralarla (bir ila on dakika) yeniden dener. Flight sheet'i düzeltip uygulayın:
  bu, madenciyi hemen yeniden başlatır.
- Loglar: HiveOS'taki madenci ekranı (rig'in kabuğunda `miner`) veya `/var/log/miner/custom/custom.log`.
- **Güncelleme:** Installation URL'deki iki sürüm numarasını da değiştirin (örneğin
  `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`) ve flight sheet'i uygulayın. HiveOS yalnızca URL'deki dosya adı
  değiştiğinde kurulum yapar.
- **İlk başlatma** modeli indirir ve GPU motorunu kurar; bu birkaç dakika sürer.
- **Model zaten sizde mi?** İlk başlatmadan önce rig'deki `/home/user/` dizinine kopyalayın (sonraki bölüme bakın).

**Rig GitHub'a erişemiyorsa:** `thenewminer-0.1.47.tar.gz` dosyasını bir PC'ye indirin (SHA-256 değerini kontrol edin),
rig'e kopyalayın ve HiveOS'un özel madencileri tuttuğu yere açın. Ardından flight sheet'i her zamanki gibi uygulayın:
HiveOS "Already installed" bildirir ve paketi indirmez. Dosya adını tam olarak `thenewminer-0.1.47.tar.gz` olarak
koruyun (tarayıcı " (1)" ekleyebilir) ve flight sheet'teki Installation URL'yi değiştirmeyin: HiveOS bu adı
karşılaştırır.

```bash
# PC'de
scp thenewminer-0.1.47.tar.gz user@<RIG_IP>:/tmp/
# rig'de, root olarak (Hive Shell ya da SSH ve ardından sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.47.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.47.tar.gz
```

## Model zaten sizde mi? Kopyalayın

Madencinin 8.1 GiB'lık tek bir model dosyasına ihtiyacı vardır: `Qwen3-8B-Q8_0-full-v2.gguf` (`8709518112` bayt, SHA-256
`408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`). Dosya bu makinede ya da ağınızdaki başka bir
bilgisayarda zaten varsa, yeniden indirmek yerine kopyalayın.

Madenci herhangi bir şey indirmeden önce dosyayı tam adına ve boyutuna göre şu yerlerde arar: kendi model klasöründe,
önceki TheNewMiner klasörlerinde, ev klasörünüzde (ve onun bir ya da iki alt düzeyinde veya oradaki bir `models`
klasöründe), `/home` altındaki ev klasörlerinde, `/data`, `/mnt/*`, `/media/*`, `/opt`, `/srv`,
`/workspace`, `/var/lib` içinde ve Hugging Face / ModelScope önbelleklerinde. Orada hiçbir şey bulunamazsa aynı diskteki
bu klasörlerde en fazla 20 saniye boyunca tam olarak bu boyutta herhangi bir `.gguf` dosyası arar (bağlı diğer diskler
yalnızca ada göre denetlenir). Bulunan bir dosya **yalnızca SHA-256 değeri eşleşirse** kullanılır; madenci ardından
dosyaya bağlantı oluşturur (aynı disk) ya da onu kendi model klasörüne kopyalar, böylece sonraki her sürüm onu hemen
bulur. Dosyayı doğrudan da belirtebilirsiniz: `./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`.

**Linux:** dosyayı ev klasörünüze kopyalayın, ardından madenciyi her zamanki gibi başlatın. Aynı ağdaki bir Mac'ten veya
PC'den (Windows 10 ve 11'de PowerShell içinde `scp` vardır) `user` ve `<MINER_IP>` değerlerini madencinizin kullanıcı
adı ve adresiyle değiştirin:

```bash
# doğrudan madenci makinesinde
cp /path/to/Qwen3-8B-Q8_0-full-v2.gguf ~/
# ya da başka bir bilgisayardan
scp Qwen3-8B-Q8_0-full-v2.gguf user@<MINER_IP>:~/
```

**HiveOS:** dosyayı rig'deki `/home/user/` dizinine kopyalayın (HiveOS'un SSH kullanıcı adı `user`), ardından flight
sheet'i uygulayın. Madenci dosyayı orada bulur, denetler ve paketin yanında (`/hive/miners/custom/.thenewminer-models`)
saklar; böylece yeni bir sürüm onu bir daha asla indirmez. HiveOS'un diski küçüktür: önce `disk-expand` çalıştırın
(`df -h /hive/miners` 12 GB'tan az boş alan gösteriyorsa).

```bash
scp Qwen3-8B-Q8_0-full-v2.gguf user@<RIG_IP>:/home/user/
```

**Windows:** ilk `setup` TheNewMiner'ın WSL dağıtımını oluşturduktan sonra dosyayı bir Komut İstemi penceresinden bu
dağıtıma kopyalayın (`D:\Models` yolunu kopyanızın bulunduğu yerle değiştirin). İndirme zaten başlamışsa pencereyi
kapatın, bu komutları çalıştırın, ardından TheNewMiner'ı yeniden başlatın: kopyayı bulur ve indirmeyi atlar. Kopyalama
birkaç dakika sürer ve ilerleme göstermez.

```bat
wsl -d TheNewMiner -u root -- mkdir -p ~/thenewminer/models
wsl -d TheNewMiner -u root -- cp /mnt/d/Models/Qwen3-8B-Q8_0-full-v2.gguf ~/thenewminer/models/
```

## Cüzdanı, havuz parolasını veya worker adını değiştirme

Havuz, her ödeme adresini kalıcı olarak kendi havuz hesabına bağlar: havuz sayfası
[tsc.the-new.win](https://tsc.the-new.win), TSC ödeme adresini "永久绑定 · 不可修改" (kalıcı olarak bağlı, değiştirilemez)
olarak tanımlar ve değiştirmek için yeni adresi doğrudan madencide kullanmanızı söyler. Bu nedenle farklı bir cüzdan
için kendi havuz hesabı ve kendi havuz parolası gerekir (bunları önce havuz sayfasında oluşturun). Havuz parolasını
havuz sayfasında değiştirirseniz parolayı her madencide güncelleyin. Havuz unutulan bir parolayı kurtaramaz.

**Linux:** madenciyi durdurun, ardından yeni ayarlarla bir kez başlatın; yeni ayarlar hatırlanan ayarların yerini alır:

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

`-u` ve `-p` seçeneklerini birlikte verin: yalnızca biri tek başına hatırlanan cüzdanı ve parolayı değiştirmez. Worker
adı tek başına değiştirilebilir: `./thenewminer -w <RIG_NAME> --background` (`--stop` komutundan sonra).

**Windows:** `TheNewMiner.bat setup` çalıştırın. Worker adınız önceden doldurulmuş olarak hesap penceresini açar; cüzdanı
ve havuz parolasını girin, ardından madenciyi her zamanki gibi başlatın (`TheNewMiner.bat` dosyasına çift tıklayın).

**HiveOS:** flight sheet'i düzenleyin: yeni cüzdan adresini "Wallet and worker template" alanına
(`<NEW_WALLET>.%WORKER_NAME%`), yeni havuz parolasını Pass alanına yazın, ardından kaydedip uygulayın. Worker adı,
rig'in HiveOS'taki adıdır (veya Extra config arguments içinde `-w <RIG_NAME>`).

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

- **Ubuntu:** `sudo ubuntu-drivers install` önerilen sürücüyü kurar (ardından sürümün 580 veya daha yeni olduğunu
  kontrol edin). Sürümü kendiniz seçmek için:
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
Cüzdan adresini ve havuz parolasını kontrol edin (hesap havuz web sitesinde mevcut olmalıdır), ardından yeniden başlatın:
Linux'ta `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows'ta `TheNewMiner.bat setup`. Madenci
hatalı kimlik bilgileriyle kendiliğinden yeniden denemez.

**Kazım yapıyor mu? Loglar nerede?**
Linux: `./thenewminer --status`, `./thenewminer --logs` veya `thenewminer/.miner/thenewminer.log` dosyası.
Windows: `TheNewMiner.bat status` ve `TheNewMiner.bat logs`.

**RTX 3080'im (10 GB) Windows'ta Linux'tan çok daha yavaş kazım yapıyor.**
Windows'ta VRAM'i başka programlar da paylaşır. [Windows sorun giderme](WINDOWS-TROUBLESHOOTING.tr.md) sayfasına bakın:
donanım hızlandırması açık tarayıcıları ve Docker Desktop'ı kapatın.
