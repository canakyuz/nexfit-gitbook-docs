# NexFit Mimari Dokümantasyonu

Bu belge, NexFit uygulamasının mimari yapısını, bileşenlerini, teknoloji seçimlerini ve tasarım kararlarını detaylandırmaktadır.

## Mimari Genel Bakış

NexFit, modern, ölçeklenebilir ve dayanıklı bir mimari üzerine inşa edilmiştir. Uygulama mimarisi aşağıdaki temel prensiplere dayanmaktadır:

1. **Modüler Tasarım**: Bağımsız olarak geliştirilebilen, test edilebilen ve dağıtılabilen bileşenler
2. **Ölçeklenebilirlik**: Artan kullanıcı ve veri hacmini destekleyebilen mimari
3. **Güvenlik**: Çok katmanlı güvenlik yapısı ile hassas kullanıcı verilerinin korunması
4. **Performans**: Düşük gecikme süresi ve yüksek verimlilik
5. **Genişletilebilirlik**: Yeni özelliklerin ve entegrasyonların kolayca eklenebilmesi

## Sistem Mimarisi

NexFit, aşağıdaki ana bileşenlerden oluşan bir mikro-servis tabanlı mimariye sahiptir:

```
                   ┌─────────────────┐
                   │                 │
                   │  İstemci Katmanı │
                   │                 │
                   └────────┬────────┘
                            │
                            ▼
┌───────────────────────────────────────────────────┐
│                                                   │
│                API Gateway Katmanı                │
│                                                   │
└───────┬───────────┬─────────────┬────────────────┘
        │           │             │
        ▼           ▼             ▼
┌──────────────┐ ┌─────────┐ ┌──────────┐
│              │ │         │ │          │
│  Kimlik      │ │ İş      │ │ İçerik   │
│  Servisi     │ │ Servisi │ │ Servisi  │
│              │ │         │ │          │
└──────┬───────┘ └────┬────┘ └────┬─────┘
       │              │           │
       │              ▼           │
       │         ┌──────────┐     │
       │         │          │     │
       └────────►│ Veritabanı◄─────┘
                 │          │
                 └──────────┘
```

### 1. İstemci Katmanı

İstemci katmanı, kullanıcıların NexFit ile etkileşime girdiği arayüzleri içerir:

- **Mobil Uygulamalar**: React Native ile geliştirilen iOS ve Android uygulamaları
- **Web Uygulaması**: React ve Next.js ile geliştirilen responsive web uygulaması
- **Admin Paneli**: İç yönetim ve analitik için dahili web arayüzü

### 2. API Gateway Katmanı

API Gateway, istemciler ile backend servisleri arasında bir ara katman olarak görev yapar:

- **İstek Yönlendirme**: İstemci isteklerini ilgili servislere yönlendirme
- **Kimlik Doğrulama**: API isteklerinin kimlik doğrulaması
- **Hız Sınırlama**: DDoS koruması ve adil kullanım politikası uygulama
- **Önbellekleme**: Sık kullanılan verilerin önbelleklenmesi
- **İzleme ve Analitik**: API kullanım metriklerinin toplanması

### 3. Mikroservisler

NexFit, aşağıdaki temel mikroservislere ayrılmıştır:

#### 3.1. Kimlik Servisi
- Kullanıcı kimlik doğrulama ve yetkilendirme
- Oturum yönetimi
- Rol tabanlı erişim kontrolü (RBAC)
- Sosyal medya entegrasyonları ile giriş
- İki faktörlü kimlik doğrulama (2FA)

#### 3.2. Kullanıcı Servisi
- Kullanıcı profil yönetimi
- Tercihleri ve ayarları
- Bildirim tercihleri
- Kullanıcı rol yönetimi

#### 3.3. Antrenman Servisi
- Antrenman programı oluşturma ve yönetme
- Egzersiz kütüphanesi
- Antrenman günlüğü ve takibi
- Performans analitiği

#### 3.4. Beslenme Servisi
- Beslenme planları oluşturma ve yönetme
- Besin veritabanı ve günlük takip
- Makro ve mikro besin hesaplamaları
- Öğün planlaması ve alışveriş listesi oluşturma

#### 3.5. İşletme Servisi
- Spor salonu ve stüdyo yönetimi
- Personel ve üye yönetimi
- Ders ve etkinlik programlaması
- Abonelik ve fatura yönetimi

#### 3.6. İçerik Servisi
- Program ve içerik pazaryeri
- İçerik dağıtımı ve yönetimi
- Kullanıcı değerlendirmeleri ve incelemeler
- İçerik önerileri

#### 3.7. Analitik Servisi
- İş zekası ve raporlama
- Kullanıcı davranış analitiği
- Performans ölçümleri ve öngörüler
- A/B test analizi

#### 3.8. Bildirim Servisi
- Push bildirimleri
- E-posta bildirimleri
- Uygulama içi bildirimler
- Bildirim zamanlaması ve hedefleme

#### 3.9. Entegrasyon Servisi
- Üçüncü taraf API entegrasyonları
- Giyilebilir cihaz entegrasyonları
- Sağlık uygulamaları entegrasyonları
- Ödeme sistemleri entegrasyonları

### 4. Veritabanı Katmanı

NexFit, veri saklama için hibrit bir veritabanı yaklaşımı kullanır:

- **MongoDB**: Ana doküman veritabanı, kullanıcı profilleri, antrenman programları vb.
- **Redis**: Önbellekleme, oturum yönetimi ve anlık veri için
- **Elasticsearch**: Arama ve analitik için
- **MinIO**: Medya içeriği (görüntüler, videolar) için nesne depolama
- **TimescaleDB**: Zaman serisi verileri için (ölçümler, performans verileri)

## Teknoloji Yığını

### Backend
- **Programlama Dili**: Node.js (TypeScript)
- **API Framework**: Express.js / NestJS
- **Veritabanı**: MongoDB, Redis, Elasticsearch, TimescaleDB
- **Nesne Depolama**: MinIO
- **API Gateway**: Kong
- **Kimlik Doğrulama**: JWT, OAuth 2.0
- **İletişim**: REST, GraphQL, WebSockets

### Frontend
- **Web**: React, Next.js, TypeScript
- **Mobil**: React Native, TypeScript
- **State Yönetimi**: Redux Toolkit
- **UI Kütüphanesi**: Custom Design System, Tailwind CSS
- **API İstemcisi**: Axios, Apollo Client (GraphQL)

### DevOps & Altyapı
- **Konteynerizasyon**: Docker
- **Orkestrasyon**: Kubernetes
- **CI/CD**: GitHub Actions
- **İzleme**: Prometheus, Grafana
- **Günlük Kaydı**: ELK Stack (Elasticsearch, Logstash, Kibana)
- **Altyapı**: AWS / DigitalOcean
- **CDN**: Cloudflare

## Veri Akışı

### Örnek Veri Akışı: Antrenman Oluşturma

```
┌────────────┐  1. Antrenman Oluştur  ┌─────────────┐
│            ├────────────────────────►            │
│  İstemci   │                        │ API Gateway │
│            │◄────────────────────────            │
└────────────┘  8. Yanıt Dön          └──────┬──────┘
                                             │
                                             │ 2. İsteği Yönlendir
                                             ▼
┌────────────────┐  3. Kullanıcı Kontrolü  ┌──────────────────┐
│                ◄───────────────────────────                │
│ Kimlik Servisi │                          │ Antrenman Servisi│
│                ├───────────────────────────                │
└────────────────┘  4. Kullanıcı Doğrulandı └────────┬─────────┘
                                                     │
                                                     │ 5. Antrenmanı Kaydet
                                                     ▼
                                           ┌──────────────────┐
                                           │                  │
                                           │    Veritabanı    │
                                           │                  │
                                           └────────┬─────────┘
                                                    │
                                                    │ 6. Kaydedildi
                                                    ▼
┌────────────────┐  7. Bildirim Gönder   ┌──────────────────┐
│                ◄─────────────────────────                │
│ Bildirim Servisi│                       │ Antrenman Servisi│
│                │                       │                │
└────────────────┘                       └──────────────────┘
```

## Mimari Tasarım Kararları

### 1. Mikro-servis Mimarisi

**Karar**: Monolitik yerine mikro-servis mimarisi kullanımı.

**Gerekçe**:
- Bağımsız ölçeklendirme: Her servisi kendi ihtiyaçlarına göre ölçeklendirebilme
- Teknoloji çeşitliliği: Her servis için en uygun teknolojinin seçilebilmesi
- Dayanıklılık: Bir servisteki sorunun tüm sistemi etkilememesi
- Geliştirme hızı: Farklı ekiplerin paralel çalışabilmesi

### 2. React Native Kullanımı

**Karar**: Hibrit yerine React Native ile çapraz platform mobil uygulama geliştirme.

**Gerekçe**:
- Performans: Native-benzeri performans
- Kod paylaşımı: iOS ve Android arasında yüksek oranda kod paylaşımı
- Geliştirici deneyimi: Web geliştiricilerin mobil geliştirmeye kolay geçiş yapabilmesi
- Topluluk desteği: Geniş topluluk ve ekosistem

### 3. Hibrit Veritabanı Yaklaşımı

**Karar**: Tek bir veritabanı türü yerine, MongoDB ve PostgreSQL'in birlikte kullanıldığı hibrit bir veritabanı yaklaşımı.

**Gerekçe**:
- **Veri Çeşitliliği**: Farklı veri tipleri için en uygun veritabanının kullanılması
- **MongoDB**: Esnek şema gerektiren, hızlı değişen ve karmaşık yapıdaki veriler için kullanılır.
  - Kullanıcı profilleri
  - Antrenman programları
  - Esnek içerik yapıları
  - Kayıt günlükleri
- **PostgreSQL**: İlişkisel veri, finansal işlemler ve güçlü ACID uyumluluğu gerektiren veriler için kullanılır.
  - Finansal işlemler ve faturalandırma kayıtları
  - İş kritik müşteri verileri
  - Rapor ve analitik verileri
  - Güçlü ilişkisel bütünlük gerektiren veriler
- **Ölçeklenebilirlik**: Her veritabanının güçlü yönlerinden faydalanma imkanı
- **Performans Optimizasyonu**: Veri tipine ve kullanım senaryosuna göre en uygun veritabanı seçimi 
- **Risk Dağıtımı**: Tek bir veritabanı teknolojisine bağımlılığın azaltılması

### 4. GraphQL ve REST Hibrit Yaklaşımı

**Karar**: Sadece REST veya sadece GraphQL yerine, hibrit bir API yaklaşımı.

**Gerekçe**:
- Esneklik: Her kullanım senaryosu için en uygun teknolojinin seçilebilmesi
- Verimlilik: İstemci tarafında veri tüketiminin optimize edilmesi
- Aşamalı geçiş: Mevcut REST API'lerinden GraphQL'e kademeli geçiş imkanı
- Kullanım durumu uygunluğu: Belirli tipteki veriler için REST, karmaşık sorgular için GraphQL

## Güvenlik Mimarisi

NexFit'in güvenlik mimarisi aşağıdaki katmanlardan oluşur:

### 1. Ağ Güvenliği
- SSL/TLS şifreleme (tüm iletişim için HTTPS)
- Web Application Firewall (WAF)
- DDoS koruması
- IP bazlı erişim kısıtlamaları

### 2. Kimlik Doğrulama ve Yetkilendirme
- JWT tabanlı kimlik doğrulama
- Role-Based Access Control (RBAC)
- OAuth 2.0 entegrasyonu
- İsteğe bağlı iki faktörlü kimlik doğrulama (2FA)

### 3. Veri Güvenliği
- Hassas kullanıcı verilerinin şifrelenmesi
- Veri tabanında şifreleme-at-rest
- Şifre hash'leme (bcrypt)
- Veri anonimleştirme ve maskeleme

### 4. Uygulama Güvenliği
- Input validation ve sanitization
- XSS ve CSRF koruması
- Rate limiting
- Content Security Policy (CSP)
- SQL/NoSQL injection koruması

### 5. İzleme ve Denetim
- Güvenlik olayları izleme ve uyarı sistemi
- Kullanıcı aktivite günlükleri
- Güvenlik açıkları için düzenli tarama
- Penetrasyon testleri

## Ölçeklenebilirlik Stratejisi

NexFit'in büyümesini desteklemek için uygulanan ölçeklenebilirlik stratejileri:

### 1. Yatay Ölçeklendirme
- Mikroservislerin konteyner orchestration ile yatay ölçeklendirilmesi
- Veritabanı sharding ve replikasyonu
- Stateless servis tasarımı

### 2. Önbellekleme Stratejisi
- Çok seviyeli önbellekleme (CDN, API Gateway, Veritabanı)
- Redis ile yaygın sorguların önbelleklenmesi
- Client-side caching ile ağ trafiğinin azaltılması

### 3. Asenkron İşleme
- Yoğun işlemler için mesaj kuyruğu kullanımı (RabbitMQ/Kafka)
- Batch işlemler için zamanlanmış görevler
- Background worker'lar ile uzun süren işlemlerin yönetimi

### 4. Veri Partitioning
- Coğrafi veri dağıtımı
- Zaman bazlı veri bölümleme
- Sıcak/ılık/soğuk veri stratejisi

## Hata Toleransı ve Dayanıklılık

NexFit'in hizmet sürekliliğini sağlamak için uygulanan stratejiler:

### 1. Devre Kesici Deseni
- Microservislerde devre kesici (circuit breaker) implementasyonu
- Hatalardan sonra kademeli yeniden deneme stratejisi
- Fallback mekanizmaları

### 2. Yük Dengeleme
- Servisler arasında dinamik yük dengeleme
- Health check ile sağlıksız servislerin devre dışı bırakılması
- Traffic splitting ile risk azaltma

### 3. Veri Dayanıklılığı
- Veritabanı replikasyonu ve otomatik failover
- Çoklu bölge yedekleme stratejisi
- Point-in-time recovery seçenekleri

### 4. Felaket Kurtarma
- Düzenli yedekleme ve kurtarma testleri
- Multi-region deployment
- Hizmet sürekliliği planı ve dokümantasyonu

## Performans Optimizasyonu

Sistem performansını optimize etmek için uygulanan stratejiler:

### 1. Frontend Optimizasyonu
- Code splitting ve lazy loading
- Statik varlıkların CDN üzerinden sunulması
- Progressive rendering
- Image optimization ve lazy loading

### 2. API Optimizasyonu
- API dökümantasyonu ve response optimization
- Endpoint consolidation
- Denormalizasyon ve query optimizasyonu
- GraphQL ile under-fetching/over-fetching önleme

### 3. Veritabanı Optimizasyonu
- İndeksleme stratejisi
- Query optimizasyonu
- Açıklama analizi
- Bağlantı havuzu yönetimi

### 4. Ağ Optimizasyonu
- HTTP/2 ve HTTP/3 kullanımı
- Content compression
- Connection pooling
- Edge computing ve CDN kullanımı

## Entegrasyon Mimarisi

NexFit'in üçüncü taraf sistemlerle entegrasyonu için mimari:

### 1. API Stratejisi
- RESTful API standartları
- GraphQL entegrasyon noktaları
- Webhook desteği
- API versiyonlama stratejisi

### 2. Entegrasyon Desenleri
- Adapter deseni
- Facade deseni
- Event-driven entegrasyon
- API Gateway routing

### 3. Desteklenen Entegrasyonlar
- Fitness cihazları ve giyilebilir teknolojiler
- Sağlık ve beslenme uygulamaları
- Ödeme işlemcileri
- Pazarlama ve CRM platformları

## DevOps ve Dağıtım Stratejisi

NexFit'in geliştirme ve dağıtım süreci için yaklaşım:

### 1. CI/CD Pipeline
- Otomatik derleme ve test
- Sürekli entegrasyon ve dağıtım
- Otomatik kabul testleri
- Canary ve blue-green deployments

### 2. Ortam Stratejisi
- Geliştirme, test, staging ve üretim ortamları
- Ortam yönetimi için Infrastructure as Code (IaC)
- Ortam özellikleri için feature flags
- Ortam izolasyonu ve güvenliği

### 3. İzleme ve Günlük Kaydı
- Distributed tracing
- Centralized logging
- Metrik toplama ve görselleştirme
- Alarm ve uyarı sistemi

## Gelecek Mimari Planları

NexFit'in mimari yol haritasında planlanan geliştirmeler:

### 1. Teknoloji Yükseltmeleri
- GraphQL kullanımının genişletilmesi
- Serverless mimari entegrasyonu
- Edge computing yeteneklerinin artırılması

### 2. AI/ML Entegrasyonu
- Kişiselleştirilmiş antrenman önerileri için ML modelleri
- Gelişmiş form analizi için computer vision
- Kullanıcı davranış tahminleri için öneri sistemleri

### 3. Altyapı İyileştirmeleri
- Multi-cloud stratejisi
- Global ölçekli deployment
- Daha granüler mikroservisler

### 4. Geliştirici Ekosistemleri
- Public API ve Developer Portal
- SDK'lar ve client kütüphaneleri
- Partner entegrasyon programı

## Sonuç

NexFit mimarisi, modern yazılım geliştirme prensiplerine, ölçeklenebilirlik ihtiyaçlarına ve fitness endüstrisinin özel gereksinimlerine uygun olarak tasarlanmıştır. Mikro-servis mimarisi, veritabanı çeşitliliği ve rol tabanlı mimari yaklaşımı ile NexFit, farklı kullanıcı tipleri için özelleştirilmiş, performanslı ve güvenli bir deneyim sunmaktadır.

Uygulama mimarisi, gelecekteki büyüme ve genişleme ihtiyaçlarını karşılayacak şekilde esneklik, bakım kolaylığı ve teknolojik çeviklik göz önünde bulundurularak tasarlanmıştır. 