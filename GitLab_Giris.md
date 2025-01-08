# GitLab Hakkında
> Bu doküman sadece giriş seviyesinde kafanızda bir şeyler canlanması için hazırlanmıştır. Pratik yaparak GitLab dökümanlarını inceleyerek daha derin bilgiler elde edebilirsiniz.

## GitLab Nedir?


GitLab, yazılım geliştirme süreçlerini tek bir platformda toplayan, açık kaynaklı bir DevOps platformudur.  
Git tabanlı versiyon kontrol sistemini temel alarak, proje yönetimi, continuous integration (CI), continuous deployment (CD), monitoring ve security gibi birçok özelliği içinde barındırır.

### GitLab'ın Temel Özellikleri:

- Source Code Management (SCM)
- Issue Tracking
- CI/CD Pipeline Yönetimi
- Container Registry
- Security Scanning
- Wiki ve Documentation
- Code Review ve Merge Request Yönetimi

## 1. En Temel Kavramlar


### Repository (Repo)


🏠 Düşünün ki bu sizin eviniz. Tüm eşyalarınızı (kodlarınızı) bu evde saklıyorsunuz. GitLab'daki repository, projenizin bütün dosyalarının saklandığı yerdir.

### Branch (Dal)


🌳 Bir ağacı düşünün:


- Ana gövde (main/master branch): Ağacın gövdesi
- Dallar: Gövdeden ayrılan kollar

Örnek: Bir web sitesi yapıyorsunuz


- Ana dal (main): Çalışan, stable site
- Yeni dal (feature branch): "login-sayfasi" adında yeni bir dal açıp, orada login sayfası geliştiriyorsunuz
- Böylece ana sitenizi bozmadan yeni özellik geliştirebiliyorsunuz

### Commit


📸 Fotoğraf çekmek gibidir. Yaptığınız değişikliklerin bir fotoğrafını çekersiniz.


- Her commit bir anlık görüntüdür
- Her commit'in bir mesajı vardır: "Login sayfası eklendi"
- Her commit'in kim tarafından, ne zaman yapıldığı bellidir

### Push


📤 WhatsApp'ta mesaj göndermek gibidir. Local bilgisayarınızda yaptığınız değişiklikleri (commit'leri) GitLab'a gönderirsiniz.

```other
git push origin master  # "origin" sunucuya, "master" dalına gönder
```


### Pull


📥 WhatsApp'ta mesaj almak gibidir. GitLab'daki değişiklikleri kendi bilgisayarınıza alırsınız.

```other
git pull  # GitLab'daki değişiklikleri al
```


## 2. Merge (Birleştirme) Kavramları


### Merge Request (MR)


✍️ Bir izin isteği gibi düşünün:


1. Yeni bir özellik geliştirdiniz (login-sayfasi branch'inde)
2. Bunu ana projeye (main branch) eklemek istiyorsunuz
3. "Merge Request" açıyorsunuz: "Login sayfasını ana siteye ekleyebilir miyim?" gibi

### Merge


🤝 İki dalı birleştirmek. Örnek:


- login-sayfasi dalındaki değişiklikleri
- main dala birleştiriyorsunuz
- Artık login sayfası ana sitenin bir parçası

## 3. Pipeline Hakkında


### Pipeline


🏭 Bir fabrika bandı gibi düşünün:


1. Hammadde giriyor (kod)
2. İşlemlerden geçiyor (build, test)
3. Ürün çıkıyor (çalışan uygulama)

### Job (İş)


📋 Pipeline'daki her bir görev. Örnek:


- build_job: Uygulamayı derle
- test_job: Testleri çalıştır
- deploy_job: Canlıya al

### Stage (Aşama)


📑 Pipeline'daki ana bölümler. Örnek:

```yaml
stages:
  - build    # 1. aşama
  - test     # 2. aşama
  - deploy   # 3. aşama
```


## 4. Güvenlik ve Yapılandırma Terimleri


### Protected (Korumalı)


🔒 Bir evin ana kapı anahtarı gibi:


- Sadece yetkili kişiler değişiklik yapabilir
- Örnek: master branch'i protected yaparsanız, sadece yetkili kişiler buraya kod ekleyebilir

### Masked (Gizlenmiş)


👁️ Banka şifresi gibi:


- Değeri gizli tutulur
- Pipeline loglarında *** şeklinde görünür
- Örnek: Database şifresi

### Environment (Ortam)


🌍 Sitenizin farklı versiyonlarının çalıştığı yerler:


- Development: Geliştirme ortamı ([test.sitem.com](http://test.sitem.com))
- Production: Gerçek ortam ([www.sitem.com](http://www.sitem.com))

### .yml Dosyası


📝 Bir yemek tarifi gibi:


- Pipeline'ın nasıl çalışacağını adım adım anlatır
- Ne yapılacağını
- Hangi sırayla yapılacağını
- Hangi koşullarda yapılacağını belirtir

```yaml
# Örnek basit .yml dosyası
build_job:  # İşin adı
  stage: build  # Hangi aşamada
  script:  # Ne yapılacak
    - echo "Hello World"  # Komutu
```


## 5. Git Komutları Açıklaması


### git add


📋 Değişiklikleri listeye alma:

```other
git add dosya.txt  # Tek dosya ekle
git add .  # Tüm değişiklikleri ekle
```


### git commit


💾 Değişiklikleri kaydetme:

```other
git commit -m "Login sayfası eklendi"
```


### git push origin master


📤 Değişiklikleri GitLab'a gönderme:


- origin: GitLab sunucusunun takma adı
- master: Göndermek istediğiniz dal

### git checkout


🚗 Dallar arası geçiş yapmak:

```other
git checkout login-sayfasi  # Login sayfası dalına geç
```


## 6. Örnek Senaryo


Diyelim ki bir e-ticaret sitesi geliştiriyorsunuz:


1. **Repository Oluşturma:**

    ```other
    # Projeyi GitLab'a bağlama
    git init
    git remote add origin git@gitlab.com:magazam/site.git
    ```

2. **Yeni Özellik Geliştirme:**

    ```other
    # Yeni dal oluştur
    git checkout -b odeme-sayfasi
    
    # Değişiklikler yap ve kaydet
    git add .
    git commit -m "Ödeme sayfası eklendi"
    
    # GitLab'a gönder
    git push origin odeme-sayfasi
    ```

3. **Merge Request Açma:**
	- GitLab'da "New Merge Request" tıkla
	- odeme-sayfasi'nı master'a birleştirmek için istek aç
4. **Pipeline Çalışır:**
	- Otomatik testler çalışır
	- Kod kalitesi kontrol edilir
	- Her şey OK ise birleştirme yapılır

Bu şekilde adım adım ilerleyerek, güvenli bir şekilde yeni özellikler ekleyebilirsiniz.
----

# Branch ve Merge İşlemleri: Gerçek hayata yakın örnekler


## 1. Branch Türleri ve Kullanım Alanları


### Main/Master Branch


🏢 Ana bina gibidir. Her zaman çalışır durumda ve stable olmalıdır.

**Gerçek hayata yakın örnek:**

Bir e-ticaret sitesinin canlı ortamda çalışan versiyonu. Müşteriler alışveriş yaparken sorun yaşamamalı.

### Development Branch


🏗️ Test binası gibidir. Yeni geliştirmeler önce burada test edilir.

**Gerçek hayata yakın örnek:**

[test.magazam.com](http://test.magazam.com) gibi bir ortamda çalışan versiyon. Yeni özellikler önce burada denenir.

### Feature Branches


🏠 Her yeni özellik için ayrı bir çalışma alanı.

**Gerçek hayata yakın örnek:**

```other
# Yeni ödeme yöntemi eklerken
git checkout -b feature/kredikart-odeme

# Yeni üyelik sistemi eklerken
git checkout -b feature/google-login
```


### Hotfix Branches


🚑 Acil tamir gereken durumlar için.

**Gerçek Dünya Örneği:**

```other
# Canlı sistemde ödeme hatası varsa
git checkout -b hotfix/odeme-hatasi-fix
```


### Release Branches


📦 Sürüm hazırlığı için kullanılır.

**Gerçek Dünya Örneği:**

```other
# Yeni versiyon çıkarırken
git checkout -b release/2.0.0
```


## 2. Ne Zaman Branch Oluşturulur?


### Yeni Özellik Geliştirirken


```other
graph TD
    A[Main] --> B[Development]
    B --> C[feature/yeni-ozellik]
    C --> B
    B --> A
```


**Gerçek hayata yakın örnek-1:**


- Durum: E-ticaret sitenize Apple Pay entegrasyonu ekleyeceksiniz
- Yaklaşım:
	1. `feature/apple-pay` branch'i oluştur
	2. Entegrasyon geliştirmesini yap
	3. Test et
	4. Development'a merge et
	5. Canlıya alma zamanı gelince main'e merge et

**Gerçek hayata yakın örnek-2:**


- Durum: Login sayfasına "Şifremi Unuttum" özelliği eklenecek
- Yaklaşım:

    ```other
    git checkout -b feature/sifremi-unuttum
    # ... geliştirmeler ...
    git commit -m "Şifremi unuttum sayfası ve email gönderme eklendi"
    ```


### Acil Düzeltme Gerektiğinde


```other
graph TD
    A[Main] --> B[hotfix/bug-fix]
    B --> A
    B --> C[Development]
```


**Gerçek Dünya Örneği:**


- Durum: Canlı sistemde ödeme alınamıyor
- Yaklaşım:
	1. `hotfix/payment-fix` branch'i main'den oluştur
	2. Hatayı düzelt
	3. Hem main'e hem development'a merge et

## 3. Merge İşlemi Ne Zaman Yapılır?


### Code Review Sonrası


👥 En az bir başka developer kodu inceledikten sonra

**Gerçek Dünya Örneği:**


1. Feature branch'te geliştirmeyi tamamladınız
2. Merge Request açtınız
3. Takım arkadaşınız kodu inceledi
4. Varsa düzeltmeleri yaptınız
5. Onay gelince merge edildi

### Test Başarılı Olduğunda


✅ Tüm testler geçtiğinde

**Gerçek Dünya Örneği:**

```yaml
# .gitlab-ci.yml
test_job:
  script:
    - npm run test
  only:
    - merge_requests
```


### Canlıya Alma Zamanı Geldiğinde


🚀 Sprint sonu veya release zamanı

**Gerçek Dünya Örneği:**


- Her ayın son Çarşamba günü yeni özellikler canlıya alınır
- Development → Main merge'ü yapılır

## 4. Dikkat Edilmesi Gereken Noktalar


### 1. Branch İsimlendirme


✍️ Açıklayıcı ve standart isimler kullanın

**Doğru Örnekler:**

```other
feature/user-login
hotfix/payment-error
release/2.1.0
```


**Yanlış Örnekler:**

```other
yeni-branch
ahmets-branch
fix
```


### 2. Commit Mesajları


📝 Ne yaptığınızı açıkça belirtin

**Doğru Örnekler:**

```other
git commit -m "Login form validasyonu eklendi"
git commit -m "Ödeme hatası düzeltildi: Timeout süresi 30 saniyeye çıkarıldı"
```


**Yanlış Örnekler:**

```other
git commit -m "fix"
git commit -m "güncelleme"
```


### 3. Merge Conflict Çözümü


⚡ Çakışmaları dikkatli çözün

**Gerçek Dünya Örneği:**

```other
# Conflict durumunda
git status  # Hangi dosyalarda conflict var gör
# Dosyaları aç ve konfliktleri çöz
git add .
git commit -m "Conflict çözüldü: Login sayfası stili güncellendi"
```


### 4. Regular Merge vs Squash Merge


🔄 Commit geçmişi önemli mi?

**Regular Merge:** Tüm commit geçmişi korunur

```other
git merge feature/login
```


**Squash Merge:** Tüm commitler tek bir commit olur

```other
git merge --squash feature/login
```


## 5. Best Practices


### 1. Sık Sık Main/Development ile Senkronize Olun


```other
# Feature branch'inizi güncel tutun
git checkout feature/yeni-ozellik
git pull origin development
```


### 2. Branch'leri Temiz Tutun

- İşi biten branch'leri silin
- Düzenli olarak local branch'leri temizleyin

```other
# Remote branch'i silme
git push origin --delete eski-branch

# Local branch'i silme
git branch -d eski-branch
```


### 3. Code Review Checklist


✅ Merge öncesi kontrol listesi:


- Kod standartlarına uygun mu?
- Testler yazıldı mı?
- Dokümantasyon güncellendi mi?
- Pipeline başarılı çalışıyor mu?

### 4. Merge Request Template Kullanın


📋 Her MR için standart template:

```other
## Ne Değişti
-- Yeni özellik eklendi
-- Bug fix yapıldı
-- Performans iyileştirmesi

## Test
-- Unit testler yazıldı
-- Manuel test yapıldı

## Dokümantasyon
-- API dokümantasyonu güncellendi
-- README güncellendi
```


## 6. Gerçek Dünya Senaryoları


### Senaryo 1: Sprint Ortası Feature Geliştirme

1. Development'tan feature branch oluştur
2. Geliştirmeyi yap
3. Test et
4. Code review al
5. Development'a merge et

### Senaryo 2: Acil Production Hatası

1. Main'den hotfix branch oluştur
2. Hızlı fix yap
3. Test et
4. Main'e merge et
5. Development'a da merge et

### Senaryo 3: Büyük Özellik Geliştirme

1. Feature branch oluştur
2. Alt feature branch'ler oluştur
3. Her alt özelliği ayrı geliştir
4. Ana feature branch'te birleştir
5. Development'a merge et

## 7. Sık Yapılan Hatalar ve Çözümleri


### 1. Büyük Merge Requestler


❌ **Hata:** Çok fazla değişikliği tek seferde merge etmek

✅ **Çözüm:** İşi küçük parçalara böl

### 2. Yetersiz Test


❌ **Hata:** Hemen merge etmek

✅ **Çözüm:** Test coverage kontrolü, manuel test

### 3. Eksik Code Review


❌ **Hata:** Hızlıca onay vermek

✅ **Çözüm:** Detaylı code review checklist kullan

## 8. Branch Stratejisi Seçimi


### GitFlow


Büyük projeler için:

```other
graph TD
    A[Main] --> B[Development]
    B --> C[Feature]
    B --> D[Release]
    A --> E[Hotfix]
```


### Trunk-Based Development


Küçük takımlar için:

```other
graph TD
    A[Main] --> B[Feature]
    A --> C[Hotfix]
```


## Sonuç


Branch ve merge işlemleri, yazılım geliştirme sürecinin kritik parçalarıdır. Doğru strateji ve pratiklerle:


- Kod kalitesi artar
- Hatalar azalır
- Takım çalışması kolaylaşır
- Deployment süreci güvenli hale gelir

----
> [mertugral](https://github.com/ertugralmert)
