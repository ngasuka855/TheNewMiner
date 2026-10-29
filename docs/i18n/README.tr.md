# TheNewMiner

TheNewMiner, 10 GB veya daha fazla VRAM'e sahip NVIDIA GeForce RTX 30, 40 ve 50 serisi kartlar için bir **TensorCash** GPU madencisidir; **`tsc-wss.the-new.win`** havuzu yerleşik olarak gelir.

[English](../../README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · **Türkçe** · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Bu çeviri kolaylık sağlamak amacıyla sunulmuştur; esas alınacak metin İngilizce README'dir. *(This translation is provided for convenience; the English README is authoritative.)*

## İndirme

Güncel sürüm: **v0.1.47** · Havuz web sitesi: [tsc.the-new.win](https://tsc.the-new.win/#/home)

| Sistem | İndirme | Ardından |
|---|---|---|
| Linux x86_64 | [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | ya da aşağıdaki tek tıkla kurulumu kullanın |
| Windows 10 / 11 | [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | zip'i çıkarın, `TheNewMiner.bat` dosyasına çift tıklayın |
| HiveOS (deneysel) | [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) | flight sheet'i içe aktarın (aşağıdaki HiveOS bölümüne bakın) |

Sağlama toplamları: [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS). Nasıl
denetleneceği [kılavuzda](GUIDE.tr.md) anlatılır. Bir dosya eşleşmiyorsa onu kullanmayın.

## Başlamadan önce: havuz hesabınızı oluşturun

Kazım için bir havuz hesabı gerekir: TSC ödeme adresiniz (cüzdan) ve havuz web sitesinde seçtiğiniz bir havuz parolası.
Sitenin İngilizce bir sürümü vardır (dil menüsünü kullanın).

1. [tsc.the-new.win](https://tsc.the-new.win/#/home) adresini açın ve **Start mining** (开始挖矿) bölümüne gidin.
2. **1. Bind Wallet** altında **TSC payout address** alanına ödeme adresinizi girin (`tc1...`; dikkatle kontrol edin:
   yanlış bir adrese gönderilen kazım geliri geri alınamaz), bir **Pool Password** seçin (6–128 karakter) ve bunu
   **Confirm Password** altına yeniden girin.
3. **Apply to join pool** düğmesine tıklayın. Sayfa, cüzdanın bağlandığını onaylar; hesap hemen kullanılabilir.
4. Madencide aynı cüzdanı ve parolayı kullanın: Linux'ta `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows'ta
   `TheNewMiner.bat` dosyasının hesap penceresi, HiveOS'ta ise flight sheet. Kazım başladıktan yaklaşık 3–5 dakika sonra
   rig'iniz havuz web sitesinde **My Miners** altında görünür.

Havuz, unutulan bir havuz parolasını kurtaramaz ve ödeme adresi daha sonra değiştirilemez. Havuz parolası cüzdanınızın
özel anahtarı değildir: özel anahtarınızı asla kimseye vermeyin.

## Tek tıkla kurulum (Linux)

Bu komutun sonundaki `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` ve `<RIG_NAME>` değerlerini (`<` `>` işaretleri dahil)
cüzdanınız, havuz parolanız ve bu rig için bir adla değiştirin, sonra komutu bir terminale yapıştırın. Komut, Linux
paketini ve `SHA256SUMS` dosyasını indirir, SHA-256'yı denetler, madenciyi `~/thenewminer` içine açar ve arka planda
başlatır:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.47 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Bir indirme hatası veya SHA-256 uyuşmazlığı durumunda hiçbir şey başlatılmaz. Parolanın etrafındaki tek tırnakları
koruyun (parolada geçen bir `'` karakteri `'\''` olarak yazılır). Tek GPU'lu bir makinede madenci, modeli indirmeden
önce (yaklaşık 8.1 GiB, yalnızca bir kez) cüzdanı ve parolayı havuzla denetler. Ardından `./thenewminer --logs` kazımı
gösterir, `./thenewminer --status` ise çalışıp çalışmadığını veya neden durduğunu söyler. Güncellemek için önce
`~/thenewminer/thenewminer --stop` komutunu, ardından aynı komutu çalıştırın: ayarlarınız ve model korunur. Diğer her
şey (günlük komutlar, güncellemeler, otomatik başlatma, kazım yapmadığında ne yapılacağı) [kılavuzda](GUIDE.tr.md)
bulunur.

## Windows

1. `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` dosyasının tamamını (sağ tık › Tümünü Ayıkla) istediğiniz yere çıkarın.
2. **`TheNewMiner.bat`** dosyasına çift tıklayın. İlk seferde TEK bir pencere havuz hesabınızı sorar (cüzdan, havuz
   parolası, worker adı); bundan sonra başka hiçbir şey sorulmaz: WSL2'yi ve Ubuntu'yu kurar, paketle gelen madenciyi
   kurar, modeli indirir ve kazıma başlar. Sonraki çift tıklamalarda kazım hemen başlar. Windows önce "Windows
   bilgisayarınızı korudu" diyebilir (paket kod imzalı değildir): **Ek bilgi**'ye, ardından **Yine de çalıştır**'a
   tıklayın. WSL'yi açmak bir kez yönetici izni ister ve bir yeniden başlatma gerektirebilir; kurulum ardından
   kendiliğinden devam eder.
3. Önceki bir sürüm bulunur ve ayarlarınız ve modelle birlikte yerinde güncellenir.

Durursa veya yavaş kazım yaparsa [Windows sorun giderme](WINDOWS-TROUBLESHOOTING.tr.md) sayfasına bakın (DNS, Windows
sürücüleri, tarayıcıların ve Docker Desktop'ın kullandığı VRAM, saat).

## HiveOS (deneysel)

1. **Wallets** (cüzdanlar): `TSC` coin'i ve TensorCash adresinizle (`tc1...`) bir cüzdan ekleyin.
2. [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) dosyasını açın, içeriğini
   kopyalayın ve `<YOUR_WALLET>` (arkasındaki `.%WORKER_NAME%` kısmını koruyun) ile `<YOUR_POOL_PASSWORD>` değerlerini
   değiştirin. HiveOS'ta Flight Sheets › **Import from clipboard** seçeneğini seçin, içeriği yapıştırın, `TSC`
   cüzdanınızı seçin ve kaydedin.
3. Flight sheet'i rig'inize uygulayın. `thenewminer-0.1.47.tar.gz` paketini kurar ve kazıma başlar.

Elle kurulum, güncellemeler ve GitHub'a erişemeyen bir rig için [kılavuza](GUIDE.tr.md) bakın.

## v0.1.47'deki yenilikler

- **Daha kolay kurulum:** tek GPU'lu bir makinede madenci, 8.1 GiB'lık model indirmesinden **önce** cüzdanınızı ve
  havuz parolanızı havuzla denetler; böylece bir yazım hatası saniyeler içinde bulunur. Yazım hatası içeren (sağlama
  toplamı eşleşmeyen) bir cüzdan adresi veya örnek adres, neyin yanlış olduğu belirtilerek reddedilir.
- **Windows: kimlik bilgileri yalnızca bir kez sorulur,** başlangıçta tek bir pencerede, uzun süren herhangi bir
  işlemden önce. Önceki bir sürümle yapılmış bir kurulum, başka bir WSL dağıtımında olsa bile, bulunur ve **yerinde
  güncellenir** (ayarlar ve model korunur). Ubuntu Microsoft'tan indirilemezse kurulum, Ubuntu'nun kendi WSL imajını
  Çin'deki bir yansıdan alır ve yalnızca SHA-256 değeri eşleşirse kullanır.
- **Standart bir Ubuntu'da apt yok:** Ubuntu 22.04 / 24.04 (WSL'deki dahil) madencinin ihtiyaç duyduğu her şeye zaten
  sahiptir, bu yüzden hiçbir şey kurulmaz. Bir şey eksikse ilk başlatma, kurmadan önce eksikleri listeler.
- **Windows sürücüsündeki bir paket** (WSL'deki `/mnt/d` gibi) hemen reddedilir; paketi Linux ev klasörünüze kopyalayıp
  orada başlatan, yapıştırmaya hazır bir satır gösterilir.
- **Açık ağ mesajları:** WSL içinde DNS çalışmadığında kurulum o adımda durur ve çözümü gösterir; internete veya havuza
  ulaşamayan bir madenci neyin kontrol edileceğini söyler.
- **Model yeniden kullanılır:** yeni bir klasöre yapılan güncelleme, önceki bir kurulumun modelini (aynı boyut ve
  SHA-256) bulur ve yeniden indirmez.
- **`--status` nedenini söyler:** madencinin neden durduğunu ve nasıl düzeltileceğini, `--background` ile başlatmadan
  sonra da gösterir.
- **HiveOS:** her hata artık giderek uzayan aralarla (bir ila on dakika) bekleyip yeniden dener ve nedenini logda ve
  panelde gösterir.

Tüm notlar: [`docs/releases/RELEASE-NOTES-v0.1.47.md`](../releases/RELEASE-NOTES-v0.1.47.md).

## Bilinen sorunlar

- **Windows'taki 10 GB'lık kartlar tam hız için yaklaşık 9.3 GB boş VRAM'e ihtiyaç duyar.** Daha azıyla madenci daha
  yavaş bir yerleşim kullanır (bir RTX 3080 10 GB, 21–22'den yaklaşık 9 N/s'ye düştü). Donanım hızlandırması açık bir
  tarayıcı veya Docker Desktop bu belleği kullanabilir: bunları kapatın (veya tarayıcının donanım hızlandırmasını
  kapatın), ardından kazımı yeniden başlatın.
- **Windows saati:** madenci bir saat hatası bildirirse Windows Ayarlar › Saat ve dil › Tarih ve saat bölümünü açın,
  "Saat dilimini otomatik olarak ayarla" ve "Saati otomatik olarak ayarla" seçeneklerini açın ve **Şimdi eşitle**'ye
  tıklayın, ardından `wsl --shutdown` çalıştırın ve yeniden başlatın.

## Hashrate değerleri

Kendi rig'lerimizde gerçek havuzda ölçülen en iyi değerler. Bunlar **en fazla** değerlerdir, bir vaat değildir: aynı
modeldeki kartlar yaklaşık 5–8% farklılık gösterir ve kart aynı zamanda bir ekranı sürüyorsa veya GPU'yu başka
programlar kullanıyorsa hashrate düşer.

| GPU | Hashrate |
|---|---|
| RTX 5090 | en fazla ~99 N/s |
| RTX 4090 | en fazla ~70 N/s |
| RTX 3080 10 GB | en fazla ~22 N/s |

Sürüm başına tarihli ölçümler: [test sonuçları](../TEST-RESULTS.md). Projenin kilometre taşları:
[geliştirme kaydı](../DEVELOPMENT-RECORD.md).

## Sorun bildirme

[GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) üzerinde bir issue açın ve hata raporu
formunu doldurun: TheNewMiner sürümü, sisteminiz, GPU model(ler)i ve sürücü, `./thenewminer --status` çıktısı ve son log
satırları. Issue'lar herkese açıktır: **havuz parolanızı veya cüzdanınızın özel anahtarını asla paylaşmayın** ve
yapıştırdığınız her şeyden parolayı (ve isterseniz cüzdan adresinizi) çıkarın.

## Güvenlik notu

Madenci ve GPU motoru, paketin içinde şifrelenmiş olarak (ChaCha20-Poly1305) gelir; madenci başlarken bunları sizin
makinenizde çözer. Bu, onları kopyalamanın maliyetini artırır, ancak **kırılamaz değildir**: madencinin çalıştığı bir
makinede yönetici erişimine sahip herkes, yeterli çabayla anahtarı veya şifresi çözülmüş kodu elde edebilir.
Cüzdan adresiniz ve havuz parolanız kendi makinenizde saklanır ve yalnızca havuza gönderilir.
