# Güvenli Banka Şubesi Ağı Tasarımı

Bu repo, Cisco Packet Tracer üzerinde uygulanabilir güvenli bir banka şubesi ağı tasarımını içerir. Projede VLAN segmentasyonu, ACL tabanlı erişim kontrolü, Guest Wi-Fi izolasyonu, ATM/Teller ağ koruması, şube-genel merkez WAN bağlantısı ve temel ağ servisleri birlikte ele alınmıştır.

## Proje Özeti

Tasarım, küçük-orta ölçekli bir banka şubesinin genel merkezdeki Core Banking Server'a kontrollü şekilde bağlanmasını simüle eder. Şube içinde personel, gişe, ATM, sunucu, IP telefon, CCTV, yönetim ve misafir ağları ayrı VLAN'lara ayrılmıştır.

Ana hedef, tüm VLAN'ların serbestçe haberleşmesini engelleyip yalnızca gerekli trafiğe izin veren güvenli ve test edilebilir bir Packet Tracer ağı oluşturmaktır.

## Temel Özellikler

- VLAN tabanlı ağ segmentasyonu
- Layer 3 switch üzerinde inter-VLAN routing
- Extended ACL ile kontrollü VLAN erişimi
- Guest Wi-Fi iç ağ izolasyonu
- ATM VLAN için yalnızca Core Banking erişimi
- Teller VLAN için sınırlı bankacılık ve yerel servis erişimi
- Management VLAN ve yalnızca Admin PC yönetimi
- Staff ve Guest için WAN/internet test çıkışı
- DHCP, DNS, NTP, Syslog ve HTTP servis simülasyonları
- Statik routing ile şube-genel merkez bağlantısı
- Cisco IOS konfigürasyon örnekleri
- Packet Tracer uygulama ve test rehberleri

## Kullanılan Ana Cihazlar

| Cihaz | Rol |
|---|---|
| BRANCH-R1 | Şube WAN router'ı |
| HO-R1 | Genel merkez router'ı |
| ISP-R1 | WAN/ISP simülasyonu |
| BR-CORE-SW1 | VLAN gateway, routing, ACL ve DHCP merkezi |
| BR-ACC-SW1 | Staff, Teller, Admin ve Voice access switch |
| BR-ACC-SW2 | ATM, CCTV, Server ve Guest AP access switch |
| BR-SERVER1 | DNS, NTP, Syslog, NVR/Monitoring servisleri |
| HO-SERVER1 | Core Banking Server simülasyonu |
| ISP-SERVER | Simulated internet test server |

## VLAN Planı

| VLAN | Ad | Subnet | Amaç |
|---:|---|---|---|
| 10 | Management | `10.10.10.0/24` | Network cihaz yönetimi |
| 20 | Staff | `10.10.20.0/24` | Personel bilgisayarları |
| 30 | Teller | `10.10.30.0/24` | Gişe bilgisayarları |
| 40 | ATM | `10.10.40.0/24` | ATM simülasyon cihazları |
| 50 | Servers | `10.10.50.0/24` | Yerel servis sunucuları |
| 60 | Voice | `10.10.60.0/24` | IP telefonlar |
| 70 | CCTV | `10.10.70.0/24` | Kamera ağı |
| 80 | Guest | `10.10.80.0/24` | Misafir Wi-Fi |
| 90 | Branch Transit | `10.10.90.0/30` | Core switch-router transit |

## Dosya Yapısı

| Dosya | Açıklama |
|---|---|
| `generated-complete-network-design.md` | Nihai genel ağ tasarımı |
| `generated-topology-design.md` | Fiziksel ve mantıksal topoloji açıklaması |
| `generated-ip-addressing-plan.md` | VLAN, subnet ve IP adresleme planı |
| `generated-vlan-security-policy.md` | VLAN güvenlik politikası ve ACL mantığı |
| `generated-device-config-plan.md` | Cihaz bazlı konfigürasyon planı |
| `generated-cisco-ios-configs.md` | Nihai Cisco IOS komutları |
| `generated-packet-tracer-implementation.md` | Packet Tracer uygulama rehberi |
| `generated-testing-checklist.md` | Test ve doğrulama kontrol listesi |
| `generated-final-report.md` | Proje sonuç raporu |

## Uygulama Sırası

1. Cisco Packet Tracer'da cihazları yerleştir ve isimlendir.
2. Fiziksel bağlantıları topoloji tablosuna göre yap.
3. Router ve switch temel güvenlik ayarlarını gir.
4. VLAN'ları core ve access switch'lerde oluştur.
5. Access portları ilgili VLAN'lara ata.
6. Trunk bağlantıları yapılandır.
7. BR-CORE-SW1 üzerinde SVI gateway'leri ve `ip routing` ayarını yap.
8. DHCP pool'larını tanımla.
9. BRANCH-R1, ISP-R1 ve HO-R1 üzerinde IP adreslerini ve static route'ları gir.
10. Server-PT cihazlarında DNS, HTTP, NTP ve Syslog servislerini aç.
11. Guest AP ve kablosuz istemciyi yapılandır.
12. Önce temel bağlantı testlerini, sonra ACL güvenlik testlerini yap.

## Kritik Testler

| Test | Beklenen Sonuç |
|---|---|
| Staff PC DHCP IP alır | Başarılı |
| Teller PC Core Banking Server'a erişir | Başarılı |
| ATM Core Banking Server'a erişir | Başarılı |
| ATM Staff VLAN'a erişemez | Başarısız olmalı |
| Guest internet test server'a erişir | Başarılı |
| Guest iç VLAN'lara erişemez | Başarısız olmalı |
| Admin PC cihaz yönetimine erişir | Başarılı |
| Normal kullanıcı Management VLAN'a erişemez | Başarısız olmalı |
| CCTV yalnızca NVR/Monitoring server'a erişir | Başarılı |

## Packet Tracer Notları

- ASA firewall yerine Packet Tracer uyumluluğu için ACL tabanlı güvenlik kullanılmıştır.
- Gerçek VPN/IPsec zorunlu tutulmamıştır; şube-genel merkez güvenliği static routing ve ACL ile temsil edilir.
- Server-PT servisleri GUI üzerinden yapılandırılır.
- HTTPS, NTP, Syslog, Voice VLAN ve AP management özellikleri Packet Tracer sürümüne göre sınırlı davranabilir.
- Gerçek ATM ve NVR cihazları yerine PC, Server-PT veya IP Camera ile simülasyon yapılabilir.

## Sonuç

Bu proje, banka şubesi ağı için güvenli, bölümlendirilmiş ve elle kurulabilir bir Cisco Packet Tracer tasarım paketi sunar. Tasarım; en az ayrıcalık, Guest izolasyonu, ATM/Teller koruması, merkezi servis kullanımı ve test edilebilir ağ güvenliği ilkelerini üniversite düzeyinde uygulanabilir şekilde gösterir.
