# GitLab CI/CD Pipeline Detaylı Rehber


## Bölüm 1: Pipeline Nedir?


### Pipeline'ı Anlamak


🏭 Bir fabrika düşünün:


1. Hammadde giriyor
2. Çeşitli işlemlerden geçiyor
3. Son ürün çıkıyor

Pipeline da aynen böyle çalışır:


1. Kod giriyor (your code)
2. Build, test, security check gibi işlemlerden geçiyor
3. Çalışan uygulama çıkıyor (deployment)

### Pipeline Ne İşe Yarar?

1. Otomatik test eder
2. Kodu derler
3. Güvenlik kontrolü yapar
4. Canlıya alır
5. Hataları erken yakalar

## Bölüm 2: Pipeline Kurulumu


### 1. .gitlab-ci.yml Dosyası


📝 Bu dosya pipeline'ın kalbidir. Projenizin ana dizininde oluşturulur.

```yaml
# En basit .gitlab-ci.yml örneği
job1:
  script: echo "Merhaba Pipeline!"
```


### 2. GitLab Runner


🏃‍♂️ Pipeline'ları çalıştıran bir servistir.

**Kurulum Adımları:**


1. GitLab Runner indirme:

```other
# Linux için
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
sudo apt-get install gitlab-runner

# Windows için
# GitLab Runner Windows Installer'ı indir ve kur
```

2. Runner'ı kaydetme:

```other
sudo gitlab-runner register
```

3. Sorulara cevap verme:
- GitLab URL: [https://gitlab.com/](https://gitlab.com/)
- Registration token: (GitLab'dan alınacak)
- Description: My-Runner
- Tags: my-tag
- Executor: docker, shell, etc.

## Bölüm 3: Pipeline Temel Kavramları


### 1. Jobs (İşler)


👨‍💼 Pipeline'daki her bir görev

```yaml
job1:
  script: 
    - echo "İlk görev"

job2:
  script:
    - echo "İkinci görev"
```


### 2. Stages (Aşamalar)


📑 İşlerin gruplandığı aşamalar

```yaml
stages:
  - build
  - test
  - deploy

build_job:
  stage: build
  script:
    - echo "Derleme yapılıyor"

test_job:
  stage: test
  script:
    - echo "Testler çalışıyor"
```


### 3. Script


📜 İşin içinde çalışacak komutlar

```yaml
build:
  script:
    - npm install
    - npm run build
    - echo "Build tamamlandı"
```


### 4. Artifacts (Çıktılar)


📦 İşler arası paylaşılan dosyalar

```yaml
build:
  script:
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 week
```


## Bölüm 4: İlk Pipeline'ımızı Oluşturalım


### Adım 1: Basit Bir Pipeline


```yaml
# .gitlab-ci.yml
stages:
  - build
  - test

build_job:
  stage: build
  script:
    - echo "Proje derleniyor..."
    - mkdir build
    - touch build/app.txt

test_job:
  stage: test
  script:
    - echo "Testler çalışıyor..."
    - test -f build/app.txt
```


### Adım 2: Node.js Projesi İçin Pipeline


```yaml
image: node:16

stages:
  - build
  - test
  - deploy

cache:
  paths:
    - node_modules/

install_dependencies:
  stage: build
  script:
    - npm install
  artifacts:
    paths:
      - node_modules/

run_tests:
  stage: test
  script:
    - npm run test

deploy_staging:
  stage: deploy
  script:
    - echo "Staging ortamına deploy ediliyor..."
  environment:
    name: staging
```


## Bölüm 5: Pipeline Değişkenleri


### 1. Predefined Variables


🔑 GitLab'ın hazır sunduğu değişkenler

```yaml
job_info:
  script:
    - echo "Branch adı: $CI_COMMIT_BRANCH"
    - echo "Commit: $CI_COMMIT_SHA"
```


### 2. Custom Variables


✏️ Kendi tanımladığımız değişkenler

**GitLab UI'dan Tanımlama:**


1. Settings > CI/CD > Variables
2. Add Variable
3. Key: DATABASE_URL
4. Value: postgresql://db:5432/myapp

```yaml
db_job:
  script:
    - echo "Database URL: $DATABASE_URL"
```


### 3. Masked Variables


🕶️ Gizli değerler için (şifreler, API anahtarları)

GitLab UI'da "Masked" seçeneğini işaretleyin.

## Bölüm 6: Gerçek Dünya Pipeline Örnekleri


### 1. Java Spring Boot Uygulaması


```yaml
image: maven:3.8-openjdk-11

stages:
  - build
  - test
  - sonar
  - deploy

variables:
  MAVEN_OPTS: "-Dmaven.repo.local=.m2/repository"

cache:
  paths:
    - .m2/repository

build:
  stage: build
  script:
    - mvn clean package
  artifacts:
    paths:
      - target/*.jar

test:
  stage: test
  script:
    - mvn test

sonar:
  stage: sonar
  script:
    - mvn sonar:sonar

deploy_staging:
  stage: deploy
  script:
    - 'curl --request POST --header "PRIVATE-TOKEN: $DEPLOY_TOKEN" "https://gitlab.example.com/api/v4/projects/$CI_PROJECT_ID/deployments"'
  environment:
    name: staging
```


### 2. React Frontend Uygulaması


```yaml
image: node:16

stages:
  - install
  - lint
  - test
  - build
  - deploy

variables:
  npm_config_cache: "$CI_PROJECT_DIR/.npm"

cache:
  paths:
    - .npm/
    - node_modules/

install:
  stage: install
  script:
    - npm ci

lint:
  stage: lint
  script:
    - npm run lint

test:
  stage: test
  script:
    - npm run test

build:
  stage: build
  script:
    - npm run build
  artifacts:
    paths:
      - build/

deploy:
  stage: deploy
  script:
    - npm install -g firebase-tools
    - firebase deploy --token $FIREBASE_TOKEN
```


## Bölüm 7: Pipeline Best Practices


### 1. Hız Optimizasyonu


⚡ Pipeline'ı hızlandırma yöntemleri

```yaml
# Cache kullanımı
cache:
  key: ${CI_COMMIT_REF_SLUG}
  paths:
    - .npm/
    - node_modules/

# Parallel jobs
test:
  parallel: 3
```


### 2. Conditional Jobs


🔄 Koşullu işler

```yaml
deploy_prod:
  script:
    - ./deploy.sh
  only:
    - master
  when: manual
```


### 3. Environment Kullanımı


🌍 Farklı ortamlar için

```yaml
deploy_staging:
  script:
    - ./deploy.sh
  environment:
    name: staging
    url: https://staging.example.com
```


## Bölüm 8: Pipeline Sorun Giderme


### 1. Common Issues


❌ Sık karşılaşılan sorunlar:


1. Runner bağlantı sorunu

```other
gitlab-runner verify
```

2. Cache sorunu

```yaml
cache:
  key: ${CI_COMMIT_REF_SLUG}
  policy: pull-push
```

3. Artifact yükleme hatası

```yaml
artifacts:
  when: always  # Hata durumunda da artifact'ı kaydet
```


### 2. Debug Teknikleri


🔍 Hata ayıklama:


1. CI Debug Trace aktif etme

```yaml
job:
  script:
    - set -x  # Debug modu
    - ./script.sh
```

2. Pipeline'ı lokalde test etme

```other
gitlab-runner exec docker job-name
```


## Bölüm 9: İleri Seviye Konular


### 1. Multi-Stage Docker Builds


```yaml
build:
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t myapp .
```


### 2. Review Apps


```yaml
review_app:
  script:
    - ./deploy-review-app.sh
  environment:
    name: review/$CI_COMMIT_REF_NAME
    url: https://$CI_ENVIRONMENT_SLUG.example.com
```


### 3. Auto DevOps


GitLab'ın otomatik pipeline oluşturma özelliği:


1. Settings > CI/CD > Auto DevOps
2. "Enable Auto DevOps" seç

## Bonus: Pipeline Template Örnekleri


### 1. Genel Template


```yaml
include:
  - template: Jobs/Build.gitlab-ci.yml
  - template: Security/SAST.gitlab-ci.yml

stages:
  - build
  - test
  - security
  - deploy

variables:
  # Global değişkenler
  APP_NAME: "my-app"
  
default:
  # Tüm jobs için default ayarlar
  tags:
    - docker
  
.base_rules:
  # Ortak kurallar
  rules:
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH
      when: always
    - when: manual

# Jobs...
```


## Pratik Alıştırmalar


### Alıştırma 1: Basit Pipeline

1. Yeni bir GitLab projesi oluşturun
2. `.gitlab-ci.yml` ekleyin
3. En basit haliyle çalıştırın
4. Pipeline sonuçlarını inceleyin

### Alıştırma 2: Test Coverage

1. Test coverage raporu oluşturun
2. Artifact olarak saklayın
3. Badge olarak gösterin

### Alıştırma 3: Staging/Production

1. İki ortam tanımlayın
2. Manuel deployment ekleyin
3. Rollback mekanizması kurun


----


