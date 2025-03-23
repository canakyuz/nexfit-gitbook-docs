# NexFit Veri Modeli

Bu dokümanda NexFit uygulamasının veritabanı yapısı ve veri modelleri detaylandırılmıştır. MongoDB doküman-tabanlı veritabanı kullanılmaktadır ve modeller arasındaki ilişkiler referanslar ile sağlanmaktadır.

## Veritabanı Mimarisi

NexFit veritabanı aşağıdaki temel prensiplere dayanmaktadır:

- **Modüler Tasarım:** Her işlevsellik için ayrı koleksiyonlar
- **Ölçeklenebilirlik:** Yüksek hacimli veri işleme kapasitesi
- **Esneklik:** Yeni özellikler için kolay genişletilebilirlik
- **Performans:** Optimum sorgu hızı için indeksleme stratejisi

## Temel Koleksiyonlar

### users
Tüm kullanıcı tiplerini içeren ana koleksiyon.

```json
{
  "_id": ObjectId,
  "email": String,
  "passwordHash": String,
  "firstName": String,
  "lastName": String,
  "profileImage": String,
  "phoneNumber": String,
  "createdAt": Date,
  "updatedAt": Date,
  "role": ["client", "coach", "gym_owner", "gym_member", "gym_coach", "studio_owner", "studio_coach", "studio_client"],
  "isActive": Boolean,
  "fcmToken": String,
  
  // Role-specific fields
  "roleSpecificData": {
    "client": {
      "dateOfBirth": Date,
      "height": Number,
      "weight": Number,
      "fitnessGoals": [String],
      "fitnessLevel": String,
      "dietaryRestrictions": [String],
      "medicalConditions": [String],
      "assignedCoachId": ObjectId
    },
    "coach": {
      "specializations": [String],
      "bio": String,
      "experience": Number,
      "certifications": [String],
      "ratings": Number,
      "reviewCount": Number,
      "isFreelance": Boolean,
      "hourlyRate": Number
    },
    "gym_owner": {
      "businessName": String,
      "businessAddress": String,
      "businessPhoneNumber": String,
      "businessEmail": String,
      "taxId": String
    },
    // ... other role specific data
  }
}
```

### measurements
Kullanıcı ölçümlerini takip eden koleksiyon.

```json
{
  "_id": ObjectId,
  "userId": ObjectId,
  "date": Date,
  "measurements": {
    "weight": Number,
    "bodyFatPercentage": Number,
    "muscleMass": Number,
    "chestCircumference": Number,
    "waistCircumference": Number,
    "hipCircumference": Number,
    "armCircumference": Number,
    "thighCircumference": Number
  },
  "photos": [
    {
      "url": String,
      "position": String,
      "date": Date
    }
  ],
  "notes": String
}
```

### exercises
Egzersiz veritabanı.

```json
{
  "_id": ObjectId,
  "name": String,
  "description": String,
  "instructions": [String],
  "media": {
    "videoUrl": String,
    "imageUrls": [String],
    "thumbnailUrl": String
  },
  "metadata": {
    "bodyAreas": [String],
    "type": String,
    "equipment": [String],
    "difficultyLevel": String,
    "calories": Number,
    "duration": Number,
    "tags": [String]
  },
  "createdBy": ObjectId,
  "isPublic": Boolean,
  "alternateExercises": [ObjectId]
}
```

### workout_templates
Antrenman şablonları.

```json
{
  "_id": ObjectId,
  "name": String,
  "description": String,
  "imageUrl": String,
  "createdBy": ObjectId,
  "isPublic": Boolean,
  "metadata": {
    "targetGoal": String,
    "difficulty": String,
    "estimatedDuration": Number,
    "estimatedCalories": Number,
    "targetBodyAreas": [String],
    "tags": [String]
  },
  "exercises": [
    {
      "exerciseId": ObjectId,
      "order": Number,
      "defaultSets": [
        {
          "reps": Number,
          "weight": Number,
          "duration": Number,
          "distance": Number,
          "restTime": Number
        }
      ],
      "notes": String
    }
  ],
  "createdAt": Date,
  "updatedAt": Date,
  "reviews": {
    "averageRating": Number,
    "count": Number
  }
}
```

### workout_programs
Antrenman programları (birden fazla antrenmandan oluşan).

```json
{
  "_id": ObjectId,
  "name": String,
  "description": String,
  "imageUrl": String,
  "createdBy": ObjectId,
  "isPublic": Boolean,
  "metadata": {
    "targetGoal": String,
    "difficulty": String,
    "duration": Number, // days
    "tags": [String]
  },
  "schedule": [
    {
      "day": Number, // 1-based index
      "workoutTemplateId": ObjectId,
      "isRestDay": Boolean,
      "notes": String
    }
  ],
  "pricing": {
    "price": Number,
    "currency": String,
    "discountPrice": Number,
    "isDiscounted": Boolean
  },
  "createdAt": Date,
  "updatedAt": Date,
  "reviews": {
    "averageRating": Number,
    "count": Number
  }
}
```

### workout_logs
Tamamlanan antrenmanların kayıtları.

```json
{
  "_id": ObjectId,
  "userId": ObjectId,
  "workoutTemplateId": ObjectId,
  "startTime": Date,
  "endTime": Date,
  "duration": Number,
  "caloriesBurned": Number,
  "exercises": [
    {
      "exerciseId": ObjectId,
      "sets": [
        {
          "reps": Number,
          "weight": Number,
          "duration": Number,
          "distance": Number,
          "restTime": Number,
          "completed": Boolean
        }
      ],
      "notes": String
    }
  ],
  "feedback": {
    "rating": Number,
    "difficulty": Number,
    "notes": String
  },
  "biometrics": {
    "heartRate": {
      "average": Number,
      "max": Number,
      "min": Number
    }
  },
  "location": {
    "type": String,
    "coordinates": [Number, Number]
  },
  "photos": [String],
  "notes": String
}
```

### nutrition_plans
Beslenme planları.

```json
{
  "_id": ObjectId,
  "name": String,
  "description": String,
  "createdBy": ObjectId,
  "forUserId": ObjectId,
  "isTemplate": Boolean,
  "isPublic": Boolean,
  "metadata": {
    "calorieTarget": Number,
    "macros": {
      "protein": Number,
      "carbs": Number,
      "fat": Number
    },
    "mealsPerDay": Number,
    "tags": [String]
  },
  "meals": [
    {
      "name": String,
      "time": String,
      "foods": [
        {
          "foodId": ObjectId,
          "quantity": Number,
          "unit": String,
          "calories": Number,
          "protein": Number,
          "carbs": Number,
          "fat": Number
        }
      ],
      "notes": String
    }
  ],
  "pricing": {
    "price": Number,
    "currency": String
  },
  "createdAt": Date,
  "updatedAt": Date
}
```

### foods
Besin veritabanı.

```json
{
  "_id": ObjectId,
  "name": String,
  "description": String,
  "image": String,
  "category": String,
  "nutrients": {
    "servingSize": Number,
    "servingUnit": String,
    "calories": Number,
    "protein": Number,
    "carbs": Number,
    "fat": Number,
    "fiber": Number,
    "sugar": Number,
    "sodium": Number
  },
  "isVerified": Boolean,
  "source": String,
  "createdBy": ObjectId,
  "tags": [String]
}
```

### facilities
Spor tesisleri (gym, stüdyo).

```json
{
  "_id": ObjectId,
  "name": String,
  "type": String, // "gym", "studio"
  "address": {
    "street": String,
    "city": String,
    "state": String,
    "zipCode": String,
    "country": String,
    "coordinates": {
      "latitude": Number,
      "longitude": Number
    }
  },
  "contact": {
    "phoneNumber": String,
    "email": String,
    "website": String
  },
  "media": {
    "logo": String,
    "images": [String],
    "virtualTour": String
  },
  "operatingHours": [
    {
      "day": String,
      "openTime": String,
      "closeTime": String,
      "isClosed": Boolean
    }
  ],
  "amenities": [String],
  "features": [String],
  "equipments": [
    {
      "name": String,
      "category": String,
      "quantity": Number,
      "brand": String
    }
  ],
  "ownerId": ObjectId,
  "staff": [
    {
      "userId": ObjectId,
      "role": String,
      "permissions": [String],
      "joinedAt": Date
    }
  ],
  "subscriptionPlan": {
    "planId": String,
    "startDate": Date,
    "endDate": Date,
    "isActive": Boolean
  },
  "createdAt": Date,
  "updatedAt": Date
}
```

### memberships
Üyelik planları ve abonelikler.

```json
{
  "_id": ObjectId,
  "facilityId": ObjectId,
  "name": String,
  "description": String,
  "duration": {
    "value": Number,
    "unit": String // "day", "week", "month", "year"
  },
  "price": Number,
  "currency": String,
  "features": [String],
  "limitations": {
    "maxVisitsPerWeek": Number,
    "maxClassBookingsPerWeek": Number,
    "classesIncluded": Boolean,
    "personalTrainingIncluded": Boolean,
    "ptSessionsIncluded": Number
  },
  "isActive": Boolean,
  "createdAt": Date,
  "updatedAt": Date
}
```

### member_subscriptions
Kullanıcı üyelikleri.

```json
{
  "_id": ObjectId,
  "userId": ObjectId,
  "facilityId": ObjectId,
  "membershipId": ObjectId,
  "startDate": Date,
  "endDate": Date,
  "status": String, // "active", "expired", "cancelled", "pending"
  "paymentHistory": [
    {
      "amount": Number,
      "currency": String,
      "date": Date,
      "method": String,
      "status": String,
      "transactionId": String
    }
  ],
  "autoRenew": Boolean,
  "createdAt": Date,
  "updatedAt": Date,
  "cancelledAt": Date,
  "cancellationReason": String
}
```

### classes
Grup dersleri.

```json
{
  "_id": ObjectId,
  "facilityId": ObjectId,
  "name": String,
  "description": String,
  "instructor": {
    "userId": ObjectId,
    "name": String
  },
  "schedule": {
    "startTime": Date,
    "endTime": Date,
    "recurrence": {
      "frequency": String, // "once", "daily", "weekly", "monthly"
      "interval": Number,
      "weekdays": [Number], // 0-6 (Sunday to Saturday)
      "endsOnDate": Date,
      "maxOccurrences": Number
    }
  },
  "location": {
    "room": String,
    "capacity": Number
  },
  "category": String,
  "difficulty": String,
  "duration": Number, // minutes
  "image": String,
  "tags": [String],
  "isActive": Boolean,
  "createdAt": Date,
  "updatedAt": Date
}
```

### class_bookings
Ders rezervasyonları.

```json
{
  "_id": ObjectId,
  "classId": ObjectId,
  "userId": ObjectId,
  "date": Date,
  "status": String, // "booked", "attended", "cancelled", "no-show"
  "bookedAt": Date,
  "cancelledAt": Date,
  "checkedInAt": Date,
  "notes": String
}
```

### coach_clients
Antrenör-danışan ilişkileri.

```json
{
  "_id": ObjectId,
  "coachId": ObjectId,
  "clientId": ObjectId,
  "relationship": {
    "status": String, // "active", "pending", "declined", "ended"
    "startDate": Date,
    "endDate": Date
  },
  "package": {
    "name": String,
    "sessions": {
      "total": Number,
      "used": Number,
      "remaining": Number
    },
    "price": Number,
    "currency": String,
    "startDate": Date,
    "endDate": Date
  },
  "goals": [String],
  "notes": {
    "visible": [
      {
        "text": String,
        "date": Date
      }
    ],
    "private": [
      {
        "text": String,
        "date": Date
      }
    ]
  },
  "createdAt": Date,
  "updatedAt": Date
}
```

### pt_sessions
Kişisel antrenman seansları.

```json
{
  "_id": ObjectId,
  "coachId": ObjectId,
  "clientId": ObjectId,
  "facilityId": ObjectId,
  "startTime": Date,
  "endTime": Date,
  "duration": Number, // minutes
  "status": String, // "scheduled", "completed", "cancelled", "no-show"
  "location": {
    "type": String, // "facility", "online", "client", "outdoor"
    "details": String
  },
  "workout": {
    "templateId": ObjectId,
    "exercises": [ObjectId],
    "notes": String
  },
  "notes": {
    "pre": String,
    "post": String
  },
  "feedback": {
    "coach": {
      "rating": Number,
      "notes": String
    },
    "client": {
      "rating": Number,
      "notes": String
    }
  },
  "createdAt": Date,
  "updatedAt": Date,
  "cancelledAt": Date,
  "cancellationReason": String,
  "cancelledBy": ObjectId
}
```

## Veri İlişkileri

### İlişki Türleri

1. **One-to-One İlişkiler:**
   - Her kullanıcının bir profili var
   - Her PT seansının bir koç ve bir danışanı var

2. **One-to-Many İlişkiler:**
   - Bir kullanıcının birden çok antrenman kaydı olabilir
   - Bir koçun birden çok danışanı olabilir
   - Bir tesiste birden çok ders olabilir

3. **Many-to-Many İlişkiler:**
   - Birden çok kullanıcı birden çok derse katılabilir
   - Bir workout birden çok antrenman programında kullanılabilir
   - Bir egzersiz birden çok workout'ta yer alabilir

## İndeksleme Stratejisi

Performans optimizasyonu için aşağıdaki alanlar indekslenmiştir:

1. **users** koleksiyonu:
   - email (unique)
   - role (sorgu optimizasyonu)

2. **measurements** koleksiyonu:
   - userId + date (compound)

3. **workout_logs** koleksiyonu:
   - userId
   - startTime
   - workoutTemplateId

4. **facilities** koleksiyonu:
   - "address.coordinates" (2dsphere - konum tabanlı sorgular için)
   - ownerId

5. **classes** koleksiyonu:
   - facilityId
   - "schedule.startTime" (zaman bazlı sorgular için)

## Veri Doğrulama ve Güvenlik

1. **Şema Doğrulama:** MongoDB şema doğrulama kuralları ile veri bütünlüğü sağlanmaktadır
2. **Erişim Kontrolü:** Rol tabanlı erişim kontrolü (RBAC) ile veri güvenliği sağlanmaktadır
3. **Veri Şifreleme:** Hassas kullanıcı verileri şifrelenmiş olarak saklanmaktadır
4. **Veri Yedekleme:** Günlük yedekleme ile veri kaybı riski minimize edilmektedir

## Veritabanı Optimizasyonu

1. **Denormalizasyon:** Sık birlikte sorgulanan veriler için kısmi denormalizasyon uygulanmıştır
2. **Bileşik İndeksler:** Sık kullanılan sorgu desenlerine göre bileşik indeksler oluşturulmuştur
3. **TTL İndeksleri:** Geçici verilerin otomatik silinmesi için TTL (Time To Live) indeksleri kullanılmaktadır
4. **Koleksiyon Bölünmesi:** Yüksek hacimli veriler için koleksiyon bölünmesi stratejisi uygulanmaktadır

## Veri Büyüme Stratejisi

Artan veri hacmi için aşağıdaki stratejiler uygulanmaktadır:

1. **Yatay Ölçeklendirme:** MongoDB sharding ile yatay ölçeklendirme
2. **Arşivleme:** Eski verilerin otomatik arşivlenmesi
3. **Sıkıştırma:** Veri sıkıştırma ile depolama optimizasyonu
4. **Çoklu Depolama Katmanları:** Sıcak/ılık/soğuk veri stratejisi ile maliyet optimizasyonu

## Veri Modeli Evrim Stratejisi

Veritabanı şeması değişikliklerini yönetmek için aşağıdaki süreçler uygulanmaktadır:

1. **Şema Versiyon Yönetimi:** Şema değişikliklerini izleme ve versiyon kontrol
2. **Aşamalı Migrasyon:** Büyük veri kümeleri için aşamalı migrasyon
3. **Geri Dönüş Planları:** Her şema değişikliği için geri dönüş planı
4. **Uygulama-Veritabanı Uyumluluğu:** Geriye dönük uyumluluk garantisi 