# SDES Logistics — Hostinger taşıma kılavuzu

Hazırlanma: 1 Kasım 2026. Bu paket mevcut SDES tasarımını ve lojistik işlevlerini bağımsız bir içerik yönetim sistemine taşır. Eski yayındaki site ve DNS kayıtları değiştirilmedi.

## 1. Paket ve gereksinimler

- Hostinger **Business Web Hosting veya Cloud** üzerinde **Node.js Web App** gerekir. Bu bir WordPress veya yalnızca HTML/PHP paketi değildir.
- Uygulama: **Node.js 24.x**, Express; veritabanı: **MariaDB 10.6+**. Geliştirme testleri Node 24.22.1 ile çalıştırıldı.
- Mevcut uygun hosting paketiniz dışında zorunlu ücretli CMS, çeviri, dosya deposu veya yapay zekâ aboneliği kullanılmaz. Hosting, alan adı ve isteğe bağlı reklam harcamaları kendi hesaplarınıza aittir.
- `package.json` ZIP'in kökündedir. ZIP'i Node.js uygulaması yükleme ekranına verin; tüm arşivi sıradan bir `public_html` statik klasörü olarak yayınlamayın.
- Veritabanı dosyalarında özel iş bilgileri bulunur. SQL/JSON dosyalarını webden indirilebilir bir klasöre koymayın. Uygulama yalnızca gerekli `public` kaynaklarını servis eder.

## 2. Veritabanını oluşturun ve içe aktarın

Hostinger'in veritabanı yönetiminden yeni bir MariaDB/MySQL veritabanı ve buna bağlı kullanıcı oluşturun. Tam veritabanı adı, kullanıcı adı ve şifreyi saklayın; hPanel kullanıcı adlarına bir ön ek ekleyebilir.

**Önerilen yöntem — phpMyAdmin:**

1. Yeni, boş veritabanını seçin.
2. İçe aktar bölümünde `database/sdes-initial.sql` dosyasını seçin.
3. İçe aktarın. Bu dosya şemayı, dört dil için sayfa kayıtlarını ve mevcut operasyon kayıtlarını içerir.
4. Bu dosyayı çalışan/dolu bir veritabanına tekrar yüklemeyin. İlerideki kod güncellemelerinde başlangıç SQL'i tekrar kullanılmaz.

**Alternatif — komut satırı:**

Uygulama dizininde gerekli DB ortam değişkenlerini ayarladıktan sonra:

```bash
npm ci --include=dev
npm run db:install -- --import-existing
```

Sadece boş operasyon verisiyle başlamak istenirse `--import-existing` eklemeyin. İki kurulum yöntemini birden uygulamayın. İçe aktarma mevcut iş verilerini ezmez; dolu bir operasyon tablosu bulursa durur. `database/schema-mariadb.sql` yalnızca şemadır; tek başına tam site içe aktarımı değildir.

## 3. Node.js uygulamasını yükleyin

Hostinger panelinde **Websites / Create site / Web App / Upload your website files** akışını kullanın. Menü isimleri panel diline göre değişebilir. Son teslim ZIP'ini yükleyin.

| Ayar | Değer |
|---|---|
| Framework | Express; otomatik algılanmazsa Other |
| Node sürümü | 24.x |
| Proje kökü | ZIP kökü (`package.json` burada) |
| Bağımlılık yükleme | `npm ci --include=dev` |
| Build komutu | `npm run build` |
| Başlatma komutu | `npm start` |
| Entry file istenirse | `server/index.mjs` |
| Build output zorunlu istenirse | `.` — sunucu, lib, public ve database dışlanmamalı |

**Yalnızca `dist-public` klasörünü yayımlamayın.** Bu klasör optimize CSS/JS içerir; yönetim, veritabanı ve API sunucusu ayrıca çalışmalıdır. Hostinger'in atadığı `PORT` kullanılır; sabit bir port tanımlamayın. Build sırasında `esbuild` gerektiği için geliştirme bağımlılıkları da yüklenmelidir.

## 4. Ortam değişkenleri

hPanel uygulama ayarlarına şu değerleri ekleyin. `.env.example` yalnızca örnektir; içinde gerçek şifre yoktur.

| Değişken | Açıklama |
|---|---|
| `NODE_ENV` | `production` |
| `APP_ORIGIN` | `https://sdestransport.com` — sonunda `/` yok |
| `DB_HOST` | Hostinger'in verdiği veritabanı sunucusu; aynı hostingde genellikle `127.0.0.1` |
| `DB_PORT` | `3306` |
| `DB_NAME` | Tam veritabanı adı |
| `DB_USER` | Tam veritabanı kullanıcı adı |
| `DB_PASSWORD` | Oluşturduğunuz veritabanı şifresi |
| `SETUP_TOKEN` | İlk yönetici kurulumuna özel rastgele 64 karakterli hex anahtar |

Anahtar oluşturmak için kendi bilgisayarınızda veya güvenilir terminalinizde:

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

Çıkan değeri `SETUP_TOKEN` olarak kullanın. Örnek yer tutucuyu kullanmayın; sistem kabul etmez. Şifreleri veya bu anahtarı açık kaynak depoya/sohbete koymayın.

Geçici Hostinger adresinde deniyorsanız `APP_ORIGIN` o HTTPS adresi olmalıdır. Gerçek alan adına geçince `https://sdestransport.com` yapıp yeniden başlatın. Aksi hâlde kayıt/giriş işlemleri, güvenlik amacıyla “Invalid request origin” hatası verir.

## 5. İlk yönetici hesabı

1. HTTPS aktif olduktan sonra sitenin `/cms` adresini açın.
2. İlk kurulum ekranına kurulum anahtarını, adınızı, e-postanızı ve en az 12 karakterli güçlü şifrenizi girin.
3. İlk hesap **tam yetkili sahibi/yönetici** olur. Hazır veya herkesçe bilinen bir yönetici şifresi yoktur.
4. Hesap oluşturulduktan sonra `SETUP_TOKEN` ortam değişkenini kaldırın ve uygulamayı yeniden başlatın. Kullanıcılar veritabanında kalır.
5. **Kullanıcılar ve yetkiler** sayfasından personel oluşturun. Her alan için görüntüleme/düzenleme izinlerini belirleyin. Düzenleme izni görüntülemeyi de içerir. PDF izni, yük seçimi için operasyonları görüntüleme iznini ekler.

Ana yönetici pasifleştirilemez veya alt role düşürülemez. Personel pasifleştirilebilir. Şifre/yetki değişiminde ilgili oturumlar kapatılır. İzinler sadece ekrandaki düğmeleri gizlemez; sunucuda da denetlenir. İzinler bölüm bazlıdır; müşteriye/tekil yüke özel personel atama sistemi değildir.

## 6. Panelde günlük kullanım

| Bölüm | Yapılan işlem |
|---|---|
| Sayfalar ve içerik | Sayfa ve dil seçin; metin, başlık, bağlantı, görsel ve açıklamalarını düzenleyin; kaydedin. Arama ve sayfalama çok sayıda alanı yönetir. |
| Çeviri merkezi | Sayfa çevirileri veya buton/sistem sözlükleri; JSON dışa/içe aktarım; yeni dil ekleme/etkinleştirme. |
| Görsel kütüphanesi | PNG/JPEG/WebP yükleme (en fazla 8 MB); ortak logo, fotoğraf ve arka planları değiştirme. WebP ve küçük dosya boyutu tercih edin. |
| SEO ve URL yönetimi | Dil bazlı URL, meta başlık, açıklama, noindex. Ana sayfa yolu boş kalır. Eski sayfa URL'leri yönlendirilir. |
| Ölçüm bağlantıları | GA4 ölçüm kimliği, Meta Pixel numarası ve Search Console doğrulama kodu. Script kodu yapıştırılmaz. |
| İletişim ve sosyal medya | Şirket adı/adres/vergi bilgisi, e-posta, telefon, WhatsApp ve sosyal profil URL'leri. Bu bilgiler yeni PDF'lerin şirket alanlarına da önerilir. |
| Görünüm | Açık/koyu/sistem varsayılanı ve marka renkleri. Ziyaretçi kendi tema tercihini değiştirebilir. |
| Blog yönetimi | Kapak görseli, kategori, taslak/yayın/arşiv, tarih, içerik, kaynaklar ve dil metinleri. |
| Yük ve sevkiyat paneli | Mevcut tek iş oluşturma akışı; taslak, aktif taşıma, teslim, arşiv/çöp; talepler, finans, müşteriler, rotalar ve PDF'ler. |
| İşlem geçmişi | Son 100 içerik/ayar/yetki değişikliği. |

İngilizce ana dildir. Mevcut İngilizce/İspanyolca/Türkçe içerikler korunur. Almanca ve sonradan açılan dillerde **girilmemiş çeviriler İngilizce gösterilir**; ücretli otomatik çeviri servisi yoktur. Yeni dilde içerikleri ve SEO bilgilerini panelden tamamlayın. PDF başlıkları da Çeviri merkezi → sistem sözlüğü → `SDES_DOCS` üzerinden düzenlenebilir; yeni PDF'ler sözlüğün o anki kopyasını saklar.

Müşteriler panele girmez: referans + özel anahtarla sevkiyat sorgular. Açık ana sayfa kartlarında sadece kamuya açık şehir/ülke, ekipman ve durum bilgileri gösterilir. Özel adres, plaka, müşteri, taşıyıcı ve anahtar gösterilmez. Plakalar ve ETA özel takipte yer alır.

Teklif talepleri panelde saklanır. Otomatik e-posta gönderimi/SMTP bu pakette kurulmuş değildir; PDF'leri indirip e-postayla gönderebilirsiniz. WhatsApp bağlantısı mesaj taslağını açar; kendiliğinden gönderim yapmaz.

## 7. Analytics, Search Console ve çerezler

GA4 için `G-...` ölçüm kimliği; Meta Pixel için sayısal kimlik; Search Console için HTML etiketindeki yalnızca `content` doğrulama değeri girilir. Google/Meta hesabı oluşturma, reklam verme veya Search Console'da doğrulama işlemi panel tarafından otomatik yapılmaz.

Analitik ve pazarlama kategorileri ayrı ve başlangıçta kapalıdır. Gerekli kategori kabul edilmeden ilgili dış script yüklenmez. Tercihler alttaki çerez bağlantısından değişir; geri çekme sonrası erişilebilir ilgili birinci taraf çerezleri temizlenir ve sayfa yenilenir. Google/Meta'nın daha önce aldığı veriler bu işlemle silinmez.

Şirketin resmî bilgileri, veri saklama süreleri, sağlayıcı düzenlemeleri ve gerçek çerez envanteri tamamlandıktan sonra mevcut ön bilgilendirme metinlerini sayfa editöründen sonlandırın. Bu paket hukuki onay veya “%100 Analytics/SEO puanı” garantisi vermez.

## 8. Alan adı ve yayına geçiş

Hostinger uygulamasına `sdestransport.com` alan adını bağlayın ve Hostinger'in **bu uygulama için gösterdiği** DNS değerlerini kullanın. Eski ChatGPT Sites/Cloudflare yönlendirme IP'lerini bu taşıma için yeniden kullanmayın. E-posta için mevcut MX/SPF/DKIM kayıtlarını koruyun.

SSL'yi etkinleştirin, HTTPS adresinden kontrol edin ve `APP_ORIGIN` değerini eşleştirin. `www` kullanılıyorsa ana alan adına yönlendirin. Paket yüklemek, DNS'in otomatik değiştiği anlamına gelmez.

## 9. Taşınan veriler ve yedekleme

31 Ekim 2026 üretim görüntüsünde 3 yük, 3 sevkiyat, 6 durum olayı, 1 talep ve 1 belge kaydı bulunur. Diğer operasyon tabloları boştur. Eski ChatGPT kullanıcı kimliği aktarılmaz; yeni yönetici hesabını siz oluşturursunuz.

**Bir mevcut PDF**, orijinal dosya deposundan indirilemediği için kaydedilmiş belge bilgileri üzerinden yeniden üretildi. Orijinal dosyayla bayt bayt aynı olduğu iddia edilmez. `database/regenerated-pdfs/manifest.json` bunu belirtir. Yeni PDF'ler ve yüklenen görseller MariaDB içinde tutulur; Hostinger'deki kod klasörünün yeniden oluşturulması bu dosyaları silmez.

İş silme, geri alınabilir çöp kutusudur. Tamamlanmış sevkiyat ve hizmet verilen firma sayaçları silinen tamamlanmış işleri de saymaya devam eder. Otomatik kalıcı silme yoktur. Listeler sayfalanır.

Yeni kod yüklemeden önce Hostinger veritabanı yedeğini alın. Günlük veritabanı yedeklerini etkinleştirin ve geri yüklemeyi deneme ortamında doğrulayın. İlk paketteki `sdes-initial.sql`, sonradan eklediğiniz kayıtların yedeği değildir. Yeni yedek, canlı veritabanından alınmalıdır. Büyük fotoğraf/PDF birikimi veritabanı kotasını tüketir; makul dosya boyutları kullanın.

## 10. Canlı ortam kabul testi

Aşağıdaki testleri önce kendi deneme kayıtlarınızla çalıştırın:

1. Yönetici giriş/çıkış; yanlış şifre; kısıtlı personelin izin dışı API işlemlerinin reddi.
2. Bir sayfayı İngilizce/Türkçe düzenleme, görsel değiştirme, Almanca çeviri ekleme.
3. SEO URL değiştirme; eski URL'nin yönlenmesi; `/sitemap.xml` ve kanonik/hreflang çıktısı.
4. Yük taslağı kaydetme ve açma: adres, GPS, çekici/dorse plakası ve ETA korunmalı.
5. Aktif taşıma: kart ve sayaç aynı işi göstermeli; özel veriler ana sayfada olmamalı.
6. Referans/özel anahtarla sorgulama; müşteri ve taşıyıcı PDF'leri; iki GPS bağlantısı.
7. Teslim/çöpe at/geri al: aktif kart kalkmalı; tamamlanan sevkiyat/müşteri toplamları korunmalı.
8. Telefonda menü, teklif, panel, tablo ve takip; açık/koyu tema; yatay taşma ve klavye kullanımını kontrol edin.
9. Çerezleri reddetme/kabul etme/geri çekme; Google/Meta araçlarını kendi doğrulama ekranlarında kontrol edin.
10. Uygulamayı yeniden yayımlayın: yeni iş, görsel, PDF ve kullanıcılar yerinde kalmalı.

Yerel otomatik testler geçti; **Hostinger hesabınızda gerçek MariaDB bağlantısı ve dağıtım testi henüz yapılmadı**. “Hostinger'de yayında ve tümü doğrulandı” olarak değerlendirmeyin. Bu canlı kabul kontrolü tamamlanmadan eski siteyi kaldırmayın.

## 11. Sorun giderme

- **503 / kurulum gerekiyor:** SQL içe aktarımını, DB ad/kullanıcı/şifre ve host bilgisini kontrol edin. Uygulama logları teknik hata kodunu gösterir.
- **Invalid request origin:** Açık tarayıcı adresi ile `APP_ORIGIN` aynı olmalı; sonuna slash eklemeyin.
- **Giriş sonrası tekrar giriş ekranı:** HTTPS ve `NODE_ENV=production` ayarını kontrol edin. Üretim çerezi HTTP üzerinden gönderilmez.
- **Build esbuild/jsdom hatası:** Node 24.x ve `npm ci --include=dev` kullanın; Node 18/20 seçmeyin.
- **Unknown column/table:** Boş veritabanına doğru SQL dosyasının tamamının aktarıldığını doğrulayın; veri olan tabloları silerek çözmeye çalışmayın.
- **Büyük dosyada DB packet hatası:** Görseli küçültün; Hostinger'in veritabanı paket boyutu/kota limitlerini kontrol edin.
- **Şifre unutma:** Sunucu erişimi olan yetkili kişi uygulama dizininde `npm run admin:reset` çalıştırabilir. Terminal girişi görünür olabilir; ortak/ekran kaydı alınan oturum kullanmayın. İşlem mevcut oturumları kapatır.
- **Değişiklik çakışması:** Başka kullanıcı aynı kaydı değiştirmiştir. Sayfayı yeniden yükleyip güncel sürüm üzerinden kaydedin.

## Resmî başvuru

Hostinger Node.js uygulaması, desteklenen plan/sürümler ve ZIP yükleme akışı:
https://www.hostinger.com/support/how-to-deploy-a-nodejs-website-in-hostinger/

Bu kılavuzdaki ayarlar SDES paketine özgüdür; panelinizin alan adlarını ve gerçek DB kimlik bilgilerini kullanın.
