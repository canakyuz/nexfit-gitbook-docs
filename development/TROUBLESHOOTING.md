# 🔧 NexFit Sorun Giderme Rehberi

Bu rehber, NexFit uygulamasının geliştirme, kurulum ve çalıştırma süreçlerinde karşılaşılabilecek yaygın sorunların çözümlerini sunmaktadır.

## İçindekiler

- [Kurulum Sorunları](#kurulum-sorunları)
- [Veritabanı Sorunları](#veritabanı-sorunları)
- [API ve Backend Sorunları](#api-ve-backend-sorunları)
- [Frontend Sorunları](#frontend-sorunları)
- [React Native / Mobil Sorunları](#react-native--mobil-sorunları)
- [Kimlik Doğrulama Sorunları](#kimlik-doğrulama-sorunları)
- [Performans Sorunları](#performans-sorunları)
- [Docker ve Konteyner Sorunları](#docker-ve-konteyner-sorunları)
- [CI/CD Sorunları](#cicd-sorunları)
- [Hata Ayıklama Teknikleri](#hata-ayıklama-teknikleri)
- [Destek ve Yardım Alma](#destek-ve-yardım-alma)

## Kurulum Sorunları

### Node.js Sürüm Uyumsuzluğu

**Belirti**: "Your Node.js version is not supported" veya "Unexpected token" hataları.

**Çözüm**:
1. NexFit, Node.js 16.x veya üzeri sürümleri gerektirir. Mevcut sürümünüzü kontrol edin:
   ```bash
   node --version
   ```
2. [Node.js web sitesinden](https://nodejs.org/) uygun sürümü yükleyin veya NVM (Node Version Manager) kullanın:
   ```bash
   nvm install 16
   nvm use 16
   ```
3. Projeyi temiz bir şekilde yeniden yükleyin:
   ```bash
   rm -rf node_modules
   npm cache clean --force
   npm install
   ```

### Bağımlılık Çakışmaları

**Belirti**: "Conflicting peer dependency" veya "Unable to resolve dependency tree" hataları.

**Çözüm**:
1. `--force` bayrağı ile yüklemeyi deneyin:
   ```bash
   npm install --force
   ```
2. Veya `--legacy-peer-deps` bayrağını kullanın:
   ```bash
   npm install --legacy-peer-deps
   ```
3. `package-lock.json` dosyasını silip yeniden oluşturun:
   ```bash
   rm package-lock.json
   npm install
   ```

### Yetkisiz Erişim Hataları

**Belirti**: "EACCES: permission denied" hataları.

**Çözüm**:
1. Global paketleri yüklerken `sudo` kullanın:
   ```bash
   sudo npm install -g <paket-adı>
   ```
2. veya npm'in global paketlerini erişilebilir bir konuma değiştirin:
   ```bash
   mkdir ~/.npm-global
   npm config set prefix '~/.npm-global'
   echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.profile
   source ~/.profile
   ```
3. Dosya izinlerini düzeltin:
   ```bash
   sudo chown -R $(whoami) ~/.npm
   sudo chown -R $(whoami) ./node_modules
   ```

## Veritabanı Sorunları

### MongoDB Bağlantı Hataları

**Belirti**: "MongoNetworkError: failed to connect to server" hatası.

**Çözüm**:
1. MongoDB servisinin çalıştığını kontrol edin:
   ```bash
   # Linux/macOS
   sudo systemctl status mongodb
   
   # macOS (Homebrew)
   brew services list
   ```
2. MongoDB için doğru bağlantı dizesini kullandığınızdan emin olun:
   ```
   # .env dosyanızdaki bağlantı dizesini kontrol edin
   DB_URI=mongodb://localhost:27017/nexfit
   ```
3. Güvenlik duvarı ayarlarını kontrol edin:
   ```bash
   # MongoDB'nin varsayılan portu olan 27017'nin açık olduğundan emin olun
   sudo ufw status
   ```
4. MongoDB'yi manuel olarak başlatın:
   ```bash
   # Sistem servisi olarak
   sudo systemctl start mongodb
   
   # veya doğrudan ikili dosya ile
   mongod --dbpath=/var/lib/mongodb
   ```

### Veritabanı Şema Sorunları

**Belirti**: Modeller için "ValidationError" veya "CastError" hataları.

**Çözüm**:
1. Veritabanı şemasını güncel tuttuğunuzdan emin olun:
   ```bash
   npm run migrate
   ```
2. Veritabanı içeriğini incelemek için Mongo shell veya MongoDB Compass kullanın:
   ```bash
   mongo
   use nexfit
   db.users.find()
   ```
3. Test amaçlı olarak veritabanını sıfırlayın (sadece geliştirme ortamında):
   ```bash
   # MongoDB'de veritabanını sıfırlama
   mongo nexfit --eval "db.dropDatabase()"
   
   # Sonra yeniden seed verilerini yükleyin
   npm run db:seed
   ```

### Redis Bağlantı Sorunları

**Belirti**: "Error connecting to Redis" veya "Failed to connect to Redis" hataları.

**Çözüm**:
1. Redis'in çalıştığını kontrol edin:
   ```bash
   redis-cli ping
   ```
2. Redis servisini yeniden başlatın:
   ```bash
   # Linux/macOS
   sudo systemctl restart redis
   
   # macOS (Homebrew)
   brew services restart redis
   ```
3. Redis yapılandırmasını kontrol edin:
   ```bash
   # Redis önbelleğini temizleyin
   redis-cli flushall
   
   # Redis durumunu incelemek için
   redis-cli info
   ```

## API ve Backend Sorunları

### API Endpoint Hataları

**Belirti**: "404 Not Found" veya "Cannot GET /api/..." hataları.

**Çözüm**:
1. API endpoint'in doğru yazıldığından emin olun. URL'lerde büyük/küçük harf duyarlılığı vardır.
2. Rota tanımlamalarını kontrol edin:
   ```javascript
   // src/routes/index.js veya benzer dosyayı inceleyip doğru rotaların eklendiğinden emin olun
   router.use('/api/users', require('./users'));
   ```
3. API'nin çalıştığını basit bir endpoint ile test edin:
   ```bash
   curl http://localhost:3000/api/health
   ```
4. Geliştirme sunucusunu yeniden başlatın:
   ```bash
   npm run dev
   ```

### JWT Kimlik Doğrulama Hataları

**Belirti**: "Invalid token" veya "jwt malformed" hataları.

**Çözüm**:
1. Token'ın doğru biçimde gönderildiğinden emin olun:
   ```
   Authorization: Bearer <token>
   ```
2. JWT_SECRET'ın .env dosyasında tanımlandığını kontrol edin.
3. Token süresi dolmuş olabilir. Yeniden giriş yapın veya refresh token kullanın.
4. JWT'yi manuel olarak decode edin ve içeriğini kontrol edin:
   ```javascript
   const jwt = require('jsonwebtoken');
   const decoded = jwt.decode(token);
   console.log(decoded);
   ```

### Middleware Sorunları

**Belirti**: İstekler beklenmeyen şekilde engelleniyor veya "next is not a function" hataları.

**Çözüm**:
1. Middleware sırasını kontrol edin. Middleware'ler tanımlandıkları sırada çalışır.
2. `next()` çağrılarının tüm koşul dallarında uygun şekilde yapıldığından emin olun.
3. Middleware içindeki hata yakalama mantığını kontrol edin:
   ```javascript
   function middleware(req, res, next) {
     try {
       // İşlem
       next();
     } catch (err) {
       next(err); // Hataları aktarmayı unutmayın
     }
   }
   ```
4. Express middleware sorunlarını debug etmek için:
   ```bash
   DEBUG=express:* npm run dev
   ```

## Frontend Sorunları

### React Render Sorunları

**Belirti**: Beyaz ekran, "Element type is invalid" veya "React.Children.only expected to receive a single React element child" hataları.

**Çözüm**:
1. Component'in JSX yapısını kontrol edin. Tüm JSX elementlerinin doğru şekilde kapatıldığından emin olun.
2. React Developer Tools ile component ağacını inceleyin.
3. Konsol hatalarını kontrol edin ve component'lerin render fonksiyonlarını gözden geçirin.
4. Prop türlerinin doğru olduğundan emin olun:
   ```javascript
   // Propların beklenen türde olduğunu kontrol edin
   console.log('Props:', props);
   ```

### Stil ve CSS Sorunları

**Belirti**: Bileşenler yanlış görünüyor, stiller uygulanmıyor.

**Çözüm**:
1. CSS/SCSS dosyalarının doğru içe aktarıldığından emin olun.
2. Tarayıcı geliştirici araçlarını kullanarak stilleri inceleyin, hangi CSS kurallarının geçersiz kılındığını kontrol edin.
3. CSS öncelik ve özgüllük sorunlarını kontrol edin.
4. Stil kütüphanesi veya Styled Components kullanıyorsanız, tema sağlayıcısının doğru kurulduğundan emin olun.
5. Çakışan stil kütüphanelerini kontrol edin ve gereksizleri kaldırın.

### State Yönetimi Sorunları

**Belirti**: Veri güncellemeleri UI'a yansımıyor veya beklenmedik davranışlar oluyor.

**Çözüm**:
1. State güncelleme işlemlerini kontrol edin:
   ```javascript
   // Class component'lerde
   this.setState(prevState => ({
     count: prevState.count + 1
   }), () => console.log('State güncellendi:', this.state));
   
   // Hooks'ta
   const [count, setCount] = useState(0);
   setCount(prevCount => {
     console.log('Önceki:', prevCount);
     return prevCount + 1;
   });
   ```
2. Redux DevTools veya Context API için console.log ile state değişimlerini izleyin.
3. Immutable state güncellemeleri yapıldığından emin olun, doğrudan state mutasyonundan kaçının.
4. Custom hook'ların doğru yazıldığından emin olun ve gereksiz render'ları önleyin:
   ```javascript
   // useMemo ve useCallback'i uygun şekilde kullanın
   const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
   ```

## React Native / Mobil Sorunları

### Derleme Hataları

**Belirti**: "Execution failed for task ':app:compileDebugJavaWithJavac'" veya "Error: Failed to construct transformer" gibi hatalar.

**Çözüm**:
1. Cache'i temizleyin:
   ```bash
   npx react-native start --reset-cache
   ```
2. Metro bundler ve bağımlılıkları yeniden başlatın:
   ```bash
   # Metro'yu durdurun, node_modules'ü temizleyin ve yeniden başlatın
   killall node
   rm -rf node_modules
   npm install
   npx react-native start
   ```
3. Android için Gradle cache'i temizleyin:
   ```bash
   cd android
   ./gradlew clean
   ```
4. iOS için pod'ları yeniden yükleyin:
   ```bash
   cd ios
   pod deintegrate
   pod install
   ```

### Cihaz Bağlantı Sorunları

**Belirti**: "Device is not connected" veya "Could not find device" hataları.

**Çözüm**:
1. Android için ADB cihazlarını kontrol edin:
   ```bash
   adb devices
   ```
2. iOS için Xcode'da cihazları kontrol edin:
   ```bash
   xcrun xctrace list devices
   ```
3. USB kablosunu değiştirin veya farklı bir port deneyin.
4. Android için, telefonda "USB Debugging" seçeneğinin açık olduğundan emin olun.
5. iOS için, geliştirici güvenini cihaz ayarlarından sağlayın.

### Yerli Modül Sorunları

**Belirti**: "Native module cannot be null" veya "Module AppRegistry is not a registered callable module" gibi hatalar.

**Çözüm**:
1. Bağımlılıkları yeniden bağlayın:
   ```bash
   npx react-native link
   ```
2. React Native sürümünüzü kontrol edin ve bağımlılıklarla uyumlu olduğundan emin olun.
3. Pod'ları ve Gradle bağımlılıklarını güncelleyin:
   ```bash
   # iOS için
   cd ios && pod update
   
   # Android için
   cd android && ./gradlew --refresh-dependencies
   ```
4. Manuel olarak yerel modül bağlantılarını kontrol edin: `android/settings.gradle` ve `ios/Podfile` dosyalarını inceleyin.

## Kimlik Doğrulama Sorunları

### Oturum Açma Başarısız

**Belirti**: Doğru kimlik bilgileriyle bile "Invalid credentials" hatası.

**Çözüm**:
1. Kullanıcı adı/e-posta ve parolanın doğru yazıldığından emin olun (büyük/küçük harf duyarlılığı, boşluklar).
2. Veritabanında kullanıcının var olduğunu kontrol edin:
   ```bash
   mongo
   use nexfit
   db.users.findOne({email: "user@example.com"})
   ```
3. Parola hash'leme algoritmasının doğru çalıştığını doğrulayın.
4. `.env` dosyasında `JWT_SECRET` değerinin doğru ayarlandığını kontrol edin.
5. Kullanıcı hesabının kilitli veya askıya alınmadığından emin olun.

### Oturum Süresinin Dolması

**Belirti**: Kullanıcılar kısa süre içinde otomatik olarak çıkış yapıyor.

**Çözüm**:
1. JWT'nin süre ayarlarını kontrol edin:
   ```javascript
   // auth.js veya benzer bir dosyada
   const token = jwt.sign(payload, secret, { expiresIn: '24h' });
   ```
2. `.env` dosyasındaki `JWT_EXPIRATION` değerini daha yüksek bir değere ayarlayın.
3. Refresh token mekanizmasının düzgün çalıştığından emin olun:
   ```javascript
   // Token yenileme işlemini kontrol edin
   const refreshToken = await refreshTokenService.generateRefreshToken(user);
   ```
4. Çerez ayarlarını kontrol edin (httpOnly, secure, sameSite, expires).

### CORS Sorunları

**Belirti**: "Cross-Origin Request Blocked" hataları.

**Çözüm**:
1. Arka uç CORS ayarlarını kontrol edin:
   ```javascript
   // server.js veya app.js
   app.use(cors({
     origin: ['http://localhost:3000', 'https://nexfit.io'],
     credentials: true,
     methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS']
   }));
   ```
2. Ön uç isteğinde credentials ayarını kontrol edin:
   ```javascript
   fetch('https://api.nexfit.io/data', {
     credentials: 'include'
   });
   ```
3. Tarayıcı konsolunda spesifik CORS hatalarını kontrol edin ve buna göre ayarları güncelleyin.

## Performans Sorunları

### API Yanıt Gecikmesi

**Belirti**: API istekleri çok yavaş yanıt veriyor.

**Çözüm**:
1. Veritabanı sorgularını optimize edin, indeksler ekleyin:
   ```javascript
   // MongoDB için indeks eklemek
   db.users.createIndex({ email: 1 });
   ```
2. N+1 sorgu problemlerini kontrol edin ve önbelleğe alma stratejilerini uygulayın.
3. API endpoint'lerde gereksiz veri döndürmediğinizden emin olun, sadece gerekli alanları seçin:
   ```javascript
   // Model.find().select() kullanarak sadece gerekli alanları seçin
   User.find().select('name email profile.avatar -_id');
   ```
4. Performans profillemesi yapın:
   ```bash
   # Node.js için
   node --prof app.js
   
   # Detaylı profilleme için
   npm install -g clinic
   clinic doctor -- node app.js
   ```

### Frontend Performans Sorunları

**Belirti**: UI yavaş, kaydırma takılıyor, animasyonlar düzgün çalışmıyor.

**Çözüm**:
1. React Developer Tools'un Profiler sekmesini kullanarak render performansını analiz edin.
2. Gereksiz render'ları önlemek için React.memo, useMemo ve useCallback kullanın:
   ```javascript
   const MemoizedComponent = React.memo(MyComponent);
   ```
3. Büyük listeleri sanal kaydırma ile optimize edin (react-window, react-virtualized).
4. Lazy loading ve code splitting uygulayın:
   ```javascript
   const LazyComponent = React.lazy(() => import('./LazyComponent'));
   ```
5. Lighthouse veya WebPageTest ile performans analizi yapın.

### Bellek Sızıntıları

**Belirti**: Uygulama zamanla yavaşlıyor, RAM kullanımı sürekli artıyor.

**Çözüm**:
1. React component'lerinde useEffect cleanup fonksiyonlarını kullandığınızdan emin olun:
   ```javascript
   useEffect(() => {
     const subscription = someAPI.subscribe();
     return () => {
       subscription.unsubscribe();
     };
   }, []);
   ```
2. Event listener'ları düzgün şekilde temizleyin.
3. Node.js memory profiling araçlarını kullanın:
   ```bash
   node --inspect app.js
   # Sonra Chrome'da chrome://inspect adresine gidin
   ```
4. Büyük nesnelerin uygun şekilde garbage collected olmasını sağlayın ve döngüsel referanslardan kaçının.

## Docker ve Konteyner Sorunları

### Docker Build Hataları

**Belirti**: "Error response from daemon" veya "The command '/bin/sh -c ...' returned a non-zero code" hataları.

**Çözüm**:
1. Dockerfile'ı kontrol edin ve gereksinimlerin doğru sırada yüklendiğinden emin olun.
2. Docker build işlemini verbose modda çalıştırın:
   ```bash
   docker build --no-cache --progress=plain -t nexfit-app .
   ```
3. Docker imajını yeniden oluşturun:
   ```bash
   docker build --no-cache -t nexfit-app .
   ```
4. Node modüllerini ve bağımlılıkları yerel olarak yükleyip ardından Docker ile paketleyin.

### Docker Compose Sorunları

**Belirti**: "Service 'app' failed to build" veya container'lar birbirine bağlanamıyor.

**Çözüm**:
1. Docker Compose dosyasındaki servis isimlerini ve bağımlılıkları kontrol edin.
2. Network ayarlarını kontrol edin.
3. Docker Compose dosyasını doğrulayın:
   ```bash
   docker-compose config
   ```
4. Docker Compose environment değişkenlerini kontrol edin, `.env` dosyasının doğru konumda olduğundan emin olun.
5. Tüm container'ları durdurup yeniden başlatın:
   ```bash
   docker-compose down -v
   docker-compose up --build
   ```

### Container Bağlantı Sorunları

**Belirti**: Servisler birbirlerine erişemiyor, "Connection refused" hataları.

**Çözüm**:
1. Container'ların aynı network'te olduğundan emin olun:
   ```bash
   docker network inspect <network-name>
   ```
2. Servislerin doğru host isimlerini kullandığından emin olun (IP yerine service adı).
3. Port bindinglerini kontrol edin:
   ```bash
   docker-compose ps
   ```
4. Container içinde bir servise manuel olarak ping atarak bağlantıyı test edin:
   ```bash
   docker exec -it <container-id> ping <service-name>
   ```

## CI/CD Sorunları

### Pipeline Hataları

**Belirti**: CI/CD pipeline'ı başarısız oluyor, testler veya build aşaması geçilemiyor.

**Çözüm**:
1. Pipeline loglarını detaylı şekilde inceleyin ve spesifik hatayı bulun.
2. Lokal ortamda aynı adımları tekrarlayın ve sorun yaşanıp yaşanmadığını kontrol edin.
3. Bağımlılık versiyonlarının CI/CD ortamı ile yerel ortam arasında tutarlı olduğundan emin olun.
4. CI/CD konfigürasyon dosyalarını kontrol edin:
   ```bash
   # GitHub Actions için
   cat .github/workflows/main.yml
   
   # GitLab CI için
   cat .gitlab-ci.yml
   ```
5. Cache mekanizmalarını temizlemeyi deneyin.

### Deployment Hataları

**Belirti**: Deployment işlemi başarısız oluyor veya uygulama çöküyor.

**Çözüm**:
1. Deployment ortamı değişkenlerinin doğru ayarlandığını kontrol edin.
2. Önceki başarılı deployment'a rollback yapın.
3. Staging ortamında denemeler yapın ve sorunu replike etmeye çalışın.
4. Deployment loglarını kontrol edin:
   ```bash
   # Heroku için
   heroku logs --tail
   
   # AWS Elastic Beanstalk için
   eb logs
   ```
5. Blue-green deployment veya canary deployment stratejileri kullanarak riski azaltın.

## Hata Ayıklama Teknikleri

### Loglar ve İzleme

1. **Yapılandırılmış Logging**: Winston veya Pino gibi yapılandırılmış log kütüphaneleri kullanın:
   ```javascript
   const logger = winston.createLogger({
     level: 'info',
     format: winston.format.json(),
     defaultMeta: { service: 'user-service' },
     transports: [
       new winston.transports.File({ filename: 'error.log', level: 'error' }),
       new winston.transports.File({ filename: 'combined.log' })
     ]
   });
   ```

2. **İzleme ve Metrik Toplama**: Prometheus, Grafana, New Relic veya Datadog kullanın.

3. **Hata İzleme**: Sentry veya Rollbar gibi hata izleme araçlarını entegre edin:
   ```javascript
   Sentry.init({
     dsn: 'https://your-dsn@sentry.io/project',
     environment: process.env.NODE_ENV,
     tracesSampleRate: 0.1
   });
   ```

### Debug Yöntemleri

1. **Node.js Debug**: Node.js inspect modunu kullanın:
   ```bash
   node --inspect server.js
   # Chrome'da chrome://inspect adresine gidin
   ```

2. **React DevTools**: React bileşenlerini analiz etmek için Chrome veya Firefox eklentisi.

3. **Network Analizi**: Chrome veya Firefox DevTools'un Network sekmesini kullanarak API isteklerini inceleyin.

4. **Conditional Debugging**: Kod içine koşullu log satırları ekleyin:
   ```javascript
   if (user.id === 'problematic-user') {
     console.log('Problematic user details:', user);
   }
   ```

## Destek ve Yardım Alma

### İç Kaynaklar

1. **Dokümantasyon**: NexFit dokümantasyonunu ve kod yorumlarını inceleyin.

2. **Git Geçmişi**: Problem yaşanan kodun son değişikliklerini git log ve git blame ile kontrol edin:
   ```bash
   git log -p src/components/ProblemFile.js
   git blame src/components/ProblemFile.js
   ```

3. **Takım İletişimi**: Slack veya Microsoft Teams'de ilgili kanallarda soruları paylaşın.

### Dış Kaynaklar

1. **GitHub Issues**: Proje bağımlılıklarının GitHub sayfalarındaki Issues bölümlerini kontrol edin.

2. **Stack Overflow**: Benzer sorunlar için Stack Overflow'da arama yapın.

3. **Topluluk Forumları**: React, React Native, Node.js veya diğer kullanılan teknolojiler için topluluk forumlarını kullanın.

4. **Destek İletişimi**:
   - Teknik Destek E-posta: [tech-support@nexfit.io](mailto:tech-support@nexfit.io)
   - Geliştirici Slack Kanalı: #nexfit-dev-support

---

Bu rehber, NexFit uygulamasında karşılaşabileceğiniz yaygın sorunları çözmenize yardımcı olmak için oluşturulmuştur. Rehberde bulunmayan bir sorunla karşılaşırsanız, lütfen geliştirici ekibiyle iletişime geçin veya bu rehbere katkıda bulunun.

Son Güncelleme: 23 Mart 2023 