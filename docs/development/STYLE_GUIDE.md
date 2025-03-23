# 🎨 NexFit Stil Kılavuzu

Bu stil kılavuzu, NexFit uygulaması geliştirme ekibi için kodlama standartlarını tanımlar. Bu standartlara uymak, kod tabanımızın tutarlılığını, okunabilirliğini ve bakımını kolaylaştırır.

## İçindekiler

- [Genel Prensipler](#genel-prensipler)
- [Dosya Organizasyonu](#dosya-organizasyonu)
- [TypeScript/JavaScript Stil Kuralları](#typescriptjavascript-stil-kuralları)
- [React/React Native Stil Kuralları](#reactreact-native-stil-kuralları)
- [CSS/SCSS Stil Kuralları](#cssscss-stil-kuralları)
- [Node.js/Express Stil Kuralları](#nodejsexpress-stil-kuralları)
- [Değişken ve Fonksiyon İsimlendirme](#değişken-ve-fonksiyon-i̇simlendirme)
- [Yorum Yazma](#yorum-yazma)
- [Formatlandırma ve Linting](#formatlandırma-ve-linting)
- [Git İşlemleri](#git-i̇şlemleri)

## Genel Prensipler

1. **DRY (Don't Repeat Yourself)**: Kod tekrarından kaçının, ortak işlemleri paylaşılan fonksiyonlara çıkarın.
2. **KISS (Keep It Simple, Stupid)**: Mümkün olduğunca basit ve anlaşılır kod yazın.
3. **YAGNI (You Aren't Gonna Need It)**: İhtiyaç olmadan karmaşık soyutlamalar oluşturmayın.
4. **Separation of Concerns**: Her modül veya bileşen, belirli bir sorumluluğa sahip olmalıdır.
5. **Consistency**: Kod tabanında tutarlı bir stil kullanın.

## Dosya Organizasyonu

### Proje Yapısı

```
nexfit-app/
├── public/                # Statik dosyalar
├── src/                   # Kaynak kodları
│   ├── api/               # API istekleri ve endpoint tanımları
│   ├── components/        # Paylaşılan UI bileşenleri
│   │   ├── common/        # Genel bileşenler
│   │   ├── forms/         # Form bileşenleri
│   │   └── layout/        # Layout bileşenleri
│   ├── contexts/          # React context tanımları
│   ├── hooks/             # Özel React hooks
│   ├── models/            # Veri modelleri/tipleri
│   ├── pages/             # Sayfa bileşenleri
│   ├── services/          # İş mantığı servisleri
│   ├── styles/            # Global stil tanımları
│   ├── utils/             # Yardımcı fonksiyonlar
│   ├── App.tsx            # Ana uygulama bileşeni
│   └── index.tsx          # Giriş noktası
├── tests/                 # Test dosyaları
├── .eslintrc.js           # ESLint yapılandırması
├── .prettierrc            # Prettier yapılandırması
├── tsconfig.json          # TypeScript yapılandırması
└── package.json           # Paket bağımlılıkları
```

### Dosya Adlandırma

- **React bileşenleri**: PascalCase (örn. `UserProfile.tsx`)
- **React Native bileşenleri**: PascalCase (örn. `UserProfile.tsx`)
- **Yardımcı fonksiyonlar**: camelCase (örn. `formatDate.ts`)
- **CSS/SCSS modülleri**: camelCase (örn. `userProfile.module.scss`)
- **Test dosyaları**: .test.tsx veya .spec.tsx eki (örn. `UserProfile.test.tsx`)

## TypeScript/JavaScript Stil Kuralları

### Genel

- Uygun yerlerde TypeScript kullanın ve tip tanımlarını sağlayın.
- `any` tipinden mümkün olduğunca kaçının.
- İsimlendirmede camelCase kullanın (değişkenler, fonksiyonlar).
- Sınıf/arayüz isimlendirmesinde PascalCase kullanın.
- Sabitler için UPPER_SNAKE_CASE kullanın.

### Değişkenler

```typescript
// ✅ İyi
const userData = getUserData();
let isLoading = true;

// ❌ Kötü
const user_data = getUserData();
let loading = true;
```

### Fonksiyonlar

```typescript
// ✅ İyi
function calculateTotalPrice(items: Item[]): number {
  return items.reduce((total, item) => total + item.price, 0);
}

// ❌ Kötü
function calc_total(i) {
  return i.reduce((t, i) => t + i.price, 0);
}
```

### İfadeler ve Operatörler

- Karşılaştırmalar için `===` ve `!==` kullanın.
- Kısa devre değerlendirmesini (`&&`, `||`) etkili bir şekilde kullanın.
- Template literal'ları tercih edin.

```typescript
// ✅ İyi
const greeting = `Merhaba, ${user.name}!`;
const canEdit = user && user.permissions && user.permissions.canEdit;

// ❌ Kötü
const greeting = "Merhaba, " + user.name + "!";
const canEdit = user != null && user.permissions != null && user.permissions.canEdit != null;
```

### Asenkron İşlemler

- Promise'lar için async/await kullanımını tercih edin.
- Promise zincirlerini düzgün şekilde ele alın.

```typescript
// ✅ İyi
async function fetchUserData(userId: string): Promise<UserData> {
  try {
    const response = await api.get(`/users/${userId}`);
    return response.data;
  } catch (error) {
    logger.error('Kullanıcı verileri alınamadı', error);
    throw new Error('Kullanıcı verileri alınamadı');
  }
}

// ❌ Kötü
function fetchUserData(userId) {
  return api.get('/users/' + userId)
    .then(response => {
      return response.data;
    })
    .catch(error => {
      console.error('Kullanıcı verileri alınamadı', error);
      throw new Error('Kullanıcı verileri alınamadı');
    });
}
```

## React/React Native Stil Kuralları

### Bileşen Tanımları

- Fonksiyonel bileşenleri ve React Hooks kullanın.
- Her dosyada bir bileşen tanımlayın.
- Prop tipi tanımlamalarını uygun şekilde yapın.

```tsx
// ✅ İyi
import React, { useState, useEffect } from 'react';

interface UserProfileProps {
  userId: string;
  showDetails?: boolean;
}

const UserProfile: React.FC<UserProfileProps> = ({ userId, showDetails = false }) => {
  const [userData, setUserData] = useState<UserData | null>(null);

  useEffect(() => {
    // ...
  }, [userId]);

  return (
    <div className="user-profile">
      {/* ... */}
    </div>
  );
};

export default UserProfile;
```

### JSX Yapısı

- JSX içindeki ifadeler için süslü parantez `{}` kullanın.
- Props için çift tırnak `""` kullanın.
- Self-closing tag'leri `/` ile kapatın.
- Uzun prop listelerini birden fazla satırda yazın.

```tsx
// ✅ İyi
<UserProfile
  userId="123"
  showDetails={true}
  onProfileUpdate={handleProfileUpdate}
/>

// ❌ Kötü
<UserProfile userId='123' showDetails={true} onProfileUpdate={handleProfileUpdate}>
</UserProfile>
```

### Durum Yönetimi

- Bileşen durumunu (state) mantıklı şekilde ayırın.
- Durum yükseltme (state lifting) ilkelerini uygulayın.
- Redux veya Context API'yi global durum için uygun şekilde kullanın.

```tsx
// ✅ İyi
const [isLoading, setIsLoading] = useState(false);
const [error, setError] = useState<Error | null>(null);
const [data, setData] = useState<Data | null>(null);

// ❌ Kötü
const [state, setState] = useState({
  isLoading: false,
  error: null,
  data: null
});
```

### Performans Optimizasyonu

- Gereksiz render'ları önlemek için `React.memo`, `useMemo` ve `useCallback` kullanın.
- Büyük listeleri render ederken `React.lazy` ve `Suspense` kullanmayı düşünün.

```tsx
// ✅ İyi
const handleSubmit = useCallback(() => {
  // ...
}, [/* bağımlılıklar */]);

const memoizedValue = useMemo(() => {
  return computeExpensiveValue(a, b);
}, [a, b]);
```

## CSS/SCSS Stil Kuralları

### Yapılandırma

- CSS Modules veya Styled Components kullanımını tercih edin.
- Global CSS'ten kaçının, modüler yaklaşımı benimseyin.
- CSS sınıf isimleri için kebab-case kullanın.

### Selector Kullanımı

```scss
// ✅ İyi
.user-profile {
  &__header {
    // ...
  }

  &__content {
    // ...
  }
}

// ❌ Kötü
.userProfile {
  .header {
    // ...
  }

  .content {
    // ...
  }
}
```

### Medya Sorguları

- Mobil öncelikli (mobile-first) yaklaşımı kullanın.
- Tutarlı breakpoint değişkenleri tanımlayın.

```scss
// _variables.scss
$breakpoints: (
  'sm': 576px,
  'md': 768px,
  'lg': 992px,
  'xl': 1200px
);

// ✅ İyi
@mixin respond-to($breakpoint) {
  $value: map-get($breakpoints, $breakpoint);
  
  @if $value {
    @media (min-width: $value) {
      @content;
    }
  }
}

.element {
  font-size: 14px;
  
  @include respond-to('md') {
    font-size: 16px;
  }
}
```

### Animasyonlar

- Animasyonlar için transform ve opacity özelliklerini tercih edin.
- Animasyon sürelerini değişkenler olarak tanımlayın.

```scss
// _variables.scss
$transition-fast: 150ms;
$transition-medium: 300ms;
$transition-slow: 500ms;

// ✅ İyi
.button {
  transition: transform $transition-fast ease-in-out, 
              opacity $transition-fast ease-in-out;
  
  &:hover {
    transform: scale(1.05);
  }
}
```

## Node.js/Express Stil Kuralları

### API Endpoint Yapısı

- RESTful prensiplerini uygulayın.
- Endpoint isimleri için kebab-case kullanın.
- Uygun HTTP method'larını kullanın (GET, POST, PUT, DELETE).

```javascript
// ✅ İyi
// GET /api/users
// GET /api/users/:id
// POST /api/users
// PUT /api/users/:id
// DELETE /api/users/:id

// ❌ Kötü
// GET /api/getUsers
// GET /api/getUserById/:id
// POST /api/createUser
// POST /api/updateUser/:id
// GET /api/deleteUser/:id
```

### Middleware Kullanımı

- Middleware fonksiyonlarını modüler olarak tanımlayın.
- Hata yönetimi için middleware kullanın.

```javascript
// ✅ İyi
const authenticateUser = (req, res, next) => {
  // ...
  if (!authorized) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
  next();
};

// Kullanım
router.get('/protected-resource', authenticateUser, (req, res) => {
  // ...
});
```

### Hata Yönetimi

- Tutarlı hata yapısı kullanın.
- Tüm Promise reddetmelerini ve istisnaları yakalayın.

```javascript
// ✅ İyi
app.use((err, req, res, next) => {
  const statusCode = err.statusCode || 500;
  res.status(statusCode).json({
    error: {
      message: err.message,
      code: err.code || 'INTERNAL_ERROR',
      stack: process.env.NODE_ENV === 'development' ? err.stack : undefined
    }
  });
});
```

## Değişken ve Fonksiyon İsimlendirme

### Değişken İsimlendirme

- Anlamlı ve açıklayıcı isimler kullanın.
- Kısaltmalardan kaçının (yaygın kısaltmalar hariç, örn. HTML, URL).
- Boolean değişkenler için `is`, `has`, `can` gibi önekler kullanın.

```typescript
// ✅ İyi
const isUserActive = true;
const hasPendingTransactions = false;
const userProfileData = { ... };

// ❌ Kötü
const active = true;
const pending = false;
const data = { ... };
```

### Fonksiyon İsimlendirme

- Eylem belirten isimler kullanın.
- İşlevi açık bir şekilde tanımlayan isimler seçin.

```typescript
// ✅ İyi
function fetchUserData() { ... }
function calculateTotal() { ... }
function validateEmail() { ... }

// ❌ Kötü
function userData() { ... }
function total() { ... }
function emailCheck() { ... }
```

### Özel İsimlendirme Kuralları

- **Bileşenler**: PascalCase, açıklayıcı isimler (UserProfileCard)
- **Hooks**: "use" ile başlayan camelCase (useAuth, useFetchData)
- **Context**: "Context" son eki ile PascalCase (UserContext, AuthContext)
- **Enum değerleri**: PascalCase (UserRole.Admin, PaymentStatus.Pending)

## Yorum Yazma

### Ne Zaman Yorum Eklemeli

- Karmaşık iş mantığını açıklamak için
- API kullanımını belgelemek için
- Neden belirli bir yaklaşımın seçildiğini açıklamak için
- TODO ve FIXME notları için

### Yorum Biçimlendirme

- Tek satır yorumlar için `//` kullanın.
- Çok satırlı yorumlar için `/* ... */` kullanın.
- JSDoc biçimini fonksiyon belgelemeleri için tercih edin.

```typescript
// ✅ İyi

/**
 * Kullanıcı tarafından girilen e-posta adresini doğrular.
 * RFC 5322 standartına uygun olarak kontrol yapar.
 * 
 * @param email - Doğrulanacak e-posta adresi
 * @returns E-postanın geçerli olup olmadığını belirten boolean değer
 */
function validateEmail(email: string): boolean {
  // RFC 5322 standardı ile uyumlu regex
  const regex = /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
  return regex.test(email);
}

// TODO: Daha kapsamlı e-posta doğrulama ekle
// FIXME: Türkçe karakterleri desteklemiyor
```

## Formatlandırma ve Linting

### Araçlar

- **Prettier**: Kod formatlaması için
- **ESLint**: Kod kalitesi ve stil kontrolü için
- **stylelint**: CSS/SCSS stil kontrolü için

### Prettier Yapılandırması

```json
// .prettierrc
{
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "jsxBracketSameLine": false,
  "arrowParens": "avoid"
}
```

### ESLint Yapılandırması

```javascript
// .eslintrc.js
module.exports = {
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'prettier'
  ],
  plugins: ['react', 'react-hooks', '@typescript-eslint'],
  rules: {
    'react-hooks/rules-of-hooks': 'error',
    'react-hooks/exhaustive-deps': 'warn',
    'react/prop-types': 'off',
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/explicit-module-boundary-types': 'off',
    // ... diğer kurallar
  }
};
```

### Pre-commit Hooks

- Formatlandırma ve linting işlemleri için Husky ve lint-staged kullanın.

```json
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{css,scss}": [
      "stylelint --fix",
      "prettier --write"
    ]
  }
}
```

## Git İşlemleri

### Commit Mesajları

- Anlamlı ve tutarlı commit mesajları yazın.
- [Conventional Commits](https://www.conventionalcommits.org/) formatını kullanmayı düşünün.

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

Tür örnekleri:
- `feat:` - Yeni bir özellik
- `fix:` - Hata düzeltmesi
- `docs:` - Sadece dokümantasyon değişiklikleri
- `style:` - Kod stilini etkileyen değişiklikler (boşluk, format, noktalı virgül eksikleri vb.)
- `refactor:` - Ne bir hata düzeltmesi ne de bir özellik eklemeyen kod değişikliği
- `perf:` - Performansı iyileştiren değişiklikler
- `test:` - Test ekleme veya düzeltme
- `chore:` - Yapı sürecinde veya yardımcı araçlarda değişiklikler

Örnek:
```
feat(auth): implement JWT authentication

- Add JWT token generation
- Add token validation middleware
- Integrate with user service

Closes #123
```

### Dallanma Stratejisi (Branching Strategy)

- Git Flow veya GitHub Flow kullanın.
- Ana dalları (main, develop) koruyun.
- Feature dallarını kısa ve odaklı tutun.

```
main              ●───●───●───●──────────●
                 /     \         \       /
develop    ●────●──────●──────────●─────●
          /            \          /
feature  ●──────●──────●        /
                       \       /
bugfix                  ●─────●
```

- **main**: Üretim ortamı
- **develop**: Geliştirme ortamı, bir sonraki sürüm
- **feature/xxx**: Yeni özellik geliştirmeleri
- **bugfix/xxx**: Hata düzeltmeleri
- **hotfix/xxx**: Üretim ortamında acil düzeltmeler

---

Bu stil kılavuzu, NexFit ekibi için hazırlanmıştır ve projemizin gelişimindeki tutarlılığı ve kaliteyi sağlamak için referans olarak kullanılmalıdır. Kılavuz, ekibimizin ihtiyaçları doğrultusunda zaman içinde gelişebilir.

Son Güncelleme: 10 Mart 2023 