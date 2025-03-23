# 🔄 NexFit Entegrasyon Rehberi

Bu belge, NexFit uygulamasıyla entegre olmak isteyen geliştiriciler için kapsamlı bir kılavuzdur. NexFit'in diğer sistemler, üçüncü taraf servisler ve uygulamalarla nasıl entegre edileceğini detaylı olarak açıklar.

## İçindekiler

- [Genel Bakış](#genel-bakış)
- [API Entegrasyonu](#api-entegrasyonu)
- [Kimlik Doğrulama](#kimlik-doğrulama)
- [Webhook Entegrasyonları](#webhook-entegrasyonları)
- [Mobil Uygulama Entegrasyonu](#mobil-uygulama-entegrasyonu)
- [Ödeme Sistemi Entegrasyonu](#ödeme-sistemi-entegrasyonu)
- [Sosyal Medya Entegrasyonu](#sosyal-medya-entegrasyonu)
- [Cihaz ve Wearable Entegrasyonu](#cihaz-ve-wearable-entegrasyonu)
- [Üçüncü Taraf Servis Entegrasyonları](#üçüncü-taraf-servis-entegrasyonları)
- [SSS ve Sorun Giderme](#sss-ve-sorun-giderme)

## Genel Bakış

NexFit, çeşitli entegrasyon noktaları sunan modern bir fitness ve sağlık yönetim platformudur. Entegrasyon türleri şunları içerir:

- **API Entegrasyonu**: RESTful API ve GraphQL API
- **Webhook Entegrasyonları**: Gerçek zamanlı olay bildirimleri
- **Single Sign-On (SSO)**: OAuth 2.0 ve OpenID Connect
- **Mobil Entegrasyonlar**: iOS ve Android SDK'ları
- **Cihaz Entegrasyonları**: Fitness cihazları ve giyilebilir teknolojiler
- **Üçüncü Taraf Entegrasyonları**: Ödeme, beslenme takibi, sosyal medya

## API Entegrasyonu

### RESTful API

NexFit, HTTP protokolü üzerine kurulmuş RESTful API sunar.

#### Temel Endpoint Yapısı

```
https://api.nexfit.com/v1/{resource}
```

#### Örnek Endpoint'ler

| HTTP Metodu | Endpoint | Açıklama |
|-------------|----------|----------|
| GET | `/users` | Kullanıcı listesini getirir |
| GET | `/users/{id}` | Belirli bir kullanıcının detaylarını getirir |
| POST | `/users` | Yeni kullanıcı oluşturur |
| PUT | `/users/{id}` | Kullanıcı bilgilerini günceller |
| DELETE | `/users/{id}` | Kullanıcıyı siler |
| GET | `/programs` | Antrenman programlarını listeler |
| GET | `/programs/{id}/workouts` | Program içindeki antrenmanları getirir |

#### API İstek Örneği

```bash
curl -X GET \
  "https://api.nexfit.com/v1/users/123" \
  -H "Authorization: Bearer your_access_token" \
  -H "Content-Type: application/json"
```

#### API Yanıt Örneği

```json
{
  "id": "123",
  "firstName": "Ahmet",
  "lastName": "Yılmaz",
  "email": "ahmet@example.com",
  "profilePicture": "https://nexfit.com/images/profiles/123.jpg",
  "membershipType": "premium",
  "createdAt": "2023-01-15T10:30:00Z",
  "updatedAt": "2023-03-10T15:45:22Z"
}
```

### GraphQL API

Daha esnek veri sorgulaması için GraphQL API de sunuyoruz.

#### GraphQL Endpoint

```
https://api.nexfit.com/graphql
```

#### Örnek GraphQL Sorgusu

```graphql
query {
  user(id: "123") {
    id
    firstName
    lastName
    email
    workouts(last: 5) {
      id
      name
      date
      duration
      exercises {
        name
        sets
        reps
        weight
      }
    }
  }
}
```

### API İstek Limitleri

| Plan | Dakikada İstek | Günlük İstek |
|------|---------------|-------------|
| Ücretsiz | 60 | 10,000 |
| Basic | 120 | 50,000 |
| Premium | 300 | 200,000 |
| Enterprise | Özelleştirilmiş | Özelleştirilmiş |

### API Versiyonlama

- API yolu içinde versiyon belirtme: `/v1/`, `/v2/` vb.
- Büyük değişiklikler yeni bir versiyon numarası gerektirir
- Eski API versiyonları en az 12 ay boyunca desteklenir

## Kimlik Doğrulama

### OAuth 2.0 Entegrasyonu

NexFit, kimlik doğrulama için OAuth 2.0 standardını kullanır.

#### OAuth 2.0 Akış Türleri

- **Authorization Code Flow**: Web uygulamaları için
- **Implicit Flow**: Tek sayfalık uygulamalar (SPA) için
- **Client Credentials Flow**: Sunucu-sunucu iletişimi için
- **Resource Owner Password Flow**: Birinci taraf uygulamalar için

#### OAuth 2.0 Endpoint'leri

| Endpoint | URL |
|----------|-----|
| Yetkilendirme | `https://auth.nexfit.com/oauth/authorize` |
| Token | `https://auth.nexfit.com/oauth/token` |
| Token Yenileme | `https://auth.nexfit.com/oauth/token` |
| Token İptal | `https://auth.nexfit.com/oauth/revoke` |

#### Örnek OAuth 2.0 Akışı

1. Kullanıcıyı yetkilendirme endpoint'ine yönlendir:

```
https://auth.nexfit.com/oauth/authorize?
  response_type=code&
  client_id=YOUR_CLIENT_ID&
  redirect_uri=https://your-app.com/callback&
  scope=user.read+workouts.read&
  state=RANDOM_STATE_VALUE
```

2. Yetkilendirme kodunu token'a dönüştür:

```bash
curl -X POST \
  https://auth.nexfit.com/oauth/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=authorization_code&
      code=AUTHORIZATION_CODE&
      client_id=YOUR_CLIENT_ID&
      client_secret=YOUR_CLIENT_SECRET&
      redirect_uri=https://your-app.com/callback"
```

3. Token yanıtı:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "def50200641f3e...",
  "scope": "user.read workouts.read"
}
```

### API Anahtarları

Basit entegrasyonlar için API anahtarı kimlik doğrulaması da desteklenmektedir.

```bash
curl -X GET \
  "https://api.nexfit.com/v1/programs" \
  -H "X-API-Key: your_api_key"
```

## Webhook Entegrasyonları

NexFit'in webhook sistemi, uygulamanızı belirli olaylara abone olmanıza ve gerçek zamanlı güncellemeler almanıza olanak tanır.

### Desteklenen Webhook Olayları

| Olay | Açıklama |
|------|----------|
| `user.created` | Yeni kullanıcı kaydedildiğinde |
| `user.updated` | Kullanıcı profili güncellendiğinde |
| `workout.created` | Yeni antrenman oluşturulduğunda |
| `workout.completed` | Antrenman tamamlandığında |
| `subscription.created` | Yeni abonelik oluşturulduğunda |
| `subscription.renewed` | Abonelik yenilendiğinde |
| `subscription.cancelled` | Abonelik iptal edildiğinde |
| `payment.succeeded` | Ödeme başarılı olduğunda |
| `payment.failed` | Ödeme başarısız olduğunda |

### Webhook Yapılandırması

Webhook'ları yapılandırmak için:

1. NexFit Geliştirici Portalı'na giriş yapın
2. "Webhook'lar" bölümüne gidin
3. "Yeni Webhook" butonuna tıklayın
4. İstek URL'inizi ve abone olmak istediğiniz olayları belirtin

### Webhook İmzalama

Webhookların güvenliğini sağlamak için, her webhook isteği `X-NexFit-Signature` başlığında bir HMAC imzası içerir. İmza, webhook sırrınız kullanılarak istek gövdesi üzerinde hesaplanır.

```javascript
// Node.js örneği
const crypto = require('crypto');

function verifyWebhookSignature(payload, signature, secret) {
  const hmac = crypto.createHmac('sha256', secret);
  const calculatedSignature = hmac.update(payload).digest('hex');
  return crypto.timingSafeEqual(
    Buffer.from(calculatedSignature, 'hex'),
    Buffer.from(signature, 'hex')
  );
}
```

### Webhook İstek Örneği

```http
POST /your-webhook-endpoint HTTP/1.1
Host: your-server.com
Content-Type: application/json
X-NexFit-Signature: sha256=7682fe915d8b25e155361d9ce679955d598b949fb5e2500facfe893cc51b3df0
X-NexFit-Event: workout.completed

{
  "event": "workout.completed",
  "created_at": "2023-03-12T15:30:22Z",
  "data": {
    "workout_id": "w_12345",
    "user_id": "u_6789",
    "workout_name": "Üst Vücut Güç",
    "completed_at": "2023-03-12T15:30:00Z",
    "duration": 45,
    "calories_burned": 320,
    "exercises": [
      {
        "name": "Bench Press",
        "sets": 3,
        "reps": 10,
        "weight": 70
      },
      {
        "name": "Pull-ups",
        "sets": 3,
        "reps": 8,
        "weight": 0
      }
    ]
  }
}
```

### Webhook Yeniden Deneme Stratejisi

Webhook istekleri başarısız olursa, NexFit aşağıdaki şekilde yeniden dener:

- İlk deneme başarısız olursa, 5 dakika bekle
- İkinci deneme başarısız olursa, 15 dakika bekle
- Üçüncü deneme başarısız olursa, 30 dakika bekle
- Dördüncü deneme başarısız olursa, 2 saat bekle
- Beşinci deneme başarısız olursa, 6 saat bekle
- 24 saat içinde başarılı olamayan webhook'lar için uyarı gönderilir

## Mobil Uygulama Entegrasyonu

### iOS SDK

NexFit iOS SDK'sı, iOS uygulamalarınızı NexFit platformu ile entegre etmenizi sağlar.

#### Kurulum (CocoaPods)

```ruby
pod 'NexFitSDK', '~> 1.0'
```

#### Temel Kullanım

```swift
import NexFitSDK

// SDK'yı başlat
NexFitSDK.initialize(apiKey: "your_api_key")

// Kullanıcı girişi
NexFitSDK.login(email: "user@example.com", password: "password") { result in
    switch result {
    case .success(let user):
        print("Giriş başarılı: \(user.fullName)")
    case .failure(let error):
        print("Giriş hatası: \(error)")
    }
}

// Antrenman listesini al
NexFitSDK.getWorkouts(page: 1, limit: 20) { result in
    switch result {
    case .success(let workouts):
        for workout in workouts {
            print("Antrenman: \(workout.name), Tarih: \(workout.date)")
        }
    case .failure(let error):
        print("Antrenman listesi hatası: \(error)")
    }
}
```

### Android SDK

NexFit Android SDK'sı, Android uygulamalarınızı NexFit platformu ile entegre etmenizi sağlar.

#### Kurulum (Gradle)

```gradle
implementation 'com.nexfit:android-sdk:1.0.0'
```

#### Temel Kullanım

```kotlin
import com.nexfit.sdk.NexFitSDK

// SDK'yı başlat
NexFitSDK.initialize(context, "your_api_key")

// Kullanıcı girişi
NexFitSDK.login("user@example.com", "password", object : NexFitCallback<User> {
    override fun onSuccess(user: User) {
        Log.d("NexFit", "Giriş başarılı: ${user.fullName}")
    }
    
    override fun onError(error: NexFitError) {
        Log.e("NexFit", "Giriş hatası: ${error.message}")
    }
})

// Antrenman listesini al
NexFitSDK.getWorkouts(1, 20, object : NexFitCallback<List<Workout>> {
    override fun onSuccess(workouts: List<Workout>) {
        for (workout in workouts) {
            Log.d("NexFit", "Antrenman: ${workout.name}, Tarih: ${workout.date}")
        }
    }
    
    override fun onError(error: NexFitError) {
        Log.e("NexFit", "Antrenman listesi hatası: ${error.message}")
    }
})
```

## Ödeme Sistemi Entegrasyonu

NexFit, çeşitli ödeme sağlayıcılarıyla entegrasyon sunar.

### Stripe Entegrasyonu

#### Ödeme Akışı

1. Sunucu tarafında ödeme intent'i oluştur:

```javascript
// Node.js örneği
const stripe = require('stripe')('your_stripe_secret_key');

app.post('/create-payment', async (req, res) => {
  const { amount, currency, customer_id } = req.body;
  
  try {
    const paymentIntent = await stripe.paymentIntents.create({
      amount: amount,
      currency: currency,
      customer: customer_id,
      metadata: {
        integration: 'nexfit'
      }
    });
    
    res.json({
      clientSecret: paymentIntent.client_secret
    });
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});
```

2. İstemci tarafında ödemeyi tamamla:

```javascript
// Front-end JavaScript örneği
const stripe = Stripe('your_stripe_publishable_key');
const elements = stripe.elements();

// Ödeme formunu oluştur
const card = elements.create('card');
card.mount('#card-element');

// Ödemeyi gönder
const form = document.getElementById('payment-form');
form.addEventListener('submit', async (event) => {
  event.preventDefault();
  
  const { error, paymentMethod } = await stripe.createPaymentMethod({
    type: 'card',
    card: card,
  });
  
  if (error) {
    console.error(error);
  } else {
    const response = await fetch('/create-payment', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        amount: 1990,  // $19.90
        currency: 'usd',
        customer_id: 'cus_123456'
      }),
    });
    
    const { clientSecret } = await response.json();
    
    const { error: confirmError } = await stripe.confirmCardPayment(clientSecret, {
      payment_method: paymentMethod.id
    });
    
    if (confirmError) {
      console.error(confirmError);
    } else {
      console.log('Ödeme başarılı!');
    }
  }
});
```

### İyzico Entegrasyonu

```php
// PHP örneği
require_once('IyzipayBootstrap.php');

IyzipayBootstrap::init();

$options = new \Iyzipay\Options();
$options->setApiKey('your_api_key');
$options->setSecretKey('your_secret_key');
$options->setBaseUrl('https://sandbox-api.iyzipay.com');

$request = new \Iyzipay\Request\CreatePaymentRequest();
$request->setLocale(\Iyzipay\Model\Locale::TR);
$request->setConversationId('123456789');
$request->setPrice('19.90');
$request->setPaidPrice('19.90');
$request->setCurrency(\Iyzipay\Model\Currency::TL);
$request->setInstallment(1);
$request->setBasketId('B67832');
$request->setPaymentChannel(\Iyzipay\Model\PaymentChannel::WEB);
$request->setPaymentGroup(\Iyzipay\Model\PaymentGroup::PRODUCT);

$payment = \Iyzipay\Model\Payment::create($request, $options);
```

## Sosyal Medya Entegrasyonu

NexFit, kullanıcıların antrenmanlarını ve başarılarını sosyal medyada paylaşmalarına olanak tanır.

### Facebook Entegrasyonu

```javascript
// JavaScript örneği
function shareToFacebook(workoutId) {
  FB.init({
    appId: 'your_facebook_app_id',
    version: 'v13.0'
  });
  
  FB.ui({
    method: 'share',
    href: `https://nexfit.com/workouts/${workoutId}`,
    quote: 'NexFit ile bugünkü antrenmanımı tamamladım!'
  }, function(response){
    if (response && !response.error_message) {
      console.log('Paylaşım başarılı!');
    } else {
      console.error('Paylaşım hatası!');
    }
  });
}
```

### Twitter Entegrasyonu

```javascript
// JavaScript örneği
function shareToTwitter(workoutId, workoutName) {
  const text = encodeURIComponent(`NexFit ile "${workoutName}" antrenmanımı tamamladım!`);
  const url = encodeURIComponent(`https://nexfit.com/workouts/${workoutId}`);
  const hashtags = encodeURIComponent('fitness,nexfit,workout');
  
  window.open(
    `https://twitter.com/intent/tweet?text=${text}&url=${url}&hashtags=${hashtags}`,
    '_blank'
  );
}
```

## Cihaz ve Wearable Entegrasyonu

NexFit, çeşitli fitness cihazları ve giyilebilir teknolojilerle entegre çalışır.

### Apple HealthKit Entegrasyonu

```swift
import HealthKit

class HealthKitManager {
    let healthStore = HKHealthStore()
    
    func requestAuthorization(completion: @escaping (Bool, Error?) -> Void) {
        let typesToRead: Set<HKObjectType> = [
            HKObjectType.quantityType(forIdentifier: .stepCount)!,
            HKObjectType.quantityType(forIdentifier: .activeEnergyBurned)!,
            HKObjectType.quantityType(forIdentifier: .heartRate)!
        ]
        
        healthStore.requestAuthorization(toShare: nil, read: typesToRead, completion: completion)
    }
    
    func getStepCount(completion: @escaping (Double, Error?) -> Void) {
        let type = HKObjectType.quantityType(forIdentifier: .stepCount)!
        let now = Date()
        let startOfDay = Calendar.current.startOfDay(for: now)
        let predicate = HKQuery.predicateForSamples(withStart: startOfDay, end: now, options: .strictStartDate)
        
        let query = HKStatisticsQuery(
            quantityType: type,
            quantitySamplePredicate: predicate,
            options: .cumulativeSum
        ) { _, result, error in
            guard let result = result, let sum = result.sumQuantity() else {
                completion(0.0, error)
                return
            }
            completion(sum.doubleValue(for: HKUnit.count()), nil)
        }
        
        healthStore.execute(query)
    }
}

// Kullanım
let healthKitManager = HealthKitManager()
healthKitManager.requestAuthorization { success, error in
    if success {
        healthKitManager.getStepCount { steps, error in
            if let error = error {
                print("Adım sayısı hatası: \(error)")
            } else {
                print("Bugün atılan adım sayısı: \(steps)")
                
                // NexFit'e adım verilerini gönder
                NexFitSDK.syncSteps(steps: Int(steps), date: Date()) { result in
                    switch result {
                    case .success:
                        print("Adım verileri senkronize edildi")
                    case .failure(let error):
                        print("Adım senkronizasyon hatası: \(error)")
                    }
                }
            }
        }
    } else {
        print("HealthKit yetkilendirme hatası: \(error?.localizedDescription ?? "")")
    }
}
```

### Google Fit Entegrasyonu

```kotlin
// Kotlin örneği
class GoogleFitManager(private val context: Context) {
    private val fitnessOptions = FitnessOptions.builder()
        .addDataType(DataType.TYPE_STEP_COUNT_DELTA, FitnessOptions.ACCESS_READ)
        .addDataType(DataType.TYPE_CALORIES_EXPENDED, FitnessOptions.ACCESS_READ)
        .addDataType(DataType.TYPE_HEART_RATE_BPM, FitnessOptions.ACCESS_READ)
        .build()
        
    fun checkPermissionsAndRun(activity: Activity, requestCode: Int) {
        if (!GoogleSignIn.hasPermissions(GoogleSignIn.getLastSignedInAccount(context), fitnessOptions)) {
            GoogleSignIn.requestPermissions(
                activity,
                requestCode,
                GoogleSignIn.getLastSignedInAccount(context),
                fitnessOptions
            )
        } else {
            accessGoogleFit()
        }
    }
    
    fun accessGoogleFit() {
        val endTime = System.currentTimeMillis()
        val startTime = getStartOfDay().timeInMillis
        
        val readRequest = DataReadRequest.Builder()
            .read(DataType.TYPE_STEP_COUNT_DELTA)
            .setTimeRange(startTime, endTime, TimeUnit.MILLISECONDS)
            .build()
            
        Fitness.getHistoryClient(context, GoogleSignIn.getLastSignedInAccount(context)!!)
            .readData(readRequest)
            .addOnSuccessListener { response ->
                val dataSet = response.getDataSet(DataType.TYPE_STEP_COUNT_DELTA)
                var totalSteps = 0
                
                for (dataPoint in dataSet.dataPoints) {
                    for (field in dataPoint.dataType.fields) {
                        totalSteps += dataPoint.getValue(field).asInt()
                    }
                }
                
                Log.d("GoogleFit", "Toplam adım: $totalSteps")
                
                // NexFit'e adım verilerini gönder
                NexFitSDK.syncSteps(totalSteps, Date(), object : NexFitCallback<Boolean> {
                    override fun onSuccess(result: Boolean) {
                        Log.d("NexFit", "Adım verileri senkronize edildi")
                    }
                    
                    override fun onError(error: NexFitError) {
                        Log.e("NexFit", "Adım senkronizasyon hatası: ${error.message}")
                    }
                })
            }
            .addOnFailureListener { e ->
                Log.e("GoogleFit", "Adım verisi okuma hatası", e)
            }
    }
    
    private fun getStartOfDay(): Calendar {
        val cal = Calendar.getInstance()
        cal.time = Date()
        cal.set(Calendar.HOUR_OF_DAY, 0)
        cal.set(Calendar.MINUTE, 0)
        cal.set(Calendar.SECOND, 0)
        cal.set(Calendar.MILLISECOND, 0)
        return cal
    }
}

// Kullanım
val googleFitManager = GoogleFitManager(context)
googleFitManager.checkPermissionsAndRun(this, REQUEST_OAUTH_REQUEST_CODE)
```

## Üçüncü Taraf Servis Entegrasyonları

### Beslenme Takip Servisleri (MyFitnessPal)

```javascript
// JavaScript örneği - MyFitnessPal API
async function syncMealFromMyFitnessPal(mealId, nexfitUserId) {
  try {
    // MyFitnessPal'dan öğün bilgilerini al
    const response = await fetch(`https://api.myfitnesspal.com/meals/${mealId}`, {
      headers: {
        'Authorization': 'Bearer YOUR_MFP_API_KEY'
      }
    });
    
    const mealData = await response.json();
    
    // NexFit'e öğün verilerini gönder
    const nexfitResponse = await fetch('https://api.nexfit.com/v1/nutrition/meals', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer YOUR_NEXFIT_API_KEY'
      },
      body: JSON.stringify({
        user_id: nexfitUserId,
        name: mealData.name,
        date: mealData.date,
        meal_type: mealData.meal_type,
        foods: mealData.foods.map(food => ({
          name: food.name,
          calories: food.calories,
          protein: food.protein,
          carbs: food.carbs,
          fat: food.fat,
          serving_size: food.serving_size,
          servings: food.servings
        }))
      })
    });
    
    const nexfitResult = await nexfitResponse.json();
    return nexfitResult;
  } catch (error) {
    console.error('MyFitnessPal senkronizasyon hatası:', error);
    throw error;
  }
}
```

### Meditasyon Uygulamaları (Calm)

```javascript
// JavaScript örneği - Calm API
async function syncCalmMeditationSession(sessionId, nexfitUserId) {
  try {
    // Calm'dan meditasyon oturumu bilgilerini al
    const response = await fetch(`https://api.calm.com/sessions/${sessionId}`, {
      headers: {
        'Authorization': 'Bearer YOUR_CALM_API_KEY'
      }
    });
    
    const sessionData = await response.json();
    
    // NexFit'e meditasyon oturumu verilerini gönder
    const nexfitResponse = await fetch('https://api.nexfit.com/v1/wellness/meditation', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer YOUR_NEXFIT_API_KEY'
      },
      body: JSON.stringify({
        user_id: nexfitUserId,
        name: sessionData.name,
        duration: sessionData.duration,
        date: sessionData.date,
        type: sessionData.type,
        source: 'Calm'
      })
    });
    
    const nexfitResult = await nexfitResponse.json();
    return nexfitResult;
  } catch (error) {
    console.error('Calm senkronizasyon hatası:', error);
    throw error;
  }
}
```

## SSS ve Sorun Giderme

### Sık Sorulan Sorular

#### API istek limitlerimi nasıl izleyebilirim?

Yanıt başlıklarında şu bilgiler yer alır:
```
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1678637580
```

#### OAuth token'ım geçersiz hale geldi. Ne yapmalıyım?

Refresh token kullanarak yeni bir access token alabilirsiniz:

```bash
curl -X POST \
  https://auth.nexfit.com/oauth/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=refresh_token&
      refresh_token=YOUR_REFRESH_TOKEN&
      client_id=YOUR_CLIENT_ID&
      client_secret=YOUR_CLIENT_SECRET"
```

#### WebSocket bağlantısı için nasıl kimlik doğrulaması yaparım?

WebSocket URL'ine token parametresi ekleyin:

```javascript
const socket = new WebSocket('wss://ws.nexfit.com?token=YOUR_ACCESS_TOKEN');
```

#### Webhook imzası nasıl doğrulanır?

Webhook isteğinin gövdesini ve X-NexFit-Signature başlığındaki imzayı kullanarak aşağıdaki işlemi gerçekleştirin:

```php
// PHP örneği
function verifyWebhookSignature($payload, $signature, $secret) {
    $calculatedSignature = hash_hmac('sha256', $payload, $secret);
    return hash_equals('sha256=' . $calculatedSignature, $signature);
}

$payload = file_get_contents('php://input');
$signature = $_SERVER['HTTP_X_NEXFIT_SIGNATURE'];
$secret = 'your_webhook_secret';

if (verifyWebhookSignature($payload, $signature, $secret)) {
    // İmza doğru, webhook'u işle
} else {
    // İmza hatalı, istek reddedilmeli
    http_response_code(401);
    exit;
}
```

### Yaygın Hata Kodları

| Kod | İsim | Açıklama |
|-----|------|----------|
| 400 | Bad Request | İstek formatı veya verileri geçersiz |
| 401 | Unauthorized | Kimlik doğrulama başarısız |
| 403 | Forbidden | Yetkilendirme başarısız |
| 404 | Not Found | İstenen kaynak bulunamadı |
| 429 | Too Many Requests | İstek limiti aşıldı |
| 500 | Internal Server Error | Sunucu hatası |

### API Hata Yanıt Formatı

```json
{
  "error": {
    "code": "invalid_parameters",
    "message": "Geçersiz parametreler sağlandı.",
    "details": [
      {
        "field": "email",
        "message": "Geçerli bir e-posta adresi olmalıdır."
      },
      {
        "field": "password",
        "message": "Şifre en az 8 karakter olmalıdır."
      }
    ]
  }
}
```

### Sorun Giderme İçin Log Birleştirme

Sorun giderme için, isteklerinize bir korelasyon kimliği ekleyin:

```bash
curl -X GET \
  "https://api.nexfit.com/v1/users/123" \
  -H "Authorization: Bearer your_access_token" \
  -H "X-Correlation-ID: abc-123-xyz"
```

Bu kimlik, tüm sistem bileşenlerinde günlükleri birleştirmek için kullanılır. Destek talebi oluştururken bu kimliği paylaşın.

---

Bu belge, NexFit uygulamasıyla entegrasyon sağlamak isteyen geliştiriciler için kapsamlı bir kılavuz sunmaktadır. Daha fazla teknik destek için lütfen [geliştirici portalımızı](https://developers.nexfit.com) ziyaret edin veya [destek@nexfit.com](mailto:destek@nexfit.com) adresine e-posta gönderin.

Son Güncelleme: 18 Mart 2023 