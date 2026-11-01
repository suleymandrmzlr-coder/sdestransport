# Son durum — 1 Kasım 2026

Hostinger'e taşınmak üzere hazırlanan bağımsız CMS paketi. Kod ve veritabanı dosyaları tamamlandı; son otomatik kontrolde 49/49 test geçti. Yayındaki eski site veya DNS değiştirilmedi.

İlk adım HOSTINGER-KURULUM.md. Node.js 24 uygulaması kurun, boş MariaDB'ye database/sdes-initial.sql aktarın, ortam değişkenlerini ayarlayın ve /cms üzerinden kendi tam yetkili hesabınızı oluşturun. Hazır yönetici şifresi yoktur.

Henüz doğrulanmayanlar: gerçek Hostinger/MariaDB üzerinde dağıtım ve kabul testi, gerçek mobil cihaz görsel kontrolleri, gerçek ölçüm hesapları ve şirketin nihai hukuki/iletişim bilgileri. Testlerin kapsamı TEST-RAPORU.md dosyasındadır.

Gelecekte geliştirme: server/ ve lib/ iş kurallarını, public/ şablonlarını ve mevcut data-cms-node anahtarlarını koruyun. İçerikler veritabanındadır. Kod değişikliği öncesi canlı DB yedeği alın. Başlangıç SQL'ini mevcut veritabanının üzerine tekrar yüklemeyin.
