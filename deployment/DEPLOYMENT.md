# NexFit Dağıtım (Deployment) Kılavuzu

Bu belge, NexFit uygulamasının farklı ortamlara dağıtımı için kapsamlı bir kılavuz sağlar. Geliştirme, test ve üretim ortamları için adım adım talimatlar içerir.

## İçindekiler

1. [Dağıtım Stratejisi](#dağıtım-stratejisi)
2. [Gereksinimler](#gereksinimler)
3. [Ortamlar](#ortamlar)
4. [Dağıtım Süreçleri](#dağıtım-süreçleri)
   - [Backend Dağıtımı](#backend-dağıtımı)
   - [Frontend Dağıtımı](#frontend-dağıtımı)
   - [Mobil Uygulama Dağıtımı](#mobil-uygulama-dağıtımı)
5. [CI/CD Pipeline](#cicd-pipeline)
6. [Veritabanı Migrasyonları](#veritabanı-migrasyonları)
7. [Ölçeklendirme](#ölçeklendirme)
8. [İzleme ve Günlük Kaydı](#i̇zleme-ve-günlük-kaydı)
9. [Geri Alma Stratejisi](#geri-alma-stratejisi)
10. [Güvenlik Kontrolleri](#güvenlik-kontrolleri)
11. [Performans İzleme](#performans-i̇zleme)
12. [Sorun Giderme](#sorun-giderme)

## Dağıtım Stratejisi

NexFit uygulaması, mikroservis mimarisi kullanılarak dağıtılmaktadır. Her servis bağımsız olarak dağıtılabilir ve ölçeklendirilebilir. Dağıtım stratejimiz aşağıdaki ilkelere dayanmaktadır:

- **Sıfır Kesinti Dağıtımı**: Üretim ortamında kullanıcı deneyimini etkilemeden güncellemeler yapabilmek
- **Aşamalı Dağıtım**: Değişiklikleri önce test ve test ortamlarında doğrulayarak risk azaltma
- **Otomatik Dağıtım**: CI/CD pipeline'ları kullanarak manuel müdahaleyi en aza indirme
- **Kolayca Geri Alınabilirlik**: Herhangi bir sorun durumunda hızlıca önceki sürüme dönebilme kapasitesi

## Gereksinimler

### Altyapı Gereksinimleri

- **Kubernetes Cluster**: Üretim ve test ortamları için
- **Docker**: Tüm servislerin konteynerizasyonu için
- **GitHub Actions/Jenkins**: CI/CD pipeline'ları için
- **Terraform/Pulumi**: Altyapı olarak kod (IaC) yönetimi için
- **Prometheus & Grafana**: İzleme ve uyarı için
- **ELK Stack**: Günlük kaydı ve analizi için

### Yazılım Gereksinimleri

- **kubectl**: Kubernetes cluster'ını yönetmek için
- **Helm**: Kubernetes uygulamalarını paketlemek ve dağıtmak için
- **Docker CLI**: Docker imajlarını oluşturmak ve yönetmek için
- **Node.js 18+**: Backend ve frontend geliştirme için
- **MongoDB Tools**: Veritabanı yönetimi için
- **AWS CLI/GCP CLI**: Bulut sağlayıcısına bağlı olarak

## Ortamlar

NexFit, aşağıdaki ortamlarda dağıtılır:

### 1. Geliştirme (Development) Ortamı

- **Amaç**: Geliştiricilerin aktif olarak çalıştığı, deneysel özelliklerin test edildiği ortam
- **Altyapı**: Hafif Kubernetes veya Docker Compose
- **Veri**: Anonimleştirilmiş veri örnekleri
- **URL**: `dev.nexfit.com`
- **Dağıtım Frekansı**: Her commit'te otomatik dağıtım

### 2. Test (Staging) Ortamı

- **Amaç**: Üretim benzeri ortamda QA testleri ve entegrasyon testleri için
- **Altyapı**: Üretim ile aynı konfigürasyonda Kubernetes
- **Veri**: Üretim verisinin anonimleştirilmiş kopyası
- **URL**: `staging.nexfit.com`
- **Dağıtım Frekansı**: Günlük veya önemli özellikler tamamlandığında

### 3. Üretim (Production) Ortamı

- **Amaç**: Son kullanıcıların eriştiği canlı ortam
- **Altyapı**: Tam ölçekli, yüksek kullanılabilirlikli Kubernetes cluster
- **Veri**: Gerçek kullanıcı verisi
- **URL**: `app.nexfit.com`, `api.nexfit.com`
- **Dağıtım Frekansı**: Haftalık planlı sürümler, kritik hatalar için acil düzeltmeler

## Dağıtım Süreçleri

### Backend Dağıtımı

#### Kubernetes ile Backend Dağıtımı

1. **Docker İmajı Oluşturma**:
   ```bash
   cd backend
   docker build -t nexfit/api:${VERSION} .
   docker push nexfit/api:${VERSION}
   ```

2. **Helm Chart Güncelleme**:
   ```bash
   cd deployment/helm
   # values.yaml dosyasında imaj etiketini güncelle
   helm upgrade --install nexfit-api ./nexfit-api -f values-${ENV}.yaml
   ```

3. **Dağıtımı Doğrulama**:
   ```bash
   kubectl get pods -n nexfit
   kubectl logs -n nexfit -l app=nexfit-api
   curl https://api.${ENV}.nexfit.com/health
   ```

#### Manuel Dağıtım (Docker-Compose)

```bash
cd deployment
docker-compose -f docker-compose.${ENV}.yml up -d
```

### Frontend Dağıtımı

#### Web Uygulaması Dağıtımı

1. **Yapılandırma**:
   ```bash
   cd frontend
   # Ortam değişkenlerini ayarla
   cp .env.${ENV} .env
   ```

2. **Derleme ve Paketleme**:
   ```bash
   npm install
   npm run build:${ENV}
   ```

3. **CDN/Statik Hosting Dağıtımı**:
   ```bash
   # AWS S3 + CloudFront için
   aws s3 sync ./dist s3://nexfit-${ENV}-frontend --delete
   aws cloudfront create-invalidation --distribution-id ${CF_DIST_ID} --paths "/*"
   
   # VEYA
   
   # Netlify için
   netlify deploy --prod --dir=dist
   ```

### Mobil Uygulama Dağıtımı

#### Android

1. **Yapılandırma**:
   ```bash
   cd mobile
   cp .env.${ENV} .env
   ```

2. **Bundle Oluşturma**:
   ```bash
   # Development için
   npm run android:bundle:dev
   
   # Production için
   npm run android:bundle:prod
   ```

3. **Google Play'e Yükleme**:
   ```bash
   cd android
   ./gradlew bundleRelease
   # Oluşturulan AAB dosyasını Google Play Console'a yükle
   ```

#### iOS

1. **Yapılandırma**:
   ```bash
   cd mobile
   cp .env.${ENV} .env
   ```

2. **Derleme ve Arşivleme**:
   ```bash
   # Development için
   npm run ios:build:dev
   
   # Production için
   npm run ios:build:prod
   ```

3. **App Store'a Yükleme**:
   ```bash
   cd ios
   # Xcode ile arşiv oluştur
   xcodebuild -workspace NexFit.xcworkspace -scheme NexFit archive -archivePath NexFit.xcarchive
   
   # IPA oluştur
   xcodebuild -exportArchive -archivePath NexFit.xcarchive -exportOptionsPlist exportOptions.plist -exportPath ./build
   
   # App Store Connect'e yükle
   xcrun altool --upload-app --file build/NexFit.ipa --username "${APPLE_ID}" --password "${APP_SPECIFIC_PASSWORD}"
   ```

## CI/CD Pipeline

NexFit, GitHub Actions kullanarak CI/CD pipeline'larını yönetir. Temel pipeline şu adımlardan oluşur:

### 1. Backend Pipeline

```yaml
name: Backend CI/CD

on:
  push:
    branches: [ main, develop ]
    paths:
      - 'backend/**'
  pull_request:
    branches: [ main, develop ]
    paths:
      - 'backend/**'

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - name: Install dependencies
        run: cd backend && npm ci
      - name: Run tests
        run: cd backend && npm test
  
  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.event_name == 'push'
    steps:
      - uses: actions/checkout@v3
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      - name: Login to DockerHub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      - name: Build and push
        uses: docker/build-push-action@v3
        with:
          context: ./backend
          push: true
          tags: nexfit/api:${{ github.sha }},nexfit/api:latest
  
  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.event_name == 'push' && github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      - name: Set up kubectl
        uses: azure/k8s-set-context@v3
        with:
          kubeconfig: ${{ secrets.KUBE_CONFIG }}
      - name: Deploy to production
        run: |
          cd deployment/helm
          helm upgrade --install nexfit-api ./nexfit-api --set image.tag=${{ github.sha }} -f values-prod.yaml
```

### 2. Frontend Pipeline

```yaml
name: Frontend CI/CD

on:
  push:
    branches: [ main, develop ]
    paths:
      - 'frontend/**'
  pull_request:
    branches: [ main, develop ]
    paths:
      - 'frontend/**'

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - name: Install dependencies
        run: cd frontend && npm ci
      - name: Run tests
        run: cd frontend && npm test
  
  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.event_name == 'push'
    steps:
      - uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - name: Install dependencies
        run: cd frontend && npm ci
      - name: Build
        run: |
          cd frontend
          if [[ $GITHUB_REF == 'refs/heads/main' ]]; then
            cp .env.production .env
            npm run build
          else
            cp .env.staging .env
            npm run build
          fi
      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: frontend-build
          path: frontend/dist
  
  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.event_name == 'push' && (github.ref == 'refs/heads/main' || github.ref == 'refs/heads/develop')
    steps:
      - uses: actions/checkout@v3
      - name: Download artifacts
        uses: actions/download-artifact@v3
        with:
          name: frontend-build
          path: frontend/dist
      - name: Deploy to AWS
        if: github.ref == 'refs/heads/main'
        uses: jakejarvis/s3-sync-action@master
        with:
          args: --delete --follow-symlinks
        env:
          AWS_S3_BUCKET: ${{ secrets.PROD_S3_BUCKET }}
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          SOURCE_DIR: 'frontend/dist'
      - name: Deploy to Staging
        if: github.ref == 'refs/heads/develop'
        uses: jakejarvis/s3-sync-action@master
        with:
          args: --delete --follow-symlinks
        env:
          AWS_S3_BUCKET: ${{ secrets.STAGING_S3_BUCKET }}
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          SOURCE_DIR: 'frontend/dist'
```

## Veritabanı Migrasyonları

Veritabanı migrasyonları, dağıtım sürecinde kritik bir aşamadır ve dikkatli planlanmalıdır.

### MongoDB Migrasyonları

```bash
# Migrasyon betiği çalıştırma
cd backend
NODE_ENV=${ENV} npm run migrate:up

# Belirli bir migrasyona geri dönme
NODE_ENV=${ENV} npm run migrate:down -- --to 20230315121517_add_user_roles
```

### İlişkisel Veritabanı Migrasyonları (PostgreSQL)

```bash
# Migrasyon betiği çalıştırma
cd backend
npx knex migrate:latest --env ${ENV}

# Belirli bir migrasyona geri dönme
npx knex migrate:rollback --env ${ENV}
```

## Ölçeklendirme

NexFit uygulaması, artan yük altında otomatik olarak ölçeklenecek şekilde tasarlanmıştır.

### Kubernetes ile Otomatik Ölçeklendirme

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nexfit-api
  namespace: nexfit
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nexfit-api
  minReplicas: 3
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

### Manuel Ölçeklendirme

```bash
# Replika sayısını manuel olarak artırma
kubectl scale deployment nexfit-api -n nexfit --replicas=5

# Pod sayısını kontrol etme
kubectl get pods -n nexfit -l app=nexfit-api
```

## İzleme ve Günlük Kaydı

### Prometheus ve Grafana Kurulumu

```bash
# Prometheus operatörünü kurma
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace

# Grafana'ya erişim
kubectl port-forward svc/prometheus-grafana -n monitoring 3000:80
# Tarayıcıda http://localhost:3000 adresine git (kullanıcı adı: admin, şifre: prom-operator)
```

### ELK Stack ile Günlük Kaydı

```bash
# ELK Stack kurulumu
helm repo add elastic https://helm.elastic.co
helm install elasticsearch elastic/elasticsearch --namespace logging --create-namespace
helm install kibana elastic/kibana --namespace logging
helm install filebeat elastic/filebeat --namespace logging

# Kibana'ya erişim
kubectl port-forward svc/kibana-kibana -n logging 5601
# Tarayıcıda http://localhost:5601 adresine git
```

## Geri Alma Stratejisi

Hatalı dağıtımlar için geri alma prosedürü:

### Kubernetes ile Geri Alma

```bash
# Önceki sürüme geri dönme
helm rollback nexfit-api 1 -n nexfit

# Belirli bir revision'a geri dönme
helm history nexfit-api -n nexfit
helm rollback nexfit-api 2 -n nexfit
```

### Manuel Geri Alma

```bash
# Önceki imaja geri dönme
kubectl set image deployment/nexfit-api -n nexfit nexfit-api=nexfit/api:previous-version

# Değişikliği doğrulama
kubectl rollout status deployment/nexfit-api -n nexfit
```

## Güvenlik Kontrolleri

Dağıtım öncesi güvenlik kontrollerini otomatikleştirmek için:

```bash
# Docker imajlarında güvenlik taraması
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image nexfit/api:latest

# Kubernetes manifestolarında güvenlik taraması
helm template nexfit-api ./nexfit-api | kubesec scan -

# Dependency güvenlik taraması
cd backend
npm audit --audit-level=high
```

## Performans İzleme

Dağıtım sonrası performans izleme:

```bash
# API yanıt sürelerini izleme
kubectl port-forward svc/prometheus-operated -n monitoring 9090
# Grafana'da API Latency paneline git

# Yük testi çalıştırma
cd performance-tests
k6 run api-load-test.js --vus 50 --duration 5m
```

## Sorun Giderme

### Yaygın Dağıtım Sorunları

1. **Pod Başlatılamıyor**:
   ```bash
   kubectl describe pod ${POD_NAME} -n nexfit
   kubectl logs ${POD_NAME} -n nexfit
   ```

2. **Veritabanı Bağlantı Sorunları**:
   ```bash
   # MongoDB bağlantı kontrolü
   kubectl exec -it ${POD_NAME} -n nexfit -- mongo --host ${MONGO_HOST} --eval "db.adminCommand('ping')"
   ```

3. **CPU/Bellek Sorunları**:
   ```bash
   kubectl top pods -n nexfit
   kubectl describe node ${NODE_NAME}
   ```

4. **Ağ Sorunları**:
   ```bash
   kubectl exec -it ${POD_NAME} -n nexfit -- curl -v http://nexfit-api-svc:3000/health
   ```

---

## Ek Kaynaklar

- [Kubernetes Resmi Dokümantasyonu](https://kubernetes.io/docs/)
- [Helm Rehberi](https://helm.sh/docs/)
- [Docker Dokümantasyonu](https://docs.docker.com/)
- [MongoDB İşlemleri Kılavuzu](https://www.mongodb.com/docs/manual/administration/production-notes/)
- [NexFit DevOps İnternal Wiki](https://wiki.internal.nexfit.com/devops)

---

*Bu belge, NexFit DevOps ekibi tarafından yönetilmektedir. Sorularınız veya katkılarınız için devops@nexfit.com adresine e-posta gönderebilirsiniz.*

*Son Güncelleme: Mart 2025* 