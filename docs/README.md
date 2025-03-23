# NexFit İş Yönetim Sistemi

<p align="center">
  <img src="docs/assets/nexfit-logo.png" alt="NexFit Logo" width="200"/>
</p>

<p align="center">
  <em>Fitness sektörü için geliştirilmiş kapsamlı iş yönetim çözümü</em>
</p>

[![Build Status](https://github.com/canakyuz/nexfit-business-management/actions/workflows/ci.yml/badge.svg)](https://github.com/canakyuz/nexfit-business-management/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/github/v/release/canakyuz/nexfit-business-management)](https://github.com/canakyuz/nexfit-business-management/releases)

## 📋 İçindekiler

- [Genel Bakış](#genel-bakış)
- [Özellikler](#özellikler)
- [Başlarken](#başlarken)
- [Kurulum](#kurulum)
- [Kullanım](#kullanım)
- [Mimari](#mimari)
- [API Dokümantasyonu](#api-dokümantasyonu)
- [Katkı Sağlama](#katkı-sağlama)
- [Yol Haritası](#yol-haritası)
- [Lisans](#lisans)
- [İletişim](#i̇letişim)

## 🌟 Genel Bakış

NexFit İş Yönetim Sistemi, fitness salonları, stüdyolar, kişisel antrenörler ve sağlık profesyonelleri için tasarlanmış kapsamlı bir iş yönetim çözümüdür. Müşteri yönetimi, rezervasyon, faturalandırma, çalışan yönetimi, envanter takibi ve analitik raporlama gibi temel işlevleri tek bir platformda birleştirir.

Modern, ölçeklenebilir ve kullanıcı dostu bir tasarıma sahip olan NexFit, fitness işletmelerinin operasyonlarını verimli bir şekilde yönetmelerini ve müşteri deneyimini geliştirmelerini sağlar.

## ✨ Özellikler

### 🧑‍💼 Müşteri Yönetimi
- Müşteri profilleri ve üyelik yönetimi
- Müşteri ilerleme ve hedef takibi
- Otomatik hatırlatmalar ve bildirimler
- Müşteri sadakat programları

### 📅 Randevu ve Ders Yönetimi
- Akıllı zamanlama ve çakışma önleme
- Grup dersleri ve PT seansları yönetimi
- Çevrimiçi rezervasyon sistemi
- Ders ve antrenör müsaitlik takibi

### 💰 Faturalandırma ve Ödemeler
- Otomatik ücretlendirme ve abonelik yönetimi
- Çoklu ödeme seçenekleri
- Fatura oluşturma ve takibi
- Gelir raporları ve finansal analitik

### 👨‍💻 Personel Yönetimi
- Antrenör ve personel programlama
- Performans takibi ve değerlendirme
- Komisyon ve ücret hesaplama
- İzin ve vardiya yönetimi

### 📊 Raporlama ve Analitik
- İş performans göstergeleri
- Müşteri davranış analizleri
- Finansal tahminler ve trendler
- Özelleştirilebilir rapor panoları

### 🔒 Güvenlik ve Uyumluluk
- KVKK uyumlu veri koruma
- Rol tabanlı erişim kontrolü
- Veri yedekleme ve kurtarma
- Güvenli ödeme işlemleri

## 🚀 Başlarken

### Sistem Gereksinimleri

#### Backend
- Node.js 18 veya üzeri
- MongoDB 6.0 veya üzeri
- Redis 7.0 veya üzeri

#### Frontend
- Modern web tarayıcısı (Chrome, Firefox, Safari, Edge)
- JavaScript desteği etkin

#### Mobil
- iOS 14 veya üzeri
- Android 9.0 veya üzeri

### Kurulum

Ayrıntılı kurulum talimatları için [INSTALLATION.md](docs/INSTALLATION.md) belgesine bakınız.

Hızlı başlangıç:

```bash
# Repoyu klonlayın
git clone https://github.com/canakyuz/nexfit-business-management.git
cd nexfit-business-management

# Bağımlılıkları yükleyin
npm install

# Ortam değişkenlerini yapılandırın
cp .env.example .env
# .env dosyasını düzenleyin

# Geliştirme sunucusunu başlatın
npm run dev
```

## 💻 Kullanım

Temel kullanım kılavuzu için [docs/usage/GETTING_STARTED.md](docs/usage/GETTING_STARTED.md) belgesine bakınız.

NexFit İş Yönetim Sistemini nasıl kullanacağınızı anlatan video eğitimleri için [eğitim portalımızı](https://training.nexfit.com) ziyaret edebilirsiniz.

## 🏗️ Mimari

NexFit, modern, ölçeklenebilir ve modüler bir mimariye sahiptir. Sistem mimarisi hakkında ayrıntılı bilgi için [ARCHITECTURE.md](docs/development/ARCHITECTURE.md) belgesine bakınız.

### Teknoloji Yığını

- **Frontend**: React, TypeScript, Tailwind CSS
- **Backend**: Node.js, Express, MongoDB, Redis
- **Mobil**: React Native
- **DevOps**: Docker, Kubernetes, GitHub Actions

## 📚 API Dokümantasyonu

API dokümantasyonu [docs/api/README.md](docs/api/README.md) dosyasında bulunabilir veya canlı Swagger arayüzü üzerinden `https://api.nexfit.com/docs` adresinden erişilebilir.

## 👥 Katkı Sağlama

Projeye katkıda bulunmak istiyorsanız, [CONTRIBUTING.md](CONTRIBUTING.md) belgesini inceleyerek başlayabilirsiniz. Tüm katkılara açığız!

## 🗺️ Yol Haritası

Planlanan özellikler ve geliştirmeler için [ROADMAP.md](docs/product/ROADMAP.md) belgesine bakınız.

## 📜 Lisans

Bu proje MIT Lisansı altında lisanslanmıştır - ayrıntılar için [LICENSE](LICENSE) dosyasına bakınız.

## 📞 İletişim

- **Web sitesi**: [https://nexfit.com](https://nexfit.com)
- **E-posta**: [info@nexfit.com](mailto:info@nexfit.com)
- **Twitter**: [@NexFitApp](https://twitter.com/NexFitApp)
- **LinkedIn**: [NexFit](https://linkedin.com/company/nexfit)

---

<p align="center">
  <sub>NexFit © 2025. Tüm hakları saklıdır.</sub>
</p>