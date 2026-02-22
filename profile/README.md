![banner](./assets/banner.png)

Bu doküman, mevcut Lider kod tabanına eklenen **LiderBildirim** (bildirim sistemi) geliştirmelerinin kısa ve yüksek seviyede özetidir.

## Amaç

LiderBildirim, yönetim panelinden bildirim kanalları tanımlamayı, bu kanalları tetikleyicilerle ilişkilendirmeyi ve farklı servisler (Email, Mattermost, Telegram, Apprise vb.) üzerinden test/persist etmeyi hedefler.

![mimari](./assets/liderbildirim_mimari.png)

## Nerede Konumlanıyor?

- Frontend: `liderui`
- Backend model/catalog: `liderapi`
- UI giriş noktası: `liderui/src/views/Settings/ServerSettings/ServerSettings.vue`
- Ana form: `liderui/src/views/Settings/ServerSettings/forms/NotificationSettingsForm.vue`
- Kanal düzenleme popup'ı: `liderui/src/views/Settings/ServerSettings/forms/NotificationChannelDialog.vue`

## Veri Modeli (Özet)

Frontend ve backend tarafı aynı kavramlar etrafında hizalanmıştır:

- `NotificationSettings`
- `NotificationChannel`
- `NotificationServiceConfig`
- `NotificationTrigger`
- `ServiceTypeSchema` / `ServiceFieldSchema`

Frontend model dosyaları: `liderui/src/models/notification/`  
Backend model dosyaları: `liderapi/src/main/java/tr/org/lider/models/notification/`

## Katalog Mantığı

Trigger ve servis tipleri backend tarafında tek kaynak olacak şekilde tanımlanır:

- `liderapi/src/main/java/tr/org/lider/models/notification/NotificationCatalogDefaults.java`

Bu yapı sayesinde frontend, tetikleyici ve servis şemalarını backend’den alıp dinamik render eder.

## API Entegrasyonu (Temel)

Frontend servis katmanı:

- `GET /api/lider/settings/notification-settings`
- `POST /api/lider/settings/update/notification-settings`
- `POST /api/lider/settings/test/notification-service`

Kod: `liderui/src/services/Settings/ServerSettingsService.js`

## Eklenen Başlıca UI Özellikleri

- Bildirim kanalı listeleme ve kanal CRUD akışı
- Kanal popup’ında trigger seçimi (gruplu gösterim)
- Trigger arama filtresi (contains tabanlı)
- Arama aktifken trigger akordiyonlarını otomatik açma
- Arama temizlenince trigger akordiyonlarını varsayılan kapalı hale döndürme
- Servis ekleme/çıkarma ve servis bazlı dinamik alan render etme
- Servis ikonları (tablo, servis başlığı, servis menüsü)
- Tabloda trigger grubu özet gösterimi + detay tooltip
- Tabloda servis ikon tooltip’leri
- Kanal kopyalama (duplicate) aksiyonu
- Apprise servisi için dokümantasyon yönlendirme linki

