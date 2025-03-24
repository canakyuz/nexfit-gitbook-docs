# NexFit Veri Modeli

Bu dokümanda NexFit uygulamasının veritabanı yapısı ve veri modelleri detaylandırılmıştır. NexFit, veri özelliklerine göre iki farklı veritabanı teknolojisi kullanmaktadır:

1. **MongoDB**: Doküman-tabanlı veritabanı olarak, esnek şema gerektiren veriler için kullanılmaktadır.
2. **PostgreSQL**: İlişkisel veritabanı olarak, finansal veriler ve güçlü ilişkisel bütünlük gerektiren veriler için kullanılmaktadır.

## Veritabanı Mimarisi

NexFit veritabanı aşağıdaki temel prensiplere dayanmaktadır:

- **Hibrit Yaklaşım:** Veri tipine göre en uygun veritabanının kullanılması
- **Modüler Tasarım:** Her işlevsellik için ayrı koleksiyonlar/tablolar
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

## PostgreSQL Veri Modelleri

PostgreSQL, NexFit sistemindeki finansal işlemler, raporlama verileri ve güçlü ilişkisel bütünlük gerektiren diğer veriler için kullanılmaktadır. Aşağıda PostgreSQL'de tutulan başlıca tablolar ve ilişkileri açıklanmıştır.

### Tablolar ve İlişkiler

#### 1. `transactions` (İşlemler)

İşletme içindeki tüm finansal işlemleri takip eder.

```sql
CREATE TABLE transactions (
    id SERIAL PRIMARY KEY,
    transaction_id VARCHAR(36) UNIQUE NOT NULL,
    user_id VARCHAR(24) NOT NULL,  -- MongoDB'deki kullanıcı ID'si
    amount DECIMAL(10, 2) NOT NULL,
    currency VARCHAR(3) NOT NULL DEFAULT 'TRY',
    payment_method VARCHAR(20) NOT NULL,
    transaction_type VARCHAR(20) NOT NULL,
    status VARCHAR(20) NOT NULL,
    description TEXT,
    metadata JSONB,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_transactions_user_id ON transactions(user_id);
CREATE INDEX idx_transactions_created_at ON transactions(created_at);
CREATE INDEX idx_transactions_status ON transactions(status);
```

#### 2. `subscriptions` (Abonelikler)

Kullanıcı aboneliklerini ve tekrarlayan ödemeleri yönetir.

```sql
CREATE TABLE subscriptions (
    id SERIAL PRIMARY KEY,
    subscription_id VARCHAR(36) UNIQUE NOT NULL,
    user_id VARCHAR(24) NOT NULL,  -- MongoDB'deki kullanıcı ID'si
    plan_id VARCHAR(36) NOT NULL,
    status VARCHAR(20) NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE,
    billing_cycle VARCHAR(20) NOT NULL,
    billing_day INTEGER,
    amount DECIMAL(10, 2) NOT NULL,
    currency VARCHAR(3) NOT NULL DEFAULT 'TRY',
    payment_method VARCHAR(20) NOT NULL,
    auto_renew BOOLEAN NOT NULL DEFAULT TRUE,
    metadata JSONB,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_subscriptions_user_id ON subscriptions(user_id);
CREATE INDEX idx_subscriptions_status ON subscriptions(status);
CREATE INDEX idx_subscriptions_end_date ON subscriptions(end_date);
```

#### 3. `invoices` (Faturalar)

Oluşturulan faturaları ve ödeme durumlarını saklar.

```sql
CREATE TABLE invoices (
    id SERIAL PRIMARY KEY,
    invoice_id VARCHAR(36) UNIQUE NOT NULL,
    transaction_id VARCHAR(36),
    subscription_id VARCHAR(36),
    user_id VARCHAR(24) NOT NULL,  -- MongoDB'deki kullanıcı ID'si
    business_id VARCHAR(24) NOT NULL,  -- MongoDB'deki işletme ID'si
    total_amount DECIMAL(10, 2) NOT NULL,
    tax_amount DECIMAL(10, 2) NOT NULL,
    currency VARCHAR(3) NOT NULL DEFAULT 'TRY',
    status VARCHAR(20) NOT NULL,
    due_date DATE NOT NULL,
    payment_date DATE,
    invoice_number VARCHAR(20) UNIQUE NOT NULL,
    invoice_data JSONB NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (transaction_id) REFERENCES transactions(transaction_id) ON DELETE SET NULL,
    FOREIGN KEY (subscription_id) REFERENCES subscriptions(subscription_id) ON DELETE SET NULL
);

CREATE INDEX idx_invoices_user_id ON invoices(user_id);
CREATE INDEX idx_invoices_business_id ON invoices(business_id);
CREATE INDEX idx_invoices_status ON invoices(status);
CREATE INDEX idx_invoices_due_date ON invoices(due_date);
```

#### 4. `analytics_daily` (Günlük Analitik)

Raporlama ve analiz için günlük verileri derler.

```sql
CREATE TABLE analytics_daily (
    id SERIAL PRIMARY KEY,
    business_id VARCHAR(24) NOT NULL,  -- MongoDB'deki işletme ID'si
    date DATE NOT NULL,
    new_members INTEGER NOT NULL DEFAULT 0,
    active_members INTEGER NOT NULL DEFAULT 0,
    total_revenue DECIMAL(12, 2) NOT NULL DEFAULT 0,
    class_attendance INTEGER NOT NULL DEFAULT 0,
    subscription_renewals INTEGER NOT NULL DEFAULT 0,
    subscription_cancellations INTEGER NOT NULL DEFAULT 0,
    metrics JSONB,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE UNIQUE INDEX idx_analytics_daily_business_date ON analytics_daily(business_id, date);
```

#### 5. `audit_logs` (Denetim Günlükleri)

Denetim ve uyumluluk için gerekli işlem kayıtlarını tutar.

```sql
CREATE TABLE audit_logs (
    id SERIAL PRIMARY KEY,
    entity_type VARCHAR(50) NOT NULL,
    entity_id VARCHAR(36) NOT NULL,
    user_id VARCHAR(24),  -- MongoDB'deki kullanıcı ID'si
    action VARCHAR(50) NOT NULL,
    previous_state JSONB,
    new_state JSONB,
    ip_address VARCHAR(45),
    user_agent TEXT,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_audit_logs_entity ON audit_logs(entity_type, entity_id);
CREATE INDEX idx_audit_logs_user_id ON audit_logs(user_id);
CREATE INDEX idx_audit_logs_created_at ON audit_logs(created_at);
```

### PostgreSQL - MongoDB Entegrasyonu

NexFit sisteminde PostgreSQL ve MongoDB veritabanları arasında veri entegrasyonu aşağıdaki yöntemlerle sağlanmaktadır:

1. **Referans ID'ler**: PostgreSQL tablolarında MongoDB doküman ID'lerine referans verilir (örn. `user_id`).

2. **Eş Zamanlı İşlemler**: Kritik iş süreçlerinde, her iki veritabanına da veri yazılması gereken durumlarda iki fazlı işlem (two-phase commit) protokolü kullanılır.

3. **Veri Senkronizasyonu**: Düzenli aralıklarla, belirli veriler iki veritabanı arasında senkronize edilir.

4. **Önbellek İlişkileri**: Redis önbellek katmanı, her iki veritabanından gelen verileri birleştirerek performans artışı sağlar.

### PostgreSQL Veri Yönetimi

#### İndeksleme Stratejisi

PostgreSQL'de performans optimizasyonu için kapsamlı bir indeksleme stratejisi uygulanmaktadır:

- **B-Tree İndeksler**: Eşitlik ve aralık sorguları için standart indeksler
- **GIN İndeksler**: JSONB alanlarında arama yapmak için
- **Partial İndeksler**: Belirli WHERE koşullarına göre filtrelenmiş veriler için 

#### Veri Bölümleme (Partitioning)

Büyük tablolar, özellikle `transactions` ve `analytics_daily` gibi zaman bazlı veriler için tablo bölümleme uygulanmaktadır:

```sql
-- Örnek: analytics_daily tablosunu yıllara göre bölümleme
CREATE TABLE analytics_daily (
    id SERIAL,
    business_id VARCHAR(24) NOT NULL,
    date DATE NOT NULL,
    -- diğer alanlar
    PRIMARY KEY (id, date)
) PARTITION BY RANGE (date);

-- 2023 yılı için bölüm
CREATE TABLE analytics_daily_2023 PARTITION OF analytics_daily
    FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');

-- 2024 yılı için bölüm  
CREATE TABLE analytics_daily_2024 PARTITION OF analytics_daily
    FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');
```

#### Yedekleme ve Kurtarma

PostgreSQL veritabanı için aşağıdaki yedekleme stratejileri uygulanmaktadır:

1. **Günlük Tam Yedekler**: `pg_dump` ile her gün tam yedek alınır
2. **Sürekli Arşivleme (WAL)**: Write-Ahead Log dosyaları noktasal kurtarma için saklanır
3. **Otomatik Yedekleme Rotasyonu**: Yedekler 30 gün saklanır ve sonra otomatik olarak arşivlenir

### Veritabanı Erişim Katmanı

PostgreSQL veritabanı ile etkileşim için aşağıdaki araçlar kullanılmaktadır:

1. **Knex.js**: SQL sorgu oluşturucu ve migrasyon aracı
2. **TypeORM/Sequelize**: ORM (Object-Relational Mapping) çerçevesi
3. **PostgreSQL İşlevleri**: Karmaşık işlemler için veritabanı içinde tanımlanan işlevler
4. **pg-promise**: Düşük seviyeli veritabanı erişimi için

### Performans Monitörü

PostgreSQL veritabanı performansını izlemek için aşağıdaki metrikler takip edilmektedir:

1. **Sorgu Performansı**: Yavaş sorgu günlükleri analiz edilir
2. **Bağlantı Havuzu**: Bağlantı kullanımı ve kaynak tüketimi izlenir
3. **İndeks Kullanımı**: İndekslerin etkinliği ve kullanım oranları takip edilir
4. **Tablo Büyüme Oranları**: Veritabanı boyutu ve büyüme hızı izlenir 