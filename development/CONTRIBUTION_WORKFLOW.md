# 🔄 NexFit Katkı Sağlama İş Akışı

Bu dokümantasyon, NexFit projesine kod katkısında bulunmak isteyen geliştiriciler için adım adım iş akışını açıklar. Tutarlı ve verimli bir geliştirme süreci sağlamak için bu kurallara uyulması önemlidir.

## İçindekiler

- [Geliştirme Ortamı Kurulumu](#geliştirme-ortamı-kurulumu)
- [Geliştirme İş Akışı](#geliştirme-iş-akışı)
- [Branching Stratejisi](#branching-stratejisi)
- [Commit Mesajları](#commit-mesajları)
- [Pull Request Süreci](#pull-request-süreci)
- [Kod İnceleme Süreci](#kod-inceleme-süreci)
- [CI/CD Pipeline](#cicd-pipeline)
- [Sürüm Yönetimi](#sürüm-yönetimi)
- [Dokümantasyon Gereksinimleri](#dokümantasyon-gereksinimleri)
- [Sorun Takibi](#sorun-takibi)

## Geliştirme Ortamı Kurulumu

Projeye katkıda bulunmadan önce geliştirme ortamınızı kurmanız gerekir. Aşağıdaki adımları izleyin:

1. **Repository'i Fork Edin**

   GitHub'da NexFit repository'sini fork edin. Bu, sizin GitHub hesabınızda repository'nin bir kopyasını oluşturacaktır.

2. **Repository'i Klonlayın**

   Fork ettiğiniz repository'i lokal makinenize klonlayın:

   ```bash
   git clone https://github.com/[kullanıcı-adınız]/nexfit.git
   cd nexfit
   ```

3. **Upstream Remote Ekleyin**

   Ana repository ile senkronize kalabilmek için upstream remote ekleyin:

   ```bash
   git remote add upstream https://github.com/nexfit/nexfit.git
   ```

4. **Bağımlılıkları Yükleyin**

   Proje bağımlılıklarını yükleyin:

   ```bash
   npm install
   # veya
   yarn
   ```

5. **Ortam Değişkenlerini Ayarlayın**

   `.env.example` dosyasını kopyalayıp `.env` olarak yeniden adlandırın ve gerekli değişkenleri ayarlayın:

   ```bash
   cp .env.example .env
   ```

6. **Geliştirme Sunucusunu Başlatın**

   ```bash
   npm run dev
   # veya
   yarn dev
   ```

## Geliştirme İş Akışı

NexFit projesi, geliştirme için GitHub Flow iş akışını kullanır. Temel adımlar şunlardır:

1. **Ana Dalı Güncelleyin**

   Çalışmaya başlamadan önce, ana dalın güncel olduğundan emin olun:

   ```bash
   git checkout main
   git pull upstream main
   ```

2. **Özellik Dalı Oluşturun**

   Yeni bir özellik veya hata düzeltmesi için dal oluşturun:

   ```bash
   git checkout -b feature/[özellik-adı]
   # veya
   git checkout -b fix/[hata-adı]
   ```

3. **Değişikliklerinizi Yapın**

   Kodunuzu düzenleyin, test edin ve tamamlayın.

4. **Linting ve Testleri Çalıştırın**

   Değişikliklerinizi yapmadan önce linting ve testleri çalıştırın:

   ```bash
   npm run lint
   npm run test
   # veya
   yarn lint
   yarn test
   ```

5. **Değişikliklerinizi Commit Edin**

   Değişikliklerinizi [Conventional Commits](#commit-mesajları) formatına göre commit edin:

   ```bash
   git add .
   git commit -m "feat: add new feature"
   ```

6. **Değişikliklerinizi Pushlayın**

   Dalınızı uzak repository'ye pushlayın:

   ```bash
   git push origin feature/[özellik-adı]
   ```

7. **Pull Request Oluşturun**

   GitHub'da, fork ettiğiniz repository'den ana repository'ye pull request oluşturun.

## Branching Stratejisi

NexFit, geliştirme sürecini yönetmek için aşağıdaki dal stratejisini kullanır:

### Ana Dallar

- **main**: Üretim ortamına dağıtılan kodun bulunduğu dal. Her commit, bir sürüm etiketiyle işaretlenmelidir.
- **develop**: Geliştirme dalı, bir sonraki sürüm için planlanmış olan kodun bulunduğu dal.

### Geçici Dallar

- **feature/xxx**: Yeni özellikler için kullanılır. `develop` dalından oluşturulur ve tamamlandığında tekrar `develop` dalına merge edilir.
- **fix/xxx**: Hata düzeltmeleri için kullanılır. `develop` dalından oluşturulur ve tamamlandığında tekrar `develop` dalına merge edilir.
- **hotfix/xxx**: Üretim ortamındaki kritik hataları düzeltmek için kullanılır. `main` dalından oluşturulur ve hem `main` hem de `develop` dalına merge edilir.
- **release/x.y.z**: Sürüm hazırlığı için kullanılır. `develop` dalından oluşturulur ve hazır olduğunda hem `main` hem de `develop` dalına merge edilir.

### Dal İsimlendirme Kuralları

- Tüm dallar küçük harfle yazılmalıdır
- Kelimeler arasında tire `-` kullanılmalıdır
- Kısa ve açıklayıcı olmalıdır
- İlgili olduğu konu veya sorun numarası ile başlamalıdır

Örnekler:
- `feature/user-authentication`
- `fix/login-validation`
- `hotfix/security-vulnerability`
- `release/1.2.0`

## Commit Mesajları

NexFit, [Conventional Commits](https://www.conventionalcommits.org/) formatını takip eder. Bu format, commit mesajlarınızın tutarlı ve makine tarafından okunabilir olmasını sağlar.

### Format

```
<type>[scope]: <description>

[optional body]

[optional footer(s)]
```

### Commit Tipleri

- **feat**: Yeni bir özellik
- **fix**: Bir hata düzeltmesi
- **docs**: Sadece dokümantasyon değişiklikleri
- **style**: Kod anlamını etkilemeyen değişiklikler (boşluk, format, noktalama, vb.)
- **refactor**: Hata düzeltmesi veya özellik eklemeyen kod değişiklikleri
- **perf**: Performansı iyileştiren değişiklikler
- **test**: Test ekleme veya düzeltme
- **build**: Yapı sistemini veya harici bağımlılıkları etkileyen değişiklikler
- **ci**: CI konfigürasyon dosyalarında ve scriptlerinde değişiklikler
- **chore**: Diğer değişiklikler (yapı süreci veya yardımcı araçlar/kütüphaneler)

### Örnekler

```
feat(auth): implement JWT authentication
fix(ui): correct button alignment on mobile view
docs(api): update authentication documentation
style: format code according to new style guide
refactor(database): optimize user query performance
test(api): add tests for user endpoint
```

## Pull Request Süreci

Pull Request (PR) süreci, kodunuzun ana kod tabanına entegre edilmeden önce incelenmesini ve test edilmesini sağlar.

### PR Oluşturma

1. GitHub'da repository sayfanıza gidin
2. "Pull Requests" sekmesine tıklayın
3. "New Pull Request" düğmesine tıklayın
4. Base repository olarak ana repository'yi ve compare repository olarak kendi fork'unuzu seçin
5. Base branch olarak hedef dalı (genellikle `develop`) ve compare branch olarak kendi dalınızı seçin
6. "Create Pull Request" düğmesine tıklayın
7. PR şablonuna göre ayrıntıları doldurun

### PR Şablonu

PR açarken, aşağıdaki şablonu kullanın:

```markdown
## Açıklama
[Değişikliğin genel açıklaması]

## Değişiklik Türü
- [ ] Yeni özellik (geriye dönük uyumlu)
- [ ] Hata düzeltmesi (geriye dönük uyumlu)
- [ ] Kırıcı değişiklik (önceki sürümlerle uyumsuz)
- [ ] Dokümantasyon güncellemesi
- [ ] Kod stili güncellemesi
- [ ] Refactoring (işlevsellik değişikliği yok)
- [ ] Performans iyileştirmesi
- [ ] Test
- [ ] Diğer (açıklayın)

## İlgili Sorunlar
[İlgili GitHub sorunları, örn. "Closes #123, #456"]

## Ek Bilgi
[Ek bilgi veya ekran görüntüleri]

## Kontrol Listesi
- [ ] Kodun stil kurallarına uygun olduğundan emin oldum
- [ ] Tüm testler başarıyla geçiyor
- [ ] Gerekli dokümantasyonu ekledim veya güncelledim
- [ ] Commit mesajları [kurallara](../docs/development/CONTRIBUTION_WORKFLOW.md#commit-mesajları) uygun
```

### PR İnceleme Kriteri

PR'ınız, aşağıdaki kriterlere göre değerlendirilecektir:

- Kod, projenin stil kılavuzuna uygun mu?
- Gerekli testler eklenmiş mi?
- Mevcut testler başarıyla geçiyor mu?
- Değişiklik, istenen işlevselliği sağlıyor mu?
- Performans, güvenlik veya diğer sorunlar var mı?
- Dokümantasyon güncellenmiş mi?

## Kod İnceleme Süreci

Kod inceleme, kodun kalitesini artırmak ve hataları erkenden tespit etmek için önemli bir adımdır.

### İnceleme Atama

- PR oluşturulduktan sonra, en az bir (tercihen iki) inceleyici atanmalıdır
- Gözden geçirenler, ilgili alanda deneyimli ekip üyeleri olmalıdır
- Karmaşık değişiklikler için, birden fazla inceleyici gereklidir

### İnceleme Süreci

1. İnceleyiciler, değişiklikleri inceler ve yorum ekler
2. Katkıda bulunan kişi, geri bildirimlere yanıt verir ve gerekli düzeltmeleri yapar
3. İnceleyiciler değişiklikleri onaylar veya daha fazla değişiklik ister
4. Tüm inceleyiciler onayladıktan sonra, PR birleştirilebilir

### İnceleme İlkeleri

- Yapıcı geri bildirim sağlayın
- Sorunları ve önerileri açıkça belirtin
- Kod yerine, probleme odaklanın
- Küçük stil sorunları için otomatik araçlar kullanın (linters, formatters)
- Yalnızca "LGTM" (Looks Good To Me) yazmak yerine, detaylı geri bildirim verin

## CI/CD Pipeline

NexFit, sürekli entegrasyon ve sürekli dağıtım için GitHub Actions kullanır.

### CI Süreci

Pull request oluşturduğunuzda veya var olan bir PR'a yeni commitler eklediğinizde, aşağıdaki işlemler otomatik olarak çalışır:

1. **Kod Linting**: ESLint ve Prettier ile kod kontrolleri
2. **Unit Testler**: Jest ile birim testleri
3. **Integration Testler**: E2E ve entegrasyon testleri
4. **Build Check**: Uygulamanın başarıyla oluşturulup oluşturulmadığının kontrolü

### CD Süreci

`main` dalına yapılan her commit veya merge işlemi sonrası:

1. **Build**: Uygulama oluşturulur
2. **Test**: Son testler çalıştırılır
3. **Deploy**: Uygulama, üretim ortamına otomatik olarak dağıtılır

### İş Akışı Yapılandırması

CI/CD iş akışı, `.github/workflows/` dizinindeki YAML dosyalarında tanımlanmıştır:

- `ci.yml`: PR'lar için CI işlemleri
- `staging.yml`: `develop` dalına merge sonrası staging ortamına dağıtım
- `production.yml`: `main` dalına merge sonrası üretim ortamına dağıtım

## Sürüm Yönetimi

NexFit, [Semantic Versioning (SemVer)](https://semver.org/) prensiplerini takip eder.

### Sürüm Numaralandırma

```
MAJOR.MINOR.PATCH
```

- **MAJOR**: Geriye dönük uyumlu olmayan API değişiklikleri
- **MINOR**: Geriye dönük uyumlu yeni işlevsellik
- **PATCH**: Geriye dönük uyumlu hata düzeltmeleri

### Sürüm Oluşturma Süreci

1. `develop` dalından `release/x.y.z` dalı oluşturulur
2. Sürüm hazırlıkları yapılır (versiyon numarası güncelleme, son düzeltmeler)
3. Testler ve doğrulamalar yapılır
4. `release/x.y.z` dalı, `main` dalına merge edilir
5. `main` dalı, sürüm etiketi ile işaretlenir
6. `release/x.y.z` dalı, `develop` dalına merge edilir

### Changelog

Her sürüm için, `CHANGELOG.md` dosyası güncellenmelidir. Bu dosya, [Keep a Changelog](https://keepachangelog.com/) formatını takip eder.

## Dokümantasyon Gereksinimleri

NexFit, iyi belgelenmiş kod ve özellikler gerektirir. Katkıda bulunurken aşağıdaki dokümantasyon gereksinimlerini göz önünde bulundurun:

### Kod İçi Dokümantasyon

- Karmaşık fonksiyonlar, sınıflar ve modüller için JSDoc yorumları ekleyin
- API endpoint'leri için detaylı açıklamalar ekleyin
- Karmaşık algoritmaları açıklayın

### API Dokümantasyonu

Yeni API endpoint'leri eklerken veya mevcut olanları değiştirirken:

1. `docs/development/API_DOCUMENTATION.md` dosyasını güncelleyin
2. Endpoint açıklaması, parametreler, request/response örnekleri ekleyin

### Kullanıcı Dokümantasyonu

Kullanıcı arayüzünü etkileyen değişiklikler için:

1. `docs/user/` dizinindeki ilgili dosyaları güncelleyin
2. Gerekirse ekran görüntüleri ekleyin

## Sorun Takibi

NexFit, sorun takibi için GitHub Issues kullanır.

### Sorun Oluşturma

Sorun oluştururken, aşağıdaki şablonu kullanın:

```markdown
## Açıklama
[Sorunun açık ve net bir açıklaması]

## Beklenen Davranış
[Olması gerektiğini düşündüğünüz davranış]

## Mevcut Davranış
[Şu anda gördüğünüz davranış]

## Reprodüksiyon Adımları
1. [İlk adım]
2. [İkinci adım]
3. [ve diğerleri...]

## Ortam
- Tarayıcı: [örn. Chrome 90]
- İşletim Sistemi: [örn. Windows 10]
- Uygulama Sürümü: [örn. 1.2.3]

## Ek Bilgi
[Ekran görüntüleri, hata mesajları, vb.]
```

### Sorun Etiketleri

- **bug**: Bir hata veya beklenmeyen davranış
- **enhancement**: Yeni özellik veya mevcut bir özelliğin iyileştirilmesi
- **documentation**: Dokümantasyon değişiklikleri
- **question**: Daha fazla bilgi gerektiren bir soru
- **help-wanted**: Topluluktan yardım istenen konular
- **good-first-issue**: Yeni katkıda bulunacaklar için uygun sorunlar

### Sorun Atama

- Sorunlar, ilgili ekip üyelerine atanabilir
- Katkıda bulunmak isterseniz, bir sorunu kendinize atayabilirsiniz
- Atanmamış sorunlar, katkıda bulunmak isteyenler için açıktır

---

Bu iş akışı kılavuzu, NexFit projesine katkıda bulunmak için temel kuralları ve süreçleri açıklar. Sorularınız veya önerileriniz için, lütfen repository'de bir sorun oluşturun veya mevcut tartışmalara katılın.

Son Güncelleme: 12 Mart 2023 