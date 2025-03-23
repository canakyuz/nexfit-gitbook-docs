# 🛟 NexFit Felaket Kurtarma Planı

Bu doküman, NexFit uygulamasının beklenmedik felaket durumlarında iş sürekliliğini sağlamak ve veri kaybını önlemek için felaket kurtarma stratejisini, politikalarını ve prosedürlerini tanımlar.

## İçindekiler

- [Amaç ve Kapsam](#amaç-ve-kapsam)
- [Felaket Senaryoları](#felaket-senaryoları)
- [Kurtarma Stratejisi](#kurtarma-stratejisi)
- [Kurtarma Hedefleri](#kurtarma-hedefleri)
- [Rol ve Sorumluluklar](#rol-ve-sorumluluklar)
- [Yedekleme Stratejisi](#yedekleme-stratejisi)
- [Kriz İletişim Planı](#kriz-i̇letişim-planı)
- [Felaket Kurtarma Prosedürleri](#felaket-kurtarma-prosedürleri)
- [Test ve Doğrulama](#test-ve-doğrulama)
- [Sürekli İyileştirme](#sürekli-i̇yileştirme)

## Amaç ve Kapsam

Bu felaket kurtarma planının amacı, doğal afetler, siber saldırılar, donanım arızaları veya insan kaynaklı hatalar gibi felaket durumlarında NexFit uygulamasının ve verilerinin korunmasını ve kurtarılmasını sağlamaktır. Bu plan, kritik iş fonksiyonlarının ve servislerin kabul edilebilir bir süre içinde yeniden çalışır hale getirilmesine rehberlik eder.

### Kapsam

Bu plan aşağıdaki unsurları kapsar:

- Uygulama sunucuları
- Veritabanı sunucuları
- Depolama sistemleri
- Ağ altyapısı
- Bulut kaynakları
- Yedekleme sistemleri
- Kritik üçüncü taraf servisler

## Felaket Senaryoları

Aşağıdaki felaket senaryoları için hazırlıklı olmalıyız:

### Doğal Afetler

- **Deprem, sel, yangın**: Veri merkezi altyapısını etkileyen doğal afetler
- **Elektrik kesintileri**: Uzun süreli güç kesintileri
- **İletişim ağı kesintileri**: İnternet veya geniş alan ağı bağlantı sorunları

### Teknik Felaketler

- **Donanım arızaları**: Sunucu, depolama veya ağ donanımı arızaları
- **Yazılım hataları**: Kritik sistem hatalarına neden olan yazılım sorunları
- **Veri bozulması**: Veritabanı tutarsızlığı veya bozulması
- **Sistem güvenlik ihlali**: Kötü amaçlı yazılım, fidye yazılımı veya veri sızıntısı

### İnsan Kaynaklı Hatalar

- **Yanlış yapılandırma**: Kritik sistem ayarlarının yanlış yapılandırılması
- **Veri silme**: Kazara veya kasıtlı veri silme
- **Erişim ihlali**: Yetkisiz sistem değişiklikleri

## Kurtarma Stratejisi

NexFit, çoklu bölge ve bulut hizmet sağlayıcılarına dayalı karma bir yaklaşım benimser:

### Bölgeler Arası Yedekleme

- Birincil Bölge: **AWS EU West (İrlanda)**
- İkincil Bölge: **AWS EU Central (Frankfurt)**
- Üçüncül Yedekleme: **Google Cloud (Belçika)**

### Kurtarma Yaklaşımları

1. **Sıcak Yedekleme (Hot Standby)**: Kritik hizmetler için
   - Aktif-aktif yapılandırma
   - Gerçek zamanlı veri replikasyonu
   - Otomatik yük devretme (failover)

2. **Ilık Yedekleme (Warm Standby)**: Orta öncelikli hizmetler için
   - Aktif-pasif yapılandırma
   - Near-real-time veri replikasyonu
   - Yarı otomatik yük devretme

3. **Soğuk Yedekleme (Cold Standby)**: Düşük öncelikli hizmetler için
   - Düzenli yedeklemelerden kurtarma
   - Manuel yük devretme

## Kurtarma Hedefleri

### Kurtarma Süresi Hedefi (RTO)

| Hizmet Kategorisi | RTO | Açıklama |
|-------------------|-----|----------|
| Tier 1 - Kritik | < 1 saat | Kullanıcı kimlik doğrulama, temel API hizmetleri |
| Tier 2 - Yüksek | < 4 saat | Antrenman takibi, ödeme işlemleri |
| Tier 3 - Orta | < 8 saat | Raporlama, analitik, sosyal özellikler |
| Tier 4 - Düşük | < 24 saat | Yönetim panoları, geçmiş veri arşivleri |

### Kurtarma Noktası Hedefi (RPO)

| Hizmet Kategorisi | RPO | Açıklama |
|-------------------|-----|----------|
| Tier 1 - Kritik | < 5 dakika | Kullanıcı profilleri, abonelikler |
| Tier 2 - Yüksek | < 30 dakika | Günlük antrenman verileri, ölçümler |
| Tier 3 - Orta | < 2 saat | Kullanıcı etkileşimleri, bildirimler |
| Tier 4 - Düşük | < 24 saat | İstatistikler, trend verileri |

## Rol ve Sorumluluklar

### Felaket Kurtarma Ekibi

| Rol | Sorumluluklar | İletişim |
|-----|---------------|----------|
| **DR Yöneticisi** | Kurtarma çabalarını koordine etmek, karar vermek | [dr-manager@nexfit.com](mailto:dr-manager@nexfit.com), +90 555 123 4567 |
| **Altyapı Lideri** | Sunucu ve ağ altyapısını kurtarmak | [infra-lead@nexfit.com](mailto:infra-lead@nexfit.com), +90 555 123 4568 |
| **Veritabanı Yöneticisi** | Veritabanı kurtarma ve bütünlük kontrolü | [db-admin@nexfit.com](mailto:db-admin@nexfit.com), +90 555 123 4569 |
| **Uygulama Yöneticisi** | Uygulama hizmetlerini yeniden başlatmak ve doğrulamak | [app-admin@nexfit.com](mailto:app-admin@nexfit.com), +90 555 123 4570 |
| **Güvenlik Uzmanı** | Güvenlik ihlallerini değerlendirmek ve ele almak | [security@nexfit.com](mailto:security@nexfit.com), +90 555 123 4571 |
| **İletişim Koordinatörü** | Paydaşlarla iletişim kurmak | [comms@nexfit.com](mailto:comms@nexfit.com), +90 555 123 4572 |

### Karar Alma Yetkisi

- **Seviye 1 (< 10K kullanıcı etkilendiğinde)**: Hizmet Ekibi Lideri
- **Seviye 2 (< 100K kullanıcı etkilendiğinde)**: CTO veya Operasyon Direktörü
- **Seviye 3 (> 100K kullanıcı etkilendiğinde)**: CEO ve Yönetim Kurulu

## Yedekleme Stratejisi

### Veri Yedekleme Sıklığı

| Veri Türü | Yedekleme Sıklığı | Saklama Süresi | Konum |
|-----------|-------------------|----------------|-------|
| Veritabanı | Her saat | 24 saat | Birincil ve ikincil bölgeler |
| Veritabanı | Günlük | 30 gün | Tüm bölgeler |
| Veritabanı | Haftalık | 12 ay | Soğuk depolama |
| Kullanıcı profil verileri | Gerçek zamanlı replikasyon | - | Birincil ve ikincil bölgeler |
| Uygulama kodu | Her dağıtımda | Tüm sürümler | Kod deposu ve arşiv |
| Medya ve dosyalar | Günlük değişimler | 90 gün | Çoklu bölge depolama |
| Yapılandırma | Her değişiklikte | 10 sürüm | Versiyon kontrollü depo |

### Yedekleme Teknolojileri

- **Veritabanı**: MongoDB Atlas otomatik yedekleme + özel anlık görüntüler
- **Nesne Depolama**: AWS S3 çapraz bölge replikasyonu + GCP yedekleme
- **Dosya Sistemi**: EBS anlık görüntüleri + uzak depolama
- **Gerçek Zamanlı Veriler**: Redis verilerinin düzenli persistans ve replikasyonu
- **Yapılandırma**: Git tabanlı altyapı kodu + Terraform durum yedeklemeleri

## Kriz İletişim Planı

### İç İletişim

1. **Bildirim Kanalları**:
   - Birincil: PagerDuty uyarı sistemi
   - İkincil: Kurumsal Slack kanalları (#dr-alerts, #sre-911)
   - Üçüncül: SMS ve arama ağacı

2. **Durum Güncellemeleri**:
   - 30 dakikada bir durum toplantıları (felaket süresince)
   - Slack kanalında sürekli güncellemeler
   - Kurumsal bilgilendirme e-postaları

### Dış İletişim

1. **Müşteri İletişimi**:
   - Durum sayfası: [status.nexfit.com](https://status.nexfit.com)
   - Uygulama içi bildirimler
   - E-posta duyuruları (yalnızca büyük olaylar için)
   - Sosyal medya güncellemeleri

2. **İş Ortakları**:
   - Doğrudan e-posta bildirimleri
   - Ayrılmış iletişim kanalı
   - API durum bildirimleri

3. **Medya İletişimi** (gerekirse):
   - Yalnızca İletişim Departmanı üzerinden
   - Onaylanmış basın açıklamaları
   - Sözcü: CTO veya CEO

## Felaket Kurtarma Prosedürleri

### Felaket Tespiti ve Bildirim

1. İzleme sistemleri bir anormallik algıladığında:
   - Otomatik uyarılar tetiklenir
   - Nöbetçi mühendis sorunu değerlendirir
   - 15 dakika içinde çözülmeyen sorunlar için DR ekibi toplanır

2. Felaket durumu ilan edildiğinde:
   - DR Yöneticisi olay yönetimini üstlenir
   - Ekip üyelerine otomatik çağrı yapılır
   - İlk değerlendirme toplantısı başlatılır

### Temel Kurtarma Adımları

#### 1. Bölgesel Kesinti Kurtarma Prosedürü

```
1. DR Yöneticisi, Felaket Kurtarma modunu ilan eder
2. AWS EU West'ten AWS EU Central'a yük devretme başlatılır:
   a. Route 53 sağlık kontrolü başarısız olursa otomatik tetiklenir
   b. Manuel tetikleme: `./scripts/failover.sh --region eu-central-1 --mode full`
3. Veritabanı yöneticisi veritabanı kümesini kontrol eder:
   a. `mongo-atlas switchover --to-region eu-central-1`
   b. Veri bütünlüğünü doğrula: `./scripts/verify-db-integrity.sh`
4. Uygulama yöneticisi servisleri başlatır:
   a. `kubectl apply -f k8s/dr-config/`
   b. Servis kontrolü: `./scripts/verify-services.sh --region eu-central-1`
5. Altyapı lideri CDN ve DNS yapılandırmasını günceller:
   a. Cloudfront dağıtımını güncelle: `./scripts/update-cdn.sh --failover`
   b. İkincil DNS kayıtlarını etkinleştir: `./scripts/dns-failover.sh`
6. Güvenlik uzmanı güvenlik kontrollerini gerçekleştirir:
   a. WAF kurallarını doğrula: `./scripts/verify-waf.sh --region eu-central-1`
   b. Giriş kontrolleri: `./scripts/verify-access-controls.sh`
7. İletişim koordinatörü durum sayfasını günceller ve bildirimleri gönderir
```

#### 2. Veritabanı Bozulması Kurtarma Prosedürü

```
1. Soruna bağlı olarak tam veya kısmi kurtarma kararı verilir
2. Veritabanının etkilenen bölümünü izole et:
   a. Salt okunur mod: `./scripts/db-readonly.sh --collection [etkilenen_koleksiyon]`
3. Son bilinen iyi duruma geri dönmek için:
   a. Nokta kurtarması: `./scripts/db-restore.sh --timestamp "YYYY-MM-DD HH:MM:SS"`
   b. Tam koleksiyon kurtarma: `./scripts/db-restore.sh --collection [koleksiyon] --latest`
4. Transaction log'larını yeniden oynat:
   a. Belirli noktaya kadar: `./scripts/replay-oplog.sh --until "YYYY-MM-DD HH:MM:SS"`
5. Veri bütünlüğünü doğrula:
   a. Doğrulama betikleri çalıştır: `./scripts/validate-schema.sh`
   b. İlişkisel bütünlük kontrolü: `./scripts/check-references.sh`
6. Servisleri kademeli olarak yeniden etkinleştir:
   a. Önce salt okunur modda başlat: `./scripts/enable-services.sh --read-only`
   b. Bütünlük doğrulandıktan sonra: `./scripts/enable-services.sh --full`
```

#### 3. Siber Saldırı Kurtarma Prosedürü

```
1. Etkilenen sistemleri hemen izole et:
   a. Ağ bölümlendirme: `./scripts/isolate-network.sh --segment [segment_id]`
   b. Güvenlik duvarı kurallarını güncelle: `./scripts/update-firewall.sh --lockdown`
2. Temiz bir ortamda yeni altyapı hazırla:
   a. Yeni VPC oluştur: `terraform apply -var-file=configs/clean-vpc.tfvars`
   b. Temiz AMI'lardan yeni sunucular başlat: `./scripts/deploy-clean-infra.sh`
3. En son temiz yedekten verileri kurtar:
   a. Yedek bütünlüğünü doğrula: `./scripts/verify-backup.sh --backup-id [id]`
   b. Temiz ortama geri yükle: `./scripts/restore-to-clean.sh --backup-id [id]`
4. Güvenlik önlemlerini artır:
   a. Ek WAF kuralları uygula: `./scripts/enhance-waf.sh --threat-level high`
   b. IDS/IPS hassasiyetini artır: `./scripts/update-ids.sh --sensitivity high`
5. Kademeli olarak trafiği yeni ortama yönlendir:
   a. Canary testi: `./scripts/traffic-shift.sh --percentage 5`
   b. Tam geçiş: `./scripts/traffic-shift.sh --percentage 100`
```

### Kurtarma Sonrası Aktiviteler

1. **Etki Değerlendirmesi**:
   - Etkilenen sistemlerin ve verilerin kapsamını belgele
   - Hizmet kesintisi süresini ve etkisini ölç
   - Müşteri etkisini değerlendir

2. **Kök Neden Analizi**:
   - Olayın kök nedenini belirle
   - Katkıda bulunan faktörleri tanımla
   - Benzer felaketleri önlemek için öneriler geliştir

3. **Raporlama**:
   - İç teknik rapor hazırla
   - Yönetim özeti oluştur
   - Gerekirse düzenleyici rapor hazırla

## Test ve Doğrulama

### Test Programı

| Test Türü | Sıklık | Kapsam | Sorumlular |
|-----------|--------|--------|------------|
| Masa Başı Egzersizleri | Aylık | Senaryoları ve prosedürleri gözden geçirme | Tüm DR ekibi |
| Bileşen Kurtarma Testi | Üç ayda bir | Belirli sistemlerin kurtarılması | Sistem sahipleri |
| Simülasyon Testi | Altı ayda bir | Belirli bir felaket senaryosunu simüle etme | DR ekibi |
| Tam Kurtarma Tatbikatı | Yıllık | Tam bölge yük devretme ve kurtarma | Tüm teknik ekip |

### Test Senaryoları

1. **Veritabanı Kurtarma Testi**:
   - Ana veritabanını salt okunur moduna geçir
   - Yedek veritabanından kurtarma yap
   - Veri bütünlüğünü doğrula

2. **Bölge Yük Devretme Testi**:
   - EU West bölgesinin çöktüğünü varsay
   - EU Central'a otomatik yük devretmeyi test et
   - Tüm hizmetlerin çalıştığını doğrula

3. **Ağ Kesintisi Testi**:
   - Ana ağ bağlantısının kesildiğini simüle et
   - Yedek bağlantı üzerinden çalışmayı doğrula
   - VPN yedekliliğini test et

## Sürekli İyileştirme

### Değerlendirme ve Gözden Geçirme

- Her testten sonra "öğrenilen dersler" oturumu düzenle
- Çeyrek bazında DR planını gözden geçir ve güncelle
- Yıllık olarak tüm felaket kurtarma stratejisini değerlendir

### Eğitim ve Bilinçlendirme

- Yeni ekip üyelerine DR eğitimi ver
- Çeyrek bazında tüm ekip için tazeleme eğitimleri düzenle
- DR planındaki değişiklikler hakkında düzenli bilgilendirmeler yap

### Plan Güncelleme Süreci

1. Güncelleme önerilerini topla
2. DR komitesi tarafından değerlendir
3. Onaylanan değişiklikleri belgeye ekle
4. Sürüm numarasını ve değişiklik günlüğünü güncelle
5. Güncellenmiş planı tüm paydaşlara dağıt

---

## Ekler

### Ek A: Kurtarma Kontrol Listeleri

#### A.1 Felaket Değerlendirme Kontrol Listesi

- [ ] Olayın kapsamını ve etkisini belirle
- [ ] Etkilenen sistemleri ve hizmetleri tanımla
- [ ] Tahmini kurtarma süresini hesapla
- [ ] Felaket seviyesini belirle (1, 2 veya 3)
- [ ] İlgili ekip üyelerine bildirim gönder
- [ ] İlk durum toplantısını planla

#### A.2 Yük Devretme Kontrol Listesi

- [ ] Yük devretme kararını dokümante et
- [ ] DNS değişikliklerini başlat
- [ ] Veritabanı yük devretmeyi etkinleştir
- [ ] Uygulama sunucularını ikincil bölgede başlat
- [ ] Hizmet durumunu doğrula
- [ ] Kullanıcı trafiğini kademeli olarak yönlendir
- [ ] Durum sayfasını güncelle

### Ek B: İletişim Matrisi

| Durum | Kime Bildirilecek | Nasıl | Kim Tarafından | Zaman Çerçevesi |
|-------|-------------------|-------|---------------|----------------|
| Felaket Uyarısı | DR Ekibi | PagerDuty, Slack | Nöbetçi Mühendis | Hemen |
| Felaket İlanı | Tüm Teknik Ekip | E-posta, Slack | DR Yöneticisi | 15 dakika içinde |
| Kurtarma Başlangıcı | Departman Liderleri | E-posta, Toplantı | DR Yöneticisi | 30 dakika içinde |
| Durum Güncellemesi | Tüm Çalışanlar | E-posta, Intranet | İletişim Koordinatörü | 1 saat içinde, sonra her 2 saatte |
| Müşteri Bildirimi | Aktif Kullanıcılar | Durum Sayfası, Uygulama İçi | İletişim Koordinatörü | Felaket ilanından 1 saat sonra |
| İş Ortağı Bildirimi | Entegrasyon Ortakları | E-posta, API | İş Geliştirme | 2 saat içinde |
| Çözüm Bildirimi | Tüm Paydaşlar | Tüm Kanallar | İletişim Koordinatörü | Hizmet geri geldiğinde |

### Ek C: Sistem Bağımlılık Haritası

![Sistem Bağımlılık Haritası](../assets/images/dr/system-dependency-map.png)

### Ek D: Kritik Tedarikçi İletişim Bilgileri

| Tedarikçi | Hizmet | İletişim Kişisi | İletişim Bilgileri | Destek ID |
|-----------|--------|-----------------|---------------------|-----------|
| AWS | Bulut Altyapı | Teknik Müdür | [aws-support@nexfit.com](mailto:aws-support@nexfit.com) | 1234-5678 |
| MongoDB Atlas | Veritabanı Hizmeti | Veritabanı Yöneticisi | [mongodb-support@nexfit.com](mailto:mongodb-support@nexfit.com) | MDB-9876 |
| Cloudflare | CDN & Güvenlik | Güvenlik Mühendisi | [cloudflare-support@nexfit.com](mailto:cloudflare-support@nexfit.com) | CF-5432 |
| Stripe | Ödeme İşlemleri | Finans Direktörü | [payments@nexfit.com](mailto:payments@nexfit.com) | STRIPE-1357 |
| SendGrid | E-posta Hizmeti | İletişim Müdürü | [email-support@nexfit.com](mailto:email-support@nexfit.com) | SG-2468 |

---

**Belge Bilgileri**

- **Sürüm**: 1.2
- **Son Güncelleme**: 15 Mart 2023
- **Onaylayan**: CTO, Adem Yılmaz
- **İletişim**: [dr-team@nexfit.com](mailto:dr-team@nexfit.com)

**Değişiklik Geçmişi**

| Sürüm | Tarih | Değişiklikler | Onaylayan |
|-------|-------|---------------|-----------|
| 1.0 | 10.01.2023 | İlk sürüm | CTO |
| 1.1 | 20.02.2023 | RTO/RPO hedeflerini güncelleme | CTO |
| 1.2 | 15.03.2023 | Siber saldırı prosedürlerini ekleme | CTO, CISO | 