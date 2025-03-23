# 👀 NexFit Kod İnceleme Kılavuzu

Bu dokümantasyon, NexFit geliştirme ekibi için kod inceleme süreçlerini ve en iyi uygulamaları tanımlar. Kod incelemeleri, kod kalitesini artırmak, hataları erken tespit etmek ve bilgi paylaşımını teşvik etmek için kritik bir süreçtir.

## İçindekiler

- [Kod İnceleme Prensiplerimiz](#kod-i̇nceleme-prensiplerimiz)
- [Kod İnceleme Süreci](#kod-i̇nceleme-süreci)
- [İnceleme Kontrol Listesi](#i̇nceleme-kontrol-listesi)
- [İnceleme Yorumları İçin En İyi Uygulamalar](#i̇nceleme-yorumları-i̇çin-en-i̇yi-uygulamalar)
- [Otomatik Kod İnceleme Araçları](#otomatik-kod-i̇nceleme-araçları)
- [Kod İnceleme Metrikleri](#kod-i̇nceleme-metrikleri)
- [Ekler](#ekler)

## Kod İnceleme Prensiplerimiz

1. **Kod, insanlar tarafından okunmak içindir** - Makineler için değil, diğer geliştiriciler için kod yazıyoruz.
2. **Özenli olun** - Hem kod yazarken hem de incelerken profesyonel ve nazik olun.
3. **Erken ve sık geri bildirim** - Küçük PR'lar ve erken incelemeler teşvik edilir.
4. **Her bir satır, en az bir kişi tarafından incelenmelidir** - Hiçbir kod, inceleme olmadan production'a gidemez.
5. **Bilgi paylaşımı odaklı** - Kod incelemelerini öğrenme ve öğretme fırsatı olarak görün.

## Kod İnceleme Süreci

### 1. Pull Request Oluşturma

PR'nızı (Pull Request) oluştururken aşağıdaki adımları izleyin:

1. **PR Şablonunu Kullanın**: Tüm gerekli bilgileri sağlamak için proje şablonumuzu kullanın.
2. **Kapsamı Sınırlayın**: PR'lar idealde 300-500 satırdan az olmalıdır. Büyük değişiklikler için birden fazla PR düşünün.
3. **Açıklayıcı Başlık ve Açıklama**: Ne, neden ve nasıl hakkında net bilgi verin.
4. **Self-review**: PR'ınızı göndermeden önce kendiniz inceleyin.
5. **İlgili İnceleyicileri Atayın**: Konuyla ilgili deneyimi olan en az bir inceleyici atayın.

```markdown
## Pull Request Şablonu

### Açıklama
[Değişikliğin amacını ve ne değiştirdiğinizi açıklayın]

### Değişiklik Türü
- [ ] Hata düzeltmesi
- [ ] Yeni özellik
- [ ] Kod iyileştirmesi
- [ ] Dokümantasyon güncellemesi
- [ ] Diğer: [belirtin]

### İlgili Sorunlar
[İlgili sorun numaralarını bağlayın]

### Test Planı
[Değişikliklerinizi nasıl test ettiğinizi açıklayın]

### Ekran Görüntüleri
[Varsa, değişikliklerinizi gösteren ekran görüntüleri ekleyin]

### Ek Notlar
[Dikkat edilmesi gereken herhangi bir ek bilgi]
```

### 2. İnceleme Süreci

İnceleyiciler aşağıdaki adımları izlemelidir:

1. **Zamanında İnceleme**: PR'ları 24 saat içinde ilk incelemeye almayı hedefleyin.
2. **Adım Adım İnceleme**: Değişikliği bir bütün olarak anlamaya çalışın, ardından detaylara inin.
3. **Yorumlarınızı İşaretleyin**: Önem derecesine göre yorumlarınızı kategorize edin.
4. **Onaylamadan Önce Kontrol Edin**: İnceleme kontrol listemizi kullanın.
5. **Takip Sorunları Açın**: İnceleme kapsamı dışında kalan sorunları ayrı sorunlar olarak açın.

### 3. Revizyon ve Onay

1. **Hızlı Geri Bildirim Döngüsü**: Revizyonlar hızlı bir şekilde yapılmalı ve incelenmelidir.
2. **Çözümlenen Yorumları İşaretleyin**: Çözümlenen yorumları "Çözümlendi" olarak işaretleyin.
3. **Son Onay**: Tüm inceleyiciler onayladıktan sonra, PR "Onaylandı" olarak işaretlenir.

### 4. Birleştirme ve Temizleme

1. **Birleştirme Stratejisi**: Genellikle "squash and merge" tercih edilir, ancak durum bazında değişebilir.
2. **Birleştirme Sonrası**: İlgili dalı silin ve tamamlanan görevleri kapatın.

## İnceleme Kontrol Listesi

### Genel

- [ ] Kod, proje gereksinimlerini ve spesifikasyonları karşılıyor mu?
- [ ] Kodun mantığı net ve anlaşılır mı?
- [ ] İsimlendirme kurallarına uyulmuş mu (değişkenler, fonksiyonlar, sınıflar)?
- [ ] Kod tekrarı var mı? DRY (Don't Repeat Yourself) ilkesine uyuluyor mu?
- [ ] Kod, proje mimarisine ve tasarım ilkelerine uygun mu?

### Fonksiyonellik

- [ ] Kod beklendiği gibi çalışıyor mu?
- [ ] Edge case'ler ve hata durumları düşünülmüş mü?
- [ ] Kullanıcı deneyimi açısından mantıklı mı?
- [ ] Geriye dönük uyumluluk düşünülmüş mü?

### Kalite ve Bakım

- [ ] Testler eklendi mi ve başarılı mı?
- [ ] Kod belgelendirmesi yeterli mi?
- [ ] Loglama uygun şekilde eklenmiş mi?
- [ ] Performans sorunları var mı?
- [ ] Teknik borç oluşturuyor mu?

### Güvenlik

- [ ] Güvenlik riskleri var mı (input validation, XSS, CSRF, vb.)?
- [ ] Hassas bilgilerin yönetimi uygun mu?
- [ ] Erişim kontrolü ve yetkilendirme doğru uygulanmış mı?

### Dil/Çerçeve Spesifik Kontroller

#### JavaScript/TypeScript

- [ ] Modern ES özellikleri uygun şekilde kullanılmış mı?
- [ ] Tip kontrolleri tam mı (TypeScript için)?
- [ ] Asenkron işlemler doğru yönetiliyor mu?
- [ ] Bellekten tasarruf ve performans düşünülmüş mü?

#### React/React Native

- [ ] Bileşen yapısı ve yaşam döngüsü doğru kullanılıyor mu?
- [ ] Gereksiz render'lar önlenmiş mi?
- [ ] Prop drilling aşırıya kaçıyor mu, yoksa state yönetimi uygun mu?
- [ ] Performans optimizasyonları (useMemo, useCallback) gerekli yerlerde kullanılmış mı?

#### Node.js/Express

- [ ] API endpoint tasarımı RESTful prensiplere uygun mu?
- [ ] Hata yönetimi ve validasyon doğru uygulanmış mı?
- [ ] Middleware kullanımı optimize edilmiş mi?
- [ ] Veritabanı sorguları verimli mi?

## İnceleme Yorumları İçin En İyi Uygulamalar

### Yorum Tipleri

- **Blocker 🛑**: Bu sorun giderilmeden PR onaylanamaz
- **Suggestion 💡**: İyileştirme önerisi, kod yazarına bırakılmıştır
- **Question ❓**: Açıklık getirmek veya anlamak için soru
- **Nitpick 🔍**: Küçük, önemsiz sorunlar (stil, yazım, vb.)
- **Praise 👏**: Beğenilen, takdir edilen kod parçası

### Etkili Yorumlar Yazma

1. **Spesifik olun**: "Bu iyi değil" yerine neden iyi olmadığını açıklayın.
2. **Yapıcı olun**: Sorunlarla birlikte çözüm önerileri sunun.
3. **Kişisel değil, kod odaklı olun**: "Sen" değil "kod" veya "fonksiyon" üzerine yorum yapın.
4. **İncelikli bir dil kullanın**: "Bu berbat" yerine "Burada iyileştirme fırsatı görüyorum" deyin.
5. **Kodu övmeyi unutmayın**: İyi yazılmış kod için olumlu geri bildirim verin.

### Yorum Örnekleri

❌ Kötü: "Bu kod kötü."  
✅ İyi: "Bu fonksiyonun karmaşıklığı yüksek, şu şekilde refaktör edilerek okunabilirliği artırılabilir: [örnek]"

❌ Kötü: "Neden böyle yazdın?"  
✅ İyi: "Bu yaklaşımın seçilme nedeni nedir? Alternatif olarak X yaklaşımı daha az karmaşık olabilir."

❌ Kötü: "Tipleri eksik bırakmışsın."  
✅ İyi: "Bu fonksiyonda parametre tiplerinin eklenmesi, tip güvenliğini artıracak."

## Otomatik Kod İnceleme Araçları

PR incelemelerimizi desteklemek için aşağıdaki otomatik araçları kullanıyoruz:

### Lint ve Kod Stili

- **ESLint**: JavaScript/TypeScript kod kalitesi
- **Prettier**: Kod formatı
- **StyleLint**: CSS/SCSS stil kontrolü

### Kod Kalitesi ve Güvenlik

- **SonarQube**: Kod kalitesi, güvenlik ve teknik borç analizi
- **CodeClimate**: Kod kalitesi ve karmaşıklık analizi
- **Snyk/Dependabot**: Bağımlılık güvenlik taraması

### Test Kapsamı

- **Jest**: JavaScript/TypeScript test kapsamı
- **Codecov**: Test kapsamı raporlaması

### CI/CD Pipeline Entegrasyonu

Otomatik inceleme araçlarımız, GitHub Actions CI/CD pipeline'ımıza entegre edilmiştir:

```yaml
# .github/workflows/code-review.yml
name: Code Review
on: [pull_request]
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install dependencies
        run: npm ci
      - name: Run ESLint
        run: npm run lint
      - name: Run Prettier
        run: npm run format:check
  
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install dependencies
        run: npm ci
      - name: Run tests
        run: npm test
      - name: Upload coverage
        uses: codecov/codecov-action@v1
  
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run Snyk
        uses: snyk/actions/node@master
```

## Kod İnceleme Metrikleri

İnceleme sürecimizi sürekli iyileştirmek için aşağıdaki metrikleri izleriz:

### Süreç Metrikleri

- **İnceleme Süresi**: İlk incelemeye kadar geçen ortalama süre
- **PR Çözüm Süresi**: PR'ın açılmasından birleştirilmesine kadar geçen süre
- **PR Boyutu**: PR başına değiştirilen ortalama satır sayısı
- **İnceleme Yoğunluğu**: PR başına yorum sayısı

### Kalite Metrikleri

- **Hata Yakalama Oranı**: İncelemelerde tespit edilen hata sayısı
- **Yapıcı Geri Bildirim Oranı**: Yapıcı yorum yüzdesi
- **İnceleme Kapsamı**: İncelenen kod yüzdesi

## Ekler

### Dil ve Çerçeve Spesifik Kontrol Listeleri

Aşağıdaki bağlantılar, belirli diller ve çerçeveler için ayrıntılı kontrol listelerine yönlendirir:

- [JavaScript/TypeScript İnceleme Kontrol Listesi](./code-review/javascript.md)
- [React/React Native İnceleme Kontrol Listesi](./code-review/react.md)
- [Node.js/Express İnceleme Kontrol Listesi](./code-review/nodejs.md)
- [MongoDB/Mongoose İnceleme Kontrol Listesi](./code-review/mongodb.md)

### Öğrenme Kaynakları

- [Etkili Kod İncelemeleri Yapma](https://www.thoughtworks.com/insights/blog/effective-code-reviews-are-about-communication)
- [Kod İnceleme En İyi Uygulamaları](https://google.github.io/eng-practices/review/)
- [İnceleme Sırasında Ne Aranmalı](https://blog.codinghorror.com/code-reviews-just-do-it/)

---

Bu kod inceleme kılavuzu NexFit geliştirme ekibi için hazırlanmıştır. Her proje ve ekip üyesi, bu sürecin iyileştirilmesine katkıda bulunabilir.

Son Güncelleme: 15 Mart 2023 