# NexFit Katkı Kılavuzu

NexFit projesine katkıda bulunmak istediğiniz için teşekkür ederiz! Bu belge, katkıda bulunma sürecini anlamanıza ve projeye nasıl en iyi şekilde dahil olabileceğinizi bulmanıza yardımcı olacaktır.

## İçindekiler

1. [Davranış Kuralları](#davranış-kuralları)
2. [Başlamadan Önce](#başlamadan-önce)
3. [Katkı Sağlama Yolları](#katkı-sağlama-yolları)
4. [Geliştirme Ortamı Kurulumu](#geliştirme-ortamı-kurulumu)
5. [Kodlama Standartları](#kodlama-standartları)
6. [Commit Mesajları](#commit-mesajları)
7. [Pull Request Süreci](#pull-request-süreci)
8. [Kod İnceleme Süreci](#kod-inceleme-süreci)
9. [Sürüm Yönetimi](#sürüm-yönetimi)
10. [Belgeleme](#belgeleme)
11. [İletişim](#iletişim)

## Davranış Kuralları

NexFit topluluğu olarak, açık ve kapsayıcı bir ortam yaratmayı taahhüt ediyoruz. Projeye katkıda bulunan herkes aşağıdaki kurallara uymayı kabul etmiş sayılır:

- Saygılı ve profesyonel davranış sergilemek
- Yapıcı geri bildirim vermek ve almak
- Topluluk üyelerinin farklılıklarına saygı göstermek
- Tehditkar, taciz edici veya ayrımcı davranışlardan kaçınmak

## Başlamadan Önce

Katkıda bulunmadan önce:

1. [GitHub Issues](https://github.com/canakyuz/nexfit-business-management/issues) bölümünü inceleyin ve ilgilendiğiniz konu hakkında açık bir issue olup olmadığını kontrol edin
2. Eğer yoksa, yeni bir issue açarak fikirlerinizi veya bulduğunuz hataları paylaşın
3. Büyük değişiklikler yapmadan önce, tartışma başlatarak topluluk geri bildirimi alın

## Katkı Sağlama Yolları

NexFit'e birçok şekilde katkıda bulunabilirsiniz:

### 1. Hata Raporlama
- Hatayı yeniden oluşturmak için adımları detaylı olarak açıklayın
- Beklenen ve gerçekleşen davranışı belirtin
- Mümkünse ekran görüntüleri ekleyin
- Hata oluştuğundaki ortam bilgilerini (işletim sistemi, tarayıcı, vb.) paylaşın

### 2. Özellik İstekleri
- Özelliğin detaylı açıklamasını yapın
- Bu özelliğin neden faydalı olacağını açıklayın
- Mümkünse, benzer özelliklerin örnek uygulamalarını gösterin
- Özelliğin nasıl uygulanabileceğine dair önerilerinizi paylaşın

### 3. Dokümantasyon Geliştirmeleri
- Mevcut belgelerdeki hataları veya eksiklikleri düzeltin
- Yeni belgeleme içeriği ekleyin
- Örnekleri ve kullanım senaryolarını geliştirin
- Belgelerin farklı dillere çevrilmesine yardımcı olun

### 4. Kod Katkıları
- Hata düzeltmeleri
- Performans iyileştirmeleri
- Yeni özellik uygulamaları
- Test kapsamını artırma

## Geliştirme Ortamı Kurulumu

1. Projeyi fork edin ve lokal makinenize klonlayın:
   ```bash
   git clone https://github.com/[kullanıcı-adınız]/nexfit-business-management.git
   cd nexfit-business-management
   ```

2. Bağımlılıkları yükleyin:
   ```bash
   npm install
   # veya
   yarn install
   ```

3. Geliştirme sunucusunu başlatın:
   ```bash
   npm run dev
   # veya
   yarn dev
   ```

4. Yapacağınız değişiklik için yeni bir dal oluşturun:
   ```bash
   git checkout -b [dal-adı]
   ```

## Kodlama Standartları

NexFit projesinde tutarlı bir kod stili sağlamak için aşağıdaki standartları takip ediyoruz:

### JavaScript/TypeScript
- [ESLint](https://eslint.org/) kurallarına uyun
- [Prettier](https://prettier.io/) formatını kullanın
- TypeScript için kesin tip tanımlamaları yapın
- Her fonksiyona JSDoc yorumları ekleyin

### React/React Native
- Fonksiyonel bileşenler ve hooks kullanın
- Prop-types veya TypeScript ile props doğrulaması yapın
- Componentleri mantıklı bir şekilde yapılandırın ve gerektiğinde bölün
- React performans optimizasyonlarını uygulayın

### CSS/SCSS
- Tailwind CSS sınıflarını öncelikli olarak kullanın
- Özel CSS gerektiğinde, BEM metodolojisini takip edin
- Renk ve diğer tasarım değişkenleri için theme dosyasını kullanın

### Test
- Her özellik için birim testleri yazın
- Entegrasyon testlerini kullanın
- %80 veya daha yüksek test kapsamı hedefleyin

## Commit Mesajları

İyi bir commit mesajı, değişikliğin ne olduğunu ve neden yapıldığını açıkça belirtmelidir. Conventional Commits standartlarını takip ediyoruz:

```
<tip>(kapsam): <açıklama>

[isteğe bağlı gövde]

[isteğe bağlı alt bilgi]
```

Örneğin:
- `feat(auth): kullanıcı oturum açma işlevi eklendi`
- `fix(dashboard): doğru olmayan metrik hesaplaması düzeltildi`
- `docs(api): API belgeleri güncellendi`
- `style(button): düğme hizalaması düzeltildi`
- `refactor(utils): yardımcı fonksiyonlar optimize edildi`
- `test(user): kullanıcı modeli için testler eklendi`
- `chore(deps): bağımlılıklar güncellendi`

## Pull Request Süreci

1. Yaptığınız değişiklikleri test edin
2. Değişikliklerinizi commit edin
3. Dalınızı GitHub'a push edin
4. GitHub'da bir Pull Request (PR) oluşturun:
   - PR başlığı için commit mesajı formatını kullanın
   - Değişikliklerinizi ayrıntılı olarak açıklayın
   - İlgili issue'ları bağlayın (örn. "Fixes #123")
   - Gerekli olabilecek özel talimatları ekleyin
5. PR'nizi incelenmesi için ekibe bildirin
6. Geri bildirimlere göre gerekli değişiklikleri yapın

## Kod İnceleme Süreci

Tüm katkılar, kalite ve tutarlılık sağlamak için kod incelemesinden geçer:

1. PR gönderildikten sonra, bir veya daha fazla ekip üyesi kodu inceleyecektir
2. İnceleyiciler şunlara dikkat edecektir:
   - Kod kalitesi ve stil uyumluluğu
   - Fonksiyonellik ve performans
   - Test kapsamı
   - Dokümantasyon
3. Geri bildirim ve değişiklik istekleri PR yorumları olarak verilecektir
4. Tüm geri bildirimler ele alındıktan sonra, PR onaylanacak ve birleştirilecektir

## Sürüm Yönetimi

NexFit, Semantic Versioning (SemVer) ilkelerini takip eder:

- **MAJOR** sürüm: geriye dönük uyumlu olmayan API değişiklikleri için
- **MINOR** sürüm: geriye dönük uyumlu yeni özellikler için
- **PATCH** sürüm: geriye dönük uyumlu hata düzeltmeleri için

## Belgeleme

İyi belgeleme, projemizin kullanılabilirliği için kritik öneme sahiptir:

1. Kod değişiklikleriniz için JSDoc yorumları ekleyin
2. Tüm yeni özellikleri README veya ilgili dokümanlarda açıklayın
3. Karmaşık özellikleri ayrıntılı açıklamalar ve örneklerle belgeleyin
4. API değişikliklerini API belgesinde güncelleyin

## İletişim

NexFit topluluğuyla iletişime geçmek için aşağıdaki kanalları kullanabilirsiniz:

- **GitHub Issues**: Hatalar, özellik istekleri ve tartışmalar için
- **Slack Kanalı**: Gerçek zamanlı iletişim için
- **Aylık Toplantı**: Aktif katkıda bulunanlar için aylık video konferans
- **E-posta**: [contributors@nexfit.com](mailto:contributors@nexfit.com)

---

Katkılarınız için tekrar teşekkür ederiz! Sorularınız veya endişeleriniz varsa, lütfen GitHub Issues üzerinden iletişime geçmekten çekinmeyin.

*Son Güncelleme: Mart 2025* 