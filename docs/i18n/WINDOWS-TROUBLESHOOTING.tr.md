[English](../WINDOWS-TROUBLESHOOTING.md) · [简体中文](WINDOWS-TROUBLESHOOTING.zh-CN.md) · [繁體中文](WINDOWS-TROUBLESHOOTING.zh-TW.md) · [Русский](WINDOWS-TROUBLESHOOTING.ru.md) · [Español](WINDOWS-TROUBLESHOOTING.es.md) · [Português (Brasil)](WINDOWS-TROUBLESHOOTING.pt-BR.md) · [Français](WINDOWS-TROUBLESHOOTING.fr.md) · [Deutsch](WINDOWS-TROUBLESHOOTING.de.md) · **Türkçe** · [Tiếng Việt](WINDOWS-TROUBLESHOOTING.vi.md) · [한국어](WINDOWS-TROUBLESHOOTING.ko.md) · [日本語](WINDOWS-TROUBLESHOOTING.ja.md)

> Bu çeviri kolaylık sağlamak amacıyla sunulmuştur; esas alınacak metin İngilizce README'dir. *(This translation is provided for convenience; the English README is authoritative.)*

# Windows sorun giderme

Windows'ta TheNewMiner, Linux madencisini WSL2 içinde çalıştırır. Kurulum durduğunda pencere hangi adımda durduğunu ve ne
yapılacağını söyler, `TheNewMiner.bat` de bunu yeniden gösterir (ayrıca
`%LOCALAPPDATA%\TheNewMiner\last-setup-failure.txt` dosyasında saklanır). Bu sayfa sık görülen durumları açıklar.
Kurulum ve günlük komutlar [kılavuzdadır](GUIDE.tr.md).

## WSL içinde DNS çalışmıyor

Kurulum 4. adımda WSL'in havuzu ve model indirme adresini çözümleyip çözümleyemediğini denetler. Çözümleyemezse kurulum
orada durur ve kendiliğinden hiçbir şeyi değiştirmez. `%UserProfile%\.wslconfig` dosyasını Not Defteri ile açın (dosya
yoksa oluşturun) ve şu iki satırı ekleyin (Windows 11 22H2 veya daha yenisinde
`networkingMode=mirrored` satırı `[wsl2]` altında da çalışır):

```ini
[wsl2]
dnsTunneling=true
```

Ardından bir Komut İstemi'nde `wsl --shutdown` çalıştırın (bu, Docker Desktop'ınki dahil tüm WSL dağıtımlarını durdurur)
ve `TheNewMiner.bat` dosyasına yeniden çift tıklayın.

WSL'i engelleyen bir VPN veya güvenlik duvarı da aynı mesaja yol açabilir: WSL'e izin verin ya da
`TheNewMiner.bat setup -Proxy ...` ile bir proxy ayarlayın ([kılavuzdaki](GUIDE.tr.md) "Proxy" bölümüne bakın).

## Ubuntu indirilemiyor veya sanallaştırma kapalı

- **Ağ:** kurulum Ubuntu 24.04'ü `wsl --install` ile kurar. Bu indirmenin bağlanamadığı yerlerde (örneğin GitHub'a
  erişilemeyen yerlerde) kurulum bunun yerine Ubuntu'nun kendi WSL imajını Çin'deki herkese açık bir yansıdan indirir ve
  yalnızca SHA-256 değeri eşleşirse kullanır. Hiçbir yansı da yanıt vermezse internet bağlantısını kontrol edin ve daha
  sonra yeniden deneyin.
- **Sanallaştırma:** sanallaştırmayla ilgili bir mesaj (örneğin `0x80370102`) sanallaştırmanın kapalı olduğu anlamına
  gelir. BIOS'ta Intel VT-x / AMD SVM'yi açın, ardından `TheNewMiner.bat` dosyasına yeniden çift tıklayın.

## Paket bir Windows sürücüsünde

Linux paketini kendiniz açıp `./thenewminer` dosyasını WSL içinde bir Windows sürücüsünden (`/mnt/d/...` gibi)
başlattıysanız madenci başlamayı reddeder: bir Windows sürücüsü madencinin özel ayarlar dosyasını tutamaz. Madenci,
klasörü Linux ev klasörünüze kopyalayıp orada başlatan, yapıştırmaya hazır bir `mkdir -p ~/thenewminer && cp -a ...`
satırı yazdırır. Ya da bunun yerine Windows zip'ini kullanın: `TheNewMiner.bat` her şeyi kendiliğinden doğru yere koyar.

## 10 GB'lık kartta yavaş (tarayıcılar, Docker Desktop)

10 GB'lık bir kart hızlı yerleşimi yalnızca kazım başladığında yaklaşık 9.3 GB (9472 MiB) VRAM boşsa kullanır.
Windows'ta kartı başka programlar da paylaşır: donanım hızlandırması açık bir tarayıcı veya Docker Desktop, madencinin
daha yavaş bir yerleşime geri dönmesine yetecek kadar VRAM kullanabilir. Bir RTX 3080 10 GB'ta bu, 21–22 N/s yerine
yaklaşık 9 N/s oldu.

1. Tarayıcılarınızı kapatın veya donanım hızlandırmalarını kapatın (Chrome ve Edge: Ayarlar › Sistem › "Kullanılabilir
   olduğunda grafik hızlandırmayı kullan"; Firefox: Ayarlar › Genel › Performans) ve Docker Desktop'tan çıkın.
2. Boş VRAM'i bir Komut İstemi'nde kontrol edin: `nvidia-smi --query-gpu=memory.used,memory.total --format=csv`
   (toplamdan kullanılan çıkarıldığında en az 9472 MiB kalmalıdır).
3. Kazımı yeniden başlatın: `TheNewMiner.bat stop`, ardından `TheNewMiner.bat` dosyasına çift tıklayın.

12 GB'lık kartlar da aynı 9472 MiB boş belleğe ihtiyaç duyar; 16 GB veya daha fazla belleğe sahip kartlar bundan
etkilenmez.

## Saat hataları

Havuz, saati yaklaşık 30 saniye kaymış bir madenciyi durdurur. WSL2'nin saati, örneğin bilgisayar uyku moduna geçtikten
sonra kayabilir ve kurulum saat kaymışsa uyarır. Madenci bir saat hatası bildirirse:

1. Windows Ayarlar › Saat ve dil › Tarih ve saat bölümünü açın, "Saat dilimini otomatik olarak ayarla" ve "Saati
   otomatik olarak ayarla" seçeneklerini açın, ardından **Şimdi eşitle**'ye tıklayın.
2. Bir Komut İstemi'nde `wsl --shutdown` çalıştırın.
3. Kazımı yeniden başlatın.

## Windows güvenlik uyarıları

SmartScreen ("Windows bilgisayarınızı korudu") ve madenciyi işaretleyen antivirüs programları için
[kılavuzdaki](GUIDE.tr.md) SSS bölümüne bakın.

## Hâlâ çözülmedi mi?

[GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) üzerinde
`TheNewMiner.bat status` çıktısı, başarısız olan adım ve `last-setup-failure.txt` dosyasının içeriğiyle bir issue açın.
Havuz parolanızı veya cüzdanınızın özel anahtarını asla paylaşmayın.
