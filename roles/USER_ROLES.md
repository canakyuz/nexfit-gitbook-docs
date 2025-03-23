# NexFit Kullanıcı Rolleri ve Sorumlulukları

Bu doküman, NexFit uygulamasındaki tüm kullanıcı rollerini, bu rollere ait sorumlulukları ve yetkileri detaylı olarak açıklamaktadır.

## Kullanıcı Rolleri Genel Bakış

| Rol | Açıklama | Temel Sorumluluklar | Arayüz Tipi |
|-----|----------|---------------------|-------------|
| CLIENT | Standart fitness kullanıcısı | Antrenman ve beslenme takibi yapma | Fitness izleme odaklı |
| COACH | Profesyonel fitness antrenörü | Danışanlar için program hazırlama | Yönetim ve içerik oluşturma odaklı |
| COACH_CLIENT | Hem antrenör hem danışan | Kendi fitness yolculuğunu takip etme ve danışanları yönetme | Hibrit arayüz |
| GYM_OWNER | Spor salonu işletmecisi | Tesis, personel ve üye yönetimi | İşletme yönetimi odaklı |
| GYM_MEMBER | Spor salonu üyesi | Salon olanaklarını kullanma ve derslere katılma | Üyelik odaklı |
| GYM_COACH | Salon bünyesindeki antrenör | Salon üyeleriyle çalışma ve grup dersleri verme | Salon içi operasyon odaklı |
| STUDIO_OWNER | PT stüdyo işletmecisi | Stüdyo, antrenör ve danışan yönetimi | Butik işletme yönetimi odaklı |
| STUDIO_COACH | Stüdyo antrenörü | Birebir danışanlarla çalışma | Kişiselleştirilmiş antrenörlük odaklı |
| STUDIO_CLIENT | Stüdyo danışanı | Özel antrenör ile çalışma | Premium danışan deneyimi odaklı |

## Rol Detayları ve Sorumlulukları

### 1. CLIENT (Bireysel Sporcu)

**Tanım:** Fitness hedeflerine ulaşmak için uygulamayı kullanan bireysel sporcular.

**Sorumluluklar:**
- Kişisel antrenman programlarını takip etme
- Beslenme planlarını uygulama
- İlerleme verilerini güncel tutma
- Ölçüm girişlerini düzenli yapma
- Antrenman kayıtlarını tutma

**Kullanıcı Deneyimi:**
- Kolay anlaşılır egzersiz rehberleri
- Beslenme takibi araçları
- Görsel ilerleme grafikleri
- Motivasyon özellikleri
- Self-servis antrenman ve beslenme içerikleri

**Tipik Kullanım Senaryoları:**
- Günlük antrenman yapma ve kaydetme
- Beslenme planını takip etme
- Vücut ölçümlerini girme ve ilerlemeyi izleme
- Hazır programları keşfetme ve satın alma
- Antrenör arama ve anlaşma yapma

### 2. COACH (Antrenör)

**Tanım:** Danışanlara profesyonel fitness ve beslenme rehberliği sunan uzmanlar.

**Sorumluluklar:**
- Kişiselleştirilmiş antrenman programları hazırlama
- Beslenme planları oluşturma
- Danışan ilerlemesini takip etme
- Düzenli geri bildirim sağlama
- Danışan portföyünü yönetme

**Kullanıcı Deneyimi:**
- Sezgisel program oluşturma araçları
- Detaylı danışan yönetim paneli
- İlerleme analiz grafikleri
- Zaman yönetimi ve randevu araçları
- İçerik pazarlama özellikleri

**Tipik Kullanım Senaryoları:**
- Danışan ekranını inceleme ve geri bildirim verme
- Yeni antrenman ve beslenme programları oluşturma
- Danışanlarla iletişim kurma
- Program pazarında içerik paylaşma
- Randevu programını yönetme

### 3. COACH_CLIENT (Antrenörlü Sporcu)

**Tanım:** Hem kendi fitness yolculuğunu takip eden hem de danışanlara hizmet veren kullanıcılar.

**Sorumluluklar:**
- İki farklı rol arasında etkin geçiş yapma
- Kendi antrenman ve beslenme planlarını takip etme
- Danışanlarına profesyonel hizmet sunma
- Her iki rolü dengeleyebilme

**Kullanıcı Deneyimi:**
- Rol değiştirme özelliği
- Her iki role özgü özelliklere tam erişim
- Verimli zaman yönetimi araçları
- Kişisel ve profesyonel içerikleri ayırma

**Tipik Kullanım Senaryoları:**
- Sabah kendi antrenmanını yapma, akşam danışanlarını yönetme
- Danışanlara önermeden önce programları kendinde test etme
- Hem antrenör hem sporcu olarak ilerleme kaydetme

### 4. GYM_OWNER (Salon Sahibi)

**Tanım:** Spor salonu veya fitness merkezi sahibi veya yöneticisi.

**Sorumluluklar:**
- Tesisin genel yönetimi ve operasyonu
- Üye kazanımı ve üyelik yönetimi
- Personel yönetimi ve ders programı oluşturma
- Mali yönetim ve performans takibi
- Ekipman bakım ve yenileme

**Kullanıcı Deneyimi:**
- Kapsamlı işletme yönetim paneli
- Detaylı mali ve operasyonel raporlama
- Personel yönetim araçları
- Üyelik ve kampanya yönetimi
- Tesis optimizasyon araçları

**Tipik Kullanım Senaryoları:**
- Günlük, haftalık, aylık performans raporlarını inceleme
- Personel çalışma saatlerini düzenleme
- Üyelik kampanyalarını yönetme
- Salon doluluk analizlerini inceleme
- Ekipman bakım takibi yapma

### 5. GYM_MEMBER (Salon Üyesi)

**Tanım:** Bir spor salonuna üye olan ve salon olanaklarını kullanan bireyler.

**Sorumluluklar:**
- Üyelik bilgilerini güncel tutma
- Salon kurallarına uyma
- Ders rezervasyonlarını yönetme
- Salon ekipmanlarını uygun şekilde kullanma

**Kullanıcı Deneyimi:**
- Kolay salon check-in
- Ders rezervasyon sistemi
- Üyelik bilgileri ve yenileme araçları
- Salon olanakları bilgisi
- Antrenör ve ders keşfetme

**Tipik Kullanım Senaryoları:**
- Salonun çalışma saatlerini kontrol etme
- Grup derslerine kayıt olma
- Salon içi PT hizmetleri için rezervasyon yapma
- Üyelik durumunu ve süresini kontrol etme
- Salon antrenörleriyle iletişim kurma

### 6. GYM_COACH (Salon Antrenörü)

**Tanım:** Spor salonu bünyesinde çalışan, grup dersleri veren ve üyelerle birebir çalışan antrenörler.

**Sorumluluklar:**
- Grup dersleri düzenleme ve yönetme
- Üyelere egzersiz rehberliği sağlama
- Ekipman kullanımı konusunda destek verme
- Ders programını takip etme
- Salon kurallarını ve standartlarını uygulama

**Kullanıcı Deneyimi:**
- Ders programı yönetim ekranı
- Üye bilgi erişimi
- Grup performans takibi
- Salon ekipman durumu görüntüleme
- Mesai takip sistemi

**Tipik Kullanım Senaryoları:**
- Günlük ders programını kontrol etme
- Sınıf katılım listelerini görüntüleme
- Özel danışanlara antrenman programı hazırlama
- Grup dersi için hazırlık yapma
- Salon yönetimine raporlar sunma

### 7. STUDIO_OWNER (PT Stüdyo Sahibi)

**Tanım:** Kişisel antrenman hizmetleri sunan butik stüdyonun sahibi veya yöneticisi.

**Sorumluluklar:**
- Stüdyo operasyonlarını yönetme
- Antrenör kadrosunu yönetme
- Müşteri portföyünü geliştirme
- Mali yönetim ve performans takibi
- Hizmet kalitesini standartlaştırma

**Kullanıcı Deneyimi:**
- Butik işletme yönetim paneli
- Antrenör performans metrikleri
- Müşteri ilişkileri yönetimi
- Gelir ve seans analizleri
- Stüdyo doluluk optimizasyonu

**Tipik Kullanım Senaryoları:**
- Günlük seans programını inceleme
- Antrenörlerin performansını değerlendirme
- Müşteri memnuniyetini takip etme
- Gelir projeksiyonlarını analiz etme
- Pazarlama faaliyetlerini yönetme

### 8. STUDIO_COACH (Stüdyo Antrenörü)

**Tanım:** PT stüdyosunda çalışan, birebir danışan eğitimi sunan uzman antrenörler.

**Sorumluluklar:**
- Kişiselleştirilmiş birebir antrenman hizmeti sunma
- Her danışan için özel programlar hazırlama
- Danışan gelişimini düzenli takip etme
- Seans notları ve ölçümler tutma
- Yüksek kalitede hizmet sağlama

**Kullanıcı Deneyimi:**
- Kişiselleştirilmiş seans yönetimi
- Detaylı danışan profilleri
- Seans planlama ve takvim
- Danışan ilerleme takibi
- Uzman kaynaklar ve araçlar

**Tipik Kullanım Senaryoları:**
- Günlük seans programını kontrol etme
- Danışan notlarını güncelleme
- Yeni seanslara hazırlık yapma
- Danışan ilerlemesini kaydetme
- Stüdyo yönetimine raporlar sunma

### 9. STUDIO_CLIENT (Stüdyo Danışanı)

**Tanım:** PT stüdyosunda birebir antrenman hizmeti alan premium danışanlar.

**Sorumluluklar:**
- Düzenli seanslara katılma
- Antrenör talimatlarını uygulama
- Gelişim verilerini güncelleme
- Stüdyo kurallarına uyma
- Seanslar için zamanında hazır olma

**Kullanıcı Deneyimi:**
- Özel antrenör profili ve iletişim
- Kişiselleştirilmiş antrenman programı
- Premium rezervasyon sistemi
- Detaylı ilerleme takibi
- VIP destek erişimi

**Tipik Kullanım Senaryoları:**
- Antrenman programını kontrol etme
- Antrenör ile randevu ayarlama
- Seans notlarını ve geri bildirimleri görüntüleme
- İlerleme raporlarını inceleme
- Antrenörle mesajlaşma

## Roller Arası Geçiş ve Çoklu Roller

NexFit, kullanıcıların birden fazla role sahip olabilmesine ve roller arasında kolayca geçiş yapabilmesine olanak tanır.

### Yaygın Rol Kombinasyonları:

1. **COACH + CLIENT**
   - Kendi fitness yolculuğunu da takip eden antrenörler
   - Kişisel ve profesyonel arayüzler arasında geçiş

2. **GYM_OWNER + COACH**
   - Hem salon işleten hem de antrenörlük yapan profesyoneller
   - İşletme ve antrenör özellikleri arasında geçiş

3. **STUDIO_OWNER + STUDIO_COACH**
   - Kendi stüdyosunda çalışan PT'ler
   - Yönetim ve antrenörlük görevleri arasında denge

### Rol Geçiş Deneyimi:

- Tek dokunuşla rol değiştirme
- Rol bazlı bildirim yönetimi
- Rol bazlı arayüz tercihleri
- Her role özel kısayollar ve özellikler

## Rol-Tabanlı Arayüz Adaptasyonu

NexFit, kullanıcının rolüne bağlı olarak arayüzü otomatik olarak uyarlar:

### 1. Navigasyon Yapısı
Farklı roller için özelleştirilmiş ana menü ve alt menüler

### 2. Veri Görünürlüğü
Role göre veri erişimi ve görüntüleme seviyeleri

### 3. Özellik Erişimi
Role özgü araçlar ve özellikler

### 4. İşlevsellik
Role uygun işlem akışları ve süreçler

### 5. Bildirimler
Role özgü bildirim türleri ve öncelikler

## Rol Seviyeleri ve İlerleme

Bazı roller içinde seviye sistemi bulunur:

### COACH Seviyeleri:
- **Başlangıç:** 0-10 danışan, temel özelliklere erişim
- **İleri:** 11-30 danışan, gelişmiş analitik özellikler
- **Uzman:** 31+ danışan, tüm premium özelliklere erişim

### GYM_OWNER Seviyeleri:
- **Tek Lokasyon:** 1 salon, temel yönetim araçları
- **Çoklu Lokasyon:** 2-5 salon, gelişmiş merkezi yönetim
- **Kurumsal:** 6+ salon, kurumsal analitik ve özelleştirme

## Uygulama İçi Roller Matrisi

| Özellik | CLIENT | COACH | GYM_OWNER | GYM_MEMBER | GYM_COACH | STUDIO_OWNER | STUDIO_COACH | STUDIO_CLIENT |
|---------|--------|-------|-----------|------------|-----------|--------------|--------------|---------------|
| Antrenman Takibi | ✅ | ⚫ | ⚫ | ✅ | ⚫ | ⚫ | ⚫ | ✅ |
| Beslenme Takibi | ✅ | ⚫ | ⚫ | ✅ | ⚫ | ⚫ | ⚫ | ✅ |
| İlerleme Analizi | ✅ | ✅ | ⚫ | ✅ | ⚫ | ⚫ | ✅ | ✅ |
| Program Oluşturma | ⚫ | ✅ | ⚫ | ⚫ | ✅ | ⚫ | ✅ | ⚫ |
| Danışan Yönetimi | ⚫ | ✅ | ⚫ | ⚫ | ✅ | ⚫ | ✅ | ⚫ |
| İşletme Analitiği | ⚫ | ⚫ | ✅ | ⚫ | ⚫ | ✅ | ⚫ | ⚫ |
| Personel Yönetimi | ⚫ | ⚫ | ✅ | ⚫ | ⚫ | ✅ | ⚫ | ⚫ |
| Üyelik Yönetimi | ⚫ | ⚫ | ✅ | ⚫ | ⚫ | ✅ | ⚫ | ⚫ |
| Ders Yönetimi | ⚫ | ⚫ | ✅ | ⚫ | ✅ | ✅ | ✅ | ⚫ |
| Ders Rezervasyonu | ⚫ | ⚫ | ⚫ | ✅ | ⚫ | ⚫ | ⚫ | ✅ |
| Seans Planlaması | ⚫ | ✅ | ⚫ | ⚫ | ✅ | ⚫ | ✅ | ⚫ |
| Seans Rezervasyonu | ⚫ | ⚫ | ⚫ | ✅ | ⚫ | ⚫ | ⚫ | ✅ |
| Finansal Raporlar | ⚫ | ✅ | ✅ | ⚫ | ⚫ | ✅ | ✅ | ⚫ |
| İçerik Pazarlama | ⚫ | ✅ | ✅ | ⚫ | ✅ | ✅ | ✅ | ⚫ |
| Çoklu Lokasyon | ⚫ | ⚫ | ✅ | ⚫ | ⚫ | ✅ | ⚫ | ⚫ |

✅ = Tam Erişim, ⚫ = Erişim Yok veya Sınırlı Erişim

## Güvenlik ve İzin Modeli

NexFit'in rol tabanlı erişim kontrolü (RBAC) aşağıdaki prensiplere dayanır:

1. **En Az Ayrıcalık Prensibi**
   - Her rol sadece ihtiyaç duyduğu kaynaklara erişebilir

2. **Hiyerarşik Erişim**
   - Üst roller, alt rollerin yetkilerine sahiptir

3. **Veri İzolasyonu**
   - Her rol sadece ilgili verilere erişebilir

4. **İşlevsel Erişim**
   - Roller, işlevlere göre gruplanmış izinlere sahiptir

5. **Rol Kombinasyonları**
   - Birden fazla rol, birleştirilmiş izin kümelerine sahiptir

## Gelecek Planları

Rol yapısında planlanan geliştirmeler:

1. **Özel Rol Tanımları**
   - İşletmelerin kendi özel rol tanımlarını oluşturabilmesi

2. **Alt Roller**
   - Ana rollerin altında daha spesifik alt roller tanımlama

3. **Geçici Rol Atamaları**
   - Belirli süreler için geçici rol yetkilendirmeleri

4. **Rol Tabanlı Analitik**
   - Her rolün performansını ve kullanım desenlerini analiz etme

5. **Gelişmiş Rol Geçişleri**
   - Rol geçişlerinde bağlam koruma ve akıllı devralma 