# ⚙️ NexFit Konfigürasyon Rehberi

Bu rehber, NexFit uygulamasının farklı ortamlar için yapılandırılmasına ilişkin detaylı bilgiler içermektedir. Ortam değişkenleri, konfigürasyon dosyaları ve sistem ayarlarının nasıl yönetileceği bu dokümanda açıklanmaktadır.

## İçindekiler

- [Konfigürasyon Prensipleri](#konfigürasyon-prensipleri)
- [Ortam Değişkenleri](#ortam-değişkenleri)
- [Konfigürasyon Dosyaları](#konfigürasyon-dosyaları)
- [Ortam Bazlı Konfigürasyon](#ortam-bazlı-konfigürasyon)
- [Harici Servis Konfigürasyonları](#harici-servis-konfigürasyonları)
- [Güvenlik Konfigürasyonu](#güvenlik-konfigürasyonu)
- [Performans Ayarları](#performans-ayarları)
- [Logging ve Monitoring](#logging-ve-monitoring)
- [Sık Karşılaşılan Konfigürasyon Sorunları](#sık-karşılaşılan-konfigürasyon-sorunları)

## Konfigürasyon Prensipleri

NexFit konfigürasyon yönetimi aşağıdaki prensiplere dayanmaktadır:

1. **Koddan Ayrı Konfigürasyon**: Konfigürasyon ayarları koddan ayrı tutulur ve dağıtım zamanında ortama özgü olarak uygulanır.
2. **Ortam Değişkenleri**: Hassas bilgiler ve ortam-spesifik değerler ortam değişkenleri yoluyla sağlanır.
3. **Varsayılan Değerler**: Tüm konfigürasyon seçenekleri için mantıklı varsayılan değerler tanımlanır.
4. **Doğrulama**: Konfigürasyon değerleri uygulama başlangıcında doğrulanır.
5. **Merkezi Yönetim**: Konfigürasyon değişiklikleri için merkezi bir yönetim yaklaşımı benimsenir.

## Ortam Değişkenleri

### Temel Ortam Değişkenleri

| Değişken | Açıklama | Varsayılan Değer |
|----------|----------|------------------|
| `NODE_ENV` | Çalışma ortamı | `development` |
| `PORT` | Uygulama port numarası | `3000` |
| `LOG_LEVEL` | Loglama seviyesi | `info` |
| `DOMAIN` | Uygulama domain adresi | `http://localhost:3000` |
| `API_PREFIX` | API endpoint önek yolu | `/api/v1` |
| `FRONTEND_URL` | Frontend URL | `http://localhost:8080` |
| `ENABLE_SWAGGER` | Swagger dokümantasyonu aktif mi | `true` (dev) / `false` (prod) |

### Veritabanı Konfigürasyonu

| Değişken | Açıklama | Varsayılan Değer |
|----------|----------|------------------|
| `DB_TYPE` | Veritabanı türü | `mongodb` |
| `DB_URI` | Ana veritabanı bağlantı URI'si | `mongodb://localhost:27017/nexfit` |
| `DB_USER` | Veritabanı kullanıcı adı | - |
| `DB_PASSWORD` | Veritabanı şifresi | - |
| `DB_HOST` | Veritabanı sunucu adresi | `localhost` |
| `DB_PORT` | Veritabanı port numarası | `27017` |
| `DB_NAME` | Veritabanı adı | `nexfit` |
| `DB_SSL` | SSL kullanımı | `false` |
| `DB_POOL_SIZE` | Bağlantı havuzu boyutu | `10` |
| `DB_DEBUG` | Debug modu açık mı | `false` |

### Redis Konfigürasyonu

| Değişken | Açıklama | Varsayılan Değer |
|----------|----------|------------------|
| `REDIS_URI` | Redis bağlantı URI'si | `redis://localhost:6379` |
| `REDIS_HOST` | Redis sunucu adresi | `localhost` |
| `REDIS_PORT` | Redis port numarası | `6379` |
| `REDIS_PASSWORD` | Redis şifresi | - |
| `REDIS_DB` | Redis veritabanı numarası | `0` |
| `REDIS_PREFIX` | Anahtar öneki | `nexfit:` |
| `CACHE_TTL` | Önbellek süre limiti (saniye) | `300` |

### Authentication ve Security

| Değişken | Açıklama | Varsayılan Değer |
|----------|----------|------------------|
| `JWT_SECRET` | JWT imzalama anahtarı | - |
| `JWT_EXPIRATION` | JWT geçerlilik süresi | `86400` (1 gün) |
| `JWT_REFRESH_EXPIRATION` | Yenileme token süresi | `604800` (7 gün) |
| `BCRYPT_SALT_ROUNDS` | Bcrypt tuz faktörü | `10` |
| `ACCESS_CONTROL_ALLOW_ORIGIN` | CORS izin verilen origin | `*` |
| `RATE_LIMIT_WINDOW` | Rate limit penceresi (ms) | `900000` (15dk) |
| `RATE_LIMIT_MAX` | Maksimum istek sayısı | `100` |
| `ENABLE_CSRF` | CSRF koruması aktif mi | `true` |

### Email ve Bildirim

| Değişken | Açıklama | Varsayılan Değer |
|----------|----------|------------------|
| `SMTP_HOST` | SMTP sunucu adresi | - |
| `SMTP_PORT` | SMTP port numarası | `587` |
| `SMTP_USER` | SMTP kullanıcı adı | - |
| `SMTP_PASS` | SMTP şifresi | - |
| `SMTP_SECURE` | SMTP güvenli bağlantı | `false` |
| `EMAIL_FROM` | Gönderen e-posta adresi | `noreply@nexfit.io` |
| `EMAIL_FROM_NAME` | Gönderen adı | `NexFit` |
| `FCM_SERVER_KEY` | Firebase Cloud Messaging anahtarı | - |

### Depolama Konfigürasyonu

| Değişken | Açıklama | Varsayılan Değer |
|----------|----------|------------------|
| `STORAGE_TYPE` | Depolama türü | `local` |
| `STORAGE_LOCAL_PATH` | Yerel depolama yolu | `./uploads` |
| `S3_BUCKET` | AWS S3 bucket adı | - |
| `S3_REGION` | AWS S3 bölgesi | `eu-central-1` |
| `S3_ACCESS_KEY` | AWS S3 erişim anahtarı | - |
| `S3_SECRET_KEY` | AWS S3 gizli anahtarı | - |
| `MINIO_ENDPOINT` | MinIO endpoint adresi | - |
| `MINIO_PORT` | MinIO port numarası | `9000` |
| `MINIO_USE_SSL` | MinIO SSL kullanımı | `false` |

## Konfigürasyon Dosyaları

### .env Dosyası

`.env` dosyası, yerel geliştirme ortamı için ortam değişkenlerini tanımlar. Bu dosya Git tarafından izlenmez ve her geliştiricinin kendi yerel konfigürasyonunu içerir.

Örnek `.env` dosyası:

```env
# Temel Konfigürasyonlar
NODE_ENV=development
PORT=3000
LOG_LEVEL=debug
DOMAIN=http://localhost:3000

# Veritabanı
DB_URI=mongodb://localhost:27017/nexfit
DB_DEBUG=true

# Redis
REDIS_URI=redis://localhost:6379

# Authentication
JWT_SECRET=your_secret_key_here
JWT_EXPIRATION=86400

# Email
SMTP_HOST=smtp.mailtrap.io
SMTP_PORT=2525
SMTP_USER=your_mailtrap_user
SMTP_PASS=your_mailtrap_password
```

### .env.example Dosyası

`.env.example` dosyası, yeni geliştiricilerin kendi `.env` dosyalarını oluşturmaları için referans olarak sunulan bir şablondur. Hassas bilgileri içermez, sadece hangi değişkenlerin tanımlanması gerektiğini gösterir.

### config/ Dizini

`config/` dizini, çeşitli modüller ve hizmetler için yapılandırma dosyalarını içerir:

- `config/default.js`: Tüm ortamlarda geçerli olan varsayılan ayarlar
- `config/development.js`: Geliştirme ortamına özgü ayarlar
- `config/test.js`: Test ortamına özgü ayarlar
- `config/production.js`: Üretim ortamına özgü ayarlar
- `config/custom-environment-variables.js`: Ortam değişkenlerinden alınacak değerlerin eşleştirmesi

### Konfigürasyon Kullanımı

JavaScript kodunda konfigürasyon değerlerine erişmek için `config` paketini kullanıyoruz:

```javascript
const config = require('config');

// Konfigürasyon değerlerine erişim
const port = config.get('server.port');
const dbUri = config.get('database.uri');
const jwtSecret = config.get('auth.jwt.secret');

// Konfigürasyonun var olup olmadığını kontrol etme
if (config.has('optionalFeature.enabled')) {
  // İsteğe bağlı özelliği etkinleştir
}
```

## Ortam Bazlı Konfigürasyon

NexFit, farklı ortamlar için farklı ayarlar kullanmayı destekler:

### Development (Geliştirme) Ortamı

- Debug modu aktif
- Detaylı logging
- CORS kısıtlamaları gevşek
- Gelişmiş hata mesajları
- In-memory veya yerel veritabanları
- Sahte ödeme entegrasyonu

### Test Ortamı

- Birim testleri ve entegrasyon testleri için özelleştirilmiş
- In-memory veritabanları
- Mock dış servisler
- Deterministik davranış için sabit değerler

### Staging Ortamı

- Üretim benzeri konfigürasyon
- Tam veri setleri
- Gerçek dış servis entegrasyonları (test ortamları)
- Ölçeklendirilmiş altyapı

### Production (Üretim) Ortamı

- Hata ayıklama modu devre dışı
- Minimum logging (sadece önemli bilgiler)
- Katı güvenlik önlemleri
- Yüksek performans ayarları
- Ölçeklendirilmiş veritabanı ve cache konfigürasyonu
- CDN entegrasyonu

## Harici Servis Konfigürasyonları

### Ödeme Servisleri

#### Stripe Konfigürasyonu

```javascript
// config/payment.js
module.exports = {
  stripe: {
    secretKey: process.env.STRIPE_SECRET_KEY,
    publicKey: process.env.STRIPE_PUBLIC_KEY,
    webhookSecret: process.env.STRIPE_WEBHOOK_SECRET,
    paymentMethods: ['card', 'ideal', 'bancontact'],
    currency: 'TRY',
    statementDescriptor: 'NEXFIT',
    statementDescriptorSuffix: 'MEMBERSHIP'
  }
};
```

#### Iyzico Konfigürasyonu

```javascript
// config/payment.js
module.exports = {
  iyzico: {
    apiKey: process.env.IYZICO_API_KEY,
    secretKey: process.env.IYZICO_SECRET_KEY,
    baseUrl: process.env.IYZICO_BASE_URL || 'https://api.iyzipay.com',
    currency: 'TRY'
  }
};
```

### Bulut Hizmetleri

#### AWS Konfigürasyonu

```javascript
// config/cloud.js
module.exports = {
  aws: {
    region: process.env.AWS_REGION || 'eu-central-1',
    s3: {
      bucket: process.env.S3_BUCKET,
      accessKeyId: process.env.S3_ACCESS_KEY,
      secretAccessKey: process.env.S3_SECRET_KEY,
      signedUrlExpires: 3600 // 1 saat
    },
    cloudfront: {
      distributionId: process.env.CLOUDFRONT_DISTRIBUTION_ID,
      domain: process.env.CLOUDFRONT_DOMAIN
    }
  }
};
```

#### Firebase Konfigürasyonu

```javascript
// config/notification.js
module.exports = {
  firebase: {
    type: process.env.FIREBASE_TYPE,
    projectId: process.env.FIREBASE_PROJECT_ID,
    privateKeyId: process.env.FIREBASE_PRIVATE_KEY_ID,
    privateKey: process.env.FIREBASE_PRIVATE_KEY?.replace(/\\n/g, '\n'),
    clientEmail: process.env.FIREBASE_CLIENT_EMAIL,
    clientId: process.env.FIREBASE_CLIENT_ID,
    authUri: process.env.FIREBASE_AUTH_URI,
    tokenUri: process.env.FIREBASE_TOKEN_URI,
    certUrl: process.env.FIREBASE_CERT_URL,
    fcmServerKey: process.env.FCM_SERVER_KEY
  }
};
```

## Güvenlik Konfigürasyonu

### Helmet Konfigürasyonu

Web uygulaması güvenliği için Helmet ayarları:

```javascript
// config/security.js
module.exports = {
  helmet: {
    contentSecurityPolicy: {
      directives: {
        defaultSrc: ["'self'"],
        scriptSrc: ["'self'", "'unsafe-inline'", 'https://js.stripe.com'],
        styleSrc: ["'self'", "'unsafe-inline'", 'https://fonts.googleapis.com'],
        fontSrc: ["'self'", 'https://fonts.gstatic.com'],
        imgSrc: ["'self'", 'data:', 'https://*.nexfit.io', 'https://nexfit-assets.s3.amazonaws.com'],
        connectSrc: ["'self'", 'https://api.nexfit.io', 'https://api-staging.nexfit.io'],
        frameSrc: ["'self'", 'https://js.stripe.com'],
        objectSrc: ["'none'"],
        upgradeInsecureRequests: []
      }
    },
    hsts: {
      maxAge: 15552000,
      includeSubDomains: true,
      preload: true
    },
    frameguard: {
      action: 'deny'
    },
    referrerPolicy: { policy: 'strict-origin-when-cross-origin' }
  },
  cors: {
    origin: process.env.CORS_ORIGIN || '*',
    methods: ['GET', 'POST', 'PUT', 'PATCH', 'DELETE', 'OPTIONS'],
    allowedHeaders: ['Content-Type', 'Authorization'],
    exposedHeaders: ['X-Total-Count', 'X-Pagination-Pages'],
    credentials: true,
    maxAge: 86400 // 24 saat
  },
  rateLimit: {
    windowMs: parseInt(process.env.RATE_LIMIT_WINDOW) || 900000, // 15 dakika
    max: parseInt(process.env.RATE_LIMIT_MAX) || 100, // IP başına istek limiti
    standardHeaders: true,
    legacyHeaders: false,
    skipSuccessfulRequests: false,
    keyGenerator: (req) => req.ip
  }
};
```

### JWT Konfigürasyonu

```javascript
// config/auth.js
module.exports = {
  jwt: {
    secret: process.env.JWT_SECRET,
    accessExpiresIn: parseInt(process.env.JWT_EXPIRATION) || 86400, // 1 gün
    refreshExpiresIn: parseInt(process.env.JWT_REFRESH_EXPIRATION) || 604800, // 7 gün
    algorithm: 'HS256',
    issuer: 'nexfit',
    audience: 'nexfit-users'
  },
  password: {
    saltRounds: parseInt(process.env.BCRYPT_SALT_ROUNDS) || 10,
    minLength: 8,
    requireUppercase: true,
    requireLowercase: true,
    requireNumbers: true,
    requireSpecialChars: true
  },
  mfa: {
    enabled: process.env.MFA_ENABLED === 'true',
    issuer: 'NexFit',
    window: 1
  }
};
```

## Performans Ayarları

### Cache Konfigürasyonu

```javascript
// config/cache.js
module.exports = {
  redis: {
    uri: process.env.REDIS_URI,
    host: process.env.REDIS_HOST || 'localhost',
    port: parseInt(process.env.REDIS_PORT) || 6379,
    password: process.env.REDIS_PASSWORD,
    db: parseInt(process.env.REDIS_DB) || 0,
    keyPrefix: process.env.REDIS_PREFIX || 'nexfit:'
  },
  caching: {
    ttl: parseInt(process.env.CACHE_TTL) || 300, // 5 dakika
    checkPeriod: 600, // 10 dakika
    enabled: process.env.NODE_ENV === 'production',
    routes: {
      '/api/v1/training/exercises': 3600, // 1 saat
      '/api/v1/content/articles': 1800, // 30 dakika
      '/api/v1/users/me': 60 // 1 dakika
    }
  }
};
```

### Veritabanı Performans Ayarları

```javascript
// config/database.js
module.exports = {
  mongodb: {
    uri: process.env.DB_URI,
    options: {
      useNewUrlParser: true,
      useUnifiedTopology: true,
      poolSize: parseInt(process.env.DB_POOL_SIZE) || 10,
      serverSelectionTimeoutMS: 5000,
      socketTimeoutMS: 45000,
      connectTimeoutMS: 10000,
      keepAlive: true,
      keepAliveInitialDelay: 300000,
      autoIndex: process.env.NODE_ENV !== 'production',
      retryWrites: true,
      w: 'majority'
    }
  }
};
```

## Logging ve Monitoring

### Winston Logger Konfigürasyonu

```javascript
// config/logging.js
module.exports = {
  logger: {
    level: process.env.LOG_LEVEL || 'info',
    format: process.env.NODE_ENV === 'production' ? 'json' : 'console',
    transports: {
      console: {
        enabled: true,
        level: process.env.LOG_LEVEL || 'info',
        handleExceptions: true
      },
      file: {
        enabled: process.env.LOG_TO_FILE === 'true',
        level: 'info',
        filename: 'logs/app-%DATE%.log',
        datePattern: 'YYYY-MM-DD',
        maxSize: '20m',
        maxFiles: '14d'
      }
    },
    meta: {
      service: 'nexfit-api',
      version: process.env.npm_package_version,
      environment: process.env.NODE_ENV
    }
  },
  morgan: {
    enabled: true,
    format: process.env.NODE_ENV === 'production' ? 'combined' : 'dev'
  }
};
```

### Sentry Konfigürasyonu

```javascript
// config/monitoring.js
module.exports = {
  sentry: {
    dsn: process.env.SENTRY_DSN,
    environment: process.env.NODE_ENV,
    release: process.env.npm_package_version,
    enabled: process.env.NODE_ENV === 'production',
    tracesSampleRate: 0.2,
    integrations: ['http', 'mongo', 'redis']
  },
  prometheus: {
    enabled: process.env.PROMETHEUS_ENABLED === 'true',
    endpoint: '/metrics',
    defaultLabels: {
      service: 'nexfit-api',
      environment: process.env.NODE_ENV
    }
  }
};
```

## Sık Karşılaşılan Konfigürasyon Sorunları

### Veritabanı Bağlantı Sorunları

**Sorun**: MongoDB'ye bağlanırken "Connection timed out" hatası.

**Çözüm**:
1. MongoDB servisinin çalıştığını kontrol edin
2. Firewall ayarlarını kontrol edin
3. Doğru bağlantı URL'sini kullandığınızdan emin olun
4. `DB_URI` değişkenini kontrol edin:
```
# Doğru format:
DB_URI=mongodb://username:password@hostname:port/database

# Örnek:
DB_URI=mongodb://nexfit_user:secure_password@localhost:27017/nexfit
```

### JWT Doğrulama Hataları

**Sorun**: "Invalid signature" veya "jwt malformed" hataları.

**Çözüm**:
1. `JWT_SECRET` değerinin tüm ortamlarda aynı olduğundan emin olun
2. Token'ın doğru şekilde oluşturulduğundan emin olun
3. JWT algoritmalarının uyumlu olduğunu kontrol edin
4. Token süresinin dolmadığından emin olun

### CORS Hataları

**Sorun**: "Access-Control-Allow-Origin" hataları.

**Çözüm**:
1. `ACCESS_CONTROL_ALLOW_ORIGIN` değişkenini kontrol edin
2. Frontend URL'sini doğru şekilde yapılandırın:
```
# Örnek:
ACCESS_CONTROL_ALLOW_ORIGIN=https://app.nexfit.io,https://admin.nexfit.io
```
3. Credentials ayarlarını kontrol edin
4. OPTIONS isteklerinin doğru işlendiğinden emin olun

### Redis Bağlantı Sorunları

**Sorun**: "Error: Redis connection to localhost:6379 failed"

**Çözüm**:
1. Redis servisinin çalıştığını kontrol edin
2. Redis bağlantı bilgilerini doğrulayın
3. Redis şifresini kontrol edin (eğer varsa)
4. `REDIS_URI` yerine ayrı ayrı parametreleri deneyin:
```
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=your_password
```

### Depolama Yapılandırma Sorunları

**Sorun**: Dosya yükleme hatası "Access Denied to bucket"

**Çözüm**:
1. S3 erişim anahtarlarının doğru olduğundan emin olun
2. Bucket izinlerini kontrol edin
3. S3 bölgesinin doğru ayarlandığından emin olun
4. Bucket adının doğru olduğunu kontrol edin

---

Bu dokümantasyon, NexFit uygulamasının konfigürasyonu hakkında kapsamlı bir rehber sunmaktadır. Proje geliştikçe ve yeni konfigürasyon parametreleri eklendikçe bu belge güncellenecektir.

Konfigürasyon hakkında sorularınız veya önerileriniz için DevOps ekibi ile iletişime geçebilirsiniz: [devops@nexfit.com](mailto:devops@nexfit.com) 