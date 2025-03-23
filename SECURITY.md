# 🔒 NexFit Güvenlik Dokümantasyonu

Bu dokümantasyon, NexFit uygulamasının güvenlik politikalarını, uyguladığı güvenlik önlemlerini ve güvenlik ile ilgili en iyi uygulamaları açıklamaktadır.

## İçindekiler

- [Güvenlik Modeli](#güvenlik-modeli)
- [Veri Güvenliği](#veri-güvenliği)
- [Kimlik Doğrulama ve Yetkilendirme](#kimlik-doğrulama-ve-yetkilendirme)
- [API Güvenliği](#api-güvenliği)
- [Güvenlik Gereksinimleri](#güvenlik-gereksinimleri)
- [Zafiyet Yönetimi](#zafiyet-yönetimi)
- [Güvenlik İhlali Yanıt Planı](#güvenlik-i̇hlali-yanıt-planı)
- [Güvenlik Testleri](#güvenlik-testleri)
- [Uyumluluk ve Sertifikasyonlar](#uyumluluk-ve-sertifikasyonlar)
- [Güvenlik İletişim Kanalları](#güvenlik-i̇letişim-kanalları)

## Güvenlik Modeli

NexFit, güvenliği temel bir tasarım prensibi olarak ele alır ve "Defense in Depth" (Derinliğine Savunma) yaklaşımını benimser. Bu yaklaşım, birden fazla güvenlik katmanı oluşturarak, herhangi bir katmandaki başarısızlığın genel sistem güvenliğini tehlikeye atmamasını sağlar.

### Güvenlik Prensipleri

1. **En Az Ayrıcalık İlkesi**: Kullanıcılar ve servisler, yalnızca görevlerini yerine getirmek için gereken minimum ayrıcalıklara sahiptir.
2. **Güvenli Varsayılanlar**: Tüm sistemler, en güvenli ayarlarla yapılandırılmış olarak gelir.
3. **Derinlemesine Savunma**: Birden fazla güvenlik katmanı kullanarak koruma sağlanır.
4. **Başarısız-Güvenli Tasarım**: Bir sistem hatası durumunda, en güvenli duruma geçilir.
5. **Güvenli SDLC**: Güvenlik, geliştirme sürecinin tüm aşamalarına entegre edilmiştir.

### Güvenlik Katmanları

NexFit'in güvenlik mimarisi aşağıdaki katmanları içerir:

1. **Ağ Güvenliği**
   - Web Application Firewall (WAF)
   - DDoS koruması
   - IP whitelisting/blacklisting

2. **Uygulama Güvenliği**
   - Güvenli kod geliştirme uygulamaları
   - Düzenli güvenlik denetimleri
   - Güvenlik açığı taramaları

3. **Veri Güvenliği**
   - Hassas verilerin şifrelenmesi
   - Veri erişim kontrolleri
   - Veri anonimleştirme

4. **Kimlik ve Erişim Yönetimi**
   - Çok faktörlü kimlik doğrulama
   - Rol tabanlı erişim kontrolü
   - OAuth 2.0 / OpenID Connect entegrasyonu

5. **İzleme ve Yanıt**
   - Güvenlik Bilgi ve Olay Yönetimi (SIEM)
   - Davranışsal anormallik tespiti
   - Güvenlik olay yanıt protokolleri

## Veri Güvenliği

NexFit, hassas kullanıcı verilerini korumak için kapsamlı önlemler uygulamaktadır.

### Veri Sınıflandırması

Veriler, hassasiyet düzeyine göre sınıflandırılmıştır:

| Sınıflandırma | Açıklama | Örnekler |
|---------------|----------|----------|
| **Yüksek Hassasiyetli** | Kişisel tanımlayıcı bilgiler | Kredi kartı bilgileri, sağlık verileri |
| **Orta Hassasiyetli** | Kişisel bilgiler | E-posta, telefon numarası, adres |
| **Düşük Hassasiyetli** | Anonim veya genel bilgiler | Anonim analitik veriler, ayarlar |

### Veri Şifreleme

- **Hareketsiz Veri**: AES-256 şifreleme, veritabanında hassas bilgiler için
- **İletim Halindeki Veri**: TLS 1.3 ile şifrelenmiş iletişim
- **İşlenen Veri**: Hassas veriler bellek içinde güvenli işlenir ve kullanımdan sonra temizlenir

### Veri Erişim Kontrolü

- Rol ve izin tabanlı erişim kontrolleri
- İlkeli Erişim Kontrolü (PAC) uygulaması
- Veri erişimi için detaylı denetim günlükleri

### Veri Saklama ve İmha

- Kullanıcı verileri, yasal gerekliliklere ve kullanım amacına uygun olarak belirli süreler boyunca saklanır
- Saklama süresi sona eren veriler, güvenli bir şekilde anonimleştirilir veya imha edilir
- Kullanıcılara veri saklama politikaları açık bir şekilde bildirilir

### Veri Yedekleme

- Düzenli tam ve artımlı yedeklemeler
- Yedekler şifrelenir ve güvenli bir şekilde depolanır
- Düzenli geri yükleme testleri yapılır

## Kimlik Doğrulama ve Yetkilendirme

### Kimlik Doğrulama

NexFit, aşağıdaki kimlik doğrulama mekanizmalarını destekler:

- **E-posta/Parola Kimlik Doğrulama**
  - Güçlü parola gereksinimleri (minimum 8 karakter, büyük/küçük harf, sayı, özel karakter)
  - Hesap kilitleme (5 başarısız giriş denemesinden sonra)
  - Parola sıfırlama güvenli bağlantılar ile

- **Sosyal Kimlik Doğrulama**
  - Google, Apple, Facebook OAuth entegrasyonu
  - E-posta doğrulaması hala gereklidir

- **Çok Faktörlü Kimlik Doğrulama (MFA)**
  - SMS veya e-posta üzerinden OTP (Tek Kullanımlık Şifre)
  - Authenticator uygulamaları (Google Authenticator, Microsoft Authenticator)
  - FIDO2/WebAuthn desteği (YubiKey gibi güvenlik anahtarları)

- **JWT Tabanlı Oturum Yönetimi**
  - Kısa ömürlü erişim tokenleri (15 dakika)
  - Daha uzun ömürlü yenileme tokenleri (7 gün)
  - Güvenli çerez ayarları (HttpOnly, Secure, SameSite)

### Yetkilendirme

NexFit, detaylı erişim kontrolü için çok katmanlı yetkilendirme mekanizmaları kullanır:

- **Rol Tabanlı Erişim Kontrolü (RBAC)**
  
  Temel roller ve erişim hakları:

  | Rol | Açıklama | Erişim Düzeyi |
  |-----|----------|---------------|
  | **ADMIN** | Sistem yöneticileri | Tüm sistem ayarları ve verilerine tam erişim |
  | **GYM_OWNER** | Spor salonu sahipleri | Kendi spor salonlarının tüm verilerine erişim |
  | **COACH** | Antrenörler | Kendilerine atanan danışanların verileri |
  | **CLIENT** | Son kullanıcılar | Yalnızca kendi verileri |
  | **GUEST** | Anonim kullanıcılar | Yalnızca genel içerik |

- **İzin Tabanlı Erişim Kontrolü**
  
  Daha detaylı erişim kontrolü için izinler:

  - `read:profile` - Profil verilerini okuma
  - `write:profile` - Profil verilerini düzenleme
  - `read:workout` - Antrenman verilerini okuma
  - `write:workout` - Antrenman verilerini düzenleme
  - `read:nutrition` - Beslenme verilerini okuma
  - `write:nutrition` - Beslenme verilerini düzenleme
  - `manage:users` - Kullanıcı yönetimi
  - `manage:business` - İşletme yönetimi

- **Veri Seviyesinde Erişim Kontrolü**
  
  Veri erişimi, veri sahipliği ve ilişkilere göre kısıtlanır:
  - Kullanıcılar yalnızca kendi verilerine erişebilir
  - Antrenörler yalnızca danışanlarının verilerine erişebilir
  - Spor salonu sahipleri yalnızca kendi işletmelerindeki verilere erişebilir

### SSO (Single Sign-On) Entegrasyonu

Kurumsal müşteriler için:
- SAML 2.0 desteği
- OIDC (OpenID Connect) desteği
- Microsoft Azure AD entegrasyonu
- Google Workspace entegrasyonu

## API Güvenliği

NexFit API'leri, aşağıdaki güvenlik önlemleriyle korunmaktadır:

### API Kimlik Doğrulama

- JWT (JSON Web Token) tabanlı kimlik doğrulama
- API anahtarı kimlik doğrulama (B2B entegrasyonları için)
- OAuth 2.0 akışları (Authorization Code, Client Credentials)

### API Koruma Mekanizmaları

- **Hız Sınırlama (Rate Limiting)**
  - IP bazlı sınırlama: 1000 istek/saat
  - Kullanıcı bazlı sınırlama: 10,000 istek/gün
  - Aşım durumunda 429 Too Many Requests yanıtı

- **Anormallik Algılama**
  - Olağandışı istek kalıplarının tespiti
  - Otomatikleştirilmiş tehditlere karşı korunma

- **İstek Doğrulama**
  - Tüm girdilerin doğrulanması ve sanitize edilmesi
  - JSON şema doğrulaması
  - İstek boyutu sınırlamaları

- **Response Güvenliği**
  - Hassas bilgilerin filtrelenmesi
  - Hata mesajlarında detay sınırlaması
  - Güvenlik başlıklarının uygulanması

### Güvenlik Başlıkları

Tüm API yanıtlarında aşağıdaki güvenlik başlıkları uygulanır:

```
Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none';
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
Cache-Control: no-store, max-age=0
```

## Güvenlik Gereksinimleri

NexFit geliştirme ekibi, aşağıdaki güvenlik gereksinimlerine uymalıdır:

### Kod Geliştirme Güvenliği

- **Güvenli Kodlama Standartları**
  - OWASP Secure Coding Practices'e uyum
  - Kod gözden geçirme süreçlerinde güvenlik kontrolü
  - Otomatik statik kod analizi

- **Gizli Bilgi Yönetimi**
  - Kod deposuna hassas bilgilerin (API anahtarları, parolalar) asla dahil edilmemesi
  - Vault veya güvenli AWS/Azure parametre depoları kullanımı
  - Ortam değişkenleri için güvenli yönetim

- **Bağımlılık Güvenliği**
  - Tüm bağımlılıkların düzenli güvenlik taraması
  - SCA (Software Composition Analysis) araçlarının kullanımı
  - Güvenlik açığı durumunda otomatik uyarı

### Altyapı Güvenliği

- **Sunucu Güvenliği**
  - Sunucular en son yamalarla güncel tutulur
  - Tüm sistemler sıkılaştırılmıştır
  - İzin verilen en az ayrıcalıklı çalışma hesabı

- **Ağ Güvenliği**
  - Segmentasyon (VLAN, Subnet)
  - Güvenlik Duvarı kuralları
  - VPN erişimi
  - Şifreli iletişim

- **Konteyner Güvenliği**
  - Docker imajları için tarama
  - En az ayrıcalık ilkesi
  - Güvenli konfigürasyon

## Zafiyet Yönetimi

NexFit, güvenlik açıklarını proaktif olarak yönetir ve yanıtlar:

### Güvenlik Açığı Tarama

- **Otomatik Taramalar**
  - Haftalık altyapı taramaları
  - Sürekli kod güvenlik analizleri
  - Dependency scanning (npm audit, Snyk)

- **Manuel Güvenlik Denetimleri**
  - Yılda 2 kez harici güvenlik denetimleri
  - İçsel güvenlik ekibi tarafından kod denetimleri
  - Penetrasyon testleri

### Zafiyet Açıklama Politikası

NexFit, güvenlik araştırmacılarının sistem güvenliğine katkıda bulunmasını teşvik eder:

- [security@nexfit.io](mailto:security@nexfit.io) üzerinden zafiyet raporları kabul edilir
- Sorumlu açıklama süreci takip edilir (90 gün düzeltme süresi)
- Ödül programı ile ciddi güvenlik açıkları için ödül verilir

### Zafiyet Yanıt Süreci

1. **Alındı Bildirimi**: Güvenlik açığı raporu 24 saat içinde alındı olarak işaretlenir
2. **Doğrulama**: Güvenlik ekibi açığı doğrular ve etkisini değerlendirir
3. **Çözüm Geliştirme**: Teknik ekip düzeltme geliştirir
4. **Test**: Düzeltme test edilir ve regresyon testleri yapılır
5. **Dağıtım**: Düzeltme üretim ortamına dağıtılır
6. **Bildirim**: İlgili paydaşlara ve rapor eden kişiye bildirim yapılır
7. **Kapanış**: Zafiyet kapatılır ve dokümante edilir

## Güvenlik İhlali Yanıt Planı

Bir güvenlik ihlali durumunda, NexFit aşağıdaki süreci izler:

### İhlal Yanıt Ekibi

- Güvenlik Yöneticisi
- Sistem Yöneticileri
- Hukuk Temsilcisi
- İletişim/PR Temsilcisi
- İlgili Ürün/Geliştirme Yöneticileri

### İhlal Yanıt Süreci

1. **Tespit ve Değerlendirme**
   - İhlalin kapsamını ve etkisini belirleme
   - Olayı kayıt altına alma ve belgeleme

2. **Çevreleme**
   - İhlalin yayılmasını önleme
   - Etkilenen sistemleri izole etme
   - Acil durum erişim kontrollerini uygulama

3. **Eradikasyon**
   - İhlal nedenini ortadan kaldırma
   - Tüm saldırı vektörlerini kapatma
   - Sistemleri temizleme

4. **Kurtarma**
   - Sistemleri güvenli bir duruma geri yükleme
   - Verileri yedeklerden geri getirme
   - Hizmetleri kademeli olarak tekrar başlatma

5. **İletişim**
   - İç paydaşları bilgilendirme
   - Düzenleyici kurumları bilgilendirme (gerekli ise)
   - Etkilenen kullanıcıları bilgilendirme (gerekli ise)

6. **Post-Mortem Analiz**
   - İhlalin kök neden analizi
   - Öğrenilen derslerin dokümantasyonu
   - Güvenlik iyileştirmelerinin uygulanması

### Olay Seviye Tanımları

| Seviye | Tanım | Örnek | Yanıt Süresi |
|--------|-------|-------|--------------|
| **Kritik** | Büyük veri ihlali, sisteme yetkisiz tam erişim | Kullanıcı verilerinin sızması | Anında (1 saat içinde) |
| **Yüksek** | Sınırlı veri erişimi veya hizmet kesintisi | API'ye yetkisiz erişim | 4 saat içinde |
| **Orta** | Potansiyel güvenlik açığı veya şüpheli aktivite | Şüpheli giriş denemeleri | 24 saat içinde |
| **Düşük** | Küçük konfigürasyon hataları, rutin güvenlik uyarıları | Eksik güvenlik başlıkları | 72 saat içinde |

## Güvenlik Testleri

NexFit, güvenliği doğrulamak için kapsamlı test süreçleri uygular:

### Otomatik Güvenlik Testleri

- **SAST (Static Application Security Testing)**
  - SonarQube
  - ESLint güvenlik kuralları
  - GitLeaks (gizli bilgi taraması)

- **DAST (Dynamic Application Security Testing)**
  - OWASP ZAP
  - Burp Suite

- **Bağımlılık Taraması**
  - npm audit
  - Snyk
  - Dependabot

- **İnfrastruktur Taraması**
  - Terrascan (IaC güvenlik taraması)
  - Docker imaj taraması
  - Bulut konfigürasyon taraması

### Manuel Güvenlik Testleri

- **Penetrasyon Testleri**
  - Yılda 2 kez kapsamlı pentest
  - Her büyük sürüm öncesi kritik alanlarda hedefli testler

- **Güvenlik Kod İncelemeleri**
  - Güvenlik kritik değişiklikler için manuel kod incelemesi
  - Güvenlikle ilgili değişikliklerde uzman gözden geçirmesi

- **Tehdit Modelleme**
  - Büyük yeni özellikler için tehdit modelleme egzersizleri
  - Güvenlik önlemlerinin doğrulanması için senaryo tabanlı analiz

### CI/CD Güvenlik Entegrasyonu

Güvenlik testleri CI/CD pipeline'ına entegre edilmiştir:

```yaml
# Örnek GitHub Actions iş akışı
security:
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v2
    
    - name: Gizli bilgi taraması
      uses: gitleaks/gitleaks-action@v1.6.0
    
    - name: Bağımlılık güvenlik taraması
      run: npm audit --audit-level=moderate
    
    - name: SAST taraması
      uses: sonarsource/sonarcloud-github-action@master
    
    - name: Docker imaj taraması
      uses: aquasecurity/trivy-action@master
      with:
        image-ref: 'nexfit-app:latest'
```

## Uyumluluk ve Sertifikasyonlar

NexFit, aşağıdaki düzenleyici gereksinimlere ve standartlara uyum sağlar:

### Veri Koruma Düzenlemeleri

- **GDPR (General Data Protection Regulation)**
  - Veri saklama ve işleme onayları
  - Veri erişim ve silme hakları
  - Veri koruma etki değerlendirmeleri

- **KVKK (Kişisel Verilerin Korunması Kanunu)**
  - Türkiye'deki kullanıcılar için veri koruma uyumluluğu
  - Açık rıza mekanizmaları
  - VERBİS kaydı

### Güvenlik Standartları

- **ISO 27001**
  - Bilgi Güvenliği Yönetim Sistemi
  - Düzenli denetimler ve uygunluk doğrulaması

- **SOC 2 Tip II**
  - Güvenlik, kullanılabilirlik, işlem bütünlüğü, gizlilik ve mahremiyet kontrolleri

- **OWASP Uyumluluğu**
  - OWASP Top 10 güvenlik riskleri adreslenmiştir
  - OWASP ASVS (Application Security Verification Standard) gereksinimleri izlenir

### Sektöre Özgü Standartlar

- **Sağlık Verileri için HIPAA Uyumluluğu** (ABD pazarı için)
  - PHI (Korunan Sağlık Bilgileri) koruma önlemleri
  - Şifreleme ve erişim kontrolleri

## Güvenlik İletişim Kanalları

### Güvenlik Sorunları Bildirimi

Güvenlik açıkları veya endişeleri için:
- E-posta: [security@nexfit.io](mailto:security@nexfit.io)
- Güvenlik açığı bildirimi sayfası: [https://nexfit.io/security/vulnerability-disclosure](https://nexfit.io/security/vulnerability-disclosure)

### Güvenlik Dokümantasyonu

- NexFit Güvenlik Whitepapers: [https://nexfit.io/security/whitepapers](https://nexfit.io/security/whitepapers)
- Güvenlik Blog: [https://nexfit.io/blog/security](https://nexfit.io/blog/security)

### Güvenlik İletişimi

- Güvenlik Duyuruları: [https://nexfit.io/security/announcements](https://nexfit.io/security/announcements)
- Güvenlik Bültenleri: Kritik güvenlik güncellemeleri için e-posta aboneliği

---

Bu güvenlik dokümantasyonu NexFit'in güvenlik uygulamalarını ve politikalarını detaylandırır. Soru veya endişeleriniz varsa, lütfen [security@nexfit.io](mailto:security@nexfit.io) adresine e-posta gönderin.

Son Güncelleme: 20 Mart 2023 