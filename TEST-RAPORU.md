# Doğrulama raporu — 1 Kasım 2026

## Gerçekleştirilen kontroller

- JavaScript modüllerinin sözdizimi kontrolü ve CSS/JS üretim derlemesi.
- Eski lojistik işlevlerinin regresyon testleri: taslak/aktif/teslim, eş zamanlı kayıt çakışmaları, özel takip, sayaçlar, çöp/geri alma, finans, teklif, blog ve PDF harita bağlantıları.
- Yeni CMS: ilk yönetici, oturum, yanlış anahtar, başlıkla kimlik taklidi reddi, personel izinleri, pasifleştirme, XSS/URL doğrulama, içerik kaydı/çakışması, çeviri, SEO yönlendirmesi, görsel yükleme ve blog.
- Çerez testi: izin öncesi dış ölçüm scripti yok; zorunlu-only seçiminde yok; ayrı kategoriler uygulanıyor.
- İzole HTTP + DOM testleri: ana sayfa, Almanca geri dönüş, takip, blog, eski operasyon ekranı ve CMS bölümleri script hatası vermeden çalıştı.
- PDF'lerde yeni dil için İngilizce geri dönüş ve kaydedilmiş sözlük desteği; harita bağlantıları.
- Sabit içerik anahtarlarının şablona yeni öğe eklenince korunması.
- Tarayıcıda masaüstü ana sayfa ve kartlar görsel olarak incelendi; açık temadaki düşük kontrastlı metinler için düzeltme yapıldı. Yönetim giriş ekranı görüldü.
- Üretim veri görüntüsündeki yük→talep, sevkiyat→yük ve olay→sevkiyat ilişkilerinde sahipsiz kayıt bulunmadı.

Son otomatik koşunun sayısı ve sonuçları `TEST-SONUCLARI.txt` dosyasındadır.

## Doğrulanmamış alanlar

- Hostinger hesabında gerçek dağıtım, MariaDB bağlantısı ve tüm SQL sorgularının o sunucudaki çalışması. Test veritabanı SQLite adaptörüdür; SQL uyarlaması ayrıca birim testlidir.
- Gerçek telefon/iOS/Android görünümü, tüm responsive ekranların görsel kontrolü, gerçek cihaz performansı ve Lighthouse puanı.
- Gerçek GA4, Search Console, Meta hesapları ve üretim çerez envanteri; kimlikler henüz verilmedi.
- Şirketin resmî bilgileri ve hukuki metinlerin işletmeye özel son onayı.

Bunlar kılavuzdaki canlı kabul testinde tamamlanmalıdır. Bir SEO skoru, hukuki uygunluk garantisi veya %100 Hostinger doğrulaması verilmez.

## Veri aktarım notu

Anlık görüntü: 31 Ekim 2026. 3 yük, 3 sevkiyat, 6 olay, 1 talep, 1 PDF metadata/snapshot kaydı. Bir PDF snapshot üzerinden yeniden üretildi; orijinal nesne deposundan birebir kopyalanmadı. Yeni başlangıç SQL'i ve PDF manifesti bu bilgiyi içerir. Kaynaklar özel iş verileridir; açık Git deposuna veya statik web köküne konulmamalıdır.
