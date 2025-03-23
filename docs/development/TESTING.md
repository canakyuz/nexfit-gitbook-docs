# 🧪 NexFit Test Rehberi

Bu rehber, NexFit uygulamasının test stratejisini ve süreçlerini açıklamaktadır. Test otomasyonu, test kapsamı, test ortamları ve kalite güvence süreçleri ile ilgili temel bilgileri içerir.

## İçindekiler

- [Test Stratejisi](#test-stratejisi)
- [Test Türleri](#test-türleri)
- [Test Ortamları](#test-ortamları)
- [Test Otomasyonu](#test-otomasyonu)
- [Test Veri Yönetimi](#test-veri-yönetimi)
- [Test Raporlama](#test-raporlama)
- [CI/CD Entegrasyonu](#cicd-entegrasyonu)
- [Manuel Test Rehberi](#manuel-test-rehberi)
- [Hata Raporlama Süreci](#hata-raporlama-süreci)
- [Performans Testi](#performans-testi)
- [Güvenlik Testi](#güvenlik-testi)
- [Erişilebilirlik Testi](#erişilebilirlik-testi)

## Test Stratejisi

NexFit test stratejisi, sürekli entegrasyon ve sürekli teslimat prensiplerine dayanmaktadır. Stratejimizin temel unsurları:

1. **Otomatik Test Önceliği**: Mümkün olduğunca çok test senaryosunun otomatikleştirilmesi
2. **Piramit Yaklaşımı**: Birim testleri > Entegrasyon testleri > E2E testleri oranı
3. **Shift-Left Testing**: Hataları geliştirme sürecinin erken aşamalarında tespit etmek
4. **Test Güdümlü Geliştirme (TDD)**: Kritik bileşenler için TDD yaklaşımının benimsenmesi
5. **Sürekli Geri Bildirim**: Her commit sonrası otomatik testlerin çalıştırılması ve sonuçların hızla paylaşılması

## Test Türleri

### Birim Testleri

Birim testleri, kodun en küçük test edilebilir parçalarının (örn. fonksiyonlar, metodlar) yalıtılmış şekilde test edilmesini kapsar. NexFit'te birim testleri için şu araçları kullanıyoruz:

- **Backend**: Jest
- **Frontend**: Jest ve React Testing Library
- **Mobile**: Jest ve React Native Testing Library

**Örnek Birim Testi**:

```javascript
// userService.test.js
describe('User Service', () => {
  test('should validate email correctly', () => {
    expect(userService.isValidEmail('test@example.com')).toBe(true);
    expect(userService.isValidEmail('invalid-email')).toBe(false);
  });
  
  test('should hash password securely', async () => {
    const password = 'securePassword123';
    const hash = await userService.hashPassword(password);
    expect(hash).not.toBe(password);
    expect(await userService.verifyPassword(password, hash)).toBe(true);
  });
});
```

### Entegrasyon Testleri

Entegrasyon testleri, birden fazla bileşenin veya servisin birlikte çalışmasını doğrular. Veritabanı, API entegrasyonları ve mikroservislerin birlikte çalışması bu testlerle doğrulanır.

Kullandığımız araçlar:
- **API Testleri**: Supertest
- **Veritabanı Entegrasyonu**: Jest + MongoDB Memory Server
- **Servis Entegrasyonu**: Docker Compose ile izole test ortamları

**Örnek Entegrasyon Testi**:

```javascript
// auth.integration.test.js
describe('Authentication API', () => {
  beforeAll(async () => {
    await setupTestDatabase();
  });
  
  afterAll(async () => {
    await cleanupTestDatabase();
  });
  
  test('should register a new user and return JWT token', async () => {
    const res = await request(app)
      .post('/api/auth/register')
      .send({
        email: 'newuser@example.com',
        password: 'Password123!',
        firstName: 'Test',
        lastName: 'User'
      });
    
    expect(res.status).toBe(201);
    expect(res.body.data).toHaveProperty('token');
    expect(res.body.data.user).toHaveProperty('email', 'newuser@example.com');
  });
  
  test('should not allow login with incorrect credentials', async () => {
    const res = await request(app)
      .post('/api/auth/login')
      .send({
        email: 'newuser@example.com',
        password: 'WrongPassword'
      });
    
    expect(res.status).toBe(401);
  });
});
```

### Uçtan Uca (E2E) Testler

E2E testleri, gerçek kullanıcı senaryolarını simüle ederek uygulamanın tamamını test eder. Kullanıcı etkileşimlerini, veri akışını ve sistem entegrasyonlarını kapsar.

Kullandığımız araçlar:
- **Web E2E**: Cypress
- **Mobile E2E**: Detox
- **API E2E**: Postman/Newman

**Örnek E2E Testi**:

```javascript
// authentication.spec.js (Cypress)
describe('User Authentication Flow', () => {
  it('should allow user to register, login and access dashboard', () => {
    const email = `test${Date.now()}@example.com`;
    const password = 'Test1234!';
    
    // Register
    cy.visit('/signup');
    cy.get('[data-testid=input-email]').type(email);
    cy.get('[data-testid=input-password]').type(password);
    cy.get('[data-testid=input-firstName]').type('Test');
    cy.get('[data-testid=input-lastName]').type('User');
    cy.get('[data-testid=signup-button]').click();
    cy.url().should('include', '/onboarding');
    
    // Complete onboarding
    cy.get('[data-testid=skip-onboarding]').click();
    
    // Logout
    cy.get('[data-testid=user-menu]').click();
    cy.get('[data-testid=logout-button]').click();
    
    // Login
    cy.visit('/login');
    cy.get('[data-testid=input-email]').type(email);
    cy.get('[data-testid=input-password]').type(password);
    cy.get('[data-testid=login-button]').click();
    cy.url().should('include', '/dashboard');
    
    // Verify dashboard access
    cy.get('[data-testid=welcome-message]').should('contain', 'Test');
  });
});
```

## Test Ortamları

NexFit'te aşağıdaki test ortamları kullanılmaktadır:

| Ortam | Amacı | URL | Veri Durumu | Erişim |
|-------|-------|-----|-------------|--------|
| Geliştirme | Geliştirici testleri | localhost | Geçici, sıfırlanabilir | Geliştirici |
| Test | QA, otomatik testler | test.nexfit.io | Test veritabanı | QA, Geliştiriciler |
| Staging | Sürüm öncesi testler | staging.nexfit.io | Prod benzeri, anonim | Tüm ekip, beta kullanıcıları |
| Production | Canlı ortam | nexfit.io | Gerçek veri | Son kullanıcılar |

### Ortam Konfigürasyonu

Her ortam için `.env` dosyaları şu şekilde düzenlenir:

```
# .env.test örneği
NODE_ENV=test
DB_URI=mongodb://localhost:27017/nexfit-test
API_URL=https://api.test.nexfit.io/v1
LOG_LEVEL=debug
DISABLE_NOTIFICATIONS=true
```

## Test Otomasyonu

### Birim ve Entegrasyon Testleri Çalıştırma

```bash
# Tüm testleri çalıştırma
npm test

# Watch modunda çalıştırma
npm run test:watch

# Belirli bir test dosyasını çalıştırma
npm test -- -t "Authentication API"

# Kapsam raporu oluşturma
npm run test:coverage
```

### E2E Testleri Çalıştırma

```bash
# Web E2E testlerini çalıştırma (Cypress)
npm run test:e2e

# Cypress UI'ı açma
npm run cypress:open

# Mobile E2E testlerini çalıştırma (Detox)
npm run test:e2e:mobile

# API E2E testlerini çalıştırma (Newman)
npm run test:e2e:api
```

## Test Veri Yönetimi

### Test Veritabanı

Test ortamı için ayrı bir veritabanı kullanılır ve her test çalıştırmasından önce temizlenir. Birim ve entegrasyon testleri için MongoDB Memory Server kullanılarak izole veritabanı ortamları oluşturulur.

### Seed Data

Test verileri, gerçekçi senaryoları simüle etmek için kullanılır. Seed data oluşturmak için:

```bash
# Test verilerini yükleme
npm run db:seed:test

# Belirli bir modül için test verilerini yükleme
npm run db:seed:test -- --module=users
```

Seed data şu dizinde bulunur: `tests/seed-data/`

### Test Fixtures

Tekrar kullanılabilir test verileri için fixtures kullanılır:

```javascript
// tests/fixtures/users.js
module.exports = {
  validUser: {
    email: 'test@example.com',
    password: 'Test1234!',
    firstName: 'Test',
    lastName: 'User'
  },
  adminUser: {
    email: 'admin@nexfit.com',
    password: 'Admin1234!',
    firstName: 'Admin',
    lastName: 'User',
    role: 'ADMIN'
  },
  // ...
};
```

## Test Raporlama

### Kapsam Raporları

Test kapsamı raporları aşağıdaki komutla oluşturulur:

```bash
npm run test:coverage
```

Hedef kapsam oranları:
- **Birim Testleri**: %85+
- **Entegrasyon Testleri**: %70+
- **E2E Testleri**: Kritik iş süreçlerinin %100'ü

### Test Raporları

CI/CD pipeline'ında çalışan testlerin raporları aşağıdaki formatlarda oluşturulur:
- JUnit XML (CI/CD entegrasyonu için)
- HTML (insan tarafından okunabilir raporlar)
- JSON (veri analizi için)

Raporlara şu adresten erişilebilir: `https://reports.nexfit.io/tests/`

## CI/CD Entegrasyonu

NexFit, GitHub Actions kullanan bir CI/CD pipeline'ına sahiptir:

1. **Kod Analizi**: ESLint, Prettier, TypeScript kontrolü
2. **Birim Testleri**: Her PR ve commit için çalıştırılır
3. **Entegrasyon Testleri**: PR'lar main/master dalına merge edilmeden önce çalıştırılır
4. **E2E Testleri**: Staging ortamına deployment öncesi çalıştırılır
5. **Önizleme Ortamı**: Her PR için otomatik önizleme ortamı oluşturulur

Örnek CI/CD workflow:

```yaml
# .github/workflows/test.yml
name: Test Suite

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v3
        with:
          node-version: '16'
      - run: npm ci
      - run: npm run lint
      
  unit-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v3
        with:
          node-version: '16'
      - run: npm ci
      - run: npm test
      - name: Upload coverage
        uses: codecov/codecov-action@v2
        
  integration-test:
    runs-on: ubuntu-latest
    services:
      mongodb:
        image: mongo:4.4
        ports:
          - 27017:27017
      redis:
        image: redis:6
        ports:
          - 6379:6379
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v3
        with:
          node-version: '16'
      - run: npm ci
      - run: npm run test:integration
      
  e2e-test:
    needs: [unit-test, integration-test]
    if: github.event_name == 'pull_request' && github.base_ref == 'main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v3
        with:
          node-version: '16'
      - name: Install dependencies
        run: npm ci
      - name: Build application
        run: npm run build
      - name: Run E2E tests
        run: npm run test:e2e
```

## Manuel Test Rehberi

Otomatize edilemeyen veya yeni geliştirilen özellikler için manuel test süreçleri uygulanır.

### Test Planı Şablonu

```markdown
# Manuel Test Planı: [Özellik Adı]

## Test Amacı
[Özelliğin test edilmesindeki amaçları açıklayın]

## Test Ortamı
- Ortam: [Test/Staging]
- Tarayıcılar: [Chrome, Firefox, Safari, vb.]
- Mobil Cihazlar: [iOS/Android sürümleri]

## Önkoşullar
- [Gerekli kullanıcı hesapları]
- [Gerekli veriler]
- [Diğer gereksinimler]

## Test Senaryoları

### Senaryo 1: [Senaryo Adı]
1. [Adım 1]
2. [Adım 2]
3. [Adım 3]
**Beklenen Sonuç**: [Beklenen davranışı açıklayın]

### Senaryo 2: [Senaryo Adı]
...

## Keşif Testi Alanları
- [Özel durumlar]
- [Sınır değer testleri]
- [Performans gözlemleri]

## Test Sonuçları
- [ ] Senaryo 1: Başarılı/Başarısız - [Notlar]
- [ ] Senaryo 2: Başarılı/Başarısız - [Notlar]
```

### Smoke Test Kontrol Listesi

Her sürümden önce aşağıdaki kritik işlevlerin çalıştığı doğrulanır:

- [ ] Kullanıcı kaydı ve girişi
- [ ] Antrenman programları görüntüleme ve oluşturma
- [ ] Antrenman günlüğü kaydetme
- [ ] Beslenme günlüğü kaydetme
- [ ] İşletme yönetimi temel işlevleri
- [ ] Bildirim sistemi
- [ ] Mobil ve web senkronizasyonu

## Hata Raporlama Süreci

Hatalar, GitHub Issues üzerinden aşağıdaki şablona göre raporlanır:

```markdown
## Hata Açıklaması
[Hatanın kısa açıklaması]

## Yeniden Üretme Adımları
1. [Adım 1]
2. [Adım 2]
3. [Adım 3]

## Beklenen Davranış
[Doğru çalışma durumunda ne olması gerektiği]

## Gerçekleşen Davranış
[Hatanın nasıl ortaya çıktığı]

## Ekran Görüntüleri
[Varsa ekran görüntüleri]

## Ortam
- Cihaz: [Masaüstü/Mobil]
- OS: [Windows/macOS/iOS/Android + sürüm]
- Tarayıcı: [Chrome/Firefox/Safari + sürüm]
- Uygulama Sürümü: [v1.2.3]

## Ek Bilgiler
[Varsa ek bilgiler, loglar, vb.]
```

### Hata Önceliklendirme

Hatalar şu şekilde önceliklendirilir:

- **P0 (Kritik)**: Uygulamanın temel işlevselliğini engelleyen, veri kaybına neden olan veya güvenlik açığı içeren hatalar
- **P1 (Yüksek)**: Önemli işlevleri engelleyen ancak geçici çözümlerle aşılabilen hatalar
- **P2 (Orta)**: İşlevselliği kısıtlayan ancak kritik olmayan hatalar
- **P3 (Düşük)**: Küçük kullanıcı deneyimi sorunları, kozmetik hatalar

## Performans Testi

Performans testleri şu araçlarla gerçekleştirilir:
- **Load Testing**: k6
- **Backend Profiling**: Node.js Performance Hooks, Clinic.js
- **Frontend Performans**: Lighthouse, WebPageTest

### Performans Test Senaryoları

1. **Kullanıcı Ölçeklendirme Testi**: Eşzamanlı 1000 kullanıcıya kadar ölçeklendirme testi
2. **Veritabanı Performans Testi**: Yüksek hacimli veri işleme senaryoları
3. **API Dayanıklılık Testi**: Yüksek RPS (saniyedeki istek) altında API dayanıklılığı

Örnek k6 performans testi:

```javascript
// loadtest.js
import { sleep, check } from 'k6';
import http from 'k6/http';

export const options = {
  stages: [
    { duration: '1m', target: 100 }, // Ramp-up
    { duration: '3m', target: 100 }, // Steady state
    { duration: '1m', target: 0 },   // Ramp-down
  ],
  thresholds: {
    http_req_duration: ['p(95)<500'], // 95% of requests should be below 500ms
    'http_req_duration{staticAsset:yes}': ['p(95)<100'], // Static assets below 100ms
  },
};

export default function() {
  const res = http.get('https://api.test.nexfit.io/v1/health');
  check(res, { 'status is 200': (r) => r.status === 200 });
  
  const loginRes = http.post('https://api.test.nexfit.io/v1/auth/login', {
    email: 'test@example.com',
    password: 'Test1234!'
  });
  check(loginRes, { 'login successful': (r) => r.status === 200 });
  
  sleep(1);
}
```

## Güvenlik Testi

Güvenlik testleri düzenli olarak yapılır ve aşağıdaki alanları kapsar:

1. **OWASP Top 10 Denetimi**: OWASP ZAP ile otomatik tarama
2. **Dependency Scanning**: npm audit, Snyk ile bağımlılık güvenlik kontrolü
3. **Statik Kod Analizi**: SonarQube ile güvenlik odaklı kod analizi
4. **Penetrasyon Testi**: Yılda iki kez profesyonel pentest

### Güvenlik Testi CI/CD Entegrasyonu

```yaml
# .github/workflows/security.yml
name: Security Scans

on:
  schedule:
    - cron: '0 0 * * 0'  # Her pazar günü
  push:
    branches: [ main ]

jobs:
  dependency-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run npm audit
        run: npm audit --audit-level=high
      - name: Snyk scan
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}

  zap-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: ZAP Scan
        uses: zaproxy/action-baseline@v0.7.0
        with:
          target: 'https://staging.nexfit.io'
```

## Erişilebilirlik Testi

Tüm kullanıcı arayüzleri WCAG 2.1 AA standartlarına uygunluk için test edilir.

### Erişilebilirlik Test Araçları

- **Otomatik Testler**: Axe, Lighthouse Accessibility Audit
- **Manuel Testler**: Ekran okuyucu uyumluluğu (NVDA, VoiceOver)

### Erişilebilirlik Kontrol Listesi

- [ ] Yeterli renk kontrastı
- [ ] Klavye ile tam erişilebilirlik
- [ ] Uygun ARIA etiketleri
- [ ] Ekran okuyucu uyumluluğu
- [ ] Sıralı navigasyon yapısı
- [ ] Duyarlı tasarım
- [ ] Form elemanları için erişilebilir etiketler

---

Bu test rehberi, NexFit uygulamasının kalite standartlarını korumak için tüm ekip üyeleri tarafından referans olarak kullanılmalıdır. Rehber, projenin gelişimiyle birlikte sürekli güncellenecektir.

Son Güncelleme: 18 Mart 2023 