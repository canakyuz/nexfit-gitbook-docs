# 📊 NexFit İzleme ve Günlükleme Rehberi

Bu rehber, NexFit uygulamasının üretim ortamında izlenmesi, günlükleme stratejileri ve performans metriklerinin takibi için standartları belirler. Bu süreçler, uygulamanın sağlıklı çalıştığından emin olmak, sorunları proaktif olarak tespit etmek ve çözmek için kritik öneme sahiptir.

## İçindekiler

- [İzleme Stratejisi](#i̇zleme-stratejisi)
- [Günlükleme (Logging)](#günlükleme-logging)
- [Metrik Toplama](#metrik-toplama)
- [Uyarı ve Bildirimleri Yapılandırma](#uyarı-ve-bildirimleri-yapılandırma)
- [Performans İzleme](#performans-i̇zleme)
- [İzleme Panoları (Dashboards)](#i̇zleme-panoları-dashboards)
- [Kapasite Planlama](#kapasite-planlama)
- [Olay Yanıt Süreci](#olay-yanıt-süreci)
- [İzleme Bakımı ve Geliştirme](#i̇zleme-bakımı-ve-geliştirme)

## İzleme Stratejisi

NexFit'in izleme stratejisi, çeşitli boyutlarda uygulamanın sağlığını ve performansını takip etmeyi amaçlar.

### İzleme Katmanları

1. **Altyapı İzleme**: Sunucular, konteynerler, veritabanları ve ağ bileşenlerinin sağlığı ve kaynakları.
2. **Uygulama İzleme**: API endpoint'leri, mikroservisler, istek hacimleri ve hata oranları.
3. **İş Metrikleri İzleme**: Kullanıcı aktivitesi, dönüşüm oranları ve kritik iş süreçleri.
4. **Güvenlik İzleme**: Olağandışı erişim kalıpları, yetkilendirme hataları ve potansiyel güvenlik tehditleri.

### İzleme Araçları

NexFit, izleme ve günlükleme için aşağıdaki araçları kullanır:

| Araç | Kullanım Alanı | Açıklama |
|------|----------------|----------|
| **Prometheus** | Metrik toplama | Zaman serisi verileri toplar ve uyarı mantığını yönetir |
| **Grafana** | Görselleştirme | Metrikler için izleme panoları ve uyarı görüntüleme |
| **ELK Stack** | Log yönetimi | Elasticsearch, Logstash ve Kibana ile log toplama ve analiz |
| **Jaeger** | Dağıtık izleme | Mikroservisler arası istek izleme ve performans analizi |
| **Healthchecks.io** | Canlılık izleme | Kritik servislerin periyodik olarak çalışıp çalışmadığını kontrol eder |
| **PagerDuty** | Uyarı yönetimi | Uyarıları ekibe ileten ve müdahale süreçlerini yöneten servis |

## Günlükleme (Logging)

NexFit, tutarlı ve etkili bir şekilde bilgi toplamak için yapılandırılmış günlükleme kullanır.

### Günlük Seviyeleri

| Seviye | Kullanım Durumu |
|--------|-----------------|
| **ERROR** | Uygulamanın normal işlevselliğini engelleyen kritik hatalar |
| **WARN** | Potansiyel sorunlar veya beklenmeyen durumlar, ancak uygulama hâlâ çalışıyor |
| **INFO** | Önemli operasyonel olaylar (başlangıç, kapanış, yapılandırma değişiklikleri) |
| **DEBUG** | Geliştirme ve sorun giderme için ayrıntılı bilgiler (üretim ortamında genellikle devre dışı) |
| **TRACE** | En ayrıntılı günlük seviyesi, yalnızca belirli hata ayıklama senaryolarında kullanılır |

### Yapılandırılmış Günlük Formatı

Tüm günlükler, aşağıdaki yapılandırılmış JSON formatında tutulur:

```json
{
  "timestamp": "2023-03-14T12:34:56.789Z",
  "level": "INFO",
  "service": "user-service",
  "trace_id": "abcdef123456",
  "user_id": "user_123",  // Eğer uygunsa
  "message": "Kullanıcı başarıyla giriş yaptı",
  "context": {
    "ip": "192.168.1.1",
    "user_agent": "Mozilla/5.0...",
    "method": "POST",
    "path": "/api/auth/login",
    "response_time": 120
  }
}
```

### Günlük Toplama ve Saklama

- Tüm mikroservisler, günlüklerini stdout/stderr'e gönderir
- Kubernetes/Docker logging driver, günlükleri yakalayıp Logstash'e iletir
- Logstash, günlükleri işler ve Elasticsearch'e gönderir
- Günlükler 30 gün süreyle saklanır, daha sonra arşivlenir
- Kritik hatalar ve güvenlik olayları 1 yıl süreyle saklanır

### Hassas Bilgi Yönetimi

- Parolalar, tokenlar ve kişisel veriler **asla** günlüklere yazılmaz
- Zorunlu durumlarda bilgiler maskelenir: `password: "********"`
- Her servis, hangi bilgilerin günlükleneceğine dair net yönergelere sahiptir

## Metrik Toplama

NexFit, dört ana kategoride metrik toplar:

### 1. RED Metrikleri (Tüm Servisler İçin)

- **Rate**: Saniyedeki istek sayısı
- **Errors**: Hata sayısı veya oranı
- **Duration**: İstek işleme süreleri

### 2. USE Metrikleri (Altyapı İçin)

- **Utilization**: Kaynak kullanım oranı (CPU, Bellek)
- **Saturation**: Kaynak doygunluğu (kuyruklardaki iş miktarı)
- **Errors**: Sistem hataları

### 3. İş Metrikleri

- Aktif kullanıcı sayısı
- Abonelik dönüşüm oranları
- Oluşturulan program sayısı
- Kayıtlı antrenman oturumları
- Ödeme işlemi başarı oranları

### 4. Özel Uygulama Metrikleri

- API endpoint kullanım oranları
- Veritabanı sorgu performansı
- Cache hit/miss oranları
- WebSocket bağlantı sayıları

### Metrik Etiketleme

Tüm metrikler, detaylı analiz için aşağıdaki etiketlerle etiketlenir:

```
environment="production"
service="user-service"
instance="user-service-pod-abc123"
region="eu-west-1"
endpoint="/api/users/{id}"
status_code="200"
```

## Uyarı ve Bildirimleri Yapılandırma

NexFit, sorunları proaktif olarak tespit etmek için çeşitli uyarı kuralları kullanır.

### Uyarı Öncelikleri

| Öncelik | Yanıt Süresi | Bildirim Kanalı | Örnek Senaryo |
|---------|--------------|-----------------|---------------|
| **P0 - Kritik** | 5 dakika | Telefon araması, SMS, E-posta | Sistem çökmesi, veri kaybı riski |
| **P1 - Yüksek** | 15 dakika | SMS, E-posta, Slack | API'lar çalışıyor ama yavaş, kritik bir iş süreci başarısız |
| **P2 - Orta** | 30 dakika | E-posta, Slack | Performans sorunları, tekrarlayan küçük hatalar |
| **P3 - Düşük** | İş saatlerinde | E-posta, Jira | Kozmetik sorunlar, optimize edilebilecek alanlar |

### Temel Uyarı Kuralları

```yaml
# Örnek Prometheus Uyarı Kuralları

# Servis Canlılık Kontrolü
- alert: ServiceDown
  expr: up{job="nexfit-services"} == 0
  for: 1m
  labels:
    severity: critical
  annotations:
    summary: "Servis Çöktü: {{ $labels.instance }}"
    description: "{{ $labels.service }} servisi {{ $labels.instance }} üzerinde en az 1 dakikadır çalışmıyor."

# Yüksek Hata Oranı
- alert: HighErrorRate
  expr: sum(rate(http_requests_total{status=~"5.."}[5m])) / sum(rate(http_requests_total[5m])) > 0.05
  for: 2m
  labels:
    severity: high
  annotations:
    summary: "Yüksek Hata Oranı: {{ $labels.service }}"
    description: "{{ $labels.service }} son 2 dakikada %5'ten fazla hata oranına sahip."

# API Gecikme Süresi
- alert: APIHighLatency
  expr: histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket{handler!="healthcheck"}[5m])) by (le, handler)) > 0.5
  for: 5m
  labels:
    severity: warning
  annotations:
    summary: "API Yüksek Gecikme: {{ $labels.handler }}"
    description: "{{ $labels.handler }} endpoint'i son 5 dakikada %95 yüzdelikte 500ms'den uzun süre yanıt veriyor."
```

### Uyarı Fatigue'ini Önleme

- Benzer uyarıları gruplandırma
- Uyarı sessizleştirme dönemleri (maintenance windows)
- Flapping'i önlemek için "for" süresi kullanımı
- Periyodik uyarı gözden geçirmeleri

## Performans İzleme

### Anahtar Performans Göstergeleri (KPI)

| Metrik | Hedef | Açıklama |
|--------|-------|----------|
| **Servis Uptime** | >99.9% | Servislerin erişilebilir olma süresi |
| **API Yanıt Süresi (p95)** | <300ms | API isteklerinin %95'inin 300ms'den hızlı yanıt vermesi |
| **API Yanıt Süresi (p99)** | <1s | API isteklerinin %99'unun 1 saniyeden hızlı yanıt vermesi |
| **Hata Oranı** | <0.1% | Başarısız isteklerin toplam isteklere oranı |
| **CPU Kullanımı** | <70% | Ortalama CPU kullanımı hedefi |
| **Bellek Kullanımı** | <80% | Ortalama bellek kullanımı hedefi |

### Otomatik Ölçeklendirme Metrikleri

- **HPA (Horizontal Pod Autoscaler)**: CPU kullanımı >70% olduğunda pod sayısını artır
- **VPA (Vertical Pod Autoscaler)**: Bellek ve CPU gereksinimlerini otomatik olarak ayarla
- **Database Connection Pool**: Etkin bağlantı sayısına göre havuz boyutunu ayarla

### Performans Testi Entegrasyonu

- CI/CD pipeline'ında otomatik yük testleri
- Üretim benzeri ortamda periyodik dayanıklılık testleri
- A/B testleri için performans karşılaştırması

## İzleme Panoları (Dashboards)

NexFit, farklı kullanıcı grupları için özelleştirilmiş izleme panoları sunar:

### 1. Genel Bakış Panosu

![Genel Bakış Panosu](../assets/images/monitoring/overview-dashboard.png)

- Tüm servislerin sağlık durumu
- Aktif kullanıcı sayısı
- Toplam istek hacmi
- Sistem kaynakları kullanımı
- Son uyarılar

### 2. Teknik Operasyon Panosu

- Detaylı servis sağlığı
- API endpoint başarı/hata oranları
- Veritabanı performans metrikleri
- Kubernetes pod durumları
- Ölçeklendirme olayları

### 3. İş Analitik Panosu

- Yeni kullanıcı kayıtları
- Abonelik dönüşüm oranları
- Günlük antrenman girişleri
- Premium özelliklerin kullanımı
- Kullanıcı grupları bazında aktif kullanım

### 4. Güvenlik İzleme Panosu

- Başarısız giriş denemeleri
- API rate limit aşımları
- Şüpheli erişim kalıpları
- İzin/yetki hataları
- Hassas veri erişim günlükleri

## Kapasite Planlama

NexFit, kapasite planlaması için aşağıdaki metrikleri kullanır:

### Trend Analizi

- Aylık/haftalık/günlük kullanıcı artış oranları
- İstek hacmi büyüme eğilimleri
- Depolama alanı kullanım artışı
- Veritabanı büyüme oranı

### Kapasite Tahmin Modelleri

- Mevcut büyüme oranlarına dayalı 3-6-12 aylık projeksiyonlar
- Özel durumlar için (örn. pazarlama kampanyaları) spesifik modeller
- Sezonsal dalgalanmaları hesaba katan tahminler

### Proaktif Ölçeklendirme Eşikleri

- Ortalama CPU kullanımı >60% olduğunda kapasite artışı planla
- Veritabanı depolama alanı >70% dolduğunda genişletme planla
- Bağlantı havuzu kullanımı >75% olduğunda genişlet

## Olay Yanıt Süreci

Bir uyarı veya olay durumunda izlenecek adımlar:

### 1. Olay Tespiti

- Otomasyon aracılığıyla olay tespiti (uyarılar)
- Kullanıcı geri bildirimleri ile olay tespiti
- Proaktif izleme ile olay tespiti

### 2. Olay Sınıflandırması

- Olayın etki düzeyi ve kapsamının belirlenmesi
- Uygun önceliğin atanması
- İlgili ekiplerin haberdar edilmesi

### 3. Olay Yanıtı

- İlk müdahale ve tanı
- Kök neden analizi
- Geçici çözüm uygulaması
- Kalıcı düzeltme planlaması

### 4. İletişim Planı

- İç paydaşlara bildirim
- Etkilenen kullanıcılara bildirim
- Durum güncellemeleri
- Çözüm bildirimi

### 5. Olay Sonrası İnceleme

- Retrospektif toplantısı
- Kök neden dokümantasyonu
- Öğrenilen dersler
- İyileştirme önerileri ve takibi

## İzleme Bakımı ve Geliştirme

İzleme sistemimizin sürekli iyileştirilmesi için:

### Düzenli Bakım Görevleri

- Aylık uyarı kuralları gözden geçirme
- Günlük saklama politikalarını gözden geçirme
- İzleme araçları sürüm güncellemeleri
- Dashboard temizliği ve optimizasyonu

### İzleme Sistemi İyileştirmeleri

- Yeni servisler için izleme entegrasyonu
- Özel iş metriklerinin eklenmesi
- Uyarı korelasyonu ve AIOps entegrasyonu
- İzleme veri hacminin optimize edilmesi

### İzleme Eğitimi

- Yeni ekip üyeleri için izleme sistemleri eğitimi
- Uyarı yanıt prosedürleri için düzenli alıştırmalar
- İzleme en iyi uygulamaları dokümantasyonunun güncellenmesi

---

Bu rehber, NexFit uygulamasının izleme ve günlükleme standartlarını belirler. Tüm geliştirme ve operasyon ekipleri bu standartlara uymalıdır. Sorularınız veya önerileriniz için, DevOps ekibiyle iletişime geçin.

Son Güncelleme: 14 Mart 2023 