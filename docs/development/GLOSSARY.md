# 📚 NexFit Terminoloji Sözlüğü

Bu sözlük, NexFit projesi kapsamında kullanılan terimler, kısaltmalar ve jargonların tutarlı bir şekilde anlaşılması için oluşturulmuştur. Projeye yeni katılan geliştiriciler, paydaşlar ve kullanıcılar için referans niteliğindedir.

## İçindekiler

- [Kullanıcı Rolleri](#kullanıcı-rolleri)
- [İş Terimleri](#i̇ş-terimleri)
- [Teknik Terimler](#teknik-terimler)
- [Metrikler](#metrikler)
- [Statüler ve Durumlar](#statüler-ve-durumlar)
- [Kısaltmalar](#kısaltmalar)

## Kullanıcı Rolleri

| Terim | Açıklama |
|-------|----------|
| **Admin** | Sistemin tüm yönlerini yönetme yetkisine sahip kullanıcı. Kullanıcı yönetimi, içerik onayı, sistem yapılandırması gibi yönetimsel görevleri yerine getirir. |
| **Client** (Danışan) | NexFit platformunu fitness hedeflerine ulaşmak için kullanan son kullanıcı. Antrenman ve beslenme programları alır, gelişimini takip eder. |
| **Coach** (Antrenör) | Danışanlara özel antrenman ve beslenme programları oluşturan ve onları takip eden fitness profesyoneli. |
| **Gym Owner** (Spor Salonu Sahibi) | Bir veya birden fazla fiziksel spor salonunu platform üzerinden yöneten işletme sahibi. |
| **Studio Coach** (Stüdyo Antrenörü) | Belirli bir spor salonunda çalışan, grup dersleri veya birebir antrenmanlar sunan antrenör. |
| **Guest** (Misafir) | Henüz kayıt olmamış, platformu sınırlı olarak görüntüleyen kullanıcı. |

## İş Terimleri

| Terim | Açıklama |
|-------|----------|
| **Training Program** (Antrenman Programı) | Belirli bir süre için (genellikle 4-12 hafta) danışana özel hazırlanmış egzersiz planı. |
| **Nutrition Plan** (Beslenme Planı) | Danışanın hedeflerine göre hazırlanmış, günlük veya haftalık kalori ve makrobesin dağılımlarını içeren beslenme rehberi. |
| **Workout** (Antrenman) | Belirli bir gün için planlanmış egzersiz seti. Genellikle ısınma, ana bölüm ve soğuma kısımlarını içerir. |
| **Exercise** (Egzersiz) | Spesifik bir fiziksel hareket (örn. squat, bench press, plank). |
| **Set** (Set) | Bir egzersizin arka arkaya tekrarlanması (örn. 3 set bench press). |
| **Rep** (Tekrar) | Bir egzersizin tek bir hareket döngüsü (örn. 10 tekrar squat). |
| **Meal** (Öğün) | Günlük beslenme planındaki tek bir yemek veya atıştırmalık. |
| **Macros** (Makrolar) | Makrobesinler - protein, karbonhidrat ve yağlar için kullanılan kısa terim. |
| **Progress Tracking** (İlerleme Takibi) | Danışanın zaman içindeki fiziksel ölçümleri, performans artışı ve hedef ilerleme durumunu izleme süreci. |
| **Assessment** (Değerlendirme) | Danışanın mevcut fitness düzeyini, kısıtlamalarını ve hedeflerini belirlemek için yapılan başlangıç analizi. |
| **Subscription** (Abonelik) | Kullanıcının belirli hizmetlere erişim için ödediği periyodik ücret modeli. |
| **Session** (Seans) | Antrenör ve danışan arasında gerçekleşen birebir veya grup antrenman oturumu. |
| **Class** (Ders) | Spor salonlarında sunulan grup aktivitesi (örn. yoga, spinning, HIIT). |
| **Check-in** (Giriş) | Kullanıcının spor salonuna veya derse varışını kaydetme işlemi. |

## Teknik Terimler

| Terim | Açıklama |
|-------|----------|
| **API** (Application Programming Interface) | NexFit backend servisleri ile diğer uygulamalar veya frontend arasındaki veri alışverişini sağlayan arayüz. |
| **JWT** (JSON Web Token) | Kullanıcı kimlik doğrulaması için kullanılan token tabanlı güvenlik standardı. |
| **Microservice** (Mikroservis) | NexFit mimari yaklaşımında, her biri belirli bir işlevsellikten sorumlu olan bağımsız servisler. |
| **GraphQL** | API sorgularında kullanılan, istemcinin tam olarak ihtiyaç duyduğu verileri belirtmesine olanak tanıyan sorgu dili. |
| **WebSocket** | Gerçek zamanlı veri iletişimi için kullanılan protokol (örn. antrenör-danışan mesajlaşması için). |
| **React Native** | NexFit mobil uygulamasının geliştirildiği cross-platform framework. |
| **Redux** | Uygulama durumunu (state) yönetmek için kullanılan kütüphane. |
| **MongoDB** | NexFit'in ana veri depolama sistemi olarak kullanılan NoSQL veritabanı. |
| **Redis** | Önbellek ve geçici veri depolama için kullanılan bellek içi veritabanı. |
| **CI/CD** (Continuous Integration/Continuous Deployment) | Kod değişikliklerinin otomatik olarak test edilmesi ve dağıtılması için kullanılan süreç. |
| **Docker** | Uygulamaların konteyner olarak paketlenmesi ve dağıtılması için kullanılan platform. |
| **Kubernetes** | Konteynerize edilmiş uygulamaların otomatik dağıtımı, ölçeklendirilmesi ve yönetimi için kullanılan sistem. |
| **Webhook** | Harici sistemlerin belirli olaylar gerçekleştiğinde NexFit'e bildirim göndermesini sağlayan HTTP callback mekanizması. |
| **OAuth** | Üçüncü taraf uygulamaların kullanıcı verilerine güvenli erişimi için kullanılan açık standart yetkilendirme protokolü. |

## Metrikler

| Terim | Açıklama |
|-------|----------|
| **BMI** (Body Mass Index) | Vücut Kitle İndeksi - kilo ve boy kullanılarak hesaplanan, vücut kompozisyonunun kabaca değerlendirilmesini sağlayan ölçü. |
| **BMR** (Basal Metabolic Rate) | Bazal Metabolik Hız - vücudun dinlenme halindeyken temel yaşamsal fonksiyonlarını sürdürmek için harcadığı kalori miktarı. |
| **TDEE** (Total Daily Energy Expenditure) | Günlük Toplam Enerji Harcaması - bir kişinin günlük aktivitelerini tamamlamak için harcadığı toplam kalori miktarı. |
| **1RM** (One Rep Maximum) | Bir Tekrar Maksimumu - bir kişinin bir egzersizi tek seferde kaldırabileceği maksimum ağırlık. |
| **RPE** (Rate of Perceived Exertion) | Algılanan Zorluk Derecesi - bir egzersizin ne kadar zorlu olduğuna dair subjektif değerlendirme ölçeği (genellikle 1-10). |
| **MET** (Metabolic Equivalent of Task) | Metabolik Eşdeğer - farklı aktivitelerin enerji harcama oranını karşılaştırmak için kullanılan birim. |
| **Body Fat Percentage** (Vücut Yağ Oranı) | Vücuttaki toplam yağ kütlesinin vücut ağırlığına oranı. |
| **Lean Body Mass** (Yağsız Vücut Kütlesi) | Vücuttaki yağ dışındaki tüm dokuların (kas, kemik, organlar vb.) toplam ağırlığı. |
| **Volume** (Hacim) | Bir antrenmanda kaldırılan toplam ağırlık (set x tekrar x ağırlık). |
| **Macronutrient Ratio** (Makrobesin Oranı) | Günlük diyette protein, karbonhidrat ve yağ alımının oranı. |

## Statüler ve Durumlar

| Terim | Açıklama |
|-------|----------|
| **Active** (Aktif) | Şu anda aktif olan kullanıcı, abonelik veya program durumu. |
| **Inactive** (Pasif) | Şu anda aktif olmayan, geçici olarak durdurulmuş kullanıcı veya öğe. |
| **Pending** (Beklemede) | Onay veya aktivasyon bekleyen işlem veya durum. |
| **Completed** (Tamamlandı) | Bir hedef, görev veya programın başarıyla sonuçlandırılması. |
| **In Progress** (Devam Ediyor) | Bir hedef, görev veya programın aktif olarak çalışılmakta olduğu durum. |
| **Verified** (Doğrulanmış) | Kimliği veya nitelikleri onaylanmış antrenör veya işletme. |
| **Featured** (Öne Çıkan) | Ana sayfada veya arama sonuçlarında öncelikli olarak gösterilen içerik. |
| **Premium** (Premium) | Ek ücretli özelliklere sahip plan veya içerik. |
| **Trial** (Deneme) | Ücretsiz deneme sürecindeki kullanıcı veya abonelik. |
| **Expired** (Süresi Dolmuş) | Geçerlilik süresi sona ermiş abonelik veya program. |
| **Rejected** (Reddedilmiş) | Onay sürecinde kabul edilmeyen içerik veya istek. |

## Kısaltmalar

| Kısaltma | Açılımı | Açıklama |
|----------|---------|----------|
| **HIIT** | High-Intensity Interval Training | Yüksek Yoğunluklu Interval Antrenman - kısa, yoğun aktivite patlamaları ve dinlenme periyotlarını içeren egzersiz yöntemi. |
| **LISS** | Low-Intensity Steady State | Düşük Yoğunluklu Sabit Durum - uzun süreli, düşük şiddetli kardiyovasküler egzersiz. |
| **PR** | Personal Record | Kişisel Rekor - bir kişinin belirli bir egzersizde elde ettiği en iyi sonuç. |
| **PT** | Personal Trainer | Kişisel Antrenör - birebir fitness eğitimi veren profesyonel. |
| **DOMS** | Delayed Onset Muscle Soreness | Gecikmiş Kas Ağrısı - antrenman sonrası genellikle 24-72 saat içinde hissedilen kas ağrısı. |
| **IIFYM** | If It Fits Your Macros | "Makrolarına Uyuyorsa" - belirli bir kalori ve makrobesin hedefine bağlı kalarak esnek beslenme yaklaşımı. |
| **KPI** | Key Performance Indicator | Anahtar Performans Göstergesi - işletme veya antrenörün başarısını ölçmek için kullanılan metrikler. |
| **MAV** | Minimum Adaptive Volume | Kas gelişimi için gereken minimum antrenman hacmi. |
| **MRV** | Maximum Recoverable Volume | Toparlanılabilir maksimum antrenman hacmi. |
| **UI** | User Interface | Kullanıcı Arayüzü - kullanıcının uygulama ile etkileşime girdiği görsel bileşenler. |
| **UX** | User Experience | Kullanıcı Deneyimi - kullanıcının uygulama ile etkileşiminin genel kalitesi. |
| **API** | Application Programming Interface | Uygulama Programlama Arayüzü - farklı yazılım bileşenlerinin birbiriyle iletişim kurmasını sağlayan arayüz. |
| **SaaS** | Software as a Service | Hizmet Olarak Yazılım - internet üzerinden erişilen ve abonelik modeliyle sunulan yazılım. |
| **CRM** | Customer Relationship Management | Müşteri İlişkileri Yönetimi - antrenörlerin danışanlarıyla ilişkilerini yönettiği sistem. |
| **ROI** | Return on Investment | Yatırım Getirisi - fitness işletmeleri veya antrenörlerin pazarlama ve diğer yatırımlarının getirisi. |

---

Bu sözlük, NexFit projesinde kullanılan terminolojiyi standardize etmek ve tüm ekip üyelerinin aynı dili konuşmasını sağlamak için oluşturulmuştur. Eklenecek veya güncellenecek terimler için lütfen geliştirme ekibi ile iletişime geçin.

Son Güncelleme: 14 Mart 2023 