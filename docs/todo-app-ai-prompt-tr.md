# Yapılacaklar Listesi Uygulaması (Expo + Web + Firebase) — Yapay Zeka Geliştirme İstemi

Aşağıdaki gereksinimlere göre **tek kod tabanından mobil (Expo/React Native) ve web (React Native Web veya uyumlu React yaklaşımı)** destekleyen bir **Yapılacaklar Listesi (Todo)** uygulaması geliştir.

## 1) Amaç
- Kullanıcılar e-posta/şifre ile kayıt olup giriş yapabilsin.
- Her kullanıcı kendi görevlerini oluşturup yönetebilsin (CRUD).
- Görevler Firestore üzerinde saklansın ve gerçek zamanlı güncellensin.
- Görevler sıralanabilsin/filtrelenebilsin.
- Bitiş tarihi yaklaşan görevler için bildirim altyapısı kurulsun (FCM).
- Uygulama mobil ve web’de responsive çalışsın.

## 2) Teknoloji Yığını
- **Frontend (Mobil):** Expo + React Native
- **Frontend (Web):** React.js (tercihen Expo Web / React Native Web uyumlu)
- **Backend servisleri:** Firebase
  - Authentication (Email/Password)
  - Firestore
  - Cloud Messaging (FCM)
- **Durum yönetimi:** Context API veya Zustand (hafif ve anlaşılır)
- **Form doğrulama:** Yup/Zod + React Hook Form (tercih edilen)

## 3) Zorunlu Özellikler

### 3.1 Kimlik Doğrulama
- Kayıt olma ekranı (email, password, password tekrar)
- Giriş ekranı (email, password)
- Oturum kapatma
- Oturum persist (uygulama yeniden açıldığında kullanıcı durumu korunmalı)

### 3.2 Görev Yönetimi (CRUD)
Her görev alanları:
- `title` (zorunlu, string)
- `description` (opsiyonel, string)
- `due_date` (opsiyonel, timestamp)
- `completed` (boolean)
- `user_id` (kullanıcı UID)
- `created_at` (timestamp)
- `updated_at` (timestamp)

İşlevler:
- Görev ekleme
- Görevleri listeleme (sadece giriş yapan kullanıcıya ait)
- Görev düzenleme
- Görev silme
- Görevi tamamlandı/tamamlanmadı olarak işaretleme

### 3.3 Listeleme, Sıralama, Filtreleme
- Bitiş tarihine göre sıralama (artan/azalan)
- Filtreleme:
  - Tümü
  - Tamamlananlar
  - Tamamlanmayanlar

### 3.4 Gerçek Zamanlı Veri
- Firestore `onSnapshot`/realtime listener kullan.
- Eklenen/güncellenen/silinen görevler UI’da anında yansısın.

### 3.5 Bildirimler (FCM)
- Temel FCM entegrasyonu kur.
- `due_date` yaklaşan görevler için bildirim gönderim taslağı oluştur.
- Not: Üretimde zamanlanmış bildirim için Cloud Functions + Scheduler tasarımı ekle.

## 4) Ekranlar ve UI Gereksinimleri

### 4.1 Auth Ekranları
- Login Screen
- Register Screen
- Basit, anlaşılır validasyon hata mesajları

### 4.2 Ana Görev Ekranı
- Üst kısım:
  - Uygulama adı
  - Çıkış Yap butonu
- İçerik:
  - Filtre ve sıralama kontrolleri
  - Görev kartları listesi
- Sağ altta FAB:
  - Yeni görev ekleme modal/formunu açar

### 4.3 Görev Kartı
- Başlık
- Açıklama (varsa)
- Bitiş tarihi (varsa)
- Tamamlandı checkbox/switch
- Düzenle butonu
- Sil butonu

### 4.4 Responsive Tasarım
- Mobilde büyük dokunmatik alanlar
- Web’de geniş ekranda daha iyi listelenme/boşluk kullanımı
- Erişilebilirlik (minimum): okunabilir font, kontrast, odak durumları

## 5) Firestore Veri Modeli ve Güvenlik

### 5.1 Koleksiyon Yapısı
- `tasks` koleksiyonu
  - Belge alanları:
    - `title`
    - `description`
    - `due_date`
    - `completed`
    - `user_id`
    - `created_at`
    - `updated_at`

### 5.2 Örnek Veri
```json
{
  "tasks": [
    {
      "title": "Market alışverişi",
      "description": "Süt, Yumurta, Ekmek",
      "due_date": "2026-04-05T12:00:00Z",
      "completed": false,
      "user_id": "USER_UID_12345"
    },
    {
      "title": "Projeyi Tamamla",
      "description": "Uygulamanın kodlarını bitir",
      "due_date": "2026-04-10T17:00:00Z",
      "completed": false,
      "user_id": "USER_UID_12345"
    }
  ]
}
```

### 5.3 Firestore Güvenlik Kuralları (beklenen)
- Kullanıcı sadece kendi görevlerini okuyup yazabilsin.
- `request.auth.uid == resource.data.user_id` mantığı uygulanmalı.

## 6) Mimari ve Kodlama Beklentileri
- Katmanlı yapı öner:
  - `src/services/firebase/*`
  - `src/features/auth/*`
  - `src/features/tasks/*`
  - `src/components/*`
  - `src/navigation/*`
- Ortak tipler ve yardımcı fonksiyonlar ayrı tutulmalı.
- Hata yönetimi kullanıcı dostu olmalı.
- Kod modüler, okunabilir ve TypeScript destekli olmalı.

## 7) Çıktı Beklentisi
Aşağıdakileri üret:
1. Proje klasör yapısı
2. Kurulum adımları (`npm` veya `pnpm`)
3. Gerekli `.env` değişkenleri (Firebase config)
4. Tüm temel ekranların ve servislerin kodu
5. Firestore güvenlik kuralları örneği
6. FCM kurulum notları (mobil + web farklarıyla)
7. Çalıştırma komutları:
   - Mobil: `npx expo start`
   - Web: `npx expo start --web` (veya seçilen web setup)

## 8) Kabul Kriterleri
- Kullanıcı kayıt/giriş/çıkış akışı sorunsuz.
- Görev CRUD işlemleri hatasız.
- Kullanıcı yalnızca kendi görevlerini görür.
- Realtime güncelleme çalışır.
- Filtreleme ve sıralama aktif.
- Mobil + web arayüz kullanılabilir ve responsive.
- Temel bildirim altyapısı entegre edilmiş.

## 9) Ek Notlar
- Üretim için güvenlik (rate limit, abuse, doğrulama) ve hata loglama önerileri ekle.
- Gereksiz bağımlılıklardan kaçın.
- Kod içinde TODO yorumları ile ileri geliştirme alanlarını belirt.
