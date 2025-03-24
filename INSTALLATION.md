# NexFit Kurulum Kılavuzu

Bu belge, NexFit uygulamasının kurulum ve yapılandırma sürecini detaylı olarak açıklamaktadır.

## Önkoşullar

NexFit'i çalıştırmak için aşağıdaki yazılımların kurulu olması gerekmektedir:

### Geliştirme Ortamı
- Node.js (v16.x veya üzeri)
- npm (v8.x veya üzeri) veya Yarn (v1.22.x veya üzeri)
- Git

### Veritabanı
- MongoDB (v5.0 veya üzeri)
- PostgreSQL (v14.0 veya üzeri)
- Redis (v6.2 veya üzeri)

### Mobil Geliştirme (isteğe bağlı)
- Android Studio (Android geliştirme için)
- Xcode (iOS geliştirme için, yalnızca macOS)
- React Native CLI

## Kurulum Adımları

### 1. Depoyu Klonlama

```bash
git clone https://github.com/canakyuz/nexfit-app.git
cd nexfit-app
```

### 2. Bağımlılıkları Yükleme

```bash
# npm ile
npm install

# veya Yarn ile
yarn install
```

### 3. Ortam Değişkenlerini Yapılandırma

`.env.example` dosyasını `.env` olarak kopyalayın ve gerekli değişkenleri ayarlayın:

```bash
cp .env.example .env
```

Aşağıdaki değişkenleri kendi ortamınıza göre düzenleyin:

```
# API ve Uygulama Yapılandırması
API_URL=http://localhost:3000/api
APP_ENV=development
APP_PORT=3000

# Veritabanı Yapılandırması
MONGODB_URI=mongodb://localhost:27017/nexfit
POSTGRES_URI=postgresql://postgres:password@localhost:5432/nexfit
REDIS_URL=redis://localhost:6379

# Kimlik Doğrulama ve Güvenlik
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRES_IN=7d
BCRYPT_SALT_ROUNDS=10

# Üçüncü Taraf API Anahtarları
FIREBASE_API_KEY=your_firebase_api_key
FIREBASE_AUTH_DOMAIN=your_firebase_auth_domain
FIREBASE_PROJECT_ID=your_firebase_project_id
FIREBASE_STORAGE_BUCKET=your_firebase_storage_bucket
FIREBASE_MESSAGING_SENDER_ID=your_firebase_messaging_sender_id
FIREBASE_APP_ID=your_firebase_app_id

# Dosya Yükleme
UPLOAD_DIR=uploads
MAX_FILE_SIZE=10485760 # 10MB

# E-posta Yapılandırması
SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_USER=your_smtp_username
SMTP_PASS=your_smtp_password
EMAIL_FROM=info@example.com
```

### 4. Veritabanı Kurulumu

MongoDB, PostgreSQL ve Redis'in kurulu ve çalışır durumda olduğundan emin olun.

```bash
# MongoDB başlatma
mongod --dbpath /path/to/your/data/directory

# PostgreSQL başlatma (Linux)
sudo systemctl start postgresql

# PostgreSQL başlatma (macOS)
brew services start postgresql

# Redis başlatma
redis-server
```

PostgreSQL'de veritabanı ve kullanıcı oluşturma:

```bash
# PostgreSQL'e bağlanma
psql -U postgres

# Veritabanı oluşturma
CREATE DATABASE nexfit;

# Kullanıcı oluşturma ve yetkilendirme
CREATE USER nexfit_user WITH ENCRYPTED PASSWORD 'secure_password';
GRANT ALL PRIVILEGES ON DATABASE nexfit TO nexfit_user;

# Bağlantıyı sonlandırma
\q
```

İlk kez başlatıyorsanız, veritabanlarını ve gerekli koleksiyonları/tabloları oluşturmak için:

```bash
# MongoDB ve PostgreSQL kurulumu
npm run db:setup

# veya
yarn db:setup
```

### 5. Geliştirme Sunucusunu Başlatma

```bash
# Geliştirme modunda backend ve frontend'i başlatma
npm run dev

# veya
yarn dev
```

Bu komut, backend API sunucusunu ve frontend geliştirme sunucusunu eşzamanlı olarak başlatacaktır.

- Backend API: `http://localhost:3000/api`
- Frontend: `http://localhost:3000`

### 6. Mobil Uygulama Geliştirme (isteğe bağlı)

Mobil uygulamayı çalıştırmak için:

```bash
# Android
npm run android

# veya
yarn android

# iOS (yalnızca macOS)
npm run ios

# veya
yarn ios
```

## Docker ile Kurulum

Docker ve Docker Compose kullanarak tüm bağımlılıklarla birlikte kurulum yapabilirsiniz.

### Önkoşullar
- Docker
- Docker Compose

### Adımlar

1. Depoyu klonlayın ve proje dizinine gidin:

```bash
git clone https://github.com/canakyuz/nexfit-app.git
cd nexfit-app
```

2. Ortam değişkenlerini yapılandırın:

```bash
cp .env.example .env
```

3. Docker Compose ile uygulamayı başlatın:

```bash
docker-compose up -d
```

Bu komut, aşağıdaki servisleri oluşturacak ve başlatacaktır:
- MongoDB
- Redis
- NexFit API
- NexFit Web
- NexFit Admin (isteğe bağlı)

4. Uygulamaya erişim:
- Web Uygulaması: `http://localhost:3000`
- API: `http://localhost:3000/api`
- Admin Paneli: `http://localhost:3000/admin`

## Kurulum Doğrulama

Kurulumun başarılı olduğunu doğrulamak için:

1. Web tarayıcınızdan `http://localhost:3000` adresine gidin
2. Giriş sayfasının düzgün yüklendiğini kontrol edin
3. API'nin çalıştığını doğrulamak için `http://localhost:3000/api/health` adresini ziyaret edin

## Ortam Yapılandırması

NexFit farklı ortamlar için yapılandırılabilir:

### Geliştirme Ortamı
```
APP_ENV=development
```

Geliştirme ortamında:
- Detaylı hata mesajları
- Otomatik yeniden yükleme
- API'de test araçları
- Performans optimizasyonları devre dışı

### Test Ortamı
```
APP_ENV=test
```

Test ortamında:
- Test veritabanı kullanımı
- Mock servisler
- Tam loglama
- E-posta gönderimi devre dışı

### Üretim Ortamı
```
APP_ENV=production
```

Üretim ortamında:
- Minimalist hata mesajları
- Performans optimizasyonları etkin
- Sıkı güvenlik kontrolleri
- CDN entegrasyonu

## Veritabanı Yönetimi

### Migrations

Veritabanı şema değişikliklerini uygulamak için:

```bash
# Tüm migration'ları çalıştırma
npm run db:migrate

# Belirli bir migration'ı çalıştırma
npm run db:migrate --name=create-users

# Migration'ları geri alma
npm run db:migrate:undo
```

### Seeding

Test verileri eklemek için:

```bash
# Tüm seed'leri çalıştırma
npm run db:seed

# Belirli bir seed'i çalıştırma
npm run db:seed --name=users

# Seed'leri geri alma
npm run db:seed:undo
```

## Yaygın Sorunlar ve Çözümleri

### MongoDB Bağlantı Hataları

**Sorun:** `MongoConnectionError: failed to connect to server`

**Çözüm:**
1. MongoDB servisinin çalıştığından emin olun
2. Doğru bağlantı URI'sini kullandığınızı kontrol edin
3. MongoDB'nin dinlediği bağlantı noktasının erişilebilir olduğunu kontrol edin

### Node.js Sürüm Uyumsuzluğu

**Sorun:** `Error: The engine "node" is incompatible with this module`

**Çözüm:**
1. `nvm` veya benzeri bir araç ile doğru Node.js sürümüne geçin:
```bash
nvm install 16
nvm use 16
```

### React Native Bağımlılık Hataları

**Sorun:** `Failed to install CocoaPods dependencies`

**Çözüm:**
1. Cocoapods'u yeniden kurun: `sudo gem install cocoapods`
2. Pod'ları güncelle: `cd ios && pod install && cd ..`

## Güvenlik Notları

- JWT anahtarlarını ve diğer hassas bilgileri güvenli bir şekilde saklayın
- Gerçek üretim ortamında güçlü ve benzersiz şifreler kullanın
- API anahtarlarını ve sırlarını `.env` dosyasında saklayın ve bu dosyayı Git'e eklemekten kaçının
- Üretim ortamında HTTPS kullanın
- Hassas API'leri rate limiting ile koruyun

## Destek ve İletişim

Kurulum ile ilgili sorunlar yaşarsanız:

- GitHub Issues: [https://github.com/canakyuz/nexfit-app/issues](https://github.com/canakyuz/nexfit-app/issues)
- E-posta: support@nexfit.com
- Dokümentasyon: [https://docs.nexfit.com](https://docs.nexfit.com)

## Katkıda Bulunma

NexFit'e katkıda bulunmak isterseniz:

1. Depoyu forklayın
2. Yeni bir dal oluşturun: `git checkout -b feature/amazing-feature`
3. Değişikliklerinizi commit edin: `git commit -m 'Add amazing feature'`
4. Dalınızı push edin: `git push origin feature/amazing-feature`
5. Bir Pull Request oluşturun

Katkı sağlamadan önce lütfen [CONTRIBUTING.md](CONTRIBUTING.md) dosyasını okuyun. 