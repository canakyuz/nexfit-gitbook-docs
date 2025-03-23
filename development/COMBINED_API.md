# 🔌 NexFit Kapsamlı API Dokümantasyonu

*Bu doküman, API.md ve API_DOCUMENTATION.md dosyalarının kapsamlı bir şekilde birleştirilmiş tam referans dokümanıdır.*

## İçindekiler

- [Genel Bilgiler](#genel-bilgiler)
- [Kimlik Doğrulama](#kimlik-doğrulama)
- [API Endpoint'leri](#api-endpointleri)
  - [Kullanıcı Yönetimi](#kullanıcı-yönetimi)
  - [Antrenman Yönetimi](#antrenman-yönetimi)
  - [Beslenme Yönetimi](#beslenme-yönetimi)
  - [İşletme Yönetimi](#i̇şletme-yönetimi)
  - [İçerik Yönetimi](#i̇çerik-yönetimi)
  - [Analitik Hizmetleri](#analitik-hizmetleri)
  - [Bildirim Yönetimi](#bildirim-yönetimi)
- [İstek ve Yanıt Formatları](#i̇stek-ve-yanıt-formatları)
- [Hata İşleme](#hata-i̇şleme)
- [Rate Limiting](#rate-limiting)
- [Sürüm Yönetimi](#sürüm-yönetimi)
- [Veri Modelleri](#veri-modelleri)
- [Webhook'lar](#webhooklar)
- [API İstemcileri](#api-i̇stemcileri)
- [SSS](#sss)
- [Örnek Kullanımlar](#örnek-kullanımlar)

## Genel Bilgiler

### API Endpoint Ana URL'leri

- **Production**: `https://api.nexfit.io/v1`
- **Staging**: `https://api-staging.nexfit.io/v1`
- **Development**: `https://api-dev.nexfit.io/v1` veya `http://localhost:3000/v1`

### İstek Başlıkları

Her API isteği şu başlıkları içermelidir:

```
Content-Type: application/json
Accept: application/json
Authorization: Bearer {JWT_TOKEN}
```

### Yanıt Formatı

Tüm API yanıtları aşağıdaki standart JSON formatında döner:

```json
{
  "success": true,
  "data": {},
  "message": "İşlem başarılı",
  "pagination": {
    "totalItems": 100,
    "totalPages": 5,
    "currentPage": 1,
    "pageSize": 20
  }
}
```

Hata durumunda:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Hata mesajı",
    "details": {}
  }
}
```

## Kimlik Doğrulama

NexFit API'si, JWT (JSON Web Token) tabanlı kimlik doğrulama kullanır.

### Giriş

```
POST /auth/login
```

**İstek:**

```json
{
  "email": "kullanici@example.com",
  "password": "sifre123"
}
```

**Yanıt:**

```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGc...",
    "refreshToken": "eyJhbGc...",
    "expiresIn": 3600,
    "user": {
      "id": "user123",
      "name": "Ahmet Yılmaz",
      "email": "kullanici@example.com",
      "role": "CLIENT"
    }
  },
  "message": "Giriş başarılı"
}
```

### Token Yenileme

```
POST /auth/refresh
```

**İstek:**

```json
{
  "refreshToken": "eyJhbGc..."
}
```

**Yanıt:**

```json
{
  "success": true,
  "data": {
    "accessToken": "yeni-token...",
    "expiresIn": 3600
  },
  "message": "Token yenilendi"
}
```

## API Endpoint'leri

### Kullanıcı Yönetimi

#### Kullanıcı Profili Alma

```
GET /users/me
```

**Yanıt:**

```json
{
  "success": true,
  "data": {
    "id": "user123",
    "name": "Ahmet Yılmaz",
    "email": "kullanici@example.com",
    "phone": "+905321234567",
    "role": "CLIENT",
    "profileImage": "https://nexfit.io/images/profile123.jpg",
    "createdAt": "2023-01-15T10:30:00Z",
    "updatedAt": "2023-03-20T15:45:00Z"
  },
  "message": "Kullanıcı profili alındı"
}
```

#### Kullanıcı Profili Güncelleme

```
PATCH /users/me
```

**İstek:**

```json
{
  "name": "Ahmet Mehmet Yılmaz",
  "phone": "+905321234568",
  "profileImage": "base64://..."
}
```

**Yanıt:**

```json
{
  "success": true,
  "data": {
    "id": "user123",
    "name": "Ahmet Mehmet Yılmaz",
    "email": "kullanici@example.com",
    "phone": "+905321234568",
    "profileImage": "https://nexfit.io/images/profile123_updated.jpg",
    "updatedAt": "2023-04-10T09:15:00Z"
  },
  "message": "Kullanıcı profili güncellendi"
}
```

### Antrenman Yönetimi

#### Antrenman Programları Listeleme

```
GET /training/programs?page=1&limit=10
```

**Yanıt:**

```json
{
  "success": true,
  "data": [
    {
      "id": "program123",
      "title": "Kuvvet Antrenmanı Programı",
      "description": "8 haftalık kuvvet gelişimi programı",
      "level": "INTERMEDIATE",
      "duration": "8_WEEKS",
      "category": "STRENGTH",
      "thumbnailUrl": "https://nexfit.io/images/program123.jpg",
      "creatorId": "coach456",
      "creatorName": "Mehmet Koç",
      "price": 299.99,
      "currency": "TRY",
      "rating": 4.7,
      "reviewCount": 123,
      "createdAt": "2023-02-10T12:00:00Z"
    },
    // Diğer programlar...
  ],
  "message": "Antrenman programları listelendi",
  "pagination": {
    "totalItems": 45,
    "totalPages": 5,
    "currentPage": 1,
    "pageSize": 10
  }
}
```

#### Antrenman Programı Detayı

```
GET /training/programs/{programId}
```

**Yanıt:**

```json
{
  "success": true,
  "data": {
    "id": "program123",
    "title": "Kuvvet Antrenmanı Programı",
    "description": "8 haftalık kuvvet gelişimi programı",
    "level": "INTERMEDIATE",
    "duration": "8_WEEKS",
    "category": "STRENGTH",
    "thumbnailUrl": "https://nexfit.io/images/program123.jpg",
    "creatorId": "coach456",
    "creatorName": "Mehmet Koç",
    "price": 299.99,
    "currency": "TRY",
    "rating": 4.7,
    "reviewCount": 123,
    "weeks": [
      {
        "weekNumber": 1,
        "title": "Adaptasyon Haftası",
        "description": "Vücudu antrenmanlara hazırlama",
        "workouts": [
          {
            "id": "workout789",
            "title": "Pazartesi - Göğüs ve Triceps",
            "exercises": [
              {
                "id": "exercise1",
                "name": "Bench Press",
                "sets": 4,
                "reps": "8-12",
                "restSeconds": 90,
                "videoUrl": "https://nexfit.io/videos/bench_press.mp4",
                "instructions": "Sırt düz, dirsekler 90 derece..."
              },
              // Diğer egzersizler...
            ]
          },
          // Diğer antrenmanlar...
        ]
      },
      // Diğer haftalar...
    ],
    "createdAt": "2023-02-10T12:00:00Z",
    "updatedAt": "2023-03-15T10:30:00Z"
  },
  "message": "Program detayları alındı"
}
```

#### Antrenman Kaydı Oluşturma

```
POST /training/logs
```

**İstek:**

```json
{
  "date": "2023-04-15",
  "workoutId": "workout789",
  "duration": 65,
  "exercises": [
    {
      "exerciseId": "exercise1",
      "sets": [
        {
          "weight": 70,
          "reps": 10,
          "completed": true
        },
        {
          "weight": 75,
          "reps": 8,
          "completed": true
        },
        {
          "weight": 75,
          "reps": 7,
          "completed": true
        },
        {
          "weight": 70,
          "reps": 8,
          "completed": true
        }
      ]
    },
    // Diğer egzersizler...
  ],
  "notes": "Güçlü hissettim, göğüs bölgesinde iyi bir pompa oldu."
}
```

**Yanıt:**

```json
{
  "success": true,
  "data": {
    "id": "log456",
    "date": "2023-04-15",
    "workoutId": "workout789",
    "workoutTitle": "Pazartesi - Göğüs ve Triceps",
    "duration": 65,
    "exercises": [
      {
        "exerciseId": "exercise1",
        "exerciseName": "Bench Press",
        "sets": [
          {
            "setNumber": 1,
            "weight": 70,
            "reps": 10,
            "completed": true
          },
          // Diğer setler...
        ]
      },
      // Diğer egzersizler...
    ],
    "notes": "Güçlü hissettim, göğüs bölgesinde iyi bir pompa oldu.",
    "createdAt": "2023-04-15T18:30:00Z"
  },
  "message": "Antrenman kaydı oluşturuldu"
}
```

### Beslenme Yönetimi

#### Yemek Günlüğü Girdisi Oluşturma

```
POST /nutrition/food-logs
```

**İstek:**

```json
{
  "date": "2023-04-15",
  "mealType": "BREAKFAST",
  "foods": [
    {
      "foodId": "food123",
      "name": "Yulaf",
      "quantity": 100,
      "unit": "g",
      "calories": 389,
      "proteins": 16.9,
      "carbs": 66.3,
      "fats": 6.9,
      "fiber": 10.6
    },
    {
      "foodId": "food456",
      "name": "Muz",
      "quantity": 1,
      "unit": "adet",
      "calories": 105,
      "proteins": 1.3,
      "carbs": 27,
      "fats": 0.3,
      "fiber": 3.1
    }
  ],
  "notes": "Antrenmandan önce"
}
```

**Yanıt:**

```json
{
  "success": true,
  "data": {
    "id": "meal789",
    "date": "2023-04-15",
    "mealType": "BREAKFAST",
    "totalCalories": 494,
    "totalProteins": 18.2,
    "totalCarbs": 93.3,
    "totalFats": 7.2,
    "totalFiber": 13.7,
    "foods": [
      // Yemek detayları...
    ],
    "notes": "Antrenmandan önce",
    "createdAt": "2023-04-15T07:30:00Z"
  },
  "message": "Yemek günlüğü kaydı oluşturuldu"
}
```

#### Günlük Beslenme Özeti

```
GET /nutrition/daily-summary?date=2023-04-15
```

**Yanıt:**

```json
{
  "success": true,
  "data": {
    "date": "2023-04-15",
    "target": {
      "calories": 2500,
      "proteins": 150,
      "carbs": 250,
      "fats": 83
    },
    "actual": {
      "calories": 2340,
      "proteins": 145,
      "carbs": 220,
      "fats": 80,
      "fiber": 35,
      "water": 2.5
    },
    "meals": [
      {
        "mealType": "BREAKFAST",
        "totalCalories": 494,
        "foods": [
          // Yemek detayları...
        ]
      },
      // Diğer öğünler...
    ],
    "progress": {
      "caloriesPercentage": 93.6,
      "proteinsPercentage": 96.7,
      "carbsPercentage": 88,
      "fatsPercentage": 96.4
    }
  },
  "message": "Günlük beslenme özeti alındı"
}
```

### İşletme Yönetimi

#### Spor Salonu Üyeleri Listeleme (GYM_OWNER)

```
GET /business/members?page=1&limit=20&status=ACTIVE
```

**Yanıt:**

```json
{
  "success": true,
  "data": [
    {
      "id": "member123",
      "name": "Ali Veli",
      "email": "ali.veli@example.com",
      "phone": "+905551234567",
      "membershipType": "PREMIUM",
      "startDate": "2023-01-10",
      "endDate": "2023-07-10",
      "status": "ACTIVE",
      "paymentStatus": "PAID",
      "lastCheckIn": "2023-04-14T18:30:00Z",
      "totalCheckIns": 65
    },
    // Diğer üyeler...
  ],
  "message": "Üyeler listelendi",
  "pagination": {
    "totalItems": 156,
    "totalPages": 8,
    "currentPage": 1,
    "pageSize": 20
  }
}
```

#### Antrenör Danışanlarını Listeleme (COACH)

```
GET /business/clients?page=1&limit=20&status=ACTIVE
```

**Yanıt:**

```json
{
  "success": true,
  "data": [
    {
      "id": "client123",
      "name": "Ayşe Demir",
      "email": "ayse.demir@example.com",
      "phone": "+905361234567",
      "programId": "program456",
      "programName": "Kilo Verme Programı",
      "startDate": "2023-02-15",
      "endDate": "2023-05-15",
      "lastTrainingDate": "2023-04-13",
      "progress": 65,
      "status": "ACTIVE",
      "notes": "Sol omzunda hafif sakatlık var"
    },
    // Diğer danışanlar...
  ],
  "message": "Danışanlar listelendi",
  "pagination": {
    "totalItems": 35,
    "totalPages": 2,
    "currentPage": 1,
    "pageSize": 20
  }
}
```

### İçerik Yönetimi

#### Blog Yazıları Listeleme

```
GET /content/articles?page=1&limit=10&category=NUTRITION
```

**Yanıt:**

```json
{
  "success": true,
  "data": [
    {
      "id": "article123",
      "title": "Protein Alımınızı Optimize Etmenin 5 Yolu",
      "slug": "protein-aliminizi-optimize-etmenin-5-yolu",
      "excerpt": "Kas gelişimi için protein alımını nasıl optimize edebileceğinizi öğrenin.",
      "category": "NUTRITION",
      "tags": ["protein", "muscle-building", "diet"],
      "coverImage": "https://nexfit.io/images/articles/protein-optimization.jpg",
      "author": {
        "id": "author456",
        "name": "Dr. Fatma Şahin",
        "title": "Beslenme Uzmanı",
        "avatarUrl": "https://nexfit.io/images/authors/fatma-sahin.jpg"
      },
      "publishedAt": "2023-03-12T10:00:00Z",
      "readTime": 6
    },
    // Diğer yazılar...
  ],
  "message": "Makaleler listelendi",
  "pagination": {
    "totalItems": 48,
    "totalPages": 5,
    "currentPage": 1,
    "pageSize": 10
  }
}
```

#### Blog Yazısı Detayı

```
GET /content/articles/{articleId}
```

**Yanıt:**

```json
{
  "success": true,
  "data": {
    "id": "article123",
    "title": "Protein Alımınızı Optimize Etmenin 5 Yolu",
    "slug": "protein-aliminizi-optimize-etmenin-5-yolu",
    "content": "# Protein Alımınızı Optimize Etmenin 5 Yolu\n\nProtein, kas yapımının...",
    "contentFormat": "markdown",
    "category": "NUTRITION",
    "tags": ["protein", "muscle-building", "diet"],
    "coverImage": "https://nexfit.io/images/articles/protein-optimization.jpg",
    "author": {
      "id": "author456",
      "name": "Dr. Fatma Şahin",
      "title": "Beslenme Uzmanı",
      "bio": "10 yıldan fazla deneyime sahip beslenme uzmanı...",
      "avatarUrl": "https://nexfit.io/images/authors/fatma-sahin.jpg"
    },
    "publishedAt": "2023-03-12T10:00:00Z",
    "updatedAt": "2023-03-14T15:30:00Z",
    "readTime": 6,
    "viewCount": 2345,
    "relatedArticles": [
      // İlgili makaleler...
    ]
  },
  "message": "Makale detayı alındı"
}
```

### Analitik Hizmetleri

#### Fitness İlerleme Raporu

```
GET /analytics/fitness-progress?startDate=2023-01-01&endDate=2023-04-15
```

**Yanıt:**

```json
{
  "success": true,
  "data": {
    "period": {
      "startDate": "2023-01-01",
      "endDate": "2023-04-15"
    },
    "measurements": {
      "weight": [
        {
          "date": "2023-01-01",
          "value": 82.5
        },
        {
          "date": "2023-02-01",
          "value": 80.2
        },
        {
          "date": "2023-03-01",
          "value": 78.6
        },
        {
          "date": "2023-04-01",
          "value": 77.3
        }
      ],
      "bodyFat": [
        {
          "date": "2023-01-01",
          "value": 24.5
        },
        {
          "date": "2023-04-01",
          "value": 21.2
        }
      ],
      // Diğer ölçümler...
    },
    "performance": {
      "benchPress": [
        {
          "date": "2023-01-15",
          "value": 80
        },
        {
          "date": "2023-04-10",
          "value": 95
        }
      ],
      // Diğer performans ölçümleri...
    },
    "activitySummary": {
      "totalWorkouts": 45,
      "totalDuration": 3240,
      "averageWorkoutDuration": 72,
      "workoutFrequency": 3.8,
      "categoryBreakdown": {
        "STRENGTH": 60,
        "CARDIO": 25,
        "FLEXIBILITY": 15
      }
    }
  },
  "message": "Fitness ilerleme raporu alındı"
}
```

#### İşletme Performans Raporu (GYM_OWNER)

```
GET /analytics/business-performance?period=LAST_30_DAYS
```

**Yanıt:**

```json
{
  "success": true,
  "data": {
    "period": {
      "start": "2023-03-16",
      "end": "2023-04-15"
    },
    "memberships": {
      "active": 345,
      "new": 28,
      "expired": 15,
      "churnRate": 4.3,
      "renewalRate": 86.7
    },
    "revenue": {
      "total": 86450,
      "recurring": 75300,
      "oneTime": 11150,
      "currency": "TRY",
      "comparisonToPreviousPeriod": 5.2
    },
    "attendance": {
      "total": 4230,
      "average": 141,
      "peakHours": [
        {
          "hour": 18,
          "count": 560
        },
        {
          "hour": 19,
          "count": 620
        },
        {
          "hour": 8,
          "count": 480
        }
      ],
      "byDay": {
        "MONDAY": 720,
        "TUESDAY": 680,
        // Diğer günler...
      }
    },
    "classes": {
      "total": 210,
      "averageAttendance": 12.4,
      "mostPopular": [
        {
          "name": "Yoga",
          "attendance": 96.5
        },
        {
          "name": "HIIT",
          "attendance": 92.3
        },
        {
          "name": "Spinning",
          "attendance": 90.1
        }
      ]
    }
  },
  "message": "İşletme performans raporu alındı"
}
```

### Bildirim Yönetimi

#### Kullanıcı Bildirimleri Listeleme

```
GET /notifications?page=1&limit=10
```

**Yanıt:**

```json
{
  "success": true,
  "data": [
    {
      "id": "notification123",
      "title": "Yeni antrenman programı eklendi",
      "message": "Yeni bir antrenman programı eklendi. Lütfen programı inceleyin.",
      "createdAt": "2023-04-15T10:00:00Z"
    },
    // Diğer bildirimler...
  ],
  "message": "Bildirimler listelendi",
  "pagination": {
    "totalItems": 45,
    "totalPages": 5,
    "currentPage": 1,
    "pageSize": 10
  }
}
```

## İstek ve Yanıt Formatları

### Filtreleme

Çoğu liste endpoint'i, sonuçları filtrelemek için query parametreleri destekler:

```
GET /training/programs?category=STRENGTH&level=INTERMEDIATE&minRating=4
```

### Sıralama

Sonuçları sıralamak için `sort` parametresi kullanılabilir:

```
GET /training/programs?sort=rating:desc,createdAt:desc
```

### Sayfalama

Tüm liste endpoint'leri sayfalama desteği sunar:

```
GET /training/programs?page=2&limit=20
```

## Hata İşleme

Hata durumunda, API aşağıdaki formatta bir yanıt döndürür:

```json
{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "Belirtilen program bulunamadı",
    "details": {
      "resourceId": "program123",
      "resourceType": "TrainingProgram"
    }
  }
}
```

### Genel Hata Kodları

| Kod | HTTP Status | Açıklama |
|-----|-------------|----------|
| `INVALID_REQUEST` | 400 | İstek formatı veya parametreleri geçersiz |
| `AUTHENTICATION_REQUIRED` | 401 | Kimlik doğrulama gerekli |
| `PERMISSION_DENIED` | 403 | Yetkisiz erişim |
| `RESOURCE_NOT_FOUND` | 404 | İstenen kaynak bulunamadı |
| `VALIDATION_ERROR` | 422 | Veri doğrulama hatası |
| `RATE_LIMIT_EXCEEDED` | 429 | İstek limiti aşıldı |
| `INTERNAL_SERVER_ERROR` | 500 | Sunucu hatası |

## Rate Limiting

API, istemcilerin aşırı isteklerini önlemek için rate limiting uygular. Rate limit bilgileri yanıt başlıklarında döndürülür:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 98
X-RateLimit-Reset: 1682086400
```

## Sürüm Yönetimi

API, URL yolunda sürüm belirtimi kullanır:

```
https://api.nexfit.io/v1/users/me
```

Büyük değişiklikler yapıldığında yeni sürümler oluşturulur. Eski sürümler uygun bir süre boyunca desteklenmeye devam eder.

## Veri Modelleri

API'nin kullandığı veri modelleri hakkında bilgi için [Veri Modelleri](#veri-modelleri) bölümüne bakınız.

## Webhook'lar

API, belirli olaylar meydana geldiğinde webhook aracılığıyla bildirim göndermeyi destekler. Webhook bilgileri için [Webhook'lar](#webhooklar) bölümüne bakınız.

## API İstemcileri

API'nin farklı programlama dilleri ve platformlar için örnek istemcileri için [API İstemcileri](#api-i̇stemcileri) bölümüne bakınız.

## SSS

API hakkında sıkça sorulan sorular için [SSS](#sss) bölümüne bakınız.

## Örnek Kullanımlar

### cURL ile Login

```bash
curl -X POST "https://api.nexfit.io/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "kullanici@example.com", 
    "password": "sifre123"
  }'
```

### JavaScript ile Antrenman Kaydetme

```javascript
const token = 'eyJhbGc...';

fetch('https://api.nexfit.io/v1/training/logs', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  },
  body: JSON.stringify({
    date: '2023-04-15',
    workoutId: 'workout789',
    duration: 65,
    exercises: [
      {
        exerciseId: 'exercise1',
        sets: [
          { weight: 70, reps: 10, completed: true },
          { weight: 75, reps: 8, completed: true },
          // Diğer setler...
        ]
      },
      // Diğer egzersizler...
    ],
    notes: 'Güçlü hissettim, göğüs bölgesinde iyi bir pompa oldu.'
  })
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

---

Bu API dokümantasyonu, NexFit platformunun API kullanımını anlamanıza yardımcı olmak için hazırlanmıştır. Sorularınız veya geri bildirimleriniz için [api-support@nexfit.io](mailto:api-support@nexfit.io) adresine e-posta gönderebilirsiniz.

Son Güncelleme: 15 Mart 2023 