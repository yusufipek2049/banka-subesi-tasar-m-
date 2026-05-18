# Test, Şifre ve Demo Komutları

Bu dosya, Cisco Packet Tracer banka şubesi ağı projesinde demo ve doğrulama sırasında kullanılacak şifreleri, kontrol komutlarını ve uç cihaz testlerini toplar.

## 1. Şifre Listesi

### 1.1 Console Şifresi

Tüm router ve switch cihazlarında console şifresi aynıdır:

```text
Console@123
```

### 1.2 Enable Secret Şifreleri

| Cihaz | Enable Secret |
|---|---|
| `BRANCH-R1` | `BankBranch@123` |
| `ISP-R1` | `Isp@123` |
| `HO-R1` | `HeadOffice@123` |
| `BR-CORE-SW1` | `Core@123` |
| `BR-ACC-SW1` | `Access1@123` |
| `BR-ACC-SW2` | `Access2@123` |

### 1.3 VTY/Telnet Kullanıcısı

```text
Username: admin
Password: Admin@12345
```

## 2. Kablosuz Ağ Bilgileri

| Alan | Değer |
|---|---|
| SSID | `BANK-GUEST` |
| Şifre | `GuestBank2026` |
| Guest VLAN | VLAN 80 |
| Beklenen DHCP IP | `10.10.80.100-10.10.80.220` |
| Gateway | `10.10.80.1` |
| DNS | `203.0.113.10` |

## 3. Önemli IP Adresleri

| Cihaz/Servis | IP |
|---|---|
| `BR-CORE-SW1 VLAN10` | `10.10.10.1` |
| `BR-ACC-SW1 VLAN10` | `10.10.10.11` |
| `BR-ACC-SW2 VLAN10` | `10.10.10.12` |
| `ADMIN-PC` | `10.10.10.50` |
| `BR-SERVER1` | `10.10.50.10` |
| `ATM-PC1` | `10.10.40.10` |
| `ATM-PC2` | `10.10.40.11` |
| `HO-SERVER1 / Core Banking` | `10.10.100.10` |
| `ISP-SERVER / Internet Test` | `203.0.113.10` |

## 4. Genel Cisco Kontrol Komutları

Bu komutları router ve switchlerde kullanabilirsin:

```cisco
show running-config
show ip interface brief
show ip route
```

Switchlerde ayrıca:

```cisco
show vlan brief
show interfaces trunk
show mac address-table
```

Core switchte ayrıca:

```cisco
show ip dhcp binding
show ip dhcp pool
show access-lists
```

Routerlarda ayrıca:

```cisco
show ip route
show ip nat translations
show ip nat statistics
```

## 5. Cihaz Bazlı Kontrol Komutları

### 5.1 BR-ACC-SW1 Kontrol

Amaç: Admin, Teller, Staff ve Voice portlarının doğru VLAN'da olduğunu görmek.

```cisco
enable
show vlan brief
show interfaces trunk
show ip interface brief
```

Beklenen:

| Port | Beklenen Rol |
|---|---|
| `Fa0/1` | Admin phone + Admin PC, access VLAN 10, voice VLAN 60 |
| `Fa0/2-Fa0/3` | Teller phone + PC, access VLAN 30, voice VLAN 60 |
| `Fa0/4-Fa0/8` | Staff phone + PC, access VLAN 20, voice VLAN 60 |
| `Fa0/9` | Catering phone, VLAN 60 |
| `G0/1` | Trunk |

### 5. BR-CORE-SW1 Kontrol

Amaç: VLAN gateway, inter-VLAN routing, DHCP ve ACL durumunu kontrol etmek.

```cisco
enable
show ip interface brief
show vlan brief
show interfaces trunk
show ip route
show ip dhcp binding
show access-lists
```

Beklenen SVI gateway adresleri:

| VLAN | Gateway |
|---:|---|
| 10 | `10.10.10.1` |
| 20 | `10.10.20.1` |
| 30 | `10.10.30.1` |
| 40 | `10.10.40.1` |
| 50 | `10.10.50.1` |
| 60 | `10.10.60.1` |
| 70 | `10.10.70.1` |
| 80 | `10.10.80.1` |

### 5.3 BRANCH-R1 Kontrol

Amaç: Şube WAN, static route ve NAT/PAT durumunu doğrulamak.

enable
show ip interface brief
show ip route
show ip nat translations
show ip nat statistics

NAT tablosunu temizleyip yeniden test etmek için:

clear ip nat translation *
show ip nat translations

Beklenen NAT kaynakları:

| Kaynak | NAT Durumu |
|---|---|
| Staff VLAN `10.10.20.0/24` | NAT/PAT var |
| Guest VLAN `10.10.80.0/24` | NAT/PAT var |
| Teller VLAN `10.10.30.0/24` | NAT yok |
| ATM VLAN `10.10.40.0/24` | NAT yok |

### 5.5 ISP-R1 Kontrol

Amaç: Branch, Head Office ve Internet Test ağları arasında route olduğunu görmek.

enable
show ip interface brief
show ip route
ping 10.10.91.1
ping 10.10.91.6
ping 203.0.113.10

### 5.6 HO-R1 Kontrol

Amaç: Head Office router'ın branch ağlarına dönüş route'una sahip olduğunu doğrulamak.

enable
show ip interface brief
show ip route
ping 10.10.91.5
ping 10.10.100.10

## 6. DHCP Testleri

PC veya laptop üzerinde:

Desktop > IP Configuration > DHCP

Beklenen IP aralıkları:

| Cihaz Grubu | Beklenen IP |
|---|---|
| Staff PC'ler | `10.10.20.100-10.10.20.199` |
| Teller PC'ler | `10.10.30.100-10.10.30.199` |
| IP Phone'lar | `10.10.60.100-10.10.60.199` |
| Guest Laptop | `10.10.80.100-10.10.80.220` |

Core switchte DHCP kontrolü:

show ip dhcp binding
show ip dhcp pool

## 7. Gateway Ping Testleri

Her cihaz kendi VLAN gateway'ine ping atabilmelidir.

| Kaynak | Komut | Beklenen |
|---|---|---|
| `ADMIN-PC` | `ping 10.10.10.1` | Başarılı |
| Staff PC | `ping 10.10.20.1` | Başarılı |
| Teller PC | `ping 10.10.30.1` | Başarılı |
| `ATM-PC1` | `ping 10.10.40.1` | Başarılı |
| `ATM-PC2` | `ping 10.10.40.1` | Başarılı |
| `BR-SERVER1` | `ping 10.10.50.1` | Başarılı |
| IP Phone | VLAN 60 IP almalı | Başarılı |
| CCTV temsil PC | `ping 10.10.70.1` | Başarılı |
| `GUEST-LAPTOP1` | `ping 10.10.80.1` | Başarılı |

## 8. WAN ve Core Banking Testleri

### 8.1 Teller Core Banking Testi

`TELLER-PC1` veya `TELLER-PC2` üzerinde:

ping 10.10.100.10

Web Browser:

http://10.10.100.10


### 8.2 ATM Core Banking Testi

`ATM-PC1` ve `ATM-PC2` üzerinde:

ping 10.10.100.10

Web Browser:

http://10.10.100.10



### 8.3 Router WAN Testi

`BRANCH-R1` üzerinde:

ping 10.10.91.2
ping 10.10.91.6
ping 10.10.100.10

## 9. NAT/PAT Testleri

NAT yalnızca Staff ve Guest için beklenir.

### 9.1 NAT Tablosunu Temizle

`BRANCH-R1` üzerinde:

clear ip nat translation *
show ip nat translations

### 9.2 Staff NAT Testi

Staff PC üzerinde:

ping 203.0.113.10


Sonra `BRANCH-R1` üzerinde:

show ip nat translations
show ip nat statistics


`Inside local` alanında `10.10.20.x`, `Inside global` alanında `10.10.91.1` görünür.

### 9.3 Guest NAT Testi

`GUEST-LAPTOP1` üzerinde:

ping 203.0.113.10


Sonra `BRANCH-R1` üzerinde:

show ip nat translations
show ip nat statistics

`Inside local` alanında `10.10.80.x`, `Inside global` alanında `10.10.91.1` görünür.

### 9.4 NAT Olmaması Gereken Trafik

Teller veya ATM üzerinden internet test server'a erişim denenirse NAT oluşmamalıdır:

ping 203.0.113.10

`BRANCH-R1` üzerinde:

show ip nat translations

Teller `10.10.30.x` veya ATM `10.10.40.x` için NAT kaydı oluşmamalıdır.

## 10. Guest İzolasyon Testleri

`GUEST-LAPTOP1` üzerinde:

ping 10.10.20.1
ping 10.10.50.10
ping 10.10.100.10
ping 203.0.113.10

Beklenen:

| Hedef | Sonuç |
|---|---|
| `10.10.20.1` Staff gateway | Başarısız |
| `10.10.50.10` Server | Başarısız |
| `10.10.100.10` Core Banking | Başarısız |
| `203.0.113.10` ISP-SERVER | Başarılı |

## 11. ATM Güvenlik Testleri

`ATM-PC1` veya `ATM-PC2` üzerinde:

ping 10.10.100.10
ping 10.10.20.1
ping 10.10.50.10


| Hedef | Sonuç |
|---|---|
| `10.10.100.10` Core Banking | Başarılı |
| `10.10.20.1` Staff VLAN | Başarısız |
| `10.10.50.10` Server VLAN | Başarısız |

## 12. Teller Güvenlik Testleri

`TELLER-PC1` veya `TELLER-PC2` üzerinde:

ping 10.10.100.10
ping 10.10.10.1
ping 10.10.40.1


Beklenen:

| Hedef | Sonuç |
|---|---|
| `10.10.100.10` Core Banking | Başarılı |
| `10.10.10.1` Management | Başarısız |
| `10.10.40.1` ATM VLAN | Başarısız |

## 13. Staff Güvenlik Testleri

Staff PC üzerinde:

ping 10.10.10.1
ping 10.10.30.1
ping 10.10.40.1
ping 203.0.113.10

Beklenen:

| Hedef | Sonuç |
|---|---|
| `10.10.10.1` Management | Başarısız |
| `10.10.30.1` Teller | Başarısız |
| `10.10.40.1` ATM | Başarısız |
| `203.0.113.10` ISP-SERVER | Başarılı |

## 14. Management VLAN Testleri

### 14.1 Admin PC Yönetim Erişimi

`ADMIN-PC` üzerinde:

ping 10.10.10.1
ping 10.10.10.11
ping 10.10.10.12

Telnet destekleniyorsa:

telnet 10.10.10.1
telnet 10.10.10.11
telnet 10.10.10.12

Beklenen: Başarılı.

### 14.2 Normal Kullanıcı Management Erişim Engeli

Staff PC üzerinde:

ping 10.10.10.1
ping 10.10.10.11
ping 10.10.10.12

Beklenen: Başarısız.

## 15. CCTV Testleri

Webcam cihazlarına IP atanamıyorsa CCTV testleri VLAN 70'e bağlı PC endpoint'leriyle yapılır.

CCTV temsil PC üzerinde:

ping 10.10.70.1
ping 10.10.50.10
ping 10.10.20.1


| Hedef | Sonuç |
|---|---|
| `10.10.70.1` CCTV gateway | Başarılı |
| `10.10.50.10` BR-SERVER1/NVR | Başarılı |
| `10.10.20.1` Staff VLAN | Başarısız |

## 16. DNS Testleri

DNS kayıtları `BR-SERVER1` üzerinde açıldıysa:

Staff veya Teller PC üzerinde Web Browser:

http://corebank.bank.local
http://branchserver.bank.local
http://nvr.bank.local

Beklenen:

| DNS Adı | IP | Beklenen |
|---|---|---|
| `corebank.bank.local` | `10.10.100.10` | Teller için başarılı |
| `branchserver.bank.local` | `10.10.50.10` | Staff için başarılı |
| `nvr.bank.local` | `10.10.50.10` | CCTV için başarılı |

Packet Tracer'da `nslookup` yoksa isimle web erişimi denenir.

## 17. ACL Sayaç Kontrolü

`BR-CORE-SW1` üzerinde:

show access-lists

Bir yasaklı erişim denedikten sonra tekrar:

show access-lists

Beklenen: İlgili deny/permit satırlarının sayaçları artabilir. Packet Tracer sürümüne göre sayaç davranışı sınırlı olabilir.

## 18. Demo İçin Kısa Test Sırası

Hoca karşısında hızlı demo için:

1. `show vlan brief`
2. `show interfaces trunk`
3. Staff PC DHCP IP kontrolü
4. Teller PC -> `http://10.10.100.10`
5. ATM-PC1 -> `http://10.10.100.10`
6. Guest Laptop -> `http://203.0.113.10`
7. Guest Laptop -> `ping 10.10.50.10` başarısız
8. Staff PC -> `ping 10.10.10.1` başarısız
9. CCTV temsil PC -> `ping 10.10.50.10` başarılı
10. BRANCH-R1 -> `show ip nat translations`

## 19. Sorun Çıkarsa İlk Bakılacak Komutlar

Switchlerde:

show vlan brief
show interfaces trunk
show ip interface brief

Core switchte:

show ip dhcp binding
show ip route
show access-lists

Branch routerda:

show ip interface brief
show ip route
show ip nat translations
show ip nat statistics

Uç cihazlarda:

ipconfig
ping <gateway>
ping <test-hedefi>

