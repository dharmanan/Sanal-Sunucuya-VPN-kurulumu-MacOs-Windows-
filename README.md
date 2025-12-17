# 🔒 VPS Sunucunuzu VPN Olarak Kullanma Rehberi

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![OpenVPN](https://img.shields.io/badge/OpenVPN-Supported-orange.svg)](https://openvpn.net/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04%2B-E95420?logo=ubuntu&logoColor=white)](https://ubuntu.com/)

> Kendi VPS sunucunuzla tamamen size ait, güvenli ve özel bir VPN bağlantısı oluşturun. Üçüncü parti VPN servislerine veda edin!

> Not: Bu rehber Ubuntu 22.04+ odaklıdır. Ubuntu 20.04 genelde çalışır ancak servis adları ve bazı paket davranışları farklı olabilir.

## 📋 İçindekiler

- [Neden Kendi VPN'inizi Kurmalısınız?](#neden-kendi-vpn)
- [Gereksinimler](#gereksinimler)
- [Kurulum Adımları](#kurulum)
- [VPN'i Farklı Cihazlarda Kullanma](#cihazlar)
- [Güvenlik İpuçları](#guvenlik)
- [Sorun Giderme](#sorun-giderme)
- [Sık Sorulan Sorular](#sss)
- [Katkıda Bulunma](#katki)
- [Lisans](#lisans)

## 🎯 Neden Kendi VPN'inizi Kurmalısınız?

<a id="neden-kendi-vpn"></a>

### Ücretsiz VPN Servislerin Riskleri

Birçok ücretsiz VPN servisi:
- 🕵️ **Kullanıcı verilerinizi** toplayıp üçüncü partilere satıyor
- 🐌 **Yavaş bağlantı** hızları sunuyor
- 📊 **Reklam ve tracker** ile dolup taşıyor
- 🚫 **Gizlilik politikaları** şeffaf değil
- 💰 **Gerçek ürün sizsiniz** - verileriniz para kazanma aracı

### Kendi VPN'inizin Avantajları

- ✅ **Tam Kontrol**: Tüm trafik sizin kontrolünüzde
- ✅ **Gizlilik**: Hiçbir üçüncü parti verilerinize erişemez
- ✅ **Performans**: Sadece sizin kullandığınız özel sunucu
- ✅ **Maliyet**: Zaten sahip olduğunuz VPS'i kullanın
- ✅ **Esneklik**: İstediğiniz gibi yapılandırın
- ✅ **Node'lara Zarar Yok**: VPS'inizdeki node'lar ve uptimeınız etkilenmez

## 📦 Gereksinimler

<a id="gereksinimler"></a>

### 1. VPS Sunucu

**Minimum Gereksinimler:**
- İşletim Sistemi: Ubuntu 22.04 veya üzeri (Ubuntu 20.04 de çalışır)
- RAM: 512 MB (1 GB önerilir)
- Disk: 5 GB boş alan
- Çekirdek: 64-bit
- Root erişimi gerekli

**Önerilen VPS Sağlayıcılar:**
- DigitalOcean
- Vultr
- Linode
- Hetzner
- Contabo
- AWS Lightsail

### 2. SSH Terminal İstemcisi

Aşağıdaki programlardan birini kullanabilirsiniz:

| Program | Platform | İndirme Linki |
|---------|----------|---------------|
| **MobaXterm** | Windows | [İndir](https://mobaxterm.mobatek.net/download.html) |
| **PuTTY** | Windows | [İndir](https://www.putty.org/) |
| **Termius** | Windows/Mac/Linux | [İndir](https://termius.com/) |
| **Terminal** | Mac/Linux | Varsayılan olarak yüklü |

### 3. OpenVPN Client

**İstemci yazılımı indirme linkleri:**

- 🪟 **Windows**: [OpenVPN Connect](https://openvpn.net/client-connect-vpn-for-windows/)
- 🍎 **macOS**: [OpenVPN Connect](https://openvpn.net/client-connect-vpn-for-mac-os/)
- 🐧 **Linux**: [OpenVPN](https://openvpn.net/openvpn-client-for-linux/)
- 📱 **Android**: [Google Play'den İndir](https://play.google.com/store/apps/details?id=net.openvpn.openvpn)
- 📱 **iOS**: [App Store'dan İndir](https://apps.apple.com/app/openvpn-connect/id590379981)

### 4. Dosya Transfer Programı (WinSCP)

VPS'ten bilgisayarınıza dosya indirmek için:
- **WinSCP** (Windows): [İndir](https://winscp.net/eng/download.php)
- **FileZilla**: [İndir](https://filezilla-project.org/) (Tüm platformlar)
- **Cyberduck** (Mac): [İndir](https://cyberduck.io/)

---

## 🚀 Kurulum Adımları

<a id="kurulum"></a>

### Adım 1: VPS Sunucunuza Bağlanın

#### MobaXterm ile Bağlantı:

1. MobaXterm'i açın
2. Sol üstteki **"Session"** butonuna tıklayın
3. **SSH** seçeneğini seçin
4. Aşağıdaki bilgileri girin:
	- **Remote host**: VPS IP adresiniz
	- **Username**: `root` veya kullanıcı adınız
	- **Port**: `22`
5. **OK** butonuna tıklayın ve şifrenizi girin

```bash
# Örnek bağlantı komutu (Terminal kullanıyorsanız)
ssh root@SUNUCU_IP_ADRESİNİZ
```

### Adım 2: OpenVPN Kurulum Script'ini Çalıştırın

Sunucunuza bağlandıktan sonra aşağıdaki komutları sırasıyla çalıştırın:

```bash
# Root kullanıcısına geçiş (alternatif: komutların başına sudo ekleyebilirsiniz)
sudo -i

# Ana dizine geçiş
cd ~

# OpenVPN kurulum script'ini indir ve çalıştır
wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh
```

İsterseniz script'i çalıştırmadan önce içeriğini kontrol edebilirsiniz:

```bash
less openvpn-install.sh
```

### Adım 3: Kurulum Sorularını Cevaplayın

Script çalıştığında size bazı sorular soracak. İşte önerilen cevaplar:

#### 📌 Soru 1: IP Adresi Seçimi
```
Which IPv4 address should be used?
[1] 10.0.0.5
[2] YOUR_PUBLIC_IP

Seçim: 2
```
**Açıklama**: Public (genel) IP adresinizi seçin. Bu, VPN'e dışarıdan bağlanabilmeniz için gerekli.

#### 📌 Soru 2: Public IP Adresi Onayı
```
This server is behind NAT. What is the public IPv4 address or hostname?
Public IPv4 address / hostname: [YOUR_IP]

Enter tuşuna basın (değiştirmeyin)
```
**Açıklama**: Otomatik algılanan IP adresi genellikle doğrudur.

#### 📌 Soru 3: IPv6 Desteği
```
Do you want to enable IPv6 support (NAT)? [y/n]: n
```
**Açıklama**: Çoğu kullanıcı için IPv6 gerekmez, `n` yazın.

#### 📌 Soru 4: Port Seçimi
```
What port should OpenVPN listen to?
Port [1194]:

Enter tuşuna basın (varsayılan portu kullanın)
```
**Açıklama**: Varsayılan 1194 portu güvenli ve standart bir seçimdir.

#### 📌 Soru 5: Protokol Seçimi
```
Which protocol should OpenVPN use?
[1] UDP (recommended)
[2] TCP

Seçim: 1
```
**Açıklama**: UDP daha hızlıdır ve VPN için önerilir.

#### 📌 Soru 6: DNS Sağlayıcı
```
Which DNS do you want to use with the VPN?
[1] Current system resolvers
[2] Google (8.8.8.8)
[3] Cloudflare (1.1.1.1)
[4] Quad9 (9.9.9.9)
[5] Custom

Seçim: 3
```
**Açıklama**: Cloudflare (1.1.1.1) hızlı ve gizlilik odaklıdır. Google veya Quad9 da iyi alternatiflerdir.

#### 📌 Soru 7: İstemci Adı
```
Enter a name for the first client:
Name [client]: vpstovpn

"vpstovpn" veya istediğiniz bir isim yazın
```
**Açıklama**: Bu isim oluşturulacak `.ovpn` dosyasının adı olacak.

#### 📌 Son Adım
```
Press any key to continue...

Herhangi bir tuşa basın ve kurulumun bitmesini bekleyin
```

### ✅ Kurulum Tamamlandı

Kurulum başarıyla tamamlandığında şu mesajı göreceksiniz:

```
Finished!

The client configuration is available in: /root/vpstovpn.ovpn
New clients can be added by running this script again.
```

**Tebrikler!** OpenVPN başarıyla kuruldu ve `/root/vpstovpn.ovpn` dosyanız hazır.

---

## 📁 Adım 4: VPN Yapılandırma Dosyasını İndirin

### WinSCP ile Dosya İndirme:

1. **WinSCP'yi açın**

2. **Yeni bağlantı oluşturun:**
	- File protocol: `SFTP`
	- Host name: VPS IP adresiniz
	- Port number: `22`
	- User name: `root`
	- Password: VPS şifreniz

3. **Login** butonuna tıklayın

4. Sağ tarafta (sunucu tarafı) `/root` dizininde **vpstovpn.ovpn** dosyasını bulun

5. Dosyayı **sürükle-bırak** ile sol tarafa (bilgisayarınız) masaüstüne taşıyın

### Alternatif: SCP Komutu ile İndirme (Linux/Mac)

```bash
# Terminal'den çalıştırın
scp root@SUNUCU_IP:/root/vpstovpn.ovpn ~/Desktop/
```

---

## 🖥️ Adım 5: OpenVPN Client'ı Yapılandırın

### 🪟 Windows için Kurulum:

#### 5.1. OpenVPN Connect'i İndirin ve Kurun

1. [OpenVPN Connect for Windows](https://openvpn.net/client-connect-vpn-for-windows/) linkine gidin
2. **Download OpenVPN Connect** butonuna tıklayın
3. İndirilen `.msi` dosyasını çalıştırın
4. Kurulum sihirbazını takip edin (**Next** → **Install** → **Finish**)
5. Windows Defender/Firewall izin isterse **"Allow access"** seçin

#### 5.2. VPN Profilini İçe Aktarın

1. Kurulum tamamlandıktan sonra **Windows sistem tepsisinde** (sağ alt köşe) OpenVPN simgesini bulun
	- Göremiyorsanız yukarı ok `^` simgesine tıklayın

2. OpenVPN simgesine **sağ tıklayın**

3. **Import** → **Import from file** seçeneğine tıklayın

4. Masaüstünüzdeki `vpstovpn.ovpn` dosyasını seçin

5. Profil başarıyla içe aktarıldı! ✅

#### 5.3. VPN'e Bağlanın

1. OpenVPN simgesine tekrar **sağ tıklayın**
2. **vpstovpn** profilinizi seçin
3. **Connect** butonuna tıklayın
4. Birkaç saniye içinde "Connected" mesajını göreceksiniz
5. Bağlantı kuruldu! 🎉

#### 5.4. Bağlantıyı Kesin

- OpenVPN simgesine sağ tıklayın → **Disconnect**

---

### 🍎 macOS için Kurulum:

#### 5.1. OpenVPN Connect'i İndirin ve Kurun

1. [OpenVPN Connect for macOS](https://openvpn.net/client-connect-vpn-for-mac-os/) linkine gidin
2. **Download OpenVPN Connect** butonuna tıklayın
3. İndirilen `.dmg` dosyasını açın
4. **OpenVPN Connect** uygulamasını **Applications** klasörüne sürükleyin
5. **Launchpad** veya **Applications** klasöründen uygulamayı başlatın

#### 5.2. İlk Çalıştırma İzinleri

1. İlk açılışta **"OpenVPN Connect" is an app downloaded from the internet** uyarısı gelirse:
	- **Open** butonuna tıklayın
   
2. macOS **VPN yapılandırması için izin** isteyecek:
	- **Allow** (İzin Ver) butonuna tıklayın
	- Yönetici **şifrenizi** girin

#### 5.3. VPN Profilini İçe Aktarın

**Yöntem 1: Sürükle-Bırak**
1. OpenVPN Connect uygulamasını açın
2. `vpstovpn.ovpn` dosyasını **Finder'dan uygulama penceresine sürükleyin**
3. **Add** butonuna tıklayın

**Yöntem 2: File Menüsünden**
1. OpenVPN Connect'i açın
2. Üst menü çubuğundan **File** → **Import Profile** → **From File**
3. `vpstovpn.ovpn` dosyasını seçin
4. **Open** → **Add** butonlarına tıklayın

#### 5.4. VPN'e Bağlanın

1. OpenVPN Connect uygulamasında **vpstovpn** profilinizi göreceksiniz
2. Profilin yanındaki **toggle switch** (açma/kapama düğmesi) tıklayın
3. İlk bağlantıda yönetici **şifrenizi** girin
4. "Connected" durumunu göreceksiniz
5. Bağlantı kuruldu! 🎉

#### 5.5. Menü Çubuğundan Hızlı Erişim

- Bağlandıktan sonra menü çubuğunda (üst sağ) OpenVPN simgesi görünür
- Buradan hızlıca bağlan/bağlantıyı kes yapabilirsiniz

---

### ✅ Bağlantıyı Kontrol Etme (Her İki Platform İçin)

VPN'in düzgün çalıştığını doğrulamak için:

1. **Tarayıcınızdan** şu sitelere gidin:
	- [https://whatismyipaddress.com/](https://whatismyipaddress.com/)
	- [https://ipleak.net/](https://ipleak.net/)
	- [https://whoer.net/](https://whoer.net/)

2. **Göreceğiniz IP adresi** VPS sunucunuzun IP'si olmalı (kendi gerçek IP'niz değil)

3. **DNS sızıntısı kontrolü**: ipleak.net sitesinde DNS sunucularının VPN üzerinden göründüğünü doğrulayın

### 🔧 Bağlantı Durumu İpuçları

**✅ Başarılı Bağlantı:**
- Durum: "Connected"
- IP adresi: VPS sunucunuzun IP'si görünüyor
- Trafik istatistikleri: Veri gönder/alım sayıları artıyor

**❌ Bağlantı Sorunları:**
- Durum: "Connecting..." (sürekli bağlanmaya çalışıyor)
- Hata mesajı: "Connection timeout" veya "Authentication failed"
- Çözüm: [Sorun Giderme](#-sorun-giderme) bölümüne bakın

---

## 📱 VPN'i Farklı Cihazlarda Kullanma

<a id="cihazlar"></a>

### Android Cihazlar:

1. **Google Play Store'dan OpenVPN Connect'i indirin**
2. `vpstovpn.ovpn` dosyasını telefonunuza aktarın (Email, WhatsApp, Google Drive vb. ile)
3. OpenVPN uygulamasını açın
4. **Import Profile** → **File** seçeneğini seçin
5. İndirdiğiniz `.ovpn` dosyasını seçin
6. **Connect** butonuna tıklayın

### iOS (iPhone/iPad):

1. **App Store'dan OpenVPN Connect'i indirin**
2. `.ovpn` dosyasını cihazınıza aktarın (iCloud, Email, AirDrop vb.)
3. Dosyaya dokunun ve **Share** → **OpenVPN** seçin
4. **Add** butonuna tıklayın
5. **Connect** ile bağlanın

### Linux:

```bash
# OpenVPN'i kurun
sudo apt update
sudo apt install openvpn

# VPN'e bağlanın
sudo openvpn --config vpstovpn.ovpn
```

### macOS (Alternatif Yöntem - Terminal):

```bash
# OpenVPN'i Homebrew ile kurun
brew install openvpn

# VPN'e bağlanın
sudo openvpn --config vpstovpn.ovpn
```

Not: Bu yöntem için Homebrew kurulu olmalıdır.

---

## 🔐 Güvenlik İpuçları

<a id="guvenlik"></a>

### Firewall Yapılandırması

Sunucunuzun güvenliğini artırmak için:

```bash
# UFW firewall'u yükleyin (yüklü değilse)
sudo apt install ufw

# OpenVPN portunu açın
sudo ufw allow 1194/udp

# SSH portunu açın (dikkatli olun!)
sudo ufw allow 22/tcp

# Firewall'u aktif edin
sudo ufw enable

# Durumu kontrol edin
sudo ufw status
```

### Güvenlik Kontrol Listesi:

- ✅ Güçlü VPS root şifresi kullanın
- ✅ SSH için key-based authentication kullanın
- ✅ `.ovpn` dosyasını güvenli saklayın
- ✅ `.ovpn` dosyasını başkalarıyla paylaşmayın
- ✅ Düzenli olarak VPS güvenlik güncellemelerini yapın
- ✅ Gereksiz portları kapatın
- ✅ Fail2ban kurun (brute force saldırıları önler)

```bash
# Fail2ban kurulumu
sudo apt install fail2ban
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

---

## 🛠️ Sorun Giderme

<a id="sorun-giderme"></a>

### Bağlantı Kurulamıyor

**Çözüm 1**: Sunucu firewall'unu kontrol edin
```bash
sudo ufw status
# 1194/udp portunu açın
sudo ufw allow 1194/udp
```

**Çözüm 2**: OpenVPN servisini yeniden başlatın
```bash
# Ubuntu 22.04+ (yaygın):
sudo systemctl restart openvpn-server@server
sudo systemctl status openvpn-server@server

# Bazı sistemlerde servis adı farklı olabilir. Emin değilseniz:
systemctl list-units --type=service | grep -i openvpn
```

**Çözüm 3**: Sunucu günlüklerini kontrol edin
```bash
sudo journalctl -u openvpn-server@server -f
```

### Yavaş Bağlantı

1. **VPS konumunu** kontrol edin - size yakın bölge seçin
2. **Farklı VPS sağlayıcı** deneyin
3. **TCP protokolünü** deneyin (UDP yerine)

```bash
# Yeni client oluşturmak için scripti tekrar çalıştırın
bash openvpn-install.sh
# TCP için 2 numaralı seçeneği seçin
```

### DNS Çözümleme Sorunları

`.ovpn` dosyasını düzenleyin ve DNS sunucularını değiştirin:

```
# Cloudflare DNS
dhcp-option DNS 1.1.1.1
dhcp-option DNS 1.0.0.1

# veya Google DNS
dhcp-option DNS 8.8.8.8
dhcp-option DNS 8.8.4.4
```

---

## ❓ Sık Sorulan Sorular

<a id="sss"></a>

### Birden fazla cihaz bağlanabilir mi?

Evet! Her cihaz için ayrı `.ovpn` dosyası oluşturabilirsiniz:

```bash
# Scripti tekrar çalıştırın
bash openvpn-install.sh

# "1) Add a new client" seçeneğini seçin
# Her cihaz için farklı isim verin (laptop, telefon, tablet vb.)
```

### Client'ı nasıl silerim?

```bash
bash openvpn-install.sh
# "2) Revoke an existing client" seçeneğini seçin
```

### OpenVPN'i tamamen nasıl kaldırırım?

```bash
bash openvpn-install.sh
# "3) Remove OpenVPN" seçeneğini seçin
```

### VPS'teki node'lara zarar verir mi?

**Hayır!** OpenVPN çok az kaynak kullanır ve node'larınızın uptime'ını etkilemez.

### Hangi VPS sağlayıcısı en iyi?

Sizin coğrafi konumunuza yakın olan ve iyi performans sunan herhangi bir VPS uygun olacaktır. Popüler seçenekler:
- Hetzner (Avrupa için harika)
- Vultr (Global coverage)
- DigitalOcean (Kullanıcı dostu)

---

## 🤝 Katkıda Bulunma

<a id="katki"></a>

Bu projeye katkıda bulunmak isterseniz:

1. Bu repo'yu **fork** edin
2. Yeni bir **branch** oluşturun (`git checkout -b feature/YeniOzellik`)
3. Değişikliklerinizi **commit** edin (`git commit -am 'Yeni özellik ekle'`)
4. Branch'inizi **push** edin (`git push origin feature/YeniOzellik`)
5. **Pull Request** açın

### Katkı Fikirleri:

- 📝 Dokümantasyon iyileştirmeleri
- 🌍 Farklı diller için çeviriler
- 🛠️ Script geliştirmeleri
- 🐛 Bug düzeltmeleri
- 💡 Yeni özellik önerileri

---

## 📝 Lisans

<a id="lisans"></a>

Bu proje [MIT Lisansı](LICENSE) altında lisanslanmıştır.

```
MIT License

Copyright (c) 2024

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```



**Not**: Bu rehber eğitim amaçlıdır. VPN kullanımı yerel yasalara tabi olabilir. Lütfen ülkenizdeki yasal düzenlemelere uygun hareket edin.
